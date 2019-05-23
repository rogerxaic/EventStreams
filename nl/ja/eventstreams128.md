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

# 管理 Kafka Java クライアント API の使用
{: #kafka_java_api}


<!-- 
17/10/17 - Karen: following info duplicated at eventstreams108
 -->

0.11 以降の Kafka クライアントまたは 0.10.2.0 以降の Kafka Streams を使用している場合、API を使用してトピックの作成と削除を行うことができます。 トピック作成時に許可される設定には、いくつかの制限があります。 現在のところ、変更できるのは以下の設定のみです。
{: shortdesc}

<dl>
<dt>cleanup.policy</dt>
<dd><code>delete</code> (デフォルト)、<code>compact</code>、または <code>delete,compact</code>
<p> に設定してください。**注:**
クリーンアップ・ポリシーが <code>compact</code> のみである場合、自動的に <code>delete</code> が追加されますが、時間に基づく削除は無効にされます。 トピック内のメッセージは、削除される前に 1 GB にまで圧縮されます。</p>
</dd>

<dt>retention.ms</dt>
<dd>デフォルトの保存期間は 24 時間です。 最小は 1 時間、最大は 30 日間です。 時間の倍数でこの値を指定してください。

<p>**注:** エンタープライズ・プランでは、これを任意の値に設定できます。</p>
</dd>

<dt>retention.bytes</dt>
<dd>スペースを解放するために古いログが廃棄されるまでの、パーティション (ログ・セグメントからなる) を拡張できる最大サイズ。

<p>**注:**
エンタープライズ・プランのみ。 1 MB より大きい任意の値に設定してください。</p>
</dd>

<dt>segment.bytes</dt>
<dd>ログのセグメント・ファイル・サイズ。

<p>**注:**
エンタープライズ・プランのみ。 100 kB より大きい任意の値に設定してください。</p>
</dd>

<dt>segment.index.bytes</dt>
<dd>オフセットをファイル位置にマップする索引のサイズ。 

<p>**注:**
エンタープライズ・プランのみ。 100 kB から 2 GB までの任意の値に設定してください。</p>
</dd>

<dt>segment.ms</dt>
<dd>この期間を過ぎると、セグメント・ファイルが満杯になっていなくても Kafka がログを強制的にロールする期間。 

<p>**注:**
エンタープライズ・プランのみ。 5 分から 30 日までの任意の値に設定してください。</p>
</dd>
</dl>

