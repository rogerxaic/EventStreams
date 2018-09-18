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

# Kafka REST API를 사용하는 이유
{: #why_rest}

** Kafka REST API는 표준 플랜의 일부로만 사용 가능합니다.**
<br/>

Kafka REST API는 다음 상황에서 사용할 수 있는 편리한 인터페이스입니다.

* 대기 시간이 중요한 요소가 아닌 낮은 처리량의 특정 유스 케이스에서
* 디버깅 및 결함 발견을 위해서

Kafka REST API가 높은 처리량, 낮은 대기 시간 인터페이스로 의도된 것은 아닙니다. 이러한 유형의 요구사항은 {{site.data.keyword.messagehub}}와의 연결에 Kafka API를 사용하도록 권장합니다. 자세한 정보는 [Kafka 클라이언트 사용](/docs/services/MessageHub/messagehub050.html#kafka_using)을 참조하십시오.


