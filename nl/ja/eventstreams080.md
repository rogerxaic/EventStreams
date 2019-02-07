---

copyright:
  years: 2015, 2019
lastupdated: "2018-05-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

<!-- 14/11/18: info moved to eventstreams075.md, moved because of doc app changes -->
# 既存 MQ Light アプリケーションへの接続方法
{: #mql_exist_apps}

**MQ Light API は、標準プランのみの一部として使用可能です。**
<br/>

現在は {{site.data.keyword.IBM_notm}} MQ または {{site.data.keyword.mql}} {{site.data.keyword.Bluemix_notm}} サービスのいずれかに対して実行されている既存のアプリケーションを当サービスに接続できます。 アプリケーションは同じように機能し続けます。
{:shortdesc}

既存のアプリケーションを接続するには、以下を確認してください。

* アプリケーションが、ご使用の言語での最新の使用可能なバージョンの {{site.data.keyword.mql}} API クライアントを使用していることを確認します。
* VCAP_SERVICES から取り出した接続詳細が、<code>messagehub</code> サービス・タイプを参照していること、接続ユーザー名を <code>credentials.username</code> プロパティーではなく <code>credentials.user</code> プロパティーから取り出していること、および、接続ルックアップ URL を <code>credentials.connectionLookupURI</code> プロパティーではなく <code>credentials.mqlight_lookup_url</code> プロパティーから取り出していることを確認します。 詳しくは、[VCAP_SERVICES 環境変数](/docs/services/EventStreams/eventstreams127.html)を参照してください。

	このステップが行われるのは、Java&trade; クライアントを使用していて、クライアント自体が情報を取り出すように create() 呼び出しの endpointService パラメーターとして「null」を指定しているユーザーのためであることに注意してください。
	
* アプリケーションは TLS v1.2 接続をサポートしている必要があります。 VCAP_SERVICES には、非 TLS 接続を作成するための <code>credentials.nonTLSConnectionLookupURI</code> プロパティーは含まれなくなりました。

以下の点にも注意する必要があります。

* メッセージ限度は {{site.data.keyword.messagehub}} と整合していますが、{{site.data.keyword.mql}} API をサポートする他のサーバーとは異なっている可能性があります。 詳しくは、[最大限度](/docs/services/EventStreams/eventstreams083.html)を参照してください。
* JMS はサポートされません。
