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





# Creación de una señal de {{site.data.keyword.registryshort_notm}} para un registro de imágenes de {{site.data.keyword.Bluemix_dedicated_notm}}
{: #cs_dedicated_tokens}

Cree una señal que no caduque para un registro de imágenes que pueda utilizar para grupos escalables y únicos con clústeres en {{site.data.keyword.containerlong}}.
{:shortdesc}

1.  Inicie sesión en el entorno de {{site.data.keyword.Bluemix_dedicated_notm}}.

    ```
    bx login -a api.<dedicated_domain>
    ```
    {: pre}

2.  Solicite una `oauth-token` para la sesión actual y guárdela como una variable.

    ```
    OAUTH_TOKEN=`bx iam oauth-tokens | awk 'FNR == 2 {print $3 " " $4}'`
    ```
    {: pre}

3.  Solicite el ID de la organización para la sesión actual y guárdela como una variable.

    ```
    ORG_GUID=`bx iam org <org_name> --guid`
    ```
    {: pre}

4.  Solicite una señal de registro permanente para la sesión actual. Sustituya su <dedicated_domain> con el dominio de su entorno {{site.data.keyword.Bluemix_dedicated_notm}}. Esta señal otorga acceso a las imágenes en el espacio de nombres actual.

    ```
    curl -XPOST -H "Authorization: ${OAUTH_TOKEN}" -H "Organization: ${ORG_GUID}" https://registry.<dedicated_domain>/api/v1/tokens?permanent=true
    ```
    {: pre}

    Salida:

    ```
    {
        "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI2MzdiM2Q4Yy1hMDg3LTVhZjktYTYzNi0xNmU3ZWZjNzA5NjciLCJpc3MiOiJyZWdpc3RyeS5jZnNkZWRpY2F0ZWQxLnVzLXNvdXRoLmJsdWVtaXgubmV0"
    }
    ```
    {: screen}

5.  Verifique el secreto de Kubernetes.

    ```
    kubectl describe secrets
    ```
    {: pre}

    Utilice este secreto para trabajar con {{site.data.keyword.containerlong}}.

6.  Cree el secreto de Kubernetes para almacenar la información de la señal.

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_url> --docker-username=token --docker-password=<token_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>Descripción de los componentes de este mandato</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Descripción de los componentes de este mandato</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace &lt;kubernetes_namespace&gt;</code></td>
    <td>Obligatorio. El espacio de nombres de Kubernetes del clúster en el que desea utilizar el secreto y desplegar los contenedores. Ejecute <code>kubectl get namespaces</code> para obtener una lista de todos los espacios de nombres del clúster.</td>
    </tr>
    <tr>
    <td><code>&lt;secret_name&gt;</code></td>
    <td>Obligatorio. El nombre que desea utilizar para su imagePullSecret.</td>
    </tr>
    <tr>
    <td><code>--docker-server &lt;registry_url&gt;</code></td>
    <td>Obligatorio. URL del registro de imágenes en donde está configurado su espacio de nombres: registry.&lt;dedicated_domain&gt;</li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username &lt;docker_username&gt;</code></td>
    <td>Obligatorio. El nombre de usuario para iniciar una sesión en su registro privado.</td>
    </tr>
    <tr>
    <td><code>--docker-password &lt;token_value&gt;</code></td>
    <td>Obligatorio. El valor de la señal de registro que ha recuperado anteriormente.</td>
    </tr>
    <tr>
    <td><code>--docker-email &lt;docker-email&gt;</code></td>
    <td>Obligatorio. Si tiene una, especifique la dirección de correo electrónico de Docker. Si no tiene una, especifique una dirección de correo electrónico ficticia, como por ejemplo a@b.c. Este correo electrónico es obligatorio para crear un secreto de Kubernetes, pero no se utiliza después de la creación.</td>
    </tr>
    </tbody></table>

7.  Cree un pod que haga referencia a imagePullSecret.

    1.  Abra el editor de texto que prefiera y cree un script de configuración de pod llamado mypod.yaml.
    2.  Defina el pod y el imagePullSecret que desea utilizar para acceder al registro. Para utilizar una imagen privada de un espacio de nombres:

        ```
        apiVersion: v1
        kind: Pod
        metadata:
          name: <pod_name>
        spec:
          containers:
            - name: <container_name>
              image: registry.<dedicated_domain>/<my_namespace>/<my_image>:<tag>
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
        <td><code>&lt;pod_name&gt;</code></td>
        <td>El nombre del pod que desea crear.</td>
        </tr>
        <tr>
        <td><code>&lt;container_name&gt;</code></td>
        <td>El nombre del contenedor que desea desplegar en el clúster.</td>
        </tr>
        <tr>
        <td><code>&lt;my_namespace&gt;</code></td>
        <td>El espacio de nombres donde se almacena la imagen. Para obtener una lista de los espacios de nombres disponibles, ejecute `bx cr namespace-list`.</td>
        </tr>
        <td><code>&lt;my_image&gt;</code></td>
        <td>El nombre del imagen que desea utilizar. Para ver una lista de las imágenes disponibles en una cuenta de {{site.data.keyword.Bluemix_notm}}, ejecute <code>bx cr image-list</code>.</td>
        </tr>
        <tr>
        <td><code>&lt;tag&gt;</code></td>
        <td>La versión de la imagen que desea utilizar. Si no se especifica ninguna etiqueta, se utiliza la imagen etiquetada como <strong>la más reciente (latest)</strong>.</td>
        </tr>
        <tr>
        <td><code>&lt;secret_name&gt;</code></td>
        <td>El nombre del imagePullSecret que ha creado anteriormente.</td>
        </tr>
        </tbody></table>

    3.  Guarde los cambios.

    4.  Cree el despliegue en el clúster.

          ```
          kubectl apply -f mypod.yaml
          ```
          {: pre}

