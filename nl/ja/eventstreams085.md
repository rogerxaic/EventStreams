---

copyright:
  years: 2015, 2019
lastupdated: "2019-01-31"
keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# プランの選択 
{: #plan_choose}

{{site.data.keyword.messagehub}} は、必要に応じて、標準プランとエンタープライズ・プランという 2 つのプランでご使用いただけます。
{: shortdesc}

## 標準プラン

標準プランが適しているのは、イベントの取り込みおよび配布の機能は必要だが、エンタープライズ・プランで提供される追加機能はどれも必要ではない場合です。 標準プランは、マルチテナント {{site.data.keyword.messagehub}} クラスターへの共有アクセスを提供します。

## エンタープライズ・プラン 

エンタープライズ・プランが適しているのは、重要な考慮事項として、データの独立性、保証されたパフォーマンス、長期の保存期間がある場合です。 エンタープライズ・プランは、専用 {{site.data.keyword.messagehub}} クラスターへの排他的アクセスを提供します。

## 標準プランとエンタープライズ・プランでサポートされるもの

次の表は、各プランで何がサポートされるのかを要約したものです。

<table>
    <caption>表 1. 標準プランおよびエンタープライズ・プランでのサポート</caption>
      <tr>
	        <th></th>
		    <th>標準プラン</th>
		    <th>エンタープライズ・プラン</th>
        </tr>
		<tr>
			<td>**テナント**</td>
			<td>マルチテナント </td>
			<td>シングル・テナント</td>
		</tr>
        <tr>
			<td>**アベイラビリティー・ゾーン**</td>
			<td>サポートされていません</td>
			<td>3</td>
		</tr>
	  		<tr>
			<td>**クラスターの Kafka バージョン**</td>
			<td>Kafka 1.1</td>
			<td>Kafka 1.1</td>
		</tr>
		<tr>
			<td>**微細化されたアクセス制御**</td>
			<td>いいえ</td>
			<td>はい</td>
		</tr>
		<tr>
			<td>**Kafka Connect および Kafka Streams のサポート **</td>
			<td>はい</td>
			<td>はい</td>
		</tr>
		<tr>
			<td>**最大パーティション数**</td>
			<td>100</td>
			<td>1000</td>
		</tr>
		<tr>
			<td>**最大保存期間**</td>
			<td>最大 30 日までパーティション当たり 1 GB </td>
			<td>プランのストレージ制限を上限として制限なし </td>
		</tr>
		<tr>
			<td>**ロケーション (地域) アベイラビリティー**</td>
			<td>ダラス (us-south)</br>
			ロンドン (eu-gb)</br>
			シドニー (au-syd)</br>
			フランクフルト (eu-de) - {{site.data.keyword.mql}} API なし </td>
			<td>ダラス (us-south)</br>
			ワシントン (us-east)<br/>
			ロンドン (eu-gb)<br/>
			シドニー (au-syd)</br>
			フランクフルト (eu-de)<br/>
			東京 (jp-tok)<br/>

			<br/>
			</td>
		</tr>
		<tr>
     	    <td>**サポートされている API**</td>
			<td>Kafka API</br>
			Admin REST API<br/>
			Kafka REST API</br>
			MQ Light API</br>
		    </td>
			<td>Kafka API<br/>
			Admin REST API</td>
		</tr>
			<td>**Cloud Object Storage ブリッジおよび<br/>
			MQ ブリッジのサポート**</td>
			<td>はい</td>
			<td>いいえ</td>
		</tr>
		<tr>
			<td>**デプロイメントの時間フレーム**</td>
			<td>即時のプロビジョン</td>
			<td>プロビジョンには最大 3 時間かかることが予想される</td>
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

