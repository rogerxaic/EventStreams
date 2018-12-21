---

copyright:
  years: 2015, 2018
lastupdated: "2018-06-22"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Kafka API 클라이언트 구성
{: #kafka_connect}


{{site.data.keyword.messagehub}}에 연결하기 위해 Kafka API는 다음 인증 정보 세트 중 하나를 사용합니다. 
* [VCAP_SERVICES 환경 변수](/docs/services/EventStreams/eventstreams127.html#vcap)의
<code>user</code> 및 <code>password</code>와 <code>kafka_brokers_sasl</code> 인증 정보
* 서비스 키. 자세한 정보는 [클러스터에 연결](/docs/services/EventStreams/eventstreams127.html#enterprise_connect)을 참조하십시오.


<!--17/10/17 - Karen: following info duplicated at messagehub104 -->
## sasl.jaas.config 특성 사용(Java 애플리케이션의 연결 및 인증)
{: #kafka_java notoc}
Kafka 클라이언트를 0.10.2.1 이상에서 사용 중인 경우 JAAS 파일 대신 클라이언트 구성에 <code>sasl.jaas.config</code> 특성을 사용할 수 있습니다. {{site.data.keyword.messagehub}}에 연결하려면 <code>sasl.jaas.config</code>를 다음과 같이 설정하십시오.
<pre>
<code>    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required \
    username="USERNAME" \
    password="PASSWORD";</code>
</pre>
{:codeblock}

여기서 USERNAME 및 PASSWORD는 {{site.data.keyword.Bluemix_notm}}의 {{site.data.keyword.messagehub}} **서비스 인증 정보** 탭에 있는 값입니다.

<code>sasl.jaas.config</code>를 사용하는 경우 동일한 JVM에서 실행 중인 클라이언트가 다른 인증 정보를 사용할 수 있습니다. 자세한 정보는
[Kafka 클라이언트 구성 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://kafka.apache.org/documentation/#security_sasl_plain_clientconfig){:new_window}을 참조하십시오.

초기 Kafka 클라이언트에서 인증 정보를 지정하려면 JAAS 구성 파일을 사용해야 합니다. 이 메커니즘의 편의성이 부족하므로, 대신 <code>sasl.jaas.config</code> 특성을 사용하는 것이 좋습니다.
## Java 이외의 애플리케이션에서 연결 및 인증
{: #kafka_notjava notoc}

{{site.data.keyword.messagehub}} 서비스는 현재 TLS로 SASL PLAIN을 사용하여 클라이언트를 인증합니다. 인증 정보는 암호화된 연결을 통해 계속 이어집니다.
이는 Kafka 0.10.0.X에 추가된 새 기능입니다. 

다음 예제는 <code>consumer.properties</code>라는 샘플 구성 파일입니다.

```
key.deserializer=org.apache.kafka.common.serialization.StringDeserializer
value.deserializer=org.apache.kafka.common.serialization.StringDeserializer
#
client.id=kafka-java-console-sample-consumer
group.id=kafka-java-console-sample-group
#
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USERNAME" password="PASSWORD";
ssl.protocol=TLSv1.2
ssl.enabled.protocols=TLSv1.2
ssl.endpoint.identification.algorithm=HTTPS
#
# please read the Kafka docs about this setting
auto.offset.reset=latest
```
{: codeblock}

SASL PLAIN으로 Kafka 0.10을 지원하는 모든 클라이언트는
{{site.data.keyword.messagehub}}에 대해 작업해야 합니다. 클라이언트 예는 다음과 같습니다.

* [librdkafka ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/edenhill/librdkafka/){:new_window} 
* [confluent-kafka-python ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/confluentinc/confluent-kafka-python){:new_window} 

초기 Kafka 0.9.0.0 클라이언트를 사용하는 경우
[{{site.data.keyword.messagehub}} 로그인 모듈
![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-0.9/message-hub-login-library/messagehub.login-1.0.0.jar){:new_window}에서 다운로드할 수 있는 사용자 정의 로그인 모듈을 사용해야 합니다. 

