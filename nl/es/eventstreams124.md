---

copyright:
  years: 2015, 2018
lastupdated: "2018-07-04"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Gestión del acceso a los recursos de {{site.data.keyword.messagehub}} (plan Empresa)
{: #security }

Puede proteger los recursos de {{site.data.keyword.messagehub}} de forma muy precisa para gestionar el acceso que desea otorgar a cada usuario sobre cada recurso.

## ¿Qué puedo proteger?

Dentro de {{site.data.keyword.messagehub}}, puede proteger el acceso a los recursos siguientes:
* Clúster (cluster): puede controlar las aplicaciones y los usuarios que se pueden conectar con el servicio
* Temas (topic): puede controlar la capacidad de los usuarios y de las aplicaciones para crear, suprimir, leer y escribir en un tema 
* Grupos de consumidores (group): puede controlar la capacidad de una aplicación para unirse a un grupo de consumidores 
* Transacciones de productor (txnid): puede controlar la capacidad para utilizar la función de productor de transacciones en Kafka (es decir, escrituras únicas y atómicas entre varias particiones)

Los niveles de acceso (también conocidos como rol) que puede asignar a un usuario sobre cada recurso son los siguientes:

| Rol acceso | Descripción de acciones | Acciones de ejemplo |
|:-----------------|:-----------------|:-----------------|
|  Lector | Realizar acciones de solo lectura dentro de {{site.data.keyword.messagehub}}, como por ejemplo ver recursos | Permitir que una app se conecte a un clúster mediante la asignación de acceso de lectura al tipo de recurso de clúster |
| Escritor | Los escritores tienen más permisos que el rol de lector, que incluyen la creación y edición de recursos de {{site.data.keyword.messagehub}}. | Permitir que una app produzca en temas mediante la asignación del acceso de escritura a los tipos recurso de tema y nombre de tema|
| Gestor | Los gestores tienen más permisos que el rol de escritor para completar acciones con privilegios. Además, pueden crear y editar recursos de {{site.data.keyword.messagehub}}. | Permitir el acceso completo a todos los recursos mediante la asignación del acceso de gestión a la instancia de {{site.data.keyword.messagehub}}|
{: caption="Tabla 1. Ejemplo de acciones y roles de usuario de {{site.data.keyword.messagehub}}" caption-side="top"}

<!-- comment from Charlie and my reply 
CM: need to confirm if hierarchical e.g. write includes read - and doc. 
KR: I think they do inherit the lower level access https://console.bluemix.net/docs/iam/users_roles.html#iamusermanrol 
-->


## ¿Cómo se asigna el acceso?

Se adjuntan políticas de Cloud Identity and Access Management (IAM) a los recursos que se van a controlar. Cada política define el nivel de acceso que debe tener un usuario concreto y sobre qué recurso o conjunto de recursos. Una política consta de la información siguiente: 
* El tipo de servicio al que se aplica la política. Por ejemplo, {{site.data.keyword.messagehub}}. Puede definir el ámbito de una política de modo que incluya todos los tipos de servicio. 
* La instancia del servicio que se va a proteger. Puede definir el ámbito de una política de modo que incluya todas las instancias de un tipo de servicio. 
* El tipo de recurso que se va a proteger. Los valores válidos son <code>cluster</code>, <code>topic</code>, <code>group</code> o <code>txnid</code>. La especificación de un tipo es opcional. Si no especifica un tipo, la política se aplica a todos los recursos de la instancia de servicio. 
* El recurso que se va a proteger. Especifique este valor para los recursos de tipo <code>topic</code>, <code>group</code> y <code>txnid</code>. Si no especifica el recurso, la política se aplica a todos los recursos del tipo especificado en la instancia de servicio. 
* El rol asignado al usuario. Por ejemplo, lector, escritor o gestor. 

## ¿Cuáles son los valores de seguridad predeterminados?

De forma predeterminada, cuando se suministra {{site.data.keyword.messagehub}}, al usuario que lo ha suministrado se le otorga el rol de gestor sobre todos los recursos de la instancia. Además, cualquier usuario que tenga un rol de gestor sobre 'todos' los servicios o 'todas' las instancias de servicio de {{site.data.keyword.messagehub}} en la misma cuenta también dispone de acceso completo. 

Luego puede aplicar políticas adicionales para ampliar el acceso a otros usuarios. Puede definir el ámbito de una política de modo que se aplique a {{site.data.keyword.messagehub}} en conjunto o a recursos individuales de {{site.data.keyword.messagehub}}. Para obtener más información, consulte [Casos de ejemplo habituales](#security_scenarios).

Solo los usuarios con un rol de administración sobre una cuenta pueden asignar políticas a los usuarios. Para asignar políticas, utilice el panel de control de IBM Cloud o los mandatos **ibmcloud**. 
<!--
For example steps for {{site.data.keyword.messagehub}}, see [Examples](#security_examples).
-->


## Casos de ejemplos habituales
{: #security_scenarios }

En esta tabla se resumen algunos casos de ejemplo habituales de {{site.data.keyword.messagehub}} y el acceso que necesita asignar:

| Acción | Rol de lector | Rol de escritor | Rol de gestor |
|---------|----------------|
| Permitir acceso completo a todos los recursos|No aplicable   |No aplicable  |Instancia de servicio: <var class="keyword varname">instancia_servicio</var>|
| Permitir que una app o un usuario cree o suprima un tema |Tipo de recurso: <code>cluster</code>   |No aplicable  |Tipo de recurso: topic <br/><br/>Opcional: ID de recurso: <var class="keyword varname">nombre_tema</var> |
| Obtener una lista de grupos, temas y desplazamientos <br/> Describir configuraciones de grupo, tema e intermediario | Tipo de recurso: <code>cluster</code>      |No aplicable  |No aplicable      |
| Permitir que una app se conecte al clúster  |Tipo de recurso: <code>cluster</code>| No aplicable     |No aplicable      |
| Permitir que una app produzca en cualquier tema  |Tipo de recurso: <code>cluster</code>|Tipo de recurso: <code>topic</code> |No aplicable     |
| Permitir que una app produzca en un tema específico  |Tipo de recurso: <code>cluster</code>|Tipo de recurso: <code>topic</code><br/>ID de recurso: <var class="keyword varname">nombre_tema</var>      |No aplicable     |
| Permitir que una app se conecte y consuma desde cualquier tema (no grupo de consumidores)  |Tipo de recurso: <code>cluster</code> <br/>Tipo de recurso: <code>topic</code> |Tipo de recurso: <code>topic</code>|No aplicable     |
| Permitir que una app se conecte y consuma desde un tema específico (no grupo de consumidores)  | Tipo de recurso: <code>cluster</code> <br/>Tipo de recurso: <code>topic</code><br/>ID de recurso: <var class="keyword varname">nombre_tema</var> |No aplicable     |No aplicable     |
| Permitir a una app consuma un tema (grupo de consumidores)  |Tipo de recurso: <code>cluster</code> <br/>Tipo de recurso: <code>topic</code><br/> Tipo de recurso: <code>group</code> |No aplicable      |No aplicable     |
| Permitir que una app produzca un tema de forma transaccional  |Tipo de recurso: <code>cluster</code> <br/> Tipo de recurso: <code>group</code>|Tipo de recurso: <code>topic</code> <br/>ID de recurso: <var class="keyword varname">nombre_tema</var> <br/>Tipo de recurso: <code>txnid</code> |No aplicable     |
| Suprimir grupo de consumidores |Tipo de recurso: <code>cluster</code> |No aplicable  |Tipo de recurso: <code>group</code> <br/>ID de recurso: <var class="keyword varname">ID_grupo</var>      |
| Utilizar Streams |Tipo de recurso: <code>cluster</code></br>Tipo de recurso: <code>group</code>| No aplicable  |Tipo de recurso: <code>topic</code>    |

Para obtener más información sobre IAM, utilice [IBM Cloud Identity and Access Management](/docs/iam/index.html#iamoverview).

Para ver un ejemplo de cómo definir políticas, consulte: [Claves de API e ID de servicio IBM Cloud IAM ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/blogs/bluemix/2017/10/introducing-ibm-cloud-iam-service-ids-api-keys/){:new_window}.


## Conexión a {{site.data.keyword.messagehub}}
{: #connect_message_enterprise }

Para obtener información sobre cómo enlazar una aplicación de Cloud Foundry o cómo obtener una credencial de clave de seguridad para una aplicación externa, consulte
[Conexión a {{site.data.keyword.messagehub}}](/docs/services/EventStreams/eventstreams127.html#connect_messagehub).

<!-- 28/06/18 - Karen: draft info only

## Examples
{: #security_examples }

I want to give a user access to create or delete a topic:

1. From the IBM Cloud dashboard, go to the **Manage** tab &gt; **Security** &gt; **Identity and Access**, and then select **Users**.
2. Click **Invite users**.
3. Specify the email address of the user that you want to invite.
4. In the **Access** section, expand the **Services** option.
5. Choose to assign access to a **Resource**.
6. In the **Services** section, select **{{site.data.keyword.messagehub}}**
7. In the **Region** section, make your selection.
8. In the **Service instance** section, locate your instance and select it.
9. In the **Resource type** section, enter **cluster**.
10. In the **Select roles** section, check the **Reader** box.
11. In the **Resource type** section, enter **topic**.
12. In the **Select roles** section, check the **Manager** box.
13. Click **Invite users**.

-->















