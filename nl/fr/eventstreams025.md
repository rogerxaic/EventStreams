---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-15"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}

# Utilisation de l'API REST Producer
{: #rest_producer_using}


**L'API REST Producer est uniquement disponible dans le cadre du nouveau plan Standard de {{site.data.keyword.messagehub}}.**
<br/>

{{site.data.keyword.messagehub}} fournit une API REST qui vous permet de connecter vos systèmes existants à votre cluster {{site.data.keyword.messagehub}} Kafka. A l'aide de l'API, vous pouvez intégrer {{site.data.keyword.messagehub}} à n'importe quel système prenant en charge les API RESTful.

L'API REST Producer est disponible sous forme d'interface REST évolutive pour la production des messages dans {{site.data.keyword.messagehub}} via un noeud final HTTP sécurisé. Envoyez les données d'événement à {{site.data.keyword.messagehub}}, utilisez la technologie Kafka pour gérer les flux de données, et profitez des fonctionnalités de {{site.data.keyword.messagehub}} pour gérer vos données.

Utilisez l'API pour connecter les systèmes existants à {{site.data.keyword.messagehub}}. Créez des demandes de production depuis vos systèmes vers {{site.data.keyword.messagehub}}, en spécifiant la clé de message, les en-têtes et les sujets dans lesquels vous voulez écrire des messages.


## Production de messages à l'aide de REST
{: #rest_produce_messages}

Utilisez l'API Producer pour écrire des messages dans des sujets. Pour être en mesure de produire sur un sujet, vous devez disposer des informations suivantes :

* Adresse URL du noeud final de l'API {{site.data.keyword.messagehub}}, y compris le numéro de port.
* Sujet dans lequel vous souhaitez produire.
* Clé d'API permettant de se connecter et de produire sur le sujet sélectionné.

Pour la connexion à l'API, vous devez extraire l'URL et les données d'identification d'un objet de données d'identification de service ou d'une clé de service de l'instance de service. Pour obtenir des informations sur la création de ces objets, voir [Connexion à {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting).

L'URL du noeud final de l'API est fournie dans la propriété <code>kafka_http_url</code>.

Utilisez l'une des méthodes suivantes pour vous authentifier :

* **Pour vous authentifier à l'aide de l'autorisation de base :**<br/>
    Utilisez les propriétés <code>user</code> et <code>api_key</code> des objets ci-dessus en tant que nom d'utilisateur et mot de passe. Placez ces valeurs dans l'en-tête <code>Authorization</code> de la demande HTTP comme ceci : <code>Basic <base64 username:password></code>.

* **Pour vous authentifier à l'aide d'un jeton bearer :**<br/>
    Pour obtenir votre jeton à l'aide de l'interface CLI d'IBM Cloud, connectez-vous à IBM Cloud, puis exécutez la commande suivante : 

    ```
    ibmcloud iam oauth-tokens
    ```
    {: codeblock}

    Placez ce jeton dans l'en-tête d'autorisation de la demande HTTP comme ceci : <code>Bearer <jeton></code>. La clé d'API et les jetons JWT sont pris en charge. 

* ** Pour vous authentifier directement à l'aide de la clé d'API :**<br/>
    Placez la clé directement comme la valeur de l'en-tête HTTP <code>X-Auth-Token</code>.

<br/>
Le code suivant montre un exemple d'envoi de message avec curl :

```
curl -v -X POST -H "Authorization: Basic <base64 username:password>" -H "Content-Type: text/plain" -H "Accept: application/json" -d 'test message' "<kafka_http_url>/topics/<topic_name>/records"
```
{: codeblock}


## Référence d'API
{: #rest_api_reference}

Pour obtenir des détails complets sur l'API, consultez la
[référence sur l'API REST Producer de {{site.data.keyword.messagehub}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://ibm.github.io/event-streams/api/){:new_window}.












