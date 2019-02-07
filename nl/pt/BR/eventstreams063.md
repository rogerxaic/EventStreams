---

copyright:
  years: 2015, 2019
lastupdated: "2018-06-22"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Configurando o cliente da API do Kafka
{: #kafka_connect}


Para se conectar ao {{site.data.keyword.messagehub}}, a API do Kafka usa um dos conjuntos de informações de credenciais
a seguir: 
* as credenciais <code>kafka_brokers_sasl</code>, além de <code>user</code> e <code>password</code> da [variável de ambiente VCAP_SERVICES](/docs/services/EventStreams/eventstreams127.html#vcap).
* a chave de serviço. Para obter mais informações, consulte
[Conectando-se ao seu cluster](/docs/services/EventStreams/eventstreams127.html#enterprise_connect).
{: shortdesc}

<!--17/10/17 - Karen: following info duplicated at messagehub104 -->
## Usando a propriedade sasl.jaas.config (conectando e autenticando em um aplicativo Java)
{: #kafka_java notoc}
Se estiver usando um cliente Kafka em 0.10.2.1 ou mais recente, será possível usar a
propriedade <code>sasl.jaas.config</code> para a configuração do cliente em vez de um arquivo do
JAAS. Para conectar-se ao {{site.data.keyword.messagehub}}, configure <code>sasl.jaas.config</code>
como segue:
<pre>
<code>    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required \
    username="USERNAME" \
    password="PASSWORD";</code>
</pre>
{:codeblock}

em que USERNAME e PASSWORD são os valores do {{site.data.keyword.messagehub}} na guia
**Credenciais de serviço** no {{site.data.keyword.Bluemix_notm}}.

Se usar <code>sasl.jaas.config</code>, os clientes em execução na mesma JVM poderão usar credenciais
diferentes. Para obter mais informações, consulte
[Configurando clientes do
Kafka ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://kafka.apache.org/documentation/#security_sasl_plain_clientconfig){:new_window}

Para um cliente Kafka anterior, deve-se usar um arquivo de configuração do JAAS para especificar as credenciais. Esse mecanismo é menos conveniente, portanto, recomendamos o uso da propriedade <code>sasl.jaas.config</code> como alternativa.
## Conectando e autenticando em um aplicativo diferente do Java
{: #kafka_notjava notoc}

O serviço {{site.data.keyword.messagehub}} atualmente autentica clientes usando
SASL PLAIN sobre TLS. As credenciais são
transportadas uma conexão criptografada.
Este é um novo recurso incluído no Kafka 0.10.0.X. 

O exemplo a seguir é um arquivo de configuração de amostra denominado <code>consumer.properties</code>:

```
key.deserializer=org.apache.kafka.common.serialization.StringDeserializer
value.deserializer=org.apache.kafka.common.serialization.StringDeserializer
#
client.id=kafka-java-console-sample-consumer
group.id=kafka-java-console-sample-group
#
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USERNAME" password="PASSWORD";
ssl.protocol=TLSv1.2
ssl.enabled.protocols=TLSv1.2
ssl.endpoint.identification.algorithm=HTTPS
#
# please read the Kafka docs about this setting
auto.offset.reset=latest
```
{: codeblock}

Qualquer cliente que suporta Kafka 0.10 com SASL PLAIN deve
trabalhar com {{site.data.keyword.messagehub}}. Os clientes de exemplo são os seguintes:

* [librdkafka ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/edenhill/librdkafka/){:new_window} 
* [confluent-kafka-python ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/confluentinc/confluent-kafka-python){:new_window} 

Se você estiver usando o cliente Kafka 0.9.0.0 mais antigo, será necessário usar um módulo de
login customizado, que pode ser transferido por download em[{{site.data.keyword.messagehub}} login module![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-0.9/message-hub-login-library/messagehub.login-1.0.0.jar){:new_window}. 

