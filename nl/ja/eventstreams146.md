---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-15"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# クラシック・プランの Kafka Java クライアントのマイグレーション 
{: #kafka_java_migrating_classic}


## Kafka Java クライアント 0.9.X または 0.10.X からそれより後のクライアント・バージョンへのマイグレーション
{: #kafka_migrate_classic}


Java クライアントを使用している場合、公開されている
Kafka クライアント 0.10 以降を使用できます。 

0.9.X から最新バージョンに移行する
ことを強くお勧めします。 Kafka クライアントは、[https://kafka.apache.org/downloads ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://kafka.apache.org/downloads){:new_window} からダウンロードできます。

0.9.X クライアントを使用する場合の影響について詳しくは、[後方互換性](/docs/services/EventStreams?topic=eventstreams-kafka_clients#compatibility)を参照してください。



### Kafka Java クライアント 0.10.2.X 以降のバージョンへのマイグレーション

0.10.2 から、JAAS ファイルを使用する代わりに、クライアントのプロパティーに直接 SASL 認証を構成できます。 この簡素化により、異なる資格情報のセットを使用して同一 JVM 内で複数のクライアントを実行できます。これは、JAAS ファイルを使用する場合は不可能です。

以下のステップを実行してください。

1. JAAS ファイルを削除します。 JVM プロパティー java.security.auth.login.config=<PATH TO JAAS> も不要になったので注意してください。
2. 0.9.X からマイグレーションする場合、{{site.data.keyword.messagehub}} ログイン jar モジュールを削除します。
2. クライアントのプロパティーに以下を追加します。
    ```
	sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USERNAME" password="PASSWORD";
	```

	ここで、USERNAME および PASSWORD は、{{site.data.keyword.Bluemix_notm}} の {{site.data.keyword.messagehub}} **「サービス資格情報」**タブからの値です。
	
	

### Kafka クライアント 0.9.X から 0.10.0.X または 0.10.1.X へのマイグレーション

以下のステップを実行してください。

1. {{site.data.keyword.messagehub}} ログイン jar モジュールを削除します。
2. <code>jaas.conf</code> ファイルを次のように変更します。
    ```
        KafkaClient {
          org.apache.kafka.common.security.plain.PlainLoginModule required
          serviceName="kafka"
            username="USERNAME"
            password="PASSWORD";
        };
    ```
    {: codeblock}

	ここで、USERNAME および PASSWORD は、{{site.data.keyword.Bluemix_notm}} の {{site.data.keyword.messagehub}} **「サービス資格情報」**タブからの値です。
	
3. コンシューマーおよびプロデューサーのプロパティーに <code>sasl.mechanism=PLAIN</code> という行を追加します。
