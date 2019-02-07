---

copyright:
  years: 2015, 2019
lastupdated: "2018-05-30"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

<!-- 15/11/18: info moved to eventstreams075.md, moved because of doc app changes -->
# Maximalwerte
{: #max_limits}

** Die MQ Light-API ist nur als Bestandteil des Plans "Standard" verfügbar.**
<br/>

Die folgenden Grenzwerte werden für die {{site.data.keyword.mql}}-API erzwungen:
{:shortdesc}

* Das maximale Datenvolumen, das gespeichert werden kann, entspricht einer einzelnen Kafka-Partition in Ihrem Zahlungsplan (in der Regel 1 GB). Bei Überschreitung dieses Grenzwerts werden die ältesten Nachrichten in der Partition entfernt, wenn neue Nachrichten gesendet werden.
* Die maximale Speicherungsdauer für eine Nachricht entspricht einer einzelnen Kafka-Partition in Ihrem Zahlungsplan (in der Regel 24 Stunden). Sie können keine Nachrichten abrufen, die älter sind als diese Speicherungsdauer.
* Die maximale Größe für eine Nachricht (ohne Header) ist 1 MB.
* Die maximale Anzahl von Clients, die gleichzeitig verbunden sein können, ist 25.
* Die maximale Anzahl von Zielen, die gleichzeitig aktiv sein können, ist 25. Die Definition für ein aktives Ziel lautet wie folgt:
  - Ein Ziel mit einem Wert 'TimeToLive > 0', unabhängig davon, ob gegenwärtig ein Client verbunden ist oder nicht.
  - Ein Ziel mit einem Wert 'TimeToLive = 0' (Standardwert), wenn ein Client verbunden ist. 
  <p>Ein von Clients gemeinsam genutztes Ziel wird als einzelnes Ziel eingestuft.</p>
