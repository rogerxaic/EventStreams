---

copyright:
  years: 2015, 2019
lastupdated: "2018-02-06"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.messagehub}}에서 KSQL 사용
{: #ksql_using}

스트림 처리를 위해 {{site.data.keyword.messagehub}}와 함께 [KSQL ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/confluentinc/ksql){:new_window}을 사용할 수 있습니다. 반드시 KSQL 0.4 이상을 사용해야 합니다. 
{: shortdesc}

다음 단계를 완료하십시오.

1. {{site.data.keyword.messagehub}} KSQL 구성 파일을 작성하십시오.

    예:
    ```
    bootstrap.servers=BOOTSTRAP_SERVERS
    application.id=ksql_server_quickstart
    ksql.command.topic.suffix=commands
    listeners=http://localhost:8080
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    ssl.protocol=TLSv1.2
    ssl.enabled.protocols=TLSv1.2
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USERNAME" password="PASSWORD";
    ksql.sink.replications.default=3
    ```
    여기서 BOOTSTRAP_SERVERS, USERNAME 및 PASSWORD는 {{site.data.keyword.Bluemix_notm}}의 {{site.data.keyword.messagehub}} **서비스 인증 정보** 탭에 있는 값입니다.

2. {{site.data.keyword.Bluemix_notm}} 콘솔의 {{site.data.keyword.messagehub}} 대시보드를 사용하여 단일 파티션 및 기본 보존 기간이 포함된
<code>ksql__commands</code>라는 토픽을 작성하십시오.
3. Docker 터미널에서 다음 명령을 사용하여 KSQL을 시작하십시오.
<pre class="pre">/bin/ksql-cli local 
--<var class="keyword varname">properties-file</var> ./config/ksqlserver.properties
</pre>
4. {{site.data.keyword.Bluemix_notm}} 콘솔의 {{site.data.keyword.messagehub}} 대시보드를 사용하여 하나의 파티션이 포함된 두 개의 토픽 즉,
<code>users</code> 및 <code>pageviews</code>를 작성하십시오.

    다음과 같이 <code>DataGen</code>을 시작하십시오.
	
    i. <code>bootstrap-server=HOSTNAME:PORTNUMBER quickstart=users format=json topic=users maxInterval=10000</code>을 사용하여
<code>users</code> 이벤트 작성을 시작합니다.
	
    ii. <code>bootstrap-server=HOSTNAME:PORTNUMBER quickstart=pageviews format=delimited topic=pageviews maxInterval=10000</code>을 사용하여
<code>pageviews</code> 이벤트 작성을 시작합니다.
	
	<code>bootstrap-server</code>의 값으로 **서비스 인증 정보** 페이지에 나열된 모든 Kafka 호스트를 삽입해야 합니다. 이 정보를 찾으려면
{{site.data.keyword.Bluemix_notm}}의 {{site.data.keyword.messagehub}} 인스턴스로 이동하고
**서비스 인증 정보** 탭으로 이동한 후 사용할 **인증 정보**를 선택하십시오.

이 단계를 완료한 경우
[KSQL 빠른 시작
안내서![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/confluentinc/ksql/tree/0.1.x/docs/quickstart#create-a-stream-and-table){:new_window}에 나열된 모든 조회를 실행할 수 있습니다.

