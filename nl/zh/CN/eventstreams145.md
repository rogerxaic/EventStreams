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


# 使用企业套餐管理服务端点
{: #manage_endpoints}

使用内部或专用端点，{{site.data.keyword.Bluemix_short}} 服务端点支持通过内部 IBM Cloud 网络连接到 {{site.data.keyword.messagehub}} 服务。此功能意味着您从 {{site.data.keyword.messagehub}} 服务发布或使用的任何数据都是通过专用网络而非公共接口进行的。
{:shortdesc}

您现在可以添加专用端点以访问和管理您的 {{site.data.keyword.messagehub}} 服务实例。

## 先决条件
{: #prereqs_endpoint}

请确保满足以下需求：
- 使用企业套餐创建服务实例。有关更多信息，请参阅[选择套餐](/docs/services/EventStreams?topic=eventstreams-plan_choose)。
- 在 {{site.data.keyword.Bluemix_notm}} 达拉斯 (us-south) 或法兰克福 (eu-de) 区域中创建服务实例。
- 为 {{site.data.keyword.Bluemix_notm}} 帐户启用 [Virtual Route Forwarding (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)。

## 添加专用端点
{: #add_endpoint}

要添加专用端点：

* 发出[凭单 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/get-support?topic=get-support-getting-customer-support#using-avatar){:new_window} 以请求专用端点。在凭单中提供以下信息：

    * 您的集群标识（如果知道）。

    如果不知道集群标识，请改为提供您的仪表板 URL、Kafka 代理程序端点或您的服务实例标识。
  

有关服务端点的更多信息，请参阅 [{{site.data.keyword.Bluemix_notm}} 服务端点文档](/docs/resources?topic=resources-service-endpoints#about){:new_window}。


## 切换到专用端点之后
{: #after_endpoint}

切换到专用端点时，外部或公共端点对您不再可用。


### 访问 IBM {{site.data.keyword.messagehub}} 控制台

当集群启用专用端点后，您用于访问 {{site.data.keyword.messagehub}} 控制台的管理 URL 将更改。

{{site.data.keyword.messagehub}} 控制台仅可从专用管理 URL 进行访问。要发现专用端点（包括专用管理 URL），您可以创建新的服务凭证。

<!--
1. On the service details page, click **Manage endpoints**. You can see the external endpoint assigned to your service instance.
2. Click  **Add internal endpoint**. An internal endpoint is assigned to your service instance.
3. **Optional.** Use the endpoint toggle to enable or disable endpoints as needed.
-->

