---

copyright:
  years: 2015, 2018
lastupdated: "2018-09-11"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}



# Fehler an das {{site.data.keyword.messagehub}}-Team melden - Plan "Standard"
{: #report_problem}

Wenn ein Problem mit {{site.data.keyword.messagehub}} auftritt, überprüfen Sie zuerst die Seite 'Status' in [{{site.data.keyword.Bluemix_notm}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/status){:new_window}. 

Wenn Sie Unterstützung vom {{site.data.keyword.messagehub}}-Team benötigen, sammeln Sie bitte die folgenden Informationen. Je mehr Informationen Sie zu Beginn angeben können, desto effizienter kann Sie das Team bei der Lösung des Problems unterstützen.
{:shortdesc}

1. In welcher {{site.data.keyword.Bluemix_notm}}-Region wird Ihre {{site.data.keyword.messagehub}}-Instanz bereitgestellt  (z. B. 'Vereinigte Staaten (Süden)' oder 'Vereintes Königreich')? 
2. In welcher Schnittstelle treten Probleme auf (Kafka, REST, AMQP oder Bridges)?
3. Wann ist das Problem zum ersten Mal aufgetreten (Datum, genaue Uhrzeit und Zeitzone)? Wie lange war Ihre App zu diesem Zeitpunkt bereits aktiv?
4. Tritt das Problem weiterhin auf? Können Sie das Problem erneut hervorrufen?
5. Wie lautet die Instanz-ID des von Ihnen verwendeten {{site.data.keyword.messagehub}}-Service? 
Sie finden diese ID im Feld "instance_id" in der JSON-Umgebungsvariablen VCAP_SERVICES. Beispiel:
 ```
 "instance_id": "e662a61b-d1ff-4cce-aab9-5eae9adb9827"
 ```
6. Welcher Kafka- oder REST-Client wird von Ihrer Anwendung verwendet? Wie lauten die zugehörigen Versionsdetails?
7. Wie lauten die Details Ihrer Clientkonfiguration?
8. Verfügen Sie über Ausschnitte des Anwendungsprotokolls, in denen das Problem angegeben ist?
9. Welches Problem haben Sie bemerkt? Welche Topics, Client-IDs und Gruppen-IDs sind betroffen?
10. Wie wirkt sich das Problem auf Ihren Service aus?


Sie können IBM die zusammengestellten Informationen in einem Support-Ticket zur Verfügung stellen, indem Sie [eine Support-Anforderung übergeben![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/get-support/howtogetsupport.html#open-ticket).










