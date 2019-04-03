---

copyright:
  years: 2015, 2019
lastupdated: "2018-06-25"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Kafka Streams mit {{site.data.keyword.messagehub}} verwenden
{: #kafka_streams }

Die Topic-APIs können ohne weitere Konfigurationsschritte zusammen mit {{site.data.keyword.messagehub}} verwendet werden. Geben Sie Ihre SASL-Berechtigungsnachweise in <code>sasl.jaas.config</code> oder in einer JAAS-Datei an und legen Sie für <code>replication.factor</code> den Wert 3 fest.
{: shortdesc}

Stellen Sie sicher, dass Sie Streams der Version 0.10.2 oder höher verwenden.   

Beispiel:

<pre>
<code>
    props.put(StreamsConfig.REPLICATION_FACTOR_CONFIG, "3");
    props.put(StreamsConfig.BOOTSTRAP_SERVERS_CONFIG, BOOTSTRAP_SERVERS);
    props.put("security.protocol","SASL_SSL");
    props.put("sasl.mechanism","PLAIN");
    props.put("ssl.protocol","TLSv1.2");
    props.put("ssl.enabled.protocols","TLSv1.2");
    props.put("sasl.jaas.config","org.apache.kafka.common.security.plain.PlainLoginModule required username=\"USERNAME\" password=\"PASSWORD\";");
</code>
</pre>
{:codeblock}

Dabei sind BOOTSTRAP_SERVERS, USERNAME und PASSWORD die Werte auf der {{site.data.keyword.messagehub}}-Registerkarte **Serviceberechtigungsnachweise** in
{{site.data.keyword.Bluemix_notm}}.

<!--
new topic that includes content from existing topics about samples and migration
-->
