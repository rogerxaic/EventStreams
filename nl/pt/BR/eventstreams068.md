---

copyright:
  years: 2015, 2019
lastupdated: "2018-08-02"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Segurança e privacidade de dados
{: #data_security}


O {{site.data.keyword.IBM}} usa os métodos a seguir para ajudar a garantir a segurança e a
privacidade dos seus dados:
{:shortdesc}

## Protocolos criptográficos
{: #cryptographic notoc}


*  Conexões ao Kafka nativo e interfaces REST devem ser feitas usando TLS 1.2.
*  Conexões estão restritas aos seguintes conjuntos de criptografia forte:

      * ECDHE-RSA-AES128-GCM-SHA256
      * ECDHE-RSA-AES256-GCM-SHA384
      * DHE-RSA-AES128-GCM-SHA256
      * kEDH+AESGCM
      * ECDHE-RSA-AES128-SHA256
      * ECDHE-RSA-AES256-SHA384
      * DHE-RSA-AES128-SHA256
      * DHE-RSA-AES256-SHA256
{: shortdesc}


*  Para acessar o painel do {{site.data.keyword.messagehub}}, deve-se usar um navegador que suporte TLS 1.2.
   
## Criptografia de cargas úteis de mensagem, de nomes de tópicos e de grupos de consumidores
{: #encryption_payloads notoc}

Os dados de mensagens são criptografados para transmissões entre o {{site.data.keyword.messagehub}}
e clientes como um resultado de TLS. O {{site.data.keyword.messagehub}} armazena dados da mensagem em espera e logs de mensagem em discos criptografados.

Os nomes de tópicos e de grupos de consumidores são criptografados para transmissão entre
o {{site.data.keyword.messagehub}} e os clientes como um resultado do TLS. No entanto,
o {{site.data.keyword.messagehub}} não criptografa esses valores em repouso. Portanto, não é recomendado usar informações confidenciais nos nomes de tópicos.



