---

copyright:
  years: 2015, 2019
lastupdated: "2018-09-11"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}



# 向 {{site.data.keyword.messagehub}} 團隊報告問題 - 經典方案
{: #report_problem_classic}

如果您遇到 {{site.data.keyword.messagehub}} 的問題，請先檢查 [{{site.data.keyword.Bluemix_notm}} 狀態頁面 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/status?selected=status){:new_window}。 

如果您需要 {{site.data.keyword.messagehub}} 團隊的協助，請收集下列所有資訊。您可以事先提供的資訊越多，團隊就越可更有效率地協助解決問題：
{:shortdesc}

1. 您的 {{site.data.keyword.messagehub}} 佈建在哪個 {{site.data.keyword.Bluemix_notm}} 位置（地區）？例如達拉斯或倫敦。 
2. 您在哪個介面看到問題？Kafka、REST、AMQP 或橋接器？
3. 何時第一次發生問題（明確的時間、日期和時區）？在這之前，應用程式執行多久了？
4. 問題是否仍在發生？可以重現嗎？
5. 您正在使用的 {{site.data.keyword.messagehub}} 服務實例 ID 為何？您可以查看服務之 VCAP_SERVICES JSON 的 "instance_id" 欄位，找到這個 ID。例如：
 ```
 "instance_id": "e662a61b-d1ff-4cce-aab9-5eae9adb9827"
 ```
6. 您的應用程式使用哪個 Kafka 或 REST 用戶端？版本詳細資料為何？
7. 您的用戶端配置詳細資料為何？
8. 您有顯示該問題的應用程式日誌 Snippet 嗎？
9. 您看到的問題為何？哪些主題、用戶端 ID 和群組 ID 受到影響？
10. 問題對您的服務有什麼影響？


您可以[提交支援要求 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/get-support?topic=get-support-getting-customer-support#using-avatar){:new_window} 在支援問題單中提供收集的資訊給 IBM。










