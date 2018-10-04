---

copyright:
  years: 2015, 2018
lastupdated: "2018-09-04"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Connexion à {{site.data.keyword.messagehub}}
{: #connecting}

La manière de se connecter à {{site.data.keyword.messagehub}} varie si vous utilisez le plan Standard ou Enterprise et si vous vous connectez depuis une application Cloud Foundry ou depuis un autre client externe. Vous devez collecter deux éléments d'information pour vous connecter à l'une des API de {{site.data.keyword.messagehub}} :

* Les URL de noeud final pour les API
* Des données d'identification pour l'authentification

Lisez les informations suivantes pour savoir comment obtenir ces éléments. Les étapes pouvant légèrement varier, veillez à exécuter les étapes appropriées pour votre instance.

## Mise à disposition d'une instance {{site.data.keyword.messagehub}}

Vous devez préalablement mettre à disposition une instance de service {{site.data.keyword.messagehub}} pour le plan Standard ou le plan Enterprise. La mise à disposition d'une instance {{site.data.keyword.messagehub}} peut générer des frais. Ensuite, procurez-vous les détails de connexion d'API {{site.data.keyword.messagehub}} en effectuant les tâches suivantes.

## Présentation du plan Standard
{: #connect_standard}

Les services mis à disposition à l'aide du plan Standard sont des services Cloud Foundry. Cela signifie qu'ils sont déployés dans une organisation et un espace Cloud Foundry et qu'ils sont regroupés dans le tableau de bord sous l'en-tête **Services Cloud Foundry**. La méthode utilisée pour la connexion d'une application dépend de l'emplacement où l'application est déployée, à savoir dans Cloud Foundry ou à l'extérieur.


## Applications Cloud Foundry sur le plan Standard
{: #connect_standard_cf}

Si votre application s'exécute dans Cloud Foundry, liez-la aux instances de service {{site.data.keyword.messagehub}}. Une fois l'application liée, les détails de connexion sont disponibles pour l'application, au format JSON, dans la variable d'environnement VCAP_SERVICES. Vous pouvez lier une application et un service à l'aide de la console IBM Cloud ou de l'Interface de ligne de commande IBM Cloud.

Exemple de variable d'environnement VCAP_SERVICES :

```
{
  "credentials": {
    "mqlight_lookup_url": "https://mqlight-lookup.messagehub.services.us-south.bluemix.net/Lookup?serviceId=584e8436-e7f5-43db-96ac-2864fccae5ae",
    "api_key": "d9JSx1SYsmLzNRbbgUFneDm2DtkedlVeViObYJIvrPAf2kJA",
    "kafka_admin_url": "https://kafka-admin.messagehub.services.us-south.bluemix.net:443",
    "kafka_rest_url": "https://kafka-rest.messagehub.services.us-south.bluemix.net:443",
    "kafka_brokers_sasl": [
      "kafka01.messagehub.services.us-south.bluemix.net:9093",
      "kafka02.messagehub.services.us-south.bluemix.net:9093",
      "kafka03.messagehub.services.us-south.bluemix.net:9093",
      "kafka04.messagehub.services.us-south.bluemix.net:9093",
      "kafka05.messagehub.services.us-south.bluemix.net:9093"
    ],
    "user": "d9JSx1SYsmLzNRbb",
    "password": "gUFneDm2DtkedlVeViObYJIvrPAf2kJA"
  }
}
```

{: codeblock}

Le contenu de la variable d'environnement est le même, quelle que soit l'API que vous utilisez pour vous connecter à {{site.data.keyword.messagehub}}. Votre application {{site.data.keyword.Bluemix_notm}} sélectionne les données d'identification appropriées depuis la variable d'environnement VCAP_SERVICES, selon l'interface utilisée.
 
Seuls vos cinq premiers courtiers sont répertoriés dans VCAP_SERVICES. Si vous avez plus de cinq courtiers, utilisez un client Kafka pour extraire les détails de vos autres courtiers. 


### Obtention des données d'identification et connexion à l'aide de la console IBM Cloud
{: #connect_standard_cf_console }

1. Vérifiez que vous vous trouvez dans l'organisation et l'espace Cloud Foundry appropriés.
2. Recherchez votre application Cloud Foundry sur le tableau de bord. Si vous n'avez pas encore d'application Cloud Foundry, vous pouvez en créer une en cliquant sur le bouton **Créer une ressource**.
3. Cliquez sur la vignette de votre application.
4. Cliquez sur **Connexions**.
5. Cliquez sur **Créer une connexion**.
6. Sélectionnez la vignette du service {{site.data.keyword.messagehub}} avec lequel vous voulez établir une liaison, puis cliquez sur **Se connecter**. Vous devrez peut-être reconstituer votre application pour que les modifications prennent effet.
7. Cliquez sur l'onglet **Contexte d'exécution** à gauche et sélectionnez l'onglet **Variables d'environnement** au centre. Vous pouvez maintenant vérifier vos informations VCAP_SERVICES et votre application peut désormais accéder à ces variables d'environnement. 


### Obtention des données d'identification à l'aide de l'interface de ligne de commande IBM Cloud 
{: #connect_standard_cf_cli }

<ol>
<li>Vérifiez que vous vous trouvez dans l'organisation et l'espace Cloud Foundry appropriés. Vous pouvez naviguer de manière interactive en exécutant la commande suivante :<br/>
<code>ibmcloud target --cf</code>
</li>
<li>Recherchez votre application :<br/> <code>ibmcloud app list</code> <br/>
</br>
Si vous disposez d'un fichier manifeste, vous pouvez créer une nouvelle application en exécutant :</br>
<code>ibmcloud app push</code>
</li>
<li>Recherchez votre service comme suit :</br> 
<code>ibmcloud service list</code>
</li>
<li>Liez votre application au service comme suit :</br>
<code>ibmcloud service bind <var class="keyword varname">nom_votre_application</var> <var class="keyword varname">nom_votre_service</var></code>
</li>
<li>Vérifiez que la variable d'environnement VCAP_SERVICES est disponible dans le contexte d'exécution d'application en exécutant :</br> 
 <code>ibmcloud app env <var class="keyword varname">nom_votre_application</var></code>. 
</li>
<li>Transmettez ces données d'identification à votre application. Indiquez <code>token</code> comme nom d'utilisateur et <var class="keyword varname">api_key</var> comme mot de passe. Séparez <code>token</code> et <var class="keyword varname">api_key</var> par une virgule. Pour plus d'informations, voir [Configuration de votre client](/docs/services/EventStreams/eventstreams063.html).
<p>Vous devrez peut-être reconstituer votre application pour que les modifications prennent effet.</p></li>
</ol>

## Applications externes sur le plan Standard
{: #connect_standard_external}

Pour les applications qui s'exécutent en dehors de Cloud Foundry, les données d'identification sont générées en créant une clé de service. Une fois que vous avez obtenu une clé de service, transmettez manuellement les détails de la clé à votre application avec la méthode de votre choix.

### Obtention des données d'identification à l'aide de la console IBM Cloud
{: #connect_standard_external_console}

1. Vérifiez que vous vous trouvez dans l'organisation et l'espace Cloud Foundry appropriés.
2. Recherchez votre service {{site.data.keyword.messagehub}} sur le tableau de bord.
3. Cliquez sur la vignette de votre service.
4. Cliquez sur **Données d'identification pour le service**.
5. Cliquez sur **Nouvelles données d'identification**.
6. Entrez les détails de votre nouvelle donnée d'identification tel un nom, puis cliquez sur **Ajouter**. Une nouvelle donnée d'identification s'affiche dans la liste des données d'identification.
7. Cliquez sur cette donnée d'identification en utilisant **Afficher les données d'identification** pour afficher ses détails au format JSON.
8. Transmettez ces données d'identification à votre application. Indiquez <code>token</code> comme nom d'utilisateur et <var class="keyword varname">api_key</var> comme mot de passe. Séparez <code>token</code> et <var class="keyword varname">api_key</var> par une virgule. Pour plus d'informations, voir [Configuration de votre client](/docs/services/EventStreams/eventstreams063.html).


### Obtention des données d'identification à l'aide de l'interface de ligne de commande IBM Cloud 
{: #connect_standard_external_cli }

<ol>
<li>Vérifiez que vous vous trouvez dans l'organisation et l'espace Cloud Foundry appropriés. Vous pouvez naviguer de manière interactive en exécutant la commande suivante :<br>
<code>ibmcloud target --cf</code>
</li>
<li>Recherchez votre service comme suit :<br>
<code>ibmcloud service list</code>
</li>
<li>Vous pouvez créer une clé de service comme suit :<br>
<code>ibmcloud service key-create <var class="keyword varname">nom_votre_service</var> <var class="keyword varname">nom_nouvelle_clé_service</var></code><br>
<br/>
ou utiliser une clé de service existante comme suit : <br/>
<code>ibmcloud service keys <var class="keyword varname">nom_votre_service</var></code> 
</li>
<li>Obtenez les détails de la clé comme suit :</br>
<code>ibmcloud service key-show <var class="keyword varname">nom_votre_service</var> <var class="keyword varname">nom_clé_service</var></code></br>
Cette ligne de commande renvoie les détails de la clé de service au format JSON.</li>
<li>Transmettez ces données d'identification à votre application. Indiquez <code>token</code> comme nom d'utilisateur et <var class="keyword varname">api_key</var> comme mot de passe. Séparez <code>token</code> et <var class="keyword varname">api_key</var> par une virgule. Pour plus d'informations, voir [Configuration de votre client](/docs/services/EventStreams/eventstreams063.html).
</li>
</ol>

 
## Présentation de plan Enterprise
{: #connect_enterprise}

Les services mis à disposition à l'aide du plan Enterprise sont regroupés dans le tableau de bord sous l'en-tête **Services**. Le plan Enterprise est [activé pour IAM ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/docs/iam/quickstart.html#getstarted){:new_window}. Vous n'avez pas besoin de comprendre parfaitement IAM pour démarrer mais un minimum de connaissances est recommandé si vous voulez sécuriser votre service {{site.data.keyword.messagehub}}. Pour plus d'informations, voir [Sécurisation de vos ressources {{site.data.keyword.messagehub}}](/docs/services/EventStreams/eventstreams124.html)

Procédez comme suit pour lier votre application et obtenir les clés de service de vos service. Pour être autorisée à créer des sujets, votre application oui votre clé de service doit avoir un rôle d'accès Responsable.

La méthode utilisée pour la connexion d'une application dépend de l'emplacement où l'application est déployée, à savoir dans Cloud Foundry ou à l'extérieur.

## Applications Cloud Foundry sur le plan Enterprise
{: #connect_enterprise_cf}

Votre application doit être liée à l'instance de service {{site.data.keyword.messagehub}}. Pour lier une application Cloud Foundry à un service autre que Cloud Foundry avec One Cloud, commencez par créer un alias de service Cloud Foundry, puis référencez cet alias depuis votre application Cloud Foundry lors de la liaison.

Une fois l'application liée, les détails de connexion sont disponibles pour l'application, au format JSON, par le biais de la variable d'environnement VCAP_SERVICES. Vous pouvez lier une application et un service à l'aide de la console IBM Cloud ou de l'Interface de ligne de commande IBM Cloud.

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

  Reconstituez votre application pour que les modifications prennent effet.<br/>
8. Cliquez sur l'onglet **Contexte d'exécution** à gauche et sélectionnez l'onglet **Variables d'environnement** au centre. Vous pouvez maintenant vérifier vos informations VCAP_SERVICES. Votre application peut maintenant y accéder en tant que variables d'environnement. 
 

### Liaison d'une application à l'aide de l'interface de ligne de commande IBM Cloud
{: #connect_enterprise_cf_cli}

<ol>
<li>Vérifiez que vous vous trouvez dans l'organisation et l'espace Cloud Foundry appropriés. Vous pouvez naviguer de manière interactive en exécutant la commande suivante :<br/>
 <code>ibmcloud target --cf</code></li>
<li>Recherchez votre application comme suit :</br>
<code>ibmcloud app list</code><br/>
<br/>
Si vous disposez d'un fichier manifeste, vous pouvez créer une nouvelle application en exécutant :<br/>
<code>ibmcloud app push</code><br/>
<br/>
L'application n'étant pas encore liée à {{site.data.keyword.messagehub}}, elle ne peut pas établir de connexion. Il est donc recommandé d'envoyer l'application par commande push avec le paramètre the <code>--no-start</code> afin d'éviter les échecs de connexion inutiles.</li>
<li>Recherchez votre service comme suit :</br>
<code>ibmcloud resource service-instances</code></li>
<li>Créez une alias de service Cloud Foundry comme suit :<br/>
<code>ibmcloud resource service-alias-create <var class="keyword varname">nom_alias</var> --instance-name <var class="keyword varname">nom_votre_service</var></code></li>
<li>Liez votre application à l'alias de service précédemment créé comme suit :<br/>
<code>ibmcloud service bind <var class="keyword varname">nom_votre_application</var> <var class="keyword varname">nom_alias</var></code>.<br/>
<br/>
Vous pouvez également mettre à jour votre fichier manifeste et envoyer de nouveau l'application par commande push.</li>
<li>Vérifiez que la variable d'environnement VCAP_SERVICES est disponible dans le contexte d'exécution d'application comme suit :<br/>
<code>ibmcloud app env <var class="keyword varname">nom_votre_application</var></code></li>
<li>Transmettez ces données d'identification à votre application. Indiquez <code>token</code> comme nom d'utilisateur et <var class="keyword varname">api_key</var> comme mot de passe. Séparez <code>token</code> et <var class="keyword varname">api_key</var> par une virgule. Pour plus d'informations, voir [Configuration de votre client](/docs/services/EventStreams/eventstreams063.html).
<p>Vous devrez peut-être reconstituer votre application pour que les modifications prennent effet.</p></li>
</ol>


## Applications externes sur le plan Enterprise
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
7. Transmettez ces données d'identification à votre application. Indiquez <code>token</code> comme nom d'utilisateur et <var class="keyword varname">api_key</var> comme mot de passe. Séparez <code>token</code> et <var class="keyword varname">api_key</var> par une virgule. Pour plus d'informations, voir [Configuration de votre client](/docs/services/EventStreams/eventstreams063.html).

   <br/><br/>Vérifiez que votre application analyse les détails.

### Obtention des données d'identification à l'aide de l'interface de ligne de commande IBM Cloud
{: #connect_enterprise_external_cli}

<ol>
<li>Recherchez votre service comme suit :<br/>
<code>ibmcloud resource service-instances</code></li>
<li>Créez une instance de service comme suit :<br/>
<code>ibmcloud resource service-key-create <var class="keyword varname">nom_clé</var> <var class="keyword varname">rôle_clé</var> --instance-name <var class="keyword varname">nom_votre_service</var></code></li>
<li>Imprimez la clé de service comme suit :<br/>
<code>ibmcloud resource service-key <var class="keyword varname">nom_clé</var></code></li>
<li>Transmettez ces données d'identification à votre application. Indiquez <code>token</code> comme nom d'utilisateur et <var class="keyword varname">api_key</var> comme mot de passe. Séparez <code>token</code> et <var class="keyword varname">api_key</var> par une virgule. Pour plus d'informations, voir [Configuration de votre client](/docs/services/EventStreams/eventstreams063.html).
<p>Vérifiez que votre application analyse les détails.</p></li>
</ol>

## Etape suivante
{: #after_connecting}

Maintenant que vous disposez d'une connexion et des données d'identification, vous pouvez sélectionner un client {{site.data.keyword.messagehub}}. Votre choix est fonction de votre plan.

* Si vous utilisez le plan Standard, voir
[Choix entre les trois API](/docs/services/EventStreams/eventstreams087.html) pour plus d'informations concernant le client à choisir et comment se connecter.
* Si vous utilisez le plan Enterprise, voir [Utilisation de l'API Kafka](/docs/services/EventStreams/eventstreams050.html).

	Vous n'avez accès au sujet Kafka <code>__consumer_offsets</code> interne qu'en lecture seule si vous utilisez le plan Enterprise. Nous vous conseillons vivement de ne tenter en aucune manière de gérer ce sujet. 

<!--
Charlie said:

"Add some info describing how to take the information made available from above e.g. like the info in the Connecting a client to the Kafka API section of the alpha docs on stage 1? https://console.stage1.bluemix.net/docs/services/EventStreams/eventstreams122.html#alpha_about "
-->







 















