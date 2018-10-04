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

# Kafka Java 클라이언트 사용
{: #kafka_using}

<!-- 21/06/18 - removing until some content ready

## To do: instructions for getting started, with links for more information


## To do: simple send source and receive source in-line


## How to use, download, and run the Java Kafka API sample

-->

Java&trade; Kafka API 샘플은 Java로 작성된 제작자 및 이용자 예제이며, Kafka API를 직접 사용합니다. 이 샘플을 로컬에서 또는 {{site.data.keyword.Bluemix_short}}에서 실행할 수 있습니다.

샘플 코드는 [event-streams-samples GitHub 프로젝트 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-console-sample){:new_window}에 있습니다. 샘플이 Kafka API를 사용하여 메시지를 보내고 받더라도, 메시지를 보내고 받는 대상 토픽을 작성하는 데에는 {{site.data.keyword.messagehub}} 관리 API를 사용합니다.

샘플 설정 및 실행에 대한 자세한 정보는 [README.md ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-console-sample){:new_window}를 참조하십시오.

샘플 실행 방법에 대한 자세한 둘러보기는 [{{site.data.keyword.messagehub}} 시작하기](/docs/services/EventStreams/index.html#getting_started_steps)를 참조하십시오. 

## Liberty for Java 샘플 사용, 다운로드 및 실행 방법
{: #liberty_sample notoc}

Liberty for Java 샘플은 Liberty 런타임에 배치된 단순한 애플리케이션을 구현합니다. 애플리케이션은 {{site.data.keyword.messagehub}}에서 메시지를 생성하고 이용하도록 Kafka API를 사용합니다.
또한, 애플리케이션은 관리에 사용할 수 있는 웹 프론트 엔드를 제공합니다.

[event-streams-samples GitHub 프로젝트 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-liberty-sample){:new_window}에서 샘플 코드를 찾을 수 있습니다. 

<!--
17/10/17 - Karen: following info duplicated at messagehub063 
-->

## sasl.jaas.config 특성 사용
{: #sasl_prop notoc}
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

<!--
23/04/18 - Karen: following migration info on production in messagehub084 
-->

## Kafka 클라이언트를 0.9.X 또는 0.10.X에서 이후 클라이언트 버전으로 마이그레이션
{: #kafka_migrate}


Java 클라이언트를 사용 중인 경우에는
오픈 소스 방식으로 사용 가능한 Kafka 클라이언트 0.10 이상을 사용할 수 있습니다. 0.9.X 버전은 최신 버전으로 업그레이드하는 것이
좋습니다. Kafka 클라이언트는
[https://kafka.apache.org/downloads ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://kafka.apache.org/downloads){:new_window}에서 다운로드할 수 있습니다. 



### Kafka 클라이언트를 0.10.2.X 이상 버전으로 마이그레이션

0.10.2부터는 JAAS 파일을 사용하는 대신 클라이언트의 특성에서 직접 SASL 인증을 구성할 수 있습니다. 이러한 단순화를 통해, 다양한 인증 정보 세트를 사용하여 동일한 JVM에서 여러 클라이언트를 실행할 수 있습니다(JAAS 파일을 사용해서는 불가능함).

다음 단계를 완료하십시오.

1. JAAS 파일을 삭제하십시오. JVM 특성 java.security.auth.login.config=<PATH TO JAAS>는 더 이상 필요하지 않다는 점을 유의하십시오.
2. 0.9.X로부터 마이그레이션하는 경우에는 {{site.data.keyword.messagehub}} 로그인 jar 모듈을 삭제하십시오.
2. 다음 항목을 클라이언트의 특성에 추가하십시오.
    ```
	sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USERNAME" password="PASSWORD";
	```

	여기서 USERNAME 및 PASSWORD는 {{site.data.keyword.Bluemix_notm}}의 {{site.data.keyword.messagehub}} **서비스 인증 정보** 탭에 있는 값입니다.
	
	

### Kafka 클라이언트를 0.9.X에서 0.10.0.X 또는 0.10.1.X로 마이그레이션

다음 단계를 완료하십시오.

1. {{site.data.keyword.messagehub}} 로그인 jar 모듈을 삭제하십시오.
2. <code>jaas.conf</code> 파일을 다음으로 변경하십시오.
    ```
        KafkaClient {
          org.apache.kafka.common.security.plain.PlainLoginModule required
          serviceName="kafka"
            username="USERNAME"
            password="PASSWORD";
        };
    ```
    {: codeblock}

	여기서 USERNAME 및 PASSWORD는 {{site.data.keyword.Bluemix_notm}}의 {{site.data.keyword.messagehub}} **서비스 인증 정보** 탭에 있는 값입니다.
	
3. 다음 행을 이용자 및 제작자 특성에 추가하십시오. <code>sasl.mechanism=PLAIN</code>
