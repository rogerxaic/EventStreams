---

copyright:
  years: 2015, 2018
lastupdated: "2018-11-16"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 選擇 Kafka 用戶端以搭配 {{site.data.keyword.messagehub}} 使用
{: #kafka_clients}

若要使用 Kafka API 搭配 {{site.data.keyword.messagehub}}，請選擇下列其中一種用戶端：

* 1.1 或更新版本的官方 Java 用戶端，例如 [Apache Kafka 1.1 用戶端 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.apache.org/dyn/closer.cgi?path=/kafka/1.1.0/kafka_2.11-1.1.0.tgz){:new_window}。官方 Java 用戶端是最佳的選項，因為它包含可用於 Apache Kafka 的最新特性。
* 其中一個[建議的協力廠商用戶端](/docs/services/EventStreams/eventstreams062.html#clients_table)。

針對兩種用戶端，我們建議一律選擇最新的用戶端版本。 

## 連接至事件串流的用戶端需求

若要連接至 {{site.data.keyword.messagehub}}，用戶端必須支援使用 SASL Plain 機制的鑑別，且使用 TLSv1.2 通訊協定的「伺服器名稱指示 (SNI)」延伸。

我們支援的最低 Kafka 通訊協定是 0.10。

<!--
## Support summary for the official Apache Kafka client (Java)

<table>
    <caption>Table 1. Kafka client support in Standard and Enterprise plans</caption>
      <tr>
	        <th></th>
		    <th>Standard and Enterprise Plans</th>
		    <th></th>
        </tr>
	  		<tr>
			<td>**Kafka version on cluster**</td>
			<td>Kafka 1.1</td>
		</tr>
	  		<tr>
			<td>**Supported client versions**</td>
			<td>Kafka 1.1, or later</td>
		</tr>
			<td>**Authentication requirements**</td>
			<td>Client must support authentication using the SASL Plain mechanism and use the Server Name Indication (SNI) extension to the TLSv1.2 protocol</td>
		</tr>

</table>
-->

<!--
* [Apache Kafka 0.11.0.X client ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.apache.org/dyn/closer.cgi?path=/kafka/0.11.0.1/kafka_2.11-0.11.0.1.tgz){:new_window}
* [Apache Kafka 0.10.2.X client ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.apache.org/dyn/closer.cgi?path=/kafka/0.10.2.1/kafka_2.11-0.10.2.1.tgz){:new_window} 
-->
	

	
## 協力廠商用戶端
{: #third_party_clients}

如果您無法執行官方的 Java 用戶端，我們建議執行其中一個[建議的協力廠商用戶端](/docs/services/EventStreams/eventstreams062.html#clients_table)，這些全都進行過 {{site.data.keyword.messagehub}} 的詳細測試。 

<!--
* [sarama (Go) ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/Shopify/sarama){:new_window}
-->  

支援最少用戶端需求集的其他協力廠商用戶端也可能適用於 {{site.data.keyword.messagehub}}。不過，我們只測試過建議的協力廠商用戶端並具有其經驗。

## 所有建議用戶端的支援摘要
{: #client_summary}

<table id="clients_table">
    <caption>表 2. 用戶端支援摘要</caption>
      <tr>
		    <th>用戶端</th>
		    <th>語言</th>
			<th>建議版本</th>
		    <th>支援的最低版本</th>
			<th>範例鏈結</th>
        </tr>
			<tr>
			<td colspan="3">**官方用戶端**</td>
			</tr>
	  		<tr>
			<td>[Apache Kafka 1.1 用戶端 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.apache.org/dyn/closer.cgi?path=/kafka/1.1.0/kafka_2.11-1.1.0.tgz)</td>
			<td>Java</td>
			<td>最新</td>
			<td>0.10.2</td>
			<td>[Java 主控台範例 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-console-sample)<br/>
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
			<td>2.3.3</td>
			<td>[Node.js 範例 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample)</td>
		</tr>
		<tr>
			<td>[confluent-kafka-python ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/confluentinc/confluent-kafka-python)</td>
			<td>Python</td>
			<td>最新</td>
			<td>0.11.6</td>
			<td>[Kafka Python 範例 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-python-console-sample)</td>
		</tr>
		<tr>
			<td>[confluent-kafka-go ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/confluentinc/confluent-kafka-go)</td>
			<td>Golang</td>
			<td>最新</td>
			<td>0.11.6</td>
			<td></td>
		</tr>
		<tr>
			<td>[librdkafka ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/edenhill/librdkafka)</td>
			<td>C 或 C++</td>
			<td>最新</td>
			<td>0.11.6</td>
			<td></td>
		</tr>

</table>

<!--
## Unsupported clients

The following clients are not supported by {{site.data.keyword.messagehub}}:

### kafka-node
The kafka-node client does not fully support SASL authentication with the PLAIN mechanism so cannot currently be used with {{site.data.keyword.messagehub}}.


### no-kafka 
The no-kafka client does not fully support SASL authentication with the PLAIN mechanism so cannot currently be used with {{site.data.keyword.messagehub}}.

-->

## 將用戶端連接至 {{site.data.keyword.messagehub}}
{: #connect_client}

如需如何配置 Java 用戶端以連接至 {{site.data.keyword.messagehub}} 的相關資訊，請參閱[配置用戶端](/docs/services/EventStreams/eventstreams063.html)。












