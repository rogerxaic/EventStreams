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

# Configurazione del tuo client API Kafka
{: #kafka_connect}


Per stabilire una connessione a {{site.data.keyword.messagehub}}, l'API Kafka utilizza uno dei seguenti insiemi di informazioni delle credenziali: 
* le credenziali <code>kafka_brokers_sasl</code> e utente (<code>user</code>) e <code>password</code> dalla [variabile di ambiente VCAP_SERVICES](/docs/services/EventStreams/eventstreams127.html#vcap).
* la chiave del servizio. Per ulteriori informazioni, vedi [Connessione al tuo cluster](/docs/services/EventStreams/eventstreams127.html#enterprise_connect).
{: shortdesc}

<!--17/10/17 - Karen: following info duplicated at messagehub104 -->
## Utilizzare la proprietà sasl.jaas.config (per la connessione e autenticazione in un'applicazione Java)
{: #kafka_java notoc}
Se utilizzi un client Kafka alla versione 0.10.2.1 o successiva, per la configurazione del client puoi usare la proprietà <code>sasl.jaas.config</code> anziché un file JAAS. Per stabilire una connessione a {{site.data.keyword.messagehub}}, imposta <code>sasl.jaas.config</code> come segue:
<pre>
<code>    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required \
    username="USERNAME" \
    password="PASSWORD";</code>
</pre>
{:codeblock}

dove USERNAME e PASSWORD sono i valori indicati nella scheda **Credenziali del servizio** {{site.data.keyword.messagehub}} in {{site.data.keyword.Bluemix_notm}}.

Se utilizzi <code>sasl.jaas.config</code>, i client in esecuzione nella stessa JVM possono utilizzare credenziali diverse. Per ulteriori informazioni, vedi
[Configurazione dei client Kafka ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://kafka.apache.org/documentation/#security_sasl_plain_clientconfig){:new_window}

Per un precedente client Kafka, devi utilizzare un file di configurazione JAAS per specificare le credenziali. Questo meccanismo è meno conveniente quindi ti raccomandiamo invece di utilizzare la proprietà <code>sasl.jaas.config</code>.
## Connessione e autenticazione a un'applicazione diversa da Java
{: #kafka_notjava notoc}

Il servizio {{site.data.keyword.messagehub}} al momento
autentica i client utilizzando SASL PLAIN su TLS. Le credenziali vengono riportate tramite una connessione crittografata.
Questa è una nuova funzione aggiunta in Kafka 0.10.0.X. 

Il seguente esempio è un file di configurazione semplice denominato <code>consumer.properties</code>:

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

Qualsiasi client che supporti Kafka 0.10 con SASL PLAIN
dovrebbe funzionare con {{site.data.keyword.messagehub}}. I client di esempio sono i seguenti:

* [librdkafka ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/edenhill/librdkafka/){:new_window} 
* [confluent-kafka-python ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/confluentinc/confluent-kafka-python){:new_window} 

Se utilizzi il client Kafka 0.9.0.0 precedente, devi usare un modulo di login personalizzato che puoi
scaricare da [{{site.data.keyword.messagehub}} login module ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-0.9/message-hub-login-library/messagehub.login-1.0.0.jar){:new_window}. 

