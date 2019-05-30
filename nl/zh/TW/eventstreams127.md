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


# 連接至 {{site.data.keyword.messagehub}}
{: #connecting}

連接至 {{site.data.keyword.messagehub}} 的方式會有所不同，取決於您的應用程式是以原本方式執行還是作為 Cloud Foundry 應用程式執行。但是，在兩種情況下，都需要以下兩項資訊：
{: shortdesc}

* API 的端點 URL
* 鑑別用的認證

請閱讀下列資訊以了解如何取得這些詳細資料。步驟可能會微妙地改變，因此請確定您完成適合您實例的步驟。

如需如何連接 {{site.data.keyword.messagehub}} 的資訊（如果使用經典方案），請參閱[使用經典方案連接](/docs/services/EventStreams?topic=eventstreams-connecting_classic)。


## 概觀
{: #connect_enterprise}

使用標準方案或企業方案佈建的服務將分組到儀表板中標題**服務**下。方案是[已啟用 IAM ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/iam?topic=iam-getstarted#getstarted){:new_window}。您不需要瞭解 IAM 即可開始使用，但如果想要保護您的 {{site.data.keyword.messagehub}} 服務，建議需要具備一些知識。如需相關資訊，請參閱[管理對 {{site.data.keyword.messagehub}} 資源的存取](/docs/services/EventStreams?topic=eventstreams-security)。

請完成下列步驟，以連結應用程式並取得服務的服務金鑰。若要獲得授權建立主題，您的應用程式或服務金鑰必須有管理員存取角色。

若要連接應用程式，使用的方法取決於應用程式部署所在的位置，究竟是在 Cloud Foundry 內還是部署在 Cloud Foundry 之外，例如在 Kubernetes 服務中。

## 佈建 {{site.data.keyword.messagehub}} 實例

作為必要條件，您必須先針對標準方案或企業方案佈建 {{site.data.keyword.messagehub}} 服務實例。接下來，請完成下列作業以取得 {{site.data.keyword.messagehub}} API 連線詳細資料。

## 連接應用程式 
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
7. 將這些認證傳遞給您的應用程式。指定 <code>token</code> 作為您的使用者名稱、<var class="keyword varname">api_key</var> 作為您的密碼。請以冒號區隔 <code>token</code> 及 <var class="keyword varname">api_key</var>。如需相關資訊，請參閱[配置 Kafka API 用戶端](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_api_client)。
   <br/><br/>請確定您的應用程式會剖析詳細資料。

### 使用 IBM Cloud CLI 取得認證
{: #connect_enterprise_external_cli}

<ol>
<li>找到您的服務：<br/>
<code>ibmcloud resource service-instances</code></li>
<li>建立服務金鑰：<br/>
<code>ibmcloud resource service-key-create <var class="keyword varname">key_name</var> <var class="keyword varname">key_role</var> --instance-name <var class="keyword varname">your_service_name</var></code></li>
<li>列印服務金鑰：<br/>
<code>ibmcloud resource service-key <var class="keyword varname">key_name</var></code></li>
<li>將這些認證傳遞給您的應用程式。指定 <code>token</code> 作為您的使用者名稱、<var class="keyword varname">api_key</var> 作為您的密碼。請以冒號區隔 <code>token</code> 及 <var class="keyword varname">api_key</var>。如需相關資訊，請參閱[配置用戶端](/docs/services/EventStreams?topic=eventstreams-kafka_connect)。
<p>請確定您的應用程式會剖析詳細資料。</p></li>
</ol>

## 連接 Cloud Foundry 應用程式
{: #connect_enterprise_cf}

您的應用程式必須連結至 {{site.data.keyword.messagehub}} 服務實例。若要將 Cloud Foundry 應用程式連結至非 Cloud Foundry 服務，請先建立 Cloud Foundry 服務別名，然後在連結時從您的 Cloud Foundry 應用程式參照此別名。 

連結之後，連線詳細資料便會以 JSON 格式，使用 VCAP_SERVICES 環境變數提供給應用程式。您可以使用 [IBM Cloud 主控台
](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_cf_console)或 [IBM Cloud CLI](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_cf_cli) 連結應用程式和服務。

### 使用 IBM Cloud 主控台連結應用程式
{: #connect_enterprise_cf_console}

1. 確定您在想要的 Cloud Foundry 組織及空間中。
2. 在儀表板上找到 Cloud Foundry 應用程式，或按一下**建立資源**按鈕來建立應用程式。
3. 按一下您的應用程式磚。
4. 按一下**連線**。
5. 按一下**建立連線**。
6. 選取您要連結至的 {{site.data.keyword.messagehub}} 服務磚，然後按一下**連接**。 
7. 在出現的**連接啟用 IAM 功能的服務**視窗中，從**連線的存取角色**中選取存取角色，並從**連線的服務 ID** 清單中選取服務 ID（您可以接受自動產生的 ID）。按一下**連接**。 

  這會為您的 {{site.data.keyword.messagehub}} 服務建立 Cloud Foundry 服務別名，然後將您的應用程式連結至此別名。 

  重新編譯打包應用程式，變更才能生效。<br/>
8. 按一下左邊的**運行環境**標籤，然後選取中間的**環境變數**標籤。您現在可以驗證您的 VCAP_SERVICES 資訊。您的應用程式現在可以用環境變數的方式存取這些資訊。 
 

### 使用 IBM Cloud CLI 連結應用程式
{: #connect_enterprise_cf_cli}

<ol>
<li>確定您在想要的 Cloud Foundry 組織及空間中。您可以執行下列指令，以互動方式進行導覽：<br/>
 <code>ibmcloud target --cf</code></li>
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
<li>將這些認證傳遞給您的應用程式。指定 <code>token</code> 作為您的使用者名稱、<var class="keyword varname">api_key</var> 作為您的密碼。請以冒號區隔 <code>token</code> 及 <var class="keyword varname">api_key</var>。如需相關資訊，請參閱[配置用戶端](/docs/services/EventStreams?topic=eventstreams-kafka_connect)。
<p>您可能需要重新編譯打包應用程式，變更才能生效。</p></li>
</ol>


## 下一步
{: #after_connecting}

現在您已有連線及認證資訊，您可以選擇 {{site.data.keyword.messagehub}} 用戶端。如需相關資訊，請參閱[使用 Kafka API](/docs/services/EventStreams?topic=eventstreams-kafka_using)。

<!--
Charlie said:

"Add some info describing how to take the information made available from above e.g. like the info in the Connecting a client to the Kafka API section of the alpha docs on stage 1? https://console.stage1.bluemix.net/docs/services/EventStreams/eventstreams122.html#alpha_about "
-->







 















