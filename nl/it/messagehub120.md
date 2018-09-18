---

copyright:
  years: 2015, 2018
lastupdated: "2018-04-12"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Introduzione al programma Alpha 
{: #alpha_program}

Il programma Alpha fornisce un accesso anticipato alle nuove versioni del servizio {{site.data.keyword.messagehub}}. 

Completa la seguente procedura per ottenere un'applicazione in esecuzione con {{site.data.keyword.messagehub}} Alpha:


## Crea il servizio {{site.data.keyword.messagehub}} 
{: alpha_create}


  1. Fai clic sul tile **Message Hub vNext - Production**, che è un servizio sperimentale nel catalogo
[ ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.stage1.bluemix.net/catalog/labs/?search=vnext).</li>

  2. Crea un servizio {{site.data.keyword.messagehub}}. Questo servizio fornisce un cluster a singolo tenant nel piano dei prezzi <code>Premium</code> e normalmente servono 1-3 ore per il provisioning. 
 


## Ottieni le credenziali utilizzando l'opzione della console: crea e collega un'applicazione di test 
{: alpha_app}

Avrai bisogno delle credenziali per utilizzare {{site.data.keyword.messagehub}}.
Se non hai ancora un'applicazione che puoi utilizzare, crea un'applicazione di test e utilizzane le credenziali per collegarti a {{site.data.keyword.messagehub}}. Ad esempio, utilizza il servizio **SDK for Node.js** come un'applicazione di test.  

  1. Passa al tile **SDK for Node.js** nel catalogo [ ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.stage1.bluemix.net/catalog/starters/sdk-for-nodejs).
   
  Prima di immettere un nome applicazione, assicurati di selezionare una regione degli Stati Uniti Sud. Crea l'applicazione.

  2. Quando l'applicazione è in esecuzione, fai clic sulla scheda **Connessioni** a sinistra. 

  3. Fai clic sul pulsante **Create connection**.

  4. Seleziona il tuo nuovo servizio {{site.data.keyword.messagehub}} dall'elenco di servizi compatibili completo e fai clic sul pulsante **Connect**.

  5. Nella finestra **Connect IAM-Enabled Service**, accetta i valori predefiniti e fai clic su **Connect**.
  Assicurati che sia stato eseguito il provisioning del tuo servizio {{site.data.keyword.messagehub}} in modo che puoi collegarti ad esso.

  6. Fai clic sulla scheda **Runtime** a sinistra e seleziona la scheda **Variabili di ambiente** al centro. Nella sezione **VCAP_SERVICES**, individua le informazioni <code>kafka_admin_url</code>, <code>apikey</code> e <code>kafka_brokers_sasl</code>, a cui dovrai poter inviare un messaggio.
  
## Ottieni le credenziali utilizzando l'opzione della riga di comando 
In alternativa, puoi ottenere le credenziali necessarie utilizzando la riga di comando. Completa la seguente procedura:

  1. Installa lo strumento della riga di comando {{site.data.keyword.Bluemix_notm}} dalla CLI [{{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/cli/index.html#overview).
  
  2. Accedi alla CLI {{site.data.keyword.Bluemix_notm}}.
  
  3. Utilizza la CLI {{site.data.keyword.Bluemix_notm}} per creare una chiave del servizio con il ruolo di gestore utilizzando il seguente comando: 
  ```
  bx resource service-key-create <NAME> Manager --instance-name <MESSAGEHUB_SERVICE_INSTANCE_NAME>
  ```
  4. L'output contiene l'apikey, l'URL dell'endpoint REST di gestione e l'elenco dei broker. Puoi visualizzare queste informazioni nuovamente immettendo il seguente comando: 
  ```
  bx resource service-key <NAME>
  ```

## Crea un argomento {{site.data.keyword.messagehub}} e invia un messaggio

Puoi utilizzare un comando CURL per creare un argomento e poi lo strumento kafkacat per produrre e utilizzare un messaggio.  

Per ogni comando, sostituisci APIKEY e KAFKA_ADMIN_URL con i valori dalla tua variabile di ambiente VCAP_SERVICES. 

  1. Dalla riga comandi, crea un argomento {{site.data.keyword.messagehub}} utilizzando il seguente comando CURL: 
  
    ```
    curl -i -X POST -H "Content-Type: application/json" -H "X-Auth-Token: APIKEY" --data '{ "name": "newtop"}' KAFKA_ADMIN_URL/admin/topics
    ```
    {: codeblock}

  2. Installa lo [strumento kafkacat![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/edenhill/kafkacat#install), che è utile per un test rapido di Kafka.
  
  3. Per eseguire i prossimi comandi, hai bisogno delle seguenti informazioni: 
  
    * Il tuo elenco dei broker, restituito nelle tue credenziali `kafka_brokers_sasl`. Il tuo elenco dei broker deve essere un elenco separato da virgole per kafkacat.
	Solo i primi cinque broker sono elencati in VCAP_SERVICES. Se hai più di cinque broker, utilizza un client Kafka per richiamare i dettagli dei tuoi altri broker.  
  
    * La tua <code>apikey</code>. I primi 8 caratteri formano il tuo sasl.username e il resto <code>apikey</code> la tua sasl.password. 
    * L'ubicazione dei tuoi certificati SSL. Ad esempio, su Ubuntu, SSL_CERTS_DIRECTORY è <code>/etc/ssl/certs/</code>
  
  4. Produci alcuni messaggi eseguendo un comando simile al seguente:
  ```
  kafkacat -X "security.protocol=sasl_ssl" -X 'sasl.mechanisms=PLAIN' -X 'sasl.username=<FIRST_8_CHARS_FROM_APIKEY>' -X 'sasl.password=<REMAINING_CHARS_FROM_APIKEY>' -X "ssl.ca.location=<SSL_CERTS_DIRECTORY>" -b <BROKERS_LIST> -P -t <TOPIC_NAME>
    ```
		
	<br/>
Dopo aver eseguito il comando, puoi immettere un testo come <code>HelloWorld</code> nel terminale del produttore. 
  
  5. Utilizza i messaggi eseguendo un comando simile al seguente: 
  ```
  kafkacat -X "security.protocol=sasl_ssl" -X 'sasl.mechanisms=PLAIN' -X 'sasl.username=<FIRST_8_CHARS_FROM_APIKEY>' -X 'sasl.password=<REMAINING_CHARS_FROM_APIKEY>' -X "ssl.ca.location=<SSL_CERTS_DIRECTORY>" -b <BROKERS_LIST> -C -t <TOPIC_NAME> -f 'Topic %t [%p] at offset %o: key %k: %s\n'
  ```
	
	<br/>
  Dovresti vedere <code>HelloWorld</code> nel terminale del consumatore.

