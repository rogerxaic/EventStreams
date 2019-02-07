---

copyright:
  years: 2015, 2019
lastupdated: "2018-12-21"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Was ist {{site.data.keyword.messagehub}}?
{: #about}

{{site.data.keyword.messagehub_full}} ist ein Nachrichtenbus mit hohem Durchsatz, der mit Apache Kafka erstellt wurde. Es handelt sich um vollständig verwaltetes Apache Kafka as a Service, das für die Aufnahme von Ereignissen in {{site.data.keyword.Bluemix_notm}} und und die Ereignisstromverteilung zwischen Ihren Services und Anwendungen optimiert ist. {{site.data.keyword.messagehub}} wurde früher als Message Hub bezeichnet.
{: shortdesc}

Mit {{site.data.keyword.messagehub}} können Sie die folgenden Tasks ausführen:

* Arbeit in Back-End-Verarbeitungsanwendungen auslagern.
* Event Streams mit Streaminganalysen verbinden, um aussagekräftige Erkenntnisse zu gewinnen.
* Ereignisdaten in mehreren Anwendungen veröffentlichen, um in Echtzeit reagieren zu können.
* Daten in einen anderen Service übertragen. Zum Beispiel in Cloud Object Storage.

Durch die Erstellung mit Apache Kafka werden in Message Hub alle Innovationen in der Community genutzt und es werden Kafka-Client-APIs, Kafka Streams, Kafka Connect und auch KSQL unterstützt.
{:shortdesc}

Apache Kafka-Tools arbeiten normalerweise direkt mit {{site.data.keyword.messagehub}}, es ist jedoch zusätzliche Konfiguration erforderlich, da bei Verbindungen zu {{site.data.keyword.messagehub}} stets Berechtigungsnachweise zur Authentifizierung verwendet werden.

In {{site.data.keyword.messagehub}} können Anwendungen Daten senden, indem eine Nachricht erstellt und an ein Topic gesendet wird. Zum Empfangen von Nachrichten abonnieren Anwendungen
ein Topic. Dabei kann ausgewählt werden, ob die Anwendungen alle Nachrichten für das abonnierte Topic empfangen oder jeweils nur freigegebene Nachrichten.
{{site.data.keyword.messagehub}} hostet und verwaltet die Nachrichten in einer geordneten Reihenfolge. 




