---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

<!-- 14/11/18: info moved to eventstreams075.md, moved because of doc app changes -->
# Como conectar aplicativos MQ Light existentes
{: #mql_exist_apps}

** A API do MQ Light está disponível como parte somente do plano Standard.**
<br/>

É possível conectar aplicativos existentes que são executados atualmente com relação ao serviço do
{{site.data.keyword.IBM_notm}} MQ ou do {{site.data.keyword.mql}}
{{site.data.keyword.Bluemix_notm}} ao serviço. Os aplicativos continuam a funcionar da mesma forma.
{:shortdesc}

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
informações, consulte [Limites máximos](/docs/services/EventStreams/eventstreams083.html).
* O JMS não é suportado.
