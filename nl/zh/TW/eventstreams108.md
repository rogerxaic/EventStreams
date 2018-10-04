---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-08"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 常見問題
{: #faqs}

{{site.data.keyword.IBM}} {{site.data.keyword.messagehub}} 服務常見問題的回答。
{: shortdesc}

<!--17/10/17 - Karen: same info duplicated at messagehub104 -->
## 如何使用 Kafka API 建立及刪除主題？
{: #topic_admin}

如果您使用 Kafka 用戶端 0.11 或更新版本，或 Kafka Streams 0.10.2.0 或更新版本，可以使用 API 來建立及刪除主題。我們已對您建立主題時接受的設定做了一些限制。目前，您只能修改下列設定：

<dl>
<dt>cleanup.policy</dt>
<dd>設為 <code>delete</code>（預設值）、<code>compact</code> 或 <code>delete,compact</code>
<p>**附註：**如果清理原則是僅 <code>compact</code>，我們會自動新增 <code>delete</code> 但停用根據時間刪除。主題中的訊息在刪除之前最多可壓縮到 1 GB。</p>
</dd>

<dt>retention.ms</dt>
<dd>預設保留期間是 24 小時。最短 1 小時，最長 30 天。請以小時的倍數來指定此值。



<p>**附註：**在企業方案中，您可以將此設定為任何值。</p>
</dd>

<dt>retention.bytes</dt>
<dd>在我們捨棄舊日誌區段以釋放空間之前，分割區（由日誌區段所組成）可以成長到的大小上限。

<p>**附註：**僅限企業方案。請設為大於 1 MB 的任何值。</p>
</dd>

<dt>segment.bytes</dt>
<dd>日誌的區段檔案大小。

<p>**附註：**僅限企業方案。請設為大於 100 KB 的任何值。</p>
</dd>

<dt>segment.index.bytes</dt>
<dd>將偏移對映至檔案位置的索引大小。 

<p>**附註：**僅限企業方案。請設為 100 KB 到 2 GB 之間的任何值。</p>
</dd>

<dt>segment.ms</dt>
<dd>時段，在此時段之後，Kafka 將強制日誌滾動，即使區段檔案尚未填滿。 

<p>**附註：**僅限企業方案。請設為 5 分鐘到 30 天之間的任何值。</p>
</dd>
</dl>


## {{site.data.keyword.messagehub}} 為消費者偏移主題所設定的日誌保留時間範圍有多長？
{: #offsets }
{{site.data.keyword.messagehub}} 會保留消費者偏移 7 天。這對應於 Kafka 配置 offsets.retention.minutes。 

偏移保留屬於系統層面，因此您不能在個別主題層次設定。所有消費者群組只會得到 7 天的儲存偏移，即使是使用已增加到最長 30 天日誌保留的主題。 

## {{site.data.keyword.messagehub}} 的可用性行為為何？
{: #availability}

如果您撰寫 {{site.data.keyword.messagehub}} 應用程式，請使用此資訊來瞭解正常的 {{site.data.keyword.messagehub}} 可用性行為為何，以及預期您的應用程式要處理的內容。

### API
{: #api_availability }

在 {{site.data.keyword.messagehub}} 的日常作業中，Kafka 叢集的節點有時會重新啟動。在某些情況下，您的應用程式會知道，因為叢集會重新指派資源。請將您的應用程式撰寫成對於這些變更具復原力，且能夠重新連接並重試作業。

### {{site.data.keyword.messagehub}} 橋接器（僅限標準方案）
{: #bridge_availability }

請將您的應用程式撰寫成可以處理橋接器會偶爾重新啟動的可能性。

## {{site.data.keyword.messagehub}} 的訊息大小上限為何？ 
{: #max_message_size }

{{site.data.keyword.messagehub}} 的訊息大小上限是 1 MB，這是 Kafka 預設值。 

## {{site.data.keyword.messagehub}} 的抄寫設定為何？ 
{: #replication }

{{site.data.keyword.messagehub}} 已配置為提供高度可用性及延續性。
下列配置設定套用於所有主題，且無法變更：
* replication.factor = 3
* min.insync.replicas = 2

## {{site.data.keyword.messagehub}} 在標準方案的計費作業如何運作？ 
{: #billing }

標準方案的 {{site.data.keyword.messagehub}} 會定期對使用者的主題計數進行取樣，而 {{site.data.keyword.Bluemix_notm}} 會記錄每一天的最大取樣值。{{site.data.keyword.messagehub}} 會針對所看到的最大並行分割區數目及每日傳送與接收的訊息數總和開立帳單。

例如，如果您一天內建立及刪除 1 個主題 10 次，則會針對最多 1 個主題向您收費。不過，如果您建立 10 個主題並予以刪除，則可能會針對 0 個或 10 個主題向您收費（視取樣發生時機）。


{{site.data.keyword.messagehub}} 會針對每一則訊息或每 64 k 開立帳單。高達 64 k 的訊息計為 1 則計費訊息。大於 64 k 的訊息則計為下列計費訊息數目：<code><var class="keyword varname">message_size</var> &divide; 64 k</code>。

<!--12/04/18 - Karen: same info duplicated at messagehub057 -->
## Kafka REST API 多久重新啟動一次？ 
{: #REST_restart }

Kafka REST API 每天會重新啟動一次，需要一小段時間。 

在此期間內，Kafka REST API 可能變成無法使用。如果發生這種情況，建議您重試要求。REST API 重新啟動之後，您需要重建 Kafka 消費者實例。如果是這種情況，REST API 會傳回下列 JSON：

```'{"error_code":40403,"message":"Consumer instance not found."}'
```
{:screen}

## {{site.data.keyword.messagehub}} 標準與 {{site.data.keyword.messagehub}} 企業方案的差異為何？
{: #plan_compare }

若要找出這兩個不同 {{site.data.keyword.messagehub}} 方案的相關資訊，請參閱[選擇方案](/docs/services/EventStreams/eventstreams085.html)。

## 如何處理災難回復？
{: #disaster_recovery }

目前，使用者負責管理自己的 {{site.data.keyword.messagehub}} 災難回復。可以在某個地區的 {{site.data.keyword.messagehub}} 實例與不同地區的另一個實例之間抄寫 {{site.data.keyword.messagehub}} 資料。不過，使用者負責佈建遠端 {{site.data.keyword.messagehub}} 實例以及管理抄寫。

使用者也負責備份訊息有效負載資料。雖然此資料會抄寫至叢集內的多個 Kafka 分配管理系統，以防禦大部分的失敗，但是此抄寫無法保障整個地區的失敗。 

{{site.data.keyword.messagehub}} 會備份主題名稱。















