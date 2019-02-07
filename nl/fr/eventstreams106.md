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

# Présentation de l'API MQ Light et de ses particularités
{: #mqlight}

<!-- 30/10/18: info moved to eventstreams075.md because of doc app changes -->
** L'API MQ Light est uniquement disponible dans le cadre du plan Standard.**
<br/>

L'API {{site.data.keyword.mql}} offre un niveau plus élevé d'abstraction à la messagerie que Kafka.
{:shortdesc}

Le choix d'un client Kafka ou de l'API {{site.data.keyword.mql}} dépend de la topologie de messagerie que vous souhaitez construire :

* Avec Kafka, vous utilisez un petit nombre de sujets et vous pouvez disposer de plusieurs partitions pour chaque sujet afin d'accroître l'évolutivité. Vous pouvez partager des messages entre consommateurs grâce aux groupes de consommateurs, mais chaque consommateur doit pouvoir suivre le débit des messages pour les partitions qui lui sont affectés.
* Avec l'API {{site.data.keyword.mql}}, vous pouvez utiliser un nombre de sujets bien plus élevé et les noms de sujet sont hiérarchiques (par exemple : <code>‘/sports/football’</code> et <code>‘/sports/tiddlywinks’</code>). 

Les sujets de l'API {{site.data.keyword.mql}} ne sont pas les mêmes que les sujets Kafka. L'API {{site.data.keyword.mql}} utilise un seul sujet Kafka nommé "MQLight" et tous les messages envoyés et reçus via l'API {{site.data.keyword.mql}} transitent par ce sujet Kafka.

L'API {{site.data.keyword.mql}} est uniquement disponible dans les régions {{site.data.keyword.Bluemix_notm}} suivantes : Sud des Etats-Unis (Dallas), Sud du Royaume-Uni (Londres) et Asie-Pacifique sud (Sydney). L'API MQ Light n'est pas disponible dans la région Centre de l'UE (Francfort) ni dans {{site.data.keyword.Bluemix_notm}} Dedicated.

<!-- begin STAGING ONLY -->
Pour plus d'informations sur le choix entre les API, voir [Choix entre les trois API](/docs/services/EventStreams/eventstreams087.html).
<!-- end STAGING ONLY -->

