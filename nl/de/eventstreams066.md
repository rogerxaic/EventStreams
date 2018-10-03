---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Vorgehensweise zum Verbinden und Authentifizieren
{: #rest_connect}

** Die Kafka-REST-API ist nur als Bestandteil des Plans "Standard" verfügbar.**
<br/>

Um eine Verbindung zu {{site.data.keyword.messagehub}} herzustellen, verwendet die Kafka-REST-API die Berechtigungsnachweise <code>api_key</code> und <code>kafka_rest_url</code>
aus [Umgebungsvariable VCAP_SERVICES](/docs/services/EventStreams/eventstreams127.html).

Um sich bei der Kafka-REST-API von {{site.data.keyword.messagehub}} zu authentifizieren, müssen Sie die Berechtigung <code>api_key</code> im X-Auth-Token-Header Ihrer Anforderungen angeben.
