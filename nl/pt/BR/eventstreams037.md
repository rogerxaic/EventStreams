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

# Usando a API de REST de administração
{: #admin_api}

O {{site.data.keyword.messagehub}} fornece uma API de RESTful de administração que você pode usar para criar, excluir, listar e atualizar tópicos.
{: shortdesc}

Deve-se recuperar os detalhes da URL e da credencial necessários para se conectar à API de um objeto de credenciais de serviço ou de uma chave de serviço da instância de serviço. Para obter informações sobre como criar esses objetos, consulte [Conectando ao {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting).

A URL para o terminal de API é fornecida na propriedade <code>kafka_admin_url</code>.

As credenciais dependem do método de autenticação e três tipos de credencial são suportados:

* **Para autenticar usando a autenticação básica**:<br/> 
    Use as propriedades <code>user</code> e <code>api_key</code> dos objetos acima como o nome do usuário e a senha. Coloque esses valores no cabeçalho de <code>Authorization</code> da solicitação de HTTP no formulário <code>Basic &lt;base64 encoding of username and password joined by a single colon (:)&gt;</code>.

* **Para se autenticar usando um token de acesso:**<br/> Para obter seu token usando a CLI do IBM Cloud, primeiro efetue login no IBM Cloud e depois execute o comando a seguir: 

    ```
    ibmcloud iam oauth-tokens
    ```
    {: codeblock}

    Coloque esse token no cabeçalho Authorization da solicitação de HTTP no formato <code>Bearer <token></code>. A chave de API ou os tokens JWT são suportados. 

* ** Para se autenticar diretamente usando a api_key:**<br/> Coloque a chave diretamente como o valor do cabeçalho de HTTP <code>X-Auth-Token</code>.

Para instâncias de serviço criadas no plano Clássico, essas informações estão disponíveis na variável de ambiente VCAP_SERVICES do seu aplicativo.

Para obter uma descrição da API com exemplos, consulte [admin-rest do {{site.data.keyword.messagehub}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/ibm-messaging/event-streams-docs/tree/master/admin-rest-api){:new_window}.

É possível fazer download da especificação completa para a API por meio do [arquivo YAML da API de REST Admin do {{site.data.keyword.messagehub}}![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/ibm-messaging/event-streams-docs/blob/master/admin-rest-api/admin-rest-api.yaml){:new_window}.
Para visualizar o arquivo swagger, use as ferramentas Swagger, por exemplo, o [Editor
Swagger ![ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://editor.swagger.io/#/){:new_window}.




