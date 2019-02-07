---

copyright:
  years: 2015, 2019
lastupdated: "2018-05-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

<!-- 12/11/18: info moved to eventstreams099.md, moved because of doc app changes -->
# Utilizzo del client Node.js MQ Light 
{: #mql_node}

** L'API MQ Light è disponibile solo come parte del piano Standard.**
<br/>

Per utilizzare l'API, aggiungi un riferimento all'ultima API client {{site.data.keyword.mql}} disponibile per Node.js nel seguente modo:
{: shortdesc}

Aggiungi il seguente riferimento alla sezione delle dipendenze del tuo file <code>package.json</code>:

<pre class="pre"><code>"mqlight" : "1.0.x"</code></pre>
{: codeblock}

E aggiungi la seguente istruzione require al tuo file di
origine:

<pre class="pre"><code>var mqlight = require(‘mqlight’);</code></pre>
{: codeblock}

<!-- Comment from Andrew
Instructions for getting started, with links for more info
Simple send source and receive source in-line

-->


