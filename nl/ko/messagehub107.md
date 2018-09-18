---

copyright:
  years: 2015, 2018
lastupdated: "2017-09-26"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Message Hub 개념

{{site.data.keyword.messagehub}}는 토픽을 사용하여 발행/구독
메시징을 구현합니다. 다른 메시징 시스템과 다르게 {{site.data.keyword.messagehub}}는 토픽을 제공하지만 큐에 저장하지 않습니다. 또한,
{{site.data.keyword.messagehub}}는 브릿지와 같은 추가 기능도 다른 시스템에 제공합니다.

{{site.data.keyword.messagehub}}는 고도로 확장 가능하고, 많은 프로덕션 환경에서 고성능 메시징 백본이 증명된 오픈 소스
Apache Kafka 프로젝트를 기반으로 합니다. 자세한 정보는 [{{site.data.keyword.messagehub}} 및 Apache Kafka](/docs/services/MessageHub/messagehub073.html)를 참조하십시오.
 {{site.data.keyword.messagehub}}에 대한 연결이 항상 신임 정보 사용을 인증하므로 추가 구성을 제공해야 하는 경우에도
Apache Kafka 도구는 일반적으로 {{site.data.keyword.messagehub}}와 함께 직접적으로 작동합니다.

{{site.data.keyword.messagehub}}에는 세 개의 API(Kafka API, Kafka REST API 및 {{site.data.keyword.mql}} API)가 있습니다.
대부분의 경우 Kafka API가 가장 적합합니다. API를 사용하여 메시징 애플리케이션 작성에 대한 자세한 정보는
[Kafka 클라이언트 사용](/docs/services/MessageHub/messagehub050.html), [Kafka REST API 사용](/docs/services/MessageHub/messagehub025.html) 및 [MQ Light API 클라이언트 사용](/docs/services/MessageHub/messagehub075.html)을 참조하십시오.

{{site.data.keyword.messagehub}}는 다른 시스템의 선택사항에
브릿지를 지원하기도 합니다. 브릿지는 다른 시스템에 대한 단방향
링크입니다. 브릿지는 다른 시스템에서 메시지를 가져와서 그 메시지를 토픽에 공개하거나
토픽에서 메시지를 이용해서 그 메시지를 다른 시스템에 보낼 수 있습니다. 이런 식으로 {{site.data.keyword.messagehub}}를 사용하여 코드를 작성하지 않고 다른 시스템과 통합할 수 있습니다. 자세한 정보는 [브릿지를 사용하여 다른 서비스에 링크](/docs/services/MessageHub/messagehub088.html)를 참조하십시오.
