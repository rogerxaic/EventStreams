---

copyright:
  years: 2015, 2018
lastupdated: "2018-04-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

<!-- Notes from chat with Charlie 

Different plan for provisioning

Quality of service from each plan

Life of a user through cycle - APIs, feature sets

-->

# Alpha プログラムについて
{: #alpha_about }

Alpha プログラムは、{{site.data.keyword.messagehub}} サービスの新機能への早期アクセスを可能にします。現行機能では、新規プランである、{{site.data.keyword.messagehub}} プレミアム・プランが初めて導入されています。

## プレミアム・プラン
{: premium_plan}

プレミアム・プランは、公開サービスを超えたパフォーマンスやその他の機能的な要件を持つユーザー向けに設計されました。このプランでは、専用の Apache Kafka クラスターがある、単一テナント・バージョンの {{site.data.keyword.messagehub}} サービスが提供されます。このバージョンでは、以下のことが可能になります。

* クラスターの容量とパフォーマンスをフルに活用できます。

* 各種上限が引き上げられるほか、パーティションの数が大きく増えます。

* 自動的に保守および更新される完全管理対象のクラスターを使用することで、管理コスト・ゼロのメリットを得られます。

## Alpha クラスターについて
{: alpha_cluster}

Alpha クラスターは、Apache Kafka バージョン 1.1 を使用してデプロイされ、最大で 90000 KB/秒のメッセージ・スループットを実現できます。 

最大 1000 個のパーティションを作成でき、各パーティションは最大 1 GB のデータを最長 30 日間保持できます。回復力のために、データは 3 つのレプリカに保管され、各パーティションのコミット済みオフセットは、最大 7 日間保持されます。

以下の 2 つの API がサポートされています。

* メッセージングでは、バージョン 0.10.x 以降の Kafka クライアントがサポートされており、Kafka Streams、Kafka Connect、および KSQL を使用するオプションが含まれます。

* 管理では、トピックを作成、削除、およびリストするための REST API を使用できます。

Alpha プログラムは、米国南部地域のみで使用可能です。クラスターへのアクセスは、IAM を使用して管理されます。

## クラスターへの接続
{: alpha_connect}

クラスター内の API に接続するには、そのエンドポイント URL と認証のための apikey が必要です。これらの詳細は、以下のいずれかの方法を使用して IAM から取得できます。

### Cloud Foundry アプリケーション
Cloud Foundry アプリケーションの場合、以下のようにします。
1. アプリの**「接続」**タブ (左側) で、**「接続の作成」**ボタンをクリックします。 
2. 接続先の {{site.data.keyword.messagehub}} サービスのインスタンスを選択し、**「接続」**をクリックします。デフォルト・オプションを受け入れます。 
3. 接続が完了したら、アプリの**「ランタイム」**タブをクリックし、次に**「環境変数」**をクリックして **VCAP_SERVICES** を表示します。

### 外部アプリケーションのコンソール
外部アプリケーションのコンソールから、以下の **bx** コマンドを使用してサービス apikey を作成します。 

```
bx resource service-key-create name-of-key Manager --instance-name name-of-your-service
``` 

生成された情報から、<code>kafka_brokers_sasl</code>、<code>kafka_admin_url</code>、および <code>apikey</code> の各フィールドをコピーします。
VCAP_SERVICES には、最初の 5 つのブローカーのみがリストされます。5 個を超えるブローカーがある場合、他のブローカーの詳細を取得するには Kafka クライアントを使用してください。 

## クライアントの Kafka API への接続

クライアントを Kafka API に接続するには、以下のステップを実行します。

1. クライアントの <code>bootstrap.servers</code> プロパティーに <code>kafka_brokers_sasl</code> にリストされたブローカーのコンマ区切りリストを設定します。

2. クライアントの <code>sasl.jaas.config</code> USERNAME フィールドに <code>apikey</code> の先頭 8 文字を設定し、PASSWORD フィールドに残りの文字を設定します (将来のバージョンでは、このような分割は不要になります)。

使用する Kafka クライアントは、以下の機能をサポートしなければなりません。

* クライアント・バージョン 0.10.x 以降

* SASL Plain メカニズムを使用した認証

* TLS v1.2 プロトコルへの Service Name Identification (SNI) 拡張

エンドポイントおよび資格情報に関する情報を取得するこの方法は、既存の {{site.data.keyword.messagehub}} サービスとは異なります。現在 {{site.data.keyword.messagehub}} に対して実行されているアプリは、VCAP_SERVICES から必要な代替フィールド名や、Kafka に送信されるユーザー名/パスワードのフィールドを反映するために変更が必要になります。このような変更は、将来のバージョンの Alpha では不要になります。

## クライアントの REST API への接続

クライアントを REST API に接続するには、以下のステップを実行します。

* REST API の URI は、<code>kafka_admin_url</code> で提供されます。

* HTTP <code>Content-Type</code> ヘッダーに <code>application/json</code> を設定します。

* HTTP <code>X-Auth-Token</code> ヘッダーに <code>apikey</code> の値を設定します。

Alpha を使用して稼働を始めるための簡単なステップについては、[Alpha プログラム入門](/docs/services/MessageHub/messagehub120.html)を参照してください。


## {{site.data.keyword.messagehub}} の管理
{: alpha_admin}

クラスター内で必要な管理タスクは、必要なトピックを作成、リスト、および削除することのみです。以下のいずれかの方法を使用して管理できます。

* アプリケーションから直接 Kafka 管理 API を使用する。例えば、Java の場合、AdminClient ![外部リンク・アイコン](http://kafka.apache.org/11/javadoc/index.html?org/apache/kafka/clients/admin/AdminClient.html){:new_window} の <code>createTopics()</code> メソッド、<code>deleteTopics()</code> メソッド、または <code>listTopics()</code> メソッドを使用します。

* IBM Cloud ポータルで使用可能なサービス・インスタンスの Web UI を使用して対話式に行う。

* クラスター内で提供される管理 REST API を使用する。

管理 REST API (これは既存の {{site.data.keyword.messagehub}} 管理 API と互換性があります) によって提供される作成、リスト、および削除の関数の詳細は、[admin-rest-api.yaml ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-messaging/message-hub-docs/blob/master/admin-rest-api/admin-rest-api.yaml){:new_window} から入手できる API の完全な仕様で確認できます。
Swagger ファイルを表示するには、Swagger ツール (例えば、[Swagger エディター ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://editor.swagger.io/#/){:new_window}) を使用します。

Curl を使用してトピックを作成する方法を示した簡単な例については、[Alpha プログラム入門](/docs/services/MessageHub/messagehub120.html)を参照してください。

将来的には、その他の構成オプションも使用可能になる予定です。


## サンプル

近日公開予定...

Alpha を使用して稼働を始めるための簡単なステップについては、[Alpha プログラム入門](/docs/services/MessageHub/messagehub120.html)を参照してください。

## Alpha の制限
{: alpha_limitations}

この Alpha プログラムの現在の制限は以下のとおりです。

### まだ使用可能でないが、近日公開予定

* 複数アベイラビリティー・ゾーンのサポート

* ユーザーが管理するスケーリングとロードの限度

* {{site.data.keyword.cloudaccesstrailfull_notm}} サービス (旧称 AccessTrail) との統合 

### 現時点で未計画

* ブリッジ

* REST メッセージング

* MQ Light API










