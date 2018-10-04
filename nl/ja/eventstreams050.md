---

copyright:
  years: 2015, 2018
lastupdated: "2018-07-02"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Kafka API の使用
{: #kafka_using}

Java クライアントを使用している場合、公開されている Kafka クライアント 0.10.x 以降を使用できます。 

複数の言語の Kafka クライアントが存在しますが、それらの言語のうちのいくつかについてのみ説明が提供されています。 他のものも使用できますが、資格情報を指定するために SASL PLAIN サポートが必要になります。 さらに、エンタープライズ・プランを使用する場合は、TLSv1.2 プロトコルの Server Name Indication (SNI) 拡張機能を使用することも必要になります。

<table>
    <caption>表 1. 標準プランおよびエンタープライズ・プランでの Kafka クライアントのサポート</caption>
      <tr>
	        <th></th>
		    <th>標準プラン</th>
		    <th>エンタープライズ・プラン</th>
        </tr>
	  		<tr>
			<td>**クラスターの Kafka バージョン**</td>
			<td>Kafka 0.10.2</td>
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

			<td>**認証要件**</td>
			<td>クライアントは SASL Plain メカニズムを使用した認証をサポートする必要があります</td>
			<td>クライアントは SASL Plain メカニズムを使用した認証をサポートする必要があり、TLSv1.2 プロトコルの Server Name Indication (SNI) 拡張機能を使用する必要があります。</td>
		</tr>

</table>

Producer API および Consumer API について詳しくは、[Kafka Producer API 0.11.0.X ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://kafka.apache.org/0110/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html){:new_window} および [Kafka Consumer API 0.11.0.X ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://kafka.apache.org/0110/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html){:new_window} を参照してください。 

