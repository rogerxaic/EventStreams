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

# 經典方案 
{: #plan_choose_classic}

根據您的需求，{{site.data.keyword.messagehub}} 可作為不同的方案提供。如需標準方案和企業方案的資訊，請參閱[選擇方案](/docs/services/EventStreams?topic=eventstreams-plan_choose#plan_choose)。
{: shortdesc}
 
## 經典方案概觀
經典方案適合您需要事件汲取與配送功能，但不需要企業方案的任何其他好處的情況。經典方案提供對多方承租戶 {{site.data.keyword.messagehub}} 叢集的共用存取。


## 經典方案支援的內容

下表彙總方案支援的內容：

<table>
    <caption>表 1. 經典方案中支援的功能</caption>
      <tr>
	        <th></th>
		    <th>經典方案</th>
        </tr>
		<tr>
			<td>**租用**</td>
			<td>多方承租戶</td>
		</tr>
        <tr>
			<td>**可用性區域**</td>
			<td>不支援</td>
		</tr>
        <tr>
			<td>**可用性**</td>
			<td>99.5%</td>
		</tr>
	  		<tr>
			<td>**叢集上的 Kafka 版本**</td>
			<td>Kafka 1.1</td>
		</tr>
		<tr>
			<td>**精細存取控制**</td>
			<td>否</td>
		</tr>
				<tr>
			<td>**Cloud Service Endpoint 支援**</td>
			<td>否</td>
		</tr>
		<tr>
			<td>**支援 Kafka Connect 及 Kafka Streams**</td>
			<td>是</td>

		</tr>
		<tr>
			<td>**分割區數目上限**</td>
			<td>100</td>

		</tr>
		<tr>
			<td>**最長保留期間**</td>
			<td>每分割區 1 GB 達 30 天</td>

		</tr>
		<tr>
			<td>**最大傳輸量**</td>
			<td>每分割區每秒 1 MB</td>
		</tr>
		<tr>
			<td>**訊息大小上限**</td>
			<td>1 MB</td>
		</tr>
		<tr>
			<td>**位置（地區）可用性**</td>
			<td>達拉斯 (us-south)</br>
			倫敦 (eu-gb)</br>
			雪梨 (au-syd)</br>
			法蘭克福 (eu-de) - 無 {{site.data.keyword.mql}} API </td>

		</tr>
		<tr>
     	    <td>**支援的 API**</td>
			<td>Kafka API</br>
			管理 REST API<br/>
			Kafka REST API</br>
			MQ Light API</br>
		    </td>
		</tr>
			<td>**支援 Cloud Object Storage 橋接器及<br/>
			MQ 橋接器**</td>
			<td>是</td>
		</tr>
		<tr>
			<td>**部署時間範圍**</td>
			<td>即時佈建</td>
		</tr>

</table>

