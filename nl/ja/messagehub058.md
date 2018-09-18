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


#暗号プロトコル
{: #cryptographic}


*  Kafka のネイティブ・インターフェースおよび REST インターフェースには、TLS 1.2 を使用して接続する必要があります。
*  接続は、以下の強力な暗号スイートに制限されます。

      * ECDHE-RSA-AES128-GCM-SHA256
      * ECDHE-RSA-AES256-GCM-SHA384
      * DHE-RSA-AES128-GCM-SHA256
      * kEDH+AESGCM
      * ECDHE-RSA-AES128-SHA256
      * ECDHE-RSA-AES256-SHA384
      * DHE-RSA-AES128-SHA256
      * DHE-RSA-AES256-SHA256



*  {{site.data.keyword.messagehub}} ダッシュボードにアクセスするには、TLS 1.2 がサポートされているブラウザーを使用する必要があります。
