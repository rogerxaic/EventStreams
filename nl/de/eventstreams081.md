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
{:table: .aria-labeledby="caption"}

<!-- 15/11/18: info moved to eventstreams075.md, moved because of doc app changes -->
# Nachrichten zwischen MQ Light-API und Kafka- oder Kafka-REST-APIs austauschen
{: #mql_exchange}

** Die MQ Light-API ist nur als Bestandteil des Plans "Standard" verfügbar.**
<br/>

{{site.data.keyword.mql}}-Nachrichten werden in einem einzigen Kafka-Basistopic mit dem Namen "MQLight" gespeichert und codiert, wie in der folgenden Tabelle angegeben. Diese Codierung kann auch von anderen API-Typen (z. B. Kafka- oder Kafka-REST-APIs) verwendet werden, um Nachrichten mit Anwendungen über die
{{site.data.keyword.mql}}-API auszutauschen.
{: shortdesc}

## Kafka-Nachrichtenformat
{: #kafka_format notoc}

<table border='1'>
<caption>Tabelle 1. Kafka-Nachrichtenformat</caption>
  <tr>
    <th> Schlüssel </th>
    <th> Wert </th>
  </tr>
  <tr>
    <td> Optional (von der API nicht verwendet)
	<p></p>
	</td>
    <td>**1 Byte**
	<p>		     Die Strukturkennung der MQ Light-API ist stets 0xFA.</p>
    <p><var class="keyword varname">**n**</var> **Byte**</p>
    <p>		    In AMQP codierte Nachricht (formatiert im AMQP-Sendeformat). </p></td>
  </tr>
</table>


