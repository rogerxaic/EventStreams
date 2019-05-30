---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-07"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}


# Utilización de las herramientas de la consola de Kafka con {{site.data.keyword.messagehub}}
{: #kafka_console_tools }

Apache Kafka se suministra con varias herramientas de consola para operaciones sencillas de administración y mensajería. Puede utilizar muchas de ellas con {{site.data.keyword.messagehub}}, aunque {{site.data.keyword.messagehub}} no permite la conexión con su clúster de ZooKeeper. A medida que Kafka se ha ido desarrollando, muchas de las herramientas que antes requerían conexión con ZooKeeper ya no la necesitan.
{: shortdesc}

Puede encontrar estas herramientas de consola en el directorio <code>bin</code> de la descarga de Kafka. Puede descargar un cliente desde [Descargas de Apache Kafka ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://kafka.apache.org/downloads){:new_window}.

Para proporcionar las credenciales de SASL a estas herramientas, cree un archivo de propiedades basado en el siguiente ejemplo:

<pre>
<code>
  sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  security.protocol=SASL_SSL
  sasl.mechanism=PLAIN
  ssl.protocol=TLSv1.2
  ssl.enabled.protocols=TLSv1.2
  ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

Sustituya USER y PASSWORD por los valores del separador **Credenciales de servicio** de {{site.data.keyword.messagehub}} en la consola de {{site.data.keyword.Bluemix_notm}}.


## Productor de la consola
{: #console_producer }

Puede utilizar la herramienta de productor de consola de Kafka con {{site.data.keyword.messagehub}}. Debe suministrar una lista de intermediarios y credenciales de SASL.

Después de crear el archivo de propiedades tal como se ha descrito anteriormente, puede ejecutar el productor de consola en un terminal del siguiente modo:

<pre>
<code>
  $ kafka-console-producer.sh --broker-list KAFKA_BROKERS_SASL --producer.config CONFIG_FILE --topic TOPIC_NAME
</code>
</pre>
{:codeblock}

Sustituya las siguientes variables del ejemplo por sus propios valores:
* KAFKA_BROKERS_SASL por el valor del separador **Credenciales de servicio** de {{site.data.keyword.messagehub}} de la consola de {{site.data.keyword.Bluemix_notm}}, como una lista de pares host:puerto separados por comas (por ejemplo, `host1:puerto1,host2:puerto2`). 
* CONFIG_FILE por la vía de acceso del archivo de configuración. 

Puede utilizar muchas de las demás opciones de esta herramienta, excepto aquellas que requieren acceso a ZooKeeper.


## Consumidor de consola
{: #console_consumer }

Puede utilizar la herramienta de consumidor de consola de Kafka con {{site.data.keyword.messagehub}}. Debe suministrar un servidor de programa de arranque y credenciales de SASL.

Después de crear el archivo de propiedades tal como se ha descrito anteriormente, puede ejecutar el consumidor de consola en un terminal del siguiente modo:

<pre>
<code>
  $ kafka-console-consumer.sh --bootstrap-server KAFKA_BROKERS_SASL --consumer.config CONFIG_FILE --topic TOPIC_NAME
</code>
</pre>
{:codeblock}

Sustituya las siguientes variables del ejemplo por sus propios valores:
* KAFKA_BROKERS_SASL por el valor del separador **Credenciales de servicio** de {{site.data.keyword.messagehub}} de la consola de {{site.data.keyword.Bluemix_notm}}, como una lista de pares host:puerto separados por comas (por ejemplo, `host1:puerto1,host2:puerto2`). 
* CONFIG_FILE por la vía de acceso del archivo de configuración. 

Puede utilizar muchas de las demás opciones de esta herramienta, excepto aquellas que requieren acceso a ZooKeeper.


## Grupos de consumidores
{: #consumer_groups_tool }

Puede utilizar la herramienta de grupos de consumidores de Kafka con {{site.data.keyword.messagehub}}. Puesto que {{site.data.keyword.messagehub}} no permite la conexión con su clúster de ZooKeeper, algunas de las opciones no están disponibles.

Después de crear el archivo de propiedades tal como se ha descrito anteriormente, puede ejecutar la herramienta de grupos de consumidores en un terminal. Por ejemplo, puede obtener una lista de los grupos de consumidores del siguiente modo:

<pre>
<code>
  $ kafka-consumer-groups.sh --bootstrap-server KAFKA_BROKERS_SASL --command-config CONFIG_FILE --list
</code>
</pre>
{:codeblock}

Sustituya las siguientes variables del ejemplo por sus propios valores:
* KAFKA_BROKERS_SASL por el valor del separador **Credenciales de servicio** de {{site.data.keyword.messagehub}} de la consola de {{site.data.keyword.Bluemix_notm}}, como una lista de pares host:puerto separados por comas (por ejemplo, `host1:puerto1,host2:puerto2`). 
* CONFIG_FILE por la vía de acceso del archivo de configuración.

Con esta herramienta también puede mostrar detalles como las posiciones actuales de los consumidores, su retardo y el id de cliente correspondiente a cada partición de un grupo. Por ejemplo:

<pre>
<code>
  $ kafka-consumer-groups.sh --bootstrap-server KAFKA_BROKERS_SASL --command-config CONFIG_FILE --describe --group GROUP
</code>
</pre>
{:codeblock}

Sustituya GROUP en el ejemplo por el nombre de grupo sobre el que desea recuperar detalles. 


## Temas
{: #topics_tool }

No puede utilizar la herramienta de temas de Kafka `kafka-topics` con {{site.data.keyword.messagehub}} porque la herramienta requiere acceso a ZooKeeper.

Sin embargo, puede administrar temas utilizando el panel de control de {{site.data.keyword.messagehub}} en la consola de {{site.data.keyword.Bluemix_notm}} o utilizando la API REST.


## Restablecimiento de Kafka Streams
{: #kafka_streams_reset }

Puede utilizar esta herramienta con {{site.data.keyword.messagehub}} para restablecer el estado de proceso de una aplicación Kafka Streams a fin de poder volver a procesar su entrada desde cero. Antes de ejecutar esta herramienta, asegúrese de que la aplicación Streams se haya detenido por completo.

Por ejemplo:

<pre>
<code>
  $ kafka-streams-application-reset.sh --bootstrap-servers KAFKA_BROKERS_SASL --config-file CONFIG_FILE --application-id APP_ID
</code>
</pre>
{:codeblock}

Sustituya las siguientes variables del ejemplo por sus propios valores:
* KAFKA_BROKERS_SASL por el valor del separador **Credenciales de servicio** de {{site.data.keyword.messagehub}} de la consola de {{site.data.keyword.Bluemix_notm}}, como una lista de pares host:puerto separados por comas (como por ejemplo `host1:puerto1,host2:puerto2`). 
* CONFIG_FILE por la vía de acceso del archivo de configuración. 
* APP_ID por el ID de la aplicación Streams.

