---

copyright:
  years: 2015, 2019
lastupdated: "2018-06-01"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Puente de Cloud Object Storage en el plan Clásico
{: #cloud_object_storage_bridge }


** El puente de Cloud Object Storage solo está disponible como parte del plan Clásico.**
<br/>

El puente de {{site.data.keyword.IBM}} Cloud Object Storage proporciona una forma de leer datos de un tema Kafka de {{site.data.keyword.messagehub}}
y colocar los datos en [{{site.data.keyword.IBM_notm}} Cloud Object Storage ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/services/cloud-object-storage?topic=cloud-object-storage-about#about){:new_window}.
{: shortdesc}

El puente de Cloud Object Storage permite archivar los datos de los temas Kafka de {{site.data.keyword.messagehub}} a una instancia del [servicio de Cloud Object Storage ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/services/cloud-object-storage?topic=cloud-object-storage-about#about){:new_window}. El puente consume lotes de mensajes de Kafka y sube los datos de mensaje como objetos a un grupo del servicio Cloud Object Storage. Configurando el puente de Cloud Object Storage, puede controlar cómo se suben los datos como objetos a Cloud Object Storage. Por ejemplo, las propiedades que puede configurar son las siguientes:

* El nombre de grupo en el que se escriben los objetos.
* La frecuencia con que se cargan los objetos en el servicio Cloud Object Storage.
* La cantidad de datos que se escriben en cada objeto antes la carga en el servicio de Cloud Object Storage.

El formato de salida del puente es un objeto de servicio de almacenamiento de objetos que contiene uno o varios registros concatenados con caracteres de nueva línea como separadores.

## Cómo se transfieren los datos utilizando el puente de Cloud Object Storage
{: #data_transfer notoc}

El puente de Cloud Object Storage funciona leyendo un número de registro de Kafka desde un tema y escribiendo los datos de los registros en cuestión en un objeto. Este objeto se sube a una instancia del servicio de Cloud Object Storage. Cada uno de los puentes de Cloud Object Storage lee los datos de mensajes de un único tema de Kafka, aunque es posible que haya varios puentes leyendo datos de un solo tema. Una nueva instancia del puente de Cloud Object Storage siempre empieza leyendo el primer desplazamiento del tema de Kafka. El puente de Cloud Object Storage utiliza la gestión de desplazamiento del consumidor de Kafka para transferir datos de forma fiable desde Kafka sin pérdidas, pero existe alguna posibilidad de que se produzcan duplicaciones.

Puede controlar cuántos registros se leen desde Kafka antes de que los datos se escriban en la instancia del servicio de Cloud Object Storage utilizando las propiedades siguientes. Especifique estas propiedades al crear o actualizar un puente:
<dl><dt>Umbral de duración de carga (segundos)</dt> 
<dd>Define un período de tiempo en segundos después del cual los datos acumulados de Kafka se suben al servicio de Cloud Object Storage.</dd>
<dt>Umbral de tamaño de carga (kB)</dt>
<dd>Controla la cantidad de datos en kilobytes que se acumulan desde Kafka antes de que los datos se suban al servicio de Cloud Object Storage.</dd>
</dl>

El desencadenante que hace que el puente de Cloud Object Storage para cargar datos leídos desde Kafka al servicio de Cloud Object Storage es el momento en que se alcanza por primera vez uno de estos valores. El servicio de Cloud Object Storage no garantiza la transferencia de datos al servicio de Cloud Object Storage en el momento exacto en que se alcanza uno de estos umbrales. Por lo tanto, los datos transferidos pueden llegar más tarde o ser de mayor tamaño que lo que indican los valores especificados por estas propiedades.

El puente de Cloud Object Storage concatena mensajes utilizando caracteres de nueva línea como separadores a medida que escribe los datos en Cloud Object Storage. Por este motivo, el puente no es apto para los mensajes que contienen caracteres de línea incluida y datos para mensajes binarios.


## Obtención de credenciales para utilizar con el puente de Cloud Object Storage
{: notoc}

Debe proporcionar credenciales para permitir que el puente de Cloud Object Storage se conecte a su instancia de Cloud Object Storage. Solicite al propietario o administrador de su instancia de Cloud Object Storage cree las credenciales utilizando la IU de Cloud Object Storage como se indica a continuación: 

1. Seleccione **Credenciales de servicio** y añada una **Nueva credencial**. 
2. Para la **Nueva credencial**, seleccione un **Rol de acceso** de **Escritor** y un **ID de servicio** de **Generar automáticamente**.
   
   Puede copiar el JSON resultante de esta nueva credencial en el panel de control de {{site.data.keyword.messagehub}} en la consola de {{site.data.keyword.Bluemix_notm}} cuando cree un puente. 
   
   Como alternativa, puede tomar los campos <code>apikey</code> e <code>id_instancia_servicio</code> y entrarlos en el panel de control de {{site.data.keyword.messagehub}} o establecerlos en el JSON de creación de puente si está creando el puente directamente utilizando una llamada REST.

La credencial que cree otorga acceso de escritor a toda la instancia de Cloud Object Storage, por lo tanto, es posible que desee restringir este acceso al grupo con el que interactuará el puente.
1. Vaya a la [página de gestión de accesos y usuarios ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/iam/overview){:new_window}. 
2. Debería ver el ID de servicio generado automáticamente en esta página. Cuando haya identificado el ID específico, seleccione la acción **Gestionar ID de servicio**. 
3. Seleccione la acción **Editar política** para restringirlo a un **Tipo de recurso** determinado, que es el grupo y un **ID de recurso**, que es el nombre del grupo. Pulse **Guardar**.


## Creación de un puente de Cloud Object Storage
{: notoc}

Para crear un puente nuevo de Cloud Object Storage mediante la API REST de Kafka, utilice JSON como en el ejemplo siguiente. Asegúrese de que el grupo de nombres son exclusivos globales, no sólo exclusivos dentro de la instancia de Cloud Object Storage.

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


## Cómo particiona datos en objetos el puente de Cloud Object Storage
{: notoc}

Una de las características del puente de Cloud Object Storage es su capacidad para particionar mensajes de Kafka y almacenarlos como objetos con un prefijo común asignado. Un grupo de objetos con un prefijo asignado se llama partición. El particionamiento del puente de Cloud Object Storage es un concepto diferente al particionamiento de Kafka.

Cada vez que el puente de Cloud Object Storage tiene un lote de datos para cargar al servicio de Cloud Object Storage, crea uno o varios objetos que contienen los datos. La decisión sobre cómo se particionan los mensajes y qué nombre se asigna a los objetos depende de cómo se ha configurado el puente. Los nombres de objetos pueden contener metadatos de Kafka y posiblemente datos de los propios mensajes. Actualmente, el puente permite particionar mensajes de Kafka en objetos de Cloud Object Storage de las dos formas siguientes:

* Por desplazamiento de mensajes Kafka.
* Por una fecha ISO 8601 presente en el mensaje de Kafka. Esta opción requiere que los mensajes de Kafka incluyan un objeto de formato JSON válido.

## Particionamiento de desplazamiento de mensajes de Kafka
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

    Los nombres de objeto generados por un puente configurado de esta forma contienen el prefijo
    `"offset=<kafka_offset>"` donde `"<kafka_offset>"` corresponde al primer mensaje de Kafka almacenado en la partición en cuestión (el grupo de objetos con este prefijo). Por
    ejemplo, si un puente genera objetos con nombres como en el ejemplo siguiente,
    `<object_a>` y `<object_b>` contienen mensajes con desplazamientos
    en el rango 0 - 999, `<object_c>` contiene mensajes con desplazamientos en el rango de 1000 -
    1999, y así sucesivamente.

    <pre class="pre"><code>
        ```
        &lt;bucket_name&gt;/offset=0/&lt;object_a&gt;
        &lt;bucket_name&gt;/offset=0/&lt;object_b&gt;
        &lt;bucket_name&gt;/offset=1000/&lt;object_c&gt;
        &lt;bucket_name&gt;/offset=2000/&lt;object_d&gt;
        ```       
    </code></pre>
    {:codeblock}
  
    
## Particionamiento por fecha ISO 8601
{: #partition_iso notoc}

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
        &lt;bucket_name&gt;/dt=2016-12-07/&lt;object_a&gt;
        &lt;bucket_name&gt;/dt=2016-12-08/&lt;object_b&gt;
        &lt;bucket_name&gt;/dt=2016-12-08/&lt;object_c&gt;
        ```       
    </code></pre>
    {:codeblock}

	Los datos de mensajes que sean un JSON válido, pero no incluyan un campo o valor de fecha válido, se escribirán con un objeto que contendrá el prefijo `"dt=1970-01-01"`.

## Métricas del puente de Cloud Object Storage
{: notoc}

El puente de Cloud Object Storage notifica métricas, que se pueden visualizar en el panel de control utilizando Grafana. Entre las métricas de interés se incluyen las siguientes:
<dl>
<dt><code>*.<var class="keyword varname">topicName</var>.<var class="keyword varname">bridgeName</var>.bytes-consumed-rate</code></dt>
<dd>Mide la velocidad con que el puente consume los datos (en bytes por segundo).</dd>
<dt><code>*.<var class="keyword varname">topicName</var>.<var class="keyword varname">bridgeName</var>.records-lag-max</code></dt>
<dd>Mide el retraso máximo en el número de registros consumidos por el puente para cualquier partición del tema. Un valor que aumente a lo largo del tiempo indica que el puente no está actualizado respecto a los productores del tema.</dd>
</dl>
