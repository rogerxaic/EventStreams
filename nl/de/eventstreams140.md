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

# Häufig gestellte Fragen zum Plan "Classic" 
{: #faqs_classic}

Antworten auf häufig gestellte Fragen zum {{site.data.keyword.IBM}} {{site.data.keyword.messagehub}}-Service für den Plan "Classic".

Antworten auf Fragen, die sich auf alle {{site.data.keyword.messagehub}}-Pläne beziehen, finden Sie in [Häufig gestellte Fragen](docs/services/EventStreams?topic=eventstreams-faqs#faqs).
{: shortdesc}

<!--17/10/17 - Karen: same info duplicated at messagehub104 -->
## Wie können mit Kafka-APIs Topics erstellt und gelöscht werden?
{: #topic_admin_classic}
{: faq}

Wenn Sie einen Kafka-Client der Version 0.11 oder höher verwenden oder Kafka Streams Version 0.10.2.0 oder höher, können Sie APIs verwenden, um Topics zu erstellen oder zu löschen. Für die zulässigen Einstellungen beim Erstellen von Topics gelten bestimmte Einschränkungen. Gegenwärtig können Sie nur die folgenden Einstellungen ändern:

<dl>
<dt>cleanup.policy</dt>
<dd>Zulässige Werte sind <code>delete</code> (Standardwert), <code>compact</code> oder <code>delete,compact</code>
<p>**Anmerkung:**
Wenn für die Bereinigungsrichtlinie (cleanup.policy) nur der Wert <code>compact</code> angegeben ist, wird automatisch der Wert <code>delete</code> hinzugefügt, aber die Löschfunktion auf Zeitbasis inaktiviert. Die Nachrichten in dem Topic werden bis 1 GB komprimiert. Die Löschfunktion wird erst beim Überschreiten dieses Werts aktiviert.</p>
</dd>

<dt>retention.ms</dt>
<dd>Der Standardaufbewahrungszeitraum ist 24 Stunden. Der Mindestwert ist 1 Stunde und der Höchstwert ist
30 Tage. Geben Sie den Wert in Stunden an.
</dd>
</dl>


## Welchen Zeitraum legt {{site.data.keyword.messagehub}} für das Protokollspeicherungsfenster für das Consumer-Offsets-Topic fest?
{: #offsets_classic }
{: faq}

{{site.data.keyword.messagehub}} speichert Consumer-Offsets für einen Zeitraum von 7 Tagen. Dies entspricht der Kafka-Konfiguration 'offsets.retention.minutes'. 

Die Offset-Aufbewahrungsdauer gilt systemweit und kann nicht für eine einzelne Topicebene festgelegt werden. Für alle Consumergruppen werden nur gespeicherte Offsets für einen Zeitraum von 7 Tagen bereitgestellt, selbst wenn die Protokollspeicherungsdauer für das zugehörige Topic auf den Maximalwert von 30 Tagen erhöht wurde. 

<!--following message retention info duplicted in eventstreams057 and evenstreams108-->

## Wie lange werden Nachrichten aufbewahrt?
{: #messages_retained}

In Kafka werden Nachrichten standardmäßig bis zu 24 Stunden aufbewahrt und jede Partition wird auf
1 GB begrenzt. Wenn die Obergrenze von 1 GB erreicht ist, werden die ältesten Nachrichten gelöscht, damit
der Grenzwert eingehalten wird.

Die Aufbewahrungsdauer für Nachrichten können Sie beim Erstellen eines Topics
in der Benutzerschnittstelle oder mit der Verwaltungs-API ändern. Das Zeitlimit muss im Bereich von 1 Stunde (Minimalwert) bis
30 Tage (Maximalwert) liegen.

Informationen zu Einschränkungen für die Einstellungen, die bei der Erstellung von Topics mit einem Kafka-Client oder Kafka Streams zulässig sind, finden Sie unter [Wie können mit Kafka-APIs Topics erstellt und gelöscht werden?](/docs/services/EventStreams?topic=eventstreams-faqs_classic#topic_admin_classic).


## Wie ist das Verfügbarkeitsverhalten von {{site.data.keyword.messagehub}}?
{: #availability_classic}
{: faq}

Verwenden Sie diese Informationen beim Schreiben von {{site.data.keyword.messagehub}}-Apps, um das normale Verfügbarkeitsverhalten von {{site.data.keyword.messagehub}} und die damit verbundenen Anforderungen an das Ausführungsverhalten Ihrer Apps kennenzulernen.

### APIs
{: #api_availability_classic }

Im regulären Betrieb von {{site.data.keyword.messagehub}} werden die Knoten der Kafka-Cluster bei Bedarf erneut gestartet.
In manchen Fällen werden Ihre Apps darauf vorbereitet, dass der Cluster Ressourcen neu zuordnet. Gestalten Sie Ihre Apps so,
dass sie auf solche Änderungen entsprechend reagieren und Verbindungen wiederherstellen bzw. Operationen wiederholen können.

### {{site.data.keyword.messagehub}}-Bridges 
{: #bridge_availability_classic }

Gestalten Sie Ihre Apps so, dass gelegentliche Neustarts der Bridges verarbeitet werden können.

## Was ist die maximale Nachrichtengröße von {{site.data.keyword.messagehub}}? 
{: #max_message_size_classic }
{: faq}

Die maximale Nachrichtengröße von {{site.data.keyword.messagehub}} ist 1 MB. Das ist der Kafka-Standardwert. 

## Was sind die Replikationseinstellungen für {{site.data.keyword.messagehub}}? 
{: #replication_classic }
{: faq}

{{site.data.keyword.messagehub}} ist für eine starke Verfügbarkeit und Dauerhaftigkeit konfiguriert.
Die folgenden Konfigurationseinstellungen gelten für alle Topics und können nicht geändert werden:
* replication.factor = 3
* min.insync.replicas = 2

## Wie funktioniert die {{site.data.keyword.messagehub}}-Abrechnung beim Plan "Classic"? 
{: #billing_classic }
{: faq}

{{site.data.keyword.messagehub}} tastet beim Plan "Classic" regelmäßig die Topicanzahl eines Benutzers ab und {{site.data.keyword.Bluemix_notm}} zeichnet jeden Tag den maximalen Tastwert auf. {{site.data.keyword.messagehub}} stellt die maximale Anzahl gleichzeitig angezeigter Partitionen sowie die Summe der täglich gesendeten und empfangenen Nachrichten in Rechnung.

Beispiel: Wenn Sie an einem Tag 1 Topic 10 Mal erstellen und löschen, wird maximal 1 Topic in Rechnung gestellt. Wenn Sie jedoch 10 Topics erstellen und löschen, werden Ihnen möglicherweise 0 oder 10 Topics in Rechnung gestellt, je nachdem, wann die Abtastung stattfindet.

{{site.data.keyword.messagehub}} stellt entweder jede Nachricht oder jede 64K in Rechnung. Eine Nachricht bis zu 64K zählt als 1 abrechnungsfähige Nachricht. Nachrichten, die größer als 64K sind, zählen als folgende Anzahl abrechnungsfähiger Nachrichten: <code><var class="keyword varname">nachrichtengröße</var> &divide; 64 k</code>.

<!--12/04/18 - Karen: same info duplicated at messagehub057 -->
## Wie oft wird die Kafka-REST-API neu gestartet? 
{: #REST_restart_classic }
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

## Was sind die Unterschiede zwischen den {{site.data.keyword.messagehub}}-Plänen?
{: #plan_compare_classic }
{: faq}

Weitere Informationen zu den anderen {{site.data.keyword.messagehub}}-Plänen finden Sie in [Plan auswählen](/docs/services/EventStreams?topic=eventstreams-plan_choose). Informationen zum Plan "Classic" finden Sie in [Plan "Classic" - Übersicht](/docs/services/EventStreams?topic=eventstreams-plan_choose_classic#plan_choose_classic).


## Wie wird die Disaster-Recovery gehandhabt?
{: #disaster_recovery_classic }
{: faq}

Zum gegenwärtigen Zeitpunkt ist der Benutzer für die {{site.data.keyword.messagehub}}-Disaster-Recovery zuständig. {{site.data.keyword.messagehub}}-Daten können zwischen einer {{site.data.keyword.messagehub}}-Instanz an einem Standort (in einer Region) und einer anderen Instanz an einem anderen Standort repliziert werden. Es ist jedoch Aufgabe des Benutzers, eine ferne {{site.data.keyword.messagehub}}-Instanz bereitzustellen und die Replikation zu verwalten.

Darüber hinaus ist der Benutzer für die Erstellung von Sicherungskopien der Nachrichtennutzdaten verantwortlich. Diese Daten werden zwar auf mehrere Kafka-Broker innerhalb eines Clusters repliziert und damit vor den meisten Ausfällen geschützt, diese Replikation bietet jedoch keinen Schutz bei einem Ausfall, der den gesamten Standort betrifft. 

Topicnamen werden durch {{site.data.keyword.messagehub}} gesichert.















