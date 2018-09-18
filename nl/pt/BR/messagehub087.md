---

copyright:
  years: 2015, 2018
lastupdated: "2018-06-06"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Escolhendo entre as três APIs (plano Standard)
{: #choose_api}

O {{site.data.keyword.messagehub}} suporta três APIs no plano Standard. A seguir há algumas informações para ajudá-lo
a escolher o que é mais apropriado:

## Por que usar a API Kafka?
{: #why_kafka notoc}

** A API do Kafka está disponível como parte dos planos Standard e Enterprise. **
<br/>

Existem algumas razões pelas quais você pode escolher usar a API Kafka em vez
de outras interfaces fornecidas pelo {{site.data.keyword.messagehub}}. Algumas dessas razões são as seguintes:
{:shortdesc}


* É mais fácil integrar seu aplicativo com sistemas existentes que possuam o suporte para Kafka, por
exemplo, {{site.data.keyword.IBM}} {{site.data.keyword.streaminganalyticsshort}} e {{site.data.keyword.sparks}}.
* Ela oferece latência inferior e rendimento mais alto do que as outras APIs.
* Oferece uma API mais detalhada do que a API REST Kafka.

## Por que usar a API REST Kafka?
{: #why_rest notoc}

** A API de REST Kafka está disponível como parte somente do plano Standard.**
<br/>

A API REST do Kafka é uma interface conveniente que pode ser usada nas situações a
            seguir:  

* Onde um desenvolvedor deseja começar a usar o {{site.data.keyword.messagehub}}
* Em determinados casos de uso de baixo rendimento em que a latência não é um fator crítico
* Para depuração e descoberta de falhas

A API REST do Kafka não foi planejada como uma interface de alto rendimento e baixa latência.​Para esses tipos de requisitos, recomendamos o uso da API Kafka para conectar-se ao {{site.data.keyword.messagehub}} e por meio dele. Para obter informações adicionais, consulte [Usando um cliente Kafka](/docs/services/MessageHub/messagehub050.html#kafka_using).

## Por que usar a API {{site.data.keyword.mql}}?
{: #why_mql notoc}

** A API do MQ Light está disponível como parte somente do plano Standard.**
<br/>

A API do {{site.data.keyword.mql}} fornece uma interface de sistema de mensagens baseada em AMQP para Java™,
Node.js, Python e Ruby. A API é fornecida para compatibilidade com versões
anteriores com o serviço {{site.data.keyword.mql}} mais antigo.
















