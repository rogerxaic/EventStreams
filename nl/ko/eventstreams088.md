---

copyright:
  years: 2015, 2019
lastupdated: "2018-11-08"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 브릿지를 사용하여 다른 서비스에 링크
{: #bridges}

** {{site.data.keyword.messagehub}} 브릿지는 표준 플랜의 일부로만 사용 가능합니다.**
<br/>

{{site.data.keyword.messagehub}} 표준 플랜은 다른 시스템의 선택사항에 대한
브릿지를 지원하기도 합니다. 브릿지는 {{site.data.keyword.messagehub}} 및 다른 서비스 사이의 단방향 링크입니다. 브릿지는
데이터를 {{site.data.keyword.messagehub}}에서 읽고 다른 서비스에
쓰도록 허용하거나 데이터를 다른 서비스에서 읽고 {{site.data.keyword.messagehub}}에 쓰도록 허용합니다. 브릿지는 다른 시스템에서 메시지를 가져와서 그 메시지를 토픽에 공개하거나
토픽에서 메시지를 이용해서 그 메시지를 다른 시스템에 보낼 수 있습니다. 이런 식으로 {{site.data.keyword.messagehub}}를 사용하여 코드를 작성하지 않고 다른 시스템과 통합할 수 있습니다.
{:shortdesc}

{{site.data.keyword.messagehub}} 브릿지 사용의 주요 이점은 다음과 같습니다.  

* 관리상 브릿지를 정의할 수 있어서 애플리케이션 코드를 작성할 필요가 없습니다.
* 각 브릿지의 라이프사이클을 {{site.data.keyword.messagehub}} 서비스에서 모니터하고 관리합니다. 예를 들어, 브릿지에 오류가 발생하면 {{site.data.keyword.messagehub}}에서 자동으로 브릿지를 다시 시작합니다.
* 브릿지는 {{site.data.keyword.Bluemix_short}} 플랫폼과 통합됩니다. 예를 들어, 브릿지에서 생성된 로깅 및 모니터링 정보는 Kibana 및 Grafana 대시보드로 경로가 지정됩니다.

다음 두 개의 공통 시나리오에서 브릿지가 유용하게 사용될 수 있습니다.

* 하나 이상의 데이터 소스에서 {{site.data.keyword.messagehub}}로 데이터 이용
* 데이터를 {{site.data.keyword.messagehub}}에서 장기 스토리지와 같은 다른 서비스에 전송

## {{site.data.keyword.messagehub}} 브릿지
{: notoc}

* 다음 유형의 브릿지를 제공합니다. 
  - [MQ 브릿지](/docs/services/EventStreams/eventstreams105.html){:new_window}는 {{site.data.keyword.IBM}} MQ에서 메시지 데이터를 택해서 {{site.data.keyword.messagehub}}의 토픽에 전송합니다. 향후에는 더 광범위한 범위의 브릿지를 지원할 계획입니다.
  - [Cloud Object Storage 브릿지](/docs/services/EventStreams/eventstreams115.html){:new_window}는 {{site.data.keyword.messagehub}} 데이터를 [{{site.data.keyword.IBM_notm}} Cloud Object Storage ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/services/cloud-object-storage/about-cos.html){:new_window} 서비스의 인스턴스로 전송합니다. 
  - [{{site.data.keyword.objectstorageshort}} 브릿지](/docs/services/EventStreams/eventstreams089.html){:new_window}는 2018년 8월 1일부터 더 이상 사용되지 않습니다. 자세한 정보는 [사용 중단 공지사항: {{site.data.keyword.objectstorageshort}} OpenStack Swift(PaaS) ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/blogs/bluemix/2018/05/end-marketing-object-storage-openstack-swift-paas/){:new_window}를 참조하십시오.
* 현재 브릿지는 모든 {{site.data.keyword.Bluemix_notm}} 퍼블릭 환경에서 사용 가능합니다. 브릿지는 {{site.data.keyword.Bluemix_short}} 데디케이티드에서는 사용할 수 없습니다.
* 다음 두 가지 방법으로 브릿지를 관리할 수 있습니다.
  - [REST API ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-messaging/event-streams-docs){:new_window}를 사용하며, 이는 기존 {{site.data.keyword.messagehub}} 관리 API에 대한 확장입니다. [message-hub-docs ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/ibm-messaging/event-streams-docs){:new_window}에서 브릿지의 라이프사이클 관리를 위한 curl 사용 방법의 예제를 찾을 수도 있습니다. 브릿지를 계속 개발함에 따라 이 REST API를 변경할 수 있습니다. 이 API를 안정화할 예정입니다.
  - {{site.data.keyword.Bluemix_notm}} 콘솔에서 {{site.data.keyword.messagehub}} 대시보드를 사용합니다.
* {{site.data.keyword.messagehub}} 서비스의 인스턴스와 모든 유형의 브릿지를 최대 두 개까지 연관시킬 수 있습니다. 브릿지를 계속 개발함에 따라 이 제한사항을 계속 검토하게 됩니다.
* 해당 메시징 오퍼레이션 이외의 브릿지 사용에 대한 추가 비용은 없습니다.
* MQ 브릿지는 브릿지와 MQ 큐 관리자 간에 전송되므로 데이터의 개인정보 보호 및 무결성을 보호하기 위해 SSL/TLS의 사용을 지원하지 않습니다. 브릿지에 SSL/TLS 사용에 대한 지원을 추가할 계획입니다. 
* 그러나 [{{site.data.keyword.SecureGatewayfull}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/services/SecureGateway/index.html#getting-started-with-sg){:new_window} 서비스를 사용하여
{{site.data.keyword.Bluemix_notm}}와 온프레미스로 설치할 수 있는 {{site.data.keyword.SecureGateway}} 클라이언트 사이의 보안 터널을 통해
데이터를 전송할 수 있습니다. 이 구성에서는 SSL/TLS를 사용하여 터널의 양쪽 끝에서 통신의 보안이 되지 않습니다.
* Cloud Object Storage 브릿지는 Cloud Object Storage에 데이터를 기록할 때
구분 기호로 줄 바꾸기 문자를 사용하여 메시지를 연결합니다. 이로 인해 임베드된 줄 바꾸기 문자를 포함하는 메시지 및 2진 메시지 데이터에는 이러한 브릿지가 적합하지 않습니다.
* Cloud Object Storage 및 Cloud Object Storage 브릿지에서 현재 사용되는 오브젝트 이름 지정 규칙은 향후에 변경될 수 있습니다.

## 다른 서비스에서 {{site.data.keyword.messagehub}}로의 브릿지
{: notoc}

* {{site.data.keyword.iot_full}}은 고유 [브릿지를 {{site.data.keyword.messagehub}}에](/docs/services/EventStreams/eventstreams119.html){:new_window} 제공합니다. 브릿지는 사용자가 히스토리 데이터를 저장할 수 있는 {{site.data.keyword.messagehub}}로의 단방향 링크를 제공합니다. {{site.data.keyword.messagehub}}를 {{site.data.keyword.iot_short_notm}}에 연결하면 {{site.data.keyword.messagehub}}를 이벤트 파이프라인으로 사용하여 Watson IoT Platform의 디바이스 이벤트를 이용하고 나머지 플랫폼에서 실시간으로 이벤트를 사용 가능하게 할 수 있습니다. 


