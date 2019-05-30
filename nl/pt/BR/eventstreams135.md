---

copyright:
  years: 2015, 2019
lastupdated: "2018-11-07"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Conectando ao {{site.data.keyword.messagehub}} usando o plano Clássico 
{: #connecting_classic}

A maneira como você se conecta ao {{site.data.keyword.messagehub}} varia dependendo de se você está se conectando por meio de um aplicativo Cloud Foundry ou de qualquer outro cliente externo. É
necessário coletar duas partes de informações para se conectar a qualquer uma das APIs do {{site.data.keyword.messagehub}}:
{: shortdesc}

* As URLs de terminal para as APIs
* Credenciais para autenticação

Leia as informações a seguir para saber como obter esses detalhes. As etapas podem variar sutilmente para assegurar que você
conclua as etapas apropriadas para a sua instância.

## Provisão de uma instância do  {{site.data.keyword.messagehub}}

Como um pré-requisito, deve-se primeiro fornecer uma instância de serviço do {{site.data.keyword.messagehub}} para o plano Clássico. Em seguida, obtenha os detalhes da conexão da API do {{site.data.keyword.messagehub}} concluindo as tarefas a
seguir.

## Visão geral do plano Clássico
{: #connect_classic_plan}

Os serviços que são fornecidos usando o plano Clássico são os serviços do Cloud Foundry. Isso significa que eles são
implementados em uma organização e espaço do Cloud Foundry e que são agrupados no painel sob o título ** Serviços do
Cloud Foundry **. O método usado para conectar um aplicativo depende de onde o aplicativo é implementado, ou seja, no Cloud Foundry ou fora dele, por exemplo, no serviço do Kubernetes.


## Aplicativos do Cloud Foundry no plano Clássico
{: #connect_classic_cf_plan}

Para aplicativos em execução dentro do Cloud Foundry, ligue o seu aplicativo à instância do serviço
{{site.data.keyword.messagehub}}. Quando ligado, os detalhes da conexão são disponibilizados para o aplicativo no formato JSON
na variável de ambiente VCAP_SERVICES. É possível ligar um aplicativo e um serviço usando o console do IBM Cloud ou a CLI do IBM Cloud.

Aqui está um exemplo de VCAP_SERVICES:

```
{
  "credentials": {
    "mqlight_lookup_url": "https://mqlight-lookup.messagehub.services.us-south.bluemix.net/Lookup?serviceId=584e8436-e7f5-43db-96ac-2864fccae5ae",
    "api_key": <YOUR_API_KEY>,
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
{: #connect_classic_plan_cf_console }

1. Verifique se você está na organização e no espaço desejados do Cloud Foundry.
2. Localize o seu aplicativo Cloud Foundry no painel. Se você ainda não tiver um aplicativo Cloud Foundry, será possível
criar um clicando no botão **Criar recurso**.
3. Clique no tile do aplicativo.
4. Clique em  ** Conexões **.
5. Clique em **Criar conexão**.
6. Selecione o bloco do serviço do {{site.data.keyword.messagehub}} com o qual deseja se ligar e clique em **Conectar**. Pode ser necessário remontar seu aplicativo para que as mudanças entrem em vigor.
7. Clique na guia **Tempo de execução** à esquerda e selecione a guia **Variáveis de
ambiente** no centro. Agora é possível verificar suas informações de VCAP_SERVICES e o seu aplicativo pode agora
acessá-las como variáveis de ambiente. 


### Obtenha credenciais usando a CLI do IBM Cloud 
{: #connect_classic_plan_cf_cli }

<ol>
<li>Verifique se você está na organização e no espaço desejados do Cloud Foundry. É possível navegar interativamente executando o comando a seguir:<br/>
<code>ibmcloud target --cf</code>
</li>
<li>Localize seu aplicativo:<br/><code>ibmcloud app list</code><br/>
</br>
Se você tiver um arquivo de manifesto, será possível criar um novo app executando:</br>
<code>push do aplicativo ibmcloud</code>
</li>
<li>Localize seu serviço:</br>
<code>ibmcloud service list</code>
</li>
<li>Ligue seu app ao serviço:</br>
<code>ibmcloud service bind <var class="keyword varname">your_app_name</var> <var class="keyword varname">your_service_name</var></code>
</li>
<li>Verifique se a variável de ambiente VCAP_SERVICES está disponível em seu tempo de execução do aplicativo executando:</br>
 <code>ibmcloud app env <var class="keyword varname">your_app_name</var></code>.
</li>
<li>Passe essas credenciais para o seu aplicativo. Especifique <code>token</code> como o nome de usuário e <var class="keyword varname">api_key</var> como a sua senha. Separe
<code>token</code> e <var class="keyword varname">api_key</var> com dois-pontos. Para obter mais informações, consulte [Configurando seu cliente](/docs/services/EventStreams?topic=eventstreams-kafka_connect).
<p>Pode ser necessário remontar seu aplicativo para que as mudanças entrem em vigor.</p></li>
</ol>

## Aplicativos externos no plano Clássico
{: #connect_classic_plan_external}

Para aplicativos em execução fora do Cloud Foundry, as credenciais são geradas criando uma chave de serviço. Quando você tiver
obtido uma chave de serviço, passe manualmente os detalhes da chave para seu o aplicativo usando o método escolhido.

### Obtenha credenciais usando o console do IBM Cloud
{: #connect_classic_plan_external_console}

1. Verifique se você está na organização e no espaço desejados do Cloud Foundry.
2. Localize o seu serviço Cloud Foundry {{site.data.keyword.messagehub}} no painel.
3. Clique em seu bloco do serviço.
4. Clique em  ** Credenciais de serviço **.
5. Clique em  ** Nova credencial **.
6. Insira os detalhes para a sua nova credencial como um nome e clique em **Incluir**. Uma nova credencial
aparece na lista de credenciais.
7. Clique nessa credencial usando **Visualizar credenciais** para revelar os detalhes no formato JSON.
8. Passe essas credenciais para o seu aplicativo. Especifique <code>token</code> como o nome de usuário e <var class="keyword varname">api_key</var> como a sua senha. Separe
<code>token</code> e <var class="keyword varname">api_key</var> com dois-pontos. Para obter mais informações, consulte [Configurando seu cliente](/docs/services/EventStreams?topic=eventstreams-kafka_connect).

### Obtenha credenciais usando a CLI do IBM Cloud 
{: #connect_classic_plan_external_cli }

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
ou use uma chave de serviço existente: <br/><code>ibmcloud service keys <var class="keyword varname">your_service_name</var></code>
</li>
<li>Obtenha os detalhes para a chave:</br><code>ibmcloud service key-show <var class="keyword varname">your_service_name</var><var class="keyword varname">service _key_name</var></code></br> Isso retorna os detalhes da chave de serviço no formato JSON.</li>
<li>Passe essas credenciais para o seu aplicativo. Especifique <code>token</code> como o nome de usuário e <var class="keyword varname">api_key</var> como a sua senha. Separe
<code>token</code> e <var class="keyword varname">api_key</var> com dois-pontos. Para obter mais informações, consulte [Configurando seu cliente](/docs/services/EventStreams?topic=eventstreams-kafka_connect).</li>
</ol>
 
## O que fazer em seguida
{: #after_connecting_next}

Agora que você tem informações de conexão e de credenciais, é possível escolher um cliente do {{site.data.keyword.messagehub}}. Consulte [Escolhendo entre as três APIs](/docs/services/EventStreams?topic=eventstreams-choose_api_classic) para obter informações sobre qual cliente escolher e como se conectar.










 















