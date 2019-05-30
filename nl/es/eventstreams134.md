---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-09"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# El plan Clásico 
{: #plan_choose_classic}

{{site.data.keyword.messagehub}} está disponible en forma de distintos planes según sus necesidades. Para obtener información sobre los planes Estándar y Empresa, consulte [Elección del plan](/docs/services/EventStreams?topic=eventstreams-plan_choose#plan_choose).
{: shortdesc}
 
## Visión general del plan Clásico
El plan Clásico resulta adecuado si necesita funciones de ingesta y distribución de sucesos, pero no necesita las ventajas adicionales del plan Empresa. El plan Clásico ofrece acceso compartido a un clúster de {{site.data.keyword.messagehub}} multiarrendatario.


## Soporte en el plan Clásico

En la tabla siguiente se resumen las funciones a las que da soporte en este plan:

<table>
    <caption>Tabla 1. Soporte en el plan Clásico</caption>
      <tr>
	        <th></th>
		    <th>Plan Clásico</th>
        </tr>
		<tr>
			<td>**Propiedad**</td>
			<td>Multiarrendatario </td>
		</tr>
        <tr>
			<td>**Zonas de disponibilidad**</td>
			<td>No soportado</td>
		</tr>
        <tr>
			<td>**Disponibilidad**</td>
			<td>99.5%</td>
		</tr>
	  		<tr>
			<td>**Versión de Kafka en el clúster**</td>
			<td>Kafka 1.1</td>
		</tr>
		<tr>
			<td>**Control de acceso preciso**</td>
			<td>No</td>
		</tr>
				<tr>
			<td>**Soporte de Punto final de servicio de IBM Cloud**</td>
			<td>No</td>
		</tr>
		<tr>
			<td>**Soporte de Kafka Connect y Kafka Streams**</td>
			<td>Sí</td>

		</tr>
		<tr>
			<td>**Número máximo de particiones**</td>
			<td>100</td>

		</tr>
		<tr>
			<td>**Periodo máximo de retención**</td>
			<td>1 GB por partición durante un máximo de 30 días </td>

		</tr>
		<tr>
			<td>**Rendimiento máximo**</td>
			<td>1 MB por segundo por partición</td>
		</tr>
		<tr>
			<td>**Tamaño máximo de mensaje**</td>
			<td>1 MB</td>
		</tr>
		<tr>
			<td>**Disponibilidad de ubicación (región)**</td>
			<td>Dallas (us-south)</br>
			Londres (eu-gb)</br>
			Sídney (au-syd)</br>
			Frankfurt (eu-de) - sin API de {{site.data.keyword.mql}} </td>

		</tr>
		<tr>
     	    <td>**API soportadas**</td>
			<td>API de Kafka</br>
			API REST de administración<br/>
			API REST de Kafka</br>
			API de MQ Light</br>
		    </td>
		</tr>
			<td>**Puente Cloud Object Storage y<br/>
			puente MQ soportados***</td>
			<td>Sí</td>
		</tr>
		<tr>
			<td>**Periodo de tiempo de despliegue**</td>
			<td>Suministro instantáneo</td>
		</tr>

</table>

