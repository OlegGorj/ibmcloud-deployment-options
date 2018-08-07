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




# Exposición de apps con Ingress
{: #ingress}

Exponga varias apps en el clúster de Kubernetes creando recursos Ingress gestionados por el equilibrador de carga de aplicación proporcionado por IBM en {{site.data.keyword.containerlong}}.
{:shortdesc}

## Gestión de tráfico de red utilizando Ingress
{: #planning}

Ingress es un servicio de Kubernetes que equilibra cargas de trabajo de tráfico de red en el clúster reenviando solicitudes públicas o privadas a sus apps. Puede utilizar Ingress para exponer varios servicios de app a la red privada o pública mediante una única ruta privada o pública.
{:shortdesc}



Ingress consta de dos componentes:
<dl>
<dt>Equilibrador de carga de aplicación (ALB)</dt>
<dd>El equilibrador de carga de aplicación (ALB) es un equilibrador de carga externo que escucha solicitudes del servicio de entrada HTTP, HTTPS, TCP o UDP y las reenvía al pod de app adecuado. Cuando se crea un clúster estándar, {{site.data.keyword.containershort_notm}} crea automáticamente un ALB altamente disponible para el clúster y le asigna una ruta pública exclusiva. La ruta pública se enlaza a una dirección IP pública portátil que se suministra a la cuenta de infraestructura de IBM Cloud (SoftLayer) durante la creación del clúster. También se crea automáticamente un ALB privado predeterminado, pero no se habilita automáticamente.</dd>
<dt>Recurso de Ingress</dt>
<dd>Para exponer una app mediante Ingress, debe crear un servicio Kubernetes para la app y registrar este servicio con el ALB mediante la definición de un recurso de Ingress. El recurso de Ingress es un recurso de Kubernetes que define las reglas sobre cómo direccionar solicitudes de entrada para una app. El recurso de Ingress también especifica la vía de acceso a su servicio de app, que se añade a la ruta pública para formar un URL de app exclusivo como, por ejemplo, `mycluster.us-south.containers.appdomain.cloud/myapp`.
<br></br><strong>Nota</strong>: Desde el 24 de mayo de 2018, el formato de subdominio de Ingress ha cambiado para nuevos clústeres.<ul><li>Los clústeres creados después del 24 de mayo de 2018 son asignados a un subdominio en el nuevo formato, <code>&lt;cluster_name&gt;.&lt;region&gt;.containers.appdomain.cloud</code>.</li><li>Los clústeres creados antes del 24 de mayo de 2018 continúan utilizando el subdominio asignado en el formato antiguo, <code>&lt;cluster_name&gt;.&lt;region&gt;.containers.mybluemix.net</code>.</li></ul></dd>
</dl>

El siguiente diagrama muestra cómo se dirige la comunicación de Ingress desde Internet a una app:

<img src="images/cs_ingress.png" width="550" alt="Exposición de una app en {{site.data.keyword.containershort_notm}} utilizando Ingress" style="width:550px; border-style: none"/>

1. Un usuario envía una solicitud a la app accediendo al URL de la app. Este URL es el URL público para la app expuesta al que se añade la vía de acceso al recurso de Ingress como, por ejemplo, `mycluster.us-south.containers.appdomain.cloud/myapp`.

2. Un servicio del sistema DNS que actúa como equilibrador de carga global resuelve el URL a la dirección IP pública portátil del ALB público predeterminado en el clúster. Lo solicitud se direcciona al servicio del ALB de Kubernetes para la app.

3. El servicio de Kubernetes direcciona la solicitud al ALB.

4. El ALB comprueba si existe una regla de direccionamiento para la vía de acceso `myapp` en el clúster. Si se encuentra una regla coincidente, la solicitud se reenvía al pod en el que se ha desplegado la app, de acuerdo con las reglas que ha definido en el recurso de Ingress. Si se despliegan varias instancias de app en el clúster, el ALB equilibra la carga de las solicitudes entre los pods de app.



<br />


## Requisitos previos
{: #config_prereqs}

Antes de empezar a trabajar con Ingress, revise los siguientes requisitos previos.
{:shortdesc}

**Requisitos previos para todas las configuraciones de Ingress:**
- Ingress únicamente está disponible para los clústeres estándares y necesita de al menos dos nodos trabajadores en el clúster para asegurar la alta disponibilidad.
- Para configurar un servicio Ingress se necesita una [política de acceso de administrador](cs_users.html#access_policies). Verifique su [política de acceso](cs_users.html#infra_access) actual.



<br />


## Planificación de red para espacios de nombres múltiples o individuales
{: #multiple_namespaces}

Se necesita al menos un recurso Ingress por espacio de nombres donde tenga apps que desee exponer.
{:shortdesc}

<dl>
<dt>Todas las apps están en un espacio de nombres</dt>
<dd>Si las apps en el clúster están en el mismo espacio de nombres, como mínimo se necesita un recurso de Ingress para definir las reglas de direccionamiento para las apps allí expuestas. Por ejemplo, si los servicios en un espacio de nombres de desarrollo exponen las apps `app1` y `app2`, puede crear un recurso de Ingress en el espacio de nombres. El recurso especifica `domain.net` como host y registra las vías que cada app escucha con `domain.net`.
<br></br><img src="images/cs_ingress_single_ns.png" width="300" alt="Como mínimo se necesita un recurso por espacio de nombres." style="width:300px; border-style: none"/>
</dd>
<dt>Las apps están en varios espacios de nombres</dt>
<dd>Si las apps en el clúster están en distintos espacios de nombres, se debe crear al menos un recurso por espacio de nombres para definir reglas para las apps allí expuestas. Para registrar varios recursos de Ingress con el ALB de Ingress del clúster, debe utilizar un dominio comodín. Cuando se registra un dominio comodín como `*.mycluster.us-south.containers.appdomain.cloud`, varios subdominios se resuelven todos ellos al mismo host. A continuación, puede crear un recurso de Ingress en cada espacio de nombres y especificar un subdominio diferente en cada recurso de Ingress.
<br><br>
Por ejemplo, considere el escenario siguiente:<ul>
<li>Tiene dos versiones de la misma app, `app1` y `app3`, para fines de prueba.</li>
<li>Despliega las apps en dos espacios de nombres diferentes dentro del mismo clúster: `app1` en el espacio de nombres de desarrollo y `app3` en el espacio de nombres de transferencia.</li></ul>
Para utilizar el mismo ALB de clúster para gestionar el tráfico a estas apps, puede crear los siguientes servicios y recursos:<ul>
<li>Un servicio Kubernetes en el espacio de nombres de desarrollo para exponer `app1`.</li>
<li>Un recurso de Ingress en el espacio de nombres de desarrollo que especifica el host como `dev.mycluster.us-south.containers.appdomain.cloud`.</li>
<li>Un servicio Kubernetes en el espacio de nombres de transferencia para exponer `app3`.</li>
<li>Un recurso de Ingress en el espacio de nombres de transferencia que especifica el host como `stage.mycluster.us-south.containers.appdomain.cloud`.</li></ul></br>
<img src="images/cs_ingress_multi_ns.png" alt="Dentro de un espacio de nombres, utilice subdominios en uno o varios recursos" style="border-style: none"/>Ahora, ambos URL se resuelven al mismo dominio y, por lo tanto, el mismo ALB da servicio a ambos URL. Sin embargo, puesto que el recurso en el espacio de nombres de transferencia está registrado con el subdominio `stage`, el ALB de Ingress direcciona correctamente las solicitudes desde el URL `stage.mycluster.us-south.containers.appdomain.cloud/app3` únicamente a `app3`.</dd>
</dl>

**Nota**:
* El comodín de subdominio de Ingress que IBM proporciona, `*.<cluster_name>.<region>.containers.appdomain.cloud`, está registrado de forma predeterminada para su clúster. Sin embargo, no se da soporte a TLS para el comodín de subdominio de Ingress que IBM proporciona.
* Si desea utilizar un dominio personalizado, debe registrar el dominio personalizado como un dominio comodín como, por ejemplo, `*.custom_domain.net`. Para utilizar TLS, debe obtener un certificado comodín.

### Varios dominios dentro del mismo espacio de nombres

En un espacio de nombres individual, puede utilizar un dominio para acceder a todas las apps en el espacio de nombres. Si desea utilizar diferentes dominios para las apps dentro de un espacio de nombres individual, utilice un dominio comodín. Cuando se registra un dominio comodín como `*.mycluster.us-south.containers.appdomain.cloud`, varios subdominios se resuelven todos ellos al mismo host. A continuación, puede utilizar un recurso para especificar varios hosts de subdominio dentro de dicho recurso. Como alternativa, puede crear varios recursos de Ingress en el espacio de nombres y especificar un subdominio diferente en cada recurso de Ingress.

<img src="images/cs_ingress_single_ns_multi_subs.png" alt="Como mínimo se necesita un recurso por espacio de nombres." style="border-style: none"/>

**Nota**:
* El comodín de subdominio de Ingress que IBM proporciona, `*.<cluster_name>.<region>.containers.appdomain.cloud`, está registrado de forma predeterminada para su clúster. Sin embargo, no se da soporte a TLS para el comodín de subdominio de Ingress que IBM proporciona.
* Si desea utilizar un dominio personalizado, debe registrar el dominio personalizado como un dominio comodín como, por ejemplo, `*.custom_domain.net`. Para utilizar TLS, debe obtener un certificado comodín.

<br />


## Exposición de apps que están dentro de su clúster al público
{: #ingress_expose_public}

Exponga apps que están dentro de su clúster al público utilizando el ALB de Ingress público.
{:shortdesc}

Antes de empezar:

-   Revise los [requisitos previos](#config_prereqs) de Ingress.
-   Si aún no tiene uno, [cree un clúster estándar](cs_clusters.html#clusters_ui).
-   [Defina su clúster como destino de la CLI](cs_cli_install.html#cs_cli_configure) para ejecutar mandatos `kubectl`.

### Paso 1: Desplegar apps y crear servicios de app
{: #public_inside_1}

Empiece desplegando sus apps y crear servicios de Kubernetes para exponerlos.
{: shortdesc}

1.  [Despliegue la app en el clúster](cs_app.html#app_cli). Asegúrese de añadir una etiqueta a su despliegue en la sección de metadatos del archivo de configuración como, por ejemplo, `app: code`. Esta etiqueta es necesaria para identificar todos los pods en los que se está ejecutando la app, de modo que puedan incluirse dichos pods en el equilibrio de carga de Ingress.

2.   Cree un servicio de Kubernetes para cada app que desee exponer. El servicio de Kubernetes debe exponer la app para incluirla en el ALB de clúster en el equilibrio de carga de Ingress.
      1.  Abra el editor que prefiera y cree un archivo de configuración de servicio llamado, por ejemplo, `myapp_service.yaml`.
      2.  Defina un servicio para la app que el ALB expondrá.

          ```
          apiVersion: v1
          kind: Service
          metadata:
            name: myapp_service
          spec:
            selector:
              <selector_key>: <selector_value>
            ports:
             - protocol: TCP
               port: 8080
          ```
          {: codeblock}

          <table>
          <thead>
          <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes del archivo YAML del servicio del ALB</th>
          </thead>
          <tbody>
          <tr>
          <td><code>selector</code></td>
          <td>Especifique el par de clave de etiqueta (<em>&lt;selector_key&gt;</em>) y valor (<em>&lt;selector_value&gt;</em>) que desee utilizar para establecer como destino los pods en los que se ejecuta la app. Para especificar e incluir sus pods en el equilibrio de carga del servicio, asegúrese de que <em>&lt;selector_key&gt;</em> y <em>&lt;selector_value&gt;</em> coinciden con el par de clave/valor en la sección <code>spec.template.metadata.labels</code> de su despliegue yaml.</td>
           </tr>
           <tr>
           <td><code>port</code></td>
           <td>El puerto que en el que está a la escucha el servicio.</td>
           </tr>
           </tbody></table>
      3.  Guarde los cambios.
      4.  Cree el servicio en el clúster. Si las apps están desplegadas en varios espacios de nombres en el clúster, asegúrese de que el servicio se despliega en el mismo espacio de nombres que la app que desea exponer.

          ```
          kubectl apply -f myapp_service.yaml [-n <namespace>]
          ```
          {: pre}
      5.  Repita estos pasos para cada app que desee exponer.


### Paso 2: Seleccionar un dominio de app y terminación TLS
{: #public_inside_2}

Cuando configure el ALB público, elija el dominio en el que sus apps serán accesibles y si desea utilizar la terminación TLS.
{: shortdesc}

<dl>
<dt>Dominio</dt>
<dd>Puede utilizar el dominio proporcionado por IBM, como por ejemplo <code>mycluster-12345.us-south.containers.appdomain.cloud/myapp</code>, para acceder a su app desde Internet. Para en su lugar utilizar un dominio personalizado, puede correlacionar su dominio personalizado con el dominio proporcionado por IBM o la dirección IP pública del ALB.</dd>
<dt>Terminación TLS</dt>
<dd>El ALB equilibra carga de tráfico de red HTTP para las apps en su clúster. Para también equilibrar la carga de conexiones HTTPS entrantes, puede configurar el ALB para descifrar el tráfico de red y reenviar las solicitudes descifradas a las apps expuestas en su clúster. Si está utilizando el subdominio de Ingress proporcionado por IBM, puede utilizar el certificado TLS proporcionado por IBM. Actualmente no se da soporte a TLS para los subdominios comodín que IBM proporciona. Si utiliza un dominio personalizado, puede utilizar su propio certificado TLS para gestionar la terminación TLS.</dd>
</dl>

Para utilizar el dominio de Ingress proporcionado por IBM:
1. Obtenga los detalles del clúster. Sustituya _&lt;cluster_name_or_ID&gt;_ con el nombre del clúster en el que se despliegan las apps que desea exponer.

    ```
    bx cs cluster-get <cluster_name_or_ID>
    ```
    {: pre}

    Salida de ejemplo:

    ```
    Name:                   mycluster
    ID:                     18a61a63c6a94b658596ca93d087aad9
    State:                  normal
    Created:                2018-01-12T18:33:35+0000
    Location:               dal10
    Master URL:             https://169.xx.xxx.xxx:26268
    Ingress Subdomain:      mycluster-12345.us-south.containers.appdomain.cloud
    Ingress Secret:         <tls_secret>
    Workers:                3
    Version:                1.9.7
    Owner Email:            owner@email.com
    Monitoring Dashboard:   <dashboard_URL>
    ```
    {: screen}
2. Obtenga el dominio proporcionado por IBM en el campo **Subdominio de Ingress**. Si desea utilizar TLS, obtenga también el secreto de TLS proporcionado por IBM en el campo **Secreto Ingress**.
    **Nota**: Si está utilizando un subdominio comodín, no se da soporte a TLS.

Para utilizar un dominio personalizado:
1.    Cree un dominio personalizado. Para registrar un dominio personalizado, póngase en contacto con su proveedor de DNS (Domain Name Service) o con [{{site.data.keyword.Bluemix_notm}} DNS](/docs/infrastructure/dns/getting-started.html#getting-started-with-dns).
      * Si las apps que desea que Ingress exponga se encuentran en diferentes espacios de nombres en un clúster, registre el dominio personalizado como un dominio comodín como, por ejemplo, `*.custom_domain.net`.

2.  Configure el dominio de modo que direccione el tráfico de red de entrada al ALB proporcionado por IBM. Puede elegir entre las siguientes opciones:
    -   Defina un alias para el dominio personalizado especificando el dominio proporcionado por IBM como CNAME (Registro de nombre canónico). Para encontrar el dominio de Ingress proporcionado por IBM, ejecute `bx cs cluster-get <cluster_name>` y busque el campo **Subdominio de Ingress**.
    -   Correlacione el dominio personalizado con la dirección IP pública portátil del ALB proporcionado por IBM añadiendo la dirección IP como registro. Para buscar la dirección IP pública portátil del ALB, ejecute `bx cs alb-get <public_alb_ID>`.
3.   Opcional: Si desea utilizar TLS, importe o cree un certificado TLS y un secreto de clave. Si utiliza un dominio comodín, asegúrese de importar o crear un certificado comodín.
      * Si hay un certificado TLS almacenado en {{site.data.keyword.cloudcerts_long_notm}} que desea utilizar, puede importar su secreto asociado a su clúster ejecutando el siguiente mandato:
        ```
        bx cs alb-cert-deploy --secret-name <secret_name> --cluster <cluster_name_or_ID> --cert-crn <certificate_crn>
        ```
        {: pre}
      * Si no tiene un certificado TLS listo, siga estos pasos:
        1. Cree un certificado TLS y una clave para el dominio que estén codificados en formato PEM.
        2. Cree un secreto que utilice su certificado TLS y clave. Sustituya <em>&lt;tls_secret_name&gt;</em> con un nombre para su secreto de Kubernetes, <em>&lt;tls_key_filepath&gt;</em> con la vía de acceso a su archivo de claves TLS personalizado y <em>&lt;tls_cert_filepath&gt;</em> con la vía de acceso al archivo de certificado TLS personalizado.
          ```
          kubectl create secret tls <tls_secret_name> --key <tls_key_filepath> --cert <tls_cert_filepath>
          ```
          {: pre}


### Paso 3: Crear el recurso de Ingress
{: #public_inside_3}

Los recursos de Ingress definen las reglas de direccionamiento que los ALB utilizan para direccionar el tráfico a su servicio de app.
{: shortdesc}

**Nota:** Si el clúster tiene varios espacios de nombres donde se exponen las apps, como mínimo se necesita un recurso de Ingress por espacio de nombres. Sin embargo, cada espacio de nombres debe utilizar un host diferente. Debe registrar un dominio comodín y especificar un subdominio diferente en cada recurso. Para obtener más información, consulte [Planificación de redes para uno o varios espacios de nombres](#multiple_namespaces).

1. Abra el editor que prefiera y cree un archivo de configuración de Ingress llamado, por ejemplo, `myingressresource.yaml`.

2. Defina un recurso de Ingress en el archivo de configuración que utilice el dominio proporcionado por IBM o su dominio personalizado para direccionar el tráfico de entrada de red a los servicios que ha creado anteriormente.

    YAML de ejemplo que no utiliza TLS:
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingressresource
    spec:
      rules:
      - host: <domain>
        http:
          paths:
          - path: /<app1_path>
            backend:
              serviceName: <app1_service>
              servicePort: 80
          - path: /<app2_path>
            backend:
              serviceName: <app2_service>
              servicePort: 80
    ```
    {: codeblock}

    YAML de ejemplo que utiliza TLS:
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingressresource
    spec:
      tls:
      - hosts:
        - <domain>
        secretName: <tls_secret_name>
      rules:
      - host: <domain>
        http:
          paths:
          - path: /<app1_path>
            backend:
              serviceName: <app1_service>
              servicePort: 80
          - path: /<app2_path>
            backend:
              serviceName: <app2_service>
              servicePort: 80
    ```
    {: codeblock}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general de los componentes del archivo YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code>tls/hosts</code></td>
    <td>Para utilizar TLS, sustituya <em>&lt;domain&gt;</em> con el subdominio de Ingress proporcionado por IBM o su dominio personalizado.

    </br></br>
    <strong>Nota:</strong><ul><li>Si sus apps están expuestas por los servicios en diferentes espacios de nombres en un clúster, añada un subdominio comodín al principio del dominio como, por ejemplo, `subdomain1.custom_domain.net` o `subdomain1.mycluster.us-south.containers.appdomain.cloud`. Utilice un subdominio exclusivo para cada recurso que cree en el clúster.</li><li>No utilice &ast; para el host ni deje la propiedad host vacía para evitar errores durante la creación de Ingress.</li></ul></td>
    </tr>
    <tr>
    <td><code>tls/secretName</code></td>
    <td><ul><li>Si está utilizando el dominio de Ingress proporcionado por IBM, sustituya <em>&lt;tls_secret_name&gt;</em> con el nombre del secreto de Ingress proporcionado por IBM.</li><li>Si utiliza un dominio personalizado, sustituya <em>&lt;tls_secret_name&gt;</em> con el secreto que ha creado anteriormente que aloja la clave y el certificado TLS personalizados. Si ha importado un certificado de {{site.data.keyword.cloudcerts_short}}, puede ejecutar <code>bx cs alb-cert-get --cluster <cluster_name_or_ID> --cert-crn <certificate_crn></code> para ver los secretos que están asociados con un certificado TLS.</li><ul><td>
    </tr>
    <tr>
    <td><code>host</code></td>
    <td>Sustituya <em>&lt;domain&gt;</em> con el subdominio de Ingress proporcionado por IBM o su dominio personalizado.

    </br></br>
    <strong>Nota:</strong><ul><li>Si sus apps están expuestas por los servicios en diferentes espacios de nombres en un clúster, añada un subdominio comodín al principio del dominio como, por ejemplo, `subdomain1.custom_domain.net` o `subdomain1.mycluster.us-south.containers.appdomain.cloud`. Utilice un subdominio exclusivo para cada recurso que cree en el clúster.</li><li>No utilice &ast; para el host ni deje la propiedad host vacía para evitar errores durante la creación de Ingress.</li></ul></td>
    </tr>
    <tr>
    <td><code>path</code></td>
    <td>Sustituya <em>&lt;app_path&gt;</em> con una barra inclinada o con la vía de acceso en la que su app está a la escucha. La vía de acceso se añade al dominio personalizado o proporcionado por IBM para crear una ruta exclusiva a su app. Cuando especifica esta ruta en un navegador web, el tráfico de la red se direcciona al ALB. El ALB consulta el servicio asociado y envía el tráfico de red al servicio. A continuación, el servicio reenvía el tráfico a los pods en los que la app está en ejecución.
    </br></br>
    Muchas apps no escuchan en una vía de acceso específica, sino que utilizan la vía de acceso raíz y un puerto específico. En este caso, defina la vía de acceso raíz como <code>/</code> y no especifique una vía de acceso individual para la app. Ejemplos: <ul><li>Para <code>http://domain/</code>, especifique <code>/</code> como la vía de acceso.</li><li>Para <code>http://domain/app1_path</code>, especifique <code>/app1_path</code> como la vía de acceso.</li></ul>
    </br>
    <strong>Sugerencia:</strong> Para configurar Ingress para que escuche en una vía de acceso que sea distinta a la vía de acceso en la que escucha la app, utilice la [anotación de reescritura](cs_annotations.html#rewrite-path).</td>
    </tr>
    <tr>
    <td><code>serviceName</code></td>
    <td>Sustituya <em>&lt;app1_service&gt;</em> y <em>&lt;app2_service&gt;</em> y así sucesivamente con el nombre de los servicios que ha creado para exponer sus apps. Si sus apps están expuestas por los servicios en diferentes espacios de nombres en el clúster, incluya únicamente los servicios de app que se encuentran en el mismo espacio de nombres. Debe crear un recurso de Ingress para cada espacio de nombres donde tiene las apps que desea exponer.</td>
    </tr>
    <tr>
    <td><code>servicePort</code></td>
    <td>El puerto en el que el servicio está a la escucha. Utilice el mismo puerto que ha definido al crear el servicio de Kubernetes para la app.</td>
    </tr>
    </tbody></table>

3.  Cree el recurso de Ingress para el clúster. Asegúrese de que el recurso se despliega en el mismo espacio de nombres que los servicios de apps que ha especificado en el recurso.

    ```
    kubectl apply -f myingressresource.yaml -n <namespace>
    ```
    {: pre}
4.   Verifique que el recurso de Ingress se haya creado correctamente.

      ```
      kubectl describe ingress myingressresource
      ```
      {: pre}

      1. Si los mensajes en los sucesos describen un error en la configuración del recurso, cambie los valores en el archivo de recursos y aplique de nuevo el archivo del recurso.


El recurso de Ingress se crea en el mismo espacio de nombres que los servicios de las apps. Sus apps en este espacio de nombres se registran con el ALB de Ingress del clúster.

### Paso 4: Acceder a la app desde Internet
{: #public_inside_4}

En un navegador web, escriba el URL del servicio de la app al que va a acceder.

```
https://<domain>/<app1_path>
```
{: pre}

Si tiene varias apps expuestas, acceda a dichas apps cambiando la vía de acceso que se añade al final del URL.

```
https://<domain>/<app2_path>
```
{: pre}

Si utiliza un dominio comodín para exponer apps en diferentes espacios de nombres, acceda a dichas apps con sus respectivos subdominios.

```
http://<subdomain1>.<domain>/<app1_path>
```
{: pre}

```
http://<subdomain2>.<domain>/<app1_path>
```
{: pre}


<br />


## Exposición de apps que están fuera de su clúster al público
{: #external_endpoint}

Exponga apps que están fuera de su clúster al público incluyéndolas en el equilibrio de carga del ALB de Ingress público. Las solicitudes públicas entrantes en el dominio personalizado o proporcionado por IBM se reenvían automáticamente a la app externa.
{:shortdesc}

Antes de empezar:

-   Revise los [requisitos previos](#config_prereqs) de Ingress.
-   Si aún no tiene uno, [cree un clúster estándar](cs_clusters.html#clusters_ui).
-   [Defina su clúster como destino de la CLI](cs_cli_install.html#cs_cli_configure) para ejecutar mandatos `kubectl`.
-   Asegúrese de que se pueda acceder a la app externa que desea incluir en el equilibrio de la carga del clúster mediante una dirección IP pública.

### Paso 1: Crear un servicio de app y un punto final externo
{: #public_outside_1}

Empiece por crear un servicio Kubernetes para exponer la app externa y configurar un punto final externo de Kubernetes para la app.
{: shortdesc}

1.  Cree un servicio de Kubernetes para el clúster que enviará las solicitudes entrantes a un punto final externo que habrá creado.
    1.  Abra el editor que prefiera y cree un archivo de configuración de servicio llamado, por ejemplo, `myexternalservice.yaml`.
    2.  Defina un servicio para la app que el ALB expondrá.

        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: myexternalservice
        spec:
          ports:
           - protocol: TCP
             port: 8080
        ```
        {: codeblock}

        <table>
        <caption>Visión general de los componentes de archivo del servicio ALB</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general de los componentes del archivo YAML</th>
        </thead>
        <tbody>
        <tr>
        <td><code>metadata/name</code></td>
        <td>Sustituya <em>&lt;myexternalservice&gt;</em> por el nombre del servicio.<p>Obtenga más información sobre cómo [proteger su información personal](cs_secure.html#pi) cuando se trabaja recursos de Kubernetes.</p></td>
        </tr>
        <tr>
        <td><code>port</code></td>
        <td>El puerto que en el que está a la escucha el servicio.</td>
        </tr></tbody></table>

    3.  Guarde los cambios.
    4.  Cree el servicio de Kubernetes para el clúster.

        ```
        kubectl apply -f myexternalservice.yaml
        ```
        {: pre}
2.  Configure un punto final de Kubernetes que defina la ubicación externa de la app que desea incluir en el equilibrio de la carga del clúster.
    1.  Abra el editor que prefiera y cree un archivo de configuración de punto final llamado, por ejemplo, `myexternalendpoint.yaml`.
    2.  Defina el punto final externo. Incluya todas las direcciones IP públicas y puertos que puede utilizar para acceder a la app externa.

        ```
        kind: Endpoints
        apiVersion: v1
        metadata:
          name: myexternalendpoint
        subsets:
          - addresses:
              - ip: <external_IP1>
              - ip: <external_IP2>
            ports:
              - port: <external_port>
        ```
        {: codeblock}

        <table>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general de los componentes del archivo YAML</th>
        </thead>
        <tbody>
        <tr>
        <td><code>name</code></td>
        <td>Sustituya <em>&lt;myexternalendpoint&gt;</em> por el nombre del servicio de Kubernetes que ha creado anteriormente.</td>
        </tr>
        <tr>
        <td><code>ip</code></td>
        <td>Sustituya <em>&lt;external_IP&gt;</em> por las direcciones IP públicas que desea conectar con la app externa.</td>
         </tr>
         <td><code>port</code></td>
         <td>Sustituya <em>&lt;external_port&gt;</em> por el puerto en el que escucha la app externa.</td>
         </tbody></table>

    3.  Guarde los cambios.
    4.  Cree el punto final de Kubernetes para el clúster.

        ```
        kubectl apply -f myexternalendpoint.yaml
        ```
        {: pre}

### Paso 2: Seleccionar un dominio de app y terminación TLS
{: #public_outside_2}

Cuando configure el ALB público, elija el dominio en el que sus apps serán accesibles y si desea utilizar la terminación TLS.
{: shortdesc}

<dl>
<dt>Dominio</dt>
<dd>Puede utilizar el dominio proporcionado por IBM, como por ejemplo <code>mycluster-12345.us-south.containers.appdomain.cloud/myapp</code>, para acceder a su app desde Internet. Para en su lugar utilizar un dominio personalizado, puede correlacionar su dominio personalizado con el dominio proporcionado por IBM o la dirección IP pública del ALB.</dd>
<dt>Terminación TLS</dt>
<dd>El ALB equilibra carga de tráfico de red HTTP para las apps en su clúster. Para también equilibrar la carga de conexiones HTTPS entrantes, puede configurar el ALB para descifrar el tráfico de red y reenviar las solicitudes descifradas a las apps expuestas en su clúster. Si está utilizando el subdominio de Ingress proporcionado por IBM, puede utilizar el certificado TLS proporcionado por IBM. Actualmente no se da soporte a TLS para los subdominios comodín que IBM proporciona. Si utiliza un dominio personalizado, puede utilizar su propio certificado TLS para gestionar la terminación TLS.</dd>
</dl>

Para utilizar el dominio de Ingress proporcionado por IBM:
1. Obtenga los detalles del clúster. Sustituya _&lt;cluster_name_or_ID&gt;_ con el nombre del clúster en el que se despliegan las apps que desea exponer.

    ```
    bx cs cluster-get <cluster_name_or_ID>
    ```
    {: pre}

    Salida de ejemplo:

    ```
    Name:                   mycluster
    ID:                     18a61a63c6a94b658596ca93d087aad9
    State:                  normal
    Created:                2018-01-12T18:33:35+0000
    Location:               dal10
    Master URL:             https://169.xx.xxx.xxx:26268
    Ingress Subdomain:      mycluster-12345.us-south.containers.appdomain.cloud
    Ingress Secret:         <tls_secret>
    Workers:                3
    Version:                1.9.7
    Owner Email:            owner@email.com
    Monitoring Dashboard:   <dashboard_URL>
    ```
    {: screen}
2. Obtenga el dominio proporcionado por IBM en el campo **Subdominio de Ingress**. Si desea utilizar TLS, obtenga también el secreto de TLS proporcionado por IBM en el campo **Secreto Ingress**. **Nota**: Si está utilizando un subdominio comodín, no se da soporte a TLS.

Para utilizar un dominio personalizado:
1.    Cree un dominio personalizado. Para registrar un dominio personalizado, póngase en contacto con su proveedor de DNS (Domain Name Service) o con [{{site.data.keyword.Bluemix_notm}} DNS](/docs/infrastructure/dns/getting-started.html#getting-started-with-dns).
      * Si las apps que desea que Ingress exponga se encuentran en diferentes espacios de nombres en un clúster, registre el dominio personalizado como un dominio comodín como, por ejemplo, `*.custom_domain.net`.

2.  Configure el dominio de modo que direccione el tráfico de red de entrada al ALB proporcionado por IBM. Puede elegir entre las siguientes opciones:
    -   Defina un alias para el dominio personalizado especificando el dominio proporcionado por IBM como CNAME (Registro de nombre canónico). Para encontrar el dominio de Ingress proporcionado por IBM, ejecute `bx cs cluster-get <cluster_name>` y busque el campo **Subdominio de Ingress**.
    -   Correlacione el dominio personalizado con la dirección IP pública portátil del ALB proporcionado por IBM añadiendo la dirección IP como registro. Para buscar la dirección IP pública portátil del ALB, ejecute `bx cs alb-get <public_alb_ID>`.
3.   Opcional: Si desea utilizar TLS, importe o cree un certificado TLS y un secreto de clave. Si utiliza un dominio comodín, asegúrese de importar o crear un certificado comodín.
      * Si hay un certificado TLS almacenado en {{site.data.keyword.cloudcerts_long_notm}} que desea utilizar, puede importar su secreto asociado a su clúster ejecutando el siguiente mandato:
        ```
        bx cs alb-cert-deploy --secret-name <secret_name> --cluster <cluster_name_or_ID> --cert-crn <certificate_crn>
        ```
        {: pre}
      * Si no tiene un certificado TLS listo, siga estos pasos:
        1. Cree un certificado TLS y una clave para el dominio que estén codificados en formato PEM.
        2. Cree un secreto que utilice su certificado TLS y clave. Sustituya <em>&lt;tls_secret_name&gt;</em> con un nombre para su secreto de Kubernetes, <em>&lt;tls_key_filepath&gt;</em> con la vía de acceso a su archivo de claves TLS personalizado y <em>&lt;tls_cert_filepath&gt;</em> con la vía de acceso al archivo de certificado TLS personalizado.
          ```
          kubectl create secret tls <tls_secret_name> --key <tls_key_filepath> --cert <tls_cert_filepath>
          ```
          {: pre}


### Paso 3: Crear el recurso de Ingress
{: #public_outside_3}

Los recursos de Ingress definen las reglas de direccionamiento que los ALB utilizan para direccionar el tráfico a su servicio de app.
{: shortdesc}

**Nota:** Si está exponiendo varias apps externas, y los servicios que ha creado para las apps en el [Paso 1](#public_outside_1) están en distintos espacios de nombres, como mínimo se necesita un recurso de Ingress por espacio de nombres. Sin embargo, cada espacio de nombres debe utilizar un host diferente. Debe registrar un dominio comodín y especificar un subdominio diferente en cada recurso. Para obtener más información, consulte [Planificación de redes para uno o varios espacios de nombres](#multiple_namespaces).

1. Abra el editor que prefiera y cree un archivo de configuración de Ingress llamado, por ejemplo, `myexternalingress.yaml`.

2. Defina un recurso de Ingress en el archivo de configuración que utilice el dominio proporcionado por IBM o su dominio personalizado para direccionar el tráfico de entrada de red a los servicios que ha creado anteriormente.

    YAML de ejemplo que no utiliza TLS:
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myexternalingress
    spec:
      rules:
      - host: <domain>
        http:
          paths:
          - path: /<external_app1_path>
            backend:
              serviceName: <app1_service>
              servicePort: 80
          - path: /<external_app2_path>
            backend:
              serviceName: <app2_service>
              servicePort: 80
    ```
    {: codeblock}

    YAML de ejemplo que utiliza TLS:
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myexternalingress
    spec:
      tls:
      - hosts:
        - <domain>
        secretName: <tls_secret_name>
      rules:
      - host: <domain>
        http:
          paths:
          - path: /<external_app1_path>
            backend:
              serviceName: <app1_service>
              servicePort: 80
          - path: /<external_app2_path>
            backend:
              serviceName: <app2_service>
              servicePort: 80
    ```
    {: codeblock}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general de los componentes del archivo YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code>tls/hosts</code></td>
    <td>Para utilizar TLS, sustituya <em>&lt;domain&gt;</em> con el subdominio de Ingress proporcionado por IBM o su dominio personalizado.

    </br></br>
    <strong>Nota:</strong><ul><li>Si sus servicios de app están en diferentes espacios de nombres en un clúster, añada un subdominio comodín al principio del dominio como, por ejemplo, `subdomain1.custom_domain.net` o `subdomain1.mycluster.us-south.containers.appdomain.cloud`. Utilice un subdominio exclusivo para cada recurso que cree en el clúster.</li><li>No utilice &ast; para el host ni deje la propiedad host vacía para evitar errores durante la creación de Ingress.</li></ul></td>
    </tr>
    <tr>
    <td><code>tls/secretName</code></td>
    <td><ul><li>Si está utilizando el dominio de Ingress proporcionado por IBM, sustituya <em>&lt;tls_secret_name&gt;</em> con el nombre del secreto de Ingress proporcionado por IBM.</li><li>Si utiliza un dominio personalizado, sustituya <em>&lt;tls_secret_name&gt;</em> con el secreto que ha creado anteriormente que aloja la clave y el certificado TLS personalizados. Si ha importado un certificado de {{site.data.keyword.cloudcerts_short}}, puede ejecutar <code>bx cs alb-cert-get --cluster <cluster_name_or_ID> --cert-crn <certificate_crn></code> para ver los secretos que están asociados con un certificado TLS.</li><ul><td>
    </tr>
    <tr>
    <td><code>rules/host</code></td>
    <td>Sustituya <em>&lt;domain&gt;</em> con el subdominio de Ingress proporcionado por IBM o su dominio personalizado.

    </br></br>
    <strong>Nota:</strong><ul><li>Si sus apps están expuestas por los servicios en diferentes espacios de nombres en un clúster, añada un subdominio comodín al principio del dominio como, por ejemplo, `subdomain1.custom_domain.net` o `subdomain1.mycluster.us-south.containers.appdomain.cloud`. Utilice un subdominio exclusivo para cada recurso que cree en el clúster.</li><li>No utilice &ast; para el host ni deje la propiedad host vacía para evitar errores durante la creación de Ingress.</li></ul></td>
    </tr>
    <tr>
    <td><code>path</code></td>
    <td>Sustituya <em>&lt;external_app_path&gt;</em> con una barra inclinada o con la vía de acceso en la que su app está a la escucha. La vía de acceso se añade al dominio personalizado o proporcionado por IBM para crear una ruta exclusiva a su app. Cuando especifica esta ruta en un navegador web, el tráfico de la red se direcciona al ALB. El ALB consulta el servicio asociado y envía el tráfico de red al servicio. El servicio que reenvía tráfico a la app externa. Se debe configurar la app para que escuche en esta vía de acceso para recibir el tráfico de red entrante.
    </br></br>
    Muchas apps no escuchan en una vía de acceso específica, sino que utilizan la vía de acceso raíz y un puerto específico. En este caso, defina la vía de acceso raíz como <code>/</code> y no especifique una vía de acceso individual para la app. Ejemplos: <ul><li>Para <code>http://domain/</code>, especifique <code>/</code> como la vía de acceso.</li><li>Para <code>http://domain/app1_path</code>, especifique <code>/app1_path</code> como la vía de acceso.</li></ul>
    </br></br>
    <strong>Sugerencia:</strong> Para configurar Ingress para que escuche en una vía de acceso que sea distinta a la vía de acceso en la que escucha la app, utilice la [anotación de reescritura](cs_annotations.html#rewrite-path).</td>
    </tr>
    <tr>
    <td><code>serviceName</code></td>
    <td>Sustituya <em>&lt;app1_service&gt;</em> y <em>&lt;app2_service&gt;</em> y así sucesivamente con el nombre de los servicios creados para exponer sus apps externas. Si sus apps están expuestas por los servicios en diferentes espacios de nombres en el clúster, incluya únicamente los servicios de app que se encuentran en el mismo espacio de nombres. Debe crear un recurso de Ingress para cada espacio de nombres donde tiene las apps que desea exponer.</td>
    </tr>
    <tr>
    <td><code>servicePort</code></td>
    <td>El puerto en el que el servicio está a la escucha. Utilice el mismo puerto que ha definido al crear el servicio de Kubernetes para la app.</td>
    </tr>
    </tbody></table>

3.  Cree el recurso de Ingress para el clúster. Asegúrese de que el recurso se despliega en el mismo espacio de nombres que los servicios de apps que ha especificado en el recurso.

    ```
    kubectl apply -f myexternalingress.yaml -n <namespace>
    ```
    {: pre}
4.   Verifique que el recurso de Ingress se haya creado correctamente.

      ```
      kubectl describe ingress myingressresource
      ```
      {: pre}

      1. Si los mensajes en los sucesos describen un error en la configuración del recurso, cambie los valores en el archivo de recursos y aplique de nuevo el archivo del recurso.


El recurso de Ingress se crea en el mismo espacio de nombres que los servicios de las apps. Sus apps en este espacio de nombres se registran con el ALB de Ingress del clúster.

### Paso 4: Acceder a la app desde Internet
{: #public_outside_4}

En un navegador web, escriba el URL del servicio de la app al que va a acceder.

```
https://<domain>/<app1_path>
```
{: pre}

Si tiene varias apps expuestas, acceda a dichas apps cambiando la vía de acceso que se añade al final del URL.

```
https://<domain>/<app2_path>
```
{: pre}

Si utiliza un dominio comodín para exponer apps en diferentes espacios de nombres, acceda a dichas apps con sus respectivos subdominios.

```
http://<subdomain1>.<domain>/<app1_path>
```
{: pre}

```
http://<subdomain2>.<domain>/<app1_path>
```
{: pre}


<br />


## Habilitación del ALB privado predeterminado
{: #private_ingress}

Cuando se crea un clúster estándar, se crea un equilibrador de carga de aplicación (ALB) privado proporcionado por IBM y se le asigna una dirección IP privada portátil y una ruta privada. Sin embargo, el ALB privado predeterminado no se habilita automáticamente. Para utilizar el ALB privado para el equilibrio de carga del tráfico de red privado para sus apps, primero debe habilitarlo con la dirección IP privada portátil proporcionada por IBM o con su propia dirección IP privada portátil.
{:shortdesc}

**Nota**: si ha utilizado el distintivo `--no-subnet` al crear el clúster, debe añadir una subred privada portátil o una subred gestionada por el usuario para poder habilitar el ALB privado. Para obtener más información, consulte [Solicitud de más subredes para el clúster](cs_subnets.html#request).

Antes de empezar:

-   Si aún no tiene uno, [cree un clúster estándar](cs_clusters.html#clusters_ui).
-   Defina su clúster como [destino de la CLI](cs_cli_install.html#cs_cli_configure).

Para habilitar el ALB privado utilizando la dirección IP privada portátil asignada previamente y proporcionada por IBM:

1. Obtenga una lista de los ALB disponibles en el clúster para obtener el ID del ALB privado. Sustituya <em>&lt;cluser_name&gt;</em> por el nombre del clúster en el que está desplegada la app que desea exponer.

    ```
    bx cs albs --cluster <cluser_name>
    ```
    {: pre}

    El campo **Status** del ALB privado es _disabled_.
    ```
    ALB ID                                            Enabled   Status     Type      ALB IP
    private-cr6d779503319d419ea3b4ab171d12c3b8-alb1   false     disabled   private   -
    public-cr6d779503319d419ea3b4ab171d12c3b8-alb1    true      enabled    public    169.xx.xxx.xxx
    ```
    {: screen}

2. Habilite el ALB privado. Sustituya <em>&lt;private_ALB_ID&gt;</em> por el ID del ALB privado de la salida del paso anterior.

   ```
   bx cs alb-configure --albID <private_ALB_ID> --enable
   ```
   {: pre}

<br>
Para habilitar el ALB privado utilizando la dirección IP privada portátil:

1. Configure la subred gestionada por el usuario de la dirección IP seleccionada para direccionar el tráfico en la VLAN privada del clúster.

   ```
   bx cs cluster-user-subnet-add <cluster_name> <subnet_CIDR> <private_VLAN_ID>
   ```
   {: pre}

   <table>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de mandatos</th>
   </thead>
   <tbody>
   <tr>
   <td><code>&lt;cluser_name&gt;</code></td>
   <td>El nombre o el ID del clúster en el que se despliega la app que desea exponer.</td>
   </tr>
   <tr>
   <td><code>&lt;subnet_CIDR&gt;</code></td>
   <td>El CIDR de la subred gestionada por el usuario.</td>
   </tr>
   <tr>
   <td><code>&lt;private_VLAN_ID&gt;</code></td>
   <td>Un ID de VLAN privada disponible. Encontrará el ID de la VLAN privada disponible ejecutando `bx cs vlans`.</td>
   </tr>
   </tbody></table>

2. Obtenga una lista de los ALB disponibles en el clúster para obtener el ID del ALB privado.

    ```
    bx cs albs --cluster <cluster_name>
    ```
    {: pre}

    El campo **Status** del ALB privado es _disabled_.
    ```
    ALB ID                                            Enabled   Status     Type      ALB IP
    private-cr6d779503319d419ea3b4ab171d12c3b8-alb1   false     disabled   private   -
    public-cr6d779503319d419ea3b4ab171d12c3b8-alb1    true      enabled    public    169.xx.xxx.xxx
    ```
    {: screen}

3. Habilite el ALB privado. Sustituya <em>&lt;private_ALB_ID&gt;</em> por el ID del ALB privado de la salida del paso anterior y <em>&lt;user_IP&gt;</em> por la dirección IP de la subred gestionada por el usuario que desea utilizar.

   ```
   bx cs alb-configure --albID <private_ALB_ID> --enable --user-ip <user_IP>
   ```
   {: pre}

<br />


## Exposición de apps de una red privada
{: #ingress_expose_private}

Exposición de apps para una red privada utilizando el ALB de Ingress privado.
{:shortdesc}

Antes de empezar:
* Revise los [requisitos previos](#config_prereqs) de Ingress.
* [Habilite el equilibrador de carga de aplicación (ALB)](#private_ingress).
* Si tiene nodos trabajadores privados y desea utilizar un proveedor de DNS externo, debe [configurar nodos de extremo con acceso público](cs_edge.html#edge) y [configurar un dispositivo Virtual Router Appliance ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/blogs/bluemix/2017/07/kubernetes-and-bluemix-container-based-workloads-part4/).
* Si tiene nodos trabajadores privados y desea que únicamente permanezcan en la red privada, debe [configurar un servicio de DNS local privado ![Icono de enlace privado](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/) para resolver solicitudes de URL para sus apps.

### Paso 1: Desplegar apps y crear servicios de app
{: #private_1}

Empiece desplegando sus apps y crear servicios de Kubernetes para exponerlos.
{: shortdesc}

1.  [Despliegue la app en el clúster](cs_app.html#app_cli). Asegúrese de añadir una etiqueta a su despliegue en la sección de metadatos del archivo de configuración como, por ejemplo, `app: code`. Esta etiqueta es necesaria para identificar todos los pods en los que se está ejecutando la app, de modo que puedan incluirse dichos pods en el equilibrio de carga de Ingress.

2.   Cree un servicio de Kubernetes para cada app que desee exponer. El servicio de Kubernetes debe exponer la app para incluirla en el ALB de clúster en el equilibrio de carga de Ingress.
      1.  Abra el editor que prefiera y cree un archivo de configuración de servicio llamado, por ejemplo, `myapp_service.yaml`.
      2.  Defina un servicio para la app que el ALB expondrá.

          ```
          apiVersion: v1
          kind: Service
          metadata:
            name: myapp_service
          spec:
            selector:
              <selector_key>: <selector_value>
            ports:
             - protocol: TCP
               port: 8080
          ```
          {: codeblock}

          <table>
          <thead>
          <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes del archivo YAML del servicio del ALB</th>
          </thead>
          <tbody>
          <tr>
          <td><code>selector</code></td>
          <td>Especifique el par de clave de etiqueta (<em>&lt;selector_key&gt;</em>) y valor (<em>&lt;selector_value&gt;</em>) que desee utilizar para establecer como destino los pods en los que se ejecuta la app. Para especificar e incluir sus pods en el equilibrio de carga del servicio, asegúrese de que <em>&lt;selector_key&gt;</em> y <em>&lt;selector_value&gt;</em> coinciden con el par de clave/valor en la sección <code>spec.template.metadata.labels</code> de su despliegue yaml.</td>
           </tr>
           <tr>
           <td><code>port</code></td>
           <td>El puerto que en el que está a la escucha el servicio.</td>
           </tr>
           </tbody></table>
      3.  Guarde los cambios.
      4.  Cree el servicio en el clúster. Si las apps están desplegadas en varios espacios de nombres en el clúster, asegúrese de que el servicio se despliega en el mismo espacio de nombres que la app que desea exponer.

          ```
          kubectl apply -f myapp_service.yaml [-n <namespace>]
          ```
          {: pre}
      5.  Repita estos pasos para cada app que desee exponer.


### Paso 2: Correlacionar el dominio personalizado y seleccionar la terminación TLS
{: #private_2}

Cuando configure el ALB privado, elija un dominio personalizado en el que sus apps serán accesibles. Elija también si desea utilizar la terminación TLS.
{: shortdesc}

El ALB equilibra carga del tráfico de red HTTP para las apps. Para también equilibrar la carga de conexiones HTTPS entrantes, puede configurar el ALB para utilizar su propio certificado TLS para descifrar el tráfico de red. El ALB, a continuación, reenvía la solicitud descifrada a las apps expuestas en el clúster.
1.   Cree un dominio personalizado. Para registrar un dominio personalizado, póngase en contacto con su proveedor de DNS (Domain Name Service) o con [{{site.data.keyword.Bluemix_notm}} DNS](/docs/infrastructure/dns/getting-started.html#getting-started-with-dns).
      * Si las apps que desea que Ingress exponga se encuentran en diferentes espacios de nombres en un clúster, registre el dominio personalizado como un dominio comodín como, por ejemplo, `*.custom_domain.net`.

2. Correlacione el dominio personalizado con la dirección IP privada portátil del ALB privado proporcionado por IBM añadiendo la dirección IP como registro. Para buscar la dirección IP privada portátil del ALB privado, ejecute `bx cs albs --cluster <cluster_name>`.
3.   Opcional: Si desea utilizar TLS, importe o cree un certificado TLS y un secreto de clave. Si utiliza un dominio comodín, asegúrese de importar o crear un certificado comodín.
      * Si hay un certificado TLS almacenado en {{site.data.keyword.cloudcerts_long_notm}} que desea utilizar, puede importar su secreto asociado a su clúster ejecutando el siguiente mandato:
        ```
        bx cs alb-cert-deploy --secret-name <secret_name> --cluster <cluster_name_or_ID> --cert-crn <certificate_crn>
        ```
        {: pre}
      * Si no tiene un certificado TLS listo, siga estos pasos:
        1. Cree un certificado TLS y una clave para el dominio que estén codificados en formato PEM.
        2. Cree un secreto que utilice su certificado TLS y clave. Sustituya <em>&lt;tls_secret_name&gt;</em> con un nombre para su secreto de Kubernetes, <em>&lt;tls_key_filepath&gt;</em> con la vía de acceso a su archivo de claves TLS personalizado y <em>&lt;tls_cert_filepath&gt;</em> con la vía de acceso al archivo de certificado TLS personalizado.
          ```
          kubectl create secret tls <tls_secret_name> --key <tls_key_filepath> --cert <tls_cert_filepath>
          ```
          {: pre}


### Paso 3: Crear el recurso de Ingress
{: #pivate_3}

Los recursos de Ingress definen las reglas de direccionamiento que los ALB utilizan para direccionar el tráfico a su servicio de app.
{: shortdesc}

**Nota:** Si el clúster tiene varios espacios de nombres donde se exponen las apps, como mínimo se necesita un recurso de Ingress por espacio de nombres. Sin embargo, cada espacio de nombres debe utilizar un host diferente. Debe registrar un dominio comodín y especificar un subdominio diferente en cada recurso. Para obtener más información, consulte [Planificación de redes para uno o varios espacios de nombres](#multiple_namespaces).

1. Abra el editor que prefiera y cree un archivo de configuración de Ingress llamado, por ejemplo, `myingressresource.yaml`.

2.  Defina un recurso de Ingress en el archivo de configuración que utilice su dominio personalizado para direccionar el tráfico de entrada de red a los servicios que ha creado anteriormente.

    YAML de ejemplo que no utiliza TLS:
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingressresource
      annotations:
        ingress.bluemix.net/ALB-ID: "<private_ALB_ID>"
    spec:
      rules:
      - host: <domain>
        http:
          paths:
          - path: /<app1_path>
            backend:
              serviceName: <app1_service>
              servicePort: 80
          - path: /<app2_path>
            backend:
              serviceName: <app2_service>
              servicePort: 80
    ```
    {: codeblock}

    YAML de ejemplo que utiliza TLS:
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingressresource
      annotations:
        ingress.bluemix.net/ALB-ID: "<private_ALB_ID>"
    spec:
      tls:
      - hosts:
        - <domain>
        secretName: <tls_secret_name>
      rules:
      - host: <domain>
        http:
          paths:
          - path: /<app1_path>
            backend:
              serviceName: <app1_service>
              servicePort: 80
          - path: /<app2_path>
            backend:
              serviceName: <app2_service>
              servicePort: 80
    ```
    {: codeblock}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general de los componentes del archivo YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code>ingress.bluemix.net/ALB-ID</code></td>
    <td>Sustituya <em>&lt;private_ALB_ID&gt;</em> por el ID del ALB privado. Ejecute <code>bx cs albs --cluster <my_cluster></code> para buscar el ID de ALB. Para obtener más información sobre esta anotación de Ingress, consulte [Direccionamiento del equilibrador de carga de aplicación privado](cs_annotations.html#alb-id).</td>
    </tr>
    <tr>
    <td><code>tls/hosts</code></td>
    <td>Para utilizar TLS, sustituya <em>&lt;domain&gt;</em> con su dominio personalizado.

    </br></br>
    <strong>Nota:</strong><ul><li>Si sus apps están expuestas por los servicios en diferentes espacios de nombres en un clúster, añada un subdominio comodín al principio del dominio como, por ejemplo, `subdomain1.custom_domain.net`. Utilice un subdominio exclusivo para cada recurso que cree en el clúster.</li><li>No utilice &ast; para el host ni deje la propiedad host vacía para evitar errores durante la creación de Ingress.</li></ul></td>
    </tr>
    <tr>
    <td><code>tls/secretName</code></td>
    <td>Sustituya <em>&lt;tls_secret_name&gt;</em> por el nombre del secreto que ha creado anteriormente y que contiene el certificado TLS personalizado y la clave. Si ha importado un certificado de {{site.data.keyword.cloudcerts_short}}, puede ejecutar <code>bx cs alb-cert-get --cluster <cluster_name_or_ID> --cert-crn <certificate_crn></code> para ver los secretos que están asociados con un certificado TLS.
    </tr>
    <tr>
    <td><code>host</code></td>
    <td>Sustituya <em>&lt;domain&gt;</em> por su dominio personalizado.
    </br></br>
    <strong>Nota:</strong><ul><li>Si sus apps están expuestas por los servicios en diferentes espacios de nombres en un clúster, añada un subdominio comodín al principio del dominio como, por ejemplo, `subdomain1.custom_domain.net`. Utilice un subdominio exclusivo para cada recurso que cree en el clúster.</li><li>No utilice &ast; para el host ni deje la propiedad host vacía para evitar errores durante la creación de Ingress.</li></ul></td>
    </td>
    </tr>
    <tr>
    <td><code>path</code></td>
    <td>Sustituya <em>&lt;app_path&gt;</em> con una barra inclinada o con la vía de acceso en la que su app está a la escucha. La vía de acceso se añade al dominio personalizado para crear una ruta exclusiva a su app. Cuando especifica esta ruta en un navegador web, el tráfico de la red se direcciona al ALB. El ALB consulta el servicio asociado y envía el tráfico de red al servicio. A continuación, el servicio reenvía el tráfico a los pods en los que la app está en ejecución.
    </br></br>
    Muchas apps no escuchan en una vía de acceso específica, sino que utilizan la vía de acceso raíz y un puerto específico. En este caso, defina la vía de acceso raíz como <code>/</code> y no especifique una vía de acceso individual para la app. Ejemplos: <ul><li>Para <code>http://domain/</code>, especifique <code>/</code> como la vía de acceso.</li><li>Para <code>http://domain/app1_path</code>, especifique <code>/app1_path</code> como la vía de acceso.</li></ul>
    </br>
    <strong>Sugerencia:</strong> Para configurar Ingress para que escuche en una vía de acceso que sea distinta a la vía de acceso en la que escucha la app, utilice la [anotación de reescritura](cs_annotations.html#rewrite-path).</td>
    </tr>
    <tr>
    <td><code>serviceName</code></td>
    <td>Sustituya <em>&lt;app1_service&gt;</em> y <em>&lt;app2_service&gt;</em> y así sucesivamente con el nombre de los servicios que ha creado para exponer sus apps. Si sus apps están expuestas por los servicios en diferentes espacios de nombres en el clúster, incluya únicamente los servicios de app que se encuentran en el mismo espacio de nombres. Debe crear un recurso de Ingress para cada espacio de nombres donde tiene las apps que desea exponer.</td>
    </tr>
    <tr>
    <td><code>servicePort</code></td>
    <td>El puerto en el que el servicio está a la escucha. Utilice el mismo puerto que ha definido al crear el servicio de Kubernetes para la app.</td>
    </tr>
    </tbody></table>

3.  Cree el recurso de Ingress para el clúster. Asegúrese de que el recurso se despliega en el mismo espacio de nombres que los servicios de apps que ha especificado en el recurso.

    ```
    kubectl apply -f myingressresource.yaml -n <namespace>
    ```
    {: pre}
4.   Verifique que el recurso de Ingress se haya creado correctamente.

      ```
      kubectl describe ingress myingressresource
      ```
      {: pre}

      1. Si los mensajes en los sucesos describen un error en la configuración del recurso, cambie los valores en el archivo de recursos y aplique de nuevo el archivo del recurso.


El recurso de Ingress se crea en el mismo espacio de nombres que los servicios de las apps. Sus apps en este espacio de nombres se registran con el ALB de Ingress del clúster.

### Paso 4: Acceder a la app desde su red privada
{: #private_4}

Desde dentro de su cortafuegos de la red privada, especifique el URL del servicio de app en un navegador web.

```
https://<domain>/<app1_path>
```
{: pre}

Si tiene varias apps expuestas, acceda a dichas apps cambiando la vía de acceso que se añade al final del URL.

```
https://<domain>/<app2_path>
```
{: pre}

Si utiliza un dominio comodín para exponer apps en diferentes espacios de nombres, acceda a dichas apps con sus respectivos subdominios.

```
http://<subdomain1>.<domain>/<app1_path>
```
{: pre}

```
http://<subdomain2>.<domain>/<app1_path>
```
{: pre}


Para obtener una guía completa sobre cómo proteger la comunicación entre microservicios en los clústeres utilizando el ALB privado con TLS, consulte [este artículo del este blog ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://medium.com/ibm-cloud/secure-microservice-to-microservice-communication-across-kubernetes-clusters-using-a-private-ecbe2a8d4fe2).

<br />


## Configuraciones opcionales del equilibrador de carga de aplicación
{: #configure_alb}

También puede configurar un equilibrador de carga de aplicación con las opciones siguientes.

-   [Apertura de puertos en el equilibrador de carga de aplicación de Ingress](#opening_ingress_ports)
-   [Configuración de protocolos SSL y cifrados SSL a nivel HTTP](#ssl_protocols_ciphers)
-   [Personalización del formato de registro de Ingress](#ingress_log_format)
-   [Personalización de su equilibrador de carga de aplicación con anotaciones](cs_annotations.html)
{: #ingress_annotation}


### Apertura de puertos en el equilibrador de carga de aplicación de Ingress
{: #opening_ingress_ports}

De forma predeterminada, sólo los puertos 80 y 443 están expuestos en el ALB de Ingress. Para exponer otros puertos, puede editar el recurso del mapa de configuración `ibm-cloud-provider-ingress-cm`.
{:shortdesc}

1. Cree y abra una versión local del archivo de configuración del recurso del mapa de configuración `ibm-cloud-provider-ingress-cm`.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. Añada una sección <code>data</code> y especifique los puertos públicos `80`, `443`, y cualquier otro puerto que desee exponer de forma separada mediante un punto y coma (;).

    **Importante**: De forma predeterminada, los puertos 80 y 443 están abiertos. Si desea mantener los puertos 80 y 443 abiertos, también debe incluirlos, además de cualquier otro puerto que especifique en el campo `public-ports`. Los puertos que no se especifiquen, permanecerán cerrados. Si ha habilitado un ALB privado, también debe especificar los puertos que desea mantener abiertos en el campo `private-ports`.

    ```
    apiVersion: v1
    data:
      public-ports: "80;443;<port3>"
      private-ports: "80;443;<port4>"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: codeblock}

    Ejemplo que mantiene abiertos los puertos `80`, `443` y `9443`:
    ```
    apiVersion: v1
 data:
   public-ports: "80;443;9443"
 kind: ConfigMap
 metadata:
   name: ibm-cloud-provider-ingress-cm
   namespace: kube-system
    ```
    {: screen}

3. Guarde el archivo de configuración.

4. Verifique que se hayan aplicado los cambios del mapa de configuración.

 ```
 kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
 ```
 {: pre}

 Salida:
 ```
 Name:        ibm-cloud-provider-ingress-cm
 Namespace:    kube-system
 Labels:        <none>
 Annotations:    <none>

 Data
 ====

  public-ports: "80;443;9443"
 ```
 {: screen}

Para obtener más información sobre los recursos del mapa de configuración, consulte la [documentación de Kubernetes](https://kubernetes-v1-4.github.io/docs/user-guide/configmap/).

### Configuración de protocolos SSL y cifrados SSL a nivel HTTP
{: #ssl_protocols_ciphers}

Habilite los protocolos SSL y cifrados a nivel HTTP global editando el mapa de configuración `ibm-cloud-provider-ingress-cm`.
{:shortdesc}

De forma predeterminada, el protocolo TLS 1.2 se utiliza para todas las configuraciones de Ingress que utilizan el dominio proporcionado por IBM. Siga estos pasos para cambiar este valor predeterminado para utilizar en su lugar los protocolos TLS 1.1 o 1.0.

**Nota**: Cuando se especifican los protocolos habilitados para todos los hosts, los parámetros TLSv1.1 y TLSv1.2 (1.1.13, 1.0.12) solo funcionan cuando se utiliza OpenSSL 1.0.1 o superior. El parámetro TLSv1.3 (1.13.0) sólo funciona cuando se utiliza OpenSSL 1.1.1 creado con soporte de TLSv1.3.

Para editar el mapa de configuración para habilitar los cifrados y los protocolos SSL:

1. Cree y abra una versión local del archivo de configuración del recurso del mapa de configuración `ibm-cloud-provider-ingress-cm`.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. Añada los cifrados y los protocolos SSL. Dé formato a los cifrados de acuerdo con [el formato de lista de cifrado de la biblioteca de OpenSSL ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.openssl.org/docs/man1.0.2/apps/ciphers.html).

   ```
   apiVersion: v1
 data:
   ssl-protocols: "TLSv1 TLSv1.1 TLSv1.2"
   ssl-ciphers: "HIGH:!aNULL:!MD5"
 kind: ConfigMap
 metadata:
   name: ibm-cloud-provider-ingress-cm
   namespace: kube-system
   ```
   {: codeblock}

3. Guarde el archivo de configuración.

4. Verifique que se hayan aplicado los cambios del mapa de configuración.

   ```
   kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
   ```
   {: pre}

   Salida:
   ```
   Name:        ibm-cloud-provider-ingress-cm
 Namespace:    kube-system
 Labels:        <none>
 Annotations:    <none>

   Data
 ====

    ssl-protocols: "TLSv1 TLSv1.1 TLSv1.2"
  ssl-ciphers: "HIGH:!aNULL:!MD5"
   ```
   {: screen}

### Personalización del formato y el contenido del registro
{: #ingress_log_format}

Existe la posibilidad de personalizar el contenido y los formatos de los registros que se recopilan para el ALB de Ingress.
{:shortdesc}

De forma predeterminada, los registros de Ingress están en formato JSON y visualizan los campos más comunes del registro. Sin embargo, también puede crear un formato de registro personalizado. Siga estos pasos para elegir los componentes de registro que se reenviarán y su disposición en la salida del registro:

1. Cree y abra una versión local del archivo de configuración del recurso del mapa de configuración `ibm-cloud-provider-ingress-cm`.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. Añada una sección <code>data</code>. Añada el campo `log-format` y, de forma opcional, el campo `log-format-escape-json`.

    ```
    apiVersion: v1
    data:
      log-format: '{<key1>: <log_variable1>, <key2>: <log_variable2>, <key3>: <log_variable3>}'
      log-format-escape-json: "true"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: pre}

    <table>
    <caption>Componentes del archivo YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono Idea"/>Descripción de la configuración del formato de registro</th>
    </thead>
    <tbody>
    <tr>
    <td><code>log-format</code></td>
    <td>Sustituya <code>&lt;key&gt;</code> con el nombre del componente de registro y <code>&lt;log_variable&gt;</code> con una variable para el componente de registro del que desea recopilar entradas de registro. Puede incluir texto y puntuación que desea que contenga la entrada de registro como, por ejemplo, comillas alrededor de valores de serie y comas para separar los componentes de registro. Por ejemplo, la aplicación del formato de un componente como, por ejemplo, <code>request: "$request",</code>, generará la siguiente entrada en una entrada de registro: <code>request: "GET / HTTP/1.1",</code>. Para obtener una lista de todas las variables que se pueden utilizar, consulte <a href="http://nginx.org/en/docs/varindex.html">Índice de variables de Nginx</a>.<br><br>Para registrar una cabecera adicional como, por ejemplo, <em>x-custom-ID</em>, añada el siguiente par de clave/valor al contenido de registro personalizado: <br><pre class="pre"><code>customID: $http_x_custom_id</code></pre> <br>Los guiones (<code>-</code>) se convierten en caracteres de subrayado (<code>_</code>). También se debe añadir <code>$http_</code> como prefijo al nombre de cabecera personalizado.</td>
    </tr>
    <tr>
    <td><code>log-format-escape-json</code></td>
    <td>Opcional: De forma predeterminada, los registros se generan en formato de texto. Para generar registros en formato JSON, añada el campo <code>log-format-escape-json</code> campo y utilice el valor <code>true</code>.</td>
    </tr>
    </tbody></table>
    </dd>
    </dl>

    Por ejemplo, su formato de registro podría contener las siguientes variables:
    ```
    apiVersion: v1
    data:
      log-format: '{remote_address: $remote_addr, remote_user: "$remote_user",
                    time_date: [$time_local], request: "$request",
                    status: $status, http_referer: "$http_referer",
                    http_user_agent: "$http_user_agent",
                    request_id: $request_id}'
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: screen}

    Una entrada de registro de acuerdo con este formato sería similar al siguiente ejemplo:
    ```
    remote_address: 127.0.0.1, remote_user: "dbmanager", time_date: [30/Mar/2018:18:52:17 +0000], request: "GET / HTTP/1.1", status: 401, http_referer: "-", http_user_agent: "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:47.0) Gecko/20100101 Firefox/47.0", request_id: a02b2dea9cf06344a25611c1d7ad72db
    ```
    {: screen}

    Para crear un formato de registro personalizado que se base en el formato predeterminado para los registros del ALB, modifique la sección siguiente según sea necesario y añada a su configmap:
    ```
    apiVersion: v1
    data:
      log-format: '{"time_date": "$time_iso8601", "client": "$remote_addr",
                    "host": "$http_host", "scheme": "$scheme",
                    "request_method": "$request_method", "request_uri": "$uri",
                    "request_id": "$request_id", "status": $status,
                    "upstream_addr": "$upstream_addr", "upstream_status":
                    $upstream_status, "request_time": $request_time,
                    "upstream_response_time": $upstream_response_time,
                    "upstream_connect_time": $upstream_connect_time,
                    "upstream_header_time": $upstream_header_time}'
      log-format-escape-json: "true"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: codeblock}

4. Guarde el archivo de configuración.

5. Verifique que se hayan aplicado los cambios del mapa de configuración.

 ```
 kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
 ```
 {: pre}

 Salida de ejemplo:
 ```
 Name:        ibm-cloud-provider-ingress-cm
 Namespace:    kube-system
 Labels:        <none>
 Annotations:    <none>

 Data
 ====

  log-format: '{remote_address: $remote_addr, remote_user: "$remote_user", time_date: [$time_local], request: "$request", status: $status, http_referer: "$http_referer", http_user_agent: "$http_user_agent", request_id: $request_id}'
  log-format-escape-json: "true"
 ```
 {: screen}

4. Para ver los registros del ALB de Ingress, [cree una configuración de registro para el servicio Ingress](cs_health.html#logging) en su clúster.

<br />



