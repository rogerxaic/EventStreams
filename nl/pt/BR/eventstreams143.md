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

# Fazendo upgrade para o novo plano Padrão do {{site.data.keyword.messagehub}} 
{: #migrate_classic_plan}

Essa nova liberação do plano Padrão de vários locatários oferece melhorias significativas na resiliência, funcionalidade e usabilidade. Para obter mais informações, consulte o [comunicado do blog Novo plano Padrão ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/cloud/blog/announcements/ibm-event-streams-releases-a-new-and-enhanced-standard-plan).
{: shortdesc}

Para migrar aplicativos do plano Padrão anterior (agora chamado de plano Clássico) para o novo plano, considere as informações a seguir.

Agora, as instâncias de serviço são fornecidas como serviços do {{site.data.keyword.cloud_notm}} em vez de como serviços do Cloud Foundry. Isso permite que o serviço suporte os padrões e recursos do {{site.data.keyword.cloud_notm}} mais recentes, incluindo implementações com múltiplas zonas e controles de acesso granulares, mas tem implicações sobre como o serviço é usado. Em particular, considere os aspectos a seguir:

* **Criando, excluindo e listando serviços**
<br/>
    Como antes, é possível gerenciar o ciclo de vida dos serviços usando o console do {{site.data.keyword.cloud_notm}} ou a ferramenta de linha de comandos da CLI do {{site.data.keyword.cloud_notm}}. Se você estiver usando o console, agora os serviços serão listados em **Serviços** em vez de em **Serviços do Cloud Foundry**. 
    
    Se você estiver usando a CLI, as instâncias serão gerenciadas usando os comandos de recurso. Por exemplo, <code>ibmcloud resource service-instance-create</code>. Isso está no lugar dos comandos **cf**, por exemplo, <code>ibmcloud cf create-service</code>.

* **Controlando o acesso**
<br/>
    Agora, a autenticação e a autorização são gerenciadas usando o serviço Cloud Identity and Access Management (IAM). Além de controlar a capacidade de um usuário de se conectar, o IAM também permite que você configure o acesso granular para recursos subjacentes, como os tópicos. O acesso é controlado designando políticas aos usuários. Para obter mais informações, consulte [Gerenciando o acesso aos recursos do {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-security).

<ul>
<li><strong>Conectando aplicativos</strong><br/> As informações que um aplicativo precisa para se conectar não foram mudadas, ou seja, uma lista de <code>bootstrap.servers</code>, <code>username</code> e <code>password</code> é necessária. No entanto, a maneira como essas propriedades são recuperadas mudou.

<ul>
<li>
      <strong>Para aplicativos nativos</strong><br/>
        Deve-se criar um objeto Credenciais ou Chave de serviço usando o console do IBM Cloud ou a CLI, respectivamente. Assim, é possível recuperar as propriedades necessárias. Para obter informações adicionais, consulte [Conectando aplicativos](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_external).
</li>
<br/>
<li><strong>Para aplicativos do Cloud Foundry</strong><br/>
        Deve-se primeiro ligar o serviço ao espaço e à organização do aplicativo, criando um alias de serviço. Então, é possível recuperar as propriedades necessárias da variável de ambiente VCAP_SERVICES como normalmente. Para obter mais informações, consulte [Conectando ao {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting).
</li>
</ul>
<br/>
Observe que os clientes devem suportar a extensão SNI para TLS em que o nome do host do servidor está incluído no handshake TLS. Esse recurso é comumente disponível e é suportado em todas as versões do cliente recomendadas em [Escolhendo um cliente Kafka para usar com o {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_clients).
</li>
</ul>

<br/>
Também é necessário estar ciente de algumas outras mudanças como a seguir:

* **Versão do Kafka**
<br/>
    Esse plano fornece acesso à liberação estável mais recente do Kafka 2.2. Os aplicativos desenvolvidos com relação ao Kafka 1.1 podem ser executados inalterados, mas consulte as informações a seguir para as versões do cliente recomendadas e as combinações testadas: [Escolhendo um cliente Kafka para usar com o {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_clients). 

* **Regiões suportadas**
<br/>
    O plano está inicialmente disponível apenas no Sul dos EUA. As regiões restantes serão introduzidas durante as próximas semanas.

* **Integrações**
<br/>
    A conexão por meio de outros serviços, como o {{site.data.keyword.iot_short_notm}} ou o {{site.data.keyword.openwhisk_short}}, que se liga ao serviço usando uma organização e um espaço do Cloud Foundry requer que um alias de serviço seja criado. Para obter mais informações, consulte [Conectar aplicativos do Cloud Foundry ao {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_cf).
    

* **Recursos suportados**
<br/>
    Há diferenças entre os recursos do plano Clássico e o novo plano Padrão. Para alinhar as ofertas do produto, adotar novas opções de tecnologia e remover recursos menos usados, nem todos os recursos foram transportados. Uma comparação dos recursos está disponível em [Escolhendo seu plano](/docs/services/EventStreams?topic=eventstreams-plan_choose). Se você depender dessas funções, informações adicionais serão fornecidas em breve para ajudá-lo a migrar.
   
<br/>
Pequenos deltas de código são enviados diariamente para a produção. Como resultado, é possível esperar ver muitas melhorias adicionais para a nossa experiência do usuário (e outras áreas) em todo o restante de 2019 e além. Em breve:

* **Métricas do cliente**
<br/>
    A capacidade de monitorar a atividade em uma instância de serviço.

<br/>
Para obter uma rápida introdução às etapas para torná-lo funcional no novo plano Padrão, experimente o [Tutorial de introdução](/docs/services/EventStreams?topic=eventstreams-getting_started).


