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

# {{site.data.keyword.messagehub}}와 사용할 Kafka 클라이언트 선택
{: #kafka_clients}

{{site.data.keyword.messagehub}}와 Kafka API를 사용하려면 다음 유형의 클라이언트 중 하나를 선택하십시오.

* 공식 Java 클라이언트 1.1 이상(예: [Apache Kafka 1.1 클라이언트 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.apache.org/dyn/closer.cgi?path=/kafka/1.1.0/kafka_2.11-1.1.0.tgz){:new_window}).
	Apache Kafka에 사용 가능한 최신 기능이 포함되어 있으므로 공식 Java 클라이언트를 선택하는 것이 가장 좋습니다.
* [권장 서드파티 클라이언트](/docs/services/EventStreams/eventstreams062.html#clients_table) 중 하나.

두 유형의 클라이언트 모두 항상 최신 클라이언트 버전을 선택하는 것이 좋습니다. 

## 이벤트 스트림에 연결하기 위한 클라이언트 요구사항

{{site.data.keyword.messagehub}}에 연결하려면 클라이언트에서 SASL Plain 메커니즘을 사용한 인증을 지원하고 TLSv1.2 프로토콜의 SNI(Server Name Indication) 확장을 사용해야 합니다.

지원되는 최소 Kafka 프로토콜은 0.10입니다.

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
	

	
## 서드파티 클라이언트
{: #third_party_clients}

공식 Java 클라이언트를 실행할 수 없으면 {{site.data.keyword.messagehub}}로 모두 적절하게 테스트한 [권장 서드파티 클라이언트](/docs/services/EventStreams/eventstreams062.html#clients_table) 중 하나를 실행하는 것이 좋습니다. 

<!--
* [sarama (Go) ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/Shopify/sarama){:new_window}
-->  

최소 클라이언트 요구사항 세트를 지원하는 기타 서드파티 클라이언트는 {{site.data.keyword.messagehub}}와 작동합니다. 그러나 권장 서드파티 클라이언트를 사용해서만 테스트하고 사용한 경험이 있습니다.

## 모든 권장 클라이언트의 지원 요약
{: #client_summary}

<table id="clients_table">
    <caption>표 2. 클라이언트 지원 요약</caption>
      <tr>
		    <th>클라이언트</th>
		    <th>언어</th>
			<th>권장 버전</th>
		    <th>지원되는 최소 버전</th>
			<th>샘플 링크</th>
        </tr>
			<tr>
			<td colspan="3">**공식 클라이언트**</td>
			</tr>
	  		<tr>
			<td>[Apache Kafka 1.1 클라이언트 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.apache.org/dyn/closer.cgi?path=/kafka/1.1.0/kafka_2.11-1.1.0.tgz)</td>
			<td>Java</td>
			<td>최신</td>
			<td>0.10.2</td>
			<td>[Java 콘솔 샘플 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-console-sample)<br/>
			[Liberty 샘플 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-liberty-sample)
			</td>
			</tr>
			<tr>
			<td colspan="3">**서드파티 클라이언트**</td>
			</tr>
	  		<tr>
			<td>[node-rdkafka ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/Blizzard/node-rdkafka)</td>
			<td>Node.js</td>
			<td>최신</td>
			<td>2.3.3</td>
			<td>[Node.js 샘플 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample)</td>
		</tr>
		<tr>
			<td>[confluent-kafka-python ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/confluentinc/confluent-kafka-python)</td>
			<td>Python</td>
			<td>최신</td>
			<td>0.11.6</td>
			<td>[Kafka Python 샘플 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-python-console-sample)</td>
		</tr>
		<tr>
			<td>[confluent-kafka-go ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/confluentinc/confluent-kafka-go)</td>
			<td>Golang</td>
			<td>최신</td>
			<td>0.11.6</td>
			<td></td>
		</tr>
		<tr>
			<td>[librdkafka ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/edenhill/librdkafka)</td>
			<td>C 또는 C++</td>
			<td>최신</td>
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

## {{site.data.keyword.messagehub}}에 클라이언트 연결
{: #connect_client}

{{site.data.keyword.messagehub}}에 연결하기 위해 Java 클라이언트를 구성하는 방법에 관한 정보는 [클라이언트 구성](/docs/services/EventStreams/eventstreams063.html)을 참조하십시오.












