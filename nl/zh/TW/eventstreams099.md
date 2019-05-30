---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-04"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 在經典方案上使用 MQ Light 用戶端
{: #mql_clients}

**MQ Light API 僅在經典方案中提供。**
{: note}
<br/>

## 使用 MQ Light Java 用戶端
{: #mql_java}

若要使用 API，請為 Java 新增參照到最新的可用 {{site.data.keyword.mql}} 用戶端 API，如下所示：
{: shortdesc}

新增下列參照到您的 <code>Maven pom</code> 檔案：

```
<dependency>
    <groupId>com.ibm.mqlight</groupId>
    <artifactId>mqlight-api</artifactId>
    <version>LATEST</version>
</dependency>
```
{:codeblock}


<!-- 12/11/18: info was in eventstreams102.md, moved because of doc app changes -->

## 使用 MQ Light Node.js 用戶端 
{: #mql_node}


若要使用 API，請為 Node.js 新增參照到最新的可用 {{site.data.keyword.mql}} 用戶端 API，如下所示：

新增下列參照到 <code>package.json</code> 檔案的 dependency 區段：

<pre class="pre"><code>"mqlight" : "1.0.x"</code></pre>
{: codeblock}

然後新增下列 require 陳述式到您的原始檔：

<pre class="pre"><code>var mqlight = require('mqlight');</code></pre>
{: codeblock}

<!-- 14/11/18: info was in eventstreams103.md, moved because of doc app changes -->

## 使用 MQ Light Ruby 用戶端
{: #mql_ruby}


若要使用 API，請為 Ruby 新增參照到最新的可用 {{site.data.keyword.mql}} 用戶端 API，如下所示：

新增下列參照到 <code>Gemfile</code>：

```
gem 'mqlight', '~> 1.0'
```
{: codeblock}

然後新增下列 require 陳述式到您的原始檔：

```
require 'mqlight'
```
{: codeblock}

<!-- 14/11/18: info was in eventstreams101.md, moved because of doc app changes -->

## 使用 MQ Light Python 用戶端
{: #mql_python}

若要使用 API，請為 Python 新增參照到最新的可用 {{site.data.keyword.mql}} 用戶端 API，如下所示：

新增下列參照到您的 <code>requirements.txt</code> 檔案：

```
git+git://github.com/mqlight/python-mqlight.git@readthedocs
```
{:codeblock}

<br>
然後新增下列 import 陳述式到您的原始檔：



```
import mqlight
```
{:codeblock}
<!-- Comment from Andrew
Instructions for getting started, with links for more info
Simple send source and receive source in-line

-->



