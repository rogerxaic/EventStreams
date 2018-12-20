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

<!-- 14/11/18: info moved to eventstreams075.md, moved because of doc app changes -->
# Procédure de connexion d'applications MQ Light existantes
{: #mql_exist_apps}

**L'API MQ Light est disponible uniquement dans le cadre du plan Standard.**
<br/>

Vous pouvez connecter des applications existantes en cours d'exécution sur {{site.data.keyword.IBM_notm}} MQ ou sur le service {{site.data.keyword.mql}} {{site.data.keyword.Bluemix_notm}} au service. Les
applications continuent de fonctionner de la même manière.
{:shortdesc}

Pour connecter des applications existantes, procédez aux vérifications suivantes :

* Vérifiez que l'application utilise la version disponible la plus récente du client API {{site.data.keyword.mql}} correspondant à votre langage.
* Vérifiez que les détails de connexion extraits de VCAP_SERVICES font référence au type de service <code>messagehub</code> et procédez à l'extraction du nom d'utilisateur des connexions à partir de la propriété <code>credentials.user</code> plutôt qu'à partir de la propriété <code>credentials.username</code>, puis procédez à l'extraction de l'URL de recherche de connexion à partir de la propriété <code>credentials.mqlight_lookup_url</code> plutôt qu'à partir de la propriété <code>credentials.connectionLookupURI</code>. Pour plus d'informations, voir [Variable d'environnement VCAP_SERVICES](/docs/services/EventStreams/eventstreams127.html).

	Notez que cette étape est effectuée pour vous si vous utilisez le client Java&trade; et que vous spécifiez 'null' comme paramètre endpointService dans l'appel create(), de telle sorte que le client procède lui-même à l'extraction des informations.
	
* Votre application doit prendre en charge les connexions TLS v1.2. VCAP_SERVICES ne contient plus de propriété <code>credentials.nonTLSConnectionLookupURI</code> pour la création d'une connexion non TLS.

Notez également les informations suivantes :

* Les limites de message sont cohérentes avec {{site.data.keyword.messagehub}}, mais peuvent différer de celles des autres serveurs prenant en charge l'API {{site.data.keyword.mql}}. Pour plus d'informations, voir [Limites maximales](/docs/services/EventStreams/eventstreams083.html).
* JMS n'est pas pris en charge.
