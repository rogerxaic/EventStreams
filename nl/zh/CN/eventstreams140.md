---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-13"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:faq: data-hd-content-type='faq'}

# 经典套餐常见问题解答 
{: #faqs_classic}

对经典套餐中有关 {{site.data.keyword.IBM}} {{site.data.keyword.messagehub}} 服务的常见问题的解答。


有关所有 {{site.data.keyword.messagehub}} 套餐的相关问题的解答，请参阅[常见问题解答](/docs/services/EventStreams?topic=eventstreams-faqs#faqs)。
{: shortdesc}

<!--17/10/17 - Karen: same info duplicated at messagehub104 -->
## 如何使用 Kafka API 来创建和删除主题？
{: #topic_admin_classic}
{: faq}

如果使用的是 0.11 或更高版本的 Kafka 客户机，或者 0.10.2.0 或更高版本的 Kafka Streams，那么可以使用 API 来创建和删除主题。我们已经对创建主题时允许使用的设置施加了一些限制。当前，只能修改以下设置：

<dl>
<dt>cleanup.policy</dt>
<dd>设置为 <code>delete</code>（缺省值）、<code>compact</code> 或 <code>delete,compact</code>
<p>**注：**如果清除策略仅为 <code>compact</code>，那么会自动添加 <code>delete</code>，但会根据时间来禁用删除。在删除主题中的消息之前，会将其压缩到最高 1 GB。</p>
</dd>

<dt>retention.ms</dt>
<dd>缺省保留期为 24 小时。最小值为 1 小时，最大值为 30 天。请将此值指定为小时的倍数。

</dd>
</dl>


## {{site.data.keyword.messagehub}} 为使用者偏移量主题设置保留时间窗口需要多长时间？
{: #offsets_classic }
{: faq}

{{site.data.keyword.messagehub}} 保留使用者偏移量 7 天。这对应于 Kafka 配置 offsets.retention.minutes。 

偏移量保留时间适用于整个系统范围，所以不能在单个主题级别对其进行设置。所有使用者组都只能获得 7 天的存储偏移量，即便所使用主题的日志保留时间已经增大到最长 30 天也是如此。 

<!--following message retention info duplicted in eventstreams057 and evenstreams108-->

## 消息保留多久？
{: #messages_retained_classic}

缺省情况下，消息在 Kafka 中最多保留 24 小时，并且每个分区的上限是 1 GB。如果达到 1 GB 上限，那么将废弃最旧的消息以防超出限制。

使用用户界面或管理 API 创建主题时，可以更改消息保留时间的限制。时间限制最短为 1 小时，最长为 30 天。

有关使用 Kafka 客户机或 Kafka Streams 创建主题时所允许设置的限制的信息，请参阅[如何使用 Kafka API 创建和删除主题？](/docs/services/EventStreams?topic=eventstreams-faqs_classic#topic_admin_classic)。


## {{site.data.keyword.messagehub}} 的可用性行为如何？
{: #availability_classic}
{: faq}

编写 {{site.data.keyword.messagehub}} 应用程序时，请使用此信息来了解正常的 {{site.data.keyword.messagehub}} 可用性行为以及您预期应用程序处理的内容。

### API
{: #api_availability_classic }

作为 {{site.data.keyword.messagehub}} 日常操作的一部分，Kafka 集群的节点有时会重新启动。
在某些情况下，应用程序将识别到集群重新分配了资源。编写应用程序时，使其能够迅速从这些更改中恢复，能够重新连接并重试操作。

### {{site.data.keyword.messagehub}} 网桥 
{: #bridge_availability_classic }

编写应用程序时，使其能够处理网桥可能不时重新启动的情况。

## {{site.data.keyword.messagehub}} 的最大消息大小是多少？ 
{: #max_message_size_classic }
{: faq}

{{site.data.keyword.messagehub}} 的最大消息大小为 1 MB，这是 Kafka 的缺省值。 

## {{site.data.keyword.messagehub}} 的复制设置是什么？ 
{: #replication_classic }
{: faq}

{{site.data.keyword.messagehub}} 配置为提供强大的可用性和耐用性。
以下配置设置适用于所有主题且不能更改：
* replication.factor = 3
* min.insync.replicas = 2

## 经典套餐中 {{site.data.keyword.messagehub}} 如何记帐？ 
{: #billing_classic }
{: faq}

经典套餐上的 {{site.data.keyword.messagehub}} 定期对用户的主题计数进行采样，{{site.data.keyword.Bluemix_notm}} 记录每天的最大采样值。{{site.data.keyword.messagehub}} 对查看的最大并发分区数以及每天发送和接收的消息总数进行记帐。

例如，如果您创建 1 个主题随后删除了该主题，并且此操作在一天中执行了 10 次，那么将按最多为 1 个主题收费。但是，如果您创建 10 个主题随后将其删除，那么可能会按 0 个主题收费，也可能会按 10 个主题收费，具体取决于执行采样的时间。

{{site.data.keyword.messagehub}} 会按每条消息或按每 64K 记帐。不大于 64K 的消息按 1 个可记帐消息计数。大于 64K 的消息按以下可记帐消息数计数：<code><var class="keyword varname">message_size</var> &divide; 64K</code>。

<!--12/04/18 - Karen: same info duplicated at messagehub057 -->
## Kafka REST API 多久重新启动一次？ 
{: #REST_restart_classic }
{: faq}

Kafka REST API 每天重新启动一次，重新启动需要很短的一段时间。 

在此时间段内，Kafka REST API 可能会变得不可用。如果发生此情况，建议重试请求。重新启动 REST API 后，必须再次创建 Kafka 使用者实例。否则，REST API 会返回以下 JSON：

```'{"error_code":40403,"message":"Consumer instance not found."}'
```
{:screen}

## {{site.data.keyword.messagehub}} 套餐之间有何不同？
{: #plan_compare_classic }
{: faq}

要了解有关其他 {{site.data.keyword.messagehub}} 套餐的更多信息，请参阅[选择套餐](/docs/services/EventStreams?topic=eventstreams-plan_choose)。有关经典套餐的信息，请参阅[经典套餐概述](/docs/services/EventStreams?topic=eventstreams-plan_choose_classic#plan_choose_classic)。


## 我要如何处理灾难恢复？
{: #disaster_recovery_classic }
{: faq}

目前，用户需要负责管理他们自己的 {{site.data.keyword.messagehub}} 灾难恢复。{{site.data.keyword.messagehub}} 数据可以在一个位置（区域）中的 {{site.data.keyword.messagehub}} 实例与其他位置中的另一个实例之间进行复制。但是，用户需要负责供应远程 {{site.data.keyword.messagehub}} 实例和管理复制。

用户还需要负责备份消息有效内容数据。此数据在集群内多个 Kafka 代理程序之间复制，避免了大多数故障造成的影响，但这种复制不能补救整个位置的故障。 

主题名称由 {{site.data.keyword.messagehub}} 进行备份。















