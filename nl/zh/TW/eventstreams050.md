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

# 使用 Kafka API
{: #kafka_using}

Kafka 提供跨越廣泛語言的豐富 API 及用戶端集合。例如：

* **Kafka 的核心 API（消費者、生產者及管理者 API）**<br/>
    用來直接從一個以上的 Kafka 主題傳送及接收訊息。
* **Streams API**<br/>
    一種較高層次的串流處理 API，可在主題之間輕鬆地使用、轉換及產生事件。
* **Connect API**<br/>
    一種架構，容許可重複使用或標準整合，以將事件串流至及串流出外部系統（例如資料庫）。
* **KSQL**<br/>
    一種介面，用於使用 SQL 型語法來處理及結合來自主題的事件。

下表彙總您可以與 {{site.data.keyword.messagehub}} 搭配使用的項目：

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
		<tr>
			<td>**鑑別需求**</td>
			<td>用戶端必須支援使用 SASL Plain 機制的鑑別，且使用 TLSv1.2 通訊協定的「伺服器名稱指示 (SNI)」延伸。</td>
			<td>用戶端必須支援使用 SASL Plain 機制的鑑別，且使用 TLSv1.2 通訊協定的「伺服器名稱指示 (SNI)」延伸。</td>
		</tr>

</table>
<br/>
如需使用經典方案上 Kafka API 的資訊，請參閱 [Kafka API - 經典](/docs/services/EventStreams?topic=eventstreams-kafka_using_classic)。


## 選擇 Kafka 用戶端以搭配 {{site.data.keyword.messagehub}} 使用
{: #kafka_clients}

Kafka API 的正式用戶端是以 Java 撰寫，因此，會包含最新特性及錯誤修正程式。如需此 API 的相關資訊，請參閱 [Kafka Producer API 2.2 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://kafka.apache.org/22/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html){:new_window} 及 [Kafka Consumer API 2.2 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://kafka.apache.org/22/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html){:new_window}。 

若為其他語言，建議執行下列其中一個用戶端，而所有這些用戶端都已使用 {{site.data.keyword.messagehub}} 進行良好測試。

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
			<td colspan="3">**正式 Apache Kafka 用戶端**</td>
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
{: #footnote_clients notoc}
1. {: #footnote1 notoc}此版本是我們已在持續測試中驗證過的最早版本。通常這是在過去 12 個月內可用的起始版本，但如果已知有重要的問題存在，可能會更新。

<br/>
如果您無法執行任何列出的用戶端，則可以使用符合下列最低需求的其他協力廠商用戶端（例如，[librdkafka ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/edenhill/librdkafka/){:new_window}）。
* 支援 Kafka 0.10 或更新版本
* 可以搭配使用 SASL PLAIN 與 TLSv1.2 來連接及鑑別
* 支援 TLS 的 SNI 延伸，其中，TLS 信號交換中包括伺服器主機名稱
* 支援橢圓曲線加密法
不過，我們只測試過建議的協力廠商用戶端並具有其經驗。

無論如何，建議使用最新版本的用戶端。

<br/>
### 將用戶端連接至 {{site.data.keyword.messagehub}}
{: #connect_client}

如需如何配置 Java 用戶端以連接至 {{site.data.keyword.messagehub}} 的相關資訊，請參閱[配置用戶端](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_api_client)。

## 配置 Kafka API 用戶端
{: #kafka_api_client}

若要建立連線，用戶端必須配置為最低透過 TLSv1.2 使用 SASL_SSL PLAIN，並且需要使用者名稱和引導伺服器清單。 

若要擷取使用者名稱、密碼和引導伺服器的清單，服務實例需要服務認證物件或服務金鑰。如需建立這些物件的相關資訊，請參閱<link to Connecting to event Streams>[連接至 {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting)。

從這些物件：
* 將 <code>kafka_brokers_sasl property</code> 用作引導伺服器的清單。將此清單的格式設置為主機:埠項目的以逗點區隔的清單。例如，<code>host1:port1,host2:port2</code>。我們建議包含 <code>kafka_brokers_sasl</code> 內容中列出的所有主機的詳細資料。
* 將 <code>user</code> 和 <code>api_key</code> 內容用作使用者名稱及密碼。

對於標準方案上的服務實例，可以改為從應用程式的 VCAP_SERVICES 環境變數中提供此資訊。如需相關資訊，請參閱[連接 {{site.data.keyword.messagehub}} - 經典](/docs/services/EventStreams?topic=eventstreams-connecting_classic)。


對於 Java 用戶端，下列範例顯示最小的內容集，其中 USERNAME、PASSWORD 和 KAFKA_BROKERS_SASL 應取代為先前擷取的值。

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
請注意，如果您使用 0.10.2.1 之前的 Kafka 用戶端，則不支援 <code>sasl.jaas.config</code> 內容，您必須改為在 JAAS 配置檔中提供用戶端配置。 








 




