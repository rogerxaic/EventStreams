---

copyright:
  years: 2015, 2019
lastupdated: "2018-05-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Como conectar e autenticar
{: #rest_connect}

<!-- info moved to eventstreams025.md because of doc app changes -->
** A API de REST do Kafka está disponível somente como parte do plano Standard.**
<br/>

Para conectar-se ao {{site.data.keyword.messagehub}}, a API de REST do Kafka usa as
credenciais <code>api_key</code> e <code>kafka_rest_url</code> da
[variável de ambiente VCAP_SERVICES](/docs/services/EventStreams/eventstreams127.html).
{: shortdesc}

Para autenticar com a API REST do {{site.data.keyword.messagehub}} Kafka, você
                deve especificar o <code>api_key</code> no cabeçalho X-Auth-Token de suas
                solicitações.
