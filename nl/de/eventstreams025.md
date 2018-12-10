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

# Kafka-REST-API verwenden
{: #rest_using}

** Die Kafka-REST-API ist nur als Bestandteil des Plans "Standard" verfügbar.**
<br/>

Die Kafka-REST-API stellt eine REST-konforme Schnittstelle zu einem Kafka-Cluster bereit. Mithilfe der API können Sie Nachrichten erstellen und
verarbeiten. Weitere Informationen einschließlich der API-Referenzdokumentation finden Sie in den [Kafka-REST-Proxy-Dokumenten ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.confluent.io/2.0.0/kafka-rest/docs/index.html){:new_window}. Nur das eingebettete Binärformat wird für Anforderungen und Antworten in {{site.data.keyword.messagehub}} unterstützt. Die eingebetteten Avro- und JSON-Formate werden nicht unterstützt.

Wenn Sie mit CURL arbeiten, können Sie für die Erstellung ein Beispiel wie das folgende verwenden:
<pre class="pre"><code>
curl -X POST -H "X-Auth-Token:<var class="keyword varname">API-Schlüssel</var>" -H "Content-Type: application/vnd.kafka.binary.v1+json" <var class="keyword varname">Kafka-REST-Endpunkt</var>/topics/<var class="keyword varname">Topicname</var> -d 

'
{
  "records": [
    {
      "value": "<var class="keyword varname">Base 64-codierte Wertzeichenfolge</var>"
    }
  ]
}
'
</code></pre>
{: codeblock}
<br/>
<br/>
Wenn Sie mit CURL arbeiten, können Sie für die Verarbeitung ein Beispiel wie das folgende verwenden:
<pre class="pre"><code>
curl -X GET -H "X-Auth-Token:<var class="keyword varname">API-Schlüssel</var>" -H "Accept: application/vnd.kafka.binary.v1+json" <var class="keyword varname">Kafka-REST-Endpunkt</var>/topics/<var class="keyword varname">Topicname</var>/partitions/<var class="keyword varname">Partitions-ID</var>/messages?offset=<var class="keyword varname">Startoffset</var>
</code></pre>
{: codeblock}

<br/>
<br/>
Für CURL können Sie auch die Codebeispiele anpassen, die in den
[Confluent-Dokumenten![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://docs.confluent.io/2.0.0/){:new_window} beschrieben sind, indem Sie die folgende Zeile zur Befehlszeile hinzufügen:
<pre class="pre">-H "X-Auth-Token: <var class="keyword varname">API-Schlüssel</var>"</pre>
{: codeblock}

<br/>
## Vorgehensweise zum Verbinden und Authentifizieren
{: #rest_connect}

<!-- info was in eventstreams066.md -->

<!-- Comment from Andrew
basic introduction, definitely including health warning
-->
Um eine Verbindung zu {{site.data.keyword.messagehub}} herzustellen, verwendet die Kafka-REST-API die Berechtigungsnachweise <code>api_key</code> und <code>kafka_rest_url</code>
aus [Umgebungsvariable VCAP_SERVICES](/docs/services/EventStreams/eventstreams127.html).

Um sich bei der Kafka-REST-API von {{site.data.keyword.messagehub}} zu authentifizieren, müssen Sie die Berechtigung <code>api_key</code> im X-Auth-Token-Header Ihrer Anforderungen angeben.


## API verwenden
{: #rest_how}

<!-- info was in eventstreams097.md -->

Das {{site.data.keyword.messagehub}}-Kafka-REST-API-Beispiel ist eine Node.js-Anwendung, die eine Verbindung zu {{site.data.keyword.messagehub}} über die Kafka-REST-API herstellt, um Nachrichten zu produzieren und zu verarbeiten.

Der Beispielcode befindet sich im [Github-Projekt 'event-streams-samples' ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample){:new_window}.

Befolgen Sie die Anweisungen in der Datei [README.md ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample){:new_window} für dieses Projekt, um das Beispiel zu erstellen und auszuführen.


