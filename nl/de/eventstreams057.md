---

copyright:
  years: 2015, 2019
lastupdated: "2018-06-26"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Bekannte Einschränkungen
{: #restrictions}

Wenn bei der Verwendung von {{site.data.keyword.messagehub}} Probleme auftreten, prüfen Sie die hier beschriebenen bekannten Einschränkungen und Problemumgehungen. 
{: shortdesc}

## Keine Funktionsübernahme durch Java-Kafka-Aufrufe, wenn ein Kafka-Bootstrap-Server fehlschlägt
{: #calls_failover}

### Problem
{: #calls_failover_problem notoc}

Die Java Virtual Machine (JVM) stellt DNS-Suchen in den Cache. Wenn die JVM eine IP-Adresse für einen Hostnamen auflöst, stellt sie die IP-Adresse für einen bestimmten Zeitraum, die so genannte Time to Live (TTL: Lebensdauer), in den Cache. Bei einigen Java-Konfigurationen ist die JVM-TTL so eingestellt, dass die IP-Adresse eines Hostnamens immer erst dann aktualisiert wird, wenn die JVM erneut gestartet wird. Ein Beispiel für eine solche Konfiguration ist eine Konfiguration mit einem Sicherheitsmanager.

### Problemumgehung
{: #calls_failover_workaround notoc}

Da {{site.data.keyword.messagehub}} Kafka-Bootstrap-Server-URLs mit mehreren IP-Adressen für die Hochverfügbarkeit verwendet, sind dem Kafka-Client nicht alle IP-Adressen des Brokers bekannt, was die Funktionsübernahme durch einen betriebsbereiten Broker verhindert. In diesen Fällen erfordert die Funktionsübernahme eine erneute Abfrage der IP-Adressen, damit die Broker-URLs eine betriebsbereite IP-Adresse abrufen können. Es empfiehlt sich, die JVM mit einem TTL-Wert von 30 bis 60 Sekunden zu konfigurieren. Mit diesem Wert wird sichergestellt, dass der Kafka-Client bei Problemen der IP-Adresse eines Bootstrap-Servers durch eine DNS-Abfrage eine neue IP-Adresse suchen und verwenden kann.

Auszug aus der Datei <code>java.security</code>: 

```
# The Java-level namelookup cache policy for successful lookups:
#
# any negative value: caching forever
# any positive value: the number of seconds to cache an address for
# zero: do not cache
#
# default value is forever (FOREVER). For security reasons, this
# caching is made forever when a security manager is set. When a security
# manager is not set, the default behavior in this implementation
# is to cache for 30 seconds.
#
# NOTE: setting this to anything other than the default value can have
#       serious security implications. Do not set it unless
#       you are sure you are not exposed to DNS spoofing attack.
#
#networkaddress.cache.ttl=-1
```

### Vorgehensweise zum Ändern der JVM-TTL
* Stellen Sie zum Ändern der JVM-TTL für alle Anwendungen den Wert <code>networkaddress.cache.ttl</code> in der Datei <code><var class="keyword varname">$JAVA_HOME</var>/jre/lib/security/java.security</code> ein.
* Stellen Sie zum Ändern der JVM-TTL für eine bestimmte Anwendung den Wert <code>networkaddress.cache.ttl</code> in Ihrem Anwendungscode wie folgt ein:
```
java.security.Security.setProperty("networkaddress.cache.ttl" , "30");
```

## Java-Kafka-Aufrufe überschreiten möglicherweise das Zeitlimit
{: #calls_timeout}

### Problem
{: #calls_timeout_problem notoc}

In einigen Fällen kann bei einem Aufruf eines Kafka-Java-Clients Kafka nicht gefunden werden. Die Fehlerursache liegt darin, dass der Kafka-Client für jeden der Bootstrap-Server dieselbe fehlerhafte IP-Adresse bestimmt hat. Der Kafka-Client startet Versuche bei den IP-Adressen jedes Brokers (die jeweils derselben fehlerhaften IP-Adresse entsprechen) und stellt fälschlicherweise fest, dass Kafka inaktiv ist. Beachten Sie, dass der Kafka-Client die erste zurückgegebene IP-Adresse in der Liste verwendet, wenn in der DNS-Abfrage mehrere IP-Adressen zurückgegeben werden.

### Problemumgehung
{: #calls_timeout_workaround notoc}

Wiederholen Sie die Aufrufe, nachdem Sie so lange gewartet haben, bis der DNS-Cache der JVM für die Broker-URLs abgelaufen ist. Bei nachfolgenden Kafka-Aufrufen sollte eine funktionierende Broker-IP-Adresse von der DNS-Abfrage zurückgegeben und verwendet werden. 

Ein Kafka Improvement Proposal (KIP) #302 wurde erstellt um sicherzustellen, dass Kafka-Clients alle verfügbaren Broker-IP-Adressen, und nicht nur ein Subset, ausprobieren. Auf diese Weise verursacht eine einzelne fehlerhafte IP-Adresse keinen Fehler mehr.


## Topics und Partitionen
{: #topics_partitions}

*  Topicnamen sind auf ein Maximum von 100 Zeichen begrenzt.
*  Die Standardanzahl von Partitionen für ein Topic ist '1'.
*  Für jeden {{site.data.keyword.Bluemix_notm}}-Bereich gilt ein Grenzwert von 100 Partitionen. Für die
Erstellung weiterer Partitionen müssen Sie einen neuen {{site.data.keyword.Bluemix_notm}}-Bereich verwenden.

## Aufbewahrungsdauer für Nachrichten
{: #message_retention}

In Kafka werden Nachrichten standardmäßig bis zu 24 Stunden aufbewahrt und jede Partition wird auf
1 GB begrenzt. Wenn die Obergrenze von 1 GB erreicht ist, werden die ältesten Nachrichten gelöscht, damit
der Grenzwert eingehalten wird.

Die Aufbewahrungsdauer für Nachrichten können Sie beim Erstellen eines Topics
in der Benutzerschnittstelle oder mit der Verwaltungs-API ändern. Das Zeitlimit muss im Bereich von 1 Stunde (Minimalwert) bis
30 Tage (Maximalwert) liegen.

Informationen zu den Einschränkungen für die zulässigen Einstellungen beim Erstellen von Topics mithilfe eines Kafka-Clients oder mithilfe von Kafka Streams finden Sie im Abschnitt [APIs für die Topicverwaltung](/docs/services/EventStreams/eventstreams104.html).

## Topics in Kafka erstellen und löschen
{: #create_delete}

Das Erstellen und das Löschen von Topics in Kafka erfolgt durch asynchrone Operationen, deren
Ausführung einige Zeit dauern kann. Es wird empfohlen, auf Verwendungsmuster zu verzichten,
die auf das schnelle Erstellen und Löschen von Topics angewiesen sind oder auf das schnelle Löschen
und erneute Erstellen von Topics.

## Kafka-REST-API
{: #trouble_rest}

*  Nur das eingebettete Binärformat wird für Anforderungen und Antworten unterstützt. Die eingebetteten Avro- und JSON-Formate werden nicht unterstützt.
*  Gleichzeitige Anforderungen werden für eine Consumer-Instanz nicht unterstützt.
   Lese-, Commit- oder Löschanforderungen, die sich auf eine Consumer-Instanz beziehen,
sollten erst gesendet werden, wenn für alle ausstehenden Anforderungen dieser Instanz entsprechende Antworten empfangen wurden.

## Ratenbegrenzung in der Kafka-REST-API
{: #kafka_rate}

Für Anwendungen, die die Kafka-REST-API verwenden, können Ratenbegrenzungen
für jeden API-Schlüssel (ApiKey) gelten. Wenn eine solche Begrenzung auftritt,
reagiert die API mit dem folgenden HTTP-Fehler:

<code>429 Too Many Requests</code>
{:screen}

Wenn dieser Fehler angezeigt wird, warten Sie etwas und übergeben Sie die Anforderung erneut.

<!--12/04/18 - Karen: same info duplicated at messagehub108 -->
## Täglicher Neustart der Kafka-REST-API
{: #rest_restart}

Die Kafka-REST-API wird einmal pro Tag für einen kurzen Zeitraum
erneut  gestartet. In diesem Zeitraum ist die Kafka-REST-API möglicherweise
nicht verfügbar. In diesem Fall wird empfohlen, dass Sie Ihre Anforderung
wiederholen. Nach dem Neustart der REST-API müssen Sie Ihre Kafka-Consumer-Instanzen
erneut erstellen. In diesem Fall gibt die
REST-API den folgenden JSON-Code zurück:

```'{"error_code":40403,"message":"Consumer instance not found."}'
```
{:screen}

## Allgemeine Kafka-Consumer-API
{: #kafka_consumer}

Die einfache oder allgemeine Apache Kafka-Consumer-API der Version 0.8.2 kann nicht mit {{site.data.keyword.messagehub}} verwendet werden. Stattdessen können Sie die früheste unterstützte Version der Kafka-Consumer-API verwenden, Version 0.9.
