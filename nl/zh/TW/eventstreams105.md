---

copyright:
  years: 2015, 2019
lastupdated: "2018-06-01"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# MQ 橋接器
{: #mq_bridge}

**MQ 橋接器只提供於標準方案中。**
<br/>

MQ 橋接器容許您將訊息資料從 {{site.data.keyword.IBM_notm}} MQ 佇列傳送到 {{site.data.keyword.messagehub}} Kafka 主題。MQ 橋接器讓您能有效率地對您企業內產生的 {{site.data.keyword.IBM_notm}} MQ 訊息資料執行雲端式工作負載（例如，資料分析）。
{:shortdesc}

MQ 橋接器會連接至 {{site.data.keyword.IBM_notm}} MQ 佇列管理程式作為 MQ 用戶端，並從 MQ 佇列取用 MQ 訊息資料。橋接器會將每個 MQ 訊息轉換成 Kafka 記錄，並將訊息傳送至 {{site.data.keyword.messagehub}} Kafka 主題。

## 支援的 {{site.data.keyword.IBM_notm}} MQ 版本
{: #mq_support}

橋接器能搭配使用的 {{site.data.keyword.IBM_notm}} MQ 版本如下所示：

* {{site.data.keyword.IBM_notm}} MQ 8.0 版以及更新版本

## 保護 MQ 橋接器
{: #mq_bridge_security}

您可以配置 MQ 橋接器，以使用者 ID 及密碼向 {{site.data.keyword.IBM_notm}} MQ 進行鑑別。建議您將下列許可權只授與給與 MQ 橋接器實例相關聯的身分：

* CONNECT 權限。MQ 橋接器必須能夠連接至 MQ 佇列管理程式。
* MQ 橋接器配置要從該處取用之佇列的 GET 權限。

## 訊息可靠性及排序
{: #mq_bridge_reliability}

MQ 橋接器對於它傳送的訊息資料實作至少一次層次的可靠性。這表示橋接器不會遺失訊息資料，但可能偶爾針對某個 MQ 訊息序列產生重複的 Kafka 記錄序列。一般而言，當事件岔斷橋接器的訊息資料處理時，會發生重複。例如：

* 重新啟動 MQ 佇列管理程式
* MQ 佇列管理程式與橋接器之間的網路中斷
* 重新平衡 Kafka 主題分割區指派
* 橋接器和 Kafka 之間的網路中斷

為了確保 MQ 橋接器可以可靠地從 MQ 傳送訊息，橋接器會要求互斥存取它讀取訊息的來源 MQ 佇列。此存取可防止其他應用程式在橋接器使用 MQ 佇列的同時從 MQ 佇列接收訊息。如果其他應用程式已經在從佇列接收訊息，在這些應用程式結束之前，MQ 橋接器可能無法開始。

通常 MQ 橋接器會按照從 {{site.data.keyword.IBM_notm}} MQ 收到的順序將 MQ 訊息轉遞至 Kafka。不過，有時 MQ 訊息資料會在 Kafka 主題內重複（原因如前所述）。這種可能的重複現象可能導致 MQ 訊息的順序與相對應記錄儲存在 Kafka 的順序不同。

## Kafka 分割區指派
{: #mq_bridge_partition}

MQ 橋接器可支援控制將 {{site.data.keyword.IBM_notm}} MQ 訊息資料指派至 Kafka 主題分割區的選項。特定分割區內的訊息排序取決於針對訊息資料排序所選取的選項（請參閱[訊息可靠性及排序](#mq_bridge_reliability)）。支援的值如下所示：
<dl><dt>Default</dt>
<dd>使用 Kafka 的預設分割區指派，這表示來自 MQ 訊息的資料會平均地分散在 Kafka 主題的分割區內。</dd>
<dt>CorrelationId</dt>
<dd>具有相同相關性 ID 的 MQ 訊息會指派給相同的 Kafka 分割區。</dd>
<dt>GroupId</dt>
<dd>具有相同群組 ID 的 MQ 訊息會指派給相同的 Kafka 分割區。</dd>
</dl>

## 訊息大小限制
{: #mq_message}

您可以配置 {{site.data.keyword.IBM_notm}} MQ 以儲存因為太大而無法放入 {{site.data.keyword.messagehub}} Kafka 記錄的訊息。Kafka 記錄大小上限是
1 000 000 個位元組，不過當橋接器已配置要根據 MQ 相關性 ID 或 MQ 群組 ID 執行
Kafka 分割區指派時，會用到此容量的一部份。我們建議使用 MQ 橋接器傳送不大於 950 KB 的訊息。

如果 MQ 橋接器遇到太大而無法轉遞給 Kafka 的訊息，橋接器會捨棄訊息並將日誌項目寫入 Kibana儀表板。請考慮為橋接器接收訊息的來源 MQ 佇列設定 MAXMSGL
屬性，以防止 MQ 應用程式傳送無法使用 MQ 橋接器傳送的訊息到佇列。
