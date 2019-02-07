---

copyright:
  years: 2015, 2019
lastupdated: "2018-07-05"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Usando a API do cliente Java do Kafka de administração
{: #kafka_java_api}


<!-- 
17/10/17 - Karen: following info duplicated at messagehub108
 -->

Se estiver usando um cliente Kafka em 0.11 ou mais recente ou o Streams Kafka em 0.10.2.0 ou
mais recente, será possível utilizar APIs para criar e excluir tópicos. Colocamos algumas restrições nas
configurações permitidas ao criar tópicos. Atualmente, é possível modificar somente as configurações
a seguir:
{: shortdesc}

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

