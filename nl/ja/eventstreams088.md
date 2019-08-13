---

copyright:
  years: 2015, 2019
lastupdated: "2018-11-08"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# クラシック・プランでのブリッジの使用による他のサービスへのリンク 
{: #bridges}

**{{site.data.keyword.messagehub}} ブリッジは、クラシック・プランの一部としてのみ使用可能です。**
<br/>

{{site.data.keyword.messagehub}} クラシック・プランは、選択された他のシステムへのブリッジもサポートします。 ブリッジは、{{site.data.keyword.messagehub}} と別のサービスとの間の単一方向のリンクです。 ブリッジによって、{{site.data.keyword.messagehub}} からデータを読み取って別のサービスに書き込むか、または、別のサービスからデータを読み取って {{site.data.keyword.messagehub}} に書き込むことが可能になります。 ブリッジは、他のシステムからメッセージを取得してトピックにパブリッシュするか、または、トピックからメッセージをコンシュームして他のシステムに送信することができます。 この方法で、コードを開発することなく、{{site.data.keyword.messagehub}} を使用して他のシステムと統合することができます。
{:shortdesc}

{{site.data.keyword.messagehub}} ブリッジを使用することの主な利点は次のとおりです。  

* ブリッジを管理的に定義できるため、アプリケーション・コードを書く必要がありません。
* 各ブリッジのライフサイクルは、{{site.data.keyword.messagehub}} サービスによってモニターおよび管理されます。 例えば、あるブリッジでエラーが発生した場合、そのブリッジは {{site.data.keyword.messagehub}} によって自動的に再始動されます。
* ブリッジは {{site.data.keyword.Bluemix_short}} プラットフォームと統合されます。 例えば、ブリッジによって生成されたロギングおよびモニタリング情報は、Kibana ダッシュボードおよび Grafana ダッシュボードに転送されます。

ブリッジが役立つ一般的なシナリオとして次の 2 つがあります。

* 1 つ以上のデータ・ソースから {{site.data.keyword.messagehub}} へデータを取り込む。
* {{site.data.keyword.messagehub}} から別のサービス (例えば、長期保管) へデータを転送する。

## {{site.data.keyword.messagehub}} ブリッジ
{: notoc}

* 以下のタイプのブリッジが提供されています。 
  - [MQ ブリッジ](/docs/services/EventStreams?topic=eventstreams-mq_bridge)。これは、{{site.data.keyword.IBM}} MQ からメッセージ・データを取り出し、{{site.data.keyword.messagehub}} のトピックへ転送します。 将来は、より広範囲のブリッジのサポートが予定されています。
  - [Cloud Object Storage ブリッジ](/docs/services/EventStreams?topic=eventstreams-cloud_object_storage_bridge)。これは、{{site.data.keyword.messagehub}} データを [{{site.data.keyword.IBM_notm}} Cloud Object Storage ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/services/cloud-object-storage?topic=cloud-object-storage-about#about){:new_window} サービスのインスタンスへ転送します。 
  - [{{site.data.keyword.objectstorageshort}} ブリッジ](/docs/services/EventStreams?topic=eventstreams-object_storage_bridge) は、2018 年 8 月 1 日より非推奨になりました。 詳しくは、[非推奨の発表: {{site.data.keyword.objectstorageshort}} OpenStack Swift (PaaS) ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/blogs/cloud-archive/2018/05/end-marketing-object-storage-openstack-swift-paas/){:new_window}を参照してください。
* 現在のところ、ブリッジはすべての {{site.data.keyword.Bluemix_notm}} パブリック環境で使用可能です。 {{site.data.keyword.Bluemix_short}} Dedicated ではブリッジは使用可能ではありません。
* 次の 2 つの方法でブリッジを管理できます。
  - 既存 {{site.data.keyword.messagehub}} 管理 API の拡張である [REST API ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-messaging/event-streams-docs){:new_window} を使用する。 curl を使用してブリッジのライフサイクルを管理する方法の例も [message-hub-docs ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-messaging/event-streams-docs){:new_window} にあります。 ブリッジの開発は継続中であるため、この REST API は変更される可能性があります。 この API は安定化される予定です。
  - {{site.data.keyword.Bluemix_notm}} コンソールで {{site.data.keyword.messagehub}} ダッシュボードを使用する。
* 任意のタイプの最大 2 つのブリッジを、{{site.data.keyword.messagehub}} サービスの 1 つのインスタンスと関連付けることができます。 ブリッジの開発は継続中であるため、この制限は引き続き検討の対象となります。
* ブリッジでのメッセージング操作以外にはブリッジ使用に対する追加料金はありません。
* MQ ブリッジは、データがブリッジと MQ キュー・マネージャーとの間で転送されるときにデータのプライバシーと保全性を保護するための SSL/TLS の使用をサポートしていません。 SSL/TLS 使用のサポートがブリッジに追加される計画があります。 
* しかし、[{{site.data.keyword.SecureGatewayfull}} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/services/SecureGateway?topic=securegateway-getting-started-with-sg#getting-started-with-sg){:new_window} サービスを使用して、{{site.data.keyword.Bluemix_notm}} と、オンプレミスにインストールできる {{site.data.keyword.SecureGateway}} クライアントとの間のセキュア・トンネルを介して、データを送信することができます。 この構成では、トンネルのどちらの終端でも通信は SSL/TLS を使用して保護されてはいません。
* Cloud Object Storage ブリッジは、データを Cloud Object Storage に書き込むときに、区切り文字として改行文字を使用してメッセージを連結します。 このため、改行文字が埋め込まれたメッセージおよびバイナリー・メッセージ・データには、このブリッジは不適切です。
* Cloud Object Storage ブリッジによって現在使用されているオブジェクト命名規則は将来変更される可能性があります。

## 他のサービスから {{site.data.keyword.messagehub}} へのブリッジ
{: notoc}

* {{site.data.keyword.iot_full}} は、[{{site.data.keyword.messagehub}} への独自のブリッジ](/docs/services/EventStreams?topic=eventstreams-consuming_messages)を提供します。 このブリッジは、{{site.data.keyword.messagehub}} への単一方向リンクを提供し、履歴データの保管に使用できます。 {{site.data.keyword.messagehub}} を {{site.data.keyword.iot_short_notm}} に接続することによって、{{site.data.keyword.messagehub}} をイベント・パイプラインとして使用して Watson IoT Platform からデバイス・イベントをコンシュームし、それらのイベントをプラットフォームの残りの部分でリアルタイムに使用できるようにすることができます。 


