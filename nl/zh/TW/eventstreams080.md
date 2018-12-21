---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

<!-- 14/11/18: info moved to eventstreams075.md, moved because of doc app changes -->
# 如何連接現有的 MQ Light 應用程式
{: #mql_exist_apps}

** MQ Light API 只提供於標準方案中。**
<br/>

您可以將目前針對 {{site.data.keyword.IBM_notm}} MQ 或 {{site.data.keyword.mql}}
{{site.data.keyword.Bluemix_notm}} 服務執行的現有應用程式連接至服務。應用程式會繼續以相同的方式運作。
{:shortdesc}

若要連接現有的應用程式，請完成下列檢查：

* 確定應用程式使用您的語言的最新可用 {{site.data.keyword.mql}} API 用戶端版本。
* 確認從 VCAP_SERVICES 擷取的連線詳細資料參照 <code>messagehub</code> 服務類型，並從 <code>credentials.user</code> 內容而非 <code>credentials.username</code> 內容擷取連線使用者名稱、從 <code>credentials.mqlight_lookup_url</code> 內容而非 <code>credentials.connectionLookupURI</code> 內容擷取連線查閱 URL。如需相關資訊，請參閱 [VCAP_SERVICES 環境變數](/docs/services/EventStreams/eventstreams127.html)。

	請注意，如果您使用 Java&trade; 用戶端並在 create() 呼叫中指定 'null' 作為 endpointService 參數，則此步驟已為您完成，用戶端會自行擷取資訊。
	
* 您的應用程式必須支援 TLS 1.2 版連線。VCAP_SERVICES 不再包含 <code>credentials.nonTLSConnectionLookupURI</code> 內容以便建立非 TLS 的連線。

您也應該注意下列資訊：

* 訊息限制與 {{site.data.keyword.messagehub}} 一致，但可能與支援 {{site.data.keyword.mql}} API 的其他伺服器不同。如需相關資訊，請參閱[限制上限](/docs/services/EventStreams/eventstreams083.html)。
* 不支援 JMS。
