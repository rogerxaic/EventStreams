---

copyright:
  years: 2015, 2018
lastupdated: "2018-04-12"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Introdução ao programa Alpha
{: #alpha_program}

O programa Alpha fornece acesso antecipado à próxima versão do serviço do {{site.data.keyword.messagehub}}. 

Conclua as etapas a seguir para que o aplicativo seja executado com o {{site.data.keyword.messagehub}} Alpha:


## Crie o serviço do {{site.data.keyword.messagehub}}
{: alpha_create}


  1. Clique no quadro **Hub de mensagens vNext - Produção**, que é um serviço experimental no
catálogo do [
![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.stage1.bluemix.net/catalog/labs/?search=vnext).</li>

  2. Crie um serviço {{site.data.keyword.messagehub}}. Esse serviço fornece um cluster de um único locatário pelo
plano de precificação <code>Premium</code> e geralmente leva de uma a três horas para ser provisionado.
 


## Obter credenciais usando a opção do console: criar e conectar um aplicativo de teste
{: alpha_app}

Você precisará de credenciais para trabalhar com o {{site.data.keyword.messagehub}}.
Se você ainda não tiver um aplicativo que possa usar, crie um aplicativo de teste e use as credenciais que o aplicativo usa
para se conectar ao {{site.data.keyword.messagehub}}. Por exemplo, use o serviço **SDK for Node.js**
para um aplicativo de teste. 

  1. Navegue para o quadro **SDK for Node.js** no catálogo
[
![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.stage1.bluemix.net/catalog/starters/sdk-for-nodejs).
   
  Antes de inserir um nome do aplicativo, assegure-se de selecionar uma região do sul dos EUA. Crie o aplicativo.

  2. Quando o aplicativo estiver em execução, clique na guia **Conexões** à esquerda.

  3. Clique no botão **Criar conexão**.

  4. Selecione seu novo serviço do {{site.data.keyword.messagehub}} na lista de serviços compatíveis existentes e
clique no botão **Conectar**.

  5. Na janela **Conectar serviço ativado para IAM**, aceite os padrões e clique em
**Conectar**.
  Assegure-se de que o seu serviço do {{site.data.keyword.messagehub}} esteja provisionado para que você possa se conectar a ele.

  6. Clique na guia **Tempo de execução** à esquerda e selecione a guia **Variáveis de
ambiente** no centro. Na seção **VCAP_SERVICES**, localize as informações
de <code>kafka_admin_url</code>, <code>apikey</code> e <code>kafka_brokers_sasl</code> necessárias para que você possa enviar uma mensagem.
  
## Obtenha credenciais usando a opção da linha de comandos
Como alternativa, é possível obter as credenciais necessárias usando a linha de comandos. Execute as seguintes etapas:

  1. Instale a ferramenta de linha de comandos do {{site.data.keyword.Bluemix_notm}} na CLI do [{{site.data.keyword.Bluemix_notm}}
![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/cli/index.html#overview).
  
  2. Efetue login na CLI do {{site.data.keyword.Bluemix_notm}}.
  
  3. Use a CLI do {{site.data.keyword.Bluemix_notm}} para criar uma chave de serviço com a função de gerenciador usando o
comando a seguir:
  ```
  bx resource service-key-create <NAME> Manager --instance-name <MESSAGEHUB_SERVICE_INSTANCE_NAME>
  ```
  4. A saída contém a chave API, a URL de terminal REST do administrador e a lista do broker. É possível visualizar essas
informações novamente executando o comando a seguir:
  ```
  bx resource service-key <NAME>
  ```

## Crie um tópico do {{site.data.keyword.messagehub}} e envie uma mensagem

É possível usar um comando CURL para criar um tópico e, depois, a ferramenta kafkacat para produzir e consumir uma mensagem. 

Para cada comando, substitua APIKEY e KAFKA_ADMIN_URL por valores de sua variável de ambiente VCAP_SERVICES.

  1. Na linha de comandos, crie um tópico do {{site.data.keyword.messagehub}} usando o comando CURL a seguir:
  
    ```
    curl -i -X POST -H "Content-Type: application/json" -H "X-Auth-Token: APIKEY" --data '{ "name": "newtop"}' KAFKA_ADMIN_URL/admin/topics
    ```
    {: codeblock}

  2. Instale a [ferramenta
kafkacat![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/edenhill/kafkacat#install),
que é útil para um teste rápido do Kafka.
  
  3. Para executar os próximos comandos, são necessárias as informações a seguir:
  
    * A lista de brokers, que foi retornada em suas credenciais `kafka_brokers_sasl`. A sua lista de brokers
deve ser uma lista separada por vírgula para kafkacat. 	Somente os seus primeiros cinco brokers são listados em VCAP_SERVICES. Se
você tiver mais de cinco brokers, use um cliente Kafka para recuperar os detalhes de seus outros brokers. 
  
    * Seu <code>apikey</code>. Os primeiros oito caracteres formam o seu sasl.username e o restante do <code>apikey</code>
forma o seu sasl.password.
    * O local de seus certificados SSL. Por exemplo, no Ubuntu, SSL_CERTS_DIRECTORY é <code>/etc/ssl/certs/</code>
  
  4. Produza algumas mensagens executando um comando como o seguinte:
  ```
  kafkacat -X "security.protocol=sasl_ssl" -X 'sasl.mechanisms=PLAIN' -X 'sasl.username=<FIRST_8_CHARS_FROM_APIKEY>' -X 'sasl.password=<REMAINING_CHARS_FROM_APIKEY>' -X "ssl.ca.location=<SSL_CERTS_DIRECTORY>" -b <BROKERS_LIST> -P -t <TOPIC_NAME>
    ```
		
	<br/>
  Depois de executar o comando, é possível inserir um texto como <code>HelloWorld</code> no terminal do produtor.
  
  5. Consuma as mensagens executando um comando como o seguinte:
  ```
  kafkacat -X "security.protocol=sasl_ssl" -X 'sasl.mechanisms=PLAIN' -X 'sasl.username=<FIRST_8_CHARS_FROM_APIKEY>' -X 'sasl.password=<REMAINING_CHARS_FROM_APIKEY>' -X "ssl.ca.location=<SSL_CERTS_DIRECTORY>" -b <BROKERS_LIST> -C -t <TOPIC_NAME> -f 'Topic %t [%p] at offset %o: key %k: %s\n'
  ```
	
	<br/>
  É necessário ver <code>HelloWorld</code> no terminal do consumidor.

