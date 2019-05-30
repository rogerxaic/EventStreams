---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-07"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}


# {{site.data.keyword.messagehub}}와 함께 Kafka 콘솔 사용
{: #kafka_console_tools }

간편한 관리 및 메시징 조작을 위해 Apache Kafka는 다양한 콘솔 도구와 함께 제공됩니다. {{site.data.keyword.messagehub}}에서 ZooKeeper
클러스터에 대한 연결이 허용되지 않는 경우에도 {{site.data.keyword.messagehub}}와 함께 해당 콘솔 도구를 사용할 수 있습니다. Kafka가 발전함에 따라, ZooKeeper에 연결하기 위해 이전에 필요했던 도구는 더 이상 필요하지 않습니다.
{: shortdesc}

이러한 콘솔 도구는 Kafka 다운로드의 <code>bin</code> 디렉토리에서 찾을 수 있습니다. [Apache Kafka 다운로드 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://kafka.apache.org/downloads){:new_window}에서 클라이언트를 다운로드할 수 있습니다.

이 도구에 SASL 인증 정보를 제공하려면 다음 예제에 따라 특성 파일을 작성해야 합니다.

<pre>
<code>
  sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  security.protocol=SASL_SSL
  sasl.mechanism=PLAIN
  ssl.protocol=TLSv1.2
  ssl.enabled.protocols=TLSv1.2
  ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

USER 및 PASSWORD를 {{site.data.keyword.Bluemix_notm}} 콘솔의
{{site.data.keyword.messagehub}} **서비스 인증 정보**에 있는 값으로 대체하십시오.


## 콘솔 제작자
{: #console_producer }

{{site.data.keyword.messagehub}}와 함께 Kafka 콘솔 제작자 도구를 사용할 수 있습니다. 브로커 및 SASL 인증 정보의 목록을 제공해야 합니다.

이전에 설명된 대로 특성 파일을 작성한 후 다음과 같이 터미널에서 콘솔 제작자를 실행할 수 있습니다.

<pre>
<code>
  $ kafka-console-producer.sh --broker-list KAFKA_BROKERS_SASL --producer.config CONFIG_FILE --topic TOPIC_NAME
</code>
</pre>
{:codeblock}

예제에 있는 다음 변수를 고유한 값으로 대체하십시오.
* KAFKA_BROKERS_SASL - 쉼표로 구분된 host:port 쌍(예: `host1:port1,host2:port2`)의 목록으로
{{site.data.keyword.Bluemix_notm}} 콘솔의
{{site.data.keyword.messagehub}} **서비스 인증 정보** 탭에 있는 값 포함 
* CONFIG_FILE - 구성 파일 경로 포함 

ZooKeeper에 액세스하는 데 필요한 옵션을 제외하고 이 도구의 다른 다양한 옵션을 사용할 수 있습니다.


## 콘솔 이용자
{: #console_consumer }

{{site.data.keyword.messagehub}}와 함께 Kafka 콘솔 이용자 도구를 사용할 수 있습니다. 부트스트랩 서버 및 SASL 인증 정보를 제공해야 합니다.

이전에 설명된 대로 특성 파일을 작성한 후 다음과 같이 터미널에서 콘솔 이용자를 실행할 수 있습니다.

<pre>
<code>
  $ kafka-console-consumer.sh --bootstrap-server KAFKA_BROKERS_SASL --consumer.config CONFIG_FILE --topic TOPIC_NAME 
</code>
</pre>
{:codeblock}

예제에 있는 다음 변수를 고유한 값으로 대체하십시오.
* KAFKA_BROKERS_SASL - 쉼표로 구분된 host:port 쌍(예: `host1:port1,host2:port2`)의 목록으로
{{site.data.keyword.Bluemix_notm}} 콘솔의
{{site.data.keyword.messagehub}} **서비스 인증 정보** 탭에 있는 값 포함 
* CONFIG_FILE - 구성 파일 경로 포함 

ZooKeeper에 액세스하는 데 필요한 옵션을 제외하고 이 도구의 다른 다양한 옵션을 사용할 수 있습니다.


## 이용자 그룹
{: #consumer_groups_tool }

{{site.data.keyword.messagehub}}와 함께 Kafka 이용자 그룹 도구를 사용할 수 있습니다. {{site.data.keyword.messagehub}}에서
ZooKeeper 클러스터의 연결을 허용하지 않으므로 일부 옵션을 사용할 수 없습니다.

이전에 설명된 대로 특성 파일을 작성한 후 터미널에서 이용자 그룹 도구를 실행할 수 있습니다. 예를 들어, 다음과 같이 이용자 그룹을 나열할 수 있습니다.

<pre>
<code>
  $ kafka-consumer-groups.sh --bootstrap-server KAFKA_BROKERS_SASL --command-config CONFIG_FILE --list
</code>
</pre>
{:codeblock}

예제에 있는 다음 변수를 고유한 값으로 대체하십시오.
* KAFKA_BROKERS_SASL - 쉼표로 구분된 host:port 쌍(예: `host1:port1,host2:port2`)의 목록으로
{{site.data.keyword.Bluemix_notm}} 콘솔의
{{site.data.keyword.messagehub}} **서비스 인증 정보** 탭에 있는 값 포함 
* CONFIG_FILE - 구성 파일 경로 포함

이 도구를 사용하여 이용자의 현재 위치, 해당 지연 시간 및 그룹의 각 파티션에 대한 client-id와 같은 세부사항을 표시할 수도 있습니다. 예:

<pre>
<code>
  $ kafka-consumer-groups.sh --bootstrap-server KAFKA_BROKERS_SASL --command-config CONFIG_FILE --describe --group GROUP
</code>
</pre>
{:codeblock}

예제에 있는 GROUP을 세부사항을 검색할 그룹 이름으로 대체하십시오. 


## 토픽
{: #topics_tool }

도구에는 ZooKeeper에 대한 액세스 권한이 필요하므로 {{site.data.keyword.messagehub}}와 함께 Kafka 토픽 도구 `kafka-topics`를
사용할 수 없습니다.

그러나 {{site.data.keyword.Bluemix_notm}} 콘솔의 {{site.data.keyword.messagehub}} 대시보드를 사용하거나
REST API를 사용하여 토픽을 관리할 수 있습니다.


## Kafka Streams 재설정
{: #kafka_streams_reset }

{{site.data.keyword.messagehub}}와 함께 이 도구를 사용하여 Kafka Streams 애플리케이션의 처리 상태를 재설정하면 스크래치에서 해당 입력을 다시 처리할 수 있습니다. 이 도구를 실행하기 전에 Streams 애플리케이션이 완전히 중지되어야 합니다.

예:

<pre>
<code>
  $ kafka-streams-application-reset.sh --bootstrap-servers KAFKA_BROKERS_SASL --config-file CONFIG_FILE --application-id APP_ID
</code>
</pre>
{:codeblock}

예제에 있는 다음 변수를 고유한 값으로 대체하십시오.
* KAFKA_BROKERS_SASL - 쉼표로 구분된 host:port 쌍(예: `host1:port1,host2:port2`)의 목록으로
{{site.data.keyword.Bluemix_notm}} 콘솔의
{{site.data.keyword.messagehub}} **서비스 인증 정보** 탭에 있는 값 포함 
* CONFIG_FILE - 구성 파일 경로 포함 
* APP_ID - Streams 애플리케이션 ID 포함

