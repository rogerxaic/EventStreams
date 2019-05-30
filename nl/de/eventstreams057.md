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
{:note: .note}


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

<!--following message retention info duplicted in FAQs eventstreams108-->

## Aufbewahrungsdauer für Nachrichten
{: #message_retention}

In Kafka werden Nachrichten standardmäßig bis zu 24 Stunden aufbewahrt und jede Partition wird auf
1 GB begrenzt. Wenn die Obergrenze von 1 GB erreicht ist, werden die ältesten Nachrichten gelöscht, damit
der Grenzwert eingehalten wird.

Die Aufbewahrungsdauer für Nachrichten können Sie beim Erstellen eines Topics
in der Benutzerschnittstelle oder mit der Verwaltungs-API ändern. Das Zeitlimit muss im Bereich von 1 Stunde (Minimalwert) bis
30 Tage (Maximalwert) liegen.

Informationen zu Einschränkungen für die Einstellungen, die bei der Erstellung von Topics mit einem Kafka-Client oder Kafka Streams zulässig sind, finden Sie unter [Wie können mit Kafka-APIs Topics erstellt und gelöscht werden?](/docs/services/EventStreams?topic=eventstreams-faqs#topic_admin).

## Topics in Kafka erstellen und löschen
{: #create_delete}

Das Erstellen und das Löschen von Topics in Kafka erfolgt durch asynchrone Operationen, deren
Ausführung einige Zeit dauern kann. Es wird empfohlen, auf Verwendungsmuster zu verzichten,
die auf das schnelle Erstellen und Löschen von Topics angewiesen sind oder auf das schnelle Löschen
und erneute Erstellen von Topics.

<!--
## Kafka REST API
{: #trouble_rest}

<br/>
**Is this specific to old Standard only? If so I'll move to specific Standard topic.**
{: note}

*  Only the binary-embedded format is supported for requests and
   responses. The Avro and JSON embedded formats are not supported.
*  Concurrent requests are not supported for a consumer instance.
   Read, commit, or delete requests corresponding to a consumer
   instance should be sent only after a response is received for
   any outstanding requests of that instance.

-->
<!--
<br/>
**Is this specific to old Standard only? If so I'll move to specific Standard topic.**
{: note}

## Kafka REST API rate limitation
{: #kafka_rate}

Applications using the Kafka REST API can be subject to rate
limiting for each ApiKey. When this limiting occurs, the API
responds with the following HTTP error:

<code>429 Too Many Requests</code>
{:screen}

If you see this error, wait and submit the request again.

<br/>
**Is this specific to old Standard only? If so I'll move to specific Standard topic.**
{: note}
-->
<!--12/04/18 - Karen: same info duplicated at messagehub108 -->
<!--
## Kafka REST API daily restart
{: #rest_restart}

The Kafka REST API restarts once a day for a short period of
time. During this period, the Kafka REST API might become
unavailable. If this happens, you are recommended to retry your
request. After the REST API has restarted, you will have to
create your Kafka consumer instances again. If this is the case, the
REST API returns the following JSON:

```'{"error_code":40403,"message":"Consumer instance not found."}'
```
{:screen}
-->
<!--
## Kafka high-level consumer API
{: #kafka_consumer}

You cannot use the Apache Kafka 0.8.2 simple or high-level
consumer API with {{site.data.keyword.messagehub}}. Instead, you can use the earliest supported Kafka consumer API, which is 0.10.
-->
