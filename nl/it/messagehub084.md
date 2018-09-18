---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-08"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Migrazione di un client Kafka da 0.9.X o 0.10.X a versioni de client successive
{: #kafka_migrate}


Se stai utilizzando i client Java, puoi usare i client Kafka pubblicamente disponibili
alla 0.10 o successiva. Si consiglia vivamente di passare dalla 0.9.X
alla versione più recente. Puoi scaricare un client Kafka da
[https://kafka.apache.org/downloads ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://kafka.apache.org/downloads){:new_window} 


## Migrazione di un client Kafka a 0.10.2.X o versioni successive

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
	
	
## Migrazione di un client Kafka da 0.9.X a 0.10.0.X o 0.10.1.X

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


