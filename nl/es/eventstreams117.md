---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-19"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: eventstreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Límites y cuotas
{: #kafka_quotas }

{{site.data.keyword.messagehub}} utiliza cuotas para controlar los recursos, como por ejemplo el ancho de banda de red, que un servicio puede consumir. Los tipos y niveles de las cuotas dependen de si está utilizando el plan Estándar o el de Empresa.

## Plan Estándar
{: #limits_standard }

### Rendimiento de red
{: #standard_throughput }

El rendimiento máximo de cada instancia de servicio equivale a 1 MB por segundo por cada partición hasta un máximo de 20 MB por segundo. Por ejemplo, para una instancia de servicio con 10 particiones, el rendimiento máximo es 10 MB por segundo, y, para 30 particiones, es 20 MB por segundo.

El rendimiento se mide por separado para los productores y los consumidores. Cuando se sobrepasa, se aplica la regulación (throttling) retrasando ligeramente las respuestas a las solicitudes, aplicando efectivamente un freno suave a los productores y consumidores hasta que se reduce el ancho de banda.

### Particiones
{: #standard_partitions}

100 particiones para cada instancia de servicio.

### Retención
{: #standard_retention}

Un máximo de 1 GB para cada partición.

### Otros límites
{: #standard_limits}

* Tamaño máximo de mensaje: 1 MB
* Número máximo de clientes Kafka activos simultáneamente: 100
* Tasa de solicitudes máxima [HTTP Produce API]: 100 por segundo
* Tasa de solicitudes máxima [HTTP Admin API]: 10 por segundo

## Plan Empresa
{: #limits_enterprise }

### Rendimiento de red
{: #enterprise_throughput }

Un máximo recomendado de 40 MB por segundo con un límite pico máximo de 75 MB por segundo. El rendimiento se expresa como el número de bytes por segundo que pueden enviarse y recibirse en un clúster.

La cifra recomendada se basa en una carga de trabajo típica y tiene en cuenta el posible impacto de acciones operativas como actualizaciones internas o modalidades de fallo, tales como la pérdida de una zona de disponibilidad. Si el rendimiento medio supera la cifra recomendada, se podría experimentar una pérdida de rendimiento mientras duren estas condiciones.


### Particiones
{: #enterprise_partitions}

1000 particiones para cada instancia de servicio.

### Retención
{: #enterprise_retention}

Ilimitado. hasta alcanzar el límite de almacenamiento del plan.

### Otros límites
{: #enterprise_limits}

*  Tamaño máximo de mensaje: 1 MB
*  Número máximo de clientes Kafka activos simultáneamente: 10000




















