---

copyright:
  years: 2015, 2019
lastupdated: "2018-09-11"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}



# 클래식 플랜의 경우 {{site.data.keyword.messagehub}} 팀에 문제점 보고
{: #report_problem_classic}

{{site.data.keyword.messagehub}}에 문제점이 생긴 경우, [{{site.data.keyword.Bluemix_notm}} 상태 페이지 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/status?selected=status){:new_window}를 우선 확인하십시오. 

{{site.data.keyword.messagehub}} 팀의 도움이 필요한 경우에는 다음 정보를 모두 수집하십시오. 미리 제공할 수 있는 정보가 많을수록 팀에서 문제점에 대해 더 효과적으로 도움을 줄 수 있습니다.
{:shortdesc}

1. 어느 {{site.data.keyword.Bluemix_notm}} 위치(지역)에서 사용자의 {{site.data.keyword.messagehub}} 인스턴스가 프로비저닝되었습니까?  예를 들면, 댈러스나 런던입니다. 
2. 어떤 인터페이스로 문제를 보고 있습니까? Kafka, REST, AMQP 또는 브릿지 중 어느 것입니까?
3. 문제점이 언제 처음 발생했습니까(구체적인 시간, 날짜 및 시간대)? 문제가 발생하기 전에 앱이 얼마나 오랫동안 실행 중이었습니까?
4. 문제점이 계속 발생하고 있습니까? 문제점을 복제할 수 있습니까?
5. 사용 중인 {{site.data.keyword.messagehub}} 서비스의 인스턴스 ID는 무엇입니까? 
서비스의 VCAP_SERVICES JSON에서 "instance_id" 필드를 확인하여 이 ID를 찾을 수 있습니다. 예를 들어, 다음과 같은 경우입니다.
 ```
 "instance_id": "e662a61b-d1ff-4cce-aab9-5eae9adb9827"
 ```
6. 사용자의 애플리케이션이 어떤 Kafka 또는 REST 클라이언트를 사용 중입니까? 버전 세부사항은 무엇입니까?
7. 클라이언트 구성 세부사항은 무엇입니까?
8. 문제점을 표시하는 애플리케이션 로그 스니펫이 있습니까?
9. 현재 확인 중인 문제는 무엇입니까? 어떤 토픽, 클라이언트 ID 및 그룹 ID가 영향을 받습니까?
10. 사용자의 서비스에 문제점이 미치는 영향은 무엇입니까?


[지원 요청
제출![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/get-support?topic=get-support-getting-customer-support#using-avatar){:new_window}을 통해 지원 티켓으로 수집한 정보를 IBM에 제공할 수 있습니다.










