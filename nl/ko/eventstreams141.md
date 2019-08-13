---

copyright:
  years: 2015, 2019
lastupdated: "2019-01-14"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.messagehub}} 클래식 플랜에 사용할 Kafka 클라이언트 선택 
{: #kafka_clients_classic}

{{site.data.keyword.messagehub}}와 Kafka API를 사용하려면 다음 유형의 클라이언트 중 하나를 선택하십시오.

* 공식 Java 클라이언트. Apache Kafka에 사용 가능한 최신 기능이 포함되어 있으므로 이를 선택하는 것이 가장 좋습니다.
* [권장 서드파티 클라이언트](/docs/services/EventStreams?topic=eventstreams-kafka_clients#clients_table) 중 하나.

두 유형의 클라이언트 모두 항상 최신 클라이언트 버전을 선택하는 것이 좋습니다. 
{: shortdesc}

## 이벤트 스트림에 연결하기 위한 클라이언트 요구사항

{{site.data.keyword.messagehub}}에 연결하려면 클라이언트에서 SASL Plain 메커니즘을 사용한 인증을 지원하고 TLSv1.2 프로토콜의 SNI(Server Name Indication) 확장을 사용해야 합니다.

지원되는 최소 Kafka 프로토콜은 0.10입니다.
	
## 서드파티 클라이언트
{: #third_party_clients_classic}

공식 Java 클라이언트를 실행할 수 없으면 {{site.data.keyword.messagehub}}로 모두 적절하게 테스트한 [권장 서드파티 클라이언트](/docs/services/EventStreams?topic=eventstreams-kafka_clients#clients_table) 중 하나를 실행하는 것이 좋습니다. 

최소 클라이언트 요구사항 세트를 지원하는 기타 서드파티 클라이언트는 {{site.data.keyword.messagehub}}와 작동합니다. 그러나 권장 서드파티 클라이언트를 사용해서만 테스트하고 사용한 경험이 있습니다.

## 모든 권장 클라이언트의 지원 요약
{: #client_summary_classic}

<table id="clients_table">
    <caption>표 2. 클라이언트 지원 요약</caption>
      <tr>
		    <th id="client" scope="col">클라이언트</th>
		    <th id="language" scope="col">언어</th>
			<th id="version" scope="col">권장 버전</th>
		    <th id="minimum version" scope="col">지원되는 최소 버전 [<sup>1</sup>](/docs/services/EventStreams?topic=eventstreams-kafka_clients_classic#footnote_clients_classic)</th>
			<th id="sample link" scope="col">샘플 링크</th>
        </tr>
			<tr>
			<td colspan="3">**공식 클라이언트**</td>
			</tr>
	  		<tr>
			<td>[Apache Kafka 클라이언트 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://kafka.apache.org/downloads)</td>
			<td>Java</td>
			<td>최신</td>
			<td>0.10.2 <p> 이전 클라이언트에 대한 정보는 [역호환성](/docs/services/EventStreams?topic=eventstreams-kafka_clients_classic#compatibility_classic)을 참조하십시오.</p></td>
			<td>[Java 콘솔 샘플 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-console-sample)<br/>
			[Liberty 샘플![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-liberty-sample)
			</td>
			</tr>
			<tr>
			<td colspan="3">**서드파티 클라이언트**</td>
			</tr>
	  		<tr>
			<td>[node-rdkafka ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/Blizzard/node-rdkafka)</td>
			<td>Node.js</td>
			<td>최신</td>
			<td>2.2.2</td>
			<td>[Node.js 샘플 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample)</td>
		</tr>
		<tr>
			<td>[confluent-kafka-python ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/confluentinc/confluent-kafka-python)</td>
			<td>Python</td>
			<td>최신</td>
			<td>0.11.0</td>
			<td>[Kafka Python 샘플 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-python-console-sample)</td>
		</tr>
		<tr>
			<td>[confluent-kafka-go ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/confluentinc/confluent-kafka-go)</td>
			<td>Golang</td>
			<td>최신</td>
			<td>0.11.0</td>
			<td></td>
		</tr>
		<tr>
			<td>[librdkafka ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/edenhill/librdkafka)</td>
			<td>C 또는 C++</td>
			<td>최신</td>
			<td>0.11.0</td>
			<td></td>
		</tr>

</table>
### 각주
{: #footnote_clients_classic notoc}
1. {: #footnote_classic}이 버전은 지속적 테스트에서 유효성 검증한 가장 빠른 버전입니다. 일반적으로 이 버전은 최근 12개월 이내에 사용 가능한 초기 버전이지만 중요한 문제가 있는 것으로 알려진 경우 새 버전일 수 있습니다.

## 역호환성(클래식 플랜 전용)
{: #compatibility_classic}

역호환성을 위해 Apache Kafka 0.9 Java 클라이언트를 {{site.data.keyword.messagehub}} 클래식 플랜과 함께 사용할 수 있습니다. 그러나 이 클라이언트의 사용 기간으로 인해 이를 사용하지 않도록 적극 권장합니다. 이 클라이언트 버전을 사용하도록 선택하는 경우 추가 [로그인 모듈 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-0.9/message-hub-login-library)이 필요합니다.

새 Kafka 서버 버전에 연결하는 데 필요한 추가 프로토콜 변환으로 인해 0.11 이전의 클라이언트 버전을 사용하면 성능이 저하될 수 있습니다.

<!--
## Unsupported clients

The following clients are not supported by {{site.data.keyword.messagehub}}:

### kafka-node
The kafka-node client does not fully support SASL authentication with the PLAIN mechanism so cannot currently be used with {{site.data.keyword.messagehub}}.


### no-kafka 
The no-kafka client does not fully support SASL authentication with the PLAIN mechanism so cannot currently be used with {{site.data.keyword.messagehub}}.

-->

## {{site.data.keyword.messagehub}}에 클라이언트 연결
{: #connect_client_classic}

{{site.data.keyword.messagehub}}에 연결하기 위해 Java 클라이언트를 구성하는 방법에 관한 정보는 [클라이언트 구성](/docs/services/EventStreams?topic=eventstreams-kafka_connect)을 참조하십시오.












