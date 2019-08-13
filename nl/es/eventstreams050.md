---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-19"

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

Kafka proporciona un conjunto enriquecido de API y clientes en una gran variedad de lenguajes. Por ejemplo:

* **API central de Kafka (API de consumidor, productor y de administrador)**<br/>
    Se utiliza para enviar y recibir mensajes directamente desde uno de los temas de Kafka, o varios temas.
* **API de secuencias**<br/>
    Una API de proceso de secuencias de nivel superior para consumir, transformar y producir sucesos entre temas con facilidad.
* **API de conexión**<br/>
    Una infraestructura que permite que las integraciones reutilizables o estándar secuencien sucesos en sistemas externos como, por ejemplo, bases de datos.
* **KSQL**<br/>
    Una interfaz para procesar y unir sucesos a partir de temas utilizando la sintaxis de tipo SQL.

En la tabla siguiente se resume lo que puede utilizar con {{site.data.keyword.messagehub}}:

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
		<tr>
			<td>**Requisitos de autenticación**</td>
			<td>El cliente debe dar soporte a la autenticación mediante el mecanismo SASL Plain y utilizar la extensión SNI (identificación de nombres de servidor) del protocolo TLSv1.2</td>
			<td>El cliente debe dar soporte a la autenticación mediante el mecanismo SASL Plain y utilizar la extensión SNI (identificación de nombres de servidor) del protocolo TLSv1.2</td>
		</tr>

</table>
<br/>
Para obtener información sobre cómo utilizar la API de Kafka en el plan Clásico, consulte [API de Kafka - Clásico](/docs/services/EventStreams?topic=eventstreams-kafka_using_classic).


## Elección de un cliente Kafka para utilizarlo con {{site.data.keyword.messagehub}}
{: #kafka_clients}

El cliente oficial de la API de Kafka está escrito en Java y, como tal, contiene las funciones y correcciones de errores más recientes. Para obtener más información sobre esta API, consulte [API de productor Kafka 2.2 ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://kafka.apache.org/22/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html){:new_window} y [API de consumidor Kafka 2.2![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://kafka.apache.org/22/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html){:new_window}. 

Para otros lenguajes, se recomienda ejecutar uno de los clientes siguientes, todos han sido debidamente probados con {{site.data.keyword.messagehub}}.

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
			<td colspan="3">**Cliente oficial Apache Kafka**</td>
			</tr>
	  		<tr>
			<td>[Cliente Apache Kafka ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://kafka.apache.org/downloads)</td>
			<td>Java</td>
			<td>Última</td>
			<td>0.10.2 </td>
			<td>[Ejemplo de consola Java](/docs/services/EventStreams?topic=eventstreams-kafka_java_using)<br/>
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
{: #footnote_clients notoc}
1. {: #footnote1 notoc}Esta versión es la más antigua que se ha validado en las pruebas continuas. Normalmente, se trata de la versión inicial disponible dentro de los últimos 12 meses, pero puede ser más nueva si existen problemas conocidos.

<br/>
Si no puede ejecutar ninguno de los clientes de la lista, puede utilizar otros clientes de terceros que cumplan los siguientes requisitos mínimos (por ejemplo, [librdkafka ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/edenhill/librdkafka/){:new_window}).
* Admite la versión 0.10, o posterior, de Kafka
* Puede conectarse y autenticarse utilizando SASL PLAIN con TLSv1.2
* Admite las extensiones SNI para TLS donde el nombre de host del servidor se incluye en el reconocimiento de TLS
* Admite la criptografía de curvas elípticas. Sin embargo, solo probamos y tenemos experiencia con los clientes de terceros recomendados.

En todos los casos, se recomienda la versión más reciente del cliente.

<br/>
### Conexión de su cliente a {{site.data.keyword.messagehub}}
{: #connect_client}

Para obtener información sobre cómo configurar el cliente Java para que se conecte a {{site.data.keyword.messagehub}}, consulte [Configuración de su cliente](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_api_client).

## Configuración del cliente de API Kafka
{: #kafka_api_client}

Para establecer una conexión, los clientes deben estar configurados para utilizar SASL_SSL PLAIN sobre TLSv1.2 como mínimo y para requerir un nombre de usuario y una lista de servidores de programa de arranque. 

Para recuperar el nombre de usuario, la contraseña y una lista de servidores de programa de arranque, se necesita un objeto de credenciales de servicio o una clave de servicio para la instancia de servicio. Para obtener más información sobre cómo crear estos objetos, consulte
<link to Connecting to event Streams>
[Conexión a {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting).

Desde estos objetos:
* Utilice la propiedad <code>kafka_brokers_sasl property</code> como lista de servidores de programa de arranque. El formato de esta lista debe ser una lista separada por comas de entradas de host:puerto. Por ejemplo, <code>host1:port1,host2:port2</code>. Recomendamos incluir detalles de todos los hosts listados en la propiedad <code>kafka_brokers_sasl</code>.
* Utilice las propiedades <code>user</code> y <code>api_key</code> como nombre de usuario y contraseña

Para las instancias de servicio en el plan Clásico, esta información está disponible en la variable de entorno VCAP_SERVICES de la aplicación. Para obtener más información, consulte [Conexión a {{site.data.keyword.messagehub}} - Clásico](/docs/services/EventStreams?topic=eventstreams-connecting_classic).

Para un cliente Java, el ejemplo siguiente muestra el conjunto mínimo de propiedades, donde USERNAME, PASSWORD y KAFKA_BROKERS_SASL deben sustituirse por los valores que se han recuperado anteriormente.

```
bootstrap.servers=KAFKA_BROKERS_SASL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USERNAME" password="PASSWORD";
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
ssl.protocol=TLSv1.2
ssl.enabled.protocols=TLSv1.2
ssl.endpoint.identification.algorithm=HTTPS
```
{: codeblock}

<br/>
Tenga presente, si utiliza un cliente de Kafka anterior a 0.10.2.1, que la propiedad <code>sasl.jaas.config</code> no está soportada y en su lugar debe proporcionar la configuración de cliente en un archivo de configuración JAAS. 








 




