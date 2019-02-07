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

# Utilisation des clients MQ Light
{: #mql_clients}

**L'API MQ Light est disponible uniquement dans le cadre du plan Standard.**
<br/>
## Utilisation du client Java MQ Light
{: #mql_java}

Pour utiliser l'API, ajoutez une référence à l'API du client {{site.data.keyword.mql}} pour Java la plus récente comme suit :
{: shortdesc}

Ajoutez la référence suivante dans le fichier <code>Maven pom</code> :

```
<dependency>
    <groupId>com.ibm.mqlight</groupId>
    <artifactId>mqlight-api</artifactId>
    <version>LATEST</version>
</dependency>
```
{:codeblock}

<!-- 12/11/18: info was in eventstreams102.md, moved because of doc app changes -->

## Utilisation du client Node.js MQ Light 
{: #mql_node}


Pour utiliser l'API, ajoutez une référence à l'API du client {{site.data.keyword.mql}} pour Node.js la plus récente comme suit :

Ajoutez la référence suivante dans la section dependency du fichier <code>package.json</code> :

<pre class="pre"><code>"mqlight" : "1.0.x"</code></pre>
{: codeblock}

Ajoutez l'instruction require suivante à votre fichier source :

<pre class="pre"><code>var mqlight = require(&lsquo;mqlight&rsquo;);</code></pre>
{: codeblock}

<!-- 14/11/18: info was in eventstreams103.md, moved because of doc app changes -->

## Utilisation du client Ruby MQ Light
{: #mql_ruby}


Pour utiliser l'API, ajoutez une référence à l'API du client {{site.data.keyword.mql}} pour Ruby la plus récente comme suit :

Ajoutez la référence suivante au fichier <code>Gemfile</code> :

```
gem 'mqlight', '~> 1.0'
```
{: codeblock}

Ajoutez l'instruction require suivante à votre fichier source :

<pre class="pre"><code>require &lsquo;mqlight&rsquo;</code></pre>
{: codeblock}

<!-- 14/11/18: info was in eventstreams101.md, moved because of doc app changes -->

## Utilisation du client Python MQ Light
{: #mql_python}

Pour utiliser l'API, ajoutez une référence à l'API du client {{site.data.keyword.mql}} pour Python la plus récente comme suit :

Ajoutez la référence suivante dans le fichier <code>requirements.txt</code> :

```
git+git://github.com/mqlight/python-mqlight.git@readthedocs
```
{:codeblock}

<br>
Puis, ajoutez l'instruction import suivante dans le fichier source :

```
import mqlight
```
{:codeblock}
<!-- Comment from Andrew
Instructions for getting started, with links for more info
Simple send source and receive source in-line

-->
