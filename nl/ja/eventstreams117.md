---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-19"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 制限と割り当て量
{: #kafka_quotas }

{{site.data.keyword.messagehub}} は、サービスが使用できるリソース (ネットワーク帯域幅など) を制御するために割り当て量を使用します。 割り当て量のタイプとレベルは、標準プランまたはエンタープライズ・プランのどちらを使用しているかで異なります。

## 標準プラン
{: #limits_standard }

### ネットワーク・スループット
{: #standard_throughput }

各サービス・インスタンスの最大スループットは、パーティション当たり 1 MB/秒であり、最大で 20 MB/秒です。 例えば、10 パーティションのサービス・インスタンスの最大スループットは 10 MB/秒であり、30 パーティションの場合は 20 MB/秒になります。

スループットはプロデューサーとコンシューマーで別個に測定されます。 超過した場合、要求への応答を少し遅らせることで抑制が適用され、帯域幅が減少するまでプロデューサーとコンシューマーにわずかなブレーキが効果的にかかります。

### パーティション
{: #standard_partitions}

各サービス・インスタンスに 100 パーティション。

### 保存
{: #standard_retention}

各パーティションに最大 1 GB。

### その他の制限
{: #standard_limits}

* 最大メッセージ・サイズ: 1 MB
* 同時にアクティブにできる Kafka クライアントの最大数: 100
* 最大要求速度: 1 秒当たり [HTTP Produce API]: 100
* 最大要求速度: 1 秒当たり [HTTP Admin API]: 10

## エンタープライズ・プラン
{: #limits_enterprise }

### ネットワーク・スループット
{: #enterprise_throughput }

推奨最大値は 40 MB/秒で、ピーク制限は 75 MB/秒です。スループットは、クラスター内で送信および受信できる 1 秒あたりのバイト数として表されます。

推奨される数値は、一般的なワークロードに基づいており、内部更新や障害モード (例: アベイラビリティー・ゾーンの喪失) などの操作アクションの影響を考慮します。 平均スループットがこの推奨数値を超えると、これらの状況でパフォーマンスが低下することがあります。


### パーティション
{: #enterprise_partitions}

各サービス・インスタンスに 1,000 パーティション。

### 保存
{: #enterprise_retention}

プランのストレージ制限を上限として制限なし。

### その他の制限
{: #enterprise_limits}

*  最大メッセージ・サイズ: 1 MB
*  同時にアクティブにできる Kafka クライアントの最大数: 10000




















