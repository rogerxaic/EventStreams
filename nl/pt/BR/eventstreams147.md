---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-15"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}

# Usando a API do Kafka no plano Clássico
{: #kafka_using_classic}

Os clientes Kafka existem em múltiplos idiomas e fornecemos instruções para alguns desses idiomas. É possível utilizar outros, no entanto, o suporte para SASL PLAIN é necessário para fornecer credenciais. Além disso, se você estiver usando o plano Enterprise, também precisará usar a extensão Server Name Indication (SNI) para o protocolo TLSv1.2.

<table>
    <caption>Tabela 1. Suporte ao cliente Kafka nos planos Padrão e Corporativo</caption>
      <tr>
	        <th></th>
		    <th>Plano Classic</th>
		    
        </tr>
	  		<tr>
			<td>**Versão do Kafka no cluster**</td>
			<td>Kafka 1.1</td>
					</tr>
	  		<tr>
			<td>**Versões do cliente suportadas**</td>
			<td>Kafka 0.10.x ou mais recente</td>
		</tr>
		<tr>
			<td>**Kafka Connect, Kafka Streams e KSQL suportados?**</td>
			<td>Sim</td>
		</tr>

			<td>**Requisitos de autenticação**</td>
			<td>O cliente deve suportar a autenticação usando o mecanismo SASL Plain e usar a extensão Server Name Indication (SNI) para o protocolo TLSv1.2</td>
				</tr>

</table>

Para obter informações sobre as APIs Producer e Consumer V1.1, consulte
[Kafka Producer API 1.1 ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://kafka.apache.org/11/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html){:new_window} e
[Kafka Consumer API 1.1 ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://kafka.apache.org/11/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html){:new_window}. 












