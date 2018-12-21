---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

<!-- 15/11/18: info moved to eventstreams075.md, moved because of doc app changes -->
# Trocando mensagens entre a API do MQ Light e o Kafka ou APIs de REST do Kafka
{: #mql_exchange}

** A API do MQ Light está disponível como parte somente do plano Standard.**
<br/>

As mensagens do {{site.data.keyword.mql}} são armazenadas em um único tópico do Kafka
subjacente chamado "MQLight" e codificadas conforme detalhado na tabela a seguir. Esta codificação também pode
ser usada por outros tipos de API, como Kafka ou REST Kafka, para trocar mensagens com aplicativos
usando a API do {{site.data.keyword.mql}}.

## Formato da mensagem do Kafka
{: #kafka_format notoc}

<table border='1'>
<caption>Tabela 1. Formato da mensagem do Kafka</caption>
  <tr>
    <th> Key </th>
    <th> Valor </th>
  </tr>
  <tr>
    <td> Opcional (não usado pela API)
	<p></p>
	</td>
    <td>**1 byte**
	<p>		     Destaque da API do MQ Light, que é sempre 0xFA.</p>
    <p><var class="keyword varname">**n**</var> **bytes**</p>
    <p>		    Mensagem codificada do AMQP (formatada com base no formato de ligação do AMQP). </p></td>
  </tr>
</table>


