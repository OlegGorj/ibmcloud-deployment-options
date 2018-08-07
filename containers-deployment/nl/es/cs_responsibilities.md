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



# Responsabilidades al utilizar {{site.data.keyword.containerlong_notm}}
Conozca las responsabilidades de gestión del clúster y los términos y las condiciones existentes para utilizar {{site.data.keyword.containerlong}}.
{:shortdesc}

## Responsabilidades de la gestión de clústeres
{: #responsibilities}

Revise las responsabilidades que comparte con IBM para gestionar sus clústeres.
{:shortdesc}

**IBM es responsable de:**

- Desplegar el maestro, los nodos trabajadores y los componentes de gestión dentro del clúster, como el equilibrador de carga de aplicación de Ingress, en el momento de la creación del clúster
- Gestionar las actualizaciones de seguridad, la supervisión, el aislamiento y la recuperación del maestro de Kubernetes para el clúster
- Supervisar la salud de los nodos trabajadores y proporcionar automatización para la actualización y recuperación de los nodos trabajadores
- Realizar tareas de automatización sobre su cuenta de la infraestructura, incluida la adición de nodos trabajadores, la eliminación de nodos trabajadores y la creación de una subred predeterminada
- Gestionar, actualizar y recuperar los componentes operativos dentro del clúster, como por ejemplo el equilibrador de carga de aplicación de Ingress y el plug-in de almacenamiento
- Suministrar volúmenes de almacenamiento cuando lo soliciten las reclamaciones de volumen permanente
- Proporcionar valores de seguridad en todos los nodos trabajadores

</br>

**El usuario es responsable de:**

- [Configurar de su cuenta de {{site.data.keyword.Bluemix_notm}} para acceder al portafolio (Softlayer) de infraestructura de IBM Cloud](cs_troubleshoot_clusters.html#cs_credentials)
- [Desplegar y gestionar los recursos de Kubernetes, como pods, servicios y despliegues, dentro del clúster](cs_app.html#app_cli)
- [Aprovechar la capacidad del servicio y de Kubernetes para garantizar la alta disponibilidad de las apps](cs_app.html#highly_available_apps)
- [Añadir o eliminar capacidad del clúster mediante la CLI para añadir o eliminar nodos trabajadores](cs_cli_reference.html#cs_worker_add)
- [Crear VLAN públicas y privadas en la infraestructura de IBM Cloud (SoftLayer) para el aislamiento de la red del clúster](/docs/infrastructure/vlans/getting-started.html#getting-started-with-vlans)
- [Garantizar que todos los nodos trabajadores tienen conectividad de red con el URL maestro de Kubernetes](cs_firewall.html#firewall) <p>**Nota**: Si un trabajador tiene VLAN tanto públicas como privadas, se configura la conectividad de red. Si los nodos trabajadores únicamente se configuran con una VLAN privada, debe configurar una solución alternativa para la conectividad de red. Para obtener más información, consulte [Conexión de VLAN para nodos trabajadores](cs_clusters.html#worker_vlan_connection). </p>
- [La actualización del maestro kube-apiserver cuando haya disponibles actualizaciones de versión de Kubernetes](cs_cluster_update.html#master)
- [Mantenimiento actualizado de nodos trabajadores para versiones mayores, menores y parches](cs_cluster_update.html#worker_node)
- [Recuperar nodos trabajadores con problemas ejecutando los mandatos `kubectl`, tales como `cordon` o `drain`, y ejecutando los mandatos `bx cs`, tales como `reboot`, `reload` o `delete`](cs_cli_reference.html#cs_worker_reboot)
- [Añadir o eliminar subredes en la infraestructura de IBM Cloud (SoftLayer) cuando sea necesario](cs_subnets.html#subnets)
- [Hacer copia de seguridad y restaurar datos en el almacenamiento permanente en la infraestructura de IBM Cloud (SoftLayer) ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](../services/RegistryImages/ibm-backup-restore/index.html)
- [Configuración de la supervisión de estado de los nodos trabajadores con recuperación automática](cs_health.html#autorecovery)

<br />


## Abuso en {{site.data.keyword.containerlong_notm}}
{: #terms}

Los clientes no puede utilizar {{site.data.keyword.containershort_notm}} de forma inapropiada.
{:shortdesc}

Entre los usos no apropiados se incluye:

*   Cualquier actividad ilegal
*   Distribuir o ejecutar malware
*   Provocar cualquier daño a {{site.data.keyword.containershort_notm}} o interferir a cualquier usuario con la utilización de {{site.data.keyword.containershort_notm}}
*   Provocar daños o interferir la utilización de otros usuarios con cualquier otro sistema o servicio
*   Acceder de forma no autorizada a los servicios o sistemas
*   Realizar modificaciones no autorizadas de los servicios o sistemas
*   Incumplir los derechos de terceros


Consulte [Términos de los servicios en la nube](https://console.bluemix.net/docs/overview/terms-of-use/notices.html#terms) para obtener una visión general de los términos uso.
