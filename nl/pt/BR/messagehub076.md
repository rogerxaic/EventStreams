---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-30"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Por que usar a API {{site.data.keyword.mql}}?
{: #why_mql}

** A API do MQ Light está disponível como parte somente do plano Standard.**
<br/>

A API {{site.data.keyword.mql}} fornece um nível mais alto de abstração do que a API Kafka. O
{{site.data.keyword.mql}} permite que os aplicativos sejam gravados de forma portável em um modelo de sistema de mensagens unificado que suporta os padrões de sistema de mensagens de fila e de publicação/assinatura.
{:shortdesc}

Os aplicativos trocam mensagens usando destinos criados dinamicamente, os quais você pode estruturar hierarquicamente (por exemplo, <code>‘/sports/football’</code>), agrupar usando curingas (por exemplo,
<code>‘/sports/#’</code>) e possui controles para garantia de entrega e validade de mensagem.
Isso permite que você implemente cenários como transferência de trabalhador, notificação de eventos e processamento em lote.

Bem como enviar mensagens entre outros aplicativos usando a API {{site.data.keyword.mql}}, também é possível trocar mensagens com aplicativos que usam as APIs Kafka REST ou Kafka. Como alternativa, também é possível usar aplicativos no local com o {{site.data.keyword.IBM}} MQ.

