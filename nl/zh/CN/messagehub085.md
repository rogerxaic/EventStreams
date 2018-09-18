---

copyright:
  years: 2015, 2018
lastupdated: "2018-07-02"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 选择您的套餐 
{: #plan_choose}

{{site.data.keyword.messagehub}} 可提供两种不同的套餐：标准套餐和企业套餐，具体取决于您的要求。

## 标准套餐

如果您需要事件获取和分发功能，但是不需要企业套餐的其他任何好处，那么标准套餐比较合适。标准套餐提供多租户 {{site.data.keyword.messagehub}} 集群的共享访问权。

## 企业套餐 

如果数据隔离、性能保证和延长保留时间是很重要的考虑事项，企业套餐比较合适。企业套餐提供对专用 {{site.data.keyword.messagehub}} 集群的互斥访问权。

## 标准套餐和企业套餐支持哪些功能

下表总结了这些套餐支持的功能：

<table>
    <caption>表 1. 标准和企业套餐中支持的功能</caption>
      <tr>
	        <th></th>
		    <th>标准套餐</th>
		    <th>企业套餐</th>
        </tr>
		<tr>
			<td>**租赁**</td>
			<td>多租户</td>
			<td>单租户</td>
		</tr>
        <tr>
			<td>**可用性区域**</td>
			<td>不支持</td>
			<td>3</td>
		</tr>
	  		<tr>
			<td>**集群上的 Kafka 版本**</td>
			<td>Kafka 0.10.2</td>
			<td>Kafka 1.1</td>
		</tr>
		<tr>
			<td>**细颗粒度访问控制**</td>
			<td>否</td>
			<td>是</td>
		</tr>
		<tr>
			<td>**是否支持 Kafka Connect 和 Kafka Streams**</td>
			<td>是</td>
			<td>是</td>
		</tr>
		<tr>
			<td>**最大分区数**</td>
			<td>100</td>
			<td>1000</td>
		</tr>
		<tr>
			<td>**最长保留期**</td>
			<td>每个分区保留 1 GB，最长 30 天</td>
			<td>无限期保留，直到达到套餐存储限制</td>
		</tr>
		<tr>
			<td>**区域可用性**</td>
			<td>美国南部</br>
			英国</br>
			悉尼</br>
			德国（无 MQ Light API）</td>
			<td>当前为美国南部</br>
			<br/>
			</td>
		</tr>
		<tr>
     	    <td>**支持的 API**</td>
			<td>Kafka API</br>
			管理 REST API<br/>
			Kafka REST API</br>
			MQ Light API</br>
		    </td>
			<td>Kafka API<br/>
			管理 REST API</td>
		</tr>
			<td>**是否支持 Cloud Object Storage 网桥和<br/>
			MQ 网桥**</td>
			<td>是</td>
			<td>否</td>
		</tr>
		<tr>
			<td></td>
			<td></td>
			<td></td>
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

