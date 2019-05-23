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

# クラシック・プランの FAQ 
{: #faqs_classic}

クラシック・プランの {{site.data.keyword.IBM}} {{site.data.keyword.messagehub}} サービスに関する一般的な質問と回答。

すべての {{site.data.keyword.messagehub}} プランに関連する質問に対する回答は、[FAQ](docs/services/EventStreams?topic=eventstreams-faqs#faqs) を参照してください。
{: shortdesc}

<!--17/10/17 - Karen: same info duplicated at messagehub104 -->
## Kafka API を使用してトピックの作成と削除を行うにはどうしたらよいか?
{: #topic_admin_classic}
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
</dd>
</dl>


## {{site.data.keyword.messagehub}} がコンシューマー・オフセット・トピックのためにログ保存ウィンドウを設定する期間はどれだけの長さか?
{: #offsets_classic }
{: faq}

{{site.data.keyword.messagehub}} はコンシューマー・オフセットを 7 日間保存します。 これは Kafka 構成 offsets.retention.minutes に対応します。 

オフセット保存はシステム全体が対象であるため、個々のトピック・レベルで設定することはできません。 すべてのコンシューマー・グループは、ログ保存を最大 30 日に増やしたトピックを使用する場合でも、保管されるオフセットは 7 日間のみです。 

<!--following message retention info duplicted in eventstreams057 and evenstreams108-->

## メッセージの保存期間は？
{: #messages_retained}

デフォルトでは、Kafka ではメッセージは最大 24 時間保存され、各パーティションは 1 GB が上限です。 この 1 GB という上限に達したら、この限界を超えないよう、最も古いメッセージが破棄されます。

ユーザー・インターフェースまたは管理 API のいずれかを使用して、トピックを作成するときにメッセージ保存の時間制限を変更できます。 時間制限は、最小 1 時間、最大 30 日間です。

Kafka クライアントまたは Kafka Streams を使用してトピックを作成するときに許可される設定の制限については、[Kafka API を使用してトピックの作成と削除を行うにはどうしたらよいか?
](/docs/services/EventStreams?topic=eventstreams-faqs_classic#topic_admin_classic)を参照してください。


## {{site.data.keyword.messagehub}} の可用性の性質は?
{: #availability_classic}
{: faq}

{{site.data.keyword.messagehub}} アプリケーションを作成する場合、以下の情報を使用して、{{site.data.keyword.messagehub}} の通常の可用性がどのような性質なのか、および、アプリケーションに要求される処理内容を理解してください。

### API
{: #api_availability_classic }

{{site.data.keyword.messagehub}} の通常の操作の一環として、Kafka クラスターのノードが再始動されることがあります。
場合によっては、クラスターがリソースを再割り当てすると、アプリケーションはそれを認識します。 そういった変更に対して回復的であり、再接続して操作を再試行できるように、アプリケーションを作成してください。

### {{site.data.keyword.messagehub}} ブリッジ 
{: #bridge_availability_classic }

ブリッジが再始動する可能性を処理できるようにアプリケーションを作成してください。

## {{site.data.keyword.messagehub}} の最大メッセージ・サイズはいくつですか? 
{: #max_message_size_classic }
{: faq}

{{site.data.keyword.messagehub}} の最大メッセージ・サイズは 1 MB であり、これは Kafka デフォルトです。 

## {{site.data.keyword.messagehub}} のレプリケーション設定はどのようになっていますか? 
{: #replication_classic }
{: faq}

{{site.data.keyword.messagehub}} は強力な可用性および耐久性を提供するように構成されています。
以下の構成設定はすべてのトピックに適用され、変更することはできません。
* replication.factor = 3
* min.insync.replicas = 2

## クラシック・プランでの {{site.data.keyword.messagehub}} の請求の仕組みは? 
{: #billing_classic }
{: faq}

クラシック・プランの {{site.data.keyword.messagehub}} はユーザーのトピック数を定期的にサンプリングし、{{site.data.keyword.Bluemix_notm}} は最大サンプル値を毎日記録します。 {{site.data.keyword.messagehub}} は、検出された並行パーティションの最大数および送受信メッセージの日次合計について請求を行います。

例えば、1 日に 1 つのトピックを 10 回作成して削除した場合、最大 1 トピックについて課金されます。 ただし、10 個のトピックを作成して削除した場合、サンプリングがいつ実行されたのかによって、0 または 10 のトピックについて課金される可能性があります。

{{site.data.keyword.messagehub}} は、メッセージごと、または 64 k ごとに、請求を行います。 64 k までのメッセージは 1 つの請求可能メッセージとしてカウントされます。 64 k を超えるメッセージは、<code><var class="keyword varname">message_size</var> &divide; 64 k</code> 個の請求メッセージとしてカウントされます。

<!--12/04/18 - Karen: same info duplicated at messagehub057 -->
## Kafka REST API 再始動の頻度は? 
{: #REST_restart_classic }
{: faq}

Kafka REST API は、1 日 1 回、短時間の再始動期間があります。 

この期間中には、Kafka REST API が利用不可になることがあります。 これが起こった場合、要求を再試行することをお勧めします。 REST API が再始動された後、Kafka コンシューマー・インスタンスの再作成が必要になります。 この場合、REST API は次の JSON を返します。

```'{"error_code":40403,"message":"Consumer instance not found."}'
```
{:screen}

## {{site.data.keyword.messagehub}} のプラン間の相違点は?
{: #plan_compare_classic }
{: faq}

他の {{site.data.keyword.messagehub}} プランについて詳しくは、[プランの選択](/docs/services/EventStreams?topic=eventstreams-plan_choose)を参照してください。クラシック・プランについて詳しくは、[クラシック・プランの概要](/docs/services/EventStreams?topic=eventstreams-plan_choose_classic#plan_choose_classic)を参照してください。


## 災害復旧は、どのようにすればよいですか?
{: #disaster_recovery_classic }
{: faq}

現在、{{site.data.keyword.messagehub}} の災害復旧の管理の責任はお客様にあります。 {{site.data.keyword.messagehub}} データについては、あるロケーション (地域) の {{site.data.keyword.messagehub}} インスタンスと別のロケーションの別のインスタンスとの間でレプリカを生成できます。 ただし、リモートの {{site.data.keyword.messagehub}} インスタンスのプロビジョンとレプリカ生成はお客様の責任です。

また、メッセージ・ペイロード・データのバックアップもお客様の責任です。 このデータはクラスター内の複数の Kafka ブローカー間でレプリカ生成されますが、大部分の障害を防ぐため、このレプリカ生成では、ロケーション全体の障害をカバーしません。 

トピック名は {{site.data.keyword.messagehub}} によってバックアップされます。















