---

copyright:
  years: 2015, 2019
lastupdated: "2018-06-01"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# MQ-Bridge
{: #mq_bridge}

**Die MQ-Bridge ist nur als Bestandteil des Plans "Standard" verfügbar.**
<br/>

Die MQ-Bridge ermöglicht das Übertragen von Nachrichtendaten aus einer {{site.data.keyword.IBM_notm}}-MQ-Warteschlange
in ein {{site.data.keyword.messagehub}}-Kafka-Topic. Die MQ-Bridge ermöglicht die effiziente Verarbeitung von Cloud-Workloads (z. B. Datenanalyse) für {{site.data.keyword.IBM_notm}} MQ-Nachrichtendaten, die in Ihrem Unternehmen generiert wurden.
 {:shortdesc}

Die MQ-Bridge stellt die Verbindung zu einem {{site.data.keyword.IBM_notm}} MQ-Warteschlangenmanager als ein MQ-Client her und verarbeitet MQ-Nachrichtendaten aus einer MQ-Warteschlange. Die Bridge wandelt jede MQ-Nachricht in einen Kafka-Datensatz um und sendet die Nachricht an ein {{site.data.keyword.messagehub}}-Kafka-Topic.

## Unterstützte Versionen von {{site.data.keyword.IBM_notm}} MQ
{: #mq_support}

Die Bridge arbeitet mit den folgenden Versionen von {{site.data.keyword.IBM_notm}} MQ zusammen:

* {{site.data.keyword.IBM_notm}} MQ Version 8.0 und höher

## MQ-Bridge schützen
{: #mq_bridge_security}

Sie können die MQ-Bridge so konfigurieren, dass die Authentifizierung in {{site.data.keyword.IBM_notm}} MQ über eine Benutzer-ID mit Kennwort erfolgt. Es wird empfohlen, die folgenden Berechtigungen nur für die Benutzeridentität zu erteilen, die einer Instanz der MQ-Bridge zugeordnet ist:

* Berechtigung CONNECT. Die MQ-Bridge muss eine Verbindung zum MQ-Warteschlangenmanager herstellen.
* Berechtigung GET für die Warteschlange, aus der die MQ-Bridge Nachrichten verarbeiten soll.

## Zuverlässigkeit und Reihenfolge der Nachrichten
{: #mq_bridge_reliability}

Die MQ-Bridge stellt eine Zuverlässigkeitsstufe bereit, die mindestens die einmalige
Übermittlung der zu übertragenden Nachrichtendaten sicherstellt. Dies bedeutet, dass in der Bridge keine
Nachrichtendaten verloren gehen. Es können jedoch gelegentlich doppelte Sequenzen der Kafka-Datensätze
für eine gegebene Sequenz der MQ-Nachrichten auftreten. Diese Duplizierung tritt normalerweise auf, wenn
die Verarbeitung der Nachrichtendaten in der Bridge durch ein Ereignis unterbrochen wird. Beispiel:

* Neustart des MQ-Warteschlangenmanagers
* Unterbrechung des Netzes zwischen dem MQ-Warteschlangenmanager und der Bridge
* Neuverteilung der Partitionszuordnung für Kafka-Topics
* Unterbrechung des Netzes zwischen der Bridge und Kafka

Um die zuverlässige Übertragung der Nachrichten aus MQ sicherzustellen, fordert die MQ-Bridge
exklusiven Zugriff auf die MQ-Warteschlange an, aus der Nachrichten gelesen werden sollen. Durch diesen exklusiven Zugriff
wird verhindert, dass andere Anwendungen Nachrichten aus der MQ-Warteschlange empfangen, solange die Warteschlange
von der Bridge verwendet wird. Wenn bereits andere Anwendungen Nachrichten aus der Warteschlange erhalten, kann die
Bridge möglicherweise erst gestartet werden, nachdem diese Anwendungen beendet wurden.

Im Allgemeinen leitet die MQ-Bridge MQ-Nachrichten an Kafka weiter, damit sie von {{site.data.keyword.IBM_notm}} MQ empfangen werden können. Aus den oben dargelegten Gründen kann es jedoch vorkommen, dass MQ-Nachrichtendaten in einem Kafka-Topic doppelt enthalten sind. Diese Duplizierung kann zu Abweichungen zwischen der Reihenfolge der MQ-Nachrichten und der Reihenfolge der Speicherung der entsprechenden Datensätze in Kafka führen.

## Kafka-Partitionszuordnung
{: #mq_bridge_partition}

Die MQ-Bridge unterstützt Optionen zum Steuern der Zuordnung von {{site.data.keyword.IBM_notm}} MQ-Nachrichtendaten zu Kafka-Topicpartitionen. Die Nachrichtenreihenfolge innerhalb einer bestimmten Partition hängt von der ausgewählten Option für die Reihenfolge der Nachrichtendaten ab (siehe [Zuverlässigkeit und Reihenfolge der Nachrichten](#mq_bridge_reliability)). Die folgenden Werte werden unterstützt:
<dl><dt>Default</dt>
<dd>Die Kafka-Standardzuordnung für Partitionen wird verwendet. Dies bedeutet, dass Daten aus MQ-Nachrichten
gleichmäßig auf die Partitionen im Kafka-Topic verteilt werden.</dd>
<dt>CorrelationId</dt>
<dd>MQ-Nachrichten mit gleicher Korrelations-ID werden derselben Kafka-Partition zugeordnet.</dd>
<dt>GroupId</dt>
<dd>MQ-Nachrichten mit gleicher Gruppen-ID werden derselben Kafka-Partition zugeordnet.
</dd>
</dl>

## Einschränkungen bei der Nachrichtengröße
{: #mq_message}

Sie können {{site.data.keyword.IBM_notm}} MQ so konfigurieren, dass zu große Nachrichten in einem {{site.data.keyword.messagehub}}-Kafka-Datensatz gespeichert werden. Die maximale
Größe für einen Kafka-Datensatz ist 1.000.000 Byte. Ein Teil dieser Kapazität wird jedoch anderweitig genutzt, wenn die Bridge für die
Durchführung der Kafka-Partitionszuordnung basierend auf der MQ-Korrelations-ID und der MQ-Gruppen-ID konfiguriert ist. Es wird empfohlen, dass die über die MQ-Bridge gesendeten Nachrichten nicht größer als 950 Kilobyte sein sollten.

Wenn die MQ-Bridge eine Nachricht findet, die zu groß für die Weiterleitung an Kafka ist, wird die
Nachricht von der Bridge gelöscht und ein Protokolleintrag wird an Ihr Kibana-Dashboard ausgegeben. Das Attribut MAXMSGL
der MQ-Warteschlange, aus der die Bridge Nachrichten empfängt, sollte so festgelegt werden, dass die MQ-Anwendungen
keine Nachrichten an die Warteschlange senden, die von der MQ-Bridge nicht übertragen werden können.
