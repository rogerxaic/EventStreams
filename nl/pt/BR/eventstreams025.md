---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Usando a API REST Kafka
{: #rest_using}

** A API de REST Kafka está disponível como parte somente do plano Standard.**
<br/>

A API REST Kafka fornece uma interface RESTful para um cluster Kafka. É possível produzir e consumir mensagens usando a API. Para obter mais informações, incluindo a documentação de referência da API, consulte [Docs do proxy REST Kafka ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://docs.confluent.io/2.0.0/kafka-rest/docs/index.html){:new_window}. Somente o formato binário integrado é suportado para solicitações e respostas no {{site.data.keyword.messagehub}}. Os formatos integrados do Avro e JSON não são suportados.

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
<br/>
<br/>
Se você estiver usando o CURL, será possível usar um exemplo como o seguinte para consumir:
<pre class="pre"><code>
curl -X GET -H "X-Auth-Token:<var class="keyword varname">APIKEY</var>" -H "Accept: application/vnd.kafka.binary.v1+json" <var class="keyword varname">kafka-rest endpoint</var>/topics/<var class="keyword varname">topic name</var>/partitions/<var class="keyword varname">partition ID</var>/messages?offset=<var class="keyword varname">offset to start from</var>
</code></pre>
{: codeblock}

<br/>
<br/>
Para o CURL, também é possível adaptar os exemplos de código detalhados nos [Confluent docs ![Ícone de linkexterno](../../icons/launch-glyph.svg "Ícone de link externo")](http://docs.confluent.io/2.0.0/){:new_window} incluindo a linha a seguir na linha de comandos:

<pre class="pre">-H "X-Auth-Token: <var class="keyword varname">APIKEY</var>"</pre>
{: codeblock}


<!-- Comment from Andrew
basic introduction, definitely including health warning
-->

