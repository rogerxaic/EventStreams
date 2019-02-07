---

copyright:
  years: 2015, 2019
lastupdated: "2019-01-14"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Elección de un cliente Kafka para utilizarlo con {{site.data.keyword.messagehub}}
{: #kafka_clients}

Para utilizar la API de Kafka con {{site.data.keyword.messagehub}}, seleccione uno de los tipos de cliente siguientes:

* El cliente Java oficial. Es la mejor elección porque contiene las últimas funciones disponibles para Apache Kafka.
* Uno de los [clientes de terceros recomendados](/docs/services/EventStreams/eventstreams062.html#clients_table).

Para los tipos de cliente, recomendamos seleccionar siempre la última versión de cliente. 
{: shortdesc}

## Requisitos del cliente para conectarse a secuencias de sucesos

Para conectarse a {{site.data.keyword.messagehub}}, los clientes deben ofrecer soporte a la autenticación utilizando el mecanismo SASL Plain y utilizar la extensión SNI (identificación de nombres de servidor) del protocolo TLSv1.2.

El protocolo Kafka mínimo al que damos soporte es 0.10.

<!--
## Support summary for the official Apache Kafka client (Java)

<table>
    <caption>Table 1. Kafka client support in Standard and Enterprise plans</caption>
      <tr>
	        <th></th>
		    <th>Standard and Enterprise Plans</th>
		    <th></th>
        </tr>
	  		<tr>
			<td>**Kafka version on cluster**</td>
			<td>Kafka 1.1</td>
		</tr>
	  		<tr>
			<td>**Supported client versions**</td>
			<td>Kafka 1.1, or later</td>
		</tr>
			<td>**Authentication requirements**</td>
			<td>Client must support authentication using the SASL Plain mechanism and use the Server Name Indication (SNI) extension to the TLSv1.2 protocol</td>
		</tr>

</table>
-->
	
## Clientes de terceros
{: #third_party_clients}

Si no puede ejecutar las clientes Java oficiales, recomendamos ejecutar uno de los [clientes de terceros recomendados](/docs/services/EventStreams/eventstreams062.html#clients_table), que están todos probados con {{site.data.keyword.messagehub}}. 

Otros clientes de terceros que ofrecen soporte al conjunto mínimo de requisitos de cliente pueden trabajar con {{site.data.keyword.messagehub}}. Sin embargo, solo probamos y tenemos experiencia con los clientes de terceros recomendados.

## Soporte al resumen de todos los clientes recomendados
{: #client_summary}

<table id="clients_table">
    <caption>Tabla 2. Resumen del soporte de cliente</caption>
      <tr>
		    <th>Cliente</th>
		    <th>Idioma</th>
			<th>Versión recomendada</th>
		    <th>Versión mínima soportada [<sup>1</sup>](/docs/services/EventStreams/eventstreams062.html#footnote1)</th>
			<th>Enlace al ejemplo</th>
        </tr>
			<tr>
			<td colspan="3">**Cliente oficial**</td>
			</tr>
	  		<tr>
			<td>[Cliente Apache Kafka ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://kafka.apache.org/downloads)</td>
			<td>Java</td>
			<td>Última</td>
			<td>0.10.2 <p> Para obtener información sobre clientes más antiguos, consulte la [compatibilidad con versiones anteriores](/docs/services/EventStreams/eventstreams062.html#compatability).</p></td>
			<td>[Ejemplo de consola Java ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-console-sample)<br/>
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

## Compatibilidad con versiones anteriores - Plan Estándar
{: #compatability}

Para la compatibilidad con versiones anteriores, puede utilizar el cliente Java Apache Kafka 0.9 con el plan Estándar de {{site.data.keyword.messagehub}}. Sin embargo, debido a la antigüedad de este cliente, no se recomienda su uso. Si decide utilizar esta versión del cliente, necesita un [módulo de inicio de sesión ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-0.9/message-hub-login-library) adicional.

Las versiones del cliente anteriores a la 0.11 pueden experimentar un rendimiento degradado debido a las conversiones de protocolo adicionales que se requieren para conectarse a las versiones más nuevas del servidor de Kafka.

<!--
## Unsupported clients

The following clients are not supported by {{site.data.keyword.messagehub}}:

### kafka-node
The kafka-node client does not fully support SASL authentication with the PLAIN mechanism so cannot currently be used with {{site.data.keyword.messagehub}}.


### no-kafka 
The no-kafka client does not fully support SASL authentication with the PLAIN mechanism so cannot currently be used with {{site.data.keyword.messagehub}}.

-->

## Conexión de su cliente a {{site.data.keyword.messagehub}}
{: #connect_client}

Para obtener información sobre cómo configurar el cliente Java para que se conecte a {{site.data.keyword.messagehub}}, consulte [Configuración de su cliente](/docs/services/EventStreams/eventstreams063.html).












