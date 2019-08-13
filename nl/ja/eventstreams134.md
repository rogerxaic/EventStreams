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

# クラシック・プラン 
{: #plan_choose_classic}

{{site.data.keyword.messagehub}} は、要件に応じて、異なるプランとして使用可能です。 標準プランとエンタープライズ・プランについて詳しくは、[プランの選択](/docs/services/EventStreams?topic=eventstreams-plan_choose#plan_choose)を参照してください。
{: shortdesc}
 
## クラシック・プランの概要
クラシック・プランが適しているのは、イベントの取り込みおよび配布の機能は必要だが、エンタープライズ・プランで提供される追加機能はどれも必要ではない場合です。 クラシック・プランは、マルチテナント {{site.data.keyword.messagehub}} クラスターへの共有アクセスを提供します。


## クラシック・プランでサポートされるもの

次の表は、このプランで何がサポートされるのかを要約したものです。

<table>
    <caption>表 1. クラシック・プランでのサポート</caption>
      <tr>
	        <th></th>
		    <th>クラシック・プラン</th>
        </tr>
		<tr>
			<td>**テナント**</td>
			<td>マルチテナント </td>
		</tr>
        <tr>
			<td>**アベイラビリティー・ゾーン**</td>
			<td>サポートされていません</td>
		</tr>
        <tr>
			<td>**可用性**</td>
			<td>99.5%</td>
		</tr>
	  		<tr>
			<td>**クラスターの Kafka バージョン**</td>
			<td>Kafka 1.1</td>
		</tr>
		<tr>
			<td>**微細化されたアクセス制御**</td>
			<td>いいえ</td>
		</tr>
				<tr>
			<td>**クラウド・サービス・エンドポイントのサポート**</td>
			<td>いいえ</td>
		</tr>
		<tr>
			<td>**Kafka Connect および Kafka Streams のサポート **</td>
			<td>はい</td>

		</tr>
		<tr>
			<td>**最大パーティション数**</td>
			<td>100</td>

		</tr>
		<tr>
			<td>**最大保存期間**</td>
			<td>最大 30 日までパーティション当たり 1 GB </td>

		</tr>
		<tr>
			<td>**最大スループット**</td>
			<td>パーティション当たり 1 MB/秒</td>
		</tr>
		<tr>
			<td>**最大メッセージ・サイズ **</td>
			<td>1 MB</td>
		</tr>
		<tr>
			<td>**ロケーション (地域) アベイラビリティー**</td>
			<td>ダラス (us-south)</br>
			ロンドン (eu-gb)</br>
			シドニー (au-syd)</br>
			フランクフルト (eu-de) - {{site.data.keyword.mql}} API なし </td>

		</tr>
		<tr>
     	    <td>**サポートされている API**</td>
			<td>Kafka API</br>
			Admin REST API<br/>
			Kafka REST API</br>
			MQ Light API</br>
		    </td>
		</tr>
			<td>**Cloud Object Storage ブリッジおよび<br/>
			MQ ブリッジのサポート**</td>
			<td>はい</td>
		</tr>
		<tr>
			<td>**デプロイメントの時間フレーム**</td>
			<td>即時のプロビジョン</td>
		</tr>

</table>

