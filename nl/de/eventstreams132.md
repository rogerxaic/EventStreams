---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-09"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}

# Service Level Agreement (SLA) für die Verfügbarkeit von {{site.data.keyword.messagehub}} 
{: #sla}

## Plan "Standard"
Der {{site.data.keyword.messagehub}}-Service wird im Rahmen des Plans "Standard" mit einer Verfügbarkeit von 99,95% bereitgestellt.
Weitere Informationen zum SLA für Hochverfügbarkeitsservices in {{site.data.keyword.Bluemix}} finden Sie in [{{site.data.keyword.Bluemix_notm}}-Servicebeschreibung ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www-03.ibm.com/software/sla/sladb.nsf/8bd55c6b9fa8039c86256c6800578854/c4ceb9f019f9eb4c862582f9001b3994/$FILE/i126-6605-16_04-2019_en_US.pdf){:new_window}.


## Plan "Enterprise"
Der {{site.data.keyword.messagehub}}-Service wird im Rahmen des Plans "Enterprise" als öffentliche Hochverfügbarkeitsumgebung mit einer Verfügbarkeit von 99,95% bereitgestellt.
Weitere Informationen zum SLA für Hochverfügbarkeitsservices in {{site.data.keyword.Bluemix}} finden Sie in [{{site.data.keyword.Bluemix_notm}}-Servicebeschreibung ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www-03.ibm.com/software/sla/sladb.nsf/8bd55c6b9fa8039c86256c6800578854/c4ceb9f019f9eb4c862582f9001b3994/$FILE/i126-6605-16_04-2019_en_US.pdf){:new_window}.

## Plan "Classic"
Der {{site.data.keyword.messagehub}}-Service wird im Rahmen des Plans "Classic" mit einer Verfügbarkeit von 99,5% bereitgestellt.
Weitere Informationen zum SLA für {{site.data.keyword.Bluemix}} finden Sie in
[{{site.data.keyword.Bluemix_notm}}-Servicebeschreibung ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www-03.ibm.com/software/sla/sladb.nsf/8bd55c6b9fa8039c86256c6800578854/c4ceb9f019f9eb4c862582f9001b3994/$FILE/i126-6605-16_04-2019_en_US.pdf){:new_window}.

<!--
## What does 99.95% availability mean?
Availability refers to the ability of applications to produce and consume messages from Kafka topics.
-->

## Wie erfolgt die Messung?
Serviceinstanzen werden kontinuierlich hinsichtlich ihrer Leistung, ihrer Fehlerraten und ihrer Reaktion auf synthetische Operationen überwacht. Ausfälle werden aufgezeichnet. Weitere Informationen finden Sie in [Servicestatus für Event Streams ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/status?component=messagehub&selected=status){:new_window}.

Die Verfügbarkeit bezieht sich auf die Fähigkeit von Anwendungen, Nachrichten aus Kafka-Topics zu erstellen und zu verarbeiten.

## Was muss zum Erreichen dieser Verfügbarkeit beachtet werden?
Damit ein hoher Grad an Verfügbarkeit aus der Perspektive der Anwendung erreicht werden kann, müssen [Konnektivität](/docs/services/EventStreams?topic=eventstreams-sla#connectivity), [Durchsatz](/docs/services/EventStreams?topic=eventstreams-sla#throughput) sowie [Konsistenz und Permanenz von Nachrichten](/docs/services/EventStreams?topic=eventstreams-sla#message_consistency) berücksichtigt werden. Es liegt im Verantwortungsbereich der Benutzer, Anwendungen so zu entwerfen, dass sie diese drei Aspekte für ihr Unternehmen optimieren.

### Konnektivität
{: #connectivity}

Aufgrund der dynamischen Eigenschaften der Cloud müssen Anwendungen auf Verbindungsunterbrechungen vorbereitet sein. Eine Verbindungsunterbrechung wird nicht als Serviceausfall bewertet.

**Neuversuche**<br/>
Kafka-Clients stellen eine Wiederverbindungslogik bereit, Verbindungswiederholungen müssen Sie für Producer jedoch explizit aktivieren. Weitere Informationen finden Sie in [ Eigenschaft <code>retries</code> ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://kafka.apache.org/11/documentation.html#producerconfigs){:new_window}. Verbindungen werden innerhalb von 60 Sekunden wiederhergestellt.
 
**Duplikate**<br/>
Das Aktivieren von Neuversuchen kann zu doppelten Nachrichten führen. Abhängig vom Zeitpunkt der Verbindungsunterbrechung kann der Producer möglicherweise nicht feststellen, ob eine Nachricht erfolgreich vom Server verarbeitet wurde, und muss daher die Nachricht nach dem Wiederherstellen der Verbindung erneut senden. Es wird empfohlen, Anwendungen so zu entwerfen, dass sie auf doppelte Nachrichten vorbereitet sind. 

Falls Duplikate nicht akzeptabel sind, können Sie das <code>idempotent</code>-Producer-Feature (aus Kafka 1.1) verwenden, um Duplikate bei Verbindungswiederholungen zu verhindern. Weitere Informationen finden Sie in [Eigenschaft <code>enable.idempotence</code> ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://kafka.apache.org/11/documentation.html#producerconfigs){:new_window}.

### Durchsatz
{: #throughput}

Der Durchsatz wird als Anzahl der Byte pro Sekunde ausgedrückt, die in einem Cluster sowohl gesendet als auch empfangen werden können.

**Spezielle Anweisungen für den Plan "Standard"**<br/>
Informationen zum Durchsatz finden Sie in [Grenzwerte und Kontingente - "Standard"](/docs/services/EventStreams?topic=eventstreams-kafka_quotas#kafka_quotas#standard_throughput). 

**Spezielle Anweisungen für den Plan "Enterprise"**<br/>

Informationen zum Durchsatz finden Sie in [Grenzwerte und Kontingente - "Enterprise"](/docs/services/EventStreams?topic=eventstreams-kafka_quotas#enterprise_throughput). 

**Messung**<br/>
Es wird empfohlen, Anwendungen zu instrumentieren, damit Sie ihre Leistung überwachen können. Hierzu gehören beispielsweise die Anzahl der gesendeten und empfangenen Nachrichten, die Nachrichtengrößen und die Rückgabecodes. Mithilfe einer Analyse der Anwendungsnutzung können Sie die zugehörigen Ressourcen entsprechend konfigurieren, wie z. B. den Aufbewahrungszeitraum für Nachrichten zu Topics.

**Sättigung**<br/>
Wenn sich die Menge des Datenverkehrs, der für den Cluster generiert werden kann, dem Grenzwert nähert, werden Producer gedrosselt, die Latenz erhöht sich und schließlich treten Fehler wie z. B. Zeitüberschreitungen auf. Abhängig von der Konfiguration kann sich dies auch auf die Konsistenz und Permanenz von Nachrichten auswirken. Weitere Informationen finden Sie in [Konsistenz und Permanenz von Nachrichten](/docs/services/EventStreams?topic=eventstreams-sla#message_consistency).

### Konsistenz und Permanenz von Nachrichten
{: #message_consistency}

Kafka erreicht die bereitgestellte Verfügbarkeit und Permanenz durch die Replikation der empfangenen Nachrichten auf andere Knoten im Cluster, die dann bei einem Ausfall genutzt werden können. {{site.data.keyword.messagehub}} verwendet drei Replikate (default.replication.factor = 3), d. h., jede von einem Knoten empfangene Nachricht wird auf zwei weitere Knoten in anderen Verfügbarkeitszonen repliziert. Auf diese Weise kann der Verlust eines Knotens oder einer Verfügbarkeitszone ohne Daten- oder Funktionalitätsverlust toleriert werden.

**Producer-<code>acks</code>-Modus**<br/>
Obwohl alle Nachrichten repliziert werden, können Anwendungen steuern, wie stabil die von ihnen generierten Nachrichten an den Service übertragen werden, indem sie die <code>acks</code>-Moduseigenschaft des Producers verwenden. Diese Eigenschaft bietet die Auswahlmöglichkeit zwischen Geschwindigkeit und dem Risiko eines Nachrichtenverlusts. Die Standardeinstellung lautet <code>acks=1</code>. Dies bedeutet, dass der Producer eine Erfolgsmeldung zurückgibt, sobald der Knoten, mit dem er verbunden ist, den Empfang der Nachricht bestätigt, jedoch bevor die Replikation abgeschlossen ist. Die empfohlene und zuverlässigste Einstellung lautet <code>acks=all</code>. Dabei gibt der Producer erst dann eine Erfolgsmeldung zurück, wenn die Nachricht in alle Replikate kopiert wurde. Auf diese Weise wird sichergestellt, dass die Replikate auf dem neuesten Stand sind, und ein Nachrichtenverlust verhindert, falls aufgrund eines Ausfalls zu einem Replikat gewechselt wird.


