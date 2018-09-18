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

# MQ Light API의 개념 및 차이점
{: #mqlight}

** MQ Light API는 표준 플랜의 일부로만 사용 가능합니다.**
<br/>

{{site.data.keyword.mql}} API에서는 Kafka와 비교하여 메시징에 대해 더 상위 레벨의 추상을 제공합니다.
{:shortdesc}

Kafka 클라이언트 또는 {{site.data.keyword.mql}} API 사용 중에서 선택하는 것은
빌드하려는 메시징 토폴로지에 좌우됩니다.

* Kafka로 적은 수의 토픽을 사용하여 추가 확장성을 위해 각 토픽에 여러 개의 파티션이 생기도록 할 수 있습니다. 이용자 그룹을 사용하여 이용자들 간에서 메시지를 공유할 수 있지만, 각 이용자가 지정된 파티션에 대한 메시지의 비율과 맞출 수 있어야 합니다.
* {{site.data.keyword.mql}} API로 훨씬 더 많은 수의 토픽을 사용할 수 있으며 토픽 이름은 계층 구조(예: <code>‘/sports/football’</code> 및 <code>‘/sports/tiddlywinks’</code>)입니다. 

{{site.data.keyword.mql}} API의 토픽은 Kafka 토픽과 같지 않습니다. 그 대신, {{site.data.keyword.mql}} API는
"MQLight"로 이름 지정된 단일 Kafka 토픽을 사용하며 {{site.data.keyword.mql}} API를 사용하여 보내고 받은 모든 메시지가 그 하나의 Kafka 토픽을 통해 이동합니다.

{{site.data.keyword.mql}}는 다음 {{site.data.keyword.Bluemix_notm}} 지역(미국 남부, 영국 및 시드니)에서만 사용 가능합니다. MQ Light API는 독일 지역 또는 {{site.data.keyword.Bluemix_notm}} 데디케이티드에서는 사용할 수 없습니다.

<!-- begin STAGING ONLY -->
API 중에서의 선택에 대한 자세한 정보는 [세 개의 API 중에서 선택](/docs/services/MessageHub/messagehub087.html)을 참조하십시오.
<!-- end STAGING ONLY -->

