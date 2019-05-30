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
{:note: .note}


# Restrictions connues
{: #restrictions}

En cas de problème lors de l'utilisation de {{site.data.keyword.messagehub}}, passez en revue ces restrictions et solutions connues. 
{: shortdesc}

## Les appels Java Kafka ne basculent pas en cas de défaillance du serveur d'amorce Kafka
{: #calls_failover}

### Problème
{: #calls_failover_problem notoc}

La machine virtuelle java (JVM) met en cache les recherches DNS. Lorsque la machine virtuelle Java résout une adresse IP pour un nom d'hôte, elle met en cache l'adresse IP pour un laps de temps défini, nommé "durée de vie". Certaines configurations Java définissent la durée de vie de la machine virtuelle Java de sorte que l'adresse IP d'un nom d'hôte ne soit pas actualisé tant que la machine virtuelle Java n'a pas été redémarrée. Par exemple, une configuration disposant d'un gestionnaire de sécurité.

### Solution palliative
{: #calls_failover_workaround notoc}

Etant donné que {{site.data.keyword.messagehub}} utilise des URL de serveur d'amorce Kafka avec plusieurs adresses IP pour la haute disponibilité, le client Kafka ne connaît pas toutes les adresses IP de courtier, ce qui empêche tout basculement du courtier de travail. Dans ce cas, le basculement nécessite une nouvelle requête d'adresses IP pour les URL de courtier afin d'obtenir une adresse IP de travail. Nous vous invitons à configurer votre machine virtuelle Java avec une valeur de durée de vie comprise entre 30 et 60 secondes. Cette valeur garantit qu'en cas de problème avec une adresse IP de serveur d'amorce, le client Kafka pourra rechercher et utiliser une nouvelle adresse IP en interrogeant le serveur de noms de domaine (DNS).

Issu du fichier <code>java.security</code> : 

```
# The Java-level namelookup cache policy for successful lookups:
#
# any negative value: caching forever
# any positive value: the number of seconds to cache an address for
# zero: do not cache
#
# default value is forever (FOREVER). For security reasons, this
# caching is made forever when a security manager is set. When a security
# manager is not set, the default behavior in this implementation
# is to cache for 30 seconds.
#
# NOTE: setting this to anything other than the default value can have
#       serious security implications. Do not set it unless
#       you are sure you are not exposed to DNS spoofing attack.
#
#networkaddress.cache.ttl=-1
```

### Comment modifier la durée de vie de la machine virtuelle Java
* Pour modifier la durée de vie de la machine virtuelle Java pour toutes les applications, définissez la valeur <code>networkaddress.cache.ttl</code> dans le fichier <code><var class="keyword varname">$JAVA_HOME</var>/jre/lib/security/java.security</code>.
* Pour modifier la durée de vie de la machine virtuelle Java pour une application donnée, définissez la valeur <code>networkaddress.cache.ttl</code> dans votre code d'application comme suit :
```
java.security.Security.setProperty("networkaddress.cache.ttl" , "30");
```

## Les appels Java Kafka risquent de dépasser le délai d'attente
{: #calls_timeout}

### Problème
{: #calls_timeout_problem notoc}

Parfois, un appel client Java Kafka n'arrive pas à trouver Kafka. La cause de cet échec tient au fait que le client Kafka a détecté le même échec d'adresse IP pour chacun des serveurs d'amorce. Le client Kafka essaie chaque adresse IP de courtier (qui est la même adresse IP en échec) et détermine à tort que Kafka est indisponible. Le client Kafka utilise la première adresse IP renvoyée dans la liste si plusieurs adresses IP sont renvoyées dans la requête au DNS.

### Solution palliative
{: #calls_timeout_workaround notoc}

Renouvelez vos appels après avoir attendu suffisamment longtemps pour que la mise en cache des URL de courtier par la machine virtuelle Java expire. Lors des appels Kafka ultérieurs, une adresse IP de courtier de travail doit être renvoyée depuis la requête DNS et utilisée. 

Une proposition d'amélioration (KIP, Kafka Improvement Proposal) N° 302 a été créée pour s'assurer que les clients Kafka essaient toutes les adresses IP de courtier disponibles, et non un sous-ensemble, de sorte que la défaillance d'une seule adresse IP n'entraîne pas d'échec.


## Sujets et partitions
{: #topics_partitions}

*  Les noms de sujet ne peuvent pas comporter plus de 100 caractères.
*  Le nombre par défaut de partitions pour un sujet est un.
*  Chaque espace {{site.data.keyword.Bluemix_notm}} est soumis à une limite de 100 partitions. Si vous voulez créer des
partitions supplémentaires, vous devez utiliser un nouvel espace {{site.data.keyword.Bluemix_notm}}.

<!--following message retention info duplicted in FAQs eventstreams108-->

## Conservation des messages
{: #message_retention}

Par défaut, les messages sont conservés dans Kafka pendant 24 heures maximum et chaque partition ne peut pas dépasser 1 Go. Si le plafond de 1 Go est atteint, les messages les plus anciens sont supprimés pour que la limite soit respectée.

Vous pouvez modifier la durée de conservation des messages lorsque vous créez un sujet à l'aide de l'interface utilisateur ou de l'API d'administration. La limite de temps a un minimum d'une heure et un maximum de 30 jours.

Pour obtenir des informations sur les restrictions concernant les paramètres autorisés lorsque vous créez des sujets à l'aide d'un client Kafka ou de Kafka Streams, voir [Comment utiliser des API Kafka pour créer et supprimer des sujets ?](/docs/services/EventStreams?topic=eventstreams-faqs#topic_admin).

## Création et suppression de sujets dans Kafka
{: #create_delete}

Dans Kafka, la création et la suppression de sujets sont des opérations asynchrones susceptibles de prendre un certain temps. Il est conseillé d'éviter l'utilisation de modèles qui reposent sur la création rapide et la suppression de sujets, ou sur la suppression rapide et la re-création de sujets.

<!--
## Kafka REST API
{: #trouble_rest}

<br/>
**Is this specific to old Standard only? If so I'll move to specific Standard topic.**
{: note}

*  Only the binary-embedded format is supported for requests and
   responses. The Avro and JSON embedded formats are not supported.
*  Concurrent requests are not supported for a consumer instance.
   Read, commit, or delete requests corresponding to a consumer
   instance should be sent only after a response is received for
   any outstanding requests of that instance.

-->
<!--
<br/>
**Is this specific to old Standard only? If so I'll move to specific Standard topic.**
{: note}

## Kafka REST API rate limitation
{: #kafka_rate}

Applications using the Kafka REST API can be subject to rate
limiting for each ApiKey. When this limiting occurs, the API
responds with the following HTTP error:

<code>429 Too Many Requests</code>
{:screen}

If you see this error, wait and submit the request again.

<br/>
**Is this specific to old Standard only? If so I'll move to specific Standard topic.**
{: note}
-->
<!--12/04/18 - Karen: same info duplicated at messagehub108 -->
<!--
## Kafka REST API daily restart
{: #rest_restart}

The Kafka REST API restarts once a day for a short period of
time. During this period, the Kafka REST API might become
unavailable. If this happens, you are recommended to retry your
request. After the REST API has restarted, you will have to
create your Kafka consumer instances again. If this is the case, the
REST API returns the following JSON:

```'{"error_code":40403,"message":"Consumer instance not found."}'
```
{:screen}
-->
<!--
## Kafka high-level consumer API
{: #kafka_consumer}

You cannot use the Apache Kafka 0.8.2 simple or high-level
consumer API with {{site.data.keyword.messagehub}}. Instead, you can use the earliest supported Kafka consumer API, which is 0.10.
-->
