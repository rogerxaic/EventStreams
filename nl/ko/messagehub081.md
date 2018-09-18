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

# MQ Light API 및 Kafka 또는 Kafka REST API 간의 메시지 교환
{: #mql_exchange}

** MQ Light API는 표준 플랜의 일부로만 사용 가능합니다.**
<br/>

{{site.data.keyword.mql}} 메시지는 "MQLight"로 이름 지정된 하나의 기본 Kafka 토픽에 저장되며 다음 표에 세부사항이 나와 있는대로 인코딩됩니다. 이 인코딩은 {{site.data.keyword.mql}} API를 사용하여 애플리케이션과 메시지를 교환하기 위해 다른 API 유형(예: Kafka 또는 Kafka REST)에서도 사용될 수 있습니다.

## Kafka 메시지 형식
{: #kafka_format notoc}

<table border='1'>
<caption>표 1. Kafka 메시지 형식</caption>
  <tr>
    <th> 키 </th>
    <th> 값 </th>
  </tr>
  <tr>
    <td> 선택사항(API에서 사용되지 않음)
	<p></p>
	</td>
    <td>**1바이트**
	<p>		     MQ Light API eye catcher이며, 항상 0xFA입니다.</p>
    <p><var class="keyword varname">**n**</var>**바이트**</p>
    <p>		    AMQP 인코딩된 메시지(AMQP 연결 형식을 기반으로 형식화됨)입니다. </p></td>
  </tr>
</table>


