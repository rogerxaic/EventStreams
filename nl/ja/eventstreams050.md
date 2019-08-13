---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-19"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}

# Kafka API の使用
{: #kafka_using}

Kafka は、広範囲の言語に渡る豊富な API とクライアントのセットを提供しています。以下に例を示します。

* **Kafka のコア API (コンシューマー、プロデューサー、および Admin API)**<br/>
    1 つ以上の Kafka トピックとの間で直接メッセージを送受信するために使用します。
* **Streams API**<br/>
    トピック間でイベントを容易にコンシューム、変換、およびプロデュースするための上位ストリーム処理 API。
* **Connect API**<br/>
    外部システム (データベースなど) にイベントをストリーミングしたり、外部システムからイベントを取り出したりするための再使用可能または標準の統合を可能にするフレームワーク。
* **KSQL**<br/>
    SQL に似た構文を使用してトピックからのイベントを処理したり結合したりするためのインターフェース。

以下の表に、{{site.data.keyword.messagehub}} で使用可能なものを要約します。

<table>
    <caption>表 1. 標準プランおよびエンタープライズ・プランでの Kafka クライアントのサポート</caption>
      <tr>
	        <th></th>
		    <th>標準プラン</th>
		    <th>エンタープライズ・プラン</th>
        </tr>
	  		<tr>
			<td>**クラスターの Kafka バージョン**</td>
			<td>Kafka 2.2</td>
			<td>Kafka 1.1</td>
		</tr>
	  		<tr>
			<td>**サポートされるクライアント・バージョン**</td>
			<td>Kafka 0.10.x 以降</td>
			<td>Kafka 0.10.x 以降</td>
		</tr>
		<tr>
			<td>**Kafka Connect、Kafka Streams、および KSQL のサポートの有無 **</td>
			<td>はい</td>
			<td>はい</td>
		</tr>
		<tr>
			<td>**認証要件**</td>
			<td>クライアントは SASL Plain メカニズムを使用した認証をサポートする必要があり、TLSv1.2 プロトコルの Server Name Indication (SNI) 拡張機能を使用する必要があります。</td>
			<td>クライアントは SASL Plain メカニズムを使用した認証をサポートする必要があり、TLSv1.2 プロトコルの Server Name Indication (SNI) 拡張機能を使用する必要があります。</td>
		</tr>

</table>
<br/>
クラシック・プランでの Kafka API の使用方法については、[Kafka API - クラシック](/docs/services/EventStreams?topic=eventstreams-kafka_using_classic)を参照してください。


## {{site.data.keyword.messagehub}} で使用する Kafka クライアントの選択
{: #kafka_clients}

Kafka API の公式クライアントは Java で書かれており、最新の機能やバグ修正が含まれています。この API について詳しくは、[Kafka Producer API 2.2 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://kafka.apache.org/22/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html){:new_window} および
[Kafka Consumer API 2.2 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://kafka.apache.org/22/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html){:new_window} を参照してください。 

その他の言語については、以下のいずれかのクライアントを実行することをお勧めします。これらのクライアントは、すべて {{site.data.keyword.messagehub}} で十分なテストが済んでいます。

### すべての推奨されるクライアントのサポートの概要
{: #client_summary}

<table id="clients_table">
    <caption>表 2. クライアントのサポートの概要</caption>
      <tr>
		    <th id="client" scope="col">クライアント</th>
		    <th id="language" scope="col">言語</th>
			<th id="version" scope="col">推奨されるバージョン</th>
		    <th id="minimum version" scope="col">サポートされる最小バージョン [<sup>1</sup>](/docs/services/EventStreams?topic=eventstreams-kafka_clients#footnote1)</th>
			<th id="sample link" scope="col">サンプルのリンク先</th>
        </tr>
			<tr>
			<td colspan="3">**公式 Apache Kafka クライアント**</td>
			</tr>
	  		<tr>
			<td>[Apache Kafka クライアント![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://kafka.apache.org/downloads)</td>
			<td>Java</td>
			<td>最新</td>
			<td>0.10.2 </td>
			<td>[Java コンソール・サンプル](/docs/services/EventStreams?topic=eventstreams-kafka_java_using)<br/>
			[Liberty サンプル ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-liberty-sample)
			</td>
			</tr>
			<tr>
			<td colspan="3">**サード・パーティーのクライアント**</td>
			</tr>
	  		<tr>
			<td>[node-rdkafka ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/Blizzard/node-rdkafka)</td>
			<td>Node.js</td>
			<td>最新</td>
			<td>2.2.2</td>
			<td>[Node.js サンプル ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample)</td>
		</tr>
		<tr>
			<td>[confluent-kafka-python ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/confluentinc/confluent-kafka-python)</td>
			<td>Python</td>
			<td>最新</td>
			<td>0.11.0</td>
			<td>[Kafka Python サンプル ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-python-console-sample)</td>
		</tr>
		<tr>
			<td>[confluent-kafka-go ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/confluentinc/confluent-kafka-go)</td>
			<td>Golang</td>
			<td>最新</td>
			<td>0.11.0</td>
			<td></td>
		</tr>
		<tr>
			<td>[librdkafka ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/edenhill/librdkafka)</td>
			<td>C または C++</td>
			<td>最新</td>
			<td>0.11.0</td>
			<td></td>
		</tr>

</table>
### 脚注
{: #footnote_clients notoc}
1. {: #footnote1 notoc}このバージョンは、継続的なテストで検証した最も古いバージョンです。 通常、これは過去 12 カ月以内に使用可能な初期バージョンですが、重大な問題が存在することがわかっている場合は、それよりも新しい可能性があります。

<br/>
リストにあるいずれのクライアントも実行できない場合は、以下の最小要件を満たすその他のサード・パーティーのクライアントを使用できます (例えば、[librdkafka ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/edenhill/librdkafka/){:new_window})。
* Kafka 0.10 以降をサポートする。
* SASL PLAIN と TLSv1.2 を使用して接続および認証できる。
* サーバーのホスト名が TLS ハンドシェークに組み込まれている、TLS の SNI 拡張をサポートする。
* 楕円曲線暗号方式をサポートする。
ただし、IBM でテストを行い、使用経験があるのは、推奨されるサード・パーティーのクライアントのみです。

すべてのケースで、最新バージョンのクライアントが推奨されます。

<br/>
### クライアントの {{site.data.keyword.messagehub}} への接続
{: #connect_client}

{{site.data.keyword.messagehub}} に接続するように Java クライアントを構成する方法については、[クライアントの構成](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_api_client)を参照してください。

## Kafka API クライアントの構成
{: #kafka_api_client}

接続を確立するには、最小でも TLSv1.2 を介した SASL_SSL PLAIN を使用し、ユーザー名、およびブートストラップ・サーバーのリストを必要とするように、クライアントを構成する必要があります。 

ユーザー名、パスワード、およびブートストラップ・サーバーのリストを取得するには、サービス・インスタンスのサービス資格情報オブジェクトまたはサービス・キーが必要です。 これらのオブジェクトの作成について詳しくは、<link to Connecting to event Streams>
[{{site.data.keyword.messagehub}} への接続](/docs/services/EventStreams?topic=eventstreams-connecting)を参照してください。

これらのオブジェクトから以下を行います。
* ブートストラップ・サーバーのリストとして <code>kafka_brokers_sasl property</code> を使用します。 このリストは、host:port 項目のコンマ区切りリストとしてフォーマット設定します。 例えば、<code>host1:port1,host2:port2</code> のようにします。 <code>kafka_brokers_sasl</code> プロパティーに示されているすべてのホストの詳細を組み込むことを推奨します。
* ユーザー名とパスワードとして <code>user</code> プロパティーと <code>api_key</code> プロパティーを使用します。

クラシック・プランのサービス・インスタンスの場合、この情報は、ユーザーのアプリケーションの VCAP_SERVICES 環境変数から代わりに取得できます。詳しくは、[{{site.data.keyword.messagehub}} への接続 - クラシック](/docs/services/EventStreams?topic=eventstreams-connecting_classic)を参照してください。

Java クライアントの場合、以下に示す例は、プロパティーの最小セットです。ここで、USERNAME、PASSWORD、および KAFKA_BROKERS_SASL は先に取得した値に置き換えてください。

```
bootstrap.servers=KAFKA_BROKERS_SASL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USERNAME" password="PASSWORD";
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
ssl.protocol=TLSv1.2
ssl.enabled.protocols=TLSv1.2
ssl.endpoint.identification.algorithm=HTTPS
```
{: codeblock}

<br/>
0.10.2.1 より前の Kafka クライアントを使用している場合、<code>sasl.jaas.config</code> プロパティーはサポートされておらず、代わりに JAAS 構成ファイルでクライアント構成を指定する必要があることにご注意ください。 








 




