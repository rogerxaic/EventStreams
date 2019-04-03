---

copyright:
  years: 2015, 2019
lastupdated: "2018-02-06"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# KSQL mit {{site.data.keyword.messagehub}} verwenden
{: #ksql_using}

Sie können [KSQL ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/confluentinc/ksql){:new_window} mit {{site.data.keyword.messagehub}} zur Streamverarbeitung verwenden. Stellen Sie sicher, dass Sie KSQL 0.4 oder höher verwenden. 
{: shortdesc}

Führen Sie die folgenden Schritte aus:

1. Erstellen Sie eine {{site.data.keyword.messagehub}}-KSQL-Konfigurationsdatei.

    Zum Beispiel:
    ```
    bootstrap.servers=BOOTSTRAP_SERVERS
    application.id=ksql_server_quickstart
    ksql.command.topic.suffix=commands
    listeners=http://localhost:8080
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    ssl.protocol=TLSv1.2
    ssl.enabled.protocols=TLSv1.2
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USERNAME" password="PASSWORD";
    ksql.sink.replications.default=3
    ```
    Dabei sind BOOTSTRAP_SERVERS, USERNAME und PASSWORD die Werte auf der {{site.data.keyword.messagehub}}-Registerkarte **Serviceberechtigungsnachweise** in
{{site.data.keyword.Bluemix_notm}}.

2. Verwenden Sie das {{site.data.keyword.messagehub}}-Dashboard in der {{site.data.keyword.Bluemix_notm}}-Konsole, um ein Topic mit der Bezeichnung <code>ksql__commands</code> mit einer einzelnen Partition und dem Standardaufbewahrungszeitraum zu erstellen.
3. Starten Sie KSQL über ein Docker-Terminal mit dem folgenden Befehl:
<pre class="pre">/bin/ksql-cli local 
--<var class="keyword varname">Eigenschaftendatei</var>./config/ksqlserver.properties
</pre>
4. Verwenden Sie das {{site.data.keyword.messagehub}}-Dashboard in der {{site.data.keyword.Bluemix_notm}}-Konsole, um zwei Topics mit jeweils einer Partition zu erstellen: <code>users</code> und <code>pageviews</code>.

    Starten Sie dann <code>DataGen</code> zweimal wie folgt:
	
    i. Mit <code>bootstrap-server=HOSTNAME:PORTNUMBER quickstart=users format=json topic=users maxInterval=10000</code>, um die Erstellung von <code>users</code>-Ereignissen zu starten.
	
    ii. Mit <code>bootstrap-server=HOSTNAME:PORTNUMBER quickstart=pageviews format=delimited topic=pageviews maxInterval=10000</code>, um die Erstellung von <code>pageviews</code>-Ereignissen zu starten.
	
	Stellen Sie sicher, dass Sie alle Kafka-Hosts, die auf der Seite **Serviceberechtigungsnachweise** aufgeführt sind, als Werte für <code>bootstrap-server</code> einfügen. Diese Informationen finden Sie in Ihrer {{site.data.keyword.messagehub}}-Instanz in {{site.data.keyword.Bluemix_notm}} auf der Registerkarte **Serviceberechtigungsnachweise** unter den gewünschten **Berechtigungsnachweisen**.

Nachdem Sie diese Schritte ausgeführt haben, können Sie alle Abfragen ausführen, die in der Veröffentlichung [KSQL - Leitfaden für den Schnelleinstieg![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/confluentinc/ksql/tree/0.1.x/docs/quickstart#create-a-stream-and-table){:new_window} aufgeführt sind.

