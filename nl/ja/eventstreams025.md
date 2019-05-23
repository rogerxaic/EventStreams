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
{:note: .note}

# REST プロデューサー API の使用
{: #rest_producer_using}


** REST プロデューサー API は、新規 {{site.data.keyword.messagehub}} 標準プランのみの一部として使用可能です。**
<br/>

{{site.data.keyword.messagehub}} が提供する REST API は、既存のシステムを {{site.data.keyword.messagehub}} Kafka クラスターに接続する際に役立ちます。この API を使用して、{{site.data.keyword.messagehub}} と、RESTful API をサポートする任意のシステムを統合することができます。

REST プロデューサー API はスケーラブルな REST インターフェースであり、セキュアな HTTP エンドポイントを経由して {{site.data.keyword.messagehub}} にメッセージをプロデュースするために使用します。イベント・データを {{site.data.keyword.messagehub}} に送信し、Kafka テクノロジーを利用してデータ・フィードを処理し、{{site.data.keyword.messagehub}} フィーチャーを活用してデータを管理します。

API を使用して既存のシステムを {{site.data.keyword.messagehub}} に接続します。システムから {{site.data.keyword.messagehub}} へのプロデュース要求を作成します。メッセージ・キー、ヘッダー、およびメッセージの書き込み先のトピックの指定などが含まれます。


## REST を使用したメッセージのプロデュース
{: #rest_produce_messages}

プロデューサー API を使用してメッセージをトピックに書き込みます。トピックへのプロデュースを実行できるようにするには、以下の情報が使用可能であることが必要です。

* {{site.data.keyword.messagehub}} API エンドポイントの URL (ポート番号を含む)。
* プロデュース先のトピック。
* 選択したトピックに接続し、プロデュースするための許可を付与する API キー。

サービス・インスタンスのサービス資格情報オブジェクトまたはサービス・キーから、 API に接続するために必要な URL と資格情報の詳細を取得する必要があります。これらのオブジェクトの作成について詳しくは、[{{site.data.keyword.messagehub}} への接続](/docs/services/EventStreams?topic=eventstreams-connecting)を参照してください。

API のエンドポイントの URL は、<code>kafka_http_url</code> プロパティーに指定されています。

次のいずれかの方法を使用して認証を行います。

* **基本認証を使用して認証を行うには:**<br/>
    ユーザー名とパスワードに、上記オブジェクトの <code>user</code> プロパティーと <code>api_key</code> プロパティーを使用します。これらの値は HTTP 要求の <code>Authorization</code> ヘッダーに、<code>Basic <単一コロン (:) で結合した Base64 エンコードの username:password></code> の形式で指定します。

* **ベアラー・トークンを使用して認証するには:**<br/>
    IBM Cloud CLI を使用してトークンを取得するには、まず IBM Cloud にログインしてから、次のコマンドを実行します。 

    ```
    ibmcloud iam oauth-tokens
    ```
    {: codeblock}

    このトークンを、HTTP 要求の Authorization ヘッダーに <code>Bearer <token></code> の形式で指定します。API キーと JWT トークンの両方がサポートされています。 

* ** API キーを直接使用して認証するには: **<br/>
    キーを <code>X-Auth-Token</code> HTTP ヘッダーの値として直接指定します。

<br/>
以下のコードが示すのは curl を使用したメッセージ送信の例です。

```
curl -v -X POST -H "Authorization: Basic <base64 username:password>" -H "Content-Type: text/plain" -H "Accept: application/json" -d 'test message' "<kafka_http_url>/topics/<topic_name>/records"
```
{: codeblock}


## API リファレンス
{: #rest_api_reference}

API についての詳細情報は、[{{site.data.keyword.messagehub}} REST プロデューサー API リファレンス![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://ibm.github.io/event-streams/api/){:new_window}を参照してください。












