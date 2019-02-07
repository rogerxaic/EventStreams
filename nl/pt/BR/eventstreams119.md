---

copyright:
  years: 2015, 2019
lastupdated: "2018-06-01"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Ponte do Watson IoT Platform
{: #consuming_messages }


O {{site.data.keyword.iot_full}} fornece uma ponte gerenciada que permite criar um link
unidirecional para o {{site.data.keyword.messagehub_full}}.
{: shortdesc}

A conexão do {{site.data.keyword.messagehub}} com o
{{site.data.keyword.iot_short_notm}}
significa que é possível usar o {{site.data.keyword.messagehub}} como um pipeline do evento para consumir
seus eventos de dispositivo do {{site.data.keyword.iot_short_notm}} e para tornar os eventos
disponíveis em tempo real para o restante da plataforma. 

Um padrão comum é usar a ponte do {{site.data.keyword.iot_short_notm}}, o
{{site.data.keyword.messagehub}} e a ponte do Cloud Object Storage para criar um
pipeline de ponta a ponta, facilitando as interações em tempo real e de lote.

Para obter informações sobre como criar esta ponte, consulte:
[Conectando e configurando um serviço historiador usando
o {{site.data.keyword.messagehub}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link
externo")](/docs/services/IoT/message_hub.html#messagehub_main){:new_window}.






