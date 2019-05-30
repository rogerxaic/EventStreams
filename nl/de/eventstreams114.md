---

copyright:
  years: 2015, 2019
lastupdated: "2018-03-23"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Nachrichten verarbeiten
{: #consuming_messages }

Ein Consumer ist eine Anwendung, die Datenströme von Nachrichten von Kafka-Topics verarbeitet. Ein Consumer kann mindestens ein Topic oder eine Partition abonnieren. Diese Informationen konzentrieren sich auf die Java-Programmierungsschnittstelle, die Teil des Apache Kafka-Projekts ist. Die Konzepte gelten auch für andere Sprachen, sie heißen nur etwas anders.
{: shortdesc}

Wenn ein Consumer sich mit Kafka verbindet, wird eine erste Bootstrap-Verbindung hergestellt. Diese Verbindung kann mit einem der Server im Cluster hergestellt werden. Der Consumer fordert Informationen zu Partition und Leadership zum Topic an, den er verarbeiten möchte. Dann baut der Consumer eine weitere Verbindung zum Partitionsleader auf und beginnt, die Nachrichten zu verarbeiten. Diese Aktionen werden automatisch intern ausgeführt, wenn Ihr Consumer eine Verbindung zum Kafka-Cluster herstellt.

Ein Consumer ist in der Regel eine Anwendung mit langer Laufzeit. Ein Consumer fordert Nachrichten von Kafka an, indem `Consumer.poll(...)` regelmäßig aufgerufen wird. Der Consumer ruft `poll()` auf, empfängt Nachrichten im Stapelbetrieb, verarbeitet sie sofort und ruft wieder `poll()` auf.

Wenn ein Consumer eine Nachricht verarbeitet, wird die Nachricht nicht aus dem Topic entfernt. Stattdessen können die Consumer auswählen, wie Kafka mitgeteilt wird, welche Nachrichten verarbeitet wurden. Dieser Prozess wird als "Offset-Commit" bezeichnet.

In den Programmierschnittstellen wird eine Nachricht als "Datensatz" bezeichnet. Die Java-Klasse "org.apache.kafka.clients.consumer.ConsumerRecord" wird beispielsweise verwendet, um eine Nachricht für die Consumer-API darzustellen. Die Begriffe _Datensatz_ und _Nachricht_ sind austauschbar, aber im Wesentlichen wird ein Datensatz verwendet, um eine Nachricht darzustellen.

Weitere nützliche Informationen finden Sie unter [Nachrichten erstellen](/docs/services/EventStreams?topic=eventstreams-producing_messages) in {{site.data.keyword.messagehub}}.

## Consumer-Eigenschaften konfigurieren 
{: #configuring_consumer_properties }

Es gibt viele Konfigurationseinstellungen für den Consumer, die Aspekte des Verhaltens steuern. Dies hier sind die wichtigsten:

| Name     |Beschreibung   | Gültige Werte   | Default   |
|----------|---------|----------|---------|
|key.deserializer     | Die Klasse, die zum Deserialisieren von Schlüsseln verwendet wird. | Java-Klasse, die die Deserializer-Schnittstelle implementiert, z. B. "org.apache.kafka.common.serialization.StringDeserializer".  |Kein Standardwert - Sie müssen einen Wert angeben.|
|value.deserializer     | Die Klasse, die zum Deserialisieren von Werten verwendet wird. | Java-Klasse, die die Deserializer-Schnittstelle implementiert, z. B. "org.apache.kafka.common.serialization.StringDeserializer".  | Kein Standardwert - Sie müssen einen Wert angeben. |
|group.id | Eine ID für die Consumergruppe, zu der der Consumer gehört. | Zeichenfolge |Kein Standardwert|
|auto.offset.reset | Das Verhalten, wenn der Consumer keinen ursprünglichen Offset hat oder der aktuelle Offset im Cluster nicht mehr verfügbar ist. | latest, earliest, none | latest |
|enable.auto.commit | Legt fest, ob der Offset des Consumers automatisch im Hintergrund per Commit festgeschrieben wird. | true, false | true |
|auto.commit.interval.ms | Die Anzahl der Millisekunden zwischen periodischen Commits von Offsets. | 0,... | 5000 (5 Sekunden) |
|max.poll.records | Die maximale Anzahl von Datensätzen in einem Aufruf von poll() | 1,... | 500 |
|session.timeout.ms | Die Anzahl der Millisekunden, innerhalb derer ein Consumer-Heartbeat empfangen werden muss, um die Mitgliedschaft eines Consumers in einer Consumergruppe zu behalten. | 6000-300000 | 10000 (10 Sekunden) |
|max.poll.interval.ms |Das maximale Zeitintervall zwischen Umfragen, bevor der Consumer die Gruppe verlässt. | 1,... | 300000 (5 Minuten) |
| | | | |

Es noch viele weitere Konfigurationseinstellungen verfügbar. Lesen Sie dennoch die [Apache Kafka-Dokumentation ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://kafka.apache.org/documentation/){:new_window} genau durch, bevor Sie mit den Einstellungen experimentieren.

## Consumergruppen

Eine _Consumergruppe_ ist eine Gruppe von Consumern, die für die Verarbeitung von Nachrichten von einem oder mehreren Topics zusammenarbeiten. Die Consumer in einer Gruppe verwenden alle denselben Wert für die `group.id`-Konfiguration. Wenn Sie mehr als einen Consumer zum Verarbeiten Ihrer Workload brauchen, können Sie mehrere Consumer in derselben Consumergruppe ausführen. Selbst wenn Sie nur einen Consumer benötigen, ist es üblich, auch einen Wert für `group.id` anzugeben.

Jede Consumergruppe hat einen Server im Cluster, der der _Koordinator_ und dafür verantwortlich ist, den Consumern in der Gruppe Partitionen zuzuweisen. Diese Verantwortung wird auf die Server im Cluster verteilt, um die Last gleichmäßig zu verteilen. Die Zuordnung von Partitionen zu Consumern kann sich bei jedem neuen Ausgleich in der Gruppe ändern.

Wenn ein Consumer einer Consumergruppe beitritt, erkennt er den Koordinator für diese Gruppe. Der Consumer teilt dem Koordinator mit, dass er der Gruppe beitreten möchte. Der Koordinator startet dann eine Neuverteilung der Partitionen innerhalb der Gruppe, einschließlich des neuen Mitglieds.

Wenn eine der folgenden Änderungen in einer Consumergruppe stattfindet, wird für die Gruppe eine Neuverteilung durchgeführt, indem die Zuweisung von Partitionen an die Gruppenmitglieder verschoben wird, um die Änderung zu berücksichtigen:

* Ein Consumer tritt der Gruppe bei.
* Ein Consumer verlässt die Gruppe.
* Der Koordinator betrachtet einen Consumer als inaktiv.
* Einem vorhandenen Topic werden neue Partitionen hinzugefügt.

Kafka erinnert sich für jede Consumergruppe an den per Commit festgeschriebenen Offset für jede verarbeitete Partition.

Wenn für eine Consumergruppe eine Neuverteilung durchgeführt wurde, beachten Sie, dass die Commits aller Consumer, die die Gruppe verlassen haben, zurückgewiesen und erst wieder akzeptiert werden, wenn die Consumer der Gruppe wieder beitreten. In diesem Fall muss der Consumer der Gruppe erneut beitreten. Möglicherweise wird ihm dann eine andere Partition zugewiesen.

## Consumer-Aktivität

Kafka erkennt fehlgeschlagene Consumer automatisch, sodass aktiven Consumern Partitionen erneut zugeordnet werden können. Dies wird mit zwei Mechanismen erreicht: Polling und Heartbeating.

Wenn die von `Consumer.poll(...)` zurückgegebenen Nachrichten im Stapelbetrieb groß ist oder die Verarbeitung zeitaufwendig ist, kann die Verzögerung vor dem erneuten Aufruf von `poll()` signifikant hoch oder unvorhersehbar sein. In einigen Fällen muss ein langes maximales Abfrageintervall konfiguriert werden, damit die Consumer nicht aus ihren Gruppen entfernt werden, wenn die Nachrichtenverarbeitung längere Zeit dauert. Wenn es nur diesen Mechanismus geben würde, bedeutete dies, dass die Zeit, die zum Erkennen eines gescheiterten Consumers benötigt wird, auch sehr lang ist.

Damit die Aktivität der Consumer einfacher gehandhabt werden kann, wurde in Kafka 0.10.1 das Heartbeating im Hintergrund hinzugefügt. Der Gruppenkoordinator erwartet von den Gruppenmitgliedern, dass sie regelmäßig Heartbeats senden, um anzuzeigen, dass sie weiterhin aktiv sind. Dazu wird im Consumer ein Heartbeat-Hintergrundthread ausgeführt, der regelmäßig Heartbeats an den Koordinator sendet. Wenn der Koordinator von einem Gruppenmitglied innerhalb des _Sitzungszeitlimits_ keinen Heartbeat empfängt, wird das Mitglied aus der Gruppe entfernt und eine Neuverteilung der Gruppe durchgeführt. Das Sitzungszeitlimit kann viel kürzer als das maximale Abfrageintervall sein, sodass die Zeit, die zum Erkennen eines fehlerhaften Consumers benötigt wird, kurz sein kann, selbst wenn die Nachrichtenverarbeitung sehr lang dauert.

Sie können das maximale Abfrageintervall mit der Eigenschaft `max.poll.interval.ms` sowie das Sitzungszeitlimit mit der Eigenschaft `session.timeout.ms` konfigurieren. Sie müssen normalerweise diese Einstellungen nicht verwenden, es sei denn, das Verarbeiten von Nachrichten im Stapelbetrieb dauert länger als 5 Minuten.

## Offsets verwalten

Kafka behält für jede Consumergruppe den per Commit festgeschriebenen Offset für jede verarbeitete Partition bei. Wenn ein Consumer eine Nachricht verarbeitet, wird die Nachricht nicht von der Partition entfernt. Stattdessen wird nur ihr aktueller Offset aktualisiert, indem der Prozess "Offset-Commit" verwendet wird.

{{site.data.keyword.messagehub}} speichert Consumer-Offset-Informationen für einen Zeitraum von 7 Tagen.

### Was ist, wenn es keinen per Commit festgeschriebenen Offset gibt?
Wenn ein Consumer gestartet und ihm eine zu verarbeitende Partition zugeordnet wird, beginnt er mit dem per Commit festgeschriebenen Offset seiner Gruppe. Wenn kein per Commit festgeschriebener Offset vorhanden ist, kann der Consumer auswählen, ob er anhand der Einstellung der Eigenschaft `auto.offset.reset` mit der ältesten ("earliest") oder neuesten ("latest") Nachricht beginnen soll. Beispiel:

- `latest` (Standardwert). Der Consumer empfängt und verarbeitet nur Nachrichten, die nach dem Abonnement ankommen. Der Consumer kennt die Nachrichten nicht, die vor dem Abonnement gesendet wurden. Daher werden wahrscheinlich nicht alle Nachrichten von einem Topic verarbeitet.
- `earliest`. Der Consumer verarbeitet alle Nachrichten von Anfang an, da alle gesendeten Nachrichten angekommen sind.

Wenn ein Consumer nach der Verarbeitung einer Nachricht fehlschlägt, bevor das Offset festgeschrieben wurde, spiegeln die festgeschriebenen Offset-Informationen die Nachrichtenverarbeitung nicht wider. Dies bedeutet, dass die Nachricht vom nächsten Consumer in dieser Gruppe, die der Partition zugewiesen wird, erneut verarbeitet wird.

Wenn per Commit festgeschriebene Offsets in Kafka gespeichert und die Consumer neu gestartet werden, fahren die Consumer dort fort, wo sie zuletzt gestoppt wurden. Wenn es keinen per Commit festgeschriebenen Offset gibt, wird die Eigenschaft `auto.offset.reset` nicht verwendet.

### Offsets automatisch per Commit festschreiben

Die einfachste Möglichkeit, Offsets per Commit festzuschreiben, ist es, wenn der Kafka-Consumer das automatisch macht. Das ist die einfachste Lösung, aber man hat weniger Kontrolle als beim manuellen Festschreiben. Ein Consumer schreibt standardmäßig alle 5 Sekunden Offsets per Commit fest. Der Standard-Commit wird alle 5 Sekunden durchgeführt. Dies ist vom Fortschritt der Nachrichtenverarbeitung durch den Consumer unabhängig. Wenn der Consumer `poll()` aufruft, bewirkt dies außerdem, dass der letzte Offset, der vom letzten Aufruf von `poll()` zurückgegeben wird, per Commit festgeschrieben wird (weil er wahrscheinlich verarbeitet wurde)..

Wenn der per Commit festgeschriebene Offset die Verarbeitung der Nachrichten übernimmt, ist es nach einem Consumer-Ausfall möglich, dass einige Nachrichten übersprungen werden. Der Grund hierfür ist, dass die Verarbeitung beim festgeschriebenen Offset neu gestartet wird, der einen späteren Zeitpunkt hat als die letzte Nachricht, die vor dem Ausfall verarbeitet wurde. Aus diesem Grund ist die Zuverlässigkeit wichtiger als eine einfache Handhabung. Daher wird empfohlen, Offsets manuell per Commit festzuschreiben.

### Offsets manuell per Commit festschreiben

Wenn `enable.auto.commit` auf `false` festgelegt wird, schreibt der Consumer die Offsets manuell per Commit fest. Dies kann synchron oder asynchron sein. Ein übliches Muster ist das Festschreiben des Offsets der zuletzt verarbeiteten Nachricht auf Basis eines periodischen Zeitgebers. Das bedeutet, dass jede Nachricht mindestens einmal bearbeitet, aber der festgeschriebene Offset die Verarbeitung der Nachrichten nicht übernimmt. Die Häufigkeit des periodischen Zeitgebers steuert die Anzahl der Nachrichten, die nach einem Consumer-Ausfall erneut verarbeitet werden können. Die Nachrichten werden aus dem zuletzt gespeicherten per Commit festgeschriebenen Offset abgerufen, wenn die Anwendung neu gestartet oder die Gruppe neu verteilt wird.

Das per Commit festgeschriebene Offset ist das Offset der Nachrichten, von dem die Verarbeitung fortgesetzt wird. Dies ist in der Regel das Offset der zuletzt verarbeiteten Nachrichten *plus eins*.

### Consumer-Verzögerung

Die Consumer-Verzögerung für eine Partition ist die Differenz zwischen dem Offset der zuletzt veröffentlichten Nachricht und dem festgeschriebenen Offset des Consumers. Schwankungen in den Erstellungs- und Verarbeitungsraten sind zwar üblich, aber über einen längeren Zeitraum sollte die Verarbeitungsrate nicht langsamer sein als die Erstellungsrate.

Wenn Sie beobachten, dass ein Consumer Nachrichten erfolgreich verarbeitet, aber gelegentlich eine Nachrichtengruppe überspringt, kann das ein Zeichen dafür sein, dass der Consumer nicht mithalten kann. Bei Topics, die keine Protokollkomprimierung verwenden, wird die Menge des Protokollspeicherbereichs verwaltet, indem alte Protokollsegmente regelmäßig gelöscht werden. Wenn ein Consumer so weit zurückgefallen ist, dass er Nachrichten in einem Protokollsegment verarbeitet, das bereits gelöscht wurde, springt der Consumer zum Anfang des nächsten Protokollsegments. Wenn es wichtig ist, dass der Consumer alle Nachrichten verarbeitet, führt dieses Verhalten zu einem Nachrichtenverlust aus Sicht des Consumers.

Sie können das Tool <code>kafka-consumer-groups</code> verwenden, um die Consumer-Verzögerung anzuzeigen. Zu diesem Zweck können Sie auch die Consumer-API und die Consumer-Metriken nutzen.


## Die Geschwindigkeit der Nachrichtenverarbeitung steuern
{: #message_consumption_speed }

Wenn Sie Probleme mit der Nachrichtenbehandlung aufgrund einer Nachrichtenüberflutung haben, können Sie eine Consumer-Option festlegen, um die Geschwindigkeit der Nachrichtenverarbeitung zu steuern. Verwenden Sie `fetch.max.bytes` und `max.poll.records`, um zu steuern, wie viele Daten ein Aufruf von `poll()` zurückgeben kann.


## Consumer-Neuverteilung handhaben
Wenn Consumer einer Gruppe hinzugefügt oder aus einer Gruppe entfernt werden, findet eine Neuverteilung in der Gruppe statt und die Consumer können keine Nachrichten verarbeiten. Dies führt dann dazu, dass alle Consumer in einer Consumer-Gruppe für einen kurzen Zeitraum nicht verfügbar sind.

Sie können einen ConsumerRebalanceListener verwenden, um Offsets per Commit manuell festzuschreiben (wenn Sie keine automatische Festschreibung verwenden), wenn Sie mit dem Callback "on partitions revoked" (bei widerrufenen Partitionen) benachrichtigt werden, und um die weitere Verarbeitung zu unterbrechen, bis Sie über die erfolgreiche Neuverteilung mithilfe des Callbacks "on partition assigned" (bei zugewiesenen Partitionen) benachrichtigt werden..


## Code-Snippets
{: #consumer_code_snippets notoc}

Diese Code-Snippets befindet sich auf einer höheren Ebene, um die beteiligten Konzepte darzustellen. Vollständige Beispiele finden Sie in den {{site.data.keyword.messagehub}}-Beispielen in [GitHub ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-messaging/event-streams-samples).

Um eine Verbindung zu {{site.data.keyword.messagehub}} herzustellen, müssen Sie zuerst die Konfigurationseigenschaften erstellen. Alle Verbindungen zu {{site.data.keyword.messagehub}} werden mithilfe von TLS und einer Benutzer-/Kennwortauthentifizierung gesichert, sodass Sie mindestens diese Eigenschaften benötigen. Ersetzen Sie KAFKA_BROKERS_SASL, USER und PASSWORD mit Ihren eigenen Serviceberechtigungsnachweisen:

```
Properties props = new Properties();
 props.put("bootstrap.servers", KAFKA_BROKERS_SASL);
 props.put("sasl.jaas.config", "org.apache.kafka.common.security.plain.PlainLoginModule required username=\"USER\" password=\"PASSWORD\";");
 props.put("security.protocol", "SASL_SSL");
 props.put("sasl.mechanism", "PLAIN");
 props.put("ssl.protocol", "TLSv1.2");
 props.put("ssl.enabled.protocols", "TLSv1.2");
 props.put("ssl.endpoint.identification.algorithm", "HTTPS");
```

Um Nachrichten zu verarbeiten, müssen Sie beispielsweise Deserializer für die Schlüssel und Werte angeben. Beispiel:

```
 props.put("key.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
 props.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
```

Verwenden Sie dann einen KafkaConsumer, um Nachrichten zu verarbeiten, wobei jede Nachricht durch einen Consumer-Datensatz (ConsumerRecord) dargestellt wird. Die gängigste Art, Nachrichten zu verarbeiten, ist es, den Consumer in eine Consumergruppe zu platzieren, indem Sie die Gruppen-ID festlegen und dann `subscribe()` für eine Liste von Topics aufrufen. Dem Consumer werden einige zu verarbeitende Partitionen zugewiesen. Wenn es aber mehr Consumer in der Gruppe als Partitionen im Topic gibt, werden dem Consumer möglicherweise keine Partitionen zugewiesen. Rufen Sie dann `poll()` in einer Schleife auf und empfangen Sie einen Stapel zu verarbeitender Nachrichten, wobei jede Nachricht durch einen Consumer-Datensatz (ConsumerRecord) dargestellt wird.

```
props.put("group.id", "G1");
Consumer<String, String> consumer = new KafkaConsumer<>(props);
consumer.subscribe(Arrays.asList("T1"));
while (true) {
   ConsumerRecords<String, String> records = consumer.poll(100);
   for (ConsumerRecord<String, String> record : records)
     System.out.printf("offset = %d, key = %s, value = %s%n", record.offset(), record.key(), record.value());
}
```

Diese Consumer-Schleife wird endlos ausgeführt, aber sie kann von einem anderen Thread unterbrochen werden. Dieser ruft `Consumer.wakeup()` auf, um ein richtiges Herunterfahren zu erzielen.

Um Offsets per Commit manuell festzuschreiben, ist es zunächst notwendig, die Konfiguration `enable.auto.commit` auf `false` festzulegen. Verwenden Sie dann entweder `Consumer.commmitSync()` oder `Consumer.commitAsync()`, um den festgeschriebenen Offset laufend zu aktualisieren. Aus Gründen der Einfachheit verarbeitet dieses Beispiel die Datensätze für jede Partition und schreibt separat den letzten Offset fest.

```
props.put("group.id", "G1");
props.put("enable.auto.commit", "false");
Consumer<String, String> consumer = new KafkaConsumer<>(props);
consumer.subscribe(Arrays.asList("T1"));
try {
  while (true) {
    ConsumerRecords<String, String> records = consumer.poll(100);
    for (TopicPartition tp : records.partitions()) {
      List<ConsumerRecord<String, String>> partRecords = records.records(tp);
      long lastOffset = 0;
      for (ConsumerRecord<String, String> record : partRecords) {
        System.out.printf("offset = %d, key = %s, value = %s%n", record.offset(), record.key(), record.value());
        lastOffset = record.offset();
      }

      consumer.commitSync(Collections.singletonMap(tp, new OffsetAndMetadata(lastOffset + 1)));
    }
  }
}
finally {
  consumer.close();
}
```

## Ausnahmebehandlung

Jede stabile Anwendung, die den Kafka-Client verwendet, muss Ausnahmen für bestimmte erwartete Situationen behandeln können. In einigen Fällen werden die Ausnahmebedingungen nicht direkt ausgelöst, da einige Methoden asynchron sind und die Ergebnisse mit einem `Future` oder Callback übermitteln. Beispielcode finden Sie in [GitHub ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-messaging/event-streams-samples). Dort werden vollständige Beispiele gezeigt.

Dies ist eine Liste mit Ausnahmebedingungen, die Sie in Ihrem Code behandeln sollten:

### org.apache.kafka.common.errors.WakeupException
Ausgelöst durch `Consumer.poll(...)` als Ergebnis des Aufrufs von `Consumer.wakeup()`. Dies ist die Standardmaßnahme, um die Sendeaufrufschleife des Consumers zu unterbrechen. Die Sendeaufrufschleife sollte beendet und `Consumer.close()` aufgerufen werden, um die Verbindung richtig zu trennen.
### org.apache.kafka.common.errors.NotLeaderForPartitionException
Ausgelöst durch das Ergebnis von `Producer.send(...)`, wenn sich das Leadership für eine Partition ändert. Der Client aktualisiert seine Metadaten automatisch, um die aktuellen Leaderinformationen zu suchen. Wiederholen Sie die Operation, die mit den aktualisierten Metadaten erfolgreich sein sollte.
### org.apache.kafka.common.errors.CommitFailedException
Ausgelöst durch das Ergebnis von `Consumer.commitSync(...)`, wenn ein nicht behebbarer Fehler auftritt. In einigen Fällen ist es nicht möglich, die Operation einfach zu wiederholen, da sich die Partitionszuordnung möglicherweise geändert hat und der Consumer seine Offsets nicht mehr per Commit festschreiben kann. Da `Consumer.commitSync(...)` teilweise erfolgreich sein kann, wenn es in einem Aufruf mit mehreren Partitionen verwendet wird, kann die Fehlerbehebung vereinfacht werden, indem ein separater `Consumer.commitSync(...)` für jede Partition verwendet wird.
### org.apache.kafka.common.errors.TimeoutException
Ausgelöst durch `Producer.send(...), Consumer.listTopics()`, wenn die Metadaten nicht abgerufen werden können. Die Ausnahmebedingung wird auch im gesendeten Callback (oder im zurückgegebenen Future) angezeigt, wenn die angeforderte Auftragsbestätigung nicht innerhalb von `request.timeout.ms` zurückgegeben wird. Der Client kann die Operation wiederholen, aber die Auswirkung der wiederholten Operation hängt von der spezifischen Operation ab. Wenn zum Beispiel das Senden einer Nachricht wiederholt wird, wird die Nachricht möglicherweise dupliziert.

