---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-24"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka, service endpoints

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# エンタープライズ・プランを使用したサービス・エンドポイントの管理
{: #manage_endpoints}

内部エンドポイントまたはプライベート・エンドポイントを使用することで、{{site.data.keyword.Bluemix_short}} サービス・エンドポイントは内部 IBM Cloud ネットワークを経由した {{site.data.keyword.messagehub}} サービスへの接続を可能にします。この機能は、ユーザーが {{site.data.keyword.messagehub}} サービスを介してパブリッシュまたはコンシュームするデータはすべて、パブリック・インターフェースでなくプライベート・ネットワークを経由することを意味します。
{:shortdesc}

{{site.data.keyword.messagehub}} サービス・インスタンスにアクセスして管理するために、プライベート・エンドポイントを追加できるようになりました。

## 前提条件
{: #prereqs_endpoint}

以下の要件を満たしていることを確認します。
- エンタープライズ・プランを使用してサービス・インスタンスを作成します。詳しくは、[プランの選択](/docs/services/EventStreams?topic=eventstreams-plan_choose)を参照してください。
- {{site.data.keyword.Bluemix_notm}} ダラス (us-south) 地域またはフランクフルト (eu-de) 地域でサービス・インスタンスを作成します。
- {{site.data.keyword.Bluemix_notm}} アカウントの[仮想経路転送 (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) を使用可能にします。

## プライベート・エンドポイントの追加
{: #add_endpoint}

プライベート・エンドポイントを追加するには、以下のようにします。

* [チケット ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/get-support?topic=get-support-getting-customer-support#using-avatar){:new_window} をオープンして、プライベート・エンドポイントを依頼します。以下の情報をチケットに記入してください。

    * クラスター ID (わかっている場合)。

    クラスター ID が不明の場合は、ご使用のダッシュボード URL、Kafka ブローカー・エンドポイント、またはサービス・インスタンス ID を代わりに提供してください。
  

サービス・エンドポイントについて詳しくは、[{{site.data.keyword.Bluemix_notm}} サービス・エンドポイントの資料](/docs/resources?topic=resources-service-endpoints#about){:new_window} を参照してください。


## プライベート・エンドポイントへの切り替え後
{: #after_endpoint}

プライベート・エンドポイントへの切り替えが完了すると、外部エンドポイントまたはパブリック・エンドポイントは使用できなくなります。


### IBM {{site.data.keyword.messagehub}} コンソールへのアクセス

クラスターのプライベート・エンドポイントが使用可能になると、{{site.data.keyword.messagehub}} コンソールにアクセスするときに使用する Admin URL が変更されます。

{{site.data.keyword.messagehub}} コンソールは、プライベート Admin URL からのみ到達可能です。プライベート Admin URL を含めたプライベート・エンドポイントをディスカバーするには、新しいサービス資格情報を作成できます。

<!--
1. On the service details page, click **Manage endpoints**. You can see the external endpoint assigned to your service instance.
2. Click  **Add internal endpoint**. An internal endpoint is assigned to your service instance.
3. **Optional.** Use the endpoint toggle to enable or disable endpoints as needed.
-->

