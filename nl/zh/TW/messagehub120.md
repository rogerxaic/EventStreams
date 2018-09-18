---

copyright:
  years: 2015, 2018
lastupdated: "2018-04-12"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 開始使用 Alpha 計畫
{: #alpha_program}

Alpha 計畫讓您能提早存取下一版的 {{site.data.keyword.messagehub}} 服務。 

請執行下列步驟，以讓應用程式搭配 {{site.data.keyword.messagehub}} Alpha 執行：


## 建立 {{site.data.keyword.messagehub}} 服務
{: alpha_create}


  1. 按一下 **Message Hub vNext - Production** 磚，這是[型錄 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.stage1.bluemix.net/catalog/labs/?search=vnext) 中的一項實驗性服務。</li>

  2. 建立 {{site.data.keyword.messagehub}} 服務。此服務會依 <code>Premium</code> 定價方案提供單一承租戶叢集，且通常需要 1-3 小時進行佈建。
 


## 使用主控台選項取得認證：建立並連接測試應用程式
{: alpha_app}

您需要認證才能使用 {{site.data.keyword.messagehub}}。如果您尚沒有可用的應用程式，請建立測試應用程式，然後使用該應用程式用的認證來連接 {{site.data.keyword.messagehub}}。例如，請針對測試應用程式使用 **SDK for Node.js** 服務。 

  1. 在 [型錄 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.stage1.bluemix.net/catalog/starters/sdk-for-nodejs) 中導覽至 **SDK for Node.js** 磚。
   
  輸入應用程式名稱之前，請確定您選取美國南部地區。建立應用程式。

  2. 應用程式執行中時，按一下左邊的**連線**標籤。

  3. 按一下**建立連線**按鈕。

  4. 從現有相容服務的清單，選取您的新 {{site.data.keyword.messagehub}} 服務，然後按一下**連接**按鈕。

  5. 在**連接已啟用 IAM 的服務**視窗中，接受預設值然後按一下**連接**。請確定您的 {{site.data.keyword.messagehub}} 服務已佈建，以便連接它。

  6. 按一下左邊的**運行環境**標籤，然後選取中間的**環境變數**標籤。在 **VCAP_SERVICES** 區段中，找到 <code>kafka_admin_url</code>、<code>apikey</code> 及 <code>kafka_brokers_sasl</code> 資訊，您需要這些資訊才能夠傳送訊息。
  
## 使用指令行選項取得認證
或者，您可以使用指令行取得必要的認證。請完成下列步驟：

  1. 從 [{{site.data.keyword.Bluemix_notm}} CLI ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/cli/index.html#overview) 安裝 {{site.data.keyword.Bluemix_notm}} 指令行工具。
  
  2. 登入 {{site.data.keyword.Bluemix_notm}} CLI。
  
  3. 使用 {{site.data.keyword.Bluemix_notm}} CLI，用下列指令以「管理員」角色建立服務金鑰：
  ```
  bx resource service-key-create <NAME> Manager --instance-name <MESSAGEHUB_SERVICE_INSTANCE_NAME>
  ```
  4. 輸出會包含 API 金鑰、管理 REST 端點 URL，以及分配管理系統清單。您可以執行下列指令，再次檢視此資訊：
  ```
  bx resource service-key <NAME>
  ```

## 建立 {{site.data.keyword.messagehub}} 主題並傳送訊息

您可以使用 CURL 指令建立主題，然後使用 kafkacat 工具產生及耗用訊息。 

針對每個指令，將 APIKEY 和 KAFKA_ADMIN_URL 取代為來自 VCAP_SERVICES 環境變數的值。

  1. 從指令行，使用下列 CURL 指令建立 {{site.data.keyword.messagehub}} 主題：
  
    ```
    curl -i -X POST -H "Content-Type: application/json" -H "X-Auth-Token: APIKEY" --data '{ "name": "newtop"}' KAFKA_ADMIN_URL/admin/topics
    ```
    {: codeblock}

  2. 安裝 [kafkacat 工具 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/edenhill/kafkacat#install)，它適用於快速測試 Kafka。
  
  3. 若要執行下一個指令，您需要下列資訊：
  
    * 您的分配管理系統清單，這是在您的認證 `kafka_brokers_sasl` 中傳回。您的分配管理系統清單必須是以逗點區隔的 kafkacat 清單。只有前五個分配管理系統會列在 VCAP_SERVICES 中。如果您有超過五個分配管理系統，請使用 Kafka 用戶端來擷取其他分配管理系統的詳細資料。 
  
    * 您的 <code>apikey</code>。前 8 個字元會形成您的 sasl.username，而 <code>apikey</code> 的剩餘部分，便形成您的 sasl.password。
    * 您的 SSL 憑證位置。例如，在 Ubuntu 上，SSL_CERTS_DIRECTORY 是 <code>/etc/ssl/certs/</code>
  
  4. 執行如下的指令，產生一些訊息：
  ```
  kafkacat -X "security.protocol=sasl_ssl" -X 'sasl.mechanisms=PLAIN' -X 'sasl.username=<FIRST_8_CHARS_FROM_APIKEY>' -X 'sasl.password=<REMAINING_CHARS_FROM_APIKEY>' -X "ssl.ca.location=<SSL_CERTS_DIRECTORY>" -b <BROKERS_LIST> -P -t <TOPIC_NAME>
    ```
		
	<br/>
  執行指令之後，您可以在生產者終端機輸入一些文字，例如 <code>HelloWorld</code>。
  
  5. 執行如下的指令，耗用訊息：
  ```
  kafkacat -X "security.protocol=sasl_ssl" -X 'sasl.mechanisms=PLAIN' -X 'sasl.username=<FIRST_8_CHARS_FROM_APIKEY>' -X 'sasl.password=<REMAINING_CHARS_FROM_APIKEY>' -X "ssl.ca.location=<SSL_CERTS_DIRECTORY>" -b <BROKERS_LIST> -C -t <TOPIC_NAME> -f 'Topic %t [%p] at offset %o: key %k: %s\n'
  ```
	
	<br/>
  您應該在消費者終端機中看到 <code>HelloWorld</code>。

