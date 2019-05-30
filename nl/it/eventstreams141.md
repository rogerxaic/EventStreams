---

copyright:
  years: 2015, 2019
lastupdated: "2019-01-14"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Scelta di un client Kafka da utilizzare con il piano Classic {{site.data.keyword.messagehub}} 
{: #kafka_clients_classic}

Per utilizzare l'API Kafka con {{site.data.keyword.messagehub}}, scegli uno dei seguenti tipi di client:

* Il client Java ufficiale. È la scelta migliore perché contiene le funzioni più recenti disponibili per Apache Kafka.
* Uno dei [client di terze parti consigliati](/docs/services/EventStreams?topic=eventstreams-kafka_clients#clients_table).

Per entrambi i tipi di client, ti consigliamo di scegliere sempre la versione del client più recente. 
{: shortdesc}

## Requisiti del client per il collegamento a Event Streams

Per il collegamento a {{site.data.keyword.messagehub}}, i client devono supportare l'autenticazione utilizzando il meccanismo SASL Plain e utilizzare l'estensione SNI (Server Name Indication) al protocollo TLSv1.2.

Il protocollo Kafka minimo che supportiamo è 0.10.
	
## Client di terze parti
{: #third_party_clients_classic}

Se non puoi eseguire i client Java ufficiali, ti consigliamo di eseguire uno dei [client di terze parti consigliati](/docs/services/EventStreams?topic=eventstreams-kafka_clients#clients_table), che sono stati tutti ben sperimentati con {{site.data.keyword.messagehub}}. 

Potrebbero funzionare anche altri client di terze parti che supportano un insieme minimo di requisiti del client con {{site.data.keyword.messagehub}}. Tuttavia, noi eseguiamo test e abbiamo esperienza solo dei client di terze parti consigliati.

## Riepilogo del supporto per tutti i client consigliati
{: #client_summary_classic}

<table id="clients_table">
    <caption>Tabella 2. Riepilogo del supporto client</caption>
      <tr>
		    <th id="client" scope="col">Client</th>
		    <th id="language" scope="col">Linguaggio</th>
			<th id="version" scope="col">Versione consigliata</th>
		    <th id="minimum version" scope="col">Versione minima supportata [<sup>1</sup>](/docs/services/EventStreams?topic=eventstreams-kafka_clients#footnote1)</th>
			<th id="sample link" scope="col">Link all'esempio</th>
        </tr>
			<tr>
			<td colspan="3">**Client ufficiale**</td>
			</tr>
	  		<tr>
			<td>[Client Apache Kafka ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://kafka.apache.org/downloads)</td>
			<td>Java</td>
			<td>La più recente</td>
			<td>0.10.2 <p> Per informazioni sui precedenti client, consulta [retrocompatibilità](/docs/services/EventStreams?topic=eventstreams-kafka_clients#compatibility).</p></td>
			<td>[Java console sample ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-console-sample)<br/>
			[Liberty sample ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-liberty-sample)
			</td>
			</tr>
			<tr>
			<td colspan="3">**Client di terze parti**</td>
			</tr>
	  		<tr>
			<td>[node-rdkafka ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/Blizzard/node-rdkafka)</td>
			<td>Node.js</td>
			<td>La più recente</td>
			<td>2.2.2</td>
			<td>[Esempio Node.js ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample)</td>
		</tr>
		<tr>
			<td>[confluent-kafka-python ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/confluentinc/confluent-kafka-python)</td>
			<td>Python</td>
			<td>La più recente</td>
			<td>0.11.0</td>
			<td>[Esempio Kafka Python ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-python-console-sample)</td>
		</tr>
		<tr>
			<td>[confluent-kafka-go ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/confluentinc/confluent-kafka-go)</td>
			<td>Golang</td>
			<td>La più recente</td>
			<td>0.11.0</td>
			<td></td>
		</tr>
		<tr>
			<td>[librdkafka ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/edenhill/librdkafka)</td>
			<td>C o C++</td>
			<td>La più recente</td>
			<td>0.11.0</td>
			<td></td>
		</tr>

</table>
### Nota a piè di pagina
1. {: #footnote1}Questa versione è la meno recente che abbiamo convalidato nella verifica continua. Normalmente, questa è la versione iniziale disponibile negli ultimi 12 mesi, ma può essere aggiornata se si conosce che sono presenti dei problemi significativi.

## Retrocompatibilità (solo piano Classic)
{: #compatibility_classic}

Per la retrocompatibilità, puoi utilizzare il client Java Apache Kafka 0.9 con il piano Classic di {{site.data.keyword.messagehub}}. Tuttavia, vista l'età di questo client, ne scoraggiamo fortemente l'utilizzo. Se scegli di utilizzare questa versione del client, hai bisogno di un [modulo di accesso aggiuntivo ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-0.9/message-hub-login-library).

Le versioni client precedenti alla 0.11 potrebbero riscontrare delle prestazioni ridotte a causa delle conversioni del protocollo aggiuntivo necessarie per la connessione alle versioni del server Kafka più recenti.

<!--
## Unsupported clients

The following clients are not supported by {{site.data.keyword.messagehub}}:

### kafka-node
The kafka-node client does not fully support SASL authentication with the PLAIN mechanism so cannot currently be used with {{site.data.keyword.messagehub}}.


### no-kafka 
The no-kafka client does not fully support SASL authentication with the PLAIN mechanism so cannot currently be used with {{site.data.keyword.messagehub}}.

-->

## Collegamento del tuo client a {{site.data.keyword.messagehub}}
{: #connect_client_classic}

Per informazioni su come configurare il tuo client Java per il collegamento a {{site.data.keyword.messagehub}}, consulta [Configurazione del tuo client](/docs/services/EventStreams?topic=eventstreams-kafka_connect).












