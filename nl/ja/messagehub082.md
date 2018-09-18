---

copyright:
  years: 2015, 2018
lastupdated: "2016-11-22"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# MQ Light API サンプル
{: #mql_samples}


まだアプリケーションがない場合、サンプルの 1 つを使用して {{site.data.keyword.mql}} API を試してみてください。
{:shortdesc}

サンプル・アプリケーションは、実際は 2 つの単純なアプリケーションからなります。
1 つは、メッセージをバックエンドに送信する Web フロントエンドであり、もう 1 つは、メッセージを処理し、語を大文字化してからメッセージをフロントエンドに返すバックエンドです。 このサンプルは、{{site.data.keyword.mql}} API を使用してアプリケーションが互いに対話できるようにするのが簡単であることを示しています。 また、{{site.data.keyword.mql}} API を使用してワーカーの負荷を軽減することもできます。
これは、スケーラブルで疎結合な分散型のアプリケーションを構築するために必要な主要な機能の 1 つです。

サンプル・コードは [message-hub-samples GitHub プロジェクト ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/ibm-messaging/message-hub-samples/tree/master/mqlight){:new_window} にあります。
