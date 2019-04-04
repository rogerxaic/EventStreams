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

# Object Storage 网桥（不推荐）
{: #object_storage_bridge }

**{{site.data.keyword.objectstorageshort}} 网桥从 2018 年 8 月 1 日起已弃用。**
<br/>

因为 {{site.data.keyword.objectstorageshort}} 网桥连接的底层服务已弃用，所以 {{site.data.keyword.objectstorageshort}} 网桥从 2018 年 8 月 1 日起也已弃用。
{: shortdesc}

在 {{site.data.keyword.objectstorageshort}} 服务达到使用期限被废弃后，{{site.data.keyword.objectstorageshort}} 网桥的所有实例也将被废弃。有关更多信息，请参阅[弃用声明：{{site.data.keyword.objectstorageshort}} OpenStack Swift (PaaS) ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/blogs/bluemix/2018/05/end-marketing-object-storage-openstack-swift-paas/){:new_window}。 

作为替代方法，您可以使用 [Cloud Object Storage 网桥](/docs/services/EventStreams?topic=eventstreams-cloud_object_storage_bridge)。
{:deprecated}

{{site.data.keyword.objectstorageshort}} 网桥支持将 {{site.data.keyword.messagehub}} 中 Kafka 主题的数据归档到 {{site.data.keyword.Bluemix_short}} 服务的实例中。网桥使用来自 Kafka 的批量消息，并将消息数据作为对象上传到 {{site.data.keyword.objectstorageshort}} 服务中的容器。

请注意，{{site.data.keyword.Bluemix_short}} 中的首选 Object Storage 服务现在为 [{{site.data.keyword.IBM_notm}} Cloud Object Storage 服务。![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage){:new_window}。

通过配置 {{site.data.keyword.objectstorageshort}} 网桥，可以控制数据如何作为对象上传到 {{site.data.keyword.objectstorageshort}}。例如，可以配置的属性如下所示：

* 对象写入其中的容器的名称。
* 对象上传到 {{site.data.keyword.objectstorageshort}} 服务的频率。
* 向每个对象写入多少数据后，将该对象上传到 {{site.data.keyword.objectstorageshort}} 服务。

网桥的输出格式是一种 Object Storage 服务对象，包含一个或多个记录，记录之间使用换行符作为分隔符进行连接。

## 如何使用 {{site.data.keyword.objectstorageshort}} 网桥传输数据
{: notoc}

{{site.data.keyword.objectstorageshort}} 网桥的工作方式是从主题读取一定数量的 Kafka 记录，并将这些记录中的数据写入一个对象。然后，将此对象上传到 {{site.data.keyword.objectstorageshort}} 服务的实例。每个 {{site.data.keyword.objectstorageshort}} 网桥都会从单个 Kafka 主题读取消息数据，但一个主题可能会有多个网桥从中读取数据。新的 {{site.data.keyword.objectstorageshort}} 网桥实例始终从 Kafka 主题中提供的最早偏移量开始读取。{{site.data.keyword.objectstorageshort}} 网桥使用 Kafka 的使用者偏移量管理，从 Kafka 可靠地传输数据，而不会丢失数据，但在极个别情况下可能会出现重复。

可以使用以下属性来控制从 Kafka 读取多少记录后，将数据写入 {{site.data.keyword.objectstorageshort}} 服务实例。创建或更新网桥时，指定以下属性：
<dl><dt>`"uploadDurationThresholdSeconds"`</dt> 
<dd>定义时间段（以秒为单位），在此时间段后，会将从 Kafka 收集的数据上传到 {{site.data.keyword.objectstorageshort}} 服务。</dd>
<dt>`"uploadSizeThresholdKB"`</dt>
<dd>控制从 Kafka 收集的数据量（以千字节为单位），达到此数据量后，会将数据上传到 {{site.data.keyword.objectstorageshort}} 服务。</dd>
</dl>

达到其中任一值时，都会触发 {{site.data.keyword.objectstorageshort}} 网桥将从 Kafka 读取的数据上传到 {{site.data.keyword.objectstorageshort}} 服务。{{site.data.keyword.objectstorageshort}} 网桥不保证在达到其中一个阈值时立即将数据传输到 {{site.data.keyword.objectstorageshort}} 服务。因此，相比这两个属性指定的值，传输的数据可能更晚到达或者更大。

{{site.data.keyword.objectstorageshort}} 网桥在将数据写入 {{site.data.keyword.objectstorageshort}} 时，使用换行符作为分隔符来连接消息。这些分隔符导致网桥不适用于包含嵌入式换行符的消息以及二进制消息数据。

## {{site.data.keyword.objectstorageshort}} 网桥如何将数据分区为对象
{: notoc}

{{site.data.keyword.objectstorageshort}} 网桥的其中一个功能是对 Kafka 消息分区，并将其存储为以公共前缀命名的对象。以公共前缀命名的一组对象称为一个分区。{{site.data.keyword.objectstorageshort}} 网桥分区的概念与 Kafka 分区不同。

每次 {{site.data.keyword.objectstorageshort}} 网桥有一批数据要上传到 {{site.data.keyword.objectstorageshort}} 服务时，都会创建一个或多个包含数据的对象。关于如何对消息分区并对生成的对象命名的决策取决于网桥配置方式。对象名称可以包含 Kafka 元数据，并可能包含来自消息本身的数据。目前，网桥支持以下两种将 Kafka 消息分区为 {{site.data.keyword.objectstorageshort}} 对象的方式：

* 按 Kafka 消息偏移量。
* 按每条 Kafka 消息中提供的 ISO 8601 日期。这需要 Kafka 消息包含有效的 JSON 格式对象。

## 使用 Kafka 消息偏移量分区
{: notoc}

要按 Kafka 消息偏移量对数据分区，请完成以下步骤：

1. 配置网桥，而不使用 `"inputFormat"` 属性。
2. 使用 `"partitioning"` 数组中值为 `"kafkaOffset"` 的属性 `"type"` 来指定对象。 

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

    通过此方式配置的网桥所生成的对象名称包含前缀 `"offset=<kafka_offset>"`，其中 `"<kafka_offset>"` 对应于该分区中存储的第一条 Kafka 消息（具有此前缀的对象组）。例如，如果网桥生成名称类似以下示例的对象，那么 `<object_a>` 和 `<object_b>` 包含具有 0 - 999 范围内偏移量的消息，`<object_c>` 包含具有 1000 - 1999 范围内偏移量的消息，依此类推。

    <pre class="pre"><code>
        ```
        &lt;container_name&gt;/offset=0/&lt;object_a&gt;
        &lt;container_name&gt;/offset=0/&lt;object_b&gt;
        &lt;container_name&gt;/offset=1000/&lt;object_c&gt;
        &lt;container_name&gt;/offset=2000/&lt;object_d&gt;
        ```       
    </code></pre>
    {:codeblock}
  
    
## 按 ISO 8601 日期分区
{: notoc}

要按 ISO 8601 日期对数据分区，请完成以下步骤：

1. 配置网桥，并将 `"inputFormat"` 属性设置为 `"json"`。不能将 `"inputFormat"` 属性设置为使用除 `"json"` 以外的其他值。
2. 使用 `"partitioning"` 数组中值为 `"dateIso8601"` 的属性 `"type"` 以及 `"propertyName"` 属性来指定对象。 

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

	按 ISO 8601 日期分区需要 Kafka 消息具有有效的 JSON 格式。用于配置网桥的 JSON 格式的 `"propertyName"` 值必须对应于每条 Kafka 消息中的 ISO 8601 日期字段。在此示例中，`"timestamp"` 字段必须包含有效的 ISO 8601 日期值。然后，将根据消息的日期来对消息分区。
	
	类似此示例进行配置的网桥会生成指定名称的对象，如下所示：
	`<object_a>` 包含 JSON 消息以及日期为 2016-12-07 的 `"timestamp"` 字段，
	`<object_b>` 和 `<object_c>` 都包含 JSON 消息以及日期为
	2016-12-08 的 `"timestamp"` 字段。

    <pre class="pre"><code>
        ```
        &lt;container_name&gt;/dt=2016-12-07/&lt;object_a&gt;
        &lt;container_name&gt;/dt=2016-12-08/&lt;object_b&gt;
        &lt;container_name&gt;/dt=2016-12-08/&lt;object_c&gt;
        ```       
    </code></pre>
    {:codeblock}

	具有有效 JSON 格式但没有有效日期字段或值的任何消息数据都会写入前缀为 `"dt=1970-01-01"` 的对象。

## {{site.data.keyword.objectstorageshort}} 网桥度量值
{: notoc}

{{site.data.keyword.objectstorageshort}} 网桥会报告度量值，这些度量值可以使用 Grafana 在仪表板上显示。相关度量值包括以下各项：
<dl>
<dt><code>*.<var class="keyword varname">topicName</var>.<var class="keyword varname">bridgeName</var>.bytes-consumed-rate</code></dt>
<dd>度量网桥使用数据的速率（以字节/秒为单位）。</dd>
<dt><code>*.<var class="keyword varname">topicName</var>.<var class="keyword varname">bridgeName</var>.records-lag-max</code></dt>
<dd>对于此主题中的任何分区，度量网桥所使用记录数的最大落后数量。值随时间增大指示网桥跟不上该主题的生产者。</dd>
</dl>
