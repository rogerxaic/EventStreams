---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-06"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Acordo de Nível de Serviço (SLA) para disponibilidade do {{site.data.keyword.messagehub}} (plano Enterprise)
{: #sla}

O serviço {{site.data.keyword.messagehub}} é fornecido com uma disponibilidade de 99,95% no plano Enterprise. 
Para obter mais sobre o SLA para o {{site.data.keyword.Bluemix}}, consulte a descrição do serviço do
[{{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www-03.ibm.com/software/sla/sladb.nsf/8bd55c6b9fa8039c86256c6800578854/c4ceb9f019f9eb4c862582f9001b3994/$FILE/i126-6605-16_04-2019_en_US.pdf){:new_window}.

## O que significa 99,95% de disponibilidade?
Disponibilidade refere-se à capacidade de aplicativos produzirem e consumirem mensagens de tópicos do Kafka.

## Como medi-lo?
As instâncias de serviço são monitoradas continuamente para desempenho, taxas de erro e sua resposta a operações sintéticas. As indisponibilidades são registradas.

## O que deve ser considerado para obter essa disponibilidade?
Para obter altos níveis de disponibilidade da perspectiva do aplicativo, é necessário considerar a [conectividade](/docs/services/EventStreams?topic=eventstreams-sla#connectivity), o [rendimento](/docs/services/EventStreams?topic=eventstreams-sla#throughput) e a [consistência e a durabilidade das mensagens](/docs/services/EventStreams?topic=eventstreams-sla#message_consistency). Os usuários são responsáveis por projetar seus aplicativos para otimizar esses três elementos para seus negócios.

### Conectividade
{: #connectivity}

Devido à natureza dinâmica da nuvem, os aplicativos devem esperar quebras de conexão. Uma quebra de conexão não é considerada uma falha de serviço.

**Novas tentativas**<br/>
Os clientes Kafka fornecem a lógica de reconexão, mas deve-se ativar explicitamente reconexões para produtores. Para obter mais informações, consulte a [propriedade <code>retries</code> ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://kafka.apache.org/11/documentation.html#producerconfigs){:new_window}. As conexões são refeitas em 60 segundos.   
 
**Duplicatas**<br/>
A ativação de novas tentativas pode resultar em mensagens duplicadas. Dependendo de quando uma conexão é perdida, o produtor pode não conseguir determinar se uma mensagem foi processada com sucesso pelo servidor e, portanto, deve enviar a mensagem novamente quando reconectada. Recomenda-se projetar aplicativos que esperem mensagens duplicadas. 

Quando duplicatas não são toleradas, pode-se usar o recurso de produtor <code>idempotent</code> (do Kafka 1.1) para evitar duplicatas durante as novas tentativas. Para obter mais informações, consulte a propriedade <code>enable.idempotence</code> do [ ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de linkexterno")](http://kafka.apache.org/11/documentation.html#producerconfigs){:new_window}.


### Rendimento
{: #throughput}

O rendimento é expresso como o número de bytes por segundo que podem ser enviados e recebidos em um cluster. 

**Recomendação**<br/>
40 MB por segundo com um limite máximo de pico de 90 MB por segundo. <br/>
O número recomendado é baseado em uma carga de trabalho típica e leva em consideração o possível impacto de ações operacionais, como atualizações internas ou modos de falha, como a perda de uma zona de disponibilidade. Por exemplo, mensagens com uma pequena carga útil (menos de 10 mil). Se o rendimento médio excede esse número, pode haver uma perda no desempenho durante essas condições.

**Medida**<br/>
Recomenda-se que os aplicativos de instrumentos estejam cientes de como eles estão sendo executados. Por exemplo, o número de mensagens enviadas e recebidas, os tamanhos das mensagens e os códigos de retorno. Entender o uso de um aplicativo ajuda a configurar seus recursos apropriadamente, como o tempo de retenção para mensagens nos tópicos.

**Saturação**<br/>
À medida que o limite do tráfego que pode ser produzido no cluster se aproxima, os produtores começam a ser regulados, a latência aumenta e, em última instância, ocorrem erros, como erros de tempo limite. Dependendo da configuração, a consistência e a durabilidade da mensagem também podem ser impactadas. Para obter mais informações, consulte [Consistência e durabilidade das mensagens](/docs/services/EventStreams?topic=eventstreams-sla#message_consistency).

### Consistência e durabilidade das mensagens
{: #message_consistency}

O Kafka obtém sua disponibilidade e durabilidade replicando as mensagens que ele recebe em outros nós no cluster, que podem, então, ser usadas no caso de falha. O {{site.data.keyword.messagehub}} usa três réplicas (default.replication.factor = 3), o que significa que cada mensagem recebida por um nó é replicada para dois outros nós em zonas de disponibilidade diferentes. Dessa maneira, a perda de um nó ou da zona de disponibilidade pode ser tolerada sem perda de dados ou função.

**Modo <code>acks</code> do produtor**<br/>
Embora todas as mensagens sejam replicadas, os aplicativos podem controlar a robustez com que as mensagens produzidas são transferidas para o serviço usando a propriedade de modo <code>acks</code> do produtor. Essa propriedade fornece uma opção entre a velocidade e o risco de perda da mensagem. A configuração padrão é <code>acks=1</code>, o que significa que o produtor retorna sucesso assim que o nó ao qual ele está conectado confirma que recebeu a mensagem, mas antes que a replicação seja concluída. A configuração recomendada e mais garantida é <code>acks=all</code>, em que o produtor só retorna sucesso após a mensagem ter sido copiada para todas as réplicas. Isso assegura que as réplicas sejam mantidas na etapa, o que evita a perda de mensagem quando uma falha causa um comutador em uma réplica.


