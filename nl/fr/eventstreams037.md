---

copyright:
  years: 2015, 2018
lastupdated: "2018-07-05"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Utilisation de l'API REST d'administration
{: #admin_api}

{{site.data.keyword.messagehub}} contient une API d'administration RESTful qui permet de créer, de supprimer et de répertorier des sujets.
{: shortdesc}

Pour découvrir le noeud final de l'API RESTful d'administration, votre application {{site.data.keyword.Bluemix_short}} peut utiliser la propriété `kafka_admin_url` dans la variable d'environnement VCAP_SERVICES de l'application.

Vous pouvez télécharger la spécification complète de l'API depuis la page [admin-rest-api.yaml ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-messaging/event-streams-docs/blob/master/admin-rest-api/admin-rest-api.yaml){:new_window}. Pour afficher le fichier swagger, utilisez les outils Swagger, par exemple [Swagger Editor ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://editor.swagger.io/#/){:new_window}.

Le répertoire [{{site.data.keyword.messagehub}} admin-rest-api ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-messaging/event-streams-docs/tree/master/admin-rest-api){:new_window} contient également un ensemble utile d'exemples permettant d'appeler l'API d'administration RESTful.


