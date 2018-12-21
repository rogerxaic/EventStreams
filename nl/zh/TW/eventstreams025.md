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

# 使用 Kafka REST API
{: #rest_using}

** Kafka REST API 只提供於標準方案中。**
<br/>

Kafka REST API 提供 Kafka 叢集的 RESTful 介面。您可以使用 API 生產和使用訊息。如需包括 API 參考資料文件的相關資訊，請參閱 [Kafka REST Proxy 文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.confluent.io/2.0.0/kafka-rest/docs/index.html){:new_window}。{{site.data.keyword.messagehub}} 中的要求及回應只支援二進位內嵌格式。不支援 Avro 及 JSON 內嵌格式。

如果您使用 CURL，可以使用類似如下的範例來生產：
<pre class="pre"><code>
curl -X POST -H "X-Auth-Token:<var class="keyword varname">APIKEY</var>" -H "Content-Type: application/vnd.kafka.binary.v1+json" <var class="keyword varname">kafka-rest endpoint</var>/topics/<var class="keyword varname">topic name</var> -d 
 

'
{
  "records": [
    {
      "value": "<var class="keyword varname">A base 64 encoded value string</var>"
    }
  ]
}
'
</code></pre>
{: codeblock}
<br/>
<br/>
如果您使用 CURL，可以使用類似如下的範例來消費：<pre class="pre"><code>
curl -X GET -H "X-Auth-Token:<var class="keyword varname">APIKEY</var>" -H "Accept: application/vnd.kafka.binary.v1+json" <var class="keyword varname">kafka-rest endpoint</var>/topics/<var class="keyword varname">topic name</var>/partitions/<var class="keyword varname">partition ID</var>/messages?offset=<var class="keyword varname">offset to start from</var>
</code></pre>
{: codeblock}

<br/>
<br/>
針對 CURL，您也可以新增下面這一行到指令行，以調整 [Confluent 文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://docs.confluent.io/2.0.0/){:new_window} 中詳述的程式碼範例：
<pre class="pre">-H "X-Auth-Token: <var class="keyword varname">APIKEY</var>"</pre>
{: codeblock}

<br/>
## 如何連接及鑑別
{: #rest_connect}

<!-- info was in eventstreams066.md -->

<!-- Comment from Andrew
basic introduction, definitely including health warning
-->
為了連接至 {{site.data.keyword.messagehub}}，Kafka REST API 會使用來自 [VCAP_SERVICES 環境變數](/docs/services/EventStreams/eventstreams127.html)的 <code>api_key</code> 及 <code>kafka_rest_url</code> 認證。

若要向 {{site.data.keyword.messagehub}} Kafka REST API 進行鑑別，您必須在要求的 X-Auth-Token 標頭中指定 <code>api_key</code>。


## 如何使用 API
{: #rest_how}

<!-- info was in eventstreams097.md -->

{{site.data.keyword.messagehub}} Kafka REST API 範例是一個 Node.js 應用程式，它會透過 Kafka REST API 連接至 {{site.data.keyword.messagehub}}，以生產及取用訊息。

範例程式碼位於 [event-streams-samples GitHub 專案 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample){:new_window}。

請遵循該專案 [README.md ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample){:new_window} 檔案中的指示，來建置及執行範例。


