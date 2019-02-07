---

copyright:
  years: 2015, 2019
lastupdated: "2018-10-29"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


#모니터링 및 로깅(표준 플랜)
{: #monitoring}

표준 플랜의 {{site.data.keyword.messagehub}}는 {{site.data.keyword.messagehub}}의 사용을 모니터링할 수 있도록
메트릭과 이벤트를 자동으로 수집합니다.
{:shortdesc}

**참고:** 메트릭과 이벤트는 댈러스(us-south), 런던(eu-gb) 및 시드니(au-syd)에서만 {{site.data.keyword.messagehub}} 표준 플랜의 일부로 사용 가능합니다. 


다음 로그 정보를 모니터할 수 있습니다.

<dl>
<dt>토픽 메트릭</dt>
<dd>각 토픽에 대한 수신 및 발신 바이트 수를 전송합니다(15분마다 체크포인트 지정). {{site.data.keyword.Bluemix_notm}} 콘솔의 {{site.data.keyword.messagehub}} 대시보드에서
**Grafana** 단추를 클릭하여 해당 메트릭에 액세스할 수 있습니다.
</dd>
<dt>토픽 이벤트</dt>
<dd>또한 토픽을 작성하거나 삭제할 때마다 이벤트를 푸시합니다. {{site.data.keyword.Bluemix_notm}} 콘솔의 {{site.data.keyword.messagehub}} 대시보드에서
**Kibana** 단추를 클릭하여 해당 이벤트에 액세스할 수 있습니다.</dd>
</dl>


{{site.data.keyword.messagehub}}는 사용자의 변경사항을 겹쳐쓸 수 있는 업데이트를
작성하기 때문에 {{site.data.keyword.messagehub}} 대시보드를 편집하지 않도록 권장합니다. 하지만 사용자 자신의 대시보드에 이러한 메트릭과 이벤트를 포함할 수는 있습니다.


