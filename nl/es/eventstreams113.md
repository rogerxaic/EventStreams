---

copyright:
  years: 2015, 2018
lastupdated: "2017-11-02"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Utilización de Kafka Connect con {{site.data.keyword.messagehub}}
{: #kafka_connect }

Puede utilizar Kafka Connect con {{site.data.keyword.messagehub}} y puede ejecutar los nodos trabajadores fuera o dentro de {{site.data.keyword.Bluemix_short}}.

Kafka Connect se puede ejecutar tanto en modalidad autónoma como en modalidad distribuida. La modalidad autónoma está pensada para pruebas y para conexiones temporales entre sistemas. La modalidad distribuida resulta más adecuada para uso en producción. La configuración necesaria para utilizar {{site.data.keyword.messagehub}} con estas dos modalidades es ligeramente diferente.
{:shortdesc}

## Configuración de un nodo trabajador autónomo
{: #standalone_worker notoc}

Debe proporcionar los servidores de programa de arranque y la información de credenciales de SASL en el archivo de propiedades del nodo trabajador que suministra cuando inicia un nodo trabajador autónomo de Kafka Connect.

El nodo trabajador autónomo no utiliza ningún tema interno. En su lugar, utiliza un archivo para almacenar la información de desplazamiento.

### Conector de origen
{: #source_connector notoc }

En el siguiente ejemplo se muestran las propiedades que debe especificar en el archivo de propiedades:

<pre>
<code>
  bootstrap.servers=KAFKA_BROKERS_SASL
  producer.sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  producer.security.protocol=SASL_SSL
  producer.sasl.mechanism=PLAIN
  producer.ssl.protocol=TLSv1.2
  producer.ssl.enabled.protocols=TLSv1.2
  producer.ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

Sustituya KAFKA_BROKERS_SASL, USER y PASSWORD por los valores del separador **Credenciales de servicio** de {{site.data.keyword.messagehub}} en la consola de {{site.data.keyword.Bluemix_notm}}.

### Conector sink
{: #sink_connector }

En el siguiente ejemplo se muestran las propiedades que debe especificar en el archivo de propiedades:

<pre>
<code>
  bootstrap.servers=KAFKA_BROKERS_SASL
  consumer.sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  consumer.security.protocol=SASL_SSL
  consumer.sasl.mechanism=PLAIN
  consumer.ssl.protocol=TLSv1.2
  consumer.ssl.enabled.protocols=TLSv1.2
  consumer.ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

Sustituya KAFKA_BROKERS_SASL, USER y PASSWORD por los valores del separador **Credenciales de servicio** de {{site.data.keyword.messagehub}} en la consola de {{site.data.keyword.Bluemix_notm}}.

## Configuración de un nodo trabajador distribuido
{: #distributed_worker notoc}

Debe proporcionar los servidores de programa de arranque y la información de credenciales de SASL en el archivo de propiedades que suministra cuando inicia los nodos trabajadores distribuidos de Kafka Connect. En el siguiente ejemplo se muestran las propiedades que debe especificar en el archivo de propiedades:

<pre>
<code>
  bootstrap.servers=KAFKA_BROKERS_SASL
  sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  security.protocol=SASL_SSL
  sasl.mechanism=PLAIN
  ssl.protocol=TLSv1.2
  ssl.enabled.protocols=TLSv1.2
  ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

Sustituya KAFKA_BROKERS_SASL, USER y PASSWORD por los valores del separador **Credenciales de servicio** de {{site.data.keyword.messagehub}} en la consola de {{site.data.keyword.Bluemix_notm}}.

Si desea utilizar un conector de origen, también debe especificar la configuración de SSL y SASL correspondiente al productor del siguiente modo:

<pre>
<code>
  producer.sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  producer.security.protocol=SASL_SSL
  producer.sasl.mechanism=PLAIN
  producer.ssl.protocol=TLSv1.2
  producer.ssl.enabled.protocols=TLSv1.2
  producer.ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

Si desea utilizar un conector sink, también debe especificar la configuración de SSL y SASL correspondiente al consumidor del siguiente modo:

<pre>
<code>
  consumer.sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  consumer.security.protocol=SASL_SSL
  consumer.sasl.mechanism=PLAIN
  consumer.ssl.protocol=TLSv1.2
  consumer.ssl.enabled.protocols=TLSv1.2
  consumer.ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

Además Kafka Connect en modalidad distribuida utiliza tres temas internamente. Estos temas se crean automáticamente cuando se arranca el nodo trabajador, si utiliza Kafka Connect en Apache Kafka versión 0.11 o posterior. Los nombres de los temas se especifican como parámetros de configuración. Asegúrese de que los valores coincidan para todos los nodos trabajadores con el valor de configuración `group.id`.

| Configuración               | Descripción                                                         |
| --------------------------- | ------------------------------------------------------------------- |
| `offset.storage.topic`      | Tema de desplazamientos de conector                                             |
| `offset.storage.partitions` | Número de particiones para tema de desplazamientos de conector (valor predeterminado 25) |
| `config.storage.topic`      | Tema de configuración de conector                                       |
| `status.storage.topic`      | Tema de estado de conector                                              |
| `status.storage.partitions` | Número de particiones para tema de estado de conector (valor predeterminado 5)          |

Por ejemplo, puede utilizar los siguientes pares clave-valor en su archivo de propiedades:

<pre>
<code>
  offset.storage.topic=connect-offsets
  config.storage.topic=connect-configs
  status.storage.topic=connect-status
</code>
</pre>
{:codeblock}

Considere la posibilidad de reducir el número de particiones si no utiliza mucho Kafka Connect.



