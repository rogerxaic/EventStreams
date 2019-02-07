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

<!-- 14/11/18: info moved to eventstreams099.md, moved because of doc app changes -->
# 使用 MQ Light Python 客户机
{: #mql_python}

**MQ Light API 仅在标准套餐中提供。**
<br/>

要使用 API，请添加对用于 Python 的最新可用 {{site.data.keyword.mql}} 客户机 API 的引用，如下所示：
{: shortdesc}

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

