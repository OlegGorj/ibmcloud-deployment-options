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




# Seguridad para {{site.data.keyword.containerlong_notm}}
{: #security}

Puede utilizar características integradas de seguridad en {{site.data.keyword.containerlong}} para análisis de riesgos y protección de la seguridad. Estas características le ayudan a proteger la infraestructura del clúster de Kubernetes y las comunicaciones en la red, a aislar sus recursos de cálculo y a garantizar la conformidad de la seguridad de los componentes de la infraestructura y de los despliegues de contenedores.
{: shortdesc}



## Seguridad por componente de clúster
{: #cluster}

Cada clúster de {{site.data.keyword.containerlong_notm}} tiene características de seguridad que están incorporadas en sus nodos [maestro](#master) y de [trabajador](#worker).
{: shortdesc}

Si tiene un cortafuegos, o si desea ejecutar mandatos `kubectl` desde el sistema local cuando las políticas de red corporativas impiden el acceso a los puntos finales de Internet pública, [abra los puertos en el cortafuegos](cs_firewall.html#firewall). Para conectar apps del clúster a una red local o a otras apps externas al clúster, [configure la conectividad de VPN](cs_vpn.html#vpn).

En el siguiente diagrama, puede ver características de seguridad agrupadas por maestro de Kubernetes, nodos trabajadores e imágenes de contenedor.

<img src="images/cs_security.png" width="400" alt="Seguridad de clústeres de {{site.data.keyword.containershort_notm}}" style="width:400px; border-style: none"/>


<table summary="La primera fila de la tabla abarca ambas columnas. El resto de las filas se deben leer de izquierda a derecha, con el componente de seguridad en la primera columna y sus características en la segunda columna. ">
<caption>Seguridad por componente</caption>
  <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Valores integrados de seguridad de clústeres de {{site.data.keyword.containershort_notm}}</th>
  </thead>
  <tbody>
    <tr>
      <td>Maestro de Kubernetes</td>
      <td>IBM gestiona el maestro de Kubernetes de cada clúster, que ofrece una alta disponibilidad. El maestro incluye valores de seguridad de {{site.data.keyword.containershort_notm}} que garantizan la conformidad de seguridad y la comunicación segura entre los nodos trabajadores. IBM realiza las actualizaciones de seguridad según sea necesario. El maestro de Kubernetes dedicado controla y supervisa de forma centralizada todos los recursos de Kubernetes del clúster. Basándose en los requisitos de despliegue y la capacidad del clúster, el maestro de Kubernetes planifica automáticamente las apps contenerizadas para desplegar en nodos trabajadores disponibles. Para obtener más información, consulte [Seguridad del maestro de Kubernetes](#master).</td>
    </tr>
    <tr>
      <td>Nodo trabajador</td>
      <td>Los contenedores se despliegan en nodos trabajadores que están dedicados a un clúster y que garantizan las funciones de cálculo, red y aislamiento del almacenamiento para los clientes de IBM. {{site.data.keyword.containershort_notm}} proporciona funciones integradas de seguridad para mantener la seguridad de los nodos trabajadores en la red privada y pública y garantizan la conformidad de seguridad de los nodos trabajadores. Para obtener más información, consulte [Seguridad de los nodos trabajadores](#worker). Además, puede añadir [políticas de red Calico](cs_network_policy.html#network_policies) para especificar el tráfico de red que desee permitir o bloquear de y desde un pod a un nodo trabajador.</td>
    </tr>
    <tr>
      <td>Imágenes</td>
      <td>Como administrador del clúster, puede configurar un repositorio seguro propio de imágenes de Docker en {{site.data.keyword.registryshort_notm}} donde puede almacenar y compartir imágenes de Docker entre los usuarios del clúster. Para garantizar despliegues de contenedor seguros, Vulnerability Advisor explora cada imagen del registro privado. Vulnerability Advisor es un componente de {{site.data.keyword.registryshort_notm}} que realiza exploraciones en busca de vulnerabilidades, realiza recomendaciones sobre seguridad y ofrece instrucciones para solucionar las vulnerabilidades. Para obtener más información, consulte [Seguridad de imágenes en {{site.data.keyword.containershort_notm}}](#images).</td>
    </tr>
  </tbody>
</table>

<br />


## Maestro de Kubernetes
{: #master}

Revise las funciones integradas de seguridad de maestro de Kubernetes para proteger el maestro de Kubernetes y para asegurar la comunicación de red del clúster.
{: shortdesc}

<dl>
  <dt>Maestro de Kubernetes totalmente gestionado y dedicado</dt>
    <dd>Cada clúster de Kubernetes de {{site.data.keyword.containershort_notm}} está controlado por un maestro de Kubernetes gestionado por IBM en una cuenta de infraestructura de IBM Cloud (SoftLayer) propiedad de IBM. El maestro de Kubernetes se configura con los siguientes componentes dedicados que no se comparten con otros clientes de IBM ni con otros clústeres en la misma cuenta de IBM.
      <ul><li>Almacén de datos etcd: almacena todos los recursos de Kubernetes de un clúster, como por ejemplo servicios, despliegues y pods. ConfigMaps y secretos de Kubernetes son datos de app que se almacenan como pares de clave y valor para que los pueda utilizar una app que se ejecuta en un pod. Los datos de etcd, de los que se hace una copia de seguridad diaria, se almacenan en un disco cifrado gestionado por IBM. Cuando se envían a un pod, los datos se cifran mediante TLS para garantizar la protección y la integridad de los datos. </li>
      <li>kube-apiserver: sirve como punto de entrada principal para todas las solicitudes procedentes del nodo trabajador destinadas al maestro de Kubernetes. kube-apiserver valida y procesa las solicitudes y puede leer y escribir en el almacén de datos
etcd.</li>
      <li>kube-scheduler: decide dónde desplegar los pods, considerando los requisitos de capacidad y de rendimiento, las restricciones de política de hardware y de software, las especificaciones anti afinidad y los requisitos de carga de trabajo. Si no se encuentra ningún nodo trabajador que se ajuste a los requisitos, el pod no se despliega en el clúster.</li>
      <li>kube-controller-manager: responsable de supervisar los conjuntos de réplicas y de crear los pods correspondientes para alcanzar el estado deseado.</li>
      <li>OpenVPN: componente específico de {{site.data.keyword.containershort_notm}} que proporciona conectividad de red segura para la comunicación entre el maestro de Kubernetes y el nodo trabajador.</li></ul></dd>
  <dt>Conectividad de red protegida por TLS para toda la comunicación entre el nodo trabajador y maestro de Kubernetes</dt>
    <dd>Para proteger la conexión con el maestro de Kubernetes, {{site.data.keyword.containershort_notm}} genera certificados TLS que cifran la comunicación entre los componentes kube-apiserver y el almacén de datos etcd para cada clúster. Estos certificados nunca se comparten entre clústeres ni entre componentes de maestro de Kubernetes.</dd>
  <dt>Conectividad de red protegida por OpenVPN para la comunicación entre el maestro de Kubernetes y el nodo trabajador</dt>
    <dd>Aunque Kubernetes protege la comunicación entre el maestro de Kubernetes y los nodos trabajadores mediante el protocolo `https`, no se proporciona autenticación en el nodo trabajador de forma predeterminada. Para proteger esta comunicación, {{site.data.keyword.containershort_notm}} configura automáticamente una conexión OpenVPN entre el maestro de Kubernetes y el nodo trabajador cuando se crea el clúster.</dd>
  <dt>Supervisión continua de la red del maestro de Kubernetes</dt>
    <dd>IBM supervisa continuamente cada maestro de Kubernetes para controlar y solucionar los ataques como los de denegación de servicio (DOS) a nivel de proceso.</dd>
  <dt>Conformidad con la seguridad del nodo maestro de Kubernetes</dt>
    <dd>{{site.data.keyword.containershort_notm}} explora automáticamente cada nodo en el que se ha desplegado el nodo maestro de Kubernetes en busca de vulnerabilidades y arreglos de seguridad específicos de Kubernetes y del sistema operativo. Si se encuentran vulnerabilidades, {{site.data.keyword.containershort_notm}} aplica automáticamente los arreglos y soluciona las vulnerabilidades en nombre del usuario para asegurarse de la protección del nodo maestro. </dd>
</dl>

<br />


## Nodos trabajadores
{: #worker}

Revise las características de seguridad integradas del nodo trabajador para proteger el entorno de nodo trabajador y para garantizar el aislamiento de recursos, red y almacenamiento.
{: shortdesc}

<dl>
  <dt>Propiedad de los nodos trabajadores</dt>
    <dd>La propiedad de los nodos trabajadores depende del tipo de clúster que cree. <p> Los nodos trabajadores de los clústeres gratuitos se suministran a la cuenta de la infraestructura de IBM Cloud (SoftLayer) que es propiedad de IBM. Los usuarios pueden desplegar apps en los nodos trabajadores, pero no pueden cambiar los valores o instalar software adicional en el nodo trabajador.</p>
    <p>Los nodos trabajadores de los clústeres estándar se suministran a la cuenta de la infraestructura de IBM Cloud (SoftLayer) que está asociada a la cuenta de IBM Cloud pública o dedicada del cliente. Los clientes son propietarios de los nodos trabajadores. Los clientes pueden elegir cambiar los valores de seguridad o instalar software adicional en los nodos trabajadores, según lo proporcionado por {{site.data.keyword.containerlong}}.</p> </dd>
  <dt>Aislamiento de la infraestructura de cálculo, red y almacenamiento</dt>
    <dd><p>Cuando se crea un clúster, IBM suministra los nodos trabajadores como máquinas virtuales. Los nodos trabajadores están dedicados a un clúster y no albergan cargas de trabajo de otros clústeres.</p>
    <p> Cada cuenta de {{site.data.keyword.Bluemix_notm}} se configura con una VLAN de infraestructura de IBM Cloud (SoftLayer) para garantizar el rendimiento y el aislamiento de red de calidad en los nodos trabajadores. También puede designar nodos trabajadores como privados conectándolos solo a una VLAN privada.</p> <p>Para conservar los datos en el clúster, puede suministrar almacenamiento de archivos dedicado basado en NFS de infraestructura de IBM Cloud (SoftLayer) y aprovechar las características integradas de seguridad de datos de dicha plataforma.</p></dd>
  <dt>Configuración protegida de nodo trabajador</dt>
    <dd><p>Cada nodo trabajador se configura con un sistema operativo Ubuntu que el propietario del nodo trabajador no puede modificar. Dado que el sistema operativo del nodo trabajador es Ubuntu, todos los contenedores desplegados en el nodo trabajador deben utilizar una distribución de Linux que utilice el kernel de Ubuntu. Las distribuciones de Linux que se deben comunicar con el kernel de otra forma no se pueden utilizar. Para proteger el sistema operativo de los nodos trabajadores frente a ataques potenciales, cada noto trabajador se configura con valores avanzados de cortafuegos que imponen las reglas de iptable de Linux.</p>
    <p>Todos los contenedores que se ejecutan en Kubernetes están protegidos por valores predefinidos de política de red de Calico configurados en cada nodo trabajador durante la creación del clúster. Esta configuración garantiza una comunicación de red segura entre nodos trabajadores y pods.</p>
    <p>El acceso SSH está inhabilitado en el nodo trabajador. Si tiene un clúster estándar y desea instalar más características en su nodo trabajador, utilice los [Conjuntos de daemons de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset) para todo lo que desee ejecutar en cada nodo trabajador. Para cualquier acción individual que deba ejecutar, utilice los [Trabajos de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/).</p></dd>
  <dt>Conformidad de seguridad de nodos trabajadores de Kubernetes</dt>
    <dd>IBM trabaja con equipos de asesoramiento de seguridad interna y externa para abordar las vulnerabilidades potenciales de conformidad de seguridad. <b>Importante</b>: Utilice con regularidad (por ejemplo, mensualmente) el [mandato](cs_cli_reference.html#cs_worker_update) `bx cs worker-update` para desplegar actualizaciones y parches de seguridad para el sistema operativo y para actualizar la versión de Kubernetes. Cuando hay actualizaciones disponibles, se le notifica al visualizar información sobre los nodos trabajadores, por ejemplo, con los mandatos `bx cs workers <cluster_name>` o `bx cs worker-get <cluster_name> <worker_ID>`.</dd>
  <dt>Opción para desplegar nodos en servidores físicos (nativos)</dt>
    <dd>Si elige suministrar los nodos trabajadores en servidores físicos nativos (en lugar de instancias de servidor virtual), tiene más control sobre el host de cálculo, como por ejemplo la memoria o la CPU. Esta configuración elimina el hipervisor de máquina virtual que asigna recursos físicos a máquinas virtuales que se ejecutan en el host. En su lugar, todos los recursos de una máquina nativa están dedicados exclusivamente al trabajador, por lo que no es necesario preocuparse por "vecinos ruidosos" que compartan recursos o ralenticen el rendimiento. Los servidores nativos están dedicados a usted, con todos los recursos disponibles para que los utilice el clúster.</dd>
  <dt id="trusted_compute">{{site.data.keyword.containershort_notm}} con Trusted Compute</dt>
    <dd><p>Cuando [despliega el clúster en dispositivos nativos](cs_clusters.html#clusters_ui) compatibles con Trusted Compute, puede habilitar la confianza. El chip de TPM (módulo de plataforma de confianza) está habilitado en cada nodo trabajador nativo en el clúster compatible con Trusted Compute (incluidos los nodos futuros que añada al clúster). Por consiguiente, después de habilitar la confianza, no puede inhabilitarla posteriormente para el clúster. Se despliega un servidor de confianza en el nodo maestro, y se despliega un agente de confianza como un pod en el nodo trabajador. Cuando el nodo trabajador se inicia, el pod del agente de confianza supervisa cada etapa del proceso.</p>
    <p>Por ejemplo, si un usuario no autorizado accede a su sistema y modifica el kernel del sistema operativo con lógica adicional para recopilar datos, el agente de confianza detecta este cambio y marca el nodo como de no confianza. Con la informática de confianza, puede verificar que los nodos trabajadores no se manipulan de forma indebida.</p>
    <p><strong>Nota</strong>: Trusted Compute está disponible para determinados tipos de máquinas nativas. Por ejemplo, los distintos tipos de GPU `mgXc` no dan soporte a Trusted Compute.</p></dd>
  <dt id="encrypted_disks">Disco cifrado</dt>
    <dd><p>De forma predeterminada, {{site.data.keyword.containershort_notm}} proporciona dos particiones locales SSD de datos cifrados para todos los nodos trabajadores cuando se suministran los nodos trabajadores. La primera partición no está cifrada y la segunda partición, montada en _/var/lib/docker_, está desbloqueada mediante claves de cifrado LUKS. Cada trabajador de cada clúster de Kubernetes tiene su propia clave de cifrado LUKS, que gestiona {{site.data.keyword.containershort_notm}}. Cuando se crea un clúster o se añade un nodo trabajador a un clúster existente, las claves se extraen de forma segura y luego se descartan una vez desbloqueado el disco cifrado.</p>
    <p><b>Nota</b>: el cifrado puede afectar al rendimiento de E/S del disco. Para las cargas de trabajo que requieren E/S de disco de alto rendimiento, pruebe un clúster con cifrado habilitado e inhabilitado para decidir mejor si desactiva el cifrado.</p></dd>
  <dt>Soporte para cortafuegos de red de infraestructura de IBM Cloud (SoftLayer)</dt>
    <dd>{{site.data.keyword.containershort_notm}} es compatible con todas las [ofertas de cortafuegos de infraestructura de IBM Cloud (SoftLayer) ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud-computing/bluemix/network-security). En {{site.data.keyword.Bluemix_notm}} Público, puede configurar un cortafuegos con políticas de red personalizadas para proporcionar seguridad de red dedicada para el clúster estándar y para detectar y solucionar problemas de intrusión en la red. Por ejemplo, podría elegir configurar un [Virtual Router Appliance](/docs/infrastructure/virtual-router-appliance/about.html) para que actuase como cortafuegos para bloquear el tráfico no deseado. Si configura un cortafuegos, [también debe abrir los puertos y direcciones IP necesarios](cs_firewall.html#firewall) para cada región para que los nodos maestro y trabajador se puedan comunicar.</dd>
  <dt>Conserve la privacidad de los servicios o exponga servicios y apps a Internet pública de forma selectiva</dt>
    <dd>Puede conservar la privacidad de sus servicios y apps y aprovechar las características seguridad integradas para garantizar una comunicación segura entre nodos trabajadores y pods. Para exponer servicios y apps a Internet pública, puede aprovechar el soporte de Ingress y del equilibrador de carga para poner los servicios a disponibilidad pública de forma segura.</dd>
  <dt>Conecte de forma segura sus nodos trabajadores y apps a un centro de datos local</dt>
    <dd><p>Para conectar los nodos trabajadores y las apps a un centro de datos local, configure un punto final IPsec VPN con un servicio strongSwan, un Virtual Router Appliance o un Fortigate Security Appliance.</p>
    <ul><li><b>Servicio VPN IPsec de strongSwan</b>: puede configurar un [servicio VPN IPSec de strongSwan ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.strongswan.org/) que se conecte de forma segura al clúster de Kubernetes con una red local. El servicio VPN IPSec de strongSwan proporciona un canal de comunicaciones de extremo a extremo seguro sobre Internet que está basado en la suite de protocolos
Internet Protocol Security (IPsec) estándar del sector. Para configurar una conexión segura entre el clúster y una red local, [configure y despliegue el servicio VPN IPSec strongSwan](cs_vpn.html#vpn-setup) directamente en un pod del clúster.
    </li>
    <li><b>Virtual Router Appliance (VRA) o Fortigate Security Appliance (FSA)</b>: Podría elegir entre configurar un [VRA](/docs/infrastructure/virtual-router-appliance/about.html) o un [FSA](/docs/infrastructure/fortigate-10g/about.html) para configurar un punto final de VPN IPSec. Esta opción es útil si tiene un clúster grande, desea acceder a recursos no Kubernetes sobre la VPN o desea acceder a varios clústeres en una sola VPN. Para configurar VRA, consulte [Configuración de la conectividad de VPN con VRA](cs_vpn.html#vyatta).</li></ul></dd>

  <dt>Supervisión y registro continuos de actividad de clúster</dt>
    <dd>Con los clústeres estándares, todos los sucesos relacionados con los clústeres se pueden registrar y enviar a {{site.data.keyword.loganalysislong_notm}} y {{site.data.keyword.monitoringlong_notm}}. Estos sucesos incluyen la adición de un nodo trabajador, el progreso de despliegue de actualizaciones o información de utilización de la capacidad. Puede [configurar la creación de registros del clúster](/docs/containers/cs_health.html#logging) y la [supervisión del clúster](/docs/containers/cs_health.html#view_metrics) para decidir los sucesos que desea supervisar. </dd>
</dl>

<br />


## Imágenes
{: #images}

Gestione la seguridad y la integridad de sus imágenes con características integradas de seguridad.
{: shortdesc}

<dl>
<dt>Repositorio de imágenes privadas seguras de Docker en {{site.data.keyword.registryshort_notm}}</dt>
  <dd>Configure su propio repositorio de imágenes de Docker en un registro privado de imágenes multiarrendatario, de alta disponibilidad y escalable alojado y gestionado por IBM. Con este registro, podrá crear, almacenar de forma segura y compartir imágenes de Docker con todos los usuarios del clúster.
  <p>Obtenga más información sobre cómo [proteger su información personal](cs_secure.html#pi) cuando se trabaja con imágenes de contenedor.</p></dd>
<dt>Conformidad con la seguridad de imágenes</dt>
  <dd>Cuando se utiliza {{site.data.keyword.registryshort_notm}}, puede aprovechar la exploración de seguridad integrada que proporciona Vulnerability Advisor. Cada imagen que se publica en el espacio de nombres se explora automáticamente para detectar vulnerabilidades especificadas en una base de datos de problemas conocidos de CentOS, Debian, Red Hat y Ubuntu. Si se encuentran vulnerabilidades, Vulnerability Advisor proporciona instrucciones sobre cómo resolverlas para garantizar la integridad y la seguridad.</dd>
</dl>

Para ver la valoración de vulnerabilidades de las imágenes, [revise la documentación de Vulnerability Advisor](/docs/services/va/va_index.html#va_registry_cli).

<br />


## Redes en clúster
{: #in_cluster_network}

Una comunicación de red en clúster segura entre nodos trabajadores y pods se consigue con redes de área local virtuales privadas (VLAN). Una VLAN configura un grupo de nodos trabajadores y pods como si estuvieran conectadas a la misma conexión física.
{:shortdesc}

Cuando crea un clúster, cada clúster se conecta automáticamente a una VLAN privada. La VLAN privada determina la dirección IP privada que se asigna a un nodo trabajador durante la creación del clúster.

|Tipo de clúster|Gestor de la VLAN privada del clúster|
|------------|-------------------------------------------|
|Clústeres gratuitos en {{site.data.keyword.Bluemix_notm}}|{{site.data.keyword.IBM_notm}}|
|Clústeres estándares en {{site.data.keyword.Bluemix_notm}}|En la cuenta de infraestructura de IBM Cloud (SoftLayer) <p>**Sugerencia:** Para tener acceso a todas las VLAN de la cuenta, active [VLAN Spanning](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning).</p>|
{: caption="Gestores de VLAN privadas" caption-side="top"}

También se asigna una dirección IP privada a todos los pods desplegados en un nodo trabajador. Se asigna a los pods
una IP del rango de direcciones privadas 172.30.0.0/16 y se direccionan solo entre nodos trabajadores. Para evitar conflictos, no utilice este rango de IP en ningún otro nodo que se vaya a comunicar con los nodos trabajadores. Los nodos trabajadores y los pods pueden comunicarse de forma segura a través de la red privada utilizando direcciones IP privadas. Sin embargo, cuando un pod se cuelga o cuando es necesario volver a crear un nodo trabajador, se signa una nueva dirección IP privada.

De forma predetermina, es difícil realizar un seguimiento de direcciones IP privadas que cambian para las apps que deben ofrecer una alta disponibilidad. Para evitar esto, puede utilizar las características incorporadas de descubrimiento de servicios de Kubernetes y exponer las apps como servicios IP del clúster en la red privada. Un servicio de Kubernetes agrupa un conjunto de pods y proporciona una conexión de red a estos pods para otros servicios del clúster sin exponer la dirección IP privada real de cada pod. Cuando se crea un servicio de IP de clúster, se le asigna una dirección IP privada del rango de direcciones privadas 10.10.10.0/24. Al igual que sucede con el rango de direcciones privadas de pod, no utilice este rango de IP en ningún otro nodo que se vaya a comunicar con los nodos trabajadores. Solo se puede acceder a esta dirección IP dentro del clúster. No puede acceder a esta dirección IP desde Internet. Al mismo tiempo, se crea una entrada de búsqueda DNS para el servicio, que se guarda en el componente kube-dns del clúster. La entrada DNS contiene el nombre del servicio, el espacio de nombres en el que se ha creado el servicio y el enlace a la dirección IP privada asignada del clúster.

Para acceder a un pod detrás de un servicio de IP de clúster, la app puede utilizar la dirección IP de clúster privada del servicio o enviar una solicitud utilizando el nombre del servicio. Si utiliza el nombre del servicio, se busca el nombre en el componente
kube-dns y se direcciona a la dirección IP privada del clúster del servicio. Cuando llega una solicitud al servicio, este asegura que todas las solicitudes se envían a los pods, independientemente de sus direcciones IP privadas y del nodo trabajador en el que estén desplegadas.

Para obtener más información sobre cómo crear un servicio de tipo IP de clúster, consulte [Servicios de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services---service-types).

Para conectar apps de forma segura en un clúster Kubernetes a una red local, consulte [Configuración de la conectividad de VPN](cs_vpn.html#vpn). Para exponer sus apps para la comunicación de redes externas, consulte [Cómo permitir el acceso público a apps](cs_network_planning.html#public_access).


<br />


## Confianza de clúster
{: cs_trust}

De forma predeterminada, {{site.data.keyword.containerlong_notm}} proporciona muchas [características para componentes del clúster](#cluster) para poder desplegar sus apps contenerizadas en un entorno enriquecido de alta seguridad. Amplíe su nivel de confianza en su clúster para asegurarse de que lo que sucede en su clúster es lo que tiene previsto. La confianza en el clúster se puede implementar de varias maneras, tal como se muestra en el siguiente diagrama.
{:shortdesc}

![Despliegue de contenedores con contenido de confianza](images/trusted_story.png)

1.  **{{site.data.keyword.containerlong_notm}} con Trusted Compute**: La confianza se puede habilitar en los clústeres nativos. El agente de confianza supervisa el proceso de inicio de hardware e informa de cualquier cambio para que pueda verificar si sus nodos trabajadores nativos han sido manipulados. Con Trusted Compute, puede desplegar contenedores en hosts nativos verificados de forma que sus cargas de trabajo se ejecuten en un hardware de confianza. Tenga en cuenta que algunas máquinas nativas, por ejemplo que utilizan GPU, no dan soporte a Trusted Compute. [Más información sobre cómo Trusted Compute funciona](#trusted_compute).

2.  **Confianza de contenido en sus imágenes**: Asegúrese de la integridad de sus imágenes habilitando la confianza de contenido en {{site.data.keyword.registryshort_notm}}. Con el contenido de confianza podrá controlar los usuarios que pueden firmar imágenes como imágenes de confianza. Después los firmantes de confianza transferirán una imagen a su registro y los usuarios podrán extraer el contenido firmado de forma que podrán verificar el origen de la misma. Para obtener más información, consulte [Firma de imágenes para contenido de confianza](/docs/services/Registry/registry_trusted_content.html#registry_trustedcontent).

3.  **Container Image Security Enforcement (beta)**: Crea un controlador de admisión con políticas personalizadas de forma que es posible verificar las imágenes del contenedor antes de desplegarlas. Con Container Image Security Enforcement, podrá controlar dónde se despliegan las imágenes y asegurarse de que se satisfacen las políticas de [Vulnerability Advisor](/docs/services/va/va_index.html) o los requisitos del [contenido de confianza](/docs/services/Registry/registry_trusted_content.html#registry_trustedcontent). Si un despliegue no cumple las políticas que ha establecido, la aplicación de la seguridad impide modificaciones en el clúster. Para obtener más información, consulte [Container Image Security Enforcement (beta)](/docs/services/Registry/registry_security_enforce.html#security_enforce).

4.  **Container Vulnerability Scanner**: De forma predeterminada, Vulnerability Advisor explora imágenes almacenadas en {{site.data.keyword.registryshort_notm}}. Para comprobar el estado de los contenedores activos que están en ejecución en el clúster, puede instalar el explorador de contenedores. Para obtener más información, consulte [Instalación del explorador de contenedores](/docs/services/va/va_index.html#va_install_livescan).

5.  **Análisis de red con Security Advisor (vista previa)**: Con {{site.data.keyword.Bluemix_notm}} Security Advisor podrá centralizar el conocimiento de seguridad de los servicios de {{site.data.keyword.Bluemix_notm}} como, por ejemplo, de Vulnerability Advisor y {{site.data.keyword.cloudcerts_short}}. Cuando se habilita Security Advisor en el clúster, puede ver informes sobre el tráfico de red entrante y saliente sospechoso. Para obtener más información, consulte [Analíticas de red](/docs/services/security-advisor/network-analytics.html#network-analytics). Para instalar, consulte [Configuración de supervisión de direcciones de IP y clientes sospechosos para un clúster Kubernetes](/docs/services/security-advisor/setup_cluster.html).

6.  **{{site.data.keyword.cloudcerts_long_notm}} (beta)**: Si posee un clúster en EE.UU. Sur y desea [exponer su app utilizando un dominio personalizado con TLS](https://console.bluemix.net/docs/containers/cs_ingress.html#ingress_expose_public), puede almacenar su certificado TLS en {{site.data.keyword.cloudcerts_short}}. El panel de control de Security Advisor también puede informar de certificados caducados o a punto de caducar. Para obtener más información, consulte [Iniciación a {{site.data.keyword.cloudcerts_short}}](/docs/services/certificate-manager/index.html#gettingstarted).

<br />


## Almacenamiento de información personal
{: #pi}

El usuario es responsable de garantizar la seguridad de la información personal en las imágenes de contenedor y en los recursos de Kubernetes. La información personal incluye nombre, dirección, número de teléfono, dirección de correo electrónico u otra información que permita identificar, ponerse en contacto o ubicar a los clientes, o a cualquier otro individuo.
{: shortdesc}

<dl>
  <dt>Utilización de un secreto de Kubernetes para almacenar información personal</dt>
  <dd>Únicamente almacene información personal en recursos Kubernetes que estén diseñados para alojar información personal. Por ejemplo, no utilice su nombre en el nombre de una correlación de configuración, servicio, despliegue o espacio de nombres de Kubernetes. En lugar de ello, para un cifrado y protección adecuados, almacene la información personal en los <a href="cs_app.html#secrets">secretos de Kubernetes</a>.</dd>

  <dt>Utilización de `imagePullSecret` de Kubernetes para almacenar credenciales de registros de imagen</dt>
  <dd>No almacene información personal en espacios de nombres de registro ni imágenes de contenedor. En lugar de ello, para un cifrado y protección adecuados, almacene las credenciales de registro en <a href="cs_images.html#other">Kubernetes imagePullSecrets</a> y otra información personal en los <a href="cs_app.html#secrets">secretos de Kubernetes</a>. Recuerde que si la información personal se almacena en una capa previa de una imagen, la supresión de la imagen no será suficiente para suprimir esta información personal.</dd>
  </dl>

<br />






