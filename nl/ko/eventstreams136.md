---

copyright:
  years: 2015, 2019
lastupdated: "2018-06-06"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 클래식 플랜의 세 개의 API 중에서 선택 
{: #choose_api_classic}

{{site.data.keyword.messagehub}}는 클래식 플랜에서 세 개의 API를 지원합니다. 다음은 가장 적합한 API를 선택하는 데 도움이 되는 정보입니다.
{: shortdesc}

## Kafka API를 사용하는 이유
{: #why_kafka_classic notoc}

{{site.data.keyword.messagehub}}에서 제공하는 기타 인터페이스가 아닌
Kafka API를 사용하도록 선택해야 하는 몇 가지 이유가 있습니다. 이러한 이유에는 다음이 포함됩니다.
{:shortdesc}


* Kafka 지원이 되는 기존 시스템(예: {{site.data.keyword.IBM}} {{site.data.keyword.streaminganalyticsshort}} 및 {{site.data.keyword.sparks}})과 사용자의 앱을 통합하는 것이 더 쉽습니다.
* 다른 API보다 처리량이 더 많고 대기 시간은 더 짧습니다.
* Kafka REST API보다 훨씬 다양한 기능을 갖춘 API를 제공합니다.

## Kafka REST API를 사용하는 이유
{: #why_rest_classic notoc}

** Kafka REST API는 클래식 플랜의 일부로만 사용 가능합니다.**
<br/>

Kafka REST API는 다음 상황에서 사용할 수 있는 편리한 인터페이스입니다.

* 개발자가 {{site.data.keyword.messagehub}}의 사용을 시작하기 원하는 경우
* 대기 시간이 중요한 요인이 아닌 낮은 처리량의 특정 유스 케이스에서
* 디버깅 및 결함 발견을 위해서

Kafka REST API는 높은 처리량과 낮은 대기 시간 인터페이스를 위한 것이 아닙니다.이러한 유형의 요구사항은 {{site.data.keyword.messagehub}}와의 연결에 Kafka API를 사용하도록 권장합니다. 자세한 정보는 [Kafka 클라이언트 사용](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_using)을 참조하십시오.

## {{site.data.keyword.mql}} API를 사용하는 이유
{: #why_mql_classic notoc}

** MQ Light API는 클래식 플랜의 일부로만 사용 가능합니다.**
<br/>

{{site.data.keyword.mql}} API는 AMQP 기반 메시징 인터페이스를 Java™, Node.js, Python 및 Ruby에 제공합니다. API는 초기 {{site.data.keyword.mql}} 서비스와의 역호환성을 위해 제공됩니다.
















