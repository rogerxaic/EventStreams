---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-24"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka, service endpoints

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 엔터프라이즈 플랜을 사용하여 서비스 엔드포인트 관리
{: #manage_endpoints}

내부 또는 사설 엔드포인트를 사용하여 {{site.data.keyword.Bluemix_short}} 서비스 엔드포인트에서 내부 IBM Cloud 네트워크를 통해 {{site.data.keyword.messagehub}} 서비스에 연결할 수 있습니다. 이 기능은 {{site.data.keyword.messagehub}} 서비스에서 데이터를 공개하거나 이용할 때 공용 인터페이스가 아니라 사설 네트워크를 통해 수행됨을 나타냅니다.
{:shortdesc}

이제 {{site.data.keyword.messagehub}} 서비스 인스턴스에 액세스하고 관리하기 위해 사설 엔드포인트를 추가할 수 있습니다.

## 전제조건
{: #prereqs_endpoint}

다음 요구사항을 충족하는지 확인하십시오.
- 엔터프라이즈 플랜을 사용하여 서비스 인스턴스를 작성하십시오. 자세한 정보는 [플랜 선택](/docs/services/EventStreams?topic=eventstreams-plan_choose)을 참조하십시오.
- {{site.data.keyword.Bluemix_notm}} 댈러스(us-south) 또는 프랑크프루트(eu-de) 지역에 서비스 인스턴스를 작성하십시오.
- [VRF(Virtual Route Forwarding)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)를 {{site.data.keyword.Bluemix_notm}} 계정에 사용하십시오.

## 사설 엔드포인트 추가
{: #add_endpoint}

사설 엔드포인트를 추가하려면 다음을 수행하십시오.

* [사설 티켓 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 아이콘 링크")](/docs/get-support?topic=get-support-getting-customer-support#using-avatar){:new_window}을 발행하여 사설 엔드포인트를 요청하십시오. 티켓에서 다음 정보를 제공하십시오.

    * 클러스터 ID(아는 경우).

    클러스터 ID를 모르면 대신 대시보드 URL, Kafka 브로커 엔드포인트 또는 서비스 인스턴스 ID를 제공하십시오.
  

서비스 엔드포인트에 관한 자세한 정보는 [{{site.data.keyword.Bluemix_notm}} 서비스 엔드포인트 문서](/docs/resources?topic=resources-service-endpoints#about){:new_window}를 참조하십시오.


## 사설 엔드포인트로 전환한 후
{: #after_endpoint}

사설 엔드포인트로 전환한 경우 더 이상 외부 또는 공용 엔드포인트를 사용할 수 없습니다.


### IBM {{site.data.keyword.messagehub}} 콘솔에 액세스

클러스터에서 사설 엔드포인트가 사용으로 설정된 경우 {{site.data.keyword.messagehub}} 콘솔에 액세스하는 데 사용하는 관리자 URL이 변경됩니다.

{{site.data.keyword.messagehub}} 콘솔은 사설 관리자 URL에서만 연결할 수 있습니다. 사설 관리자 URL을 포함하여 사설 엔드포인트를 검색하려면 새 서비스 인증 정보를 작성할 수 있습니다.

<!--
1. On the service details page, click **Manage endpoints**. You can see the external endpoint assigned to your service instance.
2. Click  **Add internal endpoint**. An internal endpoint is assigned to your service instance.
3. **Optional.** Use the endpoint toggle to enable or disable endpoints as needed.
-->

