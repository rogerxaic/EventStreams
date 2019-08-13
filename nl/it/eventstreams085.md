---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-23"

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

# Scelta del tuo piano 
{: #plan_choose}

{{site.data.keyword.messagehub}} è disponibile come piani diversi, a seconda dei tuoi requisiti: Standard, Enterprise e Classic. 

<!--
For information about the Classic plan, see
[Classic plan](/docs/services/EventStreams?topic=eventstreams-plan_choose_classic#plan_choose_classic).
-->
{: shortdesc}

## Piano Standard
{: #plan_standard}

Il piano Standard è appropriato se richiedi delle funzionalità di inserimento e distribuzione ma non richiedi alcun altro vantaggio aggiuntivo del piano Enterprise. Il piano Standard offre un accesso condiviso a un cluster {{site.data.keyword.messagehub}} a più tenant.

## Piano Enterprise 
{: #plan_enterprise}

Il piano Enterprise è appropriato se l'isolamento dei dati, delle prestazioni garantite e una conservazione aumentata sono considerazioni importanti. Il piano Enterprise offre un accesso esclusivo a un cluster {{site.data.keyword.messagehub}} dedicato. Puoi anche eseguire il provisioning di un cluster {{site.data.keyword.messagehub}} in un'ubicazione geograficamente locale ma [a zona singola (SZR)](/docs/services/EventStreams?topic=eventstreams-sla#sla_szr).

## Piano Classic
{: #plan_classic}

Il piano Classic fornisce l'accesso alla precedente edizione del piano Standard e viene fornito per i carichi di lavoro esistenti solo per la retrocompatibilità. Dovresti eseguire il provisioning dei nuovi carichi di lavoro con il piano Standard.


## Cosa è supportato dai piani Standard, Enterprise e Classic

La seguente tabella riepiloga cosa è supportato dai piani:

<table>
    <caption>Tabella 1. Supporto nei piani Standard, Enterprise e Classic</caption>
      <tr>
	        <th></th>
		    <th>Piano Standard</th>
		    <th>Piano Enterprise</th>
		    <th>Piano Classic</th>
        </tr>
		<tr>
			<td>**Tenancy**</td>
			<td>Più tenant </td>
			<td>Singolo tenant</td>
			<td>Più tenant</td>
		</tr>
        <tr>
			<td>**Zone di disponibilità**</td>
			<td>3</td>
			<td>3<br/>(1 in ubicazioni a singola zona)
			</td>
			<td>Non supportato</td>
		</tr>
        <tr>
			<td>**Disponibilità**</td>
			<td>99.95%</td>
			<td>99.95%<br/>(99.5% in ubicazioni a singola zona)  [<sup>1</sup>](/docs/services/EventStreams?topic=eventstreams-plan_choose#footnote_plans)</td>
			<td>99.5%</td>
		</tr>
	  		<tr>
			<td>**Versione Kafka sul cluster **</td>
			<td>Kafka 2.2</td>
			<td>Kafka 1.1 <br/>(Kafka 2.2 presto disponibile)</td>
			<td>Kafka 1.1</td>
		</tr>
		<tr>
			<td>**Controllo dell'accesso dettagliato**</td>
			<td>Sì</td>
			<td>Sì</td>
			<td>No</td>
		</tr>
				<tr>
			<td>**Supporto endpoint servizio cloud**</td>
			<td>No</td>
			<td>Sì</td>
			<td>No</td>
		</tr>
		<tr>
			<td>**Kafka Connect e Kafka Streams supportati **</td>
			<td>Sì</td>
			<td>Sì</td>
			<td>Sì</td>
		</tr>
		<tr>
			<td>**Numero massimo di partizioni**</td>
			<td>100</td>
			<td>1000</td>
			<td>100</td>
		</tr>
		<tr>
			<td>**Periodo di conservazione massimo**</td>
			<td>1 GB per partizione per un massimo di 30 giorni </td>
			<td>2 TB di archiviazione disponibile<!--Unlimited up to the storage limit of your plan --></td>
			<td>1 GB per partizione per un massimo di 30 giorni </td>
		</tr>
		<tr>
			<td>**Velocità massima di trasmissione**</td>
			<td>1 MB al secondo per partizione (al massimo 20 MB al secondo) </td>
			<td>40 MB al secondo per cluster (velocità effettiva di picco di 75 MB al secondo)</td>
			<td>1 MB al secondo per partizione</td>
		</tr>
		<tr>
			<td>**Dimensione massima messaggio**</td>
			<td>1 MB</td>
			<td>1 MB</td>
			<td>1 MB</td>
		</tr>
		<tr>
			<td>**Numero massimo di client connessi**</td>
			<td>100</td>
			<td>10 000</td>
			<td>100</td>
		</tr>
		<tr>
			<td>**Disponibilità ubicazione (regione)**</td>
			<td>**Ubicazione multizona (MZR)**<br/>
			Dallas (us-south)</br>
			Washington (us-east)<br/>
			Londra (eu-gb)<br/>
			Sydney (au-syd)</br>
			Francoforte (eu-de)<br/>
			Tokyo (jp-tok)<br/>
			<br/>
			</td>
			<td>**Ubicazione multizona (MZR)**</br>
			Dallas (us-south)</br>
			Washington (us-east)<br/>
			Londra (eu-gb)<br/>
			Sydney (au-syd)</br>
			Francoforte (eu-de)<br/>
			Tokyo (jp-tok)<br/>
			<br/>
			**Ubicazione singola zona (SZR)**</br>
			Seoul (seo01)<br/>
			<br/>
			</td>
			<td>Dallas (us-south)</br>
			Londra (eu-gb)</br>
			Sydney (au-syd)</br>
			Francoforte (eu-de) - nessuna API {{site.data.keyword.mql}} </td>
		</tr>
		<tr>
     	    <td>**API supportate**</td>
			<td>API Kafka</br>
			API REST Admin<br/>
			API REST Producer</br>
		    </td>
			<td>API Kafka<br/>
			API REST Admin</br>
			API REST Producer</br>
			</td>
			<td>API Kafka</br>
			API REST Admin<br/>
			API REST Kafka</br>
			API MQ Light</br>
		    </td>
		</tr>
		</tr>
			<td>**Bridge Cloud Object Storage e<br/>
			bridge MQ supportati**</td>
			<td>No</td>
			<td>No</td>
			<td>Sì</td>
		</tr>
		<tr>
			<td>**Periodo di tempo di distribuzione**</td>
			<td>Provisioning istantaneo</td>
			<td>Il provisioning previsto può durare fino a 3 ore. Poiché il piano Enterprise dispone di proprie risorse dedicate per ogni cluster, richiede più tempo per il provisioning.</td>
			<td>Provisioning istantaneo</td>
		</tr>

</table>
### Nota a piè di pagina
{: #footnote_plans notoc}

1. {: #footnote_szr notoc} Per ulteriori informazioni sulla disponibilità, vedi [Distribuzioni a ubicazione a singola zona](/docs/services/EventStreams?topic=eventstreams-sla#sla_szr).



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

For more information, see [{{site.data.keyword.Bluemix_notm}} Public ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud/free/){:new_window}.
-->

