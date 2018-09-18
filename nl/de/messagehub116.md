---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-12"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Kafka-Konsolentools mit {{site.data.keyword.messagehub}} verwenden
{: #kafka_console_tools }

Apache Kafka umfasst verschiedene Konsolentools für einfache Verwaltungs- und Messaging-Operationen. Viele dieser Tools können mit {{site.data.keyword.messagehub}} verwendet werden, {{site.data.keyword.messagehub}} lässt jedoch keine Verbindung zum ZooKeeper-Cluster zu. Durch die Weiterentwicklung von Kafka gilt für zahlreiche Tools, für die bisher eine Verbindung zu ZooKeeper erforderlich war, diese Anforderung nicht mehr.
Die Konsolentools befinden sich im Verzeichnis <code>bin</code> in Ihrem Kafka-Download. Beispiel: [Apache Kafka 0.10.2.X Client ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.apache.org/dyn/closer.cgi?path=/kafka/0.10.2.1/kafka_2.11-0.10.2.1.tgz){:new_window}.

Zur Angabe der SASL-Berechtigungsnachweise für diese Tools erstellen Sie eine Eigenschaftendatei auf der Basis des folgenden Beispiels:

<pre>
<code>
  sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
  security.protocol=SASL_SSL
  sasl.mechanism=PLAIN
  ssl.protocol=TLSv1.2
  ssl.enabled.protocols=TLSv1.2
  ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

Ersetzen Sie USER und PASSWORD durch die Werte auf der {{site.data.keyword.messagehub}}-Registerkarte **Serviceberechtigungsnachweise** in der {{site.data.keyword.Bluemix_notm}}-Konsole.


## Konsolenproducer
{: #console_producer }

Sie können das Konsolenproducer-Tool von Kafka mit {{site.data.keyword.messagehub}} verwenden. Sie müssen eine Liste mit Brokern und SASL-Berechtigungsnachweise angeben.

Nachdem Sie die Eigenschaftendatei erstellt haben, wie zuvor beschrieben, können Sie den Konsolenproducer in einem Terminal wie folgt ausführen:

<pre>
<code>
  $ kafka-console-producer.sh --broker-list KAFKA_BROKERS_SASL --producer.config CONFIG_FILE --topic TOPIC_NAME
</code>
</pre>
{:codeblock}

Ersetzen Sie die folgenden Variablen im Beispiel durch eigene Werte:
* Ersetzen Sie KAFKA_BROKERS_SASL mit dem Wert auf der {{site.data.keyword.messagehub}}-Registerkarte **Serviceberechtigungsnachweise** in der {{site.data.keyword.Bluemix_notm}}-Konsole, und zwar in Form einer Liste aus Host:Port-Paaren, die durch Kommas getrennt sind (z. B. `Host1:Port1,Host2:Port2`). 
* Ersetzen Sie CONFIG_FILE durch den Pfad der Konfigurationsdatei. 

Es können zahlreiche weitere Optionen dieses Tools verwendet werden; ausgenommen sind die Optionen, für die Zugriff auf ZooKeeper erforderlich ist.


## Konsolenconsumer
{: #console_consumer }

Sie können das Konsolenconsumer-Tool von Kafka mit {{site.data.keyword.messagehub}} verwenden. Sie müssen einen Bootstrap-Server und SASL-Berechtigungsnachweise angeben.

Nachdem Sie die Eigenschaftendatei erstellt haben, wie zuvor beschrieben, können Sie den Konsolenconsumer in einem Terminal wie folgt ausführen:

<pre>
<code>
  $ kafka-console-consumer.sh --bootstrap-server KAFKA_BROKERS_SASL --consumer.config CONFIG_FILE --topic TOPIC_NAME 
</code>
</pre>
{:codeblock}

Ersetzen Sie die folgenden Variablen im Beispiel durch eigene Werte:
* Ersetzen Sie KAFKA_BROKERS_SASL mit dem Wert auf der {{site.data.keyword.messagehub}}-Registerkarte **Serviceberechtigungsnachweise** in der {{site.data.keyword.Bluemix_notm}}-Konsole, und zwar in Form einer Liste aus Host:Port-Paaren, die durch Kommas getrennt sind (z. B. `Host1:Port1,Host2:Port2`). 
* Ersetzen Sie CONFIG_FILE durch den Pfad der Konfigurationsdatei. 

Es können zahlreiche weitere Optionen dieses Tools verwendet werden; ausgenommen sind die Optionen, für die Zugriff auf ZooKeeper erforderlich ist.


## Consumergruppen
{: #consumer_groups_tool }

Sie können das Consumergruppentool von Kafka mit {{site.data.keyword.messagehub}} verwenden. Da {{site.data.keyword.messagehub}} keine Verbindung zum ZooKeeper-Cluster zulässt, stehen einige der Optionen nicht zur Verfügung.

Nachdem Sie die Eigenschaftendatei erstellt haben, wie zuvor beschrieben, können Sie die Consumergruppentools in einem Terminal ausführen. Zum Beispiel können Sie die Consumergruppen wie folgt auflisten:

<pre>
<code>
  $ kafka-consumer-groups.sh --bootstrap-server KAFKA_BROKERS_SASL --command-config CONFIG_FILE --list
</code>
</pre>
{:codeblock}

Ersetzen Sie die folgenden Variablen im Beispiel durch eigene Werte:
* Ersetzen Sie KAFKA_BROKERS_SASL mit dem Wert auf der {{site.data.keyword.messagehub}}-Registerkarte **Serviceberechtigungsnachweise** in der {{site.data.keyword.Bluemix_notm}}-Konsole, und zwar in Form einer Liste aus Host:Port-Paaren, die durch Kommas getrennt sind (z. B. `Host1:Port1,Host2:Port2`). 
* Ersetzen Sie CONFIG_FILE durch den Pfad der Konfigurationsdatei.

Mit diesem Tool können Sie auch Details anzeigen, wie z. B. die aktuellen Positionen der Consumer, ihre Verzögerung und die Client-ID für die einzelnen Partitionen einer Gruppe. Beispiel:

<pre>
<code>
  $ kafka-consumer-groups.sh --bootstrap-server KAFKA_BROKERS_SASL --command-config CONFIG_FILE --describe --group GROUP
</code>
</pre>
{:codeblock}

Ersetzen Sie GROUP im Beispiel durch den Gruppennamen, für den Details abgerufen werden sollen. 


## Topics
{: #topics_tool }

Sie können das Topics-Tool von Kafka, `kafka-topics`, nicht mit {{site.data.keyword.messagehub}} verwenden, da für das Tool Zugriff auf ZooKeeper erforderlich ist.

Zur Verwaltung von Topics können Sie jedoch das {{site.data.keyword.messagehub}}-Dashboard in der {{site.data.keyword.Bluemix_notm}}-Konsole oder die REST-API verwenden.


## Kafka-Tool zum Zurücksetzen von Streams
{: #kafka_streams_reset }

Sie können dieses Tool mit {{site.data.keyword.messagehub}} verwenden, um den Verarbeitungsstatus einer Kafka-Streams-Anwendung zurückzusetzen, sodass die Eingabe von Beginn an erneut verarbeitet werden kann. Stellen Sie vor der Ausführung dieses Tools sicher, dass die Streams-Anwendung vollständig gestoppt ist.

Beispiel:

<pre>
<code>
  $ kafka-streams-application-reset.sh --bootstrap-servers KAFKA_BROKERS_SASL --config-file CONFIG_FILE --application-id APP_ID
</code>
</pre>
{:codeblock}

Ersetzen Sie die folgenden Variablen im Beispiel durch eigene Werte:
* Ersetzen Sie KAFKA_BROKERS_SASL mit dem Wert auf der {{site.data.keyword.messagehub}}-Registerkarte **Serviceberechtigungsnachweise** in der {{site.data.keyword.Bluemix_notm}}-Konsole, und zwar in Form einer Liste aus Host:Port-Paaren, die durch Kommas getrennt sind (z. B. `Host1:Port1,Host2:Port2`). 
* Ersetzen Sie CONFIG_FILE durch den Pfad der Konfigurationsdatei. 
* Ersetzen Sie APP_ID durch die ID der Streams-Anwendung.

