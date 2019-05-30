---

copyright:
  years: 2015, 2019
lastupdated: "2018-06-01"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Pont Cloud Object Storage dans le plan Classic
{: #cloud_object_storage_bridge }


** Le pont Cloud Object Storage est uniquement disponible dans le plan Classic.**
<br/>

Le pont {{site.data.keyword.IBM}} Cloud Object Storage offre un moyen de lire des données à partir d'un sujet {{site.data.keyword.messagehub}} Kafka et de placer ces données dans [{{site.data.keyword.IBM_notm}} Cloud Object Storage ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](docs/services/cloud-object-storage?topic=cloud-object-storage-about#about){:new_window}.
{: shortdesc}

Le pont Cloud Object Storage permet d'archiver des données provenant des sujets Kafka de {{site.data.keyword.messagehub}} dans une instance
du [service Cloud Object Storage ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](docs/services/cloud-object-storage?topic=cloud-object-storage-about#about){:new_window}. Le pont consomme des lots de messages issus de Kafka et transfère les données des messages sous forme d'objets vers un compartiment du service
Cloud Object Storage. En configurant
le pont Cloud Object Storage, vous pouvez
contrôler le mode de transfert des données sous forme d'objets vers Cloud Object Storage. Les
propriétés que vous pouvez configurer sont les suivantes :

* Le nom du compartiment dans lequel les objets sont inscrits.
* La fréquence de transfert des objets vers le service Cloud Object Storage.
* La quantité de données écrites dans chaque objet avant son transfert vers le service Cloud Object Storage.

Le format de sortie du pont est un objet de service de stockage d'objets contenant un ou plusieurs
enregistrements concaténés avec des caractères de retour à la ligne comme séparateurs.

## Transfert des données via le pont Cloud Object Storage
{: #data_transfer notoc}

Le pont Cloud Object Storage lit
un certain nombre d'enregistrements Kafka à partir d'un sujet et écrit les données de ces enregistrements
dans un objet. Cet objet est transféré vers une instance du service Cloud Object Storage. Chaque pont Cloud Object Storage
lit les données des messages à partir d'un
seul sujet Kafka, même si un sujet peut être lu par plusieurs ponts à la fois. Une nouvelle instance du pont Cloud Object Storage
commence toujours sa lecture par la position la moins récente du sujet Kafka. Le pont Cloud Object Storage utilise la gestion des positions de
consommateur de Kafka pour transférer les données de manière fiable depuis Kafka sans
perte, mais avec un léger risque de duplication.

Vous pouvez contrôler le nombre d'enregistrements lus depuis Kafka avant l'écriture des données dans l'instance de service Cloud Object Storage
à l'aide des propriétés ci-après. Indiquez ces propriétés lorsque vous créez ou mettez à jour un pont :
<dl><dt>Upload Duration Threshold (seconds)</dt> 
<dd>Définit une durée en secondes au bout de laquelle les données accumulées à partir de Kafka
sont transférées vers le service Cloud Object Storage.</dd>
<dt>Upload Size Threshold (kB)</dt>
<dd>Contrôle la quantité de données en kilooctets accumulées à partir de Kafka avant leur
transfert vers le service
Cloud Object Storage.</dd>
</dl>

Le transfert des données lues à partir de Kafka par le pont Cloud Object Storage
vers le service Cloud Object Storage est déclenché lorsque l'une de ces
valeurs est atteinte. Le pont Cloud Object Storage ne garantit pas le transfert des données vers le service Cloud Object Storage
au moment précis où le premier point de ces seuils est atteint. Les données
transférées peuvent ainsi arriver plus tard ou être plus volumineuses que les valeurs indiquées pour ces propriétés.

Le pont Cloud Object Storage concatène les messages en utilisant des caractères de retour à la ligne comme séparateurs lorsqu'il écrit les
données dans Cloud Object Storage. Du fait de ces
séparateurs, le pont ne convient ni aux messages contenant des caractères de retour à la ligne imbriqués, ni aux données de message binaires.


## Obtention de données d'identification à utiliser avec le pont Cloud Object Storage
{: notoc}

Vous devez fournir des données d'identification pour permettre au pont Cloud Object Storage de se connecter à votre instance Cloud Object Storage. Demandez au
propriétaire ou à l'administrateur de votre instance Cloud Object Storage de créer les données d'identification à l'aide de l'interface utilisateur de Cloud Object Storage comme suit : 

1. Sélectionnez **Données d'identification pour le service**, puis **Nouvelles données d'identification**. 
2. Dans **Nouvelles données d'identification**, sélectionnez **Auteur** pour **Rôle d'accès** et **Générer automatiquement** pour **ID de service**.
   
   Vous pouvez copier le JSON obtenu à partir de ces nouvelles données d'identification dans le tableau de bord {{site.data.keyword.messagehub}} de la console {{site.data.keyword.Bluemix_notm}} lorsque vous créez un pont. 
   
   Sinon, vous pouvez prendre les valeurs des zones <code>apikey</code> et <code>resource_instance_id</code> et les entrer dans le tableau de bord {{site.data.keyword.messagehub}} ou les définir dans le JSON de création du pont si vous créez ce dernier directement à l'aide d'un appel REST.

Les donnée d'identification que vous créez accordent un accès en tant qu'auteur à l'intégralité de l'instance Cloud Object Storage,
de sorte que vous voudrez sans doute limiter cet accès aux seuls compartiments avec lesquels le pont interagira.
1. Accédez à la [page Gérer l'accès et les utilisateurs ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/iam#/overview){:new_window}.
2. Cette page doit afficher l'ID de service généré automatiquement. Une fois que vous avez identifié l'ID requis, sélectionnez
l'action **Gérer un ID de service**. 
3. Sélectionnez l'action **Editer la règle** afin d'étendre la restriction à un **Type de ressource** spécifique, qui est un compartiment, et à un **ID de ressource**, qui correspond
au nom du compartiment. Cliquez sur **Sauvegarder**.


## Création d'un pont Cloud Object Storage
{: notoc}

Pour créer un nouveau pont Cloud Object Storage à l'aide de l'API REST de Kafka, utilisez JSON comme dans l'exemple qui suit. Vérifiez que vos noms de compartiment sont globalement uniques, pas uniquement au sein de votre instance Cloud Object Storage.

<pre class="pre"><code>
{
  "name": "cosbridge",
  "topic": "kafka-java-console-sample-topic",
  "type": "objectStorageS3Out",
  "configuration" : {
    "credentials": {
      "endpoint" : "https://s3-api.us-geo.objectstorage.softlayer.net",
      "resourceInstanceId" : "crn::",
      "apiKey" : "your_api_key"
    },
    "bucket" : "cosbridge0",
    "uploadDurationThresholdSeconds" : 600,
    "uploadSizeThresholdKB" : 1024,
    "partitioning" : [ {
        "type" : "kafkaOffset"
      }
    ]
  }
}
</code></pre>
{: codeblock}


## Partitionnement des données en objets par le pont Cloud Object Storage
{: notoc}

Le pont Cloud Object Storage offre une fonctionnalité permettant de partitionner
les messages Kafka et de les stocker sous forme d'objets nommés avec un préfixe commun. Un
groupe d'objets nommés avec un préfixe commun s'appelle une partition. Le partitionnement du pont Cloud Object Storage
est différent du partitionnement Kafka.

A chaque fois que le pont Cloud Object Storage
dispose d'un lot de données à transférer vers le service Cloud Object Storage, il crée un ou plusieurs
objets contenant les données. La décision quant au mode de partitionnement des messages et de nommage des objets qui en
résultent dépend de la configuration du pont. Les noms d'objet peuvent contenir des
métadonnées Kafka et éventuellement des données provenant des messages eux-mêmes. Actuellement, le pont prend en charge les deux modes de
partitionnement suivants des messages Kafka en objets Cloud Object Storage :

* Par la position de message Kafka
* Par une date ISO 8601 figurant dans chaque message Kafka. Les messages Kafka doivent alors contenir un objet au format JSON valide.

## Partitionnement par position de message Kafka
{: notoc}

Pour partitionner des données selon la position de message Kafka, procédez comme suit :

1. Configurez un pont sans la propriété `"inputFormat"`.
2. Spécifiez un objet avec une propriété `"type"` définie sur la valeur `"kafkaOffset"` dans le tableau `"partitioning"`. 

    Exemple :
    <pre class="pre"><code>
        ```
        {
          "topic": "topic1",
          "type": "objectStorageOut",
          "name": "bridge1",
          "configuration" : {
            "credentials" : { ... },
            "bucket" : "bucket1",
            "uploadDurationThresholdSeconds" : "1000",
            "uploadSizeThresholdKB" : "1000",
            "partitioning" : [ {
                "type" : "kafkaOffset"
              }
            ]
          }
        }
        ```
     	</code></pre>
    {:codeblock}

    Les noms d'objet générés par un pont configuré de cette manière contiennent le préfixe `"offset=<kafka_offset>"` où `"<kafka_offset>"` correspond au premier message Kafka stocké dans cette partition (le groupe d'objets avec ce préfixe). Par exemple, si un pont génère des objets avec des noms comme dans l'exemple suivant,
    `<object_a>` et `<object_b>` contiennent des messages avec des décalages compris entre  0 et 999, `<object_c>` contient des messages avec des décalages compris entre 1000 et 1999, etc.

    <pre class="pre"><code>
        ```
        &lt;bucket_name&gt;/offset=0/&lt;object_a&gt;
        &lt;bucket_name&gt;/offset=0/&lt;object_b&gt;
        &lt;bucket_name&gt;/offset=1000/&lt;object_c&gt;
        &lt;bucket_name&gt;/offset=2000/&lt;object_d&gt;
        ```       
    </code></pre>
    {:codeblock}
  
    
## Partitionnement par date ISO 8601
{: #partition_iso notoc}

Pour partitionner des données selon la date ISO 8601, procédez comme suit :

1. Configurez un pont avec la propriété `"inputFormat"` définie sur `"json"`. Vous
ne pouvez pas utiliser une propriété `"inputFormat"` autre que `"json"`.
2. Spécifiez un objet avec une propriété `"type"` définie sur la valeur `"dateIso8601"` et une propriété `"propertyName"` dans le tableau `"partitioning"`. 

	Exemple :
    <pre class="pre"><code>
    ```
    {
      "topic": "topic2",
      "type": "objectStorageOut",
      "name": "bridge2",
      "configuration" : {
        "credentials" : { ... },
        "bucket" : "bucket2",
        "inputFormat" : "json",
        "uploadDurationThresholdSeconds" : "1000",
        "uploadSizeThresholdKB" : "1000",
        "partitioning": [
          {
            "type": "dateIso8601",
            "propertyName": "timestamp"
          }
        ]
      }
    }
    ```
    </code></pre>
    {:codeblock}

	Le partitionnement par date ISO 8601 exige que les messages Kafka aient un format JSON valide. La
valeur de `"propertyName"` dans le JSON utilisé pour configurer le pont
doit correspondre à la zone de date ISO 	8601 dans chaque message Kafka. Dans cet exemple, la zone `"timestamp"` doit contenir une valeur de date ISO 8601
valide. Les messages sont ensuite partitionnés selon leurs dates.
	
	Un pont configuré comme dans cet exemple génère des objets avec des noms spécifiés comme suit :
	`<object_a>` contient des messages JSON avec des zones `"timestamp"` ayant
pour date 2016-12-07 tandis que `<object_b>` et `<object_c>` contiennent des messages JSON avec des zones `"timestamp"` ayant pour date
2016-12-08.

    <pre class="pre"><code>
        ```
        &lt;bucket_name&gt;/dt=2016-12-07/&lt;object_a&gt;
        &lt;bucket_name&gt;/dt=2016-12-08/&lt;object_b&gt;
        &lt;bucket_name&gt;/dt=2016-12-08/&lt;object_c&gt;
        ```       
    </code></pre>
    {:codeblock}

	Les données des messages au format JSON valide mais sans zone de date ou valeur valide sont écrites dans un objet dont le préfixe est `"dt=1970-01-01"`.

## Mesures du pont Cloud Object Storage
{: notoc}

Le pont Cloud Object Storage indique
des mesures que vous pouvez afficher sur votre tableau de bord à l'aide de Grafana. Les
mesures susceptibles de vous intéresser sont les suivantes :
<dl>
<dt><code>*.<var class="keyword varname">topicName</var>.<var class="keyword varname">bridgeName</var>.bytes-consumed-rate</code></dt>
<dd>Mesure le débit de consommation des données par le pont (en octets par seconde).</dd>
<dt><code>*.<var class="keyword varname">topicName</var>.<var class="keyword varname">bridgeName</var>.records-lag-max</code></dt>
<dd>Mesure le décalage maximal du nombre d'enregistrements consommés par le pont pour n'importe quelle partition du sujet. Si
cette valeur augmente dans le temps, cela signifie que le pont ne suit pas le rythme des auteurs du sujet.</dd>
</dl>
