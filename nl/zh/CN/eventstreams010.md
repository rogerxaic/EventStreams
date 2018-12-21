---

copyright:
  years: 2015, 2018
lastupdated: "2018-11-07"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 什么是 {{site.data.keyword.messagehub}}？
{: #about}

{{site.data.keyword.messagehub_full}} 是使用 Apache Kafka 构建的高吞吐量消息传递总线。已针对 {{site.data.keyword.Bluemix_notm}} 事件获取以及服务和应用程序之间的事件流分发进行了优化。{{site.data.keyword.messagehub}} 以前称为 Message Hub。
{: shortdesc}

可以使用 {{site.data.keyword.messagehub}} 来完成以下任务：

* 将工作分流到后端工作程序应用程序。
* 将事件流连接到 Streaming Analytics 以获得深刻的洞察。
* 将事件数据发布到多个应用程序以实时作出反应。
* 将数据传输到其他服务中。例如，传输到 Cloud Object Storage。

使用 Apache Kafka 构建之后，它可以直接受益于社区内发生的所有创新，并支持 Kafka 客户机 API、Kafka Streams、Kafka Connect 以及 KSQL。
{:shortdesc}

虽然由于与 {{site.data.keyword.messagehub}} 的连接始终使用凭证进行认证，您需要提供其他配置，但是 Apache Kafka 工具通常直接使用 {{site.data.keyword.messagehub}}。

在 {{site.data.keyword.messagehub}} 中，应用程序通过创建消息并将其发送到主题来发送数据。要接收消息，应用程序可预订主题，并选择是接收该主题的所有消息还是在应用程序之间共享消息。
{{site.data.keyword.messagehub}} 按有序的顺序托管和维护消息。 




