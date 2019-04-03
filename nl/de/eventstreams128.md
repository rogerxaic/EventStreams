---

copyright:
  years: 2015, 2019
lastupdated: "2018-07-05"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Verwaltungs-API von Kafka-Java-Client verwenden
{: #kafka_java_api}


<!-- 
17/10/17 - Karen: following info duplicated at messagehub108
 -->

Wenn Sie einen Kafka-Client der Version 0.11 oder höher verwenden oder Kafka Streams Version 0.10.2.0 oder höher, können Sie APIs verwenden, um Topics zu erstellen oder zu löschen. Für die zulässigen Einstellungen beim Erstellen von Topics gelten bestimmte Einschränkungen. Gegenwärtig können Sie nur die folgenden Einstellungen ändern:
{: shortdesc}

<dl>
<dt>cleanup.policy</dt>
<dd>Zulässige Werte sind <code>delete</code> (Standardwert), <code>compact</code> oder <code>delete,compact</code>
<p>**Anmerkung:**
Wenn für die Bereinigungsrichtlinie (cleanup.policy) nur der Wert <code>compact</code> angegeben ist, wird automatisch der Wert <code>delete</code> hinzugefügt, aber die Löschfunktion auf Zeitbasis inaktiviert. Die Nachrichten in dem Topic werden bis 1 GB komprimiert. Die Löschfunktion wird erst beim Überschreiten dieses Werts aktiviert.</p>
</dd>

<dt>retention.ms</dt>
<dd>Der Standardaufbewahrungszeitraum ist 24 Stunden. Der Mindestwert ist 1 Stunde und der Höchstwert ist
30 Tage. Geben Sie den Wert in Stunden an.

<p>**Anmerkung:**
Im Plan "Enterprise" können Sie für diese Einstellung einen beliebigen Wert festlegen.</p>
</dd>

<dt>retention.bytes</dt>
<dd>Die maximale Größe, auf die eine Partition (die aus Protokollsegmenten besteht) anwachsen kann, bis alte Protokollsegmente gelöscht werden, um Speicherbereich freizugeben.

<p>**Anmerkung:**
Nur für Plan "Enterprise". Auf einen beliebigen Wert größer als 1 MB festlegen.</p>
</dd>

<dt>segment.bytes</dt>
<dd>Die Segmentdateigröße für das Protokoll.

<p>**Anmerkung:**
Nur für Plan "Enterprise". Auf einen beliebigen Wert größer als 100 kB festlegen.</p>
</dd>

<dt>segment.index.bytes</dt>
<dd>Die Größe des Index, der Offsets Dateipositionen zuordnet. 

<p>**Anmerkung:**
Nur für Plan "Enterprise". Auf einen beliebigen Wert zwischen 100 kB und 2 GB festlegen.</p>
</dd>

<dt>segment.ms</dt>
<dd>Der Zeitraum, nach dem Kafka das Blättern des Protokolls erzwingt, auch wenn das Segment nicht voll ist. 

<p>**Anmerkung:**
Nur für Plan "Enterprise". Auf einen beliebigen Wert zwischen 5 Minuten und 30 Tagen festlegen.</p>
</dd>
</dl>

