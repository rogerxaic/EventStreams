---

copyright:
  years: 2015, 2019
lastupdated: "2018-03-23"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 메시지 이용
{: #consuming_messages }

이용자는 Kafka 토픽에서 메시지 스트림을 이용하는 애플리케이션입니다. 이용자는 하나 이상의 토픽 또는 파티션을 구독할 수 있습니다. 이 정보는 Apache Kafka 프로젝트의 일부인 Java 프로그래밍 인터페이스에 초점을 맞추고 있습니다. 개념은 다른 언어에도 적용되지만 이름은 때때로 조금 다릅니다.
{: shortdesc}

이용자가 Kafka에 연결되면 초기 부트스트랩 연결이 이루어집니다. 이 연결은 클러스터에 있는 임의의 서버에 대한 연결일 수 있습니다. 이용자는 이용할 토픽에 대한 파티션 및 리더십 정보를 요청합니다. 그런 다음 이용자는 파티션 리더에 대한 다른 연결을 설정하고 메시지 이용을 시작할 수 있습니다. 이용자가 Kafka 클러스터에 연결되면 이러한 조치가 내부적으로 자동으로 이루어집니다.

이용자는 대개 장기 실행 애플리케이션입니다. 이용자는 주기적으로 `Consumer.poll(...)`을 호출하여 Kafka의 메시지를 요청합니다. 이용자는 `poll()`을 호출하고 일괄처리 메시지를 수신하며 즉시 이를 처리한 다음 다시 `poll()`을 호출합니다.

이용자가 메시지를 처리할 때 메시지가 해당 토픽에서 제거되지 않습니다. 대신 이용자는 처리된 메시지를 Kafka에서 알게 하는 방법 중 하나를 선택할 수 있습니다. 이 프로세스는 오프셋 커미트로 알려져 있습니다.

프로그래밍 인터페이스에서 메시지는 실제로 레코드라고 합니다. 예를 들어, Java 클래스 org.apache.kafka.clients.consumer.ConsumerRecord는 이용자 API의 메시지를 나타내는 데 사용됩니다. 용어 _레코드_와 _메시지_는 상호 교환적으로 사용될 수 있지만 기본적으로 레코드는 메시지를 나타내는 데 사용됩니다.

{{site.data.keyword.messagehub}}에서 [메시지 생성](/docs/services/EventStreams?topic=eventstreams-producing_messages)과 관련된 정보를 읽는 것이 좋습니다.

## 이용자 특성 구성 
{: #configuring_consumer_properties }

동작 측면을 제어하는 많은 이용자 구성 설정이 있습니다. 다음은 가장 중요한 내용입니다.

|이름     |설명   |올바른 값   |기본값   |
|----------|---------|----------|---------|
|key.deserializer     |키를 직렬화 해제하는 데 사용되는 클래스. |디시리얼라이저 인터페이스를 구현하는 Java 클래스(예: org.apache.kafka.common.serialization.StringDeserializer)  |기본값 없음 - 값을 지정해야 함|
|value.deserializer     |값을 직렬화 해제하는 데 사용되는 클래스. |디시리얼라이저 인터페이스를 구현하는 Java 클래스(예: org.apache.kafka.common.serialization.StringDeserializer)  |기본값 없음 - 값을 지정해야 함 |
|group.id |이용자가 속한 이용자 그룹의 ID. |문자열 |기본값 없음|
|auto.offset.reset |이용자에게 초기 오프셋이 없거나 클러스터에서 현재 오프셋을 더 이상 사용할 수 없는 경우의 동작. |latest, earliest, none |latest |
|enable.auto.commit |백그라운드에서 이용자의 오프셋을 자동으로 커미트할지 여부를 판별합니다. |true, false |true |
|auto.commit.interval.ms |오프셋에 대한 주기적 커미트 사이의 시간(밀리초). |0,... |5000(5초) |
|max.poll.records |poll()에 대한 호출에서 리턴된 최대 레코드 수. |1,... |500 |
|session.timeout.ms |이용자 그룹의 이용자 멤버십을 유지보수하기 위해 이용자 하트비트를 수신해야 하는 제한시간(밀리초). |6000 - 300000 |10000(10초) |
|max.poll.interval.ms |이용자가 그룹을 떠나기 전 폴 간 최대 시간 간격. |1,... |300000(5분) |
| | | | |

많은 추가 구성 설정을 사용할 수 있지만, 이를 시도하기 전에 [Apache Kafka 문서![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://kafka.apache.org/documentation/){:new_window}를 철저히 읽어야 합니다.

## 이용자 그룹

_이용자 그룹_은 하나 이상의 토픽의 메시지를 이용하기 위해 협력하는 이용자 그룹입니다. 그룹의 이용자는 모두 동일한 값의 `group.id` 구성을 사용합니다. 워크로드를 처리하는 데 둘 이상의 이용자가 필요한 경우 동일한 이용자 그룹에서 여러 이용자를 실행할 수 있습니다. 하나의 이용자만 필요한 경우에도 대개 `group.id` 값을 지정합니다.

각 이용자 그룹에는 그룹의 이용자에 대한 파티션 지정을 책임지는 클러스터의 서버(_조정자_)가 하나씩 있습니다. 이 책임은 로드가 고르게 나뉘도록 클러스터의 서버 전체에 퍼져 있습니다. 이용자에 대한 파티션 지정은 그룹을 재밸런싱할 때마다 변경될 수 있습니다.

이용자는 이용자 그룹에 참여할 때 그룹의 조정자를 검색합니다. 그런 다음 이용자는 조정자에게 그룹에 참여하고 싶다고 알리고 조정자는 새 멤버가 포함된 그룹 전체에서 파티션의 재밸런싱을 시작합니다.

다음 변경 중 하나가 이용자 그룹에서 이루어지면 변경사항을 수용하도록 그룹 멤버에 대한 파티션 지정을 바꿔 그룹이 재밸런싱됩니다.

* 이용자가 그룹에 참여함
* 이용자가 그룹을 떠남
* 조정자에 의해 이용자가 더 이상 기능하지 않는 것으로 간주됨
* 새 파티션이 기존 토픽에 추가됨

각 이용자 그룹에 대해 Kafka는 이용 중인 각 파티션의 커미트된 오프셋을 기억합니다.

재밸런싱된 이용자 그룹이 있는 경우, 그룹을 떠난 이용자가 그룹에 다시 참여할 때까지 해당 커미트는 거부됩니다. 이 경우 이용자는 그룹에 다시 참여해야 하며 이전에 이용한 파티션에 다른 파티션이 지정될 수 있습니다.

## 이용자 활성 상태(liveness)

Kafka는 작업 중인 이용자에게 파티션을 다시 지정할 수 있도록 실패한 이용자를 자동으로 발견합니다. 이를 위해 폴링과 하트비트라는 두 가지 메커니즘을 사용합니다.

`Consumer.poll(...)`에서 리턴된 일괄처리 메시지가 크거나 처리에 시간이 걸리는 경우, `poll()`을 다시 호출하기 전까지 지연 시간은 아주 길거나 예측 불가능할 수 있습니다. 어떤 경우에는, 메시지 처리에 시간이 걸린다는 이유로 이용자가 그룹에서 제거되지 않도록 최대 폴링 간격을 길게 구성해야 합니다. 이것이 유일한 메커니즘인 경우 실패한 이용자 발견에 걸리는 시간도 길어집니다.

이용자 활성 상태(liveness) 확인을 쉽게 처리할 수 있도록 Kafka 0.10.1에 백그라운드 하트비트가 추가되었습니다. 그룹 조정자는 그룹 멤버가 활성임을 표시하기 위해 주기적으로 하트비트를 전송하도록 요청합니다. 백그라운드 하트비트 스레드는 조정자에게 주기적으로 하트비트를 전송하는 이용자에서 실행됩니다. 조정자는 _세션 제한시간_ 내에 그룹 멤버로부터 하트비트를 수신하지 못하면 그룹에서 멤버를 제거하고 그룹의 재밸런싱을 시작합니다. 메시지 처리에 시간이 오래 걸리더라도 실패한 이용자를 발견하는 데 걸리는 시간은 짧도록 세션 제한시간이 최대 폴링 간격보다 훨씬 짧을 수 있습니다.

`max.poll.interval.ms` 특성을 사용하여 최대 폴링 간격을 구성하고 `session.timeout.ms` 특성을 사용하여 세션 제한시간을 구성할 수 있습니다. 일괄처리 메시지 처리에 걸리는 시간이 5분을 넘지 않는 한 일반적으로 이러한 설정을 사용하지 않아도 됩니다.

## 오프셋 관리

각 이용자 그룹에 대해 Kafka는 이용 중인 각 파티션의 커미트된 오프셋을 유지보수합니다. 이용자는 메시지를 처리할 때 파티션에서 제거하지 않습니다. 대신 오프셋 커미트라고 하는 프로세스를 사용하여 현재 오프셋을 업데이트합니다.

{{site.data.keyword.messagehub}}는 커미트된 오프셋 정보를 7일 동안 보존합니다.

### 커미트된 기존 오프셋이 없는 경우에는 어떻게 됩니까?
이용자가 시작되고 이용할 파티션이 지정되면 그룹의 커미트된 오프셋에서 시작합니다. 커미트된 기존 오프셋이 없는 경우 이용자는 다음과 같이 `auto.offset.reset` 특성의 설정에 따라 최초 사용 가능 메시지에서 시작되는지 아니면 최근 메시지에서 시작되는지를 선택할 수 있습니다.

- `latest`(기본값). 이용자는 구독 후 도착한 메시지만 수신하고 이용합니다. 이용자가 구독 전에 전송된 메시지를 확인하지 못하므로 토픽에서 모든 메시지가 이용되지는 않습니다.
- `earliest`. 이용자가 전송된 모든 메시지를 알게 되므로 처음부터 모든 메시지를 이용합니다.

이용자가 해당 오프셋을 커미트하기 전에 메시지를 처리한 후 실패하면 커미트된 오프셋 정보가 메시지 처리에 영향을 주지 않습니다. 즉, 파티션이 지정되는 해당 그룹의 다음 이용자가 메시지를 다시 처리합니다.

커미트된 오프셋이 Kafka에 저장되고 이용자가 다시 시작되면 이용자가 마지막으로 중지한 지점에서 재개됩니다. 커미트된 오프셋이 있으면 `auto.offset.reset` 특성이 사용되지 않습니다.

### 오프셋 자동 커미트

오프셋을 커미트하는 가장 쉬운 방법은 Kafka 이용자가 자동으로 수행하게 하는 것입니다. 이는 간단하지만 수동 커미트보다 통제하기 어렵습니다. 기본적으로 이용자는 5초마다 자동으로 오프셋을 커미트합니다. 이 기본 커미트는 이용자의 메시지 처리 진행상태와 관계 없이 5초마다 수행됩니다. 또한 이용자가 `poll()`을 호출하면 이로 인해 `poll()`에 대한 이전 호출에서 리턴된 최신 오프셋이 커미트됩니다(아마도 처리되었기 때문에).

커미트된 오프셋이 메시지 처리를 앞선 상태에서 이용자 실패가 발생한 경우에는 일부 메시지가 처리되지 않을 수 있습니다. 이는 커미트된 오프셋에서 처리가 다시 시작되기 때문이며, 이 오프셋은 실패 전에 처리할 마지막 메시지보다 늦습니다. 이러한 이유로 신뢰성이 단순성보다 중요한 경우 대개 수동으로 오프셋을 커미트하는 것이 좋습니다.

### 오프셋 수동 커미트

`enable.auto.commit`가 `false`로 설정되면 이용자가 수동으로 해당 오프셋을 커미트합니다. 동기식으로 또는 비동기식으로 이를 수행할 수 있습니다. 일반적인 패턴은 주기적 타이머에 따라 최신 처리 메시지의 오프셋을 커미트하는 것입니다. 이 패턴은 모든 메시지가 한 번 이상 처리되지만, 커미트된 오프셋이 현재 처리되고 있는 메시지의 진행상태를 결코 앞서지는 않음을 의미합니다. 주기적 타이머의 빈도는 이용자 실패 후에 다시 처리할 수 있는 메시지 수를 제어합니다. 애플리케이션이 다시 시작되거나 그룹이 재밸런싱되면 메시지가 마지막으로 저장된 커미트된 오프셋부터 다시 검색됩니다.

커미트된 오프셋은 처리가 재개된 메시지의 오프셋입니다. 이는 대개 가장 최근에 처리된 메시지 *더하기 하나*인 오프셋입니다.

### 이용자 지연 시간

파티션에 대한 이용자 지연 시간은 가장 최근에 공개된 메시지의 오프셋과 이용자의 커미트된 오프셋 간 차이입니다. 생성 속도와 이용 속도에서 대개 자연적인 변화가 있지만 연장된 기간 동안 이용 속도가 생성 속도보다 느리면 안됩니다.

이용자가 메시지를 성공적으로 처리하지만 때때로 메시지 그룹을 건너뛰면 이용자가 뒤쳐지는 것일 수 있습니다. 로그 압축을 사용하지 않는 토픽의 경우 이전 로그 세그먼트를 주기적으로 삭제하여 로그 공간의 크기를 관리합니다. 이용자가 너무 뒤쳐져서 삭제된 로그 세그먼트의 메시지를 이용하는 경우 다음 로그 세그먼트의 처음으로 건너뜁니다. 이용자가 모든 메시지를 처리해야 하는 경우 이 동작은 이 이용자의 관점에서 메시지 유실을 나타냅니다.

<code>kafka-consumer-groups</code> 도구를 사용하여 이용자 지연 시간을 확인할 수 있습니다. 또한 동일한 목적을 위해 이용자 API 및 이용자 메트릭을 사용할 수 있습니다.


## 메시지 이용 속도 제어
{: #message_consumption_speed }

메시지 넘침으로 인해 메시지 처리에 문제점이 발생하면 메시지 이용 속도를 제어하도록 이용자 옵션을 설정할 수 있습니다. `fetch.max.bytes` 및 `max.poll.records`를 사용하여 `poll()`에 대한 호출이 리턴할 수 있는 데이터 양을 제어하십시오.


## 이용자 재밸런싱 처리
이용자가 그룹에 추가되거나 그룹에서 제거되면 그룹 재밸런싱이 이루어지고 이용자는 메시지를 이용할 수 없습니다. 그 결과 이용자 그룹의 모든 이용자는 짧은 기간 동안 사용할 수 없게 됩니다.

ConsumerRebalanceListener를 사용하면 "on partitions revoked" 콜백으로 알림을 받을 때 수동으로 오프셋을 커미트하고(자동 커미트를 사용하지 않는 경우) "on partition assigned" 콜백을 사용하여 재밸런싱 성공에 대한 알림을 받을 때까지 추가 처리를 일시정지할 수 있습니다.


## 코드 스니펫
{: #consumer_code_snippets notoc}

이러한 코드 스니펫은 관련 개념을 보여주는 상위 레벨에 있습니다. 전체 예는 GitHub에 있는 {{site.data.keyword.messagehub}} 샘플(https://github.com/ibm-messaging/event-streams-samples) 을 참조하십시오.

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

메시지를 이용하려면 키 및 값에 디시리얼라이저를 지정해야 합니다. 예를 들어, 다음과 같습니다.

```
 props.put("key.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
 props.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
```

그런 다음 KafkaConsumer를 사용하여 메시지를 이용하십시오. 여기서 각 메시지는 ConsumerRecord로 표시됩니다. 메시지를 이용하는 가장 일반적인 방법은 그룹 ID를 설정하여 이용자 그룹에 이용자를 포함시킨 다음 토픽 목록에 대해 `subscribe()`를 호출하는 것입니다. 이용자에게 이용할 몇 가지 파티션이 지정되지만, 토픽의 파티션보다 그룹의 이용자가 많은 경우 이용자에 파티션을 지정할 수 없습니다. 그 다음, 루프에서 `poll()`을 호출하십시오. 처리할 일괄처리 메시지가 수신되며, 각 메시지는 ConsumerRecord로 표시됩니다.

```
props.put("group.id", "G1");
Consumer<String, String> consumer = new KafkaConsumer<>(props);
consumer.subscribe(Arrays.asList("T1"));
while (true) {
   ConsumerRecords<String, String> records = consumer.poll(100);
   for (ConsumerRecord<String, String> record : records)
     System.out.printf("offset = %d, key = %s, value = %s%n", record.offset(), record.key(), record.value());
}
```

이 이용자 루프는 영원히 실행되지만 `Consumer.wakeup()`을 호출하는 다른 스레드에 의해 인터럽트되어 완전히 종료될 수 있습니다.

오프셋을 수동으로 커미트하려면 먼저 `enable.auto.commit` 구성을 `false`로 설정해야 합니다. 그런 다음 `Consumer.commmitSync()` 또는 `Consumer.commitAsync()`를 사용하여 이용자의 커미트된 오프셋을 주기적으로 업데이트하십시오. 단순성을 위해 이 예제는 각 파티션의 레코드를 처리하고 마지막 오프셋을 별도로 커미트합니다.

```
props.put("group.id", "G1");
props.put("enable.auto.commit", "false");
Consumer<String, String> consumer = new KafkaConsumer<>(props);
consumer.subscribe(Arrays.asList("T1"));
try {
  while (true) {
    ConsumerRecords<String, String> records = consumer.poll(100);
    for (TopicPartition tp : records.partitions()) {
      List<ConsumerRecord<String, String>> partRecords = records.records(tp);
      long lastOffset = 0;
      for (ConsumerRecord<String, String> record : partRecords) {
        System.out.printf("offset = %d, key = %s, value = %s%n", record.offset(), record.key(), record.value());
        lastOffset = record.offset();
      }

      consumer.commitSync(Collections.singletonMap(tp, new OffsetAndMetadata(lastOffset + 1)));
    }
  }
}
finally {
  consumer.close();
}
```

## 예외 처리

Kafka 클라이언트를 사용하는 강력한 애플리케이션은 예상되는 특정 상황에 대한 예외를 처리해야 합니다. 어떤 경우에는 일부 메소드가 비동기식이고 `Future` 또는 콜백을 사용하여 해당 결과를 전달하므로 예외가 직접적으로 발생하지 않습니다. [GitHub![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-messaging/event-streams-samples) 에서 전체 예제를 보여주는 코드 예제를 찾을 수 있습니다.

다음은 코드에서 처리해야 하는 예외 목록입니다.

### org.apache.kafka.common.errors.WakeupException
`Consumer.wakeup()` 호출의 결과로 `Consumer.poll(...)`에서 발생합니다. 이는 이용자의 폴링 루프를 인터럽트하는 표준 방식입니다. 폴링 루프를 종료하고 `Consumer.close()`를 호출하여 연결을 완전하게 끊습니다.
### org.apache.kafka.common.errors.NotLeaderForPartitionException
파티션의 리더십이 변경될 때 `Producer.send(...)`의 결과로 발생합니다. 클라이언트가 자동으로 해당 메타데이터를 새로 고쳐 최신 리더 정보를 찾습니다. 조작을 재시도하십시오. 그러면 업데이트된 메타데이터에서 성공하게 됩니다.
### org.apache.kafka.common.errors.CommitFailedException
복구 불가능한 오류가 발생할 때 `Consumer.commitSync(...)`의 결과로 발생합니다. 어떤 경우에는, 파티션 지정이 변경되고 이용자가 해당 오프셋을 더 이상 커미트할 수 없어 조작을 반복할 수 없습니다. `Consumer.commitSync(...)`가 단일 호출 시 여러 파티션에서 사용될 때 부분적으로 성공할 수 있으므로 각 파티션에 개별 `Consumer.commitSync(...)` 호출을 사용하여 오류 복구를 단순화할 수 있습니다.
### org.apache.kafka.common.errors.TimeoutException
메타데이터를 검색할 수 없는 경우 `Producer.send(...),  Consumer.listTopics()`에서 발생합니다. 이 예외는 요청된 수신확인이 `request.timeout.ms` 내에 반환되지 않을 때의 전송 콜백(또는 리턴된 Future)에도 표시됩니다. 클라이언트가 조작을 재시도할 수 있지만 반복된 조작의 결과는 특정 조작에 따라 다릅니다. 예를 들어, 메시지 전송이 재시도되면 메시지가 중복될 수 있습니다.

