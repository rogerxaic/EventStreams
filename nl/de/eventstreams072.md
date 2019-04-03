---

copyright:
  years: 2015, 2019
lastupdated: "2018-10-29"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Überwachung und Protokollierung (Plan "Standard")
{: #monitoring}

{{site.data.keyword.messagehub}} erfasst im Rahmen des Plans "Standard" automatisch Metriken und Ereignisse, damit Sie Ihre
Verwendung von {{site.data.keyword.messagehub}} überwachen können.
{:shortdesc}

**Hinweis:** Metriken sind nur im Rahmen des {{site.data.keyword.messagehub}}-Plans "Standard" an den Standorten Dallas (us-south), London (eu-gb) und Sydney (au-syd) verfügbar. 


Sie können die folgenden Protokolldaten überwachen:

<dl>
<dt>Topicmetriken</dt>
<dd>Die Anzahl von eingehenden und abgehenden Byte wird für jedes Ihrer Topics gesendet (jede 15 Minuten wird ein Prüfpunkt gesetzt). Auf diese Metriken
können Sie durch Klicken auf die Schaltfläche **Grafana** im {{site.data.keyword.messagehub}}-Dashboard in der {{site.data.keyword.Bluemix_notm}}-Konsole zugreifen.
</dd>
<dt>Topicereignisse</dt>
<dd>Jedes Mal, wenn Sie ein Topic erstellen oder löschen, werden Ereignisse mit Push-Operation übertragen. Auf diese Ereignisse können Sie
durch Klicken auf die Schaltfläche **Kibana** im {{site.data.keyword.messagehub}}-Dashoard in der {{site.data.keyword.Bluemix_notm}}-Konsole zugreifen.</dd>
</dl>


Es wird empfohlen, die {{site.data.keyword.messagehub}}-Dashboards nicht zu bearbeiten, da
in {{site.data.keyword.messagehub}} Aktualisierungen vorgenommen werden, die Ihre Änderungen
überschreiben können. Sie können diese Metriken und Ereignisse jedoch in Ihren eigenen Dashboards speichern.


