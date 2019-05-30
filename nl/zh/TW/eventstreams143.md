---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-15"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}

# 升級到新的 {{site.data.keyword.messagehub}} 標準方案 
{: #migrate_classic_plan}

新版的多方承租戶標準方案在備援性、功能性和可用性方面有顯著改進。如需相關資訊，請參閱[新的標準方案部落格公告 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/cloud/blog/announcements/ibm-event-streams-releases-a-new-and-enhanced-standard-plan)。
{: shortdesc}

若要將應用程式從先前的標準方案（現在稱為經典方案）移轉到新的方案，請考慮下列資訊。

服務實例現在作為 {{site.data.keyword.cloud_notm}} 服務而不是 Cloud Foundry 服務佈建。這將讓服務能支援最新的 {{site.data.keyword.cloud_notm}} 標準和功能，包括多區域部署和精細的存取控制，但會影響服務的使用方式。尤其是需要考慮下列方面：

* **建立、刪除和列出服務**
<br/>
    像以前一樣，您可以使用 {{site.data.keyword.cloud_notm}} 主控台或 {{site.data.keyword.cloud_notm}} CLI 指令行工具管理服務的生命週期。如果您使用的是主控台，那麼服務現在將在**服務**下而不是 **Cloud Foundry 服務**下列出。 
    
    如果您使用的是 CLI，將使用資源指令管理實例。例如，<code>ibmcloud resource service-instance-create</code>。而不使用 **cf** 指令，例如 <code>ibmcloud cf create-service</code>。

* **控制存取**
<br/>
    現在使用 Cloud Identity and Access Management (IAM) 服務管理鑑別和授權。為了控制使用者的連接能力，IAM 還可讓您配置對基礎資源（例如主題）的精細存取權。存取權是透過向使用者指派原則來控制的。如需相關資訊，請參閱[管理對 {{site.data.keyword.messagehub}} 資源的存取](/docs/services/EventStreams?topic=eventstreams-security)。

<ul>
<li><strong>連接應用程式</strong>
<br/>
    應用程式進行連接所需的資訊不變，也就是說，還是需要 <code>bootstrap.servers</code>、<code>username</code> 和 <code>password</code> 的清單。但是，擷取這些內容的方式已變更。

<ul>
<li>
      <strong>對於原生應用程式</strong>
        <br/>
您必須分別使用 IBM Cloud 主控台或 CLI，建立認證或服務金鑰物件。然後，您可以擷取所需的內容。如需相關資訊，請參閱[連接應用程式](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_external)。</li>
<br/>
<li><strong>對於 Cloud Foundry 應用程式</strong>
        <br/>
您必須透過建立服務別名，先將服務連結至應用程式的組織和空間。然後，您可以如常地從 VCAP_SERVICES 環境變數擷取所需的內容。如需相關資訊，請參閱[連接 {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting)。
</li>
</ul>
<br/>
注意，用戶端必須支援 TLS 的 SNI 延伸，其中伺服器的主機名稱包含在 TLS 信號交換中。在[選擇 Kafka 用戶端以用於 {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_clients) 中建議的所有用戶端版本中，通常會提供並支援此特性。
</li>
</ul>

<br/>
您還應注意如下所示的其他一些變更：

* **Kafka 版本**
<br/>
    此方案提供對最新穩定的 2.2 版 Kafka  的存取權。無需更改就可以執行針對 Kafka 1.1 開發的應用程式，但是對於建議的用戶端版本和測試的組合，請參閱下列資訊：
[選擇 Kafka 用戶端以用於 {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_clients)。 

* **支援的地區**
<br/>
    本方案最初僅在 us-south 中提供。其餘地區將在未來幾週內引進。

* ** 整合       **
<br/>
    從 {{site.data.keyword.iot_short_notm}} 或 {{site.data.keyword.openwhisk_short}} 之類的其他服務進行的連線使用 Cloud Foundry 組織和空間連結到服務，需要建立服務別名。如需相關資訊，請參閱[將 Cloud Foundry 應用程式連接至 {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_cf)。
    

* **支援的功能**
<br/>
    經典方案和新的標準方案的功能有不同之處。要與提供的供應項目保持一致，請採用新的技術選項，並移除使用較少的特性，並非所有功能都會繼續提供。[選擇您的方案](/docs/services/EventStreams?topic=eventstreams-plan_choose)中提供了特性的比較。如果您依賴這些功能，稍後將提供進一步的資訊以協助您移轉。
   
<br/>
每天會向正式作業環境小幅增加程式碼。因此，在 2019 年餘下的時間以及往後，您將看到我們的使用者體驗（以及其他方面）會有許多進一步的改進。即將發佈：

* **客戶度量值**
<br/>
    監視服務實例中活動的功能。

<br/>
要取得快速入門和熟悉運用新標準方案的步驟的快速說明，請試用[入門指導教學](/docs/services/EventStreams?topic=eventstreams-getting_started)。


