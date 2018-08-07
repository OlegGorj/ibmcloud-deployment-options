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



# Asignación de acceso al clúster
{: #users}

Como administrador del clúster, puede definir políticas de acceso para el clúster de Kubernetes para crear distintos niveles de acceso para distintos usuarios. Por ejemplo, puede autorizar a determinados usuarios a trabajar con recursos del clúster mientras que a otros sólo desplegar contenedores.
{: shortdesc}


## Planificación para solicitudes de acceso
{: #planning_access}

Como administrador del clúster, puede ser difícil realizar un seguimiento de las solicitudes de acceso. Establecer un patrón de comunicación para las solicitudes de acceso es esencial para mantener la seguridad de su clúster.
{: shortdesc}

Para garantizar que las personas adecuadas tienen el acceso correcto, sea muy claro con las políticas de acceso o con la obtención de ayuda para las tareas más habituales.

Es posible que ya tenga un método que funciona para su equipo. Si está buscando un punto de partida, considere alguno de los siguientes métodos.

*  Crear un sistema de incidencias
*  Crear una plantilla de formulario
*  Crear una página de wiki
*  Requerir una solicitud de correo electrónico
*  Utilizar el sistema de seguimiento de problemas que ya utilice para realizar el seguimiento del trabajo diario del equipo

Si desea más información o resolver dudas, consulte esta guía de aprendizaje sobre las [prácticas recomendadas para organizar usuarios, equipos y aplicaciones](/docs/tutorials/users-teams-applications.html).
{: tip}

## Permisos y políticas de acceso
{: #access_policies}

El ámbito de una política de acceso se basa en un rol o roles definidos para los usuarios que determinan las acciones que pueden realizar. Puede establecer políticas que son específicas de su clúster, infraestructura, instancias de servicio o roles de Cloud Foundry.
{: shortdesc}

Debe definir políticas de acceso para cada usuario que trabaje con {{site.data.keyword.containershort_notm}}. Algunas políticas son predefinidas, otras se pueden personalizar. Consulte la siguiente imagen y sus definiciones para ver qué roles se relacionan con las tareas de los usuarios e identifique dónde podría querer personalizar una política.

![{{site.data.keyword.containershort_notm}} - Roles de acceso](/images/user-policies.png)

Figura. Roles de acceso de {{site.data.keyword.containershort_notm}}

<dl>
  <dt>Políticas de identidad y gestión de acceso (IAM (Identity and Access Management))</dt>
    <dd><p><strong>Plataforma</strong>: Puede determinar las acciones que las personas pueden realizar en un clúster de {{site.data.keyword.containershort_notm}}. Puede establecer estas políticas por región. Ejemplos de acciones: crear o eliminar clústeres o añadir nodos trabajadores adicionales. Estas políticas deben establecerse junto con las políticas de infraestructura.</p>
    <p><strong>Infraestructura</strong>: Puede determinar los niveles de acceso para su infraestructura como, por ejemplo, máquinas de nodo, red o recursos de almacenamiento de clúster. La misma política se aplica independientemente de si el usuario realiza una solicitud desde la GUI de {{site.data.keyword.containershort_notm}} o mediante la CLI, incluso cuando las acciones se completan en la infraestructura de IBM Cloud (SoftLayer). Debe establecer este tipo de política junto con las políticas de acceso de la plataforma {{site.data.keyword.containershort_notm}}. Para obtener más información sobre los roles disponibles, consulte [permisos de infraestructura](/docs/iam/infrastructureaccess.html#infrapermission).</p> </br></br><strong>Nota:</strong> Asegúrese de que su cuenta de {{site.data.keyword.Bluemix_notm}} está [configurada con un acceso al portafolio (SoftLayer) de la infraestructura de IBM Cloud](cs_troubleshoot_clusters.html#cs_credentials) de forma que los usuarios autorizados puedan realizar acciones en la cuenta (SoftLayer) de la infraestructura de IBM Cloud con base a los permisos asignados. </dd>
  <dt>Roles de control de acceso basado en recursos (RBAC (Resource Based Access Control)) de Kubernetes</dt>
    <dd>Cada usuario asignado a la política de acceso de la plataforma, se le asigna también de forma automática un rol de Kubernetes. En Kubernetes, el [Role Based Access Control (RBAC) ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview) determina las acciones que el usuario puede realizar en los recursos dentro de un clúster. <strong>Nota</strong>: Los roles RBAC se configuran automáticamente para el espacio de nombres <code>default</code>, sin embargo como administrador del clúster, puede asignar roles a otros espacios de nombres.</dd>
  <dt>Cloud Foundry</dt>
    <dd>No es posible gestionar todos los servicios con Cloud IAM. Si está utilizando uno de estos servicios, puede continuar utilizando los [roles de usuario de Cloud Foundry](/docs/iam/cfaccess.html#cfaccess) para controlar el acceso a los servicios.</dd>
</dl>


¿Degradación de permisos? Para completar la acción pueden ser necesarios unos minutos.
{: tip}

### Roles de plataforma
{: #platform_roles}

{{site.data.keyword.containershort_notm}} está configurado para utilizar roles de plataforma de {{site.data.keyword.Bluemix_notm}}. Los permisos de los roles se agregan entre sí, lo que significa que el rol de `Editor` tiene los mismos permisos que el rol de `Visor`, y además se le otorgan permisos para editar. En la siguiente tabla se explica los tipos de acciones que cada rol puede realizar.

Los correspondientes roles RBAC se asigna de forma automática al espacio de nombres predeterminado al asignar un rol de plataforma. Si cambia un rol de plataforma del usuario, también se actualiza el rol RBAC.
{: tip}

<table>
<caption>Acciones y roles de plataforma</caption>
  <tr>
    <th>Roles de plataforma</th>
    <th>Acciones de ejemplo</th>
    <th>Rol RBAC correspondiente</th>
  </tr>
  <tr>
      <td>Visor</td>
      <td>Visualiza los detalles de una instancia de clúster u otras instancias de servicio.</td>
      <td>Ver</td>
  </tr>
  <tr>
    <td>Editor</td>
    <td>Puede enlazar o desenlazar un servicio IBM Cloud a un clúster, o puede crear un webhook. <strong>Nota</strong>: Para enlazar servicios, también debe tener asignado el rol de Cloud Foundry de desarrollador.</td>
    <td>Editar</td>
  </tr>
  <tr>
    <td>Operador</td>
    <td>Puede crear, eliminar, rearrancar o recargar un nodo trabajador. Puede añadir una subred a un clúster.</td>
    <td>Administrador</td>
  </tr>
  <tr>
    <td>Administrador</td>
    <td>Puede crear y eliminar clústeres. Puede editar políticas de acceso para otros en el nivel de cuenta para el servicio y la infraestructura. <strong>Nota</strong>: El acceso de administrador se puede asignar a un clúster específico o a todas las instancias del servicio a través de la cuenta. Para suprimir clústeres, debe tener acceso de administrador al clúster que desea suprimir. Para crear clústeres debe tener el rol de administrador para todas las instancias del servicio.</td>
    <td>Administrador de clúster</td>
  </tr>
</table>

Para obtener más información sobre la asignación de roles de usuario en la IU, consulte [Gestión del acceso IAM](/docs/iam/mngiam.html#iammanidaccser).


### Roles de infraestructura
{: #infrastructure_roles}

La infraestructura de roles habilita a que los usuarios puedan realizar tareas en recursos a nivel de la infraestructura. En la siguiente tabla se explica los tipos de acciones que cada rol puede realizar. Los roles de la infraestructura son personalizables; asegúrese de dar acceso a los usuarios únicamente de lo que necesiten para hacer su trabajo.

Además de otorgar los roles de infraestructura específicos, también debe otorgar acceso a los usuarios a los dispositivos que funcionan con la infraestructura.
{: tip}

<table>
<caption>Acciones y roles de infraestructura</caption>
  <tr>
    <th>Rol de infraestructura</th>
    <th>Acciones de ejemplo</th>
  </tr>
  <tr>
    <td><i>Solo visualización</i></td>
    <td>Puede visualizar los detalles de la infraestructura y ver un resumen de la cuenta, incluidas facturas y pagos.</td>
  </tr>
  <tr>
    <td><i>Usuario básico</i></td>
    <td>Puede editar las configuraciones de servicio, incluidas las direcciones IP, añadir o editar registros DNS y añadir nuevos usuarios con acceso a la infraestructura.</td>
  </tr>
  <tr>
    <td><i>Superusuario</i></td>
    <td>Puede realizar todas las acciones relacionadas con la infraestructura.</td>
  </tr>
</table>

Para iniciar la asignación de roles, siga los pasos en [Personalización de permisos de infraestructura para un usuario](#infra_access).

### Roles de RBAC
{: #rbac_roles}

El control de acceso en recursos (RBAC) es una forma de proteger los recursos que están dentro del clúster y decidir qué usuarios realizan cada una de las acciones de Kubernetes. En la tabla siguiente se muestran los tipos de rol de RBAC y los tipos de acción que los usuarios pueden realizar con dicho rol. Los permisos se agregan entre sí, lo que significa que un `Administrador` también tiene todas las políticas de los roles de `Ver` y `Editar`. Asegúrese de que los usuarios sólo posean el acceso que necesitan.

Los roles RBAC se establecen automáticamente junto con el rol de plataforma para el espacio de nombres predeterminado. [Puede actualizar el rol, o asignar roles a otros espacios de nombres](#rbac).
{: tip}

<table>
<caption>Acciones y roles RBAC</caption>
  <tr>
    <th>Rol RBAC</th>
    <th>Acciones de ejemplo</th>
  </tr>
  <tr>
    <td>Ver</td>
    <td>Puede ver recursos dentro del espacio de nombres predeterminado. Los visores no pueden ver secretos de Kubernetes. </td>
  </tr>
  <tr>
    <td>Editar</td>
    <td>Puede leer y escribir recursos dentro del espacio de nombres predeterminado.</td>
  </tr>
  <tr>
    <td>Administrador</td>
    <td>Puede leer y escribir recursos dentro del espacio de nombres predeterminado pero no el espacio de nombres en sí mismo. Puede crear roles dentro de un espacio de nombres.</td>
  </tr>
  <tr>
    <td>Administrador de clúster</td>
    <td>Puede leer y escribir recursos en cada espacio de nombres. Puede crear roles dentro de un espacio de nombres. Puede acceder al panel de control de Kubernetes. Puede crear un recurso de Ingress y hacer públicas las apps.</td>
  </tr>
</table>

<br />


## Adición de usuarios a una cuenta de {{site.data.keyword.Bluemix_notm}}
{: #add_users}

Puede añadir usuarios a una cuenta de {{site.data.keyword.Bluemix_notm}} para otorgarles acceso a sus clústeres.
{:shortdesc}

Antes de empezar, verifique que tiene asignado el rol de `Gestor` de Cloud Foundry para una cuenta de {{site.data.keyword.Bluemix_notm}}.

1.  [Añada el usuario a la cuenta](../iam/iamuserinv.html#iamuserinv).
2.  En la sección **Acceso**, expanda **Servicios**.
3.  Asigne un rol de plataforma a un usuario para establecer el acceso para {{site.data.keyword.containershort_notm}}.
      1. En la lista desplegable **Servicios**, seleccione **{{site.data.keyword.containershort_notm}}**.
      2. En la lista desplegable **Región**, seleccione la zona a la que va a invitar al usuario.
      3. En la lista desplegable **Instancia de servicio**, seleccione el clúster al que va a invitar al usuario. Para encontrar el ID de un clúster específico, ejecute ` bx cs clusters`.
      4. En la sección **Seleccionar roles**, seleccione un rol. Para ver una lista de las acciones admitidas por rol, consulte [Permisos y políticas de acceso](#access_policies).
4. [Opcional: asigne un rol de Cloud Foundry](/docs/iam/mngcf.html#mngcf).
5. [Opcional: asigne un rol de infraestructura](/docs/iam/infrastructureaccess.html#infrapermission).
6. Pulse **Invitar a usuarios**.

<br />


## Descripción de la clave de API IAM y del mandato `bx cs credentials-set`
{: #api_key}

Para suministrar correctamente clústeres en la cuenta y trabajar con ellos, debe asegurarse de que la cuenta esté correctamente configurada para acceder al portafolio de infraestructura de IBM Cloud (SoftLayer). En función de la configuración de la cuenta, puede utilizar la clave de API IAM o las credenciales de infraestructura que establece manualmente mediante el mandato `bx cs credentials-set`.

<dl>
  <dt>Clave de API IAM</dt>
    <dd><p>La clave de API de Identity and Access Management (IAM) se establece automáticamente para una región cuando se realiza la primera acción que requiere la política de acceso de administrador de {{site.data.keyword.containershort_notm}}. Por ejemplo, uno de los usuarios administrativos crea el primer clúster en la región <code>us-south</code>. De ese modo, la clave de API IAM para este usuario se almacena en la cuenta para esta región. La clave de API se utiliza para pedir infraestructura de IBM Cloud (SoftLayer), como nodos trabajadores nuevos o VLAN.</p> <p>Cuando un usuario distinto realiza una acción en esta región que requiere interacción con el portafolio de infraestructura de IBM Cloud (SoftLayer), como crear un nuevo clúster o recargar un nodo trabajador, la clave de API almacenada se utiliza para determinar si existen suficientes permisos para realizar esa acción. Para asegurarse de que las acciones relacionadas con la infraestructura en el clúster se realicen correctamente, puede asignar a los usuarios administradores de {{site.data.keyword.containershort_notm}} la política de acceso a infraestructura <strong>Superusuario</strong>.</p>
    <p>Puede encontrar el propietario de clave de API actual ejecutando [<code>bx cs api-key-info</code>](cs_cli_reference.html#cs_api_key_info). Si necesita actualizar la clave de API que hay almacenada para una región, puede hacerlo mediante la ejecución del mandato [<code>bx cs api-key-reset</code>](cs_cli_reference.html#cs_api_key_reset). Este mandato requiere la política de acceso de administrador de {{site.data.keyword.containershort_notm}} y almacena la clave de API del usuario que ejecuta este mandato en la cuenta.</p>
    <p><strong>Nota:</strong> La clave de API que se almacena para la región no puede utilizarse si las credenciales de infraestructura de IBM Cloud (SoftLayer) se han establecido manualmente mediante el mandato <code>bx cs credentials-set</code>.</p></dd>
  <dt>Credenciales de infraestructura de IBM Cloud (SoftLayer) mediante <code>bx cs credentials-set</code></dt>
    <dd><p>Si tiene una cuenta de pago según uso de {{site.data.keyword.Bluemix_notm}}, tiene acceso al portafolio de infraestructura de IBM Cloud (de SoftLayer) de forma predeterminada. Sin embargo, es posible que desee utilizar otra cuenta de infraestructura de IBM Cloud (SoftLayer) que ya tenga para solicitar infraestructura. Puede enlazar esta cuenta de infraestructura a la cuenta de {{site.data.keyword.Bluemix_notm}} mediante el mandato [<code>bx cs credentials-set</code>](cs_cli_reference.html#cs_credentials_set).</p>
    <p>Si las credenciales de infraestructura de IBM Cloud (SoftLayer) se establecen manualmente, estas credenciales se utilizan para pedir infraestructura, aunque exista una clave de API IAM para la cuenta. Si el usuario cuyas credenciales se almacenan no tiene los permisos necesarios para pedir infraestructura, entonces las acciones relacionadas con la infraestructura, como crear un clúster o recargar un nodo trabajador, pueden fallar.</p>
    <p>Para eliminar las credenciales de infraestructura de IBM Cloud (SoftLayer) establecidas manualmente, puede utilizar el mandato [<code>bx cs credentials-unset</code>](cs_cli_reference.html#cs_credentials_unset). Una vez eliminadas las credenciales, la clave de API IAM se utiliza para pedir infraestructura.</p></dd>
</dl>

<br />


## Personalización de permisos de infraestructura para un usuario
{: #infra_access}

Cuando se establecen las políticas de infraestructura en Identity and Access Management, se otorgan permisos a un usuario que están asociados con un rol. Para personalizar esos permisos, debe iniciar sesión en la infraestructura de IBM Cloud (SoftLayer) y ajustar los permisos allí.
{: #view_access}

Por ejemplo, los **Usuarios básicos** pueden rearrancar un nodo trabajador, pero no pueden recargarlo. Sin dar a esa persona permisos de **Superusuario**, puede ajustar los permisos de la infraestructura de IBM Cloud (SoftLayer) y añadir el permiso para ejecutar un mandato de recarga.

1.  Inicie la sesión en [cuenta de {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/) y, a continuación, desde el menú, seleccione **Infraestructura**.

2.  Vaya a **Cuenta** > **Usuarios** > **Lista de usuarios**.

3.  Para modificar permisos, seleccione el nombre de un perfil de usuario o la columna **Acceso de dispositivos**.

4.  En el separador **Permisos de portal**, personalice el acceso de usuario. Los permisos que los usuarios necesitan dependen de los recursos de infraestructura que necesitan utilizar:

    * Utilice la lista desplegable **Permisos rápidos** para asignar el rol **Superusuario**, que da al usuario todos los permisos.
    * Utilice la lista desplegable **Permisos rápidos** para asignar el rol **Usuario básico**, que da al usuario algunos permisos necesarios, pero no todos.
    * Si no desea otorgar todos los permisos con el rol **Superusuario** o necesita añadir más permisos que los del rol **Usuario básico**, consulte la tabla siguiente, que describe los permisos necesarios para realizar tareas comunes en {{site.data.keyword.containershort_notm}}.

    <table summary="Permisos de infraestructura para casos de ejemplo comunes de {{site.data.keyword.containershort_notm}}.">
     <caption>Permisos de infraestructura necesarios habitualmente para {{site.data.keyword.containershort_notm}}</caption>
     <thead>
      <th>Tareas comunes en {{site.data.keyword.containershort_notm}}</th>
      <th>Permisos de infraestructura necesarios por separador</th>
     </thead>
     <tbody>
       <tr>
         <td><strong>Permisos mínimos</strong>: <ul><li>Cree un clúster.</li></ul></td>
         <td><strong>Dispositivos</strong>:<ul><li>Ver detalles de servidores virtuales</li><li>Rearrancar servidor y ver información del sistema IPMI</li><li>Emitir recargas de SO e iniciar el kernel de rescate</li></ul><strong>Cuenta</strong>: <ul><li>Añadir/actualizar instancias de nube</li><li>Añadir servidor</li></ul></td>
       </tr>
       <tr>
         <td><strong>Administración de clúster</strong>: <ul><li>Crear, actualizar y suprimir clústeres.</li><li>Añadir, cargar y rearrancar nodos trabajadores.</li><li>Ver VLAN.</li><li>Crear subredes.</li><li>Desplegar pods y servicios del equilibrador de carga.</li></ul></td>
         <td><strong>Soporte</strong>:<ul><li>Ver tíquets</li><li>Añadir tíquets</li><li>Editar tíquets</li></ul>
         <strong>Dispositivos</strong>:<ul><li>Ver detalles de servidores virtuales</li><li>Rearrancar servidor y ver información del sistema IPMI</li><li>Actualizar servidor</li><li>Emitir recargas de SO e iniciar el kernel de rescate</li></ul>
         <strong>Servicios</strong>:<ul><li>Gestionar claves SSH</li></ul>
         <strong>Cuenta</strong>:<ul><li>Ver información de cuenta</li><li>Añadir/actualizar instancias de nube</li><li>Cancelar servidor</li><li>Añadir servidor</li></ul></td>
       </tr>
       <tr>
         <td><strong>Almacenamiento</strong>: <ul><li>Crear reclamaciones de volumen persistente para suministrar volúmenes persistentes.</li><li>Crear y gestionar recursos de infraestructura de almacenamiento.</li></ul></td>
         <td><strong>Servicios</strong>:<ul><li>Gestionar almacenamiento</li></ul><strong>Cuenta</strong>:<ul><li>Añadir almacenamiento</li></ul></td>
       </tr>
       <tr>
         <td><strong>Gestión de redes privadas</strong>: <ul><li>Gestionar VLAN privadas para redes en clúster.</li><li>Configurar conectividad de VPN en redes privadas.</li></ul></td>
         <td><strong>Red</strong>:<ul><li>Gestionar rutas de subredes de la red</li><li>Gestionar túneles de red IPSEC</li><li>Gestionar pasarelas de red</li><li>Administración de VPN</li></ul></td>
       </tr>
       <tr>
         <td><strong>Gestión de redes públicas</strong>:<ul><li>Configurar las redes públicas de Ingress y del equilibrador de carga para exponer apps.</li></ul></td>
         <td><strong>Dispositivos</strong>:<ul><li>Gestionar equilibradores de carga</li><li>Editar nombre de host/dominio</li><li>Gestionar control de puertos</li></ul>
         <strong>Red</strong>:<ul><li>Añadir sistema con puerto de red pública</li><li>Gestionar rutas de subredes de la red</li><li>Añadir direcciones IP</li></ul>
         <strong>Servicios</strong>:<ul><li>Gestionar DNS, DNS inverso y WHOIS</li><li>Ver certificados (SSL)</li><li>Gestionar certificados (SSL)</li></ul></td>
       </tr>
     </tbody>
    </table>

5.  Para guardar los cambios, pulse **Editar permisos del portal**.

6.  En el separador **Acceso de dispositivos**, seleccione los dispositivos a los que desea otorgar acceso.

    * En el desplegable de **Tipo de dispositivo**, puede otorgar acceso a **Todos los servidores virtuales**.
    * Para permitir a los usuarios acceder a nuevos dispositivos que se crean, seleccione **Otorgar acceso automáticamente cuando se añadan nuevos dispositivos**.
    * Para guardar los cambios, pulse **Actualizar acceso de dispositivos**.

<br />


## Autorización de usuarios con roles de RBAC de Kubernetes personalizados
{: #rbac}

Las políticas de acceso de {{site.data.keyword.containershort_notm}} corresponden a determinados roles de control de acceso basado en roles (RBAC) de Kubernetes, tal como se describe en [Permisos y políticas de acceso](#access_policies). Para autorizar otros roles de Kubernetes que difieren de la política de acceso correspondiente, puede personalizar los roles de RBAC y luego asignar los roles a usuarios individuales o grupos de usuarios.
{: shortdesc}

Por ejemplo, es posible que desee otorgar permisos a un equipo de desarrolladores para trabajar en un determinado grupo de API o con recursos dentro de un espacio de nombres de Kubernetes en el clúster, pero no en todo el clúster. Puede crear un rol y luego enlazar el rol a los usuarios utilizando un nombre de usuario que sea exclusivo para {{site.data.keyword.containershort_notm}}. Para obtener información, consulte [Utilización de la autorización RBAC ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview) en la documentación de Kubernetes.

Antes de empezar, [defina el clúster como destino de la CLI de Kubernetes](cs_cli_install.html#cs_cli_configure).

1.  Cree el rol con el acceso que desea asignar.

    1. Cree un archivo `.yaml` para definir el rol con el acceso que desea asignar.

        ```
        kind: Role
        apiVersion: rbac.authorization.k8s.io/v1
        metadata:
          namespace: default
          name: my_role
        rules:
        - apiGroups: [""]
          resources: ["pods"]
          verbs: ["get", "watch", "list"]
        - apiGroups: ["apps", "extensions"]
          resources: ["daemonsets", "deployments"]
          verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
        ```
        {: codeblock}

        <table>
        <caption>Visión general de los componentes de YAML</caption>
          <thead>
            <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general de los componentes de YAML</th>
          </thead>
          <tbody>
            <tr>
              <td><code>kind</code></td>
              <td>Utilice `Role` para otorgar acceso a los recursos dentro de un espacio de nombres único o `ClusterRole` para los recursos de todo el clúster.</td>
            </tr>
            <tr>
              <td><code>apiVersion</code></td>
              <td><ul><li>Para clústeres que ejecutan Kubernetes 1.8 o posterior, utilice `rbac.authorization.k8s.io/v1`. </li><li>Para versiones anteriores, utilice `apiVersion: rbac.authorization.k8s.io/v1beta1`.</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/namespace</code></td>
              <td><ul><li>Para el tipo `Role`: Especifique el espacio de nombres de Kubernetes al que se otorga acceso.</li><li>No utilice el campo `namespace` si está creando un `ClusterRole` que se aplica a nivel de clúster.</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/name</code></td>
              <td>Asigne un nombre al rol y utilice el nombre más adelante cuando enlace el rol.</td>
            </tr>
            <tr>
              <td><code>rules/apiGroups</code></td>
              <td><ul><li>Especifique los grupos de API de Kubernetes con los que desea que puedan interactuar los usuarios, como `"apps"`, `"batch"` o `"extensions"`. </li><li>Para acceder al grupo de API principal en la vía de acceso de REST `api/v1`, deje el grupo en blanco: `[""]`.</li><li>Para obtener más información, consulte [grupos de API ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://v1-9.docs.kubernetes.io/docs/reference/api-overview/#api-groups) en la documentación de Kubernetes.</li></ul></td>
            </tr>
            <tr>
              <td><code>rules/resources</code></td>
              <td><ul><li>Especifique los recursos de Kubernetes a los que desea otorgar acceso, como `"daemonsets"`, `"deployments"`, `"events"` o `"ingresses"`.</li><li>Si especifica `"nodes"`, entonces el tipo de rol debe ser `ClusterRole`.</li><li>Para obtener una lista de recursos, consulte la tabla de [tipos de recurso ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/reference/kubectl/cheatsheet/) en la hoja de apuntes de Kubernetes.</li></ul></td>
            </tr>
            <tr>
              <td><code>rules/verbs</code></td>
              <td><ul><li>Especifique los tipos de acciones que desea que los usuarios puedan hacer, como `"get"`, `"list"`, `"describe"`, `"create"` o `"delete"`. </li><li>Para ver una lista completa de verbos, consulte la [documentación de `kubectl`![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/reference/kubectl/overview/).</li></ul></td>
            </tr>
          </tbody>
        </table>

    2.  Cree el rol en el clúster.

        ```
        kubectl apply -f <path_to_yaml_file>
        ```
        {: pre}

    3.  Verifique que se ha creado el rol.

        ```
        kubectl get roles
        ```
        {: pre}

2.  Enlace usuarios al rol.

    1. Cree un archivo `.yaml` para enlazar usuarios al rol. Anote el URL exclusivo que se debe utilizar para el nombre de cada sujeto.

        ```
        kind: RoleBinding
        apiVersion: rbac.authorization.k8s.io/v1
        metadata:
          name: my_role_team1
          namespace: default
        subjects:
        - kind: User
          name: https://iam.ng.bluemix.net/kubernetes#user1@example.com
          apiGroup: rbac.authorization.k8s.io
        - kind: User
          name: https://iam.ng.bluemix.net/kubernetes#user2@example.com
          apiGroup: rbac.authorization.k8s.io
        roleRef:
          kind: Role
          name: custom-rbac-test
          apiGroup: rbac.authorization.k8s.io
        ```
        {: codeblock}

        <table>
        <caption>Visión general de los componentes de YAML</caption>
          <thead>
            <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general de los componentes de YAML</th>
          </thead>
          <tbody>
            <tr>
              <td><code>kind</code></td>
              <td>En `kind`, especifique `RoleBinding` para los dos tipos de archivos `.yaml` de roles: `Role` en espacio de nombres `ClusterRole` en todo el clúster.</td>
            </tr>
            <tr>
              <td><code>apiVersion</code></td>
              <td><ul><li>Para clústeres que ejecutan Kubernetes 1.8 o posterior, utilice `rbac.authorization.k8s.io/v1`. </li><li>Para versiones anteriores, utilice `apiVersion: rbac.authorization.k8s.io/v1beta1`.</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/namespace</code></td>
              <td><ul><li>Para el tipo `Role`: Especifique el espacio de nombres de Kubernetes al que se otorga acceso.</li><li>No utilice el campo `namespace` si está creando un `ClusterRole` que se aplica a nivel de clúster.</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/name</code></td>
              <td>Asigne un nombre al enlace de rol.</td>
            </tr>
            <tr>
              <td><code>subjects/kind</code></td>
              <td>Especifique el tipo como `User`.</td>
            </tr>
            <tr>
              <td><code>subjects/name</code></td>
              <td><ul><li>Añada la dirección de correo electrónico del usuario al URL siguiente: `https://iam.ng.bluemix.net/kubernetes#`.</li><li>Por ejemplo, `https://iam.ng.bluemix.net/kubernetes#user1@example.com`</li></ul></td>
            </tr>
            <tr>
              <td><code>subjects/apiGroup</code></td>
              <td>Utilice `rbac.authorization.k8s.io`.</td>
            </tr>
            <tr>
              <td><code>roleRef/kind</code></td>
              <td>Especifique el mismo valor que el de `kind` en el archivo `.yaml` de roles: `Role` o `ClusterRole`.</td>
            </tr>
            <tr>
              <td><code>roleRef/name</code></td>
              <td>Especifique el nombre del archivo `.yaml` de roles.</td>
            </tr>
            <tr>
              <td><code>roleRef/apiGroup</code></td>
              <td>Utilice `rbac.authorization.k8s.io`.</td>
            </tr>
          </tbody>
        </table>

    2. Cree el recurso de enlace de rol en el clúster.

        ```
        kubectl apply -f filepath/my_role_team1.yaml
        ```
        {: pre}

    3.  Verifique que se ha creado el enlace.

        ```
        kubectl get rolebinding
        ```
        {: pre}

Ahora que ha creado y enlazado un rol de RBAC de Kubernetes personalizado, siga con los usuarios. Pídales que prueben una acción que tengan permiso para realizar debido al rol, como suprimir un pod.
