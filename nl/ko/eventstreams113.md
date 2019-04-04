---

copyright:
  years: 2015, 2019
lastupdated: "2017-11-02"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.messagehub}}와 함께 Kafka Connect 사용
{: #kafka_connect }

{{site.data.keyword.messagehub}}와 함께 Kafka Connect를 사용하고 {{site.data.keyword.Bluemix_short}} 내부 또는 외부에서 작업자를 실행할 수 있습니다.
{: shortdesc}

Kafka Connect는 독립형 또는 분산 모드로 실행될 수 있습니다. 독립형 모드는 테스트 및 시스템 간의 임시 연결에 사용됩니다. 분산 모드는
프로덕션 사용에 좀 더 적합합니다. 이러한 두 개의 모드로 {{site.data.keyword.messagehub}}를 사용하는 데 필요한 구성은 약간 다릅니다.
{:shortdesc}

## 독립형 작업자 구성
{: #standalone_worker notoc}

Kafka Connect 독립형 작업자를 시작할 때 제공하는 작업자 특성 파일에 부트스트랩 서버 및 SASL 인증 정보를 제공해야 합니다.

독립형 작업자는 내부 토픽을 사용하지 않습니다. 대신, 오프셋 정보를 저장하기 위해 파일을 사용합니다.

### 소스 커넥터
{: #source_connector notoc }

다음 예제는 특성 파일에 제공해야 하는 특성을 나열합니다.

<pre>
<code>
  bootstrap.servers=KAFKA_BROKERS_SASL
  producer.sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  producer.security.protocol=SASL_SSL
  producer.sasl.mechanism=PLAIN
  producer.ssl.protocol=TLSv1.2
  producer.ssl.enabled.protocols=TLSv1.2
  producer.ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

KAFKA_BROKERS_SASL, USER 및 PASSWORD를 {{site.data.keyword.Bluemix_notm}} 콘솔의
{{site.data.keyword.messagehub}} **서비스 인증 정보**에 있는 값으로 대체하십시오.

### 싱크 커넥터
{: #sink_connector }

다음 예제는 특성 파일에 제공해야 하는 특성을 나열합니다.

<pre>
<code>
  bootstrap.servers=KAFKA_BROKERS_SASL
  consumer.sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  consumer.security.protocol=SASL_SSL
  consumer.sasl.mechanism=PLAIN
  consumer.ssl.protocol=TLSv1.2
  consumer.ssl.enabled.protocols=TLSv1.2
  consumer.ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

KAFKA_BROKERS_SASL, USER 및 PASSWORD를 {{site.data.keyword.Bluemix_notm}} 콘솔의
{{site.data.keyword.messagehub}} **서비스 인증 정보**에 있는 값으로 대체하십시오.

## 분산 작업자 구성
{: #distributed_worker notoc}

Kafka Connect 분산 작업자를 시작할 때 제공하는 구성 파일에 부트스트랩 서버 및 SASL 인증 정보를 제공해야 합니다. 다음 예제는 특성 파일에 제공해야 하는 특성을 나열합니다.

<pre>
<code>
  bootstrap.servers=KAFKA_BROKERS_SASL
  sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  security.protocol=SASL_SSL
  sasl.mechanism=PLAIN
  ssl.protocol=TLSv1.2
  ssl.enabled.protocols=TLSv1.2
  ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

KAFKA_BROKERS_SASL, USER 및 PASSWORD를 {{site.data.keyword.Bluemix_notm}} 콘솔의
{{site.data.keyword.messagehub}} **서비스 인증 정보**에 있는 값으로 대체하십시오.

소스 커넥터를 사용하려는 경우에는 다음과 같이 제작자를 위한 SSL 및 SASL 구성도 지정해야 합니다.

<pre>
<code>
  producer.sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  producer.security.protocol=SASL_SSL
  producer.sasl.mechanism=PLAIN
  producer.ssl.protocol=TLSv1.2
  producer.ssl.enabled.protocols=TLSv1.2
  producer.ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

싱크 커넥터를 사용할 경우 다음과 같이 고객을 위한 SSL 및 SASL 구성도 지정해야 합니다.

<pre>
<code>
  consumer.sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  consumer.security.protocol=SASL_SSL
  consumer.sasl.mechanism=PLAIN
  consumer.ssl.protocol=TLSv1.2
  consumer.ssl.enabled.protocols=TLSv1.2
  consumer.ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

또한, 분산 모드의 Kafka Connect는 내부적으로 세 개의 토픽을 사용합니다. 이러한 토픽은 Apache Kafka 버전 0.11 이상에서 Kafka Connect를 사용하는 경우 작업자 시작 시에 자동으로 작성됩니다. 구성 매개변수로 토픽의 이름을 제공합니다. 동일한 `group.id` 구성 값을 갖는 모든 작업자에 대해 값이 동일한지 확인하십시오.

|구성               |설명                                                         |
| --------------------------- | ------------------------------------------------------------------- |
|`offset.storage.topic`      |커넥터 오프셋 토픽                                             |
|`offset.storage.partitions` |커넥터 오프셋 토픽의 파티션 수(기본값: 25) |
|`config.storage.topic`      |커넥터 구성 토픽                                       |
|`status.storage.topic`      |커넥터 상태 토픽                                              |
|`status.storage.partitions` |커넥터 상태 토픽의 파티션 수(기본값: 5)          |

예를 들어, 특성 파일에서 다음 키-값 쌍을 사용할 수 있습니다.

<pre>
<code>
  offset.storage.topic=connect-offsets
  config.storage.topic=connect-configs
  status.storage.topic=connect-status
</code>
</pre>
{:codeblock}

Kafka Connect를 많이 사용하지 않는 경우에는 파티션 수를 줄이는 것을 고려하십시오.



