---

copyright:
  years: 2015, 2019
lastupdated: "2018-03-23"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Messages relatifs à la consommation
{: #consuming_messages }

Un consommateur est une application qui consomme les flux de messages issus des sujets Kafka. Un consommateur peut s'abonner à un ou plusieurs sujets ou partitions. Ces informations se concentrent sur l'interface de programmation Java qui fait partie du projet Apache Kafka. Les concepts s'appliquent également à d'autres langages, mais avec des noms parfois légèrement différents.
{: shortdesc}

Lorsqu'un consommateur se connecte à Kafka, il établit une connexion d'amorçage initiale. Cette connexion peut viser n'importe quel serveur du cluster. Le consommateur demande des informations concernant la partition et le responsable du sujet qu'il veut "consommer". Puis il établit une autre connexion au responsable de la partition et peut commencer à consommer des messages. Ces actions s'exécutent automatiquement en interne lorsque votre consommateur se connecte au cluster Kafka.

En général, un consommateur est une application à exécution longue. Un consommateur demande régulièrement des messages à Kafka en appelant `Consumer.poll(...)` . Le consommateur appelle `poll()`, reçoit un lot de messages, les traite rapidement, puis appelle de nouveau `poll()`.

Lorsqu'un consommateur traite un message, ce dernier n'est pas supprimé du sujet dont il fait partie. Les consommateurs disposent de plusieurs méthodes pour laisser Kafka déterminer quels messages ont été traités. Ce processus est appelé validation de la position.

Dans les interfaces de programmation, un message est appelé enregistrement. Par exemple, la classe Java org.apache.kafka.clients.consumer.ConsumerRecord est utilisée pour représenter l'API consommateur. Les termes _enregistrement_ et _message_ sont interchangeables, mais le terme enregistrement est majoritairement utilisé pour désigner un message.

Vous trouverez sans doute utile de coupler ces informations à la [génération de messages](/docs/services/EventStreams/eventstreams112.html) dans {{site.data.keyword.messagehub}}.

## Configuration des propriétés du consommateur 
{: #configuring_consumer_properties }

Il existe un grand nombre de paramètres de configuration associés au consommateur, qui contrôlent les divers aspects de son comportement. Les plus importants sont décrits ci après :

| Nom     |Description   | Valeurs valides   | Valeur par défaut   |
|----------|---------|----------|---------|
|key.deserializer     | Classe utilisée pour désérialiser les clés. | Classe Java qui implémente l'interface Deserializer, telle que org.apache.kafka.common.serialization.StringDeserializer  |Aucune valeur par défaut - vous devez indiquer une valeur|
|value.deserializer     | Classe utilisée pour désérialiser les valeurs. | Classe Java qui implémente l'interface Deserializer, telle que org.apache.kafka.common.serialization.StringDeserializer  | Aucune valeur par défaut - vous devez indiquer une valeur |
|group.id | Identificateur du groupe de consommateur auquel appartient le consommateur. | Chaîne |Aucune valeur par défaut|
|auto.offset.reset | Comportement lorsque le consommateur n'a pas de position initiale ou que la position en cours n'est plus disponible dans le cluster. | latest, earliest, none | latest |
|enable.auto.commit | Détermine si la position du consommateur doit être automatiquement validée en arrière-plan. | true, false | true |
|auto.commit.interval.ms | Nombre de millisecondes entre chaque validation régulière des positions. | 0,... | 5000 (5 secondes) |
|max.poll.records | Nombre maximum d'enregistrements renvoyés dans un appel de poll() | 1,... | 500 |
|session.timeout.ms | Nombre de millisecondes pendant lesquelles le signal de présence d'un consommateur doit être reçu pour conserver le consommateur comme membre d'un groupe de consommateurs. | 6000-300000 | 10000 (10 secondes) |
|max.poll.interval.ms |Intervalle de temps maximum entre les interrogations avant que le consommateur quitte le groupe. | 1,... | 300000 (5 minutes) |
| | | | |

De nombreux autres paramètres de configuration sont disponibles, mais lisez d'abord minutieusement la [documentation Apache Kafka ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://kafka.apache.org/documentation/){:new_window} avant de les utiliser.

## Groupes de consommateurs

Un _groupe de consommateurs_ est un groupe d'utilisateurs qui coopèrent pour consommer des messages issus d'un ou plusieurs sujets. Les consommateur d'un groupe utilisent tous la même valeur pour le paramètre de configuration `group.id`. Si vous avez besoin de plusieurs consommateurs pour gérer votre charge de travail, vous pouvez exécuter plusieurs consommateurs dans le même groupe de consommateurs. Même si vous n'avez besoin que d'un seul consommateur, il est habituel de spécifier également une valeur pour `group.id`.

Chaque groupe de consommateurs dispose d'un serveur du cluster, appelé _coordinateur_, responsable de l'affectation des partitions aux consommateurs du groupe. Cette responsabilité est répartie dans le cluster afin d'équilibrer la charge. L'affectation des partitions aux consommateurs peut se modifier à chaque rééquilibrage du groupe.

Lorsqu'un consommateur rejoint un groupe de consommateurs, il découvre le coordinateur du groupe. Il indique alors au coordinateur qu'il veut rejoindre le groupe et le coordinateur lance un rééquilibrage des partitions au sein du groupe en incluant le nouveau membre.

Lorsque l'une des modifications suivantes intervient dans un groupe de consommateurs, le groupe se rééquilibre en modifiant les affectations de partitions aux membres du groupe afin de tenir compte de la modification :

* un consommateur rejoint le groupe
* un consommateur quitte le groupe
* un consommateur est considéré comme n'étant plus actif par le coordinateur
* de nouvelles partitions sont ajoutées à un sujet existant

Pour chaque groupe de consommateurs, Kafka mémorise la position validée pour chaque partition consommée.

Lorsqu'un groupe de consommateurs est rééquilibré, sachez que toute les validations d'un consommateur qui a quitté le groupe seront rejetées jusqu'à ce qu'il rejoigne le groupe. Dans ce cas, le consommateur doit rejoindre le groupe, où il lui sera affecté une autre partition que celle à partir de laquelle il consommait précédemment.

## Continuité des consommateurs

Kafka détecte automatiquement les consommateurs en échec de manière à réaffecter les partitions aux consommateurs actifs. Deux mécanismes permettent cette détection : l'interrogation et le signal de présence.

Si le lot de messages que renvoie `Consumer.poll(...)` est volumineux ou que son traitement est long, le délai avant nouvel appel de `poll()` peut être important ou imprévisible. Dans certains cas, il est nécessaire de configurer un long intervalle maximum entre les interrogations pour que des consommateurs ne soient pas supprimés de leurs groupes uniquement parce que le traitement des messages prend du temps. Avec ce seul mécanisme, la détection d'un consommateur en échec prendrait également beaucoup de temps.

Afin de faciliter la gestion de la continuité des consommateurs, un mécanisme de signal de présence en arrière-plan a été ajouté dans Kafka 0.10.1. Le coordinateur des groupes attend des membres des groupes qu'ils envoient régulièrement un signal de présence pour indiquer qu'ils sont toujours actifs. Une unité d'exécution de signal de présence en arrière-plan s'exécute avec l'envoi régulier des signaux de présence au coordinateur. Si le coordinateur ne reçoit pas de signal de présence d'un membre d'un groupe dans le _délai d'expiration de session_, il supprime le membre du groupe et démarre un rééquilibrage du groupe. Le délai d'expiration de session peut être plus court que l'intervalle maximum entre les interrogations de sorte que le temps nécessaire pour détecter un consommateur en échec soit court même si le traitement du message prend beaucoup de temps.

Vous pouvez configurer l'intervalle maximum entre les interrogations avec la propriété `max.poll.interval.ms` et le délai d'expiration de session avec la propriété `session.timeout.ms`. Vous n'aurez généralement pas besoin de ces paramètres, sauf si le traitement d'un lot de messages prend plus de 5 minutes.

## Gestion des positions

Pour chaque groupe de consommateurs, Kafka conserve la position validée pour chaque partition consommée. Lorsqu'un consommateur traite un message, ce dernier n'est pas supprimé de la partition. Sa position en cours est simplement mise à jour via un processus de validation de position.

{{site.data.keyword.messagehub}} conserve les informations de validation de position pendant 7 jours.

### Que se passe-t-il lorsqu'il n'existe aucune position validée ?
Lorsqu'un consommateur démarre et qu'une partition à consommer lui est affectée, il commence à la position validée de son groupe. S'il n'existe aucune position validée, le consommateur peut choisir de commencer par le message disponible le plus ancien ou le plus récent, selon la valeur de la propriété `auto.offset.reset` comme suit :

- `latest` (valeur par défaut). Votre consommateur reçoit et consomme uniquement les messages arrivés après son abonnement. Il n'a pas connaissance des messages envoyés avant qu'il ne s'abonne et, par conséquent, vous ne devez pas vous attendre à ce que tous les messages d'un sujet soit consommés.
- `earliest`. Votre consommateur consomme tous les messages depuis le début car il a accès à tous les messages qui ont été envoyés.

Si un consommateur passe en échec après le traitement d'un message mais avant validation de sa position, les informations relatives à sa position validée n'intégreront pas le traitement du message. Cela signifie que le message sera de nouveau traité par le prochain consommateur de ce groupe auquel la partition sera affectée.

Une fois les position validées et enregistrées dans Kafka, lorsque les consommateurs redémarrent, ils reprennent au point où ils s'étaient précédemment arrêtés. Lorsqu'il existe une position validée, la propriété `auto.offset.reset` n'est pas utilisée.

### Validation automatique des positions

La meilleure manière de valider les positions est de laisser le consommateur Kafka le faire automatiquement. C'est une opération simple mais qui offre moins de contrôle que la validation manuelle. Par défaut, les positions d'un consommateur sont validées automatiquement toutes les 5 secondes. Cette validation par défaut s'effectue toutes les 5 secondes, quelle que soit la progression du consommateur dans le traitement des messages. De plus, lorsque le consommateur appelle `poll()`, la dernière position renvoyée par le précédent appel de `poll()` est validée (car le traitement est probablement terminé).

Si la position validée intervient après le traitement des messages et qu'un échec de consommateur se produit, il est possible que certains messages ne soient pas traités. Ceci découle du fait que le traitement redémarre à la position validée, qui est postérieure au dernier message à traiter avant l'échec. C'est pourquoi, si la fiabilité est plus importante que la simplicité, il est généralement préférable de valider les positions manuellement.

### Validation manuelle des positions

Si `enable.auto.commit` est défini sur `false`, le consommateur valide manuellement ses positions. Il peut effectuer cette opération de manière synchrone ou asynchrone. Un schéma classique consiste à valider la position du dernier message traité sur la base d'un temporisateur périodique. Ce schéma implique que chaque message est traité au moins une fois, mais que la position validée ne supplante jamais la progression des messages en cours de traitement. La fréquence du temporisateur périodique contrôle le nombre de messages pouvant être de nouveau traités suite à un échec de consommateur. Les messages sont de nouveau extraits à partir de la dernière position validée enregistrée lorsque l'application redémarre ou que le groupe se rééquilibre.

La position validée est celle des messages à partir desquels le traitement reprend. Il s'agit généralement de la position du dernier message traité *plus un*.

### Décalage d'un consommateur

Le décalage d'un consommateur pour une partition est la différence entre la position du plus récent message publié et la position validée du consommateur. Même si des variations naturelles sont courantes dans les débits de génération et de consommation, le débit de consommation ne doit pas être inférieur au débit de génération sur une longue période de temps.

Si vous constatez qu'un consommateur traite avec succès des messages mais saute parfois un groupe de messages, cela peut signifier que le consommateur n'arrive pas à suivre la cadence du débit. Pour les sujets qui n'utilisent pas la compression des journaux, la quantité d'espace occupée par les journaux est gérée en supprimant régulièrement les anciens segments des journaux. Lorsqu'un consommateur est en retard sur le débit au point de consommer des messages se trouvant dans un segment de journal qui a été supprimé, il fait brusquement un saut jusqu'au début du segment de journal suivant. S'il est essentiel que le consommateur traite tous les messages, ce comportement signale une perte de messages du point de vue de ce consommateur.

Vous pouvez utiliser l'outil <code>kafka-consumer-groups</code> pour visualiser le décalage du consommateur. Pour ce faire, vous pouvez également utiliser l'API consommateur et les mesures du consommateur.


## Contrôle de la vitesse de consommation des messages
{: #message_consumption_speed }

Si vous avez des problèmes concernant la gestion des messages provoqués par un afflux trop important de messages, vous pouvez définir une option de consommateur afin de contrôler la vitesse de consommation des messages. Utilisez `fetch.max.bytes` et `max.poll.records` pour contrôler la quantité de données qu'un appel de `poll()` peut renvoyer.


## Gestion du rééquilibrage des consommateurs
Lorsque des consommateurs sont ajoutés dans un groupe ou en sont supprimés, un rééquilibrage du groupe s'effectue et les consommateurs ne peuvent plus consommer de messages. Lors de ce rééquilibrage, le groupe de consommateurs concerné n'est pas disponible pendant un court instant.

Vous pouvez utiliser un programme d'écoute de rééquilibrage des consommateurs (ConsumerRebalanceListener) pour valider manuellement des positions (si vous n'utilisez pas la validation automatique) lorsque vous recevez un rappel "on partitions revoked" et pour mettre en pause tout traitement ultérieur jusqu'à réception d'une notification de succès du rééquilibrage par un rappel "on partition assigned".


## Fragments de code
{: #consumer_code_snippets notoc}

Les fragments de code qui suivent sont d'un niveau très élevé afin d'illustrer les concepts impliqués. Pour obtenir des exemples complets, voir les exemples de {{site.data.keyword.messagehub}} dans GitHub, https://github.com/ibm-messaging/event-streams-samples.

Pour pouvoir vous connecter à {{site.data.keyword.messagehub}}, vous devez d'abord générer le jeu de propriétés de configuration. Etant donné que toutes les connexions à {{site.data.keyword.messagehub}} sont sécurisées à l'aide de TLS et d'une authentification par nom d'utilisateur/mot de passe, vous avez au minimum besoin de ces propriétés. Remplacez KAFKA_BROKERS_SASL, USER et PASSWORD par vos propres données d'identification de service :

```
Properties props = new Properties();
 props.put("bootstrap.servers", KAFKA_BROKERS_SASL);
 props.put("sasl.jaas.config", "org.apache.kafka.common.security.plain.PlainLoginModule required username=\"USER\" password=\"PASSWORD\";");
 props.put("security.protocol", "SASL_SSL");
 props.put("sasl.mechanism", "PLAIN");
 props.put("ssl.protocol", "TLSv1.2");
 props.put("ssl.enabled.protocols", "TLSv1.2");
 props.put("ssl.endpoint.identification.algorithm", "HTTPS");
```

Pour consommer des messages, vous devez également spécifier des désérialiseurs pour les clés et les valeurs, par exemple :

```
 props.put("key.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
 props.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
```

Utilisez ensuite un KafkaConsumer pour consommer des messages, chaque message étant représenté par un ConsumerRecord. La méthode la plus courante pour consommer des messages consiste à placer le consommateur dans un groupe de consommateurs en définissant l'ID de groupe, puis d'appeler `subscribe()` afin d'obtenir une liste de sujets. Le consommateur se voit affecter des partitions à consommer, mais si le groupe contient plus de consommateurs que de partitions dans le sujet, le consommateur risque de ne pas avoir de partition affectée. Ensuite, appelez `poll()` dans une boucle pour réceptionner un lot de messages à traiter, chaque message étant représenté par un ConsumerRecord.

```
props.put("group.id", "G1");
Consumer<String, String> consumer = new KafkaConsumer<>(props);
consumer.subscribe(Arrays.asList("T1"));
while (true) {
   ConsumerRecords<String, String> records = consumer.poll(100);
   for (ConsumerRecord<String, String> record : records)
     System.out.printf("offset = %d, key = %s, value = %s%n", record.offset(), record.key(), record.value());
}
```

Cette boucle de consommateur s'exécute sans fin mais elle peut être interrompue à partir d'une autre unité d'exécution qui appelle `Consumer.wakeup()` afin de l'arrêter proprement.

Pour valider manuellement des positions, vous devez commencer par définir le paramètre de configuration `enable.auto.commit` sur `false`. Ensuite, utilisez `Consumer.commmitSync()` ou `Consumer.commitAsync()` pour mettre à jour régulièrement la validation des positions des consommateurs. Par souci de simplicité, cet exemple traite les enregistrements de chaque partition et valide la dernière position séparément.

```
props.put("group.id", "G1");
props.put("enable.auto.commit", "false");
Consumer<String, String> consumer = new KafkaConsumer<>(props);
consumer.subscribe(Arrays.asList("T1"));
try {
  while (true) {
    ConsumerRecords<String, String> records = consumer.poll(100);
    for (TopicPartition tp : records.partitions()) {
      List<ConsumerRecord<String, String>> partRecords = records.records(tp);
      long lastOffset = 0;
      for (ConsumerRecord<String, String> record : partRecords) {
        System.out.printf("offset = %d, key = %s, value = %s%n", record.offset(), record.key(), record.value());
        lastOffset = record.offset();
      }

      consumer.commitSync(Collections.singletonMap(tp, new OffsetAndMetadata(lastOffset + 1)));
    }
  }
}
finally {
  consumer.close();
}
```

## Gestion des exceptions

Toute application puissante qui utilise le client Kafka a besoin de gérer les exceptions relatives à certaines situations prévisibles. Parfois, les exceptions ne sont pas émises directement car certaines méthodes sont asynchrones et fournissent leurs résultats via un `Future` ou un rappel. Vous trouverez un exemple de code dans [GitHub ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-messaging/event-streams-samples) qui fournit des exemples complets.

Voici une liste des exceptions que vous devez gérer dans votre code :

### org.apache.kafka.common.errors.WakeupException
Emise par `Consumer.poll(...)` comme résultat de l'appel de `Consumer.wakeup()`. Il s'agit de la méthode standard pour interrompre la boucle d'interrogation du consommateur. La boucle d'interrogation doit s'arrêter et `Consumer.close()` être appelé pour se déconnecter proprement.
### org.apache.kafka.common.errors.NotLeaderForPartitionException
Emise comme résultat de `Producer.send(...)` lorsque le responsable d'une partition change. Le client actualise automatiquement ses métadonnées afin de trouver les information à jour concernant le responsable. Relancez l'opération, qui doit réussir avec les métadonnées mises à jour.
### org.apache.kafka.common.errors.CommitFailedException
Emise comme résultat de `Consumer.commitSync(...)` lorsqu'une erreur irrémédiable se produit. Dans certains cas, il n'est pas possible de simplement répéter l'opération car l'affectation de partition peut avoir changé et le consommateur ne plus être en mesure de valider ses positions. Etant donné que le succès de `Consumer.commitSync(...)` peut n'être que partiel dans le cas d'une utilisation avec plusieurs partitions dans un unique appel, la correction des erreurs est simplifiée si vous utilisez un appel `Consumer.commitSync(...)` distinct pour chaque partition.
### org.apache.kafka.common.errors.TimeoutException
Emise par `Producer.send(...),  Consumer.listTopics()` lorsque les métadonnées ne peuvent pas être extraites. Cette exception est également visible dans le rappel d'envoi (ou le Future renvoyé) lorsque l'accusé de réception demandé n'est pas retourné dans `request.timeout.ms`. Le client peut relancer l'opération, mais l'effet d'une opération répétée dépend de l'opération proprement dite. Ainsi, la répétition de l'envoi d'un message peut entraîner la duplication du message.

