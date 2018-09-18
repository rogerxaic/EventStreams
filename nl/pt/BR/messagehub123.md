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

# Usando o {{site.data.keyword.iamshort}} (IAM) com o programa Alpha
{: #alpha_iam }

As permissões do {{site.data.keyword.messagehub}} são configuradas usando políticas do IAM. Uma política do IAM
consiste nas seguintes informações:

* um ID de serviço
* Uu recurso do {{site.data.keyword.Bluemix_short}} que é definido por um nome de serviço, instância de serviço,
região, tipo de recurso do IAM e recursos do IAM
* uma função (leitor, gravador ou gerenciador)

Para obter mais informações sobre o IAM, consulte: [IBM Cloud Identity and Access
Management](/docs/iam/index.html#iamoverview).

Para obter um exemplo sobre como configurar políticas, consulte:
[Introdução aos IDs e chaves
API do IBM Cloud IAM Service ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/blogs/bluemix/2017/10/introducing-ibm-cloud-iam-service-ids-api-keys/){:new_window}.

## Recursos do IAM do {{site.data.keyword.messagehub}}
{: iam_resources}

{{site.data.keyword.messagehub}} expõe quatro tipos de recurso do IAM:

* cluster
* topic
* grupo
* txnid

É possível identificar recursos exclusivos para tópico, grupo e txnid configurando o recurso do IAM. O tipo de recurso de
cluster é um singleton, portanto, não precisa ser identificado exclusivamente.

## Cenários comuns
{: iam_scenarios}

Aqui estão alguns cenários comuns do {{site.data.keyword.messagehub}} e o acesso que você precisa
designar:

Produtor para alguns tópicos (mesmo para idempotent):
* Função de leitor no tipo de recurso de cluster
* Função de gravador em cada tipo de recurso de tópico e recurso de nome do tópico

Produtor transacional:
* Mesmo que produtor
* Função de gravador no tipo de recurso de txnid e no recurso de identificador de transação
* Função de leitor no tipo de recurso do grupo e no recurso do ID do grupo

Consumidor (sem grupo):
* Função de leitor no tipo de recurso de cluster
* Função de leitor em cada tipo de recurso de tópico e recurso de nome do tópico

Consumidor com grupo:
* Igual a consumidor (sem grupo)
* Função de leitor no tipo de recurso do grupo e no recurso do ID do grupo

Criar ou excluir tópico:
* Função de leitor no tipo de recurso de cluster
* Função de gerenciador em cada tipo de recurso do tópico e recurso do nome do tópico

Excluir grupo de consumidores:
* Função de leitor no tipo de recurso de cluster
* Função de gerenciador no tipo de recurso do grupo e recurso de ID do grupo

Listar grupos, tópicos e deslocamentos e descrever configurações de grupo, tópico e broker
* Função de leitor no tipo de recurso de cluster














