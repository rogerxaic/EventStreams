---

copyright:
  years: 2015, 2019
lastupdated: "2018-11-07"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Connexion à {{site.data.keyword.messagehub}} à l'aide du plan Classic 
{: #connecting_classic}

La manière de se connecter à {{site.data.keyword.messagehub}} varie si vous vous connectez depuis une application Cloud Foundry ou depuis un autre client externe. Vous devez collecter deux éléments d'information pour vous connecter à l'une des API de {{site.data.keyword.messagehub}} :
{: shortdesc}

* Les URL de noeud final pour les API
* Des données d'identification pour l'authentification

Lisez les informations suivantes pour savoir comment obtenir ces éléments. Les étapes peuvent varier légèrement, par conséquent veillez à exécuter les étapes appropriées pour votre instance.

## Mise à disposition d'une instance {{site.data.keyword.messagehub}}
{: #provision_classic}

Vous devez préalablement mettre à disposition une instance de service {{site.data.keyword.messagehub}} pour le plan Classic. Ensuite, vous devez vous procurer les détails de connexion de l'API {{site.data.keyword.messagehub}} comme suit.

## Présentation du plan Classic
{: #connect_classic_plan}

Les services mis à disposition à l'aide du plan Classic sont des services Cloud Foundry. Cela signifie qu'ils sont déployés dans une organisation et un espace Cloud Foundry et qu'ils sont regroupés dans le tableau de bord sous l'en-tête **Services Cloud Foundry**. La méthode utilisée pour la connexion d'une application dépend de l'emplacement où l'application est déployée, c'est-à-dire dans Cloud Foundry ou en dehors de Cloud Foundry, par exemple dans le service Kubernetes.


## Applications Cloud Foundry dans le plan Classic
{: #connect_classic_cf_plan}

Si votre application s'exécute dans Cloud Foundry, liez-la aux instances de service {{site.data.keyword.messagehub}}. Une fois l'application liée, les détails de connexion sont disponibles pour l'application, au format JSON, dans la variable d'environnement VCAP_SERVICES. Vous pouvez lier une application et un service à l'aide de la console IBM Cloud ou de l'interface de ligne de commande IBM Cloud.

Exemple de variable d'environnement VCAP_SERVICES :

```
{
  "credentials": {
    "mqlight_lookup_url": "https://mqlight-lookup.messagehub.services.us-south.bluemix.net/Lookup?serviceId=584e8436-e7f5-43db-96ac-2864fccae5ae",
    "api_key": <YOUR_API_KEY>,
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

Le contenu de la variable d'environnement est le même, quelle que soit l'API que vous utilisez pour vous connecter à {{site.data.keyword.messagehub}}. Votre application {{site.data.keyword.Bluemix_notm}} sélectionne les données d'identification appropriées dans la variable d'environnement VCAP_SERVICES, selon l'interface utilisée.
 
Seuls vos cinq premiers courtiers sont répertoriés dans VCAP_SERVICES. Si vous avez plus de cinq courtiers, utilisez un client Kafka pour extraire les détails de vos autres courtiers. 


### Obtention des données d'identification et connexion à l'aide de la console IBM Cloud
{: #connect_classic_plan_cf_console }

1. Vérifiez que vous vous trouvez dans l'organisation et l'espace Cloud Foundry appropriés.
2. Recherchez votre application Cloud Foundry sur le tableau de bord. Si vous n'avez pas encore d'application Cloud Foundry, vous pouvez en créer une en cliquant sur le bouton **Créer une ressource**.
3. Cliquez sur la vignette de votre application.
4. Cliquez sur **Connexions**.
5. Cliquez sur **Créer une connexion**.
6. Sélectionnez la vignette du service {{site.data.keyword.messagehub}} avec lequel vous voulez établir une liaison, puis cliquez sur **Se connecter**. Vous devrez peut-être reconstituer votre application pour que les modifications prennent effet.
7. Cliquez sur l'onglet **Contexte d'exécution** à gauche et sélectionnez l'onglet **Variables d'environnement** au centre. Vous pouvez maintenant vérifier vos informations VCAP_SERVICES et votre application peut désormais accéder à ces variables d'environnement. 


### Obtention des données d'identification à l'aide de l'interface de ligne de commande IBM Cloud 
{: #connect_classic_plan_cf_cli }

<ol>
<li>Vérifiez que vous vous trouvez dans l'organisation et l'espace Cloud Foundry appropriés. Vous pouvez naviguer de manière interactive en exécutant la commande suivante :<br/>
<code>ibmcloud target --cf</code>
</li>
<li>Recherchez votre application :<br/> <code>ibmcloud app list</code> <br/>
</br>
Si vous avez un fichier manifest, vous pouvez créer une nouvelle application en exécutant :</br>
<code>ibmcloud app push</code>
</li>
<li>Recherchez votre service :</br>
<code>ibmcloud service list</code>
</li>
<li>Liez votre application au service :</br>
<code>ibmcloud service bind <var class="keyword varname">nom_de_votre_application</var> <var class="keyword varname">nom_de_votre_service</var></code>
</li>
<li>Vérifiez que la variable d'environnement VCAP_SERVICES est disponible dans le contexte d'exécution de votre application en exécutant :</br>
 <code>ibmcloud app env <var class="keyword varname">nom_de_votre_application</var></code>. 
</li>
<li>Transmettez ces données d'identification à votre application. Indiquez <code>token</code> comme nom d'utilisateur et <var class="keyword varname">api_key</var> comme mot de passe. Séparez <code>token</code> et <var class="keyword varname">api_key</var> par une virgule. Pour plus d'informations, voir [Configuration de votre client](/docs/services/EventStreams?topic=eventstreams-kafka_connect).
<p>Vous devrez peut-être reconstituer votre application pour que les modifications prennent effet.</p></li>
</ol>

## Applications externes dans le plan Classic
{: #connect_classic_plan_external}

Pour les applications qui s'exécutent en dehors de Cloud Foundry, les données d'identification sont générées en créant une clé de service. Une fois que vous avez obtenu une clé de service, transmettez manuellement les détails de la clé à votre application avec la méthode de votre choix.

### Obtention des données d'identification à l'aide de la console IBM Cloud
{: #connect_classic_plan_external_console}

1. Vérifiez que vous vous trouvez dans l'organisation et l'espace Cloud Foundry appropriés.
2. Recherchez votre service {{site.data.keyword.messagehub}} sur le tableau de bord.
3. Cliquez sur la vignette de votre service.
4. Cliquez sur **Données d'identification pour le service**.
5. Cliquez sur **Nouvelles données d'identification**.
6. Entrez les détails de votre nouvelle donnée d'identification tel un nom, puis cliquez sur **Ajouter**. Une nouvelle donnée d'identification s'affiche dans la liste des données d'identification.
7. Cliquez sur cette donnée d'identification en utilisant **Afficher les données d'identification** pour afficher ses détails au format JSON.
8. Transmettez ces données d'identification à votre application. Indiquez <code>token</code> comme nom d'utilisateur et <var class="keyword varname">api_key</var> comme mot de passe. Séparez <code>token</code> et <var class="keyword varname">api_key</var> par une virgule. Pour plus d'informations, voir [Configuration de votre client](/docs/services/EventStreams?topic=eventstreams-kafka_connect).

### Obtention des données d'identification à l'aide de l'interface de ligne de commande IBM Cloud 
{: #connect_classic_plan_external_cli }

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
ou utilisez une clé de service existante : <br/>
<code>ibmcloud service keys <var class="keyword varname">nom_de_votre_service</var></code> 
</li>
<li>Obtenez les détails de la clé :</br>
<code>ibmcloud service key-show <var class="keyword varname">nom_de_votre_service</var> <var class="keyword varname">nom_clé_service</var></code></br>
Ceci renvoie les détails de la clé de service au format JSON.</li>
<li>Transmettez ces données d'identification à votre application. Indiquez <code>token</code> comme nom d'utilisateur et <var class="keyword varname">api_key</var> comme mot de passe. Séparez <code>token</code> et <var class="keyword varname">api_key</var> par une virgule. Pour plus d'informations, voir [Configuration de votre client](/docs/services/EventStreams?topic=eventstreams-kafka_connect).</li>
</ol>
 
## Etape suivante
{: #after_connecting_next}

Maintenant que vous disposez d'une connexion et des données d'identification, vous pouvez sélectionner un client {{site.data.keyword.messagehub}}. Voir
[Choix entre les trois API](/docs/services/EventStreams?topic=eventstreams-choose_api_classic) pour savoir quel client choisir et comment se connecter.










 















