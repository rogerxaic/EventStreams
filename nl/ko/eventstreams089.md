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

# Object Storage 브릿지(더 이상 사용되지 않음)
{: #object_storage_bridge }

** {{site.data.keyword.objectstorageshort}} 브릿지는 2018년 8월 1일부터 더 이상 사용되지 않습니다.**
<br/>

{{site.data.keyword.objectstorageshort}} 브릿지에서 연결하는 기본 서비스가 더 이상 사용되지 않으므로 {{site.data.keyword.objectstorageshort}} 브릿지도 2018년 8월 1일부터 더 이상 사용되지 않습니다. 
{: shortdesc}

{{site.data.keyword.objectstorageshort}} 서비스가 기한 끝에 도달하고 사용이 중지되면 {{site.data.keyword.objectstorageshort}} 브릿지의 모든 인스턴스도 사용이 중지됩니다. 자세한 정보는 [사용 중단 공지사항: {{site.data.keyword.objectstorageshort}} OpenStack Swift(PaaS) ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/blogs/bluemix/2018/05/end-marketing-object-storage-openstack-swift-paas/){:new_window}를 참조하십시오. 

대안으로서 [Cloud Object Storage 브릿지](/docs/services/EventStreams?topic=eventstreams-cloud_object_storage_bridge)를 사용할 수 있습니다. 
{:deprecated}

{{site.data.keyword.objectstorageshort}} 브릿지는 {{site.data.keyword.messagehub}}의 Kafka 토픽에서 데이터를 {{site.data.keyword.Bluemix_short}} 서비스의 인스턴스에 아카이브합니다. 브릿지는
Kafka에서 일괄처리 메시지를 이용하고 메시지 데이터를
{{site.data.keyword.objectstorageshort}} 서비스의 컨테이너에 오브젝트로
업로드합니다.

{{site.data.keyword.Bluemix_short}}의 선호되는 오브젝트 스토리지 서비스는 이제 [{{site.data.keyword.IBM_notm}} Cloud Object Storage 서비스![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage){:new_window}입니다.

{{site.data.keyword.objectstorageshort}} 브릿지를 구성하여
데이터가 {{site.data.keyword.objectstorageshort}}에 오브젝트로 업로드되는 방식을 제어할 수 있습니다. 예를 들면, 구성할 수 있는
특성은 다음과 같습니다.

* 오브젝트가 작성되는 컨테이너 이름.
* 오브젝트가 {{site.data.keyword.objectstorageshort}} 서비스에 업로드되는 빈도.
* {{site.data.keyword.objectstorageshort}} 서비스에 업로드되기 전에 각 오브젝트에 작성되는 데이터의 크기.

브릿지의 출력 형식은 구분 기호로 줄 바꾸기 문자를 사용하여 연결된 하나 이상의 레코드가 포함된 오브젝트 스토리지 서비스 오브젝트입니다.

## {{site.data.keyword.objectstorageshort}} 브릿지를 사용하여 데이터가 전송되는 방법
{: notoc}

{{site.data.keyword.objectstorageshort}} 브릿지는
토픽에서 다수의 Kafka 레코드를 읽고 이러한 레코드의 데이터를 오브젝트에 써서
작업을 수행합니다. 이 오브젝트는 {{site.data.keyword.objectstorageshort}} 서비스의 인스턴스에 업로드됩니다. 각 {{site.data.keyword.objectstorageshort}} 브릿지는
하나의 토픽이 데이터를 읽는 여러 개의 브릿지가 있을 수 있더라도 단일 Kafka 토픽에서 데이터를
읽습니다. {{site.data.keyword.objectstorageshort}} 브릿지의 새 인스턴스는 항상
Kafka 토픽에 있는 가장 이전 오프셋부터 읽기를 시작합니다. {{site.data.keyword.objectstorageshort}} 브릿지는
Kafka의 이용자 오프셋 관리를 사용하여 어느 정도의 중복 가능성이 있지만 손실 없이 Kafka에서 안정적으로 데이터를
전송합니다.

다음 특성을 사용하여 {{site.data.keyword.objectstorageshort}} 서비스 인스턴스에 데이터가 기록되기 전에 Kafka에서 읽는 레코드의 수를
제어할 수 있습니다. 브릿지를 작성하거나 업데이트할 때 해당 특성을 지정하십시오.
<dl><dt>`"uploadDurationThresholdSeconds"`</dt> 
<dd>Kafka에서 누적된 데이터가
{{site.data.keyword.objectstorageshort}} 서비스에 업로드된 후의 기간을 초 단위로 정의합니다.</dd>
<dt>`"uploadSizeThresholdKB"`</dt>
<dd>데이터가
{{site.data.keyword.objectstorageshort}} 서비스에 업로드되기 전에 Kafka에서 누적되는 데이터의 양(KB)을
제어합니다.</dd>
</dl>

Kafka에서 읽은 데이터를 {{site.data.keyword.objectstorageshort}} 서비스로
업로드하기 위한 {{site.data.keyword.objectstorageshort}} 브릿지의 트리거는 해당 값 중 하나가
처음 도달한 때입니다. {{site.data.keyword.objectstorageshort}} 브릿지는 정확히 해당 임계값 중 하나에 도달되는 지점에 {{site.data.keyword.objectstorageshort}}
서비스로 데이터 전송을 보장하지 않습니다. 따라서 전송된 데이터는 이러한 특성에 의해 지정된 값보다 더 크거나
더 나중에 도착할 수 있습니다.

{{site.data.keyword.objectstorageshort}} 브릿지는
{{site.data.keyword.objectstorageshort}}에 데이터를 기록할 때 구분 기호로 줄 바꾸기 문자를 사용하여 메시지를 연결합니다. 이러한 구분 기호로 인해
임베드된 줄 바꾸기 문자를 포함하는 메시지 및 2진 메시지 데이터에는 이 브릿지가 적합하지 않습니다.

## {{site.data.keyword.objectstorageshort}} 브릿지가 데이터를 오브젝트로 파티셔닝하는 방법
{: notoc}

{{site.data.keyword.objectstorageshort}} 브릿지의 기능 중 하나는
Kafka 메시지를 파티셔닝하고 공통 접두부가 있는 이름이 지정된 오브젝트로 메시지를 저장하는 기능입니다. 공통 접두부와 함께 이름 지정된 오브젝트의
그룹을 파티션이라고 부릅니다. {{site.data.keyword.objectstorageshort}} 브릿지 파티셔닝은
Kafka 파티셔닝과 다른 개념입니다.

{{site.data.keyword.objectstorageshort}} 브릿지는
{{site.data.keyword.objectstorageshort}} 서비스로 업로드할 일괄처리 데이터가 있을 때마다
데이터를 포함하는 하나 이상의 오브젝트를 작성합니다. 메시지를 파티셔닝하고 결과 오브젝트의 이름을 지정하는 방법은
브릿지가 구성된 방식에 따라 결정됩니다. 오브젝트 이름에는 Kafka 메타데이터가 포함되며
메시지 자체의 데이터가 포함될 수도 있습니다. 현재 브릿지는
Kafka 메시지를 {{site.data.keyword.objectstorageshort}} 오브젝트로 파티셔닝하기 위해 다음 두 가지 방법을 지원합니다.

* Kafka 메시지 오프셋 사용.
* 각 Kafka 메시지에 있는 ISO 8601 날짜 사용. 여기에는 올바른 JSON 형식 오브젝트를 구성하기 위해 Kafka 메시지가 필요합니다.

## Kafka 메시지 오프셋을 사용하여 파티셔닝
{: notoc}

Kafka 메시지 오프셋으로 데이터를 파티셔닝하려면 다음 단계를 완료하십시오.

1. `"inputFormat"` 특성 없이 브릿지를 구성하십시오.
2. `"partitioning"` 배열에서 값 `"kafkaOffset"`의 `"type"` 특성으로 오브젝트를 지정하십시오. 

    예:
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

    이 방법으로 구성된 브릿지에서 생성한 오브젝트 이름에는
    `"offset=<kafka_offset>"` 접두부가 포함됩니다. 여기서 `"<kafka_offset>"`은
    해당 파티션(이 접두부를 사용하는 오브젝트 그룹)에 저장된 첫 번째 Kafka 메시지에 해당됩니다. 예를 들어,
    브릿지가 다음 예제와 같은 이름이 있는 오브젝트를 생성하는 경우
    `<object_a>` 및 `<object_b>`에는 0 - 999 범위의 오프셋이 있는
    메시지가 포함되고, `<object_c>`에는 1000 - 1999 범위의 오프셋이 있는
    메시지가 포함됩니다.

    <pre class="pre"><code>
        ```
        &lt;container_name&gt;/offset=0/&lt;object_a&gt;
        &lt;container_name&gt;/offset=0/&lt;object_b&gt;
        &lt;container_name&gt;/offset=1000/&lt;object_c&gt;
        &lt;container_name&gt;/offset=2000/&lt;object_d&gt;
        ```       
    </code></pre>
    {:codeblock}
  
    
## ISO 8601 날짜로 파티셔닝
{: notoc}

ISO 8601 날짜로 데이터를 파티셔닝하려면 다음 단계를 완료하십시오.

1. `"json"`으로 설정된 `"inputFormat"` 특성을 사용하여 브릿지를 구성하십시오. `"json"` 이외의 `"inputFormat"` 특성은
사용할 수 없습니다.
2. `"partitioning"` 배열에서 값 `"dateIso8601"` 및 `"propertyName"` 특성의 `"type"` 특성으로 오브젝트를 지정하십시오. 

	예:
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

	ISO 8601 날짜로 파티셔닝하려면 Kafka 메시지에 올바른 JSON 형식이 있어야 합니다. 브릿지를
	구성하기 위해 사용된 JSON에서 `"propertyName"`의 값이 각 Kafka 메시지의 ISO 8601
	날짜 필드에 해당해야 합니다. 이 예제에서 `"timestamp"` 필드에는
	올바른 ISO 8601 날짜 값이 포함되어야 합니다. 그러면 그 날짜에 따라서 메시지가 파티셔닝됩니다.
	
	이 예제처럼 구성된 브릿지는 다음과 같이 이름이 지정된 오브젝트를 생성합니다.
	`<object_a>`에는 날짜가 2016-12-07인 `"timestamp"` 필드가 있는
	JSON 메시지가 포함되어 있으며, `<object_b>` 및 `<object_c>`에는 모두 날짜가 2016-12-08인 `"timestamp"` 필드가 있는
	JSON 메시지가 포함되어 있습니다.

    <pre class="pre"><code>
        ```
        &lt;container_name&gt;/dt=2016-12-07/&lt;object_a&gt;
        &lt;container_name&gt;/dt=2016-12-08/&lt;object_b&gt;
        &lt;container_name&gt;/dt=2016-12-08/&lt;object_c&gt;
        ```       
    </code></pre>
    {:codeblock}

	올바른 JSON이지만 올바른 날짜 필드 또는 값이 없는 메시지 데이터의 경우 접두부가
	`"dt=1970-01-01"`인 오브젝트에 기록됩니다.

## {{site.data.keyword.objectstorageshort}} 브릿지 메트릭
{: notoc}

{{site.data.keyword.objectstorageshort}} 브릿지는 메트릭을
보고하며 Grafana를 사용하여 대시보드에 표시할 수 있습니다. 관심있는 메트릭에는 다음이 포함됩니다.
<dl>
<dt><code>*.<var class="keyword varname">topicName</var>.<var class="keyword varname">bridgeName</var>.bytes-consumed-rate</code></dt>
<dd>브릿지가 데이터를 이용하는 비율을 측정합니다(초당 바이트 단위).</dd>
<dt><code>*.<var class="keyword varname">topicName</var>.<var class="keyword varname">bridgeName</var>.records-lag-max</code></dt>
<dd>이 토픽의 파티션에 대한 브릿지에서 이용한 레코드의 수에서 최대 지연 기간을
측정합니다. 시간이 지남에 따라서 값이 증가하는 것은 브릿지가 토픽에 대해 제작자를 따르고 있지 않음을
나타냅니다.</dd>
</dl>
