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




# Temas populares sobre {{site.data.keyword.containershort_notm}}
{: #cs_popular_topics}

Manténgase al día con lo que está pasando con {{site.data.keyword.containerlong}}. Explore las nuevas características, obtenga consejos o conozca temas populares que otros desarrolladores ahora mismo han encontrado útiles.
{:shortdesc}

## Temas populares en mayo de 2018
{: #may18}


<table summary="La tabla muestra los temas populares. Las filas se leen de izquierda a derecha, con la fecha en la columna uno, el título de la característica en la columna dos y una descripción en la columna tres.">
<caption>Temas populares para contenedores y clústeres de Kubernetes en mayo de 2018</caption>
<thead>
<th>Fecha</th>
<th>Título</th>
<th>Descripción</th>
</thead>
<tbody>
<tr>
<td>24 de mayo</td>
<td>[Nuevo formato subdominio de Ingress](cs_ingress.html)</td>
<td>Los clústeres creados después del 24 de mayo son asignados a un subdominio de Ingress en el nuevo formato: <code>&lt;cluster_name&gt;.&lt;region&gt;.containers.appdomain.cloud</code>. Cuando se utiliza Ingress para exponer sus apps, puede utilizar el nuevo subdominio para acceder a las apps desde Internet.</td>
</tr>
<tr>
<td>14 de mayo</td>
<td>[Actualización: Despliegue en todo el mundo de cargas de trabajo en máquinas nativas con GPU](cs_app.html#gpu_app)</td>
<td>Si tiene un [tipo de máquina con GPU (Graphics Processing Unit) nativa](cs_clusters.html#shared_dedicated_node) en el clúster, puede planificar apps matemáticas intensivas. El nodo trabajador con GPU puede procesar la carga de trabajo de la app tanto con la CPU como con la GPU para incrementar el rendimiento.</td>
</tr>
<tr>
<td>3 de mayo</td>
<td>[Container Image Security Enforcement (beta)](/docs/services/Registry/registry_security_enforce.html#security_enforce)</td>
<td>¿Su equipo necesita un poco más ayuda para saber qué imagen tiene que extraer en sus contenedores de app? Pruebe Container Image Security Enforcement beta para verificar las imágenes de contenedor antes de desplegarlas. Disponible para clústeres que ejecutan Kubernetes 1.9 o posterior.</td>
</tr>
<tr>
<td>01 de mayo</td>
<td>[Despliegue del panel de control de Kubernetes desde la GUI](cs_app.html#cli_dashboard)</td>
<td>¿Alguna vez ha deseado acceder al panel de control de Kubernetes con una sola pulsación? Pruebe el botón **Panel de control de Kubernetes** en la interfaz gráfica de usuario de {{site.data.keyword.Bluemix_notm}}.</td>
</tr>
</tbody></table>




## Temas populares en abril de 2018
{: #apr18}

<table summary="La tabla muestra los temas populares. Las filas se leen de izquierda a derecha, con la fecha en la columna uno, el título de la característica en la columna dos y una descripción en la columna tres.">
<caption>Temas populares para contenedores y clústeres de Kubernetes en abril de 2018</caption>
<thead>
<th>Fecha</th>
<th>Título</th>
<th>Descripción</th>
</thead>
<tbody>
<tr>
<td>17 de abril</td>
<td>{{site.data.keyword.Bluemix_notm}} Block Storage</td>
<td>Instale el [plugin](cs_storage.html#install_block) {{site.data.keyword.Bluemix_notm}} Block Storage para guardar datos persistentes en el almacenamiento en bloque. A continuación, puede [crear nuevo almacenamiento en bloque](cs_storage.html#create) o [utilizar almacenamiento en bloque existente](cs_storage.html#existing_block) para su clúster.</td>
</tr>
<tr>
<td>13 de abril</td>
<td>[Nueva guía de aprendizaje para migrar apps de Cloud Foundry a clústeres ](cs_tutorials_cf.html#cf_tutorial)</td>
<td>¿Tiene una app de Cloud Foundry? Aprenda a desplegar el mismo código de la app en un contenedor que se ejecute en un clúster Kubernetes.</td>
</tr>
<tr>
<td>5 de abril</td>
<td>[Filtrado de registros](cs_health.html#filter-logs)</td>
<td>Filtre registros específicos al reenviarlos. Los registros se pueden filtrar por una serie de mensaje, un nivel de registro, un nombre de contenedor o un espacio de nombres específico.</td>
</tr>
</tbody></table>

## Temas populares en marzo de 2018
{: #mar18}

<table summary="La tabla muestra los temas populares. Las filas se leen de izquierda a derecha, con la fecha en la columna uno, el título de la característica en la columna dos y una descripción en la columna tres.">
<caption>Temas populares para contenedores y clústeres de Kubernetes en marzo de 2018</caption>
<thead>
<th>Fecha</th>
<th>Título</th>
<th>Descripción</th>
</thead>
<tbody>
<tr>
<td>16 de marzo</td>
<td>[Suministre un clúster nativo con Trusted Compute](cs_clusters.html#shared_dedicated_node)</td>
<td>Cree un clúster nativo que ejecute la [versión 1.9 de Kubernetes](cs_versions.html#cs_v19) o posterior y habilitar Trusted Compute para verificar que los nodos trabajadores no se manipulan de forma indebida.</td>
</tr>
<tr>
<td>14 de marzo</td>
<td>[Inicio de sesión seguro con {{site.data.keyword.appid_full}}](cs_integrations.html#appid)</td>
<td>Mejore las apps que se ejecutan en {{site.data.keyword.containershort_notm}} requiriendo a los usuarios que inicien sesión.</td>
</tr>
<tr>
<td>13 de marzo</td>
<td>[Ubicación disponible en São Paulo](cs_regions.html)</td>
<td>Damos la bienvenida a São Paulo, Brasil, como nueva ubicación en la región EE.UU. sur. Si tiene un cortafuegos, [abra los puertos del cortafuegos](cs_firewall.html#firewall) para esta ubicación y los otros dentro de la región en la que se encuentra el clúster.</td>
</tr>
<tr>
<td>12 de marzo</td>
<td>[¿Se acaba de unir a {{site.data.keyword.Bluemix_notm}} con una cuenta de prueba? Pruebe un clúster de Kubernetes gratuito.](container_index.html#clusters)</td>
<td>Con una [cuenta de {{site.data.keyword.Bluemix_notm}} de prueba](https://console.bluemix.net/registration/), puede desplegar un clúster gratuito durante 21 días para probar las funciones de Kubernetes.</td>
</tr>
</tbody></table>

## Temas populares en febrero de 2018
{: #feb18}

<table summary="La tabla muestra los temas populares. Las filas se leen de izquierda a derecha, con la fecha en la columna uno, el título de la característica en la columna dos y una descripción en la columna tres.">
<caption>Temas populares para contenedores y clústeres de Kubernetes en febrero de 2018</caption>
<thead>
<th>Fecha</th>
<th>Título</th>
<th>Descripción</th>
</thead>
<tbody>
<tr>
<td>27 de febrero</td>
<td>Imágenes de máquina virtual de hardware (HVM) para nodos trabajadores</td>
<td>Aumente el rendimiento de E/S de las cargas de trabajo con imágenes de HVM. Active la función en cada nodo trabajador existente mediante el [mandato](cs_cli_reference.html#cs_worker_reload) `bx cs worker-reload` o el [mandato](cs_cli_reference.html#cs_worker_update) `bx cs worker-update`.</td>
</tr>
<tr>
<td>26 de febrero</td>
<td>[Escalado automático de KubeDNS](https://kubernetes.io/docs/tasks/administer-cluster/dns-horizontal-autoscaling/)</td>
<td>KubeDNS ahora se amplía con el clúster a medida que crece. Puede ajustar los índices de escalado mediante el mandato siguiente: `kubectl -n kube-system edit cm kube-dns-autoscaler`.</td>
</tr>
<tr>
<td>23 de febrero</td>
<td>Vea la interfaz de usuario web para los [registros](cs_health.html#view_logs) y las [métricas](cs_health.html#view_metrics)</td>
<td>Vea fácilmente los datos de registros y métricas del clúster y sus componentes con una mejor interfaz de usuario web. Consulte la página de detalles de clúster para el acceso.</td>
</tr>
<tr>
<td>20 de febrero</td>
<td>Imágenes cifradas y [contenido firmado y de confianza](../services/Registry/registry_trusted_content.html#registry_trustedcontent)</td>
<td>En {{site.data.keyword.registryshort_notm}}, puede firmar y cifrar imágenes para garantizar su integridad al almacenar en el espacio de nombres del registro. Ejecute las instancias de contenedor utilizando únicamente contenido fiable.</td>
</tr>
<tr>
<td>19 de febrero</td>
<td>[Configure la VPN IPSec de strongSwan](cs_vpn.html#vpn-setup)</td>
<td>Despliegue rápidamente el diagrama de Helm de la VPN IPSec de strongSwan Helm para conectar el clúster de {{site.data.keyword.containershort_notm}} de forma segura al centro de datos local sin un Virtual Router Appliance.</td>
</tr>
<tr>
<td>14 de febrero</td>
<td>[Ubicación disponible en Seúl](cs_regions.html)</td>
<td>Justo a tiempo para los Juegos Olímpicos, despliegue un clúster de Kubernetes en Seúl en la región AP Norte. Si tiene un cortafuegos, [abra los puertos del cortafuegos](cs_firewall.html#firewall) para esta ubicación y los otros dentro de la región en la que se encuentra el clúster.</td>
</tr>
<tr>
<td>8 de febrero</td>
<td>[Actualice Kubernetes 1.9](cs_versions.html#cs_v19)</td>
<td>Revise los cambios que debe realizar en los clústeres antes de actualizar Kubernetes 1.9.</td>
</tr>
</tbody></table>

## Temas populares en enero de 2018
{: #jan18}

<table summary="La tabla muestra los temas populares. Las filas se leen de izquierda a derecha, con la fecha en la columna uno, el título de la característica en la columna dos y una descripción en la columna tres.">
<caption>Temas populares para contenedores y clústeres de Kubernetes en enero de 2018</caption>
<thead>
<th>Fecha</th>
<th>Título</th>
<th>Descripción</th>
</thead>
<tbody>
<td>25 de enero</td>
<td>[Registro global disponible](../services/Registry/registry_overview.html#registry_regions)</td>
<td>Con {{site.data.keyword.registryshort_notm}}, puede utilizar `registry.bluemix.net` global para extraer imágenes públicas proporcionadas por IBM.</td>
</tr>
<tr>
<td>23 de enero</td>
<td>[Ubicaciones disponibles en Singapur y Montreal, CA](cs_regions.html)</td>
<td>Singapur y Montreal son ubicaciones disponibles en las regiones AP Norte y EE.UU. este de {{site.data.keyword.containershort_notm}}. Si tiene un cortafuegos, [abra los puertos del cortafuegos](cs_firewall.html#firewall) para estas ubicaciones y los otros dentro de la región en la que se encuentra el clúster.</td>
</tr>
<tr>
<td>8 de enero</td>
<td>[Disponibles tipos mejorados](cs_cli_reference.html#cs_machine_types)</td>
<td>Los tipos de máquina virtual de la serie 2 incluyen cifrado de disco y almacenamiento SSD. [Traspase sus cargas de trabajo](cs_cluster_update.html#machine_type) a estos tipos para una mayor estabilidad y rendimiento.</td>
</tr>
</tbody></table>

## Chat con desarrolladores de Slack
{: #slack}

Vea de qué están hablando otros desarrolladores y realice sus propias preguntas en [{{site.data.keyword.containershort_notm}} Slack. ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://ibm-container-service.slack.com)
{:shortdesc}

Si no utiliza un ID de IBM para la cuenta de {{site.data.keyword.Bluemix_notm}}, [solicite una invitación](https://bxcs-slack-invite.mybluemix.net/) a este Slack.
{: tip}
