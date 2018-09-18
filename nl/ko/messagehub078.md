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

# {{site.data.keyword.messagehub}}에서 {{site.data.keyword.mql}} API 사용 방법
{: #mql_how}


API를 사용하려면 다음과 같이 선택한 언어에 대한 사용 가능한 최신 {{site.data.keyword.mql}} 클라이언트 API에 참조를 추가하십시오.

## Java의 경우
{: #mql_java_how notoc}

<code>Maven pom</code> 파일에 다음 참조를 추가하십시오.

```
<dependency>
    <groupId>com.ibm.mqlight</groupId>
    <artifactId>mqlight-api</artifactId>
    <version>LATEST</version>
</dependency>
```
{:codeblock}



## Node.js의 경우
{: #mql_node_how notoc}

<code>package.json</code> 파일의 종속성 섹션에 다음 참조를 추가하십시오.

<pre class="pre"><code>"mqlight" : "1.0.x"</code></pre>
{: codeblock}

그리고 소스 파일에 다음 require 문을 추가하십시오.

<pre class="pre"><code>var mqlight = require(‘mqlight’);</code></pre>
{: codeblock}


## Ruby의 경우
{: #mql_ruby_how notoc}

<code>Gemfile</code>에 다음 참조를 추가하십시오.

```
gem 'mqlight', '~> 1.0'
```
{: codeblock}

<br>
그리고 소스 파일에 다음 require 문을 추가하십시오.

```
require ‘mqlight’
```
{: codeblock}



## Python의 경우
{: #mql_python_how notoc}

<code>requirements.txt</code> 파일에 다음 참조를 추가하십시오.

```
git+git://github.com/mqlight/python-mqlight.git@readthedocs
```
{:codeblock}

<br>
그리고 소스 파일에 다음 import 문을 추가하십시오.

```
import mqlight
```
{:codeblock}


