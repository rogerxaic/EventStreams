---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-03"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}

# {{site.data.keyword.messagehub}} 可用性的服務水準合約 (SLA)  
{: #sla}

## 標準方案
{: #sla_standard}
在標準方案上提供的 {{site.data.keyword.messagehub}} 服務的可用性為 99.95%。如需 {{site.data.keyword.Bluemix}} 中高可用性服務的 SLA 相關資訊，請參閱 [{{site.data.keyword.Bluemix_notm}} 服務說明 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www-03.ibm.com/software/sla/sladb.nsf/8bd55c6b9fa8039c86256c6800578854/c4ceb9f019f9eb4c862582f9001b3994/$FILE/i126-6605-16_04-2019_en_US.pdf){:new_window}。


## 企業方案
{: #sla_enterprise}

使用企業方案且作為高可用性「公用」環境上，所提供的 {{site.data.keyword.messagehub}} 服務可用性為 99.95%。{{site.data.keyword.messagehub}} 服務以非 HA 環境執行時（例如[單一區域位置](#sla_szr)），可用性為 99.5%。如需 {{site.data.keyword.Bluemix_notm}} 中高可用性服務的 SLA 相關資訊，請參閱 [{{site.data.keyword.Bluemix_notm}} 服務說明 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www-03.ibm.com/software/sla/sladb.nsf/8bd55c6b9fa8039c86256c6800578854/c4ceb9f019f9eb4c862582f9001b3994/$FILE/i126-6605-16_04-2019_en_US.pdf){:new_window}。

## 經典方案
{: #sla_classic}
在經典方案上提供的 {{site.data.keyword.messagehub}} 服務的可用性為 99.5%。如需 {{site.data.keyword.Bluemix_notm}} 的 SLA 相關資訊，請參閱 [{{site.data.keyword.Bluemix_notm}} service description ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www-03.ibm.com/software/sla/sladb.nsf/8bd55c6b9fa8039c86256c6800578854/c4ceb9f019f9eb4c862582f9001b3994/$FILE/i126-6605-16_04-2019_en_US.pdf){:new_window}。

<!--
## What does 99.95% availability mean?
Availability refers to the ability of applications to produce and consume messages from Kafka topics.
-->

## 如何測量？
服務實例持續受到效能、錯誤率及其對合成作業之回應的監視。運作中斷會被記錄下來。如需相關資訊，請參閱 [Event Streams 的服務狀態 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/status?component=messagehub&selected=status){:new_window}。

可用性是指應用程式產生及耗用 Kafka 主題之訊息的能力。

## 達到這樣的可用性需要考量什麼？
從應用程式的角度而言，若要達到高層次的可用性，您應該考量[連線功能](/docs/services/EventStreams?topic=eventstreams-sla#connectivity)、[傳輸量](/docs/services/EventStreams?topic=eventstreams-sla#throughput)及[訊息的一致性及延續性](/docs/services/EventStreams?topic=eventstreams-sla#message_consistency)。使用者負責設計應用程式以便使他們企業的這三個元素最佳化。

### 連線功能
{: #connectivity}

由於雲端的動態本質，應用程式必須預期連線會中斷。連線中斷不視為服務失敗。

**重試**<br/>
Kafka 用戶端提供重新連接邏輯，但您必須明確地為生產者啟用重新連接。如需相關資訊，請參閱 [ <code>retries</code> 內容 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://kafka.apache.org/11/documentation.html#producerconfigs){:new_window}。連線會在 60 秒內重新進行。
 
**重複項目**<br/>
啟用重試可能導致重複的訊息。視連線遺失的時間而定，生產者可能無法判斷伺服器是否成功處理訊息，因此在重新連接時必須傳送訊息。建議您將應用程式設計為預期會有重複的訊息。 

如果無法容忍重複項目，您可以使用 <code>idempotent</code> 生產者特性（來自 Kafka 1.1）以避免重試期間的重複項目。如需相關資訊，請參閱 [ <code>enable.idempotence</code> 內容 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://kafka.apache.org/11/documentation.html#producerconfigs){:new_window}。

### 傳輸量
{: #throughput}

傳輸量會表達為在叢集中可以傳送及接收的每秒位元組數。

**標準方案的具體指引資訊**<br/>
如需傳輸量指引資訊，請參閱[限制和配額 - 標準](/docs/services/EventStreams?topic=eventstreams-kafka_quotas#standard_throughput)。 

**企業方案的具體指引資訊**<br/>

如需傳輸量指引資訊，請參閱[限制和配額 - 企業](/docs/services/EventStreams?topic=eventstreams-kafka_quotas#enterprise_throughput)。 

**測量**<br/>
建議您檢測應用程式，以便知道它們的執行情況。例如，傳送及接收的訊息數目、訊息大小，以及回覆碼。瞭解應用程式的用量有助於您適當地配置其資源，例如主題上的訊息保留時間。

**飽和度**<br/>
接近可以產生至叢集中的資料流量限制時，生產者會開始節流控制、延遲會增加，且最後會發生像是逾時錯誤之類的錯誤。視配置而定，訊息一致性及延續性也可能受到影響。如需相關資訊，請參閱[訊息的一致性及延續性](/docs/services/EventStreams?topic=eventstreams-sla#message_consistency)。

### 訊息的一致性及延續性
{: #message_consistency}

Kafka 會藉由將它收到的訊息抄寫在叢集中的其他節點（然後訊息便可用於失敗的情況），達到可用性及延續性。{{site.data.keyword.messagehub}} 使用三個抄本 (default.replication.factor = 3)，意思是節點收到的每則訊息會抄寫到不同可用性區域中的兩個其他節點。如此，便可以容忍節點或可用性區域的遺失，而不會遺失資料或功能。

**生產者 <code>acks</code> 模式**<br/>
雖然所有訊息都會抄寫，應用程式仍可以控制它們生產的訊息傳送到服務時的穩健程度，方法是使用生產者的 <code>acks</code> 模式內容。這個內容提供速度與訊息遺失風險之間的選擇。預設設定是 <code>acks=1</code>，這表示只要生產者連接到的節點確認收到訊息，但是在抄寫完成之前，便會傳回成功。建議及最受到保證的設定是 <code>acks=all</code>，生產者只有在訊息已複製到所有抄本之後才會傳回成功。如此可確保抄本維持同步，這樣可避免在失敗導致切換至抄本時發生訊息遺失。

## 單一區域位置部署
{: #sla_szr}

若要取得最高可用性，建議使用在多區域位置建置的高可用性「公用」環境。在多區域位置中，Kafka 叢集會分散至 3 個可用性區域，這表示叢集對於單一區域失敗或該區域內任何元件失敗的情況具有復原力。
部分客戶需要地理地區，因此想要在地理區域但單一區域位置中佈建 {{site.data.keyword.messagehub}} 叢集。{{site.data.keyword.messagehub}} 支援此部署模型，不過，請注意下列可用性取捨：
* 在單一區域位置中，具有單一失敗種類，可能會導致叢集離線一段時間。例如，整個資料中心失敗，或是基礎 Hypervisor、SAN 或網路這類共用元件的更新或失敗。這些失敗會反映在單一區域位置的精簡 SLA 中。
* 將 Kafka 分散到許多區域的優點是讓可能會導致整個叢集關閉的失敗機會降到最低。另一方面，單一失敗可能會導致一個區域內整個叢集關閉的機率較小。在特別情況下，也可能會遺失資料。例如，即使生產者使用 <code>acks=all</code>，如果所有 Kafka 節點都同時關閉，則可能會有分配管理系統確認收到一些訊息，但基礎檔案系統尚未完成清除到磁碟。這些未清除的訊息可能會遺失。 

    如需相關資訊，請參閱[訊息確認](/docs/services/EventStreams?topic=eventstreams-producing_messages#message_acknowledgments)。在許多使用案例中，這不一定是問題。不過，如果在任何情況下都不接受訊息遺失，則請考慮使用其他策略，例如使用多可用性區域叢集、跨地區抄寫或生產者端訊息檢查點檢查作業。

如需相關資訊，請參閱[單一區域叢集](/docs/containers?topic=containers-regions-and-zones#regions_single_zone)及[多區域叢集](/docs/containers?topic=containers-regions-and-zones#regions_multizone)。
