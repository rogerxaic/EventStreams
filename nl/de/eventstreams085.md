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

# Plan auswählen 
{: #plan_choose}

{{site.data.keyword.messagehub}} steht entsprechend Ihren Anforderungen in Form unterschiedlicher Pläne zur Verfügung: "Standard", "Enterprise" und "Classic". 

<!--
For information about the Classic plan, see
[Classic plan](/docs/services/EventStreams?topic=eventstreams-plan_choose_classic#plan_choose_classic).
-->
{: shortdesc}

## Plan "Standard"

Der Plan "Standard" eignet sich, wenn Sie Funktionen für Ereignisaufnahme und -verteilung, aber keine zusätzlichen Vorteile des Plans "Enterprise" benötigen. Der Plan "Standard" bietet gemeinsamen Zugriff auf einen {{site.data.keyword.messagehub}}-Multi-Tenant-Cluster.

## Plan "Enterprise" 

Der Plan "Enterprise" ist geeignet, wenn Datenisolation, garantierte Leistung und längere Aufbewahrungsdauer wichtige Aspekte sind. Der Plan "Enterprise" bietet exklusiven Zugriff auf einen dedizierten {{site.data.keyword.messagehub}}-Cluster.

## Plan "Classic"

Der Plan "Classic" ermöglicht den Zugriff auf die vorherige Edition des Plans "Standard" und wird nur für bestehende Workloads und die Abwärtskompatibilität bereitgestellt. Für neue Workloads sollte der Plan "Standard" verwendet werden.


## Was wird von den Plänen "Standard", "Enterprise" und "Classic" unterstützt?

In der folgenden Tabelle ist zusammengefasst, was von den Plänen jeweils unterstützt wird:

<table>
    <caption>Tabelle 1. Unterstützung in den Plänen "Standard", "Enterprise" und "Classic"</caption>
      <tr>
	        <th></th>
		    <th>Plan "Standard"</th>
		    <th>Plan "Enterprise"</th>
		    <th>Plan "Classic"</th>
        </tr>
		<tr>
			<td>**Tenancy**</td>
			<td>Multi-Tenant-Lösungen </td>
			<td>Single-Tenant-Lösungen</td>
			<td>Multi-Tenant-Lösungen</td>
		</tr>
        <tr>
			<td>**Verfügbarkeitszonen**</td>
			<td>3</td>
			<td>3</td>
			<td>Nicht unterstützt</td>
		</tr>
        <tr>
			<td>**Verfügbarkeit**</td>
			<td>99,95%</td>
			<td>99,95%</td>
			<td>99,5%</td>
		</tr>
	  		<tr>
			<td>**Kafka-Version in Cluster**</td>
			<td>Kafka 2.2</td>
			<td>Kafka 1.1 <br/>(Kafka 2.2 in Kürze)</td>
			<td>Kafka 1.1</td>
		</tr>
		<tr>
			<td>**Differenzierte Zugriffssteuerung**</td>
			<td>Ja</td>
			<td>Ja</td>
			<td>Nein</td>
		</tr>
				<tr>
			<td>**Cloud Service Endpoint-Unterstützung**</td>
			<td>Nein</td>
			<td>Ja</td>
			<td>Nein</td>
		</tr>
		<tr>
			<td>**Kafka Connect und Kafka Streams unterstützt **</td>
			<td>Ja</td>
			<td>Ja</td>
			<td>Ja</td>
		</tr>
		<tr>
			<td>**Maximale Anzahl von Partitionen**</td>
			<td>100</td>
			<td>1000</td>
			<td>100</td>
		</tr>
		<tr>
			<td>**Maximale Aufbewahrungsdauer**</td>
			<td>1 GB pro Partition bis zu 30 Tage </td>
			<td>Unbegrenzt bis zur Speichergrenze Ihres Plans </td>
			<td>1 GB pro Partition bis zu 30 Tage </td>
		</tr>
		<tr>
			<td>**Maximaler Durchsatz**</td>
			<td>1 MB pro Sekunde pro Partition (maximal 20 MB pro Sekunde) </td>
			<td>40 MB pro Sekunde (Spitzendurchsatz von 90 MB pro Sekunde)</td>
			<td>1 MB pro Sekunde pro Partition</td>
		</tr>
		<tr>
			<td>**Maximale Nachrichtengröße**</td>
			<td>1 MB</td>
			<td>1 MB</td>
			<td>1 MB</td>
		</tr>
		<tr>
			<td>**Verfügbarkeit an den Standorten (Regionen)**</td>
			<td>Dallas (us-south)</br>
 </td>
			<td>Dallas (us-south)</br>
			Washington (us-east)<br/>
			London (eu-gb)<br/>
			Sydney (au-syd)</br>
			Frankfurt (eu-de)<br/>
			Tokyo (jp-tok)<br/>
			<br/>
			</td>
			<td>Dallas (us-south)</br>
			London (eu-gb)</br>
			Sydney (au-syd)</br>
			Frankfurt (eu-de) - keine {{site.data.keyword.mql}}-API </td>
		</tr>
		<tr>
     	    <td>**Unterstützte APIs**</td>
			<td>Kafka-API</br>
			REST-konforme Verwaltungs-API<br/>
			REST-Producer-API</br>
		    </td>
			<td>Kafka-API<br/>
			REST-konforme Verwaltungs-API</td>
			<td>Kafka-API</br>
			REST-konforme Verwaltungs-API<br/>
			Kafka-REST-API</br>
			MQ Light-API</br>
		    </td>
		</tr>
		</tr>
			<td>**Cloud Object Storage-Bridge und<br/>
			MQ-Bridge unterstützt**</td>
			<td>Nein</td>
			<td>Nein</td>
			<td>Ja</td>
		</tr>
		<tr>
			<td>**Bereitstellungszeitrahmen**</td>
			<td>Sofortige Bereitstellung</td>
			<td>Bereitstellung innerhalb von 3 Stunden. Da "Enterprise" über eigene dedizierte Ressourcen für jeden Cluster verfügt, wird mehr Zeit für die Bereitstellung benötigt</td>
			<td>Sofortige Bereitstellung</td>
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

