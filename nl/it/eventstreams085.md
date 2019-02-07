---

copyright:
  years: 2015, 2019
lastupdated: "2018-11-28"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# Scelta del tuo piano 
{: #plan_choose}

{{site.data.keyword.messagehub}} è disponibile come due piani diversi, a seconda dei tuoi requisiti: Standard e Enterprise.
{: shortdesc}

## Piano Standard

Il piano Standard è appropriato se richiedi delle funzionalità di inserimento e distribuzione ma non richiedi alcun altro vantaggio aggiuntivo del piano Enterprise. Il piano Standard offre un accesso condiviso a un cluster {{site.data.keyword.messagehub}} a più tenant.

## Piano Enterprise 

Il piano Enterprise è appropriato se l'isolamento dei dati, delle prestazioni garantite e una conservazione aumentata sono considerazioni importanti. Il piano Enterprise offre un accesso esclusivo a un cluster {{site.data.keyword.messagehub}} dedicato.

## Cosa è supportato dai piani Standard ed Enterprise

La seguente tabella riepiloga cosa è supportato dai piani:

<table>
    <caption>Tabella 1. Supporto nei piani Standard ed Enterprise</caption>
      <tr>
	        <th></th>
		    <th>Piano Standard</th>
		    <th>Piano Enterprise</th>
        </tr>
		<tr>
			<td>**Tenancy**</td>
			<td>Più tenant </td>
			<td>Singolo tenant</td>
		</tr>
        <tr>
			<td>**Zone di disponibilità**</td>
			<td>Non supportato</td>
			<td>3</td>
		</tr>
	  		<tr>
			<td>**Versione Kafka sul cluster **</td>
			<td>Kafka 1.1</td>
			<td>Kafka 1.1</td>
		</tr>
		<tr>
			<td>**Controllo dell'accesso dettagliato**</td>
			<td>No</td>
			<td>Sì</td>
		</tr>
		<tr>
			<td>**Kafka Connect e Kafka Streams supportati **</td>
			<td>Sì</td>
			<td>Sì</td>
		</tr>
		<tr>
			<td>**Numero massimo di partizioni**</td>
			<td>100</td>
			<td>1000</td>
		</tr>
		<tr>
			<td>**Periodo di conservazione massimo**</td>
			<td>1 GB per partizione per un massimo di 30 giorni </td>
			<td>Illimitato fino al limite di archiviazione del tuo piano </td>
		</tr>
		<tr>
			<td>**Disponibilità ubicazione (regione)**</td>
			<td>Dallas (us-south)</br>
			Londra (eu-gb)</br>
			Sydney (au-syd))</br>
			Francoforte (eu-de) - nessuna API {{site.data.keyword.mql}} </td>
			<td>Dallas (us-south)</br>
			Washington (us-east))<br/>
			Londra (eu-gb)<br/>
			Francoforte (eu-de)<br/>
			Tokyo (jp-tok)<br/>

			<br/>
			</td>
		</tr>
		<tr>
     	    <td>**API supportate**</td>
			<td>API Kafka</br>
			API REST Admin<br/>
			API REST Kafka</br>
			API MQ Light</br>
		    </td>
			<td>API Kafka<br/>
			API REST Admin</td>
		</tr>
			<td>**Bridge Cloud Object Storage e<br/>
			bridge MQ supportati**</td>
			<td>Sì</td>
			<td>No</td>
		</tr>
		<tr>
			<td>**Periodo di tempo di distribuzione**</td>
			<td>Provisioning istantaneo</td>
			<td>Il provisioning previsto può durare fino a 3 ore</td>
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

