---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-09"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# O plano Clássico 
{: #plan_choose_classic}

O {{site.data.keyword.messagehub}} está disponível como planos diferentes, dependendo de seus requisitos. Para obter informações sobre os planos Padrão e Corporativo, consulte [Escolhendo seu plano](/docs/services/EventStreams?topic=eventstreams-plan_choose#plan_choose).
{: shortdesc}
 
## Visão geral do plano Clássico
O plano Clássico será apropriado se você precisar de recursos de alimentação e distribuição de eventos, mas não requerer nenhum benefício adicional do plano Corporativo. O plano Clássico oferece acesso compartilhado a um cluster de diversos locatários do {{site.data.keyword.messagehub}}.


## O que é suportado pelo plano Clássico

A tabela a seguir resume o que é suportado pelo plano:

<table>
    <caption>Tabela 1. Suporte no plano Clássico</caption>
      <tr>
	        <th></th>
		    <th>Plano Classic</th>
        </tr>
		<tr>
			<td>**Coarrendamento**</td>
			<td>Multivarrendatário </td>
		</tr>
        <tr>
			<td>**Zonas de disponibilidade**</td>
			<td>Não suportado</td>
		</tr>
        <tr>
			<td>**Disponibilidade**</td>
			<td>99,5%</td>
		</tr>
	  		<tr>
			<td>**Versão do Kafka no cluster**</td>
			<td>Kafka 1.1</td>
		</tr>
		<tr>
			<td>**Controle de acesso de baixa granularidade**</td>
			<td>Não</td>
		</tr>
				<tr>
			<td>**Suporte do Cloud Service Endpoint**</td>
			<td>Não</td>
		</tr>
		<tr>
			<td>**Kafka Connect e Kafka Streams suportados**</td>
			<td>Sim</td>

		</tr>
		<tr>
			<td>**Número máximo de partições**</td>
			<td>100</td>

		</tr>
		<tr>
			<td>**Período de retenção máximo**</td>
			<td>1 GB por partição para até 30 dias </td>

		</tr>
		<tr>
			<td>**Rendimento máximo**</td>
			<td>1 MB por segundo por partição</td>
		</tr>
		<tr>
			<td>**Tamanho máximo da mensagem**</td>
			<td>1 MB</td>
		</tr>
		<tr>
			<td>**Disponibilidade da localização (região)**</td>
			<td>Dallas (us-south)</br> 			Londres (eu-gb)</br> 			Sydney (au-syd)</br> 			Frankfurt (eu-de) - nenhuma API do {{site.data.keyword.mql}} </td>

		</tr>
		<tr>
     	    <td>**APIs suportadas**</td>
			<td>API do Kafka</br>
			API de REST administrativa<br/>
			API de REST do Kafka</br>
			API do MQ Light</br>
		    </td>
		</tr>
			<td>**Ponte do Cloud Object Storage e<br/> ponte do MQ suportadas**</td>
			<td>Sim</td>
		</tr>
		<tr>
			<td>** Período de tempo de implementação **</td>
			<td>Provisionamento instantâneo</td>
		</tr>

</table>

