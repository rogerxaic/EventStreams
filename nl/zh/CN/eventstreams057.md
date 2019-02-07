---

copyright:
  years: 2015, 2019
lastupdated: "2018-06-26"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


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
* 要针对所有应用程序修改 JVM 的 TTL，请设置 <code><var class="keyword varname">$JAVA_HOME</var>/jre/lib/security/java.security</code> 文件中的 <code>networkaddress.cache.ttl</code> 值。
* 要修改给定应用程序的 JVM TTL，请设置应用程序代码中的 <code>networkaddress.cache.ttl</code>，如下所示：
```
java.security.Security.setProperty("networkaddress.cache.ttl" , "30");
```

## Java Kafka 调用可能超时
{: #calls_timeout}

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

## 消息保留时间
{: #message_retention}

缺省情况下，消息在 Kafka 中最多保留 24 小时，并且每个分区的上限是 1 GB。如果达到 1 GB 上限，那么将废弃最旧的消息以防超出限制。

使用用户界面或管理 API 创建主题时，可以更改消息保留时间的限制。时间限制最短为 1 小时，最长为 30 天。

有关使用 Kafka 客户机或 Kafka Streams 创建主题时允许的设置限制的信息，请参阅[主题管理 API](/docs/services/EventStreams/eventstreams104.html)。

## 在 Kafka 中创建和删除主题
{: #create_delete}

在 Kafka 中，主题的创建和删除是异步操作，可能需要一些时间才能完成。建议避免采用依赖于快速创建和删除主题的使用模式，或依赖于快速删除并重新创建主题的使用模式。

## Kafka REST API
{: #trouble_rest}

*  仅支持二进制嵌入格式的请求和响应。不支持 Avro 和 JSON 嵌入格式。
*  使用者实例不支持并行请求。只有接收到使用者实例的任何待处理请求的响应之后，才可以读取、落实或删除该使用者实例对应的请求。

## Kafka REST API 速率限制
{: #kafka_rate}

使用 Kafka REST API 的应用程序可能会受每个 ApiKey 的速率限制的影响。达到此限制时，API 会使用以下 HTTP 错误进行响应：

<code>429 Too Many Requests</code>
{:screen}

如果看到此错误，请稍后重新提交请求。

<!--12/04/18 - Karen: same info duplicated at messagehub108 -->
## Kafka REST API 每日重新启动
{: #rest_restart}

Kafka REST API 每天重新启动一次，重新启动需要很短的一段时间。在此时间段内，Kafka REST API 可能会变得不可用。如果发生此情况，建议重试请求。重新启动 REST API 后，必须重新创建 Kafka 使用者实例。否则，REST API 会返回以下 JSON：

```'{"error_code":40403,"message":"Consumer instance not found."}'
```
{:screen}

## Kafka 高级别使用者 API
{: #kafka_consumer}

不能将 Apache Kafka 0.8.2 简单或高级别使用者 API 与 {{site.data.keyword.messagehub}} 配合使用。但可以使用 Kafka 0.9 使用者 API，
