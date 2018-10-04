---

copyright:
  years: 2015, 2018
lastupdated: "2018-06-01"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 使用橋接器鏈結至其他服務
{: #bridges}

** {{site.data.keyword.messagehub}} 橋接器只提供於標準方案中。**
<br/>

{{site.data.keyword.messagehub}} 標準方案也支援對於其他精選系統的橋接器。橋接器是 {{site.data.keyword.messagehub}} 與另一個服務之間的單向鏈結。橋接器容許從 {{site.data.keyword.messagehub}} 讀取資料然後寫入另一個服務，或是從另一個服務讀取資料然後寫入 {{site.data.keyword.messagehub}}。橋接器可以從另一個系統取得訊息然後發佈到主題，或是從主題取用訊息然後傳送到另一個系統。如此，您可以使用 {{site.data.keyword.messagehub}} 和其他系統整合，而不需撰寫程式碼。
{:shortdesc}

使用 {{site.data.keyword.messagehub}} 橋接器的主要好處如下：  

* 您可以用管理方式定義橋接器，因此不需要撰寫應用程式碼。
* 每一個橋接器的生命週期由 {{site.data.keyword.messagehub}} 服務所監視及管理。例如，如果橋接器發現錯誤，{{site.data.keyword.messagehub}} 會自動重新啟動它。
* 橋接器與 {{site.data.keyword.Bluemix_short}} 平台整合。例如，橋接器所產生的記載及監視資訊，會導向至 Kibana 及 Grafana 儀表板。

您可能會發現橋接器適用於下列兩種常見的情境：

* 從一個以上的資料來源將資料取用到 {{site.data.keyword.messagehub}}。
* 將資料從 {{site.data.keyword.messagehub}} 傳送到另一個服務。例如，傳送到長期儲存空間。

## {{site.data.keyword.messagehub}} 橋接器
{: notoc}

* 我們提供下列類型的橋接器： 
  - [MQ 橋接器](/docs/services/EventStreams/eventstreams105.html){:new_window}，它會從 {{site.data.keyword.IBM}} MQ 取得訊息資料，然後傳送到 {{site.data.keyword.messagehub}} 的主題。我們打算在未來支援更廣泛的橋接器範圍。
  - [Cloud Object Storage 橋接器](/docs/services/EventStreams/eventstreams115.html){:new_window}，它會將 {{site.data.keyword.messagehub}} 資料傳送到 [{{site.data.keyword.IBM_notm}} Cloud Object Storage ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/cloud-object-storage/about-cos.html){:new_window} 服務的實例。 
    
    Cloud Object Storage 服務是 {{site.data.keyword.Bluemix_short}} 的偏好物件儲存空間服務。
  - [{{site.data.keyword.objectstorageshort}} 橋接器](/docs/services/EventStreams/eventstreams089.html){:new_window}，它會將 {{site.data.keyword.messagehub}} 資料傳送到 [Object Storage 服務 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/ObjectStorage/index.html){:new_window} 的實例。
* 目前，橋接器可以用於所有 {{site.data.keyword.Bluemix_notm}} 公用環境。橋接器無法用於「{{site.data.keyword.Bluemix_short}} 專用」。
* 您可以用下列兩種方式管理橋接器：
  - 使用 [REST API ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-messaging/event-streams-docs){:new_window}，這是現有 {{site.data.keyword.messagehub}} 管理 API 的延伸。您也可以在 [message-hub-docs ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-messaging/event-streams-docs){:new_window} 找到如何使用 curl 管理橋接器生命週期的範例。我們可能會在繼續開發橋接器時變更此 REST API。我們打算讓這個 API 穩定化。
  - 在 {{site.data.keyword.Bluemix_notm}} 主控台中使用 {{site.data.keyword.messagehub}} 儀表板。
* 您可以將最多兩個任何類型的橋接器與 {{site.data.keyword.messagehub}} 服務的實例相關聯。我們將在繼續開發橋接器時繼續檢閱此限制。
* 使用橋接器超出其傳訊作業時不會有額外的費用。
* MQ 橋接器不支援使用 SSL/TLS 以保護資料在橋接器與 MQ 佇列管理程式之間傳送時的隱私和完整性。我們打算為橋接器新增使用 SSL/TLS 的支援。 
* 不過，您可以使用 [{{site.data.keyword.SecureGatewayfull}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/SecureGateway/secure_gateway.html){:new_window}
服務，透過 {{site.data.keyword.Bluemix_notm}} 與內部部署安裝的 {{site.data.keyword.SecureGateway}} 用戶端之間的安全通道傳送資料。在此配置中，通道的任一端都不會使用
SSL/TLS 來保護。
* Cloud Object Storage 及 {{site.data.keyword.objectstorageshort}} 橋接器在將資料分別寫入 Cloud Object Storage 或 {{site.data.keyword.objectstorageshort}} 時，會使用換行字元作為分隔字元來連結訊息。如此一來這些橋接器即不適用於包含內嵌換行字元的訊息，以及二進位訊息資料。
* Cloud Object Storage 及 {{site.data.keyword.objectstorageshort}} 橋接器目前所使用的物件命名慣例未來可能會變更。

## 從其他服務到 {{site.data.keyword.messagehub}} 的橋接器
{: notoc}

* {{site.data.keyword.iot_full}} 提供它自己的 [{{site.data.keyword.messagehub}} 橋接器](/docs/services/EventStreams/eventstreams119.html){:new_window}。橋接器提供到 {{site.data.keyword.messagehub}} 的單向鏈結，容許您儲存歷程資料。藉由將 {{site.data.keyword.messagehub}} 連接至 {{site.data.keyword.iot_short_notm}}，您可以使用 {{site.data.keyword.messagehub}} 作為事件管線，以從 Watson IoT Platform 中取用裝置事件，並即時將事件提供給平台的其餘部分使用。 


