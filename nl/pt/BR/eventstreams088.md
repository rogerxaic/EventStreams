---

copyright:
  years: 2015, 2018
lastupdated: "2018-06-01"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Vinculando-se a outros serviços usando pontes
{: #bridges}

**As pontes do {{site.data.keyword.messagehub}} estão disponíveis como parte somente do plano Standard.**
<br/>

O plano Standard do {{site.data.keyword.messagehub}} também suporta pontes para uma seleção de outros
sistemas. Pontes são links unidirecionais entre o {{site.data.keyword.messagehub}} e outro serviço. As pontes permitem que os dados sejam lidos do {{site.data.keyword.messagehub}} e gravados em outro serviço ou que sejam lidos de outro serviço e gravados no {{site.data.keyword.messagehub}}. Uma ponte pode tomar mensagens do outro sistema e
publicá-las em um tópico ou consumir mensagens de um tópico e enviá-las para o outro sistema. Dessa maneira, é
possível usar o {{site.data.keyword.messagehub}} para integração com outros sistemas sem gravar
código.
{:shortdesc}

Os principais benefícios do uso das pontes do {{site.data.keyword.messagehub}} são os seguintes:  

* É possível definir as pontes administrativamente, de modo que não é necessário gravar código do aplicativo.
* O ciclo de vida de cada ponte é monitorado e gerenciado pelo serviço do {{site.data.keyword.messagehub}}. Por exemplo, se uma ponte encontra um erro, ela é reiniciada automaticamente pelo {{site.data.keyword.messagehub}}.
* As pontes são integradas com a plataforma do {{site.data.keyword.Bluemix_short}}. Por exemplo, as informações de criação de log e de monitoramento geradas pelas pontes são direcionadas aos painéis do Kibana e Grafana.

Talvez você ache as pontes úteis nos dois cenários comuns a seguir:

* Consumindo dados de uma ou mais origens de dados para o {{site.data.keyword.messagehub}}.
* Transferindo dados do {{site.data.keyword.messagehub}} para outro serviço. Por exemplo, para armazenamento de longo prazo.

## Pontes de {{site.data.keyword.messagehub}}
{: notoc}

* Fornecemos os seguintes tipos de ponte: 
  - [Ponte do MQ](/docs/services/EventStreams/eventstreams105.html){:new_window}, que seleciona dados da mensagem do {{site.data.keyword.IBM}} MQ e os transfere para um tópico no {{site.data.keyword.messagehub}}. Pretendemos oferecer suporte para uma ampla variedade de pontes futuramente.
  - [Ponte do Cloud Object Storage](/docs/services/EventStreams/eventstreams115.html){:new_window}, que transfere dados do {{site.data.keyword.messagehub}} para uma instância do [{{site.data.keyword.IBM_notm}} Cloud Object Storage ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/cloud-object-storage/about-cos.html){:new_window} de serviço. 
    
    O serviço do Cloud Object Storage é o serviço de armazenamento de objeto preferencial no {{site.data.keyword.Bluemix_short}}. 
  - [Ponte do {{site.data.keyword.objectstorageshort}}](/docs/services/EventStreams/eventstreams089.html){:new_window}, que transfere dados do {{site.data.keyword.messagehub}} para uma instância do [serviço Object Storage ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/ObjectStorage/index.html){:new_window}.
* Atualmente, as pontes estão disponíveis em todos os ambientes públicos do {{site.data.keyword.Bluemix_notm}}. As pontes não estão disponíveis no {{site.data.keyword.Bluemix_short}} Dedicated.
* É possível administrar as pontes nos dois modos a seguir:
  - Usando uma [API de REST ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/ibm-messaging/event-streams-docs){:new_window}, que é uma extensão para a API de administração do {{site.data.keyword.messagehub}}. Também é possível localizar exemplos de como usar o curl para gerenciar o ciclo de vida de pontes em [message-hub-docs ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/ibm-messaging/event-streams-docs){:new_window}. Podemos mudar essa API de REST conforme continuamos a desenvolver pontes. Pretendemos estabilizar esta API.
  - Usando o painel do {{site.data.keyword.messagehub}} no console do {{site.data.keyword.Bluemix_notm}}.
* É possível associar um máximo de duas pontes de qualquer tipo a uma instância do serviço do {{site.data.keyword.messagehub}}. Continuaremos a revisar esta limitação conforme continuamos a desenvolver pontes.
* Não há encargos adicionais pelo uso de pontes além das operações de sistema de mensagens.
* A ponte do MQ não suporta o uso de SSL/TLS para proteger a privacidade e a integridade dos dados enquanto eles são transferidos entre a ponte e o gerenciador de filas do MQ. Pretendemos incluir suporte para o uso de SSL/TLS para a ponte. 
* No entanto, é possível usar o serviço do [{{site.data.keyword.SecureGatewayfull}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/SecureGateway/secure_gateway.html){:new_window} para enviar seus dados por meio de um túnel seguro entre o {{site.data.keyword.Bluemix_notm}} e um cliente do {{site.data.keyword.SecureGateway}} que pode ser instalado no local. Nessa configuração, a comunicação em qualquer extremidade do túnel não é protegida usando SSL/TLS.
* As pontes do Cloud Object Storage e do {{site.data.keyword.objectstorageshort}} concatenam mensagens usando caracteres de nova linha como separadores à medida que eles gravam os dados no Cloud Object Storage ou no {{site.data.keyword.objectstorageshort}} respectivamente. Isso torna essas pontes inadequadas para mensagens que contêm caracteres de nova linha integrados e para dados da mensagem binários.
* As convenções de nomenclatura de objeto que são usadas atualmente pelas pontes do Cloud Object Storage e do {{site.data.keyword.objectstorageshort}} podem mudar futuramente.

## Pontes de outros serviços no {{site.data.keyword.messagehub}}
{: notoc}

* O {{site.data.keyword.iot_full}} fornece sua própria ponte do [ no {{site.data.keyword.messagehub}}](/docs/services/EventStreams/eventstreams119.html){:new_window}. A ponte fornece um link unidirecional para o {{site.data.keyword.messagehub}} que permite armazenamento de dados históricos. Ao conectar o {{site.data.keyword.messagehub}} ao {{site.data.keyword.iot_short_notm}}, é possível usar o {{site.data.keyword.messagehub}} como um pipeline do evento para consumir seus eventos de dispositivo por meio do Watson IoT Platform e disponibilizar os eventos em tempo real para o restante da plataforma. 


