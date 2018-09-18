---

copyright:
  years: 2015, 2018
lastupdated: "2017-09-26"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Message Hub とは

{{site.data.keyword.messagehub}} は、パブリッシュ/サブスクライブ・メッセージングをトピックを使用して実装します。 他のメッセージング・システムとは違って、{{site.data.keyword.messagehub}} は、トピックを提供しますが、キューは提供しません。 {{site.data.keyword.messagehub}} は、他のシステムへのブリッジなどの追加機能も提供します。

{{site.data.keyword.messagehub}} は、オープン・ソース Apache Kafka プロジェクトをベースにしている、多くの実稼働環境で実証済みの、高度にスケーラブルで高パフォーマンスのメッセージング・バックボーンです。 詳しくは、[{{site.data.keyword.messagehub}} と Apache Kafka](/docs/services/MessageHub/messagehub073.html) を参照してください。
{{site.data.keyword.messagehub}} への接続は常に資格情報を使用して認証されるので追加の構成を提供する必要はありますが、Apache Kafka ツールは、通常 {{site.data.keyword.messagehub}} で直接使用できます。

{{site.data.keyword.messagehub}} には、Kafka API、Kafka REST API、{{site.data.keyword.mql}} API という 3 つの API があります。
ほとんどの場合、Kafka API が最適な選択となります。 これらの API を使用してメッセージング・アプリケーションを作成する方法について詳しくは、
[Kafka クライアントの使用](/docs/services/MessageHub/messagehub050.html)、[Kafka REST API の使用](/docs/services/MessageHub/messagehub025.html)、[MQ Light API クライアントの使用](/docs/services/MessageHub/messagehub075.html)を参照してください。

{{site.data.keyword.messagehub}} は、選択された他のシステムへのブリッジもサポートします。 ブリッジは、別のシステムへの単一方向リンクです。 ブリッジは、他のシステムからメッセージを取得してトピックにパブリッシュするか、または、トピックからメッセージをコンシュームして他のシステムに送信することができます。 この方法で、コードを開発することなく、{{site.data.keyword.messagehub}} を使用して他のシステムと統合することができます。 詳しくは、[ブリッジの使用による他のサービスへのリンク](/docs/services/MessageHub/messagehub088.html)を参照してください。
