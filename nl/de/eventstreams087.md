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

# Geeignete API auswählen (Plan "Standard")
{: #choose_api}

{{site.data.keyword.messagehub}} unterstützt drei APIs beim Plan "Standard". Die nachfolgenden Informationen sollen Ihnen helfen, die geeignete API für Ihre Zwecke auszuwählen:

## Gründe für die Verwendung der Kafka-API
{: #why_kafka notoc}

** Die Kafka-API ist sowohl als Bestandteil des Plans "Standard" als auch des Plans "Enterprise" verfügbar.**
<br/>

Es gibt einige Gründe, warum Sie die Kafka-API den anderen von {{site.data.keyword.messagehub}} bereitgestellten Schnittstellen vorziehen könnten. Zu diesen Gründen zählen:
{:shortdesc}


* Ihre App kann einfacher in vorhandene Systeme integriert werden, für die Kafka-Unterstützung bereitgestellt wird (z. B. {{site.data.keyword.IBM}} {{site.data.keyword.streaminganalyticsshort}} und {{site.data.keyword.sparks}}).
* Sie bietet eine niedrigere Latenz und einen höheren Durchsatz als die anderen APIs.
* Sie bietet eine umfassendere API als die Kafka-REST-API.

## Gründe für die Verwendung der Kafka-REST-API
{: #why_rest notoc}

** Die Kafka-REST-API ist nur als Bestandteil des Plans "Standard" verfügbar.**
<br/>

Die Kafka-REST-API ist eine praktische Schnittstelle, die in den folgenden Situationen verwendet werden kann:  

* Wenn ein Entwickler mit der Verwendung von {{site.data.keyword.messagehub}} beginnen möchte.
* In bestimmten Anwendungsfällen mit niedrigem Durchsatz, bei denen die Latenz kein kritischer Faktor ist.
* Bei der Fehlerbehebung und Fehlersuche.

Die Kafka-REST-API ist nicht konzipiert als Schnittstelle für hohen Durchsatz und niedrige Latenz.Für solche Anforderungen wird empfohlen, die Kafka-API für die Verbindung zu und von {{site.data.keyword.messagehub}} zu verwenden. Weitere Informationen finden Sie in [Kafka-Client verwenden](/docs/services/EventStreams/eventstreams050.html#kafka_using).

## Gründe für die Verwendung der {{site.data.keyword.mql}}-API
{: #why_mql notoc}

** Die MQ Light-API ist nur als Bestandteil des Plans "Standard" verfügbar.**
<br/>

Die {{site.data.keyword.mql}}-API bietet eine Messaging-Schnittstelle auf AMQP-Basis für Java™, Node.js, Python und Ruby. Mit der API steht Abwärtskompatibilität mit dem früheren {{site.data.keyword.mql}}-Service bereit.
















