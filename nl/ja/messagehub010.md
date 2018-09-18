---

copyright:
  years: 2015, 2018
lastupdated: "2018-06-28"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Message Hub とは
{: #about}

{{site.data.keyword.messagehub_full}} は、Apache Kafka で構築されている高スループットのメッセージ・バスです。これは、{{site.data.keyword.Bluemix_notm}} へのイベントの取り込みのため、および、お客様のサービスおよびアプリケーション間でイベント・ストリームを分散させるために最適化されています。

{{site.data.keyword.messagehub}} を使用することによって、以下のタスクを実行できます。

* バックエンド・ワーカー・プロセスに分担させることで負荷を軽減する。
* ストリーム・データを Analytics に接続して強力な洞察を実現する
* イベント・データを複数のアプリケーションにパブリッシュしてリアルタイムに対応する。
* 別のサービス (例えば、長期保管) にデータを転送する。

Apache Kafka で構築されることによって、コミュニティーで起こっているすべてのイノベーションの利点を直接活用でき、Kafka クライアント API、Kafka Stream、Kafka Connect、および KSQL をサポートできます。
{:shortdesc}

{{site.data.keyword.messagehub}} への接続は常に資格情報を使用して認証されるので追加の構成を提供する必要はありますが、Apache Kafka ツールは、通常 {{site.data.keyword.messagehub}} で直接使用できます。

{{site.data.keyword.messagehub}} では、アプリケーションはメッセージを作成してトピックに送信することによって、データを送信します。 メッセージを受信するには、アプリケーションはトピックをサブスクライブし、アプリケーションがトピックのメッセージをすべて受信するのか、アプリケーション間でメッセージを共有するのかのいずれかを選択します。
{{site.data.keyword.messagehub}} は、特定の順序でメッセージのホストと保守を行います。 




