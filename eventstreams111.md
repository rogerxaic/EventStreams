---

copyright:
  years: 2015, 2020
lastupdated: "2020-03-04"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: EventStreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Using Kafka Streams with {{site.data.keyword.messagehub}}
{: #kafka_streams }

Kafka Streams is a stream processing library that is part of the Apache Kafka project. It simplifies building applications that process and analyze data in Kafka. 

The topic APIs work with {{site.data.keyword.messagehub}} with no setup required. Specify your SASL credentials using <code>sasl.jaas.config</code> or a JAAS file and set <code>replication.factor</code> to 3.
{: shortdesc}

Ensure that you are using Streams at 0.10.2, or later.   

For example:

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

where BOOTSTRAP_SERVERS, USERNAME, and PASSWORD are the values from your {{site.data.keyword.messagehub}} **Service Credentials** tab in
{{site.data.keyword.Bluemix_notm}}.

<!--
new topic that includes content from existing topics about samples and migration
-->
