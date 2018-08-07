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



# Por qué {{site.data.keyword.containerlong_notm}}
{: #cs_ov}

{{site.data.keyword.containerlong}} combina Docker y
Kubernetes para ofrecer herramientas potentes, una interfaz intuitiva para el usuario y funciones integradas de seguridad e identificación para automatizar el despliegue, operación, escalado y supervisión de apps contenerizadas sobre un clúster de hosts de cálculo.
{:shortdesc}


## Ventajas de utilizar el servicio
{: #benefits}

Los clústeres se despliegan en hosts de cálculo que proporcionan funciones de Kubernetes nativas y específicas de {{site.data.keyword.IBM_notm}}.
{:shortdesc}

|Ventaja|Descripción|
|-------|-----------|
|Clústeres de Kubernetes de un solo arrendatario con funciones de aislamiento de la infraestructura de cálculo, red y almacenamiento|<ul><li>Cree su propia infraestructura personalizada que se ajuste a los requisitos de su empresa.</li><li>Suministre un maestro de Kubernetes dedicado y seguro, nodos trabajadores, redes virtuales y almacenamiento utilizando los recursos que proporciona la infraestructura de IBM Cloud (SoftLayer).</li><li>Maestro de Kubernetes completamente gestionado que {{site.data.keyword.IBM_notm}} supervisa y actualiza continuamente para mantener el clúster disponible.</li><li>Opción para suministrar nodos trabajadores como servidores nativos con Trusted Compute.</li><li>Almacene datos permanentes, comparta datos entre pods de Kubernetes y restaure datos cuando lo necesite con el servicio de volúmenes seguro e integrado.</li><li>Aproveche el soporte de todas las API nativas de Kubernetes.</li></ul>|
|Conformidad de seguridad de imágenes con Vulnerability Advisor|<ul><li>Configure su propio registro seguro de imágenes privadas de Docker donde todas las imágenes se almacenan y se comparten entre todos los usuarios de la organización.</li><li>Aproveche la exploración automática de imágenes en su registro de {{site.data.keyword.Bluemix_notm}} privado.</li><li>Revise recomendaciones específicas del sistema operativo utilizado en la imagen para solucionar vulnerabilidades potenciales.</li></ul>|
|Supervisión continua del estado del clúster|<ul><li>Utilice el panel de control del clúster para ver y gestionar rápidamente el estado del clúster, de los nodos trabajadores y de los despliegues de contenedores.</li><li>Busque métricas detalladas sobre consumo mediante {{site.data.keyword.monitoringlong}}
y expanda rápidamente su clúster para ajustarlo a las cargas de trabajo.</li><li>Revise la información de registro mediante {{site.data.keyword.loganalysislong}} para ver las actividades detalladas del clúster.</li></ul>|
|Exposición segura de apps al público|<ul><li>Elija entre una dirección IP pública, una ruta proporcionada por {{site.data.keyword.IBM_notm}} o su propio dominio personalizado para acceder a servicios del clúster desde Internet.</li></ul>|
|Integración de servicios de {{site.data.keyword.Bluemix_notm}}|<ul><li>Añada funciones adicionales a la app a través de la integración de servicios de {{site.data.keyword.Bluemix_notm}}, como por ejemplo API de Watson, Blockchain, servicios de datos o Internet de las cosas.</li></ul>|
{: caption="Ventajas de {{site.data.keyword.containerlong_notm}}" caption-side="top"}



<br />


## Comparación de ofertas y sus combinaciones
{: #differentiation}

{{site.data.keyword.containershort_notm}} se puede ejecutar en {{site.data.keyword.Bluemix_notm}} Público o Dedicado, en {{site.data.keyword.Bluemix_notm}} Privado, o en una configuración híbrida.
{:shortdesc}


<table>
<caption>Diferencias entre configuraciones de {{site.data.keyword.containershort_notm}}</caption>
<col width="22%">
<col width="78%">
 <thead>
 <th>Configuración de {{site.data.keyword.containershort_notm}}</th>
 <th>Descripción</th>
 </thead>
 <tbody>
 <tr>
 <td>{{site.data.keyword.Bluemix_notm}} Público
 </td>
 <td>Con {{site.data.keyword.Bluemix_notm}} Público en [máquinas nativas o en hardware dedicado o compartido](cs_clusters.html#shared_dedicated_node), puede alojar sus apps en clústeres en la nube utilizando {{site.data.keyword.containershort_notm}}. {{site.data.keyword.containershort_notm}} en {{site.data.keyword.Bluemix_notm}} Público ofrece herramientas potentes, una interfaz intuitiva para el usuario y funciones integradas de seguridad e identificación para automatizar el despliegue, operación, escalado y supervisión de apps contenerizadas sobre un clúster de hosts de cálculo.<br><br>Para obtener más información, consulte [Tecnología de {{site.data.keyword.containershort_notm}}](cs_tech.html).
 </td>
 </tr>
 <tr>
 <td>{{site.data.keyword.Bluemix_notm}} Dedicado
 </td>
 <td>{{site.data.keyword.Bluemix_notm}} Dedicado ofrece las mismas funcionalidades de {{site.data.keyword.containershort_notm}} en la nube que {{site.data.keyword.Bluemix_notm}} Público. Sin embargo, con una cuenta de {{site.data.keyword.Bluemix_notm}} Dedicado, los [recursos físicos disponibles son dedicados únicamente a su clúster](cs_clusters.html#shared_dedicated_node) y no se comparten con otros clústeres de otros clientes de {{site.data.keyword.IBM_notm}}. Es posible que elija configurar un entorno de {{site.data.keyword.Bluemix_notm}} Dedicado cuando necesite aislamiento para su clúster y otros servicios de {{site.data.keyword.Bluemix_notm}} que desee utilizar.<br><br>Para obtener más información, consulte [Iniciación a los clústeres en {{site.data.keyword.Bluemix_notm}} Dedicado](cs_dedicated.html#dedicated).
 </td>
 </tr>
 <tr>
 <td>{{site.data.keyword.Bluemix_notm}} Privado
 </td>
 <td>{{site.data.keyword.Bluemix_notm}} Privado es una plataforma de aplicaciones que se puede instalar de forma local en sus propias máquinas. Utilice {{site.data.keyword.containershort_notm}} en {{site.data.keyword.Bluemix_notm}} Privado cuando necesite desarrollar y gestionar apps contenerizadas localmente en su propio entorno controlado detrás de un cortafuegos. <br><br>Para obtener más información, consulte [Información del producto {{site.data.keyword.Bluemix_notm}} Privado ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud/private) y esta [documentación ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_1.2.0/kc_welcome_containers.html).
 </td>
 </tr>
 <tr>
 <td>Configuración híbrida
 </td>
 <td>Híbrida es la utilización combinada de los servicios que se ejecutan en {{site.data.keyword.Bluemix_notm}} Público o Dedicado y otros servicios que se ejecutan de forma local como, por ejemplo, una app en {{site.data.keyword.Bluemix_notm}} Privado. Ejemplos de una configuración híbrida: <ul><li>Suministro de un clúster con {{site.data.keyword.containershort_notm}} en {{site.data.keyword.Bluemix_notm}} Público conectando dicho clúster a una base de datos local.</li><li>Suministro de un clúster con {{site.data.keyword.containershort_notm}} en {{site.data.keyword.Bluemix_notm}} Privado desplegando una app en dicho clúster. No obstante, esta app podría utilizar un servicio de {{site.data.keyword.ibmwatson}} como, por ejemplo, {{site.data.keyword.toneanalyzershort}}, en {{site.data.keyword.Bluemix_notm}} Público.</li></ul><br>Para habilitar la comunicación entre los servicios que se ejecutan en {{site.data.keyword.Bluemix_notm}} Público o Dedicado y los servicios que se ejecutan localmente, debe [configurar una conexión VPN](cs_vpn.html).
 </td>
 </tr>
 </tbody>
</table>

<br />


## Comparación entre clústeres gratuitos y estándares
{: #cluster_types}

Puede crear un clúster gratuito o cualquier número de clústeres estándar. Pruebe los clústeres gratuitos para familiarizarse y probar algunas prestaciones de Kubernetes o cree clústeres estándares para utilizar todas las capacidades de Kubernetes para desplegar apps.
{:shortdesc}

|Características|Clústeres gratuitos|Clústeres estándares|
|---------------|-------------|-----------------|
|[Redes en clúster](cs_secure.html#in_cluster_network)
|[Almacenamiento permanente basado en archivo NFS con volúmenes](cs_storage.html#planning)| |<img src="images/confirm.svg" width="32" alt="Característica disponible" style="width:32px;" />|
|[Acceso a app de red pública o privada por parte de un servicio de equilibrador de carga a una dirección IP estable](cs_loadbalancer.html#planning)| |<img src="images/confirm.svg" width="32" alt="Característica disponible" style="width:32px;" />|
|[Acceso a app de red pública por parte de un servicio Ingress a una dirección IP estable y un URL personalizable](cs_ingress.html#planning)| |<img src="images/confirm.svg" width="32" alt="Característica disponible" style="width:32px;" />|
|[Direcciones IP públicas portátiles](cs_subnets.html#manage)| |<img src="images/confirm.svg" width="32" alt="Característica disponible" style="width:32px;" />|
|[Registro y supervisión](cs_health.html#logging)| |<img src="images/confirm.svg" width="32" alt="Característica disponible" style="width:32px;" />|
|[Opción para suministrar los nodos trabajadores en servidores físicos (nativos)](cs_clusters.html#shared_dedicated_node) | |<img src="images/confirm.svg" width="32" alt="Característica disponible" style="width:32px;" />|
|[Opción para suministrar trabajadores nativos con Trusted Compute](cs_clusters.html#shared_dedicated_node) | |<img src="images/confirm.svg" width="32" alt="Característica disponible" style="width:32px;" />|
|[Disponible en {{site.data.keyword.Bluemix_dedicated_notm}}](cs_dedicated.html#dedicated_environment)| |<img src="images/confirm.svg" width="32" alt="Característica disponible" style="width:32px;" />|
{: caption="Características de los clústeres gratuitos y estándares" caption-side="top"}

<br />




{: #responsibilities}
**Nota**: ¿Está buscando las responsabilidades y los términos de {{site.data.keyword.containerlong}} de utilización del servicio?

{: #terms}
Consulte [Responsabilidades de {{site.data.keyword.containershort_notm}}](cs_responsibilities.html).
