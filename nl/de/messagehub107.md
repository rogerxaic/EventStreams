---

copyright:
  years: 2015, 2018
lastupdated: "2017-09-26"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Was ist Message Hub?

{{site.data.keyword.messagehub}} implementiert das Publish/Subscribe-Messaging
unter Verwendung von Topics. Im Unterschied zu anderen Messaging-Systemen arbeitet {{site.data.keyword.messagehub}} mit Topics und nicht mit Warteschlangen. Darüber hinaus bietet {{site.data.keyword.messagehub}}
zusätzliche Funktionen wie Bridges für andere Systeme.

{{site.data.keyword.messagehub}} basiert auf dem Open-Source-Projekt Apache Kafka,
einem hoch skalierbaren und leistungsstarken Messaging-Backbone, das sich bereits in zahlreichen
Produktionsumgebungen bewährt hat. Weitere Informationen finden Sie in [{{site.data.keyword.messagehub}} und Apache Kafka](/docs/services/MessageHub/messagehub073.html).
Apache Kafka-Tools arbeiten normalerweise direkt mit {{site.data.keyword.messagehub}}, es ist jedoch zusätzliche Konfiguration erforderlich, da bei Verbindungen zu {{site.data.keyword.messagehub}} stets Berechtigungsnachweise zur Authentifizierung verwendet werden.

{{site.data.keyword.messagehub}} stellt drei APIs bereit: die Kafka-API, die Kafka-REST-API und die {{site.data.keyword.mql}}-API.
In den meisten Fällen ist die Kafka-API die am besten geeignete API. Weitere Informationen zum Erstellen von Messaging-Anwendungen mithilfe der APIs finden Sie in
[Kafka-Client verwenden](/docs/services/MessageHub/messagehub050.html), [Kafka-REST-API verwenden](/docs/services/MessageHub/messagehub025.html) und [MQLight-API-Client verwenden](/docs/services/MessageHub/messagehub075.html).

Außerdem unterstützt {{site.data.keyword.messagehub}} Bridges für
mehrere andere Systeme. Eine Bridge ist eine unidirektionale Verbindung zu einem anderen
System. Eine Bridge kann Nachrichten von dem anderen System entgegennehmen und in einem Topic veröffentlichen,
oder Nachrichten von einem Topic verarbeiten und an das andere System senden. Auf diese Weise können Sie
{{site.data.keyword.messagehub}} für die Integration mit anderen Systemen verwenden, ohne Code zu
schreiben. Weitere Informationen finden Sie in [Über Bridges mit anderen Services verbinden](/docs/services/MessageHub/messagehub088.html).
