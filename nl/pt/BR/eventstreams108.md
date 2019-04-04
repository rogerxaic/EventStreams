---

copyright:
  years: 2015, 2019
lastupdated: "2018-08-08"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:faq: data-hd-content-type='faq'}

# FAQs
{: #faqs}

Respostas para perguntas comuns sobre o serviço do {{site.data.keyword.IBM}}
{{site.data.keyword.messagehub}}.
{: shortdesc}

<!--17/10/17 - Karen: same info duplicated at messagehub104 -->
## Como uso as APIs Kafka para criar e excluir tópicos?
{: #topic_admin}
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

<p>**Nota:** no plano Enterprise, é possível configurar isso com qualquer valor.</p>
</dd>

<dt>retention.bytes</dt>
<dd>O tamanho máximo que uma partição (que consiste em segmentos de log) pode atingir antes que descartemos segmentos de log antigos para liberar espaço.

<p>**Nota:** somente o plano Enterprise. Configure qualquer valor maior que 1 MB.</p>
</dd>

<dt>segment.bytes</dt>
<dd>O tamanho do arquivo de segmento para o log.

<p>**Nota:** somente o plano Enterprise. Configure qualquer valor maior que 100 kB.</p>
</dd>

<dt>segment.index.bytes</dt>
<dd>O tamanho do índice que mapeia deslocamentos para posições do arquivo. 

<p>**Nota:** somente o plano Enterprise. Configure qualquer valor entre 100 kB e 2 GB.</p>
</dd>

<dt>segment.ms</dt>
<dd>O período após o qual o Kafka força o log a rolar, mesmo que o arquivo de segmento ainda não esteja completo. 

<p>**Nota:** somente o plano Enterprise. Configure qualquer valor entre cinco minutos e 30 dias</p>
</dd>
</dl>


## Por quanto tempo o {{site.data.keyword.messagehub}} configura a janela de retenção de log para o tópico de compensações do consumidor?
{: #offsets }
{: faq}
O {{site.data.keyword.messagehub}} retém as compensações do consumidor por sete dias. Isso corresponde
à configuração offsets.retention.minutes do Kafka. 

A retenção de compensação é feita no sistema inteiro, portanto, não é possível configurá-la
em um nível de tópico individual. Todos os grupos de consumidores obtêm somente 7 dias de compensações armazenadas, mesmo se usando um tópico com uma retenção de log que foi aumentada para
o máximo de 30 dias. 

## O que é comportamento de disponibilidade do {{site.data.keyword.messagehub}}?
{: #availability}
{: faq}

Se você gravar aplicativos do {{site.data.keyword.messagehub}}, use estas informações para
entender o que é um comportamento normal de disponibilidade do
{{site.data.keyword.messagehub}} e o que seus aplicativos esperam manipular.

### APIs
{: #api_availability }

Como parte da operação regular do {{site.data.keyword.messagehub}}, os nós do cluster do Kafka
são reiniciados ocasionalmente.
Em alguns casos, seus aplicativos se tornam perceptivos conforme o cluster redesigna recursos. Grave seus
aplicativos para que sejam resilientes a essas mudanças e capazes de se reconectarem e tentarem novamente as
operações.

### Pontes do {{site.data.keyword.messagehub}} (somente o plano Standard)
{: #bridge_availability }

Grave seus aplicativos para manipular a possibilidade de que pontes possam reiniciar ocasionalmente.

## Qual é o tamanho máximo da mensagem do {{site.data.keyword.messagehub}}? 
{: #max_message_size }
{: faq}

O tamanho máximo de mensagem do {{site.data.keyword.messagehub}} é 1 MB, que é o padrão do Kafka. 

## O que são as configurações de replicação do {{site.data.keyword.messagehub}}? 
{: #replication }
{: faq}

O {{site.data.keyword.messagehub}} é configurado para fornecer disponibilidade e
durabilidade fortes.
As seguintes definições de configuração se aplicam a todos os tópicos e não podem mudar:
* replication.factor = 3
* min.insync.replicas = 2

## Como o faturamento do {{site.data.keyword.messagehub}} funciona no plano Standard? 
{: #billing }
{: faq}

{{site.data.keyword.messagehub}} no plano Standard regularmente executa amostragem da contagem de tópicos
de um usuário e o {{site.data.keyword.Bluemix_notm}} registra o valor máximo de amostra todos os dias. O {{site.data.keyword.messagehub}} cobra pelo número máximo de partições simultâneas vistas e pela
soma de mensagens que são enviadas e recebidas diariamente.

Por exemplo, se você cria e exclui 1 tópico 10 vezes em um dia, você é cobrado pelo máximo de 1
tópico. No entanto, se você cria 10 tópicos e os exclui, talvez você seja cobrado por 0 ou 10
tópicos, dependendo de quando a amostragem ocorre.

O {{site.data.keyword.messagehub}} cobra por mensagem ou a cada 64 k. Uma mensagem de até 64 k
é cobrada como 1 mensagem faturável. Já as mensagens maiores que 64 k são contadas como o seguinte número de
mensagens faturáveis: <code><var class="keyword varname">message_size</var> &divide; 64 k</code>.

<!--12/04/18 - Karen: same info duplicated at messagehub057 -->
## Com que frequência a API de REST do Kafka é reiniciada? 
{: #REST_restart }
{: faq}

A API de REST do Kafka reinicia uma vez por dia por um curto período de tempo. 

Durante esse período, a
API de REST do Kafka pode se tornar indisponível. Se isso acontecer, é recomendado tentar novamente
sua solicitação. Após a API de REST ser reiniciada, será necessário recriar suas instâncias do consumidor
do Kafka. Se este for o caso, a API de REST retornará o JSON a seguir:

```'{"error_code":40403,"message":"Consumer instance not found."}'
```
{:screen}

## Quais são as diferenças entre os planos {{site.data.keyword.messagehub}} Standard e {{site.data.keyword.messagehub}} Enterprise?
{: #plan_compare }
{: faq}

Para descobrir mais informações sobre os dois planos diferentes do {{site.data.keyword.messagehub}}, consulte [Escolhendo seu plano](/docs/services/EventStreams?topic=eventstreams-plan_choose).

## Como eu manipulo a recuperação de desastre?
{: #disaster_recovery }
{: faq}

Atualmente, é responsabilidade do usuário gerenciar sua própria recuperação de desastre do {{site.data.keyword.messagehub}}. Os dados do {{site.data.keyword.messagehub}} podem ser replicados entre uma instância do {{site.data.keyword.messagehub}} em uma localização (região) e outra instância em uma localização diferente. No entanto, o usuário é responsável por provisionar uma instância remota do {{site.data.keyword.messagehub}} e gerenciar a replicação.

O usuário também é responsável pelo backup dos dados de carga útil da mensagem. Embora esses dados sejam replicados por múltiplos brokers do Kafka dentro de um cluster, o que protege contra a maioria das falhas, essa replicação não cobre uma falha em toda a localização. 

Os nomes de tópicos são submetidos a backup pelo  {{site.data.keyword.messagehub}}.















