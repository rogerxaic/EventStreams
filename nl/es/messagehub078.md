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

# Cómo utilizar la API {{site.data.keyword.mql}} en {{site.data.keyword.messagehub}}
{: #mql_how}


Para utilizar la API, añada una referencia a la última API de cliente {{site.data.keyword.mql}} disponible para el lenguaje elegido de la siguiente manera:


## Java
{: #mql_java_how notoc}

Añada la siguiente referencia al archivo <code>Maven</code>:

```
<dependency>
    <groupId>com.ibm.mqlight</groupId>
    <artifactId>mqlight-api</artifactId>
    <version>LATEST</version>
</dependency>
```
{:codeblock}



## Node.js
{: #mql_node_how notoc}

Añada la siguiente referencia a la sección dependencia del archivo <code>package.json</code>:

<pre class="pre"><code>"mqlight" : "1.0.x"</code></pre>
{: codeblock}

A continuación, añada la siguiente sentencia necesaria al archivo de origen:

<pre class="pre"><code>var mqlight = require(‘mqlight’);</code></pre>
{: codeblock}


## Ruby
{: #mql_ruby_how notoc}

Añada la siguiente referencia a <code>Gemfile</code>:

```
gem 'mqlight', '~> 1.0'
```
{: codeblock}

<br>
A continuación, añada la siguiente sentencia necesaria al archivo de origen:

```
require ‘mqlight’
```
{: codeblock}



## Python
{: #mql_python_how notoc}

Añada la siguiente referencia al archivo <code>requirements.txt</code>:

```
git+git://github.com/mqlight/python-mqlight.git@readthedocs
```
{:codeblock}

<br>
A continuación, añada la siguiente sentencia al archivo de origen:

```
import mqlight
```
{:codeblock}


