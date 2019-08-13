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


# 使用企業方案來管理服務端點
{: #manage_endpoints}

藉由使用內部或專用端點，「{{site.data.keyword.Bluemix_short}} 服務端點」會啟用透過內部 IBM Cloud 網路的 {{site.data.keyword.messagehub}} 服務連線。此功能表示您從 {{site.data.keyword.messagehub}} 服務發佈或使用的任何資料都是透過專用網路，而非公用介面。
{:shortdesc}

您現在可以新增專用端點以存取及管理 {{site.data.keyword.messagehub}} 服務實例。

## 必要條件
{: #prereqs_endpoint}

請確定您符合下列需求：
- 使用企業方案來建立服務實例。如需相關資訊，請參閱[選擇方案](/docs/services/EventStreams?topic=eventstreams-plan_choose)。
- 在 {{site.data.keyword.Bluemix_notm}} 達拉斯 (us-south) 或法蘭克福 (eu-de) 地區中建立服務實例。
- 啟用 {{site.data.keyword.Bluemix_notm}} 帳戶的[虛擬路由轉遞 (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)。

## 新增專用端點
{: #add_endpoint}

若要新增專用端點，請執行下列動作：

* 開立[問題單 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/get-support?topic=get-support-getting-customer-support#using-avatar){:new_window}，以要求專用端點。請在問題單中提供下列資訊：

    * 叢集 ID（如果知道的話）。

    如果您不知道叢集 ID，則請改為提供儀表板 URL、Kafka 分配管理系統端點或服務實例 ID。
  

如需服務端點的相關資訊，請參閱 [{{site.data.keyword.Bluemix_notm}} 服務端點文件](/docs/resources?topic=resources-service-endpoints#about){:new_window}。


## 切換至專用端點之後
{: #after_endpoint}

當您切換至專用端點之後，就無法再使用外部或公用端點。


### 存取 IBM {{site.data.keyword.messagehub}} 主控台

叢集已啟用專用端點時，您用來存取 {{site.data.keyword.messagehub}} 主控台的管理 URL 會變更。

{{site.data.keyword.messagehub}} 主控台只能從專用管理 URL 聯繫。若要探索專用端點（包括專用管理 URL），您可以建立新的服務認證。

<!--
1. On the service details page, click **Manage endpoints**. You can see the external endpoint assigned to your service instance.
2. Click  **Add internal endpoint**. An internal endpoint is assigned to your service instance.
3. **Optional.** Use the endpoint toggle to enable or disable endpoints as needed.
-->

