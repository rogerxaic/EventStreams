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


#암호화 프로토콜
{: #cryptographic}


*  Kafka 네이티브 및 REST 인터페이스에 대한 연결은 TLS 1.2를 사용하여 작성되어야 합니다.
*  연결은 다음과 같은 강력한 암호 스위트로 제한됩니다.

      * ECDHE-RSA-AES128-GCM-SHA256
      * ECDHE-RSA-AES256-GCM-SHA384
      * DHE-RSA-AES128-GCM-SHA256
      * kEDH+AESGCM
      * ECDHE-RSA-AES128-SHA256
      * ECDHE-RSA-AES256-SHA384
      * DHE-RSA-AES128-SHA256
      * DHE-RSA-AES256-SHA256



*  {{site.data.keyword.messagehub}} 대시보드에
액세스하려면 TLS 1.2를 지원하는 브라우저를 사용해야 합니다.
