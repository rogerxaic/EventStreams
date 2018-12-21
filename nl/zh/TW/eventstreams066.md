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

# 如何連接及鑑別
{: #rest_connect}

<!-- info moved to eventstreams025.md because of doc app changes -->
** Kafka REST API 只提供於標準方案中。**
<br/>

為了連接至 {{site.data.keyword.messagehub}}，Kafka REST API 會使用來自 [VCAP_SERVICES 環境變數](/docs/services/EventStreams/eventstreams127.html)的 <code>api_key</code> 及 <code>kafka_rest_url</code> 認證。

若要向 {{site.data.keyword.messagehub}} Kafka REST API 進行鑑別，您必須在要求的 X-Auth-Token 標頭中指定 <code>api_key</code>。
