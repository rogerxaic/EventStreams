---

copyright:
  years: 2015, 2019
lastupdated: "2018-05-25"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Utilisation de l'API REST Kafka dans le plan Classic 
{: #rest_using_classic}

** Cette version de l'API REST Kafka est uniquement disponible dans le plan Classic.**
<br/>

L'API REST Kafka fournit une interface RESTful à un cluster Kafka. Vous pouvez générer et consommer des messages en utilisant l'API. Pour plus d'informations, notamment sur la documentation de référence sur les API, voir la [documentation Kafka REST Proxy ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://docs.confluent.io/2.0.0/kafka-rest/docs/index.html){:new_window}. Seul le format binaire intégré est pris en charge pour les demandes et les réponses dans {{site.data.keyword.messagehub}}. Les formats Avro et JSON intégrés ne sont pas pris en charge.
{: shortdesc}

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
<br/>
<br/>
Si vous utilisez CURL, vous pouvez utiliser l'exemple suivant pour la consommation :
<pre class="pre"><code>
curl -X GET -H "X-Auth-Token:<var class="keyword varname">APIKEY</var>" -H "Accept: application/vnd.kafka.binary.v1+json" <var class="keyword varname">kafka-rest endpoint</var>/topics/<var class="keyword varname">topic name</var>/partitions/<var class="keyword varname">partition ID</var>/messages?offset=<var class="keyword varname">offset to start from</var>
</code></pre>
{: codeblock}

<br/>
<br/>
Pour CURL, vous pouvez également adapter les exemples de code détaillés
dans la [documentation Confluent ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://docs.confluent.io/2.0.0/){:new_window} en ajoutant la ligne suivante sur la ligne de commande :
<pre class="pre">-H "X-Auth-Token: <var class="keyword varname">APIKEY</var>"</pre>
{: codeblock}

<br/>
## Procédure de connexion et d'authentification
{: #rest_connect_classic}

<!-- info was in eventstreams066.md -->

Pour se connecter à {{site.data.keyword.messagehub}}, l'API Kafka utilise les données d'identification <code>api_key</code> et <code>kafka_rest_url</code> provenant de la [variable d'environnement VCAP_SERVICES](/docs/services/EventStreams?topic=eventstreams-connecting#connect_classic_cf).

Pour vous authentifier à l'API REST Kafka {{site.data.keyword.messagehub}}, vous devez spécifier l'attribut <code>api_key</code> dans l'en-tête X-Auth-Token de vos demandes.


## Utilisation de l'API
{: #rest_how_classic}

<!-- info was in eventstreams097.md -->

L'exemple d'API REST Kafka de {{site.data.keyword.messagehub}} est une application Node.js qui se connecte à {{site.data.keyword.messagehub}} par le biais de l'API REST Kafka pour produire et
consommer des messages.

L'exemple de code est disponible dans le [projet GitHub event-streams-samples ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample){:new_window}.

Suivez les instructions du fichier [README.md ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample){:new_window} de ce projet pour générer et exécuter l'exemple.

	Le blogue suivant présente les avantages et les inconvénients de l'API REST Kafka. [A Comprehensive, Open Source REST Proxy for Kafka. ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://www.confluent.io/blog/a-comprehensive-open-source-rest-proxy-for-kafka/){: new_window}.








