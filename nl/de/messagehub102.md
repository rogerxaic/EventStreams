---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# MQ Light-Node.js-Client verwenden 
{: #mql_node}

** Die MQ Light-API ist nur als Bestandteil des Plans "Standard" verfügbar.**
<br/>

Um die API zu verwenden, fügen Sie einen Verweis auf die neueste verfügbare {{site.data.keyword.mql}}-Client-API für Node.js wie folgt hinzu:

Fügen Sie den folgenden Verweis im Abschnitt für Abhängigkeiten in Ihrer Datei <code>package.json</code> hinzu:

<pre class="pre"><code>"mqlight" : "1.0.x"</code></pre>
{: codeblock}

Fügen Sie außerdem die folgende Anweisung 'require' in Ihrer Quellendatei hinzu:

<pre class="pre"><code>var mqlight = require(‘mqlight’);</code></pre>
{: codeblock}

<!-- Comment from Andrew
Instructions for getting started, with links for more info
Simple send source and receive source in-line

-->


