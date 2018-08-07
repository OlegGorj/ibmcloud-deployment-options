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




# Compilación de contenedores a partir de imágenes
{: #images}

Una imagen de Docker es la base para cada contenedor que pueda crear con {{site.data.keyword.containerlong}}.
{:shortdesc}

Se crea una imagen a partir de Dockerfile, que es un archivo que contiene instrucciones para crear la imagen. Un Dockerfile puede hacer referencia a artefactos de compilación en sus instrucciones que se almacenan por separado, como por ejemplo una app, la configuración de la app y sus dependencias.

## Planificación de registros de imagen
{: #planning}

Las imágenes normalmente se almacenan en un registro que puede ser tanto de acceso público (registro público) como estar configurado con acceso limitado para un pequeño grupo de usuarios (registro privado).
{:shortdesc}

Los registros públicos, como por ejemplo Docker Hub, se pueden utilizar para empezar a trabajar con Docker y Kubernetes para crear la primera app contenerizada de un clúster. Pero, cuando se trate de aplicaciones de empresa, utilice un registro privado, como el que se suministra en {{site.data.keyword.registryshort_notm}}, para proteger sus imágenes frente a un posible uso y modificación por parte de usuarios no autorizados. El administrador del clúster debe configurar los registros privados para asegurarse de que las credenciales para acceder al registro privado están disponibles para los usuarios del clúster.


Puede utilizar varios registros con {{site.data.keyword.containershort_notm}} para desplegar apps en el clúster.

|Registro|Descripción|Ventaja|
|--------|-----------|-------|
|[{{site.data.keyword.registryshort_notm}}](/docs/services/Registry/index.html)|Con esta opción, puede configurar su propio repositorio seguro de imágenes de Docker en {{site.data.keyword.registryshort_notm}} donde puede almacenar y compartir de forma segura imágenes entre los usuarios del clúster.|<ul><li>Gestione el acceso a las imágenes de la cuenta.</li><li>Utilice las apps de ejemplo y las imágenes proporcionadas por {{site.data.keyword.IBM_notm}}, como {{site.data.keyword.IBM_notm}} Liberty, como imagen padre y añádales su propio código de app.</li><li>Exploración automática de imágenes en busca de vulnerabilidades potenciales por parte de Vulnerability Advisor, que incluyen recomendaciones específicas del sistema operativo para solucionarlas.</li></ul>|
|Cualquier otro registro privado|Para conectar cualquier registro privado existente a su clúster, cree un [imagePullSecret ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/containers/images/). El secreto se utiliza para guardar el URL del registro y las credenciales de forma segura en un secreto de Kubernetes.|<ul><li>Utilice registros privados existentes independientemente de su origen (Docker Hub, registros propiedad de la organización u otros registros privados de Cloud).</li></ul>|
|[Public Docker Hub![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://hub.docker.com/){: #dockerhub}|Utilice esta opción para utilizar directamente imágenes públicas existentes en Docker Hub en sus [despliegues de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) cuando no sean necesarios cambios de Dockerfile. <p>**Nota:** Tenga en cuenta que es posible que esta opción no se ajuste a los requisitos de seguridad de su organización, como la gestión de accesos, la exploración de vulnerabilidades o la privacidad de las apps.</p>|<ul><li>No se requiere configuración adicional para el clúster.</li><li>Incluye diversas aplicaciones de código abierto.</li></ul>|
{: caption="Opciones de registro de imágenes público y privado" caption-side="top"}

Después de configurar el registro de imágenes, los usuarios del clúster pueden utilizar las imágenes de sus despliegues de apps en el clúster.

Obtenga más información sobre cómo [proteger su información personal](cs_secure.html#pi) cuando se trabaja con imágenes de contenedor.

<br />


## Configuración de contenido de confianza para imágenes de contenedor
{: #trusted_images}

Puede crear contenedores a partir de imágenes de confianza que están firmadas y almacenadas en {{site.data.keyword.registryshort_notm}}, y evitar despliegues de imágenes vulnerables o que no hayan sido firmadas.
{:shortdesc}

1.  [Firme imágenes para un contenido de confianza](/docs/services/Registry/registry_trusted_content.html#registry_trustedcontent). Después de establecer la confianza en sus imágenes, puede gestionar el contenido de confianza y los firmantes que pueden extraer las imágenes para su registro.
2.  Para imponer una política en la que únicamente se pueden utilizar imágenes firmadas para crear contenedores en su clúster, [añada Container Image Security Enforcement (beta)](/docs/services/Registry/registry_security_enforce.html#security_enforce).
3.  Despliegue la app.
    1. [Despliegue en el espacio de nombres de Kubernetes `default`](#namespace).
    2. [Despliegue en un espacio de nombres de Kubernetes diferente, o desde una cuenta o región diferente de {{site.data.keyword.Bluemix_notm}}](#other).

<br />


## Despliegue de contenedores desde una imagen de {{site.data.keyword.registryshort_notm}} en el espacio de nombres de Kubernetes `default`
{: #namespace}

Puede desplegar contenedores en su clúster desde una imagen pública proporcionada por IBM o desde una imagen privada almacenada en el espacio de nombres de {{site.data.keyword.registryshort_notm}}.
{:shortdesc}

Cuando crea un clúster, se crean automáticamente señales y secretos tanto para [el registro regional más cercano y el registro global](/docs/services/Registry/registry_overview.html#registry_regions). El registro global almacena de forma segura imágenes públicas proporcionadas por IBM a las que puede hacer referencia en sus despliegues en lugar de tener referencias distintas para las imágenes que se almacenan en cada registro regional. El registro regional almacena de forma segura sus imágenes de Docker privadas así como las mismas imágenes públicas que se almacenan en el registro global. Las señales se utilizan para autorizar el acceso de solo lectura a cualquiera de los espacios de nombres que configure en {{site.data.keyword.registryshort_notm}} para que pueda trabajar con estas imágenes públicas (registro global) y privadas (registros regionales).

Cada señal se debe guardar en `imagePullSecret` de Kubernetes para que resulte accesible para un clúster de Kubernetes cuando se despliegue una app contenerizada. Cuando se crea el clúster, {{site.data.keyword.containershort_notm}} almacena automáticamente las señales para los registros global (imágenes públicas proporcionadas por IBM) y regionales en los secretos de extracción de imágenes de Kubernetes. Los secretos de extracción de imágenes se añaden al espacio de nombres `predeterminado` de Kubernetes, la lista predeterminada de secretos en la `ServiceAccount` de dicho espacio de nombres y el espacio de nombres `kube-system`.

**Nota:** Con esta configuración inicial, puede desplegar contenedores desde cualquier imagen disponible en un espacio de nombres de la cuenta de {{site.data.keyword.Bluemix_notm}} en el espacio de nombres **default** del clúster. Para desplegar un contenedor en otros espacios de nombres del clúster, o utilizar una imagen almacenada en otra región de {{site.data.keyword.Bluemix_notm}} o en otra cuenta de {{site.data.keyword.Bluemix_notm}}, debe [crear su propio imagePullSecret para el clúster](#other).

Antes de empezar:
1. [Configure un espacio de nombres en {{site.data.keyword.registryshort_notm}} en {{site.data.keyword.Bluemix_notm}} Público o {{site.data.keyword.Bluemix_dedicated_notm}} y envíe por push imágenes a este espacio de nombres](/docs/services/Registry/registry_setup_cli_namespace.html#registry_namespace_add).
2. [Cree un clúster](cs_clusters.html#clusters_cli).
3. [Defina su clúster como destino de la CLI](cs_cli_install.html#cs_cli_configure).

Para desplegar un contenedor en el espacio de nombres **default** del clúster, cree un file de configuración.

1.  Cree un archivo de configuración de despliegue denominado `mydeployment.yaml`.
2.  Defina el despliegue y la imagen que desee utilizar del espacio de nombres de {{site.data.keyword.registryshort_notm}}.

    Para utilizar una imagen privada de un espacio de nombres de {{site.data.keyword.registryshort_notm}}:

    ```
    apiVersion: apps/v1beta1
    kind: Deployment
    metadata:
      name: ibmliberty-deployment
    spec:
      replicas: 3
      template:
        metadata:
          labels:
            app: ibmliberty
        spec:
          containers:
          - name: ibmliberty
            image: registry.<region>.bluemix.net/<namespace>/<my_image>:<tag>
    ```
    {: codeblock}

    **Sugerencia:** Para recuperar la información del espacio de nombres, ejecute `bx cr namespace-list`.

3.  Cree el despliegue en el clúster.

    ```
    kubectl apply -f mydeployment.yaml
    ```
    {: pre}

    **Sugerencia:** También puede desplegar un archivo de configuración existente, como por ejemplo una de las imágenes públicas proporcionadas por IBM. En este ejemplo se utiliza la imagen **ibmliberty** de la región EE.UU.-Sur.

    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/deploy-ibmliberty.yaml
    ```
    {: pre}


<br />



## Creación de un `imagePullSecret` para acceder a {{site.data.keyword.Bluemix_notm}} o registros privados en otros espacios de nombres de Kubernetes, cuentas y regiones de {{site.data.keyword.Bluemix_notm}}
{: #other}

Cree sus propios `imagePullSecret` para desplegar contenedores para otros espacios de nombres de Kubernetes, utilice imágenes almacenadas en otras cuentas o regiones de {{site.data.keyword.Bluemix_notm}}, utilice imágenes almacenadas en {{site.data.keyword.Bluemix_dedicated_notm}} o utilice imágenes almacenadas en registros privados externos.
{:shortdesc}

Los ImagePullSecrets solo son válidos para espacios de nombres de Kubernetes para los que fueron creados. Repita estos pasos para cada espacio de nombres en el que desee desplegar contenedores. Las imágenes de [DockerHub](#dockerhub) no necesitan ImagePullSecrets.
{: tip}

Antes de empezar:

1.  [Configure un espacio de nombres en {{site.data.keyword.registryshort_notm}} en {{site.data.keyword.Bluemix_notm}} Público o {{site.data.keyword.Bluemix_dedicated_notm}} y envíe por push imágenes a este espacio de nombres](/docs/services/Registry/registry_setup_cli_namespace.html#registry_namespace_add).
2.  [Cree un clúster](cs_clusters.html#clusters_cli).
3.  [Defina su clúster como destino de la CLI](cs_cli_install.html#cs_cli_configure).

<br/>
Al crear su propio imagePullSecret puede elegir entre las siguientes opciones:
- [Copiar el imagePullSecret desde el espacio de nombres predeterminado a otros espacios de nombres en su clúster](#copy_imagePullSecret).
- [Crear un imagePullSecret para acceder a imágenes en otras cuentas y regiones de {{site.data.keyword.Bluemix_notm}}](#other_regions_accounts).
- [Crear un imagePullSecret para acceder a imágenes en registros privados externos](#private_images).

<br/>
Si ya creó un imagePullSecret en su espacio de nombres que desea utilizar en su despliegue, consulte [Despliegue de contenedores utilizando el imagePullSecret creado](#use_imagePullSecret).

### Copia del imagePullSecret desde el espacio de nombres predeterminado a otros espacios de nombres en el clúster
{: #copy_imagePullSecret}

Puede copiar el imagePullSecret que se crea automáticamente para el espacio de nombres de Kubernetes `default` en otros espacios de nombres en el clúster.
{: shortdesc}

1. Obtenga una lista de los espacios de nombres disponibles en el clúster.
   ```
   kubectl get namespaces
   ```
   {: pre}

   Salida de ejemplo:
   ```
   default          Active    79d
   ibm-cert-store   Active    79d
   ibm-system       Active    79d
   istio-system     Active    34d
   kube-public      Active    79d
   kube-system      Active    79d
   ```
   {: screen}

2. Opcional: Cree un espacio de nombres en el clúster.
   ```
   kubectl create namespace <namespace_name>
   ```
   {: pre}

3. Copie imagePullSecrets desde el espacio de nombres `default` en el espacio de nombres que elija. El nuevo imagePullSecrets se denomina `bluemix-<namespace_name>-secret-regional` y `bluemix-<namespace_name>-secret-international`.
   ```
   kubectl get secret bluemix-default-secret-regional -o yaml | sed 's/default/<namespace_name>/g' | kubectl -n <namespace_name> create -f -
   ```
   {: pre}
   
   ```
   kubectl get secret bluemix-default-secret-international -o yaml | sed 's/default/<namespace_name>/g' | kubectl -n <namespace_name> create -f -
   ```
   {: pre}

4.  Verifique que el secreto se haya creado correctamente.
    ```
    kubectl get secrets --namespace <namespace_name>
    ```
    {: pre}

5. [Despliegue un contenedor utilizando el imagePullSecret](#use_imagePullSecret) en su espacio de nombres.


### Creación de un imagePullSecret para acceder a imágenes en otras cuentas y regiones de {{site.data.keyword.Bluemix_notm}}
{: #other_regions_accounts}

Para acceder a imágenes en otras regiones o cuentas de {{site.data.keyword.Bluemix_notm}}, debe crear una señal de registro y guardar sus credenciales en un imagePullSecret.
{: shortdesc}

1.  Si no tiene una señal, [cree una señal para el registro al que desea acceder.](/docs/services/Registry/registry_tokens.html#registry_tokens_create)
2.  Obtenga una lista de las señales en la cuenta de {{site.data.keyword.Bluemix_notm}}.

    ```
    bx cr token-list
    ```
    {: pre}

3.  Anote el ID de la señal que desea utilizar.
4.  Recupere el valor de la señal. Sustituya <em>&lt;token_ID&gt;</em> por el ID de la señal que ha recuperado en el paso anterior.

    ```
    bx cr token-get <token_id>
    ```
    {: pre}

    El valor de la señal se muestra en el campo **Token** de la salida de la CLI.

5.  Cree el secreto de Kubernetes para almacenar la información de la señal.

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_URL> --docker-username=token --docker-password=<token_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>Descripción de los componentes de este mandato</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Descripción de los componentes de este mandato</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>Obligatorio. El espacio de nombres de Kubernetes del clúster en el que desea utilizar el secreto y desplegar los contenedores. Ejecute <code>kubectl get namespaces</code> para obtener una lista de todos los espacios de nombres del clúster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>Obligatorio. El nombre que desea utilizar para su imagePullSecret.</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>Obligatorio. El URL del registro de imágenes en el que está configurado el espacio de nombres.<ul><li>Para los espacios de nombres definidos en EE.UU. Sur y EE.UU. este en registry.ng.bluemix.net</li><li>Para los espacios definidos en UK-Sur registry.eu-gb.bluemix.net</li><li>Para los espacios de nombres definidos en UE-Central (Frankfurt) registry.eu-de.bluemix.net</li><li>Para los espacios de nombres definidos en Australia (Sidney) registry.au-syd.bluemix.net</li><li>Para los espacios definidos en {{site.data.keyword.Bluemix_dedicated_notm}} registry.<em>&lt;dedicated_domain&gt;</em></li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>Obligatorio. El nombre de usuario para iniciar una sesión en su registro privado. Para {{site.data.keyword.registryshort_notm}}, el nombre de usuario se establece en <code>token</code>.</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>Obligatorio. El valor de la señal de registro que ha recuperado anteriormente.</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>Obligatorio. Si tiene una, especifique la dirección de correo electrónico de Docker. Si no tiene, especifique una dirección de correo electrónico ficticia, como por ejemplo a@b.c. Este correo electrónico es obligatorio para crear un secreto de Kubernetes, pero no se utiliza después de la creación.</td>
    </tr>
    </tbody></table>

6.  Verifique que el secreto se haya creado correctamente. Sustituya <em>&lt;kubernetes_namespace&gt;</em> por el espacio de nombres en el que ha creado imagePullSecret.

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

7.  [Despliegue un contenedor utilizando el imagePullSecret](#use_imagePullSecret) en su espacio de nombres.

### Acceso a imágenes almacenadas en otros registros privados
{: #private_images}

Si ya tiene un registro privado, debe almacenar las credenciales del registro en un imagePullSecret de Kubernetes y hacer referencia a dicho secreto en su archivo de configuración.
{:shortdesc}

Antes de empezar:

1.  [Cree un clúster](cs_clusters.html#clusters_cli).
2.  [Defina su clúster como destino de la CLI](cs_cli_install.html#cs_cli_configure).

Para crear un imagePullSecret:

1.  Cree el secreto de Kubernetes para almacenar las credenciales del registro privado.

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_URL> --docker-username=<docker_username> --docker-password=<docker_password> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>Descripción de los componentes de este mandato</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Descripción de los componentes de este mandato</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>Obligatorio. El espacio de nombres de Kubernetes del clúster en el que desea utilizar el secreto y desplegar los contenedores. Ejecute <code>kubectl get namespaces</code> para obtener una lista de todos los espacios de nombres del clúster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>Obligatorio. El nombre que desea utilizar para su imagePullSecret.</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>Obligatorio. El URL al registro en el que se están almacenadas sus imágenes privadas.</td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>Obligatorio. El nombre de usuario para iniciar una sesión en su registro privado.</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>Obligatorio. El valor de la señal de registro que ha recuperado anteriormente.</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>Obligatorio. Si tiene una, especifique la dirección de correo electrónico de Docker. Si no tiene una, especifique una dirección de correo electrónico ficticia, como por ejemplo a@b.c. Este correo electrónico es obligatorio para crear un secreto de Kubernetes, pero no se utiliza después de la creación.</td>
    </tr>
    </tbody></table>

2.  Verifique que el secreto se haya creado correctamente. Sustituya <em>&lt;kubernetes_namespace&gt;</em> por el nombre del espacio de nombres en el que ha creado imagePullSecret.

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

3.  [Cree un pod que haga referencia al imagePullSecret](#use_imagePullSecret).

## Despliegue de contenedores utilizando el imagePullSecret creado
{: #use_imagePullSecret}

Puede definir un imagePullSecret en su despliegue de pod o almacenar el imagePullSecret en su cuenta de servicio de Kubernetes para que esté disponible para todos los despliegues que no especifican una cuenta de servicio.
{: shortdesc}

Seleccione una de las opciones siguientes:
* [Hacer referencia al imagePullSecret en su despliegue de pod](#pod_imagePullSecret): Utilice esta opción si de forma predeterminada no desea otorgar acceso a su registro a todos los pods en su espacio de nombres.
* [Almacenar el imagePullSecret en la cuenta de servicio de Kubernetes](#store_imagePullSecret): Utilice esta opción para otorgar acceso a las imágenes en su registro para despliegues en los espacios de nombres de Kubernetes seleccionados.

Antes de empezar:
* [Cree un imagePullSecret](#other) para acceder a imágenes en otros registros, espacios de nombres de Kubernetes, cuentas o regiones de {{site.data.keyword.Bluemix_notm}}.
* [Defina su clúster como destino de la CLI](cs_cli_install.html#cs_cli_configure).

### Cómo hacer referencia al `imagePullSecret` en su despliegue de pod
{: #pod_imagePullSecret}

Cuando haga referencia al imagePullSecret en un despliegue de pod, el imagePullSecret solo es válido para este pod y no se puede compartir con otros pods en el espacio de nombres.
{:shortdesc}

1.  Cree un archivo de configuración de pod denominado `mypod.yaml`.
2.  Defina el pod y el imagePullSecret para acceder al {{site.data.keyword.registrylong_notm}} privado.

    Para acceder a una imagen privada:
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod
    spec:
      containers:
        - name: <container_name>
          image: registry.<region>.bluemix.net/<namespace_name>/<image_name>:<tag>
      imagePullSecrets:
        - name: <secret_name>
    ```
    {: codeblock}

    Para acceder a una imagen pública de {{site.data.keyword.Bluemix_notm}}:
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod
    spec:
      containers:
        - name: <container_name>
          image: registry.bluemix.net/<image_name>:<tag>
      imagePullSecrets:
        - name: <secret_name>
    ```
    {: codeblock}

    <table>
    <caption>Visión general de los componentes del archivo YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general de los componentes del archivo YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>&lt;container_name&gt;</em></code></td>
    <td>El nombre del contenedor para desplegar en el clúster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;namespace_name&gt;</em></code></td>
    <td>El espacio de nombres donde se almacena la imagen. Para obtener una lista de los espacios de nombres disponibles, ejecute `bx cr namespace-list`.</td>
    </tr>
    <tr>
    <td><code><em>&lt;image_name&gt;</em></code></td>
    <td>El nombre del imagen que desea utilizar. Para ver una lista de las imágenes disponibles en una cuenta de {{site.data.keyword.Bluemix_notm}}, ejecute `bx cr image-list`.</td>
    </tr>
    <tr>
    <td><code><em>&lt;tag&gt;</em></code></td>
    <td>La versión de la imagen que desea utilizar. Si no se especifica ninguna etiqueta, se utiliza la imagen etiquetada como <strong>la más reciente (latest)</strong>.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>El nombre del imagePullSecret que ha creado anteriormente.</td>
    </tr>
    </tbody></table>

3.  Guarde los cambios.
4.  Cree el despliegue en el clúster.
    ```
    kubectl apply -f mypod.yaml
    ```
    {: pre}

### Almacenamiento del imagePullSecret en la cuenta de servicio de Kubernetes para el espacio de nombres seleccionado
{:#store_imagePullSecret}

Cada espacio de nombres tiene una cuenta de servicio que se denomina `default`. Puede añadir el imagePullSecret a esta cuenta de servicio para otorgar acceso a las imágenes en el registro. Los despliegues que automáticamente no especifican una cuenta de servicio utiliza la cuenta de servicio `default` para este espacio de nombres.
{:shortdesc}

1. Compruebe si ya existe un imagePullSecret para la cuenta de servicio predeterminada.
   ```
   kubectl describe serviceaccount default -n <namespace_name>
   ```
   {: pre}
   Cuando `<none>` se visualiza en la entrada **Image pull secrets**, no existe imagePullSecret.  
2. Añada el imagePullSecret a su cuenta de servicio predeterminada.
   - **Para añadir el imagePullSecret cuando no se ha definido un imagePullSecret:**
       ```
       kubectl patch -n <namespace_name> serviceaccount/default -p '{"imagePullSecrets":[{"name": "bluemix-<namespace_name>-secret-regional"}]}'
       ```
       {: pre}
   - **Para añadir el imagePullSecret cuando ya se ha definido un imagePullSecret:**
       ```
       kubectl patch -n <namespace_name> serviceaccount/default --type='json' -p='[{"op":"add","path":"/imagePullSecrets/-","value":{"name":"bluemix-<namespace_name>-secret-regional"}}]'
       ```
       {: pre}
3. Verifique que su imagePullSecret se añadió a la cuenta de servicio predeterminada.
   ```
   kubectl describe serviceaccount default -n <namespace_name>
   ```
   {: pre}

   Salida de ejemplo:
   ```
   Name:                default
   Namespace:           <namespace_name>
   Labels:              <none>
   Annotations:         <none>
   Image pull secrets:  bluemix-namespace_name-secret-regional
   Mountable secrets:   default-token-sh2dx
   Tokens:              default-token-sh2dx
   Events:              <none>
   ```
   {: pre}

4. Despliegue un contenedor desde una imagen en el registro.
   ```
   apiVersion: v1
   kind: Pod
   metadata:
     name: mypod
   spec:
     containers:
       - name: <container_name>
         image: registry.<region>.bluemix.net/<namespace_name>/<image_name>:<tag>
   ```
   {: codeblock}

5. Crear el despliegue en el clúster.
   ```
   kubectl apply -f mypod.yaml
   ```
   {: pre}

<br />


