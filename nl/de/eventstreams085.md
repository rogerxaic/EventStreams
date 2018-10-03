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

# Plan auswählen 
{: #plan_choose}

{{site.data.keyword.messagehub}} steht entsprechend Ihren Anforderungen in Form von zwei unterschiedlichen Plänen, "Standard" und "Enterprise", zur Verfügung.

## Plan "Standard"

Der Plan "Standard" eignet sich, wenn Sie Funktionen für Ereignisaufnahme und -verteilung, aber keine zusätzlichen Vorteile des Plans "Enterprise" benötigen. Der Plan "Standard" bietet gemeinsamen Zugriff auf einen {{site.data.keyword.messagehub}}-Multi-Tenant-Cluster.

## Plan "Enterprise" 

Der Plan "Enterprise" ist geeignet, wenn Datenisolation, garantierte Leistung und längere Aufbewahrungsdauer wichtige Aspekte sind. Der Plan "Enterprise" bietet exklusiven Zugriff auf einen dedizierten {{site.data.keyword.messagehub}}-Cluster.

## Was wird von den Plänen "Standard" und "Enterprise" jeweils unterstützt?

In der folgenden Tabelle ist zusammengefasst, was von den beiden Plänen jeweils unterstützt wird:

<table>
    <caption>Tabelle 1. Unterstützung in den Plänen "Standard" und "Enterprise"</caption>
      <tr>
	        <th></th>
		    <th>Plan "Standard"</th>
		    <th>Plan "Enterprise"</th>
        </tr>
		<tr>
			<td>**Tenancy**</td>
			<td>Multi-Tenant-Lösungen </td>
			<td>Single-Tenant-Lösungen</td>
		</tr>
        <tr>
			<td>**Verfügbarkeitszonen**</td>
			<td>Nicht unterstützt</td>
			<td>3</td>
		</tr>
	  		<tr>
			<td>**Kafka-Version in Cluster**</td>
			<td>Kafka 0.10.2</td>
			<td>Kafka 1.1</td>
		</tr>
		<tr>
			<td>**Differenzierte Zugriffssteuerung**</td>
			<td>Nein</td>
			<td>Ja</td>
		</tr>
		<tr>
			<td>**Kafka Connect und Kafka Streams unterstützt **</td>
			<td>Ja</td>
			<td>Ja</td>
		</tr>
		<tr>
			<td>**Maximale Anzahl von Partitionen**</td>
			<td>100</td>
			<td>1000</td>
		</tr>
		<tr>
			<td>**Maximale Aufbewahrungsdauer**</td>
			<td>1 GB pro Partition bis zu 30 Tage </td>
			<td>Unbegrenzt bis zur Speichergrenze Ihres Plans </td>
		</tr>
		<tr>
			<td>**Verfügbarkeit in Region**</td>
			<td>Vereinigte Staaten (Süden)</br>
			Großbritannien</br>
			Sydney</br>
			Deutschland (keine MQ Light-API)</td>
			<td>Vereinigte Staaten (Süden)</br>
			Vereinigte Staaten (Osten)<br/>
			Deutschland<br/>
			<br/>
			</td>
		</tr>
		<tr>
     	    <td>**Unterstützte APIs**</td>
			<td>Kafka-API</br>
			REST-konforme Verwaltungs-API<br/>
			Kafka-REST-API</br>
			MQ Light-API</br>
		    </td>
			<td>Kafka-API<br/>
			REST-konforme Verwaltungs-API</td>
		</tr>
			<td>**Cloud Object Storage-Bridge und<br/>
			MQ-Bridge unterstützt**</td>
			<td>Ja</td>
			<td>Nein</td>
		</tr>
		<tr>
			<td>**Bereitstellungszeitrahmen**</td>
			<td>Sofortige Bereitstellung</td>
			<td>Bereitstellung innerhalb von 3 Stunden</td>
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

