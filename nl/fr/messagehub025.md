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

# Utilisation de l'API REST Kafka
{: #rest_using}

**L'API REST Kafka est disponible uniquement dans le cadre du plan Standard.**
<br/>

L'API REST Kafka fournit une interface RESTful à un cluster Kafka. Vous pouvez générer et consommer des messages en utilisant
l'API. Pour accéder à des documents de référence, voir la [documentation Kafka REST Proxy ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://docs.confluent.io/2.0.0/kafka-rest/docs/index.html){:new_window}. Seul le format binaire intégré est pris en charge pour les demandes et les réponses dans {{site.data.keyword.messagehub}}. Les formats Avro et JSON intégrés ne sont pas pris en charge.

Avec CURL, vous pouvez utiliser un exemple tel que le suivant pour la génération :
<pre class="pre"><code>
curl -X POST -H "X-Auth-Token:<var class="keyword varname">APIKEY</var>" -H "Content-Type: application/vnd.kafka.binary.v1+json" <var class="keyword varname">kafka-rest endpoint</var>/topics/<var class="keyword varname">topic name</var> -d 

'
{
  "records": [
    {
      "value": "<var class="keyword varname">A base 64 encoded value string</var>"
    }
  ]
}
'
</code></pre>
{: codeblock}

Avec CURL, vous pouvez utiliser un exemple tel que le suivant pour la consommation :
<pre class="pre"><code>
curl -X GET -H "X-Auth-Token:<var class="keyword varname">APIKEY</var>" -H "Accept: application/vnd.kafka.binary.v1+json" <var class="keyword varname">kafka-rest endpoint</var>/topics/<var class="keyword varname">topic name</var>/partitions/<var class="keyword varname">partition ID</var>/messages?offset=<var class="keyword varname">offset to start from</var>
</code></pre>
{: codeblock}


Pour CURL, vous pouvez également adapter les exemples de code détaillés
dans la [documentation Confluent ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://docs.confluent.io/2.0.0/){:new_window} en ajoutant la ligne suivante sur la ligne de commande :
<pre class="pre">-H "X-Auth-Token: <var class="keyword varname">APIKEY</var>"</pre>
{: codeblock}


<!-- Comment from Andrew
basic introduction, definitely including health warning
-->

