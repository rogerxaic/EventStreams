---

copyright:
  years: 2015, 2018
lastupdated: "2018-06-01"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Ponte do Cloud Object Storage 
{: #cloud_object_storage_bridge }

** A ponte do Cloud Object Storage está disponível somente como parte do plano Standard.**
<br/>

A ponte do {{site.data.keyword.IBM}} Cloud Object Storage fornece uma maneira de ler dados de um tópico do {{site.data.keyword.messagehub}} Kafka
e de colocá-los no [{{site.data.keyword.IBM_notm}} Cloud Object Storage ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/cloud-object-storage/about-cos.html){:new_window}.

A ponte do Cloud Object Storage permite arquivar dados de tópicos do Kafka no
{{site.data.keyword.messagehub}} em uma instância do serviço do [Cloud Object Storage ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/cloud-object-storage/about-cos.html){:new_window}. A ponte consome lotes de mensagens do Kafka e faz upload dos
dados da mensagem como objetos para um depósito no serviço do Cloud Object Storage. Ao configurar a ponte do
Cloud Object Storage, é possível controlar como os dados são transferidos por upload como objetos
para o Cloud Object Storage. Por exemplo, as propriedades que podem ser configuradas são as seguintes:

* O nome do depósito no qual os objetos são gravados.
* A frequência com que os objetos são transferidos por upload para o serviço do Cloud Object Storage.
* A quantia de dados que são gravados em cada objeto antes que seja transferido por upload para o
serviço do Cloud Object Storage.

O formato de saída da ponte é um objeto de serviço de armazenamento de objeto que contém um ou mais registros concatenados com caracteres de nova linha como separadores.

## Como os dados são transferidos usando a ponte do Cloud Object Storage
{: #data_transfer notoc}

A ponte do Cloud Object Storage funciona lendo um número de registros do Kafka de um tópico e gravando os
dados desses registros em um objeto. Este objeto é transferido por upload para uma instância do serviço do
Cloud Object Storage. Cada ponte do Cloud Object Storage lê dados de mensagem de um único tópico do Kafka,
embora um tópico possa ter múltiplas pontes lendo dados dele. Uma nova instância da ponte do Cloud Object
Storage sempre começa a leitura no deslocamento inicial presente no tópico do Kafka. A ponte do Cloud Object
Storage usa o gerenciamento de deslocamento do consumidor do Kafka para transferir dados de forma confiável do
Kafka sem perda, mas com uma pequena chance de duplicação.

É possível controlar quantos registros são lidos no Kafka antes de os dados serem gravados na instância
de serviço do Cloud Object Storage usando as propriedades a seguir. Especifique essas propriedades ao criar ou atualizar uma ponte:
<dl><dt>Limite de duração do upload (segundos)</dt> 
<dd>Define um período de tempo em segundos após o qual os dados acumulados no Kafka são transferidos por
upload para o serviço do Cloud Object Storage.</dd>
<dt>Limite de tamanho do upload (KB)</dt>
<dd>Controla a quantia de dados em kilobytes que são acumulados no Kafka antes de os dados serem transferidos
por upload para o serviço do Cloud Object Storage.</dd>
</dl>

O acionador para que a ponte do Cloud Object Storage faça upload dos dados lidos no Kafka
para o serviço do Cloud Object Storage é quando um desses valores é atingido pela primeira vez. A ponte do
Cloud Object Storage não garante que os dados serão transferidos para o serviço do Cloud Object Storage
exatamente no ponto em que um desses limites é atingido. Portanto, os dados transferidos podem chegar mais
tarde ou ser maiores que os valores especificados por essas propriedades.

A ponte do Cloud Object Storage concatena mensagens usando caracteres de nova linha como separadores à
medida que grava os dados no Cloud Object Storage. Esses separadores tornam a ponte inadequada para mensagens que contêm caracteres de nova linha integrados e
para dados da mensagem binários.


## Obtendo credenciais para uso com a ponte do Cloud Object Storage
{: notoc}

Deve-se fornecer credenciais para permitir que a ponte do Cloud Object Storage se
conecte à instância do seu Cloud Object Storage. Solicite que o proprietário ou o administrador de sua
instância do Cloud Object Storage crie as credenciais usando a UI do Cloud Object Storage da seguinte maneira: 

1. Selecione **Credenciais de serviço** e, em seguida, inclua uma **Nova credencial**. 
2. Para a **Nova credencial**, selecione uma **Função de acesso** de **Gravador** e um **ID de serviço** de **Gerar automaticamente**.
   
   É possível copiar o JSON resultante desta nova credencial para o painel do {{site.data.keyword.messagehub}} no console do {{site.data.keyword.Bluemix_notm}} ao criar uma ponte. 
   
   Como alternativa, é possível selecionar os campos <code>apikey</code> e <code>resource_instance_id</code> e colocá-los no painel do {{site.data.keyword.messagehub}} ou configurá-los na criação de ponte JSON, caso esteja criando a ponte diretamente usando uma chamada REST.

A credencial que é criada concede acesso de gravador à instância inteira do Cloud Object Storage,
portanto, talvez você queira restringir esse acesso ao depósito específico com o qual a ponte irá interagir.
1. Acesse a [Página de identidade & e de gerenciamento de acesso ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/iam/?env_id=ibm%3Ayp%3Aus-south#/serviceids){:new_window}.
2. Veja o ID de serviço gerado automaticamente nesta página. Quando tiver identificado o ID
específico, selecione a ação **Gerenciar ID do serviço**. 
3. Selecione a ação **Editar política** para restringi-la ainda mais a um **Tipo de recurso** específico, que é um depósito e um **ID do recurso**,
que é o nome do depósito. Clique **Salvar.**


## Criando uma ponte do Cloud Object Storage
{: notoc}

Para criar uma nova ponte do Cloud Object Storage usando a API de REST Kafka, use JSON como o exemplo a seguir. Assegure-se de que seus nomes de depósito sejam globalmente exclusivos, não apenas exclusivos dentro de sua instância do Cloud Object Storage.

<pre class="pre"><code>
{
  "name": "cosbridge",
  "topic": "kafka-java-console-sample-topic",
  "type": "objectStorageS3Out",
  "configuration" : {
    "credentials": {
      "endpoint" : "https://s3-api.us-geo.objectstorage.softlayer.net",
      "resourceInstanceId" : "crn::",
      "apiKey" : "your_api_key"
    },
    "bucket" : "cosbridge0",
    "uploadDurationThresholdSeconds" : 600,
    "uploadSizeThresholdKB" : 1024,
    "partitioning" : [ {
        "type" : "kafkaOffset"
      }
    ]
  }
}
</code></pre>
{: codeblock}


## Como a ponte do Cloud Object Storage particiona dados em objetos
{: notoc}

Um dos recursos da ponte do Cloud Object Storage é a capacidade de particionar
mensagens do Kafka e de armazená-las como objetos nomeados com um prefixo comum. Um grupo de objetos
nomeados com um prefixo comum é chamado de partição. O particionamento de ponte do Cloud Object Storage
é um conceito diferente do particionamento do Kafka.

Sempre que a ponte do Cloud Object Storage possui um lote de dados a ser transferido por upload para o
serviço do Cloud Object Storage, ela cria um ou mais objetos contendo os dados. A decisão sobre como particionar as mensagens e nomear os objetos
resultantes depende de como a ponte está configurada. Os nomes de objetos podem conter metadados do Kafka e
possivelmente dados das mensagens em si. Atualmente, a ponte suporta as duas maneiras a seguir para
particionar mensagens do Kafka nos objetos do Cloud Object Storage:

* Pelo deslocamento de mensagem do Kafka.
* Por uma data do ISO 8601 presente em cada mensagem do Kafka. Isso requer que as mensagens do Kafka
contenham um objeto no formato JSON válido.

## Particionando pelo deslocamento de mensagem do Kafka
{: notoc}

Para particionar dados por deslocamento de mensagem do Kafka, conclua as etapas a seguir:

1. Configure uma ponte sem a propriedade `"inputFormat"`.
2. Especifique um objeto com uma propriedade `"type"` de valor
`"kafkaOffset"` na matriz `"partitioning"`. 

    Por exemplo:
    <pre class="pre"><code>
        ```
        {
          "topic": "topic1",
          "type": "objectStorageOut",
          "name": "bridge1",
          "configuration" : {
            "credentials" : { ... },
            "bucket" : "bucket1",
            "uploadDurationThresholdSeconds" : "1000",
            "uploadSizeThresholdKB" : "1000",
            "partitioning" : [ {
                "type" : "kafkaOffset"
              }
            ]
          }
        }
        ```
     	</code></pre>
    {:codeblock}

    Os nomes de objeto gerados por uma ponte configurada dessa maneira contêm o prefixo `"
offset=<kafka_offset>"`, em que `"<kafka_offset>"` corresponde à primeira mensagem
do Kafka armazenada em tal partição (o grupo de objetos com este prefixo). Por exemplo, se uma ponte gera
objetos com nomes como o exemplo a seguir, `<object_a>` e `<object_b>`
contêm mensagens com deslocamentos no intervalo de 0 a 999, `<object_c>` contém mensagens com
deslocamentos no intervalo de 1000 a 1999, e assim por diante.

    <pre class="pre"><code>
        ```
        &lt;bucket_name&gt;/offset=0/&lt;object_a&gt;
        &lt;bucket_name&gt;/offset=0/&lt;object_b&gt;
        &lt;bucket_name&gt;/offset=1000/&lt;object_c&gt;
        &lt;bucket_name&gt;/offset=2000/&lt;object_d&gt;
        ```       
    </code></pre>
    {:codeblock}
  
    
## Particionando pela data do ISO 8601
{: #partition_iso notoc}

Para particionar dados pela data do ISO 8601, conclua as etapas a seguir:

1. Configure uma ponte com a propriedade `"inputFormat"` configurada como
`"json"`. Não é possível usar uma propriedade `"inputFormat"`
além de `"json"`.
2. Especifique um objeto com um a propriedade `"type"` de valor
`"dateIso8601"` e uma propriedade `"propertyName"` na
matriz `"partitioning"`. 

	Por exemplo:
    <pre class="pre"><code>
    ```
    {
      "topic": "topic2",
      "type": "objectStorageOut",
      "name": "bridge2",
      "configuration" : {
        "credentials" : { ... },
        "bucket" : "bucket2",
        "inputFormat" : "json",
        "uploadDurationThresholdSeconds" : "1000",
        "uploadSizeThresholdKB" : "1000",
        "partitioning": [
          {
            "type": "dateIso8601",
            "propertyName": "timestamp"
          }
        ]
      }
    }
    ```
    </code></pre>
    {:codeblock}

	O particionamento pela data do ISO 8601 requer que as mensagens do Kafka tenham um formato JSON
válido. O valor de `"propertyName"` no JSON usado para configurar a ponte deve corresponder
ao campo de data do ISO 8601 em cada mensagem do Kafka. Neste exemplo, o campo `"timestamp"`
deve conter um valor de data de ISO 8601 válido. Em seguida, as mensagens são particionadas de acordo com
suas datas.
	
	Uma ponte configurada como este exemplo gera objetos com nomes especificados como segue:
 `<object_a>` contém mensagens JSON com campos `"timestamp"` com
 uma data de 07/12/2016 e `<object_b>` e `<object_c>` contêm mensagens JSON com campos `"timestamp"` com uma data de
 08/12/2016.

    <pre class="pre"><code>
        ```
        &lt;bucket_name&gt;/dt=2016-12-07/&lt;object_a&gt;
        &lt;bucket_name&gt;/dt=2016-12-08/&lt;object_b&gt;
        &lt;bucket_name&gt;/dt=2016-12-08/&lt;object_c&gt;
        ```       
    </code></pre>
    {:codeblock}

	Quaisquer dados da mensagem que possuam um formato JSON válido, mas sem um campo ou valor de data válido, são gravados em um objeto
com o prefixo `"dt=1970-01-01"`.

## Métricas de ponte do Cloud Object Storage
{: notoc}

A ponte do Cloud Object Storage relata métricas, que podem ser exibidas em seu painel usando o Grafana. As métricas de interesse incluem o seguinte:
<dl>
<dt><code>*.<var class="keyword varname">topicName</var>.<var class="keyword varname">bridgeName</var>.bytes-consumed-rate</code></dt>
<dd>Mede a taxa em que a ponte consome dados (em bytes por segundo).</dd>
<dt><code>*.<var class="keyword varname">topicName</var>.<var class="keyword varname">bridgeName</var>.records-lag-max</code></dt>
<dd>Mede o atraso máximo no número de registros consumidos pela ponte para qualquer partição neste tópico. Um valor crescente ao longo do tempo indica que a ponte não está acompanhando os produtores para o tópico.</dd>
</dl>
