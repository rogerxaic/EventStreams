---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-03"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 何謂 {{site.data.keyword.messagehub}}？
{: #about}

{{site.data.keyword.messagehub_full}} 以 Apache Kafka 建置的高傳輸量訊息匯流排。它針對將事件汲取至 {{site.data.keyword.Bluemix_notm}} 及服務與應用程式之間的事件串流配送而最佳化。{{site.data.keyword.messagehub}} 先前稱為 Message Hub。
{: shortdesc}

您可以使用 {{site.data.keyword.messagehub}} 來完成下列作業：

* 將工作卸載至後端工作者節點應用程式。
* 將事件串流連接至串流分析，以實現強大的見解。
* 將事件資料提發佈多個應用程式，以便即時回應。

藉由使用 Apache Kafka 建置，它能直接獲益於在社群中發生的所有創新，並支援 Kafka 用戶端 API、Kafka Streams、Kafka Connect 以及 KSQL。


Apache Kafka 工具通常會直接與 {{site.data.keyword.messagehub}} 一起運作，不過您需要提供其他配置，因為與 {{site.data.keyword.messagehub}} 的連線一律都需要使用認證來進行鑑別。

在 {{site.data.keyword.messagehub}} 中，應用程式會藉由建立訊息並傳送給主題來傳送資料。為了接收訊息，應用程式會訂閱主題，並選擇接收主題的所有訊息或在它們之間共用訊息。{{site.data.keyword.messagehub}} 會以有序的序列來管理及維護訊息。 




