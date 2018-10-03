---

copyright:
  years: 2015, 2018
lastupdated: "2018-06-22"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Kafka-Java-Client verwenden
{: #kafka_using}

<!-- 21/06/18 - removing until some content ready

## To do: instructions for getting started, with links for more information


## To do: simple send source and receive source in-line


## How to use, download, and run the Java Kafka API sample

-->

Das Java&trade;-Kafka-API-Beispiel ist ein in Java geschriebenes Beispiel für einen Producer und einen Consumer unter Verwendung der Kafka-API. Dieses Beispiel kann lokal oder in {{site.data.keyword.Bluemix_short}} ausgeführt werden.

Der Beispielcode befindet sich im [GitHub-Projekt 'event-streams-samples' ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-console-sample){:new_window}. Obwohl das Beispiel die Kafka-API zum Senden und Empfangen von Nachrichten nutzt, verwendet es die {{site.data.keyword.messagehub}}-Verwaltungs-API zum Erstellen des Topics, an das es Nachrichten sendet und von dem es Nachrichten empfängt.

Weitere Informationen zum Einrichten und Ausführen des Beispiels finden Sie in der Datei [README.md ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-console-sample){:new_window}.

Eine schrittweise Anleitung zum Ausführen des Beispiels finden Sie in [Einführung in {{site.data.keyword.messagehub}}](/docs/services/EventStreams/index.html#getting_started_steps).

## Liberty for Java-Beispiel verwenden, herunterladen und ausführen
{: #liberty_sample notoc}

Das Liberty for Java-Beispiel implementiert eine einfache Anwendung, die in der Liberty-Laufzeitumgebung bereitgestellt wird. Die Anwendung verwendet die Kafka-API für {{site.data.keyword.messagehub}} zum Erstellen und Verarbeiten von Nachrichten.
Die Anwendung stellt darüber hinaus ein Web-Front-End bereit, das Sie für die Verwaltung verwenden können.

Den zugehörigen Beispielcode finden Sie im [GitHub-Projekt 'event-streams-samples' ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-liberty-sample){:new_window}.

<!--
17/10/17 - Karen: following info duplicated at messagehub063 
-->

## Eigenschaft 'sasl.jaas.config' verwenden
{: #sasl_prop notoc}
Wenn Sie einen Kafka-Client der Version 0.10.2.1 oder höher verwenden, können Sie die Eigenschaft <code>sasl.jaas.config</code> anstelle einer JAAS-Datei
für die Clientkonfiguration verwenden. Um eine Verbindung zu {{site.data.keyword.messagehub}} herzustellen, legen Sie
<code>sasl.jaas.config</code> wie folgt fest:
<pre>
<code>    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required \
    username="USERNAME" \
    password="PASSWORD";</code>
</pre>
{:codeblock}

Dabei sind USERNAME und PASSWORD die Werte auf der {{site.data.keyword.messagehub}}-Registerkarte **Serviceberechtigungsnachweise** in {{site.data.keyword.Bluemix_notm}}.

Wenn Sie <code>sasl.jaas.config</code> verwenden, können Clients, die in derselben JVM ausgeführt werden, verschiedene Berechtigungsnachweise verwenden. Weitere
Informationen finden Sie unter [Kafka-Clients konfigurieren ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://kafka.apache.org/documentation/#security_sasl_plain_clientconfig){:new_window}.

Für einen älteren Kafka-Client müssen Sie eine JAAS-Konfigurationsdatei zur Angabe der Berechtigungsnachweise verwenden. Dieses Verfahren ist weniger benutzerfreundlich, daher wird stattdessen die Verwendung der Eigenschaft <code>sasl.jaas.config</code> empfohlen.

<!--
23/04/18 - Karen: following migration info on production in messagehub084 
-->

## Einen Kafka-Client von 0.9.X oder 0.10.X auf höhere Clientversionen migrieren
{: #kafka_migrate}


Wenn Sie mit den Java-Clients arbeiten, können Sie die offiziell verfügbaren Kafka-Clients der Version 0.10 oder höher verwenden. Es wird dringend empfohlen, von der Version 0.9.X auf die aktuelle Version umzustellen. Sie können einen Kafka-Client von [https://kafka.apache.org/downloads ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://kafka.apache.org/downloads){:new_window} herunterladen. 



### Einen Kafka-Client von 0.10.2.X auf höhere Versionen migrieren

Ab 0.10.2 können Sie die SASL-Authentifizierung direkt in den Eigenschaften des Clients konfigurieren, anstatt eine JAAS-Datei zu verwenden. Mit dieser Vereinfachung können Sie mehrere Clients in derselben JVM mit unterschiedlichen Berechtigungsnachweisen ausführen, was bei einer JAAS-Datei nicht möglich ist.

Führen Sie die folgenden Schritte aus:

1. Löschen Sie die JAAS-Datei. Beachten Sie, dass die JVM-Eigenschaft java.security.auth.login.config=<PATH TO JAAS> nicht mehr notwendig ist.
2. Wenn Sie von der Version 0.9.X migrieren, löschen Sie das JAR-Modul für die {{site.data.keyword.messagehub}}-Anmeldung.
2. Fügen Sie die folgenden Clienteigenschaften hinzu:
    ```
	sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USERNAME" password="PASSWORD";
	```

	Dabei sind USERNAME und PASSWORD die Werte auf der {{site.data.keyword.messagehub}}-Registerkarte **Serviceberechtigungsnachweise** in {{site.data.keyword.Bluemix_notm}}.
	
	

### Einen Kafka-Client von 0.9.X auf 0.10.0.X oder 0.10.1.X migrieren

Führen Sie die folgenden Schritte aus:

1. Löschen Sie das JAR-Modul für die {{site.data.keyword.messagehub}}-Anmeldung.
2. Ändern Sie Ihre Datei <code>jaas.conf</code> wie folgt:
    ```
        KafkaClient {
          org.apache.kafka.common.security.plain.PlainLoginModule required
          serviceName="kafka"
            username="USERNAME"
            password="PASSWORD";
        };
    ```
    {: codeblock}

	Dabei sind USERNAME und PASSWORD die Werte auf der {{site.data.keyword.messagehub}}-Registerkarte **Serviceberechtigungsnachweise** in {{site.data.keyword.Bluemix_notm}}.
	
3. Fügen Sie die folgende Zeile in Ihren Eigenschaften 'consumer' und 'producer' hinzu: <code>sasl.mechanism=PLAIN</code>.
