---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-09"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 限制和配额
{: #kafka_quotas }

{{site.data.keyword.messagehub}} 使用配额控制资源，例如服务可以使用的网络带宽。配额的类型和级别取决于您使用的是标准套餐还是企业套餐。

## 标准套餐
{: #limits_standard }

### 网络吞吐量
{: #standard_throughput }

每个服务实例的最大吞吐量相当于每个分区每秒 1 MB，最多每秒 20 MB。例如，对于有 10 个分区的服务实例，最大吞吐量是每秒 10 MB，对于 30 个分区，为每秒 20 MB。

将针对生产者和使用者分别度量吞吐量。如果超过此值，将应用节流，方法是稍微延迟请求响应，从而有效地对生产者和使用者应用微调制动器，直到带宽减少。

### 分区
{: #standard_partitions}

每个服务实例 100 个分区。

### 保留
{: #standard_retention}

每个分区最大 1 GB。

### 其他限制
{: #standard_limits}

* 最大消息大小：1 MB
* 最大并发活动 Kafka 客户机数：100
* 每秒最大请求率 [HTTP Produce API]: 100
* 每秒最大请求率 [HTTP Admin API]: 10

## 企业套餐
{: #limits_enterprise }

### 网络吞吐量
{: #enterprise_throughput }

建议的最大吞吐量为每秒 40 MB（每秒峰值限制为 90 MB）。吞吐量以集群中每秒可以发送和接收的字节数表示。

此建议数字基于典型的工作负载，并考虑到运行操作的可能影响，例如，内部更新或故障模式，如可用性专区丢失。如果平均吞吐量超过建议数字，在这些状况下可能会遇到性能下降的情况。


### 分区
{: #enterprise_partitions}

每个服务实例 1000 个分区。

### 保留
{: #enterprise_retention}

无限制，直至达到套餐的存储限制。

### 其他限制
{: #enterprise_limits}

最大消息大小：1 MB




















