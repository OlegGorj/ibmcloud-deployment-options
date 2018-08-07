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



# Registro de cambios de versiones
{: #changelog}

Visualice información de cambios de versiones de actualizaciones mayores, menores y parches de actualización que están disponibles para los clústeres {{site.data.keyword.containerlong}} Kubernetes. Los cambios incluyen actualizaciones a Kubernetes y a los componentes de Proveedores de {{site.data.keyword.Bluemix_notm}}. 
{:shortdesc}

IBM aplica actualizaciones a nivel de parche de forma automática al maestro, sin embargo, debe [actualizar los nodos trabajadores](cs_cluster_update.html#worker_node). Compruebe de forma mensual las actualizaciones disponibles. A medida que las actualizaciones pasan a estar disponibles, se le notifica al visualizar información sobre los nodos trabajadores, por ejemplo, con los mandatos `bx cs workers <cluster>` o `bx cs worker-get <cluster> <worker>`.

Consulte las [versiones de Kubernetes](cs_versions.html) para obtener un resumen de las acciones de migración.
{: tip}

Para obtener información sobre los cambios desde la versión anterior, consulte los siguientes registros de cambios.
-  [Registros de cambios](#110_changelog) de la versión 1.10.
-  [Registros de cambios](#19_changelog) de la versión 1.9.
-  [Registros de cambios](#18_changelog) de la versión 1.8.
-  **En desuso**: [Registro de cambios](#17_changelog) de la versión 1.7.

## Registro de cambios de la versión 1.10
{: #110_changelog}

Revise los siguientes cambios.

### Registro de cambios para el fix pack de nodo trabajador 1.10.1_1510, lanzado el 18 de mayo de 2018
{: #1101_1510}

<table summary="Cambios realizados desde la versión 1.10.1_1509">
<caption>Cambios desde la versión 1.10.1_1509</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/D</td>
<td>N/D</td>
<td>Corregido un error que se producía al utilizar el plugin de almacenamiento en bloques.</td>
</tr>
</tbody>
</table>

### Registro de cambios para el fix pack de nodo trabajador 1.10.1_1509, lanzado el 16 de mayo de 2018
{: #1101_1509}

<table summary="Cambios realizados desde la versión 1.10.1_1508">
<caption>Cambios desde la versión 1.10.1_1508</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/D</td>
<td>N/D</td>
<td>Los datos que se almacenan en el `kubelet` del directorio raíz se guardan ahora en el disco secundario más grande de la máquina del nodo trabajador.</td>
</tr>
</tbody>
</table>

### Registro de cambios para 1.10.1_1508, lanzado el 01 de mayo de 2018
{: #1101_1508}

<table summary="Cambios realizados desde la versión 1.9.7_1510">
<caption>Cambios desde la versión 1.9.7_1510</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v2.6.5</td>
<td>v3.1.1</td>
<td>Consulte las [notas del release de Calico ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.projectcalico.org/v3.1/releases/#v311).</td>
</tr>
<tr>
<td>Kubernetes Heapster</td>
<td>v1.5.0</td>
<td>v1.5.2</td>
<td>Consulte las [notas del release de Kubernetes Heapster ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/heapster/releases/tag/v1.5.2).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.9.7</td>
<td>v1.10.1</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.1).</td>
</tr>
<tr>
<td>Configuración de Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>Añadido <code>StorageObjectInUseProtection</code> a la opción <code>--enable-admission-plugins</code> para el servidor de API de Kubernetes del clúster.</td>
</tr>
<tr>
<td>DNS de Kubernetes</td>
<td>1.14.8</td>
<td>1.14.10</td>
<td>Consulte las [notas del release de DNS de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/dns/releases/tag/1.14.10).</td>
</tr>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.9.7-102</td>
<td>v1.10.1-52</td>
<td>Actualizado para dar soporte al release 1.10 de Kubernetes.</td>
</tr>
<tr>
<td>Soporte de GPU</td>
<td>N/D</td>
<td>N/D</td>
<td>Ahora hay disponible soporte para [cargas de trabajo de contenedor con unidad de proceso de gráficos (GPU)](cs_app.html#gpu_app) para su planificación y ejecución. Para obtener una lista de los tipos de máquina con GPU que hay disponibles, consulte [Hardware para nodos de trabajador](cs_clusters.html#shared_dedicated_node). Para obtener más información, consulte la documentación de Kubernetes de [planificación de GPU ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/).</td>
</tr>
</tbody>
</table>

## Registro de cambios de la versión 1.9
{: #19_changelog}

Revise los siguientes cambios.

### Registro de cambios para el fix pack de nodo trabajador 1.9.7_1512, lanzado el 18 de mayo de 2018
{: #197_1512}

<table summary="Cambios realizados desde la versión 1.9.7_1511">
<caption>Cambios desde la versión 1.9.7_1511</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/D</td>
<td>N/D</td>
<td>Corregido un error que se producía al utilizar el plugin de almacenamiento en bloques.</td>
</tr>
</tbody>
</table>

### Registro de cambios para el fix pack de nodo trabajador 1.9.7_1511, lanzado el 16 de mayo de 2018
{: #197_1511}

<table summary="Cambios realizados desde la versión 1.9.7_1510">
<caption>Cambios desde la versión 1.9.7_1510</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/D</td>
<td>N/D</td>
<td>Los datos que se almacenan en el `kubelet` del directorio raíz se guardan ahora en el disco secundario más grande de la máquina del nodo trabajador.</td>
</tr>
</tbody>
</table>

### Registro de cambios para 1.9.7_1510, lanzado el 30 de abril de 2018
{: #197_1510}

<table summary="Cambios realizados desde la versión 1.9.3_1506">
<caption>Cambios desde la versión 1.9.3_1506</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.9.3</td>
<td>v1.9.7	</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.7). Este release corrige las vulnerabilidades [CVE-2017-1002101 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) y [CVE-2017-1002102 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102).</td>
</tr>
<tr>
<td>Configuración de Kubernetes</td>
<td>N/D</td>
<td>N/D</td>
<td>Añadido `admissionregistration.k8s.io/v1alpha1=true` a la opción `--runtime-config` para el servidor de API de Kubernetes del clúster.</td>
</tr>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.9.3-71</td>
<td>v1.9.7-102</td>
<td>Ahora los servicios `NodePort` y `LoadBalancer` dan soporte a la [conservación de la dirección IP de origen del cliente](cs_loadbalancer.html#node_affinity_tolerations) estableciendo `service.spec.externalTrafficPolicy` en `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Corregida la configuración de tolerancia del [nodo extremo](cs_edge.html#edge) en clústeres antiguos.</td>
</tr>
</tbody>
</table>

## Registro de cambios de la versión 1.8
{: #18_changelog}

Revise los siguientes cambios.

### Registro de cambios para el fix pack de nodo trabajador 1.8.11_1511, lanzado el 18 de mayo de 2018
{: #1811_1511}

<table summary="Cambios realizados desde la versión 1.8.11_1510">
<caption>Cambios desde la versión 1.8.11_1510</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/D</td>
<td>N/D</td>
<td>Corregido un error que se producía al utilizar el plugin de almacenamiento en bloques.</td>
</tr>
</tbody>
</table>

### Registro de cambios para el fix pack de nodo trabajador 1.8.11_1510, lanzado el 16 de mayo de 2018
{: #1811_1510}

<table summary="Cambios realizados desde la versión 1.8.11_1509">
<caption>Cambios desde la versión 1.8.11_1509</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/D</td>
<td>N/D</td>
<td>Los datos que se almacenan en el `kubelet` del directorio raíz se guardan ahora en el disco secundario más grande de la máquina del nodo trabajador.</td>
</tr>
</tbody>
</table>


### Registro de cambios para 1.8.11_1509, lanzado el 19 de abril de 2018
{: #1811_1509}

<table summary="Cambios realizados desde la versión 1.8.8_1507">
<caption>Cambios desde la versión 1.8.8_1507</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.8.8</td>
<td>v1.8.11	</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.11). Este release corrige las vulnerabilidades [CVE-2017-1002101 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) y [CVE-2017-1002102 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102).</td>
</tr>
<tr>
<td>Imagen de contenedor en pausa</td>
<td>3.0</td>
<td>3.1</td>
<td>Elimina procesos zombie huérfanos heredados.</td>
</tr>
<tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.8.8-86</td>
<td>v1.8.11-126</td>
<td>Ahora los servicios `NodePort` y `LoadBalancer` dan soporte a la [conservación de la dirección IP de origen del cliente](cs_loadbalancer.html#node_affinity_tolerations) estableciendo `service.spec.externalTrafficPolicy` en `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Corregida la configuración de tolerancia del [nodo extremo](cs_edge.html#edge) en clústeres antiguos.</td>
</tr>
</tbody>
</table>

## Archivo
{: #changelog_archive}

### Registros de cambios de la versión 1.7 (en desuso)
{: #17_changelog}

Revise los siguientes cambios.

#### Registro de cambios para el fix pack de nodo trabajador 1.7.16_1513, lanzado el 18 de mayo de 2018
{: #1716_1513}

<table summary="Cambios realizados desde la versión 1.7.16_1512">
<caption>Cambios desde la versión 1.7.16_1512</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/D</td>
<td>N/D</td>
<td>Corregido un error que se producía al utilizar el plugin de almacenamiento en bloques.</td>
</tr>
</tbody>
</table>

#### Registro de cambios para el fix pack de nodo trabajador 1.7.16_1512, lanzado el 16 de mayo de 2018
{: #1716_1512}

<table summary="Cambios realizados desde la versión 1.7.16_1511">
<caption>Cambios desde la versión 1.7.16_1511</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/D</td>
<td>N/D</td>
<td>Los datos que se almacenan en el `kubelet` del directorio raíz se guardan ahora en el disco secundario más grande de la máquina del nodo trabajador.</td>
</tr>
</tbody>
</table>

#### Registro de cambios para 1.7.16_1511, lanzado el 19 de abril de 2018
{: #1716_1511}

<table summary="Cambios realizados desde la versión 1.7.4_1509">
<caption>Cambios desde la versión 1.7.4_1509</caption>
<thead>
<tr>
<th>Componente</th>
<th>Anterior</th>
<th>Actual</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.7.4</td>
<td>v1.7.16	</td>
<td>Consulte las [notas del release de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/kubernetes/kubernetes/releases/tag/v1.7.16). Este release corrige las vulnerabilidades [CVE-2017-1002101 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) y [CVE-2017-1002102 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102).</td>
</tr>
<td>Proveedor de {{site.data.keyword.Bluemix_notm}}</td>
<td>v1.7.4-133</td>
<td>v1.7.16-17</td>
<td>Ahora los servicios `NodePort` y `LoadBalancer` dan soporte a la [conservación de la dirección IP de origen del cliente](cs_loadbalancer.html#node_affinity_tolerations) estableciendo `service.spec.externalTrafficPolicy` en `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Corregida la configuración de tolerancia del [nodo extremo](cs_edge.html#edge) en clústeres antiguos.</td>
</tr>
</tbody>
</table>
