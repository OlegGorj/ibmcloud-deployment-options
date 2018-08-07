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


# Configuración de clústeres
{: #clusters}

Diseñe la configuración del clúster de Kubernetes para maximizar la disponibilidad y la capacidad del contenedor con {{site.data.keyword.containerlong}}.
{:shortdesc}



## Planificación de configuración de clúster
{: #planning_clusters}

Utilice clústeres estándares para incrementar la disponibilidad de las apps.
{:shortdesc}

Si distribuye su configuración entre varios clústeres y nodos trabajadores, es menos probable que los usuarios experimenten un tiempo de inactividad del sistema. Características incorporadas como, por ejemplo, el aislamiento y el equilibrio de carga, incrementan la resiliencia con relación a posibles anomalías con hosts, redes o apps.

Revise estas configuraciones potenciales de clústeres que están ordenadas por grados de disponibilidad en orden ascendente:

![Etapas de alta disponibilidad de un clúster](images/cs_cluster_ha_roadmap.png)

1.  Un clúster con varios nodos trabajadores
2.  Dos clústeres que se ejecutan en distintas ubicaciones de la misma región, cada uno con varios nodos trabajadores
3.  Dos clústeres que se ejecutan en distintas regiones, cada uno con varios nodos trabajadores


### Aumente la disponibilidad de su clúster

<dl>
  <dt>Distribuya las apps entre nodos trabajadores</dt>
    <dd>Permita que los desarrolladores distribuyan sus apps en contenedores entre varios nodos trabajadores por clúster. Una instancia de app en cada uno de los tres nodos trabajadores permite que se produzca el tiempo de inactividad de un nodo trabajador sin que se interrumpa el uso de la app. Puede especificar el número de nodos trabajadores que desea incluir al crear un clúster desde la GUI de [{{site.data.keyword.Bluemix_notm}} o desde la ](cs_clusters.html#clusters_ui) [CLI](cs_clusters.html#clusters_cli). Kubernetes limita el número máximo de nodos trabajadores que puede tener en un clúster, así que debe tener en cuenta las [cuotas de pod y nodo trabajador ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/admin/cluster-large/).
    <pre class="codeblock"><code>bx cs cluster-create --location dal10 --workers 3 --public-vlan &lt;public_VLAN_ID&gt; --private-vlan &lt;private_VLAN_ID&gt; --machine-type &lt;u2c.2x4&gt; --name &lt;cluster_name_or_ID&gt;</code></pre></dd>
  <dt>Distribuya las apps entre clústeres</dt>
    <dd>Cree varios clústeres, cada uno con varios nodos trabajadores. Si se produce una interrupción en un clúster, los usuarios pueden acceder a una app que también está desplegada en otro clúster.
      <p>Clúster
1:</p>
        <pre class="codeblock"><code>bx cs cluster-create --location dal10 --workers 3 --public-vlan &lt;public_VLAN_ID&gt; --private-vlan &lt;private_VLAN_ID&gt; --machine-type &lt;u2c.2x4&gt; --name &lt;cluster_name_or_ID&gt;</code></pre>
      <p>Clúster
2:</p>
        <pre class="codeblock"><code>bx cs cluster-create --location dal12 --workers 3 --public-vlan &lt;public_VLAN_ID&gt; --private-vlan &lt;private_VLAN_ID&gt; --machine-type &lt;u2c.2x4&gt; --name &lt;cluster_name_or_ID&gt;</code></pre></dd>
  <dt>Distribuya las apps entre clústeres de distintas regiones</dt>
    <dd>Si distribuye las apps entre clústeres de distintas regiones, permite que se equilibre la carga
en función de la región en la que se encuentra el usuario. Si el clúster, el hardware, o incluso toda la ubicación de una región pasa a estar inactivo, el tráfico se redirecciona al contenedor desplegado en otro centro de datos.
      <p><strong>Importante:</strong> Después de configurar un dominio personalizado, puede utilizar estos mandatos para crear los clústeres.</p>
      <p>Ubicación
1:</p>
        <pre class="codeblock"><code>bx cs cluster-create --location dal10 --workers 3 --public-vlan &lt;public_VLAN_ID&gt; --private-vlan &lt;private_VLAN_ID&gt; --machine-type &lt;u2c.2x4&gt; --name &lt;cluster_name_or_ID&gt;</code></pre>
      <p>Ubicación 2:</p>
        <pre class="codeblock"><code>bx cs cluster-create --location ams03 --workers 3 --public-vlan &lt;public_VLAN_ID&gt; --private-vlan &lt;private_VLAN_ID&gt; --machine-type &lt;u2c.2x4&gt; --name &lt;cluster_name_or_ID&gt;</code></pre></dd>
</dl>

<br />







## Planificación de la configuración de nodos trabajadores
{: #planning_worker_nodes}

Un clúster de Kubernetes está formado por nodos trabajadores y está supervisado y gestionado de forma centralizada desde el maestro de Kubernetes. Los administradores del clúster deciden cómo configurar el clúster de nodos trabajadores para garantizar que los usuarios del clúster disponen de todos los recursos para desplegar y ejecutar las apps en el clúster.
{:shortdesc}

Cuando se crea un clúster estándar, los nodos trabajadores se ordenan en la infraestructura de IBM Cloud (SoftLayer) en su nombre y se añaden a la agrupación predeterminada de nodos trabajadores del clúster. A cada nodo trabajador se la asigna un ID exclusivo y un nombre de dominio que no se debe cambiar después de haber creado el clúster.

Puede elegir entre servidores virtuales o físicos (nativos). En función del nivel de aislamiento de hardware que elija, los nodos trabajadores virtuales se pueden configurar como nodos compartidos o dedicados. También puede elegir si desea que los nodos trabajadores se conecten a una VLAN pública y a una VLAN privada, o sólo a una VLAN privada. Cada nodo trabajador se suministra con un tipo de máquina específico que determina el número de vCPU, la memoria y el espacio en disco que están disponibles para los contenedores que se despliegan en el nodo trabajador. Kubernetes limita el número máximo de nodos trabajadores que puede tener en un clúster. Consulte el apartado sobre [nodo trabajador y cuotas de pod ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/admin/cluster-large/) para obtener más información.



### Hardware de los nodos trabajadores
{: #shared_dedicated_node}

Cuando se crea un clúster estándar en {{site.data.keyword.Bluemix_notm}}, se selecciona entre suministrar los nodos trabajadores como máquinas físicas (nativas) o como máquinas virtuales que se ejecutan en hardware físico. Cuando se crea un clúster gratuito, el nodo trabajador se suministra automáticamente como nodo compartido virtual en la cuenta de infraestructura de IBM Cloud (SoftLayer).
{:shortdesc}

![Opciones de hardware para los nodos trabajadores en un clúster estándar](images/cs_clusters_hardware.png)

Revise la información siguiente para decidir qué tipo de agrupaciones de trabajadores desea. A medida que planifique, considere el [umbral mínimo de límite de memoria para los nodos trabajadores](#resource_limit_node) del 10% de la capacidad total de memoria.

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


Puede desplegar clústeres mediante la [interfaz de usuario de la consola](#clusters_ui) o la [CLI](#clusters_cli).

### Conexión VLAN para nodos trabajadores
{: #worker_vlan_connection}

Cuando crea un clúster, cada clúster se conecta automáticamente a una VLAN de su cuenta de infraestructura de IBM Cloud (SoftLayer).
{:shortdesc}

Una VLAN configura un grupo de nodos trabajadores y pods como si estuvieran conectadas a la misma conexión física.
* La VLAN pública se suministra de forma automática con dos subredes. La subred pública primaria determina la dirección IP pública que se asigna a un nodo trabajador durante la creación del clúster y la subred pública portátil proporciona direcciones IP pública para los servicios de red de equilibrador de carga e Ingress.
* La VLAN privada también se suministra de forma automática con dos subredes. La subred privada primaria determina la dirección IP privada que se asigna a un nodo trabajador durante la creación del clúster y la subred privada portátil proporciona direcciones IP privada para los servicios de red de equilibrador de carga e Ingress.

Para los clústeres gratuitos, los nodos trabajadores del clúster se conectan de forma predeterminada a una VLAN pública y VLAN privada propiedad de IBM durante la creación del clúster.

Con clústeres estándares, la primera vez que crea un clúster en una ubicación, se suministra automáticamente con una VLAN pública y una VLAN privada. Para cada clúster posterior que cree en la ubicación, deberá elegir las VLAN que desee utilizar. Puede conectar los nodos trabajadores tanto a una VLAN pública y a la VLAN privada, o solo a la VLAN privada. Si desea conectar sus nodos trabajadores únicamente a una VLAN privada, utilice el ID de una VLAN privada existente o [cree una VLAN privada](/docs/cli/reference/softlayer/index.html#sl_vlan_create) y utilice el ID durante la creación del clúster. Si los nodos trabajadores únicamente se configuran con una VLAN privada, debe configurar una solución alternativa para la conectividad de red como, por ejemplo con un [Virtual Router Appliance](cs_vpn.html#vyatta), de forma que los nodos trabajadores se puedan comunicar con el maestro.

**Nota**: Si tiene varias VLAN para un clúster o varias subredes en la misma VLAN, debe activar la expansión de VLAN para que los nodos trabajadores puedan comunicarse entre sí en la red privada. Para obtener instrucciones, consulte [Habilitar o inhabilitar la expansión de VLAN](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning).

### Límites de memoria de nodos trabajadores
{: #resource_limit_node}

{{site.data.keyword.containershort_notm}} establece un límite de memoria en cada nodo trabajador. Cuando los pods que se ejecutan en el nodo trabajador superan este límite de memoria, se eliminan los pods. En Kubernetes, este límite se llama [umbral de desalojo de hardware ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/administer-cluster/out-of-resource/#hard-eviction-thresholds).
{:shortdesc}

Si los pods se eliminan con frecuencia, añada más nodos trabajadores al clúster o establezca [límites de recurso ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#resource-requests-and-limits-of-pod-and-container) a los pods.

**Cada máquina tiene un umbral mínimo que equivale a 10% de su capacidad total de memoria**. Cuando hay menos memoria disponible en el nodo trabajador que el umbral mínimo permitido, Kubernetes elimina inmediatamente el pod. El pod vuelve a planificar el nodo trabajador si hay uno disponible. Por ejemplo, si tiene una máquina virtual `b2c.4x16`, su capacidad total de memoria es de 16GB. Si hay menos de 1600MB (10%) de memoria disponible, no se pueden planificar nuevos pods en este nodo trabajador de forma que se planificarán en otro nodo trabajador. Si no hay más nodos trabajadores disponibles, los nuevos pods quedarán sin planificar.

Para revisar cuánta memoria se utiliza en el nodo trabajador, ejecute [kubectl top node ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/reference/kubectl/overview/#top).

### Recuperación automática para los nodos trabajadores
`Docker`, `kubelet`, `kube-proxy` y `calico` con componentes críticos que deben ser funcionales para tener un nodo trabajador de Kubernetes en buen estado. Con el tiempo, estos componentes se pueden estropear dejando así el nodo trabajador en estado fuera de servicio. Los nodos trabajadores averiados reducen la capacidad total del clúster y pueden provocar tiempo de inactividad en la app.

Puede [configurar comprobaciones de estado del nodo de trabajador y habilitar la recuperación automática](cs_health.html#autorecovery). Si la recuperación automática detecta un nodo trabajador erróneo basado en las comprobaciones configuradas, desencadena una acción correctiva, como una recarga del sistema operativo, en el nodo trabajador. Para obtener más información sobre cómo funciona la recuperación automática, consulte el [blog sobre recuperación automática ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/blogs/bluemix/2017/12/autorecovery-utilizes-consistent-hashing-high-availability/).

<br />



## Creación de clústeres con la GUI
{: #clusters_ui}

La finalidad del clúster de Kubernetes es definir un conjunto de recursos, nodos, redes y dispositivos de almacenamiento que mantengan la alta disponibilidad de las apps. Para poder desplegar una app, debe crear un clúster y establecer las definiciones de los nodos trabajadores en dicho clúster.
{:shortdesc}





Antes de empezar, debe poseer una [cuenta de {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/registration/) de pago según uso o de suscripción configurada para el [acceso al portafolio (SoftlLayer) de la infraestructura de IBM Cloud](cs_troubleshoot_clusters.html#cs_credentials). Para probar algunas de las funcionalidades, puede crear un clúster gratuito que caducará después de 21 días. Puede disponer como máximo de un clúster gratuito de forma simultánea.

Puede eliminar su clúster gratuito siempre que lo desee. Tenga en cuenta que después de 21 días los clústeres gratuitos y su contenido se suprimen sin la posibilidad de restaurarlos. Asegúrese de hacer una copia de seguridad de los datos.
{: tip}

Para personalizar completamente su clúster y añadir, entre otras características, la versión de API, la zona o el aislamiento de hardware, cree un clúster estándar.

Para crear un clúster:

1. En el catálogo, seleccione **Clúster de Kubernetes**.

2. Seleccione una región en la que desea desplegar el clúster.

3. Seleccione un tipo de plan de clúster. Puede elegir **Gratuito** o **Estándar**. Con un clúster estándar, obtiene acceso a características como, por ejemplo, varios nodos trabajadores para obtener un entorno de alta disponibilidad.

4. Configure los detalles del clúster. Complete los pasos que se aplican al tipo de clúster que está creando.

    1. **Gratuito y Estándar**: Asigne un nombre al clúster. El nombre debe empezar por una letra, puede contener letras, números, guiones (-) y debe tener 35 caracteres como máximo. El nombre del clúster y la región en la que el clúster se despliega forman el nombre de dominio completo para el subdominio de Ingress. Para garantizar que el subdominio de Ingress es exclusivo dentro de una región, el nombre del clúster podría ser truncado y añadírsele un valor aleatorio dentro del nombre de dominio de Ingress.
 
    2. **Estándar**: Seleccione una ubicación en la que desplegar su clúster. Para obtener el mejor rendimiento, seleccione la ubicación que esté físicamente más cercana. Tenga en cuenta que es posible que se requiera autorización legal para poder almacenar datos físicamente en un país extranjero si selecciona una ubicación que esté fuera del país.

    3. **Estándar**: Elija la versión del servidor de API de Kubernetes para el nodo maestro del clúster.

    4. **Estándar**: Seleccione un tipo de aislamiento de hardware. La opción virtual se factura por hora, y la nativa se factura mensualmente.

        - **Virtual - Dedicado**: Los nodos trabajadores están alojados en una infraestructura que está dedicada a su cuenta. Sus recursos físicos están completamente aislados.

        - **Virtual - Compartido**: Los recursos de infraestructura, como por ejemplo el hipervisor y el hardware físico, están compartidos entre usted y otros clientes de IBM, pero cada nodo trabajador es accesible sólo por usted. Aunque esta opción es menos costosa y suficiente en la mayoría de los casos, es posible que desee verificar los requisitos de rendimiento e infraestructura con las políticas de la empresa.

        - **Nativo**: Los servidores nativos se facturan de forma mensual y su suministro se realiza mediante interacción manual con la infraestructura de IBM Cloud (SoftLayer), por lo que puede tardar más de un día laborable en realizarse. Los servidores nativos son más apropiados para aplicaciones de alto rendimiento que necesitan más recursos y control de host.

        Asegúrese de que desea suministrar una máquina nativa. Puesto que se factura mensualmente, si cancela la operación inmediatamente tras realizar un pedido por error, se le cobrará el mes completo.
        {:tip}

    5.  **Estándar**: Seleccione un tipo de máquina. El tipo de máquina define la cantidad de memoria, espacio de disco y CPU virtual que se configura en cada nodo trabajador y que está disponible para todos los contenedores. Los tipos de máquinas nativas y virtuales varían según la ubicación en la que se despliega el clúster. Después de crear el clúster, puede añadir distintos tipos de máquina añadiendo un nodo al clúster.

    6. **Estándar**: Especifique el número de nodos trabajadores que necesita en el clúster.

    7. **Estándar**: Seleccione una VLAN pública (opcional) y una VLAN privada (necesario) en la cuenta de infraestructura de IBM Cloud (SoftLayer). Ambas VLAN comunican entre nodos trabajadores, pero la VLAN pública también se comunica con el maestro de Kubernetes gestionado por IBM. Puede utilizar la misma VLAN para varios clústeres.
        **Nota**: Si los nodos trabajadores únicamente se configuran con una VLAN privada, debe configurar una solución alternativa para la conectividad de red. Para obtener más información, consulte [Conexión de VLAN para nodos trabajadores](cs_clusters.html#worker_vlan_connection).

    8. De forma predeterminada, **Cifrar disco local** está seleccionado. Si elige desmarcar el recuadro de selección, no se cifran los datos de Docker del host.


4. Pulse **Crear clúster**. Verá el progreso del despliegue del nodo trabajador en el separador **Nodos trabajadores**. Cuando finalice el despliegue, podrá ver que el clúster está listo en el separador **Visión general**.
    **Nota:** A cada nodo trabajador se la asigna un ID exclusivo y un nombre de dominio que no se debe cambiar de forma manual después de haber creado el clúster. Si se cambia el nombre de dominio o el ID se impide que el maestro de Kubernetes gestione el clúster.



**¿Qué es lo siguiente?**

Cuando el clúster esté activo y en ejecución, puede realizar las siguientes tareas:


-   [Instalar las CLI para empezar a trabajar con el clúster.](cs_cli_install.html#cs_cli_install)
-   [Desplegar una app en el clúster.](cs_app.html#app_cli)
-   [Configure su propio registro privado en {{site.data.keyword.Bluemix_notm}} para almacenar y compartir imágenes de Docker con otros usuarios. ](/docs/services/Registry/index.html)
- Si tiene varias VLAN para un clúster o varias subredes en la misma VLAN, debe [activar la expansión de VLAN](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning) para que los nodos trabajadores puedan comunicarse entre sí en la red privada.
- Si tiene un cortafuegos, es posible que tenga que [abrir los puertos necesarios](cs_firewall.html#firewall) para utilizar los mandatos `bx`, `kubectl` o `calicotl`, para permitir el tráfico de salida desde el clúster o para permitir el tráfico de entrada para los servicios de red.

<br />


## Creación de clústeres con la CLI
{: #clusters_cli}

La finalidad del clúster de Kubernetes es definir un conjunto de recursos, nodos, redes y dispositivos de almacenamiento que mantengan la alta disponibilidad de las apps. Para poder desplegar una app, debe crear un clúster y establecer las definiciones de los nodos trabajadores en dicho clúster.
{:shortdesc}

Antes de empezar:
- Debe poseer una [cuenta de {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/registration/) de pago según uso o de suscripción configurada para el [acceso al portafolio (SoftlLayer) de la infraestructura de IBM Cloud](cs_troubleshoot_clusters.html#cs_credentials). Puede crear un clúster gratuito para probar algunas de las funcionalidades durante 21 días, o crear clústeres estándares totalmente personalizables con el aislamiento de hardware que elija.
- [Asegúrese de que tiene los permisos necesarios mínimos en la infraestructura de IBM Cloud (SoftLayer) para suministrar un clúster estándar](cs_users.html#infra_access).

Para crear un clúster:

1.  Instale la CLI de {{site.data.keyword.Bluemix_notm}} y el plug-in de [{{site.data.keyword.containershort_notm}}](cs_cli_install.html#cs_cli_install).

2.  Inicie la sesión en la CLI de {{site.data.keyword.Bluemix_notm}}. Escriba sus credenciales de {{site.data.keyword.Bluemix_notm}} cuando se le solicite.

    ```
    bx login
    ```
    {: pre}

    **Nota:** Si tiene un ID federado, utilice `bx login --sso` para iniciar la sesión en la CLI de {{site.data.keyword.Bluemix_notm}}. Especifique su nombre de usuario y utilice el URL proporcionado en la salida de la CLI para recuperar el código de acceso de un solo uso. Sabe tiene un ID federado cuando el inicio de sesión falla sin el `--sso` y se lleva a cabo correctamente con la opción `--sso`.

3. Si tiene varias cuentas de {{site.data.keyword.Bluemix_notm}}, seleccione la cuenta donde desea crear el clúster de Kubernetes.

4.  Si desea crear o acceder a clústeres de Kubernetes en una región distinta de la región de {{site.data.keyword.Bluemix_notm}} seleccionada anteriormente, ejecute `bx cs region-set`.

6.  Cree un clúster.

    1.  **Clústeres estándares**: Revise las ubicaciones que están disponibles. Las ubicaciones que se muestran dependen de la región de {{site.data.keyword.containershort_notm}} en la que ha iniciado la sesión.

        ```
        bx cs locations
        ```
        {: pre}
        
        La salida de la CLI coincide con las [ubicaciones correspondientes a la región de {{site.data.keyword.containerlong}}](cs_regions.html#locations).
        
    2.  **Clústeres estándares**: Elija una ubicación y revise los tipos de máquinas disponibles en dicha ubicación. El tipo de máquina especifica los hosts de cálculo físicos o virtuales que están disponibles para cada nodo trabajador.

        -  Consulte el campo **Tipo de servidor** para elegir máquinas virtuales o físicas (nativas).
        -  **Virtual**: Las máquinas virtuales se facturan por horas y se suministran en hardware compartido o dedicado.
        -  **Físico**: Los servidores nativos se facturan de forma mensual y su suministro se realiza mediante interacción manual con la infraestructura de IBM Cloud (SoftLayer), por lo que puede tardar más de un día laborable en realizarse. Los servidores nativos son más apropiados para aplicaciones de alto rendimiento que necesitan más recursos y control de host.
        - **Máquinas físicas con Trusted Compute**: Para clústeres nativos que ejecutan la versión 1.9 o posterior de Kubernetes, también puede optar por habilitar [Trusted Compute](cs_secure.html#trusted_compute) para verificar que los nodos trabajadores nativos no se manipulan de forma indebida. Trusted Compute está disponible para determinados tipos de máquinas nativas. Por ejemplo, los distintos tipos de GPU `mgXc` no dan soporte a Trusted Compute. Si no habilita la confianza durante la creación del clúster pero desea hacerlo posteriormente, puede utilizar el [mandato](cs_cli_reference.html#cs_cluster_feature_enable) `bx cs feature-enable`. Una vez que habilita la confianza, no puede inhabilitarla posteriormente.
        -  **Tipos de máquina**: Para decidir qué tipo de máquina desplegar, revise las combinaciones de núcleo, memoria y almacenamiento o consulte la [documentación del mandato `bx cs machine-types`](cs_cli_reference.html#cs_machine_types). Después de crear el clúster, puede añadir distintos tipos de máquina física o virtual mediante el [mandato](cs_cli_reference.html#cs_worker_add) `bx cs worker-add`.

           Asegúrese de que desea suministrar una máquina nativa. Puesto que se factura mensualmente, si cancela la operación inmediatamente tras realizar un pedido por error, se le cobrará el mes completo.
           {:tip}

        ```
        bx cs machine-types <location>
        ```
        {: pre}

    3.  **Clústeres estándares**: Compruebe si ya existe una VLAN pública y privada en la infraestructura de IBM Cloud (SoftLayer) para esta cuenta.

        ```
        bx cs vlans <location>
        ```
        {: pre}

        ```
        ID        Name                Number   Type      Router
        1519999   vlan   1355     private   bcr02a.dal10
        1519898   vlan   1357     private   bcr02a.dal10
        1518787   vlan   1252     public   fcr02a.dal10
        1518888   vlan   1254     public    fcr02a.dal10
        ```
        {: screen}

        Si ya existen una VLAN pública y privada, anote los direccionadores correspondientes. Los direccionadores de VLAN privadas siempre
empiezan por <code>bcr</code> (back-end router, direccionador de fondo) y los direccionadores de VLAN públicas siempre
empiezan por <code>fcr</code> (direccionador frontal). Al crear un clúster especificando las VLAN privadas y públicas, deben coincidir el número y la combinación de letras después de dichos prefijos. En la salida de ejemplo, se puede utilizar cualquier VLAN privada con cualquier VLAN pública porque todos los direccionadores incluyen `02a.dal10`.

        Los nodos trabajadores los debe conectar a una VLAN privada y, opcionalmente, puede conectarlos a una VLAN pública. **Nota**: Si los nodos trabajadores únicamente se configuran con una VLAN privada, debe configurar una solución alternativa para la conectividad de red. Para obtener más información, consulte [Conexión de VLAN para nodos trabajadores](cs_clusters.html#worker_vlan_connection).

    4.  **Clústeres estándares y gratuitos**: Ejecute el mandato `cluster-create`. Puede elegir un clúster gratuito, que incluye un nodo trabajador configurado con 2 vCPU y 4 GB de memoria que se suprime de forma automática después de 21 días. Cuando se crea un clúster estándar, de forma predeterminada, los discos del nodo trabajador están cifrados, su hardware se comparte entre varios clientes de IBM y se factura por horas de uso. </br>Ejemplo para un clúster estándar. Especifique las opciones del clúster:

        ```
        bx cs cluster-create --location dal10 --machine-type u2c.2x4 --hardware <shared_or_dedicated> --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID> --workers 3 --name <cluster_name> --kube-version <major.minor.patch> [--disable-disk-encrypt][--trusted]
        ```
        {: pre}

        Ejemplo para un clúster gratuito. Especifique el nombre del clúster:

        ```
        bx cs cluster-create --name my_cluster
        ```
        {: pre}

        <table>
        <caption>Componentes de cluster-create</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Descripción de los componentes de este mandato</th>
        </thead>
        <tbody>
        <tr>
        <td><code>cluster-create</code></td>
        <td>El mandato para crear un clúster en la organización de {{site.data.keyword.Bluemix_notm}}.</td>
        </tr>
        <tr>
        <td><code>--location <em>&lt;location&gt;</em></code></td>
        <td>**Clústeres estándares**: Sustituya <em>&lt;location&gt;</em> por el ID de ubicación de {{site.data.keyword.Bluemix_notm}} donde desea crear el clúster. Las [ubicaciones disponibles](cs_regions.html#locations) dependen de la región de {{site.data.keyword.containershort_notm}} en la que ha iniciado la sesión.</td>
        </tr>
        <tr>
        <td><code>--machine-type <em>&lt;machine_type&gt;</em></code></td>
        <td>**Clústeres estándares**: Elija un tipo de máquina. Puede desplegar los nodos trabajadores como máquinas virtuales en hardware dedicado o compartido, o como máquinas físicas en servidores nativos. Los tipos de máquinas físicas y virtuales varían según la ubicación en la que se despliega el clúster. Para obtener más información, consulte la documentación del [mandato](cs_cli_reference.html#cs_machine_types) `bx cs machine-type`. Para los clústeres gratuitos, no tiene que definir el tipo de máquina.</td>
        </tr>
        <tr>
        <td><code>--hardware <em>&lt;shared_or_dedicated&gt;</em></code></td>
        <td>**Clústeres estándares, sólo virtuales**: El nivel de aislamiento del hardware del nodo trabajador. Utilice el valor dedicated para tener recursos físicos disponibles dedicados solo a usted, o shared para permitir que los recursos físicos se compartan con otros clientes de IBM. El valor predeterminado es shared. Este valor es opcional para clústeres estándares y no está disponible para clústeres gratuitos.</td>
        </tr>
        <tr>
        <td><code>--public-vlan <em>&lt;public_vlan_id&gt;</em></code></td>
        <td><ul>
          <li>**Clústeres gratuitos**: No tiene que definir una VLAN pública. El clúster gratuito se conecta automáticamente a una VLAN pública propiedad de IBM.</li>
          <li>**Clústeres estándares**: Si ya tiene una VLAN pública configurada en su cuenta de infraestructura de IBM Cloud (SoftLayer) para esta ubicación, escriba el ID de la VLAN pública. Si desea conectar los nodos trabajadores solo a una VLAN privada, no especifique esta opción. **Nota**: Si los nodos trabajadores únicamente se configuran con una VLAN privada, debe configurar una solución alternativa para la conectividad de red. Para obtener más información, consulte [Conexión de VLAN para nodos trabajadores](cs_clusters.html#worker_vlan_connection).<br/><br/>
          <strong>Nota</strong>: Los direccionadores de VLAN privadas siempre empiezan por <code>bcr</code> (back-end router, direccionador de fondo) y los direccionadores de VLAN públicas siempre empiezan por <code>fcr</code> (direccionador frontal). Al crear un clúster especificando las VLAN privadas y públicas, deben coincidir el número y la combinación de letras después de dichos prefijos.</li>
        </ul></td>
        </tr>
        <tr>
        <td><code>--private-vlan <em>&lt;private_vlan_id&gt;</em></code></td>
        <td><ul><li>**Clústeres gratuitos**: No tiene que definir una VLAN privada. El clúster gratuito se conecta automáticamente a una VLAN privada propiedad de IBM.</li><li>**Clústeres estándares**: Si ya tiene una VLAN privada configurada en su cuenta de infraestructura de IBM Cloud (SoftLayer) para esta ubicación, escriba el ID de la VLAN privada. Si no tiene una VLAN privada en la ubicación, no especifique esta opción. {{site.data.keyword.containershort_notm}} crea automáticamente una VLAN privada.<br/><br/><strong>Nota</strong>: Los direccionadores de VLAN privadas siempre empiezan por <code>bcr</code> (back-end router, direccionador de fondo) y los direccionadores de VLAN públicas siempre empiezan por <code>fcr</code> (direccionador frontal). Al crear un clúster especificando las VLAN privadas y públicas, deben coincidir el número y la combinación de letras después de dichos prefijos.</li></ul></td>
        </tr>
        <tr>
        <td><code>--name <em>&lt;name&gt;</em></code></td>
        <td>**Clústeres estándares y gratuitos**: Sustituya <em>&lt;name&gt;</em> por el nombre del clúster. El nombre debe empezar por una letra, puede contener letras, números, guiones (-) y debe tener 35 caracteres como máximo. El nombre del clúster y la región en la que el clúster se despliega forman el nombre de dominio completo para el subdominio de Ingress. Para garantizar que el subdominio de Ingress es exclusivo dentro de una región, el nombre del clúster podría ser truncado y añadírsele un valor aleatorio dentro del nombre de dominio de Ingress.
</td>
        </tr>
        <tr>
        <td><code>--workers <em>&lt;number&gt;</em></code></td>
        <td>**Clústeres estándares**: El número de nodos trabajadores que desea incluir en el clúster. Si no se especifica la opción <code>--workers</code>, se crea 1 nodo trabajador.</td>
        </tr>
        <tr>
          <td><code>--kube-version <em>&lt;major.minor.patch&gt;</em></code></td>
          <td>**Clústeres estándares**: La versión de Kubernetes del nodo maestro del clúster. Este valor es opcional. Cuando no se especifica la versión, el clúster se crea con el valor predeterminado de las versiones de Kubernetes soportadas. Para ver todas las versiones disponibles, ejecute <code>bx cs kube-versions</code>.
</td>
        </tr>
        <tr>
        <td><code>--disable-disk-encrypt</code></td>
        <td>**Clústeres estándares y gratuitos**: Los nodos trabajadores tienen cifrado de disco de forma predeterminada; [más información](cs_secure.html#encrypted_disks). Si desea inhabilitar el cifrado, incluya esta opción.</td>
        </tr>
        <tr>
        <td><code>--trusted</code></td>
        <td>**Clústeres nativos estándares**: Habilite [Trusted Compute](cs_secure.html#trusted_compute) para verificar que los nodos trabajadores nativos no se manipulan de forma indebida. Trusted Compute está disponible para determinados tipos de máquinas nativas. Por ejemplo, los distintos tipos de GPU `mgXc` no dan soporte a Trusted Compute. Si no habilita la confianza durante la creación del clúster pero desea hacerlo posteriormente, puede utilizar el [mandato](cs_cli_reference.html#cs_cluster_feature_enable) `bx cs feature-enable`. Una vez que habilita la confianza, no puede inhabilitarla posteriormente.</td>
        </tr>
        </tbody></table>

7.  Verifique que ha solicitado la creación del clúster.

    ```
    bx cs clusters
    ```
    {: pre}

    **Nota:** Para máquinas virtuales, se puede tardar varios minutos en pedir las máquinas de nodo trabajador y en configurar y suministrar el clúster en la cuenta. El suministro de las máquinas físicas nativas se realiza mediante interacción manual con la infraestructura de IBM Cloud (SoftLayer), por lo que puede tardar más de un día laborable en realizarse.

    Una vez completado el suministro del clúster, el estado del clúster pasa a ser **deployed**.

    ```
    Name         ID                                   State      Created          Workers   Location   Version
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   1         mil01      1.9.7
    ```
    {: screen}

8.  Compruebe el estado de los nodos trabajadores.

    ```
    bx cs workers <cluster_name_or_ID>
    ```
    {: pre}

    Cuando los nodos trabajadores están listos, el estado pasa a **normal** y el estado es **Ready**. Cuando el estado del nodo sea **Preparado**, podrá acceder al clúster.

    **Nota:** A cada nodo trabajador se la asigna un ID exclusivo y un nombre de dominio que no se debe cambiar de forma manual después de haber creado el clúster. Si se cambia el nombre de dominio o el ID se impide que el maestro de Kubernetes gestione el clúster.

    ```
    ID                                                 Public IP       Private IP      Machine Type   State    Status   Location   Version
    kube-mil01-paf97e8843e29941b49c598f516de72101-w1   169.xx.xxx.xxx  10.xxx.xx.xxx   free           normal   Ready    mil01      1.9.7
    ```
    {: screen}

9. Defina el clúster que ha creado como contexto para esta sesión. Siga estos pasos de configuración cada vez que de trabaje con el clúster.
    1.  Obtenga el mandato para establecer la variable de entorno y descargar los archivos de configuración de Kubernetes.

        ```
        bx cs cluster-config <cluster_name_or_ID>
        ```
        {: pre}

        Cuando termine la descarga de los archivos de configuración, se muestra un mandato que puede utilizar para establecer la vía de acceso al archivo de configuración de
Kubernetes como variable de entorno.

        Ejemplo para OS X:

        ```
        export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
        ```
        {: screen}

    2.  Copie y pegue el mandato que se muestra en el terminal para definir la variable de entorno `KUBECONFIG`.
    3.  Compruebe que la variable de entorno `KUBECONFIG` se haya establecido correctamente.

        Ejemplo para OS X:

        ```
        echo $KUBECONFIG
        ```
        {: pre}

        Salida:

        ```
        /Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml

        ```
        {: screen}

10. Inicie el panel de control de Kubernetes con el puerto predeterminado `8001`.
    1.  Establezca el proxy con el número de puerto predeterminado.

        ```
        kubectl proxy
        ```
        {: pre}

        ```
        Starting to serve on 127.0.0.1:8001
        ```
        {: screen}

    2.  Abra el siguiente URL en un navegador web para ver el panel de control de Kubernetes.

        ```
        http://localhost:8001/ui
        ```
        {: codeblock}


**¿Qué es lo siguiente?**


-   [Desplegar una app en el clúster.](cs_app.html#app_cli)
-   [Gestionar el clúster con la línea de mandatos de `kubectl`. ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/reference/kubectl/overview/)
-   [Configure su propio registro privado en {{site.data.keyword.Bluemix_notm}} para almacenar y compartir imágenes de Docker con otros usuarios. ](/docs/services/Registry/index.html)
- Si tiene varias VLAN para un clúster o varias subredes en la misma VLAN, debe [activar la expansión de VLAN](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning) para que los nodos trabajadores puedan comunicarse entre sí en la red privada.
- Si tiene un cortafuegos, es posible que tenga que [abrir los puertos necesarios](cs_firewall.html#firewall) para utilizar los mandatos `bx`, `kubectl` o `calicotl`, para permitir el tráfico de salida desde el clúster o para permitir el tráfico de entrada para los servicios de red.

<br />





## Visualización de estados de clúster
{: #states}

Revise el estado de un clúster de Kubernetes para obtener información sobre la disponibilidad y la capacidad del clúster, y posibles problemas que puedan haberse producido.
{:shortdesc}

Para ver información sobre un clúster específico, como sus ubicaciones, el URL maestro, el subdominio de Ingress, la versión, el propietario y el panel de control de supervisión, utilice el [mandato](cs_cli_reference.html#cs_cluster_get) `bx cs cluster-get <cluster_name_or_ID>`. Incluya el distintivo `--showResources` para ver más recursos de clúster, como complementos para pods de almacenamiento o VLAN de subred para IP públicas y privadas.

Puede ver el estado actual del clúster ejecutando el mandato `bx cs clusters` y localizando el campo **State**. Para resolver el clúster y los nodos de trabajador, consulte [Resolución de problemas de clústeres](cs_troubleshoot.html#debug_clusters).

<table summary="Cada fila de la tabla se debe leer de izquierda a derecha, con el estado del clúster en la columna uno y una descripción en la columna dos.">
<caption>Estados de clúster</caption>
   <thead>
   <th>Estado del clúster</th>
   <th>Descripción</th>
   </thead>
   <tbody>
<tr>
   <td>Terminado anormalmente</td>
   <td>El usuario ha solicitado la supresión del clúster antes de desplegar el maestro de Kubernetes. Una vez realizada la supresión del clúster, el clúster se elimina del panel de control. Si el clúster está bloqueado en este estado durante mucho tiempo, abra una [incidencia de soporte de {{site.data.keyword.Bluemix_notm}}](cs_troubleshoot.html#ts_getting_help).</td>
   </tr>
 <tr>
     <td>Crítico</td>
     <td>No se puede acceder al maestro de Kubernetes o todos los nodos trabajadores del clúster están inactivos. </td>
    </tr>
   <tr>
     <td>Error al suprimir</td>
     <td>No se puede suprimir el maestro de Kubernetes o al menos un nodo trabajador.  </td>
   </tr>
   <tr>
     <td>Suprimido</td>
     <td>El clúster se ha suprimido pero todavía no se ha eliminado del panel de control. Si el clúster está bloqueado en este estado durante mucho tiempo, abra una [incidencia de soporte de {{site.data.keyword.Bluemix_notm}}](cs_troubleshoot.html#ts_getting_help). </td>
   </tr>
   <tr>
   <td>Suprimiendo</td>
   <td>El clúster se está suprimiendo y la infraestructura del clúster se está desmontando. No puede acceder al clúster.  </td>
   </tr>
   <tr>
     <td>Error al desplegar</td>
     <td>El despliegue del maestro de Kubernetes no se ha podido realizar. No puede resolver este estado. Póngase en contacto con el soporte de IBM Cloud abriendo una [incidencia de soporte de {{site.data.keyword.Bluemix_notm}}](cs_troubleshoot.html#ts_getting_help).</td>
   </tr>
     <tr>
       <td>Despliegue</td>
       <td>El maestro de Kubernetes no está completamente desplegado. No puede acceder a su clúster. Espere hasta que el clúster se haya desplegado por completo para revisar el estado del clúster.</td>
      </tr>
      <tr>
       <td>Normal</td>
       <td>Todos los nodos trabajadores de un clúster están activos y en ejecución. Puede acceder al clúster y desplegar apps en el clúster. Este estado se considera correcto y no requiere ninguna acción por su parte. **Nota**: Aunque los nodos trabajadores podrían poseer un estado normal, otros recursos de infraestructura como, por ejemplo la [red](cs_troubleshoot_network.html) y el [almacenamiento](cs_troubleshoot_storage.html), podrían requerir su atención.</td>
    </tr>
      <tr>
       <td>Pendiente</td>
       <td>El maestro de Kubernetes está desplegado. Los nodos trabajadores se están suministrando y aún no están disponibles en el clúster. Puede acceder al clúster, pero no puede desplegar apps en el clúster.  </td>
     </tr>
   <tr>
     <td>Solicitado</td>
     <td>Se ha enviado una solicitud para crear el clúster y pedir la infraestructura para el maestro de Kubernetes y los nodos trabajadores. Cuando se inicia el despliegue del clúster, el estado del clúster cambia a <code>Desplegando</code>. Si el clúster está bloqueado en el estado <code>Solicitado</code> durante mucho tiempo, abra una [incidencia de soporte de {{site.data.keyword.Bluemix_notm}}](cs_troubleshoot.html#ts_getting_help). </td>
   </tr>
   <tr>
     <td>Actualizando</td>
     <td>El servidor de API de Kubernetes que se ejecuta en el maestro de Kubernetes está siendo actualizado a una versión nueva de API de Kubernetes. Durante la actualización, no puede acceder ni cambiar el clúster. Los nodos trabajadores, apps y recursos que el usuario despliega no se modifican y continúan en ejecución. Espere a que la actualización se complete para revisar el estado del clúster. </td>
   </tr>
    <tr>
       <td>Aviso</td>
       <td>Al menos un nodo trabajador del clúster no está disponible, pero los otros nodos trabajadores están disponibles y pueden asumir la carga de trabajo. </td>
    </tr>
   </tbody>
 </table>


<br />


## Eliminación de clústeres
{: #remove}

Los clústeres gratuitos y estándares que se crean con una cuenta Pago según uso se deben eliminar de forma manual cuando dejan de necesitarse para que dejen de consumir recursos.
{:shortdesc}

**Aviso:**
  - No se crean copias de seguridad del clúster ni de los datos del almacén permanente. La supresión de un clúster o del almacenamiento permanente es irreversible y no se puede deshacer.
  - Cuando se elimina un clúster, también se eliminan todas las subredes suministradas de forma automática al crear el clúster con el mandato `bx cs cluster-subnet-create`. Sin embargo, si ha añadido de forma manual subredes existentes al clúster con el mandato `bx cs cluster-subnet-add`, estas subredes no se eliminan de su cuenta (SoftLayer) de la infraestructura IBM Cloud y podrá reutilizarlas en otros clústeres.

Antes de empezar, anote su ID de clúster. Podría necesitar el ID de clúster para investigar y eliminar recursos (Softlayer) de infraestructura IBM Cloud que no se suprimen de forma automática al suprimir su clúster como, por ejemplo, almacenamiento persistente.
{: tip}

Para eliminar un clúster:

-   Desde la interfaz gráfica de usuario de {{site.data.keyword.Bluemix_notm}}
    1.  Seleccione el clúster y pulse **Suprimir** en el menú **Más acciones...**.

-   Desde la CLI de {{site.data.keyword.Bluemix_notm}}
    1.  Liste los clústeres disponibles.

        ```
        bx cs clusters
        ```
        {: pre}

    2.  Suprima el clúster.

        ```
        bx cs cluster-rm <cluster_name_or_ID>
        ```
        {: pre}

    3.  Siga las indicaciones y decidir si desea suprimir los recursos del clúster (contenedores, pods, servicios enlazados, almacenamiento o secretos).
      - **Almacenamiento persistente**: El almacenamiento persistente proporciona alta disponibilidad a los datos. Si ha creado una reclamación de volumen persistente utilizando una [compartición de archivo existente](cs_storage.html#existing), no puede suprimir la compartición de archivo al suprimir el clúster. Suprima manualmente la compartición de archivo desde el portafolio de infraestructura de IBM Cloud (SoftLayer).

          **Nota**: Debido al ciclo de facturación mensual, una reclamación de volumen permanente no se puede suprimir el último día del mes. Si suprime la reclamación de volumen permanente el último día del mes, la supresión permanece pendiente hasta el principio del siguiente mes.

Pasos siguientes:
- Después de que deje de aparecer en la lista de clústeres disponibles, al ejecutar el mandato `bx cs clusters`, podrá reutilizar el nombre de un clúster eliminado.
- Para mantener las subredes, puede [reutilizarlas en un nuevo clúster](cs_subnets.html#custom) o suprimirlas de forma manual más tarde de su portfolio (SoftLayer) de la infraestructura IBM Cloud.
- Si conserva el almacenamiento persistente, puede suprimirlo más tarde a través el panel de control (SoftLayer) de la infraestructura IBM Cloud en la interfaz gráfica de usuario de {{site.data.keyword.Bluemix_notm}}.
