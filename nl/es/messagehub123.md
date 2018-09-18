---

copyright:
  years: 2015, 2018
lastupdated: "2018-04-19"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Uso de {{site.data.keyword.iamshort}} (IAM) con el programa Alpha
{: #alpha_iam }

Los permisos de {{site.data.keyword.messagehub}} están configurados utilizando políticas de IAM. Una política de IAM consta de la información siguiente:

* un ID de servicio
* un recurso de {{site.data.keyword.Bluemix_short}} definido por un nombre de servicio, una instancia de servicio, una región, un tipo de recurso de IAM y un recurso de IAM
* un rol (Lector, Escritor o Gestor)

Para obtener más información sobre IAM, consulte:
[IBM Cloud Identity and Access Management](/docs/iam/index.html#iamoverview).

Para obtener un ejemplo sobre cómo establecer las políticas, consulte:
[Introducción a los ID de servicio y a las claves de API de IAM de IBM Cloud ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/blogs/bluemix/2017/10/introducing-ibm-cloud-iam-service-ids-api-keys/){:new_window}.

## Recursos de IAM de {{site.data.keyword.messagehub}}
{: iam_resources}

{{site.data.keyword.messagehub}} expone cuatro tipos de recursos de IAM:

* clúster
* tema
* grupo
* txnid

Puede identificar recursos exclusivos por tema, grupo y txnid estableciendo el recurso de IAM. El tipo de recurso de clúster es un singleton, por lo que no necesita identificarse de forma exclusiva.

## Casos de ejemplos habituales
{: iam_scenarios}

Aquí hay algunos casos de ejemplo habituales de {{site.data.keyword.messagehub}} y el acceso que necesita asignar:

Productor a algunos temas (lo mismo para idempotent):
* Rol de lector en el tipo de recurso de clúster
* Rol de escritor en cada tipo de recurso de tema y en el recurso de nombre de tema

Productor transaccional:
* Lo mismo que productor
* Rol de escritor en el tipo de recurso txnid y el recurso de ID de transacción
* Rol de lector en el tipo de recurso de grupo y el recurso de ID de grupo

Consumidor (sin grupo):
* Rol de lector en el tipo de recurso de clúster
* Rol de lector en cada tipo de recurso de tema y en el recurso de nombre de tema

Consumidor con grupo:
* Lo mismo que consumidor (sin grupo)
* Rol de lector en el tipo de recurso de grupo y el recurso de ID de grupo

Crear o suprimir el tema:
* Rol de lector en el tipo de recurso de clúster
* Rol de gestor en cada tipo de recurso de tema y en el recurso de nombre de tema

Suprimir el grupo de consumidor:
* Rol de lector en el tipo de recurso de clúster
* Rol de gestor en el tipo de recurso de grupo y en el recurso de ID de grupo

Liste grupos, temas y desplazamientos y describa configuraciones de grupos, temas e intermediarios
* Rol de lector en el tipo de recurso de clúster














