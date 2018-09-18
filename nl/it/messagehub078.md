---

copyright:
  years: 2015, 2018
lastupdated: "2016-11-22"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Come utilizzare l'API {{site.data.keyword.mql}} in {{site.data.keyword.messagehub}}
{: #mql_how}


Per utilizzare l'API, aggiungi un riferimento all'ultima API client {{site.data.keyword.mql}} disponibile per il linguaggio da te scelto nel seguente modo:

## Per Java
{: #mql_java_how notoc}

Aggiungi il seguente riferimento al file <code>Maven pom</code>:

```
<dependency>
    <groupId>com.ibm.mqlight</groupId>
    <artifactId>mqlight-api</artifactId>
    <version>LATEST</version>
</dependency>
```
{:codeblock}



## Per Node.js
{: #mql_node_how notoc}

Aggiungi il seguente riferimento alla sezione delle dipendenze del tuo file <code>package.json</code>:

<pre class="pre"><code>"mqlight" : "1.0.x"</code></pre>
{: codeblock}

E aggiungi la seguente istruzione require al tuo file di origine:

<pre class="pre"><code>var mqlight = require(‘mqlight’);</code></pre>
{: codeblock}


## Per Ruby
{: #mql_ruby_how notoc}

Aggiungi il seguente riferimento al <code>Gemfile</code>:

```
gem 'mqlight', '~> 1.0'
```
{: codeblock}

<br>
E aggiungi la seguente istruzione require al tuo file di origine:

```
require ‘mqlight’
```
{: codeblock}



## Per Python
{: #mql_python_how notoc}

Aggiungi il seguente riferimento al file
<code>requirements.txt</code>:

```
git+git://github.com/mqlight/python-mqlight.git@readthedocs
```
{:codeblock}

<br>
E aggiungi la seguente istruzione import al tuo file di origine:

```
import mqlight
```
{:codeblock}


