---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-02"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


#Seguridad y privacidad de datos
{: #data_security}


{{site.data.keyword.IBM}} utiliza los métodos siguientes para asegurar la
seguridad y privacidad de sus datos:
{:shortdesc}

## Protocolos criptográficos
{: #cryptographic notoc}


*  Las conexiones a Kafka nativo y a las interfaces REST
deben hacerse mediante TLS 1.2.
*  Las conexiones están restringidas a las siguientes
suites de cifrado fuerte:

      * ECDHE-RSA-AES128-GCM-SHA256
      * ECDHE-RSA-AES256-GCM-SHA384
      * DHE-RSA-AES128-GCM-SHA256
      * kEDH+AESGCM
      * ECDHE-RSA-AES128-SHA256
      * ECDHE-RSA-AES256-SHA384
      * DHE-RSA-AES128-SHA256
      * DHE-RSA-AES256-SHA256



*  Para acceder al panel de control
de
{{site.data.keyword.messagehub}},
debe utilizar un navegador que admita TLS 1.2.
   
## Cifrado de cargas útiles de mensajes, nombres de tema y grupos de consumidores
{: #encryption_payloads notoc}

Los datos de los mensajes se cifran para la
transmisión entre
{{site.data.keyword.messagehub}}
y los clientes como resultado de TLS. {{site.data.keyword.messagehub}} almacena datos de mensajes en reposo y registros de mensajes en discos cifrados.

Los nombres de tema y grupos de consumidores se cifran para la transmisión entre {{site.data.keyword.messagehub}} y los clientes como resultado de TLS. Sin embargo, {{site.data.keyword.messagehub}} no cifra estos valores en el resto. Por lo tanto, no se le recomienda que utilice información confidencial en los nombres de los temas.



