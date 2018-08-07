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



# Iniciación a {{site.data.keyword.containerlong_notm}}
{: #container_index}

Empiece a trabajar sin interrupción con {{site.data.keyword.containerlong}} desplegando apps de alta disponibilidad en contenedores Docker que se ejecutan en clústeres de Kubernetes.
{:shortdesc}

Los contenedores son una forma estándar de empaquetar apps y todas sus dependencias para poder moverlas entre entornos sin complicaciones. A diferencia de las máquinas virtuales, los contenedores no incorporan el sistema operativo. El contenedor solo contiene código de la app, tiempo de ejecución, herramientas del sistema, bibliotecas y valores. Los contenedores son más ligeros, portátiles y eficientes que una máquina virtual.


Pulse en una opción para empezar:

<img usemap="#home_map" border="0" class="image" id="image_ztx_crb_f1b" src="images/cs_public_dedicated_options.png" width="440" alt="Pulse un icono para empezar rápidamente con {{site.data.keyword.containershort_notm}}. Con {{site.data.keyword.Bluemix_dedicated_notm}}, pulse este icono para ver las opciones." style="width:440px;" />
<map name="home_map" id="home_map">
<area href="#clusters" alt="Iniciación a clústeres de Kubernetes en {{site.data.keyword.Bluemix_notm}}" title="Iniciación a clústeres de Kubernetes en {{site.data.keyword.Bluemix_notm}}" shape="rect" coords="-7, -8, 108, 211" />
<area href="cs_cli_install.html" alt="Instale las CLI." title="Instale las CLI." shape="rect" coords="155, -1, 289, 210" />
<area href="cs_dedicated.html#dedicated_environment" alt="{{site.data.keyword.Bluemix_dedicated_notm}} Entorno de nube de" title="{{site.data.keyword.Bluemix_notm}} Entorno de nube de" shape="rect" coords="326, -10, 448, 218" />
</map>


## Iniciación a los clústeres
{: #clusters}

Entonces ¿desea desplegar una app en un contenedor? ¡Un momento! Cree antes un clúster de Kubernetes. Kubernetes es una herramienta de organización para contenedores. Con Kubernetes, los desarrolladores pueden desplegar rápidamente apps de alta disponibilidad utilizando la potencia y la flexibilidad de los clústeres.
{:shortdesc}

Y ¿qué es un clúster? Un clúster es un conjunto de recursos, nodos, redes y dispositivos de almacenamiento que mantienen la alta disponibilidad de las apps. Cuando tenga un clúster, podrá desplegar sus apps en contenedores.

**Antes de empezar**

Debe tener una [cuenta de {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/registration/) de prueba, pago según uso o de suscripción.

Con una cuenta de prueba, puede crear un clúster gratuito que puede utilizar durante 21 días para familiarizarse con el servicio. Con una cuenta de pago según uso o de suscripción, podrá crear igualmente un clúster de prueba gratuito, pero también aprovisionar recursos (SoftLayer) de infraestructura de IBM Cloud para utilizar en clústeres estándares.
{:tip}

Para crear un clúster gratuito:

1.  En el [**Catálogo** de {{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/catalog/?category=containers), seleccione **Contenedores en clústeres Kubernetes** y pulse **Crear**. Se abre una página de configuración del clúster. De forma predeterminada, se selecciona **Clúster gratuito**.

2. Dele un nombre exclusivo al clúster.

3.  Pulse **Crear clúster**. Se crea un nodo trabajador, es posible que tarde unos minutos en ser suministrado. En el separador **Nodos trabajadores** verá el progreso de la creación. Cuando el estado sea `Listo`, podrá empezar a trabajar con el clúster.

Enhorabuena. Ha creado su primer clúster de Kubernetes. Estos son algunos detalles sobre su clúster gratuito:

*   **Tipo de máquina**: El clúster gratuito tiene un nodo trabajador virtual con 2 CPU y 4 GB de memoria disponible para que utilicen sus apps. Cuando se crea un clúster estándar, puede elegir entre máquina físicas (nativas) o máquinas virtuales, junto con diversos tamaños de máquina.
*   **Maestro gestionado**: El nodo trabajador se supervisa y se gestiona de forma centralizada mediante un maestro de Kubernetes de alta disponibilidad propiedad de {{site.data.keyword.IBM_notm}} que controla y supervisa todos los recursos de Kubernetes del clúster. Puede centrarse en el nodo trabajador y en las apps que se despliegan en el nodo trabajador sin tener que preocuparse de gestionar también este maestro.
*   **Recursos de infraestructura**: Los recursos necesarios para ejecutar el clúster, como VLANS y direcciones IP, se gestionan en una cuenta de infraestructura de IBM Cloud (SoftLayer) propiedad de {{site.data.keyword.IBM_notm}}. Cuando cree un clúster estándar, podrá gestionar estos recursos en su propia cuenta de infraestructura de IBM Cloud (SoftLayer). Encontrará más información sobre estos recursos y los [permisos necesarios](cs_users.html#infra_access) cuando cree un clúster estándar.
*   **Otras opciones**: Los clústeres gratuitos se despliegan dentro de la región que selecciona, pero no puede elegir en qué ubicación (centro de datos). Para obtener un control sobre la ubicación, las redes y el almacenamiento persistente, cree un clúster estándar. [Más información sobre las ventajas de los clústeres gratuitos y estándar](cs_why.html#cluster_types).


**¿Qué es lo siguiente?**
En los próximos 21 días, pruebe lo siguiente en su clúster gratuito.

* [Instalar las CLI para empezar a trabajar con el clúster.](cs_cli_install.html#cs_cli_install)
* [Desplegar una app en el clúster.](cs_app.html#app_cli)
* [Cree un clúster estándar con varios nodos para aumentar la disponibilidad.](cs_clusters.html#clusters_ui)
* [Configure un registro privado en {{site.data.keyword.Bluemix_notm}} para almacenar y compartir imágenes de Docker con otros usuarios. ](/docs/services/Registry/index.html)
