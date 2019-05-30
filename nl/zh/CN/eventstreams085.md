---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-16"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:note: .note}

# 选择您的套餐 
{: #plan_choose}

根据您的需求，{{site.data.keyword.messagehub}} 可作为不同的套餐提供：标准、企业和经典。 

<!--
For information about the Classic plan, see
[Classic plan](/docs/services/EventStreams?topic=eventstreams-plan_choose_classic#plan_choose_classic).
-->
{: shortdesc}

## 标准套餐

如果您需要事件摄入和分发功能，但是不需要企业套餐的其他任何好处，那么标准套餐比较合适。标准套餐提供多租户 {{site.data.keyword.messagehub}} 集群的共享访问权。

## 企业套餐 

如果数据隔离、性能保证和延长保留时间是很重要的考虑事项，企业套餐比较合适。企业套餐提供对专用 {{site.data.keyword.messagehub}} 集群的互斥访问权。

## 经典套餐

经典套餐提供标准套餐先前版本的访问权，仅针对现有工作负载以及出于向后兼容性的目的而提供。您应该针对标准套餐供应新的工作负载。


## 标准套餐、企业套餐和经典套餐支持哪些功能

下表总结了这些套餐支持的功能：

<table>
    <caption>表 1. 标准、企业和经典套餐中支持的功能</caption>
      <tr>
	        <th></th>
		    <th>标准套餐</th>
		    <th>企业套餐</th>
		    <th>经典套餐</th>
        </tr>
		<tr>
			<td>**租赁**</td>
			<td>多租户</td>
			<td>单租户</td>
			<td>多租户</td>
		</tr>
        <tr>
			<td>**可用性区域**</td>
			<td>3</td>
			<td>3</td>
			<td>不支持</td>
		</tr>
        <tr>
			<td>**可用性**</td>
			<td>99.95%</td>
			<td>99.95%</td>
			<td>99.5%</td>
		</tr>
	  		<tr>
			<td>**集群上的 Kafka 版本**</td>
			<td>Kafka 2.2</td>
			<td>Kafka 1.1 <br/>（即将发布 Kafka 2.2）</td>
			<td>Kafka 1.1</td>
		</tr>
		<tr>
			<td>**细颗粒度访问控制**</td>
			<td>是</td>
			<td>是</td>
			<td>否</td>
		</tr>
				<tr>
			<td>**Cloud Service Endpoint 支持**</td>
			<td>否</td>
			<td>是</td>
			<td>否</td>
		</tr>
		<tr>
			<td>**是否支持 Kafka Connect 和 Kafka Streams**</td>
			<td>是</td>
			<td>是</td>
			<td>是</td>
		</tr>
		<tr>
			<td>**最大分区数**</td>
			<td>100</td>
			<td>1000</td>
			<td>100</td>
		</tr>
		<tr>
			<td>**最长保留期**</td>
			<td>每个分区保留 1 GB，最长 30 天</td>
			<td>无限期保留，直到达到套餐存储限制</td>
			<td>每个分区保留 1 GB，最长 30 天</td>
		</tr>
		<tr>
			<td>**最大吞吐量**</td>
			<td>每个分区每秒 1 MB（每秒最大 20 MB）</td>
			<td>每秒 40 MB（每秒峰值吞吐量为 90 MB）</td>
			<td>每个分区每秒 1 MB</td>
		</tr>
		<tr>
			<td>**最大消息大小**</td>
			<td>1 MB</td>
			<td>1 MB</td>
			<td>1 MB</td>
		</tr>
		<tr>
			<td>**位置（区域）可用性**</td>
			<td>达拉斯 (us-south)</br>
 </td>
			<td>达拉斯 (us-south)</br>
			华盛顿 (us-east)<br/>
			伦敦 (eu-gb)<br/>
			悉尼 (au-syd)</br>
			法兰克福 (eu-de)<br/>
			东京 (jp-tok)<br/>
			<br/>
			</td>
			<td>达拉斯 (us-south)</br>
			伦敦 (eu-gb)</br>
			悉尼 (au-syd)</br>
			法兰克福 (eu-de) - 无 {{site.data.keyword.mql}} API </td>
		</tr>
		<tr>
     	    <td>**支持的 API**</td>
			<td>Kafka API</br>
			Admin REST API<br/>
			REST Producer API</br>
		    </td>
			<td>Kafka API<br/>
			Admin REST API</td>
			<td>Kafka API</br>
			Admin REST API<br/>
			Kafka REST API</br>
			MQ Light API</br>
		    </td>
		</tr>
		</tr>
			<td>**支持 Cloud Object Storage 网桥和 <br/>
			MQ 网桥**</td>
			<td>否</td>
			<td>否</td>
			<td>是</td>
		</tr>
		<tr>
			<td>**部署时间范围**</td>
			<td>即时供应</td>
			<td>预计供应最多需要 3 小时。因为企业对于每个集群都有自己的专用资源，所以需要更多时间进行供应。</td>
			<td>即时供应</td>
		</tr>

</table>




<!--
## {{site.data.keyword.Bluemix_notm}} Public environment
{: notoc}

{{site.data.keyword.Bluemix_notm}} Public provides an
economical public cloud service where you pay for what you use and share infrastructure with
others.

In {{site.data.keyword.Bluemix_notm}} Public, the cost of
{{site.data.keyword.messagehub}} is determined by two factors: the
number of partitions that you use and the number of messages that you send and receive. There is no
charge for message data while it is retained on the topics, but the data that each partition retains
is capped at 1 GB.

For more information, see [{{site.data.keyword.Bluemix_notm}} Public ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud-computing/bluemix/public){:new_window}.
-->

