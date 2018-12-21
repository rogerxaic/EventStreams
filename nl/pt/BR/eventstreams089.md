---

copyright:
  years: 2015, 2018
lastupdated: "2018-11-08"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .deprecated}

# Ponte do Object Storage (descontinuada)
{: #object_storage_bridge }

** A ponte do {{site.data.keyword.objectstorageshort}} foi descontinuada em 1º de agosto de 2018. **
<br/>

Como o serviço subjacente ao qual a ponte do {{site.data.keyword.objectstorageshort}} se conecta foi descontinuado, a ponte do {{site.data.keyword.objectstorageshort}} também foi descontinuada em 1º de agosto de 2018. 

Quando o serviço {{site.data.keyword.objectstorageshort}} atingir seu término de vida e for desatribuído, todas as instâncias da ponte do {{site.data.keyword.objectstorageshort}} também serão desatribuídas. Para obter mais informações, consulte o [comunicado de descontinuação: {{site.data.keyword.objectstorageshort}} OpenStack Swift (PaaS) ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/blogs/bluemix/2018/05/end-marketing-object-storage-openstack-swift-paas/){:new_window}. 

Como alternativa, é possível usar a [ponte do Cloud Object Storage](/docs/services/EventStreams/eventstreams115.html){:new_window}.
{:deprecated}

A ponte do {{site.data.keyword.objectstorageshort}} permite arquivar dados de tópicos Kafka no {{site.data.keyword.messagehub}} em uma instância do serviço do {{site.data.keyword.Bluemix_short}} [{{site.data.keyword.objectstorageshort}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/ObjectStorage/index.html){:new_window}. A ponte consome lotes de mensagens do Kafka e faz upload dos dados da mensagem como objetos para um contêiner no serviço do {{site.data.keyword.objectstorageshort}}.

Observe que o serviço de armazenamento de objeto preferencial no {{site.data.keyword.Bluemix_short}} agora é o serviço do [{{site.data.keyword.IBM_notm}} Cloud Object Storage. ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/cloud-object-storage/about-cos.html){:new_window}.

Ao configurar a ponte do {{site.data.keyword.objectstorageshort}}, é possível controlar como os dados são transferidos por upload como objetos para o {{site.data.keyword.objectstorageshort}}. Por exemplo, as propriedades que podem ser configuradas são as seguintes:

* O nome do contêiner no qual os objetos são gravados.
* Com que frequência os objetos são transferidos por upload para o serviço do {{site.data.keyword.objectstorageshort}}.
* Quantia de dados que são gravados em cada objeto antes de serem transferidos por upload para o serviço do {{site.data.keyword.objectstorageshort}}.

O formato de saída da ponte é um objeto de serviço de armazenamento de objeto que contém um ou mais registros concatenados com caracteres de nova linha como separadores.

## Como os dados são transferidos usando a ponte do {{site.data.keyword.objectstorageshort}}
{: notoc}

A ponte do {{site.data.keyword.objectstorageshort}} funciona lendo um número de registros do Kafka de um tópico e gravando os dados desses registros em um objeto. Este objeto é transferido por upload para uma instância do serviço do {{site.data.keyword.objectstorageshort}}. Cada ponte do {{site.data.keyword.objectstorageshort}} lê dados de mensagens de um único tópico do Kafka, embora um tópico possa ter múltiplas pontes lendo dados dele. Uma nova instância da ponte do {{site.data.keyword.objectstorageshort}} sempre começa a ler no deslocamento inicial presente no tópico Kafka. A ponte do {{site.data.keyword.objectstorageshort}} usa o gerenciamento de deslocamento do consumidor Kafka para transferir os dados de forma confiável do Kafka sem perda, mas com uma chance pequena de duplicação.

É possível controlar quantos registros são lidos no Kafka antes que os dados sejam gravados na instância de serviço do {{site.data.keyword.objectstorageshort}} usando as propriedades a seguir. Especifique essas propriedades ao criar ou atualizar uma ponte:
<dl><dt>`"uploadDurationThresholdSeconds"`</dt> 
<dd>Define um período de tempo em segundos após o qual os dados acumulados no Kafka são transferidos por upload para o serviço do {{site.data.keyword.objectstorageshort}}.</dd>
<dt>`"uploadSizeThresholdKB"`</dt>
<dd>Controla a quantia de dados em kilobytes que são acumulados no Kafka antes de serem transferidos por upload para o serviço do {{site.data.keyword.objectstorageshort}}.</dd>
</dl>

A ponte do {{site.data.keyword.objectstorageshort}} é acionada para fazer upload dos
dados lidos no Kafka para o serviço do {{site.data.keyword.objectstorageshort}} quando um
desses valores é atingido pela primeira vez. A ponte do {{site.data.keyword.objectstorageshort}}
não garante transferência dos dados para o serviço do {{site.data.keyword.objectstorageshort}}
exatamente no ponto em que um desses limites é atingido. Portanto, os dados transferidos podem chegar mais
tarde ou ser maiores que os valores especificados por essas propriedades.

A ponte do {{site.data.keyword.objectstorageshort}} concatena mensagens usando caracteres de
nova linha como separadores à medida que ele grava os dados no {{site.data.keyword.objectstorageshort}}. Esses separadores tornam a ponte inadequada para mensagens que contêm caracteres de nova linha integrados e
para dados da mensagem binários.

## Como a ponte do {{site.data.keyword.objectstorageshort}} particiona dados em objetos
{: notoc}

Um dos recursos da ponte do {{site.data.keyword.objectstorageshort}} é a capacidade de
particionar mensagens do Kafka e de armazená-las como objetos nomeados com um prefixo comum. Um grupo de objetos
nomeados com um prefixo comum é chamado de partição. O
particionamento de ponte do {{site.data.keyword.objectstorageshort}}
possui um conceito diferente do particionamento Kafka.

Cada vez que a ponte do {{site.data.keyword.objectstorageshort}} possui um lote de dados para
fazer upload para o serviço do {{site.data.keyword.objectstorageshort}}, ela cria um ou mais
objetos contendo os dados. A decisão sobre como particionar as mensagens e nomear os objetos
resultantes depende de como a ponte está configurada. Os nomes de objetos podem conter metadados do Kafka e
possivelmente dados das mensagens em si. Atualmente, a ponte suporta as duas maneiras a seguir para
particionar mensagens do Kafka em objetos do {{site.data.keyword.objectstorageshort}}:

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
            "container" : "container1",
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
        &lt;container_name&gt;/offset=0/&lt;object_a&gt;
        &lt;container_name&gt;/offset=0/&lt;object_b&gt;
        &lt;container_name&gt;/offset=1000/&lt;object_c&gt;
        &lt;container_name&gt;/offset=2000/&lt;object_d&gt;
        ```       
    </code></pre>
    {:codeblock}
  
    
## Particionando pela data do ISO 8601
{: notoc}

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
        "container" : "container2",
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
        &lt;container_name&gt;/dt=2016-12-07/&lt;object_a&gt;
        &lt;container_name&gt;/dt=2016-12-08/&lt;object_b&gt;
        &lt;container_name&gt;/dt=2016-12-08/&lt;object_c&gt;
        ```       
    </code></pre>
    {:codeblock}

	Quaisquer dados da mensagem que possuam um formato JSON válido, mas sem um campo ou valor de data válido, são gravados em um objeto
com o prefixo `"dt=1970-01-01"`.

## Métricas de ponte do {{site.data.keyword.objectstorageshort}}
{: notoc}

A ponte do {{site.data.keyword.objectstorageshort}} relata métricas, que podem ser exibidas em seu painel usando Grafana. As métricas de interesse incluem o seguinte:
<dl>
<dt><code>*.<var class="keyword varname">topicName</var>.<var class="keyword varname">bridgeName</var>.bytes-consumed-rate</code></dt>
<dd>Mede a taxa em que a ponte consome dados (em bytes por segundo).</dd>
<dt><code>*.<var class="keyword varname">topicName</var>.<var class="keyword varname">bridgeName</var>.records-lag-max</code></dt>
<dd>Mede o atraso máximo no número de registros consumidos pela ponte para qualquer partição neste tópico. Um valor crescente ao longo do tempo indica que a ponte não está acompanhando os produtores para o tópico.</dd>
</dl>
