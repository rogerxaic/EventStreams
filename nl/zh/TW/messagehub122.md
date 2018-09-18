---

copyright:
  years: 2015, 2018
lastupdated: "2018-04-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

<!-- Notes from chat with Charlie 

Different plan for provisioning

Quality of service from each plan

Life of a user through cycle - APIs, feature sets

-->

# 關於 Alpha 計畫
{: #alpha_about }

Alpha 計畫讓您能提早存取 {{site.data.keyword.messagehub}} 服務中的新特性。現行特性是新計畫的簡介－{{site.data.keyword.messagehub}} 超值方案。

## 超值方案
{: premium_plan}

超值方案是針對效能和其他功能需求超越公用服務的使用者而設計。它提供單一承租戶版本的 {{site.data.keyword.messagehub}} 服務，您可以獨自使用 Apache Kafka 叢集。此版本容許您：

* 完整利用叢集容量和效能

* 大幅提升分割區的限制及數目

* 得益於零管理成本，並具有完全受管理且自動維護與更新的叢集

## 關於您的 Alpha 叢集
{: alpha_cluster}

您的 Alpha 叢集是以 Apache Kafka 1.1 版部署，能夠提供最高每秒 90 000 KB 的通訊量。 

您可以建立最多 1000 個分割區，且每個分割區可以保留最多 1 GB 的資料，最長達到 30 天。為了復原力的緣故，資料會儲存在 3 個抄本之間，每個分割區的已確定偏移會保留最長 7 天。

支援下列兩個 API：

* 針對傳訊，支援 0.10.x 版以及更新版本的 Kafka 用戶端，包括使用 Kafka Streams、Kafka Connect 及 KSQL 的選項。

* 針對管理，可以使用 REST API 來建立、刪除及列出主題。

Alpha 計畫僅適用於美國南部地區。對叢集的存取權是使用 IAM 來管理。

## 連接叢集
{: alpha_connect}

若要在叢集中連接 API，您需要它的端點 URL 和 API 金鑰以進行鑑別。您可以使用下列其中一項方法從 IAM 擷取這些詳細資料：

### Cloud Foundry 應用程式
對於 Cloud Foundry 應用程式：
1. 在應用程式的**連線**標籤（在左邊），按一下**建立連線**按鈕。 
2. 選取您要連接的 {{site.data.keyword.messagehub}} 服務實例，然後按一下**連接**。接受預設選項。 
3. 連線完成時，按一下應用程式的**運行環境**標籤，然後按一下**環境變數**以顯示 **VCAP_SERVICES**。

### 外部應用程式的主控台
從外部應用程式的主控台，使用下列 **bx** 指令建立服務 API 金鑰： 

```
bx resource service-key-create name-of-key Manager --instance-name name-of-your-service
``` 

從產生的資訊複製 <code>kafka_brokers_sasl</code>、<code>kafka_admin_url</code> 和 <code>apikey</code> 欄位。只有前五個分配管理系統會列在 VCAP_SERVICES 中。如果您有超過五個分配管理系統，請使用 Kafka 用戶端來擷取其他分配管理系統的詳細資料。 

## 將用戶端連接至 Kafka API

若要將用戶端連接至 Kafka API，請完成下列步驟：

1. 將用戶端的 <code>bootstrap.servers</code> 內容設為 <code>kafka_brokers_sasl</code> 所列之分配管理系統的逗點區隔清單。

2. 將用戶端 <code>sasl.jaas.config</code> USERNAME 欄位設為 <code>apikey</code> 的前 8 個字元，將 PASSWORD 欄位設為剩餘的字元（在未來的版本不需要進行此分割）。

您使用的 Kafka 用戶端必須支援下列特性：

* 用戶端 0.10.x 版或更新版本

* 使用 SASL Plain 機制的鑑別

* TLS 1.2 版通訊協定的服務名稱識別 (SNI) 延伸規格

這種擷取端點及認證資訊的方法與現有的 {{site.data.keyword.messagehub}} 服務不同。目前針對 {{site.data.keyword.messagehub}} 執行的應用程式需要進行變更，以便反映從 VCAP_SERVICES 要求的替代欄位名稱，以及變更提交給 Kafka 的 username/password 欄位。未來版本的 Alpha 不需要這些變更。

## 將用戶端連接至 REST API

若要將用戶端連接至 REST API，請完成下列步驟：

* REST API 的 URI 提供於 <code>kafka_admin_url</code>

* 將 HTTP <code>Content-Type</code> 標頭設定為 <code>application/json</code>

* 將 HTTP <code>X-Auth-Token</code> 標頭設定為 <code>apikey</code> 的值

如需開始使用 Alpha 的簡單步驟，請參閱[開始使用 Alpha 計畫](/docs/services/MessageHub/messagehub120.html)。


## 管理 {{site.data.keyword.messagehub}}
{: alpha_admin}

叢集中唯一需要的管理作業，是建立、列出及刪除您需要的主題。您可以使用下列其中一項方法來管理：

* 直接來自您應用程式的 Kafka 管理 API。例如，針對 Java，使用 <code>createTopics()</code>、<code>deleteTopics()</code> 或 <code>listTopics()</code> 方法，這些來自 [AdminClient ![外部鏈結圖示](http://kafka.apache.org/11/javadoc/index.html?org/apache/kafka/clients/admin/AdminClient.html){:new_window}。

* 以互動方式，使用 IBM Cloud 入口網站中可用服務實例的 Web 使用者介面。

* 叢集中提供的管理 REST API。

如需管理 REST API（與現有 {{site.data.keyword.messagehub}} 管理 API 相容）所提供之建立、列出及刪除功能的進一步詳細資料，您可以從 [admin-rest-api.yaml ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/ibm-messaging/message-hub-docs/blob/master/admin-rest-api/admin-rest-api.yaml){:new_window} 提供的 API 完整規格找到。若要檢視 swagger 檔案，請使用 Swagger 工具，例如 [Swagger Editor ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://editor.swagger.io/#/){:new_window}。

如需示範如何使用 Curl 建立主題的簡單範例，請參閱[開始使用 Alpha 計畫](/docs/services/MessageHub/messagehub120.html)。

在未來，也將可以使用其他配置選項。


## 範例

即將提供...

如需開始使用 Alpha 的簡單步驟，請參閱[開始使用 Alpha 計畫](/docs/services/MessageHub/messagehub120.html)。

## Alpha 限制
{: alpha_limitations}

此 Alpha 計畫的現行限制如下所示：

### 尚無法使用，但即將提供

* 多可用性區域的支援

* 使用者控制的擴充及負載限制

* 與 {{site.data.keyword.cloudaccesstrailfull_notm}} 服務（先前稱為 AccessTrail）的整合 

### 目前未規劃

* 橋接器

* REST 傳訊

* MQ Light API










