---

copyright:
  years: 2015, 2019
lastupdated: "2018-11-20"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Usando a API do MQ Light 
{: #mql_using}

** A API do MQ Light está disponível como parte somente do plano Standard.**
<br/>

A API do {{site.data.keyword.mql}} é fornecida para compatibilidade com versões anteriores com o serviço
{{site.data.keyword.mql}} anterior. A API fornece uma interface de sistema de mensagens baseada em AMQP para o Java&trade;,
o Node.js, o Python e o Ruby. 
{:shortdesc}

<!-- 02/07/18 - removing words to help deprecate MQ Light
In most cases, {{site.data.keyword.messagehub}} is best used with a Kafka client. The {{site.data.keyword.mql}} API is simple to learn but has very limited scalability and does not offer interoperability with other {{site.data.keyword.messagehub}} APIs.
The {{site.data.keyword.mql}} API is available in the following {{site.data.keyword.Bluemix_short}} regions only: US South, United Kingdom, and Sydney. The {{site.data.keyword.mql}} API not available in the Germany region or in {{site.data.keyword.Bluemix_notm}} Dedicated.
-->

## O que é API do MQ Light e como ela é diferente?
{: #mqlight}

<!-- 30/10/18: info was in eventstreams106.md, moved because of doc app changes -->

A API do {{site.data.keyword.mql}} fornece um nível mais alto de abstração para o sistema de mensagens comparado ao
Kafka.

A escolha entre o uso de um cliente Kafka ou da API do {{site.data.keyword.mql}} depende da
topologia do sistema de mensagens que você deseja construir.

* Com o Kafka, você usa um pequeno número de tópicos e pode ter múltiplas partições para cada tópico
para escalabilidade adicional. É possível compartilhar mensagens entre os consumidores usando grupos de
consumidores, mas cada consumidor deve poder acompanhar a taxa de mensagens para as partições designadas a
ele.
* Com a API do {{site.data.keyword.mql}}, é possível usar um número muito maior de tópicos
e os nomes de tópico são hierárquicos (por exemplo: <code>&lsquo;/sports/football&rsquo;</code> e <code>&lsquo;/sports/tiddlywinks&rsquo;</code>).  

Os tópicos na API do {{site.data.keyword.mql}} não são os mesmos que os tópicos do Kafka. Em vez
disso, a API do {{site.data.keyword.mql}} usa um tópico do Kafka único chamado "MQLight" e todas as
mensagens enviadas e recebidas usando a API do {{site.data.keyword.mql}} passam por esse tópico
do Kafka.

O {{site.data.keyword.mql}} está disponível somente nas seguintes localizações (regiões) do {{site.data.keyword.Bluemix_notm}}: Dallas (us-south), Londres (eu-gb) e Sydney (au-syd). A API do MQ Light não está disponível na localização de Frankfurt (eu-de) ou no {{site.data.keyword.Bluemix_notm}} Dedicated.

Para obter mais informações sobre como escolher entre as APIs, consulte [Escolhendo entre as três APIs](/docs/services/EventStreams/eventstreams087.html).


## O que é necessário para usar a API do MQ Light com o {{site.data.keyword.messagehub}}?
{: #mql_reqs}

<!-- 30/10/18: info was in eventstreams098.md, moved because of doc app changes -->

Os requisitos a seguir são necessários para usar a API do {{site.data.keyword.messagehub}}
com o {{site.data.keyword.mql}}: 

**Deve-se criar explicitamente um tópico de Kafka denominado "MQLight" antes de poder usar a API porque todas as mensagens passam pelo tópico "MQLight". Este tópico deve ter uma única partição. A criação deste tópico ativa a API do MQ Light para a sua instância de serviço. Os tópicos usados na API do MQ Light são criados automaticamente à medida que são usados, mas todas as mensagens estão realmente no único tópico "MQLight" Kafka.** 

O tópico "MQLight" é usado pela API MQ Light para armazenar seus dados da mensagem e interagir com outros clientes Kafka. Saiba que, quando este tópico é criado, ele incorre em encargos com a taxa padrão descrita no plano de pagamento dos serviços.

Para desativar a API MQ Light, exclua o tópico "MQLight". Observe que todos os dados foram destruídos quando o tópico foi excluído.

<!-- 12/11/18: following info was in eventstreams079.md, moved because of doc app changes -->

## Como conectar e autenticar
{: #mql_connect}

Para conectar um aplicativo ao serviço, o aplicativo deve usar os detalhes de <code>user</code>,
<code>password</code> e <code>mqlight_lookup_url</code> da
[variável de ambiente VCAP_SERVICES](/docs/services/EventStreams/eventstreams127.html). Use a
seguinte orientação para sua linguagem escolhida:

**Para Java**

Se você especificar &lsquo;null&rsquo; como o parâmetro endpointService da chamada create(), isto instruirá o cliente a ler os detalhes de <code>user</code>, <code>password</code> e
<code>mqlight_lookup_url</code> no VCAP_SERVICES:

<pre>
<code>NonBlockingClient.create(null, new NonBlockingClientAdapter<Void>() {
    public void onStarted(NonBlockingClient client, Void context) {
                client.send("my/topic", "Hello World!", null);
    }
}, null);</code>
</pre>
{:codeblock}

<br>

**Para Node.js**

Recupere <code>user</code>, <code>password</code> e
detalhes de <code>mqlight_lookup_url</code> de VCAP_SERVICES; use para criar o cliente conforme a seguir:

<pre>
<code>var services = JSON.parse(process.env.VCAP_SERVICES);
mqlightService = services['messagehub'][0];
opts.service = mqlightService.credentials.mqlight_lookup_url;
opts.user = mqlightService.credentials.user;
opts.password = mqlightService.credentials.password;
var mqlightClient = mqlight.createClient(opts, function(err) {
...</code>
</pre>
{:codeblock}

<br>

**Para Ruby**

Recupere <code>user</code>, <code>password</code> e
detalhes de <code>mqlight_lookup_url</code> de VCAP_SERVICES; use para criar o cliente conforme a seguir:
<pre>
<code>vcap_services = JSON.parse(ENV['VCAP_SERVICES'])
conn_details = vcap_services['messagehub']
credentials = conn_details.first['credentials']
service = credentials['mqlight_lookup_url']
opts[:user] = credentials['user']
opts[:password] = credentials['password']
set :client, Mqlight::BlockingClient.new(service, opts)
...</code>
</pre>
{:codeblock}

<br>

**Para Python**

Recupere <code>user</code>, <code>password</code> e
detalhes de <code>mqlight_lookup_url</code> de VCAP_SERVICES; use para criar o cliente conforme a seguir:
<pre>
<code>vcap_services = json.loads(os.environ.get('VCAP_SERVICES'))
conn_details = vcap_services['messagehub'][0]
service = str(conn_details['credentials']['mqlight_lookup_url'])
security_options = {
      'user': str(conn_details['credentials']['user']),
      'password': str(conn_details['credentials']['password'])
}
client = mqlight.Client(service=service, 
                        security_options=security_options,
                        on_started=on_started)</code>
</pre>
{:codeblock}

<br>

Para obter mais informações sobre as APIs do {{site.data.keyword.mql}}, consulte: site
[{{site.data.keyword.mql}} developerWorks
&reg; ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/messaging/mq-light/){:new_window}.


<!-- 14/11/18: info was in eventstreams080.md, moved because of doc app changes -->

## Como conectar aplicativos MQ Light existentes
{: #mql_exist_apps}


É possível conectar aplicativos existentes que são executados atualmente com relação ao serviço do
{{site.data.keyword.IBM_notm}} MQ ou do {{site.data.keyword.mql}}
{{site.data.keyword.Bluemix_notm}} ao serviço. Os aplicativos continuam a funcionar da mesma forma.

Para conectar aplicativos existentes, conclua as seguintes verificações:

* O app deve usar a versão mais recente disponível do cliente da API {{site.data.keyword.mql}}
para o seu idioma.
* Verifique se os detalhes da conexão extraídos do VCAP_SERVICES referenciam o
tipo de serviço <code>messagehub</code>, recuperam o nome de usuário de conexões por meio da propriedade
<code>credentials.user</code> em vez da propriedade <code>credentials.username</code> e recuperam a URL de
consulta de conexão por meio da propriedade <code>credentials.mqlight_lookup_url</code> em vez da propriedade
<code>credentials.connectionLookupURI</code>. Para obter mais informações, consulte
[Variável de ambiente VCAP_SERVICES](/docs/services/EventStreams/eventstreams127.html).

	Observe que esta etapa será executada se o cliente Java&trade; for utilizado e se 'null'
for especificado como o parâmetro endpointService na chamada create(), para que o cliente recupere as informações
sozinho.
	
* Seu aplicativo deve suportar conexões TLS v1.2. O VCAP_SERVICES não contém mais uma
propriedade <code>credentials.nonTLSConnectionLookupURI</code> para criar uma conexão não TLS.

Também é necessário observar as seguintes informações:

* Os limites de mensagens são consistentes com o {{site.data.keyword.messagehub}}, mas podem
ser diferentes de outros servidores que suportam a API do {{site.data.keyword.mql}}. Para obter mais
informações, consulte [Limites máximos](/docs/services/EventStreams/eventstreams075.html#max_limits).
* O JMS não é suportado.

<!-- 15/11/18: info was in eventstreams081.md, moved because of doc app changes -->

## Trocando mensagens entre a API do MQ Light e o Kafka ou APIs de REST do Kafka
{: #mql_exchange}

As mensagens do {{site.data.keyword.mql}} são armazenadas em um único tópico do Kafka
subjacente chamado "MQLight" e codificadas conforme detalhado na tabela a seguir. Esta codificação também pode
ser usada por outros tipos de API, como Kafka ou REST Kafka, para trocar mensagens com aplicativos
usando a API do {{site.data.keyword.mql}}.

### Formato da mensagem do Kafka
{: #kafka_format notoc}

<table border='1'>
<caption>Tabela 1. Formato da mensagem do Kafka</caption>
  <tr>
    <th> Key </th>
    <th> Valor </th>
  </tr>
  <tr>
    <td> Opcional (não usado pela API)
	<p></p>
	</td>
    <td>**1 byte**
	<p>		     Destaque da API do MQ Light, que é sempre 0xFA.</p>
    <p><var class="keyword varname">**n**</var> **bytes**</p>
    <p>		    Mensagem codificada do AMQP (formatada com base no formato de ligação do AMQP). </p></td>
  </tr>
</table>


<!-- 15/11/18: info was in eventstreams082.md, moved because of doc app changes -->

## Amostras da API do MQ Light
{: #mql_samples}

Se você ainda não tem um aplicativo, tente a API do {{site.data.keyword.mql}} com uma das
amostras.

Na realidade, o aplicativo de amostra consiste em dois aplicativos simples: um frontend da web
que envia mensagens para um backend e um backend que processa as mensagens, altera as palavras para letras
maiúsculas e, em seguida, retorna as mensagens para o frontend. Essa amostra exibe como é possível que os apps conversem entre si usando
a API do {{site.data.keyword.mql}}. Também é possível usar a API do {{site.data.keyword.mql}} para executar a transferência do
trabalhador; um recurso-chave necessário para a construção de aplicativos escaláveis, fracamente acoplados e
distribuídos.

É possível localizar o código de amostra no [projeto do GitHub event-streams-samples ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/ibm-messaging/event-streams-samples/tree/master/mqlight){:new_window}.


<!-- 15/11/18: info was in eventstreams083.md, moved because of doc app changes -->

## Limites máximos
{: #max_limits}

Os seguintes limites são aplicados à API do {{site.data.keyword.mql}}:

* A quantia máxima de dados que podem ser armazenados é consistente com uma partição única do Kafka
para seu plano de pagamento (geralmente 1 GB). Se esse limite de dados for excedido, as mensagens mais antigas
na partição serão removidas conforme novas mensagens forem enviadas.
* O tempo máximo em que uma mensagem é armazenada é consistente com uma partição única do Kafka para seu
plano de pagamento (geralmente 24 horas). Não é possível recuperar mensagens que são mais antigas que esse
período de tempo.
* O tamanho máximo de uma mensagem (excluindo cabeçalhos) é de 1 MB.
* O número máximo de clientes que podem ser conectados em uma única vez é 25.
* O número máximo de destinos que podem estar ativos em uma única vez é 25. Um destino ativo é definido
conforme a seguir:
  - Um destino com um TimeToLive > 0 com ou sem um cliente atualmente conectado.
  - Um destino com um TimeToLive = 0 (o padrão) em que um cliente está conectado. 
  <p>Um destino que é compartilhado entre os clientes conta como um único destino.</p>
