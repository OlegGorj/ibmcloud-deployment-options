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





# {{site.data.keyword.containerlong_notm}} CLI reference
{: #cs_cli_reference}

Consulte estos mandatos para crear y gestionar clústeres de Kubernetes en {{site.data.keyword.containerlong}}.
{:shortdesc}

Para instalar el plugin de CLI, consulte [Instalación de la CLI](cs_cli_install.html#cs_cli_install_steps).

¿Está buscando mandatos `bx cr`? Revise la guía de [consulta de la CLI de {{site.data.keyword.registryshort_notm}}](/docs/cli/plugins/registry/index.html). ¿Está buscando mandatos `kubectl`? Consulte la [documentación de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/reference/kubectl/overview/).
{:tip}

## Mandatos bx cs
{: #cs_commands}

**Sugerencia:** Para ver la versión del plug-in de {{site.data.keyword.containershort_notm}}, consulte el siguiente mandato.

```
bx plugin list
```
{: pre}



<table summary="Tabla de mandatos de API">
<caption>Mandatos de API</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Mandatos de API</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs api](#cs_api)</td>
    <td>[bx cs api-key-info](#cs_api_key_info)</td>
    <td>[bx cs api-key-reset](#cs_api_key_reset)</td>
    <td>[bx cs apiserver-config-get](#cs_apiserver_config_get)</td>
  </tr>
  <tr>
    <td>[bx cs apiserver-config-set](#cs_apiserver_config_set)</td>
    <td>[bx cs apiserver-config-unset](#cs_apiserver_config_unset)</td>
    <td>[bx cs apiserver-refresh](#cs_apiserver_refresh)</td>
    <td></td>
 </tr>
</tbody>
</table>

<br>

<table summary="Tabla de mandatos de uso de plug-in de CLI">
<caption>Mandatos de uso del plug-in de CLI</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Mandatos de uso del plug-in de CLI</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs help](#cs_help)</td>
    <td>[bx cs init](#cs_init)</td>
    <td>[bx cs messages](#cs_messages)</td>
    <td></td>
  </tr>
</tbody>
</table>

<br>

<table summary="Tabla Mandatos de clúster: Gestión">
<caption>Mandatos de clúster: Mandatos de gestión</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Mandatos de clúster: Gestión</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs cluster-config](#cs_cluster_config)</td>
    <td>[bx cs cluster-create](#cs_cluster_create)</td>
    <td>[bx cs cluster-feature-enable](#cs_cluster_feature_enable)</td>
    <td>[bx cs cluster-get](#cs_cluster_get)</td>
  </tr>
  <tr>
    <td>[bx cs cluster-rm](#cs_cluster_rm)</td>
    <td>[bx cs cluster-update](#cs_cluster_update)</td>
    <td>[bx cs clusters](#cs_clusters)</td>
    <td>[bx cs kube-versions](#cs_kube_versions)</td>
  </tr>
</tbody>
</table>

<br>

<table summary="Tabla Mandatos de clúster: Servicios e integraciones">
<caption>Mandatos de clúster: Mandatos de servicios e integraciones</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Mandatos de clúster: Servicios e integraciones</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs cluster-service-bind](#cs_cluster_service_bind)</td>
    <td>[bx cs cluster-service-unbind](#cs_cluster_service_unbind)</td>
    <td>[bx cs cluster-services](#cs_cluster_services)</td>
    <td>[bx cs webhook-create](#cs_webhook_create)</td>
  </tr>
</tbody>
</table>

</br>

<table summary="Tabla Mandatos de clúster: Subredes">
<caption>Mandatos de clúster: Mandatos de subredes</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Mandatos de clúster: Subredes</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs cluster-subnet-add](#cs_cluster_subnet_add)</td>
    <td>[bx cs cluster-subnet-create](#cs_cluster_subnet_create)</td>
    <td>[bx cs cluster-user-subnet-add](#cs_cluster_user_subnet_add)</td>
    <td>[bx cs cluster-user-subnet-rm](#cs_cluster_user_subnet_rm)</td>
  </tr>
  <tr>
    <td>[bx cs subnets](#cs_subnets)</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
</tbody>
</table>

</br>

<table summary="Tabla Mandatos de infraestructura">
<caption>Mandatos de clúster: Mandatos de infraestructura</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Mandatos de infraestructura</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs credentials-set](#cs_credentials_set)</td>
    <td>[bx cs credentials-unset](#cs_credentials_unset)</td>
    <td>[bx cs machine-types](#cs_machine_types)</td>
    <td>[bx cs vlans](#cs_vlans)</td>
  </tr>
</tbody>
</table>

</br>

<table summary="Tabla Mandatos del equilibrador de carga de aplicación (ALB) de Ingress">
<caption>Mandatos del equilibrador de carga de aplicación (ALB) de Ingress</caption>
<col width = 25%>
<col width = 25%>
<col width = 25%>
  <thead>
    <tr>
      <th colspan=4>Mandatos del equilibrador de carga de aplicación (ALB) de Ingress</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>[bx cs alb-cert-deploy](#cs_alb_cert_deploy)</td>
      <td>[bx cs alb-cert-get](#cs_alb_cert_get)</td>
      <td>[bx cs alb-cert-rm](#cs_alb_cert_rm)</td>
      <td>[bx cs alb-certs](#cs_alb_certs)</td>
    </tr>
    <tr>
      <td>[bx cs alb-configure](#cs_alb_configure)</td>
      <td>[bx cs alb-get](#cs_alb_get)</td>
      <td>[bx cs alb-types](#cs_alb_types)</td>
      <td>[bx cs albs](#cs_albs)</td>
    </tr>
  </tbody>
</table>

</br>

<table summary="Tabla Mandatos de registro">
<caption>Mandatos de registro</caption>
<col width = 25%>
<col width = 25%>
<col width = 25%>
  <thead>
    <tr>
      <th colspan=4>Mandatos de registro</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>[bx cs logging-config-create](#cs_logging_create)</td>
      <td>[bx cs logging-config-get](#cs_logging_get)</td>
      <td>[bx cs logging-config-refresh](#cs_logging_refresh)</td>
      <td>[bx cs logging-config-rm](#cs_logging_rm)</td>
    </tr>
    <tr>
      <td>[bx cs logging-config-update](#cs_logging_update)</td>
      <td>[bx cs logging-filter-create](#cs_log_filter_create)</td>
      <td>[bx cs logging-filter-update](#cs_log_filter_update)</td>
      <td>[bx cs logging-filter-get](#cs_log_filter_view)</td>
    </tr>
    <tr>
      <td>[bx cs logging-filter-rm](#cs_log_filter_delete)</td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>

</br>

<table summary="Tabla Mandatos de región">
<caption>Mandatos de región</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Mandatos de región</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs locations](#cs_datacenters)</td>
    <td>[bx cs region](#cs_region)</td>
    <td>[bx cs region-set](#cs_region-set)</td>
    <td>[bx cs regions](#cs_regions)</td>
  </tr>
</tbody>
</table>

</br>

<table summary="Tabla Mandatos de nodo trabajador">
<caption>Mandatos de nodo trabajador</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Mandatos de nodo trabajador</th>
 </thead>
 <tbody>
    <tr>
      <td>[bx cs worker-add](#cs_worker_add)</td>
      <td>[bx cs worker-get](#cs_worker_get)</td>
      <td>[bx cs worker-reboot](#cs_worker_reboot)</td>
      <td>[bx cs worker-reload](#cs_worker_reload)</td></staging>
    </tr>
    <tr>
      <td>[bx cs worker-rm](#cs_worker_rm)</td>
      <td>[bx cs worker-update](#cs_worker_update)</td>
      <td>[bx cs workers](#cs_workers)</td>
      <td></td>
    </tr>
  </tbody>
</table>

## Mandatos de API
{: #api_commands}

### bx cs api ENDPOINT [--insecure][--skip-ssl-validation] [--api-version VALUE][-s]
{: #cs_api}

Establezca como destino el punto final de API para {{site.data.keyword.containershort_notm}}. Si no especifica un punto final, puede ver información sobre el punto final actual de destino.

¿Desea cambiar de región? En su lugar, utilice el [mandato](#cs_region-set) `bx cs region-set`.
{: tip}

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code><em>ENDPOINT</em></code></dt>
   <dd>El punto final de API de {{site.data.keyword.containershort_notm}}. Tenga en cuenta que este punto final es distinto de los puntos finales de {{site.data.keyword.Bluemix_notm}}. Este valor es necesario para establecer el punto final de API. Los valores aceptados son:<ul>
   <li>Punto final global: https://containers.bluemix.net</li>
   <li>Punto final AP Norte: https://ap-north.containers.bluemix.net</li>
   <li>Punto final AP Sur: https://ap-south.containers.bluemix.net</li>
   <li>Punto final UE Central: https://eu-central.containers.bluemix.net</li>
   <li>Punto final UK Sur: https://uk-south.containers.bluemix.net</li>
   <li>Punto final EE.UU. Este: https://us-east.containers.bluemix.net</li>
   <li>Punto final EE.UU. Sur: https://us-south.containers.bluemix.net</li></ul>
   </dd>

   <dt><code>--insecure</code></dt>
   <dd>Permite una conexión HTTP insegura. Este distintivo es opcional.</dd>

   <dt><code>--skip-ssl-validation</code></dt>
   <dd>Permite certificados SSL inseguros. Este distintivo es opcional.</dd>

   <dt><code>--api-version VALUE</code></dt>
   <dd>Especifica la versión de API del servicio que desea utilizar. Este valor es opcional.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

   </dl>

**Ejemplo**: Ver información sobre el punto final de API actual de destino.
```
bx cs api
```
{: pre}

```
API Endpoint:          https://containers.bluemix.net   
API Version:           v1   
Skip SSL Validation:   false   
Region:                us-south
```
{: screen}


### bx cs api-key-info CLUSTER [--json][-s]
{: #cs_api_key_info}

Visualice el nombre y la dirección de correo electrónico del propietario de la clave de API IAM en una región de {{site.data.keyword.containershort_notm}}.

La clave de API de Identity and Access Management (IAM) se establece automáticamente para una región cuando se realiza la primera acción que requiere la política de acceso de administrador de {{site.data.keyword.containershort_notm}}. Por ejemplo, uno de los usuarios administrativos crea el primer clúster en la región `us-south`. De ese modo, la clave de API IAM para este usuario se almacena en la cuenta para esta región. La clave de API se utiliza para pedir recursos en la infraestructura de IBM Cloud (SoftLayer), como nodos trabajadores nuevos o VLAN.

Cuando un usuario distinto realiza una acción en esta región que requiere interacción con el portafolio de infraestructura de IBM Cloud (SoftLayer), como crear un nuevo clúster o recargar un nodo trabajador, la clave de API almacenada se utiliza para determinar si existen suficientes permisos para realizar esa acción. Para asegurarse de que las acciones relacionadas con la infraestructura en el clúster se realicen correctamente, puede asignar a los usuarios administradores de {{site.data.keyword.containershort_notm}} la política de acceso a infraestructura **Superusuario**. Para obtener más información, consulte [Gestión de acceso de usuario](cs_users.html#infra_access).

Si necesita actualizar la clave de API que hay almacenada para una región, puede hacerlo mediante la ejecución del mandato [bx cs api-key-reset](#cs_api_key_reset). Este mandato requiere la política de acceso de administrador de {{site.data.keyword.containershort_notm}} y almacena la clave de API del usuario que ejecuta este mandato en la cuenta.

**Nota:** La clave de API que se devuelve en este mandato no puede utilizarse si las credenciales de infraestructura de IBM Cloud (SoftLayer) se han establecido manualmente mediante el mandato [bx cs credentials-set](#cs_credentials_set).

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>--json</code></dt>
   <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

   </dl>

**Ejemplo**:

  ```
  bx cs api-key-info my_cluster
  ```
  {: pre}


### bx cs api-key-reset [-s]
{: #cs_api_key_reset}

Sustituya la clave de API IAM actual en una región de {{site.data.keyword.containershort_notm}}.

Este mandato requiere la política de acceso de administrador de {{site.data.keyword.containershort_notm}} y almacena la clave de API del usuario que ejecuta este mandato en la cuenta. La clave de API IAM es necesaria para pedir infraestructura del portfolio de infraestructura de IBM Cloud (SoftLayer). Una vez almacenada, la clave de API se utiliza para cada acción en una región que requiere permisos de infraestructura, con independencia del usuario que ejecute este mandato. Para obtener más información sobre cómo funcionan las claves de API IAM, consulte el [mandato `bx cs api-key-info`](#cs_api_key_info).

**Importante:** Antes de utilizar este mandato, asegúrese de que el usuario que lo ejecuta tiene los [permisos de infraestructura de IBM Cloud (SoftLayer) e {{site.data.keyword.containershort_notm}}](cs_users.html#users) necesarios.

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
   </dl>


**Ejemplo**:

  ```
  bx cs api-key-reset
  ```
  {: pre}


### bx cs apiserver-config-get
{: #cs_apiserver_config_get}

Obtener información sobre una opción para la configuración de servidor de API de Kubernetes de un clúster. Este mandato debe combinarse con uno de los siguientes submandatos para la opción de configuración de la que desea información.

#### bx cs apiserver-config-get audit-webhook CLUSTER
{: #cs_apiserver_api_webhook_get}

Ver el URL para el servicio de registro remoto al que está enviando registros de auditoría de servidor de API. El URL se ha especificado al crear el programa de fondo del webhook para la configuración del servidor de API.

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>
   </dl>

**Ejemplo**:

  ```
  bx cs apiserver-config-get audit-webhook my_cluster
  ```
  {: pre}

### bx cs apiserver-config-set
{: #cs_apiserver_config_set}

Establecer una opción para la configuración de servidor de API de Kubernetes de un clúster. Este mandato debe combinarse con uno de los siguientes submandatos para la opción de configuración que desea establecer.

#### bx cs apiserver-config-set audit-webhook CLUSTER [--remoteServer SERVER_URL_OR_IP][--caCert CA_CERT_PATH] [--clientCert CLIENT_CERT_PATH][--clientKey CLIENT_KEY_PATH]
{: #cs_apiserver_api_webhook_set}

Establecer el programa de fondo del webhook para la configuración del servidor de API. El programa de fondo del webhook reenvía registros de auditoría del servidor de API a un servidor remoto. Se crea una configuración de webhook basándose en la información que proporciona en los distintivos de este mandato. Si no proporciona ninguna información en los distintivos, se utiliza una configuración de webhook predeterminada.

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>--remoteServer <em>SERVER_URL</em></code></dt>
   <dd>La dirección IP o el URL del servicio de registro remoto al que desea enviar registros de auditoría. Si proporciona un URL de servidor no seguro, se ignoran los certificados. Este valor es opcional.</dd>

   <dt><code>--caCert <em>CA_CERT_PATH</em></code></dt>
   <dd>La vía de acceso de archivo del certificado de CA que se utiliza para verificar el servicio de registro remoto. Este valor es opcional.</dd>

   <dt><code>--clientCert <em>CLIENT_CERT_PATH</em></code></dt>
   <dd>La vía de acceso de archivo del certificado de cliente que se utiliza para autenticarse en el servicio de registro remoto. Este valor es opcional.</dd>

   <dt><code>--clientKey <em> CLIENT_KEY_PATH</em></code></dt>
   <dd>La vía de acceso de archivo de la clave de cliente correspondiente que se utiliza para conectarse al servicio de registro remoto. Este valor es opcional.</dd>
   </dl>

**Ejemplo**:

  ```
  bx cs apiserver-config-set audit-webhook my_cluster --remoteServer https://audit.example.com/audit --caCert /mnt/etc/kubernetes/apiserver-audit/ca.pem --clientCert /mnt/etc/kubernetes/apiserver-audit/cert.pem --clientKey /mnt/etc/kubernetes/apiserver-audit/key.pem
  ```
  {: pre}


### bx cs apiserver-config-unset
{: #cs_apiserver_config_unset}

Inhabilitar una opción para la configuración de servidor de API de Kubernetes de un clúster. Este mandato debe combinarse con uno de los siguientes submandatos para la opción de configuración que desea eliminar.

#### bx cs apiserver-config-unset audit-webhook CLUSTER
{: #cs_apiserver_api_webhook_unset}

Inhabilitar la configuración del programa de fondo del webhook para el servidor de API del clúster. Al inhabilitar el programa de fondo del webhook, se detiene el reenvío registros de auditoría del servidor de API a un servidor remoto.

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>
   </dl>

**Ejemplo**:

  ```
  bx cs apiserver-config-unset audit-webhook my_cluster
  ```
  {: pre}

### bx cs apiserver-refresh CLUSTER [-s]
{: #cs_apiserver_refresh}

Reinicie el maestro de Kubernetes en el clúster para aplicar los cambios a la configuración del servidor de API.

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

   </dl>

**Ejemplo**:

  ```
  bx cs apiserver-refresh my_cluster
  ```
  {: pre}


<br />


## Mandatos de uso del plug-in de CLI
{: #cli_plug-in_commands}

### bx cs help
{: #cs_help}

Ver una lista de mandatos y parámetros admitidos.

<strong>Opciones del mandato</strong>:

   Ninguno

**Ejemplo**:

  ```
  bx cs help
  ```
  {: pre}


### bx cs init [--host HOST][--insecure] [-p][-u] [-s]
{: #cs_init}

Inicialice el plug-in de {{site.data.keyword.containershort_notm}} o especifique la región donde desea crear o acceder a clústeres de Kubernetes.

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--host <em>HOST</em></code></dt>
   <dd>El punto final de API de {{site.data.keyword.containershort_notm}} que se va a utilizar.  Este valor es opcional. [Ver valores disponibles de punto final de API.](cs_regions.html#container_regions)</dd>

   <dt><code>--insecure</code></dt>
   <dd>Permite una conexión HTTP insegura.</dd>

   <dt><code>-p</code></dt>
   <dd>Su contraseña de IBM Cloud.</dd>

   <dt><code>-u</code></dt>
   <dd>Su nombre de usuario de IBM Cloud.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

   </dl>

**Ejemplo**:


```
bx cs init --host https://uk-south.containers.bluemix.net
```
{: pre}


### bx cs messages
{: #cs_messages}

Ver los mensajes actuales para el usuario IBMid.

**Ejemplo**:

```
bx cs messages
```
{: pre}


<br />


## Mandatos de clúster: Gestión
{: #cluster_mgmt_commands}


### bx cs cluster-config CLUSTER [--admin][--export] [-s][--yaml]
{: #cs_cluster_config}

Después de iniciar una sesión, descargue certificados y datos de configuración de Kubernetes para conectar el clúster y ejecutar mandatos `kubectl`. Los archivos se descargan en `user_home_directory/.bluemix/plugins/container-service/clusters/<cluster_name>`.

**Opciones del mandato**:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>--admin</code></dt>
   <dd>Descargar los certificados TLS y archivos de permisos para el rol de superusuario. Puede utilizar los certificados para automatizar las tareas de un clúster sin tener que volverse a autenticar. Los archivos se descargan en `<user_home_directory>/.bluemix/plugins/container-service/clusters/<cluster_name>-admin`. Este valor es opcional.</dd>

   <dt><code>--export</code></dt>
   <dd>Descargar los datos de configuración y certificados de Kubernetes sin ningún mensaje aparte del mandato de exportación. Como no se muestran mensajes, puede utilizar este distintivo cuando cree scripts automatizados. Este valor es opcional.</dd>

  <dt><code>-s</code></dt>
  <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

  <dt><code>--yaml</code></dt>
  <dd>Imprime la salida del mandato en formato YAML. Este valor es opcional.</dd>

   </dl>

**Ejemplo**:

```
bx cs cluster-config my_cluster
```
{: pre}


### bx cs cluster-create [--file FILE_LOCATION][--hardware HARDWARE] --location LOCATION --machine-type MACHINE_TYPE --name NAME [--kube-version MAJOR.MINOR.PATCH][--no-subnet] [--private-vlan PRIVATE_VLAN][--public-vlan PUBLIC_VLAN] [--workers WORKER][--disable-disk-encrypt] [--trusted][-s]
{: #cs_cluster_create}

Cree un clúster en la organización. Para los clústeres gratuitos, especifique el nombre de clúster; todo lo demás se establece en un valor predeterminado. Un clúster gratuito se suprime automáticamente después de 21 días. Puede disponer de un clúster gratuito al mismo tiempo. Para aprovechar todas las funciones de Kubernetes, cree un clúster estándar.

<strong>Opciones del mandato</strong>

<dl>
<dt><code>--file <em>FILE_LOCATION</em></code></dt>

<dd>La vía de acceso al archivo YAML para crear el clúster estándar. En lugar de definir las características de su clúster mediante las opciones que se proporcionan en este mandato, puede utilizar un archivo YAML.  Este valor es opcional para clústeres estándares y no está disponible para clústeres gratuitos.

<p><strong>Nota:</strong> Si especifica la misma opción en el mandato y como parámetro en el archivo YAML, el valor en el mandato prevalece sobre el valor del archivo YAML. Por ejemplo, supongamos que define una ubicación en el archivo YAML y utiliza la opción <code>--location</code> en el mandato; el valor que especifique en la opción del mandato prevalece sobre el valor del archivo YAML.

<pre class="codeblock">
<code>name: <em>&lt;cluster_name&gt;</em>
location: <em>&lt;location&gt;</em>
no-subnet: <em>&lt;no-subnet&gt;</em>
machine-type: <em>&lt;machine_type&gt;</em>
private-vlan: <em>&lt;private_VLAN&gt;</em>
public-vlan: <em>&lt;public_VLAN&gt;</em>
hardware: <em>&lt;shared_or_dedicated&gt;</em>
workerNum: <em>&lt;number_workers&gt;</em>
kube-version: <em>&lt;kube-version&gt;</em>
diskEncryption: <em>false</em>
trusted: <em>true</em>
</code></pre>


<table>
    <caption>Visión general de los componentes del archivo YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general de los componentes del archivo YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>name</em></code></td>
    <td>Sustituya <code><em>&lt;cluster_name&gt;</em></code> por el nombre del clúster. El nombre debe empezar por una letra, puede contener letras, números, guiones (-) y debe tener 35 caracteres como máximo. El nombre del clúster y la región en la que el clúster se despliega forman el nombre de dominio completo para el subdominio de Ingress. Para garantizar que el subdominio de Ingress es exclusivo dentro de una región, el nombre del clúster podría ser truncado y añadírsele un valor aleatorio dentro del nombre de dominio de Ingress.
</td>
    </tr>
    <tr>
    <td><code><em>location</em></code></td>
    <td>Sustituya <code><em>&lt;location&gt;</em></code> por la ubicación en la que desea crear el clúster. Las ubicaciones disponibles dependen de la región en la que ha iniciado la sesión. Para ver una lista de las ubicaciones disponibles, ejecute <code>cs bx ubicaciones</code>. </td>
     </tr>
     <tr>
     <td><code><em>no-subnet</em></code></td>
     <td>De forma predeterminada, se crea una subred portátil pública y una privada en la VLAN asociada con el clúster. Sustituya <code><em>&lt;no-subnet&gt;</em></code> por <code><em>true</em></code> para evitar crear subredes con el clúster. Puede [crear](#cs_cluster_subnet_create) o [añadir](#cs_cluster_subnet_add) subredes a un clúster más adelante.</td>
      </tr>
     <tr>
     <td><code><em>machine-type</em></code></td>
     <td>Sustituya <code><em>&lt;machine_type&gt;</em></code> por el tipo de máquina en el que desea desplegar los nodos trabajadores. Puede desplegar los nodos trabajadores como máquinas virtuales en hardware dedicado o compartido, o como máquinas físicas en servidores nativos. Los tipos de máquinas físicas y virtuales varían según la ubicación en la que se despliega el clúster. Para obtener más información, consulte la documentación del [mandato](cs_cli_reference.html#cs_machine_types) `bx cs machine-type`.</td>
     </tr>
     <tr>
     <td><code><em>private-vlan</em></code></td>
     <td>Sustituya <code><em>&lt;private_VLAN&gt;</em></code> por el ID de la VLAN privada que desea utilizar para sus nodos trabajadores. Para ver una lista de las VLAN disponibles, ejecute <code>bx cs vlans <em>&lt;location&gt;</em></code> y busque direccionadores de VLAN que empiecen por <code>bcr</code> ("back-end router", direccionador de fondo).</td>
     </tr>
     <tr>
     <td><code><em>public-vlan</em></code></td>
     <td>Sustituya <code><em>&lt;public_VLAN&gt;</em></code> por el ID de la VLAN pública que desea utilizar para sus nodos trabajadores. Para ver una lista de las VLAN disponibles, ejecute <code>bx cs vlans <em>&lt;location&gt;</em></code> y busque direccionadores de VLAN que empiecen por <code>fcr</code> ("front-end router", direccionador frontal).</td>
     </tr>
     <tr>
     <td><code><em>hardware</em></code></td>
     <td>Para tipos de máquina virtual: El nivel de aislamiento del hardware del nodo trabajador. Utilice el valor dedicated si desea tener recursos físicos disponibles dedicados solo a usted, o el valor shared para permitir que los recursos físicos se compartan con otros clientes de IBM. El valor predeterminado es <code>shared</code>.</td>
     </tr>
     <tr>
     <td><code><em>workerNum</em></code></td>
     <td>Sustituya <code><em>&lt;number_workers&gt;</em></code> por el número de nodos trabajadores que desea desplegar.</td>
     </tr>
     <tr>
      <td><code><em>kube-version</em></code></td>
      <td>La versión Kubernetes del nodo maestro del clúster. Este valor es opcional. Cuando no se especifica la versión, el clúster se crea con el valor predeterminado de las versiones de Kubernetes soportadas. Para ver todas las versiones disponibles, ejecute <code>bx cs kube-versions</code>.
</td></tr>
      <tr>
      <td><code>diskEncryption: <em>false</em></code></td>
      <td>Los nodos trabajadores tienen cifrado de disco de forma predeterminada; [más información](cs_secure.html#worker). Para inhabilitar el cifrado, incluya esta opción y establezca el valor en <code>false</code>.</td></tr>
      <tr>
      <td><code>trusted: <em>true</em></code></td>
      <td>**Solo nativos**: Habilite [Trusted Compute](cs_secure.html#trusted_compute) para verificar que los nodos trabajadores nativos no se manipulan de forma indebida. Si no habilita la confianza durante la creación del clúster pero desea hacerlo posteriormente, puede utilizar el [mandato](cs_cli_reference.html#cs_cluster_feature_enable) `bx cs feature-enable`. Una vez que habilita la confianza, no puede inhabilitarla posteriormente.</td></tr>
     </tbody></table>
    </p></dd>

<dt><code>--hardware <em>HARDWARE</em></code></dt>
<dd>El nivel de aislamiento del hardware del nodo trabajador. Utilice el valor dedicated para tener recursos físicos disponibles dedicados solo a usted, o shared para permitir que los recursos físicos se compartan con otros clientes de IBM. El valor predeterminado es shared.  Este valor es opcional para clústeres estándares y no está disponible para clústeres gratuitos.</dd>

<dt><code>--location <em>LOCATION</em></code></dt>
<dd>La ubicación en la que desea crear el clúster. Las ubicaciones disponibles dependen de la región de {{site.data.keyword.Bluemix_notm}} en la que haya iniciado la sesión. Para obtener el mejor rendimiento, seleccione la región que esté físicamente más cercana a su ubicación.  Este valor es obligatorio para clústeres estándares y opcional para clústeres gratuitos.

<p>Consulte [ubicaciones disponibles](cs_regions.html#locations).
</p>

<p><strong>Nota:</strong> Si selecciona una ubicación que se encuentra fuera de su país, tenga en cuenta que es posible que se requiera autorización legal para poder almacenar datos físicamente en un país extranjero.</p>
</dd>

<dt><code>--machine-type <em>MACHINE_TYPE</em></code></dt>
<dd>Elija un tipo de máquina. Puede desplegar los nodos trabajadores como máquinas virtuales en hardware dedicado o compartido, o como máquinas físicas en servidores nativos. Los tipos de máquinas físicas y virtuales varían según la ubicación en la que se despliega el clúster. Para obtener más información, consulte la documentación del [mandato](cs_cli_reference.html#cs_machine_types) `bx cs machine-types`. Este valor es obligatorio para clústeres estándares y no está disponible para clústeres gratuitos.</dd>

<dt><code>--name <em>NAME</em></code></dt>
<dd>El nombre del clúster.  Este valor es obligatorio. El nombre debe empezar por una letra, puede contener letras, números, guiones (-) y debe tener 35 caracteres como máximo. El nombre del clúster y la región en la que el clúster se despliega forman el nombre de dominio completo para el subdominio de Ingress. Para garantizar que el subdominio de Ingress es exclusivo dentro de una región, el nombre del clúster podría ser truncado y añadírsele un valor aleatorio dentro del nombre de dominio de Ingress.
</dd>

<dt><code>--kube-version <em>MAJOR.MINOR.PATCH</em></code></dt>
<dd>La versión Kubernetes del nodo maestro del clúster. Este valor es opcional. Cuando no se especifica la versión, el clúster se crea con el valor predeterminado de las versiones de Kubernetes soportadas. Para ver todas las versiones disponibles, ejecute <code>bx cs kube-versions</code>.
</dd>

<dt><code>--no-subnet</code></dt>
<dd>De forma predeterminada, se crea una subred portátil pública y una privada en la VLAN asociada con el clúster. Incluya el distintivo <code>--no-subnet</code> para evitar crear subredes con el clúster. Puede [crear](#cs_cluster_subnet_create) o [añadir](#cs_cluster_subnet_add) subredes a un clúster más adelante.</dd>

<dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
<dd>

<ul>
<li>Este parámetro no está disponible para clústeres gratuitos.</li>
<li>Si este es el primer clúster estándar que crea en esta ubicación, no incluya este distintivo. Al crear clústeres se crea automáticamente una VLAN privada.</li>
<li>Si previamente ha creado un clúster estándar en esta ubicación o una VLAN privada en la infraestructura de IBM Cloud (SoftLayer), debe especificar la VLAN privada.

<p><strong>Nota:</strong> Los direccionadores de VLAN privadas siempre empiezan por <code>bcr</code> (back-end router, direccionador de fondo) y los direccionadores de VLAN públicas siempre empiezan por <code>fcr</code> (direccionador frontal). Al crear un clúster especificando las VLAN privadas y públicas, deben coincidir el número y la combinación de letras después de dichos prefijos.</p></li>
</ul>

<p>Para saber si ya tiene una VLAN privada para una ubicación específica o para encontrar el nombre de una VLAN privada existente, ejecute <code>bx cs vlans <em>&lt;location&gt;</em></code>.</p></dd>

<dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
<dd>
<ul>
<li>Este parámetro no está disponible para clústeres gratuitos.</li>
<li>Si este es el primer clúster estándar que crea en esta ubicación, no utilice este distintivo. Al crear el clúster se crea automáticamente una VLAN pública.</li>
<li>Si previamente ha creado un clúster estándar en esta ubicación o una VLAN pública en la infraestructura de IBM Cloud (SoftLayer), especifique la VLAN pública. Si desea conectar los nodos trabajadores solo a una VLAN privada, no especifique esta opción.

<p><strong>Nota:</strong> Los direccionadores de VLAN privadas siempre empiezan por <code>bcr</code> (back-end router, direccionador de fondo) y los direccionadores de VLAN públicas siempre empiezan por <code>fcr</code> (direccionador frontal). Al crear un clúster especificando las VLAN privadas y públicas, deben coincidir el número y la combinación de letras después de dichos prefijos.</p></li>
</ul>

<p>Para saber si ya tiene una VLAN pública para una ubicación específica o para encontrar el nombre de una VLAN pública existente, ejecute <code>bx cs vlans <em>&lt;location&gt;</em></code>.</p></dd>

<dt><code>--workers WORKER</code></dt>
<dd>El número de nodos trabajadores que desea desplegar en el clúster. Si no especifica esta opción, se crea un clúster con 1 nodo trabajador. Este valor es opcional para clústeres estándares y no está disponible para clústeres gratuitos.

<p><strong>Nota:</strong> A cada nodo trabajador se la asigna un ID exclusivo y un nombre de dominio que no se debe cambiar de forma manual después de haber creado el clúster. Si se cambia el nombre de dominio o el ID se impide que el maestro de Kubernetes gestione el clúster.</p></dd>

<dt><code>--disable-disk-encrypt</code></dt>
<dd>Los nodos trabajadores tienen cifrado de disco de forma predeterminada; [más información](cs_secure.html#worker). Para inhabilitar el cifrado, incluya esta opción.</dd>

<dt><code>--trusted</code></dt>
<dd><p>**Solo nativos**: Habilite [Trusted Compute](cs_secure.html#trusted_compute) para verificar que los nodos trabajadores nativos no se manipulan de forma indebida. Si no habilita la confianza durante la creación del clúster pero desea hacerlo posteriormente, puede utilizar el [mandato](cs_cli_reference.html#cs_cluster_feature_enable) `bx cs feature-enable`. Una vez que habilita la confianza, no puede inhabilitarla posteriormente.</p>
<p>Para comprobar si el tipo de máquina nativa es compatible con la confianza, compruebe el campo `Trustable` en la salida del [mandato](#cs_machine_types) `bx cs machine-types <location>`. Para verificar que un clúster tiene habilitada la confianza, consulte el campo **Trust ready** en la salida del [mandato](#cs_cluster_get) `bx cs cluster-get`. Para verificar que un nodo trabajador nativo tiene habilitada la confianza, consulte el campo **Trust** en la salida del [mandato](#cs_worker_get) `bx cs worker-get`.</p></dd>

<dt><code>-s</code></dt>
<dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplos**:

  

  **Crear un clúster gratuito**: Especifique únicamente el nombre del clúster, el resto se establece en los valores predeterminados. Un clúster gratuito se suprime automáticamente después de 21 días. Puede disponer de un clúster gratuito al mismo tiempo. Para aprovechar todas las funciones de Kubernetes, cree un clúster estándar.

  ```
  bx cs cluster-create --name my_cluster
  ```
  {: pre}

  **Crear su primer clúster estándar**: El primer clúster estándar que se crea en una ubicación también se crea con una VLAN privada. Por lo tanto, no incluya el distintivo `--public-vlan`.
  {: #example_cluster_create}

  ```
  bx cs cluster-create --location dal10 --private-vlan my_private_VLAN_ID --machine-type u2c.2x4 --name my_cluster --hardware shared --workers 2
  ```
  {: pre}

  **Crear clústeres estándar posteriores**: Si ya ha creado un clúster estándar en esta ubicación o con anterioridad ha creado una VLAN pública en la infraestructura de IBM Cloud (SoftLayer), especifique dicha VLAN pública con el distintivo `--public-vlan`. Para saber si ya tiene una VLAN pública para una ubicación específica o para encontrar el nombre de una VLAN pública existente, ejecute `bx cs vlans <location>`.

  ```
  bx cs cluster-create --location dal10 --public-vlan my_public_VLAN_ID --private-vlan my_private_VLAN_ID --machine-type u2c.2x4 --name my_cluster --hardware shared --workers 2
  ```
  {: pre}

  **Crear un clúster en un entorno de {{site.data.keyword.Bluemix_dedicated_notm}}**:

  ```
  bx cs cluster-create --machine-type machine-type --workers number --name cluster_name
  ```
  {: pre}

### bx cs cluster-feature-enable [-f] CLUSTER [--trusted][-s]
{: #cs_cluster_feature_enable}

Habilite una característica en un clúster existente.

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>-f</code></dt>
   <dd>Utilice esta opción para forzar la opción <code>--trusted</code> sin solicitudes de usuario. Este valor es opcional.</dd>

   <dt><code><em>--trusted</em></code></dt>
   <dd><p>Incluya el distintivo para habilitar [Trusted Compute](cs_secure.html#trusted_compute) en todos los nodos trabajadores nativos soportados que están en el clúster. Después de habilitar la confianza, no puede inhabilitarla posteriormente para el clúster.</p>
   <p>Para comprobar si el tipo de máquina nativa es compatible con la confianza, compruebe el campo **Trustable** en la salida del [mandato](#cs_machine_types) `bx cs machine-types <location>`. Para verificar que un clúster tiene habilitada la confianza, consulte el campo **Trust ready** en la salida del [mandato](#cs_cluster_get) `bx cs cluster-get`. Para verificar que un nodo trabajador nativo tiene habilitada la confianza, consulte el campo **Trust** en la salida del [mandato](#cs_worker_get) `bx cs worker-get`.</p></dd>

  <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Mandato de ejemplo**:

  ```
  bx cs cluster-feature-enable my_cluster --trusted=true
  ```
  {: pre}

### bx cs cluster-get CLUSTER [--json][--showResources] [-s]
{: #cs_cluster_get}

Ver información sobre un clúster de la organización.

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>--json</code></dt>
   <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

   <dt><code><em>--showResources</em></code></dt>
   <dd>Muestre más recursos del clúster, como complementos, VLAN, subredes y almacenamiento.</dd>


  <dt><code>-s</code></dt>
  <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
  </dl>



**Mandato de ejemplo**:

  ```
  bx cs cluster-get my_cluster --showResources
  ```
  {: pre}

**Salida de ejemplo**:

  ```
  Name:        my_cluster
  ID:          abc1234567
  State:       normal
  Trust ready: false
  Created:     2018-01-01T17:19:28+0000
  Location:    dal10
  Master URL:  https://169.xx.xxx.xxx:xxxxx
  Ingress subdomain: my_cluster.us-south.containers.appdomain.cloud
  Ingress secret:    my_cluster
  Workers:     3
  Version:     1.7.16_1511* (1.8.11_1509 latest)
  Owner Email: name@example.com
  Monitoring dashboard: https://metrics.ng.bluemix.net/app/#/grafana4/dashboard/db/link

  Addons
  Name                   Enabled
  customer-storage-pod   true
  basic-ingress-v2       true
  storage-watcher-pod    true

  Subnet VLANs
  VLAN ID   Subnet CIDR         Public   User-managed
  2234947   10.xxx.xx.xxx/29    false    false
  2234945   169.xx.xxx.xxx/29  true    false

  ```
  {: screen}

### bx cs cluster-rm [-f] CLUSTER [-s]
{: #cs_cluster_rm}

Eliminar un clúster de la organización.

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>-f</code></dt>
   <dd>Utilice esta opción para forzar la eliminación de un clúster sin solicitudes de usuario. Este valor es opcional.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

   </dl>

**Ejemplo**:

  ```
  bx cs cluster-rm my_cluster
  ```
  {: pre}


### bx cs cluster-update [-f] CLUSTER [--kube-version MAJOR.MINOR.PATCH][--force-update] [-s]
{: #cs_cluster_update}

Actualice el maestro de Kubernetes a la versión predeterminada de la API. Durante la actualización, no puede acceder ni cambiar el clúster. Los nodos trabajadores, las apps y los recursos que los usuarios del clúster despliegan no se modifican y continúan ejecutándose.

Es posible que tenga que modificar los archivos YAML para futuros despliegues. Revise esta [nota del release](cs_versions.html) para ver detalles.

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>--kube-version <em>MAJOR.MINOR.PATCH</em></code></dt>
   <dd>La versión de Kubernetes del clúster. Si no especifica una versión, el maestro de Kubernetes se actualiza a la versión predeterminada de la API. Para ver todas las versiones disponibles, ejecute [bx cs kube-versions](#cs_kube_versions). Este valor es opcional.</dd>

   <dt><code>-f</code></dt>
   <dd>Utilice esta opción para forzar la actualización del maestro sin solicitudes de usuario. Este valor es opcional.</dd>

   <dt><code>--force-update</code></dt>
   <dd>Intente la actualización incluso si el cambio es superior a dos versiones anteriores. Este valor es opcional.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
   </dl>

**Ejemplo**:

  ```
  bx cs cluster-update my_cluster
  ```
  {: pre}


### bx cs clusters [--json][-s]
{: #cs_clusters}

Ver una lista de los clústeres de la organización.

<strong>Opciones del mandato</strong>:

  <dl>
  <dt><code>--json</code></dt>
  <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

  <dt><code>-s</code></dt>
  <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
  </dl>

**Ejemplo**:

  ```
  bx cs clusters
  ```
  {: pre}


### bx cs kube-versions [--json][-s]
{: #cs_kube_versions}

Visualice una lista de las versiones de Kubernetes soportadas en {{site.data.keyword.containershort_notm}}. Actualice su [clúster maestro](#cs_cluster_update) y [nodos trabajadores](cs_cli_reference.html#cs_worker_update) a la versión predeterminada para obtener las prestaciones más estables y recientes.

**Opciones del mandato**:

  <dl>
  <dt><code>--json</code></dt>
  <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

  <dt><code>-s</code></dt>
  <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
  </dl>

**Ejemplo**:

  ```
  bx cs kube-versions
  ```
  {: pre}



<br />



## Mandatos de clúster: Servicios e integraciones
{: #cluster_services_commands}


### bx cs cluster-service-bind CLUSTER KUBERNETES_NAMESPACE SERVICE_INSTANCE_NAME [-s]
{: #cs_cluster_service_bind}

Añadir un servicio de {{site.data.keyword.Bluemix_notm}} a un clúster. Para ver los servicios de {{site.data.keyword.Bluemix_notm}} disponibles en el catálogo de {{site.data.keyword.Bluemix_notm}}, ejecute `bx service offerings`. **Nota**: Sólo puede añadir servicios de {{site.data.keyword.Bluemix_notm}} que den soporte a claves de servicio.

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code><em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>El nombre del espacio de Kubernetes. Este valor es obligatorio.</dd>

   <dt><code><em>SERVICE_INSTANCE_NAME</em></code></dt>
   <dd>El nombre de la instancia de servicio de {{site.data.keyword.Bluemix_notm}} que desea vincular. Para encontrar el nombre de la instancia de servicio, ejecute <code>bx service list</code>. Si más de una instancia tiene el mismo nombre en la cuenta, utilice el ID de instancia de servicio en lugar del nombre. Para encontrar el ID, ejecute <code>bx service show <service instance name> --guid</code>. Uno de estos valores es obligatorio.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

   </dl>

**Ejemplo**:

  ```
  bx cs cluster-service-bind my_cluster my_namespace my_service_instance
  ```
  {: pre}


### bx cs cluster-service-unbind CLUSTER KUBERNETES_NAMESPACE SERVICE_INSTANCE_GUID [-s]
{: #cs_cluster_service_unbind}

Eliminar un servicio de {{site.data.keyword.Bluemix_notm}} de un clúster.

**Nota:** Cuando se elimina un servicio de {{site.data.keyword.Bluemix_notm}}, las credenciales del servicio se eliminan del clúster. Si un pod sigue utilizando el servicio, falla porque no encuentra las credenciales del servicio.

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code><em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>El nombre del espacio de Kubernetes. Este valor es obligatorio.</dd>

   <dt><code><em>SERVICE_INSTANCE_GUID</em></code></dt>
   <dd>El ID de la instancia de servicio de {{site.data.keyword.Bluemix_notm}} que desea eliminar. Para encontrar el ID de la instancia de servicio, ejecute `bx cs cluster-services <cluster_name_or_ID>`. Este valor es obligatorio.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

   </dl>

**Ejemplo**:

  ```
  bx cs cluster-service-unbind my_cluster my_namespace 8567221
  ```
  {: pre}


### bx cs cluster-services CLUSTER [--namespace KUBERNETES_NAMESPACE][--all-namespaces] [--json][-s]
{: #cs_cluster_services}

Crear una lista de los servicios que están vinculados a uno o a todos los espacios de nombres de Kubernetes de un clúster. Si no se especifica ninguna opción, se muestran los servicios correspondientes al espacio de nombres predeterminado.

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code>, <code>-n
<em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>Incluya los servicios que están vinculados a un determinado espacio de nombres de un clúster. Este valor es opcional.</dd>

   <dt><code>--all-namespaces</code></dt>
    <dd>Incluya los servicios que están vinculados a todos los espacios de nombres de un clúster. Este valor es opcional.</dd>

    <dt><code>--json</code></dt>
    <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

    <dt><code>-s</code></dt>
    <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

    </dl>

**Ejemplo**:

  ```
  bx cs cluster-services my_cluster --namespace my_namespace
  ```
  {: pre}



### bx cs webhook-create --cluster CLUSTER --level LEVEL --type slack --url URL  [-s]
{: #cs_webhook_create}

Registrar un webhook.

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>--level <em>LEVEL</em></code></dt>
   <dd>El nivel de notificación, como por ejemplo <code>Normal</code> o <code>Warning</code>. <code>Warning</code> es el valor predeterminado. Este valor es opcional.</dd>

   <dt><code>--type <em>slack</em></code></dt>
   <dd>El tipo de webhook. Actualmente se admite slack. Este valor es obligatorio.</dd>

   <dt><code>--url <em>URL</em></code></dt>
   <dd>El URL del webhook. Este valor es obligatorio.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
   </dl>

**Ejemplo**:

  ```
  bx cs webhook-create --cluster my_cluster --level Normal --type slack --url http://github.com/mywebhook
  ```
  {: pre}


<br />


## Mandatos de clúster: Subredes
{: #cluster_subnets_commands}

### bx cs cluster-subnet-add CLUSTER SUBNET [-s]
{: #cs_cluster_subnet_add}

Poner la subred de la cuenta de una infraestructura de IBM Cloud (SoftLayer)
a disponibilidad de un determinado clúster.

**Nota:**
* Cuando pone una subred a disponibilidad de un clúster, las direcciones IP de esta subred se utilizan para la gestión de redes del clúster. Para evitar conflictos de direcciones IP, asegúrese de utilizar una subred con un solo clúster. No utilice una subred para varios clústeres o para otros fines externos a {{site.data.keyword.containershort_notm}} al mismo tiempo.
* Para direccionar entre subredes de la misma VLAN, se debe [activar la expansión de VLAN](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning).

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code><em>SUBNET</em></code></dt>
   <dd>El ID de la subred. Este valor es obligatorio.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

   </dl>

**Ejemplo**:

  ```
  bx cs cluster-subnet-add my_cluster 1643389
  ```
  {: pre}


### bx cs cluster-subnet-create CLUSTER SIZE VLAN_ID [-s]
{: #cs_cluster_subnet_create}

Crear una subred en una cuenta de infraestructura de IBM Cloud (SoftLayer) y ponerla a disponibilidad de un determinado clúster en {{site.data.keyword.containershort_notm}}.

**Nota:**
* Cuando pone una subred a disponibilidad de un clúster, las direcciones IP de esta subred se utilizan para la gestión de redes del clúster. Para evitar conflictos de direcciones IP, asegúrese de utilizar una subred con un solo clúster. No utilice una subred para varios clústeres o para otros fines externos a {{site.data.keyword.containershort_notm}} al mismo tiempo.
* Para direccionar entre subredes de la misma VLAN, se debe [activar la expansión de VLAN](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning).

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio. Para obtener una lista de los clústeres, utilice el [mandato](#cs_clusters) `cs bx clusters`.</dd>

   <dt><code><em>SIZE</em></code></dt>
   <dd>El número de direcciones IP de la subred. Este valor es obligatorio. Los valores posibles son 8, 16, 32 o 64.</dd>

   <dt><code><em>VLAN_ID</em></code></dt>
   <dd>La VLAN en el que se va a crear la subred. Este valor es obligatorio. Para ver una lista de las VLAN disponibles, utilice el [mandato](#cs_vlans) `bx cs vlans <location>`. </dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

   </dl>

**Ejemplo**:

  ```
  bx cs cluster-subnet-create my_cluster 8 1764905
  ```
  {: pre}


### bx cs cluster-user-subnet-add CLUSTER SUBNET_CIDR PRIVATE_VLAN
{: #cs_cluster_user_subnet_add}

Traer su propia subred privada a sus clústeres de {{site.data.keyword.containershort_notm}}.

Esta subred privada no es la que proporciona la infraestructura de IBM Cloud (SoftLayer). Por lo tanto, debe configurar el direccionamiento del tráfico de la red de entrada y de salida para la subred. Para añadir una subred de infraestructura de IBM Cloud (SoftLayer), utilice el [mandato](#cs_cluster_subnet_add) `bx cs cluster-subnet-add`.

**Nota**:
* Cuando añade una subred de usuario privada a un clúster, las direcciones IP de esta subred se utilizan para los equilibradores de carga privados del clúster. Para evitar conflictos de direcciones IP, asegúrese de utilizar una subred con un solo clúster. No utilice una subred para varios clústeres o para otros fines externos a {{site.data.keyword.containershort_notm}} al mismo tiempo.
* Para direccionar entre subredes de la misma VLAN, se debe [activar la expansión de VLAN](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning).

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code><em>SUBNET_CIDR</em></code></dt>
   <dd>El Classless InterDomain Routing (CIDR) de la subred. Este valor es obligatorio y no debe estar en conflicto con ninguna subred que utilice la infraestructura de IBM Cloud (SoftLayer).

   Los prefijos válidos son los comprendidos entre `/30` (1 dirección IP) y `/24` (253 direcciones IP). Si establece el valor de CIDR en una longitud de prefijo y luego lo tiene que modificar, añada primero un nuevo CIDR y luego [elimine el CIDR antiguo](#cs_cluster_user_subnet_rm).</dd>

   <dt><code><em>PRIVATE_VLAN</em></code></dt>
   <dd>El ID de la VLAN privada. Este valor es obligatorio. Debe coincidir con el ID de VLAN privada de uno o varios nodos trabajadores del clúster.</dd>
   </dl>

**Ejemplo**:

  ```
  bx cs cluster-user-subnet-add my_cluster 169.xx.xxx.xxx/29 1502175
  ```
  {: pre}


### bx cs cluster-user-subnet-rm CLUSTER SUBNET_CIDR PRIVATE_VLAN
{: #cs_cluster_user_subnet_rm}

Elimine su propia subred privada de un clúster especificado.

**Nota:** Cualquier servicio que se haya desplegado en una dirección IP desde su propia subred privada permanece activo después de que se elimine la subred.

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code><em>SUBNET_CIDR</em></code></dt>
   <dd>El Classless InterDomain Routing (CIDR) de la subred. Este valor es obligatorio y debe coincidir con el CIDR especificado mediante en [mandato](#cs_cluster_user_subnet_add) `bx cs cluster-user-subnet-add`.</dd>

   <dt><code><em>PRIVATE_VLAN</em></code></dt>
   <dd>El ID de la VLAN privada. Este valor es obligatorio y debe coincidir con el ID de VLAN especificado mediante en [mandato](#cs_cluster_user_subnet_add) `bx cs cluster-user-subnet-add`.</dd>
   </dl>

**Ejemplo**:

  ```
  bx cs cluster-user-subnet-rm my_cluster 169.xx.xxx.xxx/29 1502175
  ```
  {: pre}

### bx cs subnets [--json][-s]
{: #cs_subnets}

Ver una lista de subredes que están disponibles en una cuenta de infraestructura de IBM Cloud (SoftLayer).

<strong>Opciones del mandato</strong>:

  <dl>
  <dt><code>--json</code></dt>
  <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

  <dt><code>-s</code></dt>
  <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
  </dl>

**Ejemplo**:

  ```
  bx cs subnets
  ```
  {: pre}


<br />


## Mandatos del equilibrador de carga de aplicación (ALB) de Ingress
{: #alb_commands}

### bx cs alb-cert-deploy [--update] --cluster CLUSTER --secret-name SECRET_NAME --cert-crn CERTIFICATE_CRN [-s]
{: #cs_alb_cert_deploy}

Despliegue o actualice un certificado de la instancia de {{site.data.keyword.cloudcerts_long_notm}} al ALB de un clúster.

**Nota:**
* Sólo un usuario con el rol de acceso de Administrador puede ejecutar este mandato.
* Sólo puede actualizar certificados que importados de la misma instancia de {{site.data.keyword.cloudcerts_long_notm}}.

<strong>Opciones del mandato</strong>

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>--update</code></dt>
   <dd>Actualiza el certificado para un secreto del ALB en un clúster. Este valor es opcional.</dd>

   <dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
   <dd>El nombre del secreto de ALB. Este valor es obligatorio.</dd>

   <dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
   <dd>El CRN de certificado. Este valor es obligatorio.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
   </dl>

**Ejemplos**:

Ejemplo para desplegar un secreto de ALB:

   ```
   bx cs alb-cert-deploy --secret-name my_alb_secret --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
   ```
   {: pre}

Ejemplo para actualizar un secreto de ALB existente:

 ```
 bx cs alb-cert-deploy --update --secret-name my_alb_secret --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:7e21fde8ee84a96d29240327daee3eb2
 ```
 {: pre}


### bx cs alb-cert-get --cluster CLUSTER [--secret-name SECRET_NAME][--cert-crn CERTIFICATE_CRN] [--json][-s]
{: #cs_alb_cert_get}

Visualice información sobre un secreto de ALB en un clúster.

**Nota:** Sólo un usuario con el rol de acceso de Administrador puede ejecutar este mandato.

<strong>Opciones del mandato</strong>

  <dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
  <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

  <dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
  <dd>El nombre del secreto de ALB. Este valor es necesario para obtener información sobre un secreto de ALB específico en el clúster.</dd>

  <dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
  <dd>El CRN de certificado. Este valor es necesario para obtener información sobre todos los secretos de ALB coincidentes con un CRN de certificado determinado en el clúster.</dd>

  <dt><code>--json</code></dt>
  <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

  <dt><code>-s</code></dt>
  <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
  </dl>

**Ejemplos**:

 Ejemplo para captar un secreto de ALB:

 ```
 bx cs alb-cert-get --cluster my_cluster --secret-name my_alb_secret
 ```
 {: pre}

 Ejemplo para obtener información sobre todos los secretos de ALB que coinciden con un CRN de certificado especificado:

 ```
 bx cs alb-cert-get --cluster my_cluster --cert-crn  crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
 ```
 {: pre}


### bx cs alb-cert-rm --cluster CLUSTER [--secret-name SECRET_NAME][--cert-crn CERTIFICATE_CRN] [-s]
{: #cs_alb_cert_rm}

Elimine un secreto de ALB en un clúster.

**Nota:** Sólo un usuario con el rol de acceso de Administrador puede ejecutar este mandato.

<strong>Opciones del mandato</strong>

  <dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
  <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

  <dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
  <dd>El nombre del secreto de ALB. Este valor es necesario para eliminar un secreto de ALB específico en el clúster.</dd>

  <dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
  <dd>El CRN de certificado. Este valor es necesario para eliminar todos los secretos de ALB coincidentes con un CRN de certificado determinado en el clúster.</dd>

  <dt><code>-s</code></dt>
  <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

  </dl>

**Ejemplos**:

 Ejemplo para eliminar un secreto de ALB:

 ```
 bx cs alb-cert-rm --cluster my_cluster --secret-name my_alb_secret
 ```
 {: pre}

 Ejemplo para eliminar todos los secretos de ALB que coinciden con un CRN de certificado especificado:

 ```
 bx cs alb-cert-rm --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
 ```
 {: pre}


### bx cs alb-certs --cluster CLUSTER [--json][-s]
{: #cs_alb_certs}

Visualice una lista de secretos de ALB en un clúster.

**Nota:** Únicamente los usuarios con el rol de acceso de Administrador puede ejecutar este mandato.

<strong>Opciones del mandato</strong>

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>
   <dt><code>--json</code></dt>
   <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>
   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
   </dl>

**Ejemplo**:

 ```
 bx cs alb-certs --cluster my_cluster
 ```
 {: pre}

### bx cs alb-configure --albID ALB_ID [--enable][--disable][--user-ip USERIP][-s]
{: #cs_alb_configure}

Habilite o inhabilite un ALB en el clúster estándar. El ALB público está habilitado de forma predeterminada.

**Opciones del mandato**:

   <dl>
   <dt><code><em>--albID </em>ALB_ID</code></dt>
   <dd>El ID de un ALB. Ejecute <code>bx cs albs <em>--cluster </em>CLUSTER</code> para ver los ID de los ALB de un clúster. Este valor es obligatorio.</dd>

   <dt><code>--enable</code></dt>
   <dd>Incluya este distintivo para habilitar un ALB en un clúster.</dd>

   <dt><code>--disable</code></dt>
   <dd>Incluya este distintivo para inhabilitar un ALB en un clúster.</dd>

   <dt><code>--user-ip <em>USER_IP</em></code></dt>
   <dd>

   <ul>
    <li>Este parámetro está disponible para habilitar un ALB privado únicamente.</li>
    <li>El ALB privado se despliega con una dirección IP de una subred privada proporcionada por un usuario. Si no se proporciona la dirección IP, el ALB se despliega con una dirección IP privada de la subred privada portátil que se suministra automáticamente cuando se crea el clúster.</li>
   </ul>
   </dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

   </dl>

**Ejemplos**:

  Ejemplo para habilitar un ALB:

  ```
  bx cs alb-configure --albID private-cr18a61a63a6a94b658596aa93a087aaa9-alb1 --enable
  ```
  {: pre}

  Ejemplo para habilitar un ALB con una dirección IP proporcionada por un usuario:

  ```
  bx cs alb-configure --albID private-cr18a61a63a6a94b658596aa93a087aaa9-alb1 --enable --user-ip user_ip
  ```
  {: pre}

  Ejemplo para inhabilitar un ALB:

  ```
  bx cs alb-configure --albID public-cr18a61a63a6a94b658596aa93a087aaa9-alb1 --disable
  ```
  {: pre}

### bx cs alb-get --albID ALB_ID [--json][-s]
{: #cs_alb_get}

Visualice los detalles de un ALB.

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code><em>--albID </em>ALB_ID</code></dt>
   <dd>El ID de un ALB. Ejecute <code>bx cs albs --cluster <em>CLUSTER</em></code> para ver los ID de los ALB de un clúster. Este valor es obligatorio.</dd>

   <dt><code>--json</code></dt>
   <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

  <dt><code>-s</code></dt>
  <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

   </dl>

**Ejemplo**:

  ```
  bx cs alb-get --albID public-cr18a61a63a6a94b658596aa93a087aaa9-alb1
  ```
  {: pre}

### bx cs alb-types [--json][-s]
{: #cs_alb_types}

Visualice los tipos de ALB que están soportados en la región.

<strong>Opciones del mandato</strong>:

  <dl>
  <dt><code>--json</code></dt>
  <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

  <dt><code>-s</code></dt>
  <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
  </dl>

**Ejemplo**:

  ```
  bx cs alb-types
  ```
  {: pre}


### bx cs albs --cluster CLUSTER [--json][-s]
{: #cs_albs}

Visualice el estado de todos los ALB de un clúster. Si no se devuelve ningún ID de ALB, significa que el clúster no tiene subred portátil. Puede [crear](#cs_cluster_subnet_create) o [añadir](#cs_cluster_subnet_add) subredes a un clúster.

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code><em>--cluster </em>CLUSTER</code></dt>
   <dd>El nombre o ID del clúster en el que se listan los ALB disponibles. Este valor es obligatorio.</dd>

   <dt><code>--json</code></dt>
   <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

   </dl>

**Ejemplo**:

  ```
  bx cs albs --cluster my_cluster
  ```
  {: pre}


<br />


## Mandatos de infraestructura
{: #infrastructure_commands}

### bx cs credentials-set --infrastructure-api-key API_KEY --infrastructure-username USERNAME [-s]
{: #cs_credentials_set}

Defina las credenciales de cuenta de la infraestructura de IBM Cloud (SoftLayer) para su cuenta de {{site.data.keyword.containershort_notm}}.

Si tiene una cuenta de pago según uso de {{site.data.keyword.Bluemix_notm}}, tiene acceso al portafolio de infraestructura de IBM Cloud (de SoftLayer) de forma predeterminada. Sin embargo, es posible que desee utilizar otra cuenta de infraestructura de IBM Cloud (SoftLayer) que ya tenga para solicitar infraestructura. Puede enlazar esta cuenta de infraestructura a la cuenta de {{site.data.keyword.Bluemix_notm}} mediante este mandato.

Si las credenciales de infraestructura de IBM Cloud (SoftLayer) se establecen manualmente, estas credenciales se utilizan para pedir infraestructura, aunque ya exista una [clave de API IAM](#cs_api_key_info) para la cuenta. Si el usuario cuyas credenciales se almacenan no tiene los permisos necesarios para pedir infraestructura, entonces las acciones relacionadas con la infraestructura, como crear un clúster o recargar un nodo trabajador, pueden fallar.

No puede definir varias credenciales para una cuenta de {{site.data.keyword.containershort_notm}}. Cada cuenta de {{site.data.keyword.containershort_notm}} está vinculada a un portafolio de la infraestructura de IBM Cloud (SoftLayer).

**Importante:** Antes de utilizar este mandato, asegúrese de que el usuario cuyas credenciales se utilizan tiene los [permisos de infraestructura de IBM Cloud (SoftLayer) e {{site.data.keyword.containershort_notm}}](cs_users.html#users) necesarios.

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--infrastructure-username <em>USERNAME</em></code></dt>
   <dd>Nombre usuario de la cuenta de infraestructura de IBM Cloud (SoftLayer). Este valor es obligatorio.</dd>


   <dt><code>--infrastructure-api-key <em>API_KEY</em></code></dt>
   <dd>Clave de API de la cuenta de infraestructura de IBM Cloud (SoftLayer). Este valor es obligatorio.

 <p>
  Para generar una clave de API:

  <ol>
  <li>Inicie sesión en el [portal de la infraestructura de IBM Cloud (SoftLayer) ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://control.softlayer.com/).</li>
  <li>Seleccione <strong>Cuenta</strong> y, a continuación, <strong>Usuarios</strong>.</li>
  <li>Pulse <strong>Generar</strong> para generar una clave de API de la infraestructura de IBM Cloud (SoftLayer) para su cuenta.</li>
  <li>Copie la clave de la API para utilizar en este mandato.</li>
  </ol>

  Para ver una clave de API existente:
  <ol>
  <li>Inicie sesión en el [portal de la infraestructura de IBM Cloud (SoftLayer) ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://control.softlayer.com/).</li>
  <li>Seleccione <strong>Cuenta</strong> y, a continuación, <strong>Usuarios</strong>.</li>
  <li>Pulse <strong>Ver</strong> para ver la clave de API existente.</li>
  <li>Copie la clave de la API para utilizar en este mandato.</li>
  </ol>
  </p></dd>

  <dt><code>-s</code></dt>
  <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

  </dl>

**Ejemplo**:

  ```
  bx cs credentials-set --infrastructure-api-key <api_key> --infrastructure-username dbmanager
  ```
  {: pre}


### bx cs credentials-unset
{: #cs_credentials_unset}

Elimine las credenciales de cuenta de la infraestructura de IBM Cloud (SoftLayer) de su cuenta de {{site.data.keyword.containershort_notm}}.

Una vez que elimina las credenciales, la [clave de API IAM](#cs_api_key_info) se utiliza para pedir recursos en la infraestructura de IBM Cloud (SoftLayer).

<strong>Opciones del mandato</strong>:

  <dl>
  <dt><code>-s</code></dt>
  <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
  </dl>

**Ejemplo**:

  ```
  bx cs credentials-unset
  ```
  {: pre}


### bx cs machine-types LOCATION [--json][-s]
{: #cs_machine_types}

Ver una lista de los tipos de máquinas disponibles para sus nodos trabajadores. Los tipos de máquina varían según la ubicación. Cada tipo de máquina incluye cantidad de CPU virtual, memoria y espacio de disco para cada nodo trabajador del clúster. De forma predeterminada, el directorio `/var/lib/docker`, donde están almacenados todos los datos de los contenedores, está cifrado mediante LUKS. Si la opción `disable-disk-encrypt` se incluye durante la creación de clústeres, los datos de Docker del host no se cifrarán. [Más información sobre el cifrado.](cs_secure.html#encrypted_disks)
{:shortdesc}

Puede suministrar el nodo trabajador como una máquina virtual en hardware dedicado o compartido, o como una máquina física en un servidor nativo.

<dl>
<dt>¿Por qué debería utilizar máquinas física (nativas)?</dt>
<dd><p><strong>Más recursos de cálculo</strong>: Puede suministrar el nodo trabajador como un servidor físico de arrendatario único, también denominado servidor nativo. Los servidores nativos ofrecen acceso directo a los recursos físicos en la máquina, como la memoria o la CPU. Esta configuración elimina el hipervisor de máquina virtual que asigna recursos físicos a máquinas virtuales que se ejecutan en el host. En su lugar, todos los recursos de una máquina nativa están dedicados exclusivamente al trabajador, por lo que no es necesario preocuparse por "vecinos ruidosos" que compartan recursos o ralenticen el rendimiento. Los tipos de máquina física tienen más almacenamiento local que virtual, y algunos tienen RAID para realizar copias de seguridad de datos locales.</p>
<p><strong>Facturación mensual</strong>: los servidores nativos son más caros que los servidores virtuales, y son más apropiados para apps de alto rendimiento que necesitan más recursos y control de host. Los servidores nativos se facturan de forma mensual. Si cancela un servidor nativo antes de fin de mes, se le facturará a finales de ese mes. La realización de pedidos de servidores nativos, y su cancelación, es un proceso manual que se realiza a través de su cuenta (SoftLayer) de la infraestructura de IBM Cloud. Puede ser necesario más de un día laborable para completar la tramitación.</p>
<p><strong>Opción para habilitar Trusted Compute</strong>: Habilite Trusted Compute para protegerse ante la manipulación indebida de nodos trabajadores. Si no habilita la confianza durante la creación del clúster pero desea hacerlo posteriormente, puede utilizar el [mandato](cs_cli_reference.html#cs_cluster_feature_enable) `bx cs feature-enable`. Una vez que habilita la confianza, no puede inhabilitarla posteriormente. Puede crear un nuevo clúster sin confianza. Para obtener más información sobre cómo funciona la confianza durante el proceso de inicio del nodo, consulte [{{site.data.keyword.containershort_notm}} con Trusted Compute](cs_secure.html#trusted_compute). Trusted Compute está disponible en los clústeres donde se ejecuta Kubernetes versión 1.9 o posterior y poseen determinados tipos de máquina nativos. Cuando ejecute el [mandato](cs_cli_reference.html#cs_machine_types) `bx cs machine-types <location>`, en el campo **Trustable** puede ver qué máquinas dan soporte a la confianza. Por ejemplo, los distintos tipos de GPU `mgXc` no dan soporte a Trusted Compute.</p></dd>
<dt>¿Por qué debería utilizar máquinas virtuales?</dt>
<dd><p>Las máquinas virtuales ofrecen una mayor flexibilidad, unos tiempos de suministro más reducidos y proporcionan más características automáticas de escalabilidad que las máquinas nativas, a un precio más reducido. Utilice máquinas virtuales en los casos de uso con un propósito más general como, por ejemplo, en entornos de desarrollo y pruebas, entornos de transferencia y producción, microservicios y apps empresariales. Sin embargo, deberá encontrar un compromiso con su rendimiento. Si necesita un alto rendimiento de cálculo con cargas de trabajo intensivas de RAM, datos o GPU, utilice máquinas nativas.</p>
<p><strong>Decida entre la tenencia múltiple o única</strong>: Cuando se crea un clúster virtual estándar, debe seleccionar si desea que el hardware subyacente se comparta entre varios clientes de {{site.data.keyword.IBM_notm}} (tenencia múltiple) o se le dedique a usted exclusivamente (tenencia única).</p>
<p>En una configuración de tenencia múltiple, los recursos físicos, como CPU y memoria, se comparten entre todas las máquinas virtuales desplegadas en el mismo hardware físico. Para asegurarse de que cada máquina virtual se pueda ejecutar de forma independiente, un supervisor de máquina virtual, también conocido como hipervisor, segmenta los recursos físicos en entidades aisladas y los asigna como recursos dedicados a una máquina virtual (aislamiento de hipervisor).</p>
<p>En una configuración de tenencia única, se dedican al usuario todos los recursos físicos. Puede desplegar varios nodos trabajadores como máquinas virtuales en el mismo host físico. De forma similar a la configuración de tenencia múltiple,
el hipervisor asegura que cada nodo trabajador recibe su parte compartida de los recursos físicos disponibles.</p>
<p>Los nodos compartidos suelen resultar más económicos que los nodos dedicados porque los costes del hardware subyacente se comparten entre varios clientes. Sin embargo, cuando decida entre nodos compartidos y dedicados, debe ponerse en contacto con el departamento legal y ver el nivel de aislamiento y de conformidad de la infraestructura que necesita el entorno de app.</p>
<p><strong>Tipos de máquinas virtuales `u2c` o `b2c`</strong>: Estas máquinas utilizan el disco local en lugar de la red de área de almacenamiento (SAN) por motivos de fiabilidad. Entre las ventajas de fiabilidad se incluyen un mejor rendimiento al serializar bytes en el disco local y una reducción de la degradación del sistema de archivos debido a anomalías de la red. Este tipo de máquinas contienen 25 GB de almacenamiento en disco local primario para el sistema de archivos de SO y 100 GB de almacenamiento en disco local secundario para `/var/lib/docker`, el directorio en el que se graban todos los datos del contenedor.</p>
<p><strong>¿Qué hago si tengo tipos de máquina `u1c` o `b1c` en desuso?</strong> Para empezar a utilizar los tipos de máquina `u2c` y `b2c`, [actualice los tipos de máquina añadiendo nodos trabajadores](cs_cluster_update.html#machine_type).</p></dd>
<dt>¿Qué tipos de máquina virtual y física puedo elegir?</dt>
<dd><p>¡Muchos! Seleccione el tipo de máquina mejor se adecue a su caso de uso. Recuerde que una agrupación de trabajadores está formada por máquinas del mismo tipo. Si desea una combinación de varios tipos de máquina en el clúster, cree agrupaciones de trabajadores separadas para cada tipo.</p>
<p>Los tipos de máquina varían por zona. Para ver los tipos de máquinas disponibles en su zona, ejecute `bx cs machine-types <zone_name>`.</p>
<p><table>
<caption>Tipos de máquina físicos (nativos) y virtuales en {{site.data.keyword.containershort_notm}}.</caption>
<thead>
<th>Nombre y caso de uso</th>
<th>Núcleos / Memoria</th>
<th>Disco primario / secundario</th>
<th>Velocidad de red</th>
</thead>
<tbody>
<tr>
<td><strong>Virtual, u2c.2x4</strong>: Utilice esta máquina virtual con el tamaño más reducido para realizar pruebas rápidas, pruebas de conceptos y ejecutar otras cargas ligeras.</td>
<td>2 / 4GB</td>
<td>25GB / 100GB</td>
<td>1000Mbps</td>
</tr>
<tr>
<td><strong>Virtual, b2c.4x16</strong>: Seleccione esta máquina virtual equilibrada para realizar pruebas y desarrollo, y para otras cargas de trabajo ligeras.</td>
<td>4 / 16GB</td>
<td>25GB / 100GB</td>
<td>1000Mbps</td>
</tr>
<tr>
<td><strong>Virtual, b2c.16x64</strong>: Seleccione esta máquina virtual equilibrada para cargas de trabajo de tamaño medio.</td></td>
<td>16 / 64GB</td>
<td>25GB / 100GB</td>
<td>1000Mbps</td>
</tr>
<tr>
<td><strong>Virtual, b2c.32x128</strong>: Seleccione esta máquina virtual equilibrada para cargas de trabajo de tamaño medio a grande, por ejemplo, como base de datos y sitio web dinámico con muchos usuarios simultáneos.</td></td>
<td>32 / 128GB</td>
<td>25GB / 100GB</td>
<td>1000Mbps</td>
</tr>
<tr>
<td><strong>Virtual, b2c.56x242</strong>: Seleccione esta máquina virtual equilibrada para cargas de trabajo grandes, por ejemplo, como base de datos y para varias apps con muchos usuarios simultáneos.</td></td>
<td>56 / 242GB</td>
<td>25GB / 100GB</td>
<td>1000Mbps</td>
</tr>
<tr>
<td><strong>Máquina nativa gran capacidad de memoria, mr1c.28x512</strong>: Maximice la RAM disponible para sus nodos trabajadores.</td>
<td>28 / 512GB</td>
<td>2TB SATA / 960GB SSD</td>
<td>10000Mbps</td>
</tr>
<tr>
<td><strong>Máquina nativas con GPU, mg1c.16x128</strong>: Elija este tipo para cargas de trabajo matemáticas intensivas, por ejemplo, para la computación de alto rendimiento, el aprendizaje máquina u otras aplicaciones 3D. Este tipo tiene una tarjeta física Tesla K80 con dos unidades de proceso gráfico (GPU) por tarjeta (2 GPU).</td>
<td>16 / 128GB</td>
<td>2TB SATA / 960GB SSD</td>
<td>10000Mbps</td>
</tr>
<tr>
<td><strong>Máquina nativas con GPU, mg1c.28x256</strong>: Elija este tipo para cargas de trabajo matemáticas intensivas, por ejemplo, para la computación de alto rendimiento, el aprendizaje máquina u otras aplicaciones 3D. Este tipo tiene 2 tarjetas físicas Tesla K80 con 2 GPU por tarjeta, para hacer un total de 4 GPU.</td>
<td>28 / 256GB</td>
<td>2TB SATA / 960GB SSD</td>
<td>10000Mbps</td>
</tr>
<tr>
<td><strong>Máquina nativa intensiva para datos, md1c.16x64.4x4tb</strong>: Para una cantidad significativa de almacenamiento local, incluido RAID para respaldar datos que se almacenan locamente en la máquina. Casos de uso de ejemplo: sistemas de archivos distribuidos, bases de datos grandes y cargas de trabajo analíticas de Big Data.</td>
<td>16 / 64GB</td>
<td>2x2TB RAID1 / 4x4TB SATA RAID10</td>
<td>10000Mbps</td>
</tr>
<tr>
<td><strong>Máquina nativa intensiva para datos, md1c.28x512.4x4tb</strong>: Para una cantidad significativa de almacenamiento local, incluido RAID para respaldar datos que se almacenan locamente en la máquina. Casos de uso de ejemplo: sistemas de archivos distribuidos, bases de datos grandes y cargas de trabajo analíticas de Big Data.</td>
<td>28 / 512 GB</td>
<td>2x2TB RAID1 / 4x4TB SATA RAID10</td>
<td>10000Mbps</td>
</tr>
<tr>
<td><strong>Máquina nativa equilibrada, mb1c.4x32</strong>: Para cargas de trabajo equilibradas que requieren más recursos de computación que los ofrecidos por las máquinas virtuales.</td>
<td>4 / 32GB</td>
<td>2TB SATA / 2TB SATA</td>
<td>10000Mbps</td>
</tr>
<tr>
<td><strong>Máquina nativa equilibrada, mb1c.16x64</strong>: Para cargas de trabajo equilibradas que requieren más recursos de computación que los ofrecidos por las máquinas virtuales.</td>
<td>16 / 64GB</td>
<td>2TB SATA / 960GB SSD</td>
<td>10000Mbps</td>
</tr>
</tbody>
</table>
</p>
</dd>
</dl>


<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code><em>LOCATION</em></code></dt>
   <dd>Especifique la ubicación de la que desea ver una lista de tipos de máquina disponibles. Este valor es obligatorio. Consulte [ubicaciones disponibles](cs_regions.html#locations).</dd>

   <dt><code>--json</code></dt>
  <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

  <dt><code>-s</code></dt>
  <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
  </dl>

**Mandato de ejemplo**:

  ```
  bx cs machine-types dal10
  ```
  {: pre}

**Salida de ejemplo**:

  ```
  Getting machine types list...
  OK
  Machine Types
  Name                 Cores   Memory   Network Speed   OS             Server Type   Storage   Secondary Storage   Trustable
  u2c.2x4              2       4GB      1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  b2c.4x16             4       16GB     1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  b2c.16x64            16      64GB     1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  b2c.32x128           32      128GB    1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  b2c.56x242           56      242GB    1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  mb1c.4x32            4       32GB     10000Mbps       UBUNTU_16_64   physical      1000GB    2000GB              False
  mb1c.16x64           16      64GB     10000Mbps       UBUNTU_16_64   physical      1000GB    1700GB              False
  mr1c.28x512          28      512GB    10000Mbps       UBUNTU_16_64   physical      1000GB    1700GB              False
  md1c.16x64.4x4tb     16      64GB     10000Mbps       UBUNTU_16_64   physical      1000GB    8000GB              False
  md1c.28x512.4x4tb    28      512GB    10000Mbps       UBUNTU_16_64   physical      1000GB    8000GB              False
  
  ```
  {: screen}


### bx cs vlans LOCATION [--all][--json] [-s]
{: #cs_vlans}

Crear una lista de las VLAN públicas y privadas disponibles para una ubicación en la cuenta de infraestructura de IBM Cloud (SoftLayer). Para ver una lista de las VLAN disponibles,
debe tener una cuenta de pago.

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code><em>LOCATION</em></code></dt>
   <dd>Escriba la ubicación donde desea listar sus VLAN públicas y privadas. Este valor es obligatorio. Consulte [ubicaciones disponibles](cs_regions.html#locations).</dd>

   <dt><code>--all</code></dt>
   <dd>Obtenga una lista de todas las VLAN disponibles. De forma predeterminada, las VLAN se filtran para mostrar solo las VLAN que son válidas. Para ser válida, una VLAN debe estar asociada con infraestructura que pueda alojar un trabajador con almacenamiento en disco local.</dd>

   <dt><code>--json</code></dt>
  <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

  <dt><code>-s</code></dt>
  <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
   </dl>

**Ejemplo**:

  ```
  bx cs vlans dal10
  ```
  {: pre}


<br />


## Mandatos de registro
{: #logging_commands}

### bx cs logging-config-create CLUSTER --logsource LOG_SOURCE [--namespace KUBERNETES_NAMESPACE][--hostname LOG_SERVER_HOSTNAME_OR_IP] [--port LOG_SERVER_PORT][--space CLUSTER_SPACE] [--org CLUSTER_ORG][--app-containers CONTAINERS] [--app-paths PATHS_TO_LOGS][--syslog-protocol PROTOCOL] --type LOG_TYPE [--json][--skip-validation] [-s]
{: #cs_logging_create}

Crear una configuración de registro. Puede utilizar este mandato para reenviar los registros correspondientes a contenedores, aplicaciones, nodos trabajadores, clústeres de Kubernetes y equilibradores de carga de aplicación de Ingress a {{site.data.keyword.loganalysisshort_notm}} o a un servidor syslog externo.

<strong>Opciones del mandato</strong>:

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>El nombre o ID del clúster.</dd>

  <dt><code>--logsource <em>LOG_SOURCE</em></code></dt>    
    <dd>El origen del registro para habilitar su reenvío. Este argumento da soporte a una lista separada por comas de orígenes de registro a los que aplicar la configuración. Los valores aceptados son <code>container</code>, <code>application</code>, <code>worker</code>, <code>kubernetes</code>, <code>ingress</code> y <code>kube-audit</code>. Si no proporciona un origen de registro, las configuraciones de registro se crean para los orígenes de registro <code>container</code> e <code>ingress</code>.</dd>

  <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
    <dd>El espacio de Kubernetes desde el que desea reenviar registros. El reenvío de registros no recibe soporte para los espacios de nombres de Kubernetes <code>ibm-system</code> y <code>kube-system</code>. Este valor sólo es válido para el origen de registro de contenedor y es opcional. Si no especifica un espacio de nombres, todos los espacios de nombres del clúster utilizarán esta configuración.</dd>

  <dt><code>--hostname <em>LOG_SERVER_HOSTNAME</em></code></dt>
    <dd>Cuando el tipo de registro es <code>syslog</code>, el nombre de host o dirección IP del servidor del recopilador de registro. Este valor es obligatorio para <code>syslog</code>. Cuando el tipo de registro es <code>ibm</code>, el URL de ingestión de {{site.data.keyword.loganalysislong_notm}}. Puede encontrar la lista de URL de ingestión disponible [aquí](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls). Si no especifica un URL de ingestión, se utiliza el punto final de la región en la que se ha creado el clúster.</dd>

  <dt><code>--port <em>LOG_SERVER_PORT</em></code></dt>
    <dd>El puerto del servidor del recopilador de registro. Este valor es opcional. Si no especifica un puerto, se utiliza el puerto estándar <code>514</code> para <code>syslog</code> y el puerto estándar <code>9091</code> para <code>ibm</code>.</dd>

  <dt><code>--space <em>CLUSTER_SPACE</em></code></dt>
    <dd>El nombre del espacio de Cloud Foundry al que desea enviar registros. Este valor sólo es válido para el registro de tipo <code>ibm</code> y es opcional. Si no especifica un espacio, los registros se envían al nivel de cuenta.</dd>

  <dt><code>--org <em>CLUSTER_ORG</em></code></dt>
    <dd>El nombre de la organización de Cloud Foundry en la que está el espacio. Este valor sólo es válido para el registro de tipo <code>ibm</code> y es necesario si ha especificado un espacio.</dd>

  <dt><code>--app-paths</code></dt>
    <dd>La vía de acceso al contenedor donde las apps realizan registros. Para reenviar registros con el tipo de origen <code>application</code>, debe proporcionar una vía de acceso. Para especificar más de una vía de acceso, utilice una lista separada por comas. Este valor es necesario para el origen de registro <code>application</code>. Ejemplo: <code>/var/log/myApp1/&ast;,/var/log/myApp2/&ast;</code></dd>

  <dt><code>--syslog-protocol</code></dt>
    <dd>El protocolo de la capa de transferencia que se utiliza cuando el tipo de registro es <code>syslog</code>. Los valores soportados son <code>TCP</code> y el predeterminado <code>UDP</code>. Al reenviar a un servidor syslog con el protocolo <code>udp</code>, se truncan los registros por encima de 1 KB.</dd>

  <dt><code>--type <em>LOG_TYPE</em></code></dt>
    <dd>El destino al que desea reenviar los registros. Las opciones son <code>ibm</code>, que reenvía los registros a {{site.data.keyword.loganalysisshort_notm}} y <code>syslog</code>, que reenvía los registros a un servidor externo.</dd>

  <dt><code>--app-containers</code></dt>
    <dd>Para reenviar registros de apps, puede especificar el nombre del contenedor que contiene la app. Puede especificar más de un contenedor mediante una lista separada por comas. Si no se especifica ningún contenedor, se reenvían los registros de todos los contenedores que contienen las vías de acceso indicadas. Esta opción únicamente es válida para el origen de registro <code>application</code>.</dd>

  <dt><code>--json</code></dt>
    <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

  <dt><code>--skip-validation</code></dt>
    <dd>Omite la validación de los nombres de espacio y organización cuando se especifican. Al omitir la validación, disminuye el tiempo de proceso, pero si la configuración de registro no es válida, los registros no se reenviarán correctamente. Este valor es opcional.</dd>

    <dt><code>-s</code></dt>
    <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplos**:

Ejemplo de registro de tipo `ibm` que reenvía desde un origen de registro de `contenedor` en el puerto predeterminado 9091:

  ```
  bx cs logging-config-create my_cluster --logsource container --namespace my_namespace --hostname ingest.logging.ng.bluemix.net --type ibm
  ```
  {: pre}

Ejemplo de registro de tipo `syslog` que reenvía desde un origen de registro de `contenedor` en el puerto predeterminado 514:

  ```
  bx cs logging-config-create my_cluster --logsource container --namespace my_namespace  --hostname 169.xx.xxx.xxx --type syslog
  ```
  {: pre}

Ejemplo de registro de tipo `syslog` que reenvía registros desde un origen de `ingress` en un puerto distinto al predeterminado:

  ```
  bx cs logging-config-create my_cluster --logsource container --hostname 169.xx.xxx.xxx --port 5514 --type syslog
  ```
  {: pre}

### bx cs logging-config-get CLUSTER [--logsource LOG_SOURCE][--json] [-s]
{: #cs_logging_get}

Vea todas las configuraciones de reenvío de registro para un clúster, o filtre las configuraciones de registro en función del origen del registro.

<strong>Opciones del mandato</strong>:

 <dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

  <dt><code>--logsource <em>LOG_SOURCE</em></code></dt>
    <dd>El tipo de origen de registro que desea filtrar. Sólo se devolverán las configuraciones de registro de este origen de registro en el clúster. Los valores aceptados son <code>container</code>, <code>application</code>, <code>worker</code>, <code>kubernetes</code>, <code>ingress</code> y <code>kube-audit</code>. Este valor es opcional.</dd>

  <dt><code>--show-covering-filters</code></dt>
    <dd>Muestra los filtros de registro que hicieron los filtros anteriores obsoletos.</dd>

  <dt><code>--json</code></dt>
    <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

  <dt><code>-s</code></dt>
    <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
 </dl>

**Ejemplo**:

  ```
  bx cs logging-config-get my_cluster --logsource worker
  ```
  {: pre}


### bx cs logging-config-refresh CLUSTER [-s]
{: #cs_logging_refresh}

Renueve la configuración de registro para el clúster. Renueva la señal de registro para cualquier configuración de registro que se esté reenviando al nivel de espacio en el clúster.

<strong>Opciones del mandato</strong>:

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>-s</code></dt>
     <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplo**:

  ```
  bx cs logging-config-refresh my_cluster
  ```
  {: pre}


### bx cs logging-config-rm CLUSTER [--id LOG_CONFIG_ID][--all] [-s]
{: #cs_logging_rm}

Suprima una configuración de reenvío de registros o todas las configuraciones de registro de un clúster. Esto detiene el reenvío del registro a un servidor syslog remoto o a {{site.data.keyword.loganalysisshort_notm}}.

<strong>Opciones del mandato</strong>:

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

  <dt><code>--id <em>LOG_CONFIG_ID</em></code></dt>
   <dd>Si desea eliminar una única configuración de registro, el ID de la configuración de registro.</dd>

  <dt><code>--all</code></dt>
   <dd>El distintivo para eliminar todas las configuraciones de registro de un clúster.</dd>

   <dt><code>-s</code></dt>
     <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplo**:

  ```
  bx cs logging-config-rm my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e
  ```
  {: pre}


### bx cs logging-config-update CLUSTER --id LOG_CONFIG_ID [--namespace NAMESPACE][--hostname LOG_SERVER_HOSTNAME_OR_IP] [--port LOG_SERVER_PORT][--space CLUSTER_SPACE] [--org CLUSTER_ORG][--app-paths PATH] [--app-containers PATH] --type LOG_TYPE [--json][--skipValidation] [-s]
{: #cs_logging_update}

Actualizar los detalles de una configuración de reenvío de registros.

<strong>Opciones del mandato</strong>:

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

  <dt><code>--id <em>LOG_CONFIG_ID</em></code></dt>
   <dd>El ID de configuración de registro que desea actualizar. Este valor es obligatorio.</dd>

  <dt><code>--namespace <em>NAMESPACE</em></code>
    <dd>El espacio de Kubernetes desde el que desea reenviar registros. El reenvío de registros no recibe soporte para los espacios de nombres de Kubernetes <code>ibm-system</code> y <code>kube-system</code>. Este valor sólo es válido para el origen de registro <code>container</code>. Si no especifica un espacio de nombres, todos los espacios de nombres del clúster utilizarán esta configuración.</dd>

  <dt><code>--hostname <em>LOG_SERVER_HOSTNAME</em></code></dt>
   <dd>Cuando el tipo de registro es <code>syslog</code>, el nombre de host o dirección IP del servidor del recopilador de registro. Este valor es obligatorio para <code>syslog</code>. Cuando el tipo de registro es <code>ibm</code>, el URL de ingestión de {{site.data.keyword.loganalysislong_notm}}. Puede encontrar la lista de URL de ingestión disponible [aquí](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls). Si no especifica un URL de ingestión, se utiliza el punto final de la región en la que se ha creado el clúster.</dd>

   <dt><code>--port <em>LOG_SERVER_PORT</em></code></dt>
   <dd>El puerto del servidor del recopilador de registro. Este valor es opcional cuando el tipo de registro es <code>syslog</code>. Si no especifica un puerto, se utiliza el puerto estándar <code>514</code> para <code>syslog</code> y el <code>9091</code> para <code>ibm</code>.</dd>

   <dt><code>--space <em>CLUSTER_SPACE</em></code></dt>
   <dd>El nombre del espacio al que desea enviar registros. Este valor sólo es válido para el registro de tipo <code>ibm</code> y es opcional. Si no especifica un espacio, los registros se envían al nivel de cuenta.</dd>

   <dt><code>--org <em>CLUSTER_ORG</em></code></dt>
   <dd>El nombre de la organización en la que está el espacio. Este valor sólo es válido para el registro de tipo <code>ibm</code> y es necesario si ha especificado un espacio.</dd>

   <dt><code>--app-paths <em>PATH</em>,<em>PATH</em></code></dt>
     <dd>Vía de acceso de archivo absoluta en el contenedor del que recopilar los registros. Es posible utilizar comodines como, por ejemplo, '/var/log/*.log', sin embargo no es posible utilizar expresiones recursivas como, por ejemplo '/var/log/**/test.log'. Para especificar más de una vía de acceso, utilice una lista separada por comas. Este valor es obligatorio cuando se especifica 'application' como el origen de registro. </dd>

   <dt><code>--app-containers <em>PATH</em>,<em>PATH</em></code></dt>
     <dd>La vía de acceso en los contenedores en los que las apps están creando registros. Para reenviar registros con el tipo de origen <code>application</code>, debe proporcionar una vía de acceso. Para especificar más de una vía de acceso, utilice una lista separada por comas. Ejemplo: <code>/var/log/myApp1/&ast;,/var/log/myApp2/&ast;</code></dd>

   <dt><code>--type <em>LOG_TYPE</em></code></dt>
   <dd>El protocolo de reenvío de registros que desea utilizar. Actualmente se da soporte a <code>syslog</code> e <code>ibm</code>. Este valor es obligatorio.</dd>

   <dt><code>--json</code></dt>
   <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

   <dt><code>--skipValidation</code></dt>
   <dd>Omite la validación de los nombres de espacio y organización cuando se especifican. Al omitir la validación, disminuye el tiempo de proceso, pero si la configuración de registro no es válida, los registros no se reenviarán correctamente. Este valor es opcional.</dd>

   <dt><code>-s</code></dt>
     <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
     </dl>

**Ejemplo de registro de tipo `ibm`**:

  ```
  bx cs logging-config-update my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e --type ibm
  ```
  {: pre}

**Ejemplo de registro de tipo `syslog`**:

  ```
  bx cs logging-config-update my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e --hostname localhost --port 5514 --type syslog
  ```
  {: pre}


### bx cs logging-filter-create CLUSTER --type LOG_TYPE [--logging-configs CONFIGS][--namespace KUBERNETES_NAMESPACE] [--container CONTAINER_NAME][--level LOGGING_LEVEL] [--regex-message MESSAGE][--json] [-s]
{: #cs_log_filter_create}

Crea un filtro de registro. Utilice este mandato para filtrar los registros que su configuración de registro reenvía.

<strong>Opciones del mandato</strong>:

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>Nombre o ID de clúster que desea para crear un filtro de registro. Este valor es obligatorio.</dd>

  <dt><code>--type <em>LOG_TYPE</em></code></dt>
    <dd>Tipo de registro al que aplicar el filtro. Actualmente se da soporte a <code>all</code>, <code>container</code> y <code>host</code>.</dd>

  <dt><code>--logging-configs <em>CONFIGS</em></code></dt>
    <dd>Una lista separada por comas de los ID de configuración de registro. Si no se proporciona, el filtro se aplica a todas las configuraciones de registro de clúster que se pasan al filtro. Puede ver las configuraciones de registro que coinciden con el filtro utilizando con el mandato el distintivo <code>--show-matching-configs</code>. Este valor es opcional.</dd>

  <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
    <dd>Espacio de nombres de Kubernetes para el que desea filtrar registros. Este valor es opcional.</dd>

  <dt><code>--container <em>CONTAINER_NAME</em></code></dt>
    <dd>Nombre del contenedor desde el que desea filtrar registros. Este distintivo sólo se aplica cuando se utiliza el tipo de registro de <code>container</code>. Este valor es opcional.</dd>

  <dt><code>--level <em>LOGGING_LEVEL</em></code></dt>
    <dd>Filtra los registros en el nivel especificado y en los inferiores. Valores aceptables en su orden canónico son <code>fatal</code>, <code>error</code>, <code>warn/warning</code>, <code>info</code>, <code>debug</code> y <code>trace</code>. Este valor es opcional. Por ejemplo, si filtra registros al nivel <code>info</code>, también se filtran los niveles <code>debug</code> y <code>trace</code>. **Nota**: Puede utilizar este distintivo sólo cuando los mensajes de registro están en formato JSON y contienen un campo de nivel. Salida de ejemplo:
<code>{"log": "hello", "level": "info"}</code></dd>

  <dt><code>--regex-message <em>MESSAGE</em></code></dt>
    <dd>Filtra todos los registros que contienen un mensaje concreto que se escribe como una expresión regular. Este valor es opcional.</dd>

  <dt><code>--json</code></dt>
    <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

  <dt><code>-s</code></dt>
    <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>

**Ejemplos**:

Este ejemplo filtra todos los registros reenviados desde contenedores con el nombre `test-container` en el espacio de nombres predeterminado que se encuentren en el nivel debug o inferior con un mensaje que contenga "GET request".

  ```
  bx cs logging-filter-create example-cluster --type container --namespace default --container test-container --level debug --message "GET request"
  ```
  {: pre}

Este ejemplo filtra todos los registros que se reenvían, en un nivel info o inferior, desde un clúster específico. La salida se devuelve como JSON.

  ```
  bx cs logging-filter-create example-cluster --type all --level info --json
  ```
  {: pre}



### bx cs logging-filter-get CLUSTER [--id FILTER_ID][--show-matching-configs] [--show-covering-filters][--json] [-s]
{: #cs_log_filter_view}

Visualiza una configuración de filtro de registro. Utilice este mandato para visualizar los filtros de registro que haya creado.

<strong>Opciones del mandato</strong>:

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>Nombre o ID de clúster del que desea visualizar los filtros. Este valor es obligatorio.</dd>

  <dt><code>--id <em>FILTER_ID</em></code></dt>
    <dd>ID del filtro de registro que desea visualizar.</dd>

  <dt><code>--show-matching-configs</code></dt>
    <dd>Muestra las configuraciones de registro que coincidan con la configuración que está visualizando. Este valor es opcional.</dd>

  <dt><code>--show-covering-filters</code></dt>
    <dd>Muestra los filtros de registro que hicieron los filtros anteriores obsoletos. Este valor es opcional.</dd>

  <dt><code>--json</code></dt>
    <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

   <dt><code>-s</code></dt>
     <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>


### bx cs logging-filter-rm CLUSTER [--id FILTER_ID][--all] [-s]
{: #cs_log_filter_delete}

Suprime un filtro de registro. Utilice este mandato para eliminar un filtro de registro que haya creado.

<strong>Opciones del mandato</strong>:

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>Nombre o ID del clúster del que desea suprimir un filtro.</dd>

  <dt><code>--id <em>FILTER_ID</em></code></dt>
    <dd>El ID del filtro de registro para suprimir.</dd>

  <dt><code>--all</code></dt>
    <dd>Suprimir todos filtros de reenvío de registro. Este valor es opcional.</dd>

  <dt><code>-s</code></dt>
    <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>


### bx cs logging-filter-update CLUSTER --id FILTER_ID --type LOG_TYPE [--logging-configs CONFIGS][--namespace KUBERNETES_NAMESPACE] [--container CONTAINER_NAME][--level LOGGING_LEVEL] [--message MESSAGE][--json] [-s]
{: #cs_log_filter_update}

Actualiza un filtro de registro. Utilice este mandato para actualizar un filtro de registro que haya creado.

<strong>Opciones del mandato</strong>:

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>Nombre o ID de clúster que desea para actualizar un filtro de registro. Este valor es obligatorio.</dd>

 <dt><code>--id <em>FILTER_ID</em></code></dt>
    <dd>El ID del filtro de registro para actualizar.</dd>

  <dt><code>--type <em>LOG_TYPE</em></code></dt>
    <dd>Tipo de registro al que aplicar el filtro. Actualmente se da soporte a <code>all</code>, <code>container</code> y <code>host</code>.</dd>

  <dt><code>--logging-configs <em>CONFIGS</em></code></dt>
    <dd>Una lista separada por comas de los ID de configuración de registro. Si no se proporciona, el filtro se aplica a todas las configuraciones de registro de clúster que se pasan al filtro. Puede ver las configuraciones de registro que coinciden con el filtro utilizando con el mandato el distintivo <code>--show-matching-configs</code>. Este valor es opcional.</dd>

  <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
    <dd>Espacio de nombres de Kubernetes para el que desea filtrar registros. Este valor es opcional.</dd>

  <dt><code>--container <em>CONTAINER_NAME</em></code></dt>
    <dd>Nombre del contenedor desde el que desea filtrar registros. Este distintivo sólo se aplica cuando se utiliza el tipo de registro de <code>container</code>. Este valor es opcional.</dd>

  <dt><code>--level <em>LOGGING_LEVEL</em></code></dt>
    <dd>Filtra los registros en el nivel especificado y en los inferiores. Valores aceptables en su orden canónico son <code>fatal</code>, <code>error</code>, <code>warn/warning</code>, <code>info</code>, <code>debug</code> y <code>trace</code>. Este valor es opcional. Por ejemplo, si filtra registros al nivel <code>info</code>, también se filtran los niveles <code>debug</code> y <code>trace</code>. **Nota**: Puede utilizar este distintivo sólo cuando los mensajes de registro están en formato JSON y contienen un campo de nivel. Salida de ejemplo:
<code>{"log": "hello", "level": "info"}</code></dd>

  <dt><code>--message <em>MESSAGE</em></code></dt>
    <dd>Filtra los registros que contienen un mensaje concreto en el registro. La coincidencia del mensaje se realiza de forma literal y no como una expresión. Ejemplo: Los mensajes "Hello", "!"y "Hello, World!", se aplicarían al registro "Hello, World!". Este valor es opcional.</dd>

  <dt><code>--json</code></dt>
    <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

  <dt><code>-s</code></dt>
    <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
</dl>




<br />


## Mandatos de región
{: #region_commands}

### bx cs locations [--json][-s]
{: #cs_datacenters}

Ver una lista de las ubicaciones disponibles en las que puede crear un clúster. Las ubicaciones disponibles varían en función de la región en la que se ha conectado. Para cambiar de región, ejecute `bx cs region-set`.

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code>--json</code></dt>
   <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
   </dl>

**Ejemplo**:

  ```
  bx cs locations
  ```
  {: pre}


### bx cs region
{: #cs_region}

Busque la región de {{site.data.keyword.containershort_notm}} en la que está actualmente. Puede crear y gestionar clústeres específicos para la región. Utilice el mandato `bx cs region-set` para cambiar regiones.

**Ejemplo**:

```
bx cs region
```
{: pre}

**Salida**:
```
Region: us-south
```
{: screen}

### bx cs region-set [REGION]
{: #cs_region-set}

Establezca la región para {{site.data.keyword.containershort_notm}}. Puede crear y gestionar clústeres específicos para la región, y es posible que desee clústeres en varias regiones para tener una alta disponibilidad.

Por ejemplo, puede iniciar una sesión en {{site.data.keyword.Bluemix_notm}} en la Región EE.UU. sur y crear un clúster. A continuación, puede utilizar `bx cs region-set eu-central` para establecer la región UE Central como destino y crear otro clúster. Por último, puede utilizar `bx cs region-set us-south` para volver a EE.UU. sur para gestionar el clúster en esa región.

**Opciones del mandato**:

<dl>
<dt><code><em>REGION</em></code></dt>
<dd>Especifique la región que desea establecer como destino. Este valor es opcional. Si no indica ninguna región, puede seleccionarla de la lista en la salida.

Para obtener una lista de regiones disponibles, consulte [regiones y ubicaciones](cs_regions.html) o utilice el [mandato](#cs_regions) `bx cs regions`.</dd></dl>

**Ejemplo**:

```
bx cs region-set eu-central
```
{: pre}

```
bx cs region-set
```
{: pre}

**Salida**:
```
Choose a region:
1. ap-north
2. ap-south
3. eu-central
4. uk-south
5. us-east
6. us-south
Enter a number> 3
OK
```
{: screen}

### bx cs regions
{: #cs_regions}

Lista las regiones disponibles. `Region Name` es el nombre de {{site.data.keyword.containershort_notm}} y `Region Alias` es el nombre de {{site.data.keyword.Bluemix_notm}} general para la región.

**Ejemplo**:

```
bx cs regions
```
{: pre}

**Salida**:
```
Region Name   Region Alias
ap-north      jp-tok
ap-south      au-syd
eu-central    eu-de
uk-south      eu-gb
us-east       us-east
us-south      us-south
```
{: screen}


<br />


## Mandatos de nodo trabajador
{: worker_node_commands}


### bx cs worker-add --cluster CLUSTER [--file FILE_LOCATION][--hardware HARDWARE] --machine-type MACHINE_TYPE --number NUMBER --private-vlan PRIVATE_VLAN --public-vlan PUBLIC_VLAN [--disable-disk-encrypt][-s]
{: #cs_worker_add}

Añadir nodos trabajadores al clúster estándar.



<strong>Opciones del mandato</strong>:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

<dt><code>--file <em>FILE_LOCATION</em></code></dt>
<dd>La vía de acceso al archivo YAML para añadir nodos trabajadores al clúster. En lugar de definir los nodos trabajadores adicionales mediante las opciones que se proporcionan en este mandato, puede utilizar un archivo YAML. Este valor es opcional.

<p><strong>Nota:</strong> Si especifica la misma opción en el mandato y como parámetro en el archivo YAML, el valor en el mandato prevalece sobre el valor del archivo YAML. Por ejemplo, supongamos que define un tipo de máquina en el archivo YAML y utiliza la opción --machine-type en el mandato; el valor que especifique en la opción del mandato prevalece sobre el valor del archivo YAML.

<pre class="codeblock">
<code>name: <em>&lt;cluster_name_or_ID&gt;</em>
location: <em>&lt;location&gt;</em>
machine-type: <em>&lt;machine_type&gt;</em>
private-vlan: <em>&lt;private_VLAN&gt;</em>
public-vlan: <em>&lt;public_VLAN&gt;</em>
hardware: <em>&lt;shared_or_dedicated&gt;</em>
workerNum: <em>&lt;number_workers&gt;</em>
diskEncryption: <em>false</em></code></pre>

<table>
<caption>Visión general de los componentes del archivo YAML</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general de los componentes del archivo YAML</th>
</thead>
<tbody>
<tr>
<td><code><em>name</em></code></td>
<td>Sustituya <code><em>&lt;cluster_name_or_ID&gt;</em></code> por el nombre o ID del clúster donde desea añadir nodos trabajadores.</td>
</tr>
<tr>
<td><code><em>location</em></code></td>
<td>Sustituya <code><em>&lt;location&gt;</em></code> por la ubicación de despliegue de los nodos trabajadores. Las ubicaciones disponibles dependen de la región en la que ha iniciado la sesión. Para ver una lista de las ubicaciones disponibles, ejecute <code>cs bx ubicaciones</code>.</td>
</tr>
<tr>
<td><code><em>machine-type</em></code></td>
<td>Sustituya <code><em>&lt;machine_type&gt;</em></code> por el tipo de máquina en el que desea desplegar los nodos trabajadores. Puede desplegar los nodos trabajadores como máquinas virtuales en hardware dedicado o compartido, o como máquinas físicas en servidores nativos. Los tipos de máquinas físicas y virtuales varían según la ubicación en la que se despliega el clúster. Para obtener más información, consulte el [mandato](cs_cli_reference.html#cs_machine_types) `bx cs machine-types`.</td>
</tr>
<tr>
<td><code><em>private-vlan</em></code></td>
<td>Sustituya <code><em>&lt;private_VLAN&gt;</em></code> por el ID de la VLAN privada que desea utilizar para sus nodos trabajadores. Para ver una lista de las VLAN disponibles, ejecute <code>bx cs vlans <em>&lt;location&gt;</em></code> y busque direccionadores de VLAN que empiecen por <code>bcr</code> ("back-end router", direccionador de fondo).</td>
</tr>
<tr>
<td><code>public-vlan</code></td>
<td>Sustituya <code>&lt;public_VLAN&gt;</code> por el ID de la VLAN pública que desea utilizar para sus nodos trabajadores. Para ver una lista de las VLAN disponibles, ejecute <code>bx cs vlans &lt;location&gt;</code> y busque direccionadores de VLAN que empiecen por <code>fcr</code> ("front-end router", direccionador frontal). <br><strong>Nota</strong>: {[private_VLAN_vyatta]}</td>
</tr>
<tr>
<td><code>hardware</code></td>
<td>Para tipos de máquina virtual: El nivel de aislamiento del hardware del nodo trabajador. Utilice el valor dedicated si desea tener recursos físicos disponibles dedicados solo a usted, o el valor shared para permitir que los recursos físicos se compartan con otros clientes de IBM. El valor predeterminado es shared.</td>
</tr>
<tr>
<td><code>workerNum</code></td>
<td>Sustituya <code><em>&lt;number_workers&gt;</em></code> por el número de nodos trabajadores que desea desplegar.</td>
</tr>
<tr>
<td><code>diskEncryption: <em>false</em></code></td>
<td>Los nodos trabajadores tienen cifrado de disco de forma predeterminada; [más información](cs_secure.html#worker). Para inhabilitar el cifrado, incluya esta opción y establezca el valor en <code>false</code>.</td></tr>
</tbody></table></p></dd>

<dt><code>--hardware <em>HARDWARE</em></code></dt>
<dd>El nivel de aislamiento del hardware del nodo trabajador. Utilice el valor dedicated si desea tener recursos físicos disponibles dedicados solo a usted, o el valor shared para permitir que los recursos físicos se compartan con otros clientes de IBM. El valor predeterminado es shared. Este valor es opcional.</dd>

<dt><code>--machine-type <em>MACHINE_TYPE</em></code></dt>
<dd>Elija un tipo de máquina. Puede desplegar los nodos trabajadores como máquinas virtuales en hardware dedicado o compartido, o como máquinas físicas en servidores nativos. Los tipos de máquinas físicas y virtuales varían según la ubicación en la que se despliega el clúster. Para obtener más información, consulte la documentación del [mandato](cs_cli_reference.html#cs_machine_types) `bx cs machine-types`. Este valor es obligatorio para clústeres estándares y no está disponible para clústeres gratuitos.</dd>

<dt><code>--number <em>NUMBER</em></code></dt>
<dd>Un entero que representa el número de nodos trabajadores que desea crear en el clúster. El valor predeterminado es 1. Este valor es opcional.</dd>

<dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
<dd>La VLAN privada que se ha especificado al crear el clúster. Este valor es obligatorio.

<p><strong>Nota:</strong> Los direccionadores de VLAN privadas siempre empiezan por <code>bcr</code> (back-end router, direccionador de fondo) y los direccionadores de VLAN públicas siempre empiezan por <code>fcr</code> (direccionador frontal). Al crear un clúster especificando las VLAN privadas y públicas, deben coincidir el número y la combinación de letras después de dichos prefijos.</p></dd>

<dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
<dd>La VLAN pública que se ha especificado al crear el clúster. Este valor es opcional. Si desea que los nodos trabajadores existan solo en una VLAN privada, no proporcione ningún ID de VLAN pública. <strong>Nota</strong>: {[private_VLAN_vyatta]}

<p><strong>Nota:</strong> Los direccionadores de VLAN privadas siempre empiezan por <code>bcr</code> (back-end router, direccionador de fondo) y los direccionadores de VLAN públicas siempre empiezan por <code>fcr</code> (direccionador frontal). Al crear un clúster especificando las VLAN privadas y públicas, deben coincidir el número y la combinación de letras después de dichos prefijos.</p></dd>

<dt><code>--disable-disk-encrypt</code></dt>
<dd>Los nodos trabajadores tienen cifrado de disco de forma predeterminada; [más información](cs_secure.html#worker). Para inhabilitar el cifrado, incluya esta opción.</dd>

<dt><code>-s</code></dt>
<dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

</dl>

**Ejemplos**:

  ```
  bx cs worker-add --cluster my_cluster --number 3 --public-vlan my_public_VLAN_ID --private-vlan my_private_VLAN_ID --machine-type u2c.2x4 --hardware shared
  ```
  {: pre}

  Ejemplo para {{site.data.keyword.Bluemix_dedicated_notm}}:

  ```
  bx cs worker-add --cluster my_cluster --number 3 --machine-type u2c.2x4
  ```
  {: pre}

 en que despliega el clúster. Para obtener más información, consulte la documentación del [mandato](cs_cli_reference.html#cs_machine_types) `bx cs machine-types`. Este valor es obligatorio para clústeres estándares y no está disponible para clústeres gratuitos.</dd>

  <dt><code>--size-per-zone <em>WORKERS_PER_ZONE</em></code></dt>
    <dd>El número de trabajadores a crear en cada zona. Este valor es obligatorio.</dd>

  <dt><code>--kube-version <em>VERSION</em></code></dt>
    <dd>La versión de Kubernetes con la que desea que se creen sus nodos trabajadores. Se utiliza la versión predeterminada si no se especifica este valor.</dd>

  <dt><code>--hardware <em>HARDWARE</em></code></dt>
    <dd>El nivel de aislamiento del hardware del nodo trabajador. Utilice el valor dedicated si desea tener recursos físicos disponibles dedicados solo a usted, o el valor shared para permitir que los recursos físicos se compartan con otros clientes de IBM. El valor predeterminado es shared. Este valor es opcional.</dd>

  <dt><code>--labels <em>LABELS</em></code></dt>
    <dd>Las etiquetas que desea asignar a los trabajadores en su agrupación. Ejemplo: <key1>=<val1>,<key2>=<val2></dd>

  <dt><code>--private-only </code></dt>
    <dd>Especifica que no hay VLAN públicas en la agrupación de trabajadores. El valor
predeterminado es <code>false</code>.</dd>

  <dt><code>--diable-disk-encrpyt</code></dt>
    <dd>Especifica que el disco no está cifrado. El valor predeterminado es <code>false</code>.</dd>

</dl>

**Mandato de ejemplo**:

  ```
  bx cs worker-pool-add my_cluster --machine-type u2c.2x4 --size-per-zone 6
  ```
  {: pre}

### bx cs worker-pools --cluster CLUSTER
{: #cs_worker_pools}

Visualiza las agrupaciones de trabajadores que tiene en un clúster.

<strong>Opciones del mandato</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTER_NAME_OR_ID</em></code></dt>
    <dd>El nombre o ID del clúster cuyas agrupaciones de trabajadores desea listar. Este valor es obligatorio.</dd>
</dl>

**Mandato de ejemplo**:

  ```
  bx cs worker-pools --cluster my_cluster
  ```
  {: pre}

### bx cs worker-pool-get --worker-pool WORKER_POOL --cluster CLUSTER
{: #cs_worker_pool_get}

Visualiza los detalles de una agrupación de trabajadores.

<strong>Opciones del mandato</strong>:

<dl>
  <dt><code>--worker-pool <em>WORKER_POOL</em></code></dt>
    <dd>El nombre de la agrupación de nodos trabajadores cuyos detalles desea visualizar. Este valor es obligatorio.</dd>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>El nombre o el ID del clúster en el que se encuentra la agrupación de trabajadores. Este valor es obligatorio.</dd>
</dl>

**Mandato de ejemplo**:

  ```
  bx cs worker-pool-get --worker-pool pool1 --cluster my_cluster
  ```
  {: pre}

### bx cs worker-pool-update --worker-pool WORKER_POOL --cluster CLUSTER
{: #cs_worker_pool_update}

Actualice todos los nodos trabajadores en su agrupación con la última versión de Kubernetes que coincida con el maestro especificado.

<strong>Opciones del mandato</strong>:

<dl>
  <dt><code>--worker-pool <em>WORKER_POOL</em></code></dt>
    <dd>El nombre de la agrupación de nodos trabajadores que desea actualizar. Este valor es obligatorio.</dd>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>El nombre o ID del clúster cuyas agrupaciones de trabajadores desea actualizar. Este valor es obligatorio.</dd>
</dl>

**Mandato de ejemplo**:

  ```
  bx cs worker-pool-update --worker-pool pool1 --cluster my_cluster
  ```
  {: pre}



### bx cs worker-pool-resize --worker-pool WORKER_POOL --cluster CLUSTER --size-per-zone WORKERS_PER_ZONE
{: #cs_worker_pool_resize}

Redimensione su agrupación de trabajadores para aumentar o disminuir el número de nodos trabajadores que están en cada zona de su clúster.

<strong>Opciones del mandato</strong>:

<dl>
  <dt><code>--worker-pool <em>WORKER_POOL</em></code></dt>
    <dd>El nombre de la agrupación de nodos trabajadores que desea actualizar. Este valor es obligatorio.</dd>

  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>El nombre o ID del clúster cuyas agrupaciones de trabajadores desea redimensionar. Este valor es obligatorio.</dd>

  <dt><code>--size-per-zone <em>WORKERS_PER_ZONE</em></code></dt>
    <dd>El número de trabajadores que desea crear en cada zona. Este valor es obligatorio.</dd>
</dl>

**Mandato de ejemplo**:

  ```
  bx cs worker-pool-update --cluster my_cluster --worker-pool pool1,pool2 --size-per-zone 3
  ```
  {: pre}

### bx cs worker-pool-rm --worker-pool WORKER_POOL --cluster CLUSTER
{: #cs_worker_pool_rm}

Elimina del clúster una agrupación de trabajadores. Se suprimen todos los nodos trabajadores en la agrupación. Al realizar la supresión se vuelven a planificar todos los pods. Para evitar el tiempo de inactividad, asegúrese de que haya suficientes trabajadores para ejecutar la carga de trabajo.

<strong>Opciones del mandato</strong>:

<dl>
  <dt><code>--worker-pool <em>WORKER_POOL</em></code></dt>
    <dd>El nombre de la agrupación de nodos trabajadores que desea eliminar. Este valor es obligatorio.</dd>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>El nombre o ID de clúster del que desea eliminar la agrupación de trabajadores. Este valor es obligatorio.</dd>
</dl>

**Mandato de ejemplo**:

  ```
  bx cs worker-pool-rm --cluster my_cluster --worker-pool pool1
  ```
  {: pre}

</staging>

### bx cs worker-get [CLUSTER_NAME_OR_ID] WORKER_NODE_ID [--json][-s]
{: #cs_worker_get}

Visualiza los detalles de un nodo trabajador.

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code><em>CLUSTER_NAME_OR_ID</em></code></dt>
   <dd>El nombre o ID del clúster del nodo trabajador. Este valor es opcional.</dd>

   <dt><code><em>WORKER_NODE_ID</em></code></dt>
   <dd>El nombre del nodo trabajador. Ejecute <code>bx cs workers <em>CLUSTER</em></code> para ver los ID de los nodos
trabajadores de un clúster. Este valor es obligatorio.</dd>

   <dt><code>--json</code></dt>
   <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
   </dl>

**Mandato de ejemplo**:

  ```
  bx cs worker-get my_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1
  ```
  {: pre}

**Salida de ejemplo**:

  ```
  ID:           kube-dal10-123456789-w1
  State:        normal
  Status:       Ready
  Trust:        disabled
  Private VLAN: 223xxxx
  Public VLAN:  223xxxx
  Private IP:   10.xxx.xx.xxx
  Public IP:    169.xx.xxx.xxx
  Hardware:     shared
  Zone:         dal10
  Version:      1.8.11_1509
  ```
  {: screen}

### bx cs worker-reboot [-f][--hard] CLUSTER WORKER [WORKER][-s]
{: #cs_worker_reboot}

Rearrancar un nodo trabajador de un clúster. Durante el rearranque, el estado del nodo trabajador no cambia.

**Atención:** Rearrancar un nodo trabajador puede provocar que se corrompan los datos en el nodo trabajador. Utilice este mandato con precaución y cuando sepa que el rearranque puede ayudar a recuperar el nodo trabajador. En todos los demás casos, [recargue el nodo trabajador](#cs_worker_reload) en su lugar.

Antes de rearrancar el nodo trabajador, asegúrese de que los pods se vuelven a programar en otros nodos trabajadores para evitar el tiempo de inactividad de la app o la corrupción de los datos en el nodo trabajador.

1. Liste todos los nodos trabajadores del clúster y anote el **nombre** del nodo trabajador que desea rearrancar.
   ```
   kubectl get nodes
   ```
   El **nombre** que se devuelve en este mandato es la dirección IP privada que se asigna al nodo trabajador. Puede encontrar más información sobre el nodo trabajador ejecutando el mandato `bx cs workers <cluster_name_or_ID>` y buscando el nodo trabajador con la misma dirección **IP privada**.
2. Marque el nodo trabajador como no programable en un proceso que se conoce como acordonamiento. Cuando se acordona un nodo trabajador, deja de estar disponible para programar pods en el futuro. Utilice el **nombre** del nodo trabajador que ha recuperado en el paso anterior.
   ```
   kubectl cordon <worker_name>
   ```
   {: pre}

3. Verifique que la programación de pods está inhabilitada en el nodo trabajador.
   ```
   kubectl get nodes
   ```
   {: pre}
   La programación de pods está inhabilitada en el nodo trabajador si el estado es **SchedulingDisabled**.
 4. Fuerce la eliminación de los pods del nodo trabajador y vuelva a programarlos en los demás nodos trabajadores del clúster.
    ```
    kubectl drain <worker_name>
    ```
    {: pre}
    Este proceso puede tardar unos minutos.
 5. Rearranque el nodo trabajador. Utilice el ID de trabajador que se devuelve del mandato `bx cs workers <cluster_name_or_ID>`.
    ```
    bx cs worker-reboot <cluster_name_or_ID> <worker_name_or_ID>
    ```
    {: pre}
 6. Espere unos 5 minutos antes de volver a permitir la programación de pods en el nodo de trabajador para asegurarse de que finalice el rearranque. Durante el rearranque, el estado del nodo trabajador no cambia. Por lo general, el rearranque de un nodo trabajador se realiza en pocos segundos.
 7. Permita la programación de pods en el nodo de trabajador. Utilice el **nombre** del nodo trabajador que se devuelve del mandato `kubectl get nodes`.
    ```
    kubectl uncordon <worker_name>
    ```
    {: pre}
    </br>

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>-f</code></dt>
   <dd>Utilice esta opción para forzar el reinicio de un nodo trabajador sin solicitudes de usuario. Este valor es opcional.</dd>

   <dt><code>--hard</code></dt>
   <dd>Utilice esta opción para forzar un reinicio de un nodo trabajador, cortando el suministro eléctrico al nodo trabajador. Utilice esta opción si el nodo trabajador no responde o si tiene un bloqueo de Docker. Este valor es opcional.</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>El nombre o ID de uno o varios nodos trabajadores. Utilice un espacio para ver una lista de varios nodos trabajadores. Este valor es obligatorio.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
   </dl>

**Ejemplo**:

  ```
  bx cs worker-reboot my_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}


### bx cs worker-reload [-f] CLUSTER WORKER [WORKER][-s]
{: #cs_worker_reload}

Recargue todas las configuraciones necesarias para un nodo trabajador. Una recarga puede ser útil si el nodo trabajador tiene problemas, como un rendimiento lento, o si queda bloqueado en un estado incorrecto.

La recarga de un nodo trabajador aplica actualizaciones de versiones de parche a dicho nodo trabajador, pero no actualizaciones mayores o menores. Para ver los cambios entre versiones de parches, revise la documentación de [Registro de cambios de versiones](cs_versions_changelog.html#changelog).
{: tip}

Antes de recargar el nodo trabajador, asegúrese de que los pods se vuelven a programar en otros nodos trabajadores para evitar el tiempo de inactividad de la app o la corrupción de los datos en el nodo trabajador.

1. Liste todos los nodos trabajadores del clúster y anote el **nombre** del nodo trabajador que desea recargar.
   ```
   kubectl get nodes
   ```
   El **nombre** que se devuelve en este mandato es la dirección IP privada que se asigna al nodo trabajador. Puede encontrar más información sobre el nodo trabajador ejecutando el mandato `bx cs workers <cluster_name_or_ID>` y buscando el nodo trabajador con la misma dirección **IP privada**.
2. Marque el nodo trabajador como no programable en un proceso que se conoce como acordonamiento. Cuando se acordona un nodo trabajador, deja de estar disponible para programar pods en el futuro. Utilice el **nombre** del nodo trabajador que ha recuperado en el paso anterior.
   ```
   kubectl cordon <worker_name>
   ```
   {: pre}

3. Verifique que la programación de pods está inhabilitada en el nodo trabajador.
   ```
   kubectl get nodes
   ```
   {: pre}
   La programación de pods está inhabilitada en el nodo trabajador si el estado es **SchedulingDisabled**.
 4. Fuerce la eliminación de los pods del nodo trabajador y vuelva a programarlos en los demás nodos trabajadores del clúster.
    ```
    kubectl drain <worker_name>
    ```
    {: pre}
    Este proceso puede tardar unos minutos.
 5. Recargue el nodo trabajador. Utilice el ID de trabajador que se devuelve del mandato `bx cs workers <cluster_name_or_ID>`.
    ```
    bx cs worker-reload <cluster_name_or_ID> <worker_name_or_ID>
    ```
    {: pre}
 6. Espere a que finalice la recarga.
 7. Permita la programación de pods en el nodo de trabajador. Utilice el **nombre** del nodo trabajador que se devuelve del mandato `kubectl get nodes`.
    ```
    kubectl uncordon <worker_name>
    ```
</br>
<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>-f</code></dt>
   <dd>Utilice esta opción para forzar que se vuelva a cargar un nodo trabajador sin solicitudes de usuario. Este valor es opcional.</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>El nombre o ID de uno o varios nodos trabajadores. Utilice un espacio para ver una lista de varios nodos trabajadores. Este valor es obligatorio.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
   </dl>

**Ejemplo**:

  ```
  bx cs worker-reload my_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}


### bx cs worker-rm [-f] CLUSTER WORKER [WORKER][-s]
{: #cs_worker_rm}

Eliminar uno o varios nodos trabajadores de un clúster. Si elimina un nodo trabajador, el clúster se desequilibra. 

Antes de eliminar el nodo trabajador, asegúrese de que los pods se vuelven a programar en otros nodos trabajadores para evitar el tiempo de inactividad de la app o la corrupción de los datos en el nodo trabajador.
{: tip}

1. Liste todos los nodos trabajadores del clúster y anote el **nombre** del nodo trabajador que desea eliminar.
   ```
   kubectl get nodes
   ```
   El **nombre** que se devuelve en este mandato es la dirección IP privada que se asigna al nodo trabajador. Puede encontrar más información sobre el nodo trabajador ejecutando el mandato `bx cs workers <cluster_name_or_ID>` y buscando el nodo trabajador con la misma dirección **IP privada**.
2. Marque el nodo trabajador como no programable en un proceso que se conoce como acordonamiento. Cuando se acordona un nodo trabajador, deja de estar disponible para programar pods en el futuro. Utilice el **nombre** del nodo trabajador que ha recuperado en el paso anterior.
   ```
   kubectl cordon <worker_name>
   ```
   {: pre}

3. Verifique que la programación de pods está inhabilitada en el nodo trabajador.
   ```
   kubectl get nodes
   ```
   {: pre}
   La programación de pods está inhabilitada en el nodo trabajador si el estado es **SchedulingDisabled**.
4. Fuerce la eliminación de los pods del nodo trabajador y vuelva a programarlos en los demás nodos trabajadores del clúster.
   ```
   kubectl drain <worker_name>
   ```
   {: pre}
   Este proceso puede tardar unos minutos.
5. Elimine el nodo trabajador. Utilice el ID de trabajador que se devuelve del mandato `bx cs workers <cluster_name_or_ID>`.
   ```
   bx cs worker-rm <cluster_name_or_ID> <worker_name_or_ID>
   ```
   {: pre}

6. Verifique que se ha eliminado el nodo trabajador.
   ```
   bx cs workers <cluster_name_or_ID>
   ```
</br>
<strong>Opciones del mandato</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>El nombre o ID del clúster. Este valor es obligatorio.</dd>

   <dt><code>-f</code></dt>
   <dd>Utilice esta opción para forzar la eliminación de un nodo trabajador sin solicitudes de usuario. Este valor es opcional.</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>El nombre o ID de uno o varios nodos trabajadores. Utilice un espacio para ver una lista de varios nodos trabajadores. Este valor es obligatorio.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
   </dl>

**Ejemplo**:

  ```
  bx cs worker-rm my_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}


###bx cs worker-update [-f] CLUSTER WORKER [WORKER][--kube-version MAJOR.MINOR.PATCH] [--force-update][-s]
{: #cs_worker_update}

Actualiza los nodos trabajadores para aplicar las últimas actualizaciones y parches de seguridad para el sistema operativo, y para actualizar la versión de Kubernetes para que coincida con la versión del nodo maestro. Puede actualizar la versión de Kubernetes del nodo maestro con el [command](cs_cli_reference.html#cs_cluster_update) `bx cs cluster-update`.



**Importante**: La ejecución de `bx cs worker-update` puede causar un tiempo de inactividad para sus apps y servicios. Durante la actualización, todos los pods se vuelven a planificar en otros nodos trabajadores y los datos se suprimen si no se guardan fuera del pod. Para evitar el tiempo de inactividad, [asegúrese de tener suficientes nodos trabajadores para manejar la carga de trabajo mientras se estén actualizando los nodos trabajadores seleccionados](cs_cluster_update.html#worker_node).

Es posible que tenga que modificar los archivos YAML para futuros despliegues antes de la actualización. Revise esta [nota del release](cs_versions.html) para ver detalles.

<strong>Opciones del mandato</strong>:

   <dl>

   <dt><em>CLUSTER</em></dt>
   <dd>El nombre o ID del clúster en el que se listan los nodos trabajadores disponibles. Este valor es obligatorio.</dd>

   <dt><code>-f</code></dt>
   <dd>Utilice esta opción para forzar la actualización del maestro sin solicitudes de usuario. Este valor es opcional.</dd>

   <dt><code>--force-update</code></dt>
   <dd>Intente la actualización incluso si el cambio es superior a dos versiones anteriores. Este valor es opcional.</dd>

   <dt><code>--kube-version <em>MAJOR.MINOR.PATCH</em></code></dt>
     <dd>La versión de Kubernetes con la que desea que se actualicen sus nodos trabajadores. Se utiliza la versión predeterminada si no se especifica este valor.</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>El ID de uno o varios nodos trabajadores. Utilice un espacio para ver una lista de varios nodos trabajadores. Este valor es obligatorio.</dd>

   <dt><code>-s</code></dt>
   <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>

   </dl>

**Ejemplo**:

  ```
  bx cs worker-update my_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}


### bx cs workers CLUSTER [--show-deleted][--json] [-s]
{: #cs_workers}

Ver una lista de los nodos trabajadores y el estado de cada uno de ellos en un clúster.

<strong>Opciones del mandato</strong>:

   <dl>
   <dt><em>CLUSTER</em></dt>
   <dd>El nombre o ID del clúster para los nodos trabajadores disponibles. Este valor es obligatorio.</dd>

   <dt><em>--show-deleted</em></dt>
   <dd>Visualiza los nodos trabajadores que se han suprimido del clúster, incluido el motivo de la supresión. Este valor es opcional.</dd>

   <dt><code>--json</code></dt>
   <dd>Imprime la salida del mandato en formato JSON. Este valor es opcional.</dd>

  <dt><code>-s</code></dt>
  <dd>No mostrar el mensaje del día ni actualizar recordatorios. Este valor es opcional.</dd>
   </dl>

**Ejemplo**:

  ```
  bx cs workers my_cluster
  ```
  {: pre}
