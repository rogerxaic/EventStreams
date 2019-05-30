---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-04"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}

# Fehler an das {{site.data.keyword.messagehub}}-Team melden - Pläne "Standard" und "Enterprise"
{: #report_problem_enterprise}


Wenn ein Problem bei {{site.data.keyword.messagehub}} auftritt, überprüfen Sie zuerst die Seite 'Status' in [{{site.data.keyword.Bluemix_notm}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/status?selected=status){:new_window}.
{: shortdesc}

Wenn Sie Unterstützung vom {{site.data.keyword.messagehub}}-Team benötigen, sammeln Sie bitte die folgenden Informationen. Je mehr Informationen Sie zu Beginn angeben können, desto effizienter kann Sie das Team bei der Lösung des Problems unterstützen.
{:shortdesc}

1. Wie lautet die CRN-ID des von Ihnen verwendeten {{site.data.keyword.messagehub}}-Service?  Sie können diese ID angeben, indem Sie die vollständige URL der {{site.data.keyword.Bluemix_notm}}-Konsole nach dem Klicken auf den Service einfügen, oder indem Sie die Ausgabe des folgenden CLI-Befehls einfügen:<br/>
   <pre class="pre">
   ibmcloud resource service-instance NAME
   </pre>
	{: codeblock}
2. Wann ist das Problem zum ersten Mal aufgetreten (Datum, genaue Uhrzeit und Zeitzone)?
   Wie lange war Ihre App zu diesem Zeitpunkt bereits aktiv?
3. Tritt das Problem weiterhin auf? Können Sie das Problem erneut hervorrufen?
4. Welcher Kafka-Client wird von Ihrer Anwendung verwendet? Wie lauten die zugehörigen Versionsdetails?
5. Wie lauten die Details Ihrer Clientkonfiguration? Es werden Informationen zu den Producer- und Consumer-Einstellungen benötigt, bitte listen Sie daher alle bei der Producer- oder Consumer-Erstellung angegebenen Optionen auf, bei denen es sich nicht um die Standardoptionen handelt.
6. Verfügen Sie über Ausschnitte des Anwendungsprotokolls, in denen das Problem angegeben ist?
7. Welches Problem haben Sie bemerkt? Welche Topics, Client-IDs, Gruppen-IDs und
   Transaktions-IDs sind betroffen?
8. Wie wirkt sich das Problem auf Ihren Service aus?

Sie können IBM die zusammengestellten Informationen in einem Support-Ticket zur Verfügung stellen, indem Sie [eine Support-Anforderung übergeben![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/get-support?topic=get-support-getting-customer-support#using-avatar){:new_window}.










