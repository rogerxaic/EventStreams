---

copyright:
  years: 2015, 2018
lastupdated: "2018-06-01"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Pont Object Storage 
{: #object_storage_bridge }

**Des ponts {{site.data.keyword.messagehub}} sont disponibles dans le cadre du plan Standard uniquement.**
<br/>

Le pont {{site.data.keyword.objectstorageshort}} vous permet d'archiver des données provenant des sujets Kafka de {{site.data.keyword.messagehub}} dans une instance du [service {{site.data.keyword.objectstorageshort}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/services/ObjectStorage/index.html){:new_window} {{site.data.keyword.Bluemix_short}}. Le
pont consomme des lots de messages issus de Kafka et transfère les données des messages sous forme d'objets vers un conteneur du service {{site.data.keyword.objectstorageshort}}.

Le service de stockage d'objets préféré dans {{site.data.keyword.Bluemix_short}} est désormais le service [{{site.data.keyword.IBM_notm}} Cloud
Object Storage. ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/services/cloud-object-storage/about-cos.html){:new_window}.

En
configurant le pont {{site.data.keyword.objectstorageshort}}, vous pouvez contrôler le mode de transfert des données sous forme d'objets vers {{site.data.keyword.objectstorageshort}}. Les
propriétés que vous pouvez configurer sont les suivantes :

* Le nom du conteneur dans lequel les objets sont inscrits.
* La fréquence de transfert des objets vers le service {{site.data.keyword.objectstorageshort}}.
* La quantité de données écrites dans chaque objet avant son transfert vers le service {{site.data.keyword.objectstorageshort}}.

Le format de sortie du pont est un objet de service de stockage d'objets contenant un ou plusieurs enregistrements concaténés avec des caractères de retour à la ligne qui font office de séparateurs.

## Transfert des données via le pont {{site.data.keyword.objectstorageshort}}
{: notoc}

Le pont {{site.data.keyword.objectstorageshort}} lit un certain nombre d'enregistrements Kafka à partir d'un sujet et écrit les données de ces enregistrements dans un objet. Cet
objet est transféré vers une instance du service {{site.data.keyword.objectstorageshort}}. Chaque
pont {{site.data.keyword.objectstorageshort}} lit les données des messages à partir d'un seul sujet Kafka, même si un sujet peut être lu par plusieurs ponts à
la fois. Une nouvelle instance du pont {{site.data.keyword.objectstorageshort}}
commence toujours sa lecture par la position la moins récente du sujet Kafka. Le pont
{{site.data.keyword.objectstorageshort}} utilise la gestion des positions de
consommateur de Kafka pour transférer les données de manière fiable depuis Kafka sans
perte, mais avec un faible risque de duplication.

Vous pouvez contrôler le nombre d'enregistrements lus depuis Kafka avant
l'écriture des données dans l'instance de service
{{site.data.keyword.objectstorageshort}} à l'aide des propriétés ci-après. Indiquez
ces propriétés lorsque vous créez ou mettez à jour un pont :
<dl><dt>`"uploadDurationThresholdSeconds"`</dt> 
<dd>Définit une durée en secondes au bout de laquelle les données accumulées à partir de Kafka
sont transférées vers le service {{site.data.keyword.objectstorageshort}}.</dd>
<dt>`"uploadSizeThresholdKB"`</dt>
<dd>Contrôle la quantité de données en kilooctets accumulées à partir de Kafka avant leur
transfert vers le service {{site.data.keyword.objectstorageshort}}.</dd>
</dl>

Le transfert des données lues à partir de Kafka par le pont {{site.data.keyword.objectstorageshort}} vers le service
{{site.data.keyword.objectstorageshort}} est déclenché lorsque l'une de ces valeurs est atteinte. Le
pont {{site.data.keyword.objectstorageshort}} ne garantit pas le transfert des données vers le service {{site.data.keyword.objectstorageshort}}
exactement au moment où le premier point de ces seuils est atteint. Les données
transférées peuvent ainsi arriver plus tard ou être plus volumineuses que ce qui est indiqué par les valeurs de ces propriétés.

Le pont {{site.data.keyword.objectstorageshort}} concatène les messages en utilisant des caractères de retour à la ligne comme séparateurs lorsqu'il écrit les
données dans {{site.data.keyword.objectstorageshort}}. Du fait de ces
séparateurs, le pont ne convient ni aux messages contenant des caractères de retour à la ligne imbriqués, ni aux données de message binaires.

## Partitionnement des données en objets par le pont {{site.data.keyword.objectstorageshort}}
{: notoc}

Le pont {{site.data.keyword.objectstorageshort}} offre une fonctionnalité
permettant de partitionner les messages Kafka et de les stocker sous forme d'objets nommés avec un préfixe commun. Un
groupe d'objets nommés avec un préfixe commun s'appelle une partition. Le
partitionnement du pont {{site.data.keyword.objectstorageshort}} est différent du partitionnement Kafka.

A chaque fois que le pont {{site.data.keyword.objectstorageshort}} dispose d'un lot de données à transférer vers le service
{{site.data.keyword.objectstorageshort}}, il crée un ou plusieurs objets contenant les données. La
décision quant au mode de partitionnement des messages et de nommage des objets qui en
résultent dépend de la configuration du pont. Les noms d'objet peuvent contenir des
métadonnées Kafka et éventuellement des données provenant des messages eux-mêmes. Actuellement,
le pont prend en charge les deux modes de partitionnement suivants des messages Kafka
en objets {{site.data.keyword.objectstorageshort}} :

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
            "container" : "container1",
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

    Les noms d'objet générés par un pont configuré de cette manière contiennent
le préfixe `"offset=<kafka_offset>"`, où `"<kafka_offset>"`
correspond au premier message Kafka stocké dans cette partition (le groupe d'objets dotés de ce préfixe). Par
exemple, si un pont génère des objets nommés comme indiqué ci-après, `<object_a>`
et `<object_b>` contiennent les messages dont les positions figurent
dans la plage 0 - 999, `<object_c>` contient les messages dont les
positions sont comprises dans la plage 1000 - 1999, et ainsi de suite.

    <pre class="pre"><code>
        ```
        &lt;container_name&gt;/offset=0/&lt;object_a&gt;
        &lt;container_name&gt;/offset=0/&lt;object_b&gt;
        &lt;container_name&gt;/offset=1000/&lt;object_c&gt;
        &lt;container_name&gt;/offset=2000/&lt;object_d&gt;
        ```       
    </code></pre>
    {:codeblock}
  
    
## Partitionnement par date ISO 8601
{: notoc}

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
        "container" : "container2",
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
	
	Un pont configuré comme dans cet exemple génère des objets dont les noms sont spécifiés comme suit :
	`<object_a>` contient les messages JSON dont les zones
`"timestamp"` indiquent la date 2016-12-07, et `<object_b>`
et `<object_c>` contiennent les messages JSON dont les zones `"timestamp"` indiquent la date
	2016-12-08.

    <pre class="pre"><code>
        ```
        &lt;container_name&gt;/dt=2016-12-07/&lt;object_a&gt;
        &lt;container_name&gt;/dt=2016-12-08/&lt;object_b&gt;
        &lt;container_name&gt;/dt=2016-12-08/&lt;object_c&gt;
        ```       
    </code></pre>
    {:codeblock}

	Les données des messages au format JSON valide mais sans zone de date ou valeur valide sont écrites dans un objet dont le préfixe est `"dt=1970-01-01"`.

## Mesures du pont {{site.data.keyword.objectstorageshort}}
{: notoc}

Le pont {{site.data.keyword.objectstorageshort}} indique des mesures que vous pouvez afficher sur votre tableau de bord à l'aide de Grafana. Les
mesures susceptibles de vous intéresser sont les suivantes :
<dl>
<dt><code>*.<var class="keyword varname">topicName</var>.<var class="keyword varname">bridgeName</var>.bytes-consumed-rate</code></dt>
<dd>Mesure le débit de consommation des données par le pont (en octets par seconde).</dd>
<dt><code>*.<var class="keyword varname">topicName</var>.<var class="keyword varname">bridgeName</var>.records-lag-max</code></dt>
<dd>Mesure le décalage maximal du nombre d'enregistrements consommés par le pont pour n'importe quelle partition du sujet. Si
cette valeur augmente dans le temps, cela signifie que le pont ne suit pas le rythme des auteurs du sujet.</dd>
</dl>
