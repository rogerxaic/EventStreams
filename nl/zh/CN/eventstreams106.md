---

copyright:
  years: 2015, 2019
lastupdated: "2018-10-29"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 什么是 MQ Light API，它有何独到之处？
{: #mqlight}

<!-- 30/10/18: info moved to eventstreams075.md because of doc app changes -->
** MQ Light API 仅在标准套餐中提供。**
<br/>

{{site.data.keyword.mql}} API 提供的消息传递抽象级别高于 Kafka。
{:shortdesc}

选择使用 Kafka 客户机还是 {{site.data.keyword.mql}} API 取决于要构建的消息传递拓扑：

* 使用 Kafka 时，您可使用数量不多的主题，并且每个主题可以有多个分区，以实现额外的可伸缩性。可以通过使用者组在使用者之间共享消息，但每个使用者必须能够跟上为其分配的分区的消息速率。
* 使用 {{site.data.keyword.mql}} API 时，您可以使用的主题数量要多得多，并且主题名称是分层的（例如：<code>‘/sports/football’</code> 和 <code>‘/sports/tiddlywinks’</code>）。 

{{site.data.keyword.mql}} API 中的主题与 Kafka 主题不同。即，{{site.data.keyword.mql}} API 使用名为“MQLight”的单个 Kafka 主题，并且使用 {{site.data.keyword.mql}} API 发送和接收的所有消息都经过这一个 Kafka 主题。

{{site.data.keyword.mql}} 仅在以下 {{site.data.keyword.Bluemix_notm}} 区域中可用：美国南部（达拉斯）、英国南部（伦敦）和亚太地区南部（悉尼）。MQ Light API 在欧洲中部（法兰克福）区域或 {{site.data.keyword.Bluemix_notm}} Dedicated 中不可用。

<!-- begin STAGING ONLY -->
有关在 API 之间进行选择的更多信息，请参阅[在三个 API 之间选择](/docs/services/EventStreams/eventstreams087.html)。
<!-- end STAGING ONLY -->

