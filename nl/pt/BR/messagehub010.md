---

copyright:
  years: 2015, 2018
lastupdated: "2018-06-28"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# O que é o Message Hub?
{: #about}

O {{site.data.keyword.messagehub_full}} é um barramento de mensagem de alto rendimento desenvolvido com o Apache Kafka. 
Ele é otimizado para a ingestão de eventos no {{site.data.keyword.Bluemix_notm}} e para a distribuição de fluxo de eventos entre
os seus serviços e os seus aplicativos.

É possível usar o {{site.data.keyword.messagehub}} para concluir as tarefas a seguir:

* Transferir trabalho para processos do trabalhador de backend.
* Conectar dados em fluxo à análise para realizar insights poderosos.
* Publicar dados do evento em múltiplos aplicativos para reagir em tempo real.
* Transferir dados para outro serviço. Por exemplo, para armazenamento de longo prazo.

Por ser desenvolvido com o Apache Kafka, ele se beneficia diretamente de toda a inovação que ocorre na comunidade e suporta as
APIs do cliente Kafka, o Kafka Streams, o Kafka Connect e também o KSQL.
{:shortdesc}

As ferramentas do Apache Kafka geralmente trabalham diretamente com o {{site.data.keyword.messagehub}}, embora você precise fornecer configuração adicional porque as conexões com o {{site.data.keyword.messagehub}} sempre são autenticadas usando credenciais.

No {{site.data.keyword.messagehub}}, os aplicativos enviam dados criando uma mensagem e
enviando-a para um tópico. Para receber mensagens, os aplicativos assinam um tópico e optam por receber
todas as mensagens do tópico ou compartilhar as mensagens entre eles.
O {{site.data.keyword.messagehub}} hospeda e mantém as mensagens em uma sequência ordenada. 




