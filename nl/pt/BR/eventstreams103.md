---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Usando o cliente Ruby do MQ Light
{: #mql_how}

** A API do MQ Light está disponível como parte somente do plano Standard.**
<br/>

Para usar a API, inclua uma referência à API do cliente {{site.data.keyword.mql}} mais recente
disponível para Ruby como segue:

Inclua a referência a seguir no <code>Gemfile</code>:

```
gem 'mqlight', '~> 1.0'
```
{: codeblock}

E inclua a instrução de solicitação a seguir em seu arquivo de origem:

```
require ‘mqlight’
```
{: codeblock}

<!-- Comment from Andrew
Instructions for getting started, with links for more info
Simple send source and receive source in-line

-->


