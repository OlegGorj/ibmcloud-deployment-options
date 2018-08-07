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


# Registro y supervisión
{: #health}

Configure el registro y la supervisión en {{site.data.keyword.containerlong}} para ayudarle a resolver los problemas y mejorar el estado y el rendimiento de las apps y los clústeres de Kubernetes.
{: shortdesc}


## Configuración del clúster y reenvío de registro de app
{: #logging}

Con un clúster de Kubernetes estándar en {{site.data.keyword.containershort_notm}}, puede reenviar registros de diferentes orígenes a {{site.data.keyword.loganalysislong_notm}}, a un servidor syslog externo o a ambos.
{: shortdesc}

Si desea reenviar los registros de un origen a ambos servidores del recopilador, debe crear dos configuraciones de registro.
{: tip}

Consulte la tabla siguiente para obtener información sobre los diferentes orígenes de registro.

<table>
<caption>Orígenes de registro</caption>
  <thead>
    <tr>
      <th>Origen de registro</th>
      <th>Características</th>
      <th>Vías de acceso de registro</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>container</code></td>
      <td>Registra el contenedor que se ejecuta en un clúster de Kubernetes. Todos que se registra en STDOUT o STDERR.</td>
      <td> </td>
    </tr>
    <tr>
      <td><code>application</code></td>
      <td>Registra la aplicación que se ejecuta en un clúster de Kubernetes.</td>
      <td><p>Puede definir las vías de acceso. Para que se puedan enviar los registros, se debe utilizar una vía de acceso absoluta en la configuración de registro o de lo contrario, no se podrán leer los registros. Si la vía de acceso está montada en su nodo trabajador, podría haber creado un enlace simbólico.</p>
      <p>Ejemplo: Si la vía de acceso especificada es <code>/usr/local/<b>spark</b>/work/app-0546/0/stderr</code> pero los registros en realidad van a <code>/usr/local/<b>spark-1.0-hadoop-1.2</b>/work/app-0546/0/stderr</code>, entonces no se podrán leer los registros.</p></td>
    </tr>
    <tr>
      <td><code>worker</code></td>
      <td>Registra nodos trabajadores de máquinas virtuales dentro de un clúster de Kubernetes.</td>
      <td><code>/var/log/syslog</code>, <code>/var/log/auth.log</code></td>
    </tr>
    <tr>
      <td><code>kubernetes</code></td>
      <td>Registra el componente del sistema de Kubernetes.</td>
      <td><code>/var/log/kubelet.log</code>, <code>/var/log/kube-proxy.log</code>, <code>/var/log/event-exporter/&ast;.log</code></td>
    </tr>
    <tr>
      <td><code>ingress</code></td>
      <td>Registra un equilibrador de carga de aplicación de Ingress que gestiona el tráfico de red que entra en un clúster.</td>
      <td><code>/var/log/alb/ids/&ast;.log</code>, <code>/var/log/alb/ids/&ast;.err</code>, <code>/var/log/alb/customerlogs/&ast;.log</code>, <code>/var/log/alb/customerlogs/&ast;.err</code></td>
    </tr>
    <tr>
      <td><code>kube-audit</code></td>
      <td>Registros para el servidor de API de Kubernetes.</td>
      <td> </td>
    </tr>
  </tbody>
</table>

Si desea habilitar la creación de registros a nivel de cuenta o configurar la creación de registros de las aplicaciones, utilice la CLI.
{: tip}


### Habilitación del reenvío de registros con la GUI
{: #enable-forwarding-ui}

Puede configurar los registros en el panel de control de {{site.data.keyword.containershort_notm}}. Puede llevar varios minutos el completar el proceso, por lo que si no ve los registros de forma inmediata, espere un poco más y compruébelo de nuevo.

1. Vaya al separador **Visión general** del panel de control.
2. Seleccione la organización Cloud Foundry y el espacio del que desea reenviar registros. Cuando configura el reenvío de registros en el panel de control, los registros se envían al punto final predeterminado de {{site.data.keyword.loganalysisshort_notm}} para su clúster. Para reenviar registros a un servidor externo, o a otro punto final de {{site.data.keyword.loganalysisshort_notm}}, puede utilizar la CLI para configurar la creación de registros.
3. Seleccione los orígenes de registro para los que desea reenviar registros.

    Para configurar el registro de aplicaciones o de espacios de nombres de contenedor específicos, utilice la CLI para configurar su configuración de creación de registros.
    {: tip}
4. Pulse **Crear**.

### Habilitación del reenvío de registros con la CLI
{: #enable-forwarding}

Puede crear una configuración para los registros del clúster. Puede diferenciar entre diferentes orígenes de registro mediante distintivos. Revise una lista completa de las opciones de configuración en la [Referencia de CLI](cs_cli_reference.html#logging_commands).

**Antes de empezar**

1. Verifique los permisos. Si ha especificado un espacio al crear el clúster o la configuración de registro, tanto el propietario de la cuenta como el propietario de claves de {{site.data.keyword.containershort_notm}} necesitan permisos de gestor, desarrollador o auditor en ese espacio.
  * Si desconoce quién es el propietario de claves de {{site.data.keyword.containershort_notm}}, ejecute el mandato siguiente.
      ```
      bx cs api-key-info <cluster_name>
      ```
      {: pre}
  * Para aplicar inmediatamente los cambios que ha realizado en los permisos, ejecute el mandato siguiente.
      ```
      bx cs logging-config-refresh <cluster_name>
      ```
      {: pre}

  Para obtener más información sobre cómo cambiar políticas de acceso y permisos de {{site.data.keyword.containershort_notm}}, consulte [Gestión de acceso a clústeres](cs_users.html#access_policies).
  {: tip}

2. Elija como [destino de la CLI](cs_cli_install.html#cs_cli_configure) el clúster en el que se encuentra el origen de registro.

  Si utiliza una cuenta dedicada, debe iniciar sesión en el punto final de {{site.data.keyword.cloud_notm}} público y definir como objetivo el espacio y la organización públicos para permitir el reenvío de registros.
  {: tip}

3. Para reenviar registros a syslog, configure un servidor que acepte un protocolo syslog de una de estas dos maneras:
  * Puede configurar y gestionar su propio servidor o dejar que lo gestione un proveedor. Si un proveedor gestiona el servidor, obtenga el punto final de registro del proveedor de registro. El servidor de syslog debe dar soporte al protocolo UDP.
  * Puede ejecutar syslog desde un contenedor. Por ejemplo, puede utilizar este archivo [deployment .yaml ![Icono de archivo externo](../icons/launch-glyph.svg "Icono de archivo externo")](https://github.com/IBM-Cloud/kube-samples/blob/master/deploy-apps-clusters/deploy-syslog-from-kube.yaml) para obtener una imagen pública de Docker que ejecute un contenedor en un clúster de Kubernetes. La imagen publica el puerto `514` en la dirección IP del clúster público y utiliza esta dirección IP del clúster público para configurar el host de syslog.

    Puede eliminar los prefijos de syslog para ver los registros como JSON válido añadiendo el siguiente código en la parte superior del archivo `etc/rsyslog.conf` donde se ejecuta el servidor de syslog.</br>
    ```$template customFormat,"%msg%\n"
    $ActionFileDefaultTemplate customFormat
    ```
    {: tip}


**Reenvío de registros**

1. Cree una configuración de reenvío de registro.
  ```
  bx cs logging-config-create <cluster_name_or_ID> --logsource <log_source> --namespace <kubernetes_namespace> --hostname <log_server_hostname_or_IP> --port <log_server_port> --type <type> --app-containers <containers> --app-paths <paths_to_logs> --syslog-protocol <protocol> --skip-validation
  ```
  {: pre}

    * Ejemplo de configuración de registro de contenedor para la salida y el espacio de nombres predeterminado:
      ```
      bx cs logging-config-create cluster1 --namespace default
      Creating logging configuration for container logs in cluster cluster1...
      OK
      Id                                     Source      Namespace   Host                                 Port    Org   Space   Protocol   Application Containers   Paths
      af7d1ff4-33e6-4275-8167-b52eb3c5f0ee   container   default     ingest-au-syd.logging.bluemix.net✣  9091✣   -     -       ibm        -                        -

      ✣ Indicates the default endpoint for the {{site.data.keyword.loganalysislong_notm}} service.

      ```
      {: screen}

    * Ejemplo de salida y configuración de registro de aplicación:
      ```
      bx cs logging-config-create cluster2 --logsource application --app-paths '/var/log/apps.log' --app-containers 'container1,container2,container3'
      Creating logging configuration for application logs in cluster cluster2...
      OK
      Id                                     Source        Namespace   Host                                    Port    Org   Space   Protocol   Application   Containers   Paths
      aa2b415e-3158-48c9-94cf-f8b298a5ae39   application   -           ingest.logging.stage1.ng.bluemix.net✣  9091✣   -     -       ibm        container1,container2,container3   /var/log/apps.log
      ```
      {: screen}

      Si tiene apps que se ejecuten en contenedores que no es posible configurar para grabar registros en STDOUT o STDERR, puede crear una configuración de registro para reenviar los registros desde los archivos de registro de dichas apps.
      {: tip}

  <table>
  <caption>Descripción de los componentes de este mandato</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Descripción de los componentes de este mandato</th>
    </thead>
    <tbody>
      <tr>
        <td><code><em>&lt;cluster_name_or_ID&gt;</em></code></td>
        <td>El nombre o ID del clúster.</td>
      </tr>
      <tr>
        <td><code><em>&lt;log_source&gt;</em></code></td>
        <td>El origen desde el que desea reenviar los registros. Los valores aceptados son <code>container</code>, <code>application</code>, <code>worker</code>, <code>kubernetes</code>, <code>ingress</code> y <code>kube-audit</code>.</td>
      </tr>
      <tr>
        <td><code><em>&lt;kubernetes_namespace&gt;</em></code></td>
        <td>Opcional: El espacio de nombres de Kubernetes desde el que desea reenviar los registros. El reenvío de registros no recibe soporte para los espacios de nombres de Kubernetes <code>ibm-system</code> y <code>kube-system</code>. Este valor sólo es válido para el origen de registro <code>container</code>. Si no especifica un espacio de nombres, todos los espacios de nombres del clúster utilizarán esta configuración.</td>
      </tr>
      <tr>
        <td><code><em>&lt;hostname_or_ingestion_URL&gt;</em></code></td>
        <td><p>Para {{site.data.keyword.loganalysisshort_notm}}, utilice el [URL de ingestión](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls). Si no especifica un URL de ingestión, se utiliza el punto final de la región en la que ha creado el clúster.</p>
        <p>Para syslog, especifique el nombre de host o la dirección IP del servicio del recopilador de registros.</p></td>
      </tr>
      <tr>
        <td><code><em>&lt;port&gt;</em></code></td>
        <td>El puerto de ingestión. Si no especifica un puerto, se utiliza el puerto estándar, <code>9091</code>.
        <p>Para syslog, especifique el puerto del servicio del recopilador de registros. Si no especifica un puerto, se utiliza el puerto estándar, <code>514</code>.</td>
      </tr>
      <tr>
        <td><code><em>&lt;cluster_space&gt;</em></code></td>
        <td>Opcional: El nombre del espacio de Cloud Foundry al que desea enviar registros. Al reenviar registros a {{site.data.keyword.loganalysisshort_notm}}, el espacio y la organización se especifican en el punto de ingestión. Si no especifica un espacio, los registros se envían al nivel de cuenta.</td>
      </tr>
      <tr>
        <td><code><em>&lt;cluster_org&gt;</em></code></td>
        <td>El nombre de la organización de Cloud Foundry en la que está el espacio. Este valor es necesario si ha especificado un espacio.</td>
      </tr>
      <tr>
        <td><code><em>&lt;type&gt;</em></code></td>
        <td>El destino al que desea reenviar los registros. Las opciones son <code>ibm</code>, que reenvía los registros a {{site.data.keyword.loganalysisshort_notm}} y <code>syslog</code>, que reenvía los registros a un servidor externo.</td>
      </tr>
      <tr>
        <td><code><em>&lt;paths_to_logs&gt;</em></code></td>
        <td>Vía de acceso en el contenedor en la que las apps crearán los registros. Para reenviar registros con el tipo de origen <code>application</code>, debe proporcionar una vía de acceso. Para especificar más de una vía de acceso, utilice una lista separada por comas. Ejemplo: <code>/var/log/myApp1/&ast;,/var/log/myApp2/&ast;</code></td>
      </tr>
      <tr>
        <td><code><em>&lt;containers&gt;</em></code></td>
        <td>Opcional: Para reenviar registros de apps, puede especificar el nombre del contenedor que contiene la app. Puede especificar más de un contenedor mediante una lista separada por comas. Si no se especifica ningún contenedor, se reenvían los registros de todos los contenedores que contienen las vías de acceso indicadas.</td>
      </tr>
      <tr>
        <td><code><em>&lt;protocol&gt;</em></code></td>
        <td>Cuando el tipo de registro es <code>syslog</code>, el protocolo de la capa de transporte. Los valores soportados son <code>TCP</code> y el predeterminado <code>UDP</code>. Al reenviar a un servidor syslog con el protocolo <code>udp</code>, se truncan los registros por encima de 1 KB.</td>
      </tr>
      <tr>
        <td><code><em>--skip-validation</em></code></td>
        <td>Opcional: Omite la validación de los nombres de espacio y organización cuando se especifican. Al omitir la validación, disminuye el tiempo de proceso, pero si la configuración de registro no es válida, los registros no se reenviarán correctamente.</td>
      </tr>
    </tbody>
  </table>

2. Verifique que la configuración es correcta de una de estas maneras:

    * Para obtener una lista de todas las configuraciones de registro en un clúster:
      ```
      bx cs logging-config-get <cluster_name_or_ID>
      ```
      {: pre}

      Salida de ejemplo:

      ```
      Id                                    Source       Namespace     Host                          Port   Org      Space      Protocol     Paths
      f4bc77c0-ee7d-422d-aabf-a4e6b977264e  kubernetes   -             172.30.xxx.xxx                5514   -        -          syslog       /var/log/kubelet.log,/var/log/kube-proxy.log
      5bd9c609-13c8-4c48-9d6e-3a6664c825a9  application  -             ingest.logging.ng.bluemix.net 9091   my_org   my_space   ibm          /var/log/apps/**/*.log,/var/log/apps/**/*.err
      8a284f1a-451c-4c48-b1b4-a4e6b977264e  containers   my-namespace  myhostname.common             5514   -        -          syslog       -
      ```
      {: screen}

    * Para obtener una lista de las configuraciones de registro para un tipo de origen de registro:
      ```
      bx cs logging-config-get <cluster_name_or_ID> --logsource worker
      ```
      {: pre}

      Salida de ejemplo:

      ```
      Id                                    Source    Namespace   Host                            Port   Org    Space     Protocol    Paths
      f4bc77c0-ee7d-422d-aabf-a4e6b977264e  worker    -           ingest.logging.ng.bluemix.net   9091   -      -         ibm         /var/log/syslog,/var/log/auth.log
      5bd9c609-13c8-4c48-9d6e-3a6664c825a9  worker    -           172.30.xxx.xxx                  5514   -      -         syslog      /var/log/syslog,/var/log/auth.log
      ```
      {: screen}

## Actualización del reenvío de registros
{: #updating-forwarding}

1. Actualice una configuración de reenvío de registro.
    ```
    bx cs logging-config-update <cluster_name_or_ID> <log_config_id> --namespace <namespace> --type <log_type> --logsource <source> --hostname <hostname_or_ingestion_URL> --port <port> --space <cluster_space> --org <cluster_org> --app-containers <containers> --app-paths <paths_to_logs>
    ```
    {: pre}

  <table>
  <caption>Descripción de los componentes de este mandato</caption>
  <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Descripción de los componentes de este mandato</th>
  </thead>
  <tbody>
    <tr>
      <td><code><em>&lt;cluster_name_or_ID&gt;</em></code></td>
      <td>El nombre o ID del clúster.</td>
    </tr>
    <tr>
      <td><code><em>&lt;log_config_id&gt;</em></code></td>
      <td>El ID de la configuración que desea actualizar.</td>
    </tr>
    <tr>
      <td><code><em>&lt;namespace&gt;</em></code></td>
      <td>Opcional: El espacio de nombres de Kubernetes desde el que reenviar los registros. El reenvío de registros no recibe soporte para los espacios de nombres de Kubernetes <code>ibm-system</code> y <code>kube-system</code>. Este valor sólo es válido para el origen de registro <code>container</code>. Si no especifica un espacio de nombres, todos los espacios de nombres en el clúster utilizarán la configuración.</td>
    </tr>
    <tr>
      <td><code><em>&lt;log_type&gt;</em></code></td>
      <td>El destino al que desea reenviar los registros. Las opciones son <code>ibm</code>, que reenvía los registros a {{site.data.keyword.loganalysisshort_notm}} y <code>syslog</code>, que reenvía los registros a un servidor externo.</td>
    </tr>
    <tr>
      <td><code><em>&lt;hostname_or_ingestion_URL&gt;</em></code></td>
      <td><p>Para {{site.data.keyword.loganalysisshort_notm}}, utilice el [URL de ingestión](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls). Si no especifica un URL de ingestión, se utiliza el punto final de la región en la que ha creado el clúster.</p>
      <p>Para syslog, especifique el nombre de host o la dirección IP del servicio del recopilador de registros.</p></td>
    </tr>
    <tr>
      <td><code><em>&lt;port&gt;</em></code></td>
      <td>El puerto de ingestión. Si no especifica un puerto, se utiliza el puerto estándar, <code>9091</code>.
      <p>Para syslog, especifique el puerto del servicio del recopilador de registros. Si no especifica un puerto, se utiliza el puerto estándar, <code>514</code>.</td>
    </tr>
    <tr>
      <td><code><em>&lt;cluster_space&gt;</em></code></td>
      <td>Opcional: El nombre del espacio de Cloud Foundry al que desea enviar registros. Al reenviar registros a {{site.data.keyword.loganalysisshort_notm}}, el espacio y la organización se especifican en el punto de ingestión. Si no especifica un espacio, los registros se envían al nivel de cuenta.</td>
    </tr>
    <tr>
      <td><code><em>&lt;cluster_org&gt;</em></code></td>
      <td>El nombre de la organización de Cloud Foundry en la que está el espacio. Este valor es necesario si ha especificado un espacio.</td>
    </tr>
    <tr>
      <td><code><em>&lt;paths_to_logs&gt;</em></code></td>
      <td>La vía de acceso al contenedor o contenedores donde las apps realizan registros. Para reenviar registros con el tipo de origen <code>application</code>, debe proporcionar una vía de acceso. Para especificar más de una vía de acceso, utilice una lista separada por comas. Ejemplo: <code>/var/log/myApp1/&ast;,/var/log/myApp2/&ast;</code></td>
    </tr>
    <tr>
      <td><code><em>&lt;containers&gt;</em></code></td>
      <td>Opcional: Para reenviar registros de apps, puede especificar el nombre del contenedor que contiene la app. Puede especificar más de un contenedor mediante una lista separada por comas. Si no se especifica ningún contenedor, se reenvían los registros de todos los contenedores que contienen las vías de acceso indicadas.</td>
    </tr>
  </tbody>
  </table>

<br />



## Filtrado de registros
{: #filter-logs}

Puede elegir qué registros reenviará filtrando registros específicos durante un periodo de tiempo.

1. Cree un filtro de registro.
  ```
  bx cs logging-filter-create <cluster_name_or_ID> --type <log_type> --logging-configs <configs> --namespace <kubernetes_namespace> --container <container_name> --level <logging_level> --regex-message <message>
  ```
  {: pre}
  <table>
  <caption>Descripción de los componentes de este mandato</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Descripción de los componentes de este mandato</th>
    </thead>
    <tbody>
      <tr>
        <td>&lt;cluster_name_or_ID&gt;</td>
        <td>Obligatorio: Nombre o ID de clúster que desea para crear un filtro de registro.</td>
      </tr>
      <tr>
        <td><code>&lt;log_type&gt;</code></td>
        <td>Tipo de registro al que aplicar el filtro. Actualmente se da soporte a <code>all</code>, <code>container</code> y <code>host</code>.</td>
      </tr>
      <tr>
        <td><code>&lt;configs&gt;</code></td>
        <td>Opcional: Una lista separada por comas de los ID de configuración de registro. Si no se proporciona, el filtro se aplica a todas las configuraciones de registro de clúster que se pasan al filtro. Puede ver las configuraciones de registro que coinciden con el filtro utilizando con el mandato el distintivo <code>--show-matching-configs</code>.</td>
      </tr>
      <tr>
        <td><code>&lt;kubernetes_namespace&gt;</code></td>
        <td>Opcional: El espacio de nombres de Kubernetes desde el que desea reenviar los registros. Este distintivo sólo se aplica cuando se utiliza el tipo de registro de <code>container</code>.</td>
      </tr>
      <tr>
        <td><code>&lt;container_name&gt;</code></td>
        <td>Opcional: Nombre del contenedor desde el que desea filtrar registros.</td>
      </tr>
      <tr>
        <td><code>&lt;logging_level&gt;</code></td>
        <td>Opcional: Filtra los registros en el nivel especificado y en los inferiores. Valores aceptables en su orden canónico son <code>fatal</code>, <code>error</code>, <code>warn/warning</code>, <code>info</code>, <code>debug</code> y <code>trace</code>. Por ejemplo, si filtra registros al nivel <code>info</code>, también se filtran los niveles <code>debug</code> y <code>trace</code>. **Nota**: Puede utilizar este distintivo sólo cuando los mensajes de registro están en formato JSON y contienen un campo de nivel. Para visualizar los mensajes en JSON, añada el distintivo <code>-- json</code> al mandato.</td>
      </tr>
      <tr>
        <td><code>&lt;message&gt;</code></td>
        <td>Opcional: Filtra los registros que contienen un mensaje concreto que se escribe como una expresión regular.</td>
      </tr>
    </tbody>
  </table>

2. Visualice el filtro de registro que haya creado.

  ```
  bx cs logging-filter-get <cluster_name_or_ID> --id <filter_ID> --show-matching-configs
  ```
  {: pre}
  <table>
  <caption>Descripción de los componentes de este mandato</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Descripción de los componentes de este mandato</th>
    </thead>
    <tbody>
      <tr>
        <td>&lt;cluster_name_or_ID&gt;</td>
        <td>Obligatorio: Nombre o ID de clúster que desea para crear un filtro de registro.</td>
      </tr>
      <tr>
        <td><code>&lt;filter_ID&gt;</code></td>
        <td>Opcional: ID del filtro de registro que desea visualizar.</td>
      </tr>
      <tr>
        <td><code>--show-matching-configs</code></td>
        <td>Muestra las configuraciones de registro que se aplican a cada filtro.</td>
      </tr>
    </tbody>
  </table>

3. Actualice el filtro de registro que haya creado.
  ```
  bx cs logging-filter-update <cluster_name_or_ID> --id <filter_ID> --type <log_type> --logging-configs <configs> --namespace <kubernetes_namespace --container <container_name> --level <logging_level> --regex-message <message>
  ```
  {: pre}
  <table>
  <caption>Descripción de los componentes de este mandato</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Descripción de los componentes de este mandato</th>
    </thead>
    <tbody>
      <tr>
        <td>&lt;cluster_name_or_ID&gt;</td>
        <td>Obligatorio: Nombre o ID de clúster que desea para actualizar un filtro de registro.</td>
      </tr>
      <tr>
        <td><code>&lt;filter_ID&gt;</code></td>
        <td>El ID del filtro de registro que desea actualizar.</td>
      </tr>
      <tr>
        <td><code><&lt;log_type&gt;</code></td>
        <td>Tipo de registro al que aplicar el filtro. Actualmente se da soporte a <code>all</code>, <code>container</code> y <code>host</code>.</td>
      </tr>
      <tr>
        <td><code>&lt;configs&gt;</code></td>
        <td>Opcional: Una lista separada por comas de todos los ID de configuración de registro a los que desea aplicar el filtro. Si no se proporciona, el filtro se aplica a todas las configuraciones de registro de clúster que se pasan al filtro. Puede ver las configuraciones de registro que coinciden con el filtro utilizando el distintivo <code>--show-matching-configs</code> con el mandato <code>bx cs logging-filter-get</code>.</td>
      </tr>
      <tr>
        <td><code>&lt;kubernetes_namespace&gt;</code></td>
        <td>Opcional: El espacio de nombres de Kubernetes desde el que desea reenviar los registros. Este distintivo sólo se aplica cuando se utiliza el tipo de registro de <code>container</code>.</td>
      </tr>
      <tr>
        <td><code>&lt;container_name&gt;</code></td>
        <td>Opcional: Nombre del contenedor desde el que desea filtrar registros. Este distintivo sólo se aplica cuando se utiliza el tipo de registro de <code>container</code>.</td>
      </tr>
      <tr>
        <td><code>&lt;logging_level&gt;</code></td>
        <td>Opcional: Filtra los registros en el nivel especificado y en los inferiores. Valores aceptables en su orden canónico son <code>fatal</code>, <code>error</code>, <code>warn/warning</code>, <code>info</code>, <code>debug</code> y <code>trace</code>. Por ejemplo, si filtra registros al nivel <code>info</code>, también se filtran los niveles <code>debug</code> y <code>trace</code>. **Nota**: Puede utilizar este distintivo sólo cuando los mensajes de registro están en formato JSON y contienen un campo de nivel.</td>
      </tr>
      <tr>
        <td><code>&lt;message&gt;</code></td>
        <td>Opcional: Filtra los registros que contienen un mensaje concreto que se escribe como una expresión regular.</td>
      </tr>
    </tbody>
  </table>

4. Suprime un filtro de registro que haya creado.

  ```
  bx cs logging-filter-rm <cluster_name_or_ID> --id <filter_ID> [--all]
  ```
  {: pre}
  <table>
  <caption>Descripción de los componentes de este mandato</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Descripción de los componentes de este mandato</th>
    </thead>
    <tbody>
      <tr>
        <td><code>&lt;cluster_name_or_ID&gt;</code></td>
        <td>Obligatorio: Nombre o ID de clúster que desea para suprimir un filtro de registro.</td>
      </tr>
      <tr>
        <td><code>&lt;filter_ID&gt;</code></td>
        <td>Opcional: ID del filtro de registro que desea eliminar.</td>
      </tr>
      <tr>
        <td><code>--all</code></td>
        <td>Opcional: Suprimir todos filtros de reenvío de registro.</td>
      </tr>
    </tbody>
  </table>

<br />




## Visualización de registros
{: #view_logs}

Para ver los registros de clústeres y contenedores, puede utilizar las características estándares de registro de Kubernetes y Docker.
{:shortdesc}

### {{site.data.keyword.loganalysislong_notm}}
{: #view_logs_k8s}

Puede ver los registros que se reenvían a {{site.data.keyword.loganalysislong_notm}} mediante el panel de control de Kibana.
{: shortdesc}

Si ha utilizado los valores predeterminados para crear el archivo de configuración, entonces los registros pueden encontrarse en la cuenta, o la organización y el espacio, en los que se ha creado el clúster. Si ha especificado una organización y un espacio en el archivo de configuración, puede encontrar los registros en ese espacio. Para obtener más información sobre el registro, consulte [Registro para {{site.data.keyword.containershort_notm}}](/docs/services/CloudLogAnalysis/containers/containers_kubernetes.html#containers_kubernetes).

Para acceder al panel de control de Kibana, vaya a uno de los siguientes URL y seleccione la cuenta o el espacio de {{site.data.keyword.Bluemix_notm}} en el que haya configurado el reenvío de registro para el clúster.
- EE.UU. sur y EE.UU. este: https://logging.ng.bluemix.net
- UK sur: https://logging.eu-gb.bluemix.net
- UE central: https://logging.eu-fra.bluemix.net
- AP sur: https://logging.au-syd.bluemix.net

Para obtener más información sobre la visualización de registros, consulte [Navegación a Kibana desde un navegador web](/docs/services/CloudLogAnalysis/kibana/launch.html#launch_Kibana_from_browser).

### Registros de Docker
{: #view_logs_docker}

Puede aprovechar las funciones incorporadas de registro de Docker para revisar las actividades de las secuencias de salida estándar STDOUT y STDERR. Para obtener más información, consulte [Visualización de registros de contenedor para un contenedor que se ejecute en un clúster Kubernetes](/docs/services/CloudLogAnalysis/containers/containers_kubernetes.html#containers_kubernetes).

<br />



## Detención del reenvío de registros
{: #log_sources_delete}

Puede detener el reenvío de registros de una o de todas las configuraciones de registro de un clúster.
{: shortdesc}

1. Elija como [destino de la CLI](cs_cli_install.html#cs_cli_configure) el clúster en el que se encuentra el origen de registro.

2. Suprima la configuración de registro.
<ul>
<li>Para suprimir una configuración de registro:</br>
  <pre><code>bx cs logging-config-rm &lt;cluster_name_or_ID&gt; --id &lt;log_config_ID&gt;</pre></code>
  <table>
  <caption>Descripción de los componentes de este mandato</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Descripción de los componentes de este mandato</th>
    </thead>
    <tbody>
      <tr>
        <td><code><em>&lt;cluster_name_or_ID&gt;</em></code></td>
        <td>El nombre del clúster en el que está la configuración de registro.</td>
      </tr>
      <tr>
        <td><code><em>&lt;log_config_ID&gt;</em></code></td>
        <td>El ID de la configuración del origen de registro.</td>
      </tr>
    </tbody>
  </table></li>
<li>Para suprimir todas las configuraciones de registro:</br>
  <pre><code>bx cs logging-config-rm <my_cluster> --all</pre></code></li>
</ul>

<br />



## Configuración de reenvío de registros para registros de auditoría de API de Kubernetes
{: #api_forward}

Kubernetes audita de forma automática los sucesos que pasan a través de su apiserver. Los sucesos se pueden reenviar a {{site.data.keyword.loganalysisshort_notm}} o a un servidor externo.
{: shortdesc}


Para obtener más información sobre los registros de auditoría de Kubernetes, consulte el <a href="https://kubernetes.io/docs/tasks/debug-application-cluster/audit/" target="blank">tema sobre auditoría <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a> en la documentación de Kubernetes.

* El reenvío de registros de auditoría de API de Kubernetes sólo está admitido en la versión 1.7 de Kubernetes y posterior.
* Actualmente, se utiliza una política de auditoría predeterminada para todos los clústeres con esta configuración de registro.
* Actualmente no se da soporte a filtros.
* Sólo puede haber una configuración `kube-audit` por clúster, sin embargo, es posible reenviar los registros a {{site.data.keyword.loganalysisshort_notm}} y a un servidor externo creando un webhook y una configuración de registro.
{: tip}


### Envío de registros de auditoría a {{site.data.keyword.loganalysisshort_notm}}
{: #audit_enable_loganalysis}

Existe la posibilidad de reenviar los registros de auditoría del servidor de API de Kubernetes a {{site.data.keyword.loganalysisshort_notm}}

**Antes de empezar**

1. Verifique los permisos. Si ha especificado un espacio al crear el clúster o la configuración de registro, tanto el propietario de la cuenta como el propietario de claves de {{site.data.keyword.containershort_notm}} necesitan permisos de gestor, desarrollador o auditor en ese espacio.

2. [Dirija su CLI](cs_cli_install.html#cs_cli_configure) al clúster del que desea recopilar registros de auditoría de servidor de API. **Nota**: Si utiliza una cuenta dedicada, debe iniciar sesión en el punto final de {{site.data.keyword.cloud_notm}} público y definir como objetivo el espacio y la organización públicos para permitir el reenvío de registros.

**Reenvío de registros**

1. Crear una configuración de registro.

    ```
    bx cs logging-config-create <cluster_name_or_ID> --logsource kube-audit --space <cluster_space> --org <cluster_org> --hostname <ingestion_URL> --type ibm
    ```
    {: pre}

    Ejemplo de mandato y su salida:

    ```
    bx cs logging-config-create myCluster --logsource kube-audit
    Creating logging configuration for kube-audit logs in cluster myCluster...
    OK
    Id                                     Source      Namespace   Host                                 Port    Org   Space   Protocol   Application Containers   Paths
    14ca6a0c-5bc8-499a-b1bd-cedcf40ab850   kube-audit  -           ingest-au-syd.logging.bluemix.net✣   9091✣   -     -       ibm        -                        -

    ✣ Indicates the default endpoint for the {{site.data.keyword.loganalysisshort_notm}} service.

    ```
    {: screen}

    <table>
    <caption>Descripción de los componentes de este mandato</caption>
      <thead>
        <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Descripción de los componentes de este mandato</th>
      </thead>
      <tbody>
        <tr>
          <td><code><em>&lt;cluster_name_or_ID&gt;</em></code></td>
          <td>El nombre o ID del clúster.</td>
        </tr>
        <tr>
          <td><code><em>&lt;ingestion_URL&gt;</em></code></td>
          <td>El punto final al que desea reenviar los registros. Si no especifica un [URL de ingestión](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls), se utilizará el punto final de la región en la que creó su clúster.</td>
        </tr>
        <tr>
          <td><code><em>&lt;cluster_space&gt;</em></code></td>
          <td>Opcional: El nombre del espacio de Cloud Foundry al que desea enviar registros. Al reenviar registros a {{site.data.keyword.loganalysisshort_notm}}, el espacio y la organización se especifican en el punto de ingestión. Si no especifica un espacio, los registros se envían al nivel de cuenta.</td>
        </tr>
        <tr>
          <td><code><em>&lt;cluster_org&gt;</em></code></td>
          <td>El nombre de la organización de Cloud Foundry en la que está el espacio. Este valor es necesario si ha especificado un espacio.</td>
        </tr>
      </tbody>
    </table>

2. Visualizar la configuración de registro para verificar que se ha implementado según lo previsto.

    ```
    bx cs logging-config-get <cluster_name_or_ID>
    ```
    {: pre}

    Ejemplo de mandato y su salida:
    ```
    bx cs logging-config-get myCluster
    Retrieving cluster myCluster logging configurations...
    OK
    Id                                     Source        Namespace   Host                                 Port    Org   Space   Protocol   Application Containers   Paths
    a550d2ba-6a02-4d4d-83ef-68f7a113325c   container     *           ingest-au-syd.logging.bluemix.net✣   9091✣   -     -       ibm        -                        -
    14ca6a0c-5bc8-499a-b1bd-cedcf40ab850   kube-audit    -           ingest-au-syd.logging.bluemix.net✣   9091✣   -     -       ibm        -                    
    ```
    {: screen}

  <table>
  <caption>Descripción de los componentes de este mandato</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Descripción de los componentes de este mandato</th>
    </thead>
    <tbody>
      <tr>
        <td><code><em>&lt;cluster_name_or_ID&gt;</em></code></td>
        <td>El nombre o ID del clúster.</td>
      </tr>
    </tbody>
  </table>

3. Opcional: Si desea detener el reenvío de registros de auditoría, puede [suprimir la configuración](#log_sources_delete).

<br />



### Envío de registros de auditoría a un servidor externo
{: #audit_enable}

**Antes de empezar**

1. Configure un servidor de registro remoto donde puede reenviar los registros. Por ejemplo, puede [utilizar Logstash con Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/debug-application-cluster/audit/#use-logstash-to-collect-and-distribute-audit-events-from-webhook-backend) para recopilar sucesos de auditoría.

2. [Dirija su CLI](cs_cli_install.html#cs_cli_configure) al clúster del que desea recopilar registros de auditoría de servidor de API. **Nota**: Si utiliza una cuenta dedicada, debe iniciar sesión en el punto final de {{site.data.keyword.cloud_notm}} público y definir como objetivo el espacio y la organización públicos para permitir el reenvío de registros.

Para reenviar registros de auditoría de API de Kubernetes:

1. Configuración del webhook. Si no proporciona ninguna información en los distintivos, se utiliza una configuración predeterminada.

    ```
    bx cs apiserver-config-set audit-webhook <cluster_name_or_ID> --remoteServer <server_URL_or_IP> --caCert <CA_cert_path> --clientCert <client_cert_path> --clientKey <client_key_path>
    ```
    {: pre}

  <table>
  <caption>Descripción de los componentes de este mandato</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Descripción de los componentes de este mandato</th>
    </thead>
    <tbody>
      <tr>
        <td><code><em>&lt;cluster_name_or_ID&gt;</em></code></td>
        <td>El nombre o ID del clúster.</td>
      </tr>
      <tr>
        <td><code><em>&lt;server_URL&gt;</em></code></td>
        <td>La dirección IP o el URL del servicio de registro remoto al que desea enviar registros. Los certificados se ignoran si se proporciona un URL de servidor no seguro.</td>
      </tr>
      <tr>
        <td><code><em>&lt;CA_cert_path&gt;</em></code></td>
        <td>La vía de acceso de archivo del certificado de CA que se utiliza para verificar el servicio de registro remoto.</td>
      </tr>
      <tr>
        <td><code><em>&lt;client_cert_path&gt;</em></code></td>
        <td>La vía de acceso de archivo del certificado de cliente que se utiliza para autenticarse en el servicio de registro remoto.</td>
      </tr>
      <tr>
        <td><code><em>&lt;client_key_path&gt;</em></code></td>
        <td>La vía de acceso de archivo de la clave de cliente correspondiente que se utiliza para conectarse al servicio de registro remoto.</td>
      </tr>
    </tbody>
  </table>

2. Verifique que el reenvío de registros se ha habilitado consultando el URL del servicio de registro remoto.

    ```
    bx cs apiserver-config-get audit-webhook <cluster_name_or_ID>
    ```
    {: pre}

    Salida de ejemplo:
    ```
    OK
    Server:			https://8.8.8.8
    ```
    {: screen}

3. Aplique la actualización de configuración reiniciando el maestro de Kubernetes.

    ```
    bx cs apiserver-refresh <cluster_name_or_ID>
    ```
    {: pre}

4. Opcional: Si desea detener el reenvío de registros de auditoría, puede inhabilitar su configuración.
    1. [Dirija su CLI](cs_cli_install.html#cs_cli_configure) al clúster del que desea detener la recopilación de registros de auditoría de servidor de API.
    2. Inhabilitar la configuración del programa de fondo del webhook para el servidor de API del clúster.

        ```
        bx cs apiserver-config-unset audit-webhook <cluster_name_or_ID>
        ```
        {: pre}

    3. Aplique la actualización de configuración reiniciando el maestro de Kubernetes.

        ```
        bx cs apiserver-refresh <cluster_name_or_ID>
        ```
        {: pre}

## Visualización de métricas
{: #view_metrics}

Las métricas le ayudan a supervisar el estado y el rendimiento de sus clústeres. Puede utilizar las funciones estándares de Kubernetes y Docker para supervisar el estado de sus clústeres y apps. **Nota**: La supervisión se soporta solo para los clústeres estándares.
{:shortdesc}

<dl>
  <dt>Página de detalles del clúster en {{site.data.keyword.Bluemix_notm}}</dt>
    <dd>{{site.data.keyword.containershort_notm}} proporciona información sobre el estado y la capacidad del clúster y sobre el uso de los recursos del clúster. Puede utilizar esta
GUI para escalar los clústeres, trabajar con el almacenamiento permanente y añadir funciones adicionales al clúster mediante la vinculación de servicios de {{site.data.keyword.Bluemix_notm}}. Para ver la página de detalles de un clúster, vaya al **Panel de control de {{site.data.keyword.Bluemix_notm}}** y seleccione un clúster.</dd>
  <dt>Panel de control de Kubernetes</dt>
    <dd>El panel de control de Kubernetes es una interfaz web administrativa que puede utilizar para revisar el estado de los nodos trabajadores, buscar recursos de Kubernetes, desplegar apps contenerizadas y resolver problemas de apps con la información de registro y supervisión. Para obtener más información sobre cómo acceder al panel de control de Kubernetes, consulte [Inicio del panel de control de Kubernetes para {{site.data.keyword.containershort_notm}}](cs_app.html#cli_dashboard).</dd>
  <dt>{{site.data.keyword.monitoringlong_notm}}</dt>
    <dd><p>En el caso de los clústeres estándares, las métricas se encuentran en el espacio de {{site.data.keyword.Bluemix_notm}} al que se inició sesión cuando se creó el clúster de Kubernetes. Si ha especificado un espacio de {{site.data.keyword.Bluemix_notm}} al crear el clúster, entonces las métricas están ubicados en el espacio en cuestión. Las métricas de contenedor se recopilan automáticamente para todos los contenedores desplegados en un clúster. Estas métricas se envían y se ponen a disponibilidad mediante Grafana. Para obtener más información sobre las métricas, consulte el tema sobre [Supervisión de {{site.data.keyword.containershort_notm}}](/docs/services/cloud-monitoring/containers/monitoring_containers_ov.html#monitoring_bmx_containers_ov).</p>
    <p>Para acceder al panel de control de Grafana, vaya a uno de los siguientes URL y seleccione la cuenta o espacio de {{site.data.keyword.Bluemix_notm}} en la que ha creado el clúster.
      <ul>
        <li>EE.UU. Sur y EE.UU. este: https://metrics.ng.bluemix.net</li>
        <li>UK-Sur: https://metrics.eu-gb.bluemix.net</li>
        <li>UE central: https://metrics.eu-de.bluemix.net</li>
      </ul></p></dd>
</dl>

### Otras herramientas de supervisión de estado
{: #health_tools}

Puede configurar otras herramientas para disponer de funciones adicionales.
<dl>
  <dt>Prometheus</dt>
    <dd>Prometheus es una herramienta de supervisión, registro y generación de alertas diseñada para Kubernetes. La herramienta recupera información detallada acerca del clúster, los nodos trabajadores y el estado de despliegue basado en la información de registro de Kubernetes. Para obtener información sobre la configuración, consulte [Integración de servicios con {{site.data.keyword.containershort_notm}}](cs_integrations.html#integrations).</dd>
</dl>

<br />


## Configuración de la supervisión de estado de los nodos trabajadores con recuperación automática
{: #autorecovery}

El sistema de recuperación automática de {{site.data.keyword.containerlong_notm}} se puede desplegar en clústeres existentes de Kubernetes versión 1.7 o posterior.
{: shortdesc}

El sistema de recuperación automática utiliza varias comprobaciones para consultar el estado de salud del nodo trabajador de la consulta. Si la recuperación automática detecta un nodo trabajador erróneo basado en las comprobaciones configuradas, desencadena una acción correctiva, como una recarga del sistema operativo, en el nodo trabajador. Solo se aplica una acción correctiva por nodo trabajador cada vez. El nodo trabajador debe completar correctamente la acción correctiva antes de otro nodo trabajador empiece otra acción correctiva. Para obtener más información, consulte esta [publicación del blog sobre recuperación automática ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/blogs/bluemix/2017/12/autorecovery-utilizes-consistent-hashing-high-availability/).</br> </br>
**Nota**: La recuperación automática requiere que al menos haya un nodo en buen estado que funcione correctamente. Configure la recuperación automática solo con las comprobaciones activas en los clústeres con dos o varios nodos trabajadores.

Antes de empezar, seleccione el clúster cuyos estados de nodos trabajadores desea comprobar como [destino de la CLI](cs_cli_install.html#cs_cli_configure).

1. [Instale Helm para el clúster y añada el repositorio de {{site.data.keyword.Bluemix_notm}} a la instancia de Helm](cs_integrations.html#helm).

2. Cree un archivo de mapa de configuración que defina las comprobaciones en formato JSON. Por ejemplo, el siguiente archivo YAML define tres comprobaciones: una comprobación HTTP y dos comprobaciones de servidor API de Kubernetes. Consulte las tablas después del archivo YAML de ejemplo para obtener información sobre los tres tipos de comprobaciones e información sobre los componentes individuales de las comprobaciones.
</br>
   **Sugerencia:** Defina cada comprobación como una clave exclusiva en la sección `data` del mapa de configuración.

   ```
   kind: ConfigMap
   apiVersion: v1
   metadata:
     name: ibm-worker-recovery-checks
     namespace: kube-system
   data:
     checknode.json: |
       {
         "Check":"KUBEAPI",
          "Resource":"NODE",
          "FailureThreshold":3,
          "CorrectiveAction":"RELOAD",
          "CooloffSeconds":1800,
          "IntervalSeconds":180,
          "TimeoutSeconds":10,
          "Enabled":true
        }
      checkpod.json: |
        {
         "Check":"KUBEAPI",
         "Resource":"POD",
         "PodFailureThresholdPercent":50,
         "FailureThreshold":3,
         "CorrectiveAction":"RELOAD",
         "CooloffSeconds":1800,
         "IntervalSeconds":180,
         "TimeoutSeconds":10,
         "Enabled":true
       }
     checkhttp.json: |
       {
         "Check":"HTTP",
         "FailureThreshold":3,
         "CorrectiveAction":"REBOOT",
         "CooloffSeconds":1800,
         "IntervalSeconds":180,
         "TimeoutSeconds":10,
         "Port":80,
         "ExpectedStatus":200,
         "Route":"/myhealth",
         "Enabled":false
       }
   ```
   {:codeblock}

   <table summary="Descripción de los componentes del mapa de configuración">
   <caption>Descripción de los componentes del mapa de configuración</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="Icono Idea"/>Descripción de los componentes del mapa de configuración</th>
   </thead>
   <tbody>
   <tr>
   <td><code>name</code></td>
   <td>El nombre de configuración <code>ibm-worker-recovery-checks</code> es una constante y no se puede cambiar.</td>
   </tr>
   <tr>
   <td><code>namespace</code></td>
   <td>El espacio de nombre <code>kube-system</code> es una constante y no se puede cambiar.</td>
   </tr>
   <tr>
   <td><code>checknode.json</code></td>
   <td>Define una comprobación de nodo de API de Kubernetes que comprueba si cada nodo trabajador está en estado <code>Listo</code>. La comprobación de un nodo trabajador específico considera que hay un fallo si dicho nodo no está en estado <code>Listo</code>. La comprobación del YAML de ejemplo se ejecuta cada 3 minutos. Si falla tres veces consecutivas, se recarga el nodo trabajador. Esta acción equivale a ejecutar <code>bx cs worker-reload</code>.<br></br>La comprobación de nodo está habilitada hasta que se establezca el campo <b>Habilitado</b> en <code>false</code> o se elimine la comprobación.</td>
   </tr>
   <tr>
   <td><code>checkpod.json</code></td>
   <td>
   Define una comprobación de pod de API que comprueba el porcentaje total de pods <code>NotReady</code> en un nodo trabajador en relación con el número total de pods que se asignan a dicho nodo trabajador. La comprobación de un nodo trabajador específico cuenta como fallo si el porcentaje total de pods <code>NotReady</code> es superior al definido en <code>PodFailureThresholdPercent</code>. La comprobación del YAML de ejemplo se ejecuta cada 3 minutos. Si falla tres veces consecutivas, se recarga el nodo trabajador. Esta acción equivale a ejecutar <code>bx cs worker-reload</code>. Por ejemplo, el valor predeterminado de <code>PodFailureThresholdPercent</code> es 50%. Si el porcentaje de pods <code>NotReady</code> es mayor del 50% tres veces consecutivas, se recargará el nodo trabajador. <br></br>De forma predeterminada, se comprueban los pods en todos los espacios de nombres. Para restringir la selección sólo a los pods en un espacio de nombres especificado, añada el campo <code>Namespace</code> a la selección. La comprobación de pod está habilitada hasta que se establezca el campo <b>Habilitado</b> en <code>false</code> o se elimine la comprobación.
   </td>
   </tr>
   <tr>
   <td><code>checkhttp.json</code></td>
   <td>Define una comprobación HTTP que comprueba si un servidor HTTP que se ejecuta en el nodo trabajador tiene un estado saludable. Para utilizar esta comprobación debe desplegar un servidor HTTP en cada nodo trabajador en su clúster utilizando un [DaemonSet ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/). Debe implementar una comprobación de salud que esté disponible en la vía de acceso <code>/myhealth</code> que pueda verificar si su servidor HTTP posee un estado saludable. Puede definir otras vías cambiando el parámetro <strong>Route</strong>. Si el servidor HTTP posee un estado saludable, debe devolver el código de respuesta HTTP que esté definido en <strong>ExpectedStatus</strong>. El servidor HTTP debe configurarse para aparecer en la lista de direcciones IP privadas del nodo trabajador. Ejecute <code>kubectl get nodes</code> para obtener las direcciones IP privadas.<br></br>
   Por ejemplo, considere dos nodos en un clúster que tienen las direcciones IP privadas 10.10.10.1 y 10.10.10.2. En este ejemplo, se comprueban dos rutas para la respuesta 200 HTTP: <code>http://10.10.10.1:80/myhealth</code> y <code>http://10.10.10.2:80/myhealth</code>.
   La comprobación del YAML de ejemplo se ejecuta cada 3 minutos. Si falla tres veces consecutivas, se rearranca el nodo trabajador. Esta acción equivale a ejecutar <code>bx cs worker-reboot</code>.<br></br>La comprobación HTTP está inhabilitada hasta que se establece el campo <b>Habilitado</b> en <code>true</code>.</td>
   </tr>
   </tbody>
   </table>

   <table summary="Componentes individuales de las comprobaciones">
   <caption>Componentes individuales de las comprobaciones</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="Icono de idea"/>Componentes individuales de las comprobaciones </th>
   </thead>
   <tbody>
   <tr>
   <td><code>Check</code></td>
   <td>Introduzca el tipo de comprobación que desee que utilice la recuperación automática. <ul><li><code>HTTP</code>: La recuperación automática llama a los servidores HTTP que se ejecutan en cada nodo para determinar si los nodos se están ejecutando correctamente.</li><li><code>KUBEAPI</code>: La recuperación automática llama al servidor de API de Kubernetes y lee los datos del estado de salud notificado por los nodos trabajadores.</li></ul></td>
   </tr>
   <tr>
   <td><code>Resource</code></td>
   <td>Cuando el tipo de comprobación es <code>KUBEAPI</code>, especifique el tipo de recurso que desee que compruebe la recuperación automática. Los valores aceptados son <code>NODE</code> o <code>POD</code>.</td>
   </tr>
   <tr>
   <td><code>FailureThreshold</code></td>
   <td>Especifique el umbral para el número de comprobaciones consecutivas fallidas. Cuando se alcance el umbral, la recuperación automática desencadenará la acción correctiva especificada. Por ejemplo, si el valor es 3 y la recuperación automática falla una comprobación configurada tres veces consecutivas, la recuperación automática desencadena la acción correctiva asociada con la comprobación.</td>
   </tr>
   <tr>
   <td><code>PodFailureThresholdPercent</code></td>
   <td>Cuando el tipo de recurso sea <code>POD</code>, especifique el umbral del porcentaje de pods en un nodo trabajador que puede encontrarse en estado [NotReady ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/#define-readiness-probes). Este porcentaje se basa en el número total de pods que están programados para un nodo trabajador. Cuando una comprobación determina que el porcentaje de pods erróneos es superior al umbral, cuenta un error.</td>
   </tr>
   <tr>
   <td><code>CorrectiveAction</code></td>
   <td>Especifique que se ejecute la acción cuando se alcance el umbral de errores. Una acción correctiva solo se ejecuta cuando no se repara ningún otro trabajador y cuando el nodo trabajador no está en un período de calma tras una acción anterior. <ul><li><code>REBOOT</code>: Reinicia el nodo trabajador.</li><li><code>RELOAD</code>: Vuelve a cargar las configuraciones necesarias del nodo trabajador desde un SO limpio.</li></ul></td>
   </tr>
   <tr>
   <td><code>CooloffSeconds</code></td>
   <td>Especifique el número de segundos que debe esperar la recuperación automática para un nodo que ya ha emitido una acción correctiva. El período de calma empieza a la hora en que se emite la acción correctiva.</td>
   </tr>
   <tr>
   <td><code>IntervalSeconds</code></td>
   <td>Especifique el número de segundos entre comprobaciones consecutivas. Por ejemplo, si el valor es 180, la recuperación automática ejecuta la comprobación en cada nodo cada 3 minutos.</td>
   </tr>
   <tr>
   <td><code>TimeoutSeconds</code></td>
   <td>Especifique el número máximo de segundos que tarda una llamada de comprobación a la base de datos antes de que la recuperación automática finalice la operación de llamada. El valor <code>TimeoutSeconds</code> debe ser inferior al valor de <code>IntervalSeconds</code>.</td>
   </tr>
   <tr>
   <td><code>Port</code></td>
   <td>Cuando el tipo de comprobación sea <code>HTTP</code>, especifique el puerto con el que se debe vincular el servidor HTTP en los nodos trabajadores. Este puerto debe estar expuesto a la IP de cada nodo trabajador en el clúster. La recuperación automática requiere un número de puerto en todos los nodos para la comprobación de servidores. Utilice [DaemonSets ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)cuando despliegue un servidor personalizado en un clúster.</td>
   </tr>
   <tr>
   <td><code>ExpectedStatus</code></td>
   <td>Cuando el tipo de comprobación sea <code>HTTP</code>, especifique el estado del servidor HTTP que espera que se devuelva desde la comprobación. Por ejemplo, un valor de 200 indica que espera una respuesta <code>OK</code> del servidor.</td>
   </tr>
   <tr>
   <td><code>Route</code></td>
   <td>Cuando el tipo de comprobación es <code>HTTP</code>, especifique la vía de acceso solicitada desde el servidor HTTP. Este valor suele ser la vía de acceso de las métricas del servidor que se ejecuta en todos los nodos trabajadores.</td>
   </tr>
   <tr>
   <td><code>Enabled</code></td>
   <td>Especifique <code>true</code> para habilitar la comprobación o <code>false</code> para inhabilitar la comprobación.</td>
   </tr>
   <tr>
   <td><code>Espacio de nombres</code></td>
   <td> Opcional: Para restringir <code>checkpod.json</code> para comprobar únicamente pods en un espacio de nombres, añada el campo <code>Namespace</code> y especifique el espacio de nombres.</td>
   </tr>
   </tbody>
   </table>

3. Cree el mapa de configuración en el clúster.

    ```
    kubectl apply -f ibm-worker-recovery-checks.yaml
    ```
    {: pre}

3. Verifique haber creado el mapa de configuración con el nombre `ibm-worker-recovery-checks` en el espacio de nombre `kube-system` con las comprobaciones adecuadas.

    ```
    kubectl -n kube-system get cm ibm-worker-recovery-checks -o yaml
    ```
    {: pre}

4. Desplegar Autorecovery en su clúster instalando el diagrama Helm `ibm-worker-recovery`.

    ```
    helm install --name ibm-worker-recovery ibm/ibm-worker-recovery  --namespace kube-system
    ```
    {: pre}

5. Pasados unos minutos, podrá comprobar la sección `Sucesos` en la salida del siguiente mandato para ver la actividad en el despliegue de la recuperación automática.

    ```
    kubectl -n kube-system describe deployment ibm-worker-recovery
    ```
    {: pre}

6. Si está visualizando actividad en el despliegue Autorecovery, puede comprobar el despliegue de Helm ejecutando las pruebas que se incluyen en la definición del diagrama Autorecovery.

    ```
    helm test ibm-worker-recovery
    ```
    {: pre}
