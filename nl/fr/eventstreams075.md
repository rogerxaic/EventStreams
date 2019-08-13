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

# Utilisation de l'API MQ Light avec le plan Classic
{: #mql_using}

**L'API MQ Light est uniquement disponible dans le plan Classic.**
<br/>

L'API {{site.data.keyword.mql}} est fournie pour compatibilité avec les versions antérieures du service {{site.data.keyword.mql}}. Elle propose une interface de messagerie basée sur AMQP pour Java&trade;, Node.js, Python et Ruby. 
{:shortdesc}


## Présentation de l'API MQ Light et de ses particularités
{: #mqlight}

<!-- 30/10/18: info was in eventstreams106.md, moved because of doc app changes -->

L'API {{site.data.keyword.mql}} offre un niveau plus élevé d'abstraction à la messagerie que Kafka.

Le choix d'un client Kafka ou de l'API {{site.data.keyword.mql}} dépend de la topologie de messagerie que vous souhaitez construire :

* Avec Kafka, vous utilisez un petit nombre de sujets et vous pouvez disposer de plusieurs partitions pour chaque sujet afin d'accroître l'évolutivité. Vous pouvez partager des messages entre consommateurs grâce aux groupes de consommateurs, mais chaque consommateur doit pouvoir suivre le débit des messages pour les partitions qui lui sont affectés.
* Avec l'API {{site.data.keyword.mql}}, vous pouvez utiliser un nombre de sujets bien plus élevé et les noms de sujet sont hiérarchiques (par exemple : <code>&lsquo;/sports/football&rsquo;</code> et <code>&lsquo;/sports/tiddlywinks&rsquo;</code>). 

Les sujets de l'API {{site.data.keyword.mql}} ne sont pas les mêmes que les sujets Kafka. L'API {{site.data.keyword.mql}} utilise un seul sujet Kafka nommé "MQLight" et tous les messages envoyés et reçus via l'API {{site.data.keyword.mql}} transitent par ce sujet Kafka.

L'API {{site.data.keyword.mql}} est uniquement disponible dans les emplacements (régions) {{site.data.keyword.Bluemix_notm}} suivants : Dallas (us-south), Londres (eu-gb) et Sydney (au-syd). Elle n'est pas disponible dans l'emplacement de Francfort (eu-de) ni dans {{site.data.keyword.Bluemix_notm}} Dedicated.

Pour plus d'informations sur le choix entre les API, voir [Choix entre les trois API](/docs/services/EventStreams?topic=eventstreams-choose_api_classic).


## Quelle est la configuration requise pour utiliser l'API MQ Light avec {{site.data.keyword.messagehub}} ?
{: #mql_reqs}

<!-- 30/10/18: info was in eventstreams098.md, moved because of doc app changes -->

La configuration suivante est nécessaire pour utiliser l'API {{site.data.keyword.mql}} avec {{site.data.keyword.messagehub}} : 

**Vous devez créer de façon explicite un sujet Kafka nommé "MQLight" pour pouvoir utiliser l'API car tous les messages passent par le sujet "MQLight". Ce sujet doit posséder une partition unique. La création de ce sujet active l'API MQ Light pour votre instance de service. Les sujets utilisés dans l'API MQ Light sont créés automatiquement lorsque vous les utilisez, mais tous les messages se trouvent en fait dans un même sujet Kafka "MQLight".** 

Le sujet "MQLight" est utilisé par l'API MQ Light pour stocker ses données de messages et interagir avec d'autres clients Kafka. Notez que la création de ce sujet entraîne la facturation de frais au tarif standard indiqué dans le plan de paiement des services.

Pour désactiver l'API MQ Light, supprimez le sujet "MQLight". La suppression de ce sujet entraîne la destruction de toutes les données.

<!-- 12/11/18: following info was in eventstreams079.md, moved because of doc app changes -->

## Procédure de connexion et d'authentification
{: #mql_connect}

Pour se connecter au service, une application doit utiliser les informations <code>user</code>, <code>password</code> et <code>mqlight_lookup_url</code> qui figurent dans la [variable d'environnement VCAP_SERVICES](/docs/services/EventStreams?topic=eventstreams-connecting#connect_classic_cf). Suivez les recommandations ci-après en fonction du langage choisi :

**Java**

Si vous spécifiez <code>null</code> comme paramètre endpointService de l'appel create(), le client reçoit l'instruction de lire les informations correspondant aux éléments <code>user</code>, <code>password</code> et
<code>mqlight_lookup_url</code> de VCAP_SERVICES :

<pre>
<code>NonBlockingClient.create(null, new NonBlockingClientAdapter<Void>() {
    public void onStarted(NonBlockingClient client, Void context) {
                client.send("my/topic", "Hello World!", null);
    }
}, null);</code>
</pre>
{:codeblock}

<br>

**Node.js**

Procédez à l'extraction des informations <code>user</code>, <code>password</code> et
<code>mqlight_lookup_url</code> de VCAP_SERVICES et employez-les pour créer le client comme suit :

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

**Ruby**

Procédez à l'extraction des informations <code>user</code>, <code>password</code> et
<code>mqlight_lookup_url</code> de VCAP_SERVICES et employez-les pour créer le client comme suit :
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

**Python**

Procédez à l'extraction des informations <code>user</code>, <code>password</code> et
<code>mqlight_lookup_url</code> de VCAP_SERVICES et employez-les pour créer le client comme suit :
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

Pour plus d'informations sur les API, voir [IBM {{site.data.keyword.mql}}, sur GitHub ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/mqlight){:new_window}.


<!-- 14/11/18: info was in eventstreams080.md, moved because of doc app changes -->

## Procédure de connexion d'applications MQ Light existantes
{: #mql_exist_apps}


Vous pouvez connecter des applications existantes en cours d'exécution sur {{site.data.keyword.IBM_notm}} MQ ou sur le service {{site.data.keyword.mql}} {{site.data.keyword.Bluemix_notm}} au service. Les
applications continuent de fonctionner de la même manière.

Pour connecter des applications existantes, procédez aux vérifications suivantes :

* Vérifiez que l'application utilise la version disponible la plus récente du client API {{site.data.keyword.mql}} correspondant à votre langage.
* Vérifiez que les détails de connexion extraits de VCAP_SERVICES font référence au type de service <code>messagehub</code> et procédez à l'extraction du nom d'utilisateur des connexions à partir de la propriété <code>credentials.user</code> plutôt qu'à partir de la propriété <code>credentials.username</code>, puis procédez à l'extraction de l'URL de recherche de connexion à partir de la propriété <code>credentials.mqlight_lookup_url</code> plutôt qu'à partir de la propriété <code>credentials.connectionLookupURI</code>. Pour plus d'informations, voir [Variable d'environnement VCAP_SERVICES](/docs/services/EventStreams?topic=eventstreams-connecting).

	Notez que cette étape est effectuée pour vous si vous utilisez le client Java&trade; et que vous spécifiez 'null' comme paramètre endpointService dans l'appel create(), de telle sorte que le client procède lui-même à l'extraction des informations.
	
* Votre application doit prendre en charge les connexions TLS v1.2. VCAP_SERVICES ne contient plus de propriété <code>credentials.nonTLSConnectionLookupURI</code> pour la création d'une connexion non TLS.

Notez également les informations suivantes :

* Les limites de message sont cohérentes avec {{site.data.keyword.messagehub}}, mais peuvent différer de celles des autres serveurs prenant en charge l'API {{site.data.keyword.mql}}. Pour plus d'informations, voir [Limites maximales](/docs/services/EventStreams?topic=eventstreams-mql_using#max_limits).
* JMS n'est pas pris en charge.

<!-- 15/11/18: info was in eventstreams081.md, moved because of doc app changes -->

## Echange de messages entre l'API MQ Light et les API Kafka ou REST Kafka
{: #mql_exchange}

Les messages {{site.data.keyword.mql}} sont stockés dans un sujet Kafka sous-jacent unique nommé "MQLight" et sont codés comme indiqué dans le tableau suivant. Ce codage peut aussi être utilisé par d'autres types d'API, comme Kafka or REST Kafka, pour l'échange de messages avec des applications utilisant l'API {{site.data.keyword.mql}}.

### Format de message Kafka
{: #kafka_format notoc}

<table border='1'>
<caption>Tableau 1. Format de message Kafka</caption>
  <tr>
    <th> Clé </th>
    <th> Valeur </th>
  </tr>
  <tr>
    <td> Facultative (non utilisée par l'API)
	<p></p>
	</td>
    <td>**1 octet**
	<p>		     Identificateur de l'API MQ Light, qui est toujours 0xFA.</p>
    <p><var class="keyword varname">**n**</var> **octets**</p>
    <p>		    Message codé AMQP (reposant sur le format Wire AMQP). </p></td>
  </tr>
</table>


<!-- 15/11/18: info was in eventstreams082.md, moved because of doc app changes -->

## Exemples de l'API MQ Light
{: #mql_samples}

Si vous ne disposez pas déjà d'une application, essayez l'API {{site.data.keyword.mql}} avec l'un des exemples.

Le modèle d'application se compose en fait de deux applications simples : une application Web frontale qui envoie des messages à une application de back end, et une
application de back end qui traite les messages, met les lettres en majuscules,
puis renvoie les messages à l'application frontale. Cet exemple illustre comment vous pouvez faire communiquer vos applications entre elles grâce à l'API {{site.data.keyword.mql}}. Vous
pouvez également utiliser l'API {{site.data.keyword.mql}} pour décharger les
agents, fonctionnalité essentielle à la génération d'applications évolutives, réparties
et à couplage lâche.

L'exemple de code est disponible dans le [projet GitHub event-streams-samples![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-messaging/event-streams-samples/tree/master/mqlight){:new_window}.


<!-- 15/11/18: info was in eventstreams083.md, moved because of doc app changes -->

## Limites maximales
{: #max_limits}

L'API {{site.data.keyword.mql}} est soumise aux limites suivantes :

* La quantité maximale de données pouvant être stockées est cohérente avec une seule partition Kafka de votre plan de paiement (généralement 1 Go). Si cette limite de données est dépassée, les messages les plus anciens de la partition sont supprimés au fur et à mesure de l'envoi de nouveaux messages.
* La durée de stockage maximale d'un message est cohérente avec une seule partition Kafka de votre plan de paiement (généralement 24 heures). Vous ne pouvez pas extraire de messages plus anciens.
* La taille maximale d'un message (à l'exclusion des en-têtes) est de 1 Mo.
* Le nombre maximal de clients pouvant être connectés simultanément est de 25.
* Le nombre maximal de destinations pouvant être actives simultanément est de 25. Une destination active se définit comme suit :
  - Une destination où la durée de vie est supérieure à 0 (TimeToLive > 0) avec ou sans client connecté.
  - Une destination où la durée de vie est nulle (TimeToLive = 0) (valeur par défaut) avec un client connecté. 
  <p>Une destination partagée entre des clients compte comme une seule destination.</p>
