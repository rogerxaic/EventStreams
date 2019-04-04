---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-06"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Accord sur les niveaux de service (SLA) pour la {{site.data.keyword.messagehub}} disponibilité (plan Enterprise)
{: #sla}

Le service {{site.data.keyword.messagehub}} est fourni avec une disponibilité de 99.95% sur le plan Enterprise. 
Pour plus d'informations sur l'accord sur les niveaux de service de {{site.data.keyword.Bluemix}}, voir
la description de service [{{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www-03.ibm.com/software/sla/sladb.nsf/pdf/6605-14/$file/i126-6605-14_08-2018_en_US.pdf){:new_window}.

## Que signifie 99.95% de disponibilité ?
La disponibilité fait référence à la capacité des applications à produire et à utiliser des messages provenant de sujets Kafka.

## Comment la mesurer ?
Les instances de service font l'objet d'une surveillance continue des performances, des taux d'erreur et de leur réponse aux opérations synthétiques. Les indisponibilités sont enregistrées.

## Que devez-vous prendre en compte pour atteindre cette disponibilité ?
Pour atteindre des niveaux de disponibilité élevés du point de vue de l'application, vous devez tenir compte de la [connectivité](/docs/services/EventStreams?topic=eventstreams-sla#connectivity), du [débit](/docs/services/EventStreams?topic=eventstreams-sla#throughput) et de la [cohérence et de la durabilité des messages](/docs/services/EventStreams?topic=eventstreams-sla#message_consistency). Les utilisateurs ont pour responsabilité de concevoir leurs applications pour qu'elles optimisent ces trois éléments pour leur entreprise.

### Connectivité
{: #connectivity}

En raison de la nature dynamique du cloud, les applications doivent s'attendre à des ruptures de connexion. Une rupture de connexion n'est pas considérée comme une défaillance de service.

**Nouvelles tentatives**<br/>
Les clients Kafka fournissent une logique de reconnexion, mais vous devez explicitement activer les reconnexions des fournisseurs. Pour plus d'informations, voir la propriété [ <code>retries</code> ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://kafka.apache.org/11/documentation.html#producerconfigs){:new_window}. Les connexions sont rétablies dans les 60 secondes.   
 
**Doublons**<br/>
L'activation de nouvelles tentatives peut générer des messages en double. Selon le moment où la connexion est interrompue, le fournisseur peut ne pas être en mesure de déterminer si un message a été traité avec succès par le serveur et s'il doit être renvoyé à la reconnexion. Il est recommandé de concevoir des applications qui prévoient des messages en double. 

Si les doublons ne peuvent pas être tolérés, vous pouvez utiliser la fonction de fournisseur <code>idempotent</code> (de Kafka 1.1) pour les empêcher lors des nouvelles tentatives. Pour plus d'informations, voir la propriété [ <code>enable.idempotence</code>![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://kafka.apache.org/11/documentation.html#producerconfigs){:new_window}.

### Débit
{: #throughput}

Le débit est exprimé en nombre d'octets par seconde pouvant être envoyés et reçus dans un cluster. 

**Recommandation**<br/>
40 Mo par seconde avec une limite maximale de 90 Mo par seconde. <br/>
Le chiffre recommandé est basé sur une charge de travail typique et prend en compte l'impact possible d'actions opérationnelles comme des mises à jour internes ou des modes de panne tels que la perte d'une zone de disponibilité. Par exemple, des messages avec peu de contenu (moins de 10 K). Si le débit moyen dépasse ce chiffre, vous risquez de perdre en performance dans ces conditions.

**Mesure**<br/>
Il est recommandé d'instrumenter les applications afin de savoir comment elles se comportent. Par exemple, le nombre de messages envoyés et reçus, les tailles des messages et les codes de retour. Comprendre l'utilisation d'une application vous aide à correctement configurer ses ressources, par exemple la durée de conservation des messages sur certains sujets.

**Saturation**<br/>
A l'approche de la limite du trafic pouvant être généré dans le cluster, les fournisseurs commencent à être saturés, la latence augmente et enfin, des erreurs telles que des erreurs de temporisation se produisent. Selon la configuration, la cohérence et la durabilité des messages peuvent également être affectées. Pour plus d'informations, voir [Cohérence et durabilité des messages](/docs/services/EventStreams?topic=eventstreams-sla#message_consistency).

### Cohérence et durabilité des messages
{: #message_consistency}

Kafka atteint sa disponibilité et sa durabilité en répliquant les messages qu'il reçoit sur d'autres noeuds du cluster, qui pourront ensuite être utilisés en cas de défaillance. {{site.data.keyword.messagehub}} utilise trois répliques (default.replication.factor = 3) ; ce qui signifie que chaque message reçu par un noeud est répliqué sur deux autres noeuds dans différentes zones de disponibilité. De cette manière, la perte d'un noeud ou d'une zone de disponibilité peut être tolérée sans perte de données ou de fonction.

**Mode <code>acks</code> du fournisseur**<br/>
Bien que tous les message soient répliqués, les applications peuvent contrôler le transfert au service des messages générés à l'aide de la propriété mode <code>acks</code> du fournisseur. Cette propriété permet de choisir entre la vitesse et le risque de perte de message. Le paramètre par défaut est <code>acks=1</code>, ce qui signifie que le fournisseur renvoie un message de succès dès que le noeud auquel il est connecté accuse réception du message, mais avant la fin de la réplication. Le paramètre recommandé et le plus sûr est <code>acks=all</code>, le fournisseur ne renvoyant un message de succès qu'une fois le message copié dans toutes les répliques. Cela permet de s'assurer que les répliques sont conservées à cette étape, et d'éviter ainsi la perte de messages si un échec entraîne le passage à une réplique.


