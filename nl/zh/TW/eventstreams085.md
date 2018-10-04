---

copyright:
  years: 2015, 2018
lastupdated: "2018-09-12"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# 選擇方案 
{: #plan_choose}

{{site.data.keyword.messagehub}} 提供為兩個不同方案，視您的需求而定：標準及企業。

## 標準方案

標準方案適合您需要事件汲取與配送功能，但不需要企業方案的任何其他好處的情況。標準方案提供對多方承租戶 {{site.data.keyword.messagehub}} 叢集的共用存取。

## 企業方案 

企業方案適合資料隔離、保證效能及增加保留是重要考量的情況。企業方案提供對專用 {{site.data.keyword.messagehub}} 叢集的互斥存取。

## 標準及企業方案支援的內容

下表彙總方案支援的內容：

<table>
    <caption>表 1. 標準及企業方案中的支援</caption>
      <tr>
	        <th></th>
		    <th>標準方案</th>
		    <th>企業方案</th>
        </tr>
		<tr>
			<td>**租用**</td>
			<td>多方承租戶</td>
			<td>單一承租戶</td>
		</tr>
        <tr>
			<td>**可用性區域**</td>
			<td>不支援</td>
			<td>3</td>
		</tr>
	  		<tr>
			<td>**叢集上的 Kafka 版本**</td>
			<td>Kafka 0.10.2</td>
			<td>Kafka 1.1</td>
		</tr>
		<tr>
			<td>**精細存取控制**</td>
			<td>否</td>
			<td>是</td>
		</tr>
		<tr>
			<td>**支援 Kafka Connect 及 Kafka Streams**</td>
			<td>是</td>
			<td>是</td>
		</tr>
		<tr>
			<td>**分割區數目上限**</td>
			<td>100</td>
			<td>1000</td>
		</tr>
		<tr>
			<td>**最長保留期間**</td>
			<td>每分割區 1 GB 達 30 天</td>
			<td>無限制，直到方案的儲存空間限制為止</td>
		</tr>
		<tr>
			<td>**地區可用性**</td>
			<td>美國南部</br>
			英國</br>
			雪梨</br>
			德國（無 MQ Light API）</td>
			<td>美國南部</br>
			美國東部<br/>
			德國<br/>
			<br/>
			</td>
		</tr>
		<tr>
     	    <td>**支援的 API**</td>
			<td>Kafka API</br>
			管理 REST API<br/>
			Kafka REST API</br>
			MQ Light API</br>
		    </td>
			<td>Kafka API<br/>
			管理 REST API</td>
		</tr>
			<td>**支援 Cloud Object Storage 橋接器及<br/>
			MQ 橋接器**</td>
			<td>是</td>
			<td>否</td>
		</tr>
		<tr>
			<td>**部署時間範圍**</td>
			<td>即時佈建</td>
			<td>預期佈建最多需要 3 小時</td>
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

