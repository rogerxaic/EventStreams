---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-13"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:faq: data-hd-content-type='faq'}

# Perguntas mais frequentes para o plano Clássico 
{: #faqs_classic}

Respostas para perguntas comuns sobre o serviço {{site.data.keyword.IBM}}{{site.data.keyword.messagehub}} para o plano Clássico.

Para obter respostas para perguntas relacionadas a todos os planos do {{site.data.keyword.messagehub}}, consulte [Perguntas mais frequentes](/docs/services/EventStreams?topic=eventstreams-faqs#faqs).
{: shortdesc}

<!--17/10/17 - Karen: same info duplicated at messagehub104 -->
## Como uso as APIs Kafka para criar e excluir tópicos?
{: #topic_admin_classic}
{: faq}

Se estiver usando um cliente Kafka em 0.11 ou mais recente ou o Streams Kafka em 0.10.2.0 ou
mais recente, será possível utilizar APIs para criar e excluir tópicos. Colocamos algumas restrições nas
configurações permitidas ao criar tópicos. Atualmente, é possível modificar somente as configurações
a seguir:

<dl>
<dt>cleanup.policy</dt>
<dd>Configure para <code>excluir</code> (padrão), <code>compact</code> ou <code>delete,compact</code>
<p>**Nota:** se a política de limpeza for somente <code>compact</code>, nós automaticamente incluiremos <code>delete</code>, mas desativaremos a exclusão com base no tempo. As mensagens
no tópico são compactadas até 1 GB antes de serem excluídas.</p>
</dd>

<dt>retention.ms</dt>
<dd>O período de retenção padrão é de 24 horas. O mínimo é uma hora e o máximo é 30 dias. Especifique esse
valor como múltiplos de horas.
</dd>
</dl>


## Por quanto tempo o {{site.data.keyword.messagehub}} configura a janela de retenção de log para o tópico de compensações do consumidor?
{: #offsets_classic }
{: faq}

O {{site.data.keyword.messagehub}} retém as compensações do consumidor por sete dias. Isso corresponde
à configuração offsets.retention.minutes do Kafka. 

A retenção de compensação é feita no sistema inteiro, portanto, não é possível configurá-la
em um nível de tópico individual. Todos os grupos de consumidores obtêm somente 7 dias de compensações armazenadas, mesmo se usando um tópico com uma retenção de log que foi aumentada para
o máximo de 30 dias. 

<!--following message retention info duplicted in eventstreams057 and evenstreams108-->

## Quanto tempo as mensagens ficam retidas?
{: #messages_retained_classic}

Por padrão, as mensagens são retidas no Kafka por até 24 horas e cada partição é limitada a 1 GB. Se um valor máximo de 1 GB for atingido, as mensagens mais antigas serão descartadas para permanecerem
no limite.

É possível mudar o limite de tempo para retenção mensagem ao criar um tópico usando a interface
com o usuário ou a API de administração. O limite de tempo é um mínimo de uma hora e um máximo de 30 dias.

Para obter informações sobre restrições das configurações permitidas ao criar tópicos usando um cliente Kafka ou o Kafka Streams, consulte [Como eu uso as APIs do Kafka para criar e excluir tópicos?](/docs/services/EventStreams?topic=eventstreams-faqs_classic#topic_admin_classic).


## O que é comportamento de disponibilidade do {{site.data.keyword.messagehub}}?
{: #availability_classic}
{: faq}

Se você gravar aplicativos do {{site.data.keyword.messagehub}}, use estas informações para
entender o que é um comportamento normal de disponibilidade do
{{site.data.keyword.messagehub}} e o que seus aplicativos esperam manipular.

### APIs
{: #api_availability_classic }

Como parte da operação regular do {{site.data.keyword.messagehub}}, os nós do cluster do Kafka
são reiniciados ocasionalmente.
Em alguns casos, seus aplicativos se tornam perceptivos conforme o cluster redesigna recursos. Grave seus
aplicativos para que sejam resilientes a essas mudanças e capazes de se reconectarem e tentarem novamente as
operações.

### Pontes de {{site.data.keyword.messagehub}} 
{: #bridge_availability_classic }

Grave seus aplicativos para manipular a possibilidade de que pontes possam reiniciar ocasionalmente.

## Qual é o tamanho máximo da mensagem do {{site.data.keyword.messagehub}}? 
{: #max_message_size_classic }
{: faq}

O tamanho máximo de mensagem do {{site.data.keyword.messagehub}} é 1 MB, que é o padrão do Kafka. 

## O que são as configurações de replicação do {{site.data.keyword.messagehub}}? 
{: #replication_classic }
{: faq}

O {{site.data.keyword.messagehub}} é configurado para fornecer disponibilidade e
durabilidade fortes.
As seguintes definições de configuração se aplicam a todos os tópicos e não podem mudar:
* replication.factor = 3
* min.insync.replicas = 2

## Como o faturamento do {{site.data.keyword.messagehub}} funciona no plano Clássico? 
{: #billing_classic }
{: faq}

O {{site.data.keyword.messagehub}} no plano Clássico executa regularmente amostragens da contagem de tópico de um usuário e o {{site.data.keyword.Bluemix_notm}} registra o valor máximo de amostra a cada dia. O {{site.data.keyword.messagehub}} cobra pelo número máximo de partições simultâneas vistas e pela
soma de mensagens que são enviadas e recebidas diariamente.

Por exemplo, se você cria e exclui 1 tópico 10 vezes em um dia, você é cobrado pelo máximo de 1
tópico. No entanto, se você cria 10 tópicos e os exclui, talvez você seja cobrado por 0 ou 10
tópicos, dependendo de quando a amostragem ocorre.

O {{site.data.keyword.messagehub}} cobra por mensagem ou a cada 64 k. Uma mensagem de até 64 k
é cobrada como 1 mensagem faturável. Já as mensagens maiores que 64 k são contadas como o seguinte número de
mensagens faturáveis: <code><var class="keyword varname">message_size</var> &divide; 64 k</code>.

<!--12/04/18 - Karen: same info duplicated at messagehub057 -->
## Com que frequência a API de REST do Kafka é reiniciada? 
{: #REST_restart_classic }
{: faq}

A API de REST do Kafka reinicia uma vez por dia por um curto período de tempo. 

Durante esse período, a
API de REST do Kafka pode se tornar indisponível. Se isso acontecer, é recomendado tentar novamente
sua solicitação. Após a API de REST ser reiniciada, será necessário recriar
suas instâncias do consumidor do Kafka. Se este for o caso, a API de REST retornará o JSON a seguir:

```'{"error_code":40403,"message":"Consumer instance not found."}'
```
{:screen}

## Quais são as diferenças entre os planos do {{site.data.keyword.messagehub}}?
{: #plan_compare_classic }
{: faq}

Para localizar mais informações sobre os outros planos do {{site.data.keyword.messagehub}}, consulte [Escolhendo seu plano](/docs/services/EventStreams?topic=eventstreams-plan_choose). Para obter informações sobre o plano Clássico, consulte [Visão geral do plano Clássico](/docs/services/EventStreams?topic=eventstreams-plan_choose_classic#plan_choose_classic).


## Como eu manipulo a recuperação de desastre?
{: #disaster_recovery_classic }
{: faq}

Atualmente, é responsabilidade do usuário gerenciar sua própria recuperação de desastre do {{site.data.keyword.messagehub}}. Os dados do {{site.data.keyword.messagehub}} podem ser replicados entre uma instância do {{site.data.keyword.messagehub}} em uma localização (região) e outra instância em uma localização diferente. No entanto, o usuário é responsável por provisionar uma instância remota do {{site.data.keyword.messagehub}} e gerenciar a replicação.

O usuário também é responsável pelo backup dos dados de carga útil da mensagem. Embora esses dados sejam replicados por múltiplos brokers do Kafka dentro de um cluster, o que protege contra a maioria das falhas, essa replicação não cobre uma falha em toda a localização. 

Os nomes de tópicos são submetidos a backup pelo  {{site.data.keyword.messagehub}}.















