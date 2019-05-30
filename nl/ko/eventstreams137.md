---

copyright:
  years: 2015, 2019
lastupdated: "2018-05-25"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 클래식 플랜에서 Kafka REST API 사용 
{: #rest_using_classic}

** 이 버전의 Kafka REST API는 클래식 플랜의 일부로만 사용 가능합니다.**
<br/>

Kafka REST API는 Kafka 클러스터에 RESTful 인터페이스를 제공합니다. API를 사용하여 메시지를 생성해서 이용할 수 있습니다. API 참조 문서를 포함한 자세한 정보는 [Kafka REST Proxy 문서 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.confluent.io/2.0.0/kafka-rest/docs/index.html){:new_window}를 참조하십시오. {{site.data.keyword.messagehub}}의 요청 및 응답에는 2진 임베디드 형식만 지원됩니다. Avro 및 JSON 임베디드 형식은 지원되지 않습니다.
{: shortdesc}

CURL을 사용하는 경우 다음과 같은 예제를 사용하여 생성할 수 있습니다.
<pre class="pre"><code>
curl -X POST -H "X-Auth-Token:<var class="keyword varname">APIKEY</var>" -H "Content-Type: application/vnd.kafka.binary.v1+json" <var class="keyword varname">kafka-rest endpoint</var>/topics/<var class="keyword varname">topic name</var> -d 

'
{
  "records": [
    {
      "value": "<var class="keyword varname">A base 64 encoded value string</var>"
    }
  ]
}
'
</code></pre>
{: codeblock}
<br/>
<br/>
CURL을 사용하는 경우 다음과 같은 예제를 사용하여 이용할 수 있습니다.
<pre class="pre"><code>
curl -X GET -H "X-Auth-Token:<var class="keyword varname">APIKEY</var>" -H "Accept: application/vnd.kafka.binary.v1+json" <var class="keyword varname">kafka-rest endpoint</var>/topics/<var class="keyword varname">topic name</var>/partitions/<var class="keyword varname">partition ID</var>/messages?offset=<var class="keyword varname">offset to start from</var>
</code></pre>
{: codeblock}

<br/>
<br/>
CURL의 경우 명령행에 다음 행을 추가하여
[Confluent 문서 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://docs.confluent.io/2.0.0/){:new_window}에 자세히 설명되어 있는 코드 예제도 조정할 수 있습니다.
<pre class="pre">-H "X-Auth-Token: <var class="keyword varname">APIKEY</var>"</pre>
{: codeblock}

<br/>
## 연결 및 인증 방법
{: #rest_connect_classic}

<!-- info was in eventstreams066.md -->

{{site.data.keyword.messagehub}}에 연결하기 위해 Kafka REST API는 [VCAP_SERVICES 환경 변수](/docs/services/EventStreams?topic=eventstreams-connecting#connect_classic_cf)에서 <code>api_key</code> 및 <code>kafka_rest_url</code>
인증 정보를 사용합니다.

{{site.data.keyword.messagehub}} Kafka REST API와 인증하려면 사용자 요청의 X-Auth-Token 헤더에 <code>api_key</code>를 지정해야 합니다.


## API 사용 방법
{: #rest_how_classic}

<!-- info was in eventstreams097.md -->

{{site.data.keyword.messagehub}} Kafka REST API 샘플은
Kafka REST API를 통해 {{site.data.keyword.messagehub}}에 연결하여
메시지를 생성하고 이용하는 Node.js 애플리케이션입니다.

샘플 코드는 [event-streams-samples GitHub 프로젝트 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample){:new_window}에 있습니다.

샘플을 빌드하고 실행하려면 해당 프로젝트에 대한 [README.md ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample){:new_window} 파일의 지시사항을 따르십시오.

	다음 블로그는 Kafka REST API의 강점과 약점에 대한 유용한 소개를 제공합니다. [A Comprehensive, Open Source REST Proxy for Kafka ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://www.confluent.io/blog/a-comprehensive-open-source-rest-proxy-for-kafka/){: new_window}.








