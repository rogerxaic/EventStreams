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

# 使用 REST 生產者 API
{: #rest_producer_using}


**REST 生產者 API 僅在新的 {{site.data.keyword.messagehub}} 標準方案中提供。**
<br/>

{{site.data.keyword.messagehub}} 提供 REST API 以協助您將現有系統連接至 {{site.data.keyword.messagehub}} Kafka 叢集。使用 API，您可以將 {{site.data.keyword.messagehub}} 與支援 RESTful API 的任何系統整合。

REST 生產者 API 是可調整的 REST 介面，用於透過安全 HTTP 端點向 {{site.data.keyword.messagehub}} 產生訊息。將事件資料傳送到 {{site.data.keyword.messagehub}}，利用 Kafka 技術處理資料資訊來源，並利用 {{site.data.keyword.messagehub}} 特性來管理資料。

使用 API 將現有系統連接至 {{site.data.keyword.messagehub}}。從系統建立針對 {{site.data.keyword.messagehub}} 的產生要求，包括指定訊息索引鍵、標頭，以及您想要將訊息寫入的主題。


## 使用 REST 產生訊息 
{: #rest_produce_messages}

使用生產者 API 將訊息寫入主題中。要能夠產生到主題中，您必須具有下列資訊：

* {{site.data.keyword.messagehub}} API 端點的 URL，包括埠號。
* 想要向其產生內容的主題。
* 用於提供連接至所選主題並向其產生內容的許可權的 API 金鑰。

您必須擷取所需的 URL 及認證詳細資料，才能從服務認證物件或服務實例的服務金鑰連接至 API。如需建立這些物件的相關資訊，請參閱[連接至 {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting)。

API 端點的 URL 提供於 <code>kafka_http_url</code> 內容。

使用下列其中一種方法進行鑑別：

* **使用基本鑑別進行鑑別：**<br/> 
    請使用上述物件的 <code>user</code> 和 <code>api_key</code> 內容作為使用者名稱和密碼。將這些值放入 HTTP 要求的 <code>Authorization</code> 標頭，格式為 <code>Basic <以單一冒號 (:) 結合的 username:password base64 編碼></code>。

* **使用持有人記號進行鑑別：**<br/> 
    若要使用 IBM Cloud CLI 取得您的記號，請先登入 IBM Cloud 然後執行下列指令： 

    ```
    ibmcloud iam oauth-tokens
    ```
    {: codeblock}

    將此記號放在 HTTP 要求的 Authorization 標頭，格式為 <code>Bearer <token></code>。同時支援 API 金鑰和 JWT 記號。 

* **直接使用 api_key 進行鑑別：**<br/> 
    將金鑰直接放置為 <code>X-Auth-Token</code> HTTP 標頭的值。

<br/>
下列程式碼顯示使用 curl 傳送訊息的範例：

```
curl -v -X POST -H "Authorization: Basic <base64 username:password>" -H "Content-Type: text/plain" -H "Accept: application/json" -d 'test message' "<kafka_http_url>/topics/<topic_name>/records"
```
{: codeblock}


## API 參考資料
{: #rest_api_reference}

有關 API 的完整詳細資料，請參閱 [{{site.data.keyword.messagehub}}REST 生產者 API 參考資料 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://ibm.github.io/event-streams/api/){:new_window}。












