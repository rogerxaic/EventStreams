---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-15"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Kafka-Java-Client beim Plan "Classic" migrieren 
{: #kafka_java_migrating_classic}


## Kafka-Java-Client von 0.9.X oder 0.10.X auf höhere Clientversionen migrieren
{: #kafka_migrate_classic}


Wenn Sie mit den Java-Clients arbeiten, können Sie die offiziell verfügbaren Kafka-Clients der Version 0.10 oder höher verwenden. 

Es wird dringend empfohlen, von der Version 0.9.X auf die aktuelle Version umzustellen. Sie können einen Kafka-Client von
[https://kafka.apache.org/downloads ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://kafka.apache.org/downloads){:new_window} herunterladen.

Informationen zu den Auswirkungen der Verwendunhg eines Clients der Verison 0.9.X finden Sie in
[Kompatibilität mit früheren Versionen](/docs/services/EventStreams?topic=eventstreams-kafka_clients#compatibility).



### Kafka-Java-Client auf 0.10.2.X oder höhere Versionen migrieren

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
