---

copyright:
  years: 2015, 2019
lastupdated: "2018-05-30"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

<!-- 15/11/18: info moved to eventstreams075.md, moved because of doc app changes -->
# Límites máximos
{: #max_limits}

**La API MQ Light solo está disponible como parte del plan Estándar.**
<br/>

Se aplican los siguientes límites a la API de {{site.data.keyword.mql}}:
{:shortdesc}

* La cantidad máxima de datos que pueden almacenarse está en consonancia con una única partición de Kafka para su plan de pago (normalmente 1 GB). Si se supera este límite de datos, se eliminarán los mensajes más antiguos de la partición a medida que se envíen mensajes nuevos.
* La cantidad máxima de tiempo que puede almacenarse un mensaje está en consonancia con una única partición de Kafka para su plan de pago (normalmente 24 horas). No se pueden recuperar mensajes con una antigüedad superior a este periodo de tiempo.
* El tamaño máximo de un mensaje (sin incluir cabeceras) es de 1 MB.
* El número máximo de clientes que pueden estar conectados a la vez es de 25.
* El número máximo de destinos que pueden estar activos a la vez es de 25. Un destino activo se define de la siguiente manera:
  - Un destino con un TimeToLive > 0 tanto con cliente conectado actualmente como sin él.
  - Un destino con un TimeToLive = 0 (valor predeterminado) donde un cliente está conectado. 
  <p>Un destino que se comparte entre los clientes cuenta como un destino único.</p>
