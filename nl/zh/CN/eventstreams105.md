---

copyright:
  years: 2015, 2018
lastupdated: "2018-06-01"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# MQ 网桥
{: #mq_bridge}

**MQ 网桥仅在标准套餐中提供。**
<br/>

通过 MQ 网桥，您可以将消息数据从 {{site.data.keyword.IBM_notm}} MQ 队列传输到 {{site.data.keyword.messagehub}} Kafka 主题。MQ 网桥支持针对您企业中生成的 {{site.data.keyword.IBM_notm}} MQ 消息数据，高效地执行云样式工作负载（例如，数据分析）。
{:shortdesc}

MQ 网桥作为 MQ 客户机连接到 {{site.data.keyword.IBM_notm}} MQ 队列管理器，并使用来自 MQ 队列的 MQ 消息数据。MQ 网桥会将每条 MQ 消息转换成一条 Kafka 记录，并将 MQ 消息发送到 {{site.data.keyword.messagehub}} Kafka 主题。

## 支持的 {{site.data.keyword.IBM_notm}} MQ 版本
{: #mq_support}

网桥使用的 {{site.data.keyword.IBM_notm}} MQ 版本如下：

* {{site.data.keyword.IBM_notm}} MQ V8.0 及更高版本

## 保护 MQ 网桥安全
{: #mq_bridge_security}

可以将 MQ 网桥配置为使用用户标识和密码来向 {{site.data.keyword.IBM_notm}} MQ 进行认证。我们建议您仅对与 MQ 网桥实例关联的身份授予以下许可权：

* CONNECT 权限。MQ 网桥必须能够连接到 MQ 队列管理器。
* GET 权限，用于获取 MQ 网桥配置为使用其中消息的队列。

## 消息可靠性和排序
{: #mq_bridge_reliability}

MQ 网桥对其传输的消息数据实施“至少一次”级别的可靠性。这意味着网桥不会丢失消息数据，但对于给定序列的 MQ 消息，有时可能会生成重复序列的 Kafka 记录。通常，在有事件中断网桥对消息数据的处理时，会发生重复。例如：

* 重新启动 MQ 队列管理器
* MQ 队列管理器与网桥之间的网络中断
* 重新均衡 Kafka 主题分区分配
* 网桥与 Kafka 之间的网络中断

为了确保 MQ 网桥能可靠地传输来自 MQ 的消息，网桥会请求对自己从中读取消息的 MQ 队列的互斥访问权。此访问权会阻止其他应用程序在网桥使用 MQ 队列时接收来自该队列的消息。如果其他应用程序已经在接收来自该队列的消息，那么在这些应用程序结束之前，MQ 网桥可能会无法启动。

通常，MQ 网桥会将 MQ 消息按照从 {{site.data.keyword.IBM_notm}} MQ 接收的顺序转发到 Kafka。然而，有时 MQ 消息数据在 Kafka 主题中会重复（原因如上所述）。这种可能的重复会导致 MQ 消息的顺序与 Kafka 中存储对应记录的顺序不一致。

## Kafka 分区分配
{: #mq_bridge_partition}

MQ 网桥支持用于控制将 {{site.data.keyword.IBM_notm}} MQ 消息数据分配给 Kafka 主题分区的选项。特定分区内的消息排序取决于为消息数据排序所选的选项（请参阅[消息可靠性和排序](#mq_bridge_reliability)）。支持的值如下所示：
<dl><dt>缺省值</dt>
<dd>将使用 Kafka 的缺省分区分配，这意味着 MQ 消息中的数据会平均分发到 Kafka 主题的各个分区上。</dd>
<dt>CorrelationId</dt>
<dd>具有相同相关标识的 MQ 消息会分配给同一 Kafka 分区。</dd>
<dt>GroupId</dt>
<dd>具有相同组标识的 MQ 消息会分配给同一 Kafka 分区。</dd>
</dl>

## 消息大小限制
{: #mq_message}

可以对 {{site.data.keyword.IBM_notm}} MQ 进行配置，以存储那些由于太大而无法存入 {{site.data.keyword.messagehub}} Kafka 记录中的消息。最大 Kafka 记录大小为 1 000 000 字节，但如果网桥配置为根据 MQ 相关标识或 MQ 组标识来执行 Kafka 分区分配，那么会使用此容量中的一部分。我们建议使用 MQ 网桥发送大小不超过 950 千字节的消息。

如果 MQ 网桥遇到太大而无法转发到 Kafka 的消息，那么网桥会废弃此消息，并向 Kibana 仪表板写入相应的日志条目。请考虑为网桥从中接收消息的 MQ 队列设置 MAXMSGL 属性，以阻止 MQ 应用程序向队列发送无法使用 MQ 网桥传输的消息。
