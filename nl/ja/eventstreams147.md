---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-15"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}

# クラシック・プランの Kafka API の使用
{: #kafka_using_classic}

複数の言語の Kafka クライアントが存在しますが、それらの言語のうちのいくつかについてのみ説明が提供されています。 他のものも使用できますが、資格情報を指定するために SASL PLAIN サポートが必要になります。 さらに、エンタープライズ・プランを使用する場合は、TLSv1.2 プロトコルの Server Name Indication (SNI) 拡張機能を使用することも必要になります。

<table>
    <caption>表 1. 標準プランおよびエンタープライズ・プランでの Kafka クライアントのサポート</caption>
      <tr>
	        <th></th>
		    <th>クラシック・プラン</th>
		    
        </tr>
	  		<tr>
			<td>**クラスターの Kafka バージョン**</td>
			<td>Kafka 1.1</td>
					</tr>
	  		<tr>
			<td>**サポートされるクライアント・バージョン**</td>
			<td>Kafka 0.10.x 以降</td>
		</tr>
		<tr>
			<td>**Kafka Connect、Kafka Streams、および KSQL のサポートの有無 **</td>
			<td>はい</td>
		</tr>

			<td>**認証要件**</td>
			<td>クライアントは SASL Plain メカニズムを使用した認証をサポートする必要があり、TLSv1.2 プロトコルの Server Name Indication (SNI) 拡張機能を使用する必要があります。</td>
				</tr>

</table>

V1.1 プロデューサー API およびコンシューマー API について詳しくは、[Kafka プロデューサー API 1.1 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://kafka.apache.org/11/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html){:new_window} および [Kafka コンシューマー API 1.1 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://kafka.apache.org/11/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html){:new_window}を参照してください。 












