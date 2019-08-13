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


# 限制和配額
{: #kafka_quotas }

{{site.data.keyword.messagehub}} 使用配額控制資源，例如服務可以使用的網路頻寬。配額的類型和層次取決於您使用的是標準方案還是企業方案。

## 標準方案
{: #limits_standard }

### 網路傳輸量
{: #standard_throughput }

每個服務實例的最大傳輸量相當於每個分割區每秒 1 MB，最多每秒 20 MB。例如，對於有 10 個分割區的服務實例，最大傳輸量是每秒 10 MB，對於 30 個分割區，為每秒 20 MB。

將針對生產者和消費者分別度量傳輸量。如果超過此值，將套用節流控制，方法是稍微延遲對要求的回應，以便有效地對生產者和消費者溫和地調整制動器，直到頻寬減少。

### 分割區
{: #standard_partitions}

每個服務實例 100 個分割區。

### 保留
{: #standard_retention}

每個分割區上限 1 GB。

### 其他限制
{: #standard_limits}

* 訊息大小上限：1 MB
* 最大並行作用中 Kafka 用戶端數：100
* 每秒最大要求率 [HTTP Produce API]: 100
* 每秒最大要求率 [HTTP Admin API]: 10

## 企業方案
{: #limits_enterprise }

### 網路傳輸量
{: #enterprise_throughput }

建議的最大傳輸量為每秒 40 MB（每秒尖峰限制為 75 MB）。傳輸量表示成在叢集中可以傳送及接收的每秒位元組數。

建議的數字是根據一般工作負載，並考量作業動作可能的影響，例如內部更新或失敗模式（像是失去可用性區域）。如果平均傳輸量超過建議數字，在這些狀況下可能會遇到效能下降的情況。


### 分割區
{: #enterprise_partitions}

每個服務實例 1000 個分割區。

### 保留
{: #enterprise_retention}

無限制，直至達到方案的儲存空間限制。

### 其他限制
{: #enterprise_limits}

*  訊息大小上限：1 MB
*  最大並行作用中 Kafka 用戶端數：10000




















