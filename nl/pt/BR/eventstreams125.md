---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-04"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}

# Relatando um problema para a equipe do {{site.data.keyword.messagehub}} - planos Padrão e Corporativo
{: #report_problem_enterprise}


Se você estiver tendo problemas com o {{site.data.keyword.messagehub}}, verifique primeiro a [página de status do {{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/status?selected=status){:new_window}.
{: shortdesc}

Se quiser ajuda da equipe do {{site.data.keyword.messagehub}}, reúna todas as informações a seguir. Quanto mais informações você puder fornecer diretamente, mais eficientemente a equipe poderá ajudar com o problema:
{:shortdesc}

1. Qual é o ID do CRN do serviço {{site.data.keyword.messagehub}} que você está usando?  É possível fornecer esse ID colando a URL completa do console do {{site.data.keyword.Bluemix_notm}} depois de clicar no serviço ou colando a saída por meio do comando da CLI a seguir:<br/>
   <pre class="pre">
   ibmcloud resource service-instance NAME
   </pre>
	{: codeblock}
2. Quando o primeiro problema ocorreu (especificamente hora, data e fuso horário)?
   Por quanto tempo
seu aplicativo esteve em execução antes disso?
3. O problema ainda está ocorrendo? É possível replicá-lo?
4. Qual cliente Kafka o seu aplicativo está usando? Quais são os detalhes da versão?
5. Quais são os detalhes de configuração do seu cliente? Como nós precisamos conhecer as suas configurações de produtor e de
consumidor, liste qualquer opção não padrão que você tenha passado para a criação do produtor e do consumidor.
6. Você possui fragmentos de log do aplicativo que exibem o problema?
7. Qual é o problema que você está vendo? Quais tópicos, identificadores de cliente, IDs do grupo e identificadores de
transação são afetados?
8. Que impacto o problema está causando em seu serviço?

É possível fornecer as informações reunidas para a IBM em um chamado de suporte [enviando uma solicitação
de suporte ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/get-support?topic=get-support-getting-customer-support#using-avatar){:new_window}.










