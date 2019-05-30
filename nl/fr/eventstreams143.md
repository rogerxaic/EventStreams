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

# Mise à niveau au nouveau plan Standard de {{site.data.keyword.messagehub}}  
{: #migrate_classic_plan}

Cette nouvelle version du plan Standard à service partagé offre des améliorations significatives en termes de résilience, de fonctionnalité et de convivialité. Pour plus d'informations, consultez l'[annonce du nouveau plan Standard ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/cloud/blog/announcements/ibm-event-streams-releases-a-new-and-enhanced-standard-plan).
{: shortdesc}

Pour migrer les applications de l'ancien plan Standard (maintenant appelé le plan Classic) vers le nouveau plan, tenez compte des informations suivantes.

Les instances de service sont maintenant fournies sous la forme de services {{site.data.keyword.cloud_notm}} au lieu de services Cloud Foundry. Cette modification permet au service de prendre en charge les dernières normes et fonctionnalités d'{{site.data.keyword.cloud_notm}}, y compris les déploiements multizones et les contrôles d'accès granulaires, et affecte également la manière dont le service est utilisé. Notez en particulier les aspects suivants :

* **Création, suppression et référencement de services**
<br/>
    Comme auparavant, vous pouvez gérer le cycle de vie des services en utilisant la console {{site.data.keyword.cloud_notm}} ou l'outil de ligne de commande de l'interface CLI d'{{site.data.keyword.cloud_notm}}. Si vous utilisez la console, les services sont maintenant répertoriés sous **Services** et non sous **Services Cloud Foundry**. 
    
    Si vous utilisez l'interface CLI, les instances sont gérées à l'aide des commandes de ressource (par exemple, <code>ibmcloud resource service-instance-create</code>) au lieu des commandes **cf** (par exemple, <code>ibmcloud cf create-service</code>).

* **Contrôle des accès**
<br/>
    L'authentification et l'autorisation sont maintenant gérées à l'aide du service Cloud Identity and Access Management (IAM). En plus de contrôler la capacité d'un utilisateur à se connecter, IAM vous permet également de configurer l'accès granulaire aux ressources sous-jacentes, telles que les sujets. L'accès est contrôlé en assignant des règles aux utilisateurs. Pour plus d'informations, voir [Gestion de l'accès à vos ressources {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-security).

<ul>
<li><strong>Connexion des applications</strong>
<br/>
    Les informations dont une application a besoin pour se connecter n'ont pas changé : les propriétés <code>bootstrap.servers</code>, <code>username</code> et <code>password</code> continuent d'être requises. Seul leur mode d'extraction a changé.

<ul>
<li>
      <strong>Pour les applications natives</strong>
        <br/>
        Vous devez créer un objet Données d'identification ou Clé de service à l'aide de la console IBM Cloud ou de l'interface CLI respectivement. Vous pouvez ensuite extraire les propriétés requises. Pour plus d'informations, voir [Connexion des applications](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_external).
</li>
<br/>
<li><strong>Pour les applications Cloud Foundry</strong>
        <br/>
        Vous devez d'abord lier le service à l'organisation et à l'espace de l'application en créant un alias de service. Vous pouvez ensuite extraire les propriétés requises de la variable d'environnement VCAP_SERVICES comme d'habitude. Pour plus d'informations, voir [Connexion à {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting).
</li>
</ul>
<br/>
Notez que les clients doivent prendre en charge l'extension SNI vers TLS lorsque le nom d'hôte du serveur est inclus dans l'établissement de liaison TLS. Cette fonctionnalité est généralement disponible et elle est prise en charge dans toutes les versions client recommandées dans la section [Choix d'un client Kafka pour une utilisation avec {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_clients).
</li>
</ul>

<br/>
Notez également les autres changements suivants :

* **Version Kafka**
<br/>
    Ce plan donne accès à la dernière version stable de Kafka 2.2. Les applications développées avec Kafka 1.1 peuvent s'exécuter sans changement, mais vous pouvez consulter les informations suivantes pour connaître les versions client recommandées et les combinaisons testées : [Choix d'un client Kafka pour une utilisation avec {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_clients). 

* **Régions prises en charge**
<br/>
    Le plan est initialement disponible dans la région us-south uniquement. Les autres régions seront introduites au cours des prochaines semaines.

* **Intégrations**
<br/>
    La connexion à partir d'autres services, tels que {{site.data.keyword.iot_short_notm}} ou {{site.data.keyword.openwhisk_short}}, qui se lient au service à l'aide d'une organisation et d'un espace Cloud Foundry, nécessitent la création d'un alias de service. Pour plus d'informations, voir [Connexion des applications Cloud Foundry à {{site.data.keyword.messagehub}}](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_cf).
    

* **Fonctionnalités prises en charge**
<br/>
    Il existe des différences entre les fonctionnalités du plan Classic et du nouveau plan Standard. Dans le but d'harmoniser les offres de produits, d'adopter de nouveaux choix technologiques et d'éliminer les fonctions moins utilisées, les fonctionnalités n'ont pas toutes été conservées. Vous trouverez une comparaison des fonctionnalités sous [Choix de votre plan](/docs/services/EventStreams?topic=eventstreams-plan_choose). Si vous utilisez ces fonctionnalités, d'autres informations vous seront fournies sous peu pour vous aider dans votre processus de migration.
   
<br/>
De petits deltas de code sont livrés quotidiennement à la production. Vous pouvez donc vous attendre à voir de nombreuses autres améliorations de notre expérience utilisateur (et d'autres domaines) tout au long de l'année 2019 et au-delà. Bientôt disponible :

* **Métriques client**
<br/>
    Capacité de surveiller l'action dans une instance de service.

<br/>
Pour obtenir un aperçu rapide des étapes à suivre pour vous familiariser avec le nouveau plan Standard, consultez le [tutoriel d'initiation](/docs/services/EventStreams?topic=eventstreams-getting_started).


