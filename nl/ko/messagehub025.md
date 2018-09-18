---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Kafka REST API 사용
{: #rest_using}

** Kafka REST API는 표준 플랜의 일부로만 사용 가능합니다.**
<br/>

Kafka REST API는 Kafka 클러스터에 RESTful 인터페이스를 제공합니다. API를 사용하여 메시지를 생성해서 이용할 수 있습니다. 참조 문서는 [Kafka REST Proxy 문서 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.confluent.io/2.0.0/kafka-rest/docs/index.html){:new_window}를 참조하십시오. {{site.data.keyword.messagehub}}의 요청 및 응답에는 2진 임베디드 형식만 지원됩니다. Avro 및 JSON 임베디드 형식은 지원되지 않습니다.

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

CURL을 사용하는 경우 다음과 같은 예제를 사용하여 이용할 수 있습니다.
<pre class="pre"><code>
curl -X GET -H "X-Auth-Token:<var class="keyword varname">APIKEY</var>" -H "Accept: application/vnd.kafka.binary.v1+json" <var class="keyword varname">kafka-rest endpoint</var>/topics/<var class="keyword varname">topic name</var>/partitions/<var class="keyword varname">partition ID</var>/messages?offset=<var class="keyword varname">offset to start from</var>
</code></pre>
{: codeblock}


CURL의 경우 명령행에 다음 행을 추가하여
[Confluent 문서 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://docs.confluent.io/2.0.0/){:new_window}에 자세히 설명되어 있는 코드 예제도 조정할 수 있습니다.
<pre class="pre">-H "X-Auth-Token: <var class="keyword varname">APIKEY</var>"</pre>
{: codeblock}


<!-- Comment from Andrew
basic introduction, definitely including health warning
-->

