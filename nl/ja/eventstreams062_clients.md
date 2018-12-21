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

# {{site.data.keyword.messagehub}} で使用する Kafka クライアントの選択
{: #kafka_clients}

{{site.data.keyword.messagehub}} で Kafka API を使用するには、以下のいずれかのタイプのクライアントを選択します。

* 公式 Java クライアント 1.1 以降。例えば、[Apache Kafka 1.1 クライアント ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.apache.org/dyn/closer.cgi?path=/kafka/1.1.0/kafka_2.11-1.1.0.tgz){:new_window}
	公式 Java クライアントは、Apache Kafka で利用できる最新のフィーチャーが含まれているため、最適な選択です。
* [推奨されるサード・パーティーのクライアント](/docs/services/EventStreams/eventstreams062.html#clients_table)の 1 つ。

どちらのタイプのクライアントについても、常に最新のクライアントのバージョンを選択することをお勧めします。 

## Event Streams に接続するためのクライアントの要件

{{site.data.keyword.messagehub}} に接続するには、クライアントは SASL Plain メカニズムを使用した認証をサポートする必要があり、TLSv1.2 プロトコルの Server Name Indication (SNI) 拡張機能を使用する必要があります。

サポートされる最小の Kafka プロトコルは 0.10 です。

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
	

	
## サード・パーティーのクライアント
{: #third_party_clients}

公式 Java クライアントを実行できない場合は、[推奨されているサード・パーティーのクライアント](/docs/services/EventStreams/eventstreams062.html#clients_table)の 1 つを実行することをお勧めします。これらは、{{site.data.keyword.messagehub}} で実証済みのものです。 

<!--
* [sarama (Go) ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/Shopify/sarama){:new_window}
-->  

クライアント要件の最小セットをサポートする他のサード・パーティーのクライアントが {{site.data.keyword.messagehub}} で動作する場合もあります。しかし、IBM でテストし、使用した経験があるのは、推奨されるサード・パーティーのクライアントのみです。

## すべての推奨されるクライアントのサポートの概要
{: #client_summary}

<table id="clients_table">
    <caption>表 2. クライアントのサポートの概要</caption>
      <tr>
		    <th>クライアント</th>
		    <th>言語</th>
			<th>推奨されるバージョン</th>
		    <th>サポートされる最小バージョン</th>
			<th>サンプルのリンク先</th>
        </tr>
			<tr>
			<td colspan="3">**公式のクライアント**</td>
			</tr>
	  		<tr>
			<td>[Apache Kafka 1.1 クライアント ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.apache.org/dyn/closer.cgi?path=/kafka/1.1.0/kafka_2.11-1.1.0.tgz)</td>
			<td>Java</td>
			<td>最新</td>
			<td>0.10.2</td>
			<td>[ Java コンソール・サンプル ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-console-sample)<br/>
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
			<td>2.3.3</td>
			<td>[Node.js サンプル ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample)</td>
		</tr>
		<tr>
			<td>[confluent-kafka-python ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/confluentinc/confluent-kafka-python)</td>
			<td>Python</td>
			<td>最新</td>
			<td>0.11.6</td>
			<td>[Kafka Python サンプル ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-python-console-sample)</td>
		</tr>
		<tr>
			<td>[confluent-kafka-go ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/confluentinc/confluent-kafka-go)</td>
			<td>Golang</td>
			<td>最新</td>
			<td>0.11.6</td>
			<td></td>
		</tr>
		<tr>
			<td>[librdkafka ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/edenhill/librdkafka)</td>
			<td>C または C++</td>
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

## クライアントの {{site.data.keyword.messagehub}} への接続
{: #connect_client}

{{site.data.keyword.messagehub}} に接続するように Java クライアントを構成する方法については、[クライアントの構成](/docs/services/EventStreams/eventstreams063.html)を参照してください。












