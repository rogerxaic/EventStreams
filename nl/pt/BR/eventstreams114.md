---

copyright:
  years: 2015, 2018
lastupdated: "2018-03-23"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Consumindo mensagens
{: #consuming_messages }

Um consumidor é um aplicativo que consome fluxos de mensagens dos tópicos do Kafka. Um consumidor pode assinar um ou mais tópicos ou partições. Essas informações se concentram na interface de programação Java que faz parte do projeto Apache Kafka. Os conceitos se aplicam a outros idiomas também, mas os nomes são, às vezes, um pouco diferentes.

Quando um consumidor se conecta ao Kafka, ele faz uma conexão de autoinicialização inicial. Essa conexão pode ser qualquer um dos servidores no cluster. O consumidor solicita as informações de partição e de liderança sobre o tópico do qual deseja consumir. Em seguida, o consumidor estabelece outra conexão com o líder da partição e pode começar a consumir mensagens. Essas ações acontecem automaticamente internamente quando o consumidor se conecta ao cluster do Kafka.

Um consumidor é normalmente um aplicativo de longa execução. Um consumidor solicita mensagens do Kafka
chamando `Consumer.poll(...)` regularmente. O consumidor chama `poll()`, recebe um lote de mensagens, processa-os imediatamente e depois chama `poll()` novamente.

Quando um consumidor processa uma mensagem, a mensagem não é removida de seu tópico. Em vez disso, os consumidores podem escolher entre várias maneiras de permitir que o Kafka saiba quais mensagens foram processadas. Esse processo é conhecido como confirmação do deslocamento.

Nas interfaces de programação, uma mensagem é realmente chamada de um registro. Por exemplo, a classe Java org.apache.kafka.clients.consumer.ConsumerRecord é usada para representar uma mensagem para a API do consumidor. Os termos _registro_ e _mensagem_ podem ser usados de forma intercambiável, mas um registro é usado essencialmente para representar uma mensagem.

Talvez você ache útil ler estas informações em conjunto com a
[produção de mensagens](/docs/services/EventStreams/eventstreams112.html) no
{{site.data.keyword.messagehub}}.

## Configurando propriedades do consumidor 
{: #configuring_consumer_properties }

Existem muitas definições de configuração para o consumidor, que controlam aspectos de seu
comportamento. Aqui estão algumas das mais importantes:

| Nome     |Descrição   | Valores válidos   | Padrão   |
|----------|---------|----------|---------|
|key.deserializer     | A classe usada para desserializar chaves. | Classe Java que implementa a interface Desserializador, como org.apache.kafka.common.serialization.StringDeserializer  |Nenhum padrão - deve-se especificar um valor|
|value.deserializer     | A classe usada para desserializar valores. | Classe Java que implementa a interface Desserializador, como org.apache.kafka.common.serialization.StringDeserializer  | Nenhum padrão - deve-se especificar um valor |
|group.id | Um identificador para o grupo de consumidores ao qual o consumidor pertence. | string |Sem padrão|
|auto.offset.reset | O comportamento quando o consumidor não possui um deslocamento inicial ou quando o
deslocamento atual não está mais disponível no cluster. | latest, earliest, none | latest |
|enable.auto.commit | Determina se o deslocamento do consumidor deve ser confirmado automaticamente em segundo
plano. | true, false | verdadeiro |
|auto.commit.interval.ms | O número de milissegundos entre as confirmações periódicas de deslocamentos. | 0,... | 5000 (5 segundos) |
|max.poll.records | O número máximo de registros retornados em uma chamada para poll() | 1,... | 500 |
|session.timeout.ms | O número de milissegundos em que uma pulsação do consumidor deve ser recebida para manter a
associação de um consumidor de um grupo de consumidores. | 6000-300000 | 10000 (10 segundos) |
|max.poll.interval.ms |O intervalo de tempo máximo entre as pesquisas antes que o consumidor saia do grupo. | 1,... | 300000
(5 minutos) |
| | | | |

Embora muito mais definições de configuração estejam disponíveis, certifique-se de ler a [Documentação do Apache Kafka ![Ícone de link externo ](../../icons/launch-glyph.svg "Ícone de link externo")](http://kafka.apache.org/documentation/){:new_window} completamente antes de experimentar com eles.

## Grupos de consumidores

Um _grupo de consumidores_ é um grupo de consumidores que colaboraram para consumir mensagens de um ou mais tópicos. Todos os consumidores em um grupo usam o mesmo valor para a configuração de `group.id`. Caso você precise de mais de um consumidor para lidar com sua carga, é possível executar múltiplos consumidores no mesmo grupo de consumidores. Mesmo que você precise somente de um consumidor, é comum especificar um valor também para `group.id`.

Cada grupo de consumidores possui um servidor no cluster chamado _coordenador_, que é responsável por designar partições para os consumidores no grupo. Essa responsabilidade é propagada entre os servidores no cluster para balancear a carga. A designação de partições para os consumidores pode mudar a cada rebalanceamento de grupo.

Quando um consumidor se associa a um grupo de consumidores, ele descobre o coordenador do grupo. O consumidor, então, informa ao coordenador que ele deseja associar-se ao grupo e o coordenador inicia um rebalanceamento das partições no grupo incluindo o novo membro.

Quando ocorre uma das mudanças a seguir em um grupo de consumidores, o grupo é rebalanceado deslocando a designação de partições para que os membros do grupo acomodem a mudança:

* um consumidor se associa ao grupo
* um consumidor sai do grupo
* um consumidor é considerado como não mais ativo pelo coordenador
* novas partições são incluídas em um tópico existente

Para cada grupo de consumidores, o Kafka lembra o deslocamento confirmado de cada partição que está sendo consumida.

Se você possui um grupo de consumidores que foi rebalanceado, lembre-se de que qualquer consumidor que saiu do grupo terá suas confirmações rejeitadas até que ele se una novamente ao grupo. Nesse caso, o consumidor precisa unir-se novamente ao grupo, em que ele pode ser designado a uma partição diferente daquela que ele estava consumindo anteriormente.

## Atividade do consumidor

O Kafka detecta automaticamente os consumidores com falha para que ele possa designar novamente partições para consumidores ativos. Ele usa dois mecanismos para fazer isso: pesquisa e pulsação.

Se o lote de mensagens retornadas do `Consumer.poll(...)` for grande ou se o processamento estiver consumindo tempo, o atraso antes da nova chamada de `poll()` poderá ser significativo ou imprevisível. Em alguns casos, é necessário configurar um intervalo de pesquisa longo máximo para que os consumidores não sejam removidos de seus grupos só porque o processamento de mensagens está demorando um pouco. Se esse fosse o único mecanismo, significaria que o tempo gasto para detectar um consumidor com falha também seria longo.

Para tornar a atividade do consumidor mais fácil de lidar, uma pulsação em segundo plano foi incluída no Kafka 0.10.1. O coordenador do grupo espera que os membros do grupo enviem pulsações regulares para indicar que eles permanecem ativos. Um encadeamento de pulsação de segundo plano é executado no consumidor enviando pulsações regulares para o coordenador. Se o coordenador não recebe uma pulsação de um membro do grupo dentro do _tempo limite de sessão_, o coordenador remove o membro do grupo e inicia um rebalanceamento do grupo. O tempo limite da sessão pode ser muito menor que o intervalo máximo de pesquisa, de modo que o tempo gasto para detectar um consumidor com falha pode ser reduzido, mesmo que o processamento da mensagem demore muito tempo.

É possível configurar o intervalo máximo de pesquisa usando a propriedade `max.poll.interval.ms` e configurar o tempo limite da sessão usando a propriedade `session.timeout.ms`. Geralmente, não será necessário usar essas configurações, a
menos que demore mais de 5 minutos para processar um lote de mensagens.

## Gerenciando deslocamentos

Para cada grupo de consumidores, o Kafka mantém o deslocamento confirmado para cada partição que está sendo consumida. Quando um consumidor processa uma mensagem, ele não a remove da partição. Em vez disso, ele apenas atualiza seu deslocamento atual usando um processo chamado confirmação do deslocamento.

O {{site.data.keyword.messagehub}} retém as informações de deslocamento confirmadas por 7 dias.

### E se não houver nenhum deslocamento confirmado existente?
Quando um consumidor inicia e é designado a uma partição para consumo, ele inicia no deslocamento confirmado do grupo. Se nenhum deslocamento confirmado existir, o consumidor poderá escolher se deseja iniciar com a primeira ou com a mensagem mais recente disponível com base na configuração da propriedade `auto.offset.reset` da seguinte forma:

- `latest` (o padrão). O consumidor recebe e consome apenas mensagens que chegam após a assinatura. Como o consumidor não tem conhecimento das mensagens que foram enviadas antes de ter sido inscrito, você não espera que todas as mensagens sejam consumidas de um tópico.
- `earliest`. O consumidor consome todas as mensagens desde o início porque ele está ciente de todas as mensagens que foram enviadas.

Se um consumidor falhar após o processamento de uma mensagem, mas antes de confirmar seu deslocamento, as informações de deslocamento confirmadas não refletirão o processamento da mensagem. Isso significa que a mensagem será processada novamente pelo próximo consumidor desse grupo a ser designado à partição.

Quando os deslocamentos confirmados são salvos no Kafka e os consumidores são reiniciados, os consumidores continuam do ponto em que eles pararam na última vez. Quando há um deslocamento confirmado, a propriedade `auto.offset.reset` não é usada.

### Confirmando deslocamentos automaticamente

A maneira mais fácil de confirmar deslocamentos é permitir que o consumidor do Kafka faça isso automaticamente. Isso é simples, mas oferece menos controle do que a confirmação manual. Por padrão, um
consumidor confirma automaticamente os deslocamentos a cada 5 segundos. Essa confirmação padrão acontece a cada cinco
segundos, independentemente do progresso que o consumidor está fazendo para processar as mensagens. Além disso, quando o consumidor chama `poll()`, isso também faz com que o deslocamento mais recente retornado da chamada anterior a `poll()` seja confirmado (porque ele provavelmente foi processado).

Caso o deslocamento confirmado ultrapasse o processamento das mensagens e aconteça uma falha de consumidor, é possível que
algumas mensagens não sejam processadas. Isso ocorre porque o processamento é reiniciado no deslocamento
confirmado, que é posterior à última mensagem a ser processada antes da falha. Por essa razão, se a confiabilidade é mais importante do que simplicidade, geralmente é melhor confirmar os deslocamentos manualmente.

### Confirmando os deslocamentos manualmente

Se `enable.auto.commit` está configurado como `false`, o consumidor
confirma seus deslocamentos manualmente. Ele pode fazer isso de forma síncrona ou assíncrona. Um padrão
comum é confirmar o deslocamento da mensagem mais recente processada com base em um cronômetro periódico. Esse padrão significa que
cada mensagem é processada pelo menos uma vez, mas o deslocamento confirmado nunca ultrapassa o progresso das mensagens que estão
sendo ativamente processadas. A frequência do cronômetro periódico controla o número de mensagens que podem ser reprocessadas após
uma falha do consumidor. As mensagens são recuperadas novamente no último
deslocamento confirmado salvo quando o aplicativo reinicia ou quando o grupo é rebalanceado.

O deslocamento confirmado é o deslocamento das mensagens nas quais o processamento é continuado. Isso é geralmente o deslocamento da mensagem processada mais recentemente *mais uma*.

### Atraso do consumidor

O atraso do consumidor para uma partição é a diferença entre o deslocamento da mensagem publicada mais
recentemente e o deslocamento confirmado do consumidor. Embora seja comum ter variações naturais nas taxas
de produção e de consumo, a taxa de consumo não deve ser menor que a taxa de produção durante um
período estendido.

Se você observar que um consumidor está processando mensagens com sucesso, mas às vezes parece pular
algum grupo de mensagens, pode ser um sinal de que o consumidor não está conseguindo acompanhar o ritmo. Para
tópicos que não usam a compactação de log, a quantia de espaço de log é gerenciada
excluindo periodicamente segmentos de log antigos. Se um consumidor estiver tão atrasado ao ponto de consumir
mensagens em um segmento de log que foi excluído, ele avançará repentinamente para o início do próximo
segmento de log. Se for importante que o consumidor processe todas as mensagens, esse comportamento indicará
perda de mensagem do ponto de vista deste consumidor.

É possível usar a ferramenta <code>kafka-consumer-groups</code> para ver o atraso do
consumidor. Também é possível utilizar a API e as métricas do consumidor para o mesmo
propósito.


## Controlando a velocidade de consumo de mensagem
{: #message_consumption_speed }

Se você tiver problemas com a manipulação de mensagem causada por sobrecarga de mensagem, será possível
configurar uma opção do consumidor para controlar a velocidade do consumo de mensagens. Use
`fetch.max.bytes` e `max.poll.records` para controlar quantos dados uma
chamada para `poll()` pode retornar.


## Manipulando o rebalanceamento do consumidor
Quando os consumidores são incluídos ou removidos de um grupo, um rebalanceamento de grupo ocorre e os
consumidores não são capazes de consumir mensagens. Isso faz com que todos os consumidores em um grupo de
consumidores se tornem indisponíveis por um curto período.

É possível usar um ConsumerRebalanceListener para confirmar deslocamentos manualmente (caso você não
esteja usando confirmação automática) quando notificado com o retorno de chamada "em partições revogadas" e
pausar um processamento adicional até que você seja notificado sobre o rebalanceamento com sucesso usando o
retorno de chamada "na partição designada".


## Fragmentos de código
{: #consumer_code_snippets notoc}

Esses fragmentos de código estão em um nível muito alto para ilustrar os conceitos envolvidos. Para obter exemplos completos, veja as amostras do {{site.data.keyword.messagehub}} no GitHub https://github.com/ibm-messaging/event-streams-samples.

Para conectar-se ao {{site.data.keyword.messagehub}}, primeiro é necessário construir o conjunto de propriedades de configuração. Como todas as conexões com o {{site.data.keyword.messagehub}} são protegidas usando TLS e autenticação
de usuário/senha, pelo menos essas propriedades são necessárias. Substitua KAFKA_BROKERS_SASL, USER e PASSWORD por suas próprias credenciais de serviço:

```
Properties props = new Properties(); props.put("bootstrap.servers", KAFKA_BROKERS_SASL); props.put("sasl.jaas.config", "org.apache.kafka.common.security.plain.PlainLoginModule required username=\"USER\" password=\"PASSWORD\";"); props.put("security.protocol", "SASL_SSL"); props.put("sasl.mechanism", "PLAIN"); props.put("ssl.protocol", "TLSv1.2"); props.put("ssl.enabled.protocols", "TLSv1.2"); props.put("ssl.endpoint.identification.algorithm", "HTTPS");
```

Para consumir mensagens, também é necessário especificar desserializadores para as chaves e valores, por
exemplo:

```
 props.put("key.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
 props.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
```

Em seguida, use um KafkaConsumer para consumir mensagens, em que cada mensagem é representada por um ConsumerRecord. A maneira mais comum de consumir mensagens é colocar o consumidor em um grupo de consumidores configurando o ID do grupo e, em seguida, chamando `subscribe()` para obter uma lista de tópicos. O consumidor receberá algumas partições para consumo, apesar de que ele poderá não receber nenhuma partição se houver mais consumidores no grupo do que partições no tópico. Em seguida, chame `poll()` em um loop, recebendo um lote de mensagens para processar, em que cada mensagem é representada por um ConsumerRecord.

```
props.put("group.id", "G1");
Consumer<String, String> consumer = new KafkaConsumer<>(props);
consumer.subscribe(Arrays.asList("T1"));
while (true) {
   ConsumerRecords<String, String> records = consumer.poll(100);
   for (ConsumerRecord<String, String> record : records)
     System.out.printf("offset = %d, key = %s, value = %s%n", record.offset(), record.key(), record.value());
}
```

Esse loop consumidor é executado indefinidamente, mas pode ser interrompido por outro encadeamento chamando `Consumer.wakeup()` para obter um encerramento normal.

Para confirmar deslocamentos manualmente, primeiramente é necessário definir a configuração do `enable.auto.commit` como `false`. Em seguida, use `Consumer.commmitSync()` ou `Consumer.commitAsync()` para atualizar o deslocamento confirmado do consumidor periodicamente. Para simplificar, este exemplo processa os registros para cada partição e confirma o último deslocamento separadamente.

```
props.put("group.id", "G1");
props.put("enable.auto.commit", "false");
Consumer<String, String> consumer = new KafkaConsumer<>(props);
consumer.subscribe(Arrays.asList("T1"));
try {
  while (true) {
    ConsumerRecords<String, String> records = consumer.poll(100);
    for (TopicPartition tp : records.partitions()) {
      List<ConsumerRecord<String, String>> partRecords = records.records(tp);
      long lastOffset = 0;
      for (ConsumerRecord<String, String> record : partRecords) {
        System.out.printf("offset = %d, key = %s, value = %s%n", record.offset(), record.key(), record.value());
        lastOffset = record.offset();
      }

      consumer.commitSync(Collections.singletonMap(tp, new OffsetAndMetadata(lastOffset + 1)));
    }
  }
}
finally {
  consumer.close();
}
```

## Manipulação de exceção

Qualquer aplicativo robusto que usa o cliente Kafka precisa manipular exceções para determinadas situações esperadas. Em alguns casos, as exceções não são emitidas diretamente porque alguns métodos são assíncronos e entregam seus resultados usando um `Future` ou um retorno de chamada. É possível localizar código de exemplo em [GitHub ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/ibm-messaging/event-streams-samples) que mostra exemplos completos.

Aqui está uma lista de exceções que devem ser manipuladas em seu código:

### org.apache.kafka.common.errors.WakeupException
Lançada pelo `Consumer.poll(...)` como um resultado da chamada de `Consumer.wakeup()`. Esta é a maneira padrão para interromper o loop de pesquisa do consumidor. O loop de pesquisa deve encerrar e `Consumer.close()` deve ser chamado para desconexão normal.
### org.apache.kafka.common.errors.NotLeaderForPartitionException
Lançada como resultado de `Producer.send(...)` quando a liderança para uma partição muda. O cliente atualiza automaticamente seus metadados para localizar informações líderes atualizadas. Tente novamente a operação, que deve ser bem-sucedida com os metadados atualizados.
### org.apache.kafka.common.errors.CommitFailedException
Lançada como resultado de `Consumer.commitSync(...)` quando ocorre um erro irrecuperável. Em alguns casos, não é possível simplesmente repetir a operação porque a designação de partição pode ter mudado e o consumidor pode não mais ser capaz de confirmar seus deslocamentos. Como a `Consumer.commitSync(...)` pode ser parcialmente bem-sucedida quando usada com múltiplas partições em uma única chamada, a recuperação de erro pode ser simplificada usando uma chamada `Consumer.commitSync(...)` separada para cada partição.
### org.apache.kafka.common.errors.TimeoutException
Lançada por `Producer.send(...),Consumer.listTopics()`, se os metadados não podem ser recuperados. A exceção
também é vista no retorno de chamada de envio (ou no futuro retornado) quando o reconhecimento solicitado não volta dentro de
`request.timeout.ms`. O cliente pode tentar novamente a operação, mas o efeito de uma operação repetida depende da operação específica. Por exemplo, se o envio de uma mensagem é tentado novamente, a mensagem pode ser duplicada.

