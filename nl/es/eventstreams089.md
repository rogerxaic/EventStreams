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

# Puente de Object Storage en el plan Clásico
{: #object_storage_bridge }

**El puente de {{site.data.keyword.objectstorageshort}} está en desuso desde el 1 de agosto de 2018.**
<br/>

Puesto que el servicio subyacente al que se conecta el puente de {{site.data.keyword.objectstorageshort}} está en desuso, el puente de {{site.data.keyword.objectstorageshort}} también ha quedado en desuso desde el 1 de agosto de 2018. 
{: shortdesc}

Cuando el servicio de {{site.data.keyword.objectstorageshort}} alcanza la finalización del ciclo de vida y está fuera de servicio, todas las instancias del puente de {{site.data.keyword.objectstorageshort}} también quedarán fuera de servicio. Para obtener más información, consulte el anuncio acerca del desuso: [deprecation announcement: {{site.data.keyword.objectstorageshort}} OpenStack Swift (PaaS) ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/blogs/bluemix/2018/05/end-marketing-object-storage-openstack-swift-paas/){:new_window}. 

Como alternativa, puede utilizar el [puente de Cloud Object Storage](/docs/services/EventStreams?topic=eventstreams-cloud_object_storage_bridge). 
{:deprecated}

El puente de {{site.data.keyword.objectstorageshort}} permite archivar los datos de temas de Kafka en una instancia del servicio de {{site.data.keyword.messagehub}} en una instancia del servicio {{site.data.keyword.Bluemix_short}}. El puente consume lotes de mensajes de Kafka y sube los datos de mensaje como objetos de datos a un contenedor del servicio de {{site.data.keyword.objectstorageshort}}.

Tenga en cuenta que el servicio de almacenamiento de objetos preferido en {{site.data.keyword.Bluemix_short}} ahora es el [servicio {{site.data.keyword.IBM_notm}} Cloud Object Storage. ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](docs/services/cloud-object-storage?topic=cloud-object-storage-about#about){:new_window}.

Al configurar el puente de {{site.data.keyword.objectstorageshort}}, puede controlar cómo se suben los datos como objetos a {{site.data.keyword.objectstorageshort}}. Por ejemplo, las propiedades que puede configurar son las siguientes:

* El nombre de contenedor en el que se graban los objetos.
* La frecuencia con que se cargan los objetos en el servicio de {{site.data.keyword.objectstorageshort}}.
* La cantidad de datos que se graban en cada objeto antes la carga en el servicio de {{site.data.keyword.objectstorageshort}}.

El formato de salida del puente es un objeto de servicio de almacenamiento de objetos que contiene uno o varios registros concatenados con caracteres de nueva línea como separadores.

## Cómo se transfieren los datos utilizando el puente de {{site.data.keyword.objectstorageshort}}
{: notoc}

El puente de {{site.data.keyword.objectstorageshort}} funciona leyendo un número de registro de Kafka desde un tema y escribiendo los datos de los registros en cuestión en un objeto. Este objeto se sube a una instancia del servicio de {{site.data.keyword.objectstorageshort}}. Cada uno de los puentes de {{site.data.keyword.objectstorageshort}} lee los datos de mensajes de un único tema de Kafka, aunque es posible que haya varios puentes leyendo datos de un solo tema. Una nueva instancia del puente de {{site.data.keyword.objectstorageshort}} siempre empieza leyendo el primer desplazamiento del tema de Kafka. El puente de {{site.data.keyword.objectstorageshort}} utiliza la gestión de desplazamiento del consumidor de Kafka para transferir datos de forma fiable desde Kafka sin pérdidas, pero existe alguna posibilidad de que se produzcan duplicaciones.

Puede controlar cuántos registros se leen desde Kafka antes de que los datos se graben en la instancia del servicio de {{site.data.keyword.objectstorageshort}} utilizando las propiedades siguientes. Especifique estas propiedades al crear o actualizar un puente:
<dl><dt>`"uploadDurationThresholdSeconds"`</dt> 
<dd>Define un período de tiempo en segundos después del cual los datos acumulados de Kafka se suben al servicio de {{site.data.keyword.objectstorageshort}}.</dd>
<dt>`"uploadSizeThresholdKB"`</dt>
<dd>Controla la cantidad de datos en kilobytes que se acumulan desde Kafka antes de que los datos se suban al servicio de {{site.data.keyword.objectstorageshort}}.</dd>
</dl>

El desencadenante que hace que el puente de {{site.data.keyword.objectstorageshort}} cargue los datos leídos desde Kafka en el servicio de {{site.data.keyword.objectstorageshort}} es el momento en que se alcanza por primera vez uno de estos valores. El puente de {{site.data.keyword.objectstorageshort}} no garantiza la transferencia de datos al servicio de {{site.data.keyword.objectstorageshort}} en el momento exacto en que se alcanza uno de estos umbrales. Por lo tanto, los datos transferidos pueden llegar más tarde o ser de mayor tamaño que lo que indican los valores especificados por estas propiedades.

El puente de {{site.data.keyword.objectstorageshort}} concatena mensajes utilizando caracteres de nueva línea como separadores a medida que graba los datos en {{site.data.keyword.objectstorageshort}}. Por este motivo, el puente no es apto para los mensajes que contienen caracteres de línea incluida y datos para mensajes binarios.

## Cómo particiona datos en objetos el puente de {{site.data.keyword.objectstorageshort}}
{: notoc}

Una de las características del puente de {{site.data.keyword.objectstorageshort}} es su capacidad para particionar mensajes de Kafka y almacenarlos como objetos con un prefijo común asignado. Un grupo de objetos con un prefijo asignado se llama partición. El particionamiento del puente de {{site.data.keyword.objectstorageshort}} es un concepto diferente al particionamiento de Kafka.

Cada vez que el puente de {{site.data.keyword.objectstorageshort}} tiene un lote de datos para cargar al servicio de {{site.data.keyword.objectstorageshort}}, crea uno o varios objetos que contienen los datos. La decisión sobre cómo se particionan los mensajes y qué nombre se asigna a los objetos depende de cómo se ha configurado el puente. Los nombres de objetos pueden contener metadatos de Kafka y posiblemente datos de los propios mensajes. Actualmente, el puente permite particionar mensajes de Kafka en objetos de {{site.data.keyword.objectstorageshort}} de las dos formas siguientes:

* Por desplazamiento de mensajes Kafka.
* Por una fecha ISO 8601 presente en el mensaje de Kafka. Esta opción requiere que los mensajes de Kafka incluyan un objeto de formato JSON válido.

## Particionamiento mediante el desplazamiento de mensajes de Kafka
{: notoc}

Para particionar datos por desplazamiento de mensajes de Kafka, siga estos pasos:

1. Configure un puente sin la propiedad `"inputFormat"`.
2. Especifique un objeto con la propiedad `"type"` del valor `"kafkaOffset"` en la matriz `"partitioning"`. 

    Por ejemplo:
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

    Los nombres de objeto generados por un puente configurado de esta forma contienen el prefijo
    `"offset=<kafka_offset>"` donde `"<kafka_offset>"` corresponde al primer mensaje de Kafka almacenado en la partición en cuestión (el grupo de objetos con este prefijo). Por
    ejemplo, si un puente genera objetos con nombres como en el ejemplo siguiente,
    `<object_a>` y `<object_b>` contienen mensajes con desplazamientos
    en el rango 0 - 999, `<object_c>` contiene mensajes con desplazamientos en el rango de 1000 -
    1999, y así sucesivamente.

    <pre class="pre"><code>
        ```
        &lt;container_name&gt;/offset=0/&lt;object_a&gt;
        &lt;container_name&gt;/offset=0/&lt;object_b&gt;
        &lt;container_name&gt;/offset=1000/&lt;object_c&gt;
        &lt;container_name&gt;/offset=2000/&lt;object_d&gt;
        ```       
    </code></pre>
    {:codeblock}
  
    
## Particionamiento por fecha ISO 8601
{: notoc}

Para particionar datos por fecha ISO 8601, siga estos pasos:

1. Configure un puente con la propiedad `"inputFormat"` establecida en `"json"`. No se puede utilizar una propiedad `"inputFormat"` que no sea `"json"`.
2. Especifique un objeto con una propiedad `"type"` con el valor `"dateIso8601"` y una propiedad `"propertyName"` en la matriz `"particionamiento"`. 

	Por ejemplo:
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

	El particionamiento por fecha ISO 8601 requiere que los mensajes de Kafka tengan un formato JSON válido. El valor de
	`"propertyName"` en el JSON que se utiliza para configurar el puente debe corresponder al campo de fecha ISO
	8601 en cada mensaje de Kafka. En este ejemplo, el campo `"timestamp"` debe contener un valor
	de fecha ISO 8601 válido. Entonces los mensajes se particionarán según las fechas correspondientes.
	
	Un puente configurado como este ejemplo genera objetos con nombres especificados de la siguiente manera:
	`<object_a>` contiene mensajes JSON con los campos `"timestamp"` con la fecha
	2016-12-07, y tanto `<object_b>` como `<object_c>` contienen mensajes JSON con campos `"timestamp"` con la fecha
	2016-12-08.

    <pre class="pre"><code>
        ```
        &lt;container_name&gt;/dt=2016-12-07/&lt;object_a&gt;
        &lt;container_name&gt;/dt=2016-12-08/&lt;object_b&gt;
        &lt;container_name&gt;/dt=2016-12-08/&lt;object_c&gt;
        ```       
    </code></pre>
    {:codeblock}

	Los datos de mensajes que sean un JSON válido, pero no incluyan un campo o valor de fecha válido, se escribirán con un objeto que contendrá el prefijo `"dt=1970-01-01"`.

## Métricas de puente de {{site.data.keyword.objectstorageshort}}
{: notoc}

El puente de {{site.data.keyword.objectstorageshort}} notifica métricas, que se pueden visualizar en el panel de control utilizando Grafana. Entre las métricas de interés se incluyen las siguientes:
<dl>
<dt><code>*.<var class="keyword varname">topicName</var>.<var class="keyword varname">bridgeName</var>.bytes-consumed-rate</code></dt>
<dd>Mide la velocidad con que el puente consume los datos (en bytes por segundo).</dd>
<dt><code>*.<var class="keyword varname">topicName</var>.<var class="keyword varname">bridgeName</var>.records-lag-max</code></dt>
<dd>Mide el retraso máximo en el número de registros consumidos por el puente para cualquier partición del tema. Un valor que aumente a lo largo del tiempo indica que el puente no está actualizado respecto a los productores del tema.</dd>
</dl>
