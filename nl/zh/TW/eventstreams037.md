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

# 使用管理 REST API
{: #admin_api}

{{site.data.keyword.messagehub}} 提供了一個管理 RESTful API，可用於建立、刪除、列出和更新主題。
{: shortdesc}

您必須擷取所需的 URL 及認證詳細資料，才能從服務認證物件或服務實例的服務金鑰連接至 API。如需建立這些物件的相關資訊，請參閱[連接至 {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting)。

API 端點的 URL 提供於 <code>kafka_admin_url</code> 內容。

認證依賴鑑別方法，並且支援三種類型的認證：

* **使用基本鑑別進行鑑別**：<br/> 
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

對於經典方案上建立的服務實例，可以改為從應用程式的 VCAP_SERVICES 環境變數中取得此資訊。

有關 API 的說明及範例，請參閱 [{{site.data.keyword.messagehub}} admin-rest ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-messaging/event-streams-docs/tree/master/admin-rest-api){:new_window}。

您可以從 [{{site.data.keyword.messagehub}} 管理 REST API yaml 檔案 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-messaging/event-streams-docs/blob/master/admin-rest-api/admin-rest-api.yaml){:new_window} 下載 API 的完整規格。
若要檢視 swagger 檔案，請使用 Swagger 工具，例如 [Swagger editor ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://editor.swagger.io/#/){:new_window}。




