---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-02"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Kafka Connect mit {{site.data.keyword.messagehub}} verwenden
{: #kafka_connect }

Sie können Kafka Connect mit {{site.data.keyword.messagehub}} verwenden und die Worker innerhalb oder außerhalb von {{site.data.keyword.Bluemix_short}} ausführen.
{: shortdesc}

Kafka Connect kann im eigenständigen oder im verteilten Modus ausgeführt werden. Der eigenständige Modus ist für Testzwecke und temporäre Verbindungen zwischen Systemen konzipiert. Der verteilte Modus ist besser für den Produktionseinsatz geeignet. Die Konfiguration, die für die Verwendung von {{site.data.keyword.messagehub}} mit diesen beiden Modi erforderlich ist, weist geringfügige Unterschiede auf.
{:shortdesc}

## Konfiguration für eigenständige Worker
{: #standalone_worker notoc}

Der eigenständige Worker verwendet keine internen Topics. Stattdessen verwendet er eine Datei zum Speichern von Offsetinformationen.

Sie müssen die Bootstrap-Server und die SASL-Berechtigungsnachweisinformationen in der Workereigenschaftendatei angeben, die Sie beim Starten eines eigenständigen Kafka Connect-Workers angeben. Im folgenden Beispiel sind die Eigenschaften aufgeführt, die in der Eigenschaftendatei angegeben werden müssen:

<pre>
<code>
  bootstrap.servers=KAFKA_BROKERS_SASL
  sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  security.protocol=SASL_SSL
  sasl.mechanism=PLAIN
  ssl.protocol=TLSv1.2
  ssl.enabled.protocols=TLSv1.2
  ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

Ersetzen Sie KAFKA_BROKERS_SASL, USER und PASSWORD durch die Werte auf der {{site.data.keyword.messagehub}}-Registerkarte **Serviceberechtigungsnachweise** in der {{site.data.keyword.Bluemix_notm}}-Konsole.

### Quellenconnector
{: #source_connector notoc }

Im folgenden Beispiel sind die Eigenschaften aufgeführt, die in der Eigenschaftendatei angegeben werden müssen:

<pre>
<code>
  bootstrap.servers=KAFKA_BROKERS_SASL
  producer.sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  producer.security.protocol=SASL_SSL
  producer.sasl.mechanism=PLAIN
  producer.ssl.protocol=TLSv1.2
  producer.ssl.enabled.protocols=TLSv1.2
  producer.ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

Ersetzen Sie KAFKA_BROKERS_SASL, USER und PASSWORD durch die Werte auf der {{site.data.keyword.messagehub}}-Registerkarte **Serviceberechtigungsnachweise** in der {{site.data.keyword.Bluemix_notm}}-Konsole.

### Zielconnector
{: #sink_connector }

Im folgenden Beispiel sind die Eigenschaften aufgeführt, die in der Eigenschaftendatei angegeben werden müssen:

<pre>
<code>
  bootstrap.servers=KAFKA_BROKERS_SASL
  consumer.sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  consumer.security.protocol=SASL_SSL
  consumer.sasl.mechanism=PLAIN
  consumer.ssl.protocol=TLSv1.2
  consumer.ssl.enabled.protocols=TLSv1.2
  consumer.ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

Ersetzen Sie KAFKA_BROKERS_SASL, USER und PASSWORD durch die Werte auf der {{site.data.keyword.messagehub}}-Registerkarte **Serviceberechtigungsnachweise** in der {{site.data.keyword.Bluemix_notm}}-Konsole.

## Konfiguration für verteilte Worker
{: #distributed_worker notoc}

Sie müssen die Bootstrap-Server und die SASL-Berechtigungsnachweisinformationen in der Eigenschaftendatei angeben, die Sie beim Starten der eigenständigen Kafka Connect-Worker angeben. Im folgenden Beispiel sind die Eigenschaften aufgeführt, die in der Eigenschaftendatei angegeben werden müssen:

<pre>
<code>
  bootstrap.servers=KAFKA_BROKERS_SASL
  sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  security.protocol=SASL_SSL
  sasl.mechanism=PLAIN
  ssl.protocol=TLSv1.2
  ssl.enabled.protocols=TLSv1.2
  ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

Ersetzen Sie KAFKA_BROKERS_SASL, USER und PASSWORD durch die Werte auf der {{site.data.keyword.messagehub}}-Registerkarte **Serviceberechtigungsnachweise** in der {{site.data.keyword.Bluemix_notm}}-Konsole.

Wenn Sie einen Quellenconnector verwenden möchten, müssen Sie auch die SSL- und SASL-Konfiguration für den Producer wie folgt angeben:

<pre>
<code>
  producer.sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  producer.security.protocol=SASL_SSL
  producer.sasl.mechanism=PLAIN
  producer.ssl.protocol=TLSv1.2
  producer.ssl.enabled.protocols=TLSv1.2
  producer.ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

Wenn Sie einen Zielconnector verwenden möchten, müssen Sie auch die SSL- und SASL-Konfiguration für den Consumer wie folgt angeben:

<pre>
<code>
  consumer.sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  consumer.security.protocol=SASL_SSL
  consumer.sasl.mechanism=PLAIN
  consumer.ssl.protocol=TLSv1.2
  consumer.ssl.enabled.protocols=TLSv1.2
  consumer.ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

Darüber hinaus verwendet Kafka Connect im verteilten Modus drei Topics intern. Diese Topics werden beim Start eines Workers automatisch erstellt, wenn Kafka Connect in Apache Kafka Version 0.11 oder höher verwendet wird. Geben Sie die Namen der Topics als Konfigurationsparameter an. Stellen Sie sicher, dass die Werte für alle Worker, die denselben Konfigurationswert für `group.id` aufweisen, identisch sind.

| Konfiguration               | Beschreibung                                                         |
| --------------------------- | ------------------------------------------------------------------- |
| `offset.storage.topic`      | Connector-Offsets-Topic                                             |
| `offset.storage.partitions` | Anzahl der Partitionen für Connector-Offsets-Topic (Standardwert 25) |
| `config.storage.topic`      | Connectorkonfigurationstopic                                       |
| `status.storage.topic`      | Connectorstatustopic                                              |
| `status.storage.partitions` | Anzahl der Partitionen für Connectorstatustopic (Standardwert 5)          |

Sie können beispielsweise die folgenden Schlüssel/Wert-Paare in den Eigenschaftendateien verwenden:

<pre>
<code>
  offset.storage.topic=connect-offsets
  config.storage.topic=connect-configs
  status.storage.topic=connect-status
</code>
</pre>
{:codeblock}

Wenn Sie Kafka Connect nicht häufig verwenden, können Sie die Anzahl der Partitionen reduzieren.



