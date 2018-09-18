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

# {{site.data.keyword.mql}}-API in {{site.data.keyword.messagehub}} verwenden
{: #mql_how}


Um die API zu verwenden, fügen Sie einen Verweis auf die neueste verfügbare {{site.data.keyword.mql}}-Client-API für die gewünschte Sprache wie folgt hinzu:

## Für Java
{: #mql_java_how notoc}

Fügen Sie den folgenden Verweis in Ihrer Datei <code>Maven pom</code> hinzu:

```
<dependency>
    <groupId>com.ibm.mqlight</groupId>
    <artifactId>mqlight-api</artifactId>
    <version>LATEST</version>
</dependency>
```
{:codeblock}



## Für Node.js
{: #mql_node_how notoc}

Fügen Sie den folgenden Verweis im Abschnitt für Abhängigkeiten in Ihrer Datei <code>package.json</code> hinzu:

<pre class="pre"><code>"mqlight" : "1.0.x"</code></pre>
{: codeblock}

Fügen Sie außerdem die folgende Anweisung 'require' in Ihrer Quellendatei hinzu:

<pre class="pre"><code>var mqlight = require(‘mqlight’);</code></pre>
{: codeblock}


## Für Ruby
{: #mql_ruby_how notoc}

Fügen Sie den folgenden Verweis in der Datei <code>Gem</code> hinzu:

```
gem 'mqlight', '~> 1.0'
```
{: codeblock}

<br>
Fügen Sie außerdem die folgende Anweisung 'require' in Ihrer Quellendatei hinzu:

```
require ‘mqlight’
```
{: codeblock}



## Für Python
{: #mql_python_how notoc}

Fügen Sie den folgenden Verweis in Ihrer Datei <code>requirements.txt</code>
hinzu:

```
git+git://github.com/mqlight/python-mqlight.git@readthedocs
```
{:codeblock}

<br>
Fügen Sie außerdem die folgende Importanweisung in Ihrer Quellendatei hinzu:

```
import mqlight
```
{:codeblock}


