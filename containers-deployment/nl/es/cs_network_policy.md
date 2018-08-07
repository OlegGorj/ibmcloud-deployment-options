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



# Control del tráfico con políticas de red
{: #network_policies}

Cada clúster de Kubernetes está configurado con un plugin de red que se denomina Calico. Las políticas de red predeterminadas se configuran para proteger la interfaz de red pública de cada nodo trabajador en {{site.data.keyword.containerlong}}.
{: shortdesc}

Si tiene requisitos de seguridad exclusivos, puede utilizar Calico y Kubernetes para crear políticas de red para un clúster. Con las políticas de red de Kubernetes, puede especificar el tráfico de red que desea permitir o bloquear de y desde un pod en un clúster. Para establecer políticas de red más avanzadas como, por ejemplo, para el bloqueo de tráfico entrante (ingress) para los servicios de LoadBalancer, utilice políticas de red de Calico.

<ul>
  <li>
  [Políticas de red de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/services-networking/network-policies/): Estas políticas especifican la forma en la que los pods se pueden comunicar con otros pods y con los puntos finales externos. A partir de Kubernetes versión 1.8, se puede permitir tanto el tráfico de red entrante como saliente con base al protocolo, el puerto y las direcciones IP de origen y destino. El tráfico también se puede filtrar basándose en las etiquetas de espacio de nombres y pod. Las políticas de red de Kubernetes se aplican mediante mandatos `kubectl` o con API de Kubernetes. Cuando se aplican estas políticas, se convierten de forma automática en políticas de red de Calico. Calico impone estas políticas.
  </li>
  <li>
  Políticas de red Calico para clústeres Kubernetes versión [1.10 y posterior ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.projectcalico.org/v3.1/getting-started/kubernetes/tutorials/advanced-policy) o clústeres [1.9 y anterior ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.projectcalico.org/v2.6/getting-started/kubernetes/tutorials/advanced-policy): Estas políticas son un superconjunto de las políticas de red de Kubernetes y se aplican con mandatos `calicoctl`. Las políticas de Calico añaden las características siguientes.
    <ul>
    <li>Permitir o bloquear el tráfico de red en las interfaces de red especificadas, independientemente del CIDR o dirección IP de origen o destino del pod Kubernetes.</li>
    <li>Permitir o bloquear el tráfico de red para pods entre espacios de nombres.</li>
    <li>[Bloquear el tráfico de entrada (ingress) a los servicios LoadBalancer o NodePort Kubernetes](#block_ingress).</li>
    </ul>
  </li>
  </ul>

Calico impone estas políticas, incluidas las políticas de Kubernetes que se convierten automáticamente en políticas de Calico, configurando reglas iptables de Linux en los nodos trabajadores de Kubernetes. Las reglas iptables sirven como cortafuegos para el nodo trabajador para definir las características que debe cumplir el tráfico de red para que se reenvíe al recurso de destino.

<br />


## Políticas de red de Kubernetes y Calico predeterminadas
{: #default_policy}

Cuando se crea un clúster con una VLAN pública, se crea de forma automática un recurso HostEndpont con la etiqueta `ibm.role: worker_public` para cada nodo trabajador y su interfaz de red pública. Para proteger la interfaz de red pública de un nodo trabajador, las políticas predeterminadas de Calico se aplican a todos los puntos finales de host con la etiqueta `ibm.role: worker_public`.
{:shortdesc}

Las políticas predeterminadas de Calico permiten todo el tráfico de red saliente y permiten el tráfico entrante a componentes específicos del clúster como, por ejemplo, servicios Ingress, LoadBalancer y NodePort de Kubernetes. Se bloquea el resto del tráfico de red entrante desde internet a los nodos trabajadores que no se especifica en las políticas predeterminadas. Las políticas predeterminadas no afectan al trafico entre pods.

Revise las políticas de red predeterminadas siguientes de Calico que se aplican automáticamente a su clúster.

**Importante:** No elimine las políticas que se aplican a un punto final de host a menos que conozca en profundidad la política. Asegúrese de que no necesita tráfico que la política está permitiendo.

 <table summary="La primera fila de la tabla abarca ambas columnas. Lea el resto de las filas de izquierda a derecha, con la ubicación del servidor en la primera columna y las direcciones de IP coincidentes en la segunda columna. ">
 <caption>Políticas predeterminadas de Calico para cada clúster</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Políticas predeterminadas de Calico para cada clúster</th>
  </thead>
  <tbody>
    <tr>
      <td><code>allow-all-outbound</code></td>
      <td>Permite todo el tráfico de salida.</td>
    </tr>
    <tr>
      <td><code>allow-bixfix-port</code></td>
      <td>Permite el tráfico entrante en el puerto 52311 a la app BigFix para permitir las actualizaciones necesarias del nodo trabajador.</td>
    </tr>
    <tr>
      <td><code>allow-icmp</code></td>
      <td>Permite paquetes ICMP de entrada (pings).</td>
     </tr>
    <tr>
      <td><code>allow-node-port-dnat</code></td>
      <td>Permite el tráfico de entrada de los servicios Ingress, LoadBalancer y NodePort en los pods que exponen dichos servicios. <strong>Nota</strong>: No necesita especificar los puertos expuestos porque Kubernetes utiliza DNAT (Destination Network Address Translation) para reenviar las solicitudes de servicio a los pods correctos. El reenvío se realiza antes de que se apliquen las políticas de punto final de host en iptables.</td>
   </tr>
   <tr>
      <td><code>allow-sys-mgmt</code></td>
      <td>Permite conexiones entrantes para sistemas de infraestructura de IBM Cloud (SoftLayer) específicos que se utilizan para gestionar los nodos trabajadores.</td>
   </tr>
   <tr>
    <td><code>allow-vrrp</code></td>
    <td>Permite paquetes VRRP, que se utilizan para supervisar y mover direcciones IP virtuales entre los nodos trabajadores.</td>
   </tr>
  </tbody>
</table>

En clústeres Kubernetes versión 1.10 y posterior, también se crea una política predeterminada de Kubernetes que limita el acceso al Panel de control de Kubernetes. Las políticas de Kubernetes no se aplican al punto final de host sino que, por el contrario, al pod `kube-dashboard`. Esta política se aplica a clústeres conectados únicamente a VLAN privadas y a clústeres conectados a una VLAN privada y pública.

<table>
<caption>Políticas predeterminadas de Kubernetes para cada clúster</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Políticas predeterminadas de Kubernetes para cada clúster</th>
</thead>
<tbody>
 <tr>
  <td><code>kubernetes-dashboard</code></td>
  <td><b>Solo en Kubernetes v1.10</b>, proporcionado en el espacio de nombres <code>kube-system</code>: Bloquea el acceso de todos los pods al Panel de control de Kubernetes. Esta política no afecta el acceso al panel de control desde la interfaz de usuario de {{site.data.keyword.Bluemix_notm}} o utilizando <code>kubectl proxy</code>. Si un pod necesita acceder al panel de control, despliegue el pod en un espacio de nombres que tenga la etiqueta <code>kubernetes-dashboard-policy: allow</code>.</td>
 </tr>
</tbody>
</table>

<br />


## Instalación y configuración de la CLI de Calico
{: #cli_install}

Instale y configure la CLI de Calico para ver, gestionar y añadir políticas de Calico.
{:shortdesc}

La compatibilidad de las versiones de Calico para políticas y la configuración de CLI varía en función de la versión de Kubernetes del clúster. Para instalar y configurar la CLI de Calico, pulse uno de los enlaces siguientes basándose en la versión del clúster:

* [Clústeres Kubernetes versión 1.10 o posterior](#1.10_install)
* [Clústeres Kubernetes versión 1.9 o anterior](#1.9_install)

Antes de actualizar su clúster desde Kubernetes versión 1.9 o anterior a la versión 1.10 o posterior, consulte [Preparación para actualizar a Calico V3](cs_versions.html#110_calicov3).
{: tip}

### Instalación y configuración de la versión 3.1.1 de la CLI de Calico para clústeres que ejecutan Kubernetes versión 1.10 o posterior
{: #1.10_install}

Antes de empezar, [defina el clúster como destino de la CLI de Kubernetes](cs_cli_install.html#cs_cli_configure). Incluya la opción `--admin` con el mandato `bx cs cluster-config`, que se utiliza para descargar los certificados y los archivos de permiso. Esta descarga también incluye las claves para el rol de superusuario, que necesita para ejecutar mandatos Calico.

  ```
  bx cs cluster-config <cluster_name> --admin
  ```
  {: pre}

Para instalar y configurar la CLI de Calico 3.1.1:

1. [Descargue la CLI de Calico ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/projectcalico/calicoctl/releases/tag/v3.1.1).

    Si está utilizando OSX, descargue la versión `-darwin-amd64`. Si utiliza Windows, instale la CLI de Calico en el mismo directorio que la CLI de {{site.data.keyword.Bluemix_notm}}. Esta configuración le ahorra algunos cambios en filepath cuando ejecute mandatos posteriormente.
    {: tip}

2. Para usuarios de OSX y Linux, siga los pasos siguientes:
    1. Mueva el archivo ejecutable al directorio _/usr/local/bin_.
        - Linux:

          ```
          mv filepath/calicoctl /usr/local/bin/calicoctl
          ```
          {: pre}

        - OS
X:

          ```
          mv filepath/calicoctl-darwin-amd64 /usr/local/bin/calicoctl
          ```
          {: pre}

    2. Convierta en ejecutable el archivo.

        ```
        chmod +x /usr/local/bin/calicoctl
        ```
        {: pre}

3. Compruebe que los mandatos `calico` se han ejecutado correctamente comprobando la versión del cliente de la CLI de Calico.

    ```
    calicoctl version
    ```
    {: pre}

4. Si las políticas de red corporativas utilizan cortafuegos o proxies para impedir el acceso desde su sistema local a puntos finales públicos, [permita el acceso TCP a mandatos de Calico](cs_firewall.html#firewall).

5. Para Linux y OS X, cree el directorio `/etc/calico`. Para Windows, se puede utilizar cualquier directorio.

  ```
  sudo mkdir -p /etc/calico/
  ```
  {: pre}

6. Cree un archivo `calicoctl.cfg`.
    - Linux y OS X:

      ```
      sudo vi /etc/calico/calicoctl.cfg
      ```
      {: pre}

    - Windows: Cree el archivo con un editor de texto.

7. Escriba la siguiente información en el archivo <code>calicoctl.cfg</code>.

    ```
    apiVersion: projectcalico.org/v3
    kind: CalicoAPIConfig
    metadata:
    spec:
        datastoreType: etcdv3
        etcdEndpoints: <ETCD_URL>
        etcdKeyFile: <CERTS_DIR>/admin-key.pem
        etcdCertFile: <CERTS_DIR>/admin.pem
        etcdCACertFile: <CERTS_DIR>/<ca-*pem_file>
    ```
    {: codeblock}

    1. Recupere `<ETCD_URL>`.

      - Linux y OS X:

          ```
          kubectl get cm -n kube-system calico-config -o yaml | grep "etcd_endpoints:" | awk '{ print $2 }'
          ```
          {: pre}

          Salida de ejemplo:

          ```
          https://169.xx.xxx.xxx:30001
          ```
          {: screen}

      - Windows:
        <ol>
        <li>Obtenga los valores de configuración de la correlación de configuración. </br><pre class="codeblock"><code>kubectl get cm -n kube-system calico-config -o yaml</code></pre></br>
        <li>En la sección `data`, localice el valor etcd_endpoints. Ejemplo: <code>https://169.xx.xxx.xxx:30001</code>
        </ol>

    2. Recupere `<CERTS_DIR>`, el directorio en el que se han descargado los certificados de Kubernetes.

        - Linux y OS X:

          ```
          dirname $KUBECONFIG
          ```
          {: pre}

          Salida de ejemplo:

          ```
          /home/sysadmin/.bluemix/plugins/container-service/clusters/<cluster_name>-admin/
          ```
          {: screen}

        - Windows:

          ```
          ECHO %KUBECONFIG%
          ```
          {: pre}

            Ejemplo de salida:

          ```
          C:/Users/<user>/.bluemix/plugins/container-service/mycluster-admin/kube-config-prod-dal10-mycluster.yml
          ```
          {: screen}

        **Nota**: Para obtener la vía de acceso del directorio, elimine el nombre de archivo `kube-config-prod-<location>-<cluster_name>.yml` del final de la salida.

    3. Recupere el valor `ca-*pem_file`.

        - Linux y OS X:

          ```
          ls `dirname $KUBECONFIG` | grep "ca-"
          ```
          {: pre}

        - Windows:
          <ol><li>Abra el directorio que ha recuperado en el último paso.</br><pre class="codeblock"><code>C:\Users\<user>\.bluemix\plugins\container-service\&lt;cluster_name&gt;-admin\</code></pre>
          <li> Localice el archivo <code>ca-*pem_file</code>.</ol>

    4. Compruebe que la configuración de Calico funciona correctamente.

        - Linux y OS X:

          ```
          calicoctl get nodes
          ```
          {: pre}

        - Windows:

          ```
          calicoctl get nodes --config=filepath/calicoctl.cfg
          ```
          {: pre}

          Salida:

          ```
          NAME
              kube-dal10-crc21191ee3997497ca90c8173bbdaf560-w1.cloud.ibm
              kube-dal10-crc21191ee3997497ca90c8173bbdaf560-w2.cloud.ibm
              kube-dal10-crc21191ee3997497ca90c8173bbdaf560-w3.cloud.ibm
          ```
          {: screen}


### Instalación y configuración de la versión 1.6.3 de la CLI de Calico para clústeres ejecutando Kubernetes versión 1.9 o anterior
{: #1.9_install}

Antes de empezar, [defina el clúster como destino de la CLI de Kubernetes](cs_cli_install.html#cs_cli_configure). Incluya la opción `--admin` con el mandato `bx cs cluster-config`, que se utiliza para descargar los certificados y los archivos de permiso. Esta descarga también incluye las claves para el rol de superusuario, que necesita para ejecutar mandatos Calico.

  ```
  bx cs cluster-config <cluster_name> --admin
  ```
  {: pre}

Para instalar y configurar la CLI de Calico 1.6.3:

1. [Descargue la CLI de Calico ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/projectcalico/calicoctl/releases/tag/v1.6.3).

    Si está utilizando OSX, descargue la versión `-darwin-amd64`. Si utiliza Windows, instale la CLI de Calico en el mismo directorio que la CLI de {{site.data.keyword.Bluemix_notm}}. Esta configuración le ahorra algunos cambios en filepath cuando ejecute mandatos posteriormente.
    {: tip}

2. Para usuarios de OSX y Linux, siga los pasos siguientes:
    1. Mueva el archivo ejecutable al directorio _/usr/local/bin_.
        - Linux:

          ```
          mv filepath/calicoctl /usr/local/bin/calicoctl
          ```
          {: pre}

        - OS
X:

          ```
          mv filepath/calicoctl-darwin-amd64 /usr/local/bin/calicoctl
          ```
          {: pre}

    2. Convierta en ejecutable el archivo.

        ```
        chmod +x /usr/local/bin/calicoctl
        ```
        {: pre}

3. Compruebe que los mandatos `calico` se han ejecutado correctamente comprobando la versión del cliente de la CLI de Calico.

    ```
    calicoctl version
    ```
    {: pre}

4. Si las políticas de red corporativas utilizan cortafuegos o proxies para impedir el acceso desde su sistema local a puntos finales públicos: Consulte [Ejecución de mandatos `calicoctl` desde detrás de un cortafuegos](cs_firewall.html#firewall) para obtener instrucciones sobre cómo permitir el acceso TCP a los mandatos de Calico.

5. Para Linux y OS X, cree el directorio `/etc/calico`. Para Windows, se puede utilizar cualquier directorio.
    ```
    sudo mkdir -p /etc/calico/
    ```
    {: pre}

6. Cree un archivo `calicoctl.cfg`.
    - Linux y OS X:

      ```
      sudo vi /etc/calico/calicoctl.cfg
      ```
      {: pre}

    - Windows: Cree el archivo con un editor de texto.

7. Escriba la siguiente información en el archivo <code>calicoctl.cfg</code>.

    ```
    apiVersion: v1
      kind: calicoApiConfig
      metadata:
      spec:
          etcdEndpoints: <ETCD_URL>
          etcdKeyFile: <CERTS_DIR>/admin-key.pem
          etcdCertFile: <CERTS_DIR>/admin.pem
          etcdCACertFile: <CERTS_DIR>/<ca-*pem_file>
    ```
    {: codeblock}

    1. Recupere `<ETCD_URL>`.

      - Linux y OS X:

          ```
          kubectl get cm -n kube-system calico-config -o yaml | grep "etcd_endpoints:" | awk '{ print $2 }'
          ```
          {: pre}

      - Ejemplo de salida:

          ```
          https://169.xx.xxx.xxx:30001
          ```
          {: screen}

      - Windows:
        <ol>
        <li>Obtenga los valores de configuración de la correlación de configuración. </br><pre class="codeblock"><code>kubectl get cm -n kube-system calico-config -o yaml</code></pre></br>
        <li>En la sección `data`, localice el valor etcd_endpoints. Ejemplo: <code>https://169.xx.xxx.xxx:30001</code>
        </ol>

    2. Recupere `<CERTS_DIR>`, el directorio en el que se han descargado los certificados de Kubernetes.

        - Linux y OS X:

          ```
          dirname $KUBECONFIG
          ```
          {: pre}

          Salida de ejemplo:

          ```
          /home/sysadmin/.bluemix/plugins/container-service/clusters/<cluster_name>-admin/
          ```
          {: screen}

        - Windows:

          ```
          ECHO %KUBECONFIG%
          ```
          {: pre}

          Salida de ejemplo:

          ```
          C:/Users/<user>/.bluemix/plugins/container-service/mycluster-admin/kube-config-prod-dal10-mycluster.yml
          ```
          {: screen}

        **Nota**: Para obtener la vía de acceso del directorio, elimine el nombre de archivo `kube-config-prod-<location>-<cluster_name>.yml` del final de la salida.

    3. Recupere el valor `ca-*pem_file`.

        - Linux y OS X:

          ```
          ls `dirname $KUBECONFIG` | grep "ca-"
          ```
          {: pre}

        - Windows:
          <ol><li>Abra el directorio que ha recuperado en el último paso.</br><pre class="codeblock"><code>C:\Users\<user>\.bluemix\plugins\container-service\&lt;cluster_name&gt;-admin\</code></pre>
          <li> Localice el archivo <code>ca-*pem_file</code>.</ol>

    4. Compruebe que la configuración de Calico funciona correctamente.

        - Linux y OS X:

          ```
          calicoctl get nodes
          ```
          {: pre}

        - Windows:

          ```
          calicoctl get nodes --config=filepath/calicoctl.cfg
          ```
          {: pre}

          Salida:

          ```
          NAME
              kube-dal10-crc21191ee3997497ca90c8173bbdaf560-w1.cloud.ibm
              kube-dal10-crc21191ee3997497ca90c8173bbdaf560-w2.cloud.ibm
              kube-dal10-crc21191ee3997497ca90c8173bbdaf560-w3.cloud.ibm
          ```
          {: screen}

<br />


## Visualización de políticas de red
{: #view_policies}

Consulte los detalles de políticas de red añadidas y predeterminadas que se aplican a su clúster.
{:shortdesc}

Antes de empezar:
1. [Instale y configure la CLI de Calico. ](#cli_install)
2. [Defina el clúster como destino de la CLI de Kubernetes](cs_cli_install.html#cs_cli_configure). Incluya la opción `--admin` con el mandato `bx cs cluster-config`, que se utiliza para descargar los certificados y los archivos de permiso. Esta descarga también incluye las claves para el rol de superusuario, que necesita para ejecutar mandatos Calico.
    ```
    bx cs cluster-config <cluster_name> --admin
    ```
    {: pre}

La compatibilidad de las versiones de Calico para políticas y la configuración de CLI varía en función de la versión de Kubernetes del clúster. Para instalar y configurar la CLI de Calico, pulse uno de los enlaces siguientes basándose en la versión del clúster:

* [Clústeres Kubernetes versión 1.10 o posterior](#1.10_examine_policies)
* [Clústeres Kubernetes versión 1.9 o anterior](#1.9_examine_policies)

Antes de actualizar su clúster desde Kubernetes versión 1.9 o anterior a la versión 1.10 o posterior, consulte [Preparación para actualizar a Calico V3](cs_versions.html#110_calicov3).
{: tip}

### Visualice las políticas de red que se ejecutan en Kubernetes versión 1.10 o posterior
{: #1.10_examine_policies}

1. Consultar el punto final del host de Calico.

    ```
    calicoctl get hostendpoint -o yaml
    ```
    {: pre}

2. Consultar todas las políticas de red de Calico y de Kubernetes que se han creado para el clúster. Esta lista incluye políticas que quizás no se apliquen todavía a ningún pod o host. Para que una política de red se pueda imponer, se debe encontrar un recurso de Kubernetes que coincida con el selector definido en la política de red de Calico.

    El objeto del ámbito de las [Políticas de red ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/networkpolicy) son espacios de nombres específicos:
    ```
    calicoctl get NetworkPolicy --all-namespaces -o wide
    ```

    El objeto del ámbito de las [Políticas de red globales ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/globalnetworkpolicy) no son espacios de nombres específicos:
    ```
    calicoctl get GlobalNetworkPolicy -o wide
    ```
    {: pre}

3. Consultar los detalles correspondientes a una política de red.

    ```
    calicoctl get NetworkPolicy -o yaml <policy_name> --namespace <policy_namespace>
    ```
    {: pre}

4. Visualizar los detalles de todas las políticas de red globales del clúster.

    ```
    calicoctl get GlobalNetworkPolicy -o yaml
    ```
    {: pre}

### Visualización de políticas de red en clústeres que ejecutan Kubernetes versión 1.9 o anterior
{: #1.9_examine_policies}

1. Consultar el punto final del host de Calico.

    ```
    calicoctl get hostendpoint -o yaml
    ```
    {: pre}

2. Consultar todas las políticas de red de Calico y de Kubernetes que se han creado para el clúster. Esta lista incluye políticas que quizás no se apliquen todavía a ningún pod o host. Para que una política de red se pueda imponer, se debe encontrar un recurso de Kubernetes que coincida con el selector definido en la política de red de Calico.

    ```
    calicoctl get policy -o wide
    ```
    {: pre}

3. Consultar los detalles correspondientes a una política de red.

    ```
    calicoctl get policy -o yaml <policy_name>
    ```
    {: pre}

4. Consultar los detalles de todas las políticas de red para el clúster.

    ```
    calicoctl get policy -o yaml
    ```
    {: pre}

<br />


## Adición de políticas de red
{: #adding_network_policies}

En la mayoría de los casos, no es necesario modificar las políticas predeterminadas. Sólo en escenarios avanzados se pueden requerir cambios. Si debe realizar cambios, cree sus propias políticas de red.
{:shortdesc}

Para crear políticas de red de Kubernetes, consulte la [Documentación de políticas de red de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/services-networking/network-policies/).

Siga estos pasos para crear políticas de Calico.

Antes de empezar:
1. [Instale y configure la CLI de Calico. ](#cli_install)
2. [Defina el clúster como destino de la CLI de Kubernetes](cs_cli_install.html#cs_cli_configure). Incluya la opción `--admin` con el mandato `bx cs cluster-config`, que se utiliza para descargar los certificados y los archivos de permiso. Esta descarga también incluye las claves para el rol de superusuario, que necesita para ejecutar mandatos Calico.
    ```
    bx cs cluster-config <cluster_name> --admin
    ```
    {: pre}

La compatibilidad de las versiones de Calico para políticas y la configuración de CLI varía en función de la versión de Kubernetes del clúster. Pulse uno de los enlaces siguientes basándose en la versión del clúster:

* [Clústeres Kubernetes versión 1.10 o posterior](#1.10_create_new)
* [Clústeres Kubernetes versión 1.9 o anterior](#1.9_create_new)

Antes de actualizar su clúster desde Kubernetes versión 1.9 o anterior a la versión 1.10 o posterior, consulte [Preparación para actualizar a Calico V3](cs_versions.html#110_calicov3).
{: tip}

### Adición de políticas de Calico en clústeres que ejecutan Kubernetes versión 1.10 o posterior
{: #1.10_create_new}

1. Defina su [política de red ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/networkpolicy) o [política de red global ![Icono de enlace externo ](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/globalnetworkpolicy) de Calico creando un script de configuración (`.yaml`). Estos archivos de configuración incluyen los selectores que describen los pods, espacios de nombres o hosts a los que se aplican estas políticas. Consulte estas [políticas de Calico de ejemplo ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://docs.projectcalico.org/v3.1/getting-started/kubernetes/tutorials/advanced-policy) como ayuda para crear la suya propia.
    **Nota**: Los clústeres Kubernetes versión 1.10 o posterior deben utilizar una sintaxis de política Calico v3.

2. Aplique las políticas al clúster.
    - Linux y OS X:

      ```
      calicoctl apply -f policy.yaml
      ```
      {: pre}

    - Windows:

      ```
      calicoctl apply -f filepath/policy.yaml --config=filepath/calicoctl.cfg
      ```
      {: pre}

### Adición de políticas de Calico en clústeres que ejecutan Kubernetes versión 1.9 o anterior
{: #1.9_create_new}

1. Defina su [política de red de Calico ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://docs.projectcalico.org/v2.6/reference/calicoctl/resources/policy) creando un script de configuración (`.yaml`). Estos archivos de configuración incluyen los selectores que describen los pods, espacios de nombres o hosts a los que se aplican estas políticas. Consulte estas [políticas de Calico de ejemplo ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://docs.projectcalico.org/v2.6/getting-started/kubernetes/tutorials/advanced-policy) como ayuda para crear la suya propia.
    **Nota**: Los clústeres Kubernetes versión 1.9 o anterior deben utilizar una sintaxis de política Calico v2.


2. Aplique las políticas al clúster.
    - Linux y OS X:

      ```
      calicoctl apply -f policy.yaml
      ```
      {: pre}

    - Windows:

      ```
      calicoctl apply -f filepath/policy.yaml --config=filepath/calicoctl.cfg
      ```
      {: pre}

<br />


## Bloqueo del tráfico entrante a los servicios LoadBalancer o NodePort
{: #block_ingress}

[De forma predeterminada](#default_policy), los servicios NodePort y LoadBalancer de Kubernetes están diseñados para hacer que las apps estén disponibles en las interfaces de clúster privadas y pública. Sin embargo, puede bloquear el tráfico de entrada a los servicios en función del origen o el destino del tráfico.
{:shortdesc}

Un servicio LoadBalancer de Kubernetes es también un servicio NodePort. Un servicio LoadBalancer hace que la app está disponible en la dirección IP y puerto del equilibrador de carga y hace que la app está disponible en los NodePorts del servicio. Se puede acceder a los NodePorts en cada dirección IP (pública y privada) para cada nodo del clúster.

El administrador del clúster puede utilizar el bloque de políticas de red `preDNAT` de Calico:

  - Tráfico destinado a servicios NodePort. Se permite el tráfico destinado a los servicios LoadBalancer.
  - Tráfico que se basa en una dirección de origen o CIDR.

Algunos usos comunes de las políticas de red `preDNAT` de Calico:

  - Bloquear el tráfico a NodePorts públicos de un servicio LoadBalancer privado.
  - Bloquear el tráfico a NodePorts públicos en clústeres que ejecuten [nodos trabajadores de extremo](cs_edge.html#edge). El bloqueo de NodePorts garantiza que los nodos trabajadores de extremo sean los únicos nodos trabajadores que manejen el tráfico entrante.

Las políticas predeterminadas de Kubernetes y Calico son difíciles de aplicar para proteger los servicios NodePort y LoadBalancer de Kubernetes debido a las reglas iptables de DNAT que se generan para estos servicios.

Las políticas de red `preDNAT` de Calico pueden ayudar porque generan reglas de iptables con base a un recurso de política de red de Calico. Los clústeres Kubernetes versión 1.10 o posterior utilizan [políticas de red con sintaxis `calicoctl.cfg` v3 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/networkpolicy). Los clústeres Kubernetes versión 1.9 o anterior utilizan [políticas con sintaxis `calicoctl.cfg` v2 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.projectcalico.org/v2.6/reference/calicoctl/resources/policy).

1. Defina una política de red `preDNAT` de Calico para el acceso de ingress (tráfico entrante) a los servicios de Kubernetes.

    * Los clústeres Kubernetes versión 1.10 o posterior deben utilizar una sintaxis de política Calico v3.

        Ejemplo de recurso que bloquea todos los NodePorts:

        ```
        apiVersion: projectcalico.org/v3
        kind: GlobalNetworkPolicy
        metadata:
          name: deny-kube-node-port-services
        spec:
          applyOnForward: true
          ingress:
          - action: Deny
            destination:
              ports:
              - 30000:32767
            protocol: TCP
            source: {}
          - action: Deny
            destination:
              ports:
              - 30000:32767
            protocol: UDP
            source: {}
          preDNAT: true
          selector: ibm.role in { 'worker_public', 'master_public' }
          types:
          - Ingress
        ```
        {: codeblock}

    * Los clústeres Kubernetes versión 1.9 o anterior deben utilizar una sintaxis de política Calico v2.

        Ejemplo de recurso que bloquea todos los NodePorts:

        ```
        apiVersion: v1
        kind: policy
        metadata:
          name: deny-kube-node-port-services
        spec:
          preDNAT: true
          selector: ibm.role in { 'worker_public', 'master_public' }
          ingress:
          - action: deny
            protocol: tcp
            destination:
              ports:
              - 30000:32767
          - action: deny
            protocol: udp
            destination:
              ports:
              - 30000:32767
        ```
        {: codeblock}

2. Aplique la política de red preDNAT de Calico. La aplicación de los cambios en la política al clúster tarda alrededor de 1 minuto.

  ```
  calicoctl apply -f deny-kube-node-port-services.yaml
  ```
  {: pre}
