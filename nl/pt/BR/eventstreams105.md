---

copyright:
  years: 2015, 2019
lastupdated: "2018-06-01"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Ponte do MQ no plano Clássico 
{: #mq_bridge}

** A ponte do MQ está disponível somente como parte do plano Clássico.**
<br/>

A ponte do MQ permite transferir dados da mensagem de uma fila do {{site.data.keyword.IBM_notm}} MQ
para um tópico do {{site.data.keyword.messagehub}} Kafka. A ponte do MQ permite executar de modo eficiente cargas de trabalho de estilo de nuvem (por exemplo, análise de
dados) com relação a dados de mensagem do {{site.data.keyword.IBM_notm}} MQ gerados na
sua empresa.
 {:shortdesc}

A ponte do MQ se conecta a um gerenciador de filas do {{site.data.keyword.IBM_notm}}
MQ como um cliente do MQ e consome dados de mensagem do MQ em uma fila do MQ. A ponte converte cada mensagem do MQ em um registro do Kafka e envia a mensagem para um tópico do {{site.data.keyword.messagehub}} Kafka.

## Versões suportadas do {{site.data.keyword.IBM_notm}} MQ
{: #mq_support}

As versões do {{site.data.keyword.IBM_notm}} MQ com que a ponte trabalha são as seguintes:

* {{site.data.keyword.IBM_notm}} MQ versão 8.0 e mais recente

## Protegendo a ponte do MQ
{: #mq_bridge_security}

É possível configurar a ponte do MQ para autenticação com o {{site.data.keyword.IBM_notm}} MQ
usando um identificador e senha de usuário. Recomendamos conceder as seguintes permissões somente para
a identidade associada a uma instância da ponte do MQ:

* Autoridade CONNECT. A ponte do MQ deve ser capaz de conectar-se ao gerenciador de filas do MQ.
* Autoridade GET para a fila da qual a ponte do MQ está configurada para consumir.

## Confiabilidade e ordenação da mensagem
{: #mq_bridge_reliability}

A ponte do MQ implementa um nível de confiabilidade pelo menos uma vez para os dados
da mensagem que ela transfere. Isso significa que a ponte não perde dados da mensagem, mas pode,
ocasionalmente, gerar sequências duplicadas de registros do Kafka para uma determinada sequência de mensagens do
MQ. Normalmente, a duplicação ocorre quando um evento interrompe o processamento de dados da mensagem da
ponte. Por exemplo:

* Reinício do gerenciador de filas do MQ
* Interrupção da rede entre o gerenciador de filas do MQ e a ponte
* Rebalanceamento da designação de partição do tópico Kafka
* Interrupção da rede entre a ponte e o Kafka

Para assegurar que a ponte do MQ possa transferir mensagens de forma confiável do MQ, a
ponte solicita acesso exclusivo para a fila do MQ da qual ele lê mensagens. Esse acesso evita que outros
aplicativos recebam mensagens da fila do MQ enquanto a ponte está usando-a. Se outros aplicativos já estiverem
recebendo mensagens da fila, a ponte do MQ poderá ser incapaz de iniciar até que esses aplicativos terminem.

Geralmente a ponte do MQ encaminha mensagens do MQ para o Kafka na ordem que elas são recebidas do
{{site.data.keyword.IBM_notm}} MQ. No entanto, ocasionalmente, os dados da mensagem do MQ são
duplicados dentro de um tópico do Kafka (para os motivos descritos anteriormente). Essa duplicação possível
pode resultar em diferenças entre a ordem das mensagens do MQ e a ordem em que os registros correspondentes
são armazenados no Kafka.

## Designação de partição do Kafka
{: #mq_bridge_partition}

A ponte MQ suporta opções para controlar a designação dos dados da mensagem do
{{site.data.keyword.IBM_notm}} MQ para partições de tópico do Kafka. A ordenação de mensagem dentro de
uma partição específica depende da opção selecionada para a ordenação de dados da mensagem (consulte
[Confiabilidade e ordenação de mensagem](#mq_bridge_reliability)). Os valores suportados são
os seguintes:
<dl><dt>Padrão</dt>
<dd>A designação de partição padrão do Kafka é utilizada, significando que os dados de mensagens do MQ são
distribuídos igualmente entre as partições no tópico do Kafka.</dd>
<dt>CorrelationId</dt>
<dd>As mensagens do MQ com o mesmo ID de correlação são designadas à mesma partição do Kafka.</dd>
<dt>GroupId</dt>
<dd>As mensagens do MQ com o mesmo ID do grupo são designadas à mesma partição do Kafka.
</dd>
</dl>

## Restrições de tamanho de mensagem
{: #mq_message}

É possível configurar o {{site.data.keyword.IBM_notm}} MQ a fim de armazenar mensagens que sejam muito grandes para que se ajustem a um registro do {{site.data.keyword.messagehub}} Kafka. O tamanho máximo de
registro do Kafka é de 1.000.000 de bytes, embora uma parte desta capacidade seja usada quando a ponte é
configurada para executar a designação de partição do Kafka com base no identificador de correlação do MQ ou
do identificador de grupo do MQ. Recomendamos enviar mensagens que não sejam maiores que 950 kilobytes usando a
ponte do MQ.

Se a ponte do MQ encontrar uma mensagem que seja muito grande para o Kafka, a ponte descartará a
mensagem e gravará uma entrada de log no seu painel do Kibana. Considere configurar o atributo MAXMSGL da fila do MQ da
qual a ponte recebe mensagens para evitar que os aplicativos do MQ enviem mensagens para a fila que não possam
ser transferidas usando a ponte do MQ.
