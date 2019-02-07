---

copyright:
  years: 2015, 2019
lastupdated: "2018-07-05"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 使用管理 Kafka Java 客户机 API
{: #kafka_java_api}


<!-- 
17/10/17 - Karen: following info duplicated at messagehub108
 -->

如果使用的是 0.11 或更高版本的 Kafka 客户机，或者 0.10.2.0 或更高版本的 Kafka Streams，那么可以使用 API 来创建和删除主题。我们已经对创建主题时允许使用的设置施加了一些限制。当前，只能修改以下设置：
{: shortdesc}

<dl>
<dt>cleanup.policy</dt>
<dd>设置为 <code>delete</code>（缺省值）、<code>compact</code> 或 <code>delete,compact</code>
<p>**注：**如果清除策略仅为 <code>compact</code>，那么会自动添加 <code>delete</code>，但会根据时间来禁用删除。在删除主题中的消息之前，会将其压缩到最高 1 GB。</p>
</dd>

<dt>retention.ms</dt>
<dd>缺省保留期为 24 小时。最小值为 1 小时，最大值为 30 天。请将此值指定为小时的倍数。



<p>**注：**
在企业套餐中，您可以将其设置为任何值。</p>
</dd>

<dt>retention.bytes</dt>
<dd>我们废弃旧的日志段以释放空间之前，分区（由日志段构成）可以增长到的最大大小。

<p>**注：**
仅限企业套餐。设置为大于 1 MB 的任何值。</p>
</dd>

<dt>segment.bytes</dt>
<dd>日志的段文件大小。

<p>**注：**
仅限企业套餐。设置为大于 100 kB 的任何值。</p>
</dd>

<dt>segment.index.bytes</dt>
<dd>将偏移量映射到文件位置的索引的大小。 

<p>**注：**
仅限企业套餐。设置为 100 kB 和 2 GB 之间的任何值。</p>
</dd>

<dt>segment.ms</dt>
<dd>一个时间段，在此时间段之后，即使段文件未满，Kafka 也将强制日志滚动。 

<p>**注：**
仅限企业套餐。设置为 5 分钟和 30 天之间的任何值。</p>
</dd>
</dl>

