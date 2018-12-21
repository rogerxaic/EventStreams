---

copyright:
  years: 2015, 2018
lastupdated: "2018-09-11"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.messagehub}} 팀에 문제점 보고 - 엔터프라이즈 플랜
{: #report_problem}

{{site.data.keyword.messagehub}}에 문제점이 생긴 경우, [{{site.data.keyword.Bluemix_notm}} 상태 페이지 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.bluemix.net/status){:new_window}를 우선 확인하십시오.

{{site.data.keyword.messagehub}} 팀의 도움이 필요한 경우에는 다음 정보를 모두 수집하십시오. 미리 제공할 수 있는 정보가 많을수록 팀에서 문제점에 대해 더 효과적으로 도움을 줄 수 있습니다.
{:shortdesc}

1. 사용 중인 {{site.data.keyword.messagehub}} 서비스의 CRN ID는 무엇입니까?  이 ID는 서비스를
   클릭한 후 전체 {{site.data.keyword.Bluemix_notm}} 콘솔 URL을 붙여넣거나,
   다음 CLI 명령의 출력을 붙여넣어 제공할 수 있습니다.<br/>
   <pre class="pre">
   ibmcloud resource service-instance NAME
   </pre>
	{: codeblock}
2. 문제점이 언제 처음 발생했습니까(구체적인 시간, 날짜 및 시간대)?
   문제가 발생하기 전에 앱이 얼마나 오랫동안 실행 중이었습니까?
3. 문제점이 계속 발생하고 있습니까? 문제점을 복제할 수 있습니까?
4. 사용자의 애플리케이션이 어떤 Kafka 클라이언트를 사용 중입니까? 버전 세부사항은 무엇입니까?
5. 클라이언트 구성 세부사항은 무엇입니까?
6. 문제점을 표시하는 애플리케이션 로그 스니펫이 있습니까?
7. 현재 확인 중인 문제는 무엇입니까? 어떤 토픽, 클라이언트 ID, 그룹 ID 및 트랜잭션 ID가
   영향을 받습니까?
8. 사용자의 서비스에 문제점이 미치는 영향은 무엇입니까?

[지원 요청
제출![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/get-support/howtogetsupport.html#open-ticket)을 통해 지원 티켓으로 수집한 정보를 IBM에 제공할 수 있습니다.










