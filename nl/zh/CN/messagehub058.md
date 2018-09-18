---

copyright:
  years: 2015, 2018
lastupdated: "2016-11-22"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


#加密协议
{: #cryptographic}


*  必须使用 TLS 1.2 建立与 Kafka 本机和 REST 接口的连接。
*  连接仅限于以下强密码套件：

      * ECDHE-RSA-AES128-GCM-SHA256
      * ECDHE-RSA-AES256-GCM-SHA384
      * DHE-RSA-AES128-GCM-SHA256
      * kEDH+AESGCM
      * ECDHE-RSA-AES128-SHA256
      * ECDHE-RSA-AES256-SHA384
      * DHE-RSA-AES128-SHA256
      * DHE-RSA-AES256-SHA256



*  要访问 {{site.data.keyword.messagehub}} 仪表板，您必须使用支持 TLS 1.2 的浏览器。
