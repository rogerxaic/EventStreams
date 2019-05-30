---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-09"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 经典套餐 
{: #plan_choose_classic}

根据您的需求，{{site.data.keyword.messagehub}} 可作为不同的套餐提供。有关标准套餐和企业套餐的信息，请参阅[选择套餐](/docs/services/EventStreams?topic=eventstreams-plan_choose#plan_choose)。
{: shortdesc}
 
## 经典套餐概述
如果您需要事件摄入和分发功能，但是不需要企业套餐的其他任何好处，那么经典套餐比较合适。经典套餐提供多租户 {{site.data.keyword.messagehub}} 集群的共享访问权。


## 经典套餐支持哪些功能

下表总结了该套餐支持的功能：

<table>
    <caption>表 1. 经典套餐中支持的功能</caption>
      <tr>
	        <th></th>
		    <th>经典套餐</th>
        </tr>
		<tr>
			<td>**租赁**</td>
			<td>多租户</td>
		</tr>
        <tr>
			<td>**可用性区域**</td>
			<td>不支持</td>
		</tr>
        <tr>
			<td>**可用性**</td>
			<td>99.5%</td>
		</tr>
	  		<tr>
			<td>**集群上的 Kafka 版本**</td>
			<td>Kafka 1.1</td>
		</tr>
		<tr>
			<td>**细颗粒度访问控制**</td>
			<td>否</td>
		</tr>
				<tr>
			<td>**Cloud Service Endpoint 支持**</td>
			<td>否</td>
		</tr>
		<tr>
			<td>**是否支持 Kafka Connect 和 Kafka Streams**</td>
			<td>是</td>

		</tr>
		<tr>
			<td>**最大分区数**</td>
			<td>100</td>

		</tr>
		<tr>
			<td>**最长保留期**</td>
			<td>每个分区保留 1 GB，最长 30 天</td>

		</tr>
		<tr>
			<td>**最大吞吐量**</td>
			<td>每个分区每秒 1 MB</td>
		</tr>
		<tr>
			<td>**最大消息大小**</td>
			<td>1 MB</td>
		</tr>
		<tr>
			<td>**位置（区域）可用性**</td>
			<td>达拉斯 (us-south)</br>
			伦敦 (eu-gb)</br>
			悉尼 (au-syd)</br>
			法兰克福 (eu-de) - 无 {{site.data.keyword.mql}} API </td>

		</tr>
		<tr>
     	    <td>**支持的 API**</td>
			<td>Kafka API</br>
			Admin REST API<br/>
			Kafka REST API</br>
			MQ Light API</br>
		    </td>
		</tr>
			<td>**支持 Cloud Object Storage 网桥和 <br/>
			MQ 网桥**</td>
			<td>是</td>
		</tr>
		<tr>
			<td>**部署时间范围**</td>
			<td>即时供应</td>
		</tr>

</table>

