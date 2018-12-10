---

copyright:
  years: 2015, 2018
lastupdated: "2018-06-23"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Nachrichten erstellen
{: #producing_messages }


Ein Producer ist eine Anwendung, die Datenströme von Nachrichten an Kafka-Topics veröffentlicht. Diese Informationen konzentrieren sich auf die Java-Programmierungsschnittstelle, die Teil des Apache Kafka-Projekts ist. Die Konzepte gelten auch für andere Sprachen, sie heißen nur etwas anders.

In den Programmierschnittstellen wird eine Nachricht als "Datensatz" bezeichnet. Die Java-Klasse "org.apache.kafka.clients.producer.ProducerRecord" wird beispielsweise verwendet, um eine Nachricht aus der Sicht der Producer-API darzustellen. Die Begriffe _Datensatz_ und _Nachricht_ sind austauschbar, aber im Wesentlichen wird ein Datensatz verwendet, um eine Nachricht darzustellen.

Wenn ein Producer sich mit Kafka verbindet, wird eine erste Bootstrap-Verbindung hergestellt. Diese Verbindung kann mit einem der Server im Cluster hergestellt werden. Der Producer fordert Informationen zu Partition und Leadership zu dem Topic an, den er veröffentlichen möchte. Dann baut der Producer eine weitere Verbindung zum Partitionsleader auf und beginnt, die Nachrichten zu veröffentlichen. Diese Aktionen werden automatisch intern ausgeführt, wenn Ihr Producer eine Verbindung zum Kafka-Cluster herstellt.
 
Wenn eine Nachricht an den Leader der Partition gesendet wird, ist diese Nachricht für die Consumer nicht sofort verfügbar. Der Leader hängt den Datensatz für die Nachricht an die Partition an und weist ihr die nächste Offset-Nummer für diese Partition zu. Nachdem alle Follower für die synchronen Replikate den Datensatz repliziert und bestätigt haben, dass sie den Datensatz in ihre Replikate geschrieben haben, ist der Datensatz jetzt *per Commit festgeschrieben* und für Consumer verfügbar.

Jede Nachricht wird als Datensatz dargestellt, der aus zwei Teilen besteht: Schlüssel und Wert. Der Schlüssel wird in der Regel für Daten zur Nachricht verwendet und der Wert ist der Nachrichtentext. Da viele Tools im Kafka-Ökosystem (wie Connectors zu anderen Systemen) nur den Wert verwenden und den Schlüssel ignorieren, wird empfohlen, alle Nachrichtendaten in den Wert aufzunehmen und nur den Schlüssel für die Partitionierung oder Protokollkomprimierung zu verwenden. Verlassen Sie sich nicht auf alles, was aus Kafka ausgelesen wird, um den Schlüssel zu nutzen.

Viele andere Nachrichtensysteme können mit den Nachrichten auch andere Informationen übertragen. Kafka 0.11 enthält erstmalig zu diesem Zweck Datensatz-Header, die vom {{site.data.keyword.messagehub}}-Plan "Enterprise" unterstützt werden. Der {{site.data.keyword.messagehub}}-Plan "Standard" basiert zurzeit auf Kafka 0.10.2.1, daher werden Datensatz-Header noch nicht unterstützt. 

Weitere nützliche Informationen finden Sie unter [Nachrichten verarbeiten](/docs/services/EventStreams/eventstreams114.html) in {{site.data.keyword.messagehub}}.

## Konfigurationseinstellungen
{: #config_settings}

Es gibt viele Konfigurationseinstellungen für den Producer. Sie können Aspekte des Producers steuern, einschließlich Stapelverarbeitung, neue Versuche und Nachrichtenbestätigung. Dies hier sind die wichtigsten:

| Name     |Beschreibung   | Gültige Werte   | Default   |
|----------|---------|----------|---------|
|key.serializer     | Die Klasse, die zum Serialisieren von Schlüsseln verwendet wird. | Java-Klasse, die die Serializer-Schnittstelle implementiert, z. B. "org.apache.kafka.common.serialization.StringSerializer". |Kein Standardwert - Sie müssen einen Wert angeben.|
|value.serializer     | Die Klasse, die zum Serialisieren von Werten verwendet wird. | Java-Klasse, die die Serializer-Schnittstelle implementiert, z. B. "org.apache.kafka.common.serialization.StringSerializer".  | Kein Standardwert - Sie müssen einen Wert angeben. |
|acks     | Die Anzahl der Server, die erforderlich sind, um jede veröffentlichte Nachricht zu bestätigen. Dies steuert die Garantien für die Dauerhaftigkeit, die der Producer benötigt. | 0, 1, all (oder -1)  | 1 |
|retries     | Die Häufigkeit, mit der der Client eine Nachricht wiederholt sendet, wenn beim Senden ein Fehler auftritt. | 0,...  | 0 |
|max.block.ms     | Die Anzahl der Millisekunden, die eine Sende- oder Metadatenanforderung blockieren kann. | 0,...  | 60000 (1 Minute) |
|max.in.flight.requests.per.connection     | Die maximale Anzahl nicht bestätigter Anforderungen, die der Client bei einer Verbindung sendet, bevor weitere Anforderungen blockiert werden.| 1,...  | 5 |
|request.timeout.ms     | Die maximale Zeit, in der der Producer auf eine Antwort auf eine Anforderung wartet. Wenn die Antwort bis zum Ablauf des Zeitlimits nicht empfangen wird, wird die Anforderung wiederholt oder schlägt fehl, wenn die maximale Anzahl der Neuversuche erreicht wurde.| 0,...  | 30000 (30 Sekunden) |

Es noch viele weitere Konfigurationseinstellungen verfügbar. Lesen Sie dennoch die [Apache Kafka-Dokumentation ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://kafka.apache.org/documentation/){:new_window} genau durch, bevor Sie mit den Einstellungen experimentieren.

## Partitionierung
{: #partitioning}

Wenn der Producer eine Nachricht zu einem Topic veröffentlicht, wählt der Producer die zu verwendende Partition aus. Wenn die Reihenfolge wichtig ist, müssen Sie berücksichtigten, dass es sich bei einer Partition um eine geordnete Reihenfolge von Datensätzen handelt. Ein Topic umfasst aber eine oder mehrere Partitionen. Wenn eine Reihe von Nachrichten in der richtigen Reihenfolge bereitgestellt werden soll, stellen Sie sicher, dass sie alle zur gleichen Partition gehören. Die einfachste Möglichkeit, um dies zu erreichen, ist es, wenn alle Nachrichten denselben Schlüssel haben. 
 
Der Producer kann explizit eine Partitionsnummer angeben, wenn er eine Nachricht veröffentlicht. Somit haben Sie eine direkte Kontrolle. Aber der Producercode wird dadurch auch komplexer, da er für die Partitionsauswahl verwaltet. Weitere Informationen dazu finden im Methodenaufruf "Producer.partitionsFor". Der Aufruf wird beispielsweise für [Kafka 0.11.0.1 ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://kafka.apache.org/0110/javadoc/org/apache/kafka/clients/producer/KafkaProducer.html){:new_window} beschrieben.
 
Wenn der Producer keine Partitionsnummer angibt, wird die Auswahl der Partition durch eine Partitionierungsfunktion getroffen. Die Standard-Partitionierungsfunktion, die im Kafka-Producer enthalten ist, funktioniert wie folgt:

* Wenn der Datensatz keinen Schlüssel enthält, wählen Sie die Partition im Round-Robin-Modus aus.
* Wenn der Datensatz einen Schlüssel enthält, wählen Sie die Partition aus, indem Sie einen Hashwert für den Schlüssel berechnen. Dadurch wird für alle Nachrichten mit demselben Schlüssel dieselbe Partition ausgewählt.

Sie können auch Ihre eigene angepasste Partitionierungsfunktion schreiben. Eine angepasste Partitionierungsfunktion kann ein beliebiges Schema für die Zuordnung von Datensätzen zu Partitionen auswählen. Verwenden Sie zum Beispiel nur ein Subset der Informationen im Schlüssel oder eine anwendungsspezifische ID.


## Nachrichtenreihenfolge
{: #message_ordering}

Kafka schreibt die Nachrichten in der Regel in der Reihenfolge, in der sie vom Producer gesendet werden. Es gibt jedoch Situationen, in denen Neuversuche dazu führen können, dass Nachrichten dupliziert werden oder deren Reihenfolge geändert wird. Wenn eine Reihe von Nachrichten in der richtigen Reihenfolge gesendet werden soll, müssen Sie sicherstellen, dass sie alle auf die gleiche Partition geschrieben werden.
 
Der Producer kann auch erneut versuchen, Nachrichten automatisch senden. Es wird empfohlen, diese Wiederholungsfunktion zu aktivieren, da anderenfalls Ihr Anwendungscode Neuversuche selbst durchführen muss. Die Kombination aus Stapelverarbeitung in Kafka und automatischen Neuversuchen kann dazu führen, dass Nachrichten dupliziert werden und deren Reihenfolge geändert wird.
 
Beispiel: Sie veröffentlichen eine Folge von den drei Nachrichten &lt;M1, M2, M3&gt; zu einem Topic. Die Datensätze passen möglicherweise in denselben Stapel, sodass sie gemeinsam an den Partitionsleader gesendet werden. Der Leader schreibt die Daten dann zur Partition und repliziert sie als separate Datensätze. Bei einem Ausfall kann es passieren, dass M1 und M2 der Partition hinzugefügt werden, M3 aber nicht. Der Producer empfängt keine Bestätigung, daher sendet er &lt;M1, M2, M3&gt; erneut. Der neue Leader schreibt M1, M2 und M3 einfach zur Partition, die jetzt &lt;M1, M2, M1, M2, M3&gt; enthält, wobei die duplizierte M1 der ursprünglichen M2 folgt. Wenn Sie die Anzahl der gerade ausgeführten Anforderungen pro Broker auf einen Broker beschränken, können Sie die Änderung der Reihenfolge verhindern. Möglicherweise wird ein einzelner Datensatz dupliziert (wie &lt;M1, M2, M2, M3&gt;), aber die Reihenfolge stimmt. Sie können in Kafka 0.11 (in {{site.data.keyword.messagehub}} nicht verfügbar) die idempotent-Producerfunktion verwenden, um die Duplizierung von M2 zu verhindern.
 
Bei Kafka ist es üblich, die Anwendungen so zu schreiben, dass sie mit gelegentlichen Nachrichtenduplikaten umgehen können, da der Leistungseinfluss bei einer einzelnen gerade ausgeführten Anforderung enorm ist.

## Auftragsbestätigung von Nachrichten
{: #message_acknowledgments}

Wenn Sie eine Nachricht veröffentlichen, können Sie die erforderliche Bestätigungsanforderung mithilfe der Producer-Konfiguration `acks` auswählen. Diese Auswahl ist ein Ausgleich zwischen Durchsatz und Zuverlässigkeit. Es gibt drei Stufen:

<dl>
<dt>acks=0 (am wenigsten zuverlässig)</dt>
<dd>Die Nachricht gilt als gesendet, sobald sie in das Netz geschrieben wurde. Es gibt keine Auftragsbestätigung vom Partitionsleader. Daher können Nachrichten verloren gehen, wenn sich das Leadership der Partition ändert. Diese Bestätigungsstufe ist sehr schnell, birgt jedoch das Risiko eines Nachrichtenverlusts in bestimmten Situationen.</dd>
<dt>acks=1 (Standardoption)</dt>
<dd>Dem Producer wird die Nachricht bestätigt, sobald der Partitionsleader seinen Datensatz erfolgreich in die Partition geschrieben hat. Da die Bestätigung auftritt, bevor bekannt ist, dass der Datensatz die synchronen Replikate erreicht hat, kann die Nachricht verloren gehen, wenn der Leader fehlschlägt, aber die Follower die Nachricht noch nicht erhalten haben. Wenn sich das Leadership der Partition ändert, informiert der alte Leader den Producer. Dieser bearbeitet den Fehler und versucht erneut, die Nachricht an den neuen Leader zu senden. Da Nachrichten bestätigt werden, bevor ihr Empfang von allen Replikaten bestätigt wird, heißt das, dass Nachrichten, die bestätigt, aber noch nicht vollständig repliziert wurden, verloren gehen können, wenn sich das Leadership der Partition ändert.</dd>
<dt>acks=all (am zuverlässigsten)</dt>
<dd>Dem Producer wird die Nachricht bestätigt, sobald der Partitionsleader seinen Datensatz erfolgreich in die Partition geschrieben hat und alle synchronen Repliken dasselbe getan haben. Die Nachricht geht nicht verloren, wenn sich das Leadership der Partition ändert. Voraussetzung ist aber, dass mindestens ein synchrones Replikat verfügbar ist.</dd>
</dl>

Auch wenn Sie nicht darauf warten, dass Nachrichten an den Producer bestätigt werden, sind die Nachrichten nur dann zur Verarbeitung verfügbar, wenn sie per Commit festgeschrieben werden. Das bedeutet, dass die Replikation mit den synchronen Replikaten abgeschlossen ist. Mit anderen Worten: Die Latenzzeit für das Senden der Nachrichten aus Sicht des Producers ist geringer als die Endpunkt-zu-Endpunkt-Latenzzeit, die gemessen wird, wenn der Producer eine Nachricht an einen Consumer sendet, der diese Nachricht empfängt.

Vermeiden Sie daher lieber auf das Warten auf das Bestätigen einer Nachricht, bevor Sie die nächste Nachricht veröffentlichen. Durch das Warten wird verhindert, dass der Producer Nachrichten im Stapel zusammenfassen kann. Außerdem wird die Rate reduziert, mit der Nachrichten unterhalb der Round-Trip-Latenz des Netzes veröffentlicht werden können.

## Stapelverarbeitung, Drosselung und Komprimierung
{: #batching}

Aus Gründen der Effizienz sammelt der Producer mehrere Datensätze, die dann an die Server gesendet werden. Wenn Sie die Komprimierung aktivieren, komprimiert der Producer jeden Stapel. Dies verbessert möglicherweise die Leistung, da weniger Daten über das Netz übertragen werden.

Wenn Sie versuchen, Nachrichten schneller zu veröffentlichen, als sie an einen Server gesendet werden können, puffert der Producer sie automatisch in Anforderungen für die Stapelverarbeitung. Der Producer verwaltet für jede Partition einen Puffer aus nicht gesendeten Datensätzen. Aber irgendwann kann sogar mit der Stapelverarbeitung die gewünschte Rate nicht mehr erreicht werden.
 
Es gibt noch einen weiteren Faktor, der Auswirkungen hat. Um zu verhindern, dass einzelne Producer oder Consumer den Cluster überfluten, wendet {{site.data.keyword.messagehub}} Größenbeschränkungen für den Durchsatz an. Die Rate, mit der jeder Producer Daten sendet, wird berechnet und jeder Producer, der versucht, seine Rate zu überschreiten, wird gedrosselt. Das Drosseln wird angewendet, indem das Senden von Antworten an den Producer leicht verzögert wird. Dies funktioniert ähnlich wie eine Bremse.
 
Zusammenfassend kann gesagt werden, dass der Datensatz bei der Veröffentlichung einer Nachricht zunächst im Producer in einen Puffer geschrieben wird. Der Producer erstellt im Hintergrund einen Stapel und sendet die Datensätze an den Server. Der Server antwortet dann dem Producer und wendet möglicherweise eine Drosselungsverzögerung an, wenn der Producer eine zu schnelle Veröffentlichung durchführt. Wenn der Puffer im Producer voll ist, wird der Sendeaufruf des Producers verzögert und kann schließlich mit einer Ausnahmebedingung fehlschlagen.

## Code-Snippets
{: #code_snippets}

Diese Code-Snippets befindet sich auf einer höheren Ebene, um die beteiligten Konzepte darzustellen. Komplette Beispiele finden Sie in den {{site.data.keyword.messagehub}}-Beispielen in GitHub unter https://github.com/ibm-messaging/event-streams-samples.

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

Um Nachrichten zu senden, müssen Sie beispielsweise Serializer für die Schlüssel und Werte angeben. Beispiel:

```
 props.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
 props.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");
```

Verwenden Sie dann einen KafkaProducer, um Nachrichten zu senden, wobei jede Nachricht durch einen Producer-Datensatz (ProducerRecord) dargestellt wird. Vergessen Sie nicht, KafkaProducer zu schließen, wenn Sie fertig sind. Dieser Code sendet nur die Nachricht, wartet jedoch nicht ab, ob der Sendevorgang erfolgreich war.

```
 Producer<String, String> producer = new KafkaProducer<>(props);
 producer.send(new ProducerRecord<String, String>("T1", "key", "value"));
 producer.close();
 ```
 
Die Methode `send()` ist asynchron und gibt ein Future zurück, mit dem sein Abschluss überprüft werden kann:

```
 Future<RecordMetadata> f = producer.send(new ProducerRecord<String, String>("T1", "key", "value"));
// Tun Sie etwas anderes
// Warten Sie auf das Sendeergebnis
 RecordMetadata rm = f.get();
 long offset = rm.offset; 
```

Sie können auch einen Callback bereitstellen, wenn Sie die Nachricht senden:

```
producer.send(new ProducerRecord<String,String>("T1","key","value", new Callback() {
          public void onCompletion(RecordMetadata metadata, Exception exception) {
                     // Wird aufgerufen, wenn der Sendevorgang erfolgreich oder mit Ausnahmebedingung abgeschlossen wurde
          }
});
```

Weitere Informationen finden Sie in der Referenz [Javadoc für den Kafka-Client ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://kafka.apache.org/0110/javadoc/index.html){:new_window}, die sehr ausführlich ist. 

