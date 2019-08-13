---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-15"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 管理 REST API の使用
{: #admin_api}

{{site.data.keyword.messagehub}} では、トピックを作成、削除、リストおよび更新するために使用できる管理 RESTful API が提供されています。
{: shortdesc}

サービス・インスタンスのサービス資格情報オブジェクトまたはサービス・キーから、 API に接続するために必要な URL と資格情報の詳細を取得する必要があります。 これらのオブジェクトの作成について詳しくは、[{{site.data.keyword.messagehub}} への接続](/docs/services/EventStreams?topic=eventstreams-connecting)を参照してください。

API のエンドポイントの URL は、<code>kafka_admin_url</code> プロパティーに指定されています。

資格情報は認証方式より異なります。以下の 3 種類の資格情報がサポートされています。

* **基本認証を使用して認証を行うには**:<br/>
    ユーザー名とパスワードに、上記オブジェクトの <code>user</code> プロパティーと <code>api_key</code> プロパティーを使用します。 これらの値を HTTP 要求の <code>Authorization</code> ヘッダーに、<code>Basic &lt;単一コロン (:) で結合されたユーザー名とパスワードを Base64 エンコードした値&gt;</code> の形式で指定します。

* **ベアラー・トークンを使用して認証するには:**<br/>
    IBM Cloud CLI を使用してトークンを取得するには、まず IBM Cloud にログインしてから、次のコマンドを実行します。 

    ```
    ibmcloud iam oauth-tokens
    ```
    {: codeblock}

    このトークンを、HTTP 要求の Authorization ヘッダーに <code>Bearer<token></code> の形式で指定します。API キーと JWT トークンの両方がサポートされています。 

* ** API キーを直接使用して認証するには: **<br/>
    キーを <code>X-Auth-Token</code> HTTP ヘッダーの値として直接指定します。

クラシック・プランで作成されたサービス・インスタンスの場合、この情報は、ユーザーのアプリケーションの VCAP_SERVICES 環境変数から代わりに取得できます。

サンプルを使用する API の説明については、
[{{site.data.keyword.messagehub}} admin-rest ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-messaging/event-streams-docs/tree/master/admin-rest-api){:new_window}を参照してください。

この API の完全な仕様は [{{site.data.keyword.messagehub}}Admin REST API YAML ファイル![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-messaging/event-streams-docs/blob/master/admin-rest-api/admin-rest-api.yaml){:new_window} からダウンロードできます。
この Swagger ファイルを表示するには、Swagger ツール (例えば、[Swagger エディター ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://editor.swagger.io/#/){:new_window} を使用してください。




