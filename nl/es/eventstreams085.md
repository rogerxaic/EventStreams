---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-16"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:note: .note}

# Elección del plan 
{: #plan_choose}

{{site.data.keyword.messagehub}} está disponible en forma de distintos planes según sus necesidades: Estándar, Empresa y Clásico. 

<!--
For information about the Classic plan, see
[Classic plan](/docs/services/EventStreams?topic=eventstreams-plan_choose_classic#plan_choose_classic).
-->
{: shortdesc}

## Plan Estándar

El plan Estándar resulta adecuado si necesita funciones de ingesta y distribución de sucesos, pero no necesita las ventajas adicionales del plan Empresa. El plan Estándar ofrece acceso compartido a un clúster de {{site.data.keyword.messagehub}} multiarrendatario.

## Plan Empresa 

El plan Empresa resulta adecuado si el aislamiento de los datos, el rendimiento garantizado y la retención incrementada constituyen consideraciones importantes. El plan Empresa ofrece acceso exclusivo a un clúster de {{site.data.keyword.messagehub}} dedicado.

## Plan Clásico

El plan Clásico le da acceso a la edición anterior del plan Estándar y se proporciona únicamente para cargas de trabajo existentes y compatibilidad con versiones anteriores. Las nuevas cargas de trabajo debe suministrarlas con el plan Estándar.


## Soporte en los planes Estándar, Empresa y Clásico

En la tabla siguiente se resumen las funciones a las que dan soporte los planes:

<table>
    <caption>Tabla 1. Soporte en los planes Estándar, Empresa y Clásico</caption>
      <tr>
	        <th></th>
		    <th>Plan Estándar</th>
		    <th>Plan Empresa</th>
		    <th>Plan Clásico</th>
        </tr>
		<tr>
			<td>**Propiedad**</td>
			<td>Multiarrendatario </td>
			<td>Un solo arrendatario</td>
			<td>Multiarrendatario</td>
		</tr>
        <tr>
			<td>**Zonas de disponibilidad**</td>
			<td>3</td>
			<td>3</td>
			<td>No soportado</td>
		</tr>
        <tr>
			<td>**Disponibilidad**</td>
			<td>99.95%</td>
			<td>99.95%</td>
			<td>99.5%</td>
		</tr>
	  		<tr>
			<td>**Versión de Kafka en el clúster**</td>
			<td>Kafka 2.2</td>
			<td>Kafka 1.1 <br/>(Kafka 2.2 disponible próximamente)</td>
			<td>Kafka 1.1</td>
		</tr>
		<tr>
			<td>**Control de acceso preciso**</td>
			<td>Sí</td>
			<td>Sí</td>
			<td>No</td>
		</tr>
				<tr>
			<td>**Soporte de Punto final de servicio de IBM Cloud**</td>
			<td>No</td>
			<td>Sí</td>
			<td>No</td>
		</tr>
		<tr>
			<td>**Soporte de Kafka Connect y Kafka Streams**</td>
			<td>Sí</td>
			<td>Sí</td>
			<td>Sí</td>
		</tr>
		<tr>
			<td>**Número máximo de particiones**</td>
			<td>100</td>
			<td>1000</td>
			<td>100</td>
		</tr>
		<tr>
			<td>**Periodo máximo de retención**</td>
			<td>1 GB por partición durante un máximo de 30 días </td>
			<td>Ilimitado hasta alcanzar el límite de almacenamiento del plan </td>
			<td>1 GB por partición durante un máximo de 30 días </td>
		</tr>
		<tr>
			<td>**Rendimiento máximo**</td>
			<td>1 MB por segundo por partición (20 MB por segundo máximo) </td>
			<td>40 MB por segundo (rendimiento máximo de 90 MB por segundo)</td>
			<td>1 MB por segundo por partición</td>
		</tr>
		<tr>
			<td>**Tamaño máximo de mensaje**</td>
			<td>1 MB</td>
			<td>1 MB</td>
			<td>1 MB</td>
		</tr>
		<tr>
			<td>**Disponibilidad de ubicación (región)**</td>
			<td>Dallas (us-south)</br>
 </td>
			<td>Dallas (us-south)</br>
			Washington (us-east)<br/>
			Londres (eu-gb)<br/>
			Sídney (au-syd)</br>
			Frankfurt (eu-de)<br/>
			Tokio (jp-tok)<br/>
			<br/>
			</td>
			<td>Dallas (us-south)</br>
			Londres (eu-gb)</br>
			Sídney (au-syd)</br>
			Frankfurt (eu-de) - sin API de {{site.data.keyword.mql}} </td>
		</tr>
		<tr>
     	    <td>**API soportadas**</td>
			<td>API de Kafka</br>
			API REST de administración<br/>
			API REST de productor</br>
		    </td>
			<td>API de Kafka<br/>
			API REST de administración</td>
			<td>API de Kafka</br>
			API REST de administración<br/>
			API REST de Kafka</br>
			API de MQ Light</br>
		    </td>
		</tr>
		</tr>
			<td>**Puente Cloud Object Storage y<br/>
			puente MQ soportados***</td>
			<td>No</td>
			<td>No</td>
			<td>Sí</td>
		</tr>
		<tr>
			<td>**Periodo de tiempo de despliegue**</td>
			<td>Suministro instantáneo</td>
			<td>El suministro puede tardar hasta 3 horas. Debido a que el plan Empresa tiene sus propios recursos dedicados para cada clúster, requiere más tiempo para el suministro</td>
			<td>Suministro instantáneo</td>
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

