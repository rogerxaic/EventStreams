---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# Intercambio de mensajes entre la API MQ Light y Kafka o la API REST de Kafka
{: #mql_exchange}

**La API MQ Light solo está disponible como parte del plan Estándar.**
<br/>

Los mensajes de {{site.data.keyword.mql}} se almacenan en un tema único de Kafka subyacente denominado "MQLight" y se codifican como se detalla en la tabla siguiente. Esta codificación también puede ser utilizada por otros tipos de API, como Kafka o REST Kafka, para intercambiar mensajes con aplicaciones utilizando la API de {{site.data.keyword.mql}}.

## Formato de mensaje de Kafka
{: #kafka_format notoc}

<table border='1'>
<caption>Tabla 1. Formato de mensaje de Kafka</caption>
  <tr>
    <th> Clave </th>
    <th> Valor </th>
  </tr>
  <tr>
    <td> Opcional (no utilizado por la API)
	<p></p>
	</td>
    <td>**1 byte**
	<p>		     Captador de atención de la API MQ Light, que siempre es 0xFA.</p>
    <p><var class="keyword varname">**n**</var> **bytes**</p>
    <p>		    Mensaje codificado AMQP (formateado basándose en el formato de conexión AMQP). </p></td>
  </tr>
</table>


