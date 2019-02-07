---

copyright:
  years: 2015, 2019
lastupdated: "2018-08-08"

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

## Wie ist das Verfügbarkeitsverhalten von {{site.data.keyword.messagehub}}?
{: #availability}
{: faq}

Verwenden Sie diese Informationen beim Schreiben von {{site.data.keyword.messagehub}}-Apps, um das normale Verfügbarkeitsverhalten von {{site.data.keyword.messagehub}} und die damit verbundenen Anforderungen an das Ausführungsverhalten Ihrer Apps kennenzulernen.

### APIs
{: #api_availability }

Im regulären Betrieb von {{site.data.keyword.messagehub}} werden die Knoten der Kafka-Cluster bei Bedarf erneut gestartet.
In manchen Fällen werden Ihre Apps darauf vorbereitet, dass der Cluster Ressourcen neu zuordnet. Gestalten Sie Ihre Apps so,
dass sie auf solche Änderungen entsprechend reagieren und Verbindungen wiederherstellen bzw. Operationen wiederholen können.

### {{site.data.keyword.messagehub}}-Bridges (nur Plan "Standard")
{: #bridge_availability }

Gestalten Sie Ihre Apps so, dass gelegentliche Neustarts der Bridges verarbeitet werden können.

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

## Wie funktioniert die {{site.data.keyword.messagehub}}-Abrechnung beim Plan "Standard"? 
{: #billing }
{: faq}

{{site.data.keyword.messagehub}} tastet beim Plan "Standard" regelmäßig die Topicanzahl eines Benutzers ab und {{site.data.keyword.Bluemix_notm}} zeichnet jeden Tag den maximalen Tastwert auf. {{site.data.keyword.messagehub}} stellt die maximale Anzahl gleichzeitig angezeigter Partitionen sowie die Summe der täglich gesendeten und empfangenen Nachrichten in Rechnung.

Beispiel: Wenn Sie an einem Tag 1 Topic 10 Mal erstellen und löschen, wird maximal 1 Topic in Rechnung gestellt. Wenn Sie jedoch 10 Topics erstellen und löschen, werden Ihnen möglicherweise 0 oder 10 Topics in Rechnung gestellt, je nachdem, wann die Abtastung stattfindet.

{{site.data.keyword.messagehub}} stellt entweder jede Nachricht oder jede 64K in Rechnung. Eine Nachricht bis zu 64K zählt als 1 abrechnungsfähige Nachricht. Nachrichten, die größer als 64K sind, zählen als folgende Anzahl abrechnungsfähiger Nachrichten: <code><var class="keyword varname">nachrichtengröße</var> &divide; 64 k</code>.

<!--12/04/18 - Karen: same info duplicated at messagehub057 -->
## Wie oft wird die Kafka-REST-API neu gestartet? 
{: #REST_restart }
{: faq}

Die Kafka-REST-API wird einmal pro Tag für einen kurzen Zeitraum
erneut  gestartet. 

In diesem Zeitraum ist die Kafka-REST-API möglicherweise
nicht verfügbar. In diesem Fall wird empfohlen, dass Sie Ihre Anforderung
wiederholen. Nach dem Neustart der REST-API müssen Sie Ihre Kafka-Consumer-Instanzen
erneut erstellen. In diesem Fall gibt die
REST-API den folgenden JSON-Code zurück:

```'{"error_code":40403,"message":"Consumer instance not found."}'
```
{:screen}

## Worin bestehen die Unterschiede zwischen dem {{site.data.keyword.messagehub}}-Plan "Standard" und dem {{site.data.keyword.messagehub}}-Plan "Enterprise"?
{: #plan_compare }
{: faq}

Weitere Informationen zu den beiden unterschiedlichen {{site.data.keyword.messagehub}}-Plänen finden Sie in [Plan auswählen](/docs/services/EventStreams/eventstreams085.html).

## Wie wird die Disaster-Recovery gehandhabt?
{: #disaster_recovery }
{: faq}

Zum gegenwärtigen Zeitpunkt ist der Benutzer für die {{site.data.keyword.messagehub}}-Disaster-Recovery zuständig. {{site.data.keyword.messagehub}}-Daten können zwischen einer {{site.data.keyword.messagehub}}-Instanz an einem Standort (in einer Region) und einer anderen Instanz an einem anderen Standort repliziert werden. Es ist jedoch Aufgabe des Benutzers, eine ferne {{site.data.keyword.messagehub}}-Instanz bereitzustellen und die Replikation zu verwalten.

Darüber hinaus ist der Benutzer für die Erstellung von Sicherungskopien der Nachrichtennutzdaten verantwortlich. Diese Daten werden zwar auf mehrere Kafka-Broker innerhalb eines Clusters repliziert und damit vor den meisten Ausfällen geschützt, diese Replikation bietet jedoch keinen Schutz bei einem Ausfall, der den gesamten Standort betrifft. 

Topicnamen werden durch {{site.data.keyword.messagehub}} gesichert.















