---

copyright:
  years: 2015, 2018
lastupdated: "2018-11-16"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Escolhendo um cliente Kafka para usar com o {{site.data.keyword.messagehub}}
{: #kafka_clients}

Para usar a API do Kafka com o {{site.data.keyword.messagehub}}, escolha um dos seguintes tipos de cliente:

* Um cliente Java oficial na 1.1 ou mais recente, por exemplo, o [cliente Apache Kafka 1.1 ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.apache.org/dyn/closer.cgi?path=/kafka/1.1.0/kafka_2.11-1.1.0.tgz){:new_window}. O cliente Java oficial é a melhor opção porque contém os recursos disponíveis mais recentes para o Apache Kafka.
* Um dos [clientes de terceiros recomendados](/docs/services/EventStreams/eventstreams062.html#clients_table).

Para ambos os tipos de cliente, recomendamos sempre escolher a versão do cliente mais recente. 

## Requisitos do cliente para conexão com o Event Streams

Para se conectar ao {{site.data.keyword.messagehub}}, os clientes devem suportar a autenticação usando o mecanismo SASL simples e usar a extensão de Server Name Indication (SNI) para o protocolo TLSv1.2.

O protocolo Kafka mínimo que suportamos é o 0.10.

<!--
## Support summary for the official Apache Kafka client (Java)

<table>
    <caption>Table 1. Kafka client support in Standard and Enterprise plans</caption>
      <tr>
	        <th></th>
		    <th>Standard and Enterprise Plans</th>
		    <th></th>
        </tr>
	  		<tr>
			<td>**Kafka version on cluster**</td>
			<td>Kafka 1.1</td>
		</tr>
	  		<tr>
			<td>**Supported client versions**</td>
			<td>Kafka 1.1, or later</td>
		</tr>
			<td>**Authentication requirements**</td>
			<td>Client must support authentication using the SASL Plain mechanism and use the Server Name Indication (SNI) extension to the TLSv1.2 protocol</td>
		</tr>

</table>
-->

<!--
* [Apache Kafka 0.11.0.X client ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.apache.org/dyn/closer.cgi?path=/kafka/0.11.0.1/kafka_2.11-0.11.0.1.tgz){:new_window}
* [Apache Kafka 0.10.2.X client ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.apache.org/dyn/closer.cgi?path=/kafka/0.10.2.1/kafka_2.11-0.10.2.1.tgz){:new_window} 
-->
	

	
## Clientes de terceiros
{: #third_party_clients}

Caso você não possa executar os clientes Java oficiais, recomendamos executar um dos [clientes de terceiros recomendados](/docs/services/EventStreams/eventstreams062.html#clients_table), que são todos bem testados com o {{site.data.keyword.messagehub}}. 

<!--
* [sarama (Go) ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/Shopify/sarama){:new_window}
-->  

Outros clientes de terceiros que suportam o conjunto mínimo de requisitos do cliente podem funcionar com o {{site.data.keyword.messagehub}}. Entretanto, testamos e temos experiência apenas com os clientes de terceiros recomendados.

## Resumo de suporte para todos os clientes recomendados
{: #client_summary}

<table id="clients_table">
    <caption>Tabela 2. Resumo do suporte a clientes</caption>
      <tr>
		    <th>Cliente</th>
		    <th>Programação</th>
			<th>Versão Recomendada</th>
		    <th>Versão mínima suportada</th>
			<th>Link para a amostra</th>
        </tr>
			<tr>
			<td colspan="3">**Cliente oficial**</td>
			</tr>
	  		<tr>
			<td>[Cliente Apache Kafka 1.1
![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.apache.org/dyn/closer.cgi?path=/kafka/1.1.0/kafka_2.11-1.1.0.tgz)</td>
			<td>Java</td>
			<td>Latest</td>
			<td>0.10.2</td>
			<td>[Amostra do console Java ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-console-sample)<br/>
			[Amostra do Liberty ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-liberty-sample)
			</td>
			</tr>
			<tr>
			<td colspan="3">**Clientes de terceiros**</td>
			</tr>
	  		<tr>
			<td>[node-rdkafka ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/Blizzard/node-rdkafka)</td>
			<td>Node.js</td>
			<td>Latest</td>
			<td>2.3.3</td>
			<td>[Amostra do Node.js ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample)</td>
		</tr>
		<tr>
			<td>[confluent-kafka-python ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/confluentinc/confluent-kafka-python)</td>
			<td>Python</td>
			<td>Latest</td>
			<td>0.11.6</td>
			<td>[Amostra do Kafka Python ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-python-console-sample)</td>
		</tr>
		<tr>
			<td>[confluent-kafka-go ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/confluentinc/confluent-kafka-go)</td>
			<td>Golang</td>
			<td>Latest</td>
			<td>0.11.6</td>
			<td></td>
		</tr>
		<tr>
			<td>[librdkafka ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/edenhill/librdkafka)</td>
			<td>C ou C++</td>
			<td>Latest</td>
			<td>0.11.6</td>
			<td></td>
		</tr>

</table>

<!--
## Unsupported clients

The following clients are not supported by {{site.data.keyword.messagehub}}:

### kafka-node
The kafka-node client does not fully support SASL authentication with the PLAIN mechanism so cannot currently be used with {{site.data.keyword.messagehub}}.


### no-kafka 
The no-kafka client does not fully support SASL authentication with the PLAIN mechanism so cannot currently be used with {{site.data.keyword.messagehub}}.

-->

## Conectando o cliente ao {{site.data.keyword.messagehub}}
{: #connect_client}

Para obter informações sobre como configurar o cliente Java para se conectar ao {{site.data.keyword.messagehub}}, consulte [Configurando o cliente](/docs/services/EventStreams/eventstreams063.html).












