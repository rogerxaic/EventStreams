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


# Conectando ao  {{site.data.keyword.messagehub}}
{: #connecting}

A maneira como você se conecta ao {{site.data.keyword.messagehub}} varia dependendo de se seu aplicativo está sendo executado nativamente ou como um aplicativo Cloud Foundry. No entanto, em ambos os casos, são necessárias duas informações:
{: shortdesc}

* As URLs de terminal para as APIs
* Credenciais para autenticação

Leia as informações a seguir para saber como obter esses detalhes. As etapas podem variar sutilmente para assegurar que você
conclua as etapas apropriadas para a sua instância.

Para obter informações sobre como se conectar ao {{site.data.keyword.messagehub}} se você estiver usando o plano Clássico, consulte [Conectando-se usando o plano Clássico](/docs/services/EventStreams?topic=eventstreams-connecting_classic).


## Visão geral
{: #connect_enterprise}

Os serviços fornecidos usando os planos Padrão ou Corporativo são agrupados no painel sob o título **Serviços**. Os planos são [ativados para IAM ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/iam?topic=iam-getstarted#getstarted){:new_window}. Não é necessário entender o IAM para iniciar, mas algum conhecimento é recomendado se você deseja proteger o seu serviço
{{site.data.keyword.messagehub}}. Para obter mais informações, consulte [Gerenciando o acesso aos recursos do {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-security).

Conclua as etapas a seguir para ligar o seu aplicativo e obter as chaves de serviço para o seu serviço. Para ser autorizado a
criar tópicos, o seu aplicativo ou chave de serviço deve ter uma função de acesso Gerenciador.

Para conectar um aplicativo, o método usado depende de onde o aplicativo é implementado, ou seja, no Cloud Foundry ou fora dele, por exemplo, no serviço do Kubernetes.

## Provisão de uma instância do  {{site.data.keyword.messagehub}}

Como pré-requisito, deve-se primeiro provisionar uma instância do serviço {{site.data.keyword.messagehub}} para o plano Standard ou o plano Enterprise. Em seguida, obtenha os detalhes da conexão da API do {{site.data.keyword.messagehub}} concluindo as tarefas a
seguir.

## Conectar aplicativos 
{: #connect_enterprise_external}

Para aplicativos em execução fora do Cloud Foundry, as credenciais são geradas criando uma chave de serviço. Ao obter a chave
de serviço, transmita manualmente os detalhes da chave para o seu aplicativo usando o método escolhido.

### Obtenha credenciais usando o console do IBM Cloud
{: #connect_enterprise_external_console}

1. Localize o seu serviço {{site.data.keyword.messagehub}} no painel.
2. Clique em seu bloco do serviço.
3. Clique em  ** Credenciais de serviço **.
4. Clique em  ** Nova credencial **. 
5. Conclua os detalhes para a sua nova credencial como um nome e uma função e clique em **Incluir**. Uma nova credencial
aparece na lista de credenciais.
6. Clique nessa credencial usando **Visualizar credenciais** para revelar os detalhes no formato JSON.
7. Passe essas credenciais para o seu aplicativo. Especifique <code>token</code> como o nome de usuário e <var class="keyword varname">api_key</var> como a sua senha. Separe
<code>token</code> e <var class="keyword varname">api_key</var> com dois-pontos. Para obter mais informações, consulte [Configurando a API do seu cliente Kafka](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_api_client).
   <br/><br/>Assegure-se de que seu aplicativo analise os detalhes.

### Obtenha credenciais usando a CLI do IBM Cloud
{: #connect_enterprise_external_cli}

<ol>
<li>Localize seu serviço:<br/><code>ibmcloud resource service-instances</code></li>
<li>Crie uma chave de serviço:<br/>
<code>ibmcloud resource service-key-create <var class="keyword varname">key_name</var> <var class="keyword varname">key_role</var> --instance-name <var class="keyword varname">your_service_name</var></code></li>
<li>Imprima a chave de serviço:<br/>
<code>ibmcloud resource service-key <var class="keyword varname">key_name</var></code></li>
<li>Passe essas credenciais para o seu aplicativo. Especifique <code>token</code> como o nome de usuário e <var class="keyword varname">api_key</var> como a sua senha. Separe
<code>token</code> e <var class="keyword varname">api_key</var> com dois-pontos. Para obter mais informações, consulte [Configurando seu cliente](/docs/services/EventStreams?topic=eventstreams-kafka_connect).
<p>Assegure-se de que seu aplicativo analise os detalhes.</p></li>
</ol>

## Conectar aplicativos do Cloud Foundry
{: #connect_enterprise_cf}

O seu aplicativo deve ser ligado à instância do serviço {{site.data.keyword.messagehub}}. Para ligar um aplicativo Cloud Foundry a um serviço que não seja do Cloud Foundry, crie um alias do serviço do Cloud Foundry primeiro e, em seguida, referencie esse alias por meio do aplicativo Cloud Foundry ao realizar a ligação. 

Quando ligado, os detalhes da conexão são, então, disponibilizados para o aplicativo no formato JSON usando a variável de
ambiente VCAP_SERVICES. É possível ligar um aplicativo e um serviço usando o [console do IBM Cloud](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_cf_console) ou a [CLI do IBM Cloud](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_cf_cli).

### Ligue um aplicativo usando o console do IBM Cloud
{: #connect_enterprise_cf_console}

1. Verifique se você está na organização e no espaço desejados do Cloud Foundry.
2. Localize o seu aplicativo Cloud Foundry no painel ou crie um aplicativo clicando no botão **Criar recurso
**.
3. Clique no tile do aplicativo.
4. Clique em  ** Conexões **.
5. Clique em **Criar conexão**.
6. Selecione o bloco do serviço do {{site.data.keyword.messagehub}} com o qual deseja se ligar e clique em **Conectar**. 
7. Na janela **Conectar serviço ativado pelo IAM** que aparece, selecione uma função de acesso em **Função de acesso para conexão** e um ID de serviço na lista **ID de serviço para conexão** (é possível aceitar o ID gerado automaticamente). Clique em  ** Conectar **. 

  Isso cria um alias de serviço do Cloud Foundry para seu serviço {{site.data.keyword.messagehub}} e, em seguida, liga seu aplicativo a esse alias. 

  Remonte o seu aplicativo para que as mudanças entrem em vigor.<br/>
8. Clique na guia **Tempo de execução** à esquerda e selecione a guia **Variáveis de
ambiente** no centro. Agora é possível verificar as suas informações de VCAP_SERVICES. Agora seu aplicativo pode acessá-las como variáveis de ambiente. 
 

### Ligar um aplicativo usando a CLI do IBM Cloud
{: #connect_enterprise_cf_cli}

<ol>
<li>Verifique se você está na organização e no espaço desejados do Cloud Foundry. É possível navegar interativamente executando o comando a seguir:<br/> <code>ibmcloud target --cf</code></li>
<li>Localize seu app:</br><code>ibmcloud app list</code><br/><br/> Se você tiver um arquivo manifest, será possível criar um novo app executando:<br/><code>ibmcloud app push</code><br/><br/>. Como o app ainda não está ligado ao {{site.data.keyword.messagehub}}, o app não pode estabelecer uma conexão. Portanto, recomendamos enviar por push o aplicativo com o parâmetro <code>--no-start</code> para evitar falhas de
conexão desnecessárias.</li>
<li>Localize seu serviço:</br><code>ibmcloud resource service-instances</code></li>
<li>Crie um alias de serviço do Cloud Foundry:<br/>
<code>ibmcloud resource service-alias-create <var class="keyword varname">alias_name</var> --instance-name <var class="keyword varname">your_service_name</var></code></li>
<li>Ligar seu app ao alias de serviço criado anteriormente:<br/>
<code>Ligação de serviço ibmcloud <var class="keyword varname">your_ app_name</var><var class="keyword varname">alias_name</var></code>.<br/>
<br/> Como alternativa, é possível atualizar seu arquivo manifest e novamente enviar por push o aplicativo.</li>
<li>Verifique se a variável de ambiente VCAP_SERVICES está disponível em seu tempo de execução do aplicativo:<br/>
<code>ibmcloud app env <var class="keyword varname">your_app_name</var></code></li>
<li>Passe essas credenciais para o seu aplicativo. Especifique <code>token</code> como o nome de usuário e <var class="keyword varname">api_key</var> como a sua senha. Separe
<code>token</code> e <var class="keyword varname">api_key</var> com dois-pontos. Para obter mais informações, consulte [Configurando seu cliente](/docs/services/EventStreams?topic=eventstreams-kafka_connect). 
<p>Pode ser necessário remontar seu aplicativo para que as mudanças entrem em vigor.</p></li>
</ol>


## O que fazer em seguida
{: #after_connecting}

Agora que você tem informações de conexão e de credenciais, é possível escolher um cliente do {{site.data.keyword.messagehub}}. Para obter mais informações, consulte [Usando a API do Kafka](/docs/services/EventStreams?topic=eventstreams-kafka_using).

<!--
Charlie said:

"Add some info describing how to take the information made available from above e.g. like the info in the Connecting a client to the Kafka API section of the alpha docs on stage 1? https://console.stage1.bluemix.net/docs/services/EventStreams/eventstreams122.html#alpha_about "
-->







 















