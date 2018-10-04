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

# 기존 MQ Light 애플리케이션 연결 방법
{: #mql_exist_apps}

** MQ Light API는 표준 플랜의 일부로만 사용 가능합니다.**
<br/>

{{site.data.keyword.IBM_notm}} MQ 또는 {{site.data.keyword.mql}}
{{site.data.keyword.Bluemix_notm}} 서비스에 대해 현재 실행되는 기존 애플리케이션을 서비스에 연결할 수 있습니다. 앱은 같은 방법으로 계속 작업합니다.
{:shortdesc}

기존 앱에 연결하려면 다음 확인사항을 완료하십시오.

* 앱이 사용자의 언어에 사용 가능한 최신 {{site.data.keyword.mql}} API 클라이언트 버전을 사용 중인지 확인하십시오.
* VCAP_SERVICES에서 추출된 연결 세부사항이 <code>messagehub</code> 서비스 유형을 참조하고, <code>credentials.username</code> 특성보다는 <code>credentials.user</code> 특성에서 연결 사용자 이름을 검색하고, <code>credentials.connectionLookupURI</code> 특성보다는 <code>credentials.mqlight_lookup_url</code> 특성에서 연결 검색 URL을 검색하는지 확인하십시오. 자세한 정보는 [VCAP_SERVICES 환경 변수](/docs/services/EventStreams/eventstreams127.html)를 참조하십시오.

	Java&trade; 클라이언트를 사용하는 경우 이 단계가 완료되도록 하고 클라이언트가 스스로 정보를 검색하도록 create() 호출에서 'null'을 endpointService 매개변수로 지정하십시오.
	
* 사용자의 앱이 TLS v1.2 연결을 지원해야 합니다. VCAP_SERVICES에는 비TLS 연결 작성을 위한 <code>credentials.nonTLSConnectionLookupURI</code> 특성이 더 이상 포함되지 않습니다.

다음 정보에도 주의해야 합니다.

* 메시지 한계는 {{site.data.keyword.messagehub}}와 일치하지만 {{site.data.keyword.mql}} API를 지원하는 다른 서버와는 다를 수 있습니다. 자세한 정보는 [최대 한계](/docs/services/EventStreams/eventstreams083.html)를 참조하십시오.
* JMS가 지원되지 않습니다.
