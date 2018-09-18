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

# 如何在 {{site.data.keyword.messagehub}} 使用 {{site.data.keyword.mql}} API
{: #mql_how}


若要使用 API，請為您選擇的語言新增參照到最新的可用 {{site.data.keyword.mql}} 用戶端 API，如下所示：

## 針對 Java
{: #mql_java_how notoc}

新增下列參照到您的 <code>Maven pom</code> 檔案：

```
<dependency>
    <groupId>com.ibm.mqlight</groupId>
    <artifactId>mqlight-api</artifactId>
    <version>LATEST</version>
</dependency>
```
{:codeblock}



## 針對 Node.js
{: #mql_node_how notoc}

新增下列參照到 <code>package.json</code> 檔案的 dependency 區段：

<pre class="pre"><code>"mqlight" : "1.0.x"</code></pre>
{: codeblock}

然後新增下列 require 陳述式到您的原始檔：

<pre class="pre"><code>var mqlight = require('mqlight');</code></pre>
{: codeblock}


## 針對 Ruby
{: #mql_ruby_how notoc}

新增下列參照到 <code>Gemfile</code>：

```
gem 'mqlight', '~> 1.0'
```
{: codeblock}

<br>
然後新增下列 require 陳述式到您的原始檔：

```
require 'mqlight'
```
{: codeblock}



## 針對 Python
{: #mql_python_how notoc}

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


