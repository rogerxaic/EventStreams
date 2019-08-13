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

# Kafka API 사용
{: #kafka_using}

Kafka에서는 다양한 언어로 풍부한 API 및 클라이언트 세트를 제공합니다. 예를 들어, 다음과 같은 경우입니다.

* **Kafka의 코어 API(이용자, 제작자 및 관리자 API)**<br/>
    하나 이상의 Kafka 주제에서 직접 메시지를 보내고 받는 데 사용합니다.
* **스트림 API**<br/>
    주제 간에 이벤트를 쉽게 이용, 변환 및 생성하는 상위 레벨 스트림 처리 API입니다.
* **연결 API**<br/>
    데이터베이스와 같은 외부 시스템에 보내고 내보내는 스트림 이벤트에 대한 재사용 가능 또는 표준 통합이 가능하게 하는 프레임워크입니다.
* **KSQL**<br/>
    SQL과 같은 구문을 사용하여 주제의 이벤트를 처리하고 결합하는 데 사용하는 인터페이스입니다.

다음 표에서는 {{site.data.keyword.messagehub}}와 함께 사용할 수 있는 사항을 요약합니다.

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
		<tr>
			<td>**인증 요구사항**</td>
			<td>클라이언트는 SASL PLAIN 메커니즘을 사용한 인증을 지원해야 하며 TLSv1.2 프로토콜에 대한 SNI(Server Name Indication) 확장을 사용해야 합니다.</td>
			<td>클라이언트는 SASL PLAIN 메커니즘을 사용한 인증을 지원해야 하며 TLSv1.2 프로토콜에 대한 SNI(Server Name Indication) 확장을 사용해야 합니다.</td>
		</tr>

</table>
<br/>
클래식 플랜에서 Kafka API를 사용하는 방법에 대한 정보는 [Kafka API - 클래식](/docs/services/EventStreams?topic=eventstreams-kafka_using_classic)을 참조하십시오.


## {{site.data.keyword.messagehub}}와 사용할 Kafka 클라이언트 선택
{: #kafka_clients}

Kafka API의 공식 클라이언트는 Java로 작성되며 최신 기능과 버그 수정을 포함합니다. 이 API에 관한 자세한 정보는 [Kafka Producer API 2.2 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://kafka.apache.org/22/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html){:new_window} 및
[Kafka Consumer API 2.2 ![](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://kafka.apache.org/22/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html){:new_window}을 참조하십시오. 

다른 언어의 경우 다음 클라이언트 중 하나를 실행하는 것이 좋습니다. 이 모두는 {{site.data.keyword.messagehub}}로 적절하게 테스트를 마쳤습니다.

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
			<td colspan="3">**공식 Apache Kafka 클라이언트**</td>
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
{: #footnote_clients notoc}
1. {: #footnote1 notoc}이 버전은 지속적 테스트에서 유효성 검증한 가장 빠른 버전입니다. 일반적으로 이 버전은 최근 12개월 이내에 사용 가능한 초기 버전이지만 중요한 문제가 있는 것으로 알려진 경우 새 버전일 수 있습니다.

<br/>
나열된 클라이언트를 실행할 수 없으면 다음 최소 요구사항을 만족하는 다른 서드파티 클라이언트를 사용할 수 있습니다(예: [librdkafka ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/edenhill/librdkafka/){:new_window}).
* Kafka 0.10 이상 지원
* TLSv1.2와 SASL PLAIN을 사용하여 연결하고 인증할 수 없음
* 서버의 호스트 이름이 TLS 핸드쉐이크에 포함된 TLS의 SNI 확장기능 지원
* 타원 곡선 암호화 지원
그러나 권장 서드파티 클라이언트를 사용해서만 테스트하고 사용한 경험이 있습니다.

어떤 경우든 최신 버전의 클라이언트를 사용하는 것이 좋습니다.

<br/>
### {{site.data.keyword.messagehub}}에 클라이언트 연결
{: #connect_client}

{{site.data.keyword.messagehub}}에 연결하기 위해 Java 클라이언트를 구성하는 방법에 관한 정보는 [클라이언트 구성](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_api_client)을 참조하십시오.

## Kafka API 클라이언트 구성
{: #kafka_api_client}

연결을 설정하려면 클라이언트가 최소한 TLSv1.2를 통해 SASL_SSL PLAIN을 사용하고 사용자 이름 및 부트스트랩 서버 목록을 요구하도록 구성되어야 합니다. 

사용자 이름, 비밀번호 및 부트스트랩 서버 목록을 검색하려면 서비스 인스턴스에 대한 서비스 인증 정보 오브젝트 또는 서비스 키가 필요합니다. 이러한 오브젝트 작성에 대한 자세한 정보는 <link to Connecting to event Streams>
[{{site.data.keyword.messagehub}}에 연결](/docs/services/EventStreams?topic=eventstreams-connecting)을 참조하십시오.

이러한 오브젝트에서:
* <code>kafka_brokers_sasl</code> 특성을 부트스트랩 서버의 목록으로 사용하십시오. 이 목록을 호스트:포트 항목의 쉼표로 구분된 목록으로 형식화하십시오 (예: <code>host1:port1,host2:port2</code>). <code>kafka_brokers_sasl</code> 특성에 나열된 모든 호스트에 대한 세부사항을 포함하도록 권장합니다.
* <code>user</code> 및 <code>api_key</code> 특성을 사용자 이름 및 비밀번호로 사용하십시오.

클래식 플랜의 서비스 인스턴스에서는 이 정보를 애플리케이션의 VCAP_SERVICES 환경 변수에서 사용할 수 있습니다. 자세한 정보는 [{{site.data.keyword.messagehub}} - 클래식](/docs/services/EventStreams?topic=eventstreams-connecting_classic)을 참조하십시오.

Java 클라이언트의 경우 다음 예제는 최소 특성 세트를 보여줍니다. 여기서 USERNAME, PASSWORD 및 KAFKA_BROKERS_SASL은 이전에 검색된 값으로 대체되어야 합니다.

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
0.10.2.1 이전의 Kafka 클라이언트를 사용 중인 경우 <code>sasl.jaas.config</code> 특성이 지원되지 않으며 대신 JAAS 구성 파일에 클라이언트 구성을 제공해야 합니다. 








 




