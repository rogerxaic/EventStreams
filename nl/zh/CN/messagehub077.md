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

# 在 {{site.data.keyword.messagehub}} 中启用 {{site.data.keyword.mql}} API
{: #mql_enable}

**MQ Light API 仅在标准套餐中提供。**
<br/>

**必须显式创建名为“MQLight”的 Kafka 主题后才能使用该 API，因为所有消息都将经过“MQLight”主题。此主题必须具有单个分区。创建此主题后，即可将 MQ Light API 用于您的服务实例。**  

MQ Light API 使用“MQLight”主题来存储其消息数据，以及与其他 Kafka 客户机进行交互。请注意，创建此主题时，会按服务支付套餐中概述的标准费率收费。

要禁用 MQ Light API，请删除“MQLight”主题。请注意，删除该主题时，将销毁所有数据。
