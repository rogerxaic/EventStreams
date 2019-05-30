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

# 클래식 플랜에서 MQ Light 클라이언트 사용
{: #mql_clients}

** MQ Light API는 클래식 플랜의 일부로만 사용 가능합니다. **
{: note}
<br/>

## MQ Light Java 클라이언트 사용
{: #mql_java}

API를 사용하려면 다음과 같이 사용 가능한 최신 Java용 {{site.data.keyword.mql}} 클라이언트 API에 참조를 추가하십시오.
{: shortdesc}

<code>Maven pom</code> 파일에 다음 참조를 추가하십시오.

```
<dependency>
    <groupId>com.ibm.mqlight</groupId>
    <artifactId>mqlight-api</artifactId>
    <version>LATEST</version>
</dependency>
```
{:codeblock}


<!-- 12/11/18: info was in eventstreams102.md, moved because of doc app changes -->

## MQ Light Node.js 클라이언트 사용 
{: #mql_node}


API를 사용하려면 다음과 같이 사용 가능한 최신 Node.js용 {{site.data.keyword.mql}} 클라이언트 API에 참조를 추가하십시오.

<code>package.json</code> 파일의 종속성 섹션에 다음 참조를 추가하십시오.

<pre class="pre"><code>"mqlight" : "1.0.x"</code></pre>
{: codeblock}

그리고 소스 파일에 다음 require 문을 추가하십시오.

<pre class="pre"><code>var mqlight = require(‘mqlight’);</code></pre>
{: codeblock}

<!-- 14/11/18: info was in eventstreams103.md, moved because of doc app changes -->

## MQ Light Ruby 클라이언트 사용
{: #mql_ruby}


API를 사용하려면 다음과 같이 사용 가능한 최신 Ruby용 {{site.data.keyword.mql}} 클라이언트 API에 참조를 추가하십시오.

<code>Gemfile</code>에 다음 참조를 추가하십시오.

```
gem 'mqlight', '~> 1.0'
```
{: codeblock}

그리고 소스 파일에 다음 require 문을 추가하십시오.

```
require ‘mqlight’
```
{: codeblock}

<!-- 14/11/18: info was in eventstreams101.md, moved because of doc app changes -->

## MQ Light Python 클라이언트 사용
{: #mql_python}

API를 사용하려면 다음과 같이 사용 가능한 최신 Python용 {{site.data.keyword.mql}} 클라이언트 API에 참조를 추가하십시오.

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
<!-- Comment from Andrew
Instructions for getting started, with links for more info
Simple send source and receive source in-line

-->



