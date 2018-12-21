---

copyright:
  years: 2015, 2018
lastupdated: "2018-06-22"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Kafka Java クライアントの使用
{: #kafka_using}

<!-- 21/06/18 - removing until some content ready

## To do: instructions for getting started, with links for more information


## To do: simple send source and receive source in-line


## How to use, download, and run the Java Kafka API sample

-->

Java&trade; Kafka API サンプルは、Kafka API を直接使用するプロデューサーとコンシューマーの例であり、Java で記述されています。 このサンプルは、ローカルで実行するか、{{site.data.keyword.Bluemix_short}} 内で実行することができます。

サンプル・コードは [event-streams-samples GitHub プロジェクト ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-console-sample){:new_window} にあります。 このサンプルは、メッセージの送受信には Kafka API を使用しますが、メッセージの送信先およびメッセージの受信元のトピックの作成には {{site.data.keyword.messagehub}} 管理 API を使用します。

このサンプルのセットアップおよび実行について詳しくは、[README.md ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-console-sample){:new_window} を参照してください。

このサンプルの実行方法の段階的な詳しい説明については、[{{site.data.keyword.messagehub}} 入門](/docs/services/EventStreams/index.html#getting_started_steps)を参照してください。

## Liberty for Java サンプルの使用、ダウンロード、および実行の方法
{: #liberty_sample notoc}

Liberty for Java サンプルは、Liberty ランタイムにデプロイされる単純なアプリケーションを実装します。 このアプリケーションは、{{site.data.keyword.messagehub}} がメッセージの生成とコンシュームを行うために、Kafka API を使用します。
また、このアプリケーションは、管理のために使用できる Web フロントエンドを提供します。

サンプル・コードは [event-streams-samples GitHub プロジェクト ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-liberty-sample){:new_window} にあります。

<!--
17/10/17 - Karen: following info duplicated at messagehub063 
-->

## sasl.jaas.config プロパティーの使用
{: #sasl_prop notoc}
0.10.2.1 以降の Kafka クライアントを使用している場合、JAAS ファイルの代わりに、クライアント構成に <code>sasl.jaas.config</code> プロパティーを使用できます。 {{site.data.keyword.messagehub}} に接続するためには、<code>sasl.jaas.config</code> を次のように設定します。
<pre>
<code>    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required \
    username="USERNAME" \
    password="PASSWORD";</code>
</pre>
{:codeblock}

ここで、USERNAME および PASSWORD は、{{site.data.keyword.Bluemix_notm}} の {{site.data.keyword.messagehub}} **「サービス資格情報」**タブからの値です。

<code>sasl.jaas.config</code> を使用する場合、同じ JVM で稼働している複数のクライアントはそれぞれ異なる資格情報を使用できます。 詳しくは、[Configuring Kafka clients  ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://kafka.apache.org/documentation/#security_sasl_plain_clientconfig){:new_window} を参照してください。

以前の Kafka クライアントでは、JAAS 構成ファイルを使用して資格情報を指定する必要がありました。 この方法は不便なため、代わりに <code>sasl.jaas.config</code> プロパティーを使用することをお勧めします。

<!--
23/04/18 - Karen: following migration info on production in messagehub084 
-->

## Kafka クライアント 0.9.X または 0.10.X からそれより後のクライアント・バージョンへのマイグレーション
{: #kafka_migrate}


Java クライアントを使用している場合、公開されている
Kafka クライアント 0.10 以降を使用できます。 0.9.X から最新バージョンに移行する
ことを強くお勧めします。 Kafka クライアントは、
[https://kafka.apache.org/downloads ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://kafka.apache.org/downloads){:new_window} からダウンロードできます。 



### Kafka クライアント 0.10.2.X 以降のバージョンへのマイグレーション

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
