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


# Connexion à {{site.data.keyword.messagehub}}
{: #connecting}

La manière dont vous vous connectez à {{site.data.keyword.messagehub}} varie selon que votre application fonctionne en mode natif ou en mode Cloud Foundry. Toutefois, dans les deux cas, deux éléments d'information sont requis :
{: shortdesc}

* Les URL de noeud final pour les API
* Des données d'identification pour l'authentification

Lisez les informations suivantes pour savoir comment obtenir ces éléments. Les étapes peuvent varier légèrement, par conséquent veillez à exécuter les étapes appropriées pour votre instance.

Pour savoir comment vous connecter à {{site.data.keyword.messagehub}} si vous utilisez le plan Classic, voir [Connexion à l'aide du plan Classic](/docs/services/EventStreams?topic=eventstreams-connecting_classic).


## Présentation
{: #connect_enterprise}

Les services fournis à l'aide des plans Standard ou Entreprise sont regroupés dans le tableau de bord sous la rubrique **Services**. Les plans sont [activés par IAM ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/iam?topic=iam-getstarted#getstarted){:new_window}. Vous n'avez pas besoin de comprendre parfaitement IAM pour commencer mais un minimum de connaissances est recommandé si vous voulez sécuriser votre service {{site.data.keyword.messagehub}}. Pour plus d'informations, voir [Gestion de l'accès à vos ressources {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-security).

Procédez comme suit pour lier votre application et obtenir les clés de service de vos services. Pour pouvoir créer des sujets, votre application ou votre clé de service doit avoir un rôle de Responsable.

Pour connecter une application, la méthode utilisée dépend de l'endroit où l'application est déployée, c'est à dire dans Cloud Foundry ou en dehors de Cloud Foundry, par exemple dans le service Kubernetes.

## Mise à disposition d'une instance {{site.data.keyword.messagehub}}

Vous devez préalablement mettre à disposition une instance de service {{site.data.keyword.messagehub}} pour le plan Standard ou le plan Enterprise. Ensuite, vous devez vous procurer les détails de connexion de l'API {{site.data.keyword.messagehub}} comme suit.

## Connexion des applications 
{: #connect_enterprise_external}

Pour les applications qui s'exécutent en dehors de Cloud Foundry, les données d'identification sont générées en créant une clé de service. Une fois que vous avez obtenu une clé de service, transmettez manuellement les détails de la clé à votre application avec la méthode de votre choix.

### Obtention des données d'identification à l'aide de la console IBM Cloud
{: #connect_enterprise_external_console}

1. Recherchez votre service {{site.data.keyword.messagehub}} sur le tableau de bord.
2. Cliquez sur la vignette de votre service.
3. Cliquez sur **Données d'identification pour le service**.
4. Cliquez sur **Nouvelles données d'identification**. 
5. Entrez les détails de votre nouvelle donnée d'identification, par exemple un nom et un rôle, puis cliquez sur **Ajouter**. Une nouvelle donnée d'identification s'affiche dans la liste des données d'identification.
6. Cliquez sur cette donnée d'identification en utilisant **Afficher les données d'identification** pour afficher ses détails au format JSON.
7. Transmettez ces données d'identification à votre application. Indiquez <code>token</code> comme nom d'utilisateur et <var class="keyword varname">api_key</var> comme mot de passe. Séparez <code>token</code> et <var class="keyword varname">api_key</var> par une virgule. Pour en savoir plus, voir [Configuration de votre client API Kafka](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_api_client).
   <br/><br/>Vérifiez que votre application analyse les détails.

### Obtention des données d'identification à l'aide de l'interface de ligne de commande IBM Cloud
{: #connect_enterprise_external_cli}

<ol>
<li>Localisez votre service :<br/>
<code>ibmcloud resource service-instances</code></li>
<li>Créez une clé de service :<br/>
<code>ibmcloud resource service-key-create <var class="keyword varname">nom_clé</var> <var class="keyword varname">rôle_clé</var> --instance-name <var class="keyword varname">nom_de votre_service</var></code></li>
<li>Imprimez la clé de service :<br/>
<code>ibmcloud resource service-key <var class="keyword varname">nom_clé</var></code></li>
<li>Transmettez ces données d'identification à votre application. Indiquez <code>token</code> comme nom d'utilisateur et <var class="keyword varname">api_key</var> comme mot de passe. Séparez <code>token</code> et <var class="keyword varname">api_key</var> par une virgule. Pour plus d'informations, voir [Configuration de votre client](/docs/services/EventStreams?topic=eventstreams-kafka_connect).
<p>Vérifiez que votre application analyse les détails.</p></li>
</ol>

## Connexion des applications Cloud Foundry
{: #connect_enterprise_cf}

Votre application doit être liée à l'instance de service {{site.data.keyword.messagehub}}. Pour lier une application Cloud Foundry à un service autre que Cloud Foundry, commencez par créer un alias de service Cloud Foundry puis référencez cet alias depuis votre application Cloud Foundry lors de la liaison. 

Une fois l'application liée, les détails de connexion sont disponibles pour l'application, au format JSON, par le biais de la variable d'environnement VCAP_SERVICES. Vous pouvez lier une application et un service à l'aide de la [console IBM Cloud](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_cf_console) ou de [l'interface CLI d'IBM Cloud](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_cf_cli).

### Liaison d'une application à l'aide de la console IBM Cloud
{: #connect_enterprise_cf_console}

1. Vérifiez que vous vous trouvez dans l'organisation et l'espace Cloud Foundry appropriés.
2. Recherchez votre application Cloud Foundry sur le tableau de bord ou créez une application en cliquant sur le bouton **Créer une ressource**.
3. Cliquez sur la vignette de votre application.
4. Cliquez sur **Connexions**.
5. Cliquez sur **Créer une connexion**.
6. Sélectionnez la vignette du service {{site.data.keyword.messagehub}} avec lequel vous voulez établir une liaison, puis cliquez sur **Se connecter**. 
7. Dans la fenêtre **Connecter un service activé pour IAM** qui s'affiche, sélectionnez un rôle d'accès sous **Rôle d'accès pour la connexion** et un ID de service dans la liste **ID de service pour la connexion** (vous pouvez accepter l'ID auto-généré). Cliquez sur **Connecter**. 

  Cette action crée un alias de service Cloud Foundry pour votre service {{site.data.keyword.messagehub}}, puis lie votre application à cet alias. 

  Reconstituez votre application pour que les changements prennent effet.<br/>
8. Cliquez sur l'onglet **Contexte d'exécution** à gauche et sélectionnez l'onglet **Variables d'environnement** au centre. Vous pouvez maintenant vérifier vos informations VCAP_SERVICES. Votre application peut maintenant y accéder en tant que variables d'environnement. 
 

### Liaison d'une application à l'aide de l'interface CLI d'IBM Cloud
{: #connect_enterprise_cf_cli}

<ol>
<li>Vérifiez que vous vous trouvez dans l'organisation et l'espace Cloud Foundry appropriés. Vous pouvez naviguer de manière interactive en exécutant la commande suivante :<br/>
 <code>ibmcloud target --cf</code></li>
<li>Localisez votre application :</br>
<code>ibmcloud app list</code><br/>
<br/>
Si vous avez un fichier manifest, vous pouvez créer une nouvelle application en exécutant :<br/>
<code>ibmcloud app push</code><br/>
<br/>
Comme l'application n'est pas encore liée à {{site.data.keyword.messagehub}}, elle ne peut pas établir de connexion. Il est donc recommandé d'envoyer l'application par commande push avec le paramètre <code>--no-start</code> afin d'éviter les échecs de connexion inutiles.</li>
<li>Localisez votre service :</br>
<code>ibmcloud resource service-instances</code></li>
<li>Créez un alias de service Cloud Foundry :<br/>
<code>ibmcloud resource service-alias-create <var class="keyword varname">nom_alias</var> --instance-name <var class="keyword varname">nom_de_votre_service</var></code></li>
<li>Liez votre application à l'alias de service créé précédemment :<br/>
<code>ibmcloud service bind <var class="keyword varname">nom_de_votre_application</var> <var class="keyword varname">nom_alias</var></code>.<br/>
<br/>Vous pouvez également mettre à jour votre fichier manifest et envoyer de nouveau l'application par commande push.</li>
<li>Vérifiez que la variable d'environnement VCAP_SERVICES est disponible dans le contexte d'exécution de votre application :<br/>
<code>ibmcloud app env <var class="keyword varname">nom_de_votre_application</var></code></li>
<li>Transmettez ces données d'identification à votre application. Indiquez <code>token</code> comme nom d'utilisateur et <var class="keyword varname">api_key</var> comme mot de passe. Séparez <code>token</code> et <var class="keyword varname">api_key</var> par une virgule. Pour plus d'informations, voir [Configuration de votre client](/docs/services/EventStreams?topic=eventstreams-kafka_connect). 
<p>Vous devrez peut-être reconstituer votre application pour que les modifications prennent effet.</p></li>
</ol>


## Etape suivante
{: #after_connecting}

Maintenant que vous disposez d'une connexion et des données d'identification, vous pouvez sélectionner un client {{site.data.keyword.messagehub}}. Pour plus d'informations, voir [Utilisation de l'API Kafka](/docs/services/EventStreams?topic=eventstreams-kafka_using).

<!--
Charlie said:

"Add some info describing how to take the information made available from above e.g. like the info in the Connecting a client to the Kafka API section of the alpha docs on stage 1? https://console.stage1.bluemix.net/docs/services/EventStreams/eventstreams122.html#alpha_about "
-->







 















