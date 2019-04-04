---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-06"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.messagehub}} 可用性（企业套餐）的服务级别协议 (SLA) 
{: #sla}

在企业套餐上提供的 {{site.data.keyword.messagehub}} 服务的可用性为 99.95%。有关 {{site.data.keyword.Bluemix}} 的 SLA 的更多信息，请参阅
[{{site.data.keyword.Bluemix_notm}} 服务描述 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www-03.ibm.com/software/sla/sladb.nsf/pdf/6605-14/$file/i126-6605-14_08-2018_en_US.pdf){:new_window}。

## 99.95% 的可用性意味着什么？
可用性是指应用程序生成和消耗 Kafka 主题中的消息的能力。

## 我们如何度量可用性？
服务实例的性能、错误率及其对合成操作的响应会受到持续监视。中断会进行记录。

## 要实现此可用性，您需要考虑什么？
要从应用程序角度实现高级别的可用性，您应考虑[连接性](/docs/services/EventStreams?topic=eventstreams-sla#connectivity)、
[吞吐量](/docs/services/EventStreams?topic=eventstreams-sla#throughput)以及[消息的一致性和耐久性](/docs/services/EventStreams?topic=eventstreams-sla#message_consistency)。用户负责设计其应用程序以优化其业务的这三个元素。

### 连接性
{: #connectivity}

因为云的性质是动态的，所以应用程序一定会发生连接中断情况。连接中断并不会视为服务故障。

**重试**<br/>
Kafka 客户机提供重新连接逻辑，但您必须明确支持生产者进行重新连接。有关更多信息，请参阅[ <code>retries</code> 属性 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://kafka.apache.org/11/documentation.html#producerconfigs){:new_window}。60 秒内再次进行连接。
**重复**<br/>
支持重试可能导致消息重复。根据连接丢失的时间，生产者可能无法确定服务器是否成功处理消息，因此必须在重新连接时再次发送消息。建议您将应用程序设计为预期有重复消息。 

如果无法容忍重复，您可以使用 <code>idempotent</code> 生产者功能（来自 Kafka 1.1）防止重试期间出现重复。有关更多信息，请参阅 [ <code>enable.idempotence</code> 属性 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://kafka.apache.org/11/documentation.html#producerconfigs){:new_window}。

### 吞吐量
{: #throughput}

吞吐量以集群中每秒可以发送和接收的字节数表示。

**建议**<br/>
每秒 40 MB（每秒最大峰值限制为 90 MB）。<br/>
此建议数字基于典型的工作负载，并考虑到运行操作的可能影响，例如，内部更新，或可用性专区丢失之类的故障模式。例如，具有少量有效内容（少于 10 K）的消息。如果平均吞吐量超过此数字，您在这些状况下可能会遇到性能下降的情况。

**度量**<br/>
建议您检测应用程序以了解其性能情况。例如，发送和接收的消息数、消息大小和返回码。了解应用程序的使用情况可帮助您恰当地配置其资源，例如有关主题的消息的保留时间。

**饱和度**<br/>
当可生成的流向集群的流量接近限制时，生产者开始遭遇调速限制，等待时间增加，最终会出现超时错误之类的错误。消息一致性和耐久性可能也会受到影响，具体取决于配置。有关更多信息，请参阅
[消息的一致性和耐久性](/docs/services/EventStreams?topic=eventstreams-sla#message_consistency)。

### 消息的一致性和耐久性
{: #message_consistency}

Kafka 通过将接收的消息复制到集群中的其他节点，以供在发生故障时使用，从而实现其可用性和耐久性。{{site.data.keyword.messagehub}} 使用三个副本 (default.replication.factor = 3)，意味着一个节点接收的每个消息都将复制到不同可用性专区中的其他两个节点。这样，在不丢失数据或功能的情况下，可以容忍丢失节点或可用性专区。

**生产者 <code>acks</code> 模式**<br/>
虽然会复制所有消息，但应用程序可以使用生产者的 <code>acks</code> 模式属性控制这些应用程序生产的消息如何稳健地传输给服务。此属性提供有关速度和消息丢失风险的选项。缺省设置为 <code>acks=1</code>，意味着生产者在其所连接的节点确认收到消息时，但在复制完成前，会立即返回成功消息。建议且最保险的设置是 <code>acks=all</code>，其中生产者仅在消息已经复制到所有副本后返回成功消息。这样可确保副本保持一致，从而防止在因发生故障而切换到某个副本时丢失消息。


