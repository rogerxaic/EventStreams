---

copyright:
  years: 2015, 2018
lastupdated: "2018-06-26"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Restrições conhecidas
{: #restrictions}

Se você localizar um problema ao usar o {{site.data.keyword.messagehub}}, revise estas restrições e soluções
alternativas conhecidas. 


## As chamadas do Java Kafka não executarão failover se um servidor de autoinicialização Kafka falhar
{: #calls_failover}

### Problema
{: #calls_failover_problem notoc}

A Java Virtual Machine (JVM) armazena em cache as consultas DNS. Quando a JVM resolve um endereço IP para um nome de host,
ela armazena em cache o endereço IP por um período de tempo especificado, conhecido como tempo de vida (TTL). Algumas
configurações Java configuram a JVM TTL para que ela nunca atualize o endereço IP de um nome do host até que a JVM seja reiniciada. Uma configuração de exemplo é aquela que tem um gerenciador de segurança.

### Solução alternativa
{: #calls_failover_workaround notoc}

Como o {{site.data.keyword.messagehub}} usa URLs do servidor de autoinicialização Kafka com múltiplos endereços
IP para alta disponibilidade, nem todos os endereços IP do broker são conhecidos pelo cliente Kafka, o que evita o failover para um
broker ativo. Nesses casos, o failover requer uma nova consulta dos endereços IP para as URLs do broker para obter um endereço IP
ativo. Recomenda-se configurar a sua JVM com um valor TTL de 30 a 60 segundos. Esse valor assegura que, se um endereço IP do servidor
de autoinicialização tiver problemas, o cliente Kafka será capaz de consultar e usar um novo endereço IP consultando o DNS.

No arquivo <code>java.security</code>: 

```
# The Java-level namelookup cache policy for successful lookups:
#
# any negative value: caching forever
# any positive value: the number of seconds to cache an address for
# zero: do not cache
#
# default value is forever (FOREVER). For security reasons, this
# caching is made forever when a security manager is set. When a security
# manager is not set, the default behavior in this implementation
# is to cache for 30 seconds.
#
# NOTE: setting this to anything other than the default value can have
#       serious security implications. Do not set it unless
#       you are sure you are not exposed to DNS spoofing attack.
#
#networkaddress.cache.ttl=-1
```

### Como modificar o TTL da JVM
* Para modificar o TTL da JVM para todos os aplicativos, configure o valor <code>networkaddress.cache.ttl</code> no
arquivo <code><var class="keyword varname">$JAVA_HOME</var>/jre/lib/security/java.security</code>.
* Para modificar o TTL da JVM para um determinado aplicativo, configure o <code>networkaddress.cache.ttl</code> em seu código do aplicativo da seguinte forma:
```
java.security.Security.setProperty("networkaddress.cache.ttl" , "30");
```

## Chamadas do Java Kafka podem atingir o tempo limite
{: #calls_timeout}

### Problema
{: #calls_timeout_problem notoc}

Às vezes, uma chamada do cliente Java Kafka falha ao localizar o Kafka. A causa da falha é que o cliente Kafka determinou o
mesmo endereço IP com falha para cada um dos servidores de autoinicialização. O cliente Kafka tenta o endereço IP de cada broker
(que é o mesmo endereço IP com falha) e determina incorretamente que o Kafka está inativo. Observe que o cliente Kafka usará o
primeiro endereço IP retornado na lista se múltiplos endereços IP forem retornados na consulta DNS.

### Solução alternativa
{: #calls_timeout_workaround notoc}

Tente novamente as suas chamadas depois de esperar tempo suficiente para que o cache DNS da JVM para as URLs do broker
expirem. Nas chamadas Kafka subsequentes, um endereço IP do broker ativo deve ser retornado da consulta DNS e usado. 

Uma Kafka Improvement Proposal (KIP) nº 302 foi criada para assegurar que os clientes Kafka testem todos os endereços
IP dos brokers disponíveis e não um subconjunto, portanto, uma falha em um único endereço IP não causará uma falha.


## Tópicos e partições
{: #topics_partitions}

*  Os nomes dos tópicos são restritos a um máximo de 100 caracteres.
*  O número padrão de partições para um tópico é um.
*  Cada espaço do {{site.data.keyword.Bluemix_notm}} possui um limite de 100 partições. Para criar
                    mais partições, deve-se usar um novo espaço do {{site.data.keyword.Bluemix_notm}}.

## Retenção de mensagem
{: #message_retention}

Por padrão, as mensagens são retidas no Kafka por até 24 horas e cada partição é limitada a 1 GB. Se um valor máximo de 1 GB for atingido, as mensagens mais antigas serão descartadas para permanecerem
no limite.

É possível mudar o limite de tempo para retenção mensagem ao criar um tópico usando a interface
com o usuário ou a API de administração. O limite de tempo é um mínimo de uma hora e um máximo de 30 dias.

Para obter informações sobre restrições nas configurações permitidas ao criar tópicos usando um cliente Kafka ou o Kafka
Streams, consulte [APIs para administração de tópico](/docs/services/EventStreams/eventstreams104.html).

## Criando e excluindo tópicos no Kafka
{: #create_delete}

No Kafka, a criação e a exclusão de tópico são operações assíncronas que podem levar algum tempo
para serem concluídas. É recomendável evitar o uso de padrões que dependam da rápida criação e exclusão
de tópicos ou da rápida exclusão e recriação de tópicos.

## API REST Kafka
{: #trouble_rest}

*  Somente o formato integrado binário é suportado para solicitações e respostas. Os formatos integrados do Avro e JSON não são suportados.
*  Solicitações simultâneas não são suportadas para uma instância do consumidor.
   Solicitações de leitura, confirmação
                    ou exclusão correspondentes a uma instância do consumidor devem ser enviadas somente após uma
                    resposta ser recebida para quaisquer solicitações pendentes dessa instância.

## Limitação de taxa da API de REST do Kafka
{: #kafka_rate}

Os aplicativos que usam a API de REST do Kafka podem estar sujeitos à limitação de taxa para cada
ApiKey. Quando esta limitação ocorre, a API responde com o erro HTTP a seguir:

<code>429 Too Many Requests</code>
{:screen}

Se este erro ocorrer, aguarde e envie a solicitação novamente.

<!--12/04/18 - Karen: same info duplicated at messagehub108 -->
## Reinício diário da API de REST do Kafka
{: #rest_restart}

A API de REST do Kafka reinicia uma vez por dia por um curto período de tempo. Durante esse período, a
API de REST do Kafka pode se tornar indisponível. Se isso acontecer, é recomendado tentar novamente
sua solicitação. Após a API de REST ser reiniciada, será necessário recriar suas instâncias do consumidor
do Kafka. Se este for o caso, a API de REST retornará o JSON a seguir:

```'{"error_code":40403,"message":"Consumer instance not found."}'
```
{:screen}

## API de consumidor de alto nível Kafka
{: #kafka_consumer}

Não é possível usar Apache Kafka 0.8.2 simples ou a API de consumidor de alto nível com o {{site.data.keyword.messagehub}}. Em vez disso, é possível usar a API de consumidor do Kafka mais antiga suportada, que é a 0.9.
