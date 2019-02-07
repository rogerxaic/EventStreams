---

copyright:
  years: 2015, 2019
lastupdated: "2018-07-05"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 使用管理 Kafka Java 用戶端 API
{: #kafka_java_api}


<!-- 
17/10/17 - Karen: following info duplicated at messagehub108
 -->

如果您使用 Kafka 用戶端 0.11 或更新版本，或 Kafka Streams 0.10.2.0 或更新版本，可以使用 API 來建立及刪除主題。我們已對您建立主題時接受的設定做了一些限制。目前，您只能修改下列設定：
{: shortdesc}

<dl>
<dt>cleanup.policy</dt>
<dd>設為 <code>delete</code>（預設值）、<code>compact</code> 或 <code>delete,compact</code>
<p>**附註：**如果清理原則是僅 <code>compact</code>，我們會自動新增 <code>delete</code> 但停用根據時間刪除。主題中的訊息在刪除之前最多可壓縮到 1 GB。</p>
</dd>

<dt>retention.ms</dt>
<dd>預設保留期間是 24 小時。最短 1 小時，最長 30 天。請以小時的倍數來指定此值。



<p>**附註：**在企業方案中，您可以將此設定為任何值。</p>
</dd>

<dt>retention.bytes</dt>
<dd>在我們捨棄舊日誌區段以釋放空間之前，分割區（由日誌區段所組成）可以成長到的大小上限。

<p>**附註：**僅限企業方案。請設為大於 1 MB 的任何值。</p>
</dd>

<dt>segment.bytes</dt>
<dd>日誌的區段檔案大小。

<p>**附註：**僅限企業方案。請設為大於 100 KB 的任何值。</p>
</dd>

<dt>segment.index.bytes</dt>
<dd>將偏移對映至檔案位置的索引大小。 

<p>**附註：**僅限企業方案。請設為 100 KB 到 2 GB 之間的任何值。</p>
</dd>

<dt>segment.ms</dt>
<dd>時段，在此時段之後，Kafka 將強制日誌滾動，即使區段檔案尚未填滿。 

<p>**附註：**僅限企業方案。請設為 5 分鐘到 30 天之間的任何值。</p>
</dd>
</dl>

