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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}



# Resolución de problemas en las redes del clúster
{: #cs_troubleshoot_network}

Si utiliza {{site.data.keyword.containerlong}}, tenga en cuenta estas técnicas para solucionar problemas relacionados con la red del clúster.
{: shortdesc}

Si tiene un problema más general, pruebe la [depuración del clúster](cs_troubleshoot.html).
{: tip}


## No se puede conectar a una app mediante un servicio de equilibrador de carga.
{: #cs_loadbalancer_fails}

{: tsSymptoms}
Ha expuesto a nivel público la app creando un servicio equilibrador de carga en el clúster. Cuando intenta conectar con la app utilizando la dirección IP pública o equilibrador de carga, la conexión falla o supera el tiempo de espera.

{: tsCauses}
Posibles motivos por los que el servicio del equilibrador de carga no funciona correctamente:

-   El clúster es un clúster gratuito o un clúster estándar con un solo nodo trabajador.
-   El clúster todavía no se ha desplegado por completo.
-   El script de configuración correspondiente al servicio equilibrador de carga incluye errores.

{: tsResolve}
Para resolver el problema del servicio equilibrador de carga:

1.  Compruebe que ha configurado un clúster estándar que se ha desplegado por completo y que tiene al menos dos nodos trabajadores para garantizar la alta disponibilidad del servicio equilibrador de carga.

  ```
  bx cs workers <cluster_name_or_ID>
  ```
  {: pre}

    En la salida de la CLI, asegúrese de que el **Estado** de los nodos trabajadores sea **Listo** y que el **Tipo de máquina** muestre un tipo de máquina que no sea **gratuito (free)**.

2.  Compruebe si el archivo de configuración correspondiente al servicio equilibrador de carga es preciso.

    ```
    apiVersion: v1
    kind: Service
    metadata:
      name: myservice
    spec:
      type: LoadBalancer
      selector:
        <selector_key>:<selector_value>
      ports:
       - protocol: TCP
         port: 8080
    ```
    {: pre}

    1.  Verifique que ha definido **LoadBalancer** como tipo de servicio.
    2.  En la sección `spec.selector` del servicio LoadBalancer, asegúrese de que `<selector_key>` y `<selector_value>` corresponden al par de clave/valor utilizado en la sección `spec.template.metadata.labels` de su despliegue yaml. Si las etiquetas no coinciden, la sección **Endpoints** en el servicio LoadBalancer visualiza **<none>** y la app no es accesible desde Internet.
    3.  Compruebe que ha utilizado el **puerto** en el que escucha la app.

3.  Compruebe el servicio equilibrador de carga y revisar la sección de **Sucesos** para ver si hay errores.

    ```
    kubectl describe service <myservice>
    ```
    {: pre}

    Busque los siguientes mensajes de error:

    <ul><li><pre class="screen"><code>Los clústeres con un nodo deben utilizar servicios de tipo NodePort</code></pre></br>Para utilizar el servicio equilibrador de carga, debe tener un clúster estándar con al menos dos nodos trabajadores.</li>
    <li><pre class="screen"><code>No hay ninguna IP de proveedor de nube disponible para dar respuesta a la solicitud de servicio del equilibrador de carga. Añada una subred portátil al clúster y vuélvalo a intentar</code></pre></br>Este mensaje de error indica que no queda ninguna dirección IP pública portátil que se pueda asignar al servicio equilibrador de carga. Consulte la sección sobre <a href="cs_subnets.html#subnets">Adición de subredes a clústeres</a> para ver información sobre cómo solicitar direcciones IP públicas portátiles para el clúster. Cuando haya direcciones IP públicas portátiles disponibles para el clúster, el servicio equilibrador de carga se creará automáticamente.</li>
    <li><pre class="screen"><code>La IP de proveedor de nube solicitada <cloud-provider-ip> no está disponible. Están disponibles las siguientes IP de proveedor de nube: <available-cloud-provider-ips></code></pre></br>Ha definido una dirección IP pública portátil para el servicio equilibrador de carga mediante la sección **loadBalancerIP**, pero esta dirección IP pública portátil no está disponible en la subred pública portátil. En la sección **loadBalancerIP** del script de configuración, elimine la dirección IP existente y añada una de las direcciones IP públicas portátiles disponibles. También puede eliminar la sección **loadBalancerIP** del script para que la dirección IP pública portátil disponible se pueda asignar automáticamente.</li>
    <li><pre class="screen"><code>No hay nodos disponibles para el servicio equilibrador de carga</code></pre>No tiene suficientes nodos trabajadores para desplegar un servicio equilibrador de carga. Una razón posible es que ha desplegado un clúster estándar con más de un nodo trabajador, pero el suministro de los nodos trabajadores ha fallado.</li>
    <ol><li>Obtenga una lista de los nodos trabajadores disponibles.</br><pre class="codeblock"><code>kubectl get nodes</code></pre></li>
    <li>Si se encuentran al menos dos nodos trabajadores disponibles, obtenga una lista de los detalles de los nodos trabajadores.</br><pre class="codeblock"><code>bx cs worker-get [&lt;cluster_name_or_ID&gt;] &lt;worker_ID&gt;</code></pre></li>
    <li>Asegúrese de que los ID de las VLAN públicas y privadas correspondientes a los nodos trabajadores devueltos por los mandatos <code>kubectl get nodes</code> y <code>bx cs [&lt;cluster_name_or_ID&gt;] worker-get</code> coinciden.</li></ol></li></ul>

4.  Si utiliza un dominio personalizado para conectar con el servicio equilibrador de carga, asegúrese de que el dominio personalizado está correlacionado con la dirección IP pública del servicio equilibrador de carga.
    1.  Busque la dirección IP pública del servicio equilibrador de carga.

        ```
        kubectl describe service <service_name> | grep "LoadBalancer Ingress"
        ```
        {: pre}

    2.  Compruebe que el dominio personalizado esté correlacionado con la dirección IP pública portátil del servicio equilibrador de carga en el registro de puntero
(PTR).

<br />




## No se puede conectar a una app mediante Ingress.
{: #cs_ingress_fails}

{: tsSymptoms}
Ha expuesto a nivel público la app creando un recurso de Ingress para la app en el clúster. Cuando intenta conectar con la app utilizando la dirección IP pública o subdominio del equilibrador de carga de aplicación (ALB), la conexión falla o supera el tiempo de espera.

{: tsCauses}
Posibles motivos por los que Ingress no funciona correctamente:
<ul><ul>
<li>El clúster todavía no se ha desplegado por completo.
<li>El clúster se ha configurado como un clúster gratuito o como un clúster estándar con un solo nodo trabajador.
<li>El script de configuración de Ingress incluye errores.
</ul></ul>

{: tsResolve}
Para resolver el problema de Ingress:

1.  Compruebe que ha configurado un clúster estándar que se ha desplegado por completo y que tiene al menos dos nodos trabajadores para garantizar la alta disponibilidad de su ALB.

  ```
  bx cs workers <cluster_name_or_ID>
  ```
  {: pre}

    En la salida de la CLI, asegúrese de que el **Estado** de los nodos trabajadores sea **Listo** y que el **Tipo de máquina** muestre un tipo de máquina que no sea **gratuito (free)**.

2.  Recupere el subdominio del ALB y la dirección IP pública y luego ejecute ping sobre cada uno.

    1.  Recupere el subdominio de ALB.

      ```
      bx cs cluster-get <cluster_name_or_ID> | grep "Ingress subdomain"
      ```
      {: pre}

    2.  Ejecute ping sobre el subdominio de ALB.

      ```
      ping <ingress_subdomain>
      ```
      {: pre}

    3.  Recupere la dirección IP pública del ALB.

      ```
      nslookup <ingress_subdomain>
      ```
      {: pre}

    4.  Ejecute ping sobre la dirección IP pública del ALB.

      ```
      ping <ALB_IP>
      ```
      {: pre}

    Si la CLI devuelve un tiempo de espera para la dirección IP pública o subdominio del ALB y ha configurado un cortafuegos personalizado que protege los nodos trabajadores, abra más puertos y grupos de redes en el [cortafuegos](cs_troubleshoot_clusters.html#cs_firewall).

3.  Si utiliza un dominio personalizado, asegúrese de que el dominio personalizado está correlacionado con la dirección IP pública o subdominio del ALB proporcionado por IBM con el proveedor de DNS.
    1.  Si ha utilizado el subdominio del ALB, compruebe el registro del nombre canónico (CNAME).
    2.  Si ha utilizado la dirección IP pública del ALB, compruebe que el dominio personalizado esté correlacionado con la dirección IP pública portátil del registro de puntero
(PTR).
4.  Compruebe el archivo de configuración del recurso de Ingress.

    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingress
    spec:
      tls:
      - hosts:
        - <ingress_subdomain>
        secretName: <ingress_tls_secret>
      rules:
      - host: <ingress_subdomain>
        http:
          paths:
          - path: /
        backend:
          serviceName: myservice
          servicePort: 80
    ```
    {: codeblock}

    1.  Compruebe que el subdominio del ALB y el certificado TLS sean correctos. Para encontrar el subdominio proporcionado por IBM y el certificado TLS, ejecute `bx cs cluster-get <cluster_name_or_ID>`.
    2.  Asegúrese de que su app está a la escucha en la misma vía de acceso que está configurada en la sección **path** de Ingress. Si la app se ha configurado para que escuche en la vía de acceso raíz, incluya **/** como vía de acceso.
5.  Compruebe el despliegue de Ingress y mire si hay algún mensaje de error o aviso.

    ```
    kubectl describe ingress <myingress>
    ```
    {: pre}

    Por ejemplo, en la sección de **sucesos** de la salida, es posible que vea mensajes de aviso sobre valores no válidos en el recurso de Ingress o en determinadas anotaciones que haya utilizado.

    ```
    Name:             myingress
    Namespace:        default
    Address:          169.xx.xxx.xxx,169.xx.xxx.xxx
    Default backend:  default-http-backend:80 (<none>)
    Rules:
      Host                                             Path  Backends
      ----                                             ----  --------
      mycluster.us-south.containers.appdomain.cloud
                                                       /tea      myservice1:80 (<none>)
                                                       /coffee   myservice2:80 (<none>)
    Annotations:
      custom-port:        protocol=http port=7490; protocol=https port=4431
      location-modifier:  modifier='~' serviceName=myservice1;modifier='^~' serviceName=myservice2
    Events:
      Type     Reason             Age   From                                                            Message
      ----     ------             ----  ----                                                            -------
      Normal   Success            1m    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  TLSSecretNotFound  1m    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress resource.
      Normal   Success            59s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  AnnotationError    40s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress.bluemix.net/custom-port annotation. Error annotation format error : One of the mandatory fields not valid/missing for annotation ingress.bluemix.net/custom-port
      Normal   Success            40s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  AnnotationError    2s    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress.bluemix.net/custom-port annotation. Invalid port 7490. Annotation cannot use ports 7481 - 7490
      Normal   Success            2s    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
    ```
    {: screen}

6.  Compruebe los registros para su ALB.
    1.  Recupere el ID de los pods de Ingress que se ejecutan en el clúster.

      ```
      kubectl get pods -n kube-system | grep alb
      ```
      {: pre}

    2.  Recuperar los registros correspondientes a cada pod de Ingress.

      ```
      kubectl logs <ingress_pod_ID> nginx-ingress -n kube-system
      ```
      {: pre}

    3.  Mire si hay mensajes de error en los registros del ALB.

<br />


## Problemas con secretos del equilibrador de carga de aplicación de Ingress
{: #cs_albsecret_fails}

{: tsSymptoms}
Después de desplegar un secreto de equilibrador de carga de aplicación (ALB) de Ingress al clúster, el campo `Descripción` no se actualiza con el nombre de secreto al visualizar el certificado en {{site.data.keyword.cloudcerts_full_notm}}.

Cuando lista información sobre el secreto del ALB, el estado indica `*_failed`. Por ejemplo, `create_failed`, `update_failed`, `delete_failed`.

{: tsResolve}
Revise los motivos siguientes por los que puede fallar el secreto del ALB y los pasos de resolución de problemas correspondientes:

<table>
<caption>Resolución de problemas de secretos del equilibrador de carga de aplicación de Ingress</caption>
 <thead>
 <th>Por qué está ocurriendo</th>
 <th>Cómo solucionarlo</th>
 </thead>
 <tbody>
 <tr>
 <td>No dispone de los roles de acceso necesarios para descargar y actualizar los datos de certificado.</td>
 <td>Solicite al administrador de su cuenta que le asigne los roles de **Operador** y **Editor** para su instancia de {{site.data.keyword.cloudcerts_full_notm}}. Para obtener más información, consulte <a href="/docs/services/certificate-manager/access-management.html#managing-service-access-roles">Gestión del acceso del servicio</a> para {{site.data.keyword.cloudcerts_short}}.</td>
 </tr>
 <tr>
 <td>El CRN de certificado proporcionado en el momento de la creación, actualización o eliminación no pertenece a la misma cuenta que el clúster.</td>
 <td>Compruebe que el CRN de certificado que ha proporcionado se haya importado a una instancia del servicio de {{site.data.keyword.cloudcerts_short}} en la misma cuenta que su clúster.</td>
 </tr>
 <tr>
 <td>El CRN de certificado proporcionado en el momento de la creación no es correcto.</td>
 <td><ol><li>Compruebe que la serie de CRN de certificado es correcta.</li><li>Si el CRN de certificado no es correcto, intente actualizar el secreto: <code>bx cs alb-cert-deploy --update --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li><li>Si el resultado de este mandato es <code>update_failed</code>, elimine el secreto: <code>bx cs alb-cert-rm --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt;</code></li><li>Vuelva a desplegar el secreto: <code>bx cs alb-cert-deploy --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li></ol></td>
 </tr>
 <tr>
 <td>El CRN de certificado proporcionado en el momento de la actualización no es correcto.</td>
 <td><ol><li>Compruebe que la serie de CRN de certificado es correcta.</li><li>Si el CRN de certificado no es correcto, elimine el secreto: <code>bx cs alb-cert-rm --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt;</code></li><li>Vuelva a desplegar el secreto: <code>bx cs alb-cert-deploy --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li><li>Intente actualizar el secreto: <code>bx cs alb-cert-deploy --update --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li></ol></td>
 </tr>
 <tr>
 <td>El servicio {{site.data.keyword.cloudcerts_long_notm}} está experimentando un tiempo de inactividad.</td>
 <td>Compruebe que el servicio {{site.data.keyword.cloudcerts_short}} esté activo y en ejecución.</td>
 </tr>
 </tbody></table>

<br />


## No se puede obtener un subdominio para el ALB de Ingress
{: #cs_subnet_limit}

{: tsSymptoms}
Cuando ejecuta `bx cs cluster-get <cluster>`, el clúster está en un estado `normal`, sin embargo no hay disponible un **Subdominio de Ingress**.

Podría ver un mensaje de error similar al siguiente.

```
Ya hay el número máximo de subredes permitidas en esta VLAN.
```
{: screen}

{: tsCauses}
Cuando se crea un clúster, se solicitan 8 subredes portátiles públicas y 8 subredes portátiles privadas en la VLAN que especifique. En {{site.data.keyword.containershort_notm}}, las VLAN tienen un límite de 40 subredes. Si la VLAN del clúster ya ha alcanzado este límite, no se puede suministrar el **Subdominio de Ingress**.

Para ver cuántas subredes tiene una VLAN:
1.  En la [consola (SoftLayer) de la infraestructura de IBM Cloud](https://control.bluemix.net/), seleccione **Red** > **Gestión de IP** > **VLAN**.
2.  Pulse el **Número de VLAN** de la VLAN que utilizó para crear el clúster. Revise la sección **Subnets** para ver si hay 40 o más subredes.

{: tsResolve}
Si necesita una nueva VLAN, [póngase en contacto con el soporte de {{site.data.keyword.Bluemix_notm}}](/docs/get-support/howtogetsupport.html#getting-customer-support) para solicitar una. A continuación, [cree un clúster](cs_cli_reference.html#cs_cluster_create) que utilice esta nueva VLAN.

Si tiene otra VLAN que esté disponible, puede [configurar la expansión de la VLAN](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning) en el clúster existente. Después, puede añadir nuevos nodos trabajadores al clúster que utilicen otra VLAN con subredes disponibles.

Si no utiliza todas las subredes en la VLAN, puede reutilizar subredes en el clúster.
1.  Compruebe que las subredes que desea utilizar están disponibles. **Nota**: La cuenta de infraestructura que está utilizando podría compartirse entre varias cuentas de {{site.data.keyword.Bluemix_notm}}. Si es así, incluso si ejecuta el mandato `bx cs subnets` para ver subredes con **Clústeres enlazados**, solo puede ver información de sus clústeres. Compruebe con el propietario de la cuenta de infraestructura para asegurarse de que las subredes están disponibles y que no las estén utilizando otra cuenta o equipo.

2.  [Cree un clúster](cs_cli_reference.html#cs_cluster_create) con la opción `--no-subnet` para que el servicio no intente crear nuevas subredes. Especifique la ubicación y la VLAN de las subredes disponibles para ser reutilizadas.

3.  Utilice el [mandato](cs_cli_reference.html#cs_cluster_subnet_add) `bx cs cluster-subnet-add` para añadir subredes existentes a su clúster. Para obtener más información, consulte [Adición o reutilización de subredes existentes o personalizadas en clústeres de Kubernetes](cs_subnets.html#custom).

<br />


## No se puede establecer la conectividad de VPN con el diagrama de Helm de strongSwan
{: #cs_vpn_fails}

{: tsSymptoms}
Cuando comprueba la conectividad de VPN ejecutando `kubectl exec -n kube-system  $STRONGSWAN_POD -- ipsec status`, no ve el estado `ESTABLISHED`, o el pod de VPN está en estado `ERROR` o sigue bloqueándose y reiniciándose.

{: tsCauses}
El archivo de configuración del diagrama de Helm tiene valores incorrectos, errores de sintaxis o le faltan valores.

{: tsResolve}
Cuando intenta establecer la conectividad de VPN con el diagrama de Helm de strongSwan, es probable que el estado de VPN no sea `ESTABLISHED` la primera vez. Puede que necesite comprobar varios tipos de problema y cambiar el archivo de configuración en consonancia. Para resolver el problema de conectividad de VPN de strongSwan:

1. Compare los valores de punto final de VPN local con los valores del archivo de configuración. Si los valores no coinciden:

    <ol>
    <li>Suprima el diagrama de Helm existente.</br><pre class="codeblock"><code>helm delete --purge <release_name></code></pre></li>
    <li>Corrija los valores incorrectos en el archivo <code>config.yaml</code> y guarde el archivo actualizado.</li>
    <li>Instale el nuevo diagrama de Helm.</br><pre class="codeblock"><code>helm install -f config.yaml --namespace=kube-system --name=<release_name> bluemix/strongswan</code></pre></li>
    </ol>

2. Si el pod de VPN está en estado `ERROR` o sigue bloqueándose y reiniciándose, puede que se deba a la validación de parámetro de los valores de `ipsec.conf` en la correlación de configuración del diagrama.

    <ol>
    <li>Compruebe los errores de validación en los registros del pod de strongSwan.</br><pre class="codeblock"><code>kubectl logs -n kube-system $STRONGSWAN_POD</code></pre></li>
    <li>Si los registros contienen errores de validación, suprima el diagrama de Helm existente.</br><pre class="codeblock"><code>helm delete --purge <release_name></code></pre></li>
    <li>Corrija los valores incorrectos en el archivo `config.yaml` y guarde el archivo actualizado.</li>
    <li>Instale el nuevo diagrama de Helm.</br><pre class="codeblock"><code>helm install -f config.yaml --namespace=kube-system --name=<release_name> bluemix/strongswan</code></pre></li>
    </ol>

3. Ejecute las 5 pruebas de Helm incluidas en la definición del diagrama de strongSwan.

    <ol>
    <li>Ejecute las pruebas de Helm.</br><pre class="codeblock"><code>helm test vpn</code></pre></li>
    <li>Si falla alguna de las pruebas, consulte [Descripción de las pruebas de conectividad de VPN de Helm](cs_vpn.html#vpn_tests_table) para obtener información sobre cada prueba y por qué puede fallar. <b>Nota</b>: Algunas de las pruebas tienen requisitos que son valores opcionales en la configuración de VPN. Si alguna de las pruebas falla, los errores pueden ser aceptables en función de si ha especificado estos valores opcionales.</li>
    <li>Puede consultar la salida de una prueba que ha fallado en los registros del pod de prueba.<br><pre class="codeblock"><code>kubectl logs -n kube-system <test_program></code></pre></li>
    <li>Suprima el diagrama de Helm existente.</br><pre class="codeblock"><code>helm delete --purge <release_name></code></pre></li>
    <li>Corrija los valores incorrectos en el archivo <code>config.yaml</code> y guarde el archivo actualizado.</li>
    <li>Instale el nuevo diagrama de Helm.</br><pre class="codeblock"><code>helm install -f config.yaml --namespace=kube-system --name=<release_name> bluemix/strongswan</code></pre></li>
    <li>Para comprobar los cambios:<ol><li>Obtenga los pods de prueba actuales.</br><pre class="codeblock"><code>kubectl get pods -a -n kube-system -l app=strongswan-test</code></pre></li><li>Limpie los pods de prueba actuales.</br><pre class="codeblock"><code>kubectl delete pods -n kube-system -l app=strongswan-test</code></pre></li><li>Ejecute las pruebas de nuevo.</br><pre class="codeblock"><code>helm test vpn</code></pre></li>
    </ol></ol>

4. Ejecute la herramienta de depuración de VPN incluida en el paquete de la imagen del pod de VPN.

    1. Establezca la variable de entorno `STRONGSWAN_POD`.

        ```
        export STRONGSWAN_POD=$(kubectl get pod -n kube-system -l app=strongswan,release=vpn -o jsonpath='{ .items[0].metadata.name }')
        ```
        {: pre}

    2. Ejecute la herramienta de depuración.

        ```
        kubectl exec -n kube-system  $STRONGSWAN_POD -- vpnDebug
        ```
        {: pre}

        La herramienta genera varias páginas de información, ya que ejecuta varias pruebas para problemas comunes de red. Las líneas de la salida que empiezan por `ERROR`, `WARNING`, `VERIFY` o `CHECK` indican posibles errores con la conectividad de VPN.

    <br />


## La conectividad de VPN de strongSwan falla después de añadir o suprimir nodos trabajadores
{: #cs_vpn_fails_worker_add}

{: tsSymptoms}
Ha establecido previamente una conexión VPN activa utilizando el servicio VPN IPSec de strongSwan. Sin embargo, después de haber añadido o suprimido un nodo trabajador en el clúster, aparecen uno o varios de los siguientes síntomas:

* El estado de la VPN no es `ESTABLISHED`
* No se puede acceder a los nuevos nodos trabajadores desde la red local
* No se puede acceder a la red remota desde los pods que se ejecutan en los nuevos nodos trabajadores

{: tsCauses}
Si ha añadido un nodo trabajador:

* El nodo trabajador se ha suministrado en una nueva subred privada que no se expone a través de la conexión VPN existente mediante los valores `localSubnetNAT` o `local.subnet`
* las rutas de VPN no pueden añadirse al nodo trabajador porque el trabajador tiene antagonismos o etiquetas que no están incluidas en los valores actuales de `tolerations` o `nodeSelector`
* El pod de VPN se ejecuta en el nodo trabajador nuevo, pero la dirección IP pública de dicho nodo trabajador no está permitida por el cortafuegos local

Si ha suprimido un nodo trabajador:

* Ese nodo trabajador era el único nodo en el que se estaba ejecutando un pod de VPN, debido a las restricciones en algunos antagonismos o etiquetas en los valores existentes de `tolerations` o `nodeSelector`

{: tsResolve}
Actualice los valores de diagrama de Helm para reflejar los cambios del nodo trabajador:

1. Suprima el diagrama de Helm existente.

    ```
    helm delete --purge <release_name>
    ```
    {: pre}

2. Abra el archivo de configuración para el servicio VPN de strongSwan.

    ```
    helm inspect values ibm/strongswan > config.yaml
    ```
    {: pre}

3. Compruebe los valores siguientes y cambie los valores para reflejar los nodos trabajadores añadidos o suprimidos necesarios.

    Si ha añadido un nodo trabajador:

    <table>
    <caption>Valores de nodo trabajador</caption>
     <thead>
     <th>Valor</th>
     <th>Descripción</th>
     </thead>
     <tbody>
     <tr>
     <td><code>localSubnetNAT</code></td>
     <td>El trabajador añadido puede estar desplegado en una subred privada nueva diferente a las demás subredes existentes en las que se encuentran otros nodos trabajadores. Si está utilizando NAT de subred para volver a correlacionar las direcciones IP locales privadas del clúster y el trabajador se añade a una nueva subred, añada el CIDR de la nueva subred a este valor.</td>
     </tr>
     <tr>
     <td><code>nodeSelector</code></td>
     <td>Si anteriormente ha limitado el despliegue pods de VPN a los trabajadores con una etiqueta específica, asegúrese de que el nodo trabajador añadido también tiene dicha etiqueta.</td>
     </tr>
     <tr>
     <td><code>tolerations</code></td>
     <td>Si el nodo trabajador añadido tiene antagonismos, cambie este valor para permitir que el pod de VPN se ejecute en todos los trabajadores con cualquier antagonismo o con antagonismos específicos.</td>
     </tr>
     <tr>
     <td><code>local.subnet</code></td>
     <td>El trabajador añadido puede estar desplegado en una subred privada nueva diferente a las subredes existentes en las que se encuentran otros trabajadores. Si las apps están expuestas por los servicios NodePort o LoadBalancer en la red privada y las apps están en el trabajador añadido, añada el CIDR de la nueva subred a este valor. **Nota**: Si añade valores a `local.subnet`, compruebe los valores de VPN para la subred local para ver si también deben actualizarse.</td>
     </tr>
     </tbody></table>

    Si ha suprimido un nodo trabajador:

    <table>
    <caption>Valores de nodo trabajador</caption>
     <thead>
     <th>Valor</th>
     <th>Descripción</th>
     </thead>
     <tbody>
     <tr>
     <td><code>localSubnetNAT</code></td>
     <td>Si está utilizando NAT de subred para volver a correlacionar direcciones IP locales privadas específicas, elimine las direcciones IP de esta configuración que pertenezcan al trabajador antiguo. Si está utilizando NAT de subred para volver a correlacionar subredes enteras y no quedan trabajadores en una subred, elimine dicho CIDR de subred de este valor.</td>
     </tr>
     <tr>
     <td><code>nodeSelector</code></td>
     <td>Si con anterioridad limitó el despliegue de pod de VPN a un trabajador individual y dicho trabajador fue suprimido, cambie este valor para permitir que el pod de VPN se ejecute en otros trabajadores.</td>
     </tr>
     <tr>
     <td><code>tolerations</code></td>
     <td>Si el trabajador que suprimió no poseía antagonismos, y los únicos trabajadores que quedan poseen antagonismos, cambie este valor para permitir que todos los pods de VPN se ejecuten en trabajadores con cualquier antagonismo o con antagonismos específicos.
     </td>
     </tr>
     </tbody></table>

4. Instale el nuevo diagrama de Helm con los valores actualizados.

    ```
    helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/strongswan
    ```
    {: pre}

5. Compruebe el estado de despliegue del diagrama. Cuando el diagrama está listo, el campo **STATUS**, situado cerca de la parte superior de la salida, tiene el valor `DEPLOYED`.

    ```
    helm status <release_name>
    ```
    {: pre}

6. En algunos casos, es posible que tenga que cambiar los valores locales y los valores del cortafuegos para que coincidan con los cambios realizados en el archivo de configuración de VPN.

7. Inicie la VPN.
    * Si el clúster inicia la conexión VPN (`ipsec.auto` se establece en `start`), inicie la VPN en la pasarela local y luego inicie la VPN en el clúster.
    * Si la pasarela local inicia la conexión VPN (`ipsec.auto` se establece en `auto`), inicie la VPN en el clúster y luego inicie la VPN en la pasarela local.

8. Establezca la variable de entorno `STRONGSWAN_POD`.

    ```
    export STRONGSWAN_POD=$(kubectl get pod -n kube-system -l app=strongswan,release=<release_name> -o jsonpath='{ .items[0].metadata.name }')
    ```
    {: pre}

9. Compruebe el estado de la VPN.

    ```
    kubectl exec -n kube-system  $STRONGSWAN_POD -- ipsec status
    ```
    {: pre}

    * Si la conexión VPN tiene el estado `ESTABLISHED`, la conexión VPN se ha realizado correctamente. No es necesario realizar ninguna otra acción.

    * Si sigue teniendo problemas de conexión, consulte [No se puede establecer la conectividad de VPN con el diagrama de Helm de strongSwan](#cs_vpn_fails) para solucionar más problemas de conexión VPN.

<br />



## No se pueden recuperar políticas de red de Calico
{: #cs_calico_fails}

{: tsSymptoms}
Cuando intenta ver las políticas de red de Calico en su clúster ejecutando `calicoctl get policy`, obtiene resultados no esperados o mensajes de error:
- Una lista vacía
- Una lista de políticas de Calico v2 antiguas en lugar de políticas de la v3
- `No se ha podido crear el cliente de API de Calico: error de sintaxis en calicoctl.cfg: archivo de configuración no válido: APIVersion desconocida 'projectcalico.org/v3'`

Cuando intenta ver las políticas de red de Calico en su clúster ejecutando `calicoctl get GlobalNetworkPolicy`, obtiene resultados no esperados o mensajes de error:
- Una lista vacía
- `No se ha podido crear el cliente de API de Calico: error de sintaxis en calicoctl.cfg: archivo de configuración no válido: APIVersion desconocida 'v1'`
- `No se ha podido crear el cliente de API de Calico: error de sintaxis en calicoctl.cfg: archivo de configuración no válido: APIVersion desconocida 'projectcalico.org/v3'`
- `No se ha podido obtener recursos: No se da soporte al tipo de recurso 'GlobalNetworkPolicy'`

{: tsCauses}
Para utilizar políticas de Calico, se deben alinear cuatro factores: la versión de Kubernetes del clúster, la versión de la CLI de Calico, la sintaxis del archivo de configuración de Calico y los mandatos de visualización de política. Uno o más de estos factores no están en la versión correcta.

{: tsResolve}
Cuando el clúster está en [Kubernetes versión 1.10 o posterior](cs_versions.html), debe utilizar Calico CLI v3.1, la sintaxis del archivo de configuración `calicoctl.cfg` v3 y los mandatos `calicoctl get GlobalNetworkPolicy` y `calicoctl get NetworkPolicy`.

Cuando el clúster está en [Kubernetes versión 1.9 o anterior](cs_versions.html), debe utilizar Calico CLI v1.6.3, la sintaxis del archivo de configuración `calicoctl.cfg` v2 y el mandato `calicoctl get policy`.

Para asegurarse de que se han alineado todos los factores de Calico:

1. Visualice la versión de Kubernetes del clúster.
    ```
    bx cs cluster-get <cluster_name>
    ```
    {: pre}

    * Si el clúster está en Kubernetes versión 1.10 o posterior:
        1. [Instale y configure la CLI de Calico versión 3.1.1](cs_network_policy.html#1.10_install). La configuración incluye la actualización manual del archivo `calicoctl.cfg` para utilizar la sintaxis de Calico v3.
        2. Asegúrese de que las políticas que desea crear y aplicar al clúster utilizan la [sintaxis Calico v3 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/networkpolicy). Si tiene un archivo de política `.yaml` o `.json` existente en sintaxis de Calico v2, puede convertirlo a la sintaxis de Calico v3 utilizando el mandato [`calicoctl convert` ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.projectcalico.org/v3.1/reference/calicoctl/commands/convert).
        3. Para [visualizar políticas](cs_network_policy.html#1.10_examine_policies), asegúrese de utilizar `calicoctl get GlobalNetworkPolicy` para políticas globales y `calicoctl get NetworkPolicy --namespace <policy_namespace>` para políticas cuyo ámbito sean espacios de nombres específicos.

    * Si el clúster está en Kubernetes versión 1.9 o anterior:
        1. [Instale y configure la CLI de Calico versión 1.6.3](cs_network_policy.html#1.9_install). Asegúrese de que el archivo `calicoctl.cfg` utiliza la sintaxis de Calico v2.
        2. Asegúrese de que todas las políticas que crea y desea aplicar al clúster utilizan la [sintaxis de Calico v2 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.projectcalico.org/v2.6/reference/calicoctl/resources/policy).
        3. Para [visualizar políticas](cs_network_policy.html#1.9_examine_policies), asegúrese de utilizar `calicoctl get policy`.

Antes de actualizar su clúster desde Kubernetes versión 1.9 o anterior a la versión 1.10 o posterior, consulte [Preparación para actualizar a Calico V3](cs_versions.html#110_calicov3).
{: tip}

<br />


## Obtención de ayuda y soporte
{: #ts_getting_help}

¿Sigue teniendo problemas con su clúster?
{: shortdesc}

-   Para ver si {{site.data.keyword.Bluemix_notm}} está disponible, [consulte la página de estado de {{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.ibm.com/bluemix/support/#status).
-   Publique una pregunta en [{{site.data.keyword.containershort_notm}}Slack ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://ibm-container-service.slack.com).

    Si no utiliza un ID de IBM para la cuenta de {{site.data.keyword.Bluemix_notm}}, [solicite una invitación](https://bxcs-slack-invite.mybluemix.net/) a este Slack.
    {: tip}
-   Revise los foros para ver si otros usuarios se han encontrado con el mismo problema. Cuando utiliza los foros para formular una pregunta, etiquete la pregunta para que la puedan ver los equipos de desarrollo de {{site.data.keyword.Bluemix_notm}}.

    -   Si tiene preguntas técnicas sobre el desarrollo o despliegue de clústeres o apps con {{site.data.keyword.containershort_notm}}, publique su pregunta en [Stack Overflow ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) y etiquete su pregunta con `ibm-cloud`, `kubernetes` y `containers`.
    -   Para las preguntas relativas a las instrucciones de inicio y el servicio, utilice el foro [IBM developerWorks dW Answers ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix). Incluya las etiquetas `ibm-cloud` y `containers`.
    Consulte [Obtención de ayuda](/docs/get-support/howtogetsupport.html#using-avatar) para obtener más detalles sobre cómo utilizar los foros.

-   Póngase en contacto con el soporte de IBM abriendo una incidencia. Para obtener información sobre cómo abrir una incidencia de soporte de IBM, o sobre los niveles de soporte y las gravedades de las incidencias, consulte [Cómo contactar con el servicio de soporte](/docs/get-support/howtogetsupport.html#getting-customer-support).

{: tip}
Al informar de un problema, incluya el ID de clúster. Para obtener el ID de clúster, ejecute `bx cs clusters`.

