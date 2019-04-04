---

copyright:
  years: 2015, 2019
lastupdated: "2018-05-25"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

<!-- 15/11/18: info moved to eventstreams075.md, moved because of doc app changes -->
# Echange de messages entre l'API MQ Light et les API Kafka ou REST Kafka
{: #mql_exchange_rest}

**L'API MQ Light est disponible uniquement dans le cadre du plan Standard.**
<br/>

Les messages {{site.data.keyword.mql}} sont stockés dans un sujet Kafka sous-jacent unique nommé "MQLight" et sont codés comme indiqué dans le tableau suivant. Ce codage peut aussi être utilisé par d'autres types d'API, comme Kafka or REST Kafka, pour l'échange de messages avec des applications utilisant l'API {{site.data.keyword.mql}}.
{: shortdesc}

## Format de message Kafka
{: #kafka_message_format notoc}

<table border='1'>
<caption>Tableau 1. Format de message Kafka</caption>
  <tr>
    <th> Clé </th>
    <th> Valeur </th>
  </tr>
  <tr>
    <td> Facultative (non utilisée par l'API)
	<p></p>
	</td>
    <td>**1 octet**
	<p>		     Identificateur de l'API MQ Light, qui est toujours 0xFA.</p>
    <p><var class="keyword varname">**n**</var> **octets**</p>
    <p>		    Message codé AMQP (reposant sur le format Wire AMQP). </p></td>
  </tr>
</table>


