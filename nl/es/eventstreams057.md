---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-13"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}


# Restricciones conocidas
{: #restrictions}

Si encuentra un problema al utilizar {{site.data.keyword.messagehub}}, revise estas restricciones conocidas y las soluciones temporales. 
{: shortdesc}

## Las llamadas de Java Kafka no se migran tras error si falla un servidor de programa de arranque.
{: #calls_failover}

### Problema
{: #calls_failover_problem notoc}

La máquina virtual Java (JVM) almacena en memoria caché las búsquedas DNS. Cuando la JVM resuelve una dirección IP para un nombre de host, almacena en la memoria caché la dirección IP durante un periodo de tiempo especificado, conocido como tiempo de vida (TTL). Algunas configuraciones de Java establecen el TTL de JVM de modo que nunca renueve la dirección IP de un nombre de host hasta que se reinicie la JVM. Una configuración de ejemplo es una que tenga un gestor de seguridad.

### Solución temporal
{: #calls_failover_workaround notoc}

Puesto que {{site.data.keyword.messagehub}} utiliza los URL de servidor de programa de arranque de Kafka con varias direcciones IP para alta disponibilidad, no todas las direcciones IP del intermediario son conocidas para el cliente Kafka, lo que impide la migración tras error a un intermediario funcional. En estos casos, la migración tras error requiere una nueva consulta de las direcciones IP para que los URL de intermediario obtengan una dirección IP funcional. Se recomienda configurar la JVM con un valor de TTL de entre 30 y 60 segundos. Este valor garantiza que si una dirección IP del servidor de programa de arranque tiene problemas, el cliente Kafka podrá buscar y utilizar una nueva dirección IP consultando el DNS.

Del archivo <code>java.security</code>: 

```
# La política de memoria caché de búsqueda de nombres a nivel Java para realizar búsquedas correctas:
#
# cualquier valor negativo: siempre almacenar en memoria caché
# cualquier valor positivo: el número de segundos que se mantendrá una dirección en memoria caché
# cero: no almacenar en memoria caché
#
# el valor predeterminado es para siempre (FOREVER). Por motivos de seguridad, este almacenamiento
# en memoria caché se establece en para siempre cuando hay un gestor de seguridad establecido. Cuando no hay
# ningún gestor de seguridad establecido, el comportamiento predeterminado en esta implementación consiste en
# almacenar en memoria caché durante 30 segundos.
#
# NOTA: el hecho de modificar el valor predeterminado de este parámetro puede tener serias
#       implicaciones de seguridad. No lo modifique a no ser que esté seguro de que no se
#       expone a un ataque de suplantación de DNS.
#
#networkaddress.cache.ttl=-1
```

### Cómo modificar el TTL de la JVM
{: #jvm_ttl notoc}
* Para modificar el TTL de la JVM para todas las aplicaciones, establezca el valor de <code>networkaddress.cache.ttl</code> en el archivo
<code><var class="keyword varname">$JAVA_HOME</var>/jre/lib/security/java.security</code>.
* Para modificar el TTL de la JVM para una aplicación determinada, establezca <code>networkaddress.cache.ttl</code> en el código de la aplicación del modo siguiente:
```
java.security.Security.setProperty("networkaddress.cache.ttl" , "30");
```

## Las llamadas de Java Kafka pueden agotar el tiempo de espera
{: #calls_timeout_kafka}

### Problema
{: #calls_timeout_problem notoc}

A veces una llamada de cliente Kafka Java no encuentra Kafka. La causa del error es que el cliente Kafka ha determinado la misma dirección IP anómala para cada uno de los servidores de programa de arranque. El cliente Kafka intenta la dirección IP de cada intermediario (que es la misma dirección IP anómala) y determina de forma incorrecta que Kafka está inactivo. Tenga en cuenta que el cliente Kafka utiliza la primera dirección IP que se devuelve en la lista si se devuelven varias direcciones IP en la consulta de DNS.

### Solución temporal
{: #calls_timeout_workaround notoc}

Vuelva a intentar las llamadas después de esperar lo suficiente para que caduque la memoria caché DNS de JVM de los URL del intermediario. En las siguientes llamadas de Kafka, la consulta DNS debería devolver y utilizar una dirección IP de intermediario que funcione. 

Se ha creado la propuesta de mejora de Kafka (KIP) 302 para garantizar que los clientes Kafka prueban todas las direcciones IP de intermediario disponibles y no un subconjunto de las mismas, de modo que un error en una sola dirección IP ocasione una anomalía.


## Temas y particiones
{: #topics_partitions}

*  La longitud de los nombres de los temas está restringida y
pueden tener hasta 100 caracteres.
*  El número predeterminado de particiones por tema es de uno.
*  Cada espacio de {{site.data.keyword.Bluemix_notm}} tiene un límite de 100 particiones. Para
crear más particiones, utilice un espacio nuevo de
{{site.data.keyword.Bluemix_notm}}.

<!--following message retention info duplicted in FAQs eventstreams108-->

## Retención de mensajes
{: #message_retention}

De forma predeterminada, los mensajes se retienen un máximo de 24 horas
en Kafka y cada partición está limitada a 1 GB. Si se alcanza 1 GB, se descartarán los mensajes más antiguos para mantenerse dentro
del límite.

Puede cambiar el límite de tiempo para la retención de mensajes cuando cree un tema utilizando la interfaz de usuario o la API de administración. El límite de tiempo es un mínimo de una hora y un máximo de 30 días.

Para obtener información sobre las restricciones de los valores permitidos al crear temas utilizando un cliente Kafka o Kafka Streams, consulte [¿Cómo se utilizan las API de Kafka para crear y suprimir temas?](/docs/services/EventStreams?topic=eventstreams-faqs#topic_admin).

## Creación y supresión de temas en Kafka
{: #create_delete}

En Kafka, la creación y supresión de temas son operaciones asíncronas que pueden tardar algún tiempo en completarse. Se recomienda evitar el uso de patrones que dependan de la creación y supresión rápidas de temas o de la supresión y recreación rápidas de temas.

<!--
## Kafka REST API
{: #trouble_rest}

<br/>
**Is this specific to old Standard only? If so I'll move to specific Standard topic.**
{: note}

*  Only the binary-embedded format is supported for requests and
   responses. The Avro and JSON embedded formats are not supported.
*  Concurrent requests are not supported for a consumer instance.
   Read, commit, or delete requests corresponding to a consumer
   instance should be sent only after a response is received for
   any outstanding requests of that instance.

-->
<!--
<br/>
**Is this specific to old Standard only? If so I'll move to specific Standard topic.**
{: note}

## Kafka REST API rate limitation
{: #kafka_rate}

Applications using the Kafka REST API can be subject to rate
limiting for each ApiKey. When this limiting occurs, the API
responds with the following HTTP error:

<code>429 Too Many Requests</code>
{:screen}

If you see this error, wait and submit the request again.

<br/>
**Is this specific to old Standard only? If so I'll move to specific Standard topic.**
{: note}
-->
<!--12/04/18 - Karen: same info duplicated at messagehub108 -->
<!--
## Kafka REST API daily restart
{: #rest_restart}

The Kafka REST API restarts once a day for a short period of
time. During this period, the Kafka REST API might become
unavailable. If this happens, you are recommended to retry your
request. After the REST API has restarted, you will have to
create your Kafka consumer instances again. If this is the case, the
REST API returns the following JSON:

```'{"error_code":40403,"message":"Consumer instance not found."}'
```
{:screen}
-->
<!--
## Kafka high-level consumer API
{: #kafka_consumer}

You cannot use the Apache Kafka 0.8.2 simple or high-level
consumer API with {{site.data.keyword.messagehub}}. Instead, you can use the earliest supported Kafka consumer API, which is 0.10.
-->
