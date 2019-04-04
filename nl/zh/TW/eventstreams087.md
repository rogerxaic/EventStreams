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

# 在三個 API 之間抉擇（標準方案）
{: #choose_api}

{{site.data.keyword.messagehub}} 支援標準方案上的三個 API。下列資訊可協助您選擇最合適的一個。
{: shortdesc}

## 為何要使用 Kafka API？
{: #why_kafka notoc}

** Kafka API 提供於標準和企業方案中。**
<br/>

有一些理由，讓您可能選擇使用 Kafka API，而不使用 {{site.data.keyword.messagehub}} 提供的其他介面。這些理由包括：
{:shortdesc}


* 可以更輕鬆地整合應用程式與具有 Kafka 支援的現有系統，例如 {{site.data.keyword.IBM}} {{site.data.keyword.streaminganalyticsshort}} 及 {{site.data.keyword.sparks}}。
* 它提供比其他 API 低的延遲時間，以及更高的傳輸量。
* 它提供比 Kafka REST API 豐富的 API。

## 為何要使用 Kafka REST API？
{: #why_rest notoc}

** Kafka REST API 只提供於標準方案中。**
<br/>

Kafka REST API 是可用於下列狀況的便利介面：

* 開發人員想要開始使用 {{site.data.keyword.messagehub}} 時
* 在延遲時間不是重要因素的特定低傳輸量使用案例中。
* 對於除錯及錯誤搜尋

Kafka REST API 不能當作高傳輸量、低延遲時間的介面。對於這些類型的需求，建議您使用 Kafka API 與 {{site.data.keyword.messagehub}} 相互連接。如需相關資訊，請參閱[使用 Kafka 用戶端](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_using)。

## 為何要使用 {{site.data.keyword.mql}} API？
{: #why_mql notoc}

** MQ Light API 只提供於標準方案中。**
<br/>

{{site.data.keyword.mql}} API 針對 Java™、Node.js、Python 及 Ruby 提供了一個以 AMQP 為基礎的傳訊介面。提供 API 的原因是為了與較早期 {{site.data.keyword.mql}} 服務的舊版相容性。

















