---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-30"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.mql}} API를 사용하는 이유
{: #why_mql}

** MQ Light API는 표준 플랜의 일부로만 사용 가능합니다.**
<br/>

{{site.data.keyword.mql}} API에서는 Kafka API보다 더 상위 레벨의 추상을 제공합니다. {{site.data.keyword.mql}}는 큐 및 발행/구독 메시징 패턴을 모두 지원하는 통합된 메시징 모델에서 앱을 이식 가능하게 작성할 수 있도록 합니다.
{:shortdesc}

앱은 동적으로 작성된 대상을 사용하여 메시지를 교환하며,
이를 통해 계층적으로 구조화하고(예: <code>‘/sports/football’</code>), 와일드카드를 사용하여 그룹화하고(예: 
<code>‘/sports/#’</code>) 전달 보증 및 메시지 만료에 대한 제어를 할 수 있습니다.
이를 사용하여 작업자 오프로드, 이벤트 알림 및 일괄처리와 같은 시나리오를 구현할 수
있습니다.

{{site.data.keyword.mql}} API를 사용하여 다른 앱 간에 메시지를 보낼 뿐만 아니라, Kafka REST 또는 Kafka API를 사용하는 앱과 메시지를 교환할 수도 있습니다. 그렇지 않으면 {{site.data.keyword.IBM}} MQ와 온프레미스로 앱을 사용할 수도 있습니다.

