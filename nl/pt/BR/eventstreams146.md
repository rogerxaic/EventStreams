---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-15"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Migrando um cliente Java Kafka no plano Clássico 
{: #kafka_java_migrating_classic}


## Migrando um cliente Java Kafka da 0.9.X ou 0.10.X para versões do cliente mais recentes
{: #kafka_migrate_classic}


Se você estiver usando os clientes Java, será possível usar os clientes Kafka publicamente disponíveis na 0.10 ou
mais recente. 

É altamente recomendado mudar da 0.9.X para a versão mais recente. É possível fazer download de um cliente Kafka em
[https://kafka.apache.org/downloads
![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://kafka.apache.org/downloads){:new_window}.

Para obter informações sobre as implicações de usar um cliente 0.9.X, consulte
[Compatibilidade com versões anteriores](/docs/services/EventStreams?topic=eventstreams-kafka_clients#compatibility).



### Migrando um cliente Java Kafka para a 0.10.2.X ou versões mais recentes

A partir da 0.10.2, é possível configurar a autenticação SASL diretamente nas propriedades do cliente em vez de usar um
arquivo JAAS. Essa simplificação permite executar múltiplos clientes na mesma JVM usando diferentes conjuntos de credenciais, o que
não é possível com um arquivo JAAS.

Execute as seguintes etapas:

1. Exclua o arquivo JAAS. Observe que a propriedade JVM java.security.auth.login.config=<PATH TO JAAS> também não é
mais necessária.
2. Se você estiver migrando da 0.9.X, exclua o módulo jar de login do {{site.data.keyword.messagehub}}.
2. Inclua o seguinte nas propriedades do cliente:
    ```
	sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USERNAME" password="PASSWORD";
	```

	em que USERNAME e PASSWORD são os valores do {{site.data.keyword.messagehub}} na guia
**Credenciais de serviço** no {{site.data.keyword.Bluemix_notm}}.
	
	

### Migrando um cliente Kafka da 0.9.X para a 0.10.0.X ou a 0.10.1.X

Execute as seguintes etapas:

1. Exclua o módulo jar de login do {{site.data.keyword.messagehub}}.
2. Mude seu arquivo <code>jaas.conf</code> para o seguinte:
    ```
        KafkaClient {
          org.apache.kafka.common.security.plain.PlainLoginModule required
          serviceName="kafka"
            username="USERNAME"
            password="PASSWORD";
        };
    ```
    {: codeblock}

	em que USERNAME e PASSWORD são os valores do {{site.data.keyword.messagehub}} na guia
**Credenciais de serviço** no {{site.data.keyword.Bluemix_notm}}.
	
3. Inclua esta linha em suas propriedades do consumidor e do produtor: <code>sasl.mechanism=PLAIN</code>
