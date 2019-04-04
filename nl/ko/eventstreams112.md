---

copyright:
  years: 2015, 2019
lastupdated: "2018-06-23"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 메시지 생성
{: #producing_messages }


제작자는 Kafka 토픽에 메시지 스트림을 공개하는 애플리케이션입니다. 이 정보는 Apache Kafka 프로젝트의 일부인 Java 프로그래밍 인터페이스에 초점을 맞추고 있습니다. 개념은 다른 언어에도 적용되지만 이름은 때때로 조금 다릅니다.
{: shortdesc}

프로그래밍 인터페이스에서 메시지는 실제로 레코드라고 합니다. 예를 들어, Java 클래스 org.apache.kafka.clients.producer.ProducerRecord는 제작자 API 관점에서 메시지를 나타내는 데 사용됩니다. 용어 _레코드_와 _메시지_는 상호 교환적으로 사용될 수 있지만 기본적으로 레코드는 메시지를 나타내는 데 사용됩니다.

제작자가 Kafka에 연결되면 초기 부트스트랩 연결이 이루어집니다. 이 연결은 클러스터에 있는 임의의 서버에 대한 연결일 수 있습니다. 제작자는 공개할 토픽에 대한 파티션 및 리더십 정보를 요청합니다. 그런 다음 제작자는 파티션 리더에 대한 다른 연결을 설정하고 메시지 공개를 시작할 수 있습니다. 제작자가 Kafka 클러스터에 연결되면 이러한 조치가 내부적으로 자동으로 이루어집니다.
 
메시지가 파티션 리더에게 전송되는 경우 이용자가 바로 해당 메시지를 사용할 수는 없습니다. 리더는 파티션에 메시지의 레코드를 추가하며 여기에 해당 파티션의 다음 오프셋 숫자를 지정합니다. 동기화 복제본에 대한 모든 팔로워가 레코드를 복제하고 이 복제본에 레코드를 썼다고 확인하면 레코드가 *커미트*되며 이용자가 이 레코드를 사용할 수 있습니다.

각 메시지는 두 파트(키와 값)로 구성된 레코드로 표시됩니다. 키는 일반적으로 메시지에 대한 데이터에 사용되며 값은 메시지의 본문입니다. Kafka 에코시스템의 많은 도구(예: 다른 시스템에 대한 커넥터)가 값만 사용하고 키는 무시하므로, 값에 모든 메시지 데이터를 넣고 파티셔닝 또는 로그 압축에는 키를 사용하는 것이 좋습니다. 키를 사용하는 데 Kafka에서 읽는 모든 내용이 필요하지는 않습니다.

기타 많은 메시징 시스템에도 메시지와 함께 다른 정보를 전달하는 방법이 있습니다. Kafka 0.11은 이를 위해 레코드 헤더를 도입했습니다. 

{{site.data.keyword.messagehub}}에서 [메시지 이용](/docs/services/EventStreams?topic=eventstreams-consuming_messages)과 관련된 정보를 읽는 것이 좋습니다.

## 구성 설정
{: #config_settings}

제작자에 대한 많은 구성 설정이 있습니다. 일괄처리, 재시도, 메시지 수신확인을 포함한 제작자 측면을 제어할 수 있습니다. 다음은 가장 중요한 내용입니다.

|이름     |설명   |올바른 값   |기본값   |
|----------|---------|----------|---------|
|key.serializer     |키를 직렬화하는 데 사용되는 클래스. |시리얼라이저 인터페이스를 구현하는 Java 클래스(예: org.apache.kafka.common.serialization.StringSerializer) |기본값 없음 - 값을 지정해야 함|
|value.serializer     |값을 직렬화하는 데 사용되는 클래스. |시리얼라이저 인터페이스를 구현하는 Java 클래스(예: org.apache.kafka.common.serialization.StringSerializer)  |기본값 없음 - 값을 지정해야 함 |
|acks     |공개된 각 메시지를 수신확인하는 데 필요한 서버의 수. 이는 제작자에게 필요한 지속성 보장을 제어합니다. |0, 1, all(또는 -1)  |1 |
|retries     |전송 시 오류가 발생할 때 클라이언트가 메시지를 재전송하는 횟수. |0,...  |0 |
|max.block.ms     |전송 또는 메타데이터 요청이 대기를 차단할 수 있는 시간(밀리초). |0,...  |60000(1분) |
|max.in.flight.requests.per.connection     |향후 요청을 차단하기 전에 클라이언트가 연결에 대해 전송하는 최대 미확인 요청 수.|1,...  |5 |
|request.timeout.ms     |제작자가 요청에 대한 응답을 기다리는 최대 시간. 제한시간이 경과하기 전에 응답이 수신되지 않은 경우, 요청이 재시도되고 재시도 수의 한계를 넘으면 요청이 실패합니다.|0,...  |30000(30초) |

많은 추가 구성 설정을 사용할 수 있지만, 이를 시도하기 전에 [Apache Kafka 문서![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://kafka.apache.org/documentation/){:new_window}를 철저히 읽어야 합니다.

## 파티셔닝
{: #partitioning}

제작자가 토픽에 대한 메시지를 공개하는 경우 사용할 파티션을 선택할 수 있습니다. 순서가 중요한 경우, 파티션이 순서가 지정된 레코드 시퀀스이지만 토픽이 하나 이상의 파티션으로 구성되어 있다는 것을 기억해야 합니다. 메시지 세트를 순서대로 전달하려는 경우 이러한 메시지가 모두 동일한 파티션으로 이동해야 합니다. 이를 수행하는 가장 간단한 방법은 모든 메시지에 동일한 키를 제공하는 것입니다. 
 
제작자는 메시지를 공개할 때 파티션 번호를 명시적으로 지정할 수 있습니다. 이렇게 하면 직접적으로 제어할 수 있지만, 파티션 선택사항을 관리해야 하므로 제작자 코드가 복잡해집니다. 자세한 정보는 메소드 호출 Producer.partitionsFor를 참조하십시오. 예를 들어, 호출은
[Kafka 1.1.0![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://kafka.apache.org/11/javadoc/org/apache/kafka/clients/producer/KafkaProducer.html){:new_window}에 대해 설명되어 있습니다.
 
제작자가 파티션 번호를 지정하지 않으면 파티셔너가 파티션을 선택합니다. Kafka 제작자에 빌드된 기본 파티셔너는 다음과 같이 작동합니다.

* 레코드에 키가 없는 경우 라운드 로빈 방식으로 파티션을 선택합니다.
* 레코드에 키가 있는 경우 키의 해시 값을 계산하여 파티션을 선택합니다. 이렇게 하면 동일한 키를 가진 모든 메시지에 대해 동일한 파티션을 선택하게 됩니다.

또한 고유 사용자 정의 파티셔너를 작성할 수 있습니다. 사용자 정의 파티셔너는 임의의 스킴을 선택하여 파티션에 레코드를 지정할 수 있습니다. 예를 들어, 키의 정보 서브세트 또는 애플리케이션 특정 ID를 사용하십시오.


## 메시지 순서 지정
{: #message_ordering}

Kafka는 일반적으로 제작자가 전송한 순서대로 메시지를 씁니다. 하지만 재시도로 인해 메시지가 복제되거나 다시 정렬되는 상황이 있습니다. 메시지 시퀀스를 순서대로 전송하려는 경우 이러한 메시지가 모두 동일한 파티션에 쓰여져야 합니다.
 
제작자는 또한 메시지 전송을 자동으로 재시도할 수 있습니다. 대안이 애플리케이션 코드가 재시도를 수행하는 것이므로 종종 이 재시도 기능을 사용하는 것이 좋습니다. Kafka의 일괄처리와 자동 재시도의 조합으로 인해 메시지가 복제되고 다시 정렬될 수 있습니다.
 
예를 들어, 토픽에 대한 세 메시지의 시퀀스 &lt;M1, M2, M3&gt;를 공개하는 경우가 있습니다. 레코드는 동일한 일괄처리 내에 모두 포함될 수 있으므로 실제로 모두 함께 파티션 리더로 전송됩니다. 그러면 리더는 파티션에 모든 레코드를 쓰고 개별 레코드로 복제합니다. 실패하는 경우 M1 및 M2가 파티션에 추가될 수 있지만 M3는 아닙니다. 제작자는 수신확인을 수신하지 않으므로 &lt;M1, M2, M3&gt; 전송을 재시도합니다. 새 리더는 단순히 M1, M2 및 M3를 파티션에 쓰며, 이 때 파티션에 &lt;M1, M2, M1, M2, M3&gt;가 포함되고 이 경우 복제된 M1은 실제로 원래 M2 뒤에 옵니다. 각 브로커로 전달되는 요청의 수를 하나로 제한하면 다시 정렬을 방지할 수 있습니다. 단일 레코드가 복제된 것을(예: &lt;M1, M2, M2, M3&gt;) 발견할 수 있지만 순서 시퀀스를 제거할 수는 없습니다. Kafka 0.11({{site.data.keyword.messagehub}}에서는 아직 사용할 수 없음)에서는 멱등원(idempotent) 제작자 기능을 사용하여 M2 복제를 방지할 수도 있습니다.
 
전달되는 요청이 하나만 있는 경우의 성능 영향이 크므로 Kafka에서는 가끔 있는 메시지 복제를 처리하도록 애플리케이션을 작성하는 것이 일반적입니다.

## 메시지 수신확인
{: #message_acknowledgments}

메시지를 공개할 때 `acks` 제작자 구성을 사용하여 필요한 수신확인 레벨을 선택할 수 있습니다. 선택사항은 처리량과 신뢰성 간의 밸런스를 나타냅니다. 다음과 같은 세 가지 레벨이 있습니다.

<dl>
<dt>acks=0(신뢰성이 가장 낮음)</dt>
<dd>메시지는 네트워크에 쓰여지자마자 전송된 것으로 간주됩니다. 파티션 리더로부터 수신확인은 없습니다. 따라서 파티션 리더십이 변경되면 메시지가 유실될 수 있습니다. 이 레벨의 수신확인은 매우 빠르지만 어떤 상황에서는 메시지 유실 가능성이 있습니다.</dd>
<dt>acks=1(기본값)</dt>
<dd>파티션 리더가 해당 레코드를 파티션에 쓰자마자 제작자에게 메시지가 수신확인됩니다. 레코드가 동기화 복제본에 기록되기 전에 수신확인이 이루어지므로, 리더가 실패하지만 팔로워가 아직 메시지를 갖지 못한 경우 메시지가 유실될 수 있습니다. 파티션 리더십이 변경되면 이전 리더가 제작자에게 알리고 제작자는 오류를 처리하고 새 리더에게 메시지 전송을 재시도할 수 있습니다. 메시지가 모든 복제본에서 수신이 확인되기 전에 수신확인되므로, 파티션 리더십이 변경되는 경우 수신확인되었지만 아직 완전히 복제되지 않은 메시지는 유실될 수 있습니다.</dd>
<dt>acks=all(신뢰성이 가장 높음)</dt>
<dd>파티션 리더가 해당 레코드를 썼고 모든 동기화 복제본에서 동일하게 수행된 경우 제작자에게 메시지가 수신확인됩니다. 하나 이상의 동기화 복제본을 사용할 수 있는 경우 파티션 리더십이 변경되어도 메시지가 유실되지 않습니다.</dd>
</dl>

제작자에게 메시지가 수신확인될 때까지 기다리지 않더라도 커미트된 메시지는 이용 가능하며 이는 동기화 복제본으로 복제되었음을 의미합니다. 즉, 제작자의 관점에서 메시지 전송 대기 시간은 메시지를 전송하는 제작자로부터 메시지를 수신하는 이용자까지 측정되는 엔드-투-엔드 대기 시간보다 짧습니다.

가능하면 다음 메시지를 공개하기 전에 메시지 수신확인을 기다리지 마십시오. 대기하면 제작자가 메시지를 함께 일괄처리할 수 없으며 메시지를 공개하는 비율이 네트워크의 라운드트립 대기 시간 아래로 줄어듭니다.

## 일괄처리, 조절 및 압축
{: #batching}

제작자는 효율성을 위해 실제로 서버로 보낼 레코드 일괄처리를 수집합니다. 압축을 사용하는 경우, 제작자는 각 일괄처리를 압축하며 이로 인해 네트워크를 통해 전송해야 하는 데이터가 줄어들어 성능이 향상될 수 있습니다.

서버에 전송할 때보다 빨리 메시지를 공개하려는 경우 제작자가 메시지를 일괄처리된 요청으로 자동 버퍼링합니다. 제작자는 각 파티션에 대해 미전송 레코드의 버퍼를 유지합니다. 물론 일괄처리 시에도 원하는 비율에 도달할 수 없는 시점이 옵니다.
 
영향을 미치는 다른 요인도 있습니다. 개별 제작자 또는 이용자가 클러스터에 너무 많지 않도록 {{site.data.keyword.messagehub}}가 처리량 할당을 적용합니다. 각 제작자가 데이터를 전송하는 비율이 계산되고 해당 할당량을 초과하려는 제작자는 조절됩니다. 제작자에 대한 응답 전송을 약간 지연하여 조절이 적용됩니다. 대개 이렇게 하면 자연스럽게 제어됩니다.
 
요약하면 메시지가 공개되면 해당 레코드가 먼저 제작자의 버퍼에 기록됩니다. 백그라운드에서 제작자는 레코드를 일괄처리하고 서버에 전송합니다. 그런 다음 서버는 제작자에게 응답하며, 제작자가 너무 빨리 공개하는 경우 조절 지연을 적용합니다. 제작자의 버퍼가 가득 차면 제작자의 전송 호출이 지연되지만 결국은 예외가 발생하며 실패할 수 있습니다.

## 코드 스니펫
{: #code_snippets}

이러한 코드 스니펫은 관련 개념을 보여주는 상위 레벨에 있습니다. 전체 예는 GitHub에 있는 {{site.data.keyword.messagehub}} 샘플(https://github.com/ibm-messaging/event-streams-samples)을 참조하십시오.

{{site.data.keyword.messagehub}}에 연결하려면 먼저 구성 특성 세트를 빌드해야 합니다. {{site.data.keyword.messagehub}}에 대한 모든 연결은 TLS 및 사용자/비밀번호 인증을 사용하여 보안이 설정되므로 최소한 이러한 특성이 필요합니다. KAFKA_BROKERS_SASL, USER 및 PASSWORD를 고유 서비스 인증 정보로 바꾸십시오.

```
Properties props = new Properties();
 props.put("bootstrap.servers", KAFKA_BROKERS_SASL);
 props.put("sasl.jaas.config", "org.apache.kafka.common.security.plain.PlainLoginModule required username=\"USER\" password=\"PASSWORD\";");
 props.put("security.protocol", "SASL_SSL");
 props.put("sasl.mechanism", "PLAIN");
 props.put("ssl.protocol", "TLSv1.2");
 props.put("ssl.enabled.protocols", "TLSv1.2");
 props.put("ssl.endpoint.identification.algorithm", "HTTPS");
```

메시지를 전송하려면 키 및 값에 시리얼라이저를 지정해야 합니다. 예를 들어, 다음과 같습니다.

```
 props.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
 props.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");
```

그런 다음 KafkaProducer를 사용하여 메시지를 전송하십시오. 여기서 각 메시지는 ProducerRecord로 표시됩니다. 완료되면 KafkaProducer를 닫으십시오. 이 코드는 메시지를 전송하지만 전송이 성공했는지 여부를 확인하기 위해 기다리지는 않습니다.

```
 Producer<String, String> producer = new KafkaProducer<>(props);
 producer.send(new ProducerRecord<String, String>("T1", "key", "value"));
 producer.close();
 ```
 
`send()` 메소드는 비동기식이며 완료를 확인하는 데 사용할 수 있는 Future를 리턴합니다.

```
 Future<RecordMetadata> f = producer.send(new ProducerRecord<String, String>("T1", "key", "value"));
// Do some other stuff
// Now wait for the result of the send
 RecordMetadata rm = f.get();
 long offset = rm.offset; 
```

또는 메시지를 전송할 때 콜백을 제공할 수 있습니다.

```
producer.send(new ProducerRecord<String,String>("T1","key","value", new Callback() {
          public void onCompletion(RecordMetadata metadata, Exception exception) {
                     // This is called when the send completes, either successfully or with an exception
          }
});
```

자세한 정보는 매우 포괄적인 내용을 포함하는 [Kafka 클라이언트의 Javadoc![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://kafka.apache.org/11/javadoc/index.html?overview-summary.html){:new_window}을 참조하십시오. 

