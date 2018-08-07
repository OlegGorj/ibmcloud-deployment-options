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



# Resolución de problemas del almacenamiento del clúster
{: #cs_troubleshoot_storage}

Si utiliza {{site.data.keyword.containerlong}}, tenga en cuenta estas técnicas para solucionar problemas relacionados con el almacenamiento del clúster.
{: shortdesc}

Si tiene un problema más general, pruebe la [depuración del clúster](cs_troubleshoot.html).
{: tip}



## Los sistemas de archivos de los nodos trabajadores pasan a ser de sólo lectura
{: #readonly_nodes}

{: tsSymptoms}
{: #stuck_creating_state}
Puede ver uno de los siguientes síntomas:
- Cuando ejecuta `kubectl get pods -o wide`, ve que varios pods que se están ejecutando en el mismo nodo trabajador quedan bloqueados en el estado `ContainerCreating`.
- Cuando se ejecuta un mandato `kubectl describe`, ve el siguiente error en la sección **Events**: `MountVolume.SetUp failed for volume ... read-only file system`.

{: tsCauses}
El sistema de archivos del nodo trabajador es de sólo lectura.

{: tsResolve}
1.  Haga una copia de seguridad de los datos que puedan estar almacenados en el nodo trabajador o en los contenedores.
2.  Para un arreglo a corto plazo para el nodo trabajador existente, recargue el nodo trabajador.
    <pre class="pre"><code>bx cs worker-reload &lt;cluster_name&gt; &lt;worker_ID&gt;</code></pre>

Para un arreglo a largo plazo, [actualice el tipo de máquina añadiendo otro nodo trabajador](cs_cluster_update.html#machine_type).

<br />



## La app falla cuando un usuario no root es el propietario de la vía de acceso de montaje del almacenamiento de archivos NFS
{: #nonroot}

{: tsSymptoms}
Después de [añadir almacenamiento NFS](cs_storage.html#app_volume_mount) a su despliegue, el despliegue de su contenedor falla. Al recuperar los registros del contenedor, podría ver errores como, "write-permission" o "do not have required permission". El pod falla y queda atascado en un ciclo de recarga.

{: tsCauses}
De forma predeterminada, los usuarios no root no tienen permiso de escritura sobre la vía de acceso de montaje del volumen para el almacenamiento respaldado por NFS. Algunas imágenes comunes de app, como por ejemplo Jenkins y Nexus3, especifican un usuario no root que posee la vía de acceso de montaje en Dockerfile. Cuando se crea un contenedor desde este Dockerfile, la creación del contenedor falla debido a permisos insuficientes del usuario no root en la vía de acceso de montaje. Para otorgar permiso de escritura, puede modificar el Dockerfile para añadir temporalmente el usuario no root al grupo de usuarios root antes de cambiar los permisos de la vía de acceso de montaje, o utilizar un contenedor de inicialización.

Si utiliza un diagrama de Helm para desplegar la imagen, edite el despliegue de Helm para utilizar un contenedor de inicialización.
{:tip}



{: tsResolve}
Cuando se incluye un [contenedor de inicialización ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) en el despliegue, puede dar a un usuario no root especificado en el Dockerfile permisos de escritura para la vía de acceso de montaje del volumen dentro del contenedor. El contenedor de inicialización de inicia antes que el contenedor de la app. El contenedor de inicialización crea la vía de acceso de montaje del volumen dentro del contenedor, cambia la propiedad de la vía de acceso de montaje al usuario (no root) correcto y se cierra. A continuación, se inicia el contenedor de la app con el usuario no root que debe escribir en la vía de acceso de montaje. Dado que la vía de acceso ya es propiedad del usuario no root, la escritura en la vía de acceso de montaje se realiza correctamente. Si no desea utilizar un contenedor de inicialización, puede modificar el Dockerfile para añadir acceso de usuario no root al almacenamiento de archivos NFS.


Antes de empezar, seleccione su clúster como [destino de la CLI](cs_cli_install.html#cs_cli_configure).

1.  Abra el Dockerfile para la app y obtenga el ID de usuario (UID) y el ID de grupo (GID) del usuario al que desea dar permiso de escritura en la vía de acceso de montaje del volumen. En el ejemplo de un Dockerfile de Jenkins, la información es la siguiente:
    - UID: `1000`
    - GID: `1000`

    **Ejemplo**:

    ```
    FROM openjdk:8-jdk

    RUN apt-get update &&apt-get install -y git curl &&rm -rf /var/lib/apt/lists/*

    ARG user=jenkins
    ARG group=jenkins
    ARG uid=1000
    ARG gid=1000
    ARG http_port=8080
    ARG agent_port=50000

    ENV JENKINS_HOME /var/jenkins_home
    ENV JENKINS_SLAVE_AGENT_PORT ${agent_port}
    ...
    ```
    {:screen}

2.  Añade almacenamiento persistente a la app creando una reclamación de volumen permanente (PVC). En este ejemplo se utiliza la clase de almacenamiento `ibmc-file-bronze`. Para revisar las clases de almacenamiento disponibles, ejecute `kubectl get storageclasses`.

    ```
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: mypvc
      annotations:
        volume.beta.kubernetes.io/storage-class: "ibmc-file-bronze"
    spec:
      accessModes:
        - ReadWriteMany
      resources:
        requests:
          storage: 20Gi
    ```
    {: codeblock}

3.  Cree la PVC.

    ```
    kubectl apply -f mypvc.yaml
    ```
    {: pre}

4.  En el archivo `.yaml` de despliegue, añada el contenedor de inicialización. Incluya el UID y el GID que ha recuperado anteriormente.

    ```
    initContainers:
    - name: initContainer # Or you can replace with any name
      image: alpine:latest
      command: ["/bin/sh", "-c"]
      args:
        - chown <UID>:<GID> /mount; # Replace UID and GID with values from the Dockerfile
      volumeMounts:
      - name: volume # Or you can replace with any name
        mountPath: /mount # Must match the mount path in the args line
    ```
    {: codeblock}

    **Ejemplo** para un despliegue de Jenkins:

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: my_pod
    spec:
      replicas: 1
      template:
        metadata:
          labels:
            app: jenkins
        spec:
          containers:
          - name: jenkins
            image: jenkins
            volumeMounts:
            - mountPath: /var/jenkins_home
              name: volume
          volumes:
          - name: volume
            persistentVolumeClaim:
              claimName: mypvc
          initContainers:
          - name: permissionsfix
            image: alpine:latest
            command: ["/bin/sh", "-c"]
            args:
              - chown 1000:1000 /mount;
            volumeMounts:
            - name: volume
              mountPath: /mount
    ```
    {: codeblock}

5.  Cree el pod y monte la PVC en el pod.

    ```
    kubectl apply -f my_pod.yaml
    ```
    {: pre}

6. Verifique que el volumen se ha montado correctamente en el pod. Anote el nombre de pod y la vía de acceso de **Containers/Mounts**.

    ```
    kubectl describe pod <my_pod>
    ```
    {: pre}

    **Salida de ejemplo**:

    ```
    Name:		    mypod-123456789
    Namespace:	default
    ...
    Init Containers:
    ...
    Mounts:
      /mount from volume (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-cp9f0 (ro)
    ...
    Containers:
      jenkins:
        Container ID:
        Image:		jenkins
        Image ID:
        Port:		  <none>
        State:		Waiting
          Reason:		PodInitializing
        Ready:		False
        Restart Count:	0
        Environment:	<none>
        Mounts:
          /var/jenkins_home from volume (rw)
          /var/run/secrets/kubernetes.io/serviceaccount from default-token-cp9f0 (ro)
    ...
    Volumes:
      myvol:
        Type: PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
        ClaimName: mypvc
        ReadOnly: false

    ```
    {: screen}

7.  Inicie la sesión en el pod utilizando el nombre de pod que ha anotado anteriormente.

    ```
    kubectl exec -it <my_pod-123456789> /bin/bash
    ```
    {: pre}

8. Verifique los permisos de la vía de acceso de montaje del contenedor. En el ejemplo, la vía de acceso de montaje es `/var/jenkins_home`.

    ```
    ls -ln /var/jenkins_home
    ```
    {: pre}

    **Salida de ejemplo**:

    ```
    jenkins@mypod-123456789:/$ ls -ln /var/jenkins_home
    total 12
    -rw-r--r-- 1 1000 1000  102 Mar  9 19:58 copy_reference_file.log
    drwxr-xr-x 2 1000 1000 4096 Mar  9 19:58 init.groovy.d
    drwxr-xr-x 9 1000 1000 4096 Mar  9 20:16 war
    ```
    {: screen}

    Esta salida muestra que el GID y el UID del Dockerfile (en este ejemplo, `1000` y `1000`) poseen la vía de acceso de montaje dentro del contenedor.

<br />


## La adición de acceso de usuario no root al almacenamiento permanente no se realiza correctamente
{: #cs_storage_nonroot}

{: tsSymptoms}
Después de [añadir acceso de usuario no root al almacenamiento permanente](#nonroot) o desplegar un diagrama de Helm con un ID de usuario no root especificado, el usuario no puede grabar en el almacenamiento montado.

{: tsCauses}
En la configuración del diagrama de Helm o el despliegue se especifica el [contexto de seguridad](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/) para el ID de grupo (`fsGroup`) y el ID de usuario (`runAsUser`) del pod. Actualmente, {{site.data.keyword.containershort_notm}} no soporta la especificación de `fsGroup` y sólo soporta que `runAsUser` se establezca como `0` (permisos root).

{: tsResolve}
Elimine los campos `securityContext` de la configuración para `fsGroup` y `runAsUser` de la imagen, despliegue o archivo de configuración de diagrama de Helm y vuelva a desplegar. Si tiene que cambiar la propiedad de la vía de acceso de montaje de `nobody`, [añada acceso de usuario non-root](#nonroot). Después de añadir [non-root initContainer](#nonroot), establezca `runAsUser` al nivel del contenedor, no a nivel de pod.

<br />




## El montaje de almacenamiento en bloques existente a un pod falla debido a un sistema de archivos erróneo
{: #block_filesystem}

{: tsSymptoms}
Cuando ejecuta `kubectl describe pod <pod_name>`, ve el siguiente error:
```
failed to mount the volume as "ext4", it already contains xfs. Mount error: mount failed: exit status 32
```
{: screen}

{: tsCauses}
Puede tener un dispositivo de almacenamiento en bloques configurado con un sistema de archivos `XFS`. Para montar este dispositivo en el pod, [creó un PV](cs_storage.html#existing_block) que especificaba `ext4` como su sistema de archivos o bien no se especificaba ningún sistema de archivos en la sección `spec/flexVolume/fsType`. Si no se define un sistema de archivos, el valor predeterminado para el PV es `ext4`.
El PV se creó satisfactoriamente y se enlazó a la instancia de almacenamiento en bloques existente. Sin embargo, cuando se intentó montar el PV a su clúster mediante una PVC coincidente, el volumen no se pudo montar. No se puede montar una instancia de almacenamiento en bloques `XFS` con un sistema de archivos `ext4` en el pod.

{: tsResolve}
Actualice el sistema de archivos en el PV existente de `ext4` a `XFS`.

1. Liste los PV existentes en el clúster y anote el nombre del VF que ha utilizado para la instancia de almacenamiento en bloques existente.
   ```
   kubectl get pv
   ```
   {: pre}

2. Guarde el yaml del PV en su máquina local.
   ```
   kubectl get pv <pv_name> -o yaml > <filepath/xfs_pv.yaml>
   ```
   {: pre}

3. Abra el archivo yaml y cambie el `fsType` de `ext4` a `xfs`.
4. Sustituya el PV en el clúster.
   ```
   kubectl replace --force -f <filepath/xfs_pv.yaml>
   ```
   {: pre}

5. Inicie una sesión en el pod en el que ha montado el PV.
   ```
   kubectl exec -it <pod_name> sh
   ```
   {: pre}

6. Verifique el sistema de archivos ha cambiado a `XFS`.
   ```
   df -Th
   ```
   {: pre}

   Salida de ejemplo:
   ```
   Filesystem Type Size Used Avail Use% Mounted on /dev/mapper/3600a098031234546d5d4c9876654e35 xfs 20G 33M 20G 1% /myvolumepath
   ```
   {: screen}

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

