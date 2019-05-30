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

# Plan Classic 
{: #plan_choose_classic}

{{site.data.keyword.messagehub}} est disponible dans le cadre de différents plans, selon vos besoins.
Pour obtenir des informations sur les plans Standard et Enterprise, voir [Choix d'un plan](/docs/services/EventStreams?topic=eventstreams-plan_choose#plan_choose).
{: shortdesc}
 
## Présentation du plan Classic
Le plan Classic est approprié si vous avez besoin de fonctions de distribution et d'ingestion mais d'aucun des avantages supplémentaires offerts par le plan Enterprise. Le plan Classic offre un accès partagé à un cluster {{site.data.keyword.messagehub}} à service partagé.


## Que prend en charge le plan Classic ?

Le tableau suivant résume ce que prend en charge le plan :

<table>
    <caption>Tableau 1. Prise en charge du plan Classic</caption>
      <tr>
	        <th></th>
		    <th>Plan Classic</th>
        </tr>
		<tr>
			<td>**Service**</td>
			<td>Service partagé </td>
		</tr>
        <tr>
			<td>**Zones de disponibilité**</td>
			<td>Non pris en charge</td>
		</tr>
        <tr>
			<td>**Disponibilité**</td>
			<td>99,5 %</td>
		</tr>
	  		<tr>
			<td>**Version Kafka sur le cluster**</td>
			<td>Kafka 1.1</td>
		</tr>
		<tr>
			<td>**Contrôle d'accès à granularité fine**</td>
			<td>Non</td>
		</tr>
				<tr>
			<td>**Prise en charge de Cloud Service Endpoint**</td>
			<td>Non</td>
		</tr>
		<tr>
			<td>**Kafka Connect et Kafka Streams pris en charge **</td>
			<td>Oui</td>

		</tr>
		<tr>
			<td>**Nombre maximum de partitions**</td>
			<td>100</td>

		</tr>
		<tr>
			<td>**Durée de conservation maximale**</td>
			<td>1 Go par partition jusqu'à 30 jours </td>

		</tr>
		<tr>
			<td>**Débit maximal**</td>
			<td>1 Mo par seconde par partition</td>
		</tr>
		<tr>
			<td>**Taille maximale des messages**</td>
			<td>1 Mo</td>
		</tr>
		<tr>
			<td>**Disponibilité des emplacements (régions)**</td>
			<td>Dallas (us-south)</br>
			Londres (eu-gb)</br>
			Sydney (au-syd)</br>
			Francfort (eu-de) - pas d'API {{site.data.keyword.mql}} </td>

		</tr>
		<tr>
     	    <td>**API prises en charge**</td>
			<td>API Kafka</br>
			API REST d'administration<br/>
			API REST Kafka</br>
			API MQ Light</br>
		    </td>
		</tr>
			<td>**Prise en charge du pont Cloud Object Storage et<br/>
			du pont MQ**</td>
			<td>Oui</td>
		</tr>
		<tr>
			<td>**Délai de déploiement**</td>
			<td>Mise à disposition instantanée</td>
		</tr>

</table>

