---

copyright:
  years: 2015, 2019
lastupdated: "2018-11-07"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Conectando ao  {{site.data.keyword.messagehub}}
{: #connecting}

A maneira pela qual você se conecta ao {{site.data.keyword.messagehub}} varia, dependendo de se você está usando o plano Standard ou o plano Enterprise e também se está se conectando por meio de um aplicativo Cloud Foundry ou por meio de qualquer outro cliente externo. É
necessário coletar duas partes de informações para se conectar a qualquer uma das APIs do {{site.data.keyword.messagehub}}:
{: shortdesc}

* As URLs de terminal para as APIs
* Credenciais para autenticação

Leia as informações a seguir para saber como obter esses detalhes. As etapas podem variar sutilmente para assegurar que você
conclua as etapas apropriadas para a sua instância.

## Provisão de uma instância do  {{site.data.keyword.messagehub}}

Como pré-requisito, deve-se primeiro provisionar uma instância do serviço {{site.data.keyword.messagehub}} para o plano Standard ou o plano Enterprise. Em seguida, obtenha os detalhes da conexão da API do {{site.data.keyword.messagehub}} concluindo as tarefas a
seguir.

## Visão geral do plano Standard
{: #connect_standard}

Os serviços que são provisionados usando o Plano Standard são serviços do Cloud Foundry. Isso significa que eles são
implementados em uma organização e espaço do Cloud Foundry e que são agrupados no painel sob o título ** Serviços do
Cloud Foundry **. O método usado para conectar um aplicativo depende de onde o aplicativo é implementado, ou seja, no Cloud Foundry ou fora dele, por exemplo, no serviço do Kubernetes.


## Aplicativos do Cloud Foundry no plano Standard
{: #connect_standard_cf}

Para aplicativos em execução dentro do Cloud Foundry, ligue o seu aplicativo à instância do serviço
{{site.data.keyword.messagehub}}. Quando ligado, os detalhes da conexão são disponibilizados para o aplicativo no formato JSON
na variável de ambiente VCAP_SERVICES. É possível ligar um aplicativo e um serviço usando o console do IBM Cloud ou a CLI do IBM Cloud.

Aqui está um exemplo de VCAP_SERVICES:

```
{
  "credentials": {
    "mqlight_lookup_url": "https://mqlight-lookup.messagehub.services.us-south.bluemix.net/Lookup?serviceId=584e8436-e7f5-43db-96ac-2864fccae5ae",
    "api_key": "d9JSx1SYsmLzNRbbgUFneDm2DtkedlVeViObYJIvrPAf2kJA",
    "kafka_admin_url": "https://kafka-admin.messagehub.services.us-south.bluemix.net:443",
    "kafka_rest_url": "https://kafka-rest.messagehub.services.us-south.bluemix.net:443",
    "kafka_brokers_sasl": [
      "kafka01.messagehub.services.us-south.bluemix.net:9093",
      "kafka02.messagehub.services.us-south.bluemix.net:9093",
      "kafka03.messagehub.services.us-south.bluemix.net:9093",
      "kafka04.messagehub.services.us-south.bluemix.net:9093",
      "kafka05.messagehub.services.us-south.bluemix.net:9093"
    ],
    "user": "d9JSx1SYsmLzNRbb",
    "password": "gUFneDm2DtkedlVeViObYJIvrPAf2kJA"
  }
}
```

{: codeblock}

O conteúdo da variável de ambiente é o mesmo, independente da API que você usar para se conectar
      ao {{site.data.keyword.messagehub}}. Seu aplicativo {{site.data.keyword.Bluemix_notm}}
seleciona as credenciais apropriadas na variável de ambiente VCAP_SERVICES, dependendo da interface em uso.
 
Somente os seus primeiros cinco brokers são listados em VCAP_SERVICES. Se você tiver mais de cinco brokers, use um cliente
Kafka para recuperar os detalhes de seus outros brokers. 


### Obter credenciais e conectar usando o console do IBM Cloud
{: #connect_standard_cf_console }

1. Verifique se você está na organização e no espaço desejados do Cloud Foundry.
2. Localize o seu aplicativo do Cloud Foundry no painel. Se você ainda não tiver um aplicativo do Cloud Foundry, será possível
criar um clicando no botão **Criar recurso**.
3. Clique no tile do aplicativo.
4. Clique em  ** Conexões **.
5. Clique em **Criar conexão**.
6. Selecione o bloco do serviço do {{site.data.keyword.messagehub}} com o qual deseja se ligar e clique em **Conectar**. Pode ser necessário remontar seu aplicativo para que as mudanças entrem em vigor.
7. Clique na guia **Tempo de execução** à esquerda e selecione a guia **Variáveis de
ambiente** no centro. Agora é possível verificar suas informações de VCAP_SERVICES e o seu aplicativo pode agora
acessá-las como variáveis de ambiente. 


### Obtenha credenciais usando a CLI do IBM Cloud 
{: #connect_standard_cf_cli }

<ol>
<li>Verifique se você está na organização e no espaço desejados do Cloud Foundry. É possível navegar interativamente executando o comando a seguir:<br/>
<code>ibmcloud target --cf</code>
</li>
<li>Localize o seu aplicativo:<br/> <code>ibmcloud app list</code> <br/>
</br>
Se você tiver um arquivo manifest, poderá criar um novo app executando:</br>
<code>ibmcloud app push</code>
</li>
<li>Localize seu serviço:</br> 
<code>ibmcloud service list</code>
</li>
<li>Ligue o seu aplicativo ao serviço:</br>
<code>ibmcloud service bind <var class="keyword varname">your_app_name</var>
<var class="keyword varname">your_service_name</var></code>
</li>
<li>Verifique se a variável de ambiente VCAP_SERVICES está disponível no tempo de execução do seu aplicativo executando:</br> 
 <code>ibmcloud app env <var class="keyword varname">your_app_name</var></code>. 
</li>
<li>Passe essas credenciais para o seu aplicativo. Especifique <code>token</code> como o nome de usuário e <var class="keyword varname">api_key</var> como a sua senha. Separe
<code>token</code> e <var class="keyword varname">api_key</var> com dois-pontos. Para obter mais informações, consulte [Configurando seu cliente](/docs/services/EventStreams/eventstreams063.html).
<p>Pode ser necessário remontar seu aplicativo para que as mudanças entrem em vigor.</p></li>
</ol>

## Aplicativos externos no plano Standard
{: #connect_standard_external}

Para aplicativos em execução fora do Cloud Foundry, as credenciais são geradas criando uma chave de serviço. Quando você tiver
obtido uma chave de serviço, passe manualmente os detalhes da chave para seu o aplicativo usando o método escolhido.

### Obtenha credenciais usando o console do IBM Cloud
{: #connect_standard_external_console}

1. Verifique se você está na organização e no espaço desejados do Cloud Foundry.
2. Localize o seu serviço Cloud Foundry {{site.data.keyword.messagehub}} no painel.
3. Clique em seu bloco do serviço.
4. Clique em  ** Credenciais de serviço **.
5. Clique em  ** Nova credencial **.
6. Insira os detalhes para a sua nova credencial como um nome e clique em **Incluir**. Uma nova credencial
aparece na lista de credenciais.
7. Clique nessa credencial usando **Visualizar credenciais** para revelar os detalhes no formato JSON.
8. Passe essas credenciais para o seu aplicativo. Especifique <code>token</code> como o nome de usuário e <var class="keyword varname">api_key</var> como a sua senha. Separe
<code>token</code> e <var class="keyword varname">api_key</var> com dois-pontos. Para obter mais informações, consulte [Configurando seu cliente](/docs/services/EventStreams/eventstreams063.html).

### Obtenha credenciais usando a CLI do IBM Cloud 
{: #connect_standard_external_cli }

<ol>
<li>Verifique se você está na organização e no espaço desejados do Cloud Foundry. É possível navegar interativamente executando o comando a seguir:<br>
<code>ibmcloud target --cf</code>
</li>
<li>Localize seu serviço:<br>
<code>ibmcloud service list</code>
</li>
<li>É possível criar uma chave de serviço:<br>
<code>ibmcloud service key-create <var class="keyword varname">your_service_name</var> <var class="keyword varname">new_service_key_name</var></code><br>
<br/>
ou usar uma chave de serviço existente: <br/>
<code>ibmcloud service keys <var class="keyword varname">your_service_name</var></code> 
</li>
<li>Obtenha os detalhes para a chave:</br>
<code>ibmcloud service key-show <var class="keyword varname">your_service_name</var> <var class="keyword varname">service _key_name</var></code></br>
Isso retorna os detalhes da chave de serviço no formato JSON.</li>
<li>Passe essas credenciais para o seu aplicativo. Especifique <code>token</code> como o nome de usuário e <var class="keyword varname">api_key</var> como a sua senha. Separe
<code>token</code> e <var class="keyword varname">api_key</var> com dois-pontos. Para obter mais informações, consulte [Configurando seu cliente](/docs/services/EventStreams/eventstreams063.html).</li>
</ol>

 
## Visão geral do plano Enterprise
{: #connect_enterprise}

Os serviços provisionados usando o Plano Enterprise são agrupados no painel sob o título **Serviços**. O
plano Enterprise é ativado para [IAM ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/docs/iam/quickstart.html#getstarted){:new_window}. Não é necessário entender o IAM para iniciar, mas algum conhecimento é recomendado se você deseja proteger o seu serviço
{{site.data.keyword.messagehub}}. Para obter mais informações, consulte
[Protegendo seus recursos do {{site.data.keyword.messagehub}}](/docs/services/EventStreams/eventstreams124.html)

Conclua as etapas a seguir para ligar o seu aplicativo e obter as chaves de serviço para o seu serviço. Para ser autorizado a
criar tópicos, o seu aplicativo ou chave de serviço deve ter uma função de acesso Gerenciador.

Para conectar um aplicativo, o método usado depende de onde o aplicativo é implementado, ou seja, no Cloud Foundry ou fora dele, por exemplo, no serviço do Kubernetes.

## Aplicativos do Cloud Foundry no plano Enterprise
{: #connect_enterprise_cf}

O seu aplicativo deve ser ligado à instância do serviço {{site.data.keyword.messagehub}}. Para ligar um aplicativo do Cloud Foundry a um serviço que não seja do Cloud Foundry, crie um alias do serviço do Cloud Foundry primeiro e, em seguida, referencie esse alias por meio do aplicativo do Cloud Foundry ao realizar a ligação.

Quando ligado, os detalhes da conexão são, então, disponibilizados para o aplicativo no formato JSON usando a variável de
ambiente VCAP_SERVICES. É possível ligar um aplicativo e um serviço usando o console do IBM Cloud ou a CLI do IBM Cloud.

### Ligue um aplicativo usando o console do IBM Cloud
{: #connect_enterprise_cf_console}

1. Verifique se você está na organização e no espaço desejados do Cloud Foundry.
2. Localize o seu aplicativo do Cloud Foundry no painel ou crie um aplicativo clicando no botão **Criar recurso
**.
3. Clique no tile do aplicativo.
4. Clique em  ** Conexões **.
5. Clique em **Criar conexão**.
6. Selecione o bloco do serviço do {{site.data.keyword.messagehub}} com o qual deseja se ligar e clique em **Conectar**. 
7. Na janela **Conectar serviço ativado pelo IAM** que aparece, selecione uma função de acesso em **Função de acesso para conexão** e um ID de serviço na lista **ID de serviço para conexão** (é possível aceitar o ID gerado automaticamente). Clique em  ** Conectar **. 

  Isso cria um alias de serviço do Cloud Foundry para seu serviço {{site.data.keyword.messagehub}} e, em seguida, liga seu aplicativo a esse alias. 

  Remonte seu aplicativo para que as mudanças entrem em vigor.<br/>
8. Clique na guia **Tempo de execução** à esquerda e selecione a guia **Variáveis de
ambiente** no centro. Agora é possível verificar as suas informações de VCAP_SERVICES. Agora seu aplicativo pode acessá-las como variáveis de ambiente. 
 

### Ligue um aplicativo usando a CLI do IBM Cloud
{: #connect_enterprise_cf_cli}

<ol>
<li>Verifique se você está na organização e no espaço desejados do Cloud Foundry. É possível navegar interativamente executando o comando a seguir:<br/>
 <code>ibmcloud target --cf</code></li>
<li>Localize o seu aplicativo:</br>
<code>ibmcloud app list</code><br/>
<br/>
Se você tiver um arquivo manifest, poderá criar um novo app executando:<br/>
<code>ibmcloud app push</code><br/>
<br/>
Como o aplicativo ainda não está ligado ao {{site.data.keyword.messagehub}}, o aplicativo não pode estabelecer uma conexão. Portanto, recomendamos enviar por push o aplicativo com o parâmetro <code>--no-start</code> para evitar falhas de
conexão desnecessárias.</li>
<li>Localize seu serviço:</br>
<code>ibmcloud resource service-instances</code></li>
<li>Crie um alias do serviço Cloud Foundry:<br/>
<code>ibmcloud resource service-alias-create <var class="keyword varname">alias_name</var> --instance-name
<var class="keyword varname">your_service_name</var></code></li>
<li>Ligue o seu aplicativo ao alias de serviço criado anteriormente:<br/>
<code>ibmcloud service bind <var class="keyword varname">your_ app_name</var> <var class="keyword varname">alias_name</var></code>.<br/>
<br/>
Como alternativa, é possível atualizar o seu arquivo manifest e enviar por push o aplicativo novamente.</li>
<li>Verifique se a variável de ambiente VCAP_SERVICES está disponível no tempo de execução do seu aplicativo:<br/>
<code>ibmcloud app env <var class="keyword varname">your_app_name</var></code></li>
<li>Passe essas credenciais para o seu aplicativo. Especifique <code>token</code> como o nome de usuário e <var class="keyword varname">api_key</var> como a sua senha. Separe
<code>token</code> e <var class="keyword varname">api_key</var> com dois-pontos. Para obter mais informações, consulte [Configurando seu cliente](/docs/services/EventStreams/eventstreams063.html). 
<p>Pode ser necessário remontar seu aplicativo para que as mudanças entrem em vigor.</p></li>
</ol>


## Aplicativos externos no plano Enterprise
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
<code>token</code> e <var class="keyword varname">api_key</var> com dois-pontos. Para obter mais informações, consulte [Configurando seu cliente](/docs/services/EventStreams/eventstreams063.html).
   <br/><br/>Assegure-se de que seu aplicativo analise os detalhes.

### Obtenha credenciais usando a CLI do IBM Cloud
{: #connect_enterprise_external_cli}

<ol>
<li>Localize seu serviço:<br/>
<code>ibmcloud resource service-instances</code></li>
<li>Crie uma chave de serviço:<br/>
<code>ibmcloud resource service-key-create <var class="keyword varname">key_name</var> <var class="keyword varname">key_role</var>
--instance-name <var class="keyword varname">your_service_name</var></code></li>
<li>Imprima a chave de serviço:<br/>
<code>ibmcloud resource service-key <var class="keyword varname">key_name</var></code></li>
<li>Passe essas credenciais para o seu aplicativo. Especifique <code>token</code> como o nome de usuário e <var class="keyword varname">api_key</var> como a sua senha. Separe
<code>token</code> e <var class="keyword varname">api_key</var> com dois-pontos. Para obter mais informações, consulte [Configurando seu cliente](/docs/services/EventStreams/eventstreams063.html).
<p>Assegure-se de que seu aplicativo analise os detalhes.</p></li>
</ol>

## O que fazer em seguida
{: #after_connecting}

Agora que você tem informações de conexão e de credenciais, é possível escolher um cliente do {{site.data.keyword.messagehub}}. A sua escolha depende de seu plano.

* Se você estiver usando o plano Standard, consulte [Escolhendo
entre as três APIs ](/docs/services/EventStreams/eventstreams087.html) para obter informações sobre qual cliente escolher e como se conectar.
* Se você estiver usando o plano Enterprise, consulte [Usando a
API do Kafka](/docs/services/EventStreams/eventstreams050.html).

	O tópico Kafka interno <code>__consumer_offsets</code> é visível para você como somente leitura caso você esteja usando
o plano Enterprise. É altamente recomendado não tentar gerenciar o tópico de nenhuma maneira. 

<!--
Charlie said:

"Add some info describing how to take the information made available from above e.g. like the info in the Connecting a client to the Kafka API section of the alpha docs on stage 1? https://console.stage1.bluemix.net/docs/services/EventStreams/eventstreams122.html#alpha_about "
-->







 















