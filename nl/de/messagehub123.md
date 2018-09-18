---

copyright:
  years: 2015, 2018
lastupdated: "2018-04-19"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.iamshort}} (IAM) mit dem Alpha-Programm verwenden
{: #alpha_iam }

{{site.data.keyword.messagehub}}-Berechtigungen werden mit IAM-Richtlinien konfiguriert. Eine IAM-Richtlinie besteht aus den folgenden Informationen:

* Service-ID
* {{site.data.keyword.Bluemix_short}}-Ressource, die durch einen Servicenamen, eine Serviceinstanz, eine Region, einen IAM-Ressourcentyp und eine IAM-Ressource definiert ist
* Rolle (Leseberechtigter, Schreibberechtigter oder Manager)

Weitere Informationen zu IAM finden Sie unter:
[IBM Cloud Identity and Access Management](/docs/iam/index.html#iamoverview).

Ein Beispiel zum Festlegen von Richtlinien finden Sie unter:
[IBM Cloud IAM Service-IDs und API-Schlüssel einführen ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/blogs/bluemix/2017/10/introducing-ibm-cloud-iam-service-ids-api-keys/){:new_window}.

## {{site.data.keyword.messagehub}}-IAM-Ressourcen
{: iam_resources}

{{site.data.keyword.messagehub}} stellt vier IAM-Ressourcentypen bereit:

* cluster
* topic
* group
* txnid

Sie können eindeutige Ressourcen für "topic", "group" und "txnid" angeben, indem Sie die IAM-Ressource festlegen. Der Ressourcentyp "cluster" ist ein Singleton und muss daher nicht eindeutig angegeben werden..

## Allgemeine Szenarios
{: iam_scenarios}

Hier finden Sie einige allgemeine {{site.data.keyword.messagehub}}-Szenarios und den Zugriff, die Sie für eine Zuordnung benötigen:

Producer für einige Themen (identisch mit idempotent):
* Rolle des Leseberechtigten für den Cluster-Ressourcentyp
* Rolle des Schreibberechtigten für jeden Themen-Ressourcentyp und jede Themen-Namenressource

Transaktionsorientierter Producer:
* Wie Producer
* Rolle des Schreibberechtigten für den Ressourcentyp "txnid" und die Transaktions-ID-Ressource
* Rolle des Leseberechtigten für den Ressourcentyp "group" und die Gruppen-ID-Ressource

Consumer (ohne Gruppe):
* Rolle des Leseberechtigten für den Cluster-Ressourcentyp
* Rolle des Leseberechtigten für jeden Themen-Ressourcentyp und jede Themen-Namenressource

Consumer mit Gruppe:
* Wie Consumer (ohne Gruppe)
* Rolle des Leseberechtigten für den Ressourcentyp "group" und die Gruppen-ID-Ressource

Topic erstellen oder löschen:
* Rolle des Leseberechtigten für den Cluster-Ressourcentyp
* Rolle für Managementaufgaben für jeden Themen-Ressourcentyp und jede Themen-Namenressource

Consumer-Gruppe löschen:
* Rolle des Leseberechtigten für den Cluster-Ressourcentyp
* Rolle für Managementaufgaben für den Ressourcentyp "group" und die Gruppen-ID-Ressource

Gruppen, Themen und Offsets auflisten und Gruppe, Topic und Broker-Konfiguration beschreiben
* Rolle des Leseberechtigten für den Cluster-Ressourcentyp














