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

# Gründe für die Verwendung der {{site.data.keyword.mql}}-API
{: #why_mql}

** Die MQ Light-API ist nur als Bestandteil des Plans "Standard" verfügbar.**
<br/>

Die {{site.data.keyword.mql}}-API stellt eine höhere Abstraktionsebene
bereit als die Kafka-API. {{site.data.keyword.mql}} ermöglicht die Erstellung portierbarer Apps in einem Unified Messaging-Modell, das sowohl Messaging-Systeme mit Warteschlangen als auch mit Publish/Subscribe- unterstützt.
{:shortdesc}

Für den Austausch von Nachrichten zwischen Apps werden dynamisch erstellte Ziele verwendet, die
hierarchisch strukturiert (z. B. <code>‘/sport/fussball’</code>) und durch Platzhalter gruppiert werden können (z. B.
<code>‘/sports/#’</code>). Außerdem können Steuerelemente für Bereitstellungszusicherung und Nachrichtenablauf verwendet werden.
Mit diesen Funktionen können Sie Szenarios wie das Auslagern von Verarbeitungsprozessen sowie Ereignisaufzeichnung und Stapelverarbeitung
implementieren.

Neben dem Senden von Nachrichten zwischen anderen Apps kann die {{site.data.keyword.mql}}-API auch für den Nachrichtenaustausch mit Apps genutzt werden, die Kafka-REST oder Kafka-APIs verwenden. Alternativ können Sie Apps auch lokal mit {{site.data.keyword.IBM}} MQ verwenden.

