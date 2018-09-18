---

copyright:
  years: 2015, 2018
lastupdated: "2018-04-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

<!-- Notes from chat with Charlie 

Different plan for provisioning

Quality of service from each plan

Life of a user through cycle - APIs, feature sets

-->

# Sobre o programa Alpha
{: #alpha_about }

O programa Alpha fornece acesso inicial para novos recursos no serviço do {{site.data.keyword.messagehub}}. O recurso
atual é a introdução de um novo plano, o plano {{site.data.keyword.messagehub}} Premium.

## Plano Premium
{: premium_plan}

O plano Premium é projetado para usuários que possuem desempenho e outros requisitos funcionais que vão além do serviço
público. Ele fornece uma versão de locatário único do serviço do {{site.data.keyword.messagehub}}, no qual você tem o uso
exclusivo de um cluster do Apache Kafka. Essa versão permite:

* Aproveitar ao máximo a capacidade e o desempenho do cluster

* Ter um aumento considerável nos limites e no número de partições

* Beneficiar-se de custo zero de gerenciamento com um cluster totalmente gerenciado que é automaticamente mantido e
atualizado

## Sobre o seu cluster Alpha
{: alpha_cluster}

O seu cluster Alpha é implementado com o Apache Kafka versão 1.1 e é capaz de entregar um rendimento da mensagem máximo de
90.000 KB por segundo. 

É possível criar um máximo de 1000 partições e cada partição pode reter um máximo de 1 GB de dados por até 30 dias. Para
resiliência, os dados são armazenados em 3 réplicas e o deslocamento confirmado para cada partição é mantido por um máximo de 7
dias.

As duas APIs a seguir são suportadas:

* Para sistema de mensagens, os clientes Kafka da versão 0.10.x e mais recente são suportados, incluindo a opção para usar o Kafka
Streams, o Kafka Connect e o KSQL.

* Para administração, uma API de REST está disponível para criar, excluir e listar tópicos.

O programa Alpha está disponível na região sul dos EUA somente. O acesso a clusters é gerenciado usando o IAM.

## Conectando ao seu cluster
{: alpha_connect}

Para se conectar a uma API no cluster, são necessárias sua URL de terminal e uma chave API para autenticação. É possível recuperar esses detalhes do IAM usando um dos métodos a seguir:

### Aplicativo Cloud Foundry
Para um aplicativo Cloud Foundry:
1. Na guia **Conexões** do aplicativo (à esquerda), clique no botão **Criar conexão**. 
2. Selecione a instância do serviço do {{site.data.keyword.messagehub}} ao qual deseja se conectar e clique em
**Conectar**. Aceite as opções padrão. 
3. Quando a conexão for concluída, clique na guia **Tempo de execução** do aplicativo e, em seguida,
clique em **Variáveis de ambiente** para mostrar **VCAP_SERVICES**.

### Console para um aplicativo externo
No console para um aplicativo externo, crie uma chave API de serviço usando o comando **bx** a seguir: 

```
bx resource service-key-create name-of-key Manager --instance-name name-of-your-service
``` 

Copie os campos <code>kafka_brokers_sasl</code>, <code>kafka_admin_url</code> e <code>apikey</code> das informações geradas.
Somente os seus primeiros cinco brokers são listados em VCAP_SERVICES. Se você tiver mais de cinco brokers, use um cliente Kafka para
recuperar os detalhes de seus outros brokers. 

## Conectando um cliente à API do Kafka

Para conectar um cliente à API do Kafka, conclua as etapas a seguir:

1. Configure a propriedade <code>bootstrap.servers</code> dos clientes para uma lista separada por vírgula dos
brokers listados em <code>kafka_brokers_sasl</code>.

2. Configure o campo USERNAME <code>sasl.jaas.config</code> dos clientes para os primeiros oito caracteres do
<code>apikey</code> e o campo PASSWORD para os caracteres restantes (essa divisão não será necessária em versões futuras)

O cliente Kafka que você usar deverá suportar os seguintes recursos:

* Cliente versão 0.10.x ou mais recente

* Autenticação usando o mecanismo SASL Plain

* A extensão do Service Name Identification (SNI) para o protocolo TLS v1.2

Esse método de recuperar as informações de terminal e de credencial difere do serviço do {{site.data.keyword.messagehub}}
existente. Os aplicativos que atualmente são executados em {{site.data.keyword.messagehub}} exigirão que as mudanças
reflitam os nomes do campo alternativos que são necessários de VCAP_SERVICES e para os campos de nome do usuário/senha enviados ao
Kafka. Essas mudanças não serão necessárias em versões futuras do Alpha.

## Conectando um cliente à API de REST

Para conectar um cliente à API de REST, conclua as etapas a seguir:

* O URI para a API de REST é fornecido no <code>kafka_admin_url</code>

* Configure o cabeçalho HTTP <code>Content-Type</code> para <code>application/json</code>

* Configure o cabeçalho HTTP <code>X-Auth-Token</code> para o valor de <code>apikey</code>

Para obter etapas simples para o funcionamento do Alpha, consulte
[Introdução ao programa Alpha](/docs/services/MessageHub/messagehub120.html).


## Administrando o {{site.data.keyword.messagehub}}
{: alpha_admin}

As únicas tarefas de administração necessárias em um cluster são criar, listar e excluir os tópicos que você
precisa. É possível administrar usando um dos métodos a seguir:

* As APIs do administrador do Kafka diretamente de seu aplicativo. Por exemplo, para Java, usando os métodos
<code>createTopics()</code>, <code>deleteTopics()</code> ou <code>listTopics()</code> por meio de [AdminClient
![Ícone de linkexterno](http://kafka.apache.org/11/javadoc/index.html?org/apache/kafka/clients/admin/AdminClient.html){:new_window}.


* Interativamente usando a UI da web para a instância de serviço disponível no portal do IBM Cloud.

* A API de REST administrativa fornecida no cluster.

É possível localizar mais detalhes das funções criar, listar e excluir fornecidas pela API de REST do administrador (que
é compatível com a API de administrador do {{site.data.keyword.messagehub}} existente) na especificação completa para a API
disponível no [admin-rest-api.yaml
![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/ibm-messaging/message-hub-docs/blob/master/admin-rest-api/admin-rest-api.yaml){:new_window}.
Para visualizar o arquivo swagger, use as ferramentas de Swagger, por exemplo, [Editor Swagger
![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://editor.swagger.io/#/){:new_window}.

Para obter um exemplo simples que demonstra como criar um tópico usando Curl, consulte
[Introdução ao programa Alpha](/docs/services/MessageHub/messagehub120.html).

No futuro, outras opções de configuração também estarão disponíveis.


## Amostras

Em breve...

Para obter etapas simples para o funcionamento do Alpha, consulte
[Introdução ao programa Alpha](/docs/services/MessageHub/messagehub120.html).

## Limitações do Alpha
{: alpha_limitations}

As limitações atuais desse programa Alpha são as seguintes:

### Ainda não está disponível, mas estará em breve

* Suporte para várias zonas de disponibilidade

* Ajuste de escala e limites de carga controlados pelo usuário

* Integração com o serviço do {{site.data.keyword.cloudaccesstrailfull_notm}} (anteriormente conhecido como
AccessTrail) 

### Atualmente não planejado

* Pontes

* Sistema de mensagens REST

* APIs do MQ Light










