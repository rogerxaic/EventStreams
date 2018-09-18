---

copyright:
  years: 2015, 2018
lastupdated: "2018-06-01"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Object Storage 橋接器 
{: #object_storage_bridge }

** {{site.data.keyword.messagehub}} 橋接器只提供於標準方案中。**
<br/>

{{site.data.keyword.objectstorageshort}} 橋接器容許您在 {{site.data.keyword.messagehub}} 中將
Kafka 主題的資料保存到 {{site.data.keyword.Bluemix_short}} [{{site.data.keyword.objectstorageshort}} 服務 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/ObjectStorage/index.html){:new_window} 的實例。橋接器會從
Kafka 取用訊息批次，然後將訊息資料當作物件上傳到 {{site.data.keyword.objectstorageshort}} 服務中的容器。

請注意，現在 {{site.data.keyword.Bluemix_short}} 的偏好物件儲存空間服務是 [{{site.data.keyword.IBM_notm}} Cloud Object Storage 服務。![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/cloud-object-storage/about-cos.html){:new_window}。

藉由配置
{{site.data.keyword.objectstorageshort}} 橋接器，您可以控制資料如何當作物件上傳到 {{site.data.keyword.objectstorageshort}}。例如，您可以配置的內容如下：

* 將物件寫入的容器名稱。
* 物件上傳至 {{site.data.keyword.objectstorageshort}} 服務的頻率。
* 在將物件上傳至 {{site.data.keyword.objectstorageshort}} 服務之前，寫入多少資料到每個物件。

橋接器的輸出格式是一個 Object Storage 服務物件，其中包含一個以上以換行字元作為分隔字元連結起來的記錄。

## 使用 {{site.data.keyword.objectstorageshort}} 橋接器傳送資料的方式
{: notoc}

{{site.data.keyword.objectstorageshort}} 橋接器在運作時會從主題讀取一些
Kafka 記錄，然後將這些記錄的資料寫入物件。此物件會上傳至 {{site.data.keyword.objectstorageshort}} 服務的實例。雖然一個主題可以有多個橋接器從中讀取資料，不過每個 {{site.data.keyword.objectstorageshort}}
橋接器會讀取單一 Kafka 主題的訊息資料。{{site.data.keyword.objectstorageshort}}
橋接器的新實例一律會從 Kafka 主題中現有的最早偏移開始讀取。{{site.data.keyword.objectstorageshort}} 橋接器使用 Kafka 的消費者偏移管理，從
Kafka 可靠地傳送資料而無流失，但會有一點重複的機率。

您可以使用下列內容，控制在資料寫入 {{site.data.keyword.objectstorageshort}} 服務實例之前，要從
Kafka 讀取多少記錄。請在建立或更新橋接器時指定這些內容：
<dl><dt>`"uploadDurationThresholdSeconds"`</dt> 
<dd>以秒為單位定義一段時間，在這時間之後，從 Kafka 累計的資料會上傳至 {{site.data.keyword.objectstorageshort}} 服務。</dd>
<dt>`"uploadSizeThresholdKB"`</dt>
<dd>以 KB 為單位，控制在資料上傳至 {{site.data.keyword.objectstorageshort}} 服務之前，要從 Kafka 累計多少資料量。</dd>
</dl>

讓 {{site.data.keyword.objectstorageshort}} 橋接器將從 Kafka 讀取的資料上傳至 {{site.data.keyword.objectstorageshort}}
服務的觸發程式，是當這些值任一者第一次到達之時。{{site.data.keyword.objectstorageshort}} 橋接器並不保證在到達這些臨界值之一的確切時刻將資料傳送到
{{site.data.keyword.objectstorageshort}} 服務。因此，傳送的資料可能會稍後才抵達，或是大於這些內容指定的值。

{{site.data.keyword.objectstorageshort}} 橋接器將資料寫入 {{site.data.keyword.objectstorageshort}} 時會連結訊息，並使用換行字元作為分隔字元。這些分隔字元使橋接器不適合包含內嵌換行字元的訊息，以及二進位訊息資料。

## {{site.data.keyword.objectstorageshort}} 橋接器將資料分割成物件的方式
{: notoc}

{{site.data.keyword.objectstorageshort}} 橋接器的其中一個特性是能夠分割
Kafka 訊息，並將它們儲存為以一般字首命名的物件。以一般字首命名的一群物件，稱為分割區。{{site.data.keyword.objectstorageshort}} 橋接器分割與
Kafka 分割是不同的概念。

每次 {{site.data.keyword.objectstorageshort}} 橋接器有一批資料要上傳至
{{site.data.keyword.objectstorageshort}} 服務時，它會建立一個以上包含資料的物件。關於如何分割訊息及命名所產生之物件的決策，取決於橋接器的配置方式。物件名稱可以包含
Kafka meta 資料，也可能包含來自訊息本身的資料。橋接器目前支援下列兩種方式，以將
Kafka 訊息分割成 {{site.data.keyword.objectstorageshort}} 物件：

* 藉由 Kafka 訊息偏移。
* 藉由每個 Kafka 訊息中出現的 ISO 8601 日期。這需要 Kafka 訊息包含有效的 JSON 格式物件。

## 依 Kafka 訊息偏移進行分割
{: notoc}

若要依 Kafka 訊息偏移分割資料，請完成下列步驟：

1. 配置橋接器而不使用 `"inputFormat"` 內容。
2. 指定具有 `"type"` 內容的物件，並且在 `"partitioning"` 陣列中具有 `"kafkaOffset"` 值。 

    例如：
    <pre class="pre"><code>
        ```
        {
          "topic": "topic1",
          "type": "objectStorageOut",
          "name": "bridge1",
          "configuration" : {
            "credentials" : { ... },
            "container" : "container1",
            "uploadDurationThresholdSeconds" : "1000",
            "uploadSizeThresholdKB" : "1000",
            "partitioning" : [ {
                "type" : "kafkaOffset"
              }
            ]
          }
        }
        ```
     	</code></pre>
    {:codeblock}

    以此方式配置的橋接器，所產生的物件名稱會包含字首
    `"offset=<kafka_offset>"` 其中 `"<kafka_offset>"` 對應於該分割區（具有此字首的物件群組）中儲存的第一個
    Kafka 訊息。例如，如果橋接器產生名稱類似下列範例的物件，`<object_a>` 及 `<object_b>` 會包含偏移在範圍
    0 - 999 的訊息、`<object_c>` 會包含偏移在範圍 1000 -
    1999 的訊息，依此類推。

    <pre class="pre"><code>
        ```
        &lt;container_name&gt;/offset=0/&lt;object_a&gt;
        &lt;container_name&gt;/offset=0/&lt;object_b&gt;
        &lt;container_name&gt;/offset=1000/&lt;object_c&gt;
        &lt;container_name&gt;/offset=2000/&lt;object_d&gt;
        ```       
    </code></pre>
    {:codeblock}
  
    
## 依 ISO 8601 日期進行分割
{: notoc}

若要依 ISO 8601 日期分割資料，請完成下列步驟：

1. 配置 `"inputFormat"` 內容設為 `"json"` 的橋接器。對於 `"inputFormat"` 內容無法使用非 `"json"` 的值。
2. 指定具有 `"type"` 內容且值為 `"dateIso8601"` 的物件，並且在 `"partitioning"` 陣列中具有 `"propertyName"` 內容。 

	例如：
    <pre class="pre"><code>
    ```
    {
      "topic": "topic2",
      "type": "objectStorageOut",
      "name": "bridge2",
      "configuration" : {
        "credentials" : { ... },
        "container" : "container2",
        "inputFormat" : "json",
        "uploadDurationThresholdSeconds" : "1000",
        "uploadSizeThresholdKB" : "1000",
        "partitioning": [
          {
            "type": "dateIso8601",
            "propertyName": "timestamp"
          }
        ]
      }
    }
    ```
    </code></pre>
    {:codeblock}

	依 ISO 8601 日期進行分割需要 Kafka 訊息具有有效的 JSON 格式。用來配置橋接器的 JSON 中，`"propertyName"` 的值必須對應於每個 Kafka 訊息中的 ISO
	8601 日期欄位。在此範例中，`"timestamp"` 欄位必須包含有效的 ISO 8601 日期值。然後會根據日期來分割訊息。
	
	像此範例這樣配置的橋接器，所產生的物件會具有如下列所指定的名稱：
	`<object_a>` 包含具有 `"timestamp"` 欄位且日期為
	2016-12-07 的 JSON 訊息，`<object_b>` 及 `<object_c>` 都包含具有 `"timestamp"` 欄位且日期為
	2016-12-08 的 JSON 訊息。

    <pre class="pre"><code>
        ```
        &lt;container_name&gt;/dt=2016-12-07/&lt;object_a&gt;
        &lt;container_name&gt;/dt=2016-12-08/&lt;object_b&gt;
        &lt;container_name&gt;/dt=2016-12-08/&lt;object_c&gt;
        ```       
    </code></pre>
    {:codeblock}

	任何訊息資料如果為有效的 JSON，但不具有有效日期欄位或值，則會寫入到具有字首 `"dt=1970-01-01"` 的物件。

## {{site.data.keyword.objectstorageshort}} 橋接器度量值
{: notoc}

{{site.data.keyword.objectstorageshort}} 橋接器會報告度量值，而度量值可以使用 Grafana
顯示在您的儀表板上。感興趣的度量值包括下列各項：
<dl>
<dt><code>*.<var class="keyword varname">topicName</var>.<var class="keyword varname">bridgeName</var>.bytes-consumed-rate</code></dt>
<dd>測量橋接器取用資料的速率（以每秒位元組為單位）。</dd>
<dt><code>*.<var class="keyword varname">topicName</var>.<var class="keyword varname">bridgeName</var>.records-lag-max</code></dt>
<dd>測量針對此主題的任何分割區，橋接器所取用記錄數的最大遲滯量。當值隨著時間不斷增加，表示橋接器趕不上主題的生產者。</dd>
</dl>
