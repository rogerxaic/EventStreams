---

copyright:
  years: 2015, 2019
lastupdated: "2018-07-05"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Utilización de la API cliente Kafka Java de administración
{: #kafka_java_api}


<!-- 
17/10/17 - Karen: following info duplicated at messagehub108
 -->

Si está utilizando un cliente Kafka de la versión 0.11 o posterior, o bien Kafka Streams versión 0.10.2.0 o posterior, puede utilizar la API para crear y suprimir temas. Existen varias restricciones para la configuración permitida al crear temas. Actualmente, solo se pueden modificar los siguientes valores:
{: shortdesc}

<dl>
<dt>cleanup.policy</dt>
<dd>Establezca este valor en <code>delete</code> (predeterminado), <code>compact</code> o <code>delete,compact</code>
<p>**Nota:**
si la política de limpieza es solo <code>compact</code>, se añade automáticamente <code>delete</code>, pero se inhabilita la supresión en función del tiempo. Los mensajes del tema se compactan a 1 GB antes de la supresión.</p>
</dd>

<dt>retention.ms</dt>
<dd>El período de retención predeterminado es de 24 horas. El valor mínimo es de 1 hora y el máximo de 30 días. Especifique este valor como múltiplo de horas.

<p>**Nota:**
en el plan de Empresa, puede definir el valor que desee.</p>
</dd>

<dt>retention.bytes</dt>
<dd>El tamaño máximo que puede alcanzar una partición (que consiste en segmentos de registro) hasta que se descartan segmentos de registro antiguos para liberar espacio.

<p>**Nota:** solo plan de Empresa. Defina cualquier valor mayor que 1 MB.</p>
</dd>

<dt>segment.bytes</dt>
<dd>El tamaño del archivo de segmento para el registro.

<p>**Nota:** solo plan de Empresa. Defina cualquier valor mayor que 100 kB.</p>
</dd>

<dt>segment.index.bytes</dt>
<dd>El tamaño del índice que correlaciona desplazamientos con posiciones de archivo. 

<p>**Nota:** solo plan de Empresa. Defina cualquier valor comprendido entre 100 kB y 2 GB.</p>
</dd>

<dt>segment.ms</dt>
<dd>El periodo de tiempo tras el que Kafka impondrá la acumulación del registro aunque el archivo de segmento no esté lleno. 

<p>**Nota:** solo plan de Empresa. Defina cualquier valor comprendido entre 5 minutos y 30 días.</p>
</dd>
</dl>

