---

copyright:
  years: 2015, 2019
lastupdated: "2018-10-19"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 已知限制
{: #restrictions}

如果您在使用 {{site.data.keyword.messagehub}} 時發現問題，請檢閱這些已知限制和暫行解決方法。
{: shortdesc}

## 如果 Kafka 引導伺服器失敗，Java Kafka 呼叫不會進行失效接手
{: #calls_failover}

### 問題
{: #calls_failover_problem notoc}

Java 虛擬機器 (JVM) 會快取 DNS 查閱。當 JVM 解析主機名稱的 IP 位址時，它會將 IP 位址快取一段指定的時間，稱為存活時間 (TTL)。部分 Java 配置會設定 JVM TTL，因此在 JVM 重新啟動之前不會再重新整理主機名稱的 IP 位址。配置範例是具有安全管理程式的配置。

### 暫行解決方法
{: #calls_failover_workaround notoc}

因為 {{site.data.keyword.messagehub}} 使用 Kafka 引導伺服器 URL 搭配多個 IP 位址以取得高可用性，所以不是所有分配管理系統 IP 位址都為 Kafka 用戶端所知，如此便導致無法失效接手到有效的分配管理系統。在這些情況下，失效接手需要重新查詢分配管理系統 URL 的 IP 位址，以取得有效的 IP 位址。建議您將 JVM 配置為具有 30 到 60 秒的 TTL 值。此值可確定如果引導伺服器的 IP 位址有問題，Kafka 用戶端將可以藉由查詢 DNS 來查閱並使用新的 IP 位址。

從 <code>java.security</code> 檔案： 

```
# The Java-level namelookup cache policy for successful lookups:
#
# any negative value: caching forever
# any positive value: the number of seconds to cache an address for
# zero: do not cache
#
# default value is forever (FOREVER). For security reasons, this
# caching is made forever when a security manager is set. When a security
# manager is not set, the default behavior in this implementation
# is to cache for 30 seconds.
#
# NOTE: setting this to anything other than the default value can have
#       serious security implications. Do not set it unless
#       you are sure you are not exposed to DNS spoofing attack.
#
#networkaddress.cache.ttl=-1
```

### 如何修改 JVM 的 TTL
* 若要針對所有應用程式修改 JVM 的 TTL，請在 <code><var class="keyword varname">$JAVA_HOME</var>/jre/lib/security/java.security</code> 檔中設定 <code>networkaddress.cache.ttl</code> 值。
* 若要針對給定的應用程式修改 JVM TTL，請在您的應用程式碼設定 <code>networkaddress.cache.ttl</code>，如下所示：
```
java.security.Security.setProperty("networkaddress.cache.ttl" , "30");
```

## Java Kafka 呼叫可能逾時
{: #calls_timeout}

### 問題
{: #calls_timeout_problem notoc}

有時 Kafka Java 用戶端呼叫找不到 Kafka。失敗的原因是 Kafka 用戶端為每個引導伺服器判斷相同的失敗 IP 位址。Kafka 用戶端會嘗試每個分配管理系統的 IP 位址（也就是相同的失敗 IP 位址），然後不正確地判斷 Kafka 關閉。請注意，如果在 DNS 查詢中傳回多個 IP 位址，Kafka 用戶端會使用清單中傳回的第一個 IP 位址。

### 暫行解決方法
{: #calls_timeout_workaround notoc}

等待足夠長的時間，讓分配管理系統 URL 的 JVM DNS 快取到期之後，重試您的呼叫。在後續的 Kafka 呼叫中，應該從 DNS 查詢傳回有效的分配管理系統 IP 位址並且使用它。 

已建立 Kafka 改善提案 (KIP) #302 以確定 Kafka 用戶端會嘗試所有可用的分配管理系統 IP 位址而非子網路，因為單一 IP 位址的失敗不會導致失敗。


## 主題及分割區
{: #topics_partitions}

*  主題名稱限制最多為 100 個字元。
*  一個主題的預設分割區數目是一 (1)。
*  每個 {{site.data.keyword.Bluemix_notm}} 空間有 100 個分割區的限制。若要建立更多分割區，您必須使用新的 {{site.data.keyword.Bluemix_notm}} 空間。

## 訊息保留
{: #message_retention}

依預設，訊息在 Kafka 中的保留時間為最長 24 小時，每個分割區的上限為 1 GB。如果到達 1 GB 的限制，將會捨棄最舊的訊息，以維持在限制之內。

當您以使用者介面或管理 API 建立主題時，可以變更訊息保留的時間限制。時間限制為最短一小時，最長 30 天。

如需使用 Kafka 用戶端或 Kafka Streams 建立主題時所接受設定之限制的相關資訊，請參閱[使用 Kafka API](/docs/services/EventStreams?topic=eventstreams-kafka_using)。

## 在 Kafka 中建立及刪除主題
{: #create_delete}

在 Kafka 中，主題建立及刪除是非同步作業，可能需要一些時間才能完成。建議您避免使用依賴快速建立及刪除主題的模式，或是使用依賴快速刪除並重建主題的模式。

## Kafka REST API
{: #trouble_rest}

*  要求及回應只支援二進位內嵌格式。不支援 Avro 及 JSON 內嵌格式。
*  消費者實例不支援並行要求。對應於消費者實例的讀取、確定或刪除要求，應該只能在收到該實例之任何未完成要求的回應之後傳送。

## Kafka REST API 速率限制
{: #kafka_rate}

使用 Kafka REST API 的應用程式，可能受限於每個 ApiKey 的速率限制。發生此限制時，API 會回應下列 HTTP 錯誤：

<code>429 要求太多</code>
{:screen}

如果您看到此錯誤，請稍候，然後重新提交要求。

<!--12/04/18 - Karen: same info duplicated at messagehub108 -->
## Kafka REST API 每日重新啟動
{: #rest_restart}

Kafka REST API 每天會重新啟動一次，需要一小段時間。在此期間內，Kafka REST API 可能變成無法使用。如果發生這種情況，建議您重試要求。REST API 重新啟動之後，您需要重建 Kafka 消費者實例。如果是這種情況，REST API 會傳回下列 JSON：

```'{"error_code":40403,"message":"Consumer instance not found."}'
```
{:screen}

## Kafka 高階消費者 API
{: #kafka_consumer}

您無法使用 Apache Kafka 0.8.2 簡單或高階消費者 API 與 {{site.data.keyword.messagehub}} 搭配。您可以改用最早版本的受支援 Kafka 消費者 API，也就是 0.9。
