---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-15"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}

# Utilización de la API de Kafka
{: #kafka_using}

Existen clientes Kafka en varios idiomas y proporcionamos instrucciones para algunos de los idiomas. Puede utilizar otros, pero necesitará soporte SASL PLAIN para proporcionar credenciales. Además, si utiliza el plan Empresa, también tendrá que utilizar la extensión SNI (identificación de nombres de servidor) del protocolo TLSv1.2.

Para obtener información sobre cómo utilizar la API de Kafka en el plan Clásico, consulte [API de Kafka - Clásico](/docs/services/EventStreams?topic=eventstreams-kafka_using_classic).

<table>
    <caption>Tabla 1. Soporte de cliente Kafka en los planes Estándar y Empresa</caption>
      <tr>
	        <th></th>
		    <th>Plan Estándar</th>
		    <th>Plan Empresa</th>
        </tr>
	  		<tr>
			<td>**Versión de Kafka en el clúster**</td>
			<td>Kafka 2.2</td>
			<td>Kafka 1.1</td>
		</tr>
	  		<tr>
			<td>**Versiones de cliente admitidas**</td>
			<td>Kafka 0.10.x, o posterior</td>
			<td>Kafka 0.10.x, o posterior</td>
		</tr>
		<tr>
			<td>**¿Se admite Kafka Connect, Kafka Streams y KSQL?**</td>
			<td>Sí</td>
			<td>Sí</td>
		</tr>

			<td>**Requisitos de autenticación**</td>
			<td>El cliente debe dar soporte a la autenticación mediante el mecanismo SASL Plain y utilizar la extensión SNI (identificación de nombres de servidor) del protocolo TLSv1.2</td>
			<td>El cliente debe dar soporte a la autenticación mediante el mecanismo SASL Plain y utilizar la extensión SNI (identificación de nombres de servidor) del protocolo TLSv1.2</td>
		</tr>

</table>

Para obtener más información sobre las API de productor y de consumidor V2.2, consulte
[Kafka Producer API 2.2 ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://kafka.apache.org/22/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html){:new_window} and
[Kafka Consumer API 2.2 ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://kafka.apache.org/22/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html){:new_window}. 


## Elección de un cliente Kafka para utilizarlo con {{site.data.keyword.messagehub}}
{: #kafka_clients}

Para utilizar la API de Kafka con {{site.data.keyword.messagehub}}, seleccione uno de los tipos de cliente siguientes:

* El cliente Java oficial. Es la mejor elección porque contiene las últimas funciones disponibles para Apache Kafka.
* Uno de los [clientes de terceros recomendados](/docs/services/EventStreams?topic=eventstreams-kafka_clients#clients_table).

Para los tipos de cliente, recomendamos seleccionar siempre la última versión de cliente. 

### Requisitos del cliente para conectarse a secuencias de sucesos

Para conectarse a {{site.data.keyword.messagehub}}, los clientes deben ofrecer soporte a la autenticación utilizando el mecanismo SASL Plain y utilizar la extensión SNI (identificación de nombres de servidor) del protocolo TLSv1.2.

El protocolo Kafka mínimo al que damos soporte es 0.10.

	
### Clientes de terceros
{: #third_party_clients}

Si no puede ejecutar las clientes Java oficiales, recomendamos ejecutar uno de los [clientes de terceros recomendados](/docs/services/EventStreams?topic=eventstreams-kafka_clients#clients_table), que están todos probados con {{site.data.keyword.messagehub}}. 
Otros clientes de terceros que ofrecen soporte al conjunto mínimo de requisitos de cliente pueden trabajar con {{site.data.keyword.messagehub}}. Sin embargo, solo probamos y tenemos experiencia con los clientes de terceros recomendados.

### Soporte al resumen de todos los clientes recomendados
{: #client_summary}

<table id="clients_table">
    <caption>Tabla 2. Resumen del soporte de cliente</caption>
      <tr>
		    <th id="client" scope="col">Cliente</th>
		    <th id="language" scope="col">Idioma</th>
			<th id="version" scope="col">Versión recomendada</th>
		    <th id="minimum version" scope="col">Versión mínima soportada [<sup>1</sup>](/docs/services/EventStreams?topic=eventstreams-kafka_clients#footnote1)</th>
			<th id="sample link" scope="col">Enlace al ejemplo</th>
        </tr>
			<tr>
			<td colspan="3">**Cliente oficial**</td>
			</tr>
	  		<tr>
			<td>[Cliente Apache Kafka ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://kafka.apache.org/downloads)</td>
			<td>Java</td>
			<td>Última</td>
			<td>0.10.2 </td>
			<td>[Ejemplo de consola de Java](/docs/services/EventStreams?topic=eventstreams-kafka_java_using)<br/>
			[Ejemplo de Liberty ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-liberty-sample)
			</td>
			</tr>
			<tr>
			<td colspan="3">**Clientes de terceros**</td>
			</tr>
	  		<tr>
			<td>[node-rdkafka ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/Blizzard/node-rdkafka)</td>
			<td>Node.js</td>
			<td>Última</td>
			<td>2.2.2</td>
			<td>[Ejemplo de Node.js ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample)</td>
		</tr>
		<tr>
			<td>[confluent-kafka-python ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/confluentinc/confluent-kafka-python)</td>
			<td>Python</td>
			<td>Última</td>
			<td>0.11.0</td>
			<td>[Ejemplo de Python de Kafka ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-python-console-sample)</td>
		</tr>
		<tr>
			<td>[confluent-kafka-go ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/confluentinc/confluent-kafka-go)</td>
			<td>Golang</td>
			<td>Última</td>
			<td>0.11.0</td>
			<td></td>
		</tr>
		<tr>
			<td>[librdkafka ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/edenhill/librdkafka)</td>
			<td>C o C++</td>
			<td>Última</td>
			<td>0.11.0</td>
			<td></td>
		</tr>

</table>
### Nota a pie de página
1. {: #footnote1}Esta versión es la más antigua que se ha validado en las pruebas continuas. Normalmente, se trata de la versión inicial disponible dentro de los últimos 12 meses, pero puede ser más nueva si existen problemas conocidos.


<!--
## Unsupported clients

The following clients are not supported by {{site.data.keyword.messagehub}}:

### kafka-node
The kafka-node client does not fully support SASL authentication with the PLAIN mechanism so cannot currently be used with {{site.data.keyword.messagehub}}.


### no-kafka 
The no-kafka client does not fully support SASL authentication with the PLAIN mechanism so cannot currently be used with {{site.data.keyword.messagehub}}.

-->

<br/>
### Conexión de su cliente a {{site.data.keyword.messagehub}}
{: #connect_client}

Para obtener información sobre cómo configurar el cliente Java para que se conecte a {{site.data.keyword.messagehub}}, consulte [Configuración de su cliente](/docs/services/EventStreams?topic=eventstreams-kafka_connect).

## Configuración del cliente de API Kafka
{: #kafka_api_client}

Para establecer una conexión, los clientes deben estar configurados para utilizar SASL_SSL PLAIN sobre TLSv1.2 como mínimo y para requerir un nombre de usuario y una lista de servidores de programa de arranque. 

Para recuperar el nombre de usuario, la contraseña y una lista de servidores de programa de arranque, se necesita un objeto de credenciales de servicio o una clave de servicio para la instancia de servicio. Para obtener más información sobre cómo crear estos objetos, consulte
<link to Connecting to event Streams>
[Conexión a {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting).

Desde estos objetos:
* Utilice la propiedad <code>kafka_brokers_sasl property</code> como lista de servidores de programa de arranque. El formato de esta lista debe ser una lista separada por comas de entradas de host:puerto. Por ejemplo, <code>host1:port1,host2:port2</code>. Recomendamos incluir detalles de todos los hosts listados en la propiedad <code>kafka_brokers_sasl</code>.
* Utilice las propiedades <code>user</code> y <code>api_key</code> como nombre de usuario y contraseña

Para las instancias del plan Clásico, esta información está disponible en su lugar de la variable de entorno VCAP_SERVICES de la aplicación. Para obtener más información, consulte [Conexión a {{site.data.keyword.messagehub}} - Clásico](/docs/services/EventStreams?topic=eventstreams-connecting_classic).

Para un cliente Java, el ejemplo siguiente muestra el conjunto mínimo de propiedades, donde USERNAME, PASSWORD y KAFKA_BROKERS_SASL deben sustituirse por los valores recuperados anteriormente.

```
bootstrap.servers=KAFKA_BROKERS_SASL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USERNAME" password="PASSWORD";
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
ssl.protocol=TLSv1.2
ssl.enabled.protocols=TLSv1.2
ssl.endpoint.identification.algorithm=HTTPS

# Para enviar o recibir mensajes, los siguientes son también obligatorios
key.deserializer=org.apache.kafka.common.serialization.StringDeserializer
value.deserializer=org.apache.kafka.common.serialization.StringDeserializer
```
{: codeblock}

<br/>
Tenga presente, si utiliza un cliente de Kafka anterior a 0.10.2.1, que la propiedad <code>sasl.jaas.config</code> no está soportada y en su lugar debe proporcionar la configuración de cliente en un archivo de configuración JAAS. 

### Conexión y autenticación en una aplicación que no sea Java
{: #kafka_notjava }

Cualquier cliente que soporte Kafka 0.10 con SASL PLAIN y TLSv1.2 debería funcionar con {{site.data.keyword.messagehub}}.

Tenga en cuenta que el cliente debe dar soporte a la extensión SNI de TLS, donde el nombre de host del servidor se incluye en el reconocimiento de TLS. 

Los clientes de ejemplo son los siguientes:
* [librdkafka ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/edenhill/librdkafka/){:new_window} 
* [confluent-kafka-python ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/confluentinc/confluent-kafka-python){:new_window} 




 




