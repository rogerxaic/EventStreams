---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-15"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}

# Upgrade auf den neuen {{site.data.keyword.messagehub}}-Plan "Standard" 
{: #migrate_classic_plan}

Diese neue Version des Multi-Tenant-Plans "Standard" bietet erhebliche Verbesserungen bezüglich Ausfallsicherheit, Funktionalität und Benutzerfreundlichkeit. Weitere Informationen finden Sie in der [Blogankündigung für neuen Plan "Standard" ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/cloud/blog/announcements/ibm-event-streams-releases-a-new-and-enhanced-standard-plan).
{: shortdesc}

Wenn Sie Anwendungen aus dem vorherigen Plan "Standard" (jetzt als Plan "Classic" bezeichnet) in den neuen Plan migrieren möchten, berücksichtigen Sie die folgenden Informationen.

Serviceinstanzen werden jetzt als {{site.data.keyword.cloud_notm}}-Services und nicht als Cloud Foundry-Services bereitgestellt. Dies ermöglicht dem Service die Unterstützung der neuesten {{site.data.keyword.cloud_notm}}-Standards und -Funktionalitäten einschließlich Mehrzonenimplementierungen und differenzierte Zugriffssteuerungen, was aber Auswirkungen auf die Verwendung des Service hat. Dabei sind insbesondere folgende Aspekte zu berücksichtigen:

* **Services erstellen, löschen und auflisten**
<br/>
    Wie zuvor können Sie den Lebenszyklus von Services über die {{site.data.keyword.cloud_notm}}-Konsole oder über die {{site.data.keyword.cloud_notm}}-Befehlszeilenschnittstelle verwalten. Wenn Sie die Konsole verwenden, werden Services jetzt unter **Services** statt **Cloud Foundry Services** aufgelistet. 
    
    Wenn Sie die Befehlszeilenschnittstelle verwenden, werden Instanzen mithilfe der Ressourcenbefehle verwaltet. Beispiel: <code>ibmcloud resource service-instance-create</code>. Diese treten an die Stelle der **cf**-Befehle, z. B. <code>ibmcloud cf create-service</code>.

* **Zugriff steuern**
<br/>
    Authentifizierung und Autorisierung werden jetzt mit dem Cloud IAM-Service (IAM - Identity and Access Management) verwaltet. IAM ermöglicht das Steuern der Verbindungsherstellung eines Benutzers und gibt Ihnen die Möglichkeit, einen differenzierten Zugriff auf zu Grunde liegende Ressourcen zu konfigurieren, wie z. B. Topics. Der Zugriff wird gesteuert, indem Benutzern Richtlinien zugeordnet werden. Weitere Informationen finden Sie in [Zugriff auf Ihre {{site.data.keyword.messagehub}}-Ressourcen verwalten](/docs/services/EventStreams?topic=eventstreams-security).

<ul>
<li><strong>Anwendungen verbinden</strong>
<br/>
    Die Informationen, die eine Anwendung benötigt, um eine Verbindung herzustellen, haben sich nicht geändert, d. h. eine Liste mit Werten für <code>bootstrap.servers</code>, <code>username</code> und <code>password</code> ist erforderlich. Die Art und Weise, wie diese Eigenschaften abgerufen werden, hat sich jedoch geändert.

<ul>
<li>
      <strong>Bei nativen Anwendungen</strong>
        <br/>
        Sie müssen ein Berechtigungsnachweis- oder Serviceschlüsselobjekt mithilfe der IBM Cloud-Konsole oder -Befehlszeilenschnittstelle erstellen. Anschließend können Sie die erforderlichen Eigenschaften abrufen. Weitere Informationen finden Sie in [Anwendungen verbinden](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_external).
</li>
<br/>
<li><strong>Bei Cloud Foundry-Anwendungen</strong>
        <br/>
        Sie müssen den Service zunächst an die Organisation und den Bereich der Anwendung binden, indem Sie einen Servicealias erstellen. Anschließend können Sie die erforderlichen Eigenschaften wie gewohnt aus der Umgebungsvariablen VCAP_SERVICES abrufen. Weitere Informationen finden Sie in
        [Mit {{site.data.keyword.messagehub}} verbinden](/docs/services/EventStreams?topic=eventstreams-connecting).
</li>
</ul>
<br/>
Beachten Sie, dass Clients die SNI-Erweiterung für TLS unterstützen müssen, bei der der Hostname des Servers im TLS-Handshake einbezogen wird. Diese Funktion ist allgemein verfügbar und wird in allen Clientversionen unterstützt, die im Abschnitt [Kafka-Client für die Verwendung mit {{site.data.keyword.messagehub}} auswählen](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_clients) empfohlen werden.</li>
</ul>

<br/>
Beachten Sie auch die folgenden Änderungen:

* **Kafka-Version**
<br/>
    Dieser Plan bietet Zugriff auf das neueste stabile Kafka-Release 2.2. Anwendungen, die für Kafka 1.1 entwickelt wurden, können unverändert ausgeführt werden, aber beachten Sie die folgenden Informationen zu empfohlenen Clientversionen und getesteten Kombinationen: [Kafka-Client für die Verwendung mit {{site.data.keyword.messagehub}} auswählen](/docs/services/EventStreams?topic=eventstreams-kafka_using#kafka_clients). 

* **Unterstützte Regionen**
<br/>
    Der Plan ist zunächst nur in us-south verfügbar. Die übrigen Regionen werden in den nächsten Wochen eingeführt.

* **Integrationen**
<br/>
    Für Verbindungen von anderen Services, z. B. {{site.data.keyword.iot_short_notm}} oder {{site.data.keyword.openwhisk_short}}, die mithilfe von Cloud Foundry-Organisation und -Bereich an den Service gebunden werden, ist die Erstellung eines Servicealias erforderlich. Weitere Informationen finden Sie in
    [Cloud Foundry-Anwendungen mit {{site.data.keyword.messagehub}} verbinden](/docs/services/EventStreams?topic=eventstreams-connecting#connect_enterprise_cf).
    

* **Unterstützte Leistungsmerkmale**
<br/>
    Es gibt Unterschiede zwischen den Leistungsmerkmalen des Plans "Classic" und dem neuen Plan "Standard". Um die Produktangebote abzustimmen, neue Technologieoptionen zu übernehmen und weniger verwendete Funktionen zu entfernen, werden nicht alle Leistungsmerkmale fortgeführt. Ein Vergleich der Funktionen ist unter [Plan auswählen](/docs/services/EventStreams?topic=eventstreams-plan_choose) verfügbar. Wenn Sie auf diese Funktionen angewiesen sind, werden Ihnen in Kürze weitere Informationen bereitgestellt, die Sie bei der Migration unterstützen.
   
<br/>
Kleine Codedeltas werden täglich in die Produktion geliefert. Deshalb können Sie im weiteren Verlauf des Jahres 2019 und darüber hinaus zahlreiche weitere Verbesserungen bei der Attraktivität für den Benutzer (und auch in anderen Bereichen) erwarten. In Kürze verfügbar:

* **Kundenmetriken**
<br/>
    Die Möglichkeit, die Aktivität in einer Serviceinstanz zu überwachen.

<br/>
Wenn Sie kurz die Schritte durchspielen möchten, die für die Herstellung der Betriebsbereitschaft des neuen Plans "Standard" erforderlich sind, beschäftigen Sie sich mit dem [Lernprogramm zur Einführung](/docs/services/EventStreams?topic=eventstreams-getting_started).


