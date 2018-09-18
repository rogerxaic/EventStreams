---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Merkmale der MQ Light-API
{: #mqlight}

** Die MQ Light-API ist nur als Bestandteil des Plans "Standard" verfügbar.**
<br/>

Die {{site.data.keyword.mql}}-API stellt eine höhere Abstraktionsebene für das Messaging bereit als Kafka.
{:shortdesc}

Das Auswahlkriterium für die Verwendung eines Kafka-Clients oder der {{site.data.keyword.mql}}-API ist
die zu erstellende Messaging-Topologie:

* Mit Kafka erstellen Sie eine kleine Anzahl von Topics, die eine Vielzahl von Partitionen für jedes Topic enthalten können, um eine hohe Skalierbarkeit zu erzielen. Mithilfe von Consumergruppen können Consumer Nachrichten zwar gemeinsam nutzen, doch jeder Consumer muss darauf achten, dass die zugeordnete Nachrichtenrate auch tatsächlich verarbeitet werden kann.
* Mit der {{site.data.keyword.mql}}-API können Sie eine große Anzahl von Topics mit hierarchisch strukturierten Topicnamen verwenden (z. B. <code>‘/sports/football’</code> und <code>‘/sports/tiddlywinks’</code>). 

Die Topics in der {{site.data.keyword.mql}}-API unterscheiden sich von
den Kafka-Topics. In der {{site.data.keyword.mql}}-API wird ein einziges Kafka-Topic
mit der Bezeichnung "MQLight" verwendet. Alle über die {{site.data.keyword.mql}}-API gesendeten
und empfangenen Nachrichten durchlaufen dieses eine Kafka-Topic.

Die {{site.data.keyword.mql}}-API ist nur in den folgenden
{{site.data.keyword.Bluemix_notm}}-Regionen verfügbar: 'Vereinigte Staaten (Süden)', 'Vereintes Königreich' und 'Sydney'. Die MQ Light-API ist weder in der Region 'Deutschland' noch in
{{site.data.keyword.Bluemix_notm}} Dedicated verfügbar.

<!-- begin STAGING ONLY -->
Weitere Informationen zur Auswahl zwischen den APIs finden Sie im Abschnitt [Geeignete API auswählen](/docs/services/MessageHub/messagehub087.html).
<!-- end STAGING ONLY -->

