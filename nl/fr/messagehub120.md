---

copyright:
  years: 2015, 2018
lastupdated: "2018-04-12"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Initiation au programme Alpha
{: #alpha_program}

Le programme Alpha offre un accès anticipé à la nouvelle version du service {{site.data.keyword.messagehub}}. 

Pour obtenir une application qui s'exécute avec le programme Alpha de {{site.data.keyword.messagehub}}, procédez comme suit :


## Création du service {{site.data.keyword.messagehub}}
{: alpha_create}


  1. Cliquez sur la vignette **Message Hub vNext - Production**, qui est un service expérimental du
[catalogue![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://console.stage1.bluemix.net/catalog/labs/?search=vnext).</li>

  2. Créez un service {{site.data.keyword.messagehub}}. Ce service fournit un cluster à service exclusif sous le plan de tarification <code>Premium</code> et sa mise à disposition prend généralement 1 à 3 heures.
 


## Obtention des données d'identification à l'aide de l'option de console de création et connexion d'une application de test
{: alpha_app}

Vous aurez besoin de données d'identification pour travailler avec {{site.data.keyword.messagehub}}.
Si vous ne disposez pas déjà d'une application utilisable, créez une application de test et utilisez les données d'identification de l'application pour vous connecter à {{site.data.keyword.messagehub}}. Par exemple, utilisez le service **SDK for Node.js** comme application de test. 

  1. Accédez à la vignette **SDK for Node.js** dans le [catalogue ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://console.stage1.bluemix.net/catalog/starters/sdk-for-nodejs).
   
  Avant d'entrer un nom d'application, vérifiez que vous avez sélectionné une région du sud des Etats-Unis. Créez l'application.

  2. Une fois l'application en cours d'exécution, cliquez sur l'onglet **Connexions** à gauche.

  3. Cliquez sur le bouton **Créer une connexion**.

  4. Sélectionnez votre nouveau service {{site.data.keyword.messagehub}} dans la liste des services compatibles existants, puis cliquez sur le bouton **Se connecter**.

  5. Dans la fenêtre **Connecter un service activé pour IAM**, acceptez les valeurs par défaut, puis cliquez sur **Connecter**.
  Assurez-vous que le service {{site.data.keyword.messagehub}} a été mis à disposition pour que vous puissiez vous y connecter.

  6. Cliquez sur l'onglet **Contexte d'exécution** à gauche et sélectionnez l'onglet **Variables d'environnement** au centre. Dans la section **VCAP_SERVICES**, repérez les informations <code>kafka_admin_url</code>, <code>apikey</code> et <code>kafka_brokers_sasl</code> dont vous aurez besoin pour pouvoir envoyer un message.
  
## Obtention des données d'identification à l'aide de l'option de ligne de commande
Vous pouvez également obtenir les données d'identification requises à l'aide de la ligne de commande. Procédez comme suit :

  1. Installez l'outil de ligne de commande {{site.data.keyword.Bluemix_notm}} à partir de l'[interface de ligne de commande {{site.data.keyword.Bluemix_notm}}![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/cli/index.html#overview).
  
  2. Connectez-vous à l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}.
  
  3. A l'aide de l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}, créez une clé de service avec le rôle de responsable via la commande suivante :
  ```
  bx resource service-key-create <NAME> Manager --instance-name <MESSAGEHUB_SERVICE_INSTANCE_NAME>
  ```
  4. La sortie de cette commande contient la clé d'API, l'URL de noeud final REST et la liste des courtiers. Vous pouvez afficher de nouveau ces informations en exécutant la commande suivante :
  ```
  bx resource service-key <NAME>
  ```

## Création d'un sujet {{site.data.keyword.messagehub}} et envoi d'un message

Vous pouvez utiliser une commande CURL pour créer un sujet, puis l'outil kafkacat pour générer et consommer un message. 

Pour chaque commande, remplacez APIKEY et KAFKA_ADMIN_URL par les valeurs issues de votre variable d'environnement VCAP_SERVICES.

  1. A partir de la ligne de commande, créez un sujet {{site.data.keyword.messagehub}} avec la commande CURL suivante :
  
    ```
    curl -i -X POST -H "Content-Type: application/json" -H "X-Auth-Token: APIKEY" --data '{ "name": "newtop"}' KAFKA_ADMIN_URL/admin/topics
    ```
    {: codeblock}

  2. Installez l'[outil kafkacat![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/edenhill/kafkacat#install), très utile pour tester rapidement Kafka.
  
  3. Pour les commandes qui suivent, vous avez besoin des informations ci-après :
  
    * Votre liste de courtiers, renvoyée dans le paramètre `kafka_brokers_sasl` des données d'identification. Cette liste doit être une liste séparée par des virgules pour kafkacat.
	Seuls vos cinq premiers courtiers sont répertoriés dans VCAP_SERVICES. Si vous avez plus de cinq courtiers, utilisez un client Kafka pour extraire les détails de vos autres courtiers. 
  
    * Votre clé d'API (<code>apikey</code>). Les 8 premiers caractères constituent votre nom d'utilisateur sasl.username et les autres caractères de <code>apikey</code> correspondent à votre mot de passe sasl.password.
    * L'emplacement de vos certificats SSL. Par exemple, sur Ubuntu, le répertoire des certificats SSL (SSL_CERTS_DIRECTORY) est <code>/etc/ssl/certs/</code>
  
  4. Générez quelques messages en exécutant une commande du type :
  ```
  kafkacat -X "security.protocol=sasl_ssl" -X 'sasl.mechanisms=PLAIN' -X 'sasl.username=<FIRST_8_CHARS_FROM_APIKEY>' -X 'sasl.password=<REMAINING_CHARS_FROM_APIKEY>' -X "ssl.ca.location=<SSL_CERTS_DIRECTORY>" -b <BROKERS_LIST> -P -t <TOPIC_NAME>
    ```
		
	<br/>
  Une fois la commande exécutée, vous pouvez entrer du texte, par exemple <code>HelloWorld</code>, sur le terminal du producteur.
  
  5. Consommez les messages en exécutant une commande du type :
  ```
  kafkacat -X "security.protocol=sasl_ssl" -X 'sasl.mechanisms=PLAIN' -X 'sasl.username=<FIRST_8_CHARS_FROM_APIKEY>' -X 'sasl.password=<REMAINING_CHARS_FROM_APIKEY>' -X "ssl.ca.location=<SSL_CERTS_DIRECTORY>" -b <BROKERS_LIST> -C -t <TOPIC_NAME> -f 'Topic %t [%p] at offset %o: key %k: %s\n'
  ```
	
	<br/>
  <code>HelloWorld</code> doit s'afficher sur le terminal du consommateur.

