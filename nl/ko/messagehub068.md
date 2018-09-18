---

copyright:
  years: 2015, 2018
lastupdated: "2017-12-08"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


#데이터 보안 및 개인정보 보호정책
{: #data_security}


{{site.data.keyword.IBM}}에서는 다음 방법을 사용하여 사용자 데이터의 보안 및 개인정보 보호정책을 보장합니다.
{:shortdesc}

## 암호화 프로토콜
{: #cryptographic notoc}


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
   
## 메시지 페이로드, 토픽 이름, 이용자 그룹의 암호화
{: #encryption_payloads notoc}

메시지 데이터는 TLS의 결과에 따라 {{site.data.keyword.messagehub}}와
클라이언트 간의 전송을 위해 암호화됩니다. {{site.data.keyword.messagehub}}는 저장 메시지 데이터 및
메시지 로그를 암호화된 디스크에 저장합니다.

토픽 이름 및 이용자 그룹은 TLS의 결과에 따라 {{site.data.keyword.messagehub}}와
클라이언트 간의 전송을 위해 암호화됩니다. 하지만 {{site.data.keyword.messagehub}}는
저장 시 이러한 값을 암호화하지 않습니다.



