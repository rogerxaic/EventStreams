---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-30"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.mql}}-API in {{site.data.keyword.messagehub}} aktivieren
{: #mql_enable}

** Die MQ Light-API ist nur als Bestandteil des Plans "Standard" verfügbar.**
<br/>

**Sie müssen explizit ein Kafka-Topic mit dem Namen "MQLight" erstellen, damit die API verwendet werden kann, da alle Nachrichten das Topic "MQLight" durchlaufen. Dieses Topic muss eine einzige Partition enthalten. Durch das Erstellen dieses Topics wird die MQ Light-API für Ihre Serviceinstanz aktiviert.**  

Das Topic "MQLight" wird von der MQ Light-API  für die Speicherung der zugehörigen Nachrichtendaten und für die Interaktion mit anderen Kafka-Clients verwendet. Beachten Sie, dass ab der Erstellung dieses Topics
Gebühren gemäß dem im Zahlungsplan für Services angegebenen Standardsatz fällig werden.

Um die MQ Light-API zu inaktivieren, löschen Sie das Topic "MQLight". Beim Löschen des Topics werden auch alle zugehörigen Daten gelöscht.
