---

copyright:
  years: 2015, 2019
lastupdated: "2018-06-06"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 在三个 API 中进行选择（标准套餐）
{: #choose_api}

标准套餐中 {{site.data.keyword.messagehub}} 支持三个 API。下面的一些信息可帮助您选择最适用的 API。
{: shortdesc}

## 为什么要使用 Kafka API？
{: #why_kafka notoc}

**Kafka API 可在标准和企业套餐中提供。**
<br/>

有一些原因可能促使您选择使用 Kafka API，而不是 {{site.data.keyword.messagehub}} 提供的其他界面。这些原因包括：
{:shortdesc}


* 可更轻松地将应用程序与支持 Kafka 的现有系统集成，例如 {{site.data.keyword.IBM}} {{site.data.keyword.streaminganalyticsshort}} 和 {{site.data.keyword.sparks}}。
* 与其他 API 相比，等待时间更短，吞吐量更高。
* 与 Kafka REST API 相比，所提供的 API 更为丰富。

## 为什么要使用 Kafka REST API？
{: #why_rest notoc}

**Kafka REST API 仅在标准套餐中提供。**
<br/>

Kafka REST API 是一个方便的接口，可在以下情况下使用：

* 开发者想要开始使用 {{site.data.keyword.messagehub}}
* 在等待时间不是关键因素的某些吞吐量较低的用例中
* 用于调试和排查故障

Kafka REST API 并不是一个等待时间短的高吞吐量接口。对于这些类型的需求，我们建议使用 Kafka API 与 {{site.data.keyword.messagehub}} 建立连接。有关更多信息，请参阅[使用 Kafka 客户机](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_using)。

## 为什么要使用 {{site.data.keyword.mql}} API？
{: #why_mql notoc}

**MQ Light API 仅在标准套餐中提供。**
<br/>

{{site.data.keyword.mql}} API 为 Java™、Node.js、Python 和 Ruby 提供基于 AMQP 的消息传递接口。提供该 API 是为了实现与较早版本 {{site.data.keyword.mql}} 服务的向后兼容性。

















