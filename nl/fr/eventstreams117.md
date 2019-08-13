---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-19"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Limites et quotas
{: #kafka_quotas }

{{site.data.keyword.messagehub}} utilise des quotas pour contrôler les ressources qu'un service peut consommer, comme par exemple la bande passante du réseau. Les types et niveaux de quotas dépendent de si vous utilisez le plan Standard ou le plan Enterprise.

## Plan Standard
{: #limits_standard }

### Débit du réseau
{: #standard_throughput }

Le débit maximal pour chaque instance de service est de 1 Mo par seconde par partition jusqu'à un maximum de 20 Mo par seconde. Par exemple, si une instance de service comprend 10 partitions, le débit maximal est de 10 Mo par seconde et si elle comprend 30 partitions, il est de 20 Mo par seconde.

Le débit est mesuré séparément pour les producteurs et les consommateurs. Lorsqu'il est dépassé, une régulation est appliquée en retardant légèrement les réponses aux demandes, ce qui a pour effet de freiner progressivement les producteurs et les consommateurs jusqu'à ce que la largeur de bande soit réduite.

### Partitions
{: #standard_partitions}

100 partitions pour chaque instance de service.

### Conservation
{: #standard_retention}

Un maximum de 1 Go pour chaque partition.

### Autres limites
{: #standard_limits}

* Taille maximale des messages : 1 Mo
* Maximum de clients Kafka actifs simultanément : 100
* Taux de demande maximal [HTTP Produce API]: 100 par seconde
* Taux de demande maximal [HTTP Admin API]: 10 par seconde

## Plan Enterprise
{: #limits_enterprise }

### Débit du réseau
{: #enterprise_throughput }

Le débit maximal recommandé est de 40 Mo par seconde, avec une limite haute de 75 Mo par seconde. Le débit est exprimé en nombre d'octets par seconde pouvant être envoyés et reçus dans un cluster.

Le chiffre recommandé est basé sur une charge de travail typique et tient compte de l'impact possible des actions opérationnelles telles que les mises à jour internes ou les modes de pannes, par exemple la perte d'une zone de disponibilité. Si le débit moyen dépasse le chiffre recommandé, les performances peuvent se être affectées dans ces conditions.


### Partitions
{: #enterprise_partitions}

1000 partitions pour chaque instance de service.

### Conservation
{: #enterprise_retention}

Illimitée, jusqu'à la limite de stockage de votre plan.

### Autres limites
{: #enterprise_limits}

*  Taille maximale des messages : 1 Mo
*  Maximum de clients Kafka actifs simultanément : 10000




















