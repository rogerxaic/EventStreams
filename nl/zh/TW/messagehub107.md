---

copyright:
  years: 2015, 2018
lastupdated: "2017-09-26"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Message Hub 是什麼？

{{site.data.keyword.messagehub}} 使用主題來實作發佈/訂閱傳訊。不像其他傳訊系統，{{site.data.keyword.messagehub}} 提供主題，但不提供佇列。{{site.data.keyword.messagehub}} 也提供其他功能，例如對其他系統的橋接器。

{{site.data.keyword.messagehub}} 是以開放程式碼的 Apache Kafka 專案為基礎，這是一個高度可擴充、高效能的傳訊骨幹，在許多正式作業環境已經過證實。如需相關資訊，請參閱 [{{site.data.keyword.messagehub}} 和 Apache Kafka](/docs/services/MessageHub/messagehub073.html)。Apache Kafka 工具通常會直接與 {{site.data.keyword.messagehub}} 一起運作，不過您需要提供其他配置，因為與 {{site.data.keyword.messagehub}} 的連線一律都需要使用認證來進行鑑別。

{{site.data.keyword.messagehub}} 有三個 API：Kafka API、Kafka REST API 及 {{site.data.keyword.mql}} API。在大部分情況下，Kafka API 是最佳選擇。如需使用 API 建立傳訊應用程式的相關資訊，請參閱[使用 Kafka 用戶端](/docs/services/MessageHub/messagehub050.html)、[使用 Kafka REST API](/docs/services/MessageHub/messagehub025.html) 及[使用 MQ Light API 用戶端](/docs/services/MessageHub/messagehub075.html)。

{{site.data.keyword.messagehub}} 也支援對於其他精選系統的橋接器。橋接器是通往另一個系統的單向鏈結。橋接器可以從另一個系統取得訊息然後發佈到主題，或是從主題取用訊息然後傳送到另一個系統。如此，您可以使用
{{site.data.keyword.messagehub}} 和其他系統整合，而不需撰寫程式碼。如需相關資訊，請參閱[使用橋接器鏈結至其他服務](/docs/services/MessageHub/messagehub088.html)。
