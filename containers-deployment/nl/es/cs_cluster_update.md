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





# Actualización de clústeres y nodos trabajadores
{: #update}

Puede instalar actualizaciones para mantener actualizados los clústeres de Kubernetes en {{site.data.keyword.containerlong}}.
{:shortdesc}

## Actualización del maestro de Kubernetes
{: #master}

Periódicamente, Kubernetes publica [actualizaciones de parche, mayores o menores](cs_versions.html#version_types). En función del tipo de actualización, podría ser responsable de actualizar los componentes del maestro de Kubernetes.
{:shortdesc}

Las actualizaciones pueden afectar a la versión del servidor de API de Kubernetes o a otros componentes del maestro de Kubernetes.  Usted siempre es el responsable de mantener actualizados los nodos trabajadores. Cuando se realizan actualizaciones, el maestro de Kubernetes se actualiza antes que los nodos trabajadores.

De forma predeterminada, la posibilidad de actualizar el servidor de API de Kubernetes está limitada en el maestro de Kubernetes a dos versiones menores posteriores a su versión actual. Por ejemplo, si el servidor de API de Kubernetes actual es de la versión 1.7 y desea actualizar a 1.10, primero se debe actualizar a la versión 1.8 o 1.9. Puede forzar que esto se produzca, sin embargo, la actualización de tres o más versiones menores podría dar lugar a resultados imprevistos. Si el clúster está ejecutando una versión no soportada de Kubernetes, es posible que tenga que forzar la actualización.

El diagrama siguiente muestra el proceso que puede realizar para actualizar el maestro.

![Práctica recomendada para actualizar el maestro](/images/update-tree.png)

Figura 1. Diagrama del proceso de actualización del maestro de Kubernetes

**Atención**: No puede retrotraer un clúster a una versión anterior después de realizar el proceso de actualización. Asegúrese de utilizar un clúster de prueba y siga las instrucciones para abordar posibles problemas antes de actualizar el maestro de producción.

Para las actualizaciones _mayores_ o _menores_, siga estos pasos:

1. Revise los [cambios de Kubernetes](cs_versions.html) y realice las actualizaciones marcadas como _Actualizar antes que maestro_.
2. Actualice el servidor de API de Kubernetes y los componentes asociados del maestro de Kubernetes mediante la GUI o mediante la ejecución del [mandato de CLI](cs_cli_reference.html#cs_cluster_update). Cuando actualice el servidor de API de Kubernetes, el servidor de API estará inactivo durante unos 5 o 10 minutos. Durante la actualización, no puede acceder ni cambiar el clúster. Sin embargo, los nodos trabajadores, las apps y los recursos que los usuarios del clúster han desplegado no se modifican y continúan ejecutándose.
3. Confirme que la actualización se ha completado. Revise la versión del servidor de API de Kubernetes en el panel de control de {{site.data.keyword.Bluemix_notm}} o ejecutando `cs bx clusters`.
4. Instale la versión de [`kubectl cli`](cs_cli_install.html#kubectl) que coincida con la versión del servidor de API de Kubernetes API que se ejecuta en el maestro de Kubernetes.

Cuando finalice la actualización del servidor de API de Kubernetes, puede actualizar los nodos trabajadores.

<br />


## Actualización de nodos trabajadores
{: #worker_node}


Ha recibido una notificación para actualizar los nodos trabajadores. ¿Qué significa esto? Como las actualizaciones y parches de seguridad se han aplicado para el servidor de API de Kubernetes y otros componentes del maestro de Kubernetes, debe asegurarse de que los nodos trabajadores permanecen sincronizados.
{: shortdesc}

La versión de Kubernetes del nodo trabajador no puede ser superior a la a la versión del servidor de API de Kubernetes que se ejecuta en el maestro de Kubernetes. Antes de empezar, [actualice el maestro de Kubernetes](#master).

**Atención:**:
<ul><li>Las actualizaciones de los nodos trabajadores pueden provocar que las apps y servicios estén un tiempo inactivos.</li>
<li>Los datos se suprimen si no están almacenados fuera del pod.</li>
<li>Utilice [réplicas ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#replicas) si los despliegues permiten volver a planificar pods en nodos disponibles.</li></ul>

¿Qué ocurre si no puedo tener un tiempo de inactividad?

Como parte del proceso de actualización, algunos nodos específicos estarán inactivos durante un periodo de tiempo. Para evitar los tiempos de inactividad para su aplicación, puede definir claves exclusivas en un mapa de configuración que especifiquen porcentajes de umbral para determinados tipos de nodo durante el proceso de actualización. Definiendo reglas basadas en etiquetas estándares de Kubernetes y dando un porcentaje de la cantidad máxima de nodos que pueden estar disponibles, puede asegurarse de que su app siga activa y en ejecución. Se considera que un nodo está disponible si todavía tiene que completar el proceso de despliegue.

¿Cómo se definen las claves?

En la sección de información de datos del mapa de configuración, puede definir hasta 10 reglas separadas para ejecutarse en cualquier momento. Para ser actualizado, los nodos trabajadores deben pasar todas las reglas definidas.

Las claves están definidas. ¿Qué hago?

Después de definir las reglas, ejecute el mandato `bx cs worker-update`. Si se devuelve una respuesta satisfactoria, los nodos trabajadores se pondrán en cola para ser actualizados. Sin embargo, los nodos no completan el proceso de actualización hasta que se satisfacen todas las reglas. Mientras están en cola, las reglas se comprueban a intervalos para ver si alguno de los nodos puede ser actualizado.

¿Qué ocurre si opto por no definir ningún mapa de configuración?

Si el mapa de configuración no está definido, se utiliza el valor predeterminado. De forma predeterminada, como máximo el 20% de todos los nodos trabajadores de cada clúster están disponibles durante el proceso de actualización.

Para actualizar los nodos trabajadores:

1. Aplique los cambios que se marcan como _Actualizar después de maestro_ en [Cambios de Kubernetes](cs_versions.html).

2. Opcional: Defina el mapa de configuración.
    Ejemplo:

    ```
    apiVersion: v1
    kind: ConfigMap
    metadata:
      name: ibm-cluster-update-configuration
      namespace: kube-system
    data:
     drain_timeout_seconds: "120"
     zonecheck.json: |
       {
         "MaxUnavailablePercentage": 70,
         "NodeSelectorKey": "failure-domain.beta.kubernetes.io/zone",
         "NodeSelectorValue": "dal13"
       }
     regioncheck.json: |
       {
         "MaxUnavailablePercentage": 80,
         "NodeSelectorKey": "failure-domain.beta.kubernetes.io/region",
         "NodeSelectorValue": "us-south"
       }
     defaultcheck.json: |
       {
         "MaxUnavailablePercentage": 100
       }
    ```
    {:pre}
  <table summary="La primera fila de la tabla abarca ambas columnas. El resto de las filas se deben leer de izquierda a derecha, con el parámetro en la columna uno y la descripción correspondiente en la columna dos. ">
  <caption>Componentes de ConfigMap</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Descripción de los componentes </th>
    </thead>
    <tbody>
      <tr>
        <td><code>drain_timeout_seconds</code></td>
        <td> Opcional: Tiempo de espera en segundos para el drenaje que se produce durante la actualización del nodo trabajador. El drenaje coloca al nodo en un estado `unschedulable`, lo que impide desplegar nuevos pods en dicho nodo. El drenaje también suprime nodos fuera del nodo. Los valores enteros aceptados van del 1 al 180. El valor predeterminado es 30.</td>
      </tr>
      <tr>
        <td><code>zonecheck.json</code></br><code>regioncheck.json</code></td>
        <td> Ejemplos de claves exclusivas para las que desea establecer reglas. Los nombres de las claves pueden ser los que desee; las configuraciones definidas en la clave analizan la información. Para cada clave que defina, sólo puede establecer un valor para <code>NodeSelectorKey</code> y <code>NodeSelectorValue</code>. Si desea establecer reglas para más de una región o ubicación (centro de datos), cree una nueva entrada de clave. </td>
      </tr>
      <tr>
        <td><code>defaultcheck.json</code></td>
        <td> De forma predeterminada, si el mapa <code>ibm-cluster-update-configuration</code> no está definido de forma válida, sólo el 20% de los clústeres podrán estar no disponibles a la vez. Si se definen una o varias reglas válidas sin un valor predeterminado global, el nuevo valor predeterminado es permitir que el 100% de los trabajadores estén no disponibles simultáneamente. Puede controlarlo creando un porcentaje predeterminado. </td>
      </tr>
      <tr>
        <td><code>MaxUnavailablePercentage</code></td>
        <td> La cantidad máxima de nodos que pueden estar no disponibles para una clave especificada, indicada como porcentaje. Un nodo no está disponible cuando está en proceso de despliegue, recarga o suministro. Los nodos trabajadores en cola están bloqueados para actualizarse si se supera cualquier porcentaje no disponible máximo definido. </td>
      </tr>
      <tr>
        <td><code>NodeSelectorKey</code></td>
        <td> El tipo de etiqueta para el que desea establecer una regla para una clave especificada. Puede establecer reglas para las etiquetas predeterminadas proporcionadas por IBM, así como para las etiquetas que haya creado. </td>
      </tr>
      <tr>
        <td><code>NodeSelectorValue</code></td>
        <td> El subconjunto de nodos en una clave especificada que la regla tiene establecido evaluar. </td>
      </tr>
    </tbody>
  </table>

    **Nota**: Se pueden definir 10 reglas como máximo. Si añade más de 10 claves a un archivo, sólo se analiza un subconjunto de la información.

3. Actualice los nodos trabajadores de la GUI o ejecutando el mandato de CLI.
  * Para actualizar desde el panel de control de {{site.data.keyword.Bluemix_notm}}, vaya a la sección `Nodos trabajadores` del clúster y pulse `Actualizar trabajador`.
  * Para obtener los ID de los nodos trabajadores, ejecute `bx cs workers <cluster_name_or_ID>`. Si selecciona varios nodos trabajadores, los nodos trabajadores se colocan en cola para la evaluación de la actualización. Si se considera que están listos tras la evaluación, se actualizarán según las reglas establecidas en las configuraciones.

    ```
    bx cs worker-update <cluster_name_or_ID> <worker_node1_ID> <worker_node2_ID>
    ```
    {: pre}

4. Opcional: Compruebe los sucesos que desencadena el mapa de configuración y los errores de validación que se produzcan ejecutando el mandato siguiente y observando los **Sucesos**.
    ```
    kubectl describe -n kube-system cm ibm-cluster-update-configuration
    ```
    {: pre}

5. Confirme que la actualización se ha completado:
  * Revise la versión de Kubernetes en el panel de control de {{site.data.keyword.Bluemix_notm}} o ejecutando `cs bx workers <cluster_name_or_ID>`.
  * Revise la versión de Kubernetes de los nodos trabajadores ejecutando `kubectl get nodes`.
  * En algunos casos, es posible que los clústeres antiguos muestren nodos trabajadores duplicados con el estado **NotReady** después de una actualización. Para eliminar los duplicados, consulte [resolución de problemas](cs_troubleshoot_clusters.html#cs_duplicate_nodes).

Pasos siguientes:
  - Repita el proceso de actualización con otros clústeres.
  - Informe a los desarrolladores que trabajan en el clúster para que actualicen su CLI de `kubectl` a la versión del maestro de Kubernetes.
  - Si el panel de control de Kubernetes no muestra los gráficos de utilización, [suprima el pod `kube-dashboard`](cs_troubleshoot_health.html#cs_dashboard_graphs).
  





<br />



## Actualización de los tipos de máquina
{: #machine_type}

Puede actualizar los tipos de máquina que utilizan los nodos trabajadores añadiendo nuevos nodos trabajadores y eliminando los antiguos. Por ejemplo, si tiene nodos trabajadores virtuales en tipos de máquina en desuso con `u1c` o `b1c` en los nombres, cree nodos trabajadores que utilicen tipos de máquina con `u2c` o `b2c` en el nombre.
{: shortdesc}

Antes de empezar:
- Defina su clúster como [destino de la CLI](cs_cli_install.html#cs_cli_configure).
- Si almacena datos en su nodo trabajador, los datos se suprimen si no los [almacena fuera del nodo trabajador](cs_storage.html#storage).


**Atención**: las actualizaciones a los nodos trabajadores pueden hacer que las apps y los servicios estén un tiempo inactivos. Los datos se suprimen si no se [almacenan fuera del pod](cs_storage.html#storage).



1. Anote los nombres y las ubicaciones de los nodos trabajadores que desea actualizar.
    ```
    bx cs workers <cluster_name>
    ```
    {: pre}

2. Consulte los tipos de máquina disponibles.
    ```
    bx cs machine-types <location>
    ```
    {: pre}

3. Añada nodos de trabajador utilizando el mandato [bx cs worker-add](cs_cli_reference.html#cs_worker_add). Especifique un tipo de máquina.

    ```
    bx cs worker-add --cluster <cluster_name> --machine-type <machine_type> --number <number_of_worker_nodes> --private-vlan <private_VLAN_ID> --public-vlan <public_VLAN_ID>
    ```
    {: pre}

4. Verifique que se añaden los nodos trabajadores.

    ```
    bx cs workers <cluster_name>
    ```
    {: pre}

5. Cuando los nodos trabajadores añadidos están en estado `Normal`, puede eliminar los nodos trabajadores obsoletos. **Nota**: Si va a eliminar un tipo de máquina que se factura mensualmente (como las nativas), se le facturará todo el mes.

    ```
    bx cs worker-rm <cluster_name> <worker_node>
    ```
    {: pre}

6. Repita estos pasos para actualizar otros nodos trabajadores a diferentes tipos de máquina.










