---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-16"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Liderazgo de particiones
{: #partition_leadership }

Cada partición tiene un servidor en el clúster que actúa como líder de la partición y otros servidores que actúan como seguidores. El líder gestiona todas las solicitudes de producción y consumo. Los seguidores replican los datos de la partición del líder con el objetivo de mantenerse actualizados con el líder. Si un seguidor se mantiene actualizado con el líder de una partición, la réplica del seguidor está sincronizada. 

Cuando se envía un mensaje al líder de la partición, este mensaje no está disponible inmediatamente para los consumidores. El líder añade el registro del mensaje a la partición, asignándole el siguiente número de desplazamiento para dicha partición. Después de que todos los seguidores de las réplicas sincronizadas hayan replicado el registro y reconocido que han anotado el registro en sus réplicas, el registro ahora está *confirmado*. El mensaje está disponible para los consumidores.

Si el líder de una partición falla, uno de los seguidores con una réplica sincronizada asume el rol de líder de la partición automáticamente. En la práctica, cada servidor es el líder de algunas particiones y seguidor para otras. El liderazgo de particiones es dinámico y cambia a medida que cambian los servidores.

Las aplicaciones no necesitan realizar ninguna acción determinada para gestionar el liderazgo de una partición. La biblioteca de cliente Kafka se reconecta automáticamente al nuevo líder, aunque observará una latencia mayor mientras se establece el clúster.
