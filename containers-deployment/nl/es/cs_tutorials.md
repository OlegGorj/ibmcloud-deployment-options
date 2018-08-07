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



# Guía de aprendizaje: Creación de clústeres
{: #cs_cluster_tutorial}

Despliegue y gestione un clúster de Kubernetes en {{site.data.keyword.containerlong}}. Puede automatizar el despliegue, operación, escalado y supervisión de apps contenerizadas en un clúster.
{:shortdesc}

En esta serie de guías de aprendizaje verá cómo una firma de relaciones públicas ficticia utiliza funciones de Kubernetes para desplegar una app contenerizada en {{site.data.keyword.Bluemix_notm}}. Mediante {{site.data.keyword.toneanalyzerfull}}, la empresa PR analiza sus notas de prensa y recibe comentarios.


## Objetivos

En esta primera guía de aprendizaje, actuará como administrador de redes de la empresa PR. Configurará clúster de Kubernetes personalizado que se utilizará para desplegar y probar una versión de
Hello World de la app.

Para configurar la infraestructura debe:

-   Crear un clúster con 1 nodo trabajador.
-   Instalar las CLI para ejecutar mandatos de Kubernetes y gestionar imágenes de Docker.
-   Crear un repositorio de imágenes privadas en {{site.data.keyword.registrylong_notm}} para almacenar las imágenes.
-   Añadir el servicio {{site.data.keyword.toneanalyzershort}} al clúster para que cualquier app del clúster pueda utilizar el servicio.


## Tiempo necesario

40 minutos


## Público

Esta guía de aprendizaje está destinada a los desarrolladores de software y administradores de la red que están creando un clúster de Kubernetes por primera vez.


## Requisitos previos

-  Una [cuenta de {{site.data.keyword.Bluemix_notm}} de Suscripción o Pago según uso ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/registration/)
-  El [rol de desarrollador de Cloud Foundry](/docs/iam/mngcf.html#mngcf) en el espacio de clúster en el que desea trabajar.


## Lección 1: Creación de un clúster y configuración de la CLI
{: #cs_cluster_tutorial_lesson1}

Cree su clúster en la GUI e instale las CLI necesarias.
{: shortdesc}

**Para crear el clúster**

Puesto que el suministro puede tardar unos minutos, cree el clúster antes de instalar las CLI.

1.  [En la interfaz gráfica de usuario ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/containers-kubernetes/catalog/cluster/create) cree un clúster gratuito o estándar que posea un nodo trabajador.

    También puede crear un [clúster en la CLI](cs_clusters.html#clusters_cli).
    {: tip}

A medida que se suministra el clúster, instale las siguientes CLI para gestionar los clústeres:
-   CLI de {{site.data.keyword.Bluemix_notm}}
-   Plug-in de {{site.data.keyword.containershort_notm}}
-   CLI de Kubernetes
-   Plug-in de {{site.data.keyword.registryshort_notm}}
-   CLI de Docker

</br>
**Para instalar las CLI y sus requisitos previos**

1.  Un requisito previo del plug-in de {{site.data.keyword.containershort_notm}} es que instale la CLI de [{{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://clis.ng.bluemix.net/ui/home.html). Para ejecutar mandatos de CLI de {{site.data.keyword.Bluemix_notm}}, utilice el prefijo `bx`.
2.  Siga las indicaciones para seleccionar una cuenta y una organización de {{site.data.keyword.Bluemix_notm}}. Los clústeres son específicos de una cuenta, pero son independientes de una organización o espacio de {{site.data.keyword.Bluemix_notm}}.

4.  Instale el plug-in de {{site.data.keyword.containershort_notm}} para crear clústeres de Kubernetes y gestionar nodos trabajadores. Para ejecutar mandatos del plug-in de {{site.data.keyword.containershort_notm}}, utilice el prefijo `bx cs`.

    ```
    bx plugin install container-service -r Bluemix
    ```
    {: pre}

5.  Para desplegar apps en los clústeres, [instale la CLI de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/tools/install-kubectl/). Para ejecutar mandatos la CLI de Kubernetes, utilice el prefijo `kubectl`.
    1.  Para disponer de compatibilidad funcional completa, descargue la versión de la CLI de Kubernetes que coincida con la versión del clúster Kubernetes que piensa utilizar. La versión de Kubernetes predeterminada actual de {{site.data.keyword.containershort_notm}} es la 1.9.7.

        OS X:   [https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/darwin/amd64/kubectl ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/darwin/amd64/kubectl)

        Linux:   [https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/linux/amd64/kubectl ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/linux/amd64/kubectl)

        Windows:   [https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/windows/amd64/kubectl.exe ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://storage.googleapis.com/kubernetes-release/release/v1.9.7/bin/windows/amd64/kubectl.exe)

          **Sugerencia:** Si utiliza Windows, instale la CLI de Kubernetes en el mismo directorio que la CLI de {{site.data.keyword.Bluemix_notm}}. Esta configuración le ahorra algunos cambios en filepath cuando ejecute mandatos posteriormente.

    2.  Si está utilizando OS X o Linux, siga estos pasos.
        1.  Mueva el archivo ejecutable al directorio `/usr/local/bin`.

            ```
            mv filepath/kubectl /usr/local/bin/kubectl
            ```
            {: pre}

        2.  Asegúrese de que `/usr/local/bin` aparezca en la variable del sistema `PATH`. La variable `PATH` contiene todos los directorios en los que el sistema operativo puede encontrar archivos ejecutables. Los directorios que se muestran en la variable `PATH` sirven para distintos propósitos. `/usr/local/bin` se utiliza para guardar archivos ejecutables correspondientes a software que no forma parte del sistema operativo y que ha instalado manualmente el administrador del sistema.

            ```
            echo $PATH
            ```
            {: pre}

            La salida de la CLI se parecerá a la siguiente.

            ```
            /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
            ```
            {: screen}

        3.  Convierta el archivo en ejecutable.

            ```
            chmod +x /usr/local/bin/kubectl
            ```
            {: pre}

6. Para configurar y gestionar un repositorio de imágenes privadas en {{site.data.keyword.registryshort_notm}}, instale el plugin de registro de {{site.data.keyword.registryshort_notm}} Containers. Para ejecutar mandatos de registro, utilice el prefijo `bx cr`.

    ```
    bx plugin install container-registry -r Bluemix
    ```
    {: pre}

    Para comprobar que los plug-ins container-service y container-registry se han instalado correctamente, ejecute el siguiente mandato:

    ```
    bx plugin list
    ```
    {: pre}

7. Para crear imágenes localmente y enviarlas por push al repositorio de imágenes privadas, [instale la CLI de Docker Community Edition ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.docker.com/community-edition#/download). Si está utilizando Windows 8 o anterior, puede instalar [Docker Toolbox ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.docker.com/toolbox/toolbox_install_windows/) en su lugar.

¡Enhorabuena! Ha instalado correctamente las CLI para las siguientes lecciones y guías de aprendizaje. A continuación, configure el entorno de clúster y añada el servicio {{site.data.keyword.toneanalyzershort}}.


## Lección 2: Configuración de su registro privado
{: #cs_cluster_tutorial_lesson2}

Configure un repositorio de imágenes privadas en {{site.data.keyword.registryshort_notm}} y añada secretos a su clúster para que la app pueda acceder al servicio {{site.data.keyword.toneanalyzershort}}.
{: shortdesc}

1.  Inicie una sesión en la CLI de {{site.data.keyword.Bluemix_notm}} utilizando sus credenciales de {{site.data.keyword.Bluemix_notm}} cuando se le soliciten.

    ```
    bx login [--sso]
    ```
    {: pre}

    **Nota:** Si tiene un ID federado, utilice el distintivo `--sso` para iniciar la sesión. Especifique su nombre de usuario y utilice el URL proporcionado en la salida de la CLI para recuperar el código de acceso de un solo uso.

2.  Configure su propio repositorio de imágenes privadas en {{site.data.keyword.registryshort_notm}} para almacenar y compartir de forma segura imágenes de Docker con todos los usuarios del clúster. Un repositorio privado en {{site.data.keyword.Bluemix_notm}} se identifica mediante un espacio de nombres. El espacio de nombres sirve para crear un URL exclusivo para el repositorio privado que los desarrolladores pueden utilizar para acceder a las imágenes de Docker.

    Obtenga más información sobre cómo [proteger su información personal](cs_secure.html#pi) cuando se trabaja con imágenes de contenedor.

    En este ejemplo, la empresa PR desea crear un solo repositorio de imágenes en {{site.data.keyword.registryshort_notm}}, por lo que eligen _pr_firm_ como el espacio de nombres para agrupar todas las imágenes de la cuenta. Sustituya _&lt;namespace&gt;_ por un espacio de nombres de su elección que no esté relacionado con la guía de aprendizaje.

    ```
    bx cr namespace-add <namespace>
    ```
    {: pre}

3.  Antes de continuar con el paso siguiente, verifique que el despliegue del nodo trabajador se ha completado.

    ```
    bx cs workers <cluster_name_or_ID>
    ```
    {: pre}

    Cuando el suministro del nodo trabajador finaliza, el estado pasa a **Preparado** y puede empezar a enlazar servicios de {{site.data.keyword.Bluemix_notm}}.

    ```
    ID                                                 Public IP       Private IP       Machine Type   State    Status   Location   Version
    kube-mil01-pafe24f557f070463caf9e31ecf2d96625-w1   169.xx.xxx.xxx   10.xxx.xx.xxx   free           normal   Ready    mil01      1.9.7
    ```
    {: screen}

## Lección 3: Configuración del entorno de clúster
{: #cs_cluster_tutorial_lesson3}

Establezca el contexto para el clúster en la CLI.
{: shortdesc}

Cada vez que inicie la sesión en la CLI de {{site.data.keyword.containerlong}} para trabajar con clústeres, deberá ejecutar estos mandatos para establecer el archivo de configuración del clúster como una variable de la sesión. La CLI de Kubernetes utiliza esta variable para buscar un archivo de configuración local y los certificados necesarios para conectar con el clúster en {{site.data.keyword.Bluemix_notm}}.

1.  Obtenga el mandato para establecer la variable de entorno y descargar los archivos de configuración de Kubernetes.

    ```
    bx cs cluster-config <cluster_name_or_ID>
    ```
    {: pre}

    Cuando termine la descarga de los archivos de configuración, se muestra un mandato que puede utilizar para establecer la vía de acceso al archivo de configuración de
Kubernetes como variable de entorno.

    Ejemplo para OS X:

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/pr_firm_cluster/kube-config-prod-par02-pr_firm_cluster.yml
    ```
    {: screen}

2.  Copie y pegue el mandato que se muestra en el terminal para definir la variable de entorno `KUBECONFIG`.

3.  Compruebe que la variable de entorno `KUBECONFIG` se haya establecido correctamente.

    Ejemplo para OS X:

    ```
    echo $KUBECONFIG
    ```
    {: pre}

    Salida:

    ```
    /Users/<user_name>/.bluemix/plugins/container-service/clusters/pr_firm_cluster/kube-config-prod-par02-pr_firm_cluster.yml
    ```
    {: screen}

4.  Compruebe que los mandatos `kubectl` se ejecutan correctamente con el clúster comprobando la versión del cliente de la CLI de Kubernetes.

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

## Lección 4: Adición de un servicio al clúster
{: #cs_cluster_tutorial_lesson4}

Con los servicios de {{site.data.keyword.Bluemix_notm}}, puede aprovechar la funcionalidad ya desarrollada de sus apps. Cualquier servicio de {{site.data.keyword.Bluemix_notm}} que esté enlazado al clúster puede ser utilizado por cualquier app desplegada en ese clúster. Repita los pasos siguientes para cada servicio de {{site.data.keyword.Bluemix_notm}} que desee utilizar con sus apps.

1.  Añada el servicio {{site.data.keyword.toneanalyzershort}} a la cuenta de {{site.data.keyword.Bluemix_notm}}. Sustituya <service_name> con un nombre para su instancia de servicio.

    **Nota:** Cuando añada el servicio {{site.data.keyword.toneanalyzershort}} a su cuenta, se visualizará un mensaje que indica que el servicio no es gratuito. Si limita la llamada a la API, en esta guía de aprendizaje no se incurre en ningún cargo por el servicio {{site.data.keyword.watson}}. [Revise la información sobre precios correspondientes al servicio {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/watson/developercloud/tone-analyzer.html#pricing-block).

    ```
    bx service create tone_analyzer standard <service_name>
    ```
    {: pre}

2.  Vincule la instancia de {{site.data.keyword.toneanalyzershort}} al espacio de nombres `default` de Kubernetes correspondiente al clúster. Luego podrá crear sus propios espacios de nombres para gestionar el acceso de los usuarios a los recursos de Kubernetes, pero por ahora utilice el espacio de nombres `default`. Los espacios de nombres de Kubernetes son diferentes del espacio de nombres del registro que ha creado anteriormente.

    ```
    bx cs cluster-service-bind <cluster_name> default <service_name>
    ```
    {: pre}

    Salida:

    ```
    bx cs cluster-service-bind pr_firm_cluster default mytoneanalyzer
    Binding service instance to namespace...
    OK
    Namespace:	default
    Secret name:	binding-mytoneanalyzer
    ```
    {: screen}

3.  Verifique que el secreto de Kubernetes se ha creado en el espacio de nombres del clúster. Cada servicio de {{site.data.keyword.Bluemix_notm}} se define mediante un archivo JSON que incluye información confidencial como, por ejemplo, el nombre de usuario, la contraseña y el URL que el contenedor utiliza para obtener acceso. Para almacenar esta información de forma segura, se utilizan secretos de Kubernetes. En este ejemplo, el secreto incluye las credenciales para acceder a la instancia de {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} suministrada en su cuenta.

    ```
    kubectl get secrets --namespace=default
    ```
    {: pre}

    Salida:

    ```
    NAME                       TYPE                                  DATA      AGE
    binding-mytoneanalyzer     Opaque                                1         1m
    bluemix-default-secret     kubernetes.io/dockercfg               1         1h
    default-token-kf97z        kubernetes.io/service-account-token   3         1h
    ```
    {: screen}

</br>
¡Buen trabajo! El clúster está configurado y su entorno local está listo para que inicie el despliegue de apps en el clúster.

## ¿Qué es lo siguiente?
{: #next}

* Probar sus conocimientos y [responder a un cuestionario ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://ibmcloud-quizzes.mybluemix.net/containers/cluster_tutorial/quiz.php)

* Probar la [Guía de aprendizaje: Despliegue de apps en clústeres de Kubernetes en {{site.data.keyword.containershort_notm}}](cs_tutorials_apps.html#cs_apps_tutorial) para desplegar la app de la empresa PR en el clúster que ha creado.
