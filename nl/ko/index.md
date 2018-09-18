---

copyright:
  years: 2015, 2018
lastupdated: "2018-07-13"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Message Hub 시작하기 
{: #messagehub}

{{site.data.keyword.messagehub}}를 시작하여
메시지 전송 및 수신을 시작하기 위해 Java™ 샘플을 사용할 수 있습니다. 샘플에서는 제작자가 토픽을 사용하여
이용자에게 메시지를 보내는 방법을 보여줍니다. 메시지를 이용하고 메시지를 생성하는 데 동일한 샘플 프로그램이 사용됩니다.

{{site.data.keyword.messagehub}} 작동 방법을 좀 더 이해하려면 [{{site.data.keyword.messagehub}} 정보](/docs/services/MessageHub/messagehub010.html)를 참조하십시오.

Node.js 및 Python의 샘플을 포함해 기타 {{site.data.keyword.messagehub}} 샘플에 액세스하려면 [{{site.data.keyword.messagehub}} 샘플![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-messaging/message-hub-samples){:new_window}을 참조하십시오.

<!-- 11/01/18 - Karen - removing diagram as requested by James
![Java sample overview diagram](getting_started_sample.gif "Overview diagram of Java sample showing the flow of messages.")
-->

다음 단계를 완료하십시오.
{: #getting_started_steps}
 
1. {{site.data.keyword.messagehub}} 서비스 인스턴스를 작성하십시오.

  a. 웹 사용자 인터페이스를 사용하여 {{site.data.keyword.Bluemix_notm}}에 로그인하십시오. 
  
  b. **카탈로그**를 클릭하십시오.
  
  c. **애플리케이션 서비스** 섹션에서 **{{site.data.keyword.messagehub}} 표준 플랜**을 선택하십시오. {{site.data.keyword.messagehub}} 서비스 인스턴스 페이지가 열립니다.
  
  d. **연결 대상** 메뉴에서 서비스를 바인딩되지 않은 상태로 두고 사용자의 서비스 및 신임 정보에 대한 이름을 입력하십시오. 기본값을 사용할 수 있습니다.
  
  e. **작성**을 클릭하십시오.

2. 아직 설치되어 있지 않다면, 다음 필수 소프트웨어를 설치하십시오.

    * [git ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://git-scm.com/){:new_window}
	* [Gradle ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://gradle.org/){:new_window}
    * Java 8 이상
 
3. 명령행에서 다음 명령을 실행하여 message-hub-samples git 저장소를 복제하십시오.

    <pre class="pre">
    git clone https://github.com/ibm-messaging/message-hub-samples.git
    </pre>
	{: codeblock}

4. 다음 명령을 실행하여 java 콘솔 샘플로 디렉토리를 변경하십시오.

    <pre class="pre">
    cd message-hub-samples/kafka-java-console-sample
    </pre>
	{: codeblock}

5. 다음 빌드 명령을 실행하십시오.

    <pre class="pre">
    gradle clean && gradle build
    </pre>
	{: codeblock}

6. 다음 명령을 실행하여 콘솔에서 이용자를 시작하십시오.

    <pre class="pre">java -jar build/libs/kafka-java-console-sample-2.0.jar 
	<var class="keyword varname">kafka_brokers_sasl</var> <var class="keyword varname">kafka_admin_url</var> token<var class="keyword varname">:api_key</var> -consumer</pre>
    {: codeblock}
    
    샘플은 `kafka-java-console-sample-topic`으로 이름 지정된 토픽을 사용합니다. 토픽이 아직 없는 경우
    샘플은 {{site.data.keyword.messagehub}} 관리 API를 사용하여 토픽을 작성합니다. 메시지를 전송 및 수신하기 위해
    샘플은 Apache Kafka Java API를 사용합니다.

    *kafka_brokers_sasl*, *kafka_admin_url*
    및 *api_key*에 대한 값을 찾으려면 {{site.data.keyword.Bluemix_notm}}의 {{site.data.keyword.messagehub}} 인스턴스로 이동하여 **서비스 신임 정보** 탭으로 이동한 후 사용하려는 **신임 정보**를 선택하십시오.
	
	사용자 이름으로 <code>token</code>을 지정하고 비밀번호로 <var class="keyword varname">api_key</var>를 지정하십시오. 콜론으로 <code>token</code>과 <var class="keyword varname">api_key</var>를 구분하십시오.
    
	**중요:** *kafka_brokers_sasl*은 단일 문자열이어야 하며 따옴표로 묶어야 합니다. 예를 들어, 다음과 같은 경우입니다.

    <pre class="pre">
    "host1:port1,host2:port2"
    </pre>
	{: codeblock}

    선택한 **신임 정보**에 나열된 모든 Kafka 호스트 사용을 권장합니다.

7. 다음 명령을 실행하여 콘솔에서 제작자를 시작하십시오.
   
    <pre class="pre">java -jar build/libs/kafka-java-console-sample-2.0.jar 
	<var class="keyword varname">kafka_brokers_sasl</var> <var class="keyword varname">kafka_admin_url</var> token<var class="keyword varname">:api_key</var> -producer</pre>
 {: codeblock}
  
8. 이제 이용자에 표시되는 제작자가 보낸 메시지를 볼 수 있습니다. 일부 샘플 출력은 다음과 같습니다.

    ```
    [2018-07-02 14:54:50,788] INFO Running in local mode. (com.messagehub.samples.MessageHubConsoleSample)
    [2018-07-02 14:54:50,789] INFO Kafka Endpoints: kafka-0.mh-zarjkgtnzzspbkfrkqgdhmq.us-south.containers.appdomain.cloud:9093,kafka-1.mh-zarjkgtnzzspbkfrkqgdhmq.us-south.containers.appdomain.cloud:9093,kafka-2.mh-zarjkgtnzzspbkfrkqgdhmq.us-south.containers.appdomain.cloud:9093 (com.messagehub.samples.MessageHubConsoleSample)
    [2018-07-02 14:54:50,789] INFO Admin REST Endpoint: https://mh-zarjkgtnzzspbkfrkqgdhmq.us-south.containers.appdomain.cloud (com.messagehub.samples.MessageHubConsoleSample)
    [2018-07-02 14:54:50,789] INFO Creating the topic kafka-java-console-sample-topic (com.messagehub.samples.MessageHubConsoleSample)
    [2018-07-02 14:54:52,680] INFO Admin REST response : (com.messagehub.samples.MessageHubConsoleSample)
    [2018-07-02 14:54:53,351] INFO Admin REST Listing Topics: [{"name":"kafka-java-console-sample-topic","partitions":1,"retentionMs":86400000,"cleanupPolicy":"delete"},{"name":"__consumer_offsets","partitions":50,"retentionMs":86400000,"cleanupPolicy":"compact"}] (com.messagehub.samples.MessageHubConsoleSample)
    [2018-07-02 14:54:55,126] INFO [Partition(topic = kafka-java-console-sample-topic, partition = 0, leader = 0, replicas = [0,2,1], isr = [0,2,1], offlineReplicas = [])] (com.messagehub.samples.ConsumerRunnable)
    [2018-07-02 14:54:55,126] INFO class com.messagehub.samples.ConsumerRunnable is starting. (com.messagehub.samples.ConsumerRunnable)
    [2018-07-02 14:54:56,328] INFO [Partition(topic = kafka-java-console-sample-topic, partition = 0, leader = 0, replicas = [0,2,1], isr = [0,2,1], offlineReplicas = [])] (com.messagehub.samples.ProducerRunnable)
    [2018-07-02 14:54:56,328] INFO MessageHubConsoleSample will run until interrupted. (com.messagehub.samples.MessageHubConsoleSample)
    [2018-07-02 14:54:56,328] INFO class com.messagehub.samples.ProducerRunnable is starting. (com.messagehub.samples.ProducerRunnable)
    [2018-07-02 14:54:57,514] INFO Message produced, offset: 0 (com.messagehub.samples.ProducerRunnable)
    [2018-07-02 14:54:59,652] INFO Message produced, offset: 1 (com.messagehub.samples.ProducerRunnable)
    [2018-07-02 14:55:00,671] INFO No messages consumed (com.messagehub.samples.ConsumerRunnable)
    [2018-07-02 14:55:01,788] INFO Message produced, offset: 2 (com.messagehub.samples.ProducerRunnable)
    [2018-07-02 14:55:01,797] INFO Message consumed: ConsumerRecord(topic = kafka-java-console-sample-topic, partition = 0, offset = 2, CreateTime = 1530539701655, serialized key size = 3, serialized value size = 25, headers = RecordHeaders(headers = [], isReadOnly = false), key = key, value = This is a test message #2) (com.messagehub.samples.ConsumerRunnable)
    [2018-07-02 14:55:03,921] INFO Message consumed: ConsumerRecord(topic = kafka-java-console-sample-topic, partition = 0, offset = 3, CreateTime = 1530539703789, serialized key size = 3, serialized value size = 25, headers = RecordHeaders(headers = [], isReadOnly = false), key = key, value = This is a test message #3) (com.messagehub.samples.ConsumerRunnable)
    [2018-07-02 14:55:03,921] INFO Message produced, offset: 3 (com.messagehub.samples.ProducerRunnable)
    [2018-07-02 14:55:06,053] INFO Message consumed: ConsumerRecord(topic = kafka-java-console-sample-topic, partition = 0, offset = 4, CreateTime = 1530539705922, serialized key size = 3, serialized value size = 25, headers = RecordHeaders(headers = [], isReadOnly = false), key = key, value = This is a test message #4) (com.messagehub.samples.ConsumerRunnable)
    [2018-07-02 14:55:06,054] INFO Message produced, offset: 4 (com.messagehub.samples.ProducerRunnable)
    [2018-07-02 14:55:08,186] INFO Message consumed: ConsumerRecord(topic = kafka-java-console-sample-topic, partition = 0, offset = 5, CreateTime = 1530539708055, serialized key size = 3, serialized value size = 25, headers = RecordHeaders(headers = [], isReadOnly = false), key = key, value = This is a test message #5) (com.messagehub.samples.ConsumerRunnable)
    ```
	{: codeblock}
	
9. 샘플은 사용자가 중지할 때까지 계속 실행됩니다. 프로세스를 중지하려면 <code>Ctrl+C</code>와 같은 명령을 실행하십시오.

<!-- 07/06/18 - Karen: removing until a newer version available
To watch a video that walks
you through getting a Java sample to run against {{site.data.keyword.messagehub}}, see [{{site.data.keyword.messagehub}} - Getting started with IBM's Kafka in the cloud ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.youtube.com/watch?v=tt-bLtFzC_4){:new_window}.
-->



