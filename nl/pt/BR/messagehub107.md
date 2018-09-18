---

copyright:
  years: 2015, 2018
lastupdated: "2017-09-26"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# O que é o Message Hub?

O {{site.data.keyword.messagehub}} implementa o sistema de mensagens de publicação/assinatura
usando tópicos. Diferentemente de outros sistemas de mensagens, o {{site.data.keyword.messagehub}}
fornece tópicos, mas não filas. O {{site.data.keyword.messagehub}} também oferece recursos
adicionais, como pontes para outros sistemas.

O {{site.data.keyword.messagehub}} é baseado no projeto Apache Kafka de software livre, um
backbone de sistema de mensagens altamente escalável e de alto desempenho comprovado em muitos ambientes de
produção. Para obter mais informações, consulte
[{{site.data.keyword.messagehub}} e Apache
Kafka](/docs/services/MessageHub/messagehub073.html).
As ferramentas do Apache Kafka geralmente trabalham diretamente com o {{site.data.keyword.messagehub}}, embora você precise fornecer configuração adicional porque as conexões com o {{site.data.keyword.messagehub}} sempre são autenticadas usando credenciais.

O {{site.data.keyword.messagehub}} tem três APIs: a API Kafka, a API de REST Kafka e a API {{site.data.keyword.mql}}.
Na maioria dos casos, a API Kafka é a melhor opção. Para obter mais informações sobre como criar
aplicativos de sistema de mensagens usando as APIs, veja [Usando um cliente Kafka](/docs/services/MessageHub/messagehub050.html), [Usando a API de REST
Kafka](/docs/services/MessageHub/messagehub025.html) e [Usando um cliente da API MQ Light](/docs/services/MessageHub/messagehub075.html).

O {{site.data.keyword.messagehub}} também suporta pontes para uma seleção de outros sistemas. Uma ponte é um link unidirecional para outro sistema. Uma ponte pode tomar mensagens do outro sistema e
publicá-las em um tópico ou consumir mensagens de um tópico e enviá-las para o outro sistema. Dessa maneira, é
possível usar o {{site.data.keyword.messagehub}} para integração com outros sistemas sem gravar
código. Para obter mais informações, consulte
[Vinculando-se a outros serviços usando pontes](/docs/services/MessageHub/messagehub088.html).
