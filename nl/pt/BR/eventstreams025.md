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
{:note: .note}

# Usando a API de REST producer
{: #rest_producer_using}


** A API de REST Producer está disponível como parte dos planos Standard e Enterprise do {{site.data.keyword.messagehub}} apenas.**
<br/>

O {{site.data.keyword.messagehub}} fornece uma API de REST para ajudar a conectar seus sistemas existentes ao seu cluster Kafka do {{site.data.keyword.messagehub}}. Usando a API, é possível integrar o {{site.data.keyword.messagehub}} a qualquer sistema que suporte APIs de RESTful.

A API de REST producer é uma interface REST escalável para produzir mensagens para o {{site.data.keyword.messagehub}} por meio de um terminal HTTP seguro. Envie dados do evento para o {{site.data.keyword.messagehub}}, use a tecnologia Kafka para manipular feeds de dados e aproveite os recursos do {{site.data.keyword.messagehub}} para gerenciar seus dados.

Use a API para conectar sistemas existentes ao {{site.data.keyword.messagehub}}. Crie solicitações de produção de seus sistemas para o {{site.data.keyword.messagehub}}, incluindo a especificação da chave da mensagem, dos cabeçalhos e dos tópicos para os quais você deseja gravar mensagens.


## Produzindo mensagens usando REST
{: #rest_produce_messages}

Use a API producer para gravar mensagens em tópicos. Para poder produzir para um tópico, deve-se ter as informações a seguir disponíveis:

* A URL do terminal de API do {{site.data.keyword.messagehub}}, incluindo o número da porta.
* O tópico para o qual você deseja produzir.
* A chave de API que fornece permissão para conectar e produzir para o tópico selecionado.

Deve-se recuperar os detalhes da URL e da credencial necessários para se conectar à API de um objeto de credenciais de serviço ou de uma chave de serviço da instância de serviço. Para obter informações sobre como criar esses objetos, consulte [Conectando ao {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting).

A URL para o terminal de API é fornecida na propriedade <code>kafka_http_url</code>.

Use um dos métodos a seguir para autenticar:

* **Para autenticar usando a autenticação básica:**<br/> use as propriedades <code>user</code> e <code>api_key</code> dos objetos acima como o nome de usuário e a senha. Coloque esses valores no cabeçalho de <code>Authorization</code> da solicitação de HTTP no formulário <code>Basic &lt;base64 encoding of username and password joined by a single colon (:)&gt;</code>.

* **Para se autenticar usando um token de acesso:**<br/> Para obter seu token usando a CLI do IBM Cloud, primeiro efetue login no IBM Cloud e depois execute o comando a seguir: 

    ```
    ibmcloud iam oauth-tokens
    ```
    {: codeblock}

    Coloque esse token no cabeçalho Authorization da solicitação de HTTP no formato <code>Bearer <token></code>. A chave de API ou os tokens JWT são suportados. 

* ** Para se autenticar diretamente usando a api_key:**<br/> Coloque a chave diretamente como o valor do cabeçalho de HTTP <code>X-Auth-Token</code>.

<br/>
O código a seguir mostra um exemplo de envio de uma mensagem usando curl:

```
curl -v -X POST -H "Authorization: Basic <base64 username:password>" -H "Content-Type: text/plain" -H "Accept: application/json" -d 'test message' "<kafka_http_url>/topics/<topic_name>/records"
```
{: codeblock}


## Referência de API
{: #rest_api_reference}

Para obter detalhes completos da API, consulte a
[Referência da API de REST producer do {{site.data.keyword.messagehub}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://ibm.github.io/event-streams/api/){:new_window}.












