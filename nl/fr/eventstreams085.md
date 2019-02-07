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

# Choix d'un plan 
{: #plan_choose}

{{site.data.keyword.messagehub}} est disponible en deux plans différents selon vos besoins : Standard et Enterprise.
{: shortdesc}

## Plan Standard

Le plan Standard est approprié si vous avez besoin de fonctions de distribution et d'ingestion mais d'aucun des avantages supplémentaires offerts par le plan Enterprise. Le plan Standard offre un accès partagé à un cluster {{site.data.keyword.messagehub}} à service partagé.

## Plan Enterprise 

Le plan Enterprise est approprié si l'isolement des données, la garantie des performances et une grande durée de conservation sont des considérations importantes. Le plan Enterprise offre un accès exclusif à un cluster {{site.data.keyword.messagehub}} dédié.

## Eléments pris en charge par les plans Standard et Enterprise

Le tableau suivant résume les éléments que les plans prennent en charge :

<table>
    <caption>Tableau 1. Prise en charge dans les plans Standard et Enterprise</caption>
      <tr>
	        <th></th>
		    <th>Plan Standard</th>
		    <th>Plan Enterprise</th>
        </tr>
		<tr>
			<td>**Service**</td>
			<td>A service partagé </td>
			<td>A service exclusif</td>
		</tr>
        <tr>
			<td>**Zones de disponibilité**</td>
			<td>Non pris en charge</td>
			<td>3</td>
		</tr>
	  		<tr>
			<td>**Version Kafka sur le cluster**</td>
			<td>Kafka 1.1</td>
			<td>Kafka 1.1</td>
		</tr>
		<tr>
			<td>**Contrôle d'accès à granularité fine**</td>
			<td>Non</td>
			<td>Oui</td>
		</tr>
		<tr>
			<td>**Kafka Connect et Kafka Streams pris en charge **</td>
			<td>Oui</td>
			<td>Oui</td>
		</tr>
		<tr>
			<td>**Nombre maximum de partitions**</td>
			<td>100</td>
			<td>1000</td>
		</tr>
		<tr>
			<td>**Durée de conservation maximale**</td>
			<td>1 Go par partition jusqu'à 30 jours </td>
			<td>Illimité jusqu'à la limite de stockage de votre plan </td>
		</tr>
		<tr>
			<td>**Disponibilité des emplacements (régions)**</td>
			<td>Dallas (us-south)</br>
			Londres (eu-gb)</br>
			Sydney (au-syd))</br>
			Francfort (eu-de) - sauf API {{site.data.keyword.mql}} </td>
			<td>Dallas (us-south)</br>
			Washington (us-east))<br/>
			Londres (eu-gb)<br/>
			Francfort (eu-de)<br/>
			Tokyo (jp-tok)<br/>

			<br/>
			</td>
		</tr>
		<tr>
     	    <td>**API prises en charge**</td>
			<td>API Kafka</br>
			API Admin REST<br/>
			API REST Kafka</br>
			API MQ Light</br>
		    </td>
			<td>API Kafka<br/>
			API Admin REST</td>
		</tr>
			<td>**Pont Cloud Object Storage et<br/>
			pont MQ pris en charge**</td>
			<td>Oui</td>
			<td>Non</td>
		</tr>
		<tr>
			<td>**Délai de déploiement**</td>
			<td>Mise à disposition instantanée</td>
			<td>La mise à disposition peut prendre jusqu'à 3 heures</td>
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

