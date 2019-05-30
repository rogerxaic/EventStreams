---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-14"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}
{:important: .important}

# Zugriff auf Ihre {{site.data.keyword.messagehub}}-Ressourcen verwalten 
{: #security }

Sie können Ihre {{site.data.keyword.messagehub}}-Ressourcen differenziert schützen, um den Zugriff zu verwalten, den Sie jedem Benutzer für die einzelnen Ressourcen erteilen möchten.
{: shortdesc}

Wenn Sie Änderungen an IAM-Richtlinien und -Berechtigungen vornehmen, kann es einige Minuten dauern, bis diese im zugrunde liegenden Service umgesetzt werden.
{: important}

## Welche Ressourcen kann ich schützen?

In {{site.data.keyword.messagehub}} können Sie den Zugriff auf die folgenden Ressourcen schützen:
* Cluster (cluster): Sie können steuern, welche Anwendungen und Benutzer eine Verbindung zum Service herstellen können
* Topics (topic): Sie können steuern, ob es Benutzern und Anwendungen möglich ist, Topics zu erstellen, zu löschen, zu lesen und zu schreiben 
* Consumergruppen (group): Sie können die Fähigkeit einer Anwendung zum Herstellen einer Verbindung zu einer Consumergruppe steuern 
* Producertransaktionen (txnid): Sie können die Fähigkeit zum Verwenden der transaktionsorientierten Producerfunktion in Kafka steuern (das heißt einzelne, atomare Schreiboperationen über mehrere Partitionen)

Nachfolgend sind die Zugriffsebenen (auch bekannt als Rollen) beschrieben, die Sie einem Benutzer für die einzelnen Ressourcen zuweisen können:

| Zugriffsrolle | Beschreibung der Aktionen | Beispielaktionen |
|:-----------------|:-----------------|:-----------------|
|  Leseberechtigter | Schreibgeschützte Aktionen in {{site.data.keyword.messagehub}} durchführen, z. B. Ressourcen anzeigen | Einer App das Herstellen einer Verbindung zu einem Cluster ermöglichen durch Zuweisen von Lesezugriff zum Ressourcentyp "cluster" |
| Schreibberechtigter | Schreibberechtigte verfügen über Berechtigungen, die über die der Rolle des Leseberechtigten hinausgehen, wie z. B. die Berechtigung zum Erstellen und Bearbeiten von {{site.data.keyword.messagehub}}-Ressourcen. | Einer App das Senden von Nachrichten an Topics ermöglichen durch das Zuweisen von Schreibzugriff auf Topic-Ressourcentypen und Topic-Namenstypen|
| Manager | Manager verfügen über Berechtigungen, die über die der Rolle des Schreibberechtigten hinausgehen und mit denen sie privilegierte Aktionen durchführen können. Darüber hinaus können sie {{site.data.keyword.messagehub}}-Ressourcen erstellen und bearbeiten. | Uneingeschränkten Zugriff auf alle Ressourcen ermöglichen durch Zuweisen von Verwaltungszugriff für die {{site.data.keyword.messagehub}}-Instanz|
{: caption="Tabelle 1. {{site.data.keyword.messagehub}}-Beispielbenutzerrollen und -aktionen" caption-side="top"}

<!-- comment from Charlie and my reply 
CM: need to confirm if hierarchical e.g. write includes read - and doc. 
KR: I think they do inherit the lower level access https://console.bluemix.net/docs/iam/users_roles.html#iamusermanrol 
-->


## Wie weise ich Zugriff zu?

An die zu steuernden Ressourcen sind Cloud-Richtlinien für das Identitäts- und Zugriffsmanagement (Identity and Access Management: IAM) angehängt. Jede Richtlinie definiert die Zugriffsebene, über die ein bestimmter Benutzer verfügen soll und die Ressource oder Gruppe von Ressourcen, auf die der Zugriff bestehen soll. Eine Richtlinie besteht aus den folgenden Informationen: 
* Dem Typ von Service, auf den die Richtlinie angewendet wird. Beispiel: {{site.data.keyword.messagehub}}. Sie können für eine Richtlinie einen Geltungsbereich definieren, der alle Servicetypen einschließt. 
* Der Instanz des zu schützenden Service. Sie können für eine Richtlinie einen Geltungsbereich definieren, der alle Instanzen eines Servicetyps einschließt. 
* Dem Typ der zu schützenden Ressource. Die gültigen Werte sind <code>cluster</code>, <code>topic</code>, <code>group</code> oder <code>txnid</code>. Das Angeben eines Typs ist optional. Falls Sie keinen Typ angeben, gilt die Richtlinie für alle Ressourcen in der Serviceinstanz. 
* Der zu schützenden Ressource. Geben Sie diese bei Ressourcen vom Typ <code>topic</code>, <code>group</code> und <code>txnid</code> an. Falls Sie die Ressource nicht angeben, gilt die Richtlinie für alle Ressourcen des Typs, der in der Serviceinstanz angegeben ist. 
* Der dem Benutzer zugewiesenen Rolle. Beispiel: Leseberechtigter, Schreibberechtigter oder Manager. 

## Was sind die Standardsicherheitseinstellungen?

Standardmäßig wird bei der Bereitstellung von {{site.data.keyword.messagehub}} dem Benutzer, der die Bereitstellung durchführte, die Managerrolle für alle Ressourcen der Instanz zugewiesen. Darüber hinaus hat auch jeder Benutzer, der über eine Managerrolle für alle Services oder alle {{site.data.keyword.messagehub}}-Serviceinstanzen in demselben Konto verfügt, uneingeschränkten Zugriff. 

Sie können außerdem zusätzliche Richtlinien anwenden, um den Zugriff auf andere Benutzer zu erweitern. Sie können den Geltungsbereich einer Richtlinie entweder so definieren, dass sie für {{site.data.keyword.messagehub}} als Ganzes gilt oder für einzelne Ressourcen innerhalb von {{site.data.keyword.messagehub}}. Weitere Informationen finden Sie im Abschnitt [Allgemeine Szenarios](#security_scenarios).

Nur Benutzer mit einer Verwaltungsrolle für ein Konto können Benutzern Richtlinien zuweisen. Sie können Richtlinien entweder über das IBM Cloud-Dashboard oder mithilfe des Befehls **ibmcloud** zuweisen. 
<!--
For example steps for {{site.data.keyword.messagehub}}, see [Examples](#security_examples).
-->


## Allgemeine Szenarios
{: #security_scenarios }

In der nachfolgenden Tabelle finden Sie eine Zusammenfassung einiger allgemeiner {{site.data.keyword.messagehub}}-Szenarios und der Zugriffsebenen, die zugewiesen werden müssen:

| Aktion | Rolle 'Leseberechtigter' | Rolle 'Schreibberechtigter' | Rolle 'Manager' |
|---------|----------------|
| Uneingeschränkten Zugriff auf alle Ressourcen ermöglichen|Nicht zutreffend   |Nicht zutreffend  |Serviceinstanz: <var class="keyword varname">your_service_instance</var>|
| Einer App oder einem Benutzer ermöglichen, ein Topic zu erstellen oder zu löschen |Ressourcentyp: <code>cluster</code>   |Nicht zutreffend  |Ressourcentyp: topic <br/><br/>Optional: Ressourcen-ID: <var class="keyword varname">Topicname</var> |
| Gruppen, Topics und Offsets auflisten <br/> Gruppen-, Topic- und Brokerkonfigurationen beschreiben | Ressourcentyp: <code>cluster</code>      |Nicht zutreffend  |Nicht zutreffend      |
| Einer App das Herstellen einer Verbindung zum Cluster ermöglichen  |Ressourcentyp: <code>cluster</code>| Nicht zutreffend     |Nicht zutreffend      |
| Einer App ermöglichen, Nachrichten an ein beliebiges Topic zu senden  |Ressourcentyp: <code>cluster</code>|Ressourcentyp: <code>topic</code> |Nicht zutreffend     |
| Einer App ermöglichen, Nachrichten an ein bestimmtes Topic zu senden  |Ressourcentyp: <code>cluster</code>|Ressourcentyp: <code>topic</code><br/>Ressourcen-ID: <var class="keyword varname">Topicname</var>      |Nicht zutreffend     |
| Einer App ermöglichen, eine Verbindung von einem beliebigen Topic herzustellen und Nachrichten von diesem zu verarbeiten (keine Consumergruppe)  |Ressourcentyp: <code>cluster</code> <br/>Ressourcentyp: <code>topic</code> |Nicht zutreffend    |Nicht zutreffend     |
| Einer App ermöglichen, eine Verbindung von einem bestimmten Topic herzustellen und Nachrichten von diesem zu verarbeiten (keine Consumergruppe)  | Ressourcentyp: <code>cluster</code> <br/>Ressourcentyp: <code>topic</code><br/>Ressourcen-ID: <var class="keyword varname">Topicname</var> |Nicht zutreffend     |Nicht zutreffend     |
| Einer App ermöglichen, ein Topic zu verarbeiten (Consumergruppe)  |Ressourcentyp: <code>cluster</code> <br/>Ressourcentyp: <code>topic</code><br/> Ressourcentyp: <code>group</code> |Nicht zutreffend      |Nicht zutreffend     |
| Einer App ermöglichen, Nachrichten transaktionsorientiert an ein Topic zu senden  |Ressourcentyp: <code>cluster</code> <br/> Ressourcentyp: <code>group</code>|Ressourcentyp:  <code>topic</code> <br/>Ressourcen-ID: <var class="keyword varname">Topicname</var> <br/>Ressourcentyp: <code>txnid</code> |Nicht zutreffend     |
| Consumergruppe löschen |Ressourcentyp: <code>cluster</code> |Nicht zutreffend  |Ressourcentyp: <code>group</code> <br/>Ressourcen-ID: <var class="keyword varname">Gruppen-ID</var>      |
| Verwendung von Streams |Ressourcentyp: <code>cluster</code></br>Ressourcentyp: <code>group</code>| Nicht zutreffend  |Ressourcentyp: <code>topic</code>    |

Weitere Informationen zu IAM finden Sie unter
[IBM Cloud Identity and Access Management](/docs/iam?topic=iam-iamoverview#iamoverview).

Ein Beispiel zum Festlegen von Richtlinien finden Sie unter:
[IBM Cloud IAM Service IDs and API Keys ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/blogs/bluemix/2017/10/introducing-ibm-cloud-iam-service-ids-api-keys/){:new_window}.


## Verbindung zu {{site.data.keyword.messagehub}} herstellen
{: #connect_message_enterprise }

Informationen zum Binden einer Cloud Foundry-Anwendung oder Abrufen eines Berechtigungsnachweises mittels Sicherheitsschlüssel für eine externe Anwendung finden Sie in
[Verbindung zu {{site.data.keyword.messagehub}} herstellen](/docs/services/EventStreams?topic=eventstreams-connecting).

<!-- 28/06/18 - Karen: draft info only

## Examples
{: #security_examples }

I want to give a user access to create or delete a topic:

1. From the IBM Cloud dashboard, go to the **Manage** tab &gt; **Security** &gt; **Identity and Access**, and then select **Users**.
2. Click **Invite users**.
3. Specify the email address of the user that you want to invite.
4. In the **Access** section, expand the **Services** option.
5. Choose to assign access to a **Resource**.
6. In the **Services** section, select **{{site.data.keyword.messagehub}}**
7. In the **Region** section, make your selection.
8. In the **Service instance** section, locate your instance and select it.
9. In the **Resource type** section, enter **cluster**.
10. In the **Select roles** section, check the **Reader** box.
11. In the **Resource type** section, enter **topic**.
12. In the **Select roles** section, check the **Manager** box.
13. Click **Invite users**.

-->















