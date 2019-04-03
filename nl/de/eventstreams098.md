---

copyright:
  years: 2015, 2019
lastupdated: "2018-05-30"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Voraussetzungen für die Verwendung der MQ Light-API mit {{site.data.keyword.messagehub}}
{: #mql_api_reqs}

<!-- 30/10/18: info moved to eventstreams075.md because of doc app changes -->
** Die MQ Light-API ist nur als Bestandteil des Plans "Standard" verfügbar. **
<br/>

Die folgenden Voraussetzungen sind für die Verwendung der {{site.data.keyword.mql}}-API mit {{site.data.keyword.messagehub}} erforderlich: 

**Sie müssen explizit ein Kafka-Topic mit dem Namen "MQLight" erstellen, damit die API verwendet werden kann, da alle Nachrichten das Topic "MQLight" durchlaufen. Dieses Topic muss eine einzige Partition enthalten. Durch das Erstellen dieses Topics wird die MQ Light-API für Ihre Serviceinstanz aktiviert. Die in der MQ Light-API verwendeten Topics werden automatisch erstellt, sobald Sie sie verwenden. Alle Nachrichten befinden sich jedoch tatsächlich in dem einzelnen Kafka-Topic "MQLight". ** 
{: shortdesc}

Das Topic "MQLight" wird von der MQ Light-API für die Speicherung der zugehörigen Nachrichtendaten und für die Interaktion mit anderen Kafka-Clients verwendet. Beachten Sie, dass ab der Erstellung dieses Topics
Gebühren gemäß dem im Zahlungsplan für Services angegebenen Standardsatz fällig werden.

Um die MQ Light-API zu inaktivieren, löschen Sie das Topic "MQLight". Beim Löschen des Topics werden auch alle zugehörigen Daten gelöscht.
