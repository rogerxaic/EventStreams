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

# Über Bridges mit anderen Services verbinden
{: #bridges}

** {{site.data.keyword.messagehub}}-Bridges sind nur als Bestandteil des Plans "Standard" verfügbar.**
<br/>

Der {{site.data.keyword.messagehub}}-Plan "Standard" unterstützt außerdem
Bridges für mehrere andere Systeme. Bridges sind unidirektionale Verbindungen zwischen {{site.data.keyword.messagehub}} und anderen Services. Sie ermöglichen
das Lesen von Daten aus {{site.data.keyword.messagehub}} und das Schreiben der Daten in andere
Services oder das Lesen von Daten aus anderen Services und das Schreiben der Daten in {{site.data.keyword.messagehub}}. Eine Bridge kann Nachrichten von dem anderen System entgegennehmen und in einem Topic veröffentlichen,
oder Nachrichten von einem Topic verarbeiten und an das andere System senden. Auf diese Weise können Sie
{{site.data.keyword.messagehub}} für die Integration mit anderen Systemen verwenden, ohne Code zu
schreiben.
{:shortdesc}

Die Verwendung von Bridges in {{site.data.keyword.messagehub}} bietet einige wichtige Vorteile:  

* Bridges können administrativ definiert werden, ohne Anwendungscode zu schreiben.
* Der Lebenszyklus jeder Bridge wird vom {{site.data.keyword.messagehub}}-Service überwacht und verwaltet. Wenn in einer Bridge beispielsweise ein Fehler auftritt, wird sie von {{site.data.keyword.messagehub}} automatisch neu gestartet.
* Bridges werden in die {{site.data.keyword.Bluemix_short}}-Plattform integriert. Beispielsweise werden die von Bridges generierten Protokollierungs- und Überwachungsdaten an das Kibana- und an das Grafana-Dashboard weitergeleitet.

Bridges sind besonders in den beiden folgenden gängigen Anwendungsszenarios hilfreich:

* Daten aus mindestens einer Datenquelle in {{site.data.keyword.messagehub}} übernehmen
* Daten von {{site.data.keyword.messagehub}} in einen anderen Service übertragen (z. B. in einen Langzeitspeicher)

## {{site.data.keyword.messagehub}}-Bridges
{: notoc}

* Die folgenden Bridge-Typen werden bereitgestellt: 
  - [MQ bridge](/docs/services/EventStreams/eventstreams105.html){:new_window} zum Übertragen von Nachrichtendaten aus {{site.data.keyword.IBM}} MQ in ein Topic in {{site.data.keyword.messagehub}}. Künftig sollen noch weitere Bridge-Typen unterstützt werden.
  - [Cloud Object Storage-Bridge](/docs/services/EventStreams/eventstreams115.html){:new_window} zum Übertragen von {{site.data.keyword.messagehub}}-Daten in eine Instanz des [{{site.data.keyword.IBM_notm}} Cloud Object Storage-Service ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/services/cloud-object-storage/about-cos.html){:new_window}. 
    
    Der Cloud Object Storage-Service ist jetzt der bevorzugte Objektspeicherservice in {{site.data.keyword.Bluemix_short}}. 
  - [{{site.data.keyword.objectstorageshort}}-Bridge](/docs/services/EventStreams/eventstreams089.html){:new_window} zum Übertragen von {{site.data.keyword.messagehub}}-Daten in eine Instanz des [Object Storage-Service ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/services/ObjectStorage/index.html){:new_window}.
* Gegenwärtig sind Bridges in allen {{site.data.keyword.Bluemix_notm}} Public-Umgebungen verfügbar. In {{site.data.keyword.Bluemix_short}} Dedicated stehen keine Bridges zur Verfügung.
* Zum Verwalten von Bridges können die beiden folgenden Methoden verwendet werden:
  - Eine [REST-API ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-messaging/event-streams-docs){:new_window}, d. h. eine Erweiterung der vorhandenen {{site.data.keyword.messagehub}}-Verwaltungs-API. Beispiele für die Verwendung von 'curl' bei der Verwaltung des Lebenszyklus von Bridges finden Sie unter [message-hub-docs ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-messaging/event-streams-docs){:new_window}. Bei der Weiterentwicklung von Bridges sind Änderungen an dieser REST-API möglich. Es ist vorgesehen, diese API noch stabiler zu gestalten.
  - Das {{site.data.keyword.messagehub}}-Dashboard in der {{site.data.keyword.Bluemix_notm}}-Konsole.
* Sie können maximal zwei Bridges eines beliebigen Typs einer Instanz des {{site.data.keyword.messagehub}}-Service zuordnen. Bei der Weiterentwicklung von Bridges wird auch diese Begrenzung immer wieder überprüft.
* Über die zugehörigen Messaging-Operationen hinaus fallen keine weiteren Gebühren für die Verwendung von Bridges an.
* Die MQ-Bridge unterstützt nicht die Verwendung von SSL/TLS für den Schutz und die Integrität bei der Datenübertragung zwischen der Bridge und dem MQ-Warteschlangenmanager. Es ist vorgesehen, Unterstützung für die Verwendung von SSL/TLS in der Bridge hinzuzufügen. 
* Sie können den [{{site.data.keyword.SecureGatewayfull}}-Service ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/services/SecureGateway/secure_gateway.html){:new_window} jedoch verwenden, um Ihre Daten
durch einen sicheren Tunnel zwischen {{site.data.keyword.Bluemix_notm}}
und einem {{site.data.keyword.SecureGateway}}-Client zu senden, den Sie lokal installieren
können. In dieser Konfiguration wird die Datenübertragung an beiden Enden des Tunnels nicht durch
SSL/TLS geschützt.
* Die Cloud Object Storage- und {{site.data.keyword.objectstorageshort}}-Bridges verknüpfen Nachrichten mit Zeilenvorschubzeichen als Trennzeichen, während sie die Daten in Cloud Object Storage bzw. {{site.data.keyword.objectstorageshort}} schreiben. Dadurch sind diese Bridges nicht für Nachrichten geeignet, die eingebettete Zeilenvorschubzeichen oder binäre Nachrichtendaten enthalten.
* Die gegenwärtig von den Cloud Object Storage- und {{site.data.keyword.objectstorageshort}}-Bridges verwendeten Namenskonventionen für Objekte, werden möglicherweise noch geändert.

## Bridges von anderen Services zu {{site.data.keyword.messagehub}}
{: notoc}

* {{site.data.keyword.iot_full}} stellt eine eigene [-Bridge zu {{site.data.keyword.messagehub}}](/docs/services/EventStreams/eventstreams119.html){:new_window} bereit. Die Bridge stellt eine unidirektionale Verknüpfung zu {{site.data.keyword.messagehub}} bereit, mit der Sie historische Daten speichern können. Wenn Sie {{site.data.keyword.messagehub}} mit {{site.data.keyword.iot_short_notm}} verbinden, können Sie {{site.data.keyword.messagehub}} als Ereignispipeline verwenden, um Ihre Einheitenereignisse von Watson IoT Platform zu verarbeiten und der restlichen Plattform die Ereignisse in Echtzeit verfügbar zu machen. 


