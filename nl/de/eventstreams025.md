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


<!-- Comment from Andrew
basic introduction, definitely including health warning
-->

