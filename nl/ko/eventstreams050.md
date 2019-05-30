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

# Kafka API 사용
{: #kafka_using}

Kafka 클라이언트는 여러 개의 언어로 존재하며 해당 언어 중 일부에 대한 지시사항이 제공됩니다. 다른 것을 사용할 수 있지만 인증 정보를 제공하기 위해서는 SASL PLAIN 지원이 필요합니다. 또한 엔터프라이즈 플랜을 사용하는 경우에는 TLSv1.2 프로토콜에 대한 SNI(Server Name Indication) 확장도 사용해야 합니다.

클래식 플랜에서 Kafka API를 사용하는 방법에 대한 정보는 [Kafka API - 클래식](/docs/services/EventStreams?topic=eventstreams-kafka_using_classic)을 참조하십시오.

<table>
    <caption>표 1. 표준 및 엔터프라이즈 플랜에서의 Kafka 클라이언트 지원</caption>
      <tr>
	        <th></th>
		    <th>표준 플랜</th>
		    <th>엔터프라이즈 플랜</th>
        </tr>
	  		<tr>
			<td>**클러스터의 Kafka 버전**</td>
			<td>Kafka 2.2</td>
			<td>Kafka 1.1</td>
		</tr>
	  		<tr>
			<td>**지원되는 클라이언트 버전**</td>
			<td>Kafka 0.10.x 이상</td>
			<td>Kafka 0.10.x 이상</td>
		</tr>
		<tr>
			<td>**Kafka Connect, Kafka Streams 및 KSQL 지원 여부 **</td>
			<td>예</td>
			<td>예</td>
		</tr>

			<td>**인증 요구사항**</td>
			<td>클라이언트는 SASL PLAIN 메커니즘을 사용한 인증을 지원해야 하며 TLSv1.2 프로토콜에 대한 SNI(Server Name Indication) 확장을 사용해야 합니다.</td>
			<td>클라이언트는 SASL PLAIN 메커니즘을 사용한 인증을 지원해야 하며 TLSv1.2 프로토콜에 대한 SNI(Server Name Indication) 확장을 사용해야 합니다.</td>
		</tr>

</table>

2.2 제작자 및 이용자 API에 대한 자세한 정보는 [Kafka Producer API 2.2 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://kafka.apache.org/22/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html){:new_window} 및 [Kafka Consumer API 2.2 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://kafka.apache.org/22/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html){:new_window}를 참조하십시오. 


## {{site.data.keyword.messagehub}}와 사용할 Kafka 클라이언트 선택
{: #kafka_clients}

{{site.data.keyword.messagehub}}와 Kafka API를 사용하려면 다음 유형의 클라이언트 중 하나를 선택하십시오.

* 공식 Java 클라이언트. Apache Kafka에 사용 가능한 최신 기능이 포함되어 있으므로 이를 선택하는 것이 가장 좋습니다.
* [권장 서드파티 클라이언트](/docs/services/EventStreams?topic=eventstreams-kafka_clients#clients_table) 중 하나.

두 유형의 클라이언트 모두 항상 최신 클라이언트 버전을 선택하는 것이 좋습니다. 

### 이벤트 스트림에 연결하기 위한 클라이언트 요구사항

{{site.data.keyword.messagehub}}에 연결하려면 클라이언트에서 SASL Plain 메커니즘을 사용한 인증을 지원하고 TLSv1.2 프로토콜의 SNI(Server Name Indication) 확장을 사용해야 합니다.

지원되는 최소 Kafka 프로토콜은 0.10입니다.

	
### 서드파티 클라이언트
{: #third_party_clients}

공식 Java 클라이언트를 실행할 수 없으면 {{site.data.keyword.messagehub}}로 모두 적절하게 테스트한 [권장 서드파티 클라이언트](/docs/services/EventStreams?topic=eventstreams-kafka_clients#clients_table) 중 하나를 실행하는 것이 좋습니다. 
최소 클라이언트 요구사항 세트를 지원하는 기타 서드파티 클라이언트는 {{site.data.keyword.messagehub}}와 작동합니다. 그러나 권장 서드파티 클라이언트를 사용해서만 테스트하고 사용한 경험이 있습니다.

### 모든 권장 클라이언트의 지원 요약
{: #client_summary}

<table id="clients_table">
    <caption>표 2. 클라이언트 지원 요약</caption>
      <tr>
		    <th id="client" scope="col">클라이언트</th>
		    <th id="language" scope="col">언어</th>
			<th id="version" scope="col">권장 버전</th>
		    <th id="minimum version" scope="col">지원되는 최소 버전 [<sup>1</sup>](/docs/services/EventStreams?topic=eventstreams-kafka_clients#footnote1)</th>
			<th id="sample link" scope="col">샘플 링크</th>
        </tr>
			<tr>
			<td colspan="3">**공식 클라이언트**</td>
			</tr>
	  		<tr>
			<td>[Apache Kafka 클라이언트 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://kafka.apache.org/downloads)</td>
			<td>Java</td>
			<td>최신</td>
			<td>0.10.2 </td>
			<td>[Java 콘솔 샘플](/docs/services/EventStreams?topic=eventstreams-kafka_java_using)<br/>
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
1. {: #footnote1}이 버전은 지속적 테스트에서 유효성 검증한 가장 빠른 버전입니다. 일반적으로 이 버전은 최근 12개월 이내에 사용 가능한 초기 버전이지만 중요한 문제가 있는 것으로 알려진 경우 새 버전일 수 있습니다.


<!--
## Unsupported clients

The following clients are not supported by {{site.data.keyword.messagehub}}:

### kafka-node
The kafka-node client does not fully support SASL authentication with the PLAIN mechanism so cannot currently be used with {{site.data.keyword.messagehub}}.


### no-kafka 
The no-kafka client does not fully support SASL authentication with the PLAIN mechanism so cannot currently be used with {{site.data.keyword.messagehub}}.

-->

<br/>
### {{site.data.keyword.messagehub}}에 클라이언트 연결
{: #connect_client}

{{site.data.keyword.messagehub}}에 연결하기 위해 Java 클라이언트를 구성하는 방법에 관한 정보는 [클라이언트 구성](/docs/services/EventStreams?topic=eventstreams-kafka_connect)을 참조하십시오.

## Kafka API 클라이언트 구성
{: #kafka_api_client}

연결을 설정하려면 클라이언트가 최소한 TLSv1.2를 통해 SASL_SSL PLAIN을 사용하고 사용자 이름 및 부트스트랩 서버 목록을 요구하도록 구성되어야 합니다. 

사용자 이름, 비밀번호 및 부트스트랩 서버 목록을 검색하려면 서비스 인스턴스에 대한 서비스 인증 정보 오브젝트 또는 서비스 키가 필요합니다. 이러한 오브젝트 작성에 대한 자세한 정보는 <link to Connecting to event Streams>
[{{site.data.keyword.messagehub}}에 연결](/docs/services/EventStreams?topic=eventstreams-connecting)을 참조하십시오.

이러한 오브젝트에서:
* <code>kafka_brokers_sasl</code> 특성을 부트스트랩 서버의 목록으로 사용하십시오. 이 목록을 호스트:포트 항목의 쉼표로 구분된 목록으로 형식화하십시오(예: <code>host1:port1,host2:port2</code>). <code>kafka_brokers_sasl</code> 특성에 나열된 모든 호스트에 대한 세부사항을 포함하도록 권장합니다.
* <code>user</code> 및 <code>api_key</code> 특성을 사용자 이름 및 비밀번호로 사용하십시오.

클래식 플랜의 서비스 인스턴스의 경우 이 정보는 대신 애플리케이션의 VCAP_SERVICES 환경 변수에서 사용할 수 있습니다. 자세한 정보는 [{{site.data.keyword.messagehub}} - 클래식](/docs/services/EventStreams?topic=eventstreams-connecting_classic)을 참조하십시오.

Java 클라이언트의 경우 다음 예제는 최소 특성 세트를 보여줍니다. 여기서 USERNAME, PASSWORD 및 KAFKA_BROKERS_SASL은 이전에 검색된 값으로 대체되어야 합니다.

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
0.10.2.1 이전의 Kafka 클라이언트를 사용 중인 경우 <code>sasl.jaas.config</code> 특성이 지원되지 않으며 대신 JAAS 구성 파일에 클라이언트 구성을 제공해야 합니다. 

### Java 이외의 애플리케이션에서 연결 및 인증
{: #kafka_notjava }

SASL PLAIN 및 TLSv1.2를 사용하는 Kafka 0.10을 지원하는 모든 클라이언트는 {{site.data.keyword.messagehub}}를 사용하여 작업해야 합니다. 

클라이언트가 서버의 호스트 이름이 TLS 핸드쉐이크에 포함된 TLS에 대한 SNI 확장을 지원해야 합니다. 

클라이언트 예는 다음과 같습니다.
* [librdkafka ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/edenhill/librdkafka/){:new_window} 
* [confluent-kafka-python ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/confluentinc/confluent-kafka-python){:new_window} 




 




