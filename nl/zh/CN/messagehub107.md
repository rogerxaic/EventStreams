---

copyright:
  years: 2015, 2018
lastupdated: "2017-09-26"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 什么是 Message Hub？

{{site.data.keyword.messagehub}} 使用主题来实施发布/预订消息传递。与其他消息传递系统不同，{{site.data.keyword.messagehub}} 提供的是主题，而不是队列。{{site.data.keyword.messagehub}} 还提供了其他功能，例如用于连接其他系统的网桥。

{{site.data.keyword.messagehub}} 基于开放式源代码 Apache Kafka 项目，这是一种高性能的消息传递主干，可扩展性强，在许多生产环境中得到证明。有关更多信息，请参阅 [{{site.data.keyword.messagehub}} 和 Apache Kafka](/docs/services/MessageHub/messagehub073.html)。虽然由于与 {{site.data.keyword.messagehub}} 的连接始终使用凭证进行认证，您需要提供其他配置，但是 Apache Kafka 工具通常直接使用 {{site.data.keyword.messagehub}}。

{{site.data.keyword.messagehub}} 有三个 API：Kafka API、Kafka REST API 和 {{site.data.keyword.mql}} API。大部分情况下，Kafka API 是最佳选择。有关使用 API 创建消息传递应用程序的更多信息，请参阅[使用 Kafka 客户机](/docs/services/MessageHub/messagehub050.html)、[使用 Kafka REST API](/docs/services/MessageHub/messagehub025.html) 和[使用 MQ Light API 客户机](/docs/services/MessageHub/messagehub075.html)。

{{site.data.keyword.messagehub}} 还支持用于连接其他所选系统的网桥。网桥是指向其他系统的单向链接。网桥可以获取来自其他系统的消息，并将其发布到主题，或者使用主题中的消息，并将其发送到其他系统。通过这种方式，可以使用 {{site.data.keyword.messagehub}} 来与其他系统集成，而无需编写代码。有关更多信息，请参阅[使用网桥链接到其他服务](/docs/services/MessageHub/messagehub088.html)。
