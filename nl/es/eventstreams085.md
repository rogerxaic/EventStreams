---

copyright:
  years: 2015, 2018
lastupdated: "2018-09-12"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# Elección del plan 
{: #plan_choose}

{{site.data.keyword.messagehub}} está disponible como dos planes diferentes en función de sus requisitos: Estándar y Empresa.

## Plan Estándar

El plan Estándar resulta adecuado si requiere funciones de ingestión y distribución de sucesos, pero no necesita las ventajas adicionales del plan Empresa. El plan Estándar ofrece acceso compartido a un clúster de {{site.data.keyword.messagehub}} multiarrendatario.

## Plan Empresa 

El plan Empresa resulta adecuado si el aislamiento de los datos, el rendimiento garantizado y la retención incrementada constituyen consideraciones importantes. El plan Empresa ofrece acceso exclusivo a un clúster de {{site.data.keyword.messagehub}} dedicado.

## Soporte en los planes Estándar y Empresa

En la tabla siguiente se resumen las funciones a las que dan soporte los planes:

<table>
    <caption>Tabla 1. Soporte en los planes Estándar y Empresa</caption>
      <tr>
	        <th></th>
		    <th>Plan Estándar</th>
		    <th>Plan Empresa</th>
        </tr>
		<tr>
			<td>**Propiedad**</td>
			<td>Multiarrendatario </td>
			<td>Un solo arrendatario</td>
		</tr>
        <tr>
			<td>**Zonas de disponibilidad**</td>
			<td>No soportado</td>
			<td>3</td>
		</tr>
	  		<tr>
			<td>**Versión de Kafka en el clúster**</td>
			<td>Kafka 0.10.2</td>
			<td>Kafka 1.1</td>
		</tr>
		<tr>
			<td>**Control de acceso preciso**</td>
			<td>No</td>
			<td>Sí</td>
		</tr>
		<tr>
			<td>**Soporte de Kafka Connect y Kafka Streams**</td>
			<td>Sí</td>
			<td>Sí</td>
		</tr>
		<tr>
			<td>**Número máximo de particiones**</td>
			<td>100</td>
			<td>1000</td>
		</tr>
		<tr>
			<td>**Periodo máximo de retención**</td>
			<td>1 GB por partición durante un máximo de 30 días </td>
			<td>Ilimitado hasta alcanzar el límite de almacenamiento del plan </td>
		</tr>
		<tr>
			<td>**Disponibilidad de regiones</td>
			<td>EE.UU. sur</br>
			Reino Unido</br>
			Sídney</br>
			Alemania (no la API MQ Light)</td>
			<td>EE.UU. sur</br>
			EE.UU. este<br/>
			Alemania<br/>
			<br/>
			</td>
		</tr>
		<tr>
     	    <td>**API soportadas**</td>
			<td>API de Kafka</br>
			API REST de administración<br/>
			API REST de Kafka</br>
			API MQ Light</br>
		    </td>
			<td>API de Kafka<br/>
			API REST de administración</td>
		</tr>
			<td>**Puente Cloud Object Storage y<br/>
			puente MQ soportados**</td>
			<td>Sí</td>
			<td>No</td>
		</tr>
		<tr>
			<td>**Periodo de tiempo de despliegue**</td>
			<td>Suministro instantáneo</td>
			<td>Se espera que el suministro lleve 3 horas</td>
		</tr>

</table>


<!--
## {{site.data.keyword.Bluemix_notm}} Public environment
{: notoc}

{{site.data.keyword.Bluemix_notm}} Public provides an
economical public cloud service where you pay for what you use and share infrastructure with
others.

In {{site.data.keyword.Bluemix_notm}} Public, the cost of
{{site.data.keyword.messagehub}} is determined by two factors: the
number of partitions that you use and the number of messages that you send and receive. There is no
charge for message data while it is retained on the topics, but the data that each partition retains
is capped at 1 GB.

For more information, see [{{site.data.keyword.Bluemix_notm}} Public ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud-computing/bluemix/public){:new_window}.
-->

