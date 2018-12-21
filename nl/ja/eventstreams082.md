---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

<!-- 15/11/18: info moved to eventstreams075.md, moved because of doc app changes -->
# MQ Light API サンプル
{: #mql_samples}

**MQ Light API は、標準プランのみの一部として使用可能です。**
<br/>

まだアプリケーションがない場合、サンプルの 1 つを使用して {{site.data.keyword.mql}} API を試してみてください。
{:shortdesc}

サンプル・アプリケーションは、実際は 2 つの単純なアプリケーションからなります。
1 つは、メッセージをバックエンドに送信する Web フロントエンドであり、もう 1 つは、メッセージを処理し、語を大文字化してからメッセージをフロントエンドに返すバックエンドです。 このサンプルでは、{{site.data.keyword.mql}} API を使用して、アプリが互いに対話できるようにする方法を示します。 また、{{site.data.keyword.mql}} API を使用してワーカーの負荷を軽減することもできます。
これは、スケーラブルで疎結合な分散型のアプリケーションを構築するために必要な主要な機能の 1 つです。

サンプル・コードは [event-streams-samples GitHub プロジェクト ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-messaging/event-streams-samples/tree/master/mqlight){:new_window} にあります。
