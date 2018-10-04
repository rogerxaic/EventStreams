---

copyright:
  years: 2015, 2018
lastupdated: "2018-07-05"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Utilisation de l'API Kafka d'administration du client Java
{: #kafka_java_api}


<!-- 
17/10/17 - Karen: following info duplicated at messagehub108
 -->

Si vous utilisez la version 0.11 ou une version ultérieure du client Kafka, ou la version 0.10.2.0 ou une version ultérieure de Kafka Streams, vous pouvez employer des API pour créer et supprimer des sujets. Certaines restrictions s'appliquent aux paramètres autorisés lors de la création des sujets. Actuellement, vous ne pouvez modifier que les paramètres suivants :

<dl>
<dt>cleanup.policy</dt>
<dd>Défini sur <code>delete</code> (par défaut), <code>compact</code> ou <code>delete,compact</code>
<p>**Remarque :**
Si la règle de nettoyage est uniquement <code>compact</code>, <code>delete</code> est automatiquement ajouté, mais la suppression est désactivée selon l'heure. Les messages du sujet sont compactés jusqu'à 1 Go avant d'être supprimés.</p>
</dd>

<dt>retention.ms</dt>
<dd>La durée de conservation par défaut est de 24 heures. La durée minimale est d'une heure et la durée maximale de 30 jours. Spécifiez cette valeur comme un nombre d'heures.

<p>**Remarque :**
Dans le plan Enterprise, ce paramètre accepte toute valeur.</p>
</dd>

<dt>retention.bytes</dt>
<dd>Taille maximale que peut atteindre une partition (constituée de segments de journaux) avant suppression des anciens segments de journaux afin de libérer de l'espace.

<p>**Remarque :**
Plan Enterprise uniquement. Toute valeur supérieure à 1 Mo.</p>
</dd>

<dt>segment.bytes</dt>
<dd>Taille du fichier de segments du journal.

<p>**Remarque :**
Plan Enterprise uniquement. Toute valeur supérieure à 100 Mo.</p>
</dd>

<dt>segment.index.bytes</dt>
<dd>La taille de l'index qui met en correspondance des décalages et des positions de fichier. 

<p>**Remarque :**
Plan Enterprise uniquement. Toute valeur comprise entre 100 Ko et 2 Go.</p>
</dd>

<dt>segment.ms</dt>
<dd>La période de temps après laquelle Kafka force le journal reprendre l'enregistrement par écrasement des segments les plus anciens même si le fichier de segments n'est pas plein. 

<p>**Remarque :**
Plan Enterprise uniquement. Toute valeur comprise entre 5 minutes et 30 jours.</p>
</dd>
</dl>

