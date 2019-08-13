---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-13"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:faq: data-hd-content-type='faq'}

# FAQ
{: #faqs}

{{site.data.keyword.IBM}} {{site.data.keyword.messagehub}} サービスに関する一般的な質問と回答。

クラシック・プラン固有の質問に対する回答は、[クラシック・プランの FAQ](/docs/services/EventStreams?topic=eventstreams-faqs_classic)を参照してください。
{: shortdesc}

<!--17/10/17 - Karen: same info duplicated at messagehub104 -->
## Kafka API を使用してトピックの作成と削除を行うにはどうしたらよいか?
{: #topic_admin}
{: faq}

0.11 以降の Kafka クライアントまたは 0.10.2.0 以降の Kafka Streams を使用している場合、API を使用してトピックの作成と削除を行うことができます。 トピック作成時に許可される設定には、いくつかの制限があります。 現在のところ、変更できるのは以下の設定のみです。

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


## {{site.data.keyword.messagehub}} がコンシューマー・オフセット・トピックのためにログ保存ウィンドウを設定する期間はどれだけの長さか?
{: #offsets }
{: faq}

{{site.data.keyword.messagehub}} はコンシューマー・オフセットを 7 日間保存します。 これは Kafka 構成 offsets.retention.minutes に対応します。 

オフセット保存はシステム全体が対象であるため、個々のトピック・レベルで設定することはできません。 すべてのコンシューマー・グループは、ログ保存を最大 30 日に増やしたトピックを使用する場合でも、保管されるオフセットは 7 日間のみです。 

内部 Kafka <code>__consumer_offsets</code> トピックが読み取り専用として可視になります。 
このトピックは、いかなる場合も絶対に管理しないでください。 

<!--following message retention info duplicted in eventstreams057-->

## メッセージの保存期間は？
{: #messages_retained}

デフォルトでは、Kafka ではメッセージは最大 24 時間保存され、各パーティションは 1 GB が上限です。 この 1 GB という上限に達したら、この限界を超えないよう、最も古いメッセージが破棄されます。

ユーザー・インターフェースまたは管理 API のいずれかを使用して、トピックを作成するときにメッセージ保存の時間制限を変更できます。 時間制限は、最小 1 時間、最大 30 日間です。

Kafka クライアントまたは Kafka Streams を使用してトピックを作成するときに許可される設定の制限については、[Kafka API を使用してトピックの作成と削除を行うにはどうしたらよいか?
](/docs/services/EventStreams?topic=eventstreams-faqs#topic_admin)を参照してください。

## {{site.data.keyword.messagehub}} の可用性の性質は?
{: #availability}
{: faq}

{{site.data.keyword.messagehub}} アプリケーションを作成する場合、以下の情報を使用して、{{site.data.keyword.messagehub}} の通常の可用性がどのような性質なのか、および、アプリケーションに要求される処理内容を理解してください。

### API
{: #api_availability }

{{site.data.keyword.messagehub}} の通常の操作の一環として、Kafka クラスターのノードが再始動されることがあります。
場合によっては、クラスターがリソースを再割り当てすると、アプリケーションはそれを認識します。 そういった変更に対して回復的であり、再接続して操作を再試行できるように、アプリケーションを作成してください。

## {{site.data.keyword.messagehub}} の最大メッセージ・サイズはいくつですか? 
{: #max_message_size }
{: faq}

{{site.data.keyword.messagehub}} の最大メッセージ・サイズは 1 MB であり、これは Kafka デフォルトです。 

## {{site.data.keyword.messagehub}} のレプリケーション設定はどのようになっていますか? 
{: #replication }
{: faq}

{{site.data.keyword.messagehub}} は強力な可用性および耐久性を提供するように構成されています。
以下の構成設定はすべてのトピックに適用され、変更することはできません。
* replication.factor = 3 
* min.insync.replicas = 2

## {{site.data.keyword.messagehub}} 標準プランと {{site.data.keyword.messagehub}} エンタープライズ・プランの違いは何ですか?
{: #plan_compare }
{: faq}

異なる {{site.data.keyword.messagehub}} プランについて詳しくは、[プランの選択](/docs/services/EventStreams?topic=eventstreams-plan_choose)を参照してください。

## 災害復旧は、どのようにすればよいですか?
{: #disaster_recovery }
{: faq}

現在、{{site.data.keyword.messagehub}} の災害復旧の管理の責任はお客様にあります。 {{site.data.keyword.messagehub}} データについては、あるロケーション (地域) の {{site.data.keyword.messagehub}} インスタンスと別のロケーションの別のインスタンスとの間でレプリカを生成できます。 ただし、リモートの {{site.data.keyword.messagehub}} インスタンスのプロビジョンとレプリカ生成はお客様の責任です。

また、メッセージ・ペイロード・データのバックアップもお客様の責任です。 このデータはクラスター内の複数の Kafka ブローカー間でレプリカ生成されますが、大部分の障害を防ぐため、このレプリカ生成では、ロケーション全体の障害をカバーしません。 

トピック名は {{site.data.keyword.messagehub}} によってバックアップされます。















