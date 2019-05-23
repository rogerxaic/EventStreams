---

copyright:
  years: 2015, 2019
lastupdated: "2018-07-05"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# クラシック・プランの管理 REST API の使用
{: #admin_api_standard}

{{site.data.keyword.messagehub}} では、トピックを作成、削除、およびリストするために使用できる管理 RESTful API が提供されています。
{: shortdesc}

管理 RESTful API のエンドポイントを発見するために、
{{site.data.keyword.Bluemix_short}} アプリケーションは、
アプリケーションの VCAP_SERVICES 環境変数の `kafka_admin_url` プロパティーを使用することができます。

この API の完全な仕様は [admin-rest-api.yaml ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-messaging/event-streams-docs/blob/master/admin-rest-api/admin-rest-api.yaml){:new_window} からダウンロードできます。
この Swagger ファイルを表示するには、Swagger ツール (例えば、[Swagger エディター ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://editor.swagger.io/#/){:new_window} を使用してください。

[{{site.data.keyword.messagehub}} admin-rest-api ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-messaging/event-streams-docs/tree/master/admin-rest-api){:new_window} ディレクトリーにも、管理 RESTful API を呼び出す方法を示す例がいくつか含まれています。


