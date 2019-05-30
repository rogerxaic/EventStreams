---

copyright:
  years: 2015, 2019
lastupdated: "2018-05-25"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Usando a API de REST do Kafka no plano Clássico 
{: #rest_using_classic}

** Essa versão da API de REST do Kafka está disponível somente como parte do plano Clássico.**
<br/>

A API REST Kafka fornece uma interface RESTful para um cluster Kafka. É possível produzir e consumir mensagens usando a API. Para obter mais informações, incluindo a documentação de referência da API, consulte [Docs do proxy REST Kafka ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://docs.confluent.io/2.0.0/kafka-rest/docs/index.html){:new_window}. Somente o formato binário integrado é suportado para solicitações e respostas no {{site.data.keyword.messagehub}}. Os formatos integrados do Avro e JSON não são suportados.
{: shortdesc}

Se você estiver usando o CURL, será possível usar um exemplo como o seguinte para produzir:
<pre class="pre"><code>
curl -X POST -H "X-Auth-Token:<var class="keyword varname">APIKEY</var>" -H "Content-Type: application/vnd.kafka.binary.v1+json" <var class="keyword varname">kafka-rest endpoint</var>/topics/<var class="keyword varname">topic name</var> -d 

'
{
  "records": [
    {
      "value": "<var class="keyword varname">A base 64 encoded value string</var>"
    }
  ]
}
'
</code></pre>
{: codeblock}
<br/><br/> Se você estiver usando a CURL, será possível usar um exemplo como o seguinte para consumir:
<pre class="pre"><code>
curl -X GET -H "X-Auth-Token:<var class="keyword varname">APIKEY</var>" -H "Accept: application/vnd.kafka.binary.v1+json" <var class="keyword varname">kafka-rest endpoint</var>/topics/<var class="keyword varname">topic name</var>/partitions/<var class="keyword varname">partition ID</var>/messages?offset=<var class="keyword varname">offset to start from</var>
</code></pre>
{: codeblock}

<br/>
<br/>
Para o CURL, também é possível adaptar os exemplos de código detalhados nos [Confluent docs ![Ícone de linkexterno](../../icons/launch-glyph.svg "Ícone de link externo")](http://docs.confluent.io/2.0.0/){:new_window} incluindo a linha a seguir na linha de comandos:

<pre class="pre">-H "X-Auth-Token: <var class="keyword varname">APIKEY</var>"</pre>
{: codeblock}

<br/>
## Como conectar e autenticar
{: #rest_connect_classic}

<!-- info was in eventstreams066.md -->

Para conectar-se ao {{site.data.keyword.messagehub}}, a API de REST do Kafka usa as
credenciais <code>api_key</code> e <code>kafka_rest_url</code> da
[variável de ambiente VCAP_SERVICES](/docs/services/EventStreams?topic=eventstreams-connecting#connect_classic_cf).

Para autenticar com a API REST do {{site.data.keyword.messagehub}} Kafka, você
                deve especificar o <code>api_key</code> no cabeçalho X-Auth-Token de suas
                solicitações.


## Como usar a API
{: #rest_how_classic}

<!-- info was in eventstreams097.md -->

A amostra da API REST Kafka do {{site.data.keyword.messagehub}} é um aplicativo Node.js
que faz conexão com o {{site.data.keyword.messagehub}} por meio da API REST Kafka para produzir e
consumir mensagens.

O código de amostra está no [projeto do GitHub event-streams-samples ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample){:new_window}.

Siga as instruções no arquivo [README.md ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample){:new_window} para esse projeto para construir e executar a amostra.

	O blog a seguir fornece uma boa introdução aos pontos fortes e fracos da API de REST do Kafka. [Um proxy REST de software livre abrangente para o Kafka. ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://www.confluent.io/blog/a-comprehensive-open-source-rest-proxy-for-kafka/){: new_window}.








