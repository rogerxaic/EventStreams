---

copyright:
  years: 2015, 2019
lastupdated: "2018-06-01"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Watson IoT Platform 网桥
{: #consuming_messages_watson}

{{site.data.keyword.iot_full}} 提供了一个受管网桥，支持您创建与 {{site.data.keyword.messagehub_full}} 的单向链接。
{: shortdesc}

将 {{site.data.keyword.messagehub}} 连接到 {{site.data.keyword.iot_short_notm}} 意味着可以使用 {{site.data.keyword.messagehub}} 作为事件管道，从 {{site.data.keyword.iot_short_notm}} 使用设备事件，并将事件实时提供给平台的其余部分。 

常用模式是使用 {{site.data.keyword.iot_short_notm}} 网桥、{{site.data.keyword.messagehub}} 和 Cloud Object Storage 网桥来创建端到端管道，使得实时和批量交互更加轻松。

有关如何创建此网桥的信息，请参阅：[连接并配置历史数据库连接器以使用 {{site.data.keyword.messagehub}} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/support/knowledgecenter/SSQP8H/iot/platform/reference/dsc/eventstreams.html){:new_window}。






