---

copyright:
  years: 2015, 2019
lastupdated: "2018-06-23"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Génération de messages
{: #producing_messages }


Un producteur est une application qui publie des flux de messages dans des sujets Kafka. Ces informations se concentrent sur l'interface de programmation Java qui fait partie du projet Apache Kafka. Les concepts s'appliquent également à d'autres langages, mais avec des noms parfois légèrement différents.
{: shortdesc}

Dans les interfaces de programmation, un message est appelé enregistrement. Ainsi, la classe Java org.apache.kafka.clients.producer.ProducerRecord est utilisée pour représenter un message du point de vue de l'API producteur. Les termes _enregistrement_ et _message_ sont interchangeables, mais le terme enregistrement est majoritairement utilisé pour désigner un message.

Lorsqu'un producteur se connecte à Kafka, il établit une connexion d'amorçage initiale. Cette connexion peut viser n'importe quel serveur du cluster. Le producteur demande des informations concernant la partition et le responsable du sujet dans lequel il veut publier. Puis il établit une autre connexion au responsable de la partition et peut commencer à publier des messages. Ces actions s'exécutent automatiquement en interne lorsque votre producteur se connecte au cluster Kafka.
 
Un message envoyé au responsable de la partition n'est pas immédiatement disponible pour les consommateurs. Le responsable ajoute l'enregistrement du message à la partition, en lui affectant le numéro de position suivant de cette partition. Une fois que tous les suiveurs des répliques totalement synchronisées ont répliqué l'enregistrement et confirmé qu'ils l'ont écrit dans leurs répliques, l'enregistrement est *validé* et disponible pour les consommateurs.

Chaque message est représenté sous forme d'enregistrement composé de deux parties : une clé et une valeur. La clé est généralement utilisée pour les données concernant le message et la valeur constitue le corps du message. Etant donné que de nombreux outils de l'écosystème Kafka (tels les connecteurs à d'autres systèmes) utilisent uniquement la valeur et ignorent la clé, il est préférable de placer toutes les données du message dans la valeur et de n'utiliser la clé que pour le partitionnement ou la compression de journal. Ne vous fiez pas à tout ce que vous pouvez lire sur Kafka pour utiliser la clé.

Bon nombre d'autres systèmes de messagerie disposent également d'un moyen de transmettre d'autres informations avec les messages. Kafka 0.11 introduit à cet effet les en-têtes d'enregistrement, qui sont pris en charge par le plan Enterprise de {{site.data.keyword.messagehub}}. Etant donné que le plan Standard de {{site.data.keyword.messagehub}} est actuellement basé sur Kafka 0.10.2.1, il ne prend pas encore en charge les en-têtes. 

Vous trouverez sans doute utile de coupler ces informations à la [consommation de messages](/docs/services/EventStreams/eventstreams114.html) dans {{site.data.keyword.messagehub}}.

## Paramètres de configuration
{: #config_settings}

Il existe un grand nombre de paramètres de configuration associés au producteur. Vous pouvez contrôler les aspects liés au producteur, notamment la création de lots, les relances et les accusés de réception des messages. Les plus importants sont décrits ci après :

| Nom     |Description   | Valeurs valides   | Valeur par défaut   |
|----------|---------|----------|---------|
|key.serializer     | Classe utilisée pour sérialiser les clés. | Classe Java qui implémente l'interface Serializer, telle que org.apache.kafka.common.serialization.StringSerializer |Aucune valeur par défaut - vous devez indiquer une valeur|
|value.serializer     | Classe utilisée pour sérialiser les valeurs. | Classe Java qui implémente l'interface Serializer, telle que org.apache.kafka.common.serialization.StringSerializer  | Aucune valeur par défaut - vous devez indiquer une valeur |
|acks     | Nombre de serveurs requis pour accuser réception de chaque message publié. Ce paramètre contrôle les garanties de durabilité requises par le producteur. | 0, 1, all (ou -1)  | 1 |
|retries     | Nombre de fois où le client renvoie un message lorsque l'envoi rencontre une erreur. | 0,...  | 0 |
|max.block.ms     | Nombre de millisecondes pendant lesquelles un envoi ou une demande de métadonnées peut rester bloqué en attente. | 0,...  | 60000 (1 minute) |
|max.in.flight.requests.per.connection     | Nombre maximum de demandes sans accusé de réception que le client envoie sur une connexion avant blocage des demandes suivantes| 1,...  | 5 |
|request.timeout.ms     | Durée maximale d'attente du producteur pour une réponse à une demande. Si la réponse n'est pas reçue dans le délai imparti, la demande est relancée ou échoue si le nombre de relances autorisées a été atteint.| 0,...  | 30000 (30 secondes) |

De nombreux autres paramètres de configuration sont disponibles, mais lisez d'abord minutieusement la [documentation Apache Kafka ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://kafka.apache.org/documentation/){:new_window} avant de les utiliser.

## Partitionnement
{: #partitioning}

Lorsqu'un producteur publie un message sur un sujet, il peut choisir la partition à utiliser. Si l'ordre est important, vous devez vous rappeler qu'une partition est une séquence ordonnée d'enregistrements et qu'un sujet contient une ou plusieurs partitions. Si vous voulez qu'une série de messages soit distribuée dans l'ordre, assurez-vous que les messages se trouvent tous dans la même partition. Le moyen le plus simple pour obtenir ce résultat consiste à attribuer la même clé à tous ces messages. 
 
Le producteur peut explicitement indiquer un numéro de partition lorsqu'il publie un message. Cela permet un contrôle direct, mais rend le code du producteur plus complexe car il prend la responsabilité de contrôler la sélection des partitions. Pour plus d'informations, voir l'appel de méthode Producer.partitionsFor. A titre d'exemple, l'appel est décrit pour [Kafka 0.11.0.1 ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://kafka.apache.org/0110/javadoc/org/apache/kafka/clients/producer/KafkaProducer.html){:new_window}
 
Si le producteur n'indique pas de numéro de partition, la sélection de la partition s'effectue via un programme de partitionnement. Le programme de partitionnement par défaut intégré au producteur Kafka fonctionne comme suit :

* Si l'enregistrement n'a pas de clé, la sélection s'effectue de façon circulaire.
* Si l'enregistrement a une clé, la sélection s'effectue en calculant une valeur de hachage pour la clé. Ainsi, la même partition est sélectionnée pour tous les messages ayant la même clé.

Vous pouvez également écrire votre propre programme de partitionnement personnalisé. Un programme de partitionnement personnalisé peut choisir n'importe quel schéma pour affecter des enregistrements à des partitions. Par exemple, utiliser uniquement un sous-ensemble des informations de la clé ou un identificateur propre à l'application.


## Ordre des messages
{: #message_ordering}

En général, Kafka écrit les messages dans l'ordre où le producteur les envoie. Cependant, il existe des cas où des relances risquent de provoquer la duplication ou le réordonnement des messages. Si vous voulez qu'une séquence de messages soit envoyée dans l'ordre, vous devez vous assurer que les messages se trouvent tous dans la même partition.
 
Le producteur peut également relancer automatiquement l'envoi de messages. Il est souvent judicieux d'activer cette fonction de relance pour éviter que votre code d'application n'ait à effectuer lui-même toutes les relances. La combinaison de la création de lots dans Kafka et des relances automatiques peut entraîner la duplication de messages et leur réordonnement.
 
Par exemple, si vous publiez une séquence de trois messages &lt;M1, M2, M3&gt; sur un sujet. Les enregistrements peuvent tous tenir dans le même lot, de sorte qu'ils sont effectivement tous envoyés ensemble au responsable de la partition. Le responsable les écrit alors dans la partition et les réplique sous forme d'enregistrements distincts. En cas d'incident, il est possible que M1 et M2 soient ajoutés à la partition, mais pas M3. Le producteur ne reçoit pas d'accusé de réception et relance l'envoi de &lt;M1, M2 et M3&gt;. Le nouveau responsable écrit simplement M1, M2 et M3 dans la partition, qui contient maintenant &lt;M1, M2, M1, M2 et M3&gt;, le message M1 dupliqué se trouvant après le message M2 d'origine. Si vous limitez à une le nombre de demandes en cours vers chaque courtier, vous évitez ce réordonnement. Vous pouvez toujours trouver un unique enregistrement dupliqué tel que &lt;M1, M2, M2, M3&gt;, mais vous n'aurez plus jamais de défaut d'ordre des séquences. Dans Kafka 0.11 (pas encore disponible dans {{site.data.keyword.messagehub}}), vous pouvez également utiliser la fonction de producteur idempotent pour éviter la duplication de M2.
 
Avec Kafka, il est d'usage d'écrire les applications destinées à gérer les duplications occasionnelles de message car le fait de n'avoir qu'une seule demande en cours a un impact non négligeable sur les performances.

## Accusés de réception des messages
{: #message_acknowledgments}

Lorsque vous publiez un message, vous pouvez choisir le niveau des accusés de réception requis à l'aide du paramètre `acks` de configuration du producteur. Ce choix s'effectue dans un souci d'équilibre entre capacité de traitement et fiabilité. Les trois niveaux possibles sont les suivants :

<dl>
<dt>acks=0 (fiabilité réduite)</dt>
<dd>Le message est considéré comme envoyé dès qu'il a été inscrit sur le réseau. Aucun accusé de réception n'est attendu du responsable de la partition. Par conséquent, des messages peuvent se perdre si le responsable de la partition change. Ce niveau d'accusé de réception est très rapide, mais il induit le risque de perdre des messages dans certaines situations.</dd>
<dt>acks=1 (valeur par défaut)</dt>
<dd>La réception du message est confirmée par accusé de réception au producteur dès que le responsable de la partition a correctement écrit son enregistrement dans la partition. L'accusé de réception intervenant avant confirmation que l'enregistrement a atteint les répliques totalement synchronisées, le message peut se perdre si le responsable passe en échec alors que les suiveurs n'ont pas encore reçu le message. Si le responsable de la partition change, l'ancien responsable en informe le producteur, qui peut traiter l'erreur et relancer l'envoi du message au nouveau responsable. Etant donné que l'accusé de réception des messages intervient avant confirmation de leur réception par toutes les répliques, les messages ayant un accusé de réception mais pas encore totalement répliqués risquent d'être perdus si le responsable de la partition change.</dd>
<dt>acks=all (fiabilité maximale)</dt>
<dd>La réception du message est confirmée par accusé de réception au producteur lorsque le responsable de la partition a correctement écrit son enregistrement et que toutes les répliques totalement synchronisées ont fait de même. Le message n'est pas perdu si le responsable de la partition change, sous réserve qu'au moins une réplique totalement synchronisée soit disponible.</dd>
</dl>

Même si vous n'attendez pas que l'accusé de réception des messages soit envoyé au producteur, les messages sont toujours uniquement disponible pour consommation une fois validés, ce qui signifie que la réplication dans les répliques totalement synchronisées a été effectuée. En d'autres termes, le temps d'attente d'envoi des messages du point de vue du producteur est inférieur au temps d'attente de bout en bout mesuré du producteur qui envoie un message au consommateur qui reçoit le message.

Si possible, évitez d'attendre l'accusé de réception d'un message avant publication du message suivant. Cette attente empêche le producteur de regrouper les messages en lots et réduit le débit de publication des messages jusqu'à atteindre le temps d'attente aller-retour du réseau.

## Création de lots, régulation et compression
{: #batching}

Dans un souci d'efficacité, le producteur collecte ensemble les lots d'enregistrements pour un envoi aux serveurs. Si vous activez la compression, le producteur compresse chaque lot, ce qui permet d'améliorer les performances puisque la quantité de données transférées sur le réseau est moindre.

Si vous tentez de publier des messages plus rapidement qu'ils peuvent être envoyés à un serveur, le producteur les place automatiquement en mémoire tampon dans des demandes par lots. Le producteur gère une mémoire tampon des enregistrements non envoyés pour chaque partition. Bien entendu, il arrive un moment où même la création de lots ne permet pas d'obtenir le débit souhaité.
 
Un autre facteur a un impact important. Pour empêcher des producteurs ou consommateurs individuels d'envahir le cluster, {{site.data.keyword.messagehub}} applique des quotas de capacité de traitement. Le débit auquel chaque producteur envoie des données est calculé et tout producteur qui tente de dépasser son quota est régulé. La régulation est appliquée en reportant légèrement l'envoi des réponses au producteur. Ce processus fait généralement office de frein naturel.
 
En résumé, lorsqu'un message est publié, son enregistrement est d'abord écrit dans une mémoire tampon au niveau du producteur. En arrière-plan, le producteur crée des lots d'enregistrements et les envoie au serveur. Le serveur répond alors au producteur, en appliquant éventuellement un délai de régulation si le producteur publie trop vite. Si la mémoire tampon du producteur est saturée, l'appel d'envoi du producteur est retardé mais peut finalement échouer avec une exception.

## Fragments de code
{: #code_snippets}

Les fragments de code qui suivent sont d'un niveau très élevé afin d'illustrer les concepts impliqués. Pour obtenir des exemples complets, voir les exemples de {{site.data.keyword.messagehub}} dans GitHub https://github.com/ibm-messaging/event-streams-samples.

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

Pour envoyer des messages, vous devez également spécifier des sérialiseurs pour les clés et les valeurs, par exemple :

```
 props.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
 props.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");
```

Utilisez ensuite un KafkaProducer pour envoyer des messages, chaque message étant représenté par un ProducerRecord. N'oubliez pas de fermer le KafkaProducer lorsque vous avez terminé. Ce code envoie seulement le message ; il n'attend pas pour vérifier si l'envoi a réussi.

```
 Producer<String, String> producer = new KafkaProducer<>(props);
 producer.send(new ProducerRecord<String, String>("T1", "key", "value"));
 producer.close();
 ```
 
La méthode `send()` est asynchrone et renvoie un Future que vous pouvez utiliser pour vérifier son achèvement :

```
 Future<RecordMetadata> f = producer.send(new ProducerRecord<String, String>("T1", "key", "value"));
// Do some other stuff
// Now wait for the result of the send
 RecordMetadata rm = f.get();
 long offset = rm.offset; 
```

Vous pouvez également fournir un rappel lors de l'envoi du message :

```
producer.send(new ProducerRecord<String,String>("T1","key","value", new Callback() {
          public void onCompletion(RecordMetadata metadata, Exception exception) {
                     // This is called when the send completes, either successfully or with an exception
          }
});
```

Pour plus d'informations, voir la [documentation Java pour le client Kafka ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://kafka.apache.org/0110/javadoc/index.html){:new_window}, qui est très complète. 

