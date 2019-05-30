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

# Choix d'un plan 
{: #plan_choose}

{{site.data.keyword.messagehub}} est disponible dans le cadre de différents plans suivant vos besoins : Standard, Enterprise et Classic. 

<!--
For information about the Classic plan, see
[Classic plan](/docs/services/EventStreams?topic=eventstreams-plan_choose_classic#plan_choose_classic).
-->
{: shortdesc}

## Plan Standard

Le plan Standard est approprié si vous avez besoin de fonctions de distribution et d'ingestion mais d'aucun des avantages supplémentaires offerts par le plan Enterprise. Le plan Standard offre un accès partagé à un cluster {{site.data.keyword.messagehub}} à service partagé.

## Plan Enterprise 

Le plan Enterprise est approprié si l'isolation des données, la garantie de performance et la durée de conservation sont des facteurs importants. Le plan Enterprise offre un accès exclusif à un cluster {{site.data.keyword.messagehub}} dédié.

## Plan Classic

Le plan Classic vous donne accès à l'édition antérieure du plan Standard. Il est uniquement disponible pour les charges de travail existantes et la compatibilité avec les versions antérieures. Les nouvelles charges de travail devront être mises à disposition dans le plan Standard.


## Que prennent en charge les plans Standard, Enterprise et Classic ?

Le tableau suivant résume ce que les plans prennent en charge :

<table>
    <caption>Tableau 1. Prise en charge des plans Standard, Enterprise et Classic</caption>
      <tr>
	        <th></th>
		    <th>Plan Standard</th>
		    <th>Plan Enterprise</th>
		    <th>Plan Classic</th>
        </tr>
		<tr>
			<td>**Service**</td>
			<td>Service partagé </td>
			<td>Service exclusif</td>
			<td>Service partagé</td>
		</tr>
        <tr>
			<td>**Zones de disponibilité**</td>
			<td>3</td>
			<td>3</td>
			<td>Non pris en charge</td>
		</tr>
        <tr>
			<td>**Disponibilité**</td>
			<td>99,95 %</td>
			<td>99,95 %</td>
			<td>99,5 %</td>
		</tr>
	  		<tr>
			<td>**Version Kafka sur le cluster**</td>
			<td>Kafka 2.2</td>
			<td>Kafka 1.1 <br/>(Kafka 2.2 bientôt disponible)</td>
			<td>Kafka 1.1</td>
		</tr>
		<tr>
			<td>**Contrôle d'accès à granularité fine**</td>
			<td>Oui</td>
			<td>Oui</td>
			<td>Non</td>
		</tr>
				<tr>
			<td>**Prise en charge de Cloud Service Endpoint**</td>
			<td>Non</td>
			<td>Oui</td>
			<td>Non</td>
		</tr>
		<tr>
			<td>**Prise en charge de Kafka Connect et Kafka Streams**</td>
			<td>Oui</td>
			<td>Oui</td>
			<td>Oui</td>
		</tr>
		<tr>
			<td>**Nombre maximum de partitions**</td>
			<td>100</td>
			<td>1000</td>
			<td>100</td>
		</tr>
		<tr>
			<td>**Durée de conservation maximale**</td>
			<td>1 Go par partition jusqu'à 30 jours </td>
			<td>Illimitée jusqu'à la limite de stockage de votre plan </td>
			<td>1 Go par partition jusqu'à 30 jours </td>
		</tr>
		<tr>
			<td>**Débit maximal**</td>
			<td>1 Mo par seconde par partition (20 Mo par seconde maximum) </td>
			<td>40 Mo par seconde (avec un débit maximal de 90 Mo par seconde)</td>
			<td>1 Mo par seconde par partition</td>
		</tr>
		<tr>
			<td>**Taille maximale des messages**</td>
			<td>1 Mo</td>
			<td>1 Mo</td>
			<td>1 Mo</td>
		</tr>
		<tr>
			<td>**Disponibilité des emplacements (régions)**</td>
			<td>Dallas (us-south)</br>
 </td>
			<td>Dallas (us-south)</br>
			Washington (us-east)<br/>
			Londres (eu-gb)<br/>
			Sydney (au-syd)</br>
			Francfort (eu-de)<br/>
			Tokyo (jp-tok)<br/>
			<br/>
			</td>
			<td>Dallas (us-south)</br>
			Londres (eu-gb)</br>
			Sydney (au-syd)</br>
			Francfort (eu-de) - pas d'API {{site.data.keyword.mql}}  </td>
		</tr>
		<tr>
     	    <td>**API prises en charge**</td>
			<td>API Kafka</br>
			API REST d'administration<br/>
			API REST Producer</br>
		    </td>
			<td>API Kafka<br/>
			API REST d'administration</td>
			<td>API Kafka</br>
			API REST d'administration<br/>
			API REST Kafka</br>
			API MQ Light</br>
		    </td>
		</tr>
		</tr>
			<td>**Prise en charge du pont Cloud Object Storage et<br/>
			du pont MQ**</td>
			<td>Non</td>
			<td>Non</td>
			<td>Oui</td>
		</tr>
		<tr>
			<td>**Délai de déploiement**</td>
			<td>Mise à disposition instantanée</td>
			<td>La mise à disposition peut prendre jusqu'à 3 heures. Comme le plan Enterprise possède ses propres ressources dédiées pour chaque cluster, la mise à disposition est plus longue</td>
			<td>Mise à disposition instantanée</td>
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

