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



# Planificación de red con servicios NodePort, LoadBalancer o Ingress
{: #planning}

Cuando crea un clúster de Kubernetes en {{site.data.keyword.containerlong}}, cada clúster debe estar conectado a una VLAN pública. La VLAN pública determina la dirección IP pública que se asigna a un nodo trabajador durante la creación del clúster.
{:shortdesc}

La interfaz de red pública para los nodos trabajadores tanto en clústeres gratuitos como estándares está protegida por las políticas de red de Calico. Estas políticas bloquean la mayor parte del tráfico de entrada de forma predeterminada. Sin embargo, se permite el tráfico de entrada que se necesita para que Kubernetes funcione, así como las conexiones con los servicios NodePort, LoadBalancer e Ingress. Para obtener más información sobre estas políticas, incluido cómo modificarlas, consulte [Políticas de red](cs_network_policy.html#network_policies).

|Tipo de clúster|Gestor de la VLAN pública del clúster|
|------------|------------------------------------------|
|Clústeres gratuitos|{{site.data.keyword.IBM_notm}}|
|Clústeres estándares|En la cuenta de infraestructura de IBM Cloud (SoftLayer)|
{: caption="Gestores de VLAN públicas según el tipo de clúster" caption-side="top"}

Para obtener más información sobre la comunicación de red segura entre nodos trabajadores y pods, consulte [Redes en clúster](cs_secure.html#in_cluster_network). Para obtener más información sobre la conexión segura de apps que se ejecutan en un clúster de Kubernetes a una red local o a otras apps externas al clúster, consulte [Configuración de la conectividad de VPN](cs_vpn.html).

## Cómo permitir el acceso público a apps
{: #public_access}

Para que una app esté disponible a nivel público en Internet, debe actualizar el archivo de configuración antes de desplegar la app en un clúster.
{:shortdesc}

*Plano de datos de Kubernetes en {{site.data.keyword.containershort_notm}}*

![{{site.data.keyword.containerlong_notm}} Arquitectura de Kubernetes](images/networking.png)

El diagrama muestra cómo Kubernetes lleva el tráfico de red de usuario en {{site.data.keyword.containershort_notm}}. Para asegurarse de que su app es accesible desde internet, puede hacerlo dependiendo si creó un clúster estándar o gratuito.

<dl>
<dt><a href="cs_nodeport.html#planning" target="_blank">Servicio NodePort</a> (clústeres gratuitos y estándares)</dt>
<dd>
 <ul>
  <li>Exponga un puerto público en cada nodo trabajador y utilice la dirección IP pública de cualquier nodo trabajador para acceder de forma pública al servicio en el clúster.</li>
  <li>Iptables es una característica de kernel de Linux que equilibra la carga de las solicitudes en los pods de la app y proporciona un direccionamiento de red de alto rendimiento y control de acceso de red.</li>
  <li>La dirección IP pública del nodo trabajador no es permanente. Cuando un nodo trabajador se elimina o se vuelve a crear, se le asigna una nueva dirección IP pública.</li>
  <li>El servicio NodePort es una muy buena opción para probar el acceso público. También se puede utilizar si se necesita acceso público para un breve periodo de tiempo únicamente.</li>
 </ul>
</dd>
<dt><a href="cs_loadbalancer.html#planning" target="_blank">Servicio LoadBalancer</a> (solo clústeres estándares)</dt>
<dd>
 <ul>
  <li>Cada clúster estándar se suministra con cuatro direcciones IP públicas portátiles y cuatro direcciones IP privadas portátiles que puede utilizar para crear un equilibrador de carga TCP/UDP externo para la app.</li>
  <li>Iptables es una característica de kernel de Linux que equilibra la carga de las solicitudes en los pods de la app y proporciona un direccionamiento de red de alto rendimiento y control de acceso de red.</li>
  <li>La dirección IP pública portátil asignada al equilibrador de carga es permanente y no cambia cuando un nodo trabajador se vuelve a crear en el clúster.</li>
  <li>Puede personalizar el equilibrador de carga exponiendo cualquier puerto que necesite la app.</li></ul>
</dd>
<dt><a href="cs_ingress.html#planning" target="_blank">Ingress</a> (solo clústeres estándares)</dt>
<dd>
 <ul>
  <li>Exponga varias apps en un clúster creando un equilibrador de carga HTTP o HTTPS, TCP o UDP externo. El equilibrador de carga utiliza un punto de entrada público único protegido para direccionar las solicitudes entrantes a sus apps.</li>
  <li>Puede utilizar una ruta pública para exponer varias apps en el clúster como servicios.</li>
  <li>Ingress consta de dos componentes:
   <ul>
    <li>El recurso de Ingress define las reglas sobre cómo direccionar y equilibrar la carga de las solicitudes de entrada para una app.</li>
    <li>El equilibrador de carga de aplicación (ALB) está a la escucha de solicitudes entrantes de servicio HTTP o HTTPS, TCP o UDP. Reenvía las solicitudes a través de los pods de las apps con base a las reglas que se definen en el recurso Ingress.</li>
   </ul>
  <li>Utilice Ingress para implementar su propio ALB con reglas de direccionamiento personalizadas y con terminación SSL si es necesario para sus apps.</li>
 </ul>
</dd></dl>

Para seleccionar la mejor opción de red para su aplicación, puede seguir este árbol de decisiones. Para obtener información de planificación e instrucciones de configuración, pulse la opción de servicio de red que elija.

<img usemap="#networking_map" border="0" class="image" src="images/networkingdt.png" width="500px" alt="Esta imagen le guía para elegir la mejor opción de red para la aplicación. Si esta imagen no se muestra, la información puede encontrarse en la documentación." style="width:500px;" />
<map name="networking_map" id="networking_map">
<area href="/docs/containers/cs_nodeport.html" alt="Servicio NodePort" shape="circle" coords="52, 283, 45"/>
<area href="/docs/containers/cs_loadbalancer.html" alt="servicio LoadBalancer" shape="circle" coords="247, 419, 44"/>
<area href="/docs/containers/cs_ingress.html" alt="Servicio Ingress" shape="circle" coords="445, 420, 45"/>
</map>
