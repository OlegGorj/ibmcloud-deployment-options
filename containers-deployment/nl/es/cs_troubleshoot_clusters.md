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



# Resolución de problemas con clústeres y nodos trabajadores
{: #cs_troubleshoot_clusters}

Si utiliza {{site.data.keyword.containerlong}}, tenga en cuenta estas técnicas para resolver problemas relacionados con los clústeres y los nodos trabajadores.
{: shortdesc}

Si tiene un problema más general, pruebe la [depuración del clúster](cs_troubleshoot.html).
{: tip}

## No se puede conectar con la cuenta de infraestructura
{: #cs_credentials}

{: tsSymptoms}
Cuando crea un nuevo clúster de Kubernetes, recibe el siguiente mensaje.

```
No se ha podido conectar con la cuenta de IBM infraestructura de Cloud (SoftLayer).
Para crear un clúster estándar, debe tener una cuenta de Pago según uso enlazada a una cuenta de infraestructura de IBM Cloud (SoftLayer) o debe haber utilizado la CLI de {{site.data.keyword.containerlong}} para configurar las claves de API de la infraestructura de {{site.data.keyword.Bluemix_notm}}.
```
{: screen}

{: tsCauses}
Las cuentas de Pago según uso de {{site.data.keyword.Bluemix_notm}} creadas después de haber enlazado una cuenta automática ya se han configurado con acceso a la infraestructura de IBM Cloud (SoftLayer). Es posible adquirir recursos de la infraestructura para el clúster sin configuración adicional.

Los usuarios con otros tipos de cuenta de {{site.data.keyword.Bluemix_notm}} o los usuarios con cuentas existentes de infraestructura de IBM Cloud (SoftLayer) no enlazadas a su cuenta de {{site.data.keyword.Bluemix_notm}} primero deben configurar sus cuentas para crear clústeres estándares.

{: tsResolve}
La configuración de la cuenta para acceder al portafolio de la infraestructura de IBM Cloud (SoftLayer) depende del tipo de cuenta que tenga. Revise la tabla para encontrar las opciones disponibles para cada tipo de cuenta.

|Tipo de cuenta|Descripción|Opciones disponibles para crear un clúster estándar|
|------------|-----------|----------------------------------------------|
|Cuentas Lite|Las cuentas Lite no pueden suministrar clústeres.|[Actualice su cuenta Lite a una cuenta de {{site.data.keyword.Bluemix_notm}} de Pago según uso](/docs/account/index.html#paygo) que está configurada para acceder al portafolio de infraestructura de IBM Cloud (SoftLayer).|
|Cuenta antiguas de Pago según uso|Las cuentas de Pago según uso que se crearon antes de que estuviese disponible el enlace de cuentas automático, no tenían acceso al portafolio de infraestructura de IBM Cloud (SoftLayer).<p>Si tiene una cuenta existente de infraestructura de IBM Cloud (SoftLayer), no puede enlazarla a una cuenta antigua de Pago según uso.</p>|<strong>Opción 1: </strong> [Crear una nueva cuenta de Pago según uso](/docs/account/index.html#paygo) que esté configurada para acceder al portafolio de infraestructura de IBM Cloud (SoftLayer). Cuando elige esta opción, tendrá dos cuentas y dos facturaciones distintas para {{site.data.keyword.Bluemix_notm}}.<p>Para continuar utilizando su cuenta antigua de Pago según uso, debe utilizar su nueva cuenta de Pago según uso para generar una clave de API para acceder al portafolio de infraestructura de IBM Cloud (SoftLayer). A continuación, debe [configurar la clave de API para su cuenta de Pago según uso de la infraestructura de IBM Cloud (SoftLayer)](cs_cli_reference.html#cs_credentials_set). </p><p><strong>Opción 2:</strong> Si ya tiene una cuenta de infraestructura de IBM Cloud (SoftLayer) existente que desea utilizar, puede [configurar sus credenciales](cs_cli_reference.html#cs_credentials_set) en su cuenta de {{site.data.keyword.Bluemix_notm}}.</p><p>**Nota:** Cuando enlaza de forma manual a una cuenta de infraestructura de IBM Cloud (SoftLayer), las credenciales se utilizan para cada acción específica de la infraestructura de IBM Cloud (SoftLayer) en su cuenta de {{site.data.keyword.Bluemix_notm}}. Debe asegurarse de que la clave de API que ha establecido tiene [suficientes permisos de infraestructura](cs_users.html#infra_access) para que los usuarios pueden crear y trabajar con clústeres.</p>|
|Cuentas de Suscripción|Las cuentas de Suscripción no se configuran con acceso al portafolio de infraestructura de IBM Cloud (SoftLayer).|<strong>Opción 1: </strong> [Crear una nueva cuenta de Pago según uso](/docs/account/index.html#paygo) que esté configurada para acceder al portafolio de infraestructura de IBM Cloud (SoftLayer). Cuando elige esta opción, tendrá dos cuentas y dos facturaciones distintas para {{site.data.keyword.Bluemix_notm}}.<p>Si desea continuar utilizando su cuenta de Suscripción, debe utilizar su nueva cuenta de Pago según uso para generar una clave de API en la infraestructura de IBM Cloud (SoftLayer). A continuación, de forma manual, debe [configurar la clave de la API de la infraestructura de IBM Cloud (SoftLayer) para su cuenta de Suscripción. ](cs_cli_reference.html#cs_credentials_set). Tenga en cuenta que los recursos de infraestructura de IBM Cloud (SoftLayer) se facturarán a través de su nueva cuenta de Pago según uso.</p><p><strong>Opción 2:</strong> Si ya tiene una cuenta de infraestructura de IBM Cloud (SoftLayer) existente que desea utilizar, puede [configurar sus credenciales](cs_cli_reference.html#cs_credentials_set) de forma manual para su cuenta de {{site.data.keyword.Bluemix_notm}}.<p>**Nota:** Cuando enlaza de forma manual a una cuenta de infraestructura de IBM Cloud (SoftLayer), las credenciales se utilizan para cada acción específica de la infraestructura de IBM Cloud (SoftLayer) en su cuenta de {{site.data.keyword.Bluemix_notm}}. Debe asegurarse de que la clave de API que ha establecido tiene [suficientes permisos de infraestructura](cs_users.html#infra_access) para que los usuarios pueden crear y trabajar con clústeres.</p>|
|Cuentas de infraestructura de IBM Cloud (SoftLayer), no cuenta de {{site.data.keyword.Bluemix_notm}}|Para crear un clúster estándar, debe tener una cuenta de {{site.data.keyword.Bluemix_notm}}.|<p>[Crear una cuenta de Pago según uso](/docs/account/index.html#paygo) que esté configurada para acceder al portafolio de infraestructura de IBM Cloud (SoftLayer). Cuando elige esta opción, se le crea en su nombre una cuenta de infraestructura de IBM Cloud (SoftLayer). Tiene dos cuentas y facturaciones separadas de infraestructura de IBM Cloud (SoftLayer).</p>|
{: caption="Opciones de creación de clúster estándar por tipo de cuenta" caption-side="top"}


<br />


## El cortafuegos impide ejecutar mandatos de CLI
{: #ts_firewall_clis}

{: tsSymptoms}
Cuando ejecuta los mandatos `bx`, `kubectl` o `calicoctl` desde la CLI, fallan.

{: tsCauses}
Puede que tenga políticas de red corporativas que impidan el acceso desde el sistema local a los puntos finales públicos mediante proxies o cortafuegos.

{: tsResolve}
[Permita el acceso TCP para que funciones los mandatos de CLI](cs_firewall.html#firewall). Esta tarea precisa de la [política de acceso de administrador](cs_users.html#access_policies). Verifique su [política de acceso](cs_users.html#infra_access) actual.


## El cortafuegos impide que el clúster se conecte a recursos
{: #cs_firewall}

{: tsSymptoms}
Cuando los nodos trabajadores no se pueden conectar, podría ver distintos síntomas. Puede ver uno de los siguientes mensajes cuando el proxy de kubectl falla o cuando intenta acceder a un servicio del clúster y la conexión falla.

  ```
  Conexión rechazada
  ```
  {: screen}

  ```
  La conexión ha excedido el tiempo de espera
  ```
  {: screen}

  ```
  No se puede conectar con el servidor: net/http: tiempo de espera excedido de reconocimiento de TLS
  ```
  {: screen}

Si ejecuta kubectl exec, attach o logs, puede ver el siguiente mensaje:

  ```
  Error from server: error dialing backend: dial tcp XXX.XXX.XXX:10250: getsockopt: connection timed out
  ```
  {: screen}

Si el proxy kubectl se ejecuta correctamente pero el panel de control no está disponible, puede ver el siguiente mensaje.

  ```
  timeout on 172.xxx.xxx.xxx
  ```
  {: screen}



{: tsCauses}
Es posible que tenga otro cortafuegos configurado o que se hayan personalizado los valores existentes del cortafuegos en la cuenta de infraestructura de IBM Cloud (SoftLayer). {{site.data.keyword.containershort_notm}} requiere que determinadas direcciones IP y puertos estén abiertos para permitir la comunicación entre el nodo trabajador y el maestro de Kubernetes y viceversa. Otro motivo puede ser que los nodos trabajadores están bloqueados en un bucle de recarga.

{: tsResolve}
[Permita que el clúster acceda a los recursos de infraestructura y a otros servicios](cs_firewall.html#firewall_outbound). Esta tarea precisa de la [política de acceso de administrador](cs_users.html#access_policies). Verifique su [política de acceso](cs_users.html#infra_access) actual.

<br />



## El acceso al nodo trabajador con SSH falla
{: #cs_ssh_worker}

{: tsSymptoms}
No puede acceder an un nodo trabajador mediante una conexión SSH.

{: tsCauses}
SSH mediante contraseña está inhabilitado en los nodos trabajadores.

{: tsResolve}
Utilice [DaemonSets ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) para lo que tenga que ejecutar en cada nodo o para los trabajos correspondientes a acciones únicas que deba ejecutar.

<br />


## `kubectl exec` y `kubectl logs` no funcionan.
{: #exec_logs_fail}

{: tsSymptoms}
Si ejecuta `kubectl exec` o `kubectl logs`, ve el siguiente mensaje.

  ```
  <workerIP>:10250: getsockopt: connection timed out
  ```
  {: screen}

{: tsCauses}
La conexión OpenVPN entre el nodo maestro y los nodos trabajadores no está funcionando correctamente.

{: tsResolve}
1. Habilite la [distribución de VLAN ](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning) para su cuenta de infraestructura de IBM Cloud (SoftLayer).
2. Reinicie el pod de cliente de OpenVPN.
  ```
  kubectl delete pod -n kube-system -l app=vpn
  ```
  {: pre}
3. Si sigue viendo el mismo mensaje de error, entonces el nodo trabajador en el que está el pod de VPN podría estar en un estado no saludable. Para reiniciar el pod de VPN y volverlo a planificar en un nodo trabajador diferente, [acordone, drene y rearranque el nodo trabajador](cs_cli_reference.html#cs_worker_reboot) en el que está el pod de VPN.

<br />


## Al enlazar un servicio a un clúster se produce un error de nombre
{: #cs_duplicate_services}

{: tsSymptoms}
Cuando ejecuta `bx cs cluster-service-bind <cluster_name> <namespace> <service_instance_name>`, ve el siguiente mensaje.

```
Se han encontrado varios servicios con el mismo nombre.
Ejecute 'bx service list' para ver las instancias de servicio de Bluemix disponibles...
```
{: screen}

{: tsCauses}
Varias instancias de servicio pueden tener el mismo nombre en distintas regiones.

{: tsResolve}
Utilice el GUID de servicio en lugar del nombre de instancia de servicio en el mandato `bx cs cluster-service-bind`.

1. [Inicie sesión en la región que incluye la instancia de servicio que se debe enlazar.](cs_regions.html#bluemix_regions)

2. Obtenga el GUID de la instancia de servicio.
  ```
  bx service show <service_instance_name> --guid
  ```
  {: pre}

  Salida:
  ```
  Invocando 'cf service <service_instance_name> --guid'...
  <service_instance_GUID>
  ```
  {: screen}
3. Vuelva a enlazar el servicio al clúster.
  ```
  bx cs cluster-service-bind <cluster_name> <namespace> <service_instance_GUID>
  ```
  {: pre}

<br />


## Al enlazar un servicio a un clúster se produce un error de no encontrado
{: #cs_not_found_services}

{: tsSymptoms}
Cuando ejecuta `bx cs cluster-service-bind <cluster_name> <namespace> <service_instance_name>`, ve el siguiente mensaje.

```
Binding service to a namespace...
FAILED

The specified IBM Cloud service could not be found. If you just created the service, wait a little and then try to bind it again. To view available IBM Cloud service instances, run 'bx service list'. (E0023)
```
{: screen}

{: tsCauses}
Para enlazar servicios a un clúster, debe tener el rol de usuario desarrollador de Cloud Foundry para el espacio de nombres donde se ha suministrado la instancia de servicio. Además, debe tener acceso de editor de IAM a {{site.data.keyword.containerlong}}. Para acceder a la instancia de servicio, debe haber iniciado una sesión en el espacio en el que se ha suministrado la instancia de servicio. 

{: tsResolve}

**Como usuario: **

1. Inicie una sesión en {{site.data.keyword.Bluemix_notm}}. 
   ```
   bx login
   ```
   {: pre}
   
2. Defina como destino la organización y el espacio donde se haya suministrado la instancia de servicio. 
   ```
   bx target -o <org> -s <space>
   ```
   {: pre}
   
3. Liste las instancias de servicio para verificar que está en el espacio adecuado. 
   ```
   bx service list 
   ```
   {: pre}
   
4. Intente enlazar de nuevo el servicio. Si recibe el mismo error, póngase en contacto con el administrador de la cuenta y verifique que tiene los permisos suficientes para enlazar a los servicios (consulte los siguientes pasos de administración de la cuenta). 

**Como administrador de la cuenta: **

1. Verifique que el usuario que experimenta este problema tiene [permisos de editor para {{site.data.keyword.containerlong}}](/docs/iam/mngiam.html#editing-existing-access). 

2. Verifique que el usuario con este problema tiene el [rol de desarrollador de Cloud Foundry para el espacio](/docs/iam/mngcf.html#updating-cloud-foundry-access) en el que se ha suministrado el servicio. 

3. Si los permisos son los correctos, intente asignar un permiso diferente y, a continuación, vuelva a asignar los permisos necesarios. 

4. Espere unos minutos y, a continuación, deje que el usuario intente enlazar el servicio de nuevo. 

5. Si esto no resuelve el problema, entonces los permisos de IAM no están sincronizados y no podrá resolver el problema usted mismo. [Póngase en contacto con el soporte de IBM](/docs/get-support/howtogetsupport.html#getting-customer-support) abriendo una incidencia de soporte. Asegúrese de proporcionar el ID de clúster, el ID de usuario y el ID de instancia de servicio. 
   1. Recupere el ID de clúster. 
      ```
      bx cs clusters
      ```
      {: pre}
      
   2. Recupere el ID de la instancia de servicio. 
      ```
      bx service show <service_name> --guid
      ```
      {: pre}


<br />



## Después de que un nodo trabajador se actualice o recargue, aparecen pods y nodos duplicados
{: #cs_duplicate_nodes}

{: tsSymptoms}
Cuando ejecuta `kubectl get nodes`, ve nodos trabajadores duplicados con el estado **NotReady**. Los nodos trabajadores con **NotReady** tienen direcciones IP públicas, mientras que los nodos trabajadores con **Ready** tienen direcciones IP privadas.

{: tsCauses}
Clústeres antiguos listaban nodos trabajadores según la dirección IP pública del clúster. Ahora los nodos trabajadores aparecen listados por la dirección IP privada del clúster. Cuando se vuelve a cargar o se actualiza un nodo, la dirección IP se modifica, pero la referencia a la dirección IP pública permanece.

{: tsResolve}
El servicio no se interrumpe debido a estos duplicados, pero puede eliminar las referencias a los nodos trabajadores antiguos del servidor de API.

  ```
  kubectl delete node <node_name1> <node_name2>
  ```
  {: pre}

<br />


## Después de que un nodo trabajador se actualice o recargue, las aplicaciones reciben errores de RBAC DENY.
{: #cs_rbac_deny}

{: tsSymptoms}
Después de actualizar a Kubernetes versión 1.7, las aplicaciones reciben errores `RBAC DENY`.

{: tsCauses}
A partir de [Kubernetes versión 1.7](cs_versions.html#cs_v17), las aplicaciones que se ejecutan en el espacio de nombres `default` ya no poseen privilegios de administrador de clúster para la API de Kubernetes para una mayor seguridad.

Si su app se ejecuta en el espacio de nombres `default`, utiliza el `default ServiceAccount`, y el acceso a la API de Kubernetes, se ve afectado por este cambio en Kubernetes. Para obtener más información, consulte [la documentación de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/admin/authorization/rbac/#upgrading-from-15).

{: tsResolve}
Antes de empezar, seleccione su clúster como [destino de la CLI](cs_cli_install.html#cs_cli_configure).

1.  **Acción temporal**: A medida que actualice las políticas de RBAC de las apps, es posible que desee volver temporalmente al valor `ClusterRoleBinding` anterior para el `default ServiceAccount` en el espacio de nombres `default`.

    1.  Copie el siguiente archivo `.yaml`.

        ```yaml
        kind: ClusterRoleBinding
        apiVersion: rbac.authorization.k8s.io/v1beta1
        metadata:
         name: admin-binding-nonResourceURLSs-default
        subjects:
          - kind: ServiceAccount
            name: default
            namespace: default
        roleRef:
         kind: ClusterRole
         name: admin-role-nonResourceURLSs
         apiGroup: rbac.authorization.k8s.io
        ---
        kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
 name: admin-binding-resourceURLSs-default
subjects:
          - kind: ServiceAccount
      name: default
      namespace: default
  roleRef:
   kind: ClusterRole
   name: admin-role-resourceURLSs
   apiGroup: rbac.authorization.k8s.io
        ```

    2.  Aplique los archivos `.yaml` a su clúster.

        ```
        kubectl apply -f FILENAME
        ```
        {: pre}

2.  [Cree recursos de autorización RBAC ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview) para actualizar el acceso de administrador `ClusterRoleBinding`.

3.  Si ha creado un enlace de rol de clúster temporal, elimínelo.

<br />


## El acceso a un pod en un nodo trabajador nuevo falla con un tiempo de espera excedido
{: #cs_nodes_duplicate_ip}

{: tsSymptoms}
Ha suprimido un nodo trabajador del clúster y luego ha añadido un nodo trabajador. Cuando se despliega un pod o un servicio de Kubernetes, el recurso no puede acceder al nodo trabajador recién creado y la conexión supera el tiempo de espera.

{: tsCauses}
Si suprime un nodo trabajador del clúster y luego añade un nodo trabajador, es posible que el nuevo nodo trabajador se asigne a la dirección IP privada del nodo trabajador suprimido. Calico utiliza esta dirección IP privada como código y sigue intentando acceder al nodo suprimido.

{: tsResolve}
Actualice manualmente la referencia a la dirección IP privada de modo que apunte al nodo correcto.

1.  Confirme que tiene dos nodos trabajadores con la misma dirección **IP privada**. Anote la **IP privada** y el **ID** del nodo trabajador suprimido.

  ```
  bx cs workers <CLUSTER_NAME>
  ```
  {: pre}

  ```
  ID                                                 Public IP       Private IP       Machine Type   State     Status   Location   Version
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w1   169.xx.xxx.xxx  10.xxx.xx.xxx    b2c.4x16       normal    Ready    dal10      1.9.7
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w2   169.xx.xxx.xxx  10.xxx.xx.xxx    b2c.4x16       deleted    -       dal10      1.9.7
  ```
  {: screen}

2.  Instale la [CLI de Calico](cs_network_policy.html#adding_network_policies).
3.  Obtenga una lista de los nodos trabajadores disponibles en Calico. Sustituya <path_to_file> por la vía de acceso local al archivo de configuración de Calico.

  ```
  calicoctl get nodes --config=filepath/calicoctl.cfg
  ```
  {: pre}

  ```
  NAME
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w1
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w2
  ```
  {: screen}

4.  Suprima el nodo trabajador duplicado en Calico. Sustituya NODE_ID por el ID del nodo trabajador.

  ```
  calicoctl delete node NODE_ID --config=<path_to_file>/calicoctl.cfg
  ```
  {: pre}

5.  Rearranque el nodo trabajador que no se ha suprimido.

  ```
  bx cs worker-reboot CLUSTER_ID NODE_ID
  ```
  {: pre}


El nodo suprimido ya no aparece en Calico.

<br />


## El clúster permanece en un estado pendiente
{: #cs_cluster_pending}

{: tsSymptoms}
Al desplegar el clúster, se queda en estado pendiente y no se inicia.

{: tsCauses}
Si acaba de crear el clúster, es posible que los nodos trabajadores aún se estén configurando. Si ya esperado algo de tiempo, es posible que tenga una VLAN no válida.

{: tsResolve}

Puede probar una de las siguientes soluciones:
  - Compruebe el estado del clúster ejecutando `bx cs clusters`. Luego, compruebe que los nodos trabajadores están desplegados ejecutando `bx cs workers <cluster_name>`.
  - Compruebe si la VLAN es válida. Para ser válida, una VLAN debe estar asociada con infraestructura que pueda alojar un trabajador con almacenamiento en disco local. Puede [ver una lista de las VLAN](/docs/containers/cs_cli_reference.html#cs_vlans) ejecutando `bx cs vlans <location>`; si la VLAN no se muestra en la lista, entonces no es válida. Elija una VLAN diferente.

<br />


## Los pods permanecen en estado pendiente
{: #cs_pods_pending}

{: tsSymptoms}
Cuando se ejecuta `kubectl get pods`, puede ver que los pods permanecen en el estado **Pending**.

{: tsCauses}
Si acaba de crear el clúster de Kubernetes, es posible que los nodos trabajadores aún se estén configurando. Si este clúster ya existe, es posible que no tenga suficiente capacidad en el clúster para desplegar el pod.

{: tsResolve}
Esta tarea precisa de la [política de acceso de administrador](cs_users.html#access_policies). Verifique su [política de acceso](cs_users.html#infra_access) actual.

Si acaba de crear el clúster de Kubernetes, ejecute el siguiente mandato y espere que se hayan inicializado los nodos trabajadores.

```
kubectl get nodes
```
{: pre}

Si este clúster ya existe, compruebe la capacidad del clúster.

1.  Establezca el proxy con el número de puerto predeterminado.

  ```
  kubectl proxy
  ```
   {: pre}

2.  Abra el panel de control de Kubernetes.

  ```
  http://localhost:8001/ui
  ```
  {: pre}

3.  Compruebe si tiene suficiente capacidad en el clúster para desplegar el pod.

4.  Si no tiene suficiente capacidad en el clúster, añada otro nodo trabajador al clúster.

    ```
    bx cs worker-add <cluster_name_or_ID> 1
    ```
    {: pre}

5.  Si los pods continúan en estado **pending** después de que se despliegue por completo el nodo trabajador, consulte la [documentación de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-pod-replication-controller/#my-pod-stays-pending) para solucionar el estado pendiente del pod.

<br />




## Los contenedores no se inician
{: #containers_do_not_start}

{: tsSymptoms}
Los pods se despliegan correctamente en los clústeres, pero los contenedores no inician.

{: tsCauses}
Es posible que los contenedores no se inicien cuando se alcanza la cuota de registro.

{: tsResolve}
[Libere almacenamiento en {{site.data.keyword.registryshort_notm}}.](../services/Registry/registry_quota.html#registry_quota_freeup)

<br />



## No se puede instalar un diagrama de Helm con valores de configuración actualizados
{: #cs_helm_install}

{: tsSymptoms}
Cuando intenta instalar un diagrama de Helm actualizado ejecutando `helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/<chart_name>`, obtiene el mensaje de error `Error: failed to download "ibm/<chart_name>"`.

{: tsCauses}
El URL del repositorio de {{site.data.keyword.Bluemix_notm}} en la instancia de Helm podría ser incorrecto.

{: tsResolve}
Para resolver el problema del diagrama de Helm:

1. Obtenga una lista de los repositorios disponibles actualmente en la instancia de Helm.

    ```
    helm repo list
    ```
    {: pre}

2. En la salida, verifique que el URL del repositorio de {{site.data.keyword.Bluemix_notm}}, `ibm`, es `https://registry.bluemix.net/helm/ibm`.

    ```
    NAME    URL
    stable  https://kubernetes-charts.storage.googleapis.com
    local   http://127.0.0.1:8888/charts
    ibm     https://registry.bluemix.net/helm/ibm
    ```
    {: screen}

    * Si el URL es incorrecto:

        1. Elimine el repositorio de {{site.data.keyword.Bluemix_notm}}.

            ```
            helm repo remove ibm
            ```
            {: pre}

        2. Añada el repositorio de {{site.data.keyword.Bluemix_notm}} de nuevo.

            ```
            helm repo add ibm  https://registry.bluemix.net/helm/ibm
            ```
            {: pre}

    * Si el URL es correcto, obtenga las últimas actualizaciones del repositorio.

        ```
        helm repo update
        ```
        {: pre}

3. Instale el diagrama de Helm con las actualizaciones.

    ```
    helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/<chart_name>
    ```
    {: pre}

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

