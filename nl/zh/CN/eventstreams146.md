---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-15"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 在经典套餐上迁移 Kafka Java 客户机  
{: #kafka_java_migrating_classic}


## 将 Kafka Java 客户机从 0.9.X 或 0.10.X 迁移到更高客户机版本
{: #kafka_migrate}


如果使用的是 Java 客户机，那么可以使用公共可用的 0.10 或更高版本的 Kafka 客户机。 

强烈建议您从 0.9.X 迁移到最新版本。您可以从 [https://kafka.apache.org/downloads ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://kafka.apache.org/downloads){:new_window} 下载 Kafka 客户机。

有关使用 0.9.X 客户机的影响的信息，请参阅[向后兼容性](/docs/services/EventStreams?topic=eventstreams-kafka_clients#compatibility)。



### 将 Kafka Java 客户机迁移到 0.10.2.X 或更高版本

从 0.10.2 开始，您可以直接在客户机属性中配置 SASL 认证，无需再使用 JAAS 文件。通过这一简化措施，您可以使用不同的凭证集在同一 JVM 中运行多个客户机，而这是使用 JAAS 文件无法实现的。

请完成以下步骤：

1. 删除 JAAS 文件。请注意，也不再需要 JVM 属性 java.security.auth.login.config=<PATH TO JAAS>。
2. 如果是从 0.9.X 进行迁移，请删除 {{site.data.keyword.messagehub}} 登录 JAR 模块。
2. 将以下内容添加到客户机属性：
    ```
	sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USERNAME" password="PASSWORD";
	```

	其中，USERNAME 和 PASSWORD 是 {{site.data.keyword.Bluemix_notm}} 的 {{site.data.keyword.messagehub}} **服务凭证**选项卡中的值。
	
	

### 将 Kafka 客户机从 0.9.X 迁移到 0.10.0.X 或 0.10.1.X

请完成以下步骤：

1. 删除 {{site.data.keyword.messagehub}} 登录 JAR 模块。
2. 将 <code>jaas.conf</code> 文件更改为以下内容：
    ```
        KafkaClient {
          org.apache.kafka.common.security.plain.PlainLoginModule required
          serviceName="kafka"
            username="USERNAME"
            password="PASSWORD";
        };
    ```
    {: codeblock}

	其中，USERNAME 和 PASSWORD 是 {{site.data.keyword.Bluemix_notm}} 的 {{site.data.keyword.messagehub}} **服务凭证**选项卡中的值。
	
3. 将以下行添加到使用者和生产者属性：<code>sasl.mechanism=PLAIN</code>
