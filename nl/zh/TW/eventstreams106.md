---

copyright:
  years: 2015, 2019
lastupdated: "2018-10-29"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 什麼是 MQ Light API，它有什麼不同？
{: #mqlight_api}

<!-- 30/10/18: info moved to eventstreams075.md because of doc app changes -->
** MQ Light API 只提供於標準方案中。**
<br/>

{{site.data.keyword.mql}} API 提供了與 Kafka 相較之下更高的摘要層次。
{:shortdesc}

使用 Kafka 用戶端或是 {{site.data.keyword.mql}} API 之間的選擇，取決於您想要建置的傳訊拓蹼：

* 使用 Kafka，您會使用少量的主題，且可以針對每個主題有多個分割區，獲得額外的可擴充性。您可以使用消費者群組在消費者之間共用訊息，但每個消費者必須能夠跟上指派給他的分割區訊息速率。
* 使用 {{site.data.keyword.mql}} API，您可以使用大量的主題，且主題名稱為階層式（例如：<code>'/sports/football'</code> 及 <code>'/sports/tiddlywinks'</code>）。 

{{site.data.keyword.mql}} API 中的主題與 Kafka 主題不同。相反地，{{site.data.keyword.mql}} API 會使用稱為 "MQLight"
的單一 Kafka 主題，使用 {{site.data.keyword.mql}} API 傳送和接收的所有訊息會通過那一個 Kafka 主題。

{{site.data.keyword.mql}} 僅適用於下列 {{site.data.keyword.Bluemix_notm}} 地區：美國南部（達拉斯）、英國南部（倫敦）和亞太地區南部（雪梨）。MQ Light API 無法用於歐盟中部（法蘭克福）地區或 {{site.data.keyword.Bluemix_notm}} Dedicated。

<!-- begin STAGING ONLY -->
如需在 API 之間選擇的相關資訊，請參閱[在三個 API 之間抉擇](/docs/services/EventStreams?topic=eventstreams-choose_api)。
<!-- end STAGING ONLY -->

