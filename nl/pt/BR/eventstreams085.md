---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-23"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:note: .note}

# Escolhendo o seu plano 
{: #plan_choose}

O {{site.data.keyword.messagehub}} está disponível como planos diferentes, dependendo de seus requisitos: Padrão, Corporativo e Clássico. 

<!--
For information about the Classic plan, see
[Classic plan](/docs/services/EventStreams?topic=eventstreams-plan_choose_classic#plan_choose_classic).
-->
{: shortdesc}

## Plano padrão
{: #plan_standard}

O plano Standard será apropriado se você precisar de recursos de alimentação e distribuição de eventos, mas não requer nenhum
benefício adicional do plano Enterprise. O plano Standard oferece acesso compartilhado a um cluster de múltiplos locatários do {{site.data.keyword.messagehub}}.

## Plano Enterprise 
{: #plan_enterprise}

O plano Enterprise será apropriado se o isolamento de dados, o desempenho garantido e a retenção aumentada forem
considerações importantes. O plano Enterprise oferece acesso exclusivo a um cluster dedicado do {{site.data.keyword.messagehub}}. Também é possível provisionar um cluster do {{site.data.keyword.messagehub}} em uma localização geograficamente local, mas de [zona única (SZR)](/docs/services/EventStreams?topic=eventstreams-sla#sla_szr).

## Plano Classic
{: #plan_classic}

O plano Clássico dá acesso à edição anterior do plano Padrão e é fornecido apenas para cargas de trabalho e compatibilidade com versões anteriores existentes. É necessário fornecer novas cargas de trabalho com relação ao plano Padrão.


## O que é suportado pelos planos Padrão, Corporativo e Clássico

A tabela a seguir resume o que é suportado pelos planos:

<table>
    <caption>Tabela 1. Suporte nos planos Padrão, Corporativo e Clássico</caption>
      <tr>
	        <th></th>
		    <th>Plano Padrão</th>
		    <th>Plano Corporativo</th>
		    <th>Plano Clássico</th>
        </tr>
		<tr>
			<td>**Coarrendamento**</td>
			<td>Multivarrendatário </td>
			<td>Locatário único</td>
			<td>Multivarrendatário</td>
		</tr>
        <tr>
			<td>**Zonas de disponibilidade**</td>
			<td>3</td>
			<td>3<br/>(1 em localizações de zona única)
			</td>
			<td>Não suportado</td>
		</tr>
        <tr>
			<td>**Disponibilidade**</td>
			<td>99,95%</td>
			<td>99,95%<br/>(99,5% em localizações de zona única) [<sup>1</sup>](/docs/services/EventStreams?topic=eventstreams-plan_choose#footnote_plans)</td>
			<td>99,5%</td>
		</tr>
	  		<tr>
			<td>**Versão do Kafka no cluster**</td>
			<td>Kafka 2.2</td>
			<td>Kafka 1.1 <br/>(Kafka 2.2 em breve)</td>
			<td>Kafka 1.1</td>
		</tr>
		<tr>
			<td>**Controle de acesso de baixa granularidade**</td>
			<td>Sim</td>
			<td>Sim</td>
			<td>Não</td>
		</tr>
				<tr>
			<td>**Suporte do Cloud Service Endpoint**</td>
			<td>Não</td>
			<td>Sim</td>
			<td>Não</td>
		</tr>
		<tr>
			<td>**Kafka Connect e Kafka Streams suportados**</td>
			<td>Sim</td>
			<td>Sim</td>
			<td>Sim</td>
		</tr>
		<tr>
			<td>**Número máximo de partições**</td>
			<td>100</td>
			<td>1000</td>
			<td>100</td>
		</tr>
		<tr>
			<td>**Período de retenção máximo**</td>
			<td>1 GB por partição para até 30 dias </td>
			<td>2 TB de armazenamento utilizável<!--Unlimited up to the storage limit of your plan --></td>
			<td>1 GB por partição para até 30 dias </td>
		</tr>
		<tr>
			<td>**Rendimento máximo**</td>
			<td>1 MB por segundo por partição (máximo de 20 MB por segundo) </td>
			<td>40 MB por segundo por cluster (rendimento de pico de 75 MB por segundo)</td>
			<td>1 MB por segundo por partição</td>
		</tr>
		<tr>
			<td>**Tamanho máximo da mensagem**</td>
			<td>1 MB</td>
			<td>1 MB</td>
			<td>1 MB</td>
		</tr>
		<tr>
			<td>**Número máximo de clientes conectados**</td>
			<td>100</td>
			<td>10 000</td>
			<td>100</td>
		</tr>
		<tr>
			<td>**Disponibilidade da localização (região)**</td>
			<td>**Localização multizona (MZR)**<br/>
			Dallas (us-south)</br>
			Washington (us-east)<br/>
			Londres (eu-gb)<br/>
			Sydney (au-syd)</br>
			Frankfurt (eu-de)<br/>
			Tóquio (jp-tok)<br/>
			<br/>
			</td>
			<td>**Localização multizona (MZR)**</br>
			Dallas (us-south)</br>
			Washington (us-east)<br/>
			Londres (eu-gb)<br/>
			Sydney (au-syd)</br>
			Frankfurt (eu-de)<br/>
			Tóquio (jp-tok)<br/>
			<br/>
			**Localização de zona única (SZR)**</br>
			Seoul (seo01)<br/>
			<br/>
			</td>
			<td>Dallas (us-south)</br> 			Londres (eu-gb)</br> 			Sydney (au-syd)</br> 			Frankfurt (eu-de) - nenhuma API do {{site.data.keyword.mql}} </td>
		</tr>
		<tr>
     	    <td>**APIs suportadas**</td>
			<td>API do Kafka</br>
			API de REST administrativa<br/>
			API de REST Producer</br>
		    </td>
			<td>API Kafka<br/>
			API de REST Admin</br>
			API de REST Producer</br>
			</td>
			<td>API do Kafka</br>
			API de REST administrativa<br/>
			API de REST do Kafka</br>
			API do MQ Light</br>
		    </td>
		</tr>
		</tr>
			<td>**Ponte do Cloud Object Storage e<br/>
			ponte do MQ suportadas**</td>
			<td>Não</td>
			<td>Não</td>
			<td>Sim</td>
		</tr>
		<tr>
			<td>** Período de tempo de implementação **</td>
			<td>Provisionamento instantâneo</td>
			<td>O fornecimento leve cerca de três horas. Como o Corporativo tem seus próprios recursos dedicados para cada cluster, ele requer mais tempo para o fornecimento</td>
			<td>Provisionamento instantâneo</td>
		</tr>

</table>
### Nota de Rodapé
{: #footnote_plans notoc}

1. {: #footnote_szr notoc} Para obter mais informações sobre a disponibilidade, consulte [Implementações de localização de zona única](/docs/services/EventStreams?topic=eventstreams-sla#sla_szr).



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

For more information, see [{{site.data.keyword.Bluemix_notm}} Public ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud/free/){:new_window}.
-->

