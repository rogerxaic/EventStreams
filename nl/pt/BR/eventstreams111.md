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

# Usando o Kafka Streams com o {{site.data.keyword.messagehub}}
{: #kafka_streams }

As APIs de tópico funcionam com o {{site.data.keyword.messagehub}} sem nenhuma configuração necessária. Especifique suas credenciais SASL usando
<code>sasl.jaas.config</code> ou um arquivo JAAS e configure <code>replication.factor</code> para 3.
{: shortdesc}

Assegure-se de que você esteja usando o Streams em 0.10.2 ou mais recente.   

Por exemplo:

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

em que BOOTSTRAP_SERVERS, USERNAME e PASSWORD são os valores de sua guia
{{site.data.keyword.messagehub}} **Credenciais de serviço** no
{{site.data.keyword.Bluemix_notm}}.

<!--
new topic that includes content from existing topics about samples and migration
-->
