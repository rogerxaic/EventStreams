---

copyright:
  years: 2015, 2019
lastupdated: "2018-02-06"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Usando o KSQL com o {{site.data.keyword.messagehub}}
{: #ksql_using}

É possível usar o [KSQL ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/confluentinc/ksql){:new_window}
com o {{site.data.keyword.messagehub}} para processamento de fluxo. Certifique-se de usar o KSQL 0.4 ou mais recente. 
{: shortdesc}

Execute as seguintes etapas:

1. Crie um arquivo de configuração  {{site.data.keyword.messagehub}}  KSQL.

    Por exemplo:
    ```
    bootstrap.servers=BOOTSTRAP_SERVERS
    application.id=ksql_server_quickstart
    ksql.command.topic.suffix=commands
    listeners=http://localhost:8080
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    ssl.protocol=TLSv1.2
    ssl.enabled.protocols=TLSv1.2
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USERNAME" password="PASSWORD";
    ksql.sink.replications.default=3
    ```
    em que BOOTSTRAP_SERVERS, USERNAME e PASSWORD são os valores de sua guia
{{site.data.keyword.messagehub}} **Credenciais de serviço** no
{{site.data.keyword.Bluemix_notm}}.

2. Use o painel {{site.data.keyword.messagehub}} no console do {{site.data.keyword.Bluemix_notm}} para criar um tópico chamado <code>ksql__commands</code>
com uma partição única e o período de retenção padrão.
3. Em um terminal do Docker, inicie o KSQL usando o comando a seguir:
<pre class="pre">/bin/ksql-cli local 
--<var class="keyword varname">properties-file</var> ./config/ksqlserver.properties
</pre>
4. Use o painel {{site.data.keyword.messagehub}} no console do {{site.data.keyword.Bluemix_notm}} para criar dois tópicos com uma partição cada:
<code>users</code> e <code>pageviews</code>.

    Em seguida, inicie o <code>DataGen</code> duas vezes, conforme a seguir:
	
    i. Com <code>bootstrap-server=HOSTNAME:PORTNUMBER quickstart=users format=json topic=users maxInterval=10000</code> para iniciar a criação de eventos <code>users</code>.
	
    ii. Com <code>bootstrap-server=HOSTNAME:PORTNUMBER quickstart=pageviews format=delimited topic=pageviews maxInterval=10000</code> para iniciar a criação de eventos <code>pageviews</code>.
	
	Assegure-se de inserir todos os hosts Kafka listados na página **Credenciais de serviço** como valores para <code>bootstrap-server</code>. Para localizar essas informações,
acesse sua instância do {{site.data.keyword.messagehub}} no {{site.data.keyword.Bluemix_notm}},
acesse a guia **Credenciais de serviço** e selecione as **Credenciais** que você deseja usar.

Quando tiver concluído essas etapas, será possível executar todas as consultas listadas no [KSQL Quick Start Guide ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/confluentinc/ksql/tree/0.1.x/docs/quickstart#create-a-stream-and-table){:new_window}

