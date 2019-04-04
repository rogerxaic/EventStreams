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

# Utilizzo di KSQL con {{site.data.keyword.messagehub}}
{: #ksql_using}

Puoi utilizzare [KSQL ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/confluentinc/ksql){:new_window} con {{site.data.keyword.messagehub}} per l'elaborazione del flusso. Assicurati di utilizzare KSQL 0.4 o successiva. 
{: shortdesc}

Completa la seguente procedura:

1. Crea un file di configurazione KSQL {{site.data.keyword.messagehub}}.

    Ad esempio:
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
    dove BOOTSTRAP_SERVERS, USERNAME e PASSWORD sono i valori indicati nella scheda **Credenziali del servizio** {{site.data.keyword.messagehub}} in
{{site.data.keyword.Bluemix_notm}}.

2. Utilizza il dashboard {{site.data.keyword.messagehub}} nella console {{site.data.keyword.Bluemix_notm}} per creare un argomento denominato
<code>ksql__commands</code> con una sola partizione e il periodo di conservazione predefinito.
3. Da un terminale Docker, avvia KSQL utilizzando il seguente comando:
<pre class="pre">/bin/ksql-cli local 
--<var class="keyword varname">properties-file</var> ./config/ksqlserver.properties
</pre>
4. Utilizza il dashboard {{site.data.keyword.messagehub}} nella console {{site.data.keyword.Bluemix_notm}} per creare due argomenti con una partizione ognuno: <code>users</code> e <code>pageviews</code>.

    Quindi avvia <code>DataGen</code> due volte nel seguente modo:
	
    i. Con <code>bootstrap-server=HOSTNAME:PORTNUMBER quickstart=users format=json topic=users maxInterval=10000</code> per avviare la creazione di eventi <code>users</code>.
	
    ii. Con <code>bootstrap-server=HOSTNAME:PORTNUMBER quickstart=pageviews format=delimited topic=pageviews maxInterval=10000</code> per avviare la creazione di eventi <code>pageviews</code>.
	
	Assicurati di aver inserito tutti gli host Kafka elencati nella pagina **Credenziali del servizio** come valori per <code>bootstrap-server</code>. Per trovare queste informazioni, vai alla tua istanza
	{{site.data.keyword.messagehub}} in {{site.data.keyword.Bluemix_notm}}, passa alla scheda **Credenziali del servizio** e seleziona le
	**Credenziali** che desideri utilizzare.

Dopo aver completato questi passi, puoi eseguire tutte le query elencate in [KSQL Quick Start Guide ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/confluentinc/ksql/tree/0.1.x/docs/quickstart#create-a-stream-and-table){:new_window}

