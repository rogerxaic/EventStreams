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

# 연결 및 인증 방법
{: #rest_connect_authenticate}

<!-- info moved to eventstreams025.md because of doc app changes -->
** Kafka REST API는 표준 플랜의 일부로만 사용 가능합니다.**
<br/>

{{site.data.keyword.messagehub}}에 연결하기 위해 Kafka REST API는 [VCAP_SERVICES 환경 변수](/docs/services/EventStreams?topic=eventstreams-connecting)에서 <code>api_key</code> 및 <code>kafka_rest_url</code>
인증 정보를 사용합니다.
{: shortdesc}

{{site.data.keyword.messagehub}} Kafka REST API와 인증하려면 사용자 요청의 X-Auth-Token 헤더에 <code>api_key</code>를 지정해야 합니다.
