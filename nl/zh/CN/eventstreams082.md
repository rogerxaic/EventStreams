---

copyright:
  years: 2015, 2019
lastupdated: "2018-05-25"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

<!-- 15/11/18: info moved to eventstreams075.md, moved because of doc app changes -->
# MQ Light API 样本
{: #mql_api_samples}

**MQ Light API 仅在标准套餐中提供。**
<br/>

如果还没有应用程序，请通过其中一个样本来试用 {{site.data.keyword.mql}} API。
{:shortdesc}

样本应用程序实际上由两个简单应用程序组成：一个 Web 前端和一个后端；前者用于将消息发送到后端，后者用于处理消息，将词语首字母转换为大写，然后将消息返回给前端。此样本显示了如何使用 {{site.data.keyword.mql}} API 让应用程序彼此进行对话。还可以使用 {{site.data.keyword.mql}} API 来执行工作程序分流；这是构建可扩展、松耦合的分布式应用程序所需的关键功能。

您可以在 [event-streams-samples GitHub 项目 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/ibm-messaging/event-streams-samples/tree/master/mqlight){:new_window} 中找到样本代码。
