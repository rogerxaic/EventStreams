---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-30"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 為何要使用 Kafka REST API？
{: #why_rest}

** Kafka REST API 只提供於標準方案中。**
<br/>

Kafka REST API 是可用於下列狀況的便利介面：

* 在延遲時間不是重要因素的特定低傳輸量使用案例中。
* 對於除錯及錯誤搜尋

Kafka REST API 不適合當作高傳輸量、低延遲時間的介面。對於這些類型的需求，建議您使用 Kafka API 與 {{site.data.keyword.messagehub}} 相互連接。如需相關資訊，請參閱[使用 Kafka 用戶端](/docs/services/MessageHub/messagehub050.html#kafka_using)。


