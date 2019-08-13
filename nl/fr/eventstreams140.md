---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-13"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:faq: data-hd-content-type='faq'}

# FAQ sur le plan Classic 
{: #faqs_classic}

Vous trouverez ici les réponses aux questions courantes concernant le plan Classic du service {{site.data.keyword.IBM}} {{site.data.keyword.messagehub}}.

Pour consulter les réponses aux questions portant sur tous les plans de {{site.data.keyword.messagehub}}, voir [FAQ](/docs/services/EventStreams?topic=eventstreams-faqs#faqs).
{: shortdesc}

<!--17/10/17 - Karen: same info duplicated at messagehub104 -->
## Comment utiliser des API Kafka pour créer et supprimer des sujets ?
{: #topic_admin_classic}
{: faq}

Si vous utilisez la version 0.11 ou une version ultérieure du client Kafka, ou la version 0.10.2.0 ou une version ultérieure de Kafka Streams, vous pouvez employer des API pour créer et supprimer des sujets. Certaines restrictions s'appliquent aux paramètres autorisés lors de la création des sujets. Actuellement, vous ne pouvez modifier que les paramètres suivants :

<dl>
<dt>cleanup.policy</dt>
<dd>Défini sur <code>delete</code> (par défaut), <code>compact</code> ou <code>delete,compact</code>
<p>**Remarque :**
Si la règle de nettoyage est uniquement <code>compact</code>, <code>delete</code> est automatiquement ajouté, mais la suppression est désactivée selon l'heure. Les messages du sujet sont compactés jusqu'à 1 Go avant d'être supprimés.</p>
</dd>

<dt>retention.ms</dt>
<dd>La durée de conservation par défaut est de 24 heures. La durée minimale est d'une heure et la durée maximale de 30 jours. Spécifiez cette valeur comme un nombre d'heures.
</dd>
</dl>


## Quelle est la durée que {{site.data.keyword.messagehub}} définit dans la fenêtre de conservation des journaux pour le sujet Décalages consommateur ?
{: #offsets_classic }
{: faq}

{{site.data.keyword.messagehub}} conserve les décalages consommateur pendant 7 jours. Cela correspond à la configuration Kafka offsets.retention.minutes. 

La conservation des décalages s'effectue au niveau du système. Vous ne pouvez donc pas la définir au niveau d'un sujet spécifique. Les groupes de consommateurs n'obtiennent que 7 jours de décalages stockés, même en utilisant un sujet dont la durée de conservation a été augmentée jusqu'au maximum de 30 jours. 

<!--following message retention info duplicted in eventstreams057 and evenstreams108-->

## Pendant combien de temps les messages sont-ils conservés ?
{: #messages_retained_classic}

Par défaut, les messages sont conservés dans Kafka pendant 24 heures maximum et chaque partition ne peut pas dépasser 1 Go. Si le plafond de 1 Go est atteint, les messages les plus anciens sont supprimés pour que la limite soit respectée.

Vous pouvez modifier la durée de conservation des messages lorsque vous créez un sujet à l'aide de l'interface utilisateur ou de l'API d'administration. La limite de temps a un minimum d'une heure et un maximum de 30 jours.

Pour obtenir des informations sur les restrictions concernant les paramètres autorisés lorsque vous créez des sujets à l'aide d'un client Kafka ou de Kafka Streams, voir [Comment utiliser des API Kafka pour créer et supprimer des sujets ?](/docs/services/EventStreams?topic=eventstreams-faqs_classic#topic_admin_classic).


## Quel est le comportement en disponibilité de {{site.data.keyword.messagehub}} ?
{: #availability_classic}
{: faq}

Si vous écrivez des applications {{site.data.keyword.messagehub}}, ces informations vous expliquent le comportement normal en disponibilité de {{site.data.keyword.messagehub}} et ce que vos applications sont censées traiter.

### API
{: #api_availability_classic }

Dans le cadre du fonctionnement normal de {{site.data.keyword.messagehub}}, les noeuds des clusters Kafka sont parfois redémarrés.
Dans certains cas, vos applications sont conscientes de la réaffectation des ressources par le cluster. Ecrivez vos applications de telle sorte qu'elles soient résilientes face à ces changements et qu'elles puissent se reconnecter et reprendre leur fonctionnement.

### Ponts {{site.data.keyword.messagehub}} 
{: #bridge_availability_classic }

Ecrivez vos applications de telle sorte qu'elles gèrent la possibilité d'un redémarrage occasionnel des ponts.

## Quelle est la taille de message maximale de {{site.data.keyword.messagehub}} ? 
{: #max_message_size_classic }
{: faq}

La taille de message maximale de {{site.data.keyword.messagehub}} est 1 Mo, qui est la valeur par défaut Kafka. 

## Quels sont les paramètres de réplication de {{site.data.keyword.messagehub}} ? 
{: #replication_classic }
{: faq}

{{site.data.keyword.messagehub}} est configuré de manière à offrir une forte disponibilité et durabilité.
Les paramètres de configuration suivants s'appliquent à tous les sujets et ne sont pas modifiables :
* replication.factor = 3
* min.insync.replicas = 2

## Comment fonctionne la facturation de {{site.data.keyword.messagehub}} dans le plan Classic ? 
{: #billing_classic }
{: faq}

Dans le plan Classic, {{site.data.keyword.messagehub}} prélève régulièrement un décompte de sujets des utilisateurs et {{site.data.keyword.Bluemix_notm}} enregistre la valeur de prélèvement maximale chaque jour. {{site.data.keyword.messagehub}} facture en fonction de cette valeur maximale de partitions concurrentes visualisées et du nombre total de messages envoyés et reçus quotidiennement.

Par exemple, si vous créez et supprimez 1 sujet 10 fois au cours d'une journée, 1 sujet maximum vous sera facturé. En revanche, si vous créez 10 sujets puis les supprimez, vous pouvez être facturé de 0 ou de 10 sujets selon le moment où s'effectue le prélèvement.

{{site.data.keyword.messagehub}} facture chaque message ou tous les 64 k. Un message qui atteint 64 k compte comme 1 message facturable. Les messages de plus de 64 k correspondent à un nombre de messages facturables déterminé selon la formule suivante : <code><var class="keyword varname">taille_message</var> &divide; 64 k</code>.

<!--12/04/18 - Karen: same info duplicated at messagehub057 -->
## Quelle est la fréquence de redémarrage de l'API REST Kafka ? 
{: #REST_restart_classic }
{: faq}

L'API REST Kafka redémarre une fois par jour pendant un court laps de
temps. 

Au cours de cette période, elle est susceptible de ne plus être disponible. Si cela se produit, il est conseillé de relancer votre demande. Après le redémarrage de l'API REST, vous devez
à nouveau créer vos instances consommateur Kafka. Dans ce cas,
l'API REST renvoie le code JSON suivant :

```'{"error_code":40403,"message":"Consumer instance not found."}'
```
{:screen}

## Quelles sont les différences entre les plans de {{site.data.keyword.messagehub}} ?
{: #plan_compare_classic }
{: faq}

Pour en savoir plus sur les autres plans de {{site.data.keyword.messagehub}}, voir [Choix d'un plan](/docs/services/EventStreams?topic=eventstreams-plan_choose).Pour plus d'informations sur le plan Classic, voir [Présentation du plan Classic](/docs/services/EventStreams?topic=eventstreams-plan_choose_classic#plan_choose_classic).


## Comment doit-on traiter les reprises après incident ?
{: #disaster_recovery_classic }
{: faq}

Actuellement, l'utilisateur est responsable de traiter sa propre reprise après incident dans {{site.data.keyword.messagehub}}. Les données de {{site.data.keyword.messagehub}} peuvent être répliquées entre une instance de {{site.data.keyword.messagehub}} dans un emplacement (région) et une autre instance dans un emplacement différent. Cependant, l'utilisateur est responsable de mettre à disposition une instance de {{site.data.keyword.messagehub}} à distance et de gérer la réplication.

L'utilisateur est également responsable de la sauvegarde des données de la charge de message. Bien que ces données soient répliquées sur plusieurs courtiers Kafka au sein d'un cluster, ce qui évite la majorité des pannes, cette réplication ne couvre pas les pannes qui se produisent au niveau de l'emplacement. 

Les noms de sujets sont sauvegardés par {{site.data.keyword.messagehub}}.















