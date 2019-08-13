---

copyright:
  years: 2015, 2019
lastupdated: "2018-09-11"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}



# Signalement d'un problème à l'équipe {{site.data.keyword.messagehub}} - Plan Classic 
{: #report_problem}

Si vous rencontrez un problème en relation avec le plan Classic de {{site.data.keyword.messagehub}}, consultez d'abord la [page de statut d'{{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/status?selected=status){:new_window}. 

Si vous souhaitez obtenir de l'aide de la part de l'équipe {{site.data.keyword.messagehub}}, rassemblez toutes les informations suivantes. Plus vous fournirez d'informations à l'avance, plus l'équipe sera en mesure de résoudre le problème de manière efficace :
{:shortdesc}

1. Dans quel emplacement (région) {{site.data.keyword.Bluemix_notm}} votre instance de {{site.data.keyword.messagehub}} est-elle mise à disposition ?  Par exemple, Dallas ou Londres. 
2. Avec quelle interface rencontrez-vous des problèmes ? Avec Kafka, REST, AMQP ou les ponts ?
3. Quand le problème s'est-il produit pour la première fois (date et heure précises, fuseau horaire) ? Depuis combien de temps votre application s'exécutait-elle ?
4. Le problème se produit-il encore ? Pouvez-vous le répliquer ?
5. Quel ID instance du service {{site.data.keyword.messagehub}} utilisez-vous ? 
Cet ID figure dans la zone "instance_id" dans le JSON VCAP_SERVICES du service. Par exemple :
 ```
 "instance_id": "e662a61b-d1ff-4cce-aab9-5eae9adb9827"
 ```
6. Quel client votre application utilise-t-elle ? Kafka ou REST ? Quelle est sa version précise ?
7. Quels sont les détails de la configuration client ?
8. Disposez-vous de fragments de journaux qui affichent le problème ?
9. A quel problème êtes-vous confronté ? Quels sont les sujets, les ID client et les ID groupe affectés ?
10. Quel impact le problème a-t-il sur votre service ?


Vous pouvez transmettre les informations rassemblées à IBM dans un ticket de demande de service en [soumettant une demande de support ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/get-support?topic=get-support-getting-customer-support#using-avatar){:new_window}.










