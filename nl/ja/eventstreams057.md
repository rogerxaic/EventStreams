---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-13"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}


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
{: #jvm_ttl notoc}
* すべてのアプリケーションに対して JVM の TTL を変更するには、<code><var class="keyword varname">$JAVA_HOME</var>/jre/lib/security/java.security</code> ファイル内に <code>networkaddress.cache.ttl</code> 値を設定します。
* ある特定のアプリケーションに対して JVM TTL を変更するには、アプリケーション・コードで次のように <code>networkaddress.cache.ttl</code> を設定します。
```
java.security.Security.setProperty("networkaddress.cache.ttl" , "30");
```

## Java Kafka 呼び出しがタイムアウトになることがある
{: #calls_timeout_kafka}

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

<!--following message retention info duplicted in FAQs eventstreams108-->

## メッセージの保存
{: #message_retention}

デフォルトでは、Kafka ではメッセージは最大 24 時間保存され、各パーティションは 1 GB が上限です。 この 1 GB という上限に達したら、この限界を超えないよう、最も古いメッセージが破棄されます。

ユーザー・インターフェースまたは管理 API のいずれかを使用して、トピックを作成するときにメッセージ保存の時間制限を変更できます。 時間制限は、最小 1 時間、最大 30 日間です。

Kafka クライアントまたは Kafka Streams を使用してトピックを作成するときに許可される設定の制限については、[Kafka API を使用してトピックの作成と削除を行うにはどうしたらよいか?
](/docs/services/EventStreams?topic=eventstreams-faqs#topic_admin)を参照してください。

## Kafka でのトピックの作成および削除
{: #create_delete}

Kafka では、トピックの作成および削除は非同期操作であり、完了するのに時間がかかることがあります。 トピックの迅速な作成および削除に依存する使用パターンや、トピックの迅速な削除および再作成に依存する使用パターンを避けることをお勧めします。

<!--
## Kafka REST API
{: #trouble_rest}

<br/>
**Is this specific to old Standard only? If so I'll move to specific Standard topic.**
{: note}

*  Only the binary-embedded format is supported for requests and
   responses. The Avro and JSON embedded formats are not supported.
*  Concurrent requests are not supported for a consumer instance.
   Read, commit, or delete requests corresponding to a consumer
   instance should be sent only after a response is received for
   any outstanding requests of that instance.

-->
<!--
<br/>
**Is this specific to old Standard only? If so I'll move to specific Standard topic.**
{: note}

## Kafka REST API rate limitation
{: #kafka_rate}

Applications using the Kafka REST API can be subject to rate
limiting for each ApiKey. When this limiting occurs, the API
responds with the following HTTP error:

<code>429 Too Many Requests</code>
{:screen}

If you see this error, wait and submit the request again.

<br/>
**Is this specific to old Standard only? If so I'll move to specific Standard topic.**
{: note}
-->
<!--12/04/18 - Karen: same info duplicated at messagehub108 -->
<!--
## Kafka REST API daily restart
{: #rest_restart}

The Kafka REST API restarts once a day for a short period of
time. During this period, the Kafka REST API might become
unavailable. If this happens, you are recommended to retry your
request. After the REST API has restarted, you will have to
create your Kafka consumer instances again. If this is the case, the
REST API returns the following JSON:

```'{"error_code":40403,"message":"Consumer instance not found."}'
```
{:screen}
-->
<!--
## Kafka high-level consumer API
{: #kafka_consumer}

You cannot use the Apache Kafka 0.8.2 simple or high-level
consumer API with {{site.data.keyword.messagehub}}. Instead, you can use the earliest supported Kafka consumer API, which is 0.10.
-->
