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

# 在經典方案上使用 Kafka API
{: #kafka_using_classic}

Kafka 用戶端有多種語言，我們提供了其中部分語言的指示。您可以使用其他語言，但您將需要 SASL PLAIN 支援以提供認證。此外，如果您使用企業方案，則也需要使用 TLSv1.2 通訊協定的「伺服器名稱指示 (SNI)」延伸。

<table>
    <caption>表 1. 標準及企業方案中的 Kafka 用戶端支援</caption>
      <tr>
	        <th></th>
		    <th>經典方案</th>
		    
        </tr>
	  		<tr>
			<td>**叢集上的 Kafka 版本**</td>
			<td>Kafka 1.1</td>
					</tr>
	  		<tr>
			<td>**受支援的用戶端版本**</td>
			<td>Kafka 0.10.x 或更新版本</td>
		</tr>
		<tr>
			<td>**是否支援 Kafka Connect、Kafka Streams 及 KSQL？**</td>
			<td>是</td>
		</tr>

			<td>**鑑別需求**</td>
			<td>用戶端必須支援使用 SASL Plain 機制的鑑別，且使用 TLSv1.2 通訊協定的「伺服器名稱指示 (SNI)」延伸。</td>
				</tr>

</table>

如需 V1.1 Producer 和 Consumer API 的資訊，請參閱 [Kafka Producer API 1.1 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://kafka.apache.org/11/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html){:new_window} 和 [Kafka Consumer API 1.1 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://kafka.apache.org/11/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html){:new_window}。 












