---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-30"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Ativando a API do {{site.data.keyword.mql}} no {{site.data.keyword.messagehub}}
{: #mql_enable}

** A API do MQ Light está disponível como parte somente do plano Standard.**
<br/>

**Deve-se criar explicitamente um tópico de Kafka denominado "MQLight" antes de poder usar a API porque todas as mensagens passam pelo tópico "MQLight". Este tópico deve ter uma única partição. A criação deste tópico ativa a API MQ Light para sua instância de serviço. **  

O tópico "MQLight" é usado pela API MQ Light para armazenar seus dados da mensagem e interagir com outros clientes Kafka. Saiba que, quando este tópico é criado, ele incorre em encargos com a taxa padrão descrita no plano de pagamento dos serviços.

Para desativar a API MQ Light, exclua o tópico "MQLight". Observe que todos os dados foram destruídos quando o tópico foi excluído.
