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

# 使用 Kafka API
{: #kafka_using}

Kafka 用戶端有多種語言，我們提供了其中部分語言的指示。您可以使用其他語言，但您將需要 SASL PLAIN 支援以提供認證。此外，如果您使用企業方案，則也需要使用 TLSv1.2 通訊協定的「伺服器名稱指示 (SNI)」延伸。

如需使用經典方案上 Kafka API 的資訊，請參閱 [Kafka API - 經典](/docs/services/EventStreams?topic=eventstreams-kafka_using_classic)。

<table>
    <caption>表 1. 標準及企業方案中的 Kafka 用戶端支援</caption>
      <tr>
	        <th></th>
		    <th>標準方案</th>
		    <th>企業方案</th>
        </tr>
	  		<tr>
			<td>**叢集上的 Kafka 版本**</td>
			<td>Kafka 2.2</td>
			<td>Kafka 1.1</td>
		</tr>
	  		<tr>
			<td>**受支援的用戶端版本**</td>
			<td>Kafka 0.10.x 或更新版本</td>
			<td>Kafka 0.10.x 或更新版本</td>
		</tr>
		<tr>
			<td>**是否支援 Kafka Connect、Kafka Streams 及 KSQL？**</td>
			<td>是</td>
			<td>是</td>
		</tr>

			<td>**鑑別需求**</td>
			<td>用戶端必須支援使用 SASL Plain 機制的鑑別，且使用 TLSv1.2 通訊協定的「伺服器名稱指示 (SNI)」延伸。</td>
			<td>用戶端必須支援使用 SASL Plain 機制的鑑別，且使用 TLSv1.2 通訊協定的「伺服器名稱指示 (SNI)」延伸。</td>
		</tr>

</table>

如需 V2.2 Producer 和 Consumer API 的資訊，請參閱 [Kafka Producer API 2.2 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://kafka.apache.org/22/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html){:new_window} 和 [Kafka Consumer API 2.2 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://kafka.apache.org/22/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html){:new_window}。 


## 選擇 Kafka 用戶端以搭配 {{site.data.keyword.messagehub}} 使用
{: #kafka_clients}

若要使用 Kafka API 搭配 {{site.data.keyword.messagehub}}，請選擇下列其中一種用戶端：

* 官方 Java 用戶端。這是最佳的選項，因為它包含可用於 Apache Kafka 的最新特性。
* 其中一個[建議的協力廠商用戶端](/docs/services/EventStreams?topic=eventstreams-kafka_clients#clients_table)。

針對兩種用戶端，我們建議一律選擇最新的用戶端版本。 

### 連接至事件串流的用戶端需求

若要連接至 {{site.data.keyword.messagehub}}，用戶端必須支援使用 SASL Plain 機制的鑑別，且使用 TLSv1.2 通訊協定的「伺服器名稱指示 (SNI)」延伸。

我們支援的最低 Kafka 通訊協定是 0.10。

	
### 協力廠商用戶端
{: #third_party_clients}

如果您無法執行官方的 Java 用戶端，我們建議執行其中一個[建議的協力廠商用戶端](/docs/services/EventStreams?topic=eventstreams-kafka_clients#clients_table)，這些全都進行過 {{site.data.keyword.messagehub}} 的詳細測試。支援最少用戶端需求集的其他協力廠商用戶端也可能適用於 {{site.data.keyword.messagehub}}。不過，我們只測試過建議的協力廠商用戶端並具有其經驗。

### 所有建議用戶端的支援摘要
{: #client_summary}

<table id="clients_table">
    <caption>表 2. 用戶端支援摘要</caption>
      <tr>
		    <th id="client" scope="col">用戶端</th>
		    <th id="language" scope="col">語言</th>
			<th id="version" scope="col">建議版本</th>
		    <th id="minimum version" scope="col">支援的最低版本 [<sup>1</sup>](/docs/services/EventStreams?topic=eventstreams-kafka_clients#footnote1)</th>
			<th id="sample link" scope="col">範例鏈結</th>
        </tr>
			<tr>
			<td colspan="3">**官方用戶端**</td>
			</tr>
	  		<tr>
			<td>[Apache Kafka 用戶端 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://kafka.apache.org/downloads)</td>
			<td>Java</td>
			<td>最新</td>
			<td>0.10.2</td>
			<td>[Java 主控台範例](/docs/services/EventStreams?topic=eventstreams-kafka_java_using)<br/>
			[Liberty 範例 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-liberty-sample)
			</td>
			</tr>
			<tr>
			<td colspan="3">**協力廠商用戶端**</td>
			</tr>
	  		<tr>
			<td>[node-rdkafka ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/Blizzard/node-rdkafka)</td>
			<td>Node.js</td>
			<td>最新</td>
			<td>2.2.2</td>
			<td>[Node.js 範例 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample)</td>
		</tr>
		<tr>
			<td>[confluent-kafka-python ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/confluentinc/confluent-kafka-python)</td>
			<td>Python</td>
			<td>最新</td>
			<td>0.11.0</td>
			<td>[Kafka Python 範例 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-python-console-sample)</td>
		</tr>
		<tr>
			<td>[confluent-kafka-go ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/confluentinc/confluent-kafka-go)</td>
			<td>Golang</td>
			<td>最新</td>
			<td>0.11.0</td>
			<td></td>
		</tr>
		<tr>
			<td>[librdkafka ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/edenhill/librdkafka)</td>
			<td>C 或 C++</td>
			<td>最新</td>
			<td>0.11.0</td>
			<td></td>
		</tr>

</table>
### 註腳
1. {: #footnote1}此版本是我們已在持續測試中驗證過的最早版本。通常這是在過去 12 個月內可用的起始版本，但如果已知有重要的問題存在，可能會更新。


<!--
## Unsupported clients

The following clients are not supported by {{site.data.keyword.messagehub}}:

### kafka-node
The kafka-node client does not fully support SASL authentication with the PLAIN mechanism so cannot currently be used with {{site.data.keyword.messagehub}}.


### no-kafka 
The no-kafka client does not fully support SASL authentication with the PLAIN mechanism so cannot currently be used with {{site.data.keyword.messagehub}}.

-->

<br/>
### 將用戶端連接至 {{site.data.keyword.messagehub}}
{: #connect_client}

如需如何配置 Java 用戶端以連接至 {{site.data.keyword.messagehub}} 的相關資訊，請參閱[配置用戶端](/docs/services/EventStreams?topic=eventstreams-kafka_connect)。

## 配置 Kafka API 用戶端
{: #kafka_api_client}

要建立連線，用戶端必須配置為最低透過 TLSv1.2 使用 SASL_SSL PLAIN，並且需要使用者名稱和引導伺服器清單。 

要擷取使用者名稱、密碼和引導伺服器的清單，服務實例需要服務認證物件或服務金鑰。如需建立這些物件的相關資訊，請參閱<link to Connecting to event Streams>[連接至 {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting)。

從這些物件：
* 將 <code>kafka_brokers_sasl property</code> 用作引導伺服器的清單。將此清單的格式設置為主機:埠項目的以逗點區隔的清單。例如，<code>host1:port1,host2:port2</code>。我們建議包含 <code>kafka_brokers_sasl</code> 內容中列出的所有主機的詳細資料。
* 將 <code>user</code> 和 <code>api_key</code> 內容用作使用者名稱及密碼。

對於經典方案上的服務實例，可以改為從應用程式的 VCAP_SERVICES 環境變數中提供此資訊。如需相關資訊，請參閱[連接 {{site.data.keyword.messagehub}} - 經典](/docs/services/EventStreams?topic=eventstreams-connecting_classic)。


對於 Java 用戶端，下列範例顯示最小的內容集，其中 USERNAME、PASSWORD 和 KAFKA_BROKERS_SASL 應取代為先前擷取的值。

```
bootstrap.servers=KAFKA_BROKERS_SASL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USERNAME" password="PASSWORD";
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
ssl.protocol=TLSv1.2
ssl.enabled.protocols=TLSv1.2
ssl.endpoint.identification.algorithm=HTTPS

# To send or receive messages, the following are also required
key.deserializer=org.apache.kafka.common.serialization.StringDeserializer
value.deserializer=org.apache.kafka.common.serialization.StringDeserializer
```
{: codeblock}

<br/>
請注意，如果您使用 0.10.2.1 之前的 Kafka 用戶端，那麼不支援 <code>sasl.jaas.config</code> 內容，您必須改為在 JAAS 配置檔中提供用戶端配置。 

### 在非 Java 的應用程式中進行連接及鑑別
{: #kafka_notjava }

支援使用 SASL PLAIN 和 TLSv1.2 的 Kafka 0.10 的任何用戶端都應該使用 {{site.data.keyword.messagehub}}。

注意，用戶端必須支援 TLS 的 SNI 延伸，其中伺服器的主機名稱包含在 TLS 信號交換中。 

範例用戶端如下所示：
* [librdkafka ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/edenhill/librdkafka/){:new_window} 
* [confluent-kafka-python ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/confluentinc/confluent-kafka-python){:new_window} 




 




