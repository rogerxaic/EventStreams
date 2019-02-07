---

copyright:
  years: 2015, 2019
lastupdated: "2018-06-01"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Pont MQ
{: #mq_bridge}

** Le pont MQ est uniquement disponible dans le cadre du plan Standard.**
<br/>

Le pont MQ vous permet de transférer des données de messages d'une file d'attente {{site.data.keyword.IBM_notm}} MQ vers un sujet Kafka {{site.data.keyword.messagehub}}. Le pont MQ vous permet d'effectuer efficacement des charges de travail de type cloud (par exemple, des analyses de données) sur des données de messages {{site.data.keyword.IBM_notm}} MQ générées au sein de votre entreprise.
 {:shortdesc}

Le pont MQ se connecte à un gestionnaire de files d'attente {{site.data.keyword.IBM_notm}} MQ en tant que client MQ et consomme des données de messages MQ à partir d'une file d'attente MQ. Le pont convertit chaque message MQ en un enregistrement Kafka et envoie le message à un sujet Kafka {{site.data.keyword.messagehub}}.

## Versions prises en charge d'{{site.data.keyword.IBM_notm}} MQ
{: #mq_support}

Les versions d'{{site.data.keyword.IBM_notm}} MQ prises en charge par le pont sont les suivantes :

* {{site.data.keyword.IBM_notm}} MQ versions 8.0 et ultérieures

## Sécurisation du pont MQ
{: #mq_bridge_security}

Vous pouvez configurer le pont MQ pour qu'il s'authentifie auprès d'{{site.data.keyword.IBM_notm}} MQ à l'aide d'un ID utilisateur et d'un mot de passe. Il est conseillé d'accorder les droits suivants uniquement à l'identité associée à une instance du pont MQ :

* Droits CONNECT. Le pont MQ doit pouvoir se connecter au gestionnaire de files d'attente MQ.
* Droits GET sur la file d'attente dans laquelle le pont MQ est configuré en vue d'une consommation.

## Fiabilité et ordre des messages
{: #mq_bridge_reliability}

Le pont MQ implémente un niveau de fiabilité d'au moins une fois pour les données de messages qu'il transfère. Cela signifie que le pont ne perd pas les données de messages, mais peut parfois générer des séquences dupliquées des enregistrements Kafka pour une séquence donnée de messages MQ. Cette duplication intervient surtout lorsqu'un événement interrompt le traitement des données des messages par le pont. Par exemple :

* Redémarrage du gestionnaire de files d'attente MQ
* Interruption du réseau entre le gestionnaire de files d'attente MQ et le pont
* Rééquilibrage de l'affectation aux partitions de sujet Kafka
* Interruption du réseau entre le pont et Kafka

Pour garantir la fiabilité du transfert des messages depuis MQ, le pont MQ dernier demande l'accès exclusif à la file d'attente MQ à partir de laquelle il lit les messages. Cet accès empêche ainsi d'autres applications de recevoir des messages de la file d'attente MQ lorsque le pont l'utilise. Si d'autres applications reçoivent déjà des messages provenant de la file d'attente, le pont MQ risque de ne pas pouvoir démarrer tant que ces applications sont en cours d'exécution.

Le pont MQ réachemine généralement les messages MQ vers Kafka dans l'ordre dans lequel il les reçoit d'{{site.data.keyword.IBM_notm}} MQ. Cependant, les données des messages MQ sont parfois dupliquées au sein d'un sujet Kafka (pour les raisons décrites précédemment). Cette possible duplication peut entraîner des différences entre l'ordre des messages MQ et l'ordre de stockage des enregistrements correspondants dans Kafka.

## Affectation aux partitions Kafka
{: #mq_bridge_partition}

Le pont MQ prend en charge des options permettant de contrôler l'affectation des données des messages {{site.data.keyword.IBM_notm}} MQ aux partitions de sujet Kafka. L'ordre des messages au sein d'une partition spécifique dépend de l'option sélectionnée pour l'ordre des données des messages (voir [Fiabilité et ordre des messages](#mq_bridge_reliability)). Les valeurs prises en charge sont les suivantes :
<dl><dt>Default</dt>
<dd>L'affectation par défaut aux partitions Kafka est utilisée, ce qui signifie que les données des messages MQ sont distribuées de manière égale entre les partitions du sujet Kafka.</dd>
<dt>CorrelationId</dt>
<dd>Les messages MQ dotés du même ID de corrélation sont affectés à la même partition Kafka.</dd>
<dt>GroupId</dt>
<dd>Les messages MQ dotés du même ID de groupe sont affectés à la même partition Kafka.
</dd>
</dl>

## Restrictions de la taille des messages
{: #mq_message}

Vous pouvez configurer {{site.data.keyword.IBM_notm}} MQ afin qu'il stocke les messages trop volumineux pour s'insérer dans un enregistrement Kafka {{site.data.keyword.messagehub}}. La taille maximale d'un enregistrement Kafka est de 1 000 000 octets, même si une partie de cette capacité est utilisée lors de la configuration du pont pour l'affectation aux partitions Kafka en fonction de l'ID de corrélation MQ ou de l'ID de groupe MQ. Il est conseillé d'envoyer les messages n'excédant pas 950 kilooctets via le pont MQ.

Si le pont MQ rencontre un message trop volumineux pour être acheminé vers Kafka, il supprime le message et écrit une entrée de journal dans votre tableau de bord Kibana. Pensez à définir l'attribut MAXMSGL de la file d'attente MQ qui envoie les messages au pont pour empêcher les applications MQ d'envoyer des messages à la file d'attente qui ne pourront pas être transférés via le pont MQ.
