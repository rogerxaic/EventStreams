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

# Cloud Object Storage ブリッジ 
{: #cloud_object_storage_bridge }

**{{site.data.keyword.messagehub}} ブリッジは、標準プランのみの一部として使用可能です。**
<br/>

{{site.data.keyword.IBM}} Cloud Object Storage ブリッジは、{{site.data.keyword.messagehub}} Kafka トピックからデータを読み取って [{{site.data.keyword.IBM_notm}} Cloud Object Storage ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/services/cloud-object-storage/about-cos.html){:new_window} にそのデータを入れる手段を提供します。

Cloud Object Storage ブリッジは、{{site.data.keyword.messagehub}} の Kafka トピックから [Cloud Object Storage サービス ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/services/cloud-object-storage/about-cos.html){:new_window} のインスタンスへデータを保存することを可能にします。 このブリッジは、メッセージのバッチを Kafka からコンシュームし、そのメッセージ・データをオブジェクトとして Cloud Object Storage サービス内のバケットにアップロードします。 Cloud Object Storage ブリッジを構成することによって、データをオブジェクトとして Cloud Object Storage にアップロードする方法を制御できます。 例えば、以下のプロパティーを構成できます。

* オブジェクトが書き込まれる先のバケットの名前。
* オブジェクトが Cloud Object Storage サービスにアップロードされる頻度。
* どれだけのデータが各オブジェクトに書き込まれると Cloud Object Storage サービスにアップロードされるのか。

ブリッジの出力フォーマットは、区切り文字として改行文字を使用して連結された 1 つ以上のレコードを含んでいる、オブジェクト・ストレージ・サービス・オブジェクトです。

## Cloud Object Storage ブリッジを使用してデータが転送される仕組み
{: #data_transfer notoc}

Cloud Object Storage ブリッジは、トピックから多数の Kafka レコードを読み取って、それらのレコードのデータをオブジェクトに書き込むことによって機能します。 このオブジェクトが Cloud Object Storage サービスのインスタンスにアップロードされます。 各 Cloud Object Storage ブリッジは単一の Kafka トピックからメッセージ・データを読み取りますが、複数のブリッジが 1 つのトピックからデータを読み取ることも可能です。 Cloud Object Storage ブリッジの新規インスタンスは、常に、Kafka トピック内にある最も早いオフセットから読み取りを開始します。 Cloud Object Storage ブリッジは、Kafka のコンシューマー・オフセット管理を使用して、Kafka からデータを確実に転送します。その際、データの逸失はありませんが、重複の可能性は少しあります。

どれだけの数のレコードが Kafka から読み取られると Cloud Object Storage サービス・インスタンスにデータが書き込まれるのかを、以下のプロパティーを使用して制御できます。 ブリッジを作成または更新するときに、これらのプロパティーを指定してください。
<dl><dt>アップロード期間しきい値 (秒) (Upload Duration Threshold (seconds))</dt> 
<dd>どれだけの時間 (秒) が経過すると Kafka から累積されたデータが Cloud Object Storage サービスにアップロードされるのかを定義します。</dd>
<dt>アップロード・サイズしきい値 (kB) (Upload Size Threshold (kB))</dt>
<dd>Kafka から累積されたデータがどれだけの量 (キロバイト) になると Cloud Object Storage サービスにアップロードされるのかを制御します。</dd>
</dl>

上記の値のいずれかに最初に到達すると、それがトリガーとなって、Cloud Object Storage ブリッジが Kafka から読み取ったデータを Cloud Object Storage サービスにアップロードします。 Cloud Object Storage ブリッジでは、これらのしきい値のいずれかに達した正確な時点での Cloud Object Storage サービスへのデータ転送は保証されていません。 したがって、転送されるデータは、これらのプロパティーに指定された値よりも遅く到着したり、量が大きくなったりする可能性があります。

Cloud Object Storage ブリッジは、データを Cloud Object Storage に書き込むときに、区切り文字として改行文字を使用してメッセージを連結します。 この区切り文字のため、改行文字が埋め込まれたメッセージおよびバイナリー・メッセージ・データには、このブリッジは不適切です。


## Cloud Object Storage ブリッジで使用する資格情報の取得
{: notoc}

Cloud Object Storage ブリッジが Cloud Object Storage インスタンスへの接続を許可されるためには、資格情報を指定する必要があります。 Cloud Object Storage インスタンスの所有者または管理者に、以下のように Cloud Object Storage UI を使用して資格情報を作成するように要請してください。 

1. **「サービス資格情報」**を選択し、**新規資格情報**を追加します。 
2. **「新規資格情報」**に対して、**「ライター」**の**「アクセス役割」**および**「自動生成」**の**「サービス ID」**を選択します。
   
   ブリッジを作成するときに、この新規資格情報から結果の JSON を {{site.data.keyword.Bluemix_notm}} コンソールの {{site.data.keyword.messagehub}} ダッシュボードにコピーできます。 
   
   代替方法として、<code>apikey</code> フィールドおよび <code>resource_instance_id</code> フィールドを取得して、{{site.data.keyword.messagehub}} ダッシュボードにそれらを入力するか、または、REST 呼び出しを使用して直接ブリッジを作成する場合はブリッジ作成 JSON 内にそれらを設定することができます。

作成する資格情報は Cloud Object Storage インスタンス全体への書き込みアクセス権限を認可するので、このアクセス権限を、ブリッジが対話する相手の特定のバケットに制限することが望ましい場合もあります。
1. [「ID およびアクセス管理」ページ ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/iam/?env_id=ibm%3Ayp%3Aus-south#/serviceids){:new_window} に移動します。
2. このページに、自動生成されたサービス ID が表示されるはずです。 特定の ID を識別した場合は、**「サービス ID の管理」**アクションを選択してください。 
3. **「ポリシーの編集」**アクションを選択し、さらに制限して特定のバケットに限定するため、**「リソース・タイプ」**でバケットを選択し、**「リソース ID」**にバケットの名前を指定します。 **「保存」**をクリックします。


## Cloud Object Storage ブリッジの作成
{: notoc}

Kafka REST API を使用して新規 Cloud Object Storage ブリッジを作成するには、以下の例のような JSON を使用します。 バケット名が、Cloud Object Storage インスタンス内で固有であるだけでなく、グローバルに固有であることを確認してください。

<pre class="pre"><code>
{
  "name": "cosbridge",
  "topic": "kafka-java-console-sample-topic",
  "type": "objectStorageS3Out",
  "configuration" : {
    "credentials" : {
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


## Cloud Object Storage ブリッジがデータをパーティション化してオブジェクトに入れる方法
{: notoc}

Cloud Object Storage ブリッジの機能の 1 つは、Kafka メッセージをパーティション化して、共通の接頭部が名前に付いている一連のオブジェクトとしてそれらのメッセージを保管する機能です。 共通の接頭部が名前に付いているオブジェクトのグループをパーティションと呼びます。 Cloud Object Storage ブリッジのパーティション化は、Kafka のパーティション化とは異なる概念です。

Cloud Object Storage ブリッジは、Cloud Object Storage サービスにアップロードするまとまったデータができるたびに、そのデータを含んでいる 1 つ以上のオブジェクトを作成します。 メッセージをパーティション化し、生成されるオブジェクトに名前を付ける方法は、ブリッジの構成方法に基づいて決まります。 オブジェクト名には Kafka メタデータを含めることができ、メッセージ自体からのデータを含めることも可能です。 現在のところ、ブリッジでは、Kafka メッセージをパーティション化して Cloud Object Storage オブジェクトに入れる方法として、次の 2 つの方法がサポートされています。

* Kafka メッセージ・オフセットによって。
* 各 Kafka メッセージ内にある ISO 8601 日付によって。 これには、有効な JSON フォーマットのオブジェクトからなる Kafka メッセージであることが必要です。

## Kafka メッセージ・オフセットによるパーティション化
{: notoc}

Kafka メッセージ・オフセットによってデータをパーティション化するには、以下のステップを実行します。

1. `"inputFormat"` プロパティーを指定せずにブリッジを構成します。
2. `"partitioning"` 配列内に値 `"kafkaOffset"` の `"type"` プロパティーを設定してオブジェクトを指定します。 

    以下に例を示します。
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

    この方法で構成されたブリッジによって生成されるオブジェクト名には、接頭部 `"offset=<kafka_offset>"` が含まれます。
ここで、`"<kafka_offset>"` は、そのパーティション (この接頭部が付いたオブジェクトのグループ) に保管される最初の Kafka メッセージに対応します。 例えば、以下の例のような名前のオブジェクトをブリッジが生成する場合、`<object_a>` と `<object_b>` はオフセットが 0 から 999 の範囲のメッセージを含み、`<object_c>` はオフセットが 1000 から 1999 の範囲のメッセージを含み、以下同様となります。

    <pre class="pre"><code>
        ```
        &lt;bucket_name&gt;/offset=0/&lt;object_a&gt;
        &lt;bucket_name&gt;/offset=0/&lt;object_b&gt;
        &lt;bucket_name&gt;/offset=1000/&lt;object_c&gt;
        &lt;bucket_name&gt;/offset=2000/&lt;object_d&gt;
        ```       
    </code></pre>
    {:codeblock}
  
    
## ISO 8601 日付によるパーティション化
{: #partition_iso notoc}

ISO 8601 日付によってデータをパーティション化するには、以下のステップを実行します。

1. `"inputFormat"` プロパティーを `"json"` に設定してブリッジを構成します。 `"json"` 以外の `"inputFormat"` プロパティーを使用することはできません。
2. `"partitioning"` 配列内に、値 `"dateIso8601"` の `"type"` プロパティーおよび `"propertyName"` プロパティーを設定して、オブジェクトを指定します。 

	以下に例を示します。
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

	ISO 8601 日付によるパーティション化には、Kafka メッセージが有効な JSON フォーマットであることが必要です。 ブリッジを構成するために使用される JSON の `"propertyName"` の値は、各 Kafka メッセージ内の 8601 日付フィールドに対応する必要があります。 上の例では、`"timestamp"` フィールドの値は、有効な ISO 8601 日付値を含んでいなければなりません。 そうすれば、メッセージはメッセージの日付に従ってパーティション化されます。
	
	この例のように構成されたブリッジは、次のように示された名前のオブジェクトを生成します。
	`<object_a>` は、`"timestamp"` フィールドが日付 2016-12-07 の JSON メッセージを含み、`<object_b>` と `<object_c>` の両方は、`"timestamp"` フィールドが日付 2016-12-08 の JSON メッセージを含みます。

    <pre class="pre"><code>
        ```
        &lt;bucket_name&gt;/dt=2016-12-07/&lt;object_a&gt;
        &lt;bucket_name&gt;/dt=2016-12-08/&lt;object_b&gt;
        &lt;bucket_name&gt;/dt=2016-12-08/&lt;object_c&gt;
        ```       
    </code></pre>
    {:codeblock}

	有効な JSON であるが、有効な日付フィールドまたは値がないメッセージ・データは、接頭部 `"dt=1970-01-01"` のオブジェクトに書き込まれます。

## Cloud Object Storage ブリッジのメトリック
{: notoc}

Cloud Object Storage ブリッジはメトリックを報告します。 メトリックは Grafana を使用してダッシュボードで表示できます。 対象のメトリックには、以下のものがあります。
<dl>
<dt><code>*.<var class="keyword varname">topicName</var>.<var class="keyword varname">bridgeName</var>.bytes-consumed-rate</code></dt>
<dd>ブリッジがデータをコンシュームする速度を測定します (秒当たりのバイト数)。</dd>
<dt><code>*.<var class="keyword varname">topicName</var>.<var class="keyword varname">bridgeName</var>.records-lag-max</code></dt>
<dd>このトピック内のパーティションでブリッジによってコンシュームされるレコード数の最大の遅れを測定します。 時間の経過につれて値が増加していく場合、このトピックのプロデューサーにブリッジがついていっていないことを示します。</dd>
</dl>
