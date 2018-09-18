---

copyright:
  years: 2015, 2018
lastupdated: "2018-07-02"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 連接至 Message Hub
{: #connect_messagehub}

您連接至 {{site.data.keyword.messagehub}} 的方式會根據您使用標準方案還是企業方案而變，也會根據您是從 Cloud Foundry 應用程式還是任何其他外部用戶端連接而變。您需要收集兩項資訊，以便連接至任何 {{site.data.keyword.messagehub}} API：

* API 的端點 URL
* 鑑別用的認證

請閱讀下列資訊以了解如何取得這些詳細資料。步驟可能會微妙地改變，因此請確定您完成適合您實例的步驟。

## 佈建 {{site.data.keyword.messagehub}} 實例

作為必要條件，您必須先針對標準方案或企業方案佈建 {{site.data.keyword.messagehub}} 服務實例。佈建 {{site.data.keyword.messagehub}} 實例可能會產生費用。接下來，請完成下列作業以取得 {{site.data.keyword.messagehub}} API 連線詳細資料。

## 標準方案概觀
{: #connect_standard}

使用標準方案佈建的服務是 Cloud Foundry 服務。這表示它們被部署到 Cloud Foundry 組織及空間，且在儀表板中分組於標題 **Cloud Foundry 服務**下。您用來連接應用程式的方法取決於應用程式部署所在的位置，究竟是在 Cloud Foundry 內還是在它之外。


## 標準方案的 Cloud Foundry 應用程式
{: #connect_standard_cf}

對於在 Cloud Foundry 內執行的應用程式，請將您的應用程式連結至 {{site.data.keyword.messagehub}} 服務實例。連結之後，連線詳細資料會以 JSON 格式，在 VCAP_SERVICES 環境變數中提供給應用程式。您可以使用 IBM Cloud 主控台或 IBM Cloud CLI 連結應用程式及服務。

VCAP_SERVICES 的範例如下：

```
{
  "credentials": {
    "mqlight_lookup_url": "https://mqlight-lookup.messagehub.services.us-south.bluemix.net/Lookup?serviceId=584e8436-e7f5-43db-96ac-2864fccae5ae",
    "api_key": "d9JSx1SYsmLzNRbbgUFneDm2DtkedlVeViObYJIvrPAf2kJA",
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
{: #connect_standard_cf_console }

1. 確定您在想要的 Cloud Foundry 組織及空間中。
2. 在儀表板上找到 Cloud Foundry 應用程式。如果您尚無 Cloud Foundry 應用程式，可以藉由按一下**建立資源**按鈕來建立一個。
3. 按一下您的應用程式磚。
4. 按一下**連線**。
5. 按一下**建立連線**。
6. 選取您要連結至的 {{site.data.keyword.messagehub}} 服務磚，然後按一下**連接**。您可能需要重新編譯打包應用程式，變更才能生效。
7. 按一下左邊的**運行環境**標籤，然後選取中間的**環境變數**標籤。您現在可以驗證您的 VCAP_SERVICES 資訊，且應用程式現在可以用環境變數的方式存取它。 


### 使用 IBM Cloud CLI 取得認證 
{: #connect_standard_cf_cli }

<ol>
<li>確定您在想要的 Cloud Foundry 組織及空間中。您可以執行下列指令，以互動方式進行導覽：<br/>
<code>ibmcloud target -cf</code>
</li>
<li>找到您的應用程式：<br/> <code>ibmcloud app list</code> <br/>
</br>
如果您有資訊清單檔，可以藉由執行下列指令建立新的應用程式：</br>
<code>ibmcloud app push</code>
</li>
<li>找到您的服務：</br> 
<code>ibmcloud service list</code>
</li>
<li>將應用程式連結至服務：</br>
<code>ibmcloud service bind <var class="keyword varname">your_app_name</var> <var class="keyword varname">your_service_name</var></code>
</li>
<li>執行下列指令，驗證可以您的應用程式運行環境中使用 VCAP_SERVICES 環境變數：</br> 
 <code>ibmcloud app env <var class="keyword varname">your_app_name</var></code>
</li>
<li>將這些認證傳遞給您的應用程式。您可能需要重新編譯打包應用程式，變更才能生效。</li>
</ol>

## 標準方案的外部應用程式
{: #connect_standard_external}

對於在 Cloud Foundry 之外執行的應用程式，認證的產生方法是建立服務金鑰。取得服務金鑰之後，請使用您選擇的方法，手動將金鑰的詳細資料傳遞給您的應用程式。

### 使用 IBM Cloud 主控台取得認證
{: #connect_standard_external_console}

1. 確定您在想要的 Cloud Foundry 組織及空間中。
2. 在儀表板上找到 Cloud Foundry {{site.data.keyword.messagehub}} 服務。
3. 按一下您的服務磚。
4. 按一下**服務認證**。
5. 按一下**新建認證**。
6. 輸入新認證的詳細資料，例如名稱，然後按一下**新增**。新認證會出現在認證清單中。
7. 使用**檢視認證**按一下此認證，以顯示 JSON 格式的詳細資料。
8. 將這些認證傳遞給您的應用程式。

### 使用 IBM Cloud CLI 取得認證 
{: #connect_standard_external_cli }

<ol>
<li>確定您在想要的 Cloud Foundry 組織及空間中。您可以執行下列指令，以互動方式進行導覽：<br>
<code>ibmcloud target -cf</code>
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
這會以 JSON 格式傳回服務金鑰詳細資料。</li>
<li>將這些認證傳遞給您的應用程式。</li>
</ol>

 
## 企業方案概觀
{: #connect_enterprise}

使用企業方案佈建的服務會在在儀表板中分組於標題**服務**下。企業方案[已啟用 IAM 功能 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/docs/iam/quickstart.html#getstarted){:new_window}。您不需要瞭解 IAM 即可開始使用，但如果想要保護您的 {{site.data.keyword.messagehub}} 服務，建議需要具備一些知識。如需相關資訊，請參閱[保護您的 {{site.data.keyword.messagehub}} 資源](/docs/services/MessageHub/messagehub124.html)。

請完成下列步驟，以連結應用程式並取得服務的服務金鑰。若要獲得授權建立主題，您的應用程式或服務金鑰必須有管理員存取角色。

若要連接應用程式，使用的方法取決於應用程式部署所在的位置，究竟是在 Cloud Foundry 內還是在它之外。

## 企業方案的 Cloud Foundry 應用程式
{: #connect_enterprise_cf}

您的應用程式必須連結至 {{site.data.keyword.messagehub}} 服務實例。若要以 One Cloud 將 Cloud Foundry 應用程式連結至非 Cloud Foundry 服務，請先建立 Cloud Foundry 服務別名，然後在連結時從您的 Cloud Foundry 應用程式參照此別名。

連結之後，連線詳細資料便會以 JSON 格式，使用 VCAP_SERVICES 環境變數提供給應用程式。您可以使用 IBM Cloud 主控台或 IBM Cloud CLI 連結應用程式及服務。

### 使用 IBM Cloud 主控台連結應用程式
{: #connect_enterprise_cf_console}

1. 確定您在想要的 Cloud Foundry 組織及空間中。
2. 在儀表板上找到 Cloud Foundry 應用程式，或按一下**建立資源**按鈕來建立應用程式。
3. 按一下您的應用程式磚。
4. 按一下**連線**。
5. 按一下**建立連線**。
6. 選取您要連結至的 {{site.data.keyword.messagehub}} 服務磚，然後按一下**連接**。這會自動為您的 {{site.data.keyword.messagehub}} 服務建立 Cloud Foundry 服務別名，然後將您的應用程式連結至此別名。您可能需要重新編譯打包應用程式，變更才能生效。
8. 按一下左邊的**運行環境**標籤，然後選取中間的**環境變數**標籤。您現在可以驗證您的 VCAP_SERVICES 資訊。您的應用程式現在可以用環境變數的方式存取這些。 
 

### 使用 IBM Cloud CLI 連結應用程式
{: #connect_enterprise_cf_cli}

<ol>
<li>確定您在想要的 Cloud Foundry 組織及空間中。您可以執行下列指令，以互動方式進行導覽：<br/>
 <code>ibmcloud target -cf</code></li>
<li>找到您的應用程式：</br>
<code>ibmcloud app list</code><br/>
<br/>
如果您有資訊清單檔，可以藉由執行下列指令建立新的應用程式：<br/>
<code>ibmcloud app push</code><br/>
<br/>
由於應用程式尚未連結至 {{site.data.keyword.messagehub}}，所以應用程式無法建立連線。因此，建議您使用 <code>--no-start</code> 參數推送應用程式，以避免不必要的連線失敗。</li>
<li>找到您的服務：</br>
<code>ibmcloud resource service-instances</code></li>
<li>建立 Cloud Foundry 服務別名：<br/>
<code>ibmcloud resource service-alias-create <var class="keyword varname">alias_name</var> --instance-name <var class="keyword varname">your_service_name</var></code></li>
<li>將應用程式連結至先前建立的服務別名：<br/>
<code>ibmcloud service bind <var class="keyword varname">your_ app_name</var> <var class="keyword varname">alias_name</var></code><br/>
<br/>
或者，您可以更新您的資訊清單檔，然後再次推送應用程式。</li>
<li>驗證可以您的應用程式運行環境中使用 VCAP_SERVICES 環境變數：<br/>
<code>ibmcloud app env <var class="keyword varname">your_app_name</var></code></li>
<li>將這些認證傳遞給您的應用程式。<br/> 您可能需要重新編譯打包應用程式，變更才能生效。</li>
</ol>


## 企業方案的外部應用程式
{: #connect_enterprise_external}

對於在 Cloud Foundry 之外執行的應用程式，認證的產生方法是建立服務金鑰。取得服務金鑰之後，請使用您選擇的方法，手動將金鑰的詳細資料傳遞給您的應用程式。

### 使用 IBM Cloud 主控台取得認證
{: #connect_enterprise_external_console}

1. 在儀表板上找到 {{site.data.keyword.messagehub}} 服務。
2. 按一下您的服務磚。
3. 按一下**服務認證**。
4. 按一下**新建認證**。 
5. 填寫新認證的詳細資料，例如名稱及角色，然後按一下**新增**。新認證會出現在認證清單中。
6. 使用**檢視認證**按一下此認證，以顯示 JSON 格式的詳細資料。
7. 將這些認證傳遞給您的應用程式。請確定您的應用程式會剖析詳細資料。

### 使用 IBM Cloud CLI 取得認證
{: #connect_enterprise_external_cli}

<ol>
<li>找到您的服務：<br/>
<code>ibmcloud resource service-instances</code></li>
<li>建立服務金鑰：<br/>
<code>ibmcloud resource service-key-create <var class="keyword varname">key_name</var> <var class="keyword varname">key_role</var> --instance-name <var class="keyword varname">your_service_name</var></code></li>
<li>列印服務金鑰：<br/>
<code>ibmcloud resource service-key <var class="keyword varname">key_name</var></code></li>
<li>將這些認證傳遞給您的應用程式。請確定您的應用程式會剖析詳細資料。</li>
</ol>

## 下一步
{: #after_connecting}

現在您已有連線及認證資訊，您可以選擇 {{site.data.keyword.messagehub}} 用戶端。您的選項取決於您的方案。

* 如果使用標準方案，請參閱[在三個 API 之間抉擇](/docs/services/MessageHub/messagehub087.html)，以取得要選擇哪個用戶端以及如何連接的相關資訊。
* 如果使用企業方案，請參閱[使用 Kafka API](/docs/services/MessageHub/messagehub050.html)。

	如果您使用企業方案，內部 Kafka <code>__consumer_offsets</code> 主題可供您以唯讀方式查看。強烈建議您不要試圖以任何方式管理主題。 

<!--
Charlie said:

"Add some info describing how to take the information made available from above e.g. like the info in the Connecting a client to the Kafka API section of the alpha docs on stage 1? https://console.stage1.bluemix.net/docs/services/MessageHub/messagehub122.html#alpha_about "
-->







 















