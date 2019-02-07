---

copyright:
  years: 2015, 2019
lastupdated: "2018-06-22"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Kafka-API-Client konfigurieren
{: #kafka_connect}


Zum Herstellen einer Verbindung zu {{site.data.keyword.messagehub}} verwendet die Kafka-API eines der folgenden Sets von Berechtigungsinformationen: 
* Die <code>kafka_brokers_sasl</code>-Berechtigungsnachweise sowie <code>user</code> und <code>password</code> aus der
[Umgebungsvariablen VCAP_SERVICES](/docs/services/EventStreams/eventstreams127.html#vcap).
* Den Serviceschlüssel. Weitere Informationen finden Sie in [Verbindung zum Cluster herstellen](/docs/services/EventStreams/eventstreams127.html#enterprise_connect).
{: shortdesc}

<!--17/10/17 - Karen: following info duplicated at messagehub104 -->
## Eigenschaft 'sasl.jaas.config' verwenden (Herstellen einer Verbindung und Authentifizierung in einer Java-Anwendung)
{: #kafka_java notoc}
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
## Herstellen einer Verbindung und Authentifizierung in einer Anwendung, bei der es sich nicht um eine Java-Anwendung handelt
{: #kafka_notjava notoc}

Der {{site.data.keyword.messagehub}}-Service verwendet für die Authentifizierung von
Clients gegenwärtig SASL PLAIN over TLS. Berechtigungsnachweise werden über eine verschlüsselte Verbindung übertragen.
Diese neue Funktion wurde in Kafka 0.10.0.X hinzugefügt. 

Im folgenden Beispiel ist eine Beispielkonfigurationsdatei mit dem Namen <code>consumer.properties</code> dargestellt:

```
key.deserializer=org.apache.kafka.common.serialization.StringDeserializer
value.deserializer=org.apache.kafka.common.serialization.StringDeserializer
#
client.id=kafka-java-console-sample-consumer
group.id=kafka-java-console-sample-group
#
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USERNAME" password="PASSWORD";
ssl.protocol=TLSv1.2
ssl.enabled.protocols=TLSv1.2
ssl.endpoint.identification.algorithm=HTTPS
#
# please read the Kafka docs about this setting
auto.offset.reset=latest
```
{: codeblock}

Jeder Client, der Kafka 0.10 mit SASL PLAIN unterstützt,
sollte auch mit {{site.data.keyword.messagehub}} verwendet werden können. Dies gilt beispielsweise für die folgenden Clients:

* [librdkafka ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/edenhill/librdkafka/){:new_window} 
* [confluent-kafka-python ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/confluentinc/confluent-kafka-python){:new_window} 

Wenn Sie den Client der früheren Kafka-Version 0.9.0.0 verwenden, müssen Sie ein angepasstes Anmeldemodul verwenden,
das Sie von [{{site.data.keyword.messagehub}}-Anmeldemodul ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-0.9/message-hub-login-library/messagehub.login-1.0.0.jar){:new_window} herunterladen können. 

