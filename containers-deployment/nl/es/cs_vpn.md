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


# Configuración de la conectividad de VPN
{: #vpn}

La conectividad de VPN le permite conectar de forma segura apps de un clúster de Kubernetes en {{site.data.keyword.containerlong}} a una red local. También puede conectar apps que son externas al clúster a una app que se ejecuta dentro del clúster.
{:shortdesc}

Para conectar de forma segura sus nodos trabajadores y apps a un centro de datos local, puede utilizar una de las siguientes opciones.

- **Servicio VPN IPSec de strongSwan**: puede configurar un [servicio VPN IPSec de strongSwan ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.strongswan.org/) que se conecte de forma segura al clúster de Kubernetes con una red local. El servicio VPN IPSec de strongSwan proporciona un canal de comunicaciones de extremo a extremo seguro sobre Internet que está basado en la suite de protocolos
Internet Protocol Security (IPsec) estándar del sector. Para configurar una conexión segura entre el clúster y una red local, [configure y despliegue el servicio VPN IPSec strongSwan](#vpn-setup) directamente en un pod del clúster.

- **Virtual Router Appliance (VRA) o Fortigate Security Appliance (FSA)**: Podría elegir entre configurar un [VRA](/docs/infrastructure/virtual-router-appliance/about.html) o un [FSA](/docs/infrastructure/fortigate-10g/about.html) para configurar un punto final de VPN IPSec. Esta opción es útil si tiene un clúster grande, desea acceder a recursos no Kubernetes sobre la VPN o desea acceder a varios clústeres en una sola VPN. Para configurar VRA, consulte [Configuración de la conectividad de VPN con VRA](#vyatta).

## Configuración de la conectividad de VPN con el diagrama de Helm del servicio VPN IPSec de strongSwan
{: #vpn-setup}

Utilice un diagrama de Helm para configurar y desplegar el servicio VPN IPSec de strongSwan dentro de un pod de Kubernetes.
{:shortdesc}

Puesto que strongSwan está integrado en su clúster, no necesita un dispositivo de pasarela externo. Cuando se establece una conectividad de VPN, las rutas se configuran automáticamente en todos los nodos trabajadores del clúster. Estas rutas permiten la conectividad bidireccional a través del túnel VPN entre pods en cualquier nodo trabajador y el sistema remoto. Por ejemplo, en el diagrama siguiente se muestra cómo una app en {{site.data.keyword.containershort_notm}} puede comunicarse con un servidor local a través de una conexión VPN strongSwan:

<img src="images/cs_vpn_strongswan.png" width="700" alt="Exponer una app en {{site.data.keyword.containershort_notm}} mediante la utilización de un equilibrador de carga" style="width:700px; border-style: none"/>

1. Una app en el clúster, `myapp`, recibe una solicitud desde un servicio Ingress o LoadBalancer. Dicha app debe conectarse de forma segura a los datos en su red local.

2. La solicitud al centro de datos local se reenvía al pod de VPN de IPSec strongSwan. La dirección IP de destino se utiliza para determinar qué paquetes de red deben enviarse al pod de VPN de IPSec strongSwan.

3. La solicitud se cifra y envía a través del túnel VPN al centro de datos local.

4. La solicitud entrante pasa a través del cortafuegos local y se entrega al punto final de túnel VPN (direccionador) donde se descifra.

5. El punto final de túnel VPN (direccionador) reenvía la solicitud al servidor o sistema principal local, dependiendo de la dirección IP de destino especificada en el paso 2. Los datos necesarios se envían a través de la conexión VPN a `myapp` mediante el mismo proceso.

### Configurar el diagrama de Helm de strongSwan
{: #vpn_configure}

Antes de empezar:
* [Instale una pasarela de VPN IPSec en su centro de datos local](/docs/infrastructure/iaas-vpn/set-up-ipsec-vpn.html#setting-up-an-ipsec-connection).
* [Cree un clúster estándar](cs_clusters.html#clusters_cli) o [actualice un clúster existente a la versión 1.7.16 o posterior](cs_cluster_update.html#master).
* El clúster debe tener disponible al menos una dirección IP del equilibrador de carga público. [Puede comprobar las direcciones IP públicas disponibles](cs_subnets.html#manage) o [liberar una dirección IP usada](cs_subnets.html#free).
* [Defina el clúster como destino de la CLI de Kubernetes](cs_cli_install.html#cs_cli_configure).

Para obtener más información sobre los mandatos Helm que se utilizan para configurar el diagrama de strongSwan, consulte la <a href="https://docs.helm.sh/helm/" target="_blank">documentación de Helm <img src="../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>.

Para configurar el diagrama de Helm:

1. [Instale Helm para el clúster y añada el repositorio de {{site.data.keyword.Bluemix_notm}} a la instancia de Helm](cs_integrations.html#helm).

2. Guarde los valores de configuración predeterminados para el diagrama de Helm de strongSwan en un archivo YAML local.

    ```
    helm inspect values ibm/strongswan > config.yaml
    ```
    {: pre}

3. Abra el archivo `config.yaml` y realice los cambios siguientes a los valores predeterminados según la configuración de VPN que desee. Puede encontrar descripciones de valores más avanzados en los comentarios del archivo de configuración.

    **Importante**: si no necesita cambiar una propiedad, coméntela colocando un signo `#` delante de ella.

    <table>
    <caption>Visión general de los componentes de YAML</caption>
    <col width="22%">
    <col width="78%">
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general de los componentes del archivo YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code>localSubnetNAT</code></td>
    <td>NAT (conversión de direcciones de red) para subredes supone una solución temporal para los conflictos de subred entre las redes locales e in situ. Puede utilizar NAT para volver a correlacionar las subredes IP locales privadas del clúster, la subred del pod (172.30.0.0/16) o la subred del servicio del pod (172.21.0.0/16) a una subred privada diferente. El túnel VPN ve subredes IP correlacionadas en lugar de las subredes originales. La correlación ocurre antes de que los paquetes se envíen a través del túnel VPN, así como después de que lleguen del túnel VPN. Puede exponer las redes correlacionadas y las no correlacionadas al mismo tiempo a través de la VPN.<br><br>Para habilitar NAT, puede añadir una subred entera o direcciones IP individuales. Si añade toda una subred (en el formato <code>10.171.42.0/24=10.10.10.0/24</code>), la correlación es de 1 a 1: todas las direcciones IP de la subred de red interna se correlacionan con la subred de red externa, y viceversa. Si añade direcciones IP individuales (en el formato <code>10.171.42.17/32=10.10.10.2/32, 10.171.42.29/32=10.10.10.3/32</code>), sólo las direcciones IP internas se correlacionan con las direcciones IP externas especificadas.<br><br>Si utiliza esta opción, la subred local que se expone a través de la conexión VPN es la subred "externa" con la que se correlaciona la subred "interna".
    </td>
    </tr>
    <tr>
    <td><code>loadBalancerIP</code></td>
    <td>Si desea especificar una dirección IP pública portátil para el servicio VPN strongSwan para las conexiones VPN entrantes, añada dicha dirección IP. Especificar una dirección IP es útil cuando necesita una dirección IP estable como, por ejemplo, cuando debe designar las direcciones IP permitidas a través de un cortafuegos local.<br><br>Para ver las direcciones IP públicas portátiles disponibles asignadas a este clúster, consulte [gestión de subredes y direcciones IP](cs_subnets.html#manage). Si deja este valor en blanco, se utilizará una dirección IP pública portátil gratuita. Si la conexión VPN se inicia desde la pasarela local (<code>ipsec.auto</code> se establece en <code>add</code>), puede utilizar esta propiedad para configurar una dirección IP pública persistente en la pasarela local para el clúster.</td>
    </tr>
    <tr>
    <td><code>connectUsingLoadBalancerIP</code></td>
    <td>Utilice la dirección IP del equilibrador de carga que ha añadido en <code>loadBalancerIP</code> para establecer también la conexión VPN de salida. Si esta opción está habilitada, todos los nodos trabajadores del clúster deben estar en la misma VLAN pública. De lo contrario, debe utilizar el valor <code>nodeSelector</code> para asegurarse de que el pod de VPN se despliega en un nodo trabajador en la misma VLAN pública que <code>loadBalancerIP</code>. Esta opción se ignora si <code>ipsec.auto</code> se establece en <code>add</code>.<p>Valores aceptados:</p><ul><li><code>"false"</code>: No conectarse a la VPN utilizando la dirección IP del equilibrador de carga. En su lugar, se utiliza la dirección IP pública del nodo trabajador en el que se está ejecutando el pod de VPN.</li><li><code>"true"</code>: Establece la VPN utilizando la dirección IP del equilibrador de carga como la dirección IP de origen local. Si <code>loadBalancerIP</code> no está establecido, se utiliza la dirección IP externa asignada al servicio de equilibrio de carga.</li><li><code>"auto"</code>: Cuando <code>ipsec.auto</code> está establecido en <code>start</code> y se ha establecido <code>loadBalancerIP</code>, establece la VPN utilizando la dirección IP del equilibrador de carga como la dirección IP de origen local.</li></ul></td>
    </tr>
    <tr>
    <td><code>nodeSelector</code></td>
    <td>Para limitar los nodos en los que se despliega el pod de VPN de strongSwan, añada la dirección IP de un nodo trabajador específico o una etiqueta de nodo trabajador. Por ejemplo, el valor <code>kubernetes.io/hostname: 10.xxx.xx.xxx</code> restringe la ejecución del pod de VPN a ese nodo trabajador sólo. El valor <code>strongswan: vpn</code> restringe la ejecución del pod de VPN a los nodos trabajadores que tengan esa etiqueta. Puede utilizar cualquier etiqueta de nodo trabajador, pero se recomienda que utilice: <code>strongswan: &lt;release_name&gt;</code> para que puedan utilizarse nodos trabajadores diferentes con despliegues diferentes de este diagrama.<br><br>Si el clúster inicia la conexión VPN (<code>ipsec.auto</code> se establece en <code>start</code>), puede utilizar esta propiedad para limitar las direcciones IP de origen de la conexión VPN que se muestran a la pasarela local. Este valor es opcional.</td>
    </tr>
    <tr>
    <td><code>ipsec.keyexchange</code></td>
    <td>Si el punto final de túnel VPN local no admite <code>ikev2</code> como protocolo para inicializar la conexión, cambie este valor a <code>ikev1</code> o <code>ike</code>.</td>
    </tr>
    <tr>
    <td><code>ipsec.esp</code></td>
    <td>Añada la lista de algoritmos de autenticación y cifrado de ESP que el punto final de túnel de VPN local utiliza para la conexión.<ul><li>Si <code>ipsec.keyexchange</code> se establece en <code>ikev1</code>, se debe especificar este valor.</li><li>Si <code>ipsec.keyexchange</code> se establece en <code>ikev2</code>, este valor es opcional. Si deja este valor en blanco, se utilizan para la conexión los algoritmos predeterminados de strongSwan <code>aes128-sha1,3des-sha1</code>.</li></ul></td>
    </tr>
    <tr>
    <td><code>ipsec.ike</code></td>
    <td>Añada la lista de algoritmos de autenticación y cifrado IKE/ISAKMP SA que el punto final de túnel de VPN local utiliza para la conexión.<ul><li>Si <code>ipsec.keyexchange</code> se establece en <code>ikev1</code>, se debe especificar este valor.</li><li>Si <code>ipsec.keyexchange</code> se establece en <code>ikev2</code>, este valor es opcional. Si deja este valor en blanco, se utilizan para la conexión los algoritmos predeterminados de strongSwan <code>aes128-sha1-modp2048,3des-sha1-modp1536</code>.</li></ul></td>
    </tr>
    <tr>
    <td><code>ipsec.auto</code></td>
    <td>Si desea que el clúster inicie la conexión VPN, cambie este valor a <code>start</code>.</td>
    </tr>
    <tr>
    <td><code>local.subnet</code></td>
    <td>Cambie este valor a la lista de CIDR de subred del clúster que se va a exponer a través de la conexión VPN a la red local. Esta lista puede incluir las subredes siguientes: <ul><li>El CIDR de subred del pod de Kubernetes: <code>172.30.0.0/16</code></li><li>El CIDR de subred del servicio de Kubernetes: <code>172.21.0.0/16</code></li><li>Si las apps están expuestas por un servicio NodePort en una red privada, el CIDR de subred privada del nodo trabajador. Recupere los tres primeros octetos de la dirección de IP privada de su trabajador ejecutando el mandato <code>bx cs worker &lt;cluster_name&gt;</code>. Por ejemplo, si es <code>&lt;10.176.48.xx&gt;</code>, anote <code>&lt;10.176.48&gt;</code>. A continuación, obtenga el CIDR de subred privado del trabajador ejecutando el siguiente mandato, sustituyendo <code>&lt;xxx.yyy.zz&gt;</code> con el octeto recuperado con anterioridad: <code>bx cs subnets | grep &lt;xxx.yyy.zzz&gt;</code>.</li><li>Si tiene apps expuestas por los servicios de LoadBalancer en la red privada, el CIDR de subred privada o gestionada por el usuario del clúster. Para encontrar estos valores, ejecute <code>bx cs cluster-get &lt;cluster_name&gt; --showResources</code>. En la sección **VLANS**, busque CIDR que tengan los valores **Public** o <code>false</code>.</li></ul>**Nota**: Si <code>ipsec.keyexchange</code> se establece en <code>ikev1</code>, solo puede especificar una subred.</td>
    </tr>
    <tr>
    <td><code>local.id</code></td>
    <td>Cambie este valor al identificador de serie para el lado del clúster de Kubernetes local que el punto final de túnel VPN utiliza para la conexión.</td>
    </tr>
    <tr>
    <td><code>remote.gateway</code></td>
    <td>Cambie este valor a la dirección IP pública de la pasarela VPN local. Cuando <code>ipsec.auto</code> se establece en <code>start</code>, este valor es obligatorio.</td>
    </tr>
    <td><code>remote.subnet</code></td>
    <td>Cambie este valor a la lista de CIDR de subred privada local a la que pueden acceder los clústeres de Kubernetes. **Nota**: Si <code>ipsec.keyexchange</code> se establece en <code>ikev1</code>, solo puede especificar una subred.</td>
    </tr>
    <tr>
    <td><code>remote.id</code></td>
    <td>Cambie este valor al identificador de serie para el lado local remoto que el punto final de túnel VPN utiliza para la conexión.</td>
    </tr>
    <tr>
    <td><code>remote.privateIPtoPing</code></td>
    <td>Añada la dirección IP privada en la subred remota que desea utilizar en los programas de validación de prueba de Helm para las pruebas de conectividad ping de VPN. Este valor es opcional.</td>
    </tr>
    <tr>
    <td><code>preshared.secret</code></td>
    <td>Cambie este valor a la clave secreta compartida previamente que la pasarela de punto final de túnel VPN local utiliza para la conexión. Este valor se almacena en <code>ipsec.secrets</code>.</td>
    </tr>
    </tbody></table>

4. Guarde el archivo `config.yaml` actualizado.

5. Instale el diagrama de Helm en el clúster con el archivo `config.yaml` actualizado. Las propiedades actualizadas se almacenan en una correlación de configuración para el diagrama.

    **Nota**: Si tiene varios despliegues de VPN en un solo clúster, puede evitar conflictos de denominación y diferenciar entre los despliegues eligiendo nombres de release más descriptivos que `vpn`. Para evitar el truncamiento del nombre de release, limite el nombre de release a 35 caracteres o menos.

    ```
    helm install -f config.yaml --name=vpn ibm/strongswan
    ```
    {: pre}

6. Compruebe el estado de despliegue del diagrama. Cuando el diagrama está listo, el campo **STATUS**, situado cerca de la parte superior de la salida, tiene el valor `DEPLOYED`.

    ```
    helm status vpn
    ```
    {: pre}

7. Una vez desplegado el diagrama, verifique que se han utilizado los valores actualizados del archivo `config.yaml`.

    ```
    helm get values vpn
    ```
    {: pre}


### Probar y verificar la conectividad de VPN
{: #vpn_test}

Después de desplegar el diagrama de Helm, pruebe la conectividad de VPN.
{:shortdesc}

1. Si la VPN de la pasarela local no está activa, inicie la VPN.

2. Establezca la variable de entorno `STRONGSWAN_POD`.

    ```
    export STRONGSWAN_POD=$(kubectl get pod -l app=strongswan,release=vpn -o jsonpath='{ .items[0].metadata.name }')
    ```
    {: pre}

3. Compruebe el estado de la VPN. Un estado `ESTABLISHED` significa que la conexión VPN se ha realizado correctamente.

    ```
    kubectl exec $STRONGSWAN_POD -- ipsec status
    ```
    {: pre}

    Salida de ejemplo:

    ```
    Security Associations (1 up, 0 connecting):
    k8s-conn[1]: ESTABLISHED 17 minutes ago, 172.30.xxx.xxx[ibm-cloud]...192.xxx.xxx.xxx[on-premises]
    k8s-conn{2}: INSTALLED, TUNNEL, reqid 12, ESP in UDP SPIs: c78cb6b1_i c5d0d1c3_o
    k8s-conn{2}: 172.21.0.0/16 172.30.0.0/16 === 10.91.152.xxx/26
    ```
    {: screen}

    **Nota**:

    <ul>
    <li>Cuando intenta establecer la conectividad de VPN con el diagrama de Helm de strongSwan, es probable que el estado de VPN no sea `ESTABLISHED` la primera vez. Puede que necesite comprobar los valores de punto final de VPN local y cambiar el archivo de configuración varias veces antes de que la conexión sea correcta: <ol><li>Ejecute `helm delete --purge <release_name>`</li><li>Corrija los valores incorrectos en el archivo de configuración.</li><li>Ejecute `helm install -f config.yaml --name=<release_name> ibm/strongswan`</li></ol>También puede ejecutar más controles en el paso siguiente.</li>
    <li>Si el pod de VPN está en estado `ERROR` o sigue bloqueándose y reiniciándose, puede que se deba a la validación de parámetro de los valores de `ipsec.conf` en la correlación de configuración del diagrama.<ol><li>Compruebe los errores de validación en los registros del pod de strongSwan ejecutando `kubectl logs -n $STRONGSWAN_POD`.</li><li>Si existen errores de validación, ejecute `helm delete --purge <release_name>`<li>Corrija los valores incorrectos en el archivo de configuración.</li><li>Ejecute `helm install -f config.yaml --name=<release_name> ibm/strongswan`</li></ol>Si el clúster tiene un número elevado de nodos trabajadores, también puede utilizar `helm upgrade` para aplicar los cambios más rápidamente en lugar de ejecutar `helm delete` y `helm install`.</li>
    </ul>

4. También puede probar la conectividad de VPN ejecutando las cinco pruebas de Helm que se incluyen en la definición del diagrama de strongSwan.

    ```
    helm test vpn
    ```
    {: pre}

    * Si pasa todas las pruebas, la conexión VPN de strongSwan está configurada correctamente.

    * Si alguno de los pasos fallan, continúe en el siguiente paso.

5. Puede consultar la salida de una prueba que ha fallado en los registros del pod de prueba.

    ```
    kubectl logs <test_program>
    ```
    {: pre}

    **Nota**: Algunas de las pruebas tienen requisitos que son valores opcionales en la configuración de VPN. Si alguna de las pruebas falla, los errores pueden ser aceptables en función de si ha especificado estos valores opcionales. Consulte la tabla siguiente para obtener más información sobre cada prueba y por qué puede fallar.

    {: #vpn_tests_table}
    <table>
    <caption>Descripción de las pruebas de conectividad de VPN de Helm</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Descripción de las pruebas de conectividad de VPN de Helm</th>
    </thead>
    <tbody>
    <tr>
    <td><code>vpn-strongswan-check-config</code></td>
    <td>Valida la sintaxis del archivo <code>ipsec.conf</code> que se genera desde el archivo <code>config.yaml</code>. Esta prueba puede fallar debido a valores incorrectos en el archivo <code>config.yaml</code>.</td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-check-state</code></td>
    <td>Comprueba que la conexión VPN tiene el estado <code>ESTABLISHED</code>. Esta prueba puede fallar por las siguientes razones:<ul><li>Diferencias entre los valores del archivo <code>config.yaml</code> y los valores del punto final de VPN local.</li><li>Si el clúster está en modalidad "de escucha" (<code>ipsec.auto</code> se establece en <code>add</code>), la conexión no se establece en el lado local.</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-gw</code></td>
    <td>Hace ping a la dirección IP pública de <code>remote.gateway</code> que ha configurado en el archivo <code>config.yaml</code>. Esta prueba puede fallar por las siguientes razones:<ul><li>No haber especificado una dirección IP de pasarela VPN local. Si <code>ipsec.auto</code> se establece en <code>start</code>, la dirección IP de <code>remote.gateway</code> es obligatoria.</li><li>La conexión VPN no tiene el estado <code>ESTABLISHED</code>. Consulte <code>vpn-strongswan-check-estado</code> para obtener más información.</li><li>La conectividad de VPN tiene el valor <code>ESTABLISHED</code>, pero un cortafuegos está bloqueando los paquetes ICMP.</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-ip-1</code></td>
    <td>Hace ping a la dirección IP privada <code>remote.privateIPtoPing</code> de la pasarela VPN local desde el pod de VPN en el clúster. Esta prueba puede fallar por las siguientes razones:<ul><li>No haber especificado una dirección IP <code>remote.privateIPtoPing</code>. Si intencionalmente no especifica una dirección IP, este error es aceptable.</li><li>No haber especificado el CIDR de subred del pod del clúster, <code>172.30.0.0/16</code>, en la lista <code>local.subnet</code>.</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-ip-2</code></td>
    <td>Hace ping a la dirección IP privada <code>remote.privateIPtoPing</code> de la pasarela VPN local desde el nodo trabajador en el clúster. Esta prueba puede fallar por las siguientes razones:<ul><li>No haber especificado una dirección IP <code>remote.privateIPtoPing</code>. Si intencionalmente no especifica una dirección IP, este error es aceptable.</li><li>No haber especificado el CIDR de subred privada del nodo trabajador del clúster en la lista <code>local.subnet</code>.</li></ul></td>
    </tr>
    </tbody></table>

6. Suprima el diagrama de Helm actual.

    ```
    helm delete --purge vpn
    ```
    {: pre}

7. Abra el archivo `config.yaml` y corrija los valores incorrectos.

8. Guarde el archivo `config.yaml` actualizado.

9. Instale el diagrama de Helm en el clúster con el archivo `config.yaml` actualizado. Las propiedades actualizadas se almacenan en una correlación de configuración para el diagrama.

    ```
    helm install -f config.yaml --name=<release_name> ibm/strongswan
    ```
    {: pre}

10. Compruebe el estado de despliegue del diagrama. Cuando el diagrama está listo, el campo **STATUS**, situado cerca de la parte superior de la salida, tiene el valor `DEPLOYED`.

    ```
    helm status vpn
    ```
    {: pre}

11. Una vez desplegado el diagrama, verifique que se han utilizado los valores actualizados del archivo `config.yaml`.

    ```
    helm get values vpn
    ```
    {: pre}

12. Limpie los pods de prueba actuales.

    ```
    kubectl get pods -a -l app=strongswan-test
    ```
    {: pre}

    ```
    kubectl delete pods -l app=strongswan-test
    ```
    {: pre}

13. Ejecute las pruebas de nuevo.

    ```
    helm test vpn
    ```
    {: pre}

<br />


## Actualización del diagrama de Helm de strongSwan
{: #vpn_upgrade}

Actualice el diagrama de Helm de strongSwan para asegurarse de que esté actualizado.
{:shortdesc}

Para actualizar el diagrama de Helm de strongSwan a la última versión:

  ```
  helm upgrade -f config.yaml <release_name> ibm/strongswan
  ```
  {: pre}

**Importante**: El diagrama Helm strongSwan 2.0.0 no funciona con Calico v3 ni con Kubernetes 1.10. Antes de [actualizar el clúster a la versión 1.10](cs_versions.html#cs_v110), actualice strongSwan al diagrama Helm 2.1.0, que es compatible para la versión anterior de Calico 2.6 y Kubernetes 1.7, 1.8 y 1.9.


### Actualización de la versión 1.0.0
{: #vpn_upgrade_1.0.0}

Debido a algunos de los valores que se utilizan en el diagrama de Helm versión 1.0.0, no puede utilizar `helm upgrade` para actualizar de 1.0.0 a la última versión.
{:shortdesc}

Para actualizar desde la versión 1.0.0, debe suprimir el diagrama 1.0.0 e instalar la versión más reciente:

1. Suprima el diagrama de Helm 1.0.0.

    ```
    helm delete --purge <release_name>
    ```
    {: pre}

2. Guarde los valores de configuración predeterminados para la última versión del diagrama de Helm de strongSwan en un archivo YAML local.

    ```
    helm inspect values ibm/strongswan > config.yaml
    ```
    {: pre}

3. Actualice el archivo de configuración y guarde el archivo con los cambios.

4. Instale el diagrama de Helm en el clúster con el archivo `config.yaml` actualizado.

    ```
    helm install -f config.yaml --name=<release_name> ibm/strongswan
    ```
    {: pre}

Además, algunos valores de tiempo de espera de `ipsec.conf` codificados en 1.0.0 se exponen como propiedades configurables en versiones posteriores. Los nombres y los valores predeterminados de algunos de estos valores de tiempo de espera de `ipsec.conf` configurables también se han cambiado para ser más coherentes con las normas de strongSwan. Si va a actualizar el diagrama de Helm de la versión 1.0.0 y quiere conservar los valores predeterminados de la versión 1.0.0 de los valores de tiempo de espera, añada los nuevos valores al archivo de configuración del diagrama con los valores predeterminados antiguos.



  <table>
  <caption>Diferencias de los valores de ipsec.conf entre la versión 1.0.0 y la última versión</caption>
  <thead>
  <th>Nombre de valor de 1.0.0</th>
  <th>Valor predeterminado de 1.0.0</th>
  <th>Nombre de valor de la versión más reciente</th>
  <th>Valor predeterminado de la versión más reciente</th>
  </thead>
  <tbody>
  <tr>
  <td><code>ikelifetime</code></td>
  <td>60m</td>
  <td><code>ikelifetime</code></td>
  <td>3h</td>
  </tr>
  <tr>
  <td><code>keylife</code></td>
  <td>20m</td>
  <td><code>lifetime</code></td>
  <td>1h</td>
  </tr>
  <tr>
  <td><code>rekeymargin</code></td>
  <td>3m</td>
  <td><code>margintime</code></td>
  <td>9m</td>
  </tr>
  </tbody></table>


## Inhabilitación del servicio VPN IPSec de strongSwan
{: vpn_disable}

Puede inhabilitar la conexión VPN suprimiendo el diagrama de Helm.
{:shortdesc}

  ```
  helm delete --purge <release_name>
  ```
  {: pre}

<br />


## Configuración de conectividad VPN con un dispositivo Virtual Router Appliance
{: #vyatta}

[Virtual Router Appliance (VRA)](/docs/infrastructure/virtual-router-appliance/about.html) proporciona el último sistema operativo Vyatta 5600 para servidores nativos x86. Puede utilizar VRA como una pasarela VPN para conectarse de forma segura a una red local.
{:shortdesc}

Todo el tráfico de red privado y público que entre o salga de las VLAN del clúster se direcciona a través de VRA. Puede utilizar VRA como un punto final de VPN para crear un túnel IPSec cifrado entre servidores en la infraestructura (Softlayer) de IBM Cloud y los recursos locales. Por ejemplo, en el diagrama siguiente se muestra cómo una app en un nodo trabajador privado en {{site.data.keyword.containershort_notm}} puede comunicarse con un servidor local a través de una conexión VPN Vyatta:

<img src="images/cs_vpn_vyatta.png" width="725" alt="Exposición de una app en {{site.data.keyword.containershort_notm}} mediante la utilización de un equilibrador de carga" style="width:725px; border-style: none"/>

1. Una app en el clúster, `myapp2`, recibe una solicitud desde un servicio Ingress o LoadBalancer. Dicha app debe conectarse de forma segura a los datos en su red local.

2. Puesto que `myapp2` se encuentra en un nodo trabajador que está en una VLAN privada únicamente, el dispositivo VRA actúa como una conexión segura entre los nodos trabajadores y la red local. El dispositivo VRA utiliza la dirección IP de destino para determinar qué paquetes de red deben enviarse a la red local.

3. La solicitud se cifra y envía a través del túnel VPN al centro de datos local.

4. La solicitud entrante pasa a través del cortafuegos local y se entrega al punto final de túnel VPN (direccionador) donde se descifra.

5. El punto final de túnel VPN (direccionador) reenvía la solicitud al servidor o sistema principal local, dependiendo de la dirección IP de destino especificada en el paso 2. Los datos necesarios se envían a través de la conexión VPN a `myapp2` mediante el mismo proceso.

Para configurar un Virtual Router Appliance:

1. [Solicitar un VRA](/docs/infrastructure/virtual-router-appliance/getting-started.html).

2. [Configurar la VLAN privada en el VRA](/docs/infrastructure/virtual-router-appliance/manage-vlans.html).

3. Para habilitar una conexión VPN utilizando el VRA, [configure VRRP en el VRA](/docs/infrastructure/virtual-router-appliance/vrrp.html#high-availability-vpn-with-vrrp).
