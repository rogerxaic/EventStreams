---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-19"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}

# Usando a API do Kafka
{: #kafka_using}

O Kafka fornece um rico conjunto de APIs e clientes em uma ampla gama de linguagens. Por exemplo:

* **API principal do Kafka (API Consumer, Producer e Admin)**<br/>
    Use para enviar e receber mensagens diretamente de um ou mais tópicos do Kafka.
* **API de fluxos**<br/>
    Uma API de processamento de fluxo de nível mais alto para facilmente consumir, transformar e produzir eventos entre tópicos.
* **API de conexão**<br/>
    Uma estrutura que permite integrações reutilizáveis ou padrão para eventos de fluxo dentro e fora de sistemas externos, como os bancos de dados.
* **KSQL**<br/>
    Uma interface para processar e associar eventos por meio de tópicos usando uma sintaxe semelhante a SQL.

A tabela a seguir resume o que é possível usar com o {{site.data.keyword.messagehub}}:

<table>
    <caption>Tabela 1. Suporte ao cliente Kafka nos planos Padrão e Corporativo</caption>
      <tr>
	        <th></th>
		    <th>Plano Padrão</th>
		    <th>Plano Corporativo</th>
        </tr>
	  		<tr>
			<td>**Versão do Kafka no cluster**</td>
			<td>Kafka 2.2</td>
			<td>Kafka 1.1</td>
		</tr>
	  		<tr>
			<td>**Versões do cliente suportadas**</td>
			<td>Kafka 0.10.x ou mais recente</td>
			<td>Kafka 0.10.x ou mais recente</td>
		</tr>
		<tr>
			<td>**Kafka Connect, Kafka Streams e KSQL suportados?**</td>
			<td>Sim</td>
			<td>Sim</td>
		</tr>
		<tr>
			<td>**Requisitos de autenticação**</td>
			<td>O cliente deve suportar a autenticação usando o mecanismo SASL Plain e usar a extensão Server Name Indication (SNI) para o protocolo TLSv1.2</td>
			<td>O cliente deve suportar a autenticação usando o mecanismo SASL Plain e usar a extensão Server Name Indication (SNI) para o protocolo TLSv1.2</td>
		</tr>

</table>
<br/>
Para obter informações sobre como usar a API do Kafka no plano Clássico, consulte [API do Kafka - Clássico](/docs/services/EventStreams?topic=eventstreams-kafka_using_classic).


## Escolhendo um cliente Kafka para usar com o {{site.data.keyword.messagehub}}
{: #kafka_clients}

O cliente oficial para a API do Kafka é gravado em Java e, como tal, contém os recursos e as correções de erro mais recentes. Para obter mais informações sobre essa API, consulte [Kafka Producer API 2.2 ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://kafka.apache.org/22/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html){:new_window} e
[Kafka Consumer API 2.2 ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://kafka.apache.org/22/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html){:new_window}. 

Para outras linguagens, recomendamos a execução de um dos clientes a seguir. Todos eles bem testados com o {{site.data.keyword.messagehub}}.

### Resumo de suporte para todos os clientes recomendados
{: #client_summary}

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
			<td colspan="3">**Cliente oficial Apache Kafka**</td>
			</tr>
	  		<tr>
			<td>[Cliente Apache Kafka
![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://kafka.apache.org/downloads)</td>
			<td>Java</td>
			<td>Latest</td>
			<td>0.10.2 </td>
			<td>[Amostra do console Java](/docs/services/EventStreams?topic=eventstreams-kafka_java_using)<br/>
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
{: #footnote_clients notoc}
1. {: #footnote1 notoc}Essa versão é a mais antiga que validamos em testes contínuos. Geralmente, essa é a versão inicial disponível dentro dos últimos 12 meses, mas ela pode ser mais recente se problemas significativos forem conhecidos.

<br/>
Se não for possível executar nenhum dos clientes listados, será possível usar outros clientes de terceiro que atendam aos requisitos mínimos a seguir (por exemplo, [librdkafka ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/edenhill/librdkafka/){:new_window}).
* Suporta o Kafka 0.10 ou mais recente
* Pode conectar e autenticar usando SASL PLAIN com TLSv1.2
* Suporta as extensões SNI para TLS em que o nome do host do servidor é incluído no handshake TLS
* Suporta criptografia de curva elíptica. No entanto, nós apenas testamos e temos experiência com os clientes de terceiro recomendados.

Em todos os casos, a versão mais recente do cliente é recomendada.

<br/>
### Conectando o cliente ao {{site.data.keyword.messagehub}}
{: #connect_client}

Para obter informações sobre como configurar o cliente Java para se conectar ao {{site.data.keyword.messagehub}}, consulte [Configurando o cliente](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_api_client).

## Configurando o cliente da API do Kafka
{: #kafka_api_client}

Para estabelecer uma conexão, os clientes devem ser configurados para usar SASL_SSL PLAIN sobre TLSv1.2 minimamente e para requerer um nome de usuário e uma lista dos servidores de autoinicialização. 

Para recuperar o nome de usuário, a senha e a lista de servidores de autoinicialização, um objeto de credenciais de serviço ou uma chave de serviço é necessária para a instância de serviço. Para obter mais informações sobre como criar esses objetos, consulte <link to Connecting to event Streams>[Conectando ao {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting).

Por meio desses objetos:
* Use a propriedade <code>kafka_brokers_sasl</code> como a lista de servidores de autoinicialização. Formate essa lista como uma lista separada por vírgula de entradas host:port. Por exemplo, <code>host1:port1,host2:port2</code>. Recomendamos incluir detalhes para todos os hosts listados na propriedade <code>kafka_brokers_sasl</code>.
* Use as propriedades <code>user</code> e <code>api_key</code> como o nome de usuário e a senha

Para as instâncias de serviço no plano Classic, essas informações estão disponíveis na variável de ambiente VCAP_SERVICES do seu aplicativo. Para obter mais informações, consulte [Conectando ao {{site.data.keyword.messagehub}} - Clássico](/docs/services/EventStreams?topic=eventstreams-connecting_classic).

Para um cliente Java, o exemplo a seguir mostra o conjunto mínimo de propriedades, em que USERNAME, PASSWORD e KAFKA_BROKERS_SASL devem ser substituídos pelos valores que você recuperou anteriormente.

```
bootstrap.servers=KAFKA_BROKERS_SASL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USERNAME" password="PASSWORD";
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
ssl.protocol=TLSv1.2
ssl.enabled.protocols=TLSv1.2
ssl.endpoint.identification.algorithm=HTTPS
```
{: codeblock}

<br/>
Observe que se você estiver usando um cliente Kafka anterior à 0.10.2.1, a propriedade <code>sasl.jaas.config</code> não será suportada e você deverá fornecer a configuração do cliente em um arquivo de configuração do JAAS no lugar. 








 




