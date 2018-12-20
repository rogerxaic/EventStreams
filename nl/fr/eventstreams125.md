---

copyright:
  years: 2015, 2018
lastupdated: "2018-09-11"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Signalement d'un problème à l'équipe {{site.data.keyword.messagehub}} - plan Enterprise
{: #report_problem}

Si vous rencontrez un problème lié à {{site.data.keyword.messagehub}}, consultez d'abord la [ page de statut {{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/status){:new_window}.

Si vous souhaitez obtenir de l'aide de la part de l'équipe {{site.data.keyword.messagehub}}, rassemblez toutes les informations suivantes. Plus vous fournirez d'informations à l'avance, plus l'équipe sera en mesure de résoudre le problème de manière efficace :
{:shortdesc}

1. Quel ID de nom de ressource de cloud du service {{site.data.keyword.messagehub}}
   utilisez-vous ?  Vous pouvez fournir cet ID en collant l'intégralité
   de l'URL de la console {{site.data.keyword.Bluemix_notm}} après avoir cliqué sur le
   service ou en collant la sortie de la commande d'interface de ligne de commande suivante :<br/>
   <pre class="pre">
   ibmcloud resource service-instance NAME
   </pre>
	{: codeblock}
2. Quand le problème s'est-il produit pour la première fois (date et heure précises, fuseau horaire) ?
   Depuis combien de temps votre application s'exécutait-elle ?
3. Le problème se produit-il encore ? Pouvez-vous le répliquer ?
4. Quel client Kafka votre application utilise-t-elle ? Quelle est sa version précise ?
5. Quels sont les détails de la configuration client ?
6. Disposez-vous de fragments de journaux qui affichent le problème ?
7. A quel problème êtes-vous confronté ? Quels sont les sujets, les ID client, les ID groupe et
   les ID transaction affectés ?
8. Quel impact le problème a-t-il sur votre service ?

Vous pouvez transmettre les informations rassemblées à IBM dans un ticket de demande de service en [soumettant une demande de support ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/get-support/howtogetsupport.html#open-ticket).










