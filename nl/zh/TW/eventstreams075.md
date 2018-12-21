---

copyright:
  years: 2015, 2018
lastupdated: "2018-11-20"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 使用 MQ Light API 
{: #mql_using}

** MQ Light API 只提供於標準方案中。**
<br/>

提供 {{site.data.keyword.mql}} API 的原因是為了與較早期 {{site.data.keyword.mql}} 服務的舊版相容性。API 針對 Java、Node.js、Python 及 Ruby 提供了一個以 AMQP 為基礎的傳訊介面。
{:shortdesc}

<!-- 02/07/18 - removing words to help deprecate MQ Light
In most cases, {{site.data.keyword.messagehub}} is best used with a Kafka client. The {{site.data.keyword.mql}} API is simple to learn but has very limited scalability and does not offer interoperability with other {{site.data.keyword.messagehub}} APIs.
The {{site.data.keyword.mql}} API is available in the following {{site.data.keyword.Bluemix_short}} regions only: US South, United Kingdom, and Sydney. The {{site.data.keyword.mql}} API not available in the Germany region or in {{site.data.keyword.Bluemix_notm}} Dedicated.
-->

## 什麼是 MQ Light API，它有什麼不同？
{: #mqlight}

<!-- 30/10/18: info was in eventstreams106.md, moved because of doc app changes -->

{{site.data.keyword.mql}} API 提供了與 Kafka 相較之下更高的摘要層次。


使用 Kafka 用戶端或是 {{site.data.keyword.mql}} API 之間的選擇，取決於您想要建置的傳訊拓蹼：

* 使用 Kafka，您會使用少量的主題，且可以針對每個主題有多個分割區，獲得額外的可擴充性。您可以使用消費者群組在消費者之間共用訊息，但每個消費者必須能夠跟上指派給他的分割區訊息速率。
* 使用 {{site.data.keyword.mql}} API，您可以使用大量的主題，且主題名稱為階層式（例如：<code>&lsquo;/sports/football&rsquo;</code> 及 <code>&lsquo;/sports/tiddlywinks&rsquo;</code>）。  

{{site.data.keyword.mql}} API 中的主題與 Kafka 主題不同。相反地，{{site.data.keyword.mql}} API 會使用稱為 "MQLight" 的單一 Kafka 主題，使用 {{site.data.keyword.mql}} API 傳送和接收的所有訊息會通過那一個 Kafka 主題。

{{site.data.keyword.mql}} 只提供於下列 {{site.data.keyword.Bluemix_notm}} 位置（地區）：達拉斯 (us-south)、倫敦 (eu-gb) 及雪梨 (au-syd)。MQ Light API 無法用於法蘭克福 (eu-de) 位置或 {{site.data.keyword.Bluemix_notm}} Dedicated。

如需在 API 之間選擇的相關資訊，請參閱[在三個 API 之間抉擇](/docs/services/EventStreams/eventstreams087.html)。


## 使用 MQ Light API 搭配 {{site.data.keyword.messagehub}} 需要些什麼嗎？
{: #mql_reqs}

<!-- 30/10/18: info was in eventstreams098.md, moved because of doc app changes -->

使用 {{site.data.keyword.mql}} API 搭配 {{site.data.keyword.messagehub}} 時有下列需求： 

**您必須明確地建立名為 "MQLight" 的 Kafka 主題，然後才能使用 API，因為所有訊息都會通過 "MQLight" 主題。這個主題必須具有單一分割區。建立這個主題會為您的服務實例啟用 MQ Light API。MQ Light API 中使用的主題會自動在您使用它們的時候建立，但所有訊息實際上是在單一的 "MQLight" Kafka 主題上。** 

MQ Light API 使用 "MQLight" 主題來儲存其訊息資料，以及與其他 Kafka 用戶端互動。注意，建立這個主題之後，它會依服務付款方案中所概述的標準費率產生費用。

若要停用 MQ Light API，請刪除 "MQLight" 主題。請注意，刪除主題時會破壞所有資料。

<!-- 12/11/18: following info was in eventstreams079.md, moved because of doc app changes -->

## 如何連接及鑑別
{: #mql_connect}

若要將應用程式連接至服務，應用程式必須使用來自 [VCAP_SERVICES 環境變數](/docs/services/EventStreams/eventstreams127.html)的 <code>user</code>、
<code>password</code> 及 <code>mqlight_lookup_url</code> 詳細資料。請使用適用於您的選擇語言的下列指引：



**針對 Java**

如果您指定 &lsquo;null&rsquo; 作為 create() 呼叫的 endpointService 參數，這會指示用戶端從 VCAP_SERVICES 讀取 <code>user</code>、<code>password</code> 及 <code>mqlight_lookup_url</code> 詳細資料：


<pre>
<code>NonBlockingClient.create(null, new NonBlockingClientAdapter<Void>() {
    public void onStarted(NonBlockingClient client, Void context) {
        client.send("my/topic", "Hello World!", null);
    }
}, null);</code>
</pre>
{:codeblock}

<br>

**針對 Node.js**

從 VCAP_SERVICES 擷取 <code>user</code>、<code>password</code> 及
<code>mqlight_lookup_url</code> 詳細資料，然後使用它們建立用戶端，如下所示：



<pre>
<code>var services = JSON.parse(process.env.VCAP_SERVICES);
mqlightService = services['messagehub'][0];
opts.service = mqlightService.credentials.mqlight_lookup_url;
opts.user = mqlightService.credentials.user;
opts.password = mqlightService.credentials.password;
var mqlightClient = mqlight.createClient(opts, function(err) {
...</code>
</pre>
{:codeblock}

<br>

**針對 Ruby**

從 VCAP_SERVICES 擷取 <code>user</code>、<code>password</code> 及
<code>mqlight_lookup_url</code> 詳細資料，然後使用它們建立用戶端，如下所示：

<pre>
<code>vcap_services = JSON.parse(ENV['VCAP_SERVICES'])
conn_details = vcap_services['messagehub']
credentials = conn_details.first['credentials']
service = credentials['mqlight_lookup_url']
opts[:user] = credentials['user']
opts[:password] = credentials['password']
set :client, Mqlight::BlockingClient.new(service, opts)
...</code>
</pre>
{:codeblock}

<br>

**針對 Python**

從 VCAP_SERVICES 擷取 <code>user</code>、<code>password</code> 及
<code>mqlight_lookup_url</code> 詳細資料，然後使用它們建立用戶端，如下所示：

<pre>
<code>vcap_services = json.loads(os.environ.get('VCAP_SERVICES'))
conn_details = vcap_services['messagehub'][0]
service = str(conn_details['credentials']['mqlight_lookup_url'])
security_options = {
      'user': str(conn_details['credentials']['user']),
      'password': str(conn_details['credentials']['password'])
}
client = mqlight.Client(service=service, 
                        security_options=security_options,
                        on_started=on_started)</code>
</pre>
{:codeblock}

<br>

如需 {{site.data.keyword.mql}} API 的相關資訊，請參閱：[{{site.data.keyword.mql}} developerWorks&reg; 網站 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/messaging/mq-light/){:new_window}。


<!-- 14/11/18: info was in eventstreams080.md, moved because of doc app changes -->

## 如何連接現有的 MQ Light 應用程式
{: #mql_exist_apps}


您可以將目前針對 {{site.data.keyword.IBM_notm}} MQ 或 {{site.data.keyword.mql}}
{{site.data.keyword.Bluemix_notm}} 服務執行的現有應用程式連接至服務。應用程式會繼續以相同的方式運作。


若要連接現有的應用程式，請完成下列檢查：

* 確定應用程式使用您的語言的最新可用 {{site.data.keyword.mql}} API 用戶端版本。
* 確認從 VCAP_SERVICES 擷取的連線詳細資料參照 <code>messagehub</code> 服務類型，並從 <code>credentials.user</code> 內容而非 <code>credentials.username</code> 內容擷取連線使用者名稱、從 <code>credentials.mqlight_lookup_url</code> 內容而非 <code>credentials.connectionLookupURI</code> 內容擷取連線查閱 URL。如需相關資訊，請參閱 [VCAP_SERVICES 環境變數](/docs/services/EventStreams/eventstreams127.html)。

	請注意，如果您使用 Java&trade; 用戶端並在 create() 呼叫中指定 'null' 作為 endpointService 參數，則此步驟已為您完成，用戶端會自行擷取資訊。
	
* 您的應用程式必須支援 TLS 1.2 版連線。VCAP_SERVICES 不再包含 <code>credentials.nonTLSConnectionLookupURI</code> 內容以便建立非 TLS 的連線。

您也應該注意下列資訊：

* 訊息限制與 {{site.data.keyword.messagehub}} 一致，但可能與支援 {{site.data.keyword.mql}} API 的其他伺服器不同。如需相關資訊，請參閱[限制上限](/docs/services/EventStreams/eventstreams075.html#max_limits)。
* 不支援 JMS。

<!-- 15/11/18: info was in eventstreams081.md, moved because of doc app changes -->

## 在 MQ Light API 與 Kafka 或 Kafka REST API 之間交換訊息
{: #mql_exchange}

{{site.data.keyword.mql}} 訊息儲存在稱為 "MQLight" 的單一基礎 Kafka 主題，並詳細編碼於下表中。此編碼也可供其他 API 類型（例如 Kafka 或 Kafka REST）用來與使用
{{site.data.keyword.mql}} API 的應用程式交換訊息。

### Kafka 訊息格式
{: #kafka_format notoc}

<table border='1'>
<caption>表 1. Kafka 訊息格式</caption>
  <tr>
    <th> 金鑰</th>
    <th> 值</th>
  </tr>
  <tr>
    <td> 選用（不是由 API 使用）
	<p></p>
	</td>
    <td>**1 個位元組**
	<p>		     MQ Light API 醒目標示，一律為 0xFA。</p>
    <p><var class="keyword varname">**n**</var> **個位元組**</p>
    <p>		    AMQP 編碼訊息（根據 AMQP 發訊格式格式化）。</p></td>
  </tr>
</table>


<!-- 15/11/18: info was in eventstreams082.md, moved because of doc app changes -->

## MQ Light API 範例
{: #mql_samples}

如果您尚沒有應用程式，請用其中一個範例來試試 {{site.data.keyword.mql}} API。


範例應用程式實際上包含兩個簡單的應用程式：Web 前端（負責傳送訊息給後端），以及後端（負責處理訊息、將文字改為大寫，然後將訊息傳回前端）。此範例顯示如何使用 {{site.data.keyword.mql}} API 讓應用程式彼此對談。您也可以使用 {{site.data.keyword.mql}} API 執行工作者卸載，這是建立可擴充、鬆散耦合且分散式應用程式所需的重要功能。

範例程式碼位於 [event-streams-samples GitHub 專案 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-messaging/event-streams-samples/tree/master/mqlight){:new_window}。


<!-- 15/11/18: info was in eventstreams083.md, moved because of doc app changes -->

## 限制上限
{: #max_limits}

{{site.data.keyword.mql}} API 有下列強制限制：


* 可以儲存的資料量上限與您付款方案的單一 Kafka 分割區一致（通常是 1 GB）。如果已超出此資料限制，當傳送新的訊息時，會移除分割區中最舊的訊息。
* 可以儲存訊息的時間上限與您付款方案的單一 Kafka 分割區一致（通常是 24 小時）。您無法擷取早於此時段的訊息。
* 訊息（不含標頭）的大小上限是 1 MB。
* 同一時間內可連接的用戶端數目上限是 25。
* 同一時間內可在作用中的目的地數目上限是 25。作用中目的地的定義如下：
  - TimeToLive > 0 的目的地，並且目前具有或沒有連接用戶端。
  - TimeToLive = 0（預設值）的目的地，並且已連接用戶端。 
  <p>在用戶端之間共用的目的地會計算為單一目的地。</p>
