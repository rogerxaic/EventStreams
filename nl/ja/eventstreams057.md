---

copyright:
  years: 2015, 2019
lastupdated: "2018-10-19"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 既知の制約事項
{: #restrictions}

{{site.data.keyword.messagehub}} を使用中に問題があった場合、既知の制約事項と回避策を検討してください。 
{: shortdesc}

## Kafka ブートストラップ・サーバーで障害が起こっても Java Kafka 呼び出しがフェイルオーバーしない
{: #calls_failover}

### 問題
{: #calls_failover_problem notoc}

Java 仮想マシン (JVM) は DNS 参照をキャッシュに入れます。 JVM は、ホスト名から IP アドレスを解決すると、その IP アドレスを、指定された期間中はキャッシュに保持します (この期間は存続時間 (TTL) と呼ばれます)。 一部の Java 構成では、JVM が再始動されるまでホスト名の IP アドレスをまったくリフレッシュしないように JVM TTL が設定されます。 そういった構成の 1 つの例が、セキュリティー・マネージャーを含む構成です。

### 回避策
{: #calls_failover_workaround notoc}

{{site.data.keyword.messagehub}} では高可用性のために複数の Kafka ブートストラップ・サーバー URL と複数の IP アドレスを使用します。そのため、すべてのブローカー IP アドレスが Kafka クライアントに認識されるとは限らず、それが原因となって、作動しているブローカーへのフェイルオーバーが妨げられます。 こういったケースでは、フェイルオーバーには、ブローカー URL の IP アドレスを再照会して、作動している IP アドレスを取得することが必要です。 TTL 値を 30 秒から 60 秒までの値に設定して JVM を構成することをお勧めします。 この値にすると、あるブートストラップ・サーバーの IP アドレスで問題があった場合に、Kafka クライアントは、DNS を照会することによって新しい IP アドレスをルックアップして使用できるようになります。

<code>java.security</code> ファイルから: 

```
# The Java-level namelookup cache policy for successful lookups:
#
# any negative value: caching forever
# any positive value: the number of seconds to cache an address for
# zero: do not cache
#
# default value is forever (FOREVER). For security reasons, this
# caching is made forever when a security manager is set. When a security
# manager is not set, the default behavior in this implementation
# is to cache for 30 seconds.
#
# NOTE: setting this to anything other than the default value can have
#       serious security implications. Do not set it unless
#       you are sure you are not exposed to DNS spoofing attack.
#
#networkaddress.cache.ttl=-1
```

### JVM の TTL を変更する方法
* すべてのアプリケーションに対して JVM の TTL を変更するには、<code><var class="keyword varname">$JAVA_HOME</var>/jre/lib/security/java.security</code> ファイル内に <code>networkaddress.cache.ttl</code> 値を設定します。
* ある特定のアプリケーションに対して JVM TTL を変更するには、アプリケーション・コードで次のように <code>networkaddress.cache.ttl</code> を設定します。
```
java.security.Security.setProperty("networkaddress.cache.ttl" , "30");
```

## Java Kafka 呼び出しがタイムアウトになることがある
{: #calls_timeout}

### 問題
{: #calls_timeout_problem notoc}

Kafka Java クライアント呼び出しで Kafka を検出できないことがあります。 この障害の原因は、Kafka クライアントが、ブートストラップ・サーバーのすべての IP アドレスが、障害が起こっている同じ IP アドレスであると判断したことです。 Kafka クライアントは、各ブローカーの IP アドレス (障害が起こっている同じ IP アドレス) を試し、Kafka がダウンしているという誤った判断を下します。 DNS 照会で複数の IP アドレスが返された場合、Kafka クライアントが使用するのは、返されたリスト中の先頭の IP アドレスです。

### 回避策
{: #calls_timeout_workaround notoc}

ブローカー URL の JVM DNS キャッシュの有効期限が切れまでしばらく待ってから、呼び出しを再試行してください。 後続の Kafka 呼び出しでは、作動するブローカー IP アドレスが DNS 照会から返され、使用されるようになるはずです。 

使用可能なブローカー IP アドレスの一部ではなくすべてを Kafka クライアントが確実に試すようにするため、Kafka Improvement Proposal (KIP) #302 が作成されました。これで、1 つの IP アドレスで障害が起こっても失敗につながることはありません。


## トピックおよびパーティション
{: #topics_partitions}

*  トピック名は最大 100 文字に制限されています。
*  トピックのデフォルトのパーティション数は 1 つです。
*  各 {{site.data.keyword.Bluemix_notm}} スペースのパーティションは 100 までに制限されています。 それより多くのパーティションを作成するには、新しい {{site.data.keyword.Bluemix_notm}} スペースを使用する必要があります。

## メッセージの保存
{: #message_retention}

デフォルトでは、Kafka ではメッセージは最大 24 時間保存され、各パーティションは 1 GB が上限です。 この 1 GB という上限に達したら、この限界を超えないよう、最も古いメッセージが破棄されます。

ユーザー・インターフェースまたは管理 API のいずれかを使用して、トピックを作成するときにメッセージ保存の時間制限を変更できます。 時間制限は、最小 1 時間、最大 30 日間です。

Kafka クライアントまたは Kafka Streams を使用してトピックを作成するときに許可される設定の制限については、[Kafka API の使用](/docs/services/EventStreams?topic=eventstreams-kafka_using)を参照してください。

## Kafka でのトピックの作成および削除
{: #create_delete}

Kafka では、トピックの作成および削除は非同期操作であり、完了するのに時間がかかることがあります。 トピックの迅速な作成および削除に依存する使用パターンや、トピックの迅速な削除および再作成に依存する使用パターンを避けることをお勧めします。

## Kafka REST API
{: #trouble_rest}

*  要求と応答では、バイナリー埋め込み形式のみがサポートされます。Avro および JSON 埋め込み形式はサポートされません。
*  コンシューマー・インスタンスに同時要求はサポートされません。
   コンシューマー・インスタンスに対応する読み取り、コミット、または削除の要求は、必ず、そのインスタンスの未完了要求の応答が受信されてから送信してください。

## Kafka REST API 速度制限
{: #kafka_rate}

Kafka REST API を使用しているアプリケーションは、各 ApiKey の速度制限の対象となることがあります。 この制限を超えると、API は次の HTTP エラーで応答します。

<code>429 Too Many Requests</code>
{:screen}

このエラーが表示されたら、待機してから要求を再サブミットしてください。

<!--12/04/18 - Karen: same info duplicated at messagehub108 -->
## Kafka REST API 日次再始動
{: #rest_restart}

Kafka REST API は、1 日 1 回、短時間の再始動期間があります。 この期間中には、Kafka REST API が利用不可になることがあります。 これが起こった場合、要求を再試行することをお勧めします。 REST API が再始動した後、Kafka コンシューマー・インスタンスを再度作成する必要があります。この場合、REST API は次の JSON を返します。

```'{"error_code":40403,"message":"Consumer instance not found."}'
```
{:screen}

## Kafka ハイレベル・コンシューマー API
{: #kafka_consumer}

Apache Kafka 0.8.2 の単純コンシューマー API または ハイレベル・コンシューマー API を {{site.data.keyword.messagehub}} で使用することはできません。 代わりに、サポートされる最初の Kafka コンシューマー API である 0.9 を使用できます。
