---

copyright:
  years: 2015, 2018
lastupdated: "2018-09-12"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# Escolhendo o seu plano 
{: #plan_choose}

O {{site.data.keyword.messagehub}} está disponível como dois planos diferentes dependendo de seus requisitos:
Standard e Enterprise.

## Plano padrão

O plano Standard será apropriado se você precisar de recursos de alimentação e distribuição de eventos, mas não requer nenhum
benefício adicional do plano Enterprise. O plano Standard oferece acesso compartilhado a um cluster de múltiplos locatários do {{site.data.keyword.messagehub}}.

## Plano Enterprise 

O plano Enterprise será apropriado se o isolamento de dados, o desempenho garantido e a retenção aumentada forem
considerações importantes. O plano Enterprise oferece acesso exclusivo a um cluster dedicado do {{site.data.keyword.messagehub}}.

## O que é suportado pelos planos Standard e Enterprise

A tabela a seguir resume o que é suportado pelos planos:

<table>
    <caption>Tabela 1. Suporte nos planos Standard e Enterprise</caption>
      <tr>
	        <th></th>
		    <th>Plano Standard</th>
		    <th>Plano Corporativo</th>
        </tr>
		<tr>
			<td>**Coarrendamento**</td>
			<td>Multivarrendatário </td>
			<td>Locatário único</td>
		</tr>
        <tr>
			<td>**Zonas de disponibilidade**</td>
			<td>Não suportado</td>
			<td>3</td>
		</tr>
	  		<tr>
			<td>**Versão do Kafka no cluster**</td>
			<td>Kafka 0.10.2</td>
			<td>Kafka 1.1</td>
		</tr>
		<tr>
			<td>**Controle de acesso de baixa granularidade**</td>
			<td>Não</td>
			<td>Sim</td>
		</tr>
		<tr>
			<td>**Kafka Connect e Kafka Streams suportados**</td>
			<td>Sim</td>
			<td>Sim</td>
		</tr>
		<tr>
			<td>**Número máximo de partições**</td>
			<td>100</td>
			<td>1000</td>
		</tr>
		<tr>
			<td>**Período de retenção máximo**</td>
			<td>1 GB por partição para até 30 dias </td>
			<td>Ilimitado até o limite de armazenamento de seu plano </td>
		</tr>
		<tr>
			<td>**Disponibilidade de região**</td>
			<td>Sul dos EUA</br>
			Reino Unido</br>
			Sydney</br>
			Alemanha (sem API do MQ Light)</td>
			<td>Sul dos EUA</br>
			Leste dos EUA<br/>
			Alemanha<br/>
			<br/>
			</td>
		</tr>
		<tr>
     	    <td>**APIs suportadas**</td>
			<td>API Kafka</br>
			API REST administrativa<br/>
			API REST Kafka</br>
			API do MQ Light</br>
		    </td>
			<td>API Kafka<br/>
			API REST administrativa</td>
		</tr>
			<td>**Ponte do Cloud Object Storage e<br/>
			ponte do MQ suportadas**</td>
			<td>Sim</td>
			<td>Não</td>
		</tr>
		<tr>
			<td>** Período de tempo de implementação **</td>
			<td>Provisionamento instantâneo</td>
			<td>O provisionamento esperado pode levar até três horas</td>
		</tr>

</table>


<!--
## {{site.data.keyword.Bluemix_notm}} Public environment
{: notoc}

{{site.data.keyword.Bluemix_notm}} Public provides an
economical public cloud service where you pay for what you use and share infrastructure with
others.

In {{site.data.keyword.Bluemix_notm}} Public, the cost of
{{site.data.keyword.messagehub}} is determined by two factors: the
number of partitions that you use and the number of messages that you send and receive. There is no
charge for message data while it is retained on the topics, but the data that each partition retains
is capped at 1 GB.

For more information, see [{{site.data.keyword.Bluemix_notm}} Public ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud-computing/bluemix/public){:new_window}.
-->

