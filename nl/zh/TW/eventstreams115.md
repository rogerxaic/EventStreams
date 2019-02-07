---

copyright:
  years: 2015, 2019
lastupdated: "2018-06-01"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Cloud Object Storage 橋接器 
{: #cloud_object_storage_bridge }

** Cloud Object Storage 橋接器只提供於標準方案中。**
<br/>

{{site.data.keyword.IBM}} Cloud Object Storage 橋接器提供一種從 {{site.data.keyword.messagehub}} Kafka 主題讀取資料的方式，並會將資料置於 [{{site.data.keyword.IBM_notm}} Cloud Object Storage ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/cloud-object-storage/about-cos.html){:new_window}。
{: shortdesc}

Cloud Object Storage 橋接器容許您在 {{site.data.keyword.messagehub}} 中將 Kafka 主題的資料保存到 [Cloud Object Storage 服務 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/cloud-object-storage/about-cos.html){:new_window} 的實例。橋接器會從 Kafka 取用訊息批次，然後將訊息資料當作物件上傳到 Cloud Object Storage 服務中的儲存區。藉由配置 Cloud Object Storage 橋接器，您可以控制如何將資料作為物件上傳到 Cloud Object Storage。例如，您可以配置的內容如下：

* 用於寫入物件的儲存區名稱。
* 物件上傳至 Cloud Object Storage 服務的頻率。
* 在將物件上傳至 Cloud Object Storage 服務之前，寫入每一個物件的資料量。

橋接器的輸出格式是一個 Object Storage 服務物件，其中包含一個以上以換行字元作為分隔字元連結起來的記錄。

## 使用 Cloud Object Storage 橋接器傳送資料的方式
{: #data_transfer notoc}

Cloud Object Storage 橋接器在運作時會從主題讀取一些
Kafka 記錄，然後將這些記錄的資料寫入物件。此物件會上傳至 Cloud Object Storage 服務的實例。雖然一個主題可以有多個橋接器從中讀取資料，不過每一個 Cloud Object Storage
橋接器會讀取單一 Kafka 主題的訊息資料。Cloud Object Storage
橋接器的新實例一律會從 Kafka 主題中現有的最早偏移開始讀取。Cloud Object Storage 橋接器使用 Kafka 的消費者偏移管理，從
Kafka 可靠地傳送資料而無流失，但會有一點重複的機率。

您可以使用下列內容，控制在資料寫入 Cloud Object Storage 服務實例之前，要從
Kafka 讀取的記錄量。請在建立或更新橋接器時指定這些內容：
<dl><dt>上傳持續時間臨界值（秒）</dt> 
<dd>以秒為單位定義一段時間，在此期間之後，從 Kafka 累計的資料會上傳至 Cloud Object Storage 服務。</dd>
<dt>上傳大小臨界值 (kB)</dt>
<dd>控制在資料上傳至 Cloud Object Storage 服務之前，要從 Kafka 累計多少資料量（以 KB 為單位）。</dd>
</dl>

用於讓 Cloud Object Storage 橋接器將從 Kafka 讀取的資料上傳至 Cloud Object Storage
服務的觸發程式，於這些值的任一者第一次到達時觸發。Cloud Object Storage 橋接器並不保證在到達這些臨界值的其中一個確切時刻將資料傳送到
Cloud Object Storage 服務。因此，傳送的資料可能會稍後才抵達，或是大於這些內容指定的值。

Cloud Object Storage 橋接器在將資料寫入 Cloud Object Storage 時，會使用換行字元作為分隔字元來連結訊息。這些分隔字元使橋接器不適合包含內嵌換行字元的訊息，以及二進位訊息資料。


## 搭配使用 Cloud Object Storage 橋接器來取得認證
{: notoc}

您必須提供認證，Cloud Object Storage 橋接器才能連接至 Cloud Object Storage 實例。要求 Cloud Object Storage 實例的擁有者或管理者使用
Cloud Object Storage 使用者介面建立認證，如下所示： 

1. 選取**服務認證**，然後新增**新認證**。 
2. 針對**新認證**，選取**存取角色**為**撰寫者**、選取**服務 ID** 為**自動產生**。
   
   在建立橋接器時，您可以將這個新認證產生的 JSON 複製到 {{site.data.keyword.Bluemix_notm}} 主控台中的 {{site.data.keyword.messagehub}} 儀表板。 
   
   或者，您也可以取用 <code>apikey</code> 及 <code>resource_instance_id</code> 欄位，然後將它們輸入至 {{site.data.keyword.messagehub}} 儀表板，或者，如果您使用 REST 呼叫直接建立橋接器，則在建立橋接器 JSON 中進行設定。

您建立的認證授權撰寫者存取整個 Cloud Object Storage 實例，因此，您可能想要將此存取限制為橋接器將與之互動的特定儲存區。
1. 請移至[身分 & 存取管理頁面 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/iam/?env_id=ibm%3Ayp%3Aus-south#/serviceids){:new_window}。
2. 您應該會在此頁面上看到自動產生的服務 ID。當您識別特定 ID 後，請選取**管理服務 ID** 動作。 
3. 選取**編輯原則**動作，進一步將它限制為特定的**資源類型**（即儲存區）和**資源 ID**（即儲存區的名稱）。按一下**儲存**。


## 建立 Cloud Object Storage 橋接器
{: notoc}

若要使用 Kafka REST API 來建立新的 Cloud Object Storage 橋接器，請如下列範例所示使用 JSON。確定您的儲存區名稱在廣域上是唯一的，而不只是在您的 Cloud Object Storage 實例中是唯一的。

<pre class="pre"><code>
{
  "name": "cosbridge",
  "topic": "kafka-java-console-sample-topic",
  "type": "objectStorageS3Out",
      "configuration" : {
"credentials": {
          "endpoint" : "https://s3-api.us-geo.objectstorage.softlayer.net",
      "resourceInstanceId" : "crn::",
      "apiKey" : "your_api_key"
    },
    "bucket" : "cosbridge0",
    "uploadDurationThresholdSeconds" : 600,
    "uploadSizeThresholdKB" : 1024,
            "partitioning" : [ {
                "type" : "kafkaOffset"
      }
    ]
  }
}
</code></pre>
{: codeblock}


## Cloud Object Storage 橋接器將資料分割成物件的方式
{: notoc}

Cloud Object Storage 橋接器的其中一個特性是能夠分割
Kafka 訊息，並將它們儲存為以一般字首命名的物件。以一般字首命名的一群物件，稱為分割區。Cloud Object Storage 橋接器分割與
Kafka 分割是不同的概念。

每次 Cloud Object Storage 橋接器有一批資料要上傳至
Cloud Object Storage 服務時，它會建立一個以上包含資料的物件。關於如何分割訊息及命名所產生之物件的決策，取決於橋接器的配置方式。物件名稱可以包含
Kafka meta 資料，也可能包含來自訊息本身的資料。橋接器目前支援下列兩種方式，以將
Kafka 訊息分割成 Cloud Object Storage 物件：

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
            "bucket" : "bucket1",
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
        &lt;bucket_name&gt;/offset=0/&lt;object_a&gt;
        &lt;bucket_name&gt;/offset=0/&lt;object_b&gt;
        &lt;bucket_name&gt;/offset=1000/&lt;object_c&gt;
        &lt;bucket_name&gt;/offset=2000/&lt;object_d&gt;
        ```       
    </code></pre>
    {:codeblock}
  
    
## 依 ISO 8601 日期進行分割
{: #partition_iso notoc}

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
        "bucket" : "bucket2",
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
        &lt;bucket_name&gt;/dt=2016-12-07/&lt;object_a&gt;
        &lt;bucket_name&gt;/dt=2016-12-08/&lt;object_b&gt;
        &lt;bucket_name&gt;/dt=2016-12-08/&lt;object_c&gt;
        ```       
    </code></pre>
    {:codeblock}

	任何訊息資料如果為有效的 JSON，但不具有有效日期欄位或值，則會寫入到具有字首 `"dt=1970-01-01"` 的物件。

## Cloud Object Storage 橋接器度量值
{: notoc}

Cloud Object Storage 橋接器會報告度量值，而度量值可以使用 Grafana 顯示在您的儀表板上。感興趣的度量值包括下列各項：
<dl>
<dt><code>*.<var class="keyword varname">topicName</var>.<var class="keyword varname">bridgeName</var>.bytes-consumed-rate</code></dt>
<dd>測量橋接器取用資料的速率（以每秒位元組為單位）。</dd>
<dt><code>*.<var class="keyword varname">topicName</var>.<var class="keyword varname">bridgeName</var>.records-lag-max</code></dt>
<dd>測量針對此主題的任何分割區，橋接器所取用記錄數的最大遲滯量。當值隨著時間不斷增加，表示橋接器趕不上主題的生產者。</dd>
</dl>
