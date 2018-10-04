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


<!-- Comment from Andrew
basic introduction, definitely including health warning
-->

