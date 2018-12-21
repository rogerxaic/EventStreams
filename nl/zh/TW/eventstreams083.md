---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-30"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

<!-- 15/11/18: info moved to eventstreams075.md, moved because of doc app changes -->
# 限制上限
{: #max_limits}

** MQ Light API 只提供於標準方案中。**
<br/>

{{site.data.keyword.mql}} API 有下列強制限制：
{:shortdesc}

* 可以儲存的資料量上限與您付款方案的單一 Kafka 分割區一致（通常是 1 GB）。如果已超出此資料限制，當傳送新的訊息時，會移除分割區中最舊的訊息。
* 可以儲存訊息的時間上限與您付款方案的單一 Kafka 分割區一致（通常是 24 小時）。您無法擷取早於此時段的訊息。
* 訊息（不含標頭）的大小上限是 1 MB。
* 同一時間內可連接的用戶端數目上限是 25。
* 同一時間內可在作用中的目的地數目上限是 25。作用中目的地的定義如下：
  - TimeToLive > 0 的目的地，並且目前具有或沒有連接用戶端。
  - TimeToLive = 0（預設值）的目的地，並且已連接用戶端。 
  <p>在用戶端之間共用的目的地會計算為單一目的地。</p>
