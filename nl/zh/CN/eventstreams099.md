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

# 在经典套餐上使用 MQ Light 客户机
{: #mql_clients}

**MQ Light API 仅在经典套餐中提供。**
{: note}
<br/>

## 使用 MQ Light Java 客户机
{: #mql_java}

要使用 API，请添加对用于 Java 的最新可用 {{site.data.keyword.mql}} 客户机 API 的引用，如下所示：
{: shortdesc}

将以下引用添加到 <code>Maven pom</code> 文件：

```
<dependency>
    <groupId>com.ibm.mqlight</groupId>
    <artifactId>mqlight-api</artifactId>
    <version>LATEST</version>
</dependency>
```
{:codeblock}


<!-- 12/11/18: info was in eventstreams102.md, moved because of doc app changes -->

## 使用 MQ Light Node.js 客户机 
{: #mql_node}


要使用 API，请添加对用于 Node.js 的最新可用 {{site.data.keyword.mql}} 客户机 API 的引用，如下所示：

将以下引用添加到 <code>package.json</code> 文件的 dependency 部分：

<pre class="pre"><code>"mqlight" : "1.0.x"</code></pre>
{: codeblock}

将以下 require 语句添加到源文件：

<pre class="pre"><code>var mqlight = require(‘mqlight’);</code></pre>
{: codeblock}

<!-- 14/11/18: info was in eventstreams103.md, moved because of doc app changes -->

## 使用 MQ Light Ruby 客户机
{: #mql_ruby}


要使用 API，请添加对用于 Ruby 的最新可用 {{site.data.keyword.mql}} 客户机 API 的引用，如下所示：

将以下引用添加到 <code>Gemfile</code>：

```
gem 'mqlight', '~> 1.0'
```
{: codeblock}

将以下 require 语句添加到源文件：

```
require ‘mqlight’
```
{: codeblock}

<!-- 14/11/18: info was in eventstreams101.md, moved because of doc app changes -->

## 使用 MQ Light Python 客户机
{: #mql_python}

要使用 API，请添加对用于 Python 的最新可用 {{site.data.keyword.mql}} 客户机 API 的引用，如下所示：

将以下引用添加到 <code>requirements.txt</code> 文件：

```
git+git://github.com/mqlight/python-mqlight.git@readthedocs
```
{:codeblock}

<br>
将以下 import 语句添加到源文件：



```
import mqlight
```
{:codeblock}
<!-- Comment from Andrew
Instructions for getting started, with links for more info
Simple send source and receive source in-line

-->



