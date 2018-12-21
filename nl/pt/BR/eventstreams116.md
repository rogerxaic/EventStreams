---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-12"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Usando as ferramentas do console Kafka com o {{site.data.keyword.messagehub}}
{: #kafka_console_tools }

O Apache Kafka é fornecido com uma variedade de ferramentas do console para administração simples
e operações de sistemas de mensagens. É possível usar muitas delas com o {{site.data.keyword.messagehub}}, mas o {{site.data.keyword.messagehub}} não permite
conexão com seu cluster ZooKeeper. Como o Kafka se desenvolveu, muitas das ferramentas que anteriormente
exigiam conexão com o ZooKeeper não têm mais essa exigência.

É possível localizar essas ferramentas do console no diretório <code>bin</code> do download de seu Kafka. Por exemplo, [Cliente Apache Kafka 0.10.2.X ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.apache.org/dyn/closer.cgi?path=/kafka/0.10.2.1/kafka_2.11-0.10.2.1.tgz){:new_window}.

Para fornecer as credenciais SASL para essas ferramentas, crie um arquivo de propriedades com
base no exemplo a seguir:

<pre>
<code>
  sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule username necessário="USER" password="PASSWORD";
  security.protocol=SASL_SSL
  sasl.mechanism=PLAIN
  ssl.protocol=TLSv1.2
  ssl.enabled.protocols=TLSv1.2
  ssl.endpoint.identification.algorithm=HTTPS
</code>
</pre>
{:codeblock}

Substitua USER e PASSWORD pelos valores de sua guia **Credenciais de serviço** do {{site.data.keyword.messagehub}} no console do {{site.data.keyword.Bluemix_notm}}.


## Produtor de console
{: #console_producer }

É possível usar a ferramenta produtor de console do Kafka com o {{site.data.keyword.messagehub}}. Deve-se fornecer uma lista de brokers e credenciais SASL.

Depois de ter criado o arquivo de propriedades conforme descrito anteriormente, será possível
executar o produtor de console em um terminal, conforme a seguir:

<pre>
<code>
  $ kafka-console-producer.sh --broker-list KAFKA_BROKERS_SASL --producer.config CONFIG_FILE --topic TOPIC_NAME
</code>
</pre>
{:codeblock}

Substitua as variáveis a seguir no exemplo pelos seus próprios valores:
* KAFKA_BROKERS_SASL com o valor de sua guia {{site.data.keyword.messagehub}} **Credenciais de serviço** no console do {{site.data.keyword.Bluemix_notm}}, como uma lista de pares host:porta separados por vírgulas (por exemplo, `host1:port1,host2:port2`). 
* CONFIG_FILE pelo caminho do arquivo de configuração. 

Será possível usar muitas das outras opções dessa ferramenta, com a exceção daquelas que requerem acesso ao ZooKeeper.


## Consumidor de console
{: #console_consumer }

É possível usar a ferramenta consumidor de console do Kafka com o {{site.data.keyword.messagehub}}. Deve-se fornecer um servidor de autoinicialização e credenciais SASL.

Depois de ter criado o arquivo de propriedades conforme descrito anteriormente, será possível
executar o consumidor de console em um terminal, conforme a seguir:

<pre>
<code>
  $ kafka-console-consumer.sh --bootstrap-server KAFKA_BROKERS_SASL --consumer.config CONFIG_FILE --topic TOPIC_NAME
</code>
</pre>
{:codeblock}

Substitua as variáveis a seguir no exemplo pelos seus próprios valores:
* KAFKA_BROKERS_SASL com o valor de sua guia {{site.data.keyword.messagehub}} **Credenciais de serviço** no console do {{site.data.keyword.Bluemix_notm}}, como uma lista de pares host:porta separados por vírgulas (por exemplo, `host1:port1,host2:port2`). 
* CONFIG_FILE pelo caminho do arquivo de configuração. 

Será possível usar muitas das outras opções dessa ferramenta, com a exceção daquelas que requerem acesso ao ZooKeeper.


## Grupos de consumidores
{: #consumer_groups_tool }

É possível usar a ferramenta grupos de consumidores do Kafka com o {{site.data.keyword.messagehub}}. Como o {{site.data.keyword.messagehub}} não permite conexão com seu cluster ZooKeeper,
algumas das opções não estão disponíveis.

Depois de ter criado o arquivo de propriedades conforme descrito anteriormente, será possível
executar as ferramentas grupos de consumidores em um terminal. Por exemplo, é possível listar os grupos
de consumidores, conforme a seguir:

<pre>
<code>
  $ kafka-consumer-groups.sh --bootstrap-server KAFKA_BROKERS_SASL --command-config CONFIG_FILE --list
</code>
</pre>
{:codeblock}

Substitua as variáveis a seguir no exemplo pelos seus próprios valores:
* KAFKA_BROKERS_SASL com o valor de sua guia {{site.data.keyword.messagehub}} **Credenciais de serviço** no console do {{site.data.keyword.Bluemix_notm}}, como uma lista de pares host:porta separados por vírgulas (por exemplo, `host1:port1,host2:port2`). 
* CONFIG_FILE pelo caminho do arquivo de configuração.

Usando essa ferramenta, também é possível exibir detalhes como as posições atuais dos consumidores,
seu atraso ou o ID do cliente para cada partição para um grupo. Por exemplo:

<pre>
<code>
  $ kafka-consumer-groups.sh --bootstrap-server KAFKA_BROKERS_SASL --command-config CONFIG_FILE --describe --group GROUP
</code>
</pre>
{:codeblock}

No exemplo, substitua GROUP pelo nome do grupo para o qual você deseja recuperar detalhes. 


## Tópicos
{: #topics_tool }

Não é possível usar a ferramenta tópicos do Kafka `kafka-tópicos` com o {{site.data.keyword.messagehub}} porque a ferramenta requer acesso ao ZooKeeper.

No entanto, é possível administrar tópicos usando o painel do {{site.data.keyword.messagehub}} no console do {{site.data.keyword.Bluemix_notm}} ou usando a API de REST.


## Reconfiguração do Kafka Streams
{: #kafka_streams_reset }

É possível usar essa ferramenta com o {{site.data.keyword.messagehub}} para reconfigurar
o estado de processamento de um aplicativo Kafka Stream, dessa forma, será possível reprocessar sua entrada
do zero. Antes de executar essa ferramenta, assegure-se de que seu aplicativo Streams esteja totalmente
parado.

Por exemplo:

<pre>
<code>
  $ kafka-streams-application-reset.sh --bootstrap-servers KAFKA_BROKERS_SASL --config-file CONFIG_FILE --application-id APP_ID
</code>
</pre>
{:codeblock}

Substitua as variáveis a seguir no exemplo pelos seus próprios valores:
* KAFKA_BROKERS_SASL pelo valor de sua guia {{site.data.keyword.messagehub}} **Credenciais de serviço** no console do {{site.data.keyword.Bluemix_notm}},
como uma lista de pares host:porta separados com vírgulas (como `host1:port1,host2:port2`). 
* CONFIG_FILE pelo caminho do arquivo de configuração. 
* APP_ID por seu ID do aplicativo Streams.

