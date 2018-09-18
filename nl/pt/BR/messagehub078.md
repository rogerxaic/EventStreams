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

# Como usar a API do {{site.data.keyword.mql}} no {{site.data.keyword.messagehub}}
{: #mql_how}


Para usar a API, inclua uma referência à API do cliente {{site.data.keyword.mql}} mais recente
disponível para sua linguagem escolhida como segue:

## Para Java
{: #mql_java_how notoc}

Inclua a referência a seguir em seu arquivo <code>Maven pom</code>:

```
<dependency>
    <groupId>com.ibm.mqlight</groupId>
    <artifactId>mqlight-api</artifactId>
    <version>LATEST</version>
</dependency>
```
{:codeblock}



## Para Node.js
{: #mql_node_how notoc}

Inclua a referência a seguir na seção de dependência de seu arquivo <code>package.json</code>:

<pre class="pre"><code>"mqlight" : "1.0.x"</code></pre>
{: codeblock}

E inclua a instrução de solicitação a seguir em seu arquivo de origem:

<pre class="pre"><code>var mqlight = require(‘mqlight’);</code></pre>
{: codeblock}


## Para Ruby
{: #mql_ruby_how notoc}

Inclua a referência a seguir no <code>Gemfile</code>:

```
gem 'mqlight', '~> 1.0'
```
{: codeblock}

<br>
E inclua a instrução de solicitação a seguir em seu arquivo de origem:

```
require ‘mqlight’
```
{: codeblock}



## Para Python
{: #mql_python_how notoc}

Inclua a referência a seguir em seu arquivo <code>requirements.txt</code>:

```
git+git://github.com/mqlight/python-mqlight.git@readthedocs
```
{:codeblock}

<br>
E inclua a instrução de importação a seguir em seu arquivo de origem:

```
import mqlight
```
{:codeblock}


