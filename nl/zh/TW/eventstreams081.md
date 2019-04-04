---

copyright:
  years: 2015, 2019
lastupdated: "2018-05-25"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

<!-- 15/11/18: info moved to eventstreams075.md, moved because of doc app changes -->
# 在 MQ Light API 與 Kafka 或 Kafka REST API 之間交換訊息
{: #mql_exchange_rest}

** MQ Light API 只提供於標準方案中。**
<br/>

{{site.data.keyword.mql}} 訊息儲存在稱為 "MQLight" 的單一基礎 Kafka 主題，並詳細編碼於下表中。此編碼也可供其他 API 類型（例如 Kafka 或 Kafka REST）用來與使用 {{site.data.keyword.mql}} API 的應用程式交換訊息。
{: shortdesc}

## Kafka 訊息格式
{: #kafka_message_format notoc}

<table border='1'>
<caption>表 1. Kafka 訊息格式</caption>
  <tr>
    <th> 金鑰</th>
    <th> 值</th>
  </tr>
  <tr>
    <td> 選用（不是由 API 使用）
	<p></p>
	</td>
    <td>**1 個位元組**
	<p>		     MQ Light API 醒目標示，一律為 0xFA。</p>
    <p><var class="keyword varname">**n**</var> **個位元組**</p>
    <p>		    AMQP 編碼訊息（根據 AMQP 發訊格式格式化）。</p></td>
  </tr>
</table>


