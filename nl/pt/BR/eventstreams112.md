---

copyright:
  years: 2015, 2019
lastupdated: "2018-06-23"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Produzindo mensagens
{: #producing_messages }


Um produtor é um aplicativo que publica fluxos de mensagens nos tópicos do Kafka. Essas informações se concentram na interface de programação Java que faz parte do projeto Apache Kafka. Os conceitos se aplicam a outros idiomas também, mas os nomes são, às vezes, um pouco diferentes.
{: shortdesc}

Nas interfaces de programação, uma mensagem é realmente chamada de um registro. Por exemplo, a classe Java org.apache.kafka.clients.producer.ProducerRecord é usada para representar uma mensagem do ponto de vista da API do produtor. Os termos _registro_ e _mensagem_ podem ser usados de forma intercambiável, mas um registro é usado essencialmente para representar uma mensagem.

Quando um produtor se conecta ao Kafka, ele faz uma conexão de autoinicialização inicial. Essa conexão pode ser qualquer um dos servidores no cluster. O produtor solicita as informações de partição e de liderança sobre o tópico no qual deseja publicar. Em seguida, o produtor estabelece outra conexão com o líder da partição e pode começar a publicar mensagens. Essas ações acontecem automaticamente e internamente quando seu produtor se conecta ao cluster do Kafka.
 
Quando uma mensagem for enviada para o líder de partição, essa mensagem não estará imediatamente disponível para os consumidores. O líder anexa o registro para a mensagem para a partição, designando o próximo número de deslocamento para essa partição. Depois que todos os seguidores para as réplicas sincronizadas replicam o registro e reconhecem que gravaram o registro em suas réplicas, o registro está agora *confirmado* e se torna disponível para os consumidores.

Cada mensagem é representada como um registro que inclui duas partes: chave e valor. A chave é comumente usada para dados sobre a mensagem e o valor é o corpo da mensagem. Como muitas ferramentas no ecossistema do Kafka (como conectores para outros sistemas) usam apenas o valor e ignoram a chave, é melhor colocar todos os dados da mensagem no valor e apenas usar a chave para particionamento ou compactação de log. Você não deve confiar em tudo que lê no Kafka para usar a chave.

Muitos outros sistemas de mensagens também têm uma forma de carregar outras informações juntamente com as mensagens. O Kafka 0.11 apresenta cabeçalhos de registro para esse propósito.

Talvez você ache útil ler estas informações em conjunto com o
[consumo de mensagens](/docs/services/EventStreams?topic=eventstreams-consuming_messages) no
{{site.data.keyword.messagehub}}.

## Definições de configuração
{: #config_settings}

Existem muitas definições de configuração para o produtor. É possível controlar os aspectos do produtor, incluindo envio em lote, novas tentativas e confirmação da mensagem. Aqui estão as mais importantes:

| Nome     |Descrição   | Valores válidos   | Padrão   |
|----------|---------|----------|---------|
|key.serializer     | A classe usada para serializar chaves. | Classe Java que implementa a interface Serializador, como org.apache.kafka.common.serialization.StringSerializer |Nenhum padrão - deve-se especificar um valor|
|value.serializer     | A classe usada para serializar valores. | Classe Java que implementa a interface Serializador, como org.apache.kafka.common.serialization.StringSerializer  | Nenhum padrão - deve-se especificar um valor |
|acks     | O número de servidores necessários para reconhecer cada mensagem publicada. Isso controla as
garantias de durabilidade que o produtor requer. | 0, 1, all (ou -1)  | 1 |
|retries     | O número de vezes que o cliente reenvia uma mensagem quando o envio encontra um erro. | 0,...  | 0 |
|max.block.ms     | O número de milissegundos que uma solicitação de envio ou de metadados pode ser bloqueada
durante a espera. | 0,...  | 60000 (1 minuto) |
|max.in.flight.requests.per.connection     | O número máximo de solicitações não reconhecidas que o cliente
envia em uma conexão antes bloquear solicitações adicionais| 1,...  | 5 |
|request.timeout.ms     | A quantia máxima de tempo que o produtor aguarda por uma resposta a uma solicitação. Se a resposta não for recebida antes de o tempo limite expirar, a solicitação será tentada novamente ou
falhará se o número de novas tentativas tiver sido esgotado.| 0,...  | 30000 (30 segundos) |

Embora muito mais definições de configuração estejam disponíveis, certifique-se de ler a [Documentação do Apache Kafka ![Ícone de link externo ](../../icons/launch-glyph.svg "Ícone de link externo")](http://kafka.apache.org/documentation/){:new_window} completamente antes de experimentar com eles.

## Particionamento
{: #partitioning}

Quando o produtor publica uma mensagem em um tópico, o produtor pode escolher qual partição usar. Se a ordenação é importante, é preciso lembrar-se de que uma partição é uma sequência ordenada de registros e que um tópico abrange uma ou mais partições. Se você deseja que um conjunto de mensagens seja entregue em ordem, assegure-se de que todas elas sejam encaminhadas para a mesma partição. A maneira mais direta de fazer isso é fornecer a mesma chave a todas essas mensagens. 
 
O produtor pode especificar explicitamente um número de partição ao publicar uma mensagem. Isso fornece um controle direto, mas torna o código do produtor mais complexo, já que ele assume a responsabilidade de gerenciar a seleção de partição. Para obter mais informações, consulte a chamada de método Producer.partitionsFor. Por exemplo, a chamada é descrita para o
[Kafka 1.1.0 ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://kafka.apache.org/11/javadoc/org/apache/kafka/clients/producer/KafkaProducer.html){:new_window}
 
Se o produtor não especificar um número de partição, a seleção de partição será feita por um particionador. O particionador padrão que é construído no produtor Kafka funciona da seguinte forma:

* Se o registro não tiver uma chave, selecione a partição em um modo round-robin.
* Se o registro tiver uma chave, selecione a partição calculando um valor do hash para a chave. Isso tem o efeito de selecionar a mesma partição para todas as mensagens com a mesma chave.

Também é possível gravar seu próprio particionador customizado. Um particionador customizado pode escolher qualquer esquema para designar registros às partições. Por exemplo, use apenas um subconjunto das informações na chave ou um identificador específico do aplicativo.


## Ordenação de mensagens
{: #message_ordering}

O Kafka geralmente grava mensagens na ordem em que elas são enviadas pelo produtor. No entanto, há situações em que as novas tentativas podem fazer com que as mensagens sejam duplicadas ou reordenadas. Se você deseja que uma sequência de mensagens seja enviada em ordem, é muito importante assegurar que todas elas estejam gravadas na mesma partição.
 
O produtor também é capaz de tentar novamente o envio de mensagens automaticamente. É sempre recomendado ativar esse recurso de nova tentativa, já que a alternativa é que o seu próprio código do aplicativo deverá executar quaisquer novas tentativas. A combinação de envio em lote no Kafka e de novas tentativas automáticas pode ter o efeito de duplicação ou de reordenação de mensagens.
 
Por exemplo, se você publica uma sequência de três mensagens &lt;M1, M2, M3&gt; em um tópico. Todos os registros podem ajustar-se dentro do mesmo lote, de modo que todos eles sejam realmente enviados juntos ao líder da partição. O líder, então, os grava na partição e os replica como registros separados. No caso de uma falha, é possível que M1 e M2 sejam incluídos na partição, exceto o M3. O produtor não recebe uma confirmação, portanto, ele tenta enviar &lt;M1, M2, M3&gt; novamente. O novo líder simplesmente grava M1, M2 e M3 na partição, que agora contém &lt;M1, M2, M1, M2, M3&gt;, em que o M1 duplicado na realidade segue o M2 original. Se você restringir o número de solicitações em andamento de cada broker para um só, essa reordenação poderá ser evitada. Talvez você ainda ache que um registro único está duplicado, como &lt;M1, M2, M2, M3&gt;, mas nunca sairá das sequências de ordem. No Kafka 0.11 (ainda não disponível no {{site.data.keyword.messagehub}}), também é possível usar o recurso do produtor idempotente para evitar a duplicação do M2.
 
O Kafka normalmente grava os aplicativos para manipular duplicatas de mensagens ocasionais, já que o impacto de desempenho de ter apenas uma única solicitação em andamento é significativo.

## Confirmações de mensagem
{: #message_acknowledgments}

Ao publicar uma mensagem, é possível escolher o nível de confirmação necessário usando a configuração do produtor `acks`. A escolha representa um equilíbrio entre rendimento e confiabilidade. Há três níveis, conforme a seguir:

<dl>
<dt>acks= 0 (menos confiável)</dt>
<dd>A mensagem é considerada enviada assim que ela é gravada na rede. Não há nenhuma confirmação por parte do líder da partição. Como resultado, as mensagens poderão ser perdidas se a liderança da partição mudar. Esse nível de confirmação é muito rápido, mas com possibilidade de perda de mensagens em algumas situações.</dd>
<dt>acks= 1 (o padrão)</dt>
<dd>A mensagem é reconhecida pelo produtor assim que o líder da partição grava seu registro com sucesso na partição. Como a confirmação ocorre antes de se saber que o registro chegou às réplicas sincronizadas, a mensagem poderá ser perdida se o líder falhar, mas os seguidores ainda não têm a mensagem. Se a liderança da partição mudar, o líder antigo informará ao produtor, que poderá manipular o erro e tentar enviar a mensagem novamente para o novo líder. Como as mensagens são reconhecidas antes de o recebimento ter sido confirmado por todas as réplicas, as mensagens que foram reconhecidas, mas que ainda não foram totalmente replicadas poderão ser perdidas se a liderança da partição for mudada.</dd>
<dt>acks=all (mais confiável)</dt>
<dd>A mensagem é reconhecida pelo produtor quando o líder da partição grava seu registro com sucesso e quando todas as réplicas sincronizadas fazem o mesmo. A mensagem não será perdida se a liderança da partição mudar, desde que pelo menos uma réplica sincronizada esteja disponível.</dd>
</dl>

Mesmo se você não espera que as mensagens sejam reconhecidas pelo produtor, as mensagens ainda estão disponíveis para serem consumidas somente quando confirmadas, o que significa que a replicação das réplicas sincronizadas está concluída. Em outras palavras, a latência de envio das mensagens do ponto de vista do produtor é menor que a latência de ponta a ponta medida entre o produtor que envia uma mensagem e um consumidor que recebe a mensagem.

Se possível, evite a espera pela confirmação de uma mensagem antes da publicação da próxima mensagem. A espera evita que o produtor envie mensagens em lote juntas e também reduz a taxa em que as mensagens podem ser publicadas abaixo da latência de roundtrip da rede.

## Envio em lote, regulagem e compactação
{: #batching}

Para propósitos de eficiência, o produtor na realidade coleta lotes de registros juntos a serem enviados para os servidores. Se você ativar a compactação, o produtor compactará cada lote, o que poderá melhorar o desempenho exigindo que menos dados sejam transferidos pela rede.

Se você tentar publicar mensagens mais rapidamente do que elas podem ser enviadas para um servidor, o produtor as armazenará em buffer automaticamente nas solicitações em lote. O produtor mantém um buffer de registros não enviados para cada partição. Logicamente, chega um ponto em que até mesmo o envio em lote não permite que a taxa desejada seja atingida.
 
Há outro fator que tem um impacto. Para evitar que produtores ou consumidores individuais sobrecarreguem o cluster, o {{site.data.keyword.messagehub}} aplica cotas de rendimento. A taxa na qual cada produtor envia dados é calculada e qualquer produtor que tenta exceder sua cota é regulado. A regulagem é aplicada
atrasando um pouco o envio de respostas para o produtor. Em geral, isso age apenas como um freio natural.
 
Em resumo, quando uma mensagem é publicada, primeiramente seu registro é gravado em um buffer no produtor. Em segundo plano, o produtor envia os registros em lote para o servidor. O servidor, então, responde ao produtor, possivelmente aplicando um atraso de regulagem se o produtor está publicando muito rápido. Se o buffer no produtor se enche, a chamada de envio do produtor é atrasada, podendo finalmente falhar com uma exceção.

## Fragmentos de código
{: #code_snippets}

Esses fragmentos de código estão em um nível muito alto para ilustrar os conceitos envolvidos. Para obter exemplos completos, veja as amostras do {{site.data.keyword.messagehub}} no GitHub https://github.com/ibm-messaging/event-streams-samples.

Para conectar-se ao {{site.data.keyword.messagehub}}, primeiro é necessário construir o conjunto de propriedades de configuração. Como todas as conexões com o {{site.data.keyword.messagehub}} são protegidas usando TLS e autenticação de usuário/senha, pelo menos essas propriedades são necessárias. Substitua KAFKA_BROKERS_SASL, USER e PASSWORD por suas próprias credenciais de serviço:

```
Properties props = new Properties(); props.put("bootstrap.servers", KAFKA_BROKERS_SASL); props.put("sasl.jaas.config", "org.apache.kafka.common.security.plain.PlainLoginModule required username=\"USER\" password=\"PASSWORD\";"); props.put("security.protocol", "SASL_SSL"); props.put("sasl.mechanism", "PLAIN"); props.put("ssl.protocol", "TLSv1.2"); props.put("ssl.enabled.protocols", "TLSv1.2"); props.put("ssl.endpoint.identification.algorithm", "HTTPS");
```

Para enviar mensagens, também será necessário especificar serializadores para as chaves e valores, por exemplo:

```
 props.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
 props.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");
```

Em seguida, use um KafkaProducer para enviar mensagens, em que cada mensagem é representada por um ProducerRecord. Não se esqueça de fechar o KafkaProducer quando tiver concluído. Esse código apenas envia a mensagem, mas não espera para ver se o envio foi bem-sucedido.

```
 Producer<String, String> producer = new KafkaProducer<>(props);
 producer.send(new ProducerRecord<String, String>("T1", "key", "value"));
 producer.close();
 ```
 
O método `send()` é assíncrono e retorna um Future que pode ser usado para verificar
sua conclusão:

```
 Future<RecordMetadata> f = producer.send(new ProducerRecord<String, String>("T1", "key", "value"));
// Do some other stuff
// Now wait for the result of the send
 RecordMetadata rm = f.get();
 long offset = rm.offset; 
```

Como alternativa, é possível fornecer um retorno de chamada ao enviar a mensagem:

```
producer.send(new ProducerRecord<String,String>("T1","key","value", new Callback() {
          public void onCompletion(RecordMetadata metadata, Exception exception) {
                     // This is called when the send completes, either successfully or with an exception
          }
});
```

Para obter mais informações, consulte o
[Javadoc para o cliente Kafka
![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://kafka.apache.org/11/javadoc/index.html?overview-summary.html){:new_window},
que é muito abrangente. 

