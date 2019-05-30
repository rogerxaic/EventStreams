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

# Utilisation de l'API REST d'administration
{: #admin_api}

{{site.data.keyword.messagehub}} fournit une API RESTful d'administration que vous pouvez utiliser pour créer, supprimer, répertorier et mettre à jour des sujets.
{: shortdesc}

Pour la connexion à l'API, vous devez extraire l'URL et les données d'identification d'un objet de données d'identification de service ou d'une clé de service de l'instance de service. Pour obtenir des informations sur la création de ces objets, voir [Connexion à {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting).

L'URL du noeud final de l'API est fournie dans la propriété <code>kafka_admin_url</code>.

Les données d'identification dépendent de la méthode d'authentification. Trois types de données d'identification sont pris en charge :

* **Pour s'authentifier avec l'autorisation de base** :<br/>
    Utilisez les propriétés <code>user</code> et <code>api_key</code> des objets ci-dessus en tant que nom d'utilisateur et mot de passe. Placez ces valeurs dans l'en-tête <code>Authorization</code> de la demande HTTP comme ceci : <code>Basic <base64 encoding of username:password></code>.

* **Pour vous authentifier à l'aide d'un jeton bearer :**<br/>
    Pour obtenir votre jeton à l'aide de l'interface CLI d'IBM Cloud, connectez-vous à IBM Cloud, puis exécutez la commande suivante : 

    ```
    ibmcloud iam oauth-tokens
    ```
    {: codeblock}

    Placez ce jeton dans l'en-tête d'autorisation de la demande HTTP comme ceci : <code>Bearer <jeton></code>. La clé d'API et les jetons JWT sont pris en charge. 

* ** Pour vous authentifier directement à l'aide de la clé d'API :**<br/>
    Placez la clé directement comme la valeur de l'en-tête HTTP <code>X-Auth-Token</code>.

Pour les instances de service créées dans le plan Classic, vous trouverez ces informations dans la variable d'environnement VCAP_SERVICES de votre application.

Pour obtenir une description de l'API avec des exemples, voir
[{{site.data.keyword.messagehub}} admin-rest ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-messaging/event-streams-docs/tree/master/admin-rest-api){:new_window}.

Vous pouvez télécharger la spécification complète de l'API depuis le [{{site.data.keyword.messagehub}} fichier yaml de l'API REST d'administration ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-messaging/event-streams-docs/blob/master/admin-rest-api/admin-rest-api.yaml){:new_window}.
Pour afficher le fichier swagger, utilisez les outils Swagger, par exemple [Swagger Editor ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://editor.swagger.io/#/){:new_window}.




