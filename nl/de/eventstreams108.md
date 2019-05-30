---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-13"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:faq: data-hd-content-type='faq'}

# Häufig gestellte Fragen
{: #faqs}

Antworten auf häufig gestellte Fragen zum {{site.data.keyword.IBM}} {{site.data.keyword.messagehub}}-Service.

Antworten auf Fragen speziell zum Plan "Classic" finden Sie in [Häufig gestellte Fragen zum Plan "Classic"](/docs/services/EventStreams?topic=eventstreams-faqs_classic).
{: shortdesc}

<!--17/10/17 - Karen: same info duplicated at messagehub104 -->
## Wie können mit Kafka-APIs Topics erstellt und gelöscht werden?
{: #topic_admin}
{: faq}

Wenn Sie einen Kafka-Client der Version 0.11 oder höher verwenden oder Kafka Streams Version 0.10.2.0 oder höher, können Sie APIs verwenden, um Topics zu erstellen oder zu löschen. Für die zulässigen Einstellungen beim Erstellen von Topics gelten bestimmte Einschränkungen. Gegenwärtig können Sie nur die folgenden Einstellungen ändern:

<dl>
<dt>cleanup.policy</dt>
<dd>Zulässige Werte sind <code>delete</code> (Standardwert), <code>compact</code> oder <code>delete,compact</code>
<p>**Anmerkung:**
Wenn für die Bereinigungsrichtlinie (cleanup.policy) nur der Wert <code>compact</code> angegeben ist, wird automatisch der Wert <code>delete</code> hinzugefügt, aber die Löschfunktion auf Zeitbasis inaktiviert. Die Nachrichten in dem Topic werden bis 1 GB komprimiert. Die Löschfunktion wird
erst beim Überschreiten dieses Werts aktiviert.</p>
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


## Welchen Zeitraum legt {{site.data.keyword.messagehub}} für das Protokollspeicherungsfenster für das Consumer-Offsets-Topic fest?
{: #offsets }
{: faq}

{{site.data.keyword.messagehub}} speichert Consumer-Offsets für einen Zeitraum von 7 Tagen. Dies entspricht der Kafka-Konfiguration 'offsets.retention.minutes'. 

Die Offset-Aufbewahrungsdauer gilt systemweit und kann nicht für eine einzelne Topicebene festgelegt werden. Für alle Consumergruppen werden nur gespeicherte Offsets für einen Zeitraum von 7 Tagen bereitgestellt, selbst wenn die Protokollspeicherungsdauer für das zugehörige Topic auf den Maximalwert von 30 Tagen erhöht wurde. 

Das interne Kafka-Topic <code>__consumer_offsets</code> ist für Sie schreibgeschützt sichtbar.
Es wird dringend empfohlen, keinen Versuch zu unternehmen, dieses Topic in irgendeiner Weise zu steuern. 

<!--following message retention info duplicted in eventstreams057-->

## Wie lange werden Nachrichten aufbewahrt?
{: #messages_retained}

In Kafka werden Nachrichten standardmäßig bis zu 24 Stunden aufbewahrt und jede Partition wird auf
1 GB begrenzt. Wenn die Obergrenze von 1 GB erreicht ist, werden die ältesten Nachrichten gelöscht, damit
der Grenzwert eingehalten wird.

Die Aufbewahrungsdauer für Nachrichten können Sie beim Erstellen eines Topics
in der Benutzerschnittstelle oder mit der Verwaltungs-API ändern. Das Zeitlimit muss im Bereich von 1 Stunde (Minimalwert) bis
30 Tage (Maximalwert) liegen.

Informationen zu Einschränkungen für die Einstellungen, die bei der Erstellung von Topics mit einem Kafka-Client oder Kafka Streams zulässig sind, finden Sie unter [Wie können mit Kafka-APIs Topics erstellt und gelöscht werden?](/docs/services/EventStreams?topic=eventstreams-faqs#topic_admin).

## Wie ist das Verfügbarkeitsverhalten von {{site.data.keyword.messagehub}}?
{: #availability}
{: faq}

Verwenden Sie diese Informationen beim Schreiben von {{site.data.keyword.messagehub}}-Apps, um das normale Verfügbarkeitsverhalten von {{site.data.keyword.messagehub}} und die damit verbundenen Anforderungen an das Ausführungsverhalten Ihrer Apps kennenzulernen.

### APIs
{: #api_availability }

Im regulären Betrieb von {{site.data.keyword.messagehub}} werden die Knoten der Kafka-Cluster bei Bedarf erneut gestartet.
In manchen Fällen werden Ihre Apps darauf vorbereitet, dass der Cluster Ressourcen neu zuordnet. Gestalten Sie Ihre Apps so,
dass sie auf solche Änderungen entsprechend reagieren und Verbindungen wiederherstellen bzw. Operationen wiederholen können.

## Was ist die maximale Nachrichtengröße von {{site.data.keyword.messagehub}}? 
{: #max_message_size }
{: faq}

Die maximale Nachrichtengröße von {{site.data.keyword.messagehub}} ist 1 MB. Das ist der Kafka-Standardwert. 

## Was sind die Replikationseinstellungen für {{site.data.keyword.messagehub}}? 
{: #replication }
{: faq}

{{site.data.keyword.messagehub}} ist für eine starke Verfügbarkeit und Dauerhaftigkeit konfiguriert.
Die folgenden Konfigurationseinstellungen gelten für alle Topics und können nicht geändert werden:
* replication.factor = 3 
* min.insync.replicas = 2

## Worin bestehen die Unterschiede zwischen dem {{site.data.keyword.messagehub}}-Plan "Standard" und dem {{site.data.keyword.messagehub}}-Plan "Enterprise"?
{: #plan_compare }
{: faq}

Weitere Informationen zu den verschiedenen {{site.data.keyword.messagehub}}-Plänen finden Sie in [Plan auswählen](/docs/services/EventStreams?topic=eventstreams-plan_choose).

## Wie wird die Disaster-Recovery gehandhabt?
{: #disaster_recovery }
{: faq}

Zum gegenwärtigen Zeitpunkt ist der Benutzer für die {{site.data.keyword.messagehub}}-Disaster-Recovery zuständig. {{site.data.keyword.messagehub}}-Daten können zwischen einer {{site.data.keyword.messagehub}}-Instanz an einem Standort (in einer Region) und einer anderen Instanz an einem anderen Standort repliziert werden. Es ist jedoch Aufgabe des Benutzers, eine ferne {{site.data.keyword.messagehub}}-Instanz bereitzustellen und die Replikation zu verwalten.

Darüber hinaus ist der Benutzer für die Erstellung von Sicherungskopien der Nachrichtennutzdaten verantwortlich. Diese Daten werden zwar auf mehrere Kafka-Broker innerhalb eines Clusters repliziert und damit vor den meisten Ausfällen geschützt, diese Replikation bietet jedoch keinen Schutz bei einem Ausfall, der den gesamten Standort betrifft. 

Topicnamen werden durch {{site.data.keyword.messagehub}} gesichert.















