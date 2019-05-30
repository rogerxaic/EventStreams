---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-04"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Restricciones conocidas para el plan Clásico 
{: #restrictions_classic}

Si encuentra un problema al utilizar {{site.data.keyword.messagehub}} en el plan Clásico, revise estas restricciones conocidas y las soluciones temporales. 
{: shortdesc}

## Las llamadas de Java Kafka no se migran tras error si falla un servidor de programa de arranque.
{: #calls_failover}

### Problema
{: #calls_failover_problem notoc}

La máquina virtual Java (JVM) almacena en memoria caché las búsquedas DNS. Cuando la JVM resuelve una dirección IP para un nombre de host, almacena en la memoria caché la dirección IP durante un periodo de tiempo especificado, conocido como tiempo de vida (TTL). Algunas configuraciones de Java establecen el TTL de JVM de modo que nunca renueve la dirección IP de un nombre de host hasta que se reinicie la JVM. Una configuración de ejemplo es una que tenga un gestor de seguridad.

### Solución temporal
{: #calls_failover_workaround notoc}

Puesto que {{site.data.keyword.messagehub}} utiliza los URL de servidor de programa de arranque de Kafka con varias direcciones IP para alta disponibilidad, no todas las direcciones IP del intermediario son conocidas para el cliente Kafka, lo que impide la migración tras error a un intermediario de trabajo. En estos casos, la migración tras error requiere una nueva consulta de las direcciones IP para que los URL de intermediario obtengan una dirección IP de trabajo. Se recomienda configurar la JVM con un valor de TTL de entre 30 y 60 segundos. Este valor garantiza que si una dirección IP del servidor de programa de arranque tiene problemas, el cliente Kafka podrá buscar y utilizar una nueva dirección IP consultando el DNS.

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
* Para modificar el TTL de la JVM para todas las aplicaciones, establezca el valor de <code>networkaddress.cache.ttl</code> en el archivo
<code><var class="keyword varname">$JAVA_HOME</var>/jre/lib/security/java.security</code>.
* Para modificar el TTL de la JVM para una aplicación determinada, establezca <code>networkaddress.cache.ttl</code> en el código de la aplicación del modo siguiente:
```
java.security.Security.setProperty("networkaddress.cache.ttl" , "30");
```

## Las llamadas de Java Kafka pueden agotar el tiempo de espera
{: #calls_timeout}

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

## Retención de mensajes
{: #message_retention}

De forma predeterminada, los mensajes se retienen un máximo de 24 horas
en Kafka y cada partición está limitada a 1 GB. Si se alcanza 1 GB, se descartarán los mensajes más antiguos para mantenerse dentro
del límite.

Puede cambiar el límite de tiempo para la retención de mensajes cuando cree un tema utilizando la interfaz de usuario o la API de administración. El límite de tiempo es un mínimo de una hora y un máximo de 30 días.

Para obtener información sobre las restricciones de los valores permitidos al crear temas utilizando un cliente Kafka o Kafka Streams, consulte [Uso de la API de Kafka](/docs/services/EventStreams?topic=eventstreams-kafka_using).

## Creación y supresión de temas en Kafka
{: #create_delete}

En Kafka, la creación y supresión de temas son operaciones asíncronas que pueden tardar algún tiempo en completarse. Se recomienda evitar el uso de patrones que dependan de la creación y supresión rápidas de temas o de la supresión y recreación rápidas de temas.

## API REST de Kafka
{: #trouble_rest}

*  Para solicitudes y respuestas, sólo se admite el formato compacto binario. No se da soporte a los formatos incorporado Avro y JSON.
*  Las solicitudes actuales no se admiten para una instancia de consumidores.
   Las solicitudes read, commit o
                    delete correspondientes a una instancia de consumidores deben enviarse sólo después de
                    que se haya recibido una respuesta para todas las solicitudes pendientes de dicha                     instancia.

## Limitación de tasa de API REST de Kafka
{: #kafka_rate}

Las aplicaciones que utilizan la API REST de Kafka pueden estar sujetas a la limitación de tasa para cada ApiKey. Cuando se produce esta limitación, la API responde con el siguiente error HTTP:

<code>429 Demasiadas solicitudes</code>
{:screen}

Si ve este error, espere y vuelva a enviar la solicitud.

<!--12/04/18 - Karen: same info duplicated at messagehub108 -->
## Reinicio diario de la API REST de Kafka
{: #rest_restart}

La API REST de Kafka se reinicia una vez al día durante un breve período de tiempo. Durante este período, la API REST de Kafka puede estar disponible. Si esto sucede, se recomienda reintentar la solicitud. Una vez reiniciada la API REST, deberá volver a crear las instancias de consumidor de Kafka. Si este es el caso, la API REST devuelve el siguiente JSON:

```'{"error_code":40403,"message":"Consumer instance not found."}'
```
{:screen}

## API de consumidor de alto nivel Kafka
{: #kafka_consumer}

No puede utilizar la API simple o de
consumidor de alto nivel de Apache Kafka 0.8.2 con {{site.data.keyword.messagehub}}. Puede utilizar en su lugar la API de consumidor de Kafka soportada anteriormente, que es la 0.10.
