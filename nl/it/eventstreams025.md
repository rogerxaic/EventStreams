---

copyright:
  years: 2015, 2019
lastupdated: "2018-05-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Utilizzo dell'API REST Kafka
{: #rest_using}

** L'API Kafka REST è disponibile solo come parte del piano Standard.**
<br/>

L'API REST Kafka fornisce un'interfaccia RESTful a un cluster Kafka. Puoi produrre e consumare messaggi utilizzando l'API. Per ulteriori informazioni, inclusa la documentazione di riferimento dell'API, consulta la [Documentazione Kafka REST Proxy ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://docs.confluent.io/2.0.0/kafka-rest/docs/index.html){:new_window}. Solo il formato integrato binario è supportato per le richieste e le risposte in {{site.data.keyword.messagehub}}. I formati incorporati Avro e JSON non sono supportati.
{: shortdesc}

Se stai utilizzando CURL, puoi utilizzare un esempio come il seguente per produrre:
<pre class="pre"><code>
curl -X POST -H "X-Auth-Token:<var class="keyword varname">APIKEY</var>" -H "Content-Type: application/vnd.kafka.binary.v1+json" <var class="keyword varname">kafka-rest endpoint</var>/topics/<var class="keyword varname">topic name</var> -d 

'
{
  "records": [
    {
      "value": "<var class="keyword varname">A base 64 encoded value string</var>"
    }
  ]
}
'
</code></pre>
{: codeblock}
<br/>
<br/>
Se stai utilizzando CURL, puoi utilizzare un esempio come il seguente per utilizzare:
<pre class="pre"><code>
curl -X GET -H "X-Auth-Token:<var class="keyword varname">APIKEY</var>" -H "Accept: application/vnd.kafka.binary.v1+json" <var class="keyword varname">kafka-rest endpoint</var>/topics/<var class="keyword varname">topic name</var>/partitions/<var class="keyword varname">partition ID</var>/messages?offset=<var class="keyword varname">offset to start from</var>
</code></pre>
{: codeblock}

<br/>
<br/>
Per CURL, puoi anche adattare gli esempi di codice
descritti nella [Documentazione di Confluent ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://docs.confluent.io/2.0.0/){:new_window} aggiungendo la seguente riga alla riga di comando:
<pre class="pre">-H "X-Auth-Token: <var class="keyword varname">APIKEY</var>"</pre>
{: codeblock}

<br/>
## Come effettuare la connessione e l'autenticazione
{: #rest_connect}

<!-- info was in eventstreams066.md -->

<!-- Comment from Andrew
basic introduction, definitely including health warning
-->
Per stabilire una connessione a {{site.data.keyword.messagehub}}, l'API REST Kafka utilizza le credenziali <code>api_key</code> e <code>kafka_rest_url</code>
dalla [variabile di ambiente VCAP_SERVICES](/docs/services/EventStreams/eventstreams127.html).

Per eseguire l'autenticazione con l'API REST Kafka {{site.data.keyword.messagehub}}, devi specificare <code>api_key</code> nell'intestazione X-Auth-Token delle tue richieste.


## Come utilizzare l'API
{: #rest_how}

<!-- info was in eventstreams097.md -->

L'esempio API REST Kafka {{site.data.keyword.messagehub}} è un'applicazione Node.js, che stabilisce una connessione a {{site.data.keyword.messagehub}} sull'API REST Kafka per produrre e consumare messaggi.

Il codice di esempio si trova nel [progetto github event-streams-samples ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample){:new_window}.

Attieniti alle istruzioni contenute nel file [README.md ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample){:new_window} per tale progetto per creare ed eseguire l'esempio.


