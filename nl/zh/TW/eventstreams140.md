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

# 經典方案常見問題 
{: #faqs_classic}

對經典方案中有關 {{site.data.keyword.IBM}} {{site.data.keyword.messagehub}} 服務常見問題的回答。


如需所有 {{site.data.keyword.messagehub}} 方案的相關問題回答，請參閱[常見問題](/docs/services/EventStreams?topic=eventstreams-faqs#faqs)。
{: shortdesc}

<!--17/10/17 - Karen: same info duplicated at messagehub104 -->
## 如何使用 Kafka API 建立及刪除主題？
{: #topic_admin_classic}
{: faq}

如果您使用 Kafka 用戶端 0.11 或更新版本，或 Kafka Streams 0.10.2.0 或更新版本，可以使用 API 來建立及刪除主題。我們已對您建立主題時接受的設定做了一些限制。目前，您只能修改下列設定：

<dl>
<dt>cleanup.policy</dt>
<dd>設為 <code>delete</code>（預設值）、<code>compact</code> 或 <code>delete,compact</code>
<p>**附註：**如果清理原則是僅 <code>compact</code>，我們會自動新增 <code>delete</code> 但停用根據時間刪除。主題中的訊息在刪除之前最多可壓縮到 1 GB。</p>
</dd>

<dt>retention.ms</dt>
<dd>預設保留期間是 24 小時。最短 1 小時，最長 30 天。請以小時的倍數來指定此值。

</dd>
</dl>


## {{site.data.keyword.messagehub}} 為消費者偏移主題所設定的日誌保留時間範圍有多長？
{: #offsets_classic }
{: faq}

{{site.data.keyword.messagehub}} 會保留消費者偏移 7 天。這對應於 Kafka 配置 offsets.retention.minutes。 

偏移保留屬於系統層面，因此您不能在個別主題層次設定。所有消費者群組只會得到 7 天的儲存偏移，即使是使用已增加到最長 30 天日誌保留的主題。 

<!--following message retention info duplicted in eventstreams057 and evenstreams108-->

## 訊息保留多久？
{: #messages_retained_classic}

依預設，訊息在 Kafka 中的保留時間為最長 24 小時，每個分割區的上限為 1 GB。如果到達 1 GB 的限制，將會捨棄最舊的訊息，以維持在限制之內。

當您以使用者介面或管理 API 建立主題時，可以變更訊息保留的時間限制。時間限制為最短一小時，最長 30 天。

如需使用 Kafka 用戶端或 Kafka Streams 建立主題時所接受設定之限制的相關資訊，請參閱[如何使用 Kafka API 刪除主題？](/docs/services/EventStreams?topic=eventstreams-faqs_classic#topic_admin_classic)。


## {{site.data.keyword.messagehub}} 的可用性行為為何？
{: #availability_classic}
{: faq}

如果您撰寫 {{site.data.keyword.messagehub}} 應用程式，請使用此資訊來瞭解正常的 {{site.data.keyword.messagehub}} 可用性行為為何，以及預期您的應用程式要處理的內容。

### API
{: #api_availability_classic }

在 {{site.data.keyword.messagehub}} 的日常作業中，Kafka 叢集的節點有時會重新啟動。在某些情況下，您的應用程式會知道，因為叢集會重新指派資源。請將您的應用程式撰寫成對於這些變更具復原力，且能夠重新連接並重試作業。

### {{site.data.keyword.messagehub}} 橋接器 
{: #bridge_availability_classic }

請將您的應用程式撰寫成可以處理橋接器會偶爾重新啟動的可能性。

## {{site.data.keyword.messagehub}} 的訊息大小上限為何？ 
{: #max_message_size_classic }
{: faq}

{{site.data.keyword.messagehub}} 的訊息大小上限是 1 MB，這是 Kafka 預設值。 

## {{site.data.keyword.messagehub}} 的抄寫設定為何？ 
{: #replication_classic }
{: faq}

{{site.data.keyword.messagehub}} 已配置為提供高度可用性及延續性。
下列配置設定套用於所有主題，且無法變更：
* replication.factor = 3
* min.insync.replicas = 2

## {{site.data.keyword.messagehub}} 在經典方案的計費作業如何運作？ 
{: #billing_classic }
{: faq}

經典方案的 {{site.data.keyword.messagehub}} 會定期對使用者的主題計數進行取樣，而 {{site.data.keyword.Bluemix_notm}} 會記錄每一天的最大取樣值。{{site.data.keyword.messagehub}} 會針對所看到的最大並行分割區數目及每日傳送與接收的訊息數總和開立帳單。

例如，如果您一天內建立及刪除 1 個主題 10 次，則會針對最多 1 個主題向您收費。不過，如果您建立 10 個主題並予以刪除，則可能會針對 0 個或 10 個主題向您收費（視取樣發生時機）。


{{site.data.keyword.messagehub}} 會針對每一則訊息或每 64 k 開立帳單。高達 64 k 的訊息計為 1 則計費訊息。大於 64 k 的訊息則計為下列計費訊息數目：<code><var class="keyword varname">message_size</var> &divide; 64 k</code>。

<!--12/04/18 - Karen: same info duplicated at messagehub057 -->
## Kafka REST API 多久重新啟動一次？ 
{: #REST_restart_classic }
{: faq}

Kafka REST API 每天會重新啟動一次，需要一小段時間。 

在此期間內，Kafka REST API 可能變成無法使用。如果發生這種情況，建議您重試要求。REST API 重新啟動之後，您需要重建 Kafka 消費者實例。如果是這種情況，REST API 會傳回下列 JSON：

```'{"error_code":40403,"message":"Consumer instance not found."}'
```
{:screen}

## {{site.data.keyword.messagehub}} 方案之間有何不同？
{: #plan_compare_classic }
{: faq}

如需其他 {{site.data.keyword.messagehub}} 方案的相關資訊，請參閱[選擇方案](/docs/services/EventStreams?topic=eventstreams-plan_choose)。如需經典方案的資訊，請參閱[經典方案概觀](/docs/services/EventStreams?topic=eventstreams-plan_choose_classic#plan_choose_classic)。


## 如何處理災難回復？
{: #disaster_recovery_classic }
{: faq}

目前，使用者負責管理自己的 {{site.data.keyword.messagehub}} 災難回復。可以在某個位置（地區）的 {{site.data.keyword.messagehub}} 實例與不同位置的另一個實例之間抄寫 {{site.data.keyword.messagehub}} 資料。不過，使用者負責佈建遠端 {{site.data.keyword.messagehub}} 實例以及管理抄寫。

使用者也負責備份訊息有效負載資料。雖然此資料會抄寫至叢集內的多個 Kafka 分配管理系統，以防禦大部分的失敗，但是此抄寫無法防禦整個位置的失敗。 

{{site.data.keyword.messagehub}} 會備份主題名稱。















