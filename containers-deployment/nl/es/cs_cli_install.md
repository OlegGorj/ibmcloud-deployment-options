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





# Configuración de la CLI y la API
{: #cs_cli_install}

Puede utilizar la CLI o la API de {{site.data.keyword.containerlong}} para crear y gestionar sus clústeres de Kubernetes.
{:shortdesc}

<br />


## Instalación de la CLI
{: #cs_cli_install_steps}

Instale las CLI necesarias para crear y gestionar sus clústeres de Kubernetes en {{site.data.keyword.containershort_notm}} y para desplegar apps contenerizadas en el clúster.
{:shortdesc}

Esta tarea incluye la información para instalar estas CLI y plug-ins:

-   {{site.data.keyword.Bluemix_notm}} CLI versión 0.5.0 o posterior
-   Plug-in de {{site.data.keyword.containershort_notm}}
-   Versión de CLI de Kubernetes que coincida con la versión `major.minor` de su clúster
-   Opcional: plug-in {{site.data.keyword.registryshort_notm}}
-   Opcional: Docker versión 1.9 o posterior

<br>
Para instalar las CLI:

1.  Un requisito previo del plug-in de {{site.data.keyword.containershort_notm}} es que instale la CLI de [{{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://clis.ng.bluemix.net/ui/home.html). El prefijo para ejecutar mandatos mediante la CLI de {{site.data.keyword.Bluemix_notm}} es `bx`.

2.  Inicie la sesión en la CLI de {{site.data.keyword.Bluemix_notm}}. Escriba
sus credenciales de {{site.data.keyword.Bluemix_notm}} cuando se le solicite.

    ```
    bx login
    ```
    {: pre}

    **Nota:** Si tiene un ID federado, utilice `bx login --sso` para iniciar
la sesión en la CLI de {{site.data.keyword.Bluemix_notm}}. Especifique su nombre de usuario y utilice el URL proporcionado en la salida de la CLI para recuperar el código de acceso de un solo uso. Sabe tiene un ID federado cuando el inicio de sesión falla sin el `--sso` y se lleva a cabo correctamente con la opción `--sso`.

3.  Para crear clústeres de Kubernetes y gestionar nodos trabajadores, instale el plug-in {{site.data.keyword.containershort_notm}}. El prefijo para ejecutar mandatos mediante el plug-in {{site.data.keyword.containershort_notm}} es `bx cs`.

    ```
    bx plugin install container-service -r Bluemix
    ```
    {: pre}

    Para comprobar que el plug-in se ha instalado correctamente, ejecute el siguiente mandato:

    ```
    bx plugin list
    ```
    {: pre}

    El plug-in {{site.data.keyword.containershort_notm}} se visualiza en los resultados como servicio de contenedor.

4.  {: #kubectl}Para ver una versión local del panel de control Kubernetes y desplegar apps en los clústeres, [instale la CLI de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/tools/install-kubectl/). El prefijo para ejecutar mandatos utilizando la CLI de Kubernetes es `kubectl`.

    1.  Descargue la versión `major.minor` de la CLI de Kubernetes que coincida con la versión `major.minor` del clúster Kubernetes que piensa utilizar. La versión de Kubernetes predeterminada actual de {{site.data.keyword.containershort_notm}} es la 1.9.7. **Nota**: Si utiliza una versión de CLI `kubectl` que no coincide al menos con la versión `major.minor` de los clústeres, puede experimentar resultados inesperados. Mantenga actualizadas las versiones de la CLI y el clúster de Kubernetes.

        - **OS X**:   [https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/darwin/amd64/kubectl ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/darwin/amd64/kubectl)
        - **Linux**:   [https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/linux/amd64/kubectl ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/linux/amd64/kubectl)
        - **Windows**:    [https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/windows/amd64/kubectl.exe ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/windows/amd64/kubectl.exe)

    2.  **Para OSX y Linux**: Complete los pasos siguientes.
        1.  Mueva el archivo ejecutable al directorio `/usr/local/bin`.

            ```
            mv /filepath/kubectl /usr/local/bin/kubectl
            ```
            {: pre}

        2.  Asegúrese de que `/usr/local/bin` aparezca en la variable del sistema `PATH`. La variable `PATH` contiene todos los directorios en los que el sistema operativo puede encontrar archivos ejecutables. Los directorios que se muestran en la variable `PATH` sirven para distintos propósitos. `/usr/local/bin` se utiliza para guardar archivos ejecutables correspondientes a software que no forma parte del sistema operativo y que ha instalado manualmente el administrador del sistema.

            ```
            echo $PATH
            ```
            {: pre}

            Ejemplo de salida de CLI:

            ```
            /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
            ```
            {: screen}

        3.  Convierta el archivo en ejecutable.

            ```
            chmod +x /usr/local/bin/kubectl
            ```
            {: pre}

    3.  **Para Windows**: Instale la CLI de Kubernetes en el mismo directorio que la CLI de {{site.data.keyword.Bluemix_notm}}. Esta configuración le ahorra algunos cambios en filepath cuando ejecute mandatos posteriormente.

5.  Para gestionar un repositorio de imágenes privadas, instale el plug-in {{site.data.keyword.registryshort_notm}}. Utilice este plug-in para configurar su propio espacio de nombres en un registro privado de imágenes multiarrendatario, de alta disponibilidad y escalable alojado por IBM, y para almacenar y compartir imágenes de Docker con otros usuarios. Las imágenes de Docker son necesarias para desplegar contenedores en un clúster. El prefijo para ejecutar mandatos de registro es `bx cr`.

    ```
    bx plugin install container-registry -r Bluemix
    ```
    {: pre}

    Para comprobar que el plug-in se ha instalado correctamente, ejecute el siguiente mandato:

    ```
    bx plugin list
    ```
    {: pre}

    El plug-in se muestra en los resultados como registro de contenedor.

6.  Para crear imágenes localmente y enviarlas por push al espacio de nombres del registro, [instale Docker ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.docker.com/community-edition#/download). Si está utilizando Windows 8 o anterior, puede instalar [Docker Toolbox ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.docker.com/toolbox/toolbox_install_windows/) en su lugar. La CLI de Docker se utiliza para crear apps en imágenes. El prefijo para ejecutar mandatos utilizando la CLI de Docker es `docker`.

A continuación, empiece a [crear clústeres de Kubernetes desde la CLI con {{site.data.keyword.containershort_notm}}](cs_clusters.html#clusters_cli).

Para obtener información acerca de estas CLI, consulte la documentación de dichas herramientas.

-   [Mandatos `bx`](/docs/cli/reference/bluemix_cli/bx_cli.html)
-   [Mandatos `bx cs`](cs_cli_reference.html#cs_cli_reference)
-   [Mandatos `kubectl`![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/reference/kubectl/overview/)
-   [Mandatos `bx cr`](/docs/cli/plugins/registry/index.html)

<br />




## Ejecución de la CLI en un contenedor en su sistema
{: #cs_cli_container}

En lugar de instalar cada CLI individualmente en el sistema, puede instalar las CLI en un contenedor que se ejecute en el sistema.
{:shortdesc}

1. Cree una imagen del Dockerfile proporcionado.

    ```
    docker build -t <image_name> https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/install-clis-container/Dockerfile
    ```
    {: pre}

2. Despliegue la imagen localmente como un contenedor y monte un volumen para acceder a los archivos locales.
  
    ```
    docker run -it -v /local/path:/container/volume <image_name>
    ```
    {: pre}
  
3. Empiece ejecutando los mandatos `bx cs` y `kubectl` desde el shell interactivo. Si crea datos que desee guardar, guarde los datos en el volumen que desea montar. Cuando salga del shell, el contenedor se detiene. 

<br />



## Configuración de la CLI para que ejecute mandatos `kubectl`
{: #cs_cli_configure}

Puede utilizar los mandatos que se proporcionan con la CLI de Kubernetes para gestionar clústeres en {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}

Todos los mandatos `kubectl` disponibles en Kubernetes 1.9.7 se pueden utilizar con clústeres en {{site.data.keyword.Bluemix_notm}}. Después de crear un clúster, establezca el contexto de la CLI local para dicho clúster con una variable de entorno. A continuación, puede ejecutar el mandato de Kubernetes `kubectl` para trabajar con el clúster en {{site.data.keyword.Bluemix_notm}}.

Para poder ejecutar mandatos `kubectl`, [instale las CLI necesarias](#cs_cli_install) y [cree un clúster](cs_clusters.html#clusters_cli).

1.  Inicie la sesión en la CLI de {{site.data.keyword.Bluemix_notm}}. Escriba
sus credenciales de {{site.data.keyword.Bluemix_notm}} cuando se le solicite. Para especificar una región de {{site.data.keyword.Bluemix_notm}}, [incluya el punto final de API](cs_regions.html#bluemix_regions).

    ```
    bx login
    ```
    {: pre}

    **Nota:** Si tiene un ID federado, utilice `bx login --sso` para iniciar
la sesión en la CLI de {{site.data.keyword.Bluemix_notm}}. Especifique su nombre de usuario y utilice el URL proporcionado en la salida de la CLI para recuperar el código de acceso de un solo uso. Sabe tiene un ID federado cuando el inicio de sesión falla sin el `--sso` y se lleva a cabo correctamente con la opción `--sso`.

2.  Seleccione una cuenta de {{site.data.keyword.Bluemix_notm}}. Si se le ha asignado a varias organizaciones de {{site.data.keyword.Bluemix_notm}}, seleccione la
organización en la que se ha creado el clúster. Los clústeres son específicos de una
organización, pero son independientes de un espacio de {{site.data.keyword.Bluemix_notm}}. Por lo tanto, no es necesario que seleccione un espacio.

3.  Para crear o acceder a clústeres de Kubernetes en una región distinta de la región de {{site.data.keyword.Bluemix_notm}} seleccionada anteriormente, ejecute `bx cs region-set`.

4.  Obtenga una lista de todos los clústeres de la cuenta para obtener el nombre del clúster.

    ```
    bx cs clusters
    ```
    {: pre}

5.  Defina el clúster que ha creado como contexto para esta sesión. Siga estos pasos de configuración cada vez que de trabaje con el clúster.
    1.  Obtenga el mandato para establecer la variable de entorno y descargar los archivos de configuración de Kubernetes.

        ```
        bx cs cluster-config <cluster_name_or_ID>
        ```
        {: pre}

        Después de descargar los archivos de configuración, se muestra un mandato que puede utilizar para establecer la vía de acceso al archivo de configuración de
Kubernetes local como variable de entorno.

        Ejemplo:

        ```
        export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
        ```
        {: screen}

    2.  Copie y pegue el mandato que se muestra en el terminal para definir la variable de entorno `KUBECONFIG`.

    3.  Compruebe que la variable de entorno `KUBECONFIG` se haya establecido correctamente.

        Ejemplo:

        ```
        echo $KUBECONFIG
        ```
        {: pre}

        Salida:
        ```
        /Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
        ```
        {: screen}

6.  Compruebe que los mandatos `kubectl` se ejecutan correctamente con el clúster comprobando la versión del cliente de la CLI de Kubernetes.

    ```
    kubectl version  --short
    ```
    {: pre}

    Salida de ejemplo:

    ```
    Client Version: v1.9.7
    Server Version: v1.9.7
    ```
    {: screen}

Ahora puede ejecutar mandatos `kubectl` para gestionar sus clústeres en {{site.data.keyword.Bluemix_notm}}. Para ver una lista completa de mandatos, consulte la [documentación de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/reference/kubectl/overview/).

**Sugerencia:** Si utiliza Windows y la CLI de Kubernetes no está instalada en el mismo directorio que la CLI de {{site.data.keyword.Bluemix_notm}}, debe cambiar los directorios por la vía de acceso donde está instalada la CLI de Kubernetes para poder ejecutar correctamente los mandatos `kubectl`.


<br />


## Actualización de la CLI
{: #cs_cli_upgrade}

Quizás desee actualizar las CLI periódicamente para usar nuevas funciones.
{:shortdesc}

Esta tarea incluye la información para actualizar estas CLI.

-   {{site.data.keyword.Bluemix_notm}} CLI versión 0.5.0 o posterior
-   Plug-in de {{site.data.keyword.containershort_notm}}
-   CLI de Kubernetes versión 1.9.7 o posterior
-   Plug-in de {{site.data.keyword.registryshort_notm}}
-   Docker versión 1.9. o posterior

<br>
Para actualizar las CLI:

1.  Actualice la CLI de {{site.data.keyword.Bluemix_notm}}. Descargue la [versión más reciente ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://clis.ng.bluemix.net/ui/home.html) y ejecute el instalador.

2. Inicie la sesión en la CLI de {{site.data.keyword.Bluemix_notm}}. Escriba
sus credenciales de {{site.data.keyword.Bluemix_notm}} cuando se le solicite. Para especificar una región de {{site.data.keyword.Bluemix_notm}}, [incluya el punto final de API](cs_regions.html#bluemix_regions).

    ```
    bx login
    ```
    {: pre}

     **Nota:** Si tiene un ID federado, utilice `bx login --sso` para iniciar
la sesión en la CLI de {{site.data.keyword.Bluemix_notm}}. Especifique su nombre de usuario y utilice el URL proporcionado en la salida de la CLI para recuperar el código de acceso de un solo uso. Sabe tiene un ID federado cuando el inicio de sesión falla sin el `--sso` y se lleva a cabo correctamente con la opción `--sso`.

3.  Actualice el plug-in de {{site.data.keyword.containershort_notm}}.
    1.  Instale la actualización desde el repositorio del plug-in de {{site.data.keyword.Bluemix_notm}}.

        ```
        bx plugin update container-service -r Bluemix
        ```
        {: pre}

    2.  Para verificar la instalación del plug-in, ejecute el mandato siguiente
y compruebe la lista de plug-ins instalados.

        ```
        bx plugin list
        ```
        {: pre}

        El plug-in {{site.data.keyword.containershort_notm}} se visualiza en los resultados como servicio de contenedor.

    3.  Inicialice la CLI.

        ```
        bx cs init
        ```
        {: pre}

4.  [Actualice la CLI de Kubernetes](#kubectl).

5.  Actualice el plug-in de {{site.data.keyword.registryshort_notm}}.
    1.  Instale la actualización desde el repositorio del plug-in de {{site.data.keyword.Bluemix_notm}}.

        ```
        bx plugin update container-registry -r Bluemix
        ```
        {: pre}

    2.  Para verificar la instalación del plug-in, ejecute el mandato siguiente
y compruebe la lista de plug-ins instalados.

        ```
        bx plugin list
        ```
        {: pre}

        El plug-in de registro se muestra en los resultados como container-registry.

6.  Actualice Docker.
    -   Si utiliza Docker Community Edition, inicie Docker, pulse el icono **Docker** y pulse **Comprobar si hay actualizaciones**.
    -   Si está utilizando Docker Toolbox, descargue la [versión más reciente ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.docker.com/toolbox/toolbox_install_windows/) y ejecute el instalador.

<br />


## Desinstalación de la CLI
{: #cs_cli_uninstall}

Si ya no necesita una CLI, puede desinstalarla.
{:shortdesc}

Esta tarea incluye la información para eliminar estas CLI:


-   Plug-in de {{site.data.keyword.containershort_notm}}
-   CLI de Kubernetes
-   Plug-in de {{site.data.keyword.registryshort_notm}}
-   Docker versión 1.9. o posterior

<br>
Para desinstalar las CLI:

1.  Desinstalar el plug-in de {{site.data.keyword.containershort_notm}}.

    ```
    bx plugin uninstall container-service
    ```
    {: pre}

2.  Desinstalar el plug-in de {{site.data.keyword.registryshort_notm}}.

    ```
    bx plugin uninstall container-registry
    ```
    {: pre}

3.  Para verificar la desinstalación de los plug-ins, ejecute el mandato siguiente y compruebe la lista de plug-ins de instalados.

    ```
    bx plugin list
    ```
    {: pre}

    El servicio de contenedor y el plug-in de servicio de contenedor no se muestran en los resultados.

6.  Desinstalar Docker. Las instrucciones para desinstalar Docker varían en función del sistema operativo utilizado.

    - [OSX ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.docker.com/docker-for-mac/#uninstall-or-reset)
    - [Linux ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/#uninstall-docker-ce)
    - [Windows ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.docker.com/toolbox/toolbox_install_windows/#how-to-uninstall-toolbox)

<br />


## Automatización de despliegues de clústeres con la API
{: #cs_api}

Puede utilizar la API de {{site.data.keyword.containershort_notm}} para automatizar la creación, el despliegue y la gestión de clústeres de Kubernetes.
{:shortdesc}

La API de {{site.data.keyword.containershort_notm}} precisa de información de cabecera que debe proporcionar en su solicitud de la API y que depende del tipo de API utilizado. Para determinar la información de cabecera que se necesita para la API, consulte la [{{site.data.keyword.containershort_notm}} documentación de la API ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://us-south.containers.bluemix.net/swagger-api).

**Nota:** Para autenticarse con {{site.data.keyword.containershort_notm}}, debe especificar su señal de Identity and Access Management (IAM) que se genera con las credenciales de {{site.data.keyword.Bluemix_notm}} y que incluye el ID de la cuenta de {{site.data.keyword.Bluemix_notm}} en la que se ha creado el clúster. Dependiendo de la forma en que se autentique con {{site.data.keyword.Bluemix_notm}}, puede elegir entre las siguientes opciones para automatizar la creación de la señal de IAM.

<table>
<caption>Opciones y tipos de ID</caption>
<thead>
<th>ID de {{site.data.keyword.Bluemix_notm}}</th>
<th>Mis opciones</th>
</thead>
<tbody>
<tr>
<td>ID no federado</td>
<td><ul><li>Nombre de usuario y contraseña de <strong>{{site.data.keyword.Bluemix_notm}}:</strong> Puede seguir los pasos de este tema para automatizar por completo la creación de la señal de acceso de IAM.</li>
<li><strong>Generar una clave de API de {{site.data.keyword.Bluemix_notm}}:</strong> Como alternativa al uso de nombre de usuario y contraseña de {{site.data.keyword.Bluemix_notm}}, puede <a href="../iam/apikeys.html#manapikey" target="_blank">utilizar claves de API de {{site.data.keyword.Bluemix_notm}} </a>. Las claves de API de {{site.data.keyword.Bluemix_notm}} dependen de la cuenta de {{site.data.keyword.Bluemix_notm}} para la que se generan. No puede combinar su clave de API de {{site.data.keyword.Bluemix_notm}} con otro ID de cuenta en la misma señal de IAM. Para acceder a los clústeres que se han creado con una cuenta distinta de aquella en la que se basa la clave de API de {{site.data.keyword.Bluemix_notm}}, debe iniciar una sesión en la cuenta para generar una nueva clave de API. </li></ul></tr>
<tr>
<td>ID federado</td>
<td><ul><li><strong>Generar una clave de API de {{site.data.keyword.Bluemix_notm}}: </strong> <a href="../iam/apikeys.html#manapikey" target="_blank">Las claves de API de {{site.data.keyword.Bluemix_notm}}</a> dependen de la cuenta de {{site.data.keyword.Bluemix_notm}} para la que se generan. No puede combinar su clave de API de {{site.data.keyword.Bluemix_notm}} con otro ID de cuenta en la misma señal de IAM. Para acceder a los clústeres que se han creado con una cuenta distinta de aquella en la que se basa la clave de API de {{site.data.keyword.Bluemix_notm}}, debe iniciar una sesión en la cuenta para generar una nueva clave de API. </li><li><strong>Utilizar un código de acceso puntual: </strong> Si se autentica con {{site.data.keyword.Bluemix_notm}} mediante un código de acceso puntual, no puede automatizar completamente la creación de la señal de IAM porque la recuperación del código de acceso puntual requiere una interacción manual con el navegador web. Para automatizar por completo la creación de la señal de IAM, debe crear en su lugar una clave de API de {{site.data.keyword.Bluemix_notm}}. </ul></td>
</tr>
</tbody>
</table>

1.  Cree su señal de acceso de IAM (Identity and Access Management). La información del cuerpo que se incluye en la solicitud varía en función del método de autenticación de {{site.data.keyword.Bluemix_notm}} que utilice. Sustituya los valores siguientes:
  - _&lt;username&gt;_: Su nombre de usuario de {{site.data.keyword.Bluemix_notm}}.
  - _&lt;password&gt;_: Su contraseña de {{site.data.keyword.Bluemix_notm}}.
  - _&lt;api_key&gt;_: Su clave de API de {{site.data.keyword.Bluemix_notm}}.
  - _&lt;passcode&gt;_: Su código de acceso puntual de {{site.data.keyword.Bluemix_notm}}. Ejecute `bx login --sso` y siga las instrucciones de la salida de la CLI para recuperar el código de acceso puntual mediante su navegador web.

    ```
    POST https://iam.<region>.bluemix.net/oidc/token
    ```
    {: codeblock}

    Ejemplo:
    ```
    POST https://iam.ng.bluemix.net/oidc/token
    ```
    {: codeblock}

    Para especificar una región de {{site.data.keyword.Bluemix_notm}}, [consulte las abreviaturas de región tal como se usan en los puntos finales de la API](cs_regions.html#bluemix_regions).

    <table summary-"Input parameters to retrieve tokens">
    <caption>Parámetros de entrada para obtener señales</caption>
    <thead>
        <th>Parámetros de entrada</th>
        <th>Valores</th>
    </thead>
    <tbody>
    <tr>
    <td>Cabecera</td>
    <td><ul><li>Content-Type:application/x-www-form-urlencoded</li> <li>Authorization: Basic Yng6Yng=<p><strong>Nota</strong>: <code>Yng6Yng=</code> equivale a la autorización codificada de URL para el nombre de usuario <strong>bx</strong> y la contraseña <strong>bx</strong>.</p></li></ul>
    </td>
    </tr>
    <tr>
    <td>Cuerpo correspondiente al nombre de usuario y contraseña de {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>grant_type: password</li>
    <li>response_type: cloud_iam uaa</li>
    <li>username: <em>&lt;username&gt;</em></li>
    <li>password: <em>&lt;password&gt;</em></li>
    <li>uaa_client_ID: cf</li>
    <li>uaa_client_secret:</li></ul>
    <strong>Nota</strong>: Añada la clave <code>uaa_client_secret</code> sin especificar un valor.</td>
    </tr>
    <tr>
    <td>Cuerpo correspondiente a las claves de API de {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>grant_type: urn:ibm:params:oauth:grant-type:apikey</li>
    <li>response_type: cloud_iam uaa</li>
    <li>apikey: <em>&lt;api_key&gt;</em></li>
    <li>uaa_client_ID: cf</li>
    <li>uaa_client_secret:</li></ul>
    <strong>Nota</strong>: Añada la clave <code>uaa_client_secret</code> sin especificar un valor.</td>
    </tr>
    <tr>
    <td>Cuerpo correspondiente al código de acceso puntual de {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>grant_type: urn:ibm:params:oauth:grant-type:passcode</li>
    <li>response_type: cloud_iam uaa</li>
    <li>passcode: <em>&lt;passcode&gt;</em></li>
    <li>uaa_client_ID: cf</li>
    <li>uaa_client_secret:</li></ul>
    <strong>Nota</strong>: Añada la clave <code>uaa_client_secret</code> sin especificar un valor.</td>
    </tr>
    </tbody>
    </table>

    Ejemplo de salida de API:

    ```
    {
    "access_token": "<iam_token>",
      "refresh_token": "<iam_refresh_token>",
      "uaa_token": "<uaa_token>",
      "uaa_refresh_token": "<uaa_refresh_token>",
      "token_type": "Bearer",
      "expires_in": 3600,
      "expiration": 1493747503
    }

    ```
    {: screen}

    Encontrará la señal de IAM en el campo **access_token** de la salida de API. Anote la señal IAM para recuperar información de cabecera adicional en los pasos siguientes.

2.  Recupere el ID de la cuenta de {{site.data.keyword.Bluemix_notm}} en la que se ha creado el clúster. Sustituya _&lt;iam_token&gt;_ por la señal de IAM que ha recuperado en el paso anterior.

    ```
    GET https://accountmanagement.<region>.bluemix.net/v1/accounts
    ```
    {: codeblock}

    <table summary="Parámetros de entrada para obtener el ID de cuenta de {{site.data.keyword.Bluemix_notm}}">
    <caption>Parámetros de entrada para obtener un ID de cuenta de {{site.data.keyword.Bluemix_notm}}</caption>
    <thead>
  	<th>Parámetros de entrada</th>
  	<th>Valores</th>
    </thead>
    <tbody>
  	<tr>
  		<td>Cabeceras</td>
  		<td><ul><li>Content-Type: application/json</li>
      <li>Authorization: bearer &lt;iam_token&gt;</li>
      <li>Accept: application/json</li></ul></td>
  	</tr>
    </tbody>
    </table>

    Ejemplo de salida de API:

    ```
    {
      "total_results": 3,
      "total_pages": 1,
      "prev_url": null,
      "next_url": null,
      "resources":
        {
          "metadata": {
            "guid": "<account_ID>",
            "url": "/v1/accounts/<account_ID>",
            "created_at": "2016-01-07T18:55:09.726Z",
            "updated_at": "2017-04-28T23:46:03.739Z",
            "origin": "BSS"
    ...
    ```
    {: screen}

    Encontrará el ID de la cuenta de {{site.data.keyword.Bluemix_notm}} en el campo **resources/metadata/guid** de la salida de la API.

3.  Genere una nueva señal de IAM que incluya sus credenciales de {{site.data.keyword.Bluemix_notm}} y el ID de la cuenta en la que se ha creado el clúster. Sustituya _&lt;account_ID&gt;_ por el ID de la cuenta de {{site.data.keyword.Bluemix_notm}} que ha recuperado en el paso anterior.

    **Nota:** Si está utilizando una clave de API de {{site.data.keyword.Bluemix_notm}}, debe utilizar el ID de cuenta de {{site.data.keyword.Bluemix_notm}} y la clave de API se ha creado para la misma. Para acceder a los clústeres en otras cuentas, inicie una sesión en esta cuenta y cree una clave de API de {{site.data.keyword.Bluemix_notm}} que se base en esta cuenta.

    ```
    POST https://iam.<region>.bluemix.net/oidc/token
    ```
    {: codeblock}

    Ejemplo:
    ```
    POST https://iam.ng.bluemix.net/oidc/token
    ```
    {: codeblock}

    Para especificar una región de {{site.data.keyword.Bluemix_notm}}, [consulte las abreviaturas de región tal como se usan en los puntos finales de la API](cs_regions.html#bluemix_regions).

    <table summary-"Input parameters to retrieve tokens">
    <caption>Parámetros de entrada para obtener señales</caption>
    <thead>
        <th>Parámetros de entrada</th>
        <th>Valores</th>
    </thead>
    <tbody>
    <tr>
    <td>Cabecera</td>
    <td><ul><li>Content-Type:application/x-www-form-urlencoded</li> <li>Authorization: Basic Yng6Yng=<p><strong>Nota</strong>: <code>Yng6Yng=</code> equivale a la autorización codificada de URL para el nombre de usuario <strong>bx</strong> y la contraseña <strong>bx</strong>.</p></li></ul>
    </td>
    </tr>
    <tr>
    <td>Cuerpo correspondiente al nombre de usuario y contraseña de {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>grant_type: password</li>
    <li>response_type: cloud_iam uaa</li>
    <li>username: <em>&lt;username&gt;</em></li>
    <li>password: <em>&lt;password&gt;</em></li>
    <li>uaa_client_ID: cf</li>
    <li>uaa_client_secret:</li>
    <li>bss_account: <em>&lt;account_ID&gt;</em></li></ul>
    <strong>Nota</strong>: Añada la clave <code>uaa_client_secret</code> sin especificar un valor.</td>
    </tr>
    <tr>
    <td>Cuerpo correspondiente a las claves de API de {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>grant_type: urn:ibm:params:oauth:grant-type:apikey</li>
    <li>response_type: cloud_iam uaa</li>
    <li>apikey: <em>&lt;api_key&gt;</em></li>
    <li>uaa_client_ID: cf</li>
    <li>uaa_client_secret:</li>
    <li>bss_account: <em>&lt;account_ID&gt;</em></li></ul>
      <strong>Nota</strong>: Añada la clave <code>uaa_client_secret</code> sin especificar un valor.</td>
    </tr>
    <tr>
    <td>Cuerpo correspondiente al código de acceso puntual de {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>grant_type: urn:ibm:params:oauth:grant-type:passcode</li>
    <li>response_type: cloud_iam uaa</li>
    <li>passcode: <em>&lt;passcode&gt;</em></li>
    <li>uaa_client_ID: cf</li>
    <li>uaa_client_secret:</li>
    <li>bss_account: <em>&lt;account_ID&gt;</em></li></ul><strong>Nota</strong>: Añada la clave <code>uaa_client_secret</code> sin especificar un valor.</td>
    </tr>
    </tbody>
    </table>

    Ejemplo de salida de API:

    ```
    {
      "access_token": "<iam_token>",
      "refresh_token": "<iam_refresh_token>",
      "uaa_token": "<uaa_token>",
      "uaa_refresh_token": "<uaa_refresh_token>",
      "token_type": "Bearer",
      "expires_in": 3600,
      "expiration": 1493747503
    }

    ```
    {: screen}

    Encontrará la señal de IAM en el campo **access_token**, y la señal de renovación de IAM en el campo **refresh_token**.

4.  Obtenga una lista de todos los clústeres de Kubernetes de la cuenta. Utilice la información que ha recuperado en pasos anteriores para crear la información de cabecera.

     ```
     GET https://containers.bluemix.net/v1/clusters
     ```
     {: codeblock}

     <table summary="Parámetros de entrada para trabajar con la API">
     <caption>Parámetros de entrada para trabajar con la API</caption>
     <thead>
     <th>Parámetros de entrada</th>
     <th>Valores</th>
     </thead>
     <tbody>
     <tr>
     <td>Cabecera</td>
     <td><ul><li>Authorization: bearer <em>&lt;iam_token&gt;</em></li>
     <li>X-Auth-Refresh-Token: <em>&lt;refresh_token&gt;</em></li></ul></td>
     </tr>
     </tbody>
     </table>

5.  Consulte la [{{site.data.keyword.containershort_notm}} documentación de la API ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://containers.bluemix.net/swagger-api) para ver una lista de las API admitidas.

<br />


## Renovación de señales de acceso de IAM y obtención de nuevas señales de renovación
{: #cs_api_refresh}

Cada señal de acceso de IAM (Identity and Access Management) que se emite mediante la API caduca transcurrida una hora. Debe renovar la señal de acceso de forma periódica para garantizar el acceso a la API de {{site.data.keyword.Bluemix_notm}}. Puede utilizar los mismos pasos para obtener una nueva señal de renovación.
{:shortdesc}

Antes de empezar, asegúrese de que tiene una señal de renovación de IAM o una clave de API de {{site.data.keyword.Bluemix_notm}} para solicitar una nueva señal de acceso.
- **Señal de renovación:** Siga las instrucciones en [Automatización del proceso de creación y gestión de clústeres con la API de {{site.data.keyword.Bluemix_notm}}](#cs_api).
- **Clave de API:** Recupere su clave de API de [{{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/) tal como se indica a continuación.
   1. Desde la barra de menús, pulse **Gestionar** > **Seguridad** > **Claves de API de plataforma**.
   2. Pulse **Crear**.
   3. Especifique un **Nombre** y una **Descripción** para la clave de API y, a continuación, pulse **Crear**.
   4. Pulse **Mostrar** para ver la clave de API generada en su nombre.
   5. Copie la clave de API de forma que la pueda utilizar para recuperar su señal de acceso de IAM.

Utilice los pasos siguientes si desea crear una señal de IAM o si desea obtener una nueva señal de renovación.

1.  Genere una nueva señal de acceso de IAM utilizando la señal de renovación o la clave de API de {{site.data.keyword.Bluemix_notm}}.
    ```
    POST https://iam.bluemix.net/identity/token
    ```
    {: codeblock}

    <table summary="Parámetros de entrada para una nueva señal de IAM">
    <caption>Parámetros de entrada para una nueva señal de IAM</caption>
    <thead>
    <th>Parámetros de entrada</th>
    <th>Valores</th>
    </thead>
    <tbody>
    <tr>
    <td>Cabecera</td>
    <td><ul><li>Content-Type: application/x-www-form-urlencoded</li>
      <li>Authorization: Basic Yng6Yng=</br></br><strong>Nota:</strong> <code>Yng6Yng=</code> equivale a la autorización codificada de URL para el nombre de usuario <strong>bx</strong> y la contraseña <strong>bx</strong>.</li></ul></td>
    </tr>
    <tr>
    <td>Cuerpo al utilizar la señal de renovación</td>
    <td><ul><li>grant_type: refresh_token</li>
    <li>response_type: cloud_iam uaa</li>
    <li>refresh_token: <em>&lt;iam_refresh_token&gt;</em></li>
    <li>uaa_client_ID: cf</li>
    <li>uaa_client_secret:</li>
    <li>bss_account: <em>&lt;account_ID&gt;</em></li></ul><strong>Nota</strong>: Añada la clave <code>uaa_client_secret</code> sin especificar un valor.</td>
    </tr>
    <tr>
      <td>Cuerpo al utilizar la clave de API de {{site.data.keyword.Bluemix_notm}}</td>
      <td><ul><li>grant_type: <code>urn:ibm:params:oauth:grant-type:apikey</code></li>
    <li>response_type: cloud_iam uaa</li>
    <li>apikey: <em>&lt;api_key&gt;</em></li>
    <li>uaa_client_ID: cf</li>
        <li>uaa_client_secret:</li></ul><strong>Nota:</strong> Añada la clave <code>uaa_client_secret</code> sin especificar un valor.</td>
    </tr>
    </tbody>
    </table>

    Ejemplo de salida de API:

    ```
    {
      "access_token": "<iam_token>",
      "refresh_token": "<iam_refresh_token>",
      "uaa_token": "<uaa_token>",
      "uaa_refresh_token": "<uaa_refresh_token>",
      "token_type": "Bearer",
      "expires_in": 3600,
      "expiration": 1493747503
    }

    ```
    {: screen}

    Encontrará la nueva señal de IAM en el campo **access_token**, y la señal de renovación de IAM en el campo **refresh_token** de la salida de la API.

2.  Siga trabajando con la [{{site.data.keyword.containershort_notm}} documentación de la API ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://us-south.containers.bluemix.net/swagger-api) utilizando la señal del paso anterior.

