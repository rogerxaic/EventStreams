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
{:table: .aria-labeledby="caption"}

# MQ Light API と Kafka API または Kafka REST API との間でのメッセージ交換
{: #mql_exchange}

**MQ Light API は、標準プランのみの一部として使用可能です。**
<br/>

{{site.data.keyword.mql}} メッセージは、基礎となっている単一の Kafka トピック「MQLight」に保管され、次の表に詳細が示されているようにエンコードされます。 {{site.data.keyword.mql}} API を使用してアプリケーションとメッセージを交換するために、他の API タイプ (Kafka や Kafka REST など) でもこのエンコードを使用できます。

## Kafka メッセージ・フォーマット
{: #kafka_format notoc}

<table border='1'>
<caption>表 1. Kafka メッセージ・フォーマット</caption>
  <tr>
    <th> キー </th>
    <th> 値 </th>
  </tr>
  <tr>
    <td> オプション (API では使用されません)
	<p></p>
	</td>
    <td>**1 バイト**
	<p>		     MQ Light API の目印であり、常に 0xFA です。</p>
    <p><var class="keyword varname">**n**</var> **バイト**</p>
    <p>		    AMQP エンコードされたメッセージ (AMQP ワイヤー・フォーマットに基づいてフォーマットされます)。 </p></td>
  </tr>
</table>


