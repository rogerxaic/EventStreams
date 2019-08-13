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

# Der Plan "Classic" 
{: #plan_choose_classic}

{{site.data.keyword.messagehub}} steht entsprechend Ihren Anforderungen in Form unterschiedlicher Pläne zur Verfügung. Informationen zu den Plänen "Standard" und "Enterprise" finden Sie in [Plan auswählen](/docs/services/EventStreams?topic=eventstreams-plan_choose#plan_choose).
{: shortdesc}
 
## Übersicht über den Plan "Classic"
Der Plan "Classic" ist geeignet, wenn Sie Funktionen für Ereignisaufnahme und -verteilung, aber keine sonstigen Vorteile des Plans "Enterprise" benötigen. Der Plan "Classic" bietet gemeinsamen Zugriff auf einen {{site.data.keyword.messagehub}}-Multi-Tenant-Cluster.


## Was wird vom Plan "Classic" unterstützt?

In der folgenden Tabelle ist zusammengefasst, was vom Plan unterstützt wird:

<table>
    <caption>Tabelle 1. Unterstützung im Plan "Classic"</caption>
      <tr>
	        <th></th>
		    <th>Plan "Classic"</th>
        </tr>
		<tr>
			<td>**Tenancy**</td>
			<td>Multi-Tenant-Lösungen </td>
		</tr>
        <tr>
			<td>**Verfügbarkeitszonen**</td>
			<td>Nicht unterstützt</td>
		</tr>
        <tr>
			<td>**Verfügbarkeit**</td>
			<td>99,5%</td>
		</tr>
	  		<tr>
			<td>**Kafka-Version in Cluster**</td>
			<td>Kafka 1.1</td>
		</tr>
		<tr>
			<td>**Differenzierte Zugriffssteuerung**</td>
			<td>Nein</td>
		</tr>
				<tr>
			<td>**Cloud Service Endpoint-Unterstützung**</td>
			<td>Nein</td>
		</tr>
		<tr>
			<td>**Kafka Connect und Kafka Streams unterstützt **</td>
			<td>Ja</td>

		</tr>
		<tr>
			<td>**Maximale Anzahl von Partitionen**</td>
			<td>100</td>

		</tr>
		<tr>
			<td>**Maximale Aufbewahrungsdauer**</td>
			<td>1 GB pro Partition bis zu 30 Tage </td>

		</tr>
		<tr>
			<td>**Maximaler Durchsatz**</td>
			<td>1 MB pro Sekunde pro Partition</td>
		</tr>
		<tr>
			<td>**Maximale Nachrichtengröße**</td>
			<td>1 MB</td>
		</tr>
		<tr>
			<td>**Verfügbarkeit an den Standorten (Regionen)**</td>
			<td>Dallas (us-south)</br>
			London (eu-gb)</br>
			Sydney (au-syd)</br>
			Frankfurt (eu-de) - keine {{site.data.keyword.mql}}-API </td>

		</tr>
		<tr>
     	    <td>**Unterstützte APIs**</td>
			<td>Kafka-API</br>
			Admin-REST-API<br/>
			Kafka-REST-API</br>
			MQ Light-API</br>
		    </td>
		</tr>
			<td>**Cloud Object Storage-Bridge und<br/>
			MQ-Bridge unterstützt**</td>
			<td>Ja</td>
		</tr>
		<tr>
			<td>**Bereitstellungszeitrahmen**</td>
			<td>Sofortige Bereitstellung</td>
		</tr>

</table>

