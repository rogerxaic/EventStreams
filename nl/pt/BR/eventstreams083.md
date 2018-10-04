---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-30"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Limites máximos
{: #max_limits}

** A API do MQ Light está disponível como parte somente do plano Standard.**
<br/>

Os seguintes limites são aplicados à API do {{site.data.keyword.mql}}:
{:shortdesc}

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
