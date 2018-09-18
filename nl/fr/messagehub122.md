---

copyright:
  years: 2015, 2018
lastupdated: "2018-04-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

<!-- Notes from chat with Charlie 

Different plan for provisioning

Quality of service from each plan

Life of a user through cycle - APIs, feature sets

-->

# A propos du programme Alpha
{: #alpha_about }

Le programme Alpha offre un accès anticipé aux nouvelles fonctions du service {{site.data.keyword.messagehub}}. La toute dernière nouveauté est l'introduction d'un nouveau plan, le plan {{site.data.keyword.messagehub}} premium.

## Plan premium
{: premium_plan}

Le plan premium est destiné aux utilisateurs ayant des exigences en matière de performances et de fonctionnement qui dépassent celles du service public. Il fournit une version à service exclusif du service {{site.data.keyword.messagehub}} dans laquelle vous êtes l'unique utilisateur d'un cluster Apache Kafka. Cette version vous permet: 

* De tirer pleinement parti de la capacité et des performances du cluster

* D'augmenter grandement les limites et le nombre de partitions

* De supprimer les coûts de gestion avec un cluster automatiquement géré aussi bien concernant la maintenance que les mises à jour

## A propos de votre cluster Alpha
{: alpha_cluster}

Votre cluster Alpha, déployé avec Apache Kafka version 1.1, est capable d'offrir un débit de messages maximal de 90 000 Ko/seconde. 

Vous pouvez créer jusqu'à 1 000 partitions et chaque partition peut conserver jusqu'à 1 Go de données pendant 30 jour. A des fins de résilience, les données sont réparties sur 3 répliques et la position validée de chaque partition est conservée pendant un maximum de 7 jours.

Les deux API suivantes sont prises en charge :

* Pour la messagerie, les clients Kafka en version 0.10.x et ultérieures sont pris en charge, avec option d'utilisation de Kafka Streams, Kafka Connect et KSQL.

* Pour l'administration, une API REST est disponible pour créer, supprimer et répertorier des sujets.

La programme Alpha est disponible uniquement dans la région du sud des Etats-Unis. L'accès aux clusters est géré via IAM.

## Connexion à votre cluster
{: alpha_connect}

Pour vous connecter à une API du cluster, vous avez besoin de son adresse URL de noeud final et d'une clé d'API pour l'authentification. Vous trouverez ces informations dans IAM en procédant de l'une des manières suivantes :

### Application Cloud Foundry
Pour une application Cloud Foundry :
1. Dans l'onglet **Connexions** de l'application (sur la gauche), cliquez sur le bouton **Créer une connexion**. 
2. Sélectionnez l'instance du service {{site.data.keyword.messagehub}} auquel vous voulez vous connecter, puis cliquez sur **Se connecter**. Acceptez les options par défaut. 
3. Une fois la connexion établie, cliquez sur l'onglet **Contexte d'exécution** de l'application, puis sur **Variables d'environnement** pour afficher **VCAP_SERVICES**.

### Console d'une application externe
Depuis la console d'une application externe, créez une clé d'API de service à l'aide de la commande **bx** suivante : 

```
bx resource service-key-create name-of-key Manager --instance-name name-of-your-service
``` 

Copiez les zones <code>kafka_brokers_sasl</code>, <code>kafka_admin_url</code> et <code>apikey</code> à partir des informations générées.
Seuls vos cinq premiers courtiers sont répertoriés dans VCAP_SERVICES. Si vous avez plus de cinq courtiers, utilisez un client Kafka pour extraire les détails de vos autres courtiers. 

## Connexion d'un client à l'API Kafka

Pour connecter un client à l'API Kafka, procédez comme suit :

1. Définissez les propriétés <code>bootstrap.servers</code> des clients sur une liste des courtiers répertoriés dans <code>kafka_brokers_sasl</code>, séparés par des virgules.

2. Définissez la zone USERNAME du fichier <code>sasl.jaas.config</code> des clients sur les 8 premiers caractères de la clé d'API (<code>apikey</code>) et la zone PASSWORD sur les caractères restants (ce fractionnement ne sera plus nécessaire dans les futures versions)

Le client Kafka que vous utilisez doit prendre en charge les fonctions suivantes :

* Client version 0.10.x ou plus récente

* Authentification via le mécanisme SASL Plain

* Extension SNI (Service Name Identification) vers le protocole TLS version 1.2

Cette méthode d'extraction du noeud final et des données d'identification est différente de celle du service {{site.data.keyword.messagehub}} existant. Les applications qui s'exécutent actuellement dans {{site.data.keyword.messagehub}} nécessiteront des modifications afin d'intégrer les nouveaux noms de zone requis par VCAP_SERVICES et les zones username/password soumises à Kafka. Ces modifications ne seront plus obligatoires dans les futures versions d'Alpha.

## Connexion d'un client à l'API REST

Pour connecter un client à l'API REST, procédez comme suit :

* L'identificateur URI de l'API REST est fourni dans <code>kafka_admin_url</code>

* Définissez l'en-tête HTTP <code>Content-Type</code> sur <code>application/json</code>

* Définissez l'en-tête HTTP <code>X-Auth-Token</code> sur la valeur de <code>apikey</code>

Pour les procédures de base de configuration et d'exécution avec Alpha, voir [Initiation au programme Alpha](/docs/services/MessageHub/messagehub120.html).


## Administration de {{site.data.keyword.messagehub}}
{: alpha_admin}

Les seules tâches d'administration à effectuer dans un cluster sont de créer, répertorier et supprimer les sujets dont vous avez besoin. Vous pouvez procéder à cette administration de l'une des manières suivantes :

* Avec l'API d'acministration Kafka directement depuis votre application. Par exemple pour Java, en utilisant la méthode <code>createTopics()</code>, <code>deleteTopics()</code> ou <code>listTopics()</code> depuis [AdminClient ![Icône de lien externe](http://kafka.apache.org/11/javadoc/index.html?org/apache/kafka/clients/admin/AdminClient.html){:new_window}.

* De manière interactive via l'interface utilisateur Web de l'instance de service disponible sur le portail IBM Cloud.

* Avec l'API REST admin fournie dans le cluster.

Vous trouverez d'autres détails concernant les fonctions de création, listage et suppression fournies par l'API REST admin (compatible avec l'API admin de {{site.data.keyword.messagehub}} existante) dans la spécification complète de l'API disponible depuis [admin-rest-api.yaml ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-messaging/message-hub-docs/blob/master/admin-rest-api/admin-rest-api.yaml){:new_window}.
Pour afficher le fichier swagger, utilisez des outils Swagger, par exemple [Swagger Editor ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://editor.swagger.io/#/){:new_window}.

Pour un exemple simple qui montre comment créer un sujet à l'aide de Curl, voir [Initiation au programme Alpha](/docs/services/MessageHub/messagehub120.html).

Dans l'avenir, d'autres options de configuration seront également disponibles.


## Exemples

Bientôt disponible...

Pour les procédures de base de configuration et d'exécution avec Alpha, voir [Initiation au programme Alpha](/docs/services/MessageHub/messagehub120.html).

## Limitations Alpha
{: alpha_limitations}

Les limitations actuelles de ce programmes Alpha sont les suivantes :

### Pas encore disponible mais bientôt proposé

* Prise en charge de plusieurs zones de disponibilité

* Limites de chargement et de mise à l'échelle contrôlées par l'utilisateur

* Intégration au service {{site.data.keyword.cloudaccesstrailfull_notm}} (précédemment nommé AccessTrail) 

### Pas encore prévu

* Ponts

* Messagerie REST

* API MQ Light










