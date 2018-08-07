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




# Cómo guardar datos en el clúster
{: #storage}
Puede conservar los datos en {{site.data.keyword.containerlong}} para compartir datos entre instancias de app y evitar que se pierdan si falla un componente del clúster de Kubernetes.


## Planificación de almacenamiento altamente disponible
{: #planning}

En {{site.data.keyword.containerlong_notm}}, puede elegir entre varias opciones para almacenar los datos de la app y compartirlos entre los distintos pods de su clúster. Sin embargo, no todas las opciones de almacenamiento ofrecen el mismo nivel de persistencia y disponibilidad en las situaciones en las que falla un componente en el clúster o todo el sitio.
{: shortdesc}


### Opciones de almacenamiento de datos no persistente
{: #non_persistent}

Puede utilizar las opciones de almacenamiento no persistente si no es necesario que sus datos se almacenen permanentemente o si los datos no tienen que compartirse entre distintas instancias de la app. Las opciones de almacenamiento no persistente también se pueden utilizar para realizar pruebas de unidad de los componentes de la app o probar nuevas características.
{: shortdesc}

La siguiente imagen muestra las opciones de datos no persistentes disponibles en {{site.data.keyword.containerlong_notm}}. Estas opciones están disponibles para clústeres gratuitos y estándares.
<p>
<img src="images/cs_storage_nonpersistent.png" alt="Opciones de almacenamiento de datos no persistente" width="500" style="width: 500px; border-style: none"/></p>

<table summary="La tabla muestra opciones de almacenamiento no persistente. Las filas se deben leer de izquierda a derecha, con el número de la opción en la primera columna, el título de la opción en la segunda columna y una descripción en la tercera columna. " style="width: 100%">
<caption>Opciones de almacenamiento no persistente</caption>
  <thead>
  <th>Opción</th>
  <th>Descripción</th>
  </thead>
  <tbody>
    <tr>
      <td>1. Dentro del contenedor o pod</td>
      <td>Los contenedores y pods son, por diseño, efímeros y pueden fallar inesperadamente. Sin embargo, puede escribir datos en el sistema de archivos local del contenedor para almacenar datos en todo el ciclo de vida del contenedor. Los datos dentro de un contenedor no se pueden compartir con otros contenedores o pods y se pierden cuando el contenedor se cuelga o se elimina. Para obtener más información, consulte [Almacenamiento de datos en un contenedor](https://docs.docker.com/storage/).</td>
    </tr>
  <tr>
    <td>2. En el nodo trabajador</td>
    <td>Cada nodo trabajador se configura con el almacenamiento primario y secundario determinado que está determinado por el tipo de máquina que seleccione para el nodo trabajador. El almacenamiento primario se utiliza para almacenar datos del sistema operativo y se puede acceder a él mediante un [volumen <code>hostPath</code> de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/storage/volumes/#hostpath). El almacenamiento secundario se utiliza para almacenar datos del `kubelet` y del motor de tiempo de ejecución del contenedor. Puede acceder al almacenamiento secundario mediante un [volumen <code>emptyDir</code> de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/storage/volumes/#emptydir)<br/><br/>Mientras que los volúmenes <code>hostPath</code> se utilizan para montar archivos del sistema de archivos del nodo trabajador en el pod, <code>emptyDir</code> crea un directorio vacío que se asigna a un pod del clúster. Todos los contenedores de dicho pod pueden leer y grabar en ese volumen. Puesto que el volumen se asigna a un determinado pod, los datos no se pueden compartir con otros pods de un conjunto de réplicas.<br/><br/><p>Un volumen <code>hostPath</code> o <code>emptyDir</code> y sus datos se eliminan cuando: <ul><li>El nodo trabajador se suprime.</li><li>El nodo trabajador se vuelve a cargar o actualizar.</li><li>El clúster se suprime.</li><li>La cuenta de {{site.data.keyword.Bluemix_notm}} alcanza un estado suspendido. </li></ul></p><p>Además, los datos de un volumen <code>emptyDir</code> se eliminan cuando: <ul><li>El pod asignado se suprime de forma permanente del nodo trabajador.</li><li>El pod asignado se planifica en otro nodo trabajador.</li></ul></p><p><strong>Nota:</strong> Si el contenedor contenido en el pod se cuelga, los datos del volumen siguen estando disponibles en el nodo trabajador.</p></td>
    </tr>
    </tbody>
    </table>


### Opciones de almacenamiento de datos persistentes de alta disponibilidad
{: #persistent}

El principal desafío al crear apps de estado altamente disponible es conservar los datos en varias instancias de la app en múltiples ubicaciones, y mantener siempre los datos sincronizados. Para los datos de alta disponibilidad, quiere asegurarse de contar con una base de datos maestra con múltiples instancias distribuidas en varios centros de datos o incluso regiones. Esta base de datos maestra se debe replicar de forma continua para mantener un único origen fiable. Todas las instancias en el clúster deben leer y escribir en esta base de datos maestra. En caso de que una instancia de la maestra esté inactiva, las otras instancias presuponen la carga de trabajo para que no se produzca una parada en sus apps.
{: shortdesc}

La imagen siguiente muestra las opciones que tiene en {{site.data.keyword.containerlong_notm}} para hacer que sus datos estén altamente disponibles en un clúster estándar. La opción más adecuada depende de los siguientes factores:
  * **El tipo de app que tenga:** Por ejemplo, puede tener una app que deba almacenar los datos en archivos en lugar de en una base de datos.
  * **Requisitos legales sobre dónde almacenar y direccionar los datos:** Por ejemplo, puede verse obligado a almacenar y direccionar los datos solo en los Estados Unidos y no poder utilizar un servicio ubicado en Europa.
  * **Opciones de copia de seguridad y restauración:** Todas las opciones de almacenamiento incluyen prestaciones de copia de seguridad y restauración de datos. Compruebe que las opciones disponibles de copia de seguridad y restauración cumplan los requisitos de su plan de recuperación tras desastre, como la frecuencia de las copias o la capacidad de almacenar datos fuera de su centro de datos primario.
  * **Réplica global:** Para alta disponibilidad, puede que quiera configurar múltiples instancias de almacenamiento que se distribuyan y repliquen en centros de datos de todo el mundo.

<br/>
<img src="images/cs_storage_ha.png" alt="Opciones de alta disponibilidad para almacén persistente"/>

<table summary="La tabla muestra las opciones de almacenamiento persistente. Las filas se leen de izquierda a derecha, con el número de la opción en la columna uno, el título de la opción en la columna dos y una descripción en la columna tres.">
<caption>Opciones de almacén persistente</caption>
  <thead>
  <th>Opción</th>
  <th>Descripción</th>
  </thead>
  <tbody>
  <tr>
  <td>1. Almacenamiento de archivos NFS o almacenamiento en bloque</td>
  <td>Con esta opción, puede conservar datos de la app y contenedores utilizando volúmenes permanentes de Kubernetes. Los volúmenes se alojan en el [almacenamiento en bloque ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud/block-storage) o [almacenamiento de archivos basados en NFS ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud/file-storage/details) de Endurance and Performance y que las apps pueden utilizar para almacenar datos en forma de archivos o en forma de bloques en lugar de hacerlo en una base de datos. Los datos en reposo que se guardan en almacenamiento en bloques y archivos se cifran.<p>{{site.data.keyword.containershort_notm}} proporciona clases de almacenamiento predefinidas que definen el rango de tamaños del almacenamiento, IOPS, la política de supresión y los permisos de lectura y escritura para el volumen. Para iniciar una solicitud de almacenamiento de archivos o almacenamiento en bloque, debe crear una [reclamación de volumen permanente (PVC)](cs_storage.html#create). Después de enviar una PVC, {{site.data.keyword.containershort_notm}} suministra de forma dinámica un volumen permanente alojado en almacenamiento de archivos basado en NFS o en almacenamiento en bloque. [Puede montar la PVC](cs_storage.html#app_volume_mount) como un volumen en su despliegue para permitir que los contenedores lean el volumen y graben en el mismo. </p><p>Los volúmenes permanentes se suministran en el centro de datos donde esté ubicado el nodo trabajador. Puede compartir datos entre el mismo conjunto de réplicas o con otros despliegues en el mismo clúster. No puede compartir datos entre clústeres cuando están ubicados en distintos centros de datos o regiones. </p><p>De forma predeterminada, no se realiza copia de seguridad automática del almacenamiento NFS o del almacenamiento en bloque. Puede configurar una copia de seguridad periódica para el clúster utilizando los [mecanismos de copia de seguridad y restauración establecidos](cs_storage.html#backup_restore). Cuando un contenedor se cuelga o un pod se retira de un nodo trabajador, los datos no se eliminan y pueden acceder a los mismos otros despliegues que monten el volumen. </p><p><strong>Nota:</strong> El almacenamiento en bloques y el almacenamiento compartido de archivos NFS persistente se puede facturar de forma horaria o mensual. Si elige hacerlo de forma mensual, al eliminar el almacenamiento persistente, seguirá pagando el cargo mensual por el mismo, incluso si lo ha utilizado por un intervalo breve de tiempo.</p></td>
  </tr>
  <tr id="cloud-db-service">
    <td>2. Servicio de base de datos en la nube</td>
    <td>Con esta opción, puede conservar datos utilizando un {{site.data.keyword.Bluemix_notm}} servicio en la nube de base de datos, como [IBM Cloudant NoSQL DB](/docs/services/Cloudant/getting-started.html#getting-started-with-cloudant). Se puede acceder a los datos que se almacenan con esta opción desde clústeres, ubicaciones y regiones. <p> Puede optar por configurar una sola instancia de base de datos a la que tengan acceso todas las apps, o [configurar varias instancias en varios centros de datos y réplica](/docs/services/Cloudant/guides/active-active.html#configuring-cloudant-nosql-db-for-cross-region-disaster-recovery) entre las instancias para aumentar la disponibilidad. En la base de datos IBM Cloudant NoSQL, la copia de seguridad de los datos no se realiza automáticamente. Puede utilizar los [mecanismos de copia de seguridad y restauración](/docs/services/Cloudant/guides/backup-cookbook.html#cloudant-nosql-db-backup-and-recovery) para proteger los datos de un fallo del sitio.</p> <p> Para utilizar un servicio en el clúster, debe [enlazar el servicio de {{site.data.keyword.Bluemix_notm}}](cs_integrations.html#adding_app) a un espacio de nombre en el clúster. Cuando enlaza el servicio al clúster, se crea un secreto de Kubernetes. El secreto de Kubernetes contiene información confidencial sobre el servicio, como por ejemplo el URL del servicio y su nombre de usuario y contraseña. Puede montar el secreto como un volumen secreto en el pod y acceder al servicio mediante las credenciales del secreto. Si monta el volumen secreto en otros pods, también puede compartir datos entre pods. Cuando un contenedor se cuelga o un pod se retira de un nodo trabajador, los datos no se eliminan y pueden acceder a los mismos otros pods que monten el volumen secreto. <p>La mayoría de los servicios de base de datos de {{site.data.keyword.Bluemix_notm}} proporcionan espacio en disco para una cantidad de datos pequeña sin coste, para así poder probar sus características.</p></td>
  </tr>
  <tr>
    <td>3. Base de datos local</td>
    <td>Si debe almacenar sus datos en local por motivos legales, puede [configurar una conexión de VPN](cs_vpn.html#vpn) a su base de datos local y utilizar los mecanismos existentes de almacenamiento, copia de seguridad y réplica en su centro de datos.</td>
  </tr>
  </tbody>
  </table>

{: caption="Tabla. Opciones de almacén permanente para despliegues en clústeres de Kubernetes" caption-side="top"}

<br />



## Utilización de recursos compartidos de archivos de NFS existentes en clústeres
{: #existing}

Si tiene recursos compartidos de archivos NFS existentes en su cuenta de infraestructura de IBM Cloud (SoftLayer) que desea utilizar, puede hacerlo creando volúmenes permanentes en el recurso compartido existente de archivos storage.
{:shortdesc}

Un volumen permanente (PV) es un recurso de Kubernetes que representa un dispositivo de almacenamiento real que se suministra en un centro de datos. En los volúmenes permanentes se abstraen los detalles de cómo {{site.data.keyword.Bluemix_notm}} Storage suministra el tipo de almacenamiento específico. Para montar un PV en el clúster, debe solicitar almacenamiento persistente para el pod creando una reclamación de volumen permanente (PVC). En el siguiente diagrama se ilustra la relación entre los PV y las PVC.

![Cree volúmenes permanentes y reclamaciones de volúmenes permanentes](images/cs_cluster_pv_pvc.png)

Tal como se muestra en el diagrama, para habilitar el almacenamiento NFS existente, debe crear un PV con un determinado tamaño y modalidad de acceso y crear una PVC que coincida con la especificación de PV. Si el PV y la PVC coinciden, se enlazan. El usuario del clúster solo puede utilizar PVC enlazadas para montar el volumen en un despliegue. Este proceso se conoce como suministro estático de almacenamiento permanente.

Antes de empezar, asegúrese de que tiene un recurso compartido de archivos NFS existentes que puede utilizar para crear el PV. Por ejemplo, si anteriormente ha [creado una PVC con una política de clase de almacenamiento `retain`](#create), puede utilizar los datos retenidos en la compartición de archivos NFS existente para esta nueva PVC.

**Nota:** El suministro estático de almacenamiento permanente sólo se aplica a recursos compartidos de archivos NFS existentes. Si no tiene recursos compartidos de archivos NFS existentes, los usuarios del clúster puedan utilizar el proceso de [suministro dinámico](cs_storage.html#create) para añadir PV.

Para crear un PV y una PVC coincidentes, siga estos pasos.

1.  En la cuenta de infraestructura de IBM Cloud (SoftLayer), busque el ID y la vía de acceso del recurso compartido de archivos NFS en el que desea crear el objeto de PV. Además, autorice el almacenamiento de archivos para las subredes del clúster. Esta autorización proporciona al clúster acceso al almacenamiento.
    1.  Inicie sesión en la cuenta de infraestructura de IBM Cloud (SoftLayer).
    2.  Pulse **Almacenamiento**.
    3.  Pulse **Almacenamiento de archivos** y, en el menú **Acciones**, seleccione **Autorizar host**.
    4.  Seleccione **Subredes**.
    5.  En la lista desplegable, seleccione la subred VLAN privada a la que está conectado el nodo trabajador. Para encontrar la subred del nodo trabajador, ejecute `bx cs workers <cluster_name>` y compare la `IP privada` del nodo trabajador con la subred que ha encontrado en la lista desplegable.
    6.  Pulse **Enviar**.
    6.  Pulse el nombre del almacenamiento de archivos.
    7.  Ante el campo **Punto de montaje**. El campo se muestra como `<server>:/<path>`.
2.  Cree un archivo de configuración de almacenamiento para el PV. Incluya el servidor y la vía de acceso del campo **Punto de montaje** del almacenamiento de archivos.

    ```
    apiVersion: v1
    kind: PersistentVolume
    metadata:
     name: mypv
    spec:
     capacity:
       storage: "20Gi"
     accessModes:
       - ReadWriteMany
     nfs:
       server: "nfslon0410b-fz.service.networklayer.com"
       path: "/IBM01SEV8491247_0908/data01"
    ```
    {: codeblock}

    <table>
    <caption>Visión general de los componentes del archivo YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general de los componentes del archivo YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code>name</code></td>
    <td>Especifique el nombre del objeto de PV que crear.</td>
    </tr>
    <tr>
    <td><code>spec/capacity/storage</code></td>
    <td>Especifique el tamaño de almacenamiento del recurso compartido de archivos NFS existente. El tamaño de almacenamiento se debe especificar en gigabytes, por ejemplo, 20Gi (20 GB) o 1000Gi (1 TB), y el tamaño debe coincidir con el tamaño del recurso compartido de archivos existente.</td>
    </tr>
    <tr>
    <td><code>accessMode</code></td>
    <td>Las modalidades de acceso definen el modo de montar la PVC en un nodo trabajador.<ul><li>ReadWriteOnce (RWO): el PV se puede montar en despliegues únicamente en un solo nodo trabajador. Los contenedores en despliegues que se montan en este PV pueden leer el volumen y grabar en el mismo.</li><li>ReadOnlyMany (ROX): el PV se puede montar en despliegues albergados en varios nodos trabajadores. Los despliegues que se montan en este PV solo pueden leer el volumen.</li><li>ReadWriteMany (RWX): el PV se puede montar en despliegues albergados en varios nodos trabajadores. Los despliegues que se montan en este PV pueden leer el volumen y grabar en el mismo.</li></ul></td>
    </tr>
    <tr>
    <td><code>spec/nfs/server</code></td>
    <td>Escriba el ID del servidor de recursos compartidos de archivos NFS.</td>
    </tr>
    <tr>
    <td><code>path</code></td>
    <td>Especifique la vía de acceso al recurso compartido de archivos NFS en el que desea crear el objeto de PV.</td>
    </tr>
    </tbody></table>

3.  Cree el objeto de PV en el clúster.

    ```
    kubectl apply -f deploy/kube-config/mypv.yaml
    ```
    {: pre}

4.  Verifique que se ha creado el PV.

    ```
    kubectl get pv
    ```
    {: pre}

5.  Cree otro archivo de configuración para crear la PVC. Para que la PVC coincida con el objeto de PV que ha creado anteriormente, debe elegir el mismo valor para `storage` y `accessMode`. El campo `storage-class` debe estar vacío. Si alguno de estos campos no coincide con el PV, se crea automáticamente un nuevo PV.

    ```
    kind: PersistentVolumeClaim
    apiVersion: v1
    metadata:
     name: mypvc
     annotations:
       volume.beta.kubernetes.io/storage-class: ""
    spec:
     accessModes:
       - ReadWriteMany
     resources:
       requests:
         storage: "20Gi"
    ```
    {: codeblock}

6.  Cree la PVC.

    ```
    kubectl apply -f deploy/kube-config/mypvc.yaml
    ```
    {: pre}

7.  Verifique que la PVC se ha creado y se ha vinculado al objeto de PV. Este proceso puede tardar unos minutos.

    ```
    kubectl describe pvc mypvc
    ```
    {: pre}

    Salida de ejemplo:

    ```
    Name: mypvc
    Namespace: default
    StorageClass: ""
    Status: Bound
    Volume: pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    Labels: <none>
    Capacity: 20Gi
    Access Modes: RWX
    Events:
      FirstSeen LastSeen Count From        SubObjectPath Type Reason Message
      --------- -------- ----- ----        ------------- -------- ------ -------
      3m 3m 1 {ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 } Normal Provisioning External provisioner is provisioning volume for claim "default/my-persistent-volume-claim"
      3m 1m	 10 {persistentvolume-controller } Normal ExternalProvisioning cannot find provisioner "ibm.io/ibmc-file", expecting that a volume for the claim is provisioned either manually or via external software
      1m 1m 1 {ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 } Normal ProvisioningSucceeded	Successfully provisioned volume pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    ```
    {: screen}


Ha creado correctamente un objeto de PV y lo ha enlazado a una PVC. Ahora los usuarios del clúster pueden [montar la PVC](#app_volume_mount) en sus despliegues y empezar a leer el objeto de PV y a grabar en el mismo.

<br />



## Utilización de almacenamiento en bloque existente en el clúster
{: #existing_block}

Antes de empezar, asegúrese de que tiene una instancia de almacenamiento en bloque existente que puede utilizar para crear el PV. Por ejemplo, si anteriormente ha [creado una PVC con una política de clase de almacenamiento `retain`](#create), puede utilizar los datos retenidos en el bloque en almacenamiento existente para esta nueva PVC.

**Nota**: El almacenamiento en bloques es un dispositivo de modalidad de acceso `ReadWriteOnce`. Solo puede montarlo en un pod en un nodo trabajador en el clúster al mismo tiempo.

Para crear un PV y una PVC coincidentes, siga estos pasos.

1.  Recupere o genere una clave de API para su cuenta (SoftLayer) de infraestructura de IBM Cloud.
    1. Inicie sesión en el [portal de la infraestructura de IBM Cloud (SoftLayer) ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://control.softlayer.com/).
    2. Seleccione **Cuenta**, **Usuarios** y, a continuación, **Lista de usuarios**.
    3. Encuentre su ID de usuario.
    4. En la columna de **Clave de API**, pulse **Generar** para generar una clave de API o **Ver** para ver su clave de API existente.
2.  Recupere el nombre de usuario de API de su cuenta (SoftLayer) de infraestructura de IBM Cloud.
    1. Desde el menú **Lista de usuarios**, seleccione su ID de usuario.
    2. En la sección **Información de acceso de API**, busque su **Nombre de usuario de API**.
3.  Inicie una sesión en el plugin de CLI de la infraestructura de IBM Cloud.
    ```
    bx sl init
    ```
    {: pre}

4.  Elija autenticarse utilizando el nombre de usuario y la clave de API de su cuenta (SoftLayer) de infraestructura de IBM Cloud.
5.  Especifique el nombre de usuario y la clave de API que recuperó en los pasos anteriores.
6.  Obtenga una lista de los dispositivos de almacenamiento en bloque disponibles.
    ```
    bx sl block volume-list
    ```
    {: pre}

    Salida de ejemplo:
    ```
    id         username            datacenter   storage_type              capacity_gb   bytes_used   ip_addr         lunId   active_transactions
    38642141   IBM02SEL1543159-1   dal10        endurance_block_storage   20            -            169.xx.xxx.xxx   170     0
    ```
    {: screen}

7.  Anote el `id`, `ip_addr`, `capacity_gb` y `lunId` del dispositivo de bloque de almacenamiento que desea montar para el clúster.
8.  Cree un archivo de configuración para el PV. Incluya el ID de almacenamiento en bloque, la dirección IP y el tamaño e ID de la unidad lógica que recuperó en el paso anterior.

    ```
    apiVersion: v1
    kind: PersistentVolume
    metadata:
      name: mypv
    spec:
      capacity:
        storage: "<storage_size>"
      accessModes:
        - ReadWriteOnce
      flexVolume:
        driver: "ibm/ibmc-block"
        fsType: "<fs_type>"
        options:
          "Lun": "<lun_ID>"
          "TargetPortal": "<IP_address>"
          "VolumeID": "<volume_ID>"
          "volumeName": "<volume_name>"
      ```
      {: codeblock}

    <table>
    <caption>Visión general de los componentes del archivo YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general de los componentes del archivo YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code>metadata/name</code></td>
    <td>Especifique el nombre del PV que crear.</td>
    </tr>
    <tr>
    <td><code>spec/flexVolume/fsType</code></td> 
    <td>Especifique el tipo de sistema de archivos que está configurado para el almacenamiento en bloques existente. Elija entre <code>ext4</code> o <code>xfs</code>. Si no especifica esta opción, de forma predeterminada será <code>ext4</code> para el PV. Cuando se define el fsType erróneo, se sigue creando el PV, sin embargo, el montaje del PV para el pod fallará. </td></tr>	    
    <tr>
    <td><code>spec/capacity/storage</code></td>
    <td>Especifique el tamaño del almacenamiento en bloque existente que ha recuperado en el paso anterior como <code>capacity-gb</code>. El tamaño de almacenamiento se debe especificar en gigabytes, por ejemplo, 20Gi (20 GB) o 1000Gi (1 TB).</td>
    </tr>
    <tr>
    <td><code>flexVolume/options/Lun</code></td>
    <td>Especifique el ID de la unidad lógica de su almacenamiento en bloque que recuperó en el paso anterior como <code>lunId</code>.</td>
    </tr>
    <tr>
    <td><code>flexVolume/options/TargetPortal</code></td>
    <td>Especifique la dirección IP del bloque de almacenamiento que recuperó en el paso anterior como <code>ip_addr</code>. </td>
    </tr>
    <tr>
	    <td><code>flexVolume/options/VolumeId</code></td>
	    <td>Especifique el ID de su almacenamiento en bloque que recuperó en el paso anterior como <code>id</code>.</td>
	    </tr>
	    <tr>
		    <td><code>flexVolume/options/volumeName</code></td>
		    <td>Especifique un nombre para el volumen.</td>
	    </tr>
    </tbody></table>

9.  Cree el PV en el clúster.
    ```
    kubectl apply -f mypv.yaml
    ```
    {: pre}

10. Verifique que se ha creado el PV.
    ```
    kubectl get pv
    ```
    {: pre}

11. Cree otro archivo de configuración para crear la PVC. Para que la PVC coincida con el PV que ha creado anteriormente, debe elegir el mismo valor para `storage` y `accessMode`. El campo `storage-class` debe estar vacío. Si alguno de estos campos no coincide con el PV, se crea automáticamente un nuevo PV.

     ```
     kind: PersistentVolumeClaim
    apiVersion: v1
    metadata:
     name: mypvc
     annotations:
       volume.beta.kubernetes.io/storage-class: ""
     spec:
      accessModes:
        - ReadWriteOnce
      resources:
        requests:
          storage: "20Gi"
     ```
     {: codeblock}

12.  Cree la PVC.
     ```
     kubectl apply -f mypvc.yaml
     ```
     {: pre}

13.  Verifique que la PVC se ha creado y se ha enlazado al PV que ha creado con anterioridad. Este proceso puede tardar unos minutos.
     ```
     kubectl describe pvc mypvc
     ```
     {: pre}

     Salida de ejemplo:

     ```
     Name: mypvc
    Namespace: default
    StorageClass: ""
     Status: Bound
     Volume: pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
     Labels: <none>
     Capacity: 20Gi
     Access Modes: RWO
     Events:
       FirstSeen LastSeen Count From        SubObjectPath Type Reason Message
       --------- -------- ----- ----        ------------- -------- ------ -------
       3m 3m 1 {ibm.io/ibmc-block 31898035-3011-11e7-a6a4-7a08779efd33 } Normal Provisioning External provisioner is provisioning volume  for claim "default/my-persistent-volume-claim"
       3m 1m	 10 {persistentvolume-controller } Normal ExternalProvisioning cannot find provisioner "ibm.io/ibmc-block", expecting that  a volume for the claim is provisioned either manually or via external software
       1m 1m 1 {ibm.io/ibmc-block 31898035-3011-11e7-a6a4-7a08779efd33 } Normal ProvisioningSucceeded	Successfully provisioned volume  pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
     ```
     {: screen}

Ha creado correctamente un PV y lo ha enlazado a una PVC. Ahora los usuarios del clúster pueden [montar la PVC](#app_volume_mount) en sus despliegues y empezar a leer el PV y a grabar en el mismo.

<br />



## Adición de almacenamiento de archivos NFS o almacenamiento en bloque a apps
{: #create}

Cree una reclamación de volumen permanente (PVC) para suministrar almacenamiento de archivos NFS o almacenamiento en bloque al clúster. Luego monte esta reclamación en un volumen permanente (PV) para asegurarse de que los datos estén disponibles aunque los pods se cuelguen o se cierren.
{:shortdesc}

IBM dispone en un clúster el almacenamiento de archivos NFS y el almacenamiento en bloque que respalda el PV con el propósito de proporcionar alta disponibilidad a los datos. Las clases de almacenamiento describen los tipos de ofertas de almacenamiento disponibles y definen aspectos como la política de retención de datos, el tamaño en gigabytes e IOPS, cuando se crea el PV.

**Nota**: El almacenamiento en bloques es un dispositivo de modalidad de acceso `ReadWriteOnce`. Solo puede montarlo en un pod en un nodo trabajador en el clúster al mismo tiempo. La modalidad de acceso del almacenamiento de archivos NFS es `ReadWriteMany`, de forma que es posible montarlo en varios pods a través de trabajadores dentro del clúster.

Antes de empezar:
- Si tiene un cortafuegos, [permita el acceso de salida](cs_firewall.html#pvc) para los rangos de IP de la infraestructura de IBM Cloud (SoftLayer) de las ubicaciones en las que están en los clústeres, de modo que pueda crear las PVC.
- Si desea montar el almacenamiento en bloque a sus apps, primero debe instalar el [plugin {{site.data.keyword.Bluemix_notm}} Storage para el almacenamiento en bloque](#install_block).

Para añadir almacenamiento persistente:

1.  Revise las clases de almacenamiento disponibles. {{site.data.keyword.containerlong}} ofrece clases de almacenamiento predefinidas de almacenamiento de archivos NFS y de almacenamiento en bloque para que el administrador del clúster no tenga que crear ninguna clase de almacenamiento. La clase de almacenamiento `ibmc-file-bronze` es la misma que la clase de almacenamiento `default`. De forma predeterminada, el almacenamiento se suministra con un sistema de archivos `nfs` y el almacenamiento en bloques se suministra con sistema de archivos `ext4`. Si desea suministrar almacenamiento en bloques con un sistema de archivos `XFS`, [cree su propia clase de almacenamiento personalizada](#custom_storageclass). 

    ```
    kubectl get storageclasses
    ```
    {: pre}

    ```
    $ kubectl get storageclasses
    NAME                         TYPE
    default                      ibm.io/ibmc-file
    ibmc-file-bronze (default)   ibm.io/ibmc-file
    ibmc-file-custom             ibm.io/ibmc-file
    ibmc-file-gold               ibm.io/ibmc-file
    ibmc-file-retain-bronze      ibm.io/ibmc-file
    ibmc-file-retain-custom      ibm.io/ibmc-file
    ibmc-file-retain-gold        ibm.io/ibmc-file
    ibmc-file-retain-silver      ibm.io/ibmc-file
    ibmc-file-silver             ibm.io/ibmc-file
    ibmc-block-custom            ibm.io/ibmc-block
    ibmc-block-bronze            ibm.io/ibmc-block
    ibmc-block-gold              ibm.io/ibmc-block
    ibmc-block-silver            ibm.io/ibmc-block
    ibmc-block-retain-bronze     ibm.io/ibmc-block
    ibmc-block-retain-silver     ibm.io/ibmc-block
    ibmc-block-retain-gold       ibm.io/ibmc-block
    ibmc-block-retain-custom     ibm.io/ibmc-block
    ```
    {: screen}

    **Sugerencia:** Si desea cambiar la clase de almacenamiento predeterminada, ejecute `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'` y sustituya `<storageclass>` con el nombre de la clase de almacenamiento.

2.  Decida si desea conservar los datos y la compartición de archivos NFS o el almacenamiento en bloque después de suprimir la PVC.
    - Si desea conservar los datos, seleccione la clase de almacenamiento `retain`. Cuando se suprime la PVC, únicamente ésta se suprime. El PV todavía existe en el clúster y se guardan sus datos, sin embargo, no es posible reutilizarlo con otra PVC. Además, los archivos NFS o el almacenamiento en bloques y los datos siguen existiendo en la cuenta de infraestructura de IBM Cloud (SoftLayer). Posteriormente, para acceder a estos datos en el clúster, cree una PVC y un PV coincidentes que hagan referencia al [archivo NFS](#existing) o almacenamiento en [block](#existing_block). 
    - Si desea que el PV, los datos y su almacenamiento en bloques o compartido de archivos NFS se supriman cuando suprima la PVC, elija una clase de almacenamiento sin `retain`.

3.  **Si utiliza las clases de almacenamiento bronce, plata y oro**: obtendrá [Almacenamiento resistente ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://knowledgelayer.softlayer.com/topic/endurance-storage), que define las IOPS por GB para cada clase. Sin embargo, puede determinar la IOPS total eligiendo un tamaño dentro del rango disponible. Puede seleccionar cualquier número entero de gigabytes dentro del rango de tamaños permitido (como 20 Gi, 256 Gi, 11854 Gi). Por ejemplo, si selecciona un tamaño de compartición de archivos o de almacenamiento en bloque de 1000Gi en la clase de almacenamiento de plata de 4 IOPS por GB, el volumen tendrá un total de 4000 IOPS. Cuantos más IOPS tenga el PV, más rápido se procesarán las operaciones de entrada y salida. La tabla siguiente describe las IOPS por gigabyte y rango de tamaño para cada clase de almacenamiento.

    <table>
         <caption>Tabla de IOPS y rangos de tamaño de clase de almacenamiento por gigabyte</caption>
         <thead>
         <th>Clase de almacenamiento</th>
         <th>IOPS por gigabyte</th>
         <th>Rango de tamaño en gigabytes</th>
         </thead>
         <tbody>
         <tr>
         <td>Bronce (predeterminada)</td>
         <td>2 IOPS/GB</td>
         <td>20-12000 Gi</td>
         </tr>
         <tr>
         <td>Plata</td>
         <td>4 IOPS/GB</td>
         <td>20-12000 Gi</td>
         </tr>
         <tr>
         <td>Oro</td>
         <td>10 IOPS/GB</td>
         <td>20-4000 Gi</td>
         </tr>
         </tbody></table>

    <p>**Mandato de ejemplo para mostrar los detalles de una clase de almacenamiento**:</p>

    <pre class="pre"><code>kubectl describe storageclasses ibmc-file-silver</code></pre>

4.  **Si elige la clase de almacenamiento personalizada**: obtendrá [Almacenamiento de rendimiento ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://knowledgelayer.softlayer.com/topic/performance-storage) y tendrá más control sobre la elección de la combinación de IOPS y el tamaño. Por ejemplo, si selecciona un tamaño de 40Gi para el PVC, puede elegir unas IOPS que sean múltiplo de 100 comprendidas entre 100 y 2000 IOPS. Las IOPS que elige son estáticas y no se escalan con el tamaño del almacenamiento. Si elige 40Gi con 100 IOPS, el total de IOPS sigue siendo 100. La tabla siguiente muestra los rangos de IOPS entre los que puede elegir en función del tamaño que seleccione.

    <table>
         <caption>Tabla de IOPS y rangos de tamaño de clase de almacenamiento personalizadas</caption>
         <thead>
         <th>Rango de tamaño en gigabytes</th>
         <th>Rango de IOPS en múltiplos de 100</th>
         </thead>
         <tbody>
         <tr>
         <td>20-39 Gi</td>
         <td>100-1000 IOPS</td>
         </tr>
         <tr>
         <td>40-79 Gi</td>
         <td>100-2000 IOPS</td>
         </tr>
         <tr>
         <td>80-99 Gi</td>
         <td>100-4000 IOPS</td>
         </tr>
         <tr>
         <td>100-499 Gi</td>
         <td>100-6000 IOPS</td>
         </tr>
         <tr>
         <td>500-999 Gi</td>
         <td>100-10000 IOPS</td>
         </tr>
         <tr>
         <td>1000-1999 Gi</td>
         <td>100-20000 IOPS</td>
         </tr>
         <tr>
         <td>2000-2999 Gi</td>
         <td>200-40000 IOPS</td>
         </tr>
         <tr>
         <td>3000-3999 Gi</td>
         <td>200-48000 IOPS</td>
         </tr>
         <tr>
         <td>4000-7999 Gi</td>
         <td>300-48000 IOPS</td>
         </tr>
         <tr>
         <td>8000-9999 Gi</td>
         <td>500-48000 IOPS</td>
         </tr>
         <tr>
         <td>10000-12000 Gi</td>
         <td>1000-48000 IOPS</td>
         </tr>
         </tbody></table>

    <p>**Mandato de ejemplo para mostrar los detalles de una clase de almacenamiento personalizada**:</p>

    <pre class="pre"><code>kubectl describe storageclasses ibmc-file-retain-custom</code></pre>

5.  Decida si desea que se le facture por horas o meses. De forma predeterminada, se le facturará mensualmente.

6.  Cree un archivo de configuración para definir su PVC y guarde la configuración como archivo `.yaml`.

    -  **Ejemplo de clases de almacenamiento bronce, plata y oro**:
       El siguiente archivo `.yaml` crea una reclamación que se denomina `mypvc` de la clase de almacenamiento `"ibmc-file-silver"`, que se factura con el valor `"hourly"`, con un tamaño de gigabyte de `24Gi`. Si desea crear una PVC para montar el almacenamiento en bloque a su clúster, asegúrese de especificar `ReadWriteOnce` en la sección `accessModes`.

       ```
       apiVersion: v1
       kind: PersistentVolumeClaim
       metadata:
         name: mypvc
         annotations:
           volume.beta.kubernetes.io/storage-class: "ibmc-file-silver"
         labels:
           billingType: "hourly"
       spec:
         accessModes:
           - ReadWriteMany
         resources:
           requests:
             storage: 24Gi
        ```
        {: codeblock}

    -  **Ejemplo de clases de almacenamiento personalizadas**:
       El siguiente archivo `.yaml` crea una reclamación que se denomina `mypvc` de la clase de almacenamiento `ibmc-file-retain-custom`, que se factura con el valor predeterminado `"monthly"`, con un tamaño de gigabyte de `45Gi` y un valor de IOPS de `"300"`. Si desea crear una PVC para montar el almacenamiento en bloque a su clúster, asegúrese de especificar `ReadWriteOnce` en la sección `accessModes`.

       ```
       apiVersion: v1
       kind: PersistentVolumeClaim
       metadata:
         name: mypvc
         annotations:
           volume.beta.kubernetes.io/storage-class: "ibmc-file-retain-custom"
         labels:
           billingType: "monthly"
       spec:
         accessModes:
           - ReadWriteMany
         resources:
           requests:
             storage: 45Gi
             iops: "300"
        ```
        {: codeblock}

        <table>
        <caption>Visión general de los componentes del archivo YAML</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general de los componentes del archivo YAML</th>
        </thead>
        <tbody>
        <tr>
        <td><code>metadata/name</code></td>
        <td>Escriba el nombre de la PVC.</td>
        </tr>
        <tr>
        <td><code>metadata/annotations</code></td>
        <td>Especifique la clase de almacenamiento para el PV:
          <ul>
          <li>ibmc-file-bronze / ibmc-file-retain-bronze : 2 IOPS por GB.</li>
          <li>ibmc-file-silver / ibmc-file-retain-silver: 4 IOPS por GB.</li>
          <li>ibmc-file-gold / ibmc-file-retain-gold: 10 IOPS por GB.</li>
          <li>ibmc-file-custom / ibmc-file-retain-custom: Varios valores de IOPS disponibles.</li>
          <li>ibmc-block-bronze / ibmc-block-retain-bronze : 2 IOPS por GB.</li>
          <li>ibmc-block-silver / ibmc-block-retain-silver: 4 IOPS por GB.</li>
          <li>ibmc-block-gold / ibmc-block-retain-gold: 10 IOPS por GB.</li>
          <li>ibmc-block-custom / ibmc-block-retain-custom: Varios valores de IOPS disponibles.</li></ul>
          <p>Si no especifica ninguna clase de almacenamiento, el PV se crea en la clase de almacenamiento predeterminada.</p><p>**Sugerencia:** Si desea cambiar la clase de almacenamiento predeterminada, ejecute <code>kubectl patch storageclass &lt;storageclass&gt; -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'</code> y sustituya <code>&lt;storageclass&gt;</code> con el nombre de la clase de almacenamiento.</p></td>
        </tr>
        <tr>
          <td><code>metadata/labels/billingType</code></td>
          <td>Especifique la frecuencia con la que desea que se calcule la factura de almacenamiento, los valores son "monthly" o "hourly". El valor predeterminado es "monthly".</td>
        </tr>
        <tr>
        <td><code>spec/resources/requests/storage</code></td>
        <td>Indique el tamaño del almacenamiento de archivos, en gigabytes (Gi). Elija un número entero dentro del rango de tamaños permitido. </br></br><strong>Nota: </strong> Una vez suministrado el almacenamiento, no puede cambiar el tamaño de la compartición de archivos NFS o del almacenamiento en bloque. Asegúrese de especificar un tamaño que coincida con la cantidad de datos que desea almacenar. </td>
        </tr>
        <tr>
        <td><code>spec/resources/requests/iops</code></td>
        <td>Esta opción es únicamente para las clases de almacenamiento personalizado (`ibmc-file-custom / ibmc-file-retain-custom / ibmc-block-custom / ibmc-block-retain-custom`). Especifique el IOPS total para el almacenamiento seleccionando un múltiplo de 100 dentro del rango permitido. Para ver todas las opciones, ejecute `kubectl describe storageclasses <storageclass>`. Si elige un IOPS distinto del que aparece en la lista, el IOPS se redondea.</td>
        </tr>
        </tbody></table>
	
    Si desea utilizar una clase de almacenamiento personalizada, cree su PVC con el nombre de clase de almacenamiento correspondiente, unas IOPS válidas y un tamaño.   
    {: tip}

7.  Cree la PVC.

    ```
    kubectl apply -f mypvc.yaml
    ```
    {: pre}

8.  Verifique que la PVC se ha creado y se ha vinculado al PV. Este proceso puede tardar unos minutos.

    ```
    kubectl describe pvc mypvc
    ```
    {: pre}

    Salida de ejemplo:

    ```
    Name: mypvc
    Namespace: default
    StorageClass: ""
    Status:		Bound
    Volume:		pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    Labels:		<none>
    Capacity:	20Gi
    Access Modes:	RWX
    Events:
      FirstSeen	LastSeen	Count	From								SubObjectPath	Type		Reason			Message
      ---------	--------	-----	----								-------------	--------	------			-------
      3m		3m		1	{ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 }			Normal		Provisioning		External provisioner is provisioning volume for claim "default/my-persistent-volume-claim"
      3m		1m		10	{persistentvolume-controller }							Normal		ExternalProvisioning	cannot find provisioner "ibm.io/ibmc-file", expecting that a volume for the claim is provisioned either manually or via external software
      1m		1m		1	{ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 }			Normal		ProvisioningSucceeded	Successfully provisioned volume pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2

    ```
    {: screen}

9.  {: #app_volume_mount}Para montar la PVC en el despliegue, cree un archivo `.yaml` de configuración.

    ```
    apiVersion: apps/v1beta1
    kind: Deployment
    metadata:
      name: <deployment_name>
      labels:
        app: <deployment_label>
    spec:
      selector:
        matchLabels:
          app: <app_name>
      template:
        metadata:
          labels:
            app: <app_name>
        spec:
          containers:
          - image: <image_name>
            name: <container_name>
            volumeMounts:
            - name: <volume_name>
              mountPath: /<file_path>
          volumes:
          - name: <volume_name>
            persistentVolumeClaim:
              claimName: <pvc_name>
    ```
    {: codeblock}

    <table>
    <caption>Visión general de los componentes del archivo YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general de los componentes del archivo YAML</th>
    </thead>
    <tbody>
        <tr>
    <td><code>metadata/labels/app</code></td>
    <td>Una etiqueta para el despliegue.</td>
      </tr>
      <tr>
        <td><code>spec/selector/matchLabels/app</code> <br/> <code>spec/template/metadata/labels/app</code></td>
        <td>Una etiqueta para la app.</td>
      </tr>
    <tr>
    <td><code>template/metadata/labels/app</code></td>
    <td>Una etiqueta para el despliegue.</td>
      </tr>
    <tr>
    <td><code>spec/containers/image</code></td>
    <td>El nombre del imagen que desea utilizar. Para ver una lista de todas las imágenes disponibles en su cuenta de {{site.data.keyword.registryshort_notm}}, ejecute `bx cr image-list`.</td>
    </tr>
    <tr>
    <td><code>spec/containers/name</code></td>
    <td>El nombre del contenedor que desea desplegar en el clúster.</td>
    </tr>
    <tr>
    <td><code>spec/containers/volumeMounts/mountPath</code></td>
    <td>La vía de acceso absoluta del directorio en el que el que está montado el volumen dentro del contenedor.</td>
    </tr>
    <tr>
    <td><code>spec/containers/volumeMounts/name</code></td>
    <td>El nombre del volumen que va a montar en el pod.</td>
    </tr>
    <tr>
    <td><code>volumes/name</code></td>
    <td>El nombre del volumen que va a montar en el pod. Normalmente, este nombre es el mismo que <code>volumeMounts/name</code>.</td>
    </tr>
    <tr>
    <td><code>volumes/persistentVolumeClaim/claimName</code></td>
    <td>El nombre de la PVC que desea utilizar como volumen. Cuando monta el volumen en el pod, Kubernetes identifica el PV enlazado a la PVC y permite al usuario realizar operaciones de lectura y escritura en el PV.</td>
    </tr>
    </tbody></table>

10.  Cree el despliegue y monte la PVC.
     ```
     kubectl apply -f <local_yaml_path>
     ```
     {: pre}

11.  Verifique que el volumen se ha montado correctamente.

     ```
     kubectl describe deployment <deployment_name>
     ```
     {: pre}

     El punto de montaje se muestra en el campo **Volume Mounts** y el volumen se muestra en el campo **Volumes**.

     ```
      Volume Mounts:
          /var/run/secrets/kubernetes.io/serviceaccount from default-token-tqp61 (ro)
          /volumemount from myvol (rw)
     ...
     Volumes:
      myvol:
        Type: PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
        ClaimName: mypvc
        ReadOnly: false
     ```
     {: screen}

{: #nonroot}
{: #enabling_root_permission}

**Permisos NFS**: ¿Buscando documentación sobre cómo habilitar permisos no root de NFS? Consulte [Adición de acceso de usuario no root al almacenamiento de archivos NFS](cs_troubleshoot_storage.html#nonroot).

<br />

	
## Personalización de una clase de almacenamiento para el almacenamiento en bloques XFS
{: #custom_storageclass}

Las clases de almacenamiento definidas de forma previa por {{site.data.keyword.containerlong}} suministran de forma previa almacenamiento en bloques con un sistema de archivos `ext4`. Puede crear una clase de almacenamiento personalizada para suministrar almacenamiento en bloques con un sistema de archivos `XFS`. 
{: shortdesc}

Antes de empezar: 
- [Defina el clúster como destino de la CLI de Kubernetes](cs_cli_install.html#cs_cli_configure).
- Instale el plugin [{{site.data.keyword.Bluemix_notm}} Storage para el almacenamiento en bloques](#install_block).

Para crear una clase de almacenamiento personalizada: 
1. Cree un archivo yaml para su clase de almacenamiento personalizada. 
   ```
   apiVersion: storage.k8s.io/v1
   kind: StorageClass
   metadata:
     name: ibmc-block-custom-xfs
     labels:
       addonmanager.kubernetes.io/mode: Reconcile
   provisioner: ibm.io/ibmc-block
   parameters:
     type: "Performance"
     sizeIOPSRange: |-
       [20-39]Gi:[100-1000]
       [40-79]Gi:[100-2000]
       [80-99]Gi:[100-4000]
       [100-499]Gi:[100-6000]
       [500-999]Gi:[100-10000]
       [1000-1999]Gi:[100-20000]
       [2000-2999]Gi:[200-40000]
       [3000-3999]Gi:[200-48000]
       [4000-7999]Gi:[300-48000]
       [8000-9999]Gi:[500-48000]
       [10000-12000]Gi:[1000-48000]
     fsType: "xfs"
     reclaimPolicy: "Delete"
     classVersion: "2"
   ```
   {: codeblock}
   
   Si desea mantener los datos después de eliminar el almacenamiento en bloques de su clúster, cambie `reclaimPolicy` a `Retain`. 
   {: tip}
   
2. Cree una clase de almacenamiento en su clúster. 
   ```
   kubectl apply -f <filepath/xfs_storageclass.yaml>
   ```
   {: pre}
       
3. Verifique que se haya creado la clase de almacenamiento personalizada. 
   ```
   kubectl get storageclasses
   ```
   {: pre}

4. Suministre [almacenamiento en bloques XFS](#create) con su clase de almacenamiento personalizada. 

<br />


## Cómo cambiar la versión del almacenamiento de archivos NFS predeterminada
{: #nfs_version}

La versión del almacenamiento de archivos NFS determina el protocolo que se utiliza para comunicarse con el servidor de almacenamiento de archivos NFS. De forma predeterminada, todas las instancias de almacenamiento de archivos se configuran con la versión 4 de NFS. Puede cambiar su PV existente a una versión anterior de NFS si una app requiere una versión específica para funcionar correctamente. 
{: shortdesc}

Para cambiar la versión predeterminada de NFS, puede crear una nueva clase de almacenamiento para suministrar de forma dinámica el almacenamiento de archivos en su clúster o elegir cambiar un PV existente que esté montado en el pod. 

Para aplicar las últimas actualizaciones de seguridad y para un mejor rendimiento, utilice la versión de NFS predeterminada y no cambie a una versión anterior de NFS. 
{: tip}

**Para crear una clase de almacenamiento personalizada con la versión deseada de NFS: **
1. Cree un archivo yaml para su clase de almacenamiento personalizada. Sustituya <nfs_version> con la versión de NFS que desee utilizar. Por ejemplo, para suministrar NFS versión 3.0, especifique **3.0**.
   ```
   apiVersion: storage.k8s.io/v1
   kind: StorageClass
   metadata:
     name: ibmc-file-mount
     #annotations:
     #  storageclass.beta.kubernetes.io/is-default-class: "true"
     labels:
       kubernetes.io/cluster-service: "true"
   provisioner: ibm.io/ibmc-file
   parameters:
     type: "Endurance"
     iopsPerGB: "2"
     sizeRange: "[1-12000]Gi"
     reclaimPolicy: "Delete"
     classVersion: "2"
     mountOptions: nfsvers=<nfs_version>
   ```
   {: codeblock}
   
   Si desea mantener los datos después de eliminar el almacenamiento en bloques de su clúster, cambie `reclaimPolicy` a `Retain`. 
   {: tip}
   
2. Cree una clase de almacenamiento en su clúster. 
   ```
   kubectl apply -f <filepath/nfsversion_storageclass.yaml>
   ```
   {: pre}
       
3. Verifique que se haya creado la clase de almacenamiento personalizada. 
   ```
   kubectl get storageclasses
   ```
   {: pre}

4. Suministre [almacenamiento de archivos](#create) con su clase de almacenamiento personalizada. 

**Para hacer que el PV existente utilice una versión diferente NFS:**

1. Obtenga el PV del almacenamiento de archivos cuya versión de NFS desee cambiar y anote el nombre del PV.
   ```
   kubectl get pv
   ```
   {: pre}

2. Añada una anotación a su PV. Sustituya `<version_number>` con la versión de NFS que desee utilizar. Por ejemplo, para cambiar a NFS versión 3.0, especifique **3**.  
   ```
   kubectl patch pv <pv_name> -p '{"metadata": {"annotations":{"volume.beta.kubernetes.io/mount-options":"vers=<version_number>"}}}'
   ```
   {: pre}
   
3. Suprima el pod que utiliza el almacenamiento de archivos y vuelva a crear el pod. 
   1. Guarde el yaml del pod en su máquina local. 
      ```
      kubect get pod <pod_name> -o yaml > <filepath/pod.yaml>
      ```
      {: pre}
      
   2. Suprima el pod. 
      ```
      kubectl deleted pod <pod_name>
      ```
      {: pre}
   
   3. Vuelva a crear el pod. 
      ```
      kubectl apply -f <filepath/pod.yaml>
      ```
      {: pre}

4. Espere a que se haya desplegado el pod. 
   ```
   kubectl get pods
   ```
   {: pre}
   
   El pod quedará completamente desplegado cuando el estado cambie a `Running`. 

5. Inicie una sesión en el pod. 
   ```
   kubectl exec -it <pod_name> sh
   ```
   {: pre}
   
6. Verifique que el almacenamiento de archivos se ha montado con la versión de NFS que ha especificado anteriormente. 
   ```
   mount | grep "nfs" | awk -F" |," '{ print $5, $8 }'
   ```
   {: pre}
   
   Salida de ejemplo: 
   ```
   nfs vers=3.0
   ```
   {: screen}
   
<br />




## Instalación de la CLI de la infraestructura de IBM Cloud (SoftLayer)
{: #slcli}

Instale la CLI de la infraestructura de IBM Cloud (SoftLayer) para interactuar con los recursos de su infraestructura como, por ejemplo, las instancias de almacenamiento en bloques y archivos NFS.
{: shortdesc}

Antes empezar, [instale Python 3.6](https://www.python.org/downloads/).

1.  Consulte los [documentos de instalación ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://softlayer-api-python-client.readthedocs.io/en/latest/install/).

    1.  Pulse el enlace en los pasos **Descargar el tarball** o **Descargar el zipball**. No utilice el mandato `curl`.
    2.  Encuentre el paquete descargado, descomprímalo y vaya al directorio.
    3.  Instale la CLI.
    
        ```
        python3 setup.py install
        ```
        {: pre}
    
2.  Obtenga la clave y el nombre de usuario de la API de la infraestructura IBM Cloud (SoftLayer).

    1.  Desde la [consola de {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/), abra el menú y seleccione **Infraestructura**.
    2.  Desde su perfil en la barra de menús, seleccione la cuenta de infraestructura que desea utilizar.
    3.  Seleccione **Cuenta** > **Usuarios** > **Lista de usuarios**.
    4.  En la tabla **Usuarios**, en la columna **CLAVE DE API**, pulse **Visualizar**. Si no ve una clave de API, pulse **Generar**.
    5.  Copie el nombre de usuario y la clave de API de la ventana emergente.

3.  Configure la CLI para conectarse a la cuenta de infraestructura de IBM Cloud (SoftLayer).

    1.  Configure la CLI de infraestructura de IBM Cloud (SoftLayer).
        ```
        slcli setup
        ```
        {: pre}

    2.  Cumplimente la información necesaria.
    
        * **Nombre de usuario**: Especifique el nombre de usuario de la API de la infraestructura de IBM Cloud (SoftLayer) que recuperó con anterioridad.
        * **Contraseña o clave de API**: Especifique la clave de la API de la infraestructura de IBM Cloud (SoftLayer) que recuperó con anterioridad.
        * **Punto final (público|privado|personalizado) [público]**: Especifique `https://api.softlayer.com/rest/v3.1`.
        * **Tiempo de espera [0]**: Especifique los segundos que la CLI debe esperar para obtener una respuesta desde la API. Un valor de `0` establece que la CLI esperará de forma indefinida.
        
        **Ejemplo**:
        
        ```
        $ slcli setup
        Username []: 1234567_user.name@example.com
        API Key or Password []:         Endpoint (public|private|custom) [public]: https://api.softlayer.com/rest/v3.1 
        Timeout [0]: 6000
        :..............:..................................................................:
        :         name : value                                                            :
        :..............:..................................................................:
        :     Username : 1234567_user.name@example.com                                    :
        :      API Key : 1111aa1111bbb22222b2b3c33333c3c3cc44d4444444444dd4444eee55e5e5e5 :
        : Endpoint URL : https://api.softlayer.com/xmlrpc/v3.1/                           :
        :      Timeout : 6000                                                             :
        :..............:..................................................................:
        Are you sure you want to write settings to "/Users/name/.softlayer"? [Y/n]: Y
        Configuration Updated Successfully
        ```
        {: screen}

Ahora está listo para utilizar la CLI de la infraestructura de IBM Cloud (SoftLayer).

## Instalación del plugin {{site.data.keyword.Bluemix_notm}} Block Storage en su clúster
{: #install_block}

Instale el plugin {{site.data.keyword.Bluemix_notm}} Block Storage con un diagrama de Helm para configurar las clases de almacenamiento predefinidas para el almacenamiento en bloque. Utilice estas clases de almacenamiento para crear una PVC para suministrar almacenamiento en bloque para sus apps.
{: shortdesc}

Antes de empezar, seleccione [como destino de la CLI](cs_cli_install.html#cs_cli_configure) el clúster en el que desea instalar el plugin {{site.data.keyword.Bluemix_notm}} Block Storage.

1. Instale [Helm](cs_integrations.html#helm) en el clúster en el que desea utilizar el plugin {{site.data.keyword.Bluemix_notm}} Block Storage.
2. Actualice el repositorio de Helm para recuperar la última versión de todos los diagramas de Helm en este repositorio.
   ```
   helm repo update
   ```
   {: pre}

3. Instale el plugin {{site.data.keyword.Bluemix_notm}} Block Storage. Cuando instala el plugin, se añaden a su clúster las clases de almacenamiento de almacenamiento en bloque predefinidas.
   ```
   helm install ibm/ibmcloud-block-storage-plugin
   ```
   {: pre}

   Salida de ejemplo:
   ```
   NAME:   bald-olm
   LAST DEPLOYED: Wed Apr 18 10:02:55 2018
   NAMESPACE: default
   STATUS: DEPLOYED

   RESOURCES:
   ==> v1beta1/DaemonSet
   NAME                           DESIRED  CURRENT  READY  UP-TO-DATE  AVAILABLE  NODE SELECTOR  AGE
   ibmcloud-block-storage-driver  0        0        0      0           0          <none>         0s

   ==> v1beta1/Deployment
   NAME                           DESIRED  CURRENT  UP-TO-DATE  AVAILABLE  AGE
   ibmcloud-block-storage-plugin  1        0        0           0          0s

   ==> v1/StorageClass
   NAME                      PROVISIONER        AGE
   ibmc-block-bronze         ibm.io/ibmc-block  0s
   ibmc-block-custom         ibm.io/ibmc-block  0s
   ibmc-block-gold           ibm.io/ibmc-block  0s
   ibmc-block-retain-bronze  ibm.io/ibmc-block  0s
   ibmc-block-retain-custom  ibm.io/ibmc-block  0s
   ibmc-block-retain-gold    ibm.io/ibmc-block  0s
   ibmc-block-retain-silver  ibm.io/ibmc-block  0s
   ibmc-block-silver         ibm.io/ibmc-block  0s

   ==> v1/ServiceAccount
   NAME                           SECRETS  AGE
   ibmcloud-block-storage-plugin  1        0s

   ==> v1beta1/ClusterRole
   NAME                           AGE
   ibmcloud-block-storage-plugin  0s

   ==> v1beta1/ClusterRoleBinding
   NAME                           AGE
   ibmcloud-block-storage-plugin  0s

   NOTES:
   Thank you for installing: ibmcloud-block-storage-plugin.   Your release is named: bald-olm
   ```
   {: screen}

4. Verifique que la instalación ha sido satisfactoria.
   ```
   kubectl get pod -n kube-system | grep block
   ```
   {: pre}

   Salida de ejemplo:
   ```
   ibmcloud-block-storage-driver-kh4mt                              1/1       Running   0          27d       10.118.98.19   10.118.98.19
   ibmcloud-block-storage-plugin-58c5f9dc86-pbl4t                   1/1       Running   0          14d       172.21.0.204   10.118.98.19
   ```
   {: screen}
   
   La instalación es satisfactoria si ve un pod `ibmcloud-block-storage-plugin` y uno o varios pods `ibmcloud-block-storage-driver`. El número de `ibmcloud-block-storage-driver` corresponde al número de nodos trabajadores en su clúster. Todos los pods deben estar en un estado **Running**. 

5. Verifique que las clases de almacenamiento para el almacenamiento en bloque se añaden a su clúster.
   ```
   kubectl get storageclasses | grep block
   ```
   {: pre}

   Salida de ejemplo:
   ```
   ibmc-block-bronze            ibm.io/ibmc-block
   ibmc-block-custom            ibm.io/ibmc-block
   ibmc-block-gold              ibm.io/ibmc-block
   ibmc-block-retain-bronze     ibm.io/ibmc-block
   ibmc-block-retain-custom     ibm.io/ibmc-block
   ibmc-block-retain-gold       ibm.io/ibmc-block
   ibmc-block-retain-silver     ibm.io/ibmc-block
   ibmc-block-silver            ibm.io/ibmc-block
   ```
   {: screen}

6. Repita estos pasos para cada clúster donde desea suministrar almacenamiento en bloques.

Ahora puede seguir para [crear una PVC](#create) para suministrar almacenamiento en bloque a la app.

<br />


### Actualización del plugin {{site.data.keyword.Bluemix_notm}} Block Storage
Actualice el plugin {{site.data.keyword.Bluemix_notm}} Block Storage existente a la última versión.
{: shortdesc}

Antes de empezar, seleccione su clúster como [destino de la CLI](cs_cli_install.html#cs_cli_configure).

1. Busque el nombre del diagrama de helm de almacenamiento en bloque que instaló en su clúster.
   ```
   helm ls | grep ibmcloud-block-storage-plugin
   ```
   {: pre}

   Salida de ejemplo:
   ```
   myhelmchart 	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibmcloud-block-storage-plugin-0.1.0	default
   ```
   {: screen}

2. Actualice el plugin {{site.data.keyword.Bluemix_notm}} Block Storage a la última versión.
   ```
   helm upgrade --force --recreate-pods <helm_chart_name>  ibm/ibmcloud-block-storage-plugin
   ```
   {: pre}

<br />


### Eliminación del plugin {{site.data.keyword.Bluemix_notm}} Block Storage
Si no desea suministrar y utilizar {{site.data.keyword.Bluemix_notm}} Block Storage para el clúster, puede desinstalar el diagrama de Helm.
{: shortdesc}

**Nota:** Al eliminar el plugin no elimina los datos, las PVC o los PV. Cuando se elimina el plugin, se eliminan del clúster todos los conjuntos de daemons y pods relacionados. No puede suministrar nuevo almacenamiento en bloque para el clúster o utilizar PV o PVC de almacenamiento en bloque existente después de eliminar el plugin.

Antes de empezar, defina su clúster como [destino de la CLI](cs_cli_install.html#cs_cli_configure) y asegúrese de que no hay ninguna PVC o ningún PV en el clúster que utilice almacenamiento en bloque.

1. Busque el nombre del diagrama de helm de almacenamiento en bloque que instaló en su clúster.
   ```
   helm ls | grep ibmcloud-block-storage-plugin
   ```
   {: pre}

   Salida de ejemplo:
   ```
   myhelmchart 	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibmcloud-block-storage-plugin-0.1.0	default
   ```
   {: screen}

2. Suprima el plugin {{site.data.keyword.Bluemix_notm}} Block Storage.
   ```
   helm delete <helm_chart_name>
   ```
   {: pre}

3. Verifique que se hayan eliminado los pods de almacenamiento en bloque.
   ```
   kubectl get pod -n kube-system | grep ibmcloud-block-storage-plugin
   ```
   {: pre}
   La eliminación de los pods es satisfactoria si dejan de aparecer en la salida de la CLI.

4. Verifique que se hayan eliminado las clases de almacenamiento en bloque.
   ```
   kubectl get storageclasses | grep block
   ```
   {: pre}
   La eliminación de las clases de almacenamiento es satisfactoria si dejan de aparecer en la salida de la CLI.

<br />



## Configuración de soluciones de copia de seguridad y restauración de comparticiones de archivos NFS y almacenamiento en bloque
{: #backup_restore}

Las comparticiones de archivos y el almacenamiento en bloque se suministran en la misma ubicación que su clúster. {{site.data.keyword.IBM_notm}} aloja el almacenamiento en servidores en clúster para proporcionar disponibilidad en el caso de que el servidor deje de funcionar. Sin embargo, no se realizan automáticamente copias de seguridad de las comparticiones de archivo ni del almacenamiento en bloque, y pueden no ser accesibles si falla toda la ubicación. Para evitar que los datos que se pierdan o se dañen, puede configurar copias de seguridad periódicas que puede utilizar para restaurar los datos cuando sea necesario.
{: shortdesc}

Consulte las opciones siguientes de copia de seguridad y restauración para las comparticiones de archivo NFS y el almacenamiento en bloque:

<dl>
  <dt>Configurar instantáneas periódicas</dt>
  <dd><p>Configure instantáneas periódicas para su almacenamiento en bloque o de compartición de archivos NFS. Se trata de imágenes de solo lectura que capturan el estado de la instancia en un instante dado. Para almacenar la instantánea, debe solicitar espacio de instantáneas en el almacenamiento en bloques o en el almacenamiento compartido de archivos NFS. Las instantáneas se almacenan en la instancia de almacenamiento existente dentro de la misma ubicación. Puede restaurar datos desde una instantánea si un usuario elimina accidentalmente datos importantes del volumen. </br></br> <strong>Para crear una instantánea para su volumen: </strong><ol><li>Liste los PV en su clúster. <pre class="pre"><code>kubectl get pv</code></pre></li><li>Obtenga los detalles de los PV para los que desea crear espacio de instantáneas y anote el ID de volumen, el tamaño y las IOPS. <pre class="pre"><code>kubectl describe pv &lt;pv_name&gt;</code></pre> Para almacenamiento de archivos, el ID de volumen, el tamaño y las IOPS puede encontrarse en la sección <strong>Labels</strong> de la salida de la CLI. Para el almacenamiento de bloques, el tamaño y las IOPS se muestran en la sección <strong>Labels</strong> de la salida de la CLI. Para encontrar el ID de volumen, revise la anotación <code>ibm.io/network-storage-id</code> de la salida de la CLI. </li><li>Cree el tamaño de instantánea para el volumen existente con los parámetros que ha recuperado en el paso anterior. <pre class="pre"><code>slcli file snapshot-order --capacity &lt;size&gt; --tier &lt;iops&gt; &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-order --capacity &lt;size&gt; --tier &lt;iops&gt; &lt;volume_id&gt;</code></pre></li><li>Espere a que se haya creado el tamaño de la instantánea. <pre class="pre"><code>slcli file volume-detail &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block volume-detail &lt;volume_id&gt;</code></pre>El tamaño de la instantánea se suministra de forma correcta cuando la <strong>Snapshot Capacity (GB)</strong> en la salida de la CLI cambia de 0 al tamaño solicitado. </li><li>Cree la instantánea para el volumen y anote el ID de la instantánea que se crea para usted. <pre class="pre"><code>slcli file snapshot-create &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-create &lt;volume_id&gt;</code></pre></li><li>Verifique que la instantánea se haya creado correctamente. <pre class="pre"><code>slcli file volume-detail &lt;snapshot_id&gt;</code></pre><pre class="pre"><code>slcli block volume-detail &lt;snapshot_id&gt;</code></pre></li></ol></br><strong>Para restaurar los datos desde una instantánea en un volumen existente: </strong><pre class="pre"><code>slcli file snapshot-restore -s &lt;snapshot_id&gt; &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-restore -s &lt;snapshot_id&gt; &lt;volume_id&gt;</code></pre></br>Para obtener más información, consulte:<ul><li>[Instantáneas periódicas NFS](/docs/infrastructure/FileStorage/snapshots.html)</li><li>[Instantáneas periódicas en bloque](/docs/infrastructure/BlockStorage/snapshots.html#snapshots)</li></ul></p></dd>
  <dt>Replicar instantáneas en otra ubicación</dt>
 <dd><p>Para proteger los datos ante un error de ubicación, puede [replicar instantáneas](/docs/infrastructure/FileStorage/replication.html#working-with-replication) a una compartición de archivos NFS o un almacenamiento en bloque que configurados en otra ubicación. Los datos únicamente se pueden replicar desde el almacenamiento primario al almacenamiento de copia de seguridad. No puede montar una instancia de almacenamiento en bloque o compartición de archivos NFS replicados en un clúster. Cuando el almacenamiento primario falla, puede establecer de forma manual el almacenamiento de copia de seguridad replicado para que sea el primario. A continuación, puede montarla en el clúster. Una vez restaurado el almacenamiento primario, puede restaurar los datos del almacenamiento de copia de seguridad.</p>
 <p>Para obtener más información, consulte:<ul><li>[Replicar instantáneas para NFS](/docs/infrastructure/FileStorage/replication.html#working-with-replication)</li><li>[Replicar instantáneas para Block](/docs/infrastructure/BlockStorage/replication.html#working-with-replication)</li></ul></p></dd>
 <dt>Duplicar almacenamiento</dt>
 <dd><p>La instancia de almacenamiento en bloque o compartición de archivos NFS se puede duplicar en la misma ubicación que la instancia de almacenamiento original. La instancia duplicada tiene los mismos datos que la instancia de almacenamiento original en el momento de duplicarla. A diferencia de las réplicas, utilice los duplicados como una instancia de almacenamiento independiente de la original. Para duplicar, primero configure instantáneas para el volumen.</p>
 <p>Para obtener más información, consulte:<ul><li>[Instantáneas duplicadas NFS](/docs/infrastructure/FileStorage/how-to-create-duplicate-volume.html#creating-a-duplicate-file-storage)</li><li>[Instantáneas duplicadas de bloque](/docs/infrastructure/BlockStorage/how-to-create-duplicate-volume.html#creating-a-duplicate-block-volume)</li></ul></p></dd>
  <dt>Copia de seguridad de datos a Object Storage</dt>
  <dd><p>Puede utilizar la [**imagen ibm-backup-restore**](/docs/services/RegistryImages/ibm-backup-restore/index.html#ibmbackup_restore_starter) para utilizar un pod de copia de seguridad y restauración en el clúster. Este pod contiene un script para ejecutar una copia de seguridad puntual o periódico para cualquier reclamación de volumen permanente (PVC) en el clúster. Los datos se almacenan en la instancia de {{site.data.keyword.objectstoragefull}} que ha configurado en una ubicación.</p>
  <p>Para aumentar la alta disponibilidad de los datos y proteger la app ante un error de ubicación, configure una segunda instancia de {{site.data.keyword.objectstoragefull}} y replique los datos en las ubicaciones. Si necesita restaurar datos desde la instancia de {{site.data.keyword.objectstoragefull}}, utilice el script de restauración que se proporciona con la imagen.</p></dd>
<dt>Copiar datos a y desde pods y contenedores</dt>
<dd><p>Utilice el [mandato ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/reference/kubectl/overview/#cp) `kubectl cp` para copiar archivos y directorios a y desde pods o contenedores específicos en el clúster.</p>
<p>Antes de empezar, establezca como [destino de su CLI de Kubernetes](cs_cli_install.html#cs_cli_configure) el clúster que desea utilizar. Si no especifica un contenedor con <code>-c</code>, el mandato utiliza el primer contenedor disponible en el pod.</p>
<p>El mandato se puede utilizar de varias maneras:</p>
<ul>
<li>Copiar datos desde su máquina local a un pod en su clúster: <pre class="pre"><code>kubectl cp <var>&lt;local_filepath&gt;/&lt;filename&gt;</var> <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var></code></pre></li>
<li>Copiar datos desde un pod en su clúster a su máquina local: <pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;/&lt;filename&gt;</var> <var>&lt;local_filepath&gt;/&lt;filename&gt;</var></code></pre></li>
<li>Copiar datos desde un pod en su clúster a un contenedor específico en otro pod: <pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var> <var>&lt;namespace&gt;/&lt;other_pod&gt;:&lt;pod_filepath&gt;</var> -c <var>&lt;container></var></code></pre></li>
</ul></dd>
  </dl>


