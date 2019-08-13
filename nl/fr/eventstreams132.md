---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-03"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}

# Accord sur les niveaux de service (SLA) relatif à la disponibilité de {{site.data.keyword.messagehub}} 
{: #sla}

## Plan Standard
{: #sla_standard}
Le service {{site.data.keyword.messagehub}} est fourni avec une disponibilité de 99,95 % dans le plan Standard. 
Pour plus d'informations sur l'accord SLA (Service Level Agreement) relatifs aux services à haute disponibilité dans {{site.data.keyword.Bluemix}}, voir [{{site.data.keyword.Bluemix_notm}} la description des services ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www-03.ibm.com/software/sla/sladb.nsf/8bd55c6b9fa8039c86256c6800578854/c4ceb9f019f9eb4c862582f9001b3994/$FILE/i126-6605-16_04-2019_en_US.pdf){:new_window}.


## Plan Enterprise
{: #sla_enterprise}

Le service {{site.data.keyword.messagehub}} est fourni avec une disponibilité de 99.95 % dans le plan Enterprise en tant qu'environnement public à haute disponibilité. Quand le service {{site.data.keyword.messagehub}} s'exécute dans des environnements qui ne sont pas en haute disponibilité, comme les [zones SZR (Single Zone Location Region)](#sla_szr), la disponibilité est 99,5 %. Pour plus d'informations sur l'accord SLA pour les services à haute disponibilité dans {{site.data.keyword.Bluemix_notm}}, voir la description des services
[{{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www-03.ibm.com/software/sla/sladb.nsf/8bd55c6b9fa8039c86256c6800578854/c4ceb9f019f9eb4c862582f9001b3994/$FILE/i126-6605-16_04-2019_en_US.pdf){:new_window}.

## Plan Classic
{: #sla_classic}
Le service {{site.data.keyword.messagehub}} est fourni avec une disponibilité de 99,5 % dans le plan Classic. 
Pour plus d'informations sur l'accord SLA (Service Level Agreement) pour {{site.data.keyword.Bluemix_notm}}, voir la description de service [{{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www-03.ibm.com/software/sla/sladb.nsf/8bd55c6b9fa8039c86256c6800578854/c4ceb9f019f9eb4c862582f9001b3994/$FILE/i126-6605-16_04-2019_en_US.pdf){:new_window}.

<!--
## What does 99.95% availability mean?
Availability refers to the ability of applications to produce and consume messages from Kafka topics.
-->

## Comment la mesurer ?
Les instances de service font l'objet d'une surveillance continue des performances, des taux d'erreur et de leur réponse aux opérations synthétiques. Les indisponibilités sont enregistrées. Pour plus d'informations, voir [Service status for Event Streams ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/status?component=messagehub&selected=status){:new_window}.

La disponibilité fait référence à la capacité des applications à produire et à utiliser des messages provenant de sujets Kafka.

## Que devez-vous prendre en compte pour atteindre cette disponibilité ?
Pour atteindre des niveaux de disponibilité élevés du point de vue de l'application, vous devez tenir compte de la [connectivité](/docs/services/EventStreams?topic=eventstreams-sla#connectivity), du [débit](/docs/services/EventStreams?topic=eventstreams-sla#throughput) ainsi que de la [cohérence et de la durabilité des messages](/docs/services/EventStreams?topic=eventstreams-sla#message_consistency). Les utilisateurs ont pour responsabilité de concevoir leurs applications pour qu'elles optimisent ces trois éléments pour leur entreprise.

### Connectivité
{: #connectivity}

En raison de la nature dynamique du cloud, les applications doivent s'attendre à des ruptures de connexion. Une rupture de connexion n'est pas considérée comme une défaillance de service.

**Nouvelles tentatives**<br/>
Les clients Kafka fournissent une logique de reconnexion, mais vous devez explicitement activer les reconnexions pour les producteurs. Pour plus d'informations, voir la propriété [ <code>retries</code> ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://kafka.apache.org/11/documentation.html#producerconfigs){:new_window}. Les connexions sont rétablies dans les 60 secondes.   
 
**Doublons**<br/>
L'activation des nouvelles tentatives peut générer des messages en double. Selon le moment où la connexion est interrompue, le fournisseur peut ne pas être en mesure de déterminer si un message a été traité avec succès par le serveur et s'il doit être renvoyé à la reconnexion. Il est recommandé de concevoir des applications qui prévoient des messages en double. 

Si les doublons ne peuvent pas être tolérés, vous pouvez utiliser la fonction de fournisseur <code>idempotent</code> (de Kafka 1.1) pour les empêcher lors des nouvelles tentatives. Pour plus d'informations, voir la propriété [<code>enable.idempotence</code> ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://kafka.apache.org/11/documentation.html#producerconfigs){:new_window}.

### Débit
{: #throughput}

Le débit est exprimé en nombre d'octets par seconde pouvant être envoyés et reçus dans un cluster.

**Conseils spécifiques au plan Standard**<br/>
Pour obtenir des informations détaillées sur le débit, voir [Limites et quotas - Standard](/docs/services/EventStreams?topic=eventstreams-kafka_quotas#standard_throughput). 

**Conseils spécifiques au plan Enterprise**<br/>

Pour obtenir des informations sur le débit, voir [Limites et quotas - Enterprise](/docs/services/EventStreams?topic=eventstreams-kafka_quotas#enterprise_throughput). 

**Mesure**<br/>
Il est recommandé de surveiller les applications que vous utilisez afin de vous assurer qu'elles sont performantes. Par exemple, le nombre de messages envoyés et reçus, les tailles des messages et les codes de retour. Comprendre l'utilisation d'une application vous aide à correctement configurer ses ressources, par exemple la durée de conservation des messages sur certains sujets.

**Saturation**<br/>
Au fur et à mesure que la limite du trafic qui peut être généré vers le cluster approche, les fournisseurs commencent à être saturés, la latence augmente et enfin, des erreurs telles que les erreurs de temporisation se produisent. Selon la configuration, la cohérence et la durabilité des messages peuvent également être affectées. Pour plus d'informations, voir [Cohérence et durabilité des messages](/docs/services/EventStreams?topic=eventstreams-sla#message_consistency).

### Cohérence et durabilité des messages
{: #message_consistency}

Kafka atteint sa disponibilité et sa durabilité en répliquant les messages qu'il reçoit sur d'autres noeuds du cluster, qui pourront ensuite être utilisés en cas de défaillance. {{site.data.keyword.messagehub}} utilise trois répliques (default.replication.factor = 3) ; ce qui signifie que chaque message reçu par un noeud est répliqué sur deux autres noeuds dans différentes zones de disponibilité. De cette manière, la perte d'un noeud ou d'une zone de disponibilité peut être tolérée sans perte de données ou de fonction.

**Mode fournisseur <code>acks</code>**<br/>
Bien que tous les messages soient répliqués, les applications peuvent contrôler la robustesse avec laquelle les messages qu'elles produisent sont transférés au service à l'aide de la propriété mode <code>acks</code> du fabricant. Cette propriété permet de choisir entre la vitesse et le risque de perte de message. Le paramètre par défaut est <code>acks=1</code>, ce qui signifie que le fournisseur renvoie un message de succès dès que le noeud auquel il est connecté accuse réception du message, mais avant la fin de la réplication. Le paramètre recommandé et le plus sûr est <code>acks=all</code>, le fournisseur ne renvoyant un message de succès qu'une fois le message copié dans toutes les répliques. Cela permet de s'assurer que les répliques sont conservées à cette étape, et d'éviter ainsi la perte de messages si un échec entraîne le passage à une réplique.

## Déploiements dans une zone SZR (Single Zone Location Region)
{: #sla_szr}

Pour accéder à la plus haute disponibilité, il est conseillé d'utiliser les environnements Public à haute disponibilité, qui sont construits dans des emplacements à zones multiples. Dans un emplacement à zone multiple, les clusters Kafka sont répartis sur trois zones de disponibilité, ce qui permet au cluster de ne pas être impacté par l'échec d'une zone unique et d'un composant dans cette zone.
Certains clients requièrent une localité géographique particulière et veulent donc mettre à disposition un cluster {{site.data.keyword.messagehub}} dans une zone locale géographique qui doit être de type SZR (Single Zone Location Region). {{site.data.keyword.messagehub}} prend en charge ce modèle de déploiement, mais vous devez toutefois tenir compte des compromis de disponibilité suivants :
* Dans une zone SZR (Single Zone Location Region), certaines catégories d'échecs uniques peuvent entraîner la mise hors ligne du cluster pendant un moment (échec d'un centre de données complet, ou mise à jour/échec d'un composant partagé, de type hyperviseur sous-jacent ou réseau SAN ou classique). Ces échecs sont reflétés dans le contrat SLA réduit pour les zones SZR (Single Zone Location Region).
* Un avantage de répartir Kafka sur de nombreuses zones est de réduire le risque d'un échec pouvant provoquer l'arrêt de tout un cluster. A l'inverse, il y a peu de chance qu'un échec unique au sein d'une seule zone ait le même effet sur tout un cluster. Dans les cas extrêmes, il existe également un risque potentiel de perte de données. Ainsi, même si <code>acks=all</code> est utilisé par les producteurs, quand tous les noeuds Kafka tombent en panne simultanément, il peut y avoir des messages dont les courtiers ont accusé réception alors que le système de fichiers sous-jacent n'a pas eu la possibilité de terminer le vidage sur le disque. Potentiellement, ces messages non vidés peuvent être perdus. 

    Pour plus d'informations, voir [Accusés de réception des messages](/docs/services/EventStreams?topic=eventstreams-producing_messages#message_acknowledgments). Dans de nombreux cas d'utilisation, ce n'est pas forcément un problème. Toutefois, si une perte de message est inacceptable quelles que soient les circonstances, envisagez d'autres stratégies, telles que l'utilisation d'un cluster de zones à disponibilité multiple, une réplication entre plusieurs régions ou une vérification des messages côté producteur.

Pour plus d'informations, voir les rubriques relatives aux clusters [à zone unique](/docs/containers?topic=containers-regions-and-zones#regions_single_zone) et [à zone multiple](/docs/containers?topic=containers-regions-and-zones#regions_multizone).
