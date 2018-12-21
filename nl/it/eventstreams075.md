---

copyright:
  years: 2015, 2018
lastupdated: "2018-11-20"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Utilizzo dell'API MQ Light 
{: #mql_using}

** L'API MQ Light è disponibile solo come parte del piano Standard.**
<br/>

L'API {{site.data.keyword.mql}} viene fornita per la compatibilità con le versioni precedenti del servizio {{site.data.keyword.mql}}. L'API fornisce un'interfaccia di messaggistica basata su AMQP per Java&trade;, Node.js, Python e Ruby.
{:shortdesc}

<!-- 02/07/18 - removing words to help deprecate MQ Light
In most cases, {{site.data.keyword.messagehub}} is best used with a Kafka client. The {{site.data.keyword.mql}} API is simple to learn but has very limited scalability and does not offer interoperability with other {{site.data.keyword.messagehub}} APIs.
The {{site.data.keyword.mql}} API is available in the following {{site.data.keyword.Bluemix_short}} regions only: US South, United Kingdom, and Sydney. The {{site.data.keyword.mql}} API not available in the Germany region or in {{site.data.keyword.Bluemix_notm}} Dedicated.
-->

## Che cos'è l'API MQ Light e in che cosa è differente?
{: #mqlight}

<!-- 30/10/18: info was in eventstreams106.md, moved because of doc app changes -->

L'API {{site.data.keyword.mql}} fornisce un livello di astrazione superiore per la messaggistica, rispetto a Kafka.


La scelta tra l'utilizzo di un client Kafka o dell'API {{site.data.keyword.mql}} dipende dalla topologia di messaggistica che vuoi
creare:

* Con Kafka, utilizzi un numero ridotto di argomenti e puoi avere più partizioni per ogni argomento per una maggiore scalabilità. Puoi condividere i messaggi tra i consumatori utilizzando gruppi di consumatori, ma ogni consumatore deve essere in grado di mantenere il passo con la frequenza dei messaggi per le partizioni assegnate.
* Con l'API {{site.data.keyword.mql}}, puoi utilizzare un numero molto maggiore di argomenti e i nomi degli argomenti sono gerarchici (ad esempio: <code>&lsquo;/sports/football&rsquo;</code> e <code>&lsquo;/sports/tiddlywinks&rsquo;</code>).  

Gli argomenti nell'API {{site.data.keyword.mql}} non sono gli stessi
argomenti presenti in Kafka. Al contrario, la API {{site.data.keyword.mql}} utilizza
un singolo argomento Kafka chiamato MQLight e tutti i messaggi inviati e ricevuti utilizzando la API {{site.data.keyword.mql}} passano attraverso quel solo argomento Kafka.

{{site.data.keyword.mql}} è disponibile solo nelle seguenti ubicazioni
{{site.data.keyword.Bluemix_notm}} (regioni): Dallas (us-south), Londra (eu-gb) e Sydney (au-syd). L'API MQ Light non è disponibile nell'ubicazione Francoforte (eu-de) o in
{{site.data.keyword.Bluemix_notm}} dedicato.

Per ulteriori informazioni sulla scelta tra le API, consulta [Scelta tra le tre API](/docs/services/EventStreams/eventstreams087.html).


## Cosa è richiesto per utilizzare l'API MQ Light con {{site.data.keyword.messagehub}}?
{: #mql_reqs}

<!-- 30/10/18: info was in eventstreams098.md, moved because of doc app changes -->

Per utilizzare l'API {{site.data.keyword.mql}} con {{site.data.keyword.messagehub}} sono necessari questi requisiti: 

**Prima di poter utilizzare l'API, devi creare in modo esplicito un argomento Kafka denominato "MQLight" in quanto tutti i messaggi passano attraverso l'argomento "MQLight". Questo argomento deve avere una singola partizione. La creazione di questo argomento abilita l'API MQ Light per la tua istanza del servizio. Gli argomenti usati nell'API MQ Light vengono creati automaticamente mentre li utilizzi, ma tutti i messaggi sono al momento nel solo argomento Kafka "MQLight".** 

L'argomento "MQLight" è utilizzato dall'API MQ Light per memorizzare i dati di messaggi e per interagire con altri client Kafka. Tieni presente che quando si crea
questo argomento, vengono applicati dei costi alla tariffa standard descritta nel piano di pagamento dei servizi.

Per disabilitare l'API MQ Light, elimina l'argomento "MQLight". Ricorda che all'eliminazione dell'argomento vengono eliminati tutti i dati.

<!-- 12/11/18: following info was in eventstreams079.md, moved because of doc app changes -->

## Come effettuare la connessione e l'autenticazione
{: #mql_connect}

Per collegare un'applicazione al servizio, l'applicazione deve utilizzare i dettagli <code>user</code>,
<code>password</code> e <code>mqlight_lookup_url</code> dalla [variabile di ambiente VCAP_SERVICES](/docs/services/EventStreams/eventstreams127.html). Utilizza le seguenti indicazioni per il linguaggio che hai scelto:

**Per Java**

Se si specifica &lsquo;null&rsquo; come parametro endpointService della chiamata create(), questo indica al
client di leggere i dettagli <code>user</code>, <code>password</code> e 
<code>mqlight_lookup_url</code> da VCAP_SERVICES:

<pre>
<code>NonBlockingClient.create(null, new NonBlockingClientAdapter<Void>() {
    public void onStarted(NonBlockingClient client, Void context) {
        client.send("my/topic", "Hello World!", null);
    }
}, null);</code>
</pre>
{:codeblock}

<br>

**Per Node.js**

Richiama i dettagli <code>user</code>, <code>password</code> e
<code>mqlight_lookup_url</code> da VCAP_SERVICES e usali per creare il client
nel seguente modo:

<pre>
<code>var services = JSON.parse(process.env.VCAP_SERVICES);
mqlightService = services['messagehub'][0];
opts.service = mqlightService.credentials.mqlight_lookup_url;
opts.user = mqlightService.credentials.user;
opts.password = mqlightService.credentials.password;
var mqlightClient = mqlight.createClient(opts, function(err) {
...</code>
</pre>
{:codeblock}

<br>

**Per Ruby**

Richiama i dettagli <code>user</code>, <code>password</code> e
<code>mqlight_lookup_url</code> da VCAP_SERVICES e usali per creare il client
nel seguente modo:
<pre>
<code>vcap_services = JSON.parse(ENV['VCAP_SERVICES'])
conn_details = vcap_services['messagehub']
credentials = conn_details.first['credentials']
service = credentials['mqlight_lookup_url']
opts[:user] = credentials['user']
opts[:password] = credentials['password']
set :client, Mqlight::BlockingClient.new(service, opts)
...</code>
</pre>
{:codeblock}

<br>

**Per Python**

Richiama i dettagli <code>user</code>, <code>password</code> e
<code>mqlight_lookup_url</code> da VCAP_SERVICES e usali per creare il client
nel seguente modo:
<pre>
<code>vcap_services = json.loads(os.environ.get('VCAP_SERVICES'))
conn_details = vcap_services['messagehub'][0]
service = str(conn_details['credentials']['mqlight_lookup_url'])
security_options = {
      'user': str(conn_details['credentials']['user']),
      'password': str(conn_details['credentials']['password'])
}
client = mqlight.Client(service=service, 
                        security_options=security_options,
                        on_started=on_started)</code>
</pre>
{:codeblock}

<br>

Per ulteriori informazioni sulle API {{site.data.keyword.mql}},
consulta il [sito di {{site.data.keyword.mql}} developerWorks&reg; ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://developer.ibm.com/messaging/mq-light/){:new_window}.


<!-- 14/11/18: info was in eventstreams080.md, moved because of doc app changes -->

## Come connettere applicazioni MQ Light esistenti
{: #mql_exist_apps}


Puoi connettere al servizio delle applicazioni esistenti attualmente in esecuzione in {{site.data.keyword.IBM_notm}} MQ o nel servizio {{site.data.keyword.mql}}
{{site.data.keyword.Bluemix_notm}}. Le applicazioni continueranno a funzionare allo stesso modo.

Per connettere applicazioni esistenti, completa i seguenti controlli:

* Assicurati che l'applicazione utilizzi l'ultima versione disponibile del client API {{site.data.keyword.mql}} per il tuo linguaggio.
* Controlla che i dettagli di connessione estratti da VCAP_SERVICES si riferiscano al tipo di servizio <code>messagehub</code> e che richiamino il nome utente di connessione dalla proprietà <code>credentials.user</code> anziché dalla proprietà <code>credentials.username</code> e richiamino l'URL di ricerca della connessione dalla proprietà <code>credentials.mqlight_lookup_url</code> anziché dalla proprietà <code>credentials.connectionLookupURI</code>. Per ulteriori informazioni, vedi [Variabile di ambiente VCAP_SERVICES](/docs/services/EventStreams/eventstreams127.html).

	Nota che questo passaggio viene completato automaticamente se utilizzi il client Java&trade; e specifichi 'null' come parametro endpointService nella chiamata create(), pertanto è il client stesso a recuperare le informazioni.
	
* La tua applicazione deve supportare le connessioni TLS v1.2. VCAP_SERVICES non contiene più una proprietà <code>credentials.nonTLSConnectionLookupURI</code> per la creazione di una connessione diversa da TLS.

Nota anche le seguenti informazioni:

* I limiti dei messaggi sono coerenti con {{site.data.keyword.messagehub}} ma potrebbero essere diversi da altri server che supportano l'API {{site.data.keyword.mql}}. Per ulteriori informazioni, vedi [Limiti massimi](/docs/services/EventStreams/eventstreams075.html#max_limits).
* JMS non è supportato.

<!-- 15/11/18: info was in eventstreams081.md, moved because of doc app changes -->

## Scambio di messaggi tra l'API MQ Light e le API Kafka o REST Kafka
{: #mql_exchange}

I messaggi di {{site.data.keyword.mql}} vengono memorizzati in un singolo argomento Kafka sottostante denominato "MQLight" e sono codificati come descritto nella seguente tabella. Questa codifica può essere utilizzata anche da altri tipi di API, come Kafka o REST Kafka, per scambiare messaggi con le applicazioni che utilizzano
l'API {{site.data.keyword.mql}}.

### Formato dei messaggi Kafka
{: #kafka_format notoc}

<table border='1'>
<caption>Tabella 1. Formato dei messaggi Kafka</caption>
  <tr>
    <th> Chiave </th>
    <th> Valore </th>
  </tr>
  <tr>
    <td> Facoltativo (non utilizzato dall'API)
	<p></p>
	</td>
    <td>**1 byte**
	<p>		     Identificativo di struttura API MQ Light, che è sempre 0xFA.</p>
    <p><var class="keyword varname">**n**</var> **bytes**</p>
    <p>		    Messaggio con codifica AMQP (formattato in base al formato wire AMQP). </p></td>
  </tr>
</table>


<!-- 15/11/18: info was in eventstreams082.md, moved because of doc app changes -->

## Esempi di API MQ Light
{: #mql_samples}

Se non hai ancora un'applicazione, prova l'API {{site.data.keyword.mql}} con uno degli esempi.

L'applicazione di esempio comprende in realtà due semplici applicazioni: un frontend web che invia i messaggi a un
backend e un backend che elabora i messaggi, converte in maiuscolo le parole e quindi restituisce i
messaggi al frontend. Questo esempio mostra come puoi fare in modo che le applicazioni parlino tra di loro utilizzando
l'API {{site.data.keyword.mql}}. Inoltre, puoi utilizzare l'API {{site.data.keyword.mql}} per effettuare operazioni di scaricamento del carico di lavoro,
una funzionalità chiave necessaria per creare applicazioni scalabili, indipendenti e distribuite.

Puoi trovare il codice di esempio nel [progetto event-streams-samples GitHub ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-messaging/event-streams-samples/tree/master/mqlight){:new_window}.


<!-- 15/11/18: info was in eventstreams083.md, moved because of doc app changes -->

## Limiti massimi
{: #max_limits}

Per l'API {{site.data.keyword.mql}} vengono applicati seguenti limiti:

* La quantità massima di dati che è possibile memorizzare è coerente con una singola partizione Kafka per il tuo piano di pagamento (in genere 1 GB). Se questo limite di dati viene superato, i messaggi più vecchi nella partizione vengono rimossi quando vengono inviati nuovi messaggi.
* Il tempo massimo per la memorizzazione di un messaggio è coerente con una singola partizione Kafka per il tuo piano di pagamento (in genere 24 ore). Non puoi recuperare i messaggi più vecchi di questo periodo di tempo.
* La dimensione massima di un messaggio (escludendo le intestazioni) è di 1 MB.
* Il numero massimo di client che possono essere collegati in una sola volta è 25.
* Il numero massimo di destinazioni che possono essere attive in una sola volta è 25. Una destinazione attiva è definita come segue:
  - Una destinazione con un TimeToLive > 0 con o senza un client attualmente connesso.
  - Una destinazione con un TimeToLive = 0 (valore predefinito) in cui è connesso un client. 
  <p>Una destinazione condivisa tra più client conta come una singola destinazione.</p>
