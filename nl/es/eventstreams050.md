---

copyright:
  years: 2015, 2019
lastupdated: "2018-12-21"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Utilización de la API de Kafka
{: #kafka_using}

Si está utilizando los clientes Java, puede utilizar los clientes Kafka públicamente disponibles de la versión 0.10x o posterior. Para obtener más información, consulte [Elección de un cliente Kafka para utilizarlo con {{site.data.keyword.messagehub}}](/docs/services/EventStreams/eventstreams062.html#kafka_clients).
{: shortdesc}

Existen clientes Kafka en varios idiomas y proporcionamos instrucciones para algunos de los idiomas. Puede utilizar otros, pero necesitará soporte SASL PLAIN para proporcionar credenciales. Además, si utiliza el plan Empresa, también tendrá que utilizar la extensión SNI (identificación de nombres de servidor) del protocolo TLSv1.2.

<table>
    <caption>Tabla 1. Soporte de cliente Kafka en los planes Estándar y Empresa</caption>
      <tr>
	        <th></th>
		    <th>Plan Estándar</th>
		    <th>Plan Empresa</th>
        </tr>
	  		<tr>
			<td>**Versión de Kafka en el clúster**</td>
			<td>Kafka 1.1</td>
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

Para obtener más información sobre las API de productor y de consumidor, consulte [Kafka Producer API 1.1.0 ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://kafka.apache.org/11/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html){:new_window} y [Kafka Consumer API 1.1.0 ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://kafka.apache.org/11/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html){:new_window}. 

