---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-15"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}

# 새 {{site.data.keyword.messagehub}} 표준 플랜으로 업그레이드 
{: #migrate_classic_plan}

표준 멀티 테넌트 플랜의 이 새로운 릴리스는 복원성, 기능 및 사용성에서 상당한 개선사항을 제공합니다. 자세한 정보는 [새 표준 플랜 블로그 공지사항 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/cloud/blog/announcements/ibm-event-streams-releases-a-new-and-enhanced-standard-plan)을 참조하십시오.
{: shortdesc}

애플리케이션을 이전 표준 플랜(이제 클래식 플랜이라고 함)에서 새 플랜으로 마이그레이션하려면 다음 정보를 고려하십시오.

서비스 인스턴스가 이제 Cloud Foundry 서비스 대신 {{site.data.keyword.cloud_notm}} 서비스로 프로비저닝됩니다. 이를 통해 서비스가 다중 구역 배치 및 세부 단위의 액세스 제어를 포함한 최신 {{site.data.keyword.cloud_notm}} 표준 및 기능을 지원할 수 있지만 서비스가 사용되는 방법에 영향을 미칩니다. 특히 다음 측면을 고려하십시오.

* **서비스 작성, 삭제 및 나열**
<br/>
    이전과 같이 {{site.data.keyword.cloud_notm}} 콘솔 또는 {{site.data.keyword.cloud_notm}} CLI 명령행 도구를 사용하여 서비스의 라이프사이클을 관리할 수 있습니다. 콘솔을 사용 중인 경우 이제 서비스가 **Cloud Foundry 서비스** 대신 **서비스** 아래에 나열됩니다. 
    
    CLI를 사용 중인 경우 resource 명령을 사용하여 인스턴스를 관리합니다(예: <code>ibmcloud resource service-instance-create</code>). 이 명령은 **cf** 명령(예: <code>ibmcloud cf create-service</code>)을 대신하는 것입니다.

* **액세스 제어**
<br/>
    이제 인증 및 권한이 Cloud IAM(Identity and Access Management) 서비스를 사용하여 관리됩니다. IAM을 사용하여 사용자의 연결 기능을 제어할 뿐만 아니라 토픽과 같은 기본 리소스에 대한 세부 단위의 액세스를 구성할 수도 있습니다. 사용자에게 정책을 지정하여 액세스를 제어합니다. 자세한 정보는 [{{site.data.keyword.messagehub}} 리소스에 대한 액세스 관리](/docs/services/EventStreams?topic=eventstreams-security)를 참조하십시오.

<ul>
<li><strong>애플리케이션 연결</strong>
<br/>
    애플리케이션이 연결하는 데 필요한 정보는 변경되지 않았습니다. 즉, <code>bootstrap.servers</code>, <code>username</code> 및 <code>password</code>의 목록이 필요합니다. 그러나 이러한 특성이 검색되는 방법은 변경되었습니다.

<ul>
<li>
      <strong>기본 애플리케이션의 경우</strong>
        <br/>
        각각 IBM Cloud 콘솔 또는 CLI를 사용하여 인증 정보 또는 서비스 키 오브젝트를 작성해야 합니다. 그런 다음 필수 특성을 검색할 수 있습니다. 자세한 정보는 [애플리케이션 연결](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_external)을 참조하십시오.
</li>
<br/>
<li><strong>Cloud Foundry 애플리케이션의 경우</strong>
        <br/>
        먼저 서비스 별명을 작성하여 서비스를 애플리케이션의 조직 및 영역에 바인드해야 합니다. 그런 다음 정상적으로 VCAP_SERVICES 환경 변수에서 필수 특성을 검색할 수 있습니다. 자세한 정보는 [{{site.data.keyword.messagehub}}에 연결](/docs/services/EventStreams?topic=eventstreams-connecting)을 참조하십시오.
</li>
</ul>
<br/>
클라이언트가 서버의 호스트 이름이 TLS 핸드쉐이크에 포함된 TLS에 대한 SNI 확장을 지원해야 합니다. 이 기능은 일반적으로 사용 가능하며 [{{site.data.keyword.messagehub}}와 사용할 Kafka 클라이언트 선택](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_clients)에서 권장되는 모든 클라이언트 버전에서 지원됩니다.
</li>
</ul>

<br/>
다음과 같은 몇 가지 다른 변경사항도 알고 있어야 합니다.

* **Kafka 버전**
<br/>
    이 플랜은 안정적인 최신 Kafka 릴리스 2.2에 대한 액세스를 제공합니다. Kafka 1.1에 대해 개발된 애플리케이션은 변경되지 않은 상태로 실행될 수 있지만 권장되는 클라이언트 버전 및 테스트된 조합은 [{{site.data.keyword.messagehub}}와 사용할 Kafka 클라이언트 선택](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_clients)을 참조하십시오. 

* **지원되는 지역**
<br/>
    이 플랜은 처음에 미국 남부에서만 사용 가능합니다. 앞으로 몇 주 동안 나머지 지역에 도입됩니다.

* **통합**
<br/>
    Cloud Foundry 조직 및 영역을 사용하여 서비스에 바인드되는 {{site.data.keyword.iot_short_notm}} 또는 {{site.data.keyword.openwhisk_short}}와 같은 다른 서비스에서 연결하는 경우 서비스 별명이 작성되어야 합니다. 자세한 정보는 [Cloud Foundry 애플리케이션을 {{site.data.keyword.messagehub}}에 연결](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_cf)을 참조하십시오.
    

* **지원되는 기능**
<br/>
    클래식 플랜과 새 표준 플랜의 기능에는 몇 가지 차이점이 있습니다. 제품 오퍼링을 맞추고 새 기술 선택사항을 채택하고 자주 사용되지 않는 기능을 제거하기 위해 모든 기능이 이월되지는 않습니다. 기능 비교는 [플랜 선택](/docs/services/EventStreams?topic=eventstreams-plan_choose)에서 볼 수 있습니다. 이러한 기능에 의존하는 경우 마이그레이션하는 데 도움이 되는 추가 정보가 곧 제공됩니다.
   
<br/>
작은 코드 델타가 매일 프로덕션에 제공됩니다. 결과적으로 2019년의 나머지 기간과 그 이후 동안 사용자 경험(및 다른 영역)에 대한 많은 추가 개선사항이 발생할 것으로 예상할 수 있습니다. 다음은 곧 지원될 예정입니다.

* **고객 메트릭**
<br/>
    서비스 인스턴스의 활동을 모니터하는 기능입니다.

<br/>
새 표준 플랜을 시작하고 실행하는 단계를 빠르게 연습하려면 [시작하기 튜토리얼](/docs/services/EventStreams?topic=eventstreams-getting_started)을 참조하십시오.


