---

copyright:
  years: 2015, 2019
lastupdated: "2018-11-20"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# MQ Light-API verwenden 
{: #mql_using}

** Die MQ Light-API ist nur als Bestandteil des Plans "Standard" verfügbar.**
<br/>

Mit der {{site.data.keyword.mql}}-API steht Abwärtskompatibilität mit dem früheren {{site.data.keyword.mql}}-Service bereit. Die API bietet eine Messaging-Schnittstelle auf AMQP-Basis für Java&trade;, Node.js, Python und Ruby. 
{:shortdesc}

<!-- 02/07/18 - removing words to help deprecate MQ Light
In most cases, {{site.data.keyword.messagehub}} is best used with a Kafka client. The {{site.data.keyword.mql}} API is simple to learn but has very limited scalability and does not offer interoperability with other {{site.data.keyword.messagehub}} APIs.
The {{site.data.keyword.mql}} API is available in the following {{site.data.keyword.Bluemix_short}} regions only: US South, United Kingdom, and Sydney. The {{site.data.keyword.mql}} API not available in the Germany region or in {{site.data.keyword.Bluemix_notm}} Dedicated.
-->

## Merkmale der MQ Light-API
{: #mqlight}

<!-- 30/10/18: info was in eventstreams106.md, moved because of doc app changes -->

Die {{site.data.keyword.mql}}-API stellt eine höhere Abstraktionsebene für das Messaging bereit als Kafka.

Das Auswahlkriterium für die Verwendung eines Kafka-Clients oder der {{site.data.keyword.mql}}-API ist
die zu erstellende Messaging-Topologie:

* Mit Kafka erstellen Sie eine kleine Anzahl von Topics, die eine Vielzahl von Partitionen für jedes Topic enthalten können, um eine hohe Skalierbarkeit zu erzielen. Mithilfe von Consumergruppen können Consumer Nachrichten zwar gemeinsam nutzen, doch jeder Consumer muss darauf achten, dass die zugeordnete Nachrichtenrate auch tatsächlich verarbeitet werden kann.
* Mit der {{site.data.keyword.mql}}-API können Sie eine große Anzahl von Topics mit hierarchisch strukturierten Topicnamen verwenden (z. B. <code>&lsquo;/sports/football&rsquo;</code> und <code>&lsquo;/sports/tiddlywinks&rsquo;</code>).  

Die Topics in der {{site.data.keyword.mql}}-API unterscheiden sich von
den Kafka-Topics. In der {{site.data.keyword.mql}}-API wird ein einziges Kafka-Topic
mit der Bezeichnung "MQLight" verwendet. Alle über die {{site.data.keyword.mql}}-API gesendeten
und empfangenen Nachrichten durchlaufen dieses eine Kafka-Topic.

{{site.data.keyword.mql}} ist nur an den folgenden {{site.data.keyword.Bluemix_notm}}-Standorten (in den folgenden Regionen) verfügbar: Dallas (us-south), London (eu-gb) und Sydney (au-syd). Die MQ Light-API ist weder am Standor Frankfurt (eu-de) noch in
{{site.data.keyword.Bluemix_notm}} Dedicated verfügbar.

Weitere Informationen zur Auswahl zwischen den APIs finden Sie im Abschnitt [Geeignete API auswählen](/docs/services/EventStreams/eventstreams087.html).


## Voraussetzungen für die Verwendung der MQ Light-API mit {{site.data.keyword.messagehub}}
{: #mql_reqs}

<!-- 30/10/18: info was in eventstreams098.md, moved because of doc app changes -->

Die folgenden Voraussetzungen sind für die Verwendung der {{site.data.keyword.mql}}-API mit {{site.data.keyword.messagehub}} erforderlich: 

**Sie müssen explizit ein Kafka-Topic mit dem Namen "MQLight" erstellen, damit die API verwendet werden kann, da alle Nachrichten das Topic "MQLight" durchlaufen. Dieses Topic muss eine einzige Partition enthalten. Durch das Erstellen dieses Topics wird die MQ Light-API für Ihre Serviceinstanz aktiviert. Die in der MQ Light-API verwendeten Topics werden automatisch erstellt, sobald Sie sie verwenden. Alle Nachrichten befinden sich jedoch tatsächlich in dem einzelnen Kafka-Topic "MQLight".** 

Das Topic "MQLight" wird von der MQ Light-API für die Speicherung der zugehörigen Nachrichtendaten und für die Interaktion mit anderen Kafka-Clients verwendet. Beachten Sie, dass ab der Erstellung dieses Topics
Gebühren gemäß dem im Zahlungsplan für Services angegebenen Standardsatz fällig werden.

Um die MQ Light-API zu inaktivieren, löschen Sie das Topic "MQLight". Beim Löschen des Topics werden auch alle zugehörigen Daten gelöscht.

<!-- 12/11/18: following info was in eventstreams079.md, moved because of doc app changes -->

## Vorgehensweise zum Verbinden und Authentifizieren
{: #mql_connect}

Zum Verbinden einer App mit dem Service muss die App den Benutzer <code>user</code>,
das Kennwort <code>password</code> und die Details für <code>mqlight_lookup_url</code> aus
[Umgebungsvariable VCAP_SERVICES](/docs/services/EventStreams/eventstreams127.html) verwenden. Verwenden Sie die nachfolgende Anleitung für Ihre gewünschte Sprache:

**Für Java**

Wenn Sie &lsquo;null&rsquo; als Parameter 'endpointService' im Aufruf 'create()' angeben, wird der Client angewiesen,
die Details für <code>user</code>, <code>password</code> und
<code>mqlight_lookup_url</code> aus VCAP_SERVICES abzurufen:

<pre>
<code>NonBlockingClient.create(null, new NonBlockingClientAdapter<Void>() {
    public void onStarted(NonBlockingClient client, Void context) {
                client.send("my/topic", "Hello World!", null);
    }
}, null);</code>
</pre>
{:codeblock}

<br>

**Für Node.js**

Rufen Sie die Details für <code>user</code>, <code>password</code> und
<code>mqlight_lookup_url</code> aus VCAP_SERVICES ab und verwenden Sie diese Details
wie nachfolgend angegeben, um den Client zu erstellen:

<pre>
<code>var services = JSON.parse(process.env.VCAP_SERVICES);
mqlightService = services['messagehub'][0];
opts.service = mqlightService.credentials.mqlight_lookup_url;
opts.user = mqlightService.credentials.user;
opts.password = mqlightService.credentials.password;
var mqlightClient = mqlight.createClient(opts, function(err) {
...</code>
</pre>
{:codeblock}

<br>

**Für Ruby**

Rufen Sie die Details für <code>user</code>, <code>password</code> und
<code>mqlight_lookup_url</code> aus VCAP_SERVICES ab und verwenden Sie diese Details
wie nachfolgend angegeben, um den Client zu erstellen:
<pre>
<code>vcap_services = JSON.parse(ENV['VCAP_SERVICES'])
conn_details = vcap_services['messagehub']
credentials = conn_details.first['credentials']
service = credentials['mqlight_lookup_url']
opts[:user] = credentials['user']
opts[:password] = credentials['password']
set :client, Mqlight::BlockingClient.new(service, opts)
...</code>
</pre>
{:codeblock}

<br>

**Für Python**

Rufen Sie die Details für <code>user</code>, <code>password</code> und
<code>mqlight_lookup_url</code> aus VCAP_SERVICES ab und verwenden Sie diese Details
wie nachfolgend angegeben, um den Client zu erstellen:
<pre>
<code>vcap_services = json.loads(os.environ.get('VCAP_SERVICES'))
conn_details = vcap_services['messagehub'][0]
service = str(conn_details['credentials']['mqlight_lookup_url'])
security_options = {
      'user': str(conn_details['credentials']['user']),
      'password': str(conn_details['credentials']['password'])
}
client = mqlight.Client(service=service, 
                        security_options=security_options,
                        on_started=on_started)</code>
</pre>
{:codeblock}

<br>

Weitere Informationen zu den {{site.data.keyword.mql}}-APIs finden Sie
auf der Website für [{{site.data.keyword.mql}} developerWorks&reg; ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/messaging/mq-light/){:new_window}.


<!-- 14/11/18: info was in eventstreams080.md, moved because of doc app changes -->

## Mit vorhandenen MQ Light-Anwendungen verbinden
{: #mql_exist_apps}


Sie können vorhandene Anwendungen mit dem Service verbinden, die entweder für {{site.data.keyword.IBM_notm}} MQ oder
für den {{site.data.keyword.mql}}-{{site.data.keyword.Bluemix_notm}}-Service ausgeführt werden. Die Apps
funktionieren weiter in der gewohnten Weise.

Führen Sie die folgenden Prüfungen durch, um vorhandene Apps zu verbinden:

* Stellen Sie sicher, dass die App die neueste verfügbare {{site.data.keyword.mql}}-API-Clientversion für Ihre Sprache verwendet.
* Stellen Sie sicher, dass die aus der Umgebungsvariablen VCAP_SERVICES extrahierten Verbindungsdetails auf den Servicetyp <code>messagehub</code> verweisen und den Benutzernamen der Verbindung aus der Eigenschaft <code>credentials.user</code> abrufen und nicht aus der Eigenschaft <code>credentials.username</code>, sowie die Such-URL der Verbindung aus der Eigenschaft <code>credentials.mqlight_lookup_url</code> und nicht aus der Eigenschaft <code>credentials.connectionLookupURI</code>. Weitere Informationen finden Sie in [Umgebungsvariable VCAP_SERVICES](/docs/services/EventStreams/eventstreams127.html).

	Beachten Sie, dass dieser Schritt automatisch ausgeführt wird, wenn Sie den Java&trade;-Client verwenden und 'null' für den Parameter 'endpointService' im Aufruf 'create()' angeben, damit der Client die Informationen selbstständig abruft.
	
* Ihre Anwendung muss Verbindungen über TLS Version 1.2 unterstützen. Die Umgebungsvariable VCAP_SERVICES enthält nicht mehr die Eigenschaft <code>credentials.nonTLSConnectionLookupURI</code> zum Erstellen einer Nicht-TLS-Verbindung.

Beachten Sie außerdem die folgenden Hinweise:

* Die Grenzwerte für Nachrichten sind mit {{site.data.keyword.messagehub}} konsistent, können jedoch von anderen Servern abweichen, die
die {{site.data.keyword.mql}}-API unterstützen. Weitere Informationen finden Sie in [Maximalwerte](/docs/services/EventStreams/eventstreams075.html#max_limits).
* JMS wird nicht unterstützt.

<!-- 15/11/18: info was in eventstreams081.md, moved because of doc app changes -->

## Nachrichten zwischen MQ Light-API und Kafka- oder Kafka-REST-APIs austauschen
{: #mql_exchange}

{{site.data.keyword.mql}}-Nachrichten werden in einem einzigen Kafka-Basistopic mit dem Namen "MQLight" gespeichert und codiert, wie in der folgenden Tabelle angegeben. Diese Codierung kann auch von anderen API-Typen (z. B. Kafka- oder Kafka-REST-APIs) verwendet werden, um Nachrichten mit Anwendungen über die
{{site.data.keyword.mql}}-API auszutauschen.

### Kafka-Nachrichtenformat
{: #kafka_format notoc}

<table border='1'>
<caption>Tabelle 1. Kafka-Nachrichtenformat</caption>
  <tr>
    <th> Schlüssel </th>
    <th> Wert </th>
  </tr>
  <tr>
    <td> Optional (von der API nicht verwendet)
	<p></p>
	</td>
    <td>**1 Byte**
	<p>		     Die Strukturkennung der MQ Light-API ist stets 0xFA.</p>
    <p><var class="keyword varname">**n**</var> **Byte**</p>
    <p>		    In AMQP codierte Nachricht (formatiert im AMQP-Sendeformat). </p></td>
  </tr>
</table>


<!-- 15/11/18: info was in eventstreams082.md, moved because of doc app changes -->

## MQ Light-API - Beispiele
{: #mql_samples}

Wenn Sie noch nicht über eine App verfügen, versuchen Sie es mit einer der {{site.data.keyword.mql}}-API-Beispielapps.

Die Beispielapp beinhaltet zwei einfache Apps: ein Web-Front-End, das Nachrichten an ein
Back-End sendet, und ein Back-End zum Verarbeiten der Nachrichten, das die Wörter in Großschreibung umwandelt
und die Nachrichten anschließend an das Front-End zurückgibt. Dieses Beispiel zeigt, wie Apps mithilfe der
{{site.data.keyword.mql}}-API miteinander kommunizieren können. Sie können die {{site.data.keyword.mql}}-API
auch zum Auslagern von Verarbeitungsprozessen verwenden. Das Auslagern ist eine Schlüsselfunktion zum Erstellen von
skalierbaren, flexibel verbundenen und verteilten Apps.

Den zugehörigen Beispielcode finden Sie im [GitHub-Projekt 'event-streams-samples' ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-messaging/event-streams-samples/tree/master/mqlight){:new_window}.


<!-- 15/11/18: info was in eventstreams083.md, moved because of doc app changes -->

## Maximalwerte
{: #max_limits}

Die folgenden Grenzwerte werden für die {{site.data.keyword.mql}}-API erzwungen:

* Das maximale Datenvolumen, das gespeichert werden kann, entspricht einer einzelnen Kafka-Partition in Ihrem Zahlungsplan (in der Regel 1 GB). Bei Überschreitung dieses Grenzwerts werden die ältesten Nachrichten in der Partition entfernt, wenn neue Nachrichten gesendet werden.
* Die maximale Speicherungsdauer für eine Nachricht entspricht einer einzelnen Kafka-Partition in Ihrem Zahlungsplan (in der Regel 24 Stunden). Sie können keine Nachrichten abrufen, die älter sind als diese Speicherungsdauer.
* Die maximale Größe für eine Nachricht (ohne Header) ist 1 MB.
* Die maximale Anzahl von Clients, die gleichzeitig verbunden sein können, ist 25.
* Die maximale Anzahl von Zielen, die gleichzeitig aktiv sein können, ist 25. Die Definition für ein aktives Ziel lautet wie folgt:
  - Ein Ziel mit einem Wert 'TimeToLive > 0', unabhängig davon, ob gegenwärtig ein Client verbunden ist oder nicht.
  - Ein Ziel mit einem Wert 'TimeToLive = 0' (Standardwert), wenn ein Client verbunden ist. 
  <p>Ein von Clients gemeinsam genutztes Ziel wird als einzelnes Ziel eingestuft.</p>
