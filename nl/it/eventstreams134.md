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

# Il piano Classic 
{: #plan_choose_classic}

{{site.data.keyword.messagehub}} è disponibile come piani diversi, a seconda dei tuoi requisiti. Per informazioni sui piani Standard e Enterprise, vedi [Scelta del tuo piano](/docs/services/EventStreams?topic=eventstreams-plan_choose#plan_choose).
{: shortdesc}
 
## Panoramica del piano Classic
Il piano Classic è appropriato se richiedi delle funzionalità di inserimento e distribuzione ma non richiedi alcun altro vantaggio aggiuntivo del piano Enterprise. Il piano Classic offre un accesso condiviso a un cluster {{site.data.keyword.messagehub}} a più tenant.


## Cosa è supportato dal piano Classic

La seguente tabella riepiloga cosa è supportato dal piano:

<table>
    <caption>Tabella 1. Supporto nel piano Classic</caption>
      <tr>
	        <th></th>
		    <th>Piano Classic</th>
        </tr>
		<tr>
			<td>**Tenancy**</td>
			<td>Più tenant </td>
		</tr>
        <tr>
			<td>**Zone di disponibilità**</td>
			<td>Non supportato</td>
		</tr>
        <tr>
			<td>**Disponibilità**</td>
			<td>99.5%</td>
		</tr>
	  		<tr>
			<td>**Versione Kafka sul cluster **</td>
			<td>Kafka 1.1</td>
		</tr>
		<tr>
			<td>**Controllo dell'accesso dettagliato**</td>
			<td>No</td>
		</tr>
				<tr>
			<td>**Supporto endpoint servizio cloud**</td>
			<td>No</td>
		</tr>
		<tr>
			<td>**Kafka Connect e Kafka Streams supportati **</td>
			<td>Sì</td>

		</tr>
		<tr>
			<td>**Numero massimo di partizioni**</td>
			<td>100</td>

		</tr>
		<tr>
			<td>**Periodo di conservazione massimo**</td>
			<td>1 GB per partizione per un massimo di 30 giorni </td>

		</tr>
		<tr>
			<td>**Velocità massima di trasmissione**</td>
			<td>1 MB al secondo per partizione</td>
		</tr>
		<tr>
			<td>**Dimensione massima messaggio**</td>
			<td>1 MB</td>
		</tr>
		<tr>
			<td>**Disponibilità ubicazione (regione)**</td>
			<td>Dallas (us-south)</br>
			Londra (eu-gb)</br>
			Sydney (au-syd)</br>
			Francoforte (eu-de) - nessuna API {{site.data.keyword.mql}} </td>

		</tr>
		<tr>
     	    <td>**API supportate**</td>
			<td>API Kafka</br>
			API REST Admin<br/>
			API REST Kafka</br>
			API MQ Light</br>
		    </td>
		</tr>
			<td>**Bridge Cloud Object Storage e<br/>
			bridge MQ supportati**</td>
			<td>Sì</td>
		</tr>
		<tr>
			<td>**Periodo di tempo di distribuzione**</td>
			<td>Provisioning istantaneo</td>
		</tr>

</table>

