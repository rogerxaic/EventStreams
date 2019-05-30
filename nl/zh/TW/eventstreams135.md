---

copyright:
  years: 2015, 2019
lastupdated: "2018-11-07"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 使用經典方案連接 {{site.data.keyword.messagehub}} 
{: #connecting_classic}

連接至 {{site.data.keyword.messagehub}} 的方式會有所不同，取決於您是從 Cloud Foundry 應用程式還是任何其他外部用戶端進行連接。您需要收集兩項資訊，以便連接至任何 {{site.data.keyword.messagehub}} API：
{: shortdesc}

* API 的端點 URL
* 鑑別用的認證

請閱讀下列資訊以了解如何取得這些詳細資料。步驟可能會微妙地改變，因此請確定您完成適合您實例的步驟。

## 佈建 {{site.data.keyword.messagehub}} 實例

作為先決條件，對於經典方案，都必須先佈建 {{site.data.keyword.messagehub}} 服務實例。接下來，請完成下列作業以取得 {{site.data.keyword.messagehub}} API 連線詳細資料。

## 經典方案概觀
{: #connect_classic_plan}

使用經典方案佈建的服務為 Cloud Foundry 服務。這表示它們被部署到 Cloud Foundry 組織及空間，且在儀表板中分組於標題 **Cloud Foundry 服務**下。您用來連接應用程式的方法取決於應用程式部署所在的位置，究竟是在 Cloud Foundry 內還是部署在 Cloud Foundry 之外，例如在 Kubernetes 服務中。


## 經典方案上的 Cloud Foundry 應用程式
{: #connect_classic_cf_plan}

對於在 Cloud Foundry 內執行的應用程式，請將您的應用程式連結至 {{site.data.keyword.messagehub}} 服務實例。連結之後，連線詳細資料會以 JSON 格式，在 VCAP_SERVICES 環境變數中提供給應用程式。您可以使用 IBM Cloud 主控台或 IBM Cloud CLI 連結應用程式及服務。

VCAP_SERVICES 的範例如下：

```
{
  "credentials": {
    "mqlight_lookup_url": "https://mqlight-lookup.messagehub.services.us-south.bluemix.net/Lookup?serviceId=584e8436-e7f5-43db-96ac-2864fccae5ae",
    "api_key": <YOUR_API_KEY>,
    "kafka_admin_url": "https://kafka-admin.messagehub.services.us-south.bluemix.net:443",
    "kafka_rest_url": "https://kafka-rest.messagehub.services.us-south.bluemix.net:443",
    "kafka_brokers_sasl": [
      "kafka01.messagehub.services.us-south.bluemix.net:9093",
      "kafka02.messagehub.services.us-south.bluemix.net:9093",
      "kafka03.messagehub.services.us-south.bluemix.net:9093",
      "kafka04.messagehub.services.us-south.bluemix.net:9093",
      "kafka05.messagehub.services.us-south.bluemix.net:9093"
    ],
    "user": "d9JSx1SYsmLzNRbb",
    "password": "gUFneDm2DtkedlVeViObYJIvrPAf2kJA"
  }
}
```

{: codeblock}

不論您用來連接 {{site.data.keyword.messagehub}} 的 API 為何，環境變數的內容都相同。您的 {{site.data.keyword.Bluemix_notm}} 應用程式會視使用的介面而定，從 VCAP_SERVICES 環境變數中選取適當的認證。
 
只有前五個分配管理系統會列在 VCAP_SERVICES 中。如果您有超過五個分配管理系統，請使用 Kafka 用戶端來擷取其他分配管理系統的詳細資料。 


### 使用 IBM Cloud 主控台取得認證並連接
{: #connect_classic_plan_cf_console }

1. 確定您在想要的 Cloud Foundry 組織及空間中。
2. 在儀表板上找到 Cloud Foundry 應用程式。如果您尚無 Cloud Foundry 應用程式，可以藉由按一下**建立資源**按鈕來建立一個。
3. 按一下您的應用程式磚。
4. 按一下**連線**。
5. 按一下**建立連線**。
6. 選取您要連結至的 {{site.data.keyword.messagehub}} 服務磚，然後按一下**連接**。您可能需要重新編譯打包應用程式，變更才能生效。
7. 按一下左邊的**運行環境**標籤，然後選取中間的**環境變數**標籤。您現在可以驗證您的 VCAP_SERVICES 資訊，且應用程式現在可以用環境變數的方式存取它。 


### 使用 IBM Cloud CLI 取得認證 
{: #connect_classic_plan_cf_cli }

<ol>
<li>確定您在想要的 Cloud Foundry 組織及空間中。您可以執行下列指令，以互動方式進行導覽：<br/>
<code>ibmcloud target --cf</code>
</li>
<li>找到您的應用程式：<br/> <code>ibmcloud app list</code> <br/>
</br>
如果您有資訊清單檔，可以藉由執行下列指令建立新的應用程式：</br>
<code>ibmcloud app push</code>
</li>
<li>尋找您的服務：</br>
<code>ibmcloud service list</code>
</li>
<li>將您的應用程序連結到服務：</br>
<code>ibmcloud service bind <var class="keyword varname">your_app_name</var> <var class="keyword varname">your_service_name</var></code>
</li>
<li>透過執行以下命令驗證 VCAP_SERVICES 環境變數在應用程式執行時期中是否可用：</br>
 <code>ibmcloud app env <var class="keyword varname">your_app_name</var></code>。
</li>
<li>將這些認證傳遞給您的應用程式。指定 <code>token</code> 作為您的使用者名稱、<var class="keyword varname">api_key</var> 作為您的密碼。請以冒號區隔 <code>token</code> 及 <var class="keyword varname">api_key</var>。如需相關資訊，請參閱[配置用戶端](/docs/services/EventStreams?topic=eventstreams-kafka_connect)。
<p>您可能需要重新編譯打包應用程式，變更才能生效。</p></li>
</ol>

## 外部應用程式（經典方案）
{: #connect_classic_plan_external}

對於在 Cloud Foundry 之外執行的應用程式，認證的產生方法是建立服務金鑰。取得服務金鑰之後，請使用您選擇的方法，手動將金鑰的詳細資料傳遞給您的應用程式。

### 使用 IBM Cloud 主控台取得認證
{: #connect_classic_plan_external_console}

1. 確定您在想要的 Cloud Foundry 組織及空間中。
2. 在儀表板上找到 Cloud Foundry {{site.data.keyword.messagehub}} 服務。
3. 按一下您的服務磚。
4. 按一下**服務認證**。
5. 按一下**新建認證**。
6. 輸入新認證的詳細資料，例如名稱，然後按一下**新增**。新認證會出現在認證清單中。
7. 使用**檢視認證**按一下此認證，以顯示 JSON 格式的詳細資料。
8. 將這些認證傳遞給您的應用程式。指定 <code>token</code> 作為您的使用者名稱、<var class="keyword varname">api_key</var> 作為您的密碼。請以冒號區隔 <code>token</code> 及 <var class="keyword varname">api_key</var>。如需相關資訊，請參閱[配置用戶端](/docs/services/EventStreams?topic=eventstreams-kafka_connect)。

### 使用 IBM Cloud CLI 取得認證 
{: #connect_classic_plan_external_cli }

<ol>
<li>確定您在想要的 Cloud Foundry 組織及空間中。您可以執行下列指令，以互動方式進行導覽：<br>
<code>ibmcloud target --cf</code>
</li>
<li>找到您的服務：<br>
<code>ibmcloud service list</code>
</li>
<li>您可以建立服務金鑰：<br>
<code>ibmcloud service key-create <var class="keyword varname">your_service_name</var> <var class="keyword varname">new_service_key_name</var></code><br>
<br/>
或使用現有的服務金鑰：<br/>
<code>ibmcloud service keys <var class="keyword varname">your_service_name</var></code> 
</li>
<li>取得金鑰的詳細資料：</br>
<code>ibmcloud service key-show <var class="keyword varname">your_service_name</var> <var class="keyword varname">service _key_name</var></code></br>
這將傳回 JSON 格式的服務金鑰詳細資料。</li>
<li>將這些認證傳遞給您的應用程式。指定 <code>token</code> 作為您的使用者名稱、<var class="keyword varname">api_key</var> 作為您的密碼。請以冒號區隔 <code>token</code> 及 <var class="keyword varname">api_key</var>。如需相關資訊，請參閱[配置用戶端](/docs/services/EventStreams?topic=eventstreams-kafka_connect)。</li>
</ol>
 
## 下一步
{: #after_connecting_next}

現在您已有連線及認證資訊，您可以選擇 {{site.data.keyword.messagehub}} 用戶端。請參閱[在三個 API 中進行選擇](/docs/services/EventStreams?topic=eventstreams-choose_api_classic)，以取得有關選擇哪個用戶端以及如何連接的資訊。










 















