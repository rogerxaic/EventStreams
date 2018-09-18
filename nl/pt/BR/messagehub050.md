---

copyright:
  years: 2015, 2018
lastupdated: "2018-07-02"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Usando a API do Kafka
{: #kafka_using}

Se você estiver usando os clientes Java, será possível usar os clientes Kafka publicamente disponíveis na 0.10.x ou mais recente. 

Os clientes Kafka existem em múltiplos idiomas e fornecemos instruções para alguns desses idiomas. É possível utilizar outros, no entanto, o suporte para SASL PLAIN é necessário para fornecer credenciais. Além disso, se você estiver usando o plano Enterprise, também precisará usar a extensão de Service Name Identification (SNI) para o protocolo TLSv1.2.

<table>
    <caption>Tabela 1. Suporte ao cliente Kafka nos planos Standard e Enterprise</caption>
      <tr>
	        <th></th>
		    <th>Plano Standard</th>
		    <th>Plano Corporativo</th>
        </tr>
	  		<tr>
			<td>**Versão do Kafka no cluster**</td>
			<td>Kafka 0.10.2</td>
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

			<td>**Requisitos de autenticação**</td>
			<td>O cliente deve suportar a autenticação usando o mecanismo SASL Plain</td>
			<td>O cliente deve suportar a autenticação usando o mecanismo SASL Plain e usar a extensão de Service Name Identification (SNI) para o protocolo TLSv1.2</td>
		</tr>

</table>

Para obter informações sobre as APIs Producer e Consumer, consulte
[API Kafka Producer 0.11.0.X ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://kafka.apache.org/0110/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html){:new_window} e
[API Kafka Consumer 0.11.0.X ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://kafka.apache.org/0110/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html){:new_window}. 

