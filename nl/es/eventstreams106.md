---

copyright:
  years: 2015, 2019
lastupdated: "2018-10-29"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# ¿Qué es la API MQ Light y cuáles son sus particularidades?
{: #mqlight}

<!-- 30/10/18: info moved to eventstreams075.md because of doc app changes -->
** La API MQ Light solo está disponible como parte del plan Estándar.**
<br/>

La API {{site.data.keyword.mql}} proporciona un nivel superior de abstracción para mensajería que la API Kafka.
{:shortdesc}

La elección entre el uso de un cliente Kafka o la API {{site.data.keyword.mql}} depende de la topología de mensajería que desee crear:

* Con Kafka, se utiliza un número reducido de temas y puede haber varias particiones para cada tema para obtener mayor escalabilidad. Puede compartir mensajes entre consumidores utilizando grupos de consumidores, pero cada consumidor debe poder mantener la tasa de mensajes de las particiones asignadas.
* Con la API {{site.data.keyword.mql}}, puede utilizar un número mucho mayor de temas y los nombres de temas son jerárquicos (por ejemplo: <code>'/sports/football'</code> y <code>'/sports/tiddlywinks'</code>). 

Los temas de la API {{site.data.keyword.mql}} no son los mismos que los temas de Kafka. En su lugar, la API {{site.data.keyword.mql}} utiliza un tema único denominado "MQLight Kafka" y todos los mensajes enviados y recibidos utilizando la API {{site.data.keyword.mql}} pasan por ese tema de Kafka concreto.

{{site.data.keyword.mql}} está disponible solo en las regiones de {{site.data.keyword.Bluemix_notm}}: EE.UU. sur (Dallas), RU sur (Londres) y AP sur (Sídney). La API de MQ Light no está disponible en la ubicación UE central (Frankfurt) o en {{site.data.keyword.Bluemix_notm}} dedicado.

<!-- begin STAGING ONLY -->
Para obtener más información sobre qué API elegir, consulte [Elección entre las tres API](/docs/services/EventStreams/eventstreams087.html).
<!-- end STAGING ONLY -->

