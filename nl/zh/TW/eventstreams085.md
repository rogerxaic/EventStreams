---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-23"

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

# 選擇方案 
{: #plan_choose}

根據您的需求，{{site.data.keyword.messagehub}} 可作為不同的方案提供：標準、企業和經典。 

<!--
For information about the Classic plan, see
[Classic plan](/docs/services/EventStreams?topic=eventstreams-plan_choose_classic#plan_choose_classic).
-->
{: shortdesc}

## 標準方案
{: #plan_standard}

標準方案適合您需要事件汲取與配送功能，但不需要企業方案的任何其他好處的情況。標準方案提供對多方承租戶 {{site.data.keyword.messagehub}} 叢集的共用存取。

## 企業方案 
{: #plan_enterprise}

企業方案適合資料隔離、保證效能及增加保留是重要考量的情況。企業方案提供對專用 {{site.data.keyword.messagehub}} 叢集的互斥存取。您也可以在地理區域但[單一區域位置 (SZR)](/docs/services/EventStreams?topic=eventstreams-sla#sla_szr) 中佈建 {{site.data.keyword.messagehub}} 叢集。

## 經典方案
{: #plan_classic}

經典方案提供標準方案先前版本的存取權，僅針對現有工作負載以及出於舊版相容性的目的而提供。您應該針對標準方案佈建新的工作負載。


## 標準方案、企業方案和經典方案支援哪些功能

下表彙總方案支援的內容：

<table>
    <caption>表 1. 標準、企業和經典方案中支援的功能</caption>
      <tr>
	        <th></th>
		    <th>標準方案</th>
		    <th>企業方案</th>
		    <th>經典方案</th>
        </tr>
		<tr>
			<td>**租用**</td>
			<td>多方承租戶</td>
			<td>單一承租戶</td>
			<td>多方承租戶</td>
		</tr>
        <tr>
			<td>**可用性區域**</td>
			<td>3</td>
			<td>3<br/>（單一區域位置中有 1 個）
			</td>
			<td>不支援</td>
		</tr>
        <tr>
			<td>**可用性**</td>
			<td>99.95%</td>
			<td>99.95%<br/>（單一區域位置中的 99.5%）[<sup>1</sup>](/docs/services/EventStreams?topic=eventstreams-plan_choose#footnote_plans)</td>
			<td>99.5%</td>
		</tr>
	  		<tr>
			<td>**叢集上的 Kafka 版本**</td>
			<td>Kafka 2.2</td>
			<td>Kafka 1.1<br/>（Kafka 2.2 即將推出）</td>
			<td>Kafka 1.1</td>
		</tr>
		<tr>
			<td>**精細存取控制**</td>
			<td>是</td>
			<td>是</td>
			<td>否</td>
		</tr>
				<tr>
			<td>**Cloud Service Endpoint 支援**</td>
			<td>否</td>
			<td>是</td>
			<td>否</td>
		</tr>
		<tr>
			<td>**支援 Kafka Connect 及 Kafka Streams**</td>
			<td>是</td>
			<td>是</td>
			<td>是</td>
		</tr>
		<tr>
			<td>**分割區數目上限**</td>
			<td>100</td>
			<td>1000</td>
			<td>100</td>
		</tr>
		<tr>
			<td>**最長保留期間**</td>
			<td>每分割區 1 GB 達 30 天</td>
			<td>2 TB 的使用儲存空間<!--Unlimited up to the storage limit of your plan --></td>
			<td>每分割區 1 GB 達 30 天</td>
		</tr>
		<tr>
			<td>**最大傳輸量**</td>
			<td>每個分割區每秒 1 MB（每秒上限 20 MB）</td>
			<td>每個叢集每秒 40 MB（每秒尖峰傳輸量為 75 MB）</td>
			<td>每分割區每秒 1 MB</td>
		</tr>
		<tr>
			<td>**訊息大小上限**</td>
			<td>1 MB</td>
			<td>1 MB</td>
			<td>1 MB</td>
		</tr>
		<tr>
			<td>**已連接用戶端數目上限**</td>
			<td>100</td>
			<td>10,000</td>
			<td>100</td>
		</tr>
		<tr>
			<td>**位置（地區）可用性**</td>
			<td>**多區域位置 (MZR)**<br/>
			達拉斯 (us-south)</br>
			華盛頓 (us-east)<br/>
			倫敦 (eu-gb)<br/>
			雪梨 (au-syd)</br>
			法蘭克福 (eu-de)<br/>
			東京 (jp-tok)<br/>
			<br/>
			</td>
			<td>**多區域位置 (MZR)**</br>
			達拉斯 (us-south)</br>
			華盛頓 (us-east)<br/>
			倫敦 (eu-gb)<br/>
			雪梨 (au-syd)</br>
			法蘭克福 (eu-de)<br/>
			東京 (jp-tok)<br/>
			<br/>
			**單一區域位置 (SZR)**</br>
			首爾 (seo01)<br/>
			<br/>
			</td>
			<td>達拉斯 (us-south)</br>
			倫敦 (eu-gb)</br>
			雪梨 (au-syd)</br>
			法蘭克福 (eu-de) - 無 {{site.data.keyword.mql}} API </td>
		</tr>
		<tr>
     	    <td>**支援的 API**</td>
			<td>Kafka API</br>
			管理 REST API<br/>
			REST 生產者 API</br>
		    </td>
			<td>Kafka API<br/>
			管理 REST API</br>
			REST Producer API</br>
			</td>
			<td>Kafka API</br>
			管理 REST API<br/>
			Kafka REST API</br>
			MQ Light API</br>
		    </td>
		</tr>
		</tr>
			<td>**支援 Cloud Object Storage 橋接器及<br/>
			MQ 橋接器**</td>
			<td>否</td>
			<td>否</td>
			<td>是</td>
		</tr>
		<tr>
			<td>**部署時間範圍**</td>
			<td>即時佈建</td>
			<td>預期佈建最多需要 3 小時。因為企業對於每個叢集都有自己的專用資源，所以需要更多時間進行佈建。</td>
			<td>即時佈建</td>
		</tr>

</table>
### 註腳
{: #footnote_plans notoc}

1. {: #footnote_szr notoc} 如需可用性的相關資訊，請參閱[單一區域位置部署](/docs/services/EventStreams?topic=eventstreams-sla#sla_szr)。



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

For more information, see [{{site.data.keyword.Bluemix_notm}} Public ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud/free/){:new_window}.
-->

