---

copyright:
  years: 2015, 2018
lastupdated: "2018-07-04"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Gerenciando o acesso aos recursos do {{site.data.keyword.messagehub}} (plano Enterprise)
{: #security }

É possível assegurar os recursos do seu {{site.data.keyword.messagehub}} de uma maneira com baixa granularidade para gerenciar o acesso que você deseja conceder a cada usuário para cada recurso.

## O que posso assegurar?

No {{site.data.keyword.messagehub}}, é possível assegurar o acesso aos recursos a seguir:
* Cluster (cluster): é possível controlar quais aplicativos e usuários podem se conectar ao serviço
* Tópicos (tópico): é possível controlar a capacidade de usuários e aplicativos para criar, excluir, ler e gravar em um tópico 
* Grupos de consumidores (grupo): é possível controlar a capacidade de um aplicativo de se associar a um grupo de consumidores 
* Transações do produtor (txnid): é possível controlar a capacidade de usar o recurso de produtor transacional em Kafka (isto é, gravações únicas e atômicas em múltiplas partições)

Os níveis de acesso (também conhecidos como uma função) que podem ser atribuídos a um usuário para cada recurso são os seguintes:

| Função de acesso | Descrição das ações | Exemplo de ações |
|:-----------------|:-----------------|:-----------------|
|  Reader | Executar ações somente leitura no {{site.data.keyword.messagehub}} como visualizar recursos | Permitir que um aplicativo se conecte a um cluster designando o acesso de leitura ao tipo de recurso de cluster |
| Gravador | Os gravadores têm permissões além da função de leitor, incluindo a criação e a edição de recursos do {{site.data.keyword.messagehub}}. | Permitir que um aplicativo produza para tópicos designando acesso de gravação a tipos de recurso de tópico e de nome de tópico|
| Gerente | Os gerentes têm permissões além da função de gravador para concluir ações privilegiadas. Além disso, é possível criar e editar recursos do {{site.data.keyword.messagehub}}. | Permitir acesso total a todos os recursos, designando acesso de gerenciador à instância do {{site.data.keyword.messagehub}}|
{: caption="Tabela 1. Exemplo de funções e ações de usuário do {{site.data.keyword.messagehub}}" caption-side="top"}

<!-- comment from Charlie and my reply 
CM: need to confirm if hierarchical e.g. write includes read - and doc. 
KR: I think they do inherit the lower level access https://console.bluemix.net/docs/iam/users_roles.html#iamusermanrol 
-->


## Como designar acesso?

Políticas do Cloud Identity and Access Management (IAM) são anexadas aos recursos para serem controladas. Cada política define o nível de acesso que um usuário específico deve ter e para qual recurso ou conjunto de recursos. Uma política consiste nas informações a seguir: 
* O tipo de serviço ao qual a política se aplica. Por exemplo,  {{site.data.keyword.messagehub}}. É possível definir o escopo de uma política para incluir todos os tipos de serviço. 
* A instância do serviço a ser assegurada. É possível definir o escopo de uma política para incluir todas as instâncias de um tipo de serviço. 
* O tipo de recurso a ser assegurado. Os valores válidos são <code>cluster</code>, <code>topic</code>, <code>group</code> ou <code>txnid</code>. Especificar um tipo é opcional. Se você não especificar um tipo, a política será então aplicada a todos os recursos na instância de serviço. 
* O recurso a ser assegurado. Especifique para recursos de tipo <code>topic</code>, <code>group</code> e <code>txnid</code>. Se você não especificar o recurso, a política será então aplicada a todos os recursos do tipo especificado na instância de serviço. 
* A função designada ao usuário. Por exemplo, Leitor, Gravador ou Gerenciador. 

## Quais são as configurações de segurança padrão?

Por padrão, quando o {{site.data.keyword.messagehub}} é provisionado, concede-se ao usuário que o provisionou a função de gerenciador para todos os recursos da instância. Além disso, qualquer usuário com uma função de gerenciador para 'Todos' os serviços ou 'Todas' as instâncias do serviço {{site.data.keyword.messagehub}} na mesma conta também tem acesso total. 

É possível, então, aplicar políticas adicionais para estender o acesso a outros usuários. É possível definir o escopo de uma política para aplicar ao {{site.data.keyword.messagehub}} como um todo ou a recursos individuais no {{site.data.keyword.messagehub}}. Para obter mais informações, consulte [Cenários comuns](#security_scenarios).

Somente usuários com uma função de administração para uma conta podem designar políticas aos usuários. Designe políticas usando o painel do IBM Cloud ou usando os comandos **ibmcloud**. 
<!--
For example steps for {{site.data.keyword.messagehub}}, see [Examples](#security_examples).
-->


## Cenários comuns
{: #security_scenarios }

Essa tabela resume alguns cenários comuns do {{site.data.keyword.messagehub}} e o acesso que você precisa designar:

| Ações | Função de leitor | Função de gravador | função de gerenciador |
|---------|----------------|
| Permitir acesso total a todos os recursos|Não aplicável   |Não aplicável  |Instância de serviço:
<var class="keyword varname">your_service_instance</var>|
| Permitir que um aplicativo ou usuário crie ou exclua o tópico |Tipo de recurso: <code>cluster</code>   |Não aplicável  |Tipo de recurso: topic <br/><br/>Opcional: ID do recurso: <var class="keyword varname">name_of_topic</var> |
| Listar grupos, tópicos e deslocamentos <br/> Descrever o grupo, o tópico e as configurações de broker | Tipo de recurso: <code>cluster</code>      |Não aplicável  |Não aplicável      |
| Permitir que um aplicativo se conecte ao cluster  |Tipo de recurso: <code>cluster</code>| Não aplicável     |Não aplicável      |
| Permitir que um aplicativo produza para qualquer tópico  |Tipo de recurso: <code>cluster</code>|Tipo de recurso: <code>topic</code> |Não aplicável     |
| Permitir que um aplicativo produza para um tópico específico  |Tipo de recurso: <code>cluster</code>|Tipo de recurso: <code>topic</code><br/>ID do recurso: <var class="keyword varname">name_of_topic</var>      |Não aplicável     |
| Permitir que um aplicativo se conecte e consuma de qualquer tópico (nenhum grupo de consumidores)  |Tipo de recurso: <code>cluster</code> <br/>Tipo de recurso: <code>topic</code> |Tipo de recurso: <code>topic</code>|Não aplicável     |
| Permitir que um aplicativo se conecte e consuma de um tópico específico (nenhum grupo de consumidores)  | Tipo de recurso: <code>cluster</code> <br/>Tipo de recurso: <code>topic</code><br/>ID do recurso: <var class="keyword varname">name_of_topic</var> |Não aplicável     |Não aplicável     |
| Permitir que um aplicativo consuma um tópico (grupo de consumidores)  |Tipo de recurso: <code>cluster</code> <br/>Tipo de recurso: <code>topic</code><br/> Tipo de recurso: <code>group</code> |Não aplicável      |Não aplicável     |
| Permitir que um aplicativo produza para um tópico transacionalmente  |Tipo de recurso: <code>cluster</code> <br/> Tipo de recurso: <code>group</code>|Tipo de recurso: <code>topic</code> <br/>ID do recurso: <var class="keyword varname">name_of_topic</var> <br/>Tipo de recurso: <code>txnid</code> |Não aplicável     |
| Excluir grupo de consumidores |Tipo de recurso: <code>cluster</code> |Não aplicável  |Tipo de recurso: <code>group</code> <br/>ID do recurso: <var class="keyword varname">group_ID</var>      |
| Para usar o Streams |Tipo de recurso: <code>cluster</code></br>Tipo de recurso: <code>group</code>| Não aplicável  |Tipo de recurso: <code>topic</code>    |

Para obter mais informações sobre o IAM, consulte [IBM Cloud Identity and
gerenciamento de acesso](/docs/iam/index.html#iamoverview).

Para obter um exemplo sobre como configurar políticas, veja:
[Chaves API e IDs de serviço do IBM Cloud IAM![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/blogs/bluemix/2017/10/introducing-ibm-cloud-iam-service-ids-api-keys/){:new_window}.


## Conectando ao  {{site.data.keyword.messagehub}}
{: #connect_message_enterprise }

Para obter informações sobre como ligar um aplicativo Cloud Foundry ou obter uma credencial de chave de segurança para um aplicativo externo, consulte
[Conectando-se ao {{site.data.keyword.messagehub}}](/docs/services/EventStreams/eventstreams127.html#connect_messagehub).

<!-- 28/06/18 - Karen: draft info only

## Examples
{: #security_examples }

I want to give a user access to create or delete a topic:

1. From the IBM Cloud dashboard, go to the **Manage** tab &gt; **Security** &gt; **Identity and Access**, and then select **Users**.
2. Click **Invite users**.
3. Specify the email address of the user that you want to invite.
4. In the **Access** section, expand the **Services** option.
5. Choose to assign access to a **Resource**.
6. In the **Services** section, select **{{site.data.keyword.messagehub}}**
7. In the **Region** section, make your selection.
8. In the **Service instance** section, locate your instance and select it.
9. In the **Resource type** section, enter **cluster**.
10. In the **Select roles** section, check the **Reader** box.
11. In the **Resource type** section, enter **topic**.
12. In the **Select roles** section, check the **Manager** box.
13. Click **Invite users**.

-->















