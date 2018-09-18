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

# Utilizzo del client Java Kafka
{: #kafka_using}

<!-- 21/06/18 - removing until some content ready

## To do: instructions for getting started, with links for more information


## To do: simple send source and receive source in-line


## How to use, download, and run the Java Kafka API sample

-->

L'esempio di API Kafka Java&trade; consiste in un produttore e consumatore di esempio scritto in Java, che utilizza direttamente l'API Kafka. Puoi eseguire questo esempio in locale o in {{site.data.keyword.Bluemix_short}}.

Il codice di esempio si trova nel [progetto GitHub message-hub-samples ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-messaging/message-hub-samples/tree/master/kafka-java-console-sample){:new_window}. Anche se l'esempio utilizza l'API Kafka per inviare e ricevere messaggi, l'esempio utilizza l'API di amministrazione {{site.data.keyword.messagehub}} per creare l'argomento a cui invia i messaggi e da cui riceve i messaggi.

Per ulteriori informazioni sulla configurazione e sull'esecuzione dell'esempio, vedi il file [README.md ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-messaging/message-hub-samples/tree/master/kafka-java-console-sample){:new_window}.

Per una procedura dettagliata su come eseguire l'esempio, vedi [Introduzione a {{site.data.keyword.messagehub}}](/docs/services/MessageHub/index.html#getting_started_steps).

## Come utilizzare, scaricare ed eseguire l'esempio di Liberty for Java
{: #liberty_sample notoc}

L'esempio di Liberty for Java implementa una semplice applicazione distribuita sul runtime Liberty. L'applicazione utilizza l'API Kafka per {{site.data.keyword.messagehub}} per produrre e consumare messaggi.
L'applicazione offre anche un frontend web che puoi utilizzare per le attività di amministrazione.

Puoi trovare il codice di esempio nel [progetto GitHub message-hub-samples ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-messaging/message-hub-samples/tree/master/kafka-java-liberty-sample){:new_window}.

<!--
17/10/17 - Karen: following info duplicated at messagehub063 
-->

## Utilizzo della proprietà sasl.jaas.config
{: #sasl_prop notoc}
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

<!--
23/04/18 - Karen: following migration info on production in messagehub084 
-->

## Migrazione di un client Kafka da 0.9.X o 0.10.X a versioni de client successive
{: #kafka_migrate}


Se stai utilizzando i client Java, puoi usare i client Kafka pubblicamente disponibili
alla 0.10 o successiva. Si consiglia vivamente di passare dalla 0.9.X
alla versione più recente. Puoi scaricare un client Kafka da
[https://kafka.apache.org/downloads ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://kafka.apache.org/downloads){:new_window} 



### Migrazione di un client Kafka a 0.10.2.X o versioni successive

dalla 0.10.2, puoi configurare l'autenticazione SASL direttamente nelle proprietà del cliente invece di utilizzare un file JAAS. Questa semplificazione ti consente di eseguire più clienti nella stessa JVM utilizzando diverse serie di credenziali, cosa non possibile con un file JAAS.

Completa la seguente procedura:

1. Elimina il file JAAS. Nota che la proprietà JVM java.security.auth.login.config=<PATH TO JAAS> non è più necessaria.
2. Se stai migrando dalla 0.9.X, elimina il modulo jar di login {{site.data.keyword.messagehub}}.
2. Aggiungi quanto segue alle proprietà del client:
    ```
	sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USERNAME" password="PASSWORD";
	```

	dove USERNAME e PASSWORD sono i valori indicati nella scheda **Credenziali del servizio** {{site.data.keyword.messagehub}} in {{site.data.keyword.Bluemix_notm}}.
	
	

### Migrazione di un client Kafka da 0.9.X a 0.10.0.X o 0.10.1.X

Completa la seguente procedura:

1. Elimina il modulo jar di login {{site.data.keyword.messagehub}}.
2. Modifica il tuo file <code>jaas.conf</code> nel seguente modo:
    ```
        KafkaClient {
          org.apache.kafka.common.security.plain.PlainLoginModule required
          serviceName="kafka"
            username="USERNAME"
            password="PASSWORD";
        };
    ```
    {: codeblock}

	dove USERNAME e PASSWORD sono i valori indicati nella scheda **Credenziali del servizio** {{site.data.keyword.messagehub}} in {{site.data.keyword.Bluemix_notm}}.
	
3. Aggiungi questa riga alle proprietà di consumatore e produttore: <code>sasl.mechanism=PLAIN</code>
