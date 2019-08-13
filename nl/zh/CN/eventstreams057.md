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
{:note: .note}


# 已知限制
{: #restrictions}

如果您在使用 {{site.data.keyword.messagehub}} 时发现问题，请查看这些已知限制和变通方法。
{: shortdesc}

## Java Kafka 调用不会在 Kafka 引导程序服务器发生故障时进行故障转移
{: #calls_failover}

### 问题
{: #calls_failover_problem notoc}

Java 虚拟机 (JVM) 会将 DNS 查找进行高速缓存。JVM 解析主机名的 IP 地址时，将在指定时间段（称为生存时间 (TTL)）内将 IP 地址进行高速缓存。有些 Java 配置对 JVM TTL 进行设置，使其在 JVM 重新启动之前从不更新主机名的 IP 地址。例如，具有安全管理器的配置。

### 变通方法
{: #calls_failover_workaround notoc}

因为 {{site.data.keyword.messagehub}} 使用 Kafka 引导程序服务器 URL 和多个 IP 地址实现高可用性，并不是所有代理程序 IP 地址为 Kafka 客户机所知，从而防止故障转移到正常运行的代理程序。在这些情况下，故障转移需要重新查询 IP 地址，代理程序 URL 才能获取有效的 IP 地址。建议您将 JVM 的 TTL 值配置为 30 到 60 秒。此值确保在引导程序服务器的 IP 地址出现问题时，Kafka 客户机能够通过查询 DNS 来查找并使用新的 IP 地址。

从 <code>java.security</code> 文件： 

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
{: #jvm_ttl notoc}
* 要针对所有应用程序修改 JVM 的 TTL，请设置 <code><var class="keyword varname">$JAVA_HOME</var>/jre/lib/security/java.security</code> 文件中的 <code>networkaddress.cache.ttl</code> 值。
* 要修改给定应用程序的 JVM TTL，请设置应用程序代码中的 <code>networkaddress.cache.ttl</code>，如下所示：
```
java.security.Security.setProperty("networkaddress.cache.ttl" , "30");
```

## Java Kafka 调用可能超时
{: #calls_timeout_kafka}

### 问题
{: #calls_timeout_problem notoc}

有时候，Kafka Java 客户机调用无法找到 Kafka。故障的原因是 Kafka 客户机认定每个引导程序服务器使用相同的故障 IP 地址。Kafka 客户机尝试每个代理程序的 IP 地址（是同一个故障 IP 地址），错误地认为 Kafka 已停机。请注意，如果 DNS 查询中返回多个 IP 地址，Kafka 客户机将使用列表中返回的第一个 IP 地址。

### 变通方法
{: #calls_timeout_workaround notoc}

等待足够长的时间，直到代理程序 URL 的 JVM DNS 高速缓存到期，然后重试调用。在后续的 Kafka 调用中，应该会从 DNS 查询返回有效的代理程序 IP 地址，并加以使用。 

已经创建了 Kafka 改进建议 (KIP) #302，以确保 Kafka 客户机尝试所有可用代理程序 IP 地址，而不是一部分 IP 地址，所以单个 IP 地址中的故障不会导致失败。


## 主题和分区
{: #topics_partitions}

*  主题名称限制为最多 100 个字符。
*  一个主题的缺省分区数为一个。
*  每个 {{site.data.keyword.Bluemix_notm}} 空间的分区数限制为 100 个。要创建更多的分区，必须使用新的 {{site.data.keyword.Bluemix_notm}} 空间。

<!--following message retention info duplicted in FAQs eventstreams108-->

## 消息保留时间
{: #message_retention}

缺省情况下，消息在 Kafka 中最多保留 24 小时，并且每个分区的上限是 1 GB。如果达到 1 GB 上限，那么将废弃最旧的消息以防超出限制。

使用用户界面或管理 API 创建主题时，可以更改消息保留时间的限制。时间限制最短为 1 小时，最长为 30 天。

有关使用 Kafka 客户机或 Kafka Streams 创建主题时所允许设置的限制的信息，请参阅[如何使用 Kafka API 创建和删除主题？](/docs/services/EventStreams?topic=eventstreams-faqs#topic_admin)。

## 在 Kafka 中创建和删除主题
{: #create_delete}

在 Kafka 中，主题的创建和删除是异步操作，可能需要一些时间才能完成。建议避免采用依赖于快速创建和删除主题的使用模式，或依赖于快速删除并重新创建主题的使用模式。

<!--
## Kafka REST API
{: #trouble_rest}

<br/>
**Is this specific to old Standard only? If so I'll move to specific Standard topic.**
{: note}

*  Only the binary-embedded format is supported for requests and
   responses. The Avro and JSON embedded formats are not supported.
*  Concurrent requests are not supported for a consumer instance.
   Read, commit, or delete requests corresponding to a consumer
   instance should be sent only after a response is received for
   any outstanding requests of that instance.

-->
<!--
<br/>
**Is this specific to old Standard only? If so I'll move to specific Standard topic.**
{: note}

## Kafka REST API rate limitation
{: #kafka_rate}

Applications using the Kafka REST API can be subject to rate
limiting for each ApiKey. When this limiting occurs, the API
responds with the following HTTP error:

<code>429 Too Many Requests</code>
{:screen}

If you see this error, wait and submit the request again.

<br/>
**Is this specific to old Standard only? If so I'll move to specific Standard topic.**
{: note}
-->
<!--12/04/18 - Karen: same info duplicated at messagehub108 -->
<!--
## Kafka REST API daily restart
{: #rest_restart}

The Kafka REST API restarts once a day for a short period of
time. During this period, the Kafka REST API might become
unavailable. If this happens, you are recommended to retry your
request. After the REST API has restarted, you will have to
create your Kafka consumer instances again. If this is the case, the
REST API returns the following JSON:

```'{"error_code":40403,"message":"Consumer instance not found."}'
```
{:screen}
-->
<!--
## Kafka high-level consumer API
{: #kafka_consumer}

You cannot use the Apache Kafka 0.8.2 simple or high-level
consumer API with {{site.data.keyword.messagehub}}. Instead, you can use the earliest supported Kafka consumer API, which is 0.10.
-->
