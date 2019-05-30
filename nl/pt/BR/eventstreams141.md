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

# Escolhendo um cliente Kafka para usar com o plano Clássico do {{site.data.keyword.messagehub}} 
{: #kafka_clients_classic}

Para usar a API do Kafka com o {{site.data.keyword.messagehub}}, escolha um dos seguintes tipos de cliente:

* O cliente Java oficial. Ele é a melhor opção porque contém os recursos mais recentes disponíveis para o Apache Kafka.
* Um dos [clientes de terceiros recomendados](/docs/services/EventStreams?topic=eventstreams-kafka_clients#clients_table).

Para ambos os tipos de cliente, recomendamos sempre escolher a versão do cliente mais recente. 
{: shortdesc}

## Requisitos do cliente para conexão com o Event Streams

Para se conectar ao {{site.data.keyword.messagehub}}, os clientes devem suportar a autenticação usando o mecanismo SASL simples e usar a extensão de Server Name Indication (SNI) para o protocolo TLSv1.2.

O protocolo Kafka mínimo que suportamos é o 0.10.
	
## Clientes de terceiros
{: #third_party_clients_classic}

Caso você não possa executar os clientes Java oficiais, recomendamos executar um dos [clientes de terceiros recomendados](/docs/services/EventStreams?topic=eventstreams-kafka_clients#clients_table), que são todos bem testados com o {{site.data.keyword.messagehub}}. 

Outros clientes de terceiros que suportam o conjunto mínimo de requisitos do cliente podem funcionar com o {{site.data.keyword.messagehub}}. Entretanto, testamos e temos experiência apenas com os clientes de terceiros recomendados.

## Resumo de suporte para todos os clientes recomendados
{: #client_summary_classic}

<table id="clients_table">
    <caption>Tabela 2. Resumo do suporte a clientes</caption>
      <tr>
		    <th id="client" scope="col">Cliente</th>
		    <th id="language" scope="col">Programação</th>
			<th id="version" scope="col">Versão Recomendada</th>
		    <th id="minimum version" scope="col">Versão mínima suportada [<sup>1</sup>](/docs/services/EventStreams?topic=eventstreams-kafka_clients#footnote1)</th>
			<th id="sample link" scope="col">Link para a amostra</th>
        </tr>
			<tr>
			<td colspan="3">**Cliente oficial**</td>
			</tr>
	  		<tr>
			<td>[Cliente Apache Kafka
![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://kafka.apache.org/downloads)</td>
			<td>Java</td>
			<td>Latest</td>
			<td>0.10.2 <p> Para obter informações sobre clientes mais antigos, consulte [compatibilidade com versões anteriores](/docs/services/EventStreams?topic=eventstreams-kafka_clients#compatibility).</p></td>
			<td>[Amostra do console Java ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-console-sample)<br/>
			[amostra do Liberty ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-liberty-sample)
			</td>
			</tr>
			<tr>
			<td colspan="3">**Clientes de terceiros**</td>
			</tr>
	  		<tr>
			<td>[node-rdkafka ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/Blizzard/node-rdkafka)</td>
			<td>Node.js</td>
			<td>Latest</td>
			<td>2.2.2</td>
			<td>[Amostra do Node.js ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample)</td>
		</tr>
		<tr>
			<td>[confluent-kafka-python ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/confluentinc/confluent-kafka-python)</td>
			<td>Python</td>
			<td>Latest</td>
			<td>0.11.0</td>
			<td>[Amostra do Kafka Python ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-python-console-sample)</td>
		</tr>
		<tr>
			<td>[confluent-kafka-go ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/confluentinc/confluent-kafka-go)</td>
			<td>Golang</td>
			<td>Latest</td>
			<td>0.11.0</td>
			<td></td>
		</tr>
		<tr>
			<td>[librdkafka ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/edenhill/librdkafka)</td>
			<td>C ou C++</td>
			<td>Latest</td>
			<td>0.11.0</td>
			<td></td>
		</tr>

</table>
### Nota de Rodapé
1. {: #footnote1}Essa versão é a mais antiga que validamos em testes contínuos. Geralmente, essa é a versão inicial disponível dentro dos últimos 12 meses, mas ela pode ser mais recente se problemas significativos forem conhecidos.

## Compatibilidade com versões anteriores (apenas plano clássico)
{: #compatibility_classic}

Para compatibilidade com versões anteriores, é possível usar o cliente Java Apache Kafka 0.9 com o plano Clássico do {{site.data.keyword.messagehub}}. No entanto, por causa da idade desse cliente, nós desencorajamos fortemente seu uso. Se você optar por usar essa versão do cliente, precisará de um [módulo de login adicional ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-0.9/message-hub-login-library).

As versões do cliente anteriores a 0.11 podem apresentar um desempenho comprometido por causa das conversões de protocolo adicionais que são necessárias para conexão com as versões mais recentes do servidor Kafka.

<!--
## Unsupported clients

The following clients are not supported by {{site.data.keyword.messagehub}}:

### kafka-node
The kafka-node client does not fully support SASL authentication with the PLAIN mechanism so cannot currently be used with {{site.data.keyword.messagehub}}.


### no-kafka 
The no-kafka client does not fully support SASL authentication with the PLAIN mechanism so cannot currently be used with {{site.data.keyword.messagehub}}.

-->

## Conectando o cliente ao {{site.data.keyword.messagehub}}
{: #connect_client_classic}

Para obter informações sobre como configurar o cliente Java para se conectar ao {{site.data.keyword.messagehub}}, consulte [Configurando o cliente](/docs/services/EventStreams?topic=eventstreams-kafka_connect).












