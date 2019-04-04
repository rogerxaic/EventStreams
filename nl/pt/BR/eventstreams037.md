---

copyright:
  years: 2015, 2019
lastupdated: "2018-07-05"

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

O {{site.data.keyword.messagehub}} fornece uma API de administração RESTful que você pode
usar para criar, excluir e listar tópicos.
{: shortdesc}

Para descobrir o terminal da API Administration RESTful, seu aplicativo
{{site.data.keyword.Bluemix_short}}
pode usar a propriedade `kafka_admin_url` na variável de ambiente VCAP_SERVICES
            do aplicativo.

É possível fazer download da especificação completa para a API do [admin-rest-api.yaml ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/ibm-messaging/event-streams-docs/blob/master/admin-rest-api/admin-rest-api.yaml){:new_window}.
Para visualizar o arquivo swagger, use as ferramentas Swagger, por exemplo, o [Editor
Swagger ![ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://editor.swagger.io/#/){:new_window}.

O diretório [{{site.data.keyword.messagehub}} admin-rest-api ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/ibm-messaging/event-streams-docs/tree/master/admin-rest-api){:new_window} também contém um conjunto útil de exemplos de como chamar a API de administração RESTful.


