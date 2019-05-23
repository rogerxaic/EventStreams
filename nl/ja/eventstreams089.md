---

copyright:
  years: 2015, 2019
lastupdated: "2018-11-08"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .deprecated}

# クラシック・プランの Object Storage ブリッジ
{: #object_storage_bridge }

** {{site.data.keyword.objectstorageshort}} ブリッジは、2018 年 8 月 1 日より非推奨になりました。**
<br/>

{{site.data.keyword.objectstorageshort}} ブリッジが接続する基になるサービスが非推奨であるため、{{site.data.keyword.objectstorageshort}} ブリッジも 2018 年 8 月 1 日より非推奨になりました。 
{: shortdesc}

{{site.data.keyword.objectstorageshort}} サービスの有効期限が切れて使用廃止になると、{{site.data.keyword.objectstorageshort}} ブリッジのすべてのインスタンスもまた使用廃止になります。 詳しくは、[非推奨の発表: {{site.data.keyword.objectstorageshort}} OpenStack Swift (PaaS) ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/blogs/bluemix/2018/05/end-marketing-object-storage-openstack-swift-paas/){:new_window}を参照してください。 

別の方法として、[Cloud Object Storage ブリッジ](/docs/services/EventStreams?topic=eventstreams-cloud_object_storage_bridge)を使用することもできます。 
{:deprecated}

{{site.data.keyword.objectstorageshort}} ブリッジを使用すると、{{site.data.keyword.messagehub}} の Kafka トピックのデータを、{{site.data.keyword.Bluemix_short}} サービスのインスタンスにアーカイブすることができます。 このブリッジは、メッセージのバッチを Kafka からコンシュームし、そのメッセージ・データをオブジェクトとして {{site.data.keyword.objectstorageshort}} サービス内のコンテナーにアップロードします。

{{site.data.keyword.Bluemix_short}} での優先オブジェクト・ストレージ・サービスは現在は [{{site.data.keyword.IBM_notm}} Cloud Object Storage サービス ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](docs/services/cloud-object-storage?topic=cloud-object-storage-about#about){:new_window} であることに注意してください。

{{site.data.keyword.objectstorageshort}} ブリッジを構成することによって、データをオブジェクトとして {{site.data.keyword.objectstorageshort}} にアップロードする方法を制御できます。 例えば、以下のプロパティーを構成できます。

* オブジェクトが書き込まれる先のコンテナーの名前。
* オブジェクトが {{site.data.keyword.objectstorageshort}} サービスにアップロードされる頻度。
* どれだけのデータが各オブジェクトに書き込まれると {{site.data.keyword.objectstorageshort}} サービスにアップロードされるのか。

ブリッジの出力フォーマットは、区切り文字として改行文字を使用して連結された 1 つ以上のレコードを含んでいる、オブジェクト・ストレージ・サービス・オブジェクトです。

## {{site.data.keyword.objectstorageshort}} ブリッジを使用したデータの転送方法
{: notoc}

{{site.data.keyword.objectstorageshort}} ブリッジは、トピックから多数の Kafka レコードを読み取って、それらのレコードのデータをオブジェクトに書き込むことによって機能します。 このオブジェクトは、{{site.data.keyword.objectstorageshort}} サービスのインスタンスにアップロードされます。 各 {{site.data.keyword.objectstorageshort}} ブリッジは単一の Kafka トピックからメッセージ・データを読み取りますが、1 つのトピックから複数のブリッジがデータを読み取ることが可能です。 {{site.data.keyword.objectstorageshort}} ブリッジの新規インスタンスは、常に、Kafka トピック内にある最も早いオフセットから読み取りを開始します。 {{site.data.keyword.objectstorageshort}} ブリッジは、Kafka のコンシューマー・オフセット管理を使用して、Kafka からデータを確実に転送します。その際、データの逸失はありませんが、重複の可能性は少しあります。

どれだけのレコードが Kafka から読み取られると {{site.data.keyword.objectstorageshort}} サービス・インスタンスにデータが書き込まれるのかを、以下のプロパティーを使用して制御できます。 ブリッジを作成または更新するときに、これらのプロパティーを指定してください。
<dl><dt>`"uploadDurationThresholdSeconds"`</dt> 
<dd>どれだけの時間 (秒) が経過すると Kafka から集積されたデータが {{site.data.keyword.objectstorageshort}} サービスにアップロードされるのかを定義します。</dd>
<dt>`"uploadSizeThresholdKB"`</dt>
<dd>Kafka から集積されたデータがどれだけの量 (キロバイト) になると {{site.data.keyword.objectstorageshort}} サービスにアップロードされるのかを制御します。</dd>
</dl>

上記の値のいずれかに最初に到達すると、Kafka から読み取ったデータを {{site.data.keyword.objectstorageshort}} ブリッジが {{site.data.keyword.objectstorageshort}} サービスにアップロードするトリガーとなります。 {{site.data.keyword.objectstorageshort}} ブリッジでは、これらのしきい値のいずれかに達した正確な時点での {{site.data.keyword.objectstorageshort}} サービスへのデータ転送は保証されていません。 したがって、転送されるデータは、これらのプロパティーに指定された値よりも遅く到着したり、量が大きくなったりする可能性があります。

{{site.data.keyword.objectstorageshort}} ブリッジは、データを {{site.data.keyword.objectstorageshort}} に書き込むときに、区切り文字として改行文字を使用してメッセージを連結します。 この区切り文字のため、改行文字が埋め込まれたメッセージおよびバイナリー・メッセージ・データには、このブリッジは不適切です。

## {{site.data.keyword.objectstorageshort}} ブリッジがデータをパーティション化してオブジェクトに入れる方法
{: notoc}

{{site.data.keyword.objectstorageshort}} ブリッジの機能の 1 つは、Kafka メッセージをパーティション化して、共通の接頭部が名前に付いている一連のオブジェクトとしてそれらのメッセージを保管する機能です。 共通の接頭部が名前に付いているオブジェクトのグループをパーティションと呼びます。 {{site.data.keyword.objectstorageshort}} ブリッジのパーティション化は、Kafka のパーティション化とは異なる概念です。

{{site.data.keyword.objectstorageshort}} ブリッジは、{{site.data.keyword.objectstorageshort}} サービスにアップロードするまとまったデータができるたびに、そのデータを含んでいる 1 つ以上のオブジェクトを作成します。 メッセージをパーティション化し、生成されるオブジェクトに名前を付ける方法は、ブリッジの構成方法に基づいて決まります。 オブジェクト名には Kafka メタデータを含めることができ、メッセージ自体からのデータを含めることも可能です。 現在のところ、ブリッジでは、
Kafka メッセージをパーティション化して {{site.data.keyword.objectstorageshort}} オブジェクトに入れる方法として、次の 2 つの方法がサポートされています。

* Kafka メッセージ・オフセットによって。
* 各 Kafka メッセージ内にある ISO 8601 日付によって。 これには、有効な JSON フォーマットのオブジェクトからなる Kafka メッセージであることが必要です。

## Kafka メッセージ・オフセットを使用したパーティション化
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

    この方法で構成されたブリッジによって生成されるオブジェクト名には、接頭部 `"offset=<kafka_offset>"` が含まれます。
ここで、`"<kafka_offset>"` は、そのパーティション (この接頭部が付いたオブジェクトのグループ) に保管される最初の Kafka メッセージに対応します。 例えば、以下の例のような名前のオブジェクトをブリッジが生成する場合、`<object_a>` と `<object_b>` はオフセットが 0 から 999 の範囲のメッセージを含み、`<object_c>` はオフセットが 1000 から 1999 の範囲のメッセージを含み、以下同様となります。

    <pre class="pre"><code>
        ```
        &lt;container_name&gt;/offset=0/&lt;object_a&gt;
        &lt;container_name&gt;/offset=0/&lt;object_b&gt;
        &lt;container_name&gt;/offset=1000/&lt;object_c&gt;
        &lt;container_name&gt;/offset=2000/&lt;object_d&gt;
        ```       
    </code></pre>
    {:codeblock}
  
    
## ISO 8601 日付によるパーティション化
{: notoc}

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

	ISO 8601 日付によるパーティション化には、Kafka メッセージが有効な JSON フォーマットであることが必要です。 ブリッジを構成するために使用される JSON の `"propertyName"` の値は、各 Kafka メッセージ内の 8601 日付フィールドに対応する必要があります。 上の例では、`"timestamp"` フィールドの値は、有効な ISO 8601 日付値を含んでいなければなりません。 そうすれば、メッセージはメッセージの日付に従ってパーティション化されます。
	
	この例のように構成されたブリッジは、次のように示された名前のオブジェクトを生成します。
	`<object_a>` は、`"timestamp"` フィールドが日付 2016-12-07 の JSON メッセージを含み、
	`<object_b>` と `<object_c>` の両方は、`"timestamp"` フィールドが日付
	2016-12-08 の JSON メッセージを含みます。

    <pre class="pre"><code>
        ```
        &lt;container_name&gt;/dt=2016-12-07/&lt;object_a&gt;
        &lt;container_name&gt;/dt=2016-12-08/&lt;object_b&gt;
        &lt;container_name&gt;/dt=2016-12-08/&lt;object_c&gt;
        ```       
    </code></pre>
    {:codeblock}

	有効な JSON であるが、有効な日付フィールドまたは値がないメッセージ・データは、接頭部 `"dt=1970-01-01"` のオブジェクトに書き込まれます。

## {{site.data.keyword.objectstorageshort}} ブリッジのメトリック
{: notoc}

{{site.data.keyword.objectstorageshort}} ブリッジはメトリックをレポートします。
メトリックは Grafana を使用してダッシュボードで表示できます。 対象のメトリックには、以下のものがあります。
<dl>
<dt><code>*.<var class="keyword varname">topicName</var>.<var class="keyword varname">bridgeName</var>.bytes-consumed-rate</code></dt>
<dd>ブリッジがデータをコンシュームする速度を測定します (秒当たりのバイト数)。</dd>
<dt><code>*.<var class="keyword varname">topicName</var>.<var class="keyword varname">bridgeName</var>.records-lag-max</code></dt>
<dd>このトピック内のパーティションでブリッジによってコンシュームされるレコード数の最大の遅れを測定します。 時間の経過につれて値が増加していく場合、このトピックのプロデューサーにブリッジがついていっていないことを示します。</dd>
</dl>
