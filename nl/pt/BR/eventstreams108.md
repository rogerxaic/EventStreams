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

# Perguntas mais frequentes
{: #faqs}

Respostas para perguntas comuns sobre o serviço do {{site.data.keyword.IBM}}
{{site.data.keyword.messagehub}}.

Para obter respostas para perguntas específicas para o plano Clássico, consulte [Perguntas mais frequentes para o plano Clássico](/docs/services/EventStreams?topic=eventstreams-faqs_classic).
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

O tópico interno <code>__consumer_offsets</code> do Kafka é visível como somente leitura. 
É altamente recomendado não tentar gerenciar o tópico de nenhuma maneira. 

<!--following message retention info duplicted in eventstreams057-->

## Quanto tempo as mensagens ficam retidas?
{: #messages_retained}

Por padrão, as mensagens são retidas no Kafka por até 24 horas e cada partição é limitada a 1 GB. Se um valor máximo de 1 GB for atingido, as mensagens mais antigas serão descartadas para permanecerem
no limite.

É possível mudar o limite de tempo para retenção mensagem ao criar um tópico usando a interface
com o usuário ou a API de administração. O limite de tempo é um mínimo de uma hora e um máximo de 30 dias.

Para obter informações sobre restrições das configurações permitidas ao criar tópicos usando um cliente Kafka ou o Kafka Streams, consulte [Como eu uso as APIs do Kafka para criar e excluir tópicos?](/docs/services/EventStreams?topic=eventstreams-faqs#topic_admin).

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

## Quais são as diferenças entre os planos {{site.data.keyword.messagehub}} Standard e {{site.data.keyword.messagehub}} Enterprise?
{: #plan_compare }
{: faq}

Para localizar mais informações sobre os diferentes planos do {{site.data.keyword.messagehub}}, consulte [Escolhendo seu plano](/docs/services/EventStreams?topic=eventstreams-plan_choose).

## Como eu manipulo a recuperação de desastre?
{: #disaster_recovery }
{: faq}

Atualmente, é responsabilidade do usuário gerenciar sua própria recuperação de desastre do {{site.data.keyword.messagehub}}. Os dados do {{site.data.keyword.messagehub}} podem ser replicados entre uma instância do {{site.data.keyword.messagehub}} em uma localização (região) e outra instância em uma localização diferente. No entanto, o usuário é responsável por provisionar uma instância remota do {{site.data.keyword.messagehub}} e gerenciar a replicação.

O usuário também é responsável pelo backup dos dados de carga útil da mensagem. Embora esses dados sejam replicados por múltiplos brokers do Kafka dentro de um cluster, o que protege contra a maioria das falhas, essa replicação não cobre uma falha em toda a localização. 

Os nomes de tópicos são submetidos a backup pelo  {{site.data.keyword.messagehub}}.















