---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-09"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}

# {{site.data.keyword.messagehub}} 可用性のサービス・レベル・アグリーメント (SLA) 
{: #sla}

## 標準プラン
{{site.data.keyword.messagehub}} サービスは、標準プランに 99.95% の可用性を提供します。
{{site.data.keyword.Bluemix}} の高可用性サービス向けの SLA について詳しくは、[{{site.data.keyword.Bluemix_notm}} サービス記述書![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www-03.ibm.com/software/sla/sladb.nsf/8bd55c6b9fa8039c86256c6800578854/c4ceb9f019f9eb4c862582f9001b3994/$FILE/i126-6605-16_04-2019_en_US.pdf){:new_window}を参照してください。


## エンタープライズ・プラン
{{site.data.keyword.messagehub}} サービスは、高可用性パブリック環境としてエンタープライズ・プランに 99.95% の可用性を提供します。
{{site.data.keyword.Bluemix}} の高可用性サービス向けの SLA について詳しくは、[{{site.data.keyword.Bluemix_notm}} サービス記述書![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www-03.ibm.com/software/sla/sladb.nsf/8bd55c6b9fa8039c86256c6800578854/c4ceb9f019f9eb4c862582f9001b3994/$FILE/i126-6605-16_04-2019_en_US.pdf){:new_window}を参照してください。

## クラシック・プラン
{{site.data.keyword.messagehub}} サービスは、クラシック・プランに 99.5% の可用性を提供します。
{{site.data.keyword.Bluemix}} の SLA について詳しくは、[{{site.data.keyword.Bluemix_notm}} サービス記述書![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www-03.ibm.com/software/sla/sladb.nsf/8bd55c6b9fa8039c86256c6800578854/c4ceb9f019f9eb4c862582f9001b3994/$FILE/i126-6605-16_04-2019_en_US.pdf){:new_window}を参照してください。

<!--
## What does 99.95% availability mean?
Availability refers to the ability of applications to produce and consume messages from Kafka topics.
-->

## 測定方法
サービス・インスタンスは、パフォーマンス、エラー率、および合成操作に対する応答を継続的にモニターされます。 障害が記録されます。詳しくは、[イベント・ストリームのサービス状況![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/status?component=messagehub&selected=status){:new_window}を参照してください。

可用性とは、Kafka トピックからのメッセージを生成およびコンシュームするアプリケーションの機能を指します。

## この可用性を実現するために必要な考慮事項
アプリケーションの観点からハイレベルの可用性を実現するには、[接続性](/docs/services/EventStreams?topic=eventstreams-sla#connectivity)、[スループット](/docs/services/EventStreams?topic=eventstreams-sla#throughput)、および[メッセージの一貫性と耐久性](/docs/services/EventStreams?topic=eventstreams-sla#message_consistency)を考慮する必要があります。 ユーザーは、ビジネスのこれらの 3 つの要素を最適化するようにアプリケーションを設計する責任があります。

### 接続性
{: #connectivity}

クラウドの動的な性質のため、アプリケーションは接続の切断を予期する必要があります。 接続の切断は、サービスの失敗とは見なされません。

**再試行**<br/>
Kafka クライアントは再接続ロジックを提供しますが、プロデューサーに対してはユーザーが明示的に接続を有効にする必要があります。詳しくは、[ <code>retries</code> プロパティー ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://kafka.apache.org/11/documentation.html#producerconfigs){:new_window} を参照してください。 接続は 60 秒以内に再接続されます。
**重複**<br/>
再試行を有効にすると、メッセージが重複する可能性があります。接続が失われたタイミングによっては、プロデューサーはメッセージがサーバーによって正常に処理されたかどうかを判断できない場合があるため、再接続時にメッセージを再度送信する必要があります。 メッセージの重複を予期したアプリケーションを設計することをお勧めします。 

重複を許容できない場合は、再接続時に重複がないように、<code>idempotent</code> プロデューサー・フィーチャー (Kafka 1.1 以降) を使用できます。 詳しくは、[ <code>enable.idempotence</code> プロパティー ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://kafka.apache.org/11/documentation.html#producerconfigs){:new_window} を参照してください。

### スループット
{: #throughput}

スループットは、クラスター内で送信および受信できる 1 秒あたりのバイト数として表されます。

**標準プランの固有のガイド**<br/>
スループットのガイド情報は、[制限と割り当て量 - 標準](/docs/services/EventStreams?topic=eventstreams-kafka_quotas#kafka_quotas#standard_throughput)を参照してください。 

**エンタープライズ・プランの固有のガイド**<br/>

スループットのガイド情報は、[制限と割り当て量 - エンタープライズ](/docs/services/EventStreams?topic=eventstreams-kafka_quotas#enterprise_throughput)を参照してください。 

**測定**<br/>
アプリケーションを計測するように構成し、そのパフォーマンスに注意することをお勧めします。例えば、送受信されたメッセージの数、メッセージ・サイズ、および戻りコードなどです。 アプリケーションの使用状況を理解すると、トピックに関するメッセージの保存期間など、リソースを適切に構成するのに役立ちます。

**飽和**<br/>
クラスターに生成できるトラフィックの制限に近づくにつれて、プロデューサーのスロットルが開始され、待機時間が増加し、最終的にはタイムアウト・エラーなどのエラーが発生します。構成によっては、メッセージの一貫性と耐久性も影響を受ける可能性があります。 詳しくは、[メッセージの一貫性と耐久性](/docs/services/EventStreams?topic=eventstreams-sla#message_consistency)を参照してください。

### メッセージの一貫性と耐久性
{: #message_consistency}

Kafka は、クラスター内の他のノード全てで受信したメッセージのレプリカを生成することによって可用性と持続性を実現し、障害が発生した場合に使用できるようにします。 {{site.data.keyword.messagehub}} では、3 つのレプリカ (default.replication.factor = 3) を使用します。これは、ノードによって受信された各メッセージのレプリカが、異なるアベイラビリティー・ゾーン内の他の 2 つのノードに生成されることを意味します。 このように、ノードやアベイラビリティー・ゾーンが損失しても、データや機能は失われず、耐性があります。

**プロデューサー <code>acks</code> モード**<br/>
すべてのメッセージはレプリカが生成されますが、アプリケーションは、プロデューサーの <code>acks</code> モード・プロパティーを使用して、プロデュースするメッセージがサービスに転送されるときの耐久性を制御できます。このプロパティーによって、スピードを取るかメッセージ損失のリスクを取るかを選択できます。 デフォルトの設定値は <code>acks=1</code> で、プロデューサーは、レプリカの生成の完了を待たず、接続されているノードがメッセージの受信を確認するとすぐに成功を返すことを意味します。 推奨される最も確実な設定は、メッセージがすべてのレプリカにコピーされた後にプロデューサーが成功を返す <code>acks=all</code> です。 これにより、レプリカ生成が完了した後に次のステップに移るため、障害によってレプリカへの切り替えが発生した場合にメッセージ損失がなくなります。


