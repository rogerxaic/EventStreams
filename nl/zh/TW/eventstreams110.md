---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-06"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 使用 KSQL 搭配 {{site.data.keyword.messagehub}}
{: #ksql_using}

您可以使用 [KSQL ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/confluentinc/ksql){:new_window} 搭配 {{site.data.keyword.messagehub}} 以進行串流處理。請確定您使用 KSQL 0.4 或更新版本。 

請完成下列步驟：

1. 建立 {{site.data.keyword.messagehub}} KSQL 配置檔。

    例如：
    ```
        bootstrap.servers=BOOTSTRAP_SERVERS
    application.id=ksql_server_quickstart
    ksql.command.topic.suffix=commands
    listeners=http://localhost:8080
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    ssl.protocol=TLSv1.2
    ssl.enabled.protocols=TLSv1.2
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USERNAME" password="PASSWORD";
    ksql.sink.replications.default=3
    ```
    其中 BOOTSTRAP_SERVERS、USERNAME 及 PASSWORD 是來自 {{site.data.keyword.Bluemix_notm}} 中 {{site.data.keyword.messagehub}} **服務認證**標籤的值。

2. 使用 {{site.data.keyword.Bluemix_notm}} 主控台中的 {{site.data.keyword.messagehub}} 儀表板建立稱為 <code>ksql__commands</code> 的主題，它具有單一分割區和預設保留期間。
3. 從 Docker 終端機，使用下列指令啟動 KSQL：
<pre class="pre">/bin/ksql-cli local 
--<var class="keyword varname">properties-file</var> ./config/ksqlserver.properties
</pre>
4. 使用 {{site.data.keyword.Bluemix_notm}} 主控台中的 {{site.data.keyword.messagehub}} 儀表板建立兩個主題，並各有一個分割區：<code>users</code> 及 <code>pageviews</code>。

    然後啟動 <code>DataGen</code> 兩次，如下所示：
	
    i. 使用 <code>bootstrap-server=HOSTNAME:PORTNUMBER quickstart=users format=json topic=users maxInterval=10000</code> 開始建立 <code>users</code> 事件。
	
    ii. 使用 <code>bootstrap-server=HOSTNAME:PORTNUMBER quickstart=pageviews format=delimited topic=pageviews maxInterval=10000</code> 開始建立 <code>pageviews</code> 事件。
	
	確定您將**服務認證**頁面中列出的所有 Kafka 主機，插入為 <code>bootstrap-server</code> 的值。若要找到此資訊，請移至 {{site.data.keyword.Bluemix_notm}} 中的 {{site.data.keyword.messagehub}} 實例、移至**服務認證**標籤，然後選取您想要使用的**認證**。

當您完成這些步驟之後，可以執行 [KSQL Quick Start Guide ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/confluentinc/ksql/tree/0.1.x/docs/quickstart#create-a-stream-and-table){:new_window} 列出的所有查詢

