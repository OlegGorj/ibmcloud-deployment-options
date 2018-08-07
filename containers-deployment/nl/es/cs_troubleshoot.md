---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-24"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}



# Depuración de clústeres
{: #cs_troubleshoot}

A medida que utiliza {{site.data.keyword.containerlong}}, considere estas técnicas para la resolución de problemas generales y la depuración de los clústeres. También puede consultar el [estado del sistema {{site.data.keyword.Bluemix_notm}}![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.ibm.com/bluemix/support/#status).
{: shortdesc}

Puede seguir estos pasos generales para asegurarse de que los clústeres están actualizados:
- Comprobar mensualmente la disponibilidad de parches de seguridad y del sistema operativo para [actualizar los nodos trabajadores](cs_cli_reference.html#cs_worker_update).
- [Actualizar el clúster ](cs_cli_reference.html#cs_cluster_update) a la última [versión predeterminada de Kubernetes](cs_versions.html) para {{site.data.keyword.containershort_notm}}

## Depuración de clústeres
{: #debug_clusters}

Revise las opciones para depurar sus clústeres y encontrar las causas raíz de los errores.

1.  Obtenga una lista con su clúster y busque el `Estado` del clúster.

  ```
  bx cs clusters
  ```
  {: pre}

2.  Revise el `Estado` de su clúster. Si el clúster está en estado **Crítico**, **Error al suprimir** o **Aviso**, o se ha bloqueado en el estado **Pendiente** durante mucho tiempo, inicie la [depuración de los nodos trabajadores](#debug_worker_nodes).

    <table summary="Cada fila de la tabla se debe leer de izquierda a derecha, con el estado del clúster en la columna uno y una descripción en la columna dos.">
<caption>Estados de clúster</caption>
   <thead>
   <th>Estado del clúster</th>
   <th>Descripción</th>
   </thead>
   <tbody>
<tr>
   <td>Terminado anormalmente</td>
   <td>El usuario ha solicitado la supresión del clúster antes de desplegar el maestro de Kubernetes. Una vez realizada la supresión del clúster, el clúster se elimina del panel de control. Si el clúster está bloqueado en este estado durante mucho tiempo, abra una [incidencia de soporte de {{site.data.keyword.Bluemix_notm}}](cs_troubleshoot.html#ts_getting_help).</td>
   </tr>
 <tr>
     <td>Crítico</td>
     <td>No se puede acceder al maestro de Kubernetes o todos los nodos trabajadores del clúster están inactivos. </td>
    </tr>
   <tr>
     <td>Error al suprimir</td>
     <td>No se puede suprimir el maestro de Kubernetes o al menos un nodo trabajador.  </td>
   </tr>
   <tr>
     <td>Suprimido</td>
     <td>El clúster se ha suprimido pero todavía no se ha eliminado del panel de control. Si el clúster está bloqueado en este estado durante mucho tiempo, abra una [incidencia de soporte de {{site.data.keyword.Bluemix_notm}}](cs_troubleshoot.html#ts_getting_help). </td>
   </tr>
   <tr>
   <td>Suprimiendo</td>
   <td>El clúster se está suprimiendo y la infraestructura del clúster se está desmontando. No puede acceder al clúster.  </td>
   </tr>
   <tr>
     <td>Error al desplegar</td>
     <td>El despliegue del maestro de Kubernetes no se ha podido realizar. No puede resolver este estado. Póngase en contacto con el soporte de IBM Cloud abriendo una [incidencia de soporte de {{site.data.keyword.Bluemix_notm}}](cs_troubleshoot.html#ts_getting_help).</td>
   </tr>
     <tr>
       <td>Despliegue</td>
       <td>El maestro de Kubernetes no está completamente desplegado. No puede acceder a su clúster. Espere hasta que el clúster se haya desplegado por completo para revisar el estado del clúster.</td>
      </tr>
      <tr>
       <td>Normal</td>
       <td>Todos los nodos trabajadores de un clúster están activos y en ejecución. Puede acceder al clúster y desplegar apps en el clúster. Este estado se considera correcto y no requiere ninguna acción por su parte. **Nota**: Aunque los nodos trabajadores podrían poseer un estado normal, otros recursos de infraestructura como, por ejemplo la [red](cs_troubleshoot_network.html) y el [almacenamiento](cs_troubleshoot_storage.html), podrían requerir su atención.</td>
    </tr>
      <tr>
       <td>Pendiente</td>
       <td>El maestro de Kubernetes está desplegado. Los nodos trabajadores se están suministrando y aún no están disponibles en el clúster. Puede acceder al clúster, pero no puede desplegar apps en el clúster.  </td>
     </tr>
   <tr>
     <td>Solicitado</td>
     <td>Se ha enviado una solicitud para crear el clúster y pedir la infraestructura para el maestro de Kubernetes y los nodos trabajadores. Cuando se inicia el despliegue del clúster, el estado del clúster cambia a <code>Desplegando</code>. Si el clúster está bloqueado en el estado <code>Solicitado</code> durante mucho tiempo, abra una [incidencia de soporte de {{site.data.keyword.Bluemix_notm}}](cs_troubleshoot.html#ts_getting_help). </td>
   </tr>
   <tr>
     <td>Actualizando</td>
     <td>El servidor de API de Kubernetes que se ejecuta en el maestro de Kubernetes está siendo actualizado a una versión nueva de API de Kubernetes. Durante la actualización, no puede acceder ni cambiar el clúster. Los nodos trabajadores, apps y recursos que el usuario despliega no se modifican y continúan en ejecución. Espere a que la actualización se complete para revisar el estado del clúster. </td>
   </tr>
    <tr>
       <td>Aviso</td>
       <td>Al menos un nodo trabajador del clúster no está disponible, pero los otros nodos trabajadores están disponibles y pueden asumir la carga de trabajo. </td>
    </tr>
   </tbody>
 </table>


<br />


## Depuración de nodos trabajadores
{: #debug_worker_nodes}

Revise las opciones para depurar sus nodos trabajadores y encontrar las causas raíz de los errores.


1.  Si el clúster está en estado **Crítico**, **Error al suprimir** o **Aviso**, o se ha bloqueado en el estado **Pendiente** durante mucho tiempo, revise el estado de los nodos trabajadores.

  ```
  bx cs workers <cluster_name_or_id>
  ```
  {: pre}

2.  Revise los campos `State` y `Status` de cada nodo trabajador en la salida de la CLI.

  <table summary="Cada fila de la tabla se debe leer de izquierda a derecha, con el estado del clúster en la columna uno y una descripción en la columna dos.">
  <caption>Estados de nodo trabajador</caption>
    <thead>
    <th>Estado del nodo trabajador</th>
    <th>Descripción</th>
    </thead>
    <tbody>
  <tr>
      <td>Crítico</td>
      <td>Un nodo trabajador puede entrar en estado Crítico por muchas razones: <ul><li>Rearrancar el nodo trabajador sin acordonarlo ni drenarlo. Rearrancar un nodo trabajador puede provocar que se corrompan los datos en <code>docker</code>, <code>kubelet</code>, <code>kube-proxy</code> y <code>calico</code>. </li><li>Los pods que se despliegan en el nodo trabajador no utilizan límites de recursos para [memoria ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/configure-pod-container/assign-memory-resource/) y [CPU ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/configure-pod-container/assign-cpu-resource/). Sin límites de recursos, los pods pueden consumir todos los recursos disponibles y no dejar recursos para que se ejecuten otros pods en este nodo trabajador. Esta sobrecarga de trabajo provoca que el nodo trabajador falle. </li><li><code>Docker</code>, <code>kubelet</code> o <code>calico</code> entraron en un estado irrecuperable después de ejecutar cientos o miles de contenedores a lo largo del tiempo. </li><li>Tener configurado un Virtual Router Appliance para el nodo trabajador que se quede inactivo y se interrumpa así la comunicación entre el nodo trabajador y el maestro de Kubernetes. </li><li> Problemas de red en {{site.data.keyword.containershort_notm}} o en la infraestructura de IBM Cloud (SoftLayer) que provoquen que falle la comunicación entre el nodo trabajador y el maestro de Kubernetes.</li><li>Que el nodo trabajador agote su capacidad. Compruebe el campo <strong>Status</strong> del nodo trabajador para ver si es <strong>Out of disk</strong> u <strong>Out of memory</strong>. Si el nodo trabajador se ha quedado sin capacidad, considere reducir la carga de trabajo del nodo trabajador o añadir un nodo trabajador al clúster para ayudar a equilibrar la carga de trabajo.</li></ul> En muchos casos, el problema se resuelve al [recargar](cs_cli_reference.html#cs_worker_reload) el nodo trabajador. Cuando recarga su nodo trabajador, se aplica la última [versión de parche](cs_versions.html#version_types) al nodo trabajador. La versión mayor y la versión menor no cambian. Antes de recargar el nodo trabajador, acordone y drene el nodo trabajador para que los pods existentes finalicen correctamente y se vuelvan a programar en los demás nodos trabajadores. </br></br> Si la recarga del nodo trabajador no resuelve el problema, vaya al siguiente paso para continuar la resolución de problemas del nodo trabajador. </br></br><strong>Sugerencia:</strong> Puede [configurar comprobaciones de estado del nodo trabajador y habilitar la recuperación automática](cs_health.html#autorecovery). Si la recuperación automática detecta un nodo trabajador erróneo basado en las comprobaciones configuradas, desencadena una acción correctiva, como una recarga del sistema operativo, en el nodo trabajador. Para obtener más información sobre cómo funciona la recuperación automática, consulte el [blog sobre recuperación automática ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/blogs/bluemix/2017/12/autorecovery-utilizes-consistent-hashing-high-availability/).
      </td>
     </tr>
      <tr>
        <td>Despliegue</td>
        <td>Cuando se actualiza la versión de Kubernetes del nodo trabajador, el nodo trabajador se vuelve a desplegar para instalar las actualizaciones. Si el nodo trabajador queda bloqueado en este estado durante mucho tiempo, continúe en el paso siguiente para ver si se ha producido un problema durante la recarga. </td>
     </tr>
        <tr>
        <td>Normal</td>
        <td>El nodo trabajador se ha suministrado por completo y está listo para ser utilizado en el clúster. Este estado se considera correcto y no requiere ninguna acción por parte del usuario. **Nota**: Aunque los nodos trabajadores podrían poseer un estado normal, otros recursos de infraestructura como, por ejemplo la [red](cs_troubleshoot_network.html) y el [almacenamiento](cs_troubleshoot_storage.html), podrían requerir su atención.</td>
     </tr>
   <tr>
        <td>Suministro</td>
        <td>El nodo trabajador se está suministrando y aún no está disponible en el clúster. Puede supervisar el proceso de suministro en la columna <strong>Estado</strong> de la salida de la CLI. Si el nodo trabajador queda bloqueado en este estado durante mucho tiempo, continúe en el paso siguiente para ver si se ha producido un problema durante el suministro.</td>
      </tr>
      <tr>
        <td>Provision_failed</td>
        <td>El nodo trabajador no se ha podido suministrar. Continúe en el paso siguiente para ver los detalles del error.</td>
      </tr>
   <tr>
        <td>Recarga</td>
        <td>El nodo trabajador se está recargando y no está disponible en el clúster. Puede supervisar el proceso de recarga en la columna <strong>Estado</strong> de la salida de la CLI. Si el nodo trabajador queda bloqueado en este estado durante mucho tiempo, continúe en el paso siguiente para ver si se ha producido un problema durante la recarga.</td>
       </tr>
       <tr>
        <td>Reloading_failed </td>
        <td>El nodo trabajador no se ha podido recargar. Continúe en el paso siguiente para ver los detalles del error.</td>
      </tr>
      <tr>
        <td>Reload_pending </td>
        <td>Se ha enviado una solicitud para recargar o actualizar la versión de Kubernetes del nodo trabajador. Cuando el nodo trabajador se está recargando, el estado cambia a <code>Recarga</code>. </td>
      </tr>
      <tr>
       <td>Desconocido</td>
       <td>No se puede acceder al maestro de Kubernetes por uno de estos motivos:<ul><li>Ha solicitado una actualización del maestro de Kubernetes. El estado del nodo trabajador no se puede recuperar durante la actualización.</li><li>Es posible que tenga otro cortafuegos que está protegiendo sus nodos trabajadores o que haya modificado los valores del cortafuegos recientemente. {{site.data.keyword.containershort_notm}} requiere que determinadas direcciones IP y puertos estén abiertos para permitir la comunicación entre el nodo trabajador y el maestro de Kubernetes y viceversa. Para obtener más información, consulte [El cortafuegos impide que los nodos trabajadores se conecten](cs_troubleshoot_clusters.html#cs_firewall).</li><li>El maestro de Kubernetes está inactivo. Para ponerse en contacto con el equipo de soporte de {{site.data.keyword.Bluemix_notm}}, abra una [incidencia de soporte de {{site.data.keyword.Bluemix_notm}}](#ts_getting_help).</li></ul></td>
  </tr>
     <tr>
        <td>Aviso</td>
        <td>El nodo trabajador está a punto de alcanzar el límite de memoria o de espacio de disco. Puede reducir la carga de trabajo del nodo trabajador o añadir un nodo trabajador al clúster para ayudar a equilibrar la carga de trabajo.</td>
  </tr>
    </tbody>
  </table>

5.  Obtener los detalles del nodo trabajador. Si en los detalles se incluye un mensaje de error, revise la lista de [mensajes de error comunes para nodos trabajadores](#common_worker_nodes_issues) para saber cómo resolver el problema.

   ```
   bx cs worker-get <worker_id>
   ```
   {: pre}

  ```
  bx cs worker-get [<cluster_name_or_id>] <worker_node_id>
  ```
  {: pre}

<br />


## Problemas comunes con los nodos trabajadores
{: #common_worker_nodes_issues}

Revise los mensajes de error comunes y busque cómo resolverlos.

  <table>
  <caption>Mensajes de error comunes</caption>
    <thead>
    <th>Mensaje de error</th>
    <th>Descripción y resolución
    </thead>
    <tbody>
      <tr>
        <td>Excepción de infraestructura de {{site.data.keyword.Bluemix_notm}}: En este momento su cuenta no tiene permitido solicitar 'Instancias de cálculo'.</td>
        <td>Puede que su cuenta de infraestructura de IBM Cloud (SoftLayer) tenga restringida la solicitud de recursos de cálculo. Para ponerse en contacto con el equipo de soporte de {{site.data.keyword.Bluemix_notm}}, abra una [incidencia de soporte de {{site.data.keyword.Bluemix_notm}}](#ts_getting_help).</td>
      </tr>
      <tr>
        <td>Excepción de infraestructura de {{site.data.keyword.Bluemix_notm}}: No se ha podido realizar el pedido. No hay suficientes recursos tras el direccionador 'router_name' para realizar la solicitud para los siguientes invitados: 'worker_id'.</td>
        <td>La VLAN que ha seleccionado está asociada a un pod del centro de datos que no tiene suficiente espacio para suministrar el nodo trabajador. Puede elegir entre las opciones siguientes:<ul><li>Utilice otro centro de datos para suministrar el nodo trabajador. Ejecute <code>bx cs locations</code> para ver una lista de los centros de datos disponibles.<li>Si tiene un par existente de VLAN pública y privada asociado a otro pod del centro de datos, utilice este par de VLAN.<li>Para ponerse en contacto con el equipo de soporte de {{site.data.keyword.Bluemix_notm}}, abra una [incidencia de soporte de {{site.data.keyword.Bluemix_notm}}](#ts_getting_help).</ul></td>
      </tr>
      <tr>
        <td>Excepción de infraestructura de {{site.data.keyword.Bluemix_notm}}: No se ha podido obtener la VLAN de red con el ID: &lt;vlan id&gt;.</td>
        <td>El nodo trabajador no se ha podido suministrar porque no se ha encontrado el ID de VLAN por una de las siguientes razones:<ul><li>Puede que haya especificado el número de VLAN en lugar del ID de VLAN. El número de VLAN tiene 3 ó 4 dígitos, mientras que el ID de VLAN tiene 7 dígitos. Ejecute <code>bx cs vlans &lt;location&gt;</code> para recuperar el ID de VLAN.<li>Es posible que el ID de VLAN no esté asociado a la cuenta de infraestructura de IBM Cloud (SoftLayer) que está utilizando. Ejecute <code>bx cs vlans &lt;location&gt;</code> ver una lista de los ID de VLAN disponibles para su cuenta. Para cambiar la cuenta de infraestructura de IBM Cloud (SoftLayer), consulte [`bx cs credentials-set`](cs_cli_reference.html#cs_credentials_set). </ul></td>
      </tr>
      <tr>
        <td>SoftLayer_Exception_Order_InvalidLocation: La ubicación suministrada para este pedido no es válida. (HTTP 500)</td>
        <td>Su cuenta de infraestructura de IBM Cloud (SoftLayer) no está configurada para solicitar recursos de cálculo en el centro de datos seleccionado. Póngase en contacto con el [equipo de soporte de {{site.data.keyword.Bluemix_notm}}](#ts_getting_help) para comprobar que la cuenta está correctamente configurada.</td>
       </tr>
       <tr>
        <td>Excepción de infraestructura de {{site.data.keyword.Bluemix_notm}}: El usuario no tiene los permisos necesarios de la infraestructura de {{site.data.keyword.Bluemix_notm}} para añadir servidores
        </br></br>
        Excepción de infraestructura de {{site.data.keyword.Bluemix_notm}}: El elemento 'Item' se debe solicitar con permiso.</td>
        <td>Es posible que no tenga los permisos necesarios para suministrar un nodo trabajador desde el portafolio de infraestructura de IBM Cloud (SoftLayer). Consulte [Configuración del acceso al portafolio de infraestructura de IBM Cloud (SoftLayer) para crear clústeres de Kubernetes estándares](cs_troubleshoot_clusters.html#cs_credentials).</td>
      </tr>
      <tr>
       <td>El trabajador no se puede comunicar con los servidores de {{site.data.keyword.containershort_notm}}. Verifique que la configuración del cortafuegos permite el tráfico de este trabajador.
       <td><ul><li>Si tiene un cortafuegos, [configure los valores de cortafuegos para permitir el tráfico saliente a los puertos y direcciones IP adecuados](cs_firewall.html#firewall_outbound).</li><li>Compruebe si el clúster no tiene una IP pública ejecutando `bx cs workers <mycluster>`. Si no se lista una IP pública, entonces su clúster solo tiene VLAN privadas.<ul><li>Si desea que el clúster sólo tenga VLAN privadas, configure la [conexión de VLAN](cs_clusters.html#worker_vlan_connection) y el [cortafuegos](cs_firewall.html#firewall_outbound).</li><li>Si desea que el clúster tenga una IP pública, [añada nuevos nodos trabajadores](cs_cli_reference.html#cs_worker_add) con VLAN públicas y privadas.</li></ul></li></ul></td>
     </tr>
      <tr>
  <td>No se puede crear la señal de portal de IMS, ya que ninguna cuenta de IMS está enlazada con la cuenta de BSS seleccionada</br></br>El usuario proporcionado no se ha encontrado o está activo</br></br>SoftLayer_Exception_User_Customer_InvalidUserStatus: La cuenta del usuario está actualmente en estado cancel_pending.</br></br>Esperando que la máquina se haga visible para el usuario</td>
  <td>El propietario de la clave de API que se utiliza para acceder al portafolio de infraestructura de IBM Cloud (SoftLayer) no tiene los permisos necesarios para realizar la acción o puede estar pendiente de supresión.</br></br><strong>Como usuario</strong>, siga estos pasos: <ol><li>Si tiene acceso a varias cuentas, asegúrese de que ha iniciado la sesión en la cuenta en la que desea trabajar con {{site.data.keyword.containerlong_notm}}. </li><li>Ejecute <code>bx cs api-key-info</code> para ver el propietario de la clave de API actual que se utiliza para acceder al portafolio de infraestructura de IBM Cloud (SoftLayer) próximo. </li><li>Ejecute <code>bx account list</code> para ver el propietario de la cuenta de {{site.data.keyword.Bluemix_notm}} que utiliza actualmente. </li><li>Póngase en contacto con el propietario de la cuenta de {{site.data.keyword.Bluemix_notm}} e infórmele de que el propietario de la clave de API tiene permisos insuficientes en la infraestructura de IBM Cloud (SoftLayer) o puede estar pendiente de supresión. </li></ol></br><strong>Como propietario de cuenta</strong>, siga estos pasos: <ol><li>Revise los [permisos necesarios en la infraestructura de IBM Cloud (SoftLayer)](cs_users.html#infra_access) para realizar la acción que ha fallado anteriormente. </li><li>Corrija los permisos del propietario de la clave de API o cree una nueva clave de API mediante [el mandato <code>bx cs api-key-reset</code>](cs_cli_reference.html#cs_api_key_reset). </li><li>Si usted u otro administrador de la cuenta define manualmente las credenciales de la infraestructura de IBM Cloud (SoftLayer) en la cuenta, ejecute [<code>bx cs credentials-unset</code>](cs_cli_reference.html#cs_credentials_unset) para eliminar las credenciales de la cuenta.</li></ol></td>
  </tr>
    </tbody>
  </table>



<br />




## Depuración de despliegues de app
{: #debug_apps}

Revise las opciones que tiene para depurar sus despliegues de app y busque las causas raíz de los errores.

1. Busque anomalías en el servicio o en los recursos de despliegue mediante el mandato `describe`.

 Ejemplo:
 <pre class="pre"><code>kubectl describe service &lt;service_name&gt; </code></pre>

2. [Compruebe si los contenedores están bloqueados en el estado ContainerCreating](cs_troubleshoot_storage.html#stuck_creating_state).

3. Compruebe si el clúster está en el estado `Critical`. Si el clúster está en el estado `Critical`, compruebe las reglas de cortafuegos y verifique que el maestro se puede comunicar con los nodos trabajadores.

4. Verifique que el servicio está a la escucha en el puerto correcto.
   1. Obtenga el nombre de un pod.
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. Inicie una sesión en un contenedor.
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   3. Ejecute curl sobre la app desde dentro del contenedor. Si no se puede acceder al puerto, es posible que el servicio no esté a la escucha en el puerto correcto o que la app tenga problemas. Actualice el archivo de configuración para el servicio con el puerto correcto y vuelva a desplegar o investigue posibles problemas con la app.
     <pre class="pre"><code>curl localhost: &lt;port&gt;</code></pre>

5. Verifique que el servicio está enlazado correctamente a los pods.
   1. Obtenga el nombre de un pod.
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. Inicie una sesión en un contenedor.
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   3. Ejecute curl sobre la dirección IP del clúster y el puerto del servicio. Si no se puede acceder a la dirección IP y al puerto, mire los puntos finales correspondientes al servicio. Si no se listan puntos finales, el selector del servicio no coincide con los pods. Si se listan puntos finales, mire el campo del puerto de destino en el servicio y asegúrese de que el puerto de destino es el mismo que el que se utiliza para los pods.
     <pre class="pre"><code>curl &lt;cluster_IP&gt;:&lt;port&gt;</code></pre>

6. Para servicios Ingress, verifique que se puede acceder al servicio desde dentro del clúster.
   1. Obtenga el nombre de un pod.
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. Inicie una sesión en un contenedor.
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   2. Ejecute curl sobre el URL especificado para el servicio Ingress. Si no se puede acceder al URL, compruebe si hay un problema de cortafuegos entre el clúster y el punto final externo. 
     <pre class="pre"><code>curl &lt;host_name&gt;.&lt;domain&gt;</code></pre>

<br />



## Obtención de ayuda y soporte
{: #ts_getting_help}

¿Sigue teniendo problemas con su clúster?
{: shortdesc}

-   Para ver si {{site.data.keyword.Bluemix_notm}} está disponible, [consulte la página de estado de {{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.ibm.com/bluemix/support/#status).
-   Publique una pregunta en [{{site.data.keyword.containershort_notm}}Slack ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://ibm-container-service.slack.com).

    Si no utiliza un ID de IBM para la cuenta de {{site.data.keyword.Bluemix_notm}}, [solicite una invitación](https://bxcs-slack-invite.mybluemix.net/) a este Slack.
    {: tip}
-   Revise los foros para ver si otros usuarios se han encontrado con el mismo problema. Cuando utiliza los foros para formular una pregunta, etiquete la pregunta para que la puedan ver los equipos de desarrollo de {{site.data.keyword.Bluemix_notm}}.

    -   Si tiene preguntas técnicas sobre el desarrollo o despliegue de clústeres o apps con {{site.data.keyword.containershort_notm}}, publique su pregunta en [Stack Overflow ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) y etiquete su pregunta con `ibm-cloud`, `kubernetes` y `containers`.
    -   Para las preguntas relativas a las instrucciones de inicio y el servicio, utilice el foro [IBM developerWorks dW Answers ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix). Incluya las etiquetas `ibm-cloud` y `containers`.
    Consulte [Obtención de ayuda](/docs/get-support/howtogetsupport.html#using-avatar) para obtener más detalles sobre cómo utilizar los foros.

-   Póngase en contacto con el soporte de IBM abriendo una incidencia. Para obtener información sobre cómo abrir una incidencia de soporte de IBM, o sobre los niveles de soporte y las gravedades de las incidencias, consulte [Cómo contactar con el servicio de soporte](/docs/get-support/howtogetsupport.html#getting-customer-support).

{: tip}
Al informar de un problema, incluya el ID de clúster. Para obtener el ID de clúster, ejecute `bx cs clusters`.

