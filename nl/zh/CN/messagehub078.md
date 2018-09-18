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

# 如何在 {{site.data.keyword.messagehub}} 中使用 {{site.data.keyword.mql}} API
{: #mql_how}


要使用 API，请添加对与所选语言对应的最新可用 {{site.data.keyword.mql}} 客户机 API 的引用，如下所示：

## 对于 Java
{: #mql_java_how notoc}

将以下引用添加到 <code>Maven pom</code> 文件：

```
<dependency>
    <groupId>com.ibm.mqlight</groupId>
    <artifactId>mqlight-api</artifactId>
    <version>LATEST</version>
</dependency>
```
{:codeblock}



## 对于 Node.js
{: #mql_node_how notoc}

将以下引用添加到 <code>package.json</code> 文件的 dependency 部分：

<pre class="pre"><code>"mqlight" : "1.0.x"</code></pre>
{: codeblock}

将以下 require 语句添加到源文件：

<pre class="pre"><code>var mqlight = require(‘mqlight’);</code></pre>
{: codeblock}


## 对于 Ruby
{: #mql_ruby_how notoc}

将以下引用添加到 <code>Gemfile</code>：

```
gem 'mqlight', '~> 1.0'
```
{: codeblock}

<br>
将以下 require 语句添加到源文件：

```
require ‘mqlight’
```
{: codeblock}



## 对于 Python
{: #mql_python_how notoc}

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


