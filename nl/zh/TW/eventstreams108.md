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

# 常見問題
{: #faqs}

{{site.data.keyword.IBM}} {{site.data.keyword.messagehub}} 服務常見問題的回答。

有關特定於經典方案的問題的解答，請參閱[經典方案常見問題解答](/docs/services/EventStreams?topic=eventstreams-faqs_classic)。
{: shortdesc}

<!--17/10/17 - Karen: same info duplicated at messagehub104 -->
## 如何使用 Kafka API 建立及刪除主題？
{: #topic_admin}
{: faq}

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
{: faq}

{{site.data.keyword.messagehub}} 會保留消費者偏移 7 天。這對應於 Kafka 配置 offsets.retention.minutes。 

偏移保留屬於系統層面，因此您不能在個別主題層次設定。所有消費者群組只會得到 7 天的儲存偏移，即使是使用已增加到最長 30 天日誌保留的主題。 

內部 Kafka <code>__consumer_offsets</code> 主題將以唯讀方式顯示。強烈建議您不要試圖以任何方式管理主題。 

<!--following message retention info duplicted in eventstreams057-->

## 訊息保留多久？
{: #messages_retained}

依預設，訊息在 Kafka 中的保留時間為最長 24 小時，每個分割區的上限為 1 GB。如果到達 1 GB 的限制，將會捨棄最舊的訊息，以維持在限制之內。

當您以使用者介面或管理 API 建立主題時，可以變更訊息保留的時間限制。時間限制為最短一小時，最長 30 天。

如需使用 Kafka 用戶端或 Kafka Streams 建立主題時所接受設定之限制的相關資訊，請參閱[如何使用 Kafka API 刪除主題？](/docs/services/EventStreams?topic=eventstreams-faqs#topic_admin)。

## {{site.data.keyword.messagehub}} 的可用性行為為何？
{: #availability}
{: faq}

如果您撰寫 {{site.data.keyword.messagehub}} 應用程式，請使用此資訊來瞭解正常的 {{site.data.keyword.messagehub}} 可用性行為為何，以及預期您的應用程式要處理的內容。

### API
{: #api_availability }

在 {{site.data.keyword.messagehub}} 的日常作業中，Kafka 叢集的節點有時會重新啟動。在某些情況下，您的應用程式會知道，因為叢集會重新指派資源。請將您的應用程式撰寫成對於這些變更具復原力，且能夠重新連接並重試作業。

## {{site.data.keyword.messagehub}} 的訊息大小上限為何？ 
{: #max_message_size }
{: faq}

{{site.data.keyword.messagehub}} 的訊息大小上限是 1 MB，這是 Kafka 預設值。 

## {{site.data.keyword.messagehub}} 的抄寫設定為何？ 
{: #replication }
{: faq}

{{site.data.keyword.messagehub}} 已配置為提供高度可用性及延續性。
下列配置設定套用於所有主題，且無法變更：
* replication.factor = 3 
* min.insync.replicas = 2

## {{site.data.keyword.messagehub}} 標準與 {{site.data.keyword.messagehub}} 企業方案的差異為何？
{: #plan_compare }
{: faq}

若要找出不同 {{site.data.keyword.messagehub}} 方案的相關資訊，請參閱[選擇方案](/docs/services/EventStreams?topic=eventstreams-plan_choose)。

## 如何處理災難回復？
{: #disaster_recovery }
{: faq}

目前，使用者負責管理自己的 {{site.data.keyword.messagehub}} 災難回復。可以在某個位置（地區）的 {{site.data.keyword.messagehub}} 實例與不同位置的另一個實例之間抄寫 {{site.data.keyword.messagehub}} 資料。不過，使用者負責佈建遠端 {{site.data.keyword.messagehub}} 實例以及管理抄寫。

使用者也負責備份訊息有效負載資料。雖然此資料會抄寫至叢集內的多個 Kafka 分配管理系統，以防禦大部分的失敗，但是此抄寫無法防禦整個位置的失敗。 

{{site.data.keyword.messagehub}} 會備份主題名稱。















