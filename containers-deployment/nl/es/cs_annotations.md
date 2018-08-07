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



# Anotaciones de Ingress
{: #ingress_annotation}

Para añadir prestaciones al equilibrador de carga de aplicación (ALB) de Ingress, especifique anotaciones como metadatos en un recurso Ingress.
{: shortdesc}

Para obtener información general sobre los servicios de Ingress y cómo empezar a utilizarlos, consulte [Gestión de tráfico de red utilizando Ingress](cs_ingress.html#planning).

<table>
<caption>Anotaciones generales</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>Anotaciones generales</th>
 <th>Nombre</th>
 <th>Descripción</th>
 </thead>
 <tbody>
 <tr>
 <td><a href="#proxy-external-service">Servicios externos</a></td>
 <td><code>proxy-external-service</code></td>
 <td>Añada definiciones de vía de acceso a servicios externos, como un servicio alojado en {{site.data.keyword.Bluemix_notm}}.</td>
 </tr>
 <tr>
 <td><a href="#location-modifier">Modificador de ubicación</a></td>
 <td><code>location-modifier</code></td>
 <td>Modifique la forma en la que el ALB coteja el URI de solicitud con la vía de acceso de la app.</td>
 </tr>
 <tr>
 <td><a href="#alb-id">Direccionamiento de ALB privado</a></td>
 <td><code>ALB-ID</code></td>
 <td>Direccione las solicitudes entrantes a sus apps con un ALB privado.</td>
 </tr>
 <tr>
 <td><a href="#rewrite-path">Vías de acceso de reescritura</a></td>
 <td><code>rewrite-path</code></td>
 <td>Se direcciona el tráfico de red entrante a una vía de acceso distinta de aquella en la que escucha la app de fondo.</td>
 </tr>
 <tr>
 <td><a href="#tcp-ports">Puertos TCP</a></td>
 <td><code>tcp-ports</code></td>
 <td>Acceda a una app a través de un puerto TCP no estándar.</td>
 </tr>
 </tbody></table>

<br>

<table>
<caption>Anotaciones de conexión</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>Anotaciones de conexión</th>
 <th>Nombre</th>
 <th>Descripción</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#proxy-connect-timeout">Tiempos de espera excedidos de conexión y tiempos de espera excedidos de lectura personalizados</a></td>
  <td><code>proxy-connect-timeout, proxy-read-timeout</code></td>
  <td>Defina el tiempo que el ALB espera para conectarse a la app de fondo y para leer información de la misma antes de considerar que la app de fondo no está disponible.</td>
  </tr>
  <tr>
  <td><a href="#keepalive-requests">Solicitudes de estado activo</a></td>
  <td><code>keepalive-requests</code></td>
  <td>Establece el número máximo de solicitudes que se pueden servir a través de una conexión en estado activo.</td>
  </tr>
  <tr>
  <td><a href="#keepalive-timeout">Tiempo de espera en estado activo</a></td>
  <td><code>keepalive-timeout</code></td>
  <td>Establece el tiempo máximo que se mantiene abierta una conexión en estado activo en el servidor.</td>
  </tr>
  <tr>
  <td><a href="#proxy-next-upstream-config">Siguiente proxy sentido ascendente</a></td>
  <td><code>proxy-next-upstream-config</code></td>
  <td>Establece cuándo el ALB puede pasar una solicitud al siguiente servidor en sentido ascendente.</td>
  </tr>
  <tr>
  <td><a href="#sticky-cookie-services">Afinidad de sesión con cookies</a></td>
  <td><code>sticky-cookie-services</code></td>
  <td>El tráfico de red de entrada siempre se direcciona al mismo servidor en sentido ascendente mediante una "sticky cookie".</td>
  </tr>
  <tr>
  <td><a href="#upstream-keepalive">Estado activo en sentido ascendente</a></td>
  <td><code>upstream-keepalive</code></td>
  <td>Establece el número máximo de conexiones de estado activo para un servidor en sentido ascendente.</td>
  </tr>
  </tbody></table>

<br>

  <table>
  <caption>Anotaciones de autenticación TLS/SSL y HTTPS</caption>
  <col width="20%">
  <col width="20%">
  <col width="60%">
  <thead>
  <th>Anotaciones de autenticación TLS/SSL y HTTPS</th>
  <th>Nombre</th>
  <th>Descripción</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#appid-auth">{{site.data.keyword.appid_short}}Autenticación</a></td>
  <td><code>appid-auth</code></td>
  <td>Utilice {{site.data.keyword.appid_full_notm}} para autenticarse con la app.</td>
  </tr>
  <tr>
  <td><a href="#custom-port">Puertos HTTP y HTTPS personalizados</a></td>
  <td><code>custom-port</code></td>
  <td>Cambie los puertos predeterminados para el tráfico de red HTTP (puerto 80) y HTTPS (puerto 443).</td>
  </tr>
  <tr>
  <td><a href="#redirect-to-https">HTTP redirige a HTTPS</a></td>
  <td><code>redirect-to-https</code></td>
  <td>Redirige las solicitudes HTTP no seguras del dominio a HTTPS.</td>
  </tr>
  <tr>
  <td><a href="#hsts">HTTP con seguridad de transporte estricta (HSTS)</a></td>
  <td><code>hsts</code></td>
  <td>Configure al navegador para acceder al dominio utilizando únicamente HTTPS.</td>
  </tr>
  <tr>
  <td><a href="#mutual-auth">Autenticación mutua</a></td>
  <td><code>mutual-auth</code></td>
  <td>Configure la autenticación mutua para el ALB.</td>
  </tr>
  <tr>
  <td><a href="#ssl-services">Soporte de servicios SSL</a></td>
  <td><code>ssl-services</code></td>
  <td>Permita el soporte de servicios SSL para cifrar el tráfico en las apps ascendentes que requieren HTTPS.</td>
  </tr>
  </tbody></table>

<br>

<table>
<caption>Anotaciones de Istio</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Anotaciones de Istio</th>
<th>Nombre</th>
<th>Descripción</th>
</thead>
<tbody>
<tr>
<td><a href="#istio-services">Servicios de Istio</a></td>
<td><code>istio-services</code></td>
<td>Direcciona el tráfico a los servicios gestionados por Istio.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>Anotaciones de almacenamiento intermedio de proxy</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>Anotaciones de almacenamiento intermedio de proxy</th>
 <th>Nombre</th>
 <th>Descripción</th>
 </thead>
 <tbody>
 <tr>
 <td><a href="#proxy-buffering">Colocación en almacenamiento intermedio de datos de respuesta del cliente</a></td>
 <td><code>proxy-buffering</code></td>
 <td>Inhabilita la colocación en almacenamiento intermedio de una respuesta del cliente en el ALB al enviar la respuesta al cliente.</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffers">Almacenamientos intermedios de proxy</a></td>
 <td><code>proxy-buffers</code></td>
 <td>Establece el número y el tamaño de los almacenamientos intermedios que leen una respuesta de una única conexión desde el servidor mediante proxy.</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffer-size">Tamaño de almacenamiento intermedio de proxy</a></td>
 <td><code>proxy-buffer-size</code></td>
 <td>Establezca el tamaño del almacenamiento intermedio que lee la primera parte de la respuesta que se recibe del servidor proxy.</td>
 </tr>
 <tr>
 <td><a href="#proxy-busy-buffers-size">Tamaño de los almacenamientos intermedios ocupados del proxy</a></td>
 <td><code>proxy-busy-buffers-size</code></td>
 <td>Establezca el tamaño de los almacenamientos intermedios de proxy que puedan estar ocupados.</td>
 </tr>
 </tbody></table>

<br>

<table>
<caption>Anotaciones de solicitud y respuesta</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Anotaciones de solicitud y respuesta</th>
<th>Nombre</th>
<th>Descripción</th>
</thead>
<tbody>
<tr>
<td><a href="#proxy-add-headers">Solicitud de cliente o cabecera de respuesta adicional</a></td>
<td><code>proxy-add-headers, response-add-headers</code></td>
<td>Añade información de cabecera a una solicitud de cliente antes de reenviar la solicitud a la app de fondo, o añade una respuesta del cliente antes de enviar la respuesta al cliente.</td>
</tr>
<tr>
<td><a href="#response-remove-headers">Eliminación de cabecera de respuesta del cliente</a></td>
<td><code>response-remove-headers</code></td>
<td>Elimina la información de cabecera de una respuesta del cliente antes de reenviar la respuesta al cliente.</td>
</tr>
<tr>
<td><a href="#client-max-body-size">Tamaño de cuerpo de solicitud del cliente</a></td>
<td><code>client-max-body-size</code></td>
<td>Defina el tamaño máximo del cuerpo que puede enviar el cliente como parte de una solicitud.</td>
</tr>
<tr>
<td><a href="#large-client-header-buffers">Almacenamientos intermedios de cabeceras de cliente largas</a></td>
<td><code>large-client-header-buffers</code></td>
<td>Establezca el tamaño y el número máximo de los almacenamientos intermedios que leen cabeceras de solicitud de cliente largas.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>Anotaciones de límite de servicio</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Anotaciones de límite de servicio</th>
<th>Nombre</th>
<th>Descripción</th>
</thead>
<tbody>
<tr>
<td><a href="#global-rate-limit">Límites de velocidad global</a></td>
<td><code>global-rate-limit</code></td>
<td>Limite la velocidad de procesamiento de solicitudes y el número de conexiones por una clave definida para todos los servicios.</td>
</tr>
<tr>
<td><a href="#service-rate-limit">Límites de velocidad de servicio</a></td>
<td><code>service-rate-limit</code></td>
<td>Limite la velocidad de procesamiento de solicitudes y el número de conexiones por una clave definida para servicios específicos.</td>
</tr>
</tbody></table>

<br>



## Anotaciones generales
{: #general}

### Servicios externos (proxy-external-service)
{: #proxy-external-service}

Añada definiciones de vía de acceso a servicios externos, como servicios alojados en {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}

<dl>
<dt>Descripción</dt>
<dd>Añada definiciones de vía de acceso a servicios externos. Utilice esta anotación sólo si su app opera en un servicio externo en lugar de un servicio de fondo. Cuando se utiliza esta anotación para crear una ruta de servicio externo, sólo las anotaciones `client-max-body-size`, `proxy-read-timeout`, `proxy-connect-timeout` y `proxy-buffering` están soportadas de forma conjunta. Las demás anotaciones no están soportadas de forma conjunta con `proxy-external-service`.
</dd>
<dt>YAML del recurso de Ingress de ejemplo</dt>
<dd>

<pre class="codeblock">
<code>
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: cafe-ingress
  annotations:
    ingress.bluemix.net/proxy-external-service: "path=&lt;mypath&gt; external-svc=https:&lt;external_service&gt; host=&lt;mydomain&gt;"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 80
</code></pre>

<table>
<caption>Componentes de anotación</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
 </thead>
 <tbody>
 <tr>
 <td><code>path</code></td>
 <td>Sustituya <code>&lt;<em>mypath</em>&gt;</code> por la vía de acceso en la que escucha el servicio externo.</td>
 </tr>
 <tr>
 <td><code>external-svc</code></td>
 <td>Sustituya <code>&lt;<em>external_service</em>&gt;</code> por el servicio externo al que se debe llamar. Por ejemplo, <code>https://&lt;myservice&gt;.&lt;region&gt;.mybluemix.net</code>.</td>
 </tr>
 <tr>
 <td><code>host</code></td>
 <td>Sustituya <code>&lt;<em>mydomain</em>&gt;</code> por el dominio de host para el servicio externo.</td>
 </tr>
 </tbody></table>

 </dd></dl>

<br />


### Modificador de ubicación (location-modifier)
{: #location-modifier}

Modifique la forma en la que el ALB coteja el URI de solicitud con la vía de acceso de la app.
{:shortdesc}

<dl>
<dt>Descripción</dt>
<dd>De forma predeterminada, los ALB procesan las vías de acceso en las que escuchan las apps como prefijos. Cuando un ALB recibe una solicitud para una app, el ALB comprueba el recurso de Ingress para una vía de acceso (como prefijo) que coincide con el comienzo del URI de solicitud. Si se encuentra una coincidencia, la solicitud se reenvía a la dirección IP del pod en el que se ha desplegado la app.<br><br>La anotación `location-modifier` cambia la forma en la que el ALB busca coincidencias modificando la configuración de bloques de ubicación. El bloque de ubicación determina cómo se gestionan las solicitudes de vía de acceso de la app.<br><br><strong>Nota</strong>: Para gestionar vías de acceso de expresión regular (regex), es necesaria esta anotación.</dd>

<dt>Modificadores soportados</dt>
<dd>

<table>
<caption>Modificadores soportados</caption>
 <col width="10%">
 <col width="90%">
 <thead>
 <th>Modificador</th>
 <th>Descripción</th>
 </thead>
 <tbody>
 <tr>
 <td><code>=</code></td>
 <td>El modificador del signo igual provoca que el ALB seleccione sólo coincidencias exactas. Cuando se encuentra una coincidencia exacta, la búsqueda se detiene y se selecciona la vía de acceso coincidente.<br>Por ejemplo, si su app está a la escucha en <code>/tea</code>, el ALB únicamente selecciona las vías de acceso <code>/tea</code> exactamente cuando coinciden con una solicitud para su app.</td>
 </tr>
 <tr>
 <td><code>~</code></td>
 <td>El modificador de tilde provoca que el ALB procese las vías de acceso como vía de acceso de expresión regular sensibles a las mayúsculas y minúsculas durante la coincidencia.<br>Por ejemplo, si su app escucha en <code>/coffee</code>, el ALB puede seleccionar las vías de acceso <code>/ab/coffee</code> o <code>/123/coffee</code> al encontrar una solicitud coincidente con su app incluso si las vías de acceso no se han establecido explícitamente para su app.</td>
 </tr>
 <tr>
 <td><code>~\*</code></td>
 <td>El modificador de tilde seguida por un asterisco provoca que el ALB procese las vías de acceso como vías de acceso de expresión regular ignorando las mayúsculas y minúsculas durante la coincidencia.<br>Por ejemplo, si su app escucha en <code>/coffee</code>, el ALB puede seleccionar las vías de acceso <code>/ab/Coffee</code> o <code>/123/COFFEE</code> al encontrar una solicitud coincidente con su app incluso si las vías de acceso no se han establecido explícitamente para su app.</td>
 </tr>
 <tr>
 <td><code>^~</code></td>
 <td>El modificador de acento circunflejo seguido de provoca que el ALB seleccione la mejor coincidencia que no sea expresión regular en lugar de una vía de acceso de expresión regular.</td>
 </tr>
 </tbody>
</table>

</dd>

<dt>YAML del recurso de Ingress de ejemplo</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
name: myingress
annotations:
  ingress.bluemix.net/location-modifier: "modifier='&lt;location_modifier&gt;' serviceName=&lt;myservice1&gt;;modifier='&lt;location_modifier&gt;' serviceName=&lt;myservice2&gt;"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: &lt;myservice&gt;
          servicePort: 80</code></pre>

 <table>
 <caption>Componentes de anotación</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
  </thead>
  <tbody>
  <tr>
  <td><code>modifier</code></td>
  <td>Sustituya <code>&lt;<em>location_modifier</em>&gt;</code> con el modificador de ubicación desea utilizar para la vía de acceso. Los modificadores soportados son <code>'='</code>, <code>'~'</code>, <code>'~\*'</code> y <code>'^~'</code>. Debe rodear los modificadores con comillas simples.</td>
  </tr>
  <tr>
  <td><code>serviceName</code></td>
  <td>Sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre del servicio de Kubernetes que ha creado para la app.</td>
  </tr>
  </tbody></table>
  </dd>
  </dl>

<br />


### Direccionamiento de ALB privado (ALB-ID)
{: #alb-id}

Direccione las solicitudes entrantes a sus apps con un ALB privado.
{:shortdesc}

<dl>
<dt>Descripción</dt>
<dd>
Elija un ALB privado para direccionar las solicitudes entrantes en lugar del ALB público.</dd>


<dt>YAML del recurso de Ingress de ejemplo</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
name: myingress
annotations:
  ingress.bluemix.net/ALB-ID: "&lt;private_ALB_ID&gt;"
spec:
tls:
- hosts:
  - mydomain
    secretName: mytlssecret
  rules:
- host: mydomain
  http:
    paths:
    - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>Componentes de anotación</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>&lt;private_ALB_ID&gt;</code></td>
<td>El ID de su ALB. Para encontrar el ID de ALB privado, ejecute <code>bx cs albs --cluster &lt;my_cluster&gt;</code>.
</td>
</tr>
</tbody></table>
</dd>
</dl>

<br />


### Vías de acceso de reescritura (rewrite-path)
{: #rewrite-path}

Direccione el tráfico de red de entrada en una vía de acceso de dominio de ALB a una vía de acceso diferente que escuche su aplicación de fondo.
{:shortdesc}

<dl>
<dt>Descripción</dt>
<dd>El dominio del ALB de Ingress direcciona el tráfico de entrada de la red de <code>mykubecluster.us-south.containers.appdomain.cloud/beans</code> a la app. La app escucha en <code>/coffee</code> en lugar de en <code>/beans</code>. Para reenviar el tráfico de red entrante a la app, añada la anotación de reescritura al archivo de configuración del recurso. La anotación de reescritura garantiza que el tráfico de red entrante de <code>/beans</code> se reenvíe a la app utilizando la vía de acceso <code>/coffee</code>. Si incluye varios servicios, utilice únicamente un punto y coma (;) para separarlos.</dd>
<dt>YAML del recurso de Ingress de ejemplo</dt>
<dd>
<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/rewrite-path: "serviceName=&lt;myservice1&gt; rewrite=&lt;target_path1&gt;;serviceName=&lt;myservice2&gt; rewrite=&lt;target_path2&gt;"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /beans
        backend:
          serviceName: myservice1
          servicePort: 80
</code></pre>

<table>
<caption>Componentes de anotación</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre del servicio de Kubernetes que ha creado para la app.</td>
</tr>
<tr>
<td><code>rewrite</code></td>
<td>Sustituya <code>&lt;<em>target_path</em>&gt;</code> por la vía de acceso en la que escucha la app. El tráfico de red de entrada del dominio de ALB se reenvía al servicio Kubernetes mediante esta vía de acceso. La mayoría de las apps no escuchan en una vía de acceso específica, sino que utilizan la vía de acceso raíz y un puerto específico. En el ejemplo anterior, la vía de acceso de reescritura se ha definido como <code>/coffee</code>.</td>
</tr>
</tbody></table>

</dd></dl>

<br />


### Puertos TCP para los equilibradores de carga de aplicación (tcp-ports)
{: #tcp-ports}

Acceda a una app a través de un puerto TCP no estándar.
{:shortdesc}

<dl>
<dt>Descripción</dt>
<dd>
Utilice esta anotación para una app que esté ejecutando una carga de trabajo con secuencias TCP.

<p>**Nota**: El ALB funciona en modalidad de paso y envía tráfico a las apps de fondo. En este caso, no se soporta la terminación SSL.</p>
</dd>


<dt>YAML del recurso de Ingress de ejemplo</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
name: myingress
annotations:
  ingress.bluemix.net/tcp-ports: "serviceName=&lt;myservice&gt; ingressPort=&lt;ingress_port&gt; [servicePort=&lt;service_port&gt;]"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: &lt;myservice&gt;
          servicePort: 80</code></pre>

 <table>
 <caption>Componentes de anotación</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
  </thead>
  <tbody>
  <tr>
  <td><code>serviceName</code></td>
  <td>Sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre del servicio Kubernetes para acceder a través del puerto TCP no estándar.</td>
  </tr>
  <tr>
  <td><code>ingressPort</code></td>
  <td>Sustituya <code>&lt;<em>ingress_port</em>&gt;</code> por el puerto TCP en el que desea acceder a la app.</td>
  </tr>
  <tr>
  <td><code>servicePort</code></td>
  <td>Este parámetro
es opcional. Cuando se proporciona, el puerto se sustituye por este valor antes de que el tráfico se envíe a la app de backend. En caso contrario, el puerto permanece igual que el puerto de Ingress.</td>
  </tr>
  </tbody></table>

 </dd>
 <dt>Uso</dt>
 <dd><ol><li>Revise los puertos abiertos en el ALB.
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
La salida de la CLI se parecerá a la siguiente:
<pre class="screen">
<code>NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                      AGE
public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx 80:30416/TCP,443:32668/TCP   109d</code></pre></li>
<li>Abra el mapa de configuración del ALB.
<pre class="pre">
<code>kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system</code></pre></li>
<li>Añada los puertos TCP al mapa de configuración. Sustituya <code>&lt;port&gt;</code> por los puertos TCP que desea abrir. <b>Nota</b>: De forma predeterminada, los puertos 80 y 443 están abiertos. Si desea mantener los puertos 80 y 443 abiertos, también debe incluirlos, además de cualquier otro puerto TCP que especifique en el campo `public-ports`. Si ha habilitado un ALB privado, también debe especificar los puertos que desea mantener abiertos en el campo `private-ports`. Para obtener más información, consulte <a href="cs_ingress.html#opening_ingress_ports">Apertura de puertos en el ALB de Ingress</a>.
<pre class="codeblock">
<code>apiVersion: v1
kind: ConfigMap
data:
 public-ports: 80;443;&lt;port1&gt;;&lt;port2&gt;
metadata:
 creationTimestamp: 2017-08-22T19:06:51Z
 name: ibm-cloud-provider-ingress-cm
 namespace: kube-system
 resourceVersion: "1320"
 selfLink: /api/v1/namespaces/kube-system/configmaps/ibm-cloud-provider-ingress-cm
 uid: &lt;uid&gt;</code></pre></li>
 <li>Verifique que el ALB se ha vuelto a configurar con los puertos TCP.
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
La salida de la CLI se parecerá a la siguiente:
<pre class="screen">
<code>NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                      AGE
public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx  169.xx.xxx.xxx &lt;port1&gt;:30776/TCP,&lt;port2&gt;:30412/TCP   109d</code></pre></li>
<li>Configure Ingress para acceder a la app a través de un puerto TCP no estándar. Utilice el archivo YAML de ejemplo de esta referencia. </li>
<li>Actualice la configuración del ALB.
<pre class="pre">
<code>kubectl apply -f myingress.yaml</code></pre>
</li>
<li>Abra el navegador web preferido para acceder a la app. Ejemplo: <code>https://&lt;ibmdomain&gt;:&lt;ingressPort&gt;/</code></li></ol></dd></dl>

<br />


## Anotaciones de conexión
{: #connection}

### Tiempos de espera excedidos de conexión y tiempos de espera excedidos de lectura personalizados (proxy-connect-timeout, proxy-read-timeout)
{: #proxy-connect-timeout}

Defina el tiempo que el ALB espera para conectarse a la app de fondo y para leer información de la misma antes de considerar que la app de fondo no está disponible.
{:shortdesc}

<dl>
<dt>Descripción</dt>
<dd>Cuando se envía una solicitud del cliente al ALB de Ingress, el ALB abre una conexión con la app de fondo. De forma predeterminada, el ALB espera 60 segundos para recibir una respuesta de la app de fondo. Si la app de fondo no responde en un plazo de 60 segundos, la solicitud de conexión se cancela y se considera que la app de fondo no está disponible.

</br></br>
Después de que el ALB se conecte a la app de fondo, lee los datos de la respuesta procedente de la app de fondo. Durante esta operación de lectura, el ALB espera un máximo de 60 segundos entre dos operaciones de lectura para recibir datos de la app de fondo. Si la app de fondo no envía datos en un plazo de 60 segundos, la conexión con la app de fondo se cierra y se considera que la app no está disponible.
</br></br>
60 segundos es el valor predeterminad para el tiempo de espera excedido de conexión (connect-timeout) y el tiempo de espera excedido de lectura (read-timeout) en un proxy y lo recomendable es no modificarlo.
</br></br>
Si la disponibilidad de la app no es estable o si la app es lenta en responder debido a cargas de trabajo elevadas, es posible que desee aumentar el valor de connect-timeout o de read-timeout. Tenga en cuenta que aumentar el tiempo de espera excedido afecta al rendimiento del ALB, ya que la conexión con la app de fondo debe permanecer abierta hasta que se alcanza el tiempo de espera excedido.
</br></br>
Por otro lado, puede reducir el tiempo de espera para aumentar el rendimiento del ALB. Asegúrese de que la app de fondo es capaz de manejar las solicitudes dentro del tiempo de espera especificado, incluso con cargas de trabajo altas.</dd>
<dt>YAML del recurso de Ingress de ejemplo</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/proxy-connect-timeout: "serviceName=&lt;myservice&gt; timeout=&lt;connect_timeout&gt;"
   ingress.bluemix.net/proxy-read-timeout: "serviceName=&lt;myservice&gt; timeout=&lt;read_timeout&gt;"
spec:
 tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
     paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>Componentes de anotación</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;connect_timeout&gt;</code></td>
 <td>El número de segundos o minutos que se debe esperar para conectar con la app de fondo, por ejemplo <code>65s</code> o <code>2m</code>. <strong>Nota:</strong> El valor de connect-timeout no puede superar los 75 segundos.</td>
 </tr>
 <tr>
 <td><code>&lt;read_timeout&gt;</code></td>
 <td>El número de segundos o minutos que se debe esperar a que se lea la app de fondo, por ejemplo <code>65s</code> o <code>2m</code>.
 </tr>
 </tbody></table>

 </dd></dl>

<br />



### Solicitudes de estado activo (keepalive-requests)
{: #keepalive-requests}

Establece el número máximo de solicitudes que se pueden servir a través de una conexión en estado activo.
{:shortdesc}

<dl>
<dt>Descripción</dt>
<dd>
Establece el número máximo de solicitudes que se pueden servir a través de una conexión en estado activo.
</dd>


<dt>YAML del recurso de Ingress de ejemplo</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
name: myingress
annotations:
  ingress.bluemix.net/keepalive-requests: "serviceName=&lt;myservice&gt; requests=&lt;max_requests&gt;"
spec:
tls:
- hosts:
  - mydomain
    secretName: mytlssecret
  rules:
- host: mydomain
  http:
    paths:
    - path: /
      backend:
        serviceName: &lt;myservice&gt;
        servicePort: 8080</code></pre>

<table>
<caption>Componentes de anotación</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre del servicio de Kubernetes que ha creado para la app. Este parámetro
es opcional. La configuración se aplica a todos los servicios del host de Ingress a menos que se especifique un servicio. Si se proporciona el parámetro, se establecen las solicitudes de estado activo para el servicio en cuestión. Si no se proporciona el parámetro, las solicitudes de estado activo se establecen a nivel de servidor de <code>nginx.conf</code> para todos los servicios que no tienen configuradas solicitudes de estado activo.</td>
</tr>
<tr>
<td><code>requests</code></td>
<td>Sustituya <code>&lt;<em>max_requests</em>&gt;</code> por el número máximo de solicitudes que se pueden servir a través de una conexión de estado activo.</td>
</tr>
</tbody></table>

</dd>
</dl>

<br />



### Tiempo de espera de estado activo (keepalive-timeout)
{: #keepalive-timeout}

Establece el tiempo máximo que se mantiene abierta una conexión en estado activo en el lado del servidor.
{:shortdesc}

<dl>
<dt>Descripción</dt>
<dd>
Establece el tiempo máximo que se mantiene abierta una conexión en estado activo en el servidor.
</dd>


<dt>YAML del recurso de Ingress de ejemplo</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/keepalive-timeout: "serviceName=&lt;myservice&gt; timeout=&lt;time&gt;s"
spec:
 tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
     paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>Componentes de anotación</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
 </thead>
 <tbody>
 <tr>
 <td><code>serviceName</code></td>
 <td>Sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre del servicio de Kubernetes que ha creado para la app. Este parámetro
es opcional. Si se proporciona el parámetro, se establece el tiempo de espera de estado activo para el servicio en cuestión. Si no se proporciona el parámetro, el tiempo de espera de estado activo se establece a nivel de servidor de <code>nginx.conf</code> para todos los servicios que no tienen configurado el tiempo de espera de estado activo.</td>
 </tr>
 <tr>
 <td><code>timeout</code></td>
 <td>Sustituya <code>&lt;<em>time</em>&gt;</code> por una cantidad de tiempo en segundos. Ejemplo:<code>timeout=20s</code>. Un valor de cero inhabilita las conexiones de cliente de estado activo.</td>
 </tr>
 </tbody></table>

 </dd>
 </dl>

<br />


### Siguiente proxy en sentido ascendente (proxy-next-upstream-config)
{: #proxy-next-upstream-config}

Establece cuándo el ALB puede pasar una solicitud al siguiente servidor en sentido ascendente.
{:shortdesc}

<dl>
<dt>Descripción</dt>
<dd>
El ALB de Ingress actúa como un proxy entre la app del cliente y la app. Algunas configuraciones de app precisan de varios servidores en sentido ascendente para manejar las solicitudes de cliente entrantes desde el ALB. A veces el servidor proxy que utiliza el ALB no puede establecer una conexión con un servidor en sentido ascendente que utilice la app. El ALB, puede entonces intentar establecer una conexión con el siguiente servidor en sentido ascendente para pasarle en su lugar la solicitud. Puede utilizar la anotación `proxy-next-upstream-config` para definir en qué casos, cuánto tiempo y cuántas veces el ALB puede intentar pasar una solicitud al siguiente servidor en sentido ascendente.<br><br><strong>Nota</strong>: Siempre se configura un tiempo de espera al utilizar `proxy-next-upstream-config`, de forma que no añada `timeout=true` a esta anotación.
</dd>
<dt>YAML del recurso de Ingress de ejemplo</dt>
<dd>
<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/proxy-next-upstream-config: "serviceName=&lt;myservice1&gt; retries=&lt;tries&gt; timeout=&lt;time&gt; error=true http_502=true; serviceName=&lt;myservice2&gt; http_403=true non_idempotent=true"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice1
          servicePort: 80
</code></pre>

<table>
<caption>Componentes de anotación</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre del servicio de Kubernetes que ha creado para la app.</td>
</tr>
<tr>
<td><code>retries</code></td>
<td>Sustituya <code>&lt;<em>tries</em>&gt;</code> con el número máximo de veces que ALB debe intentar pasar una solicitud al siguiente servidor en sentido ascendente. Este valor incluye la solicitud original. Para desactivar esta limitación, utilice <code>0</code>. Si no especifica un valor, se utiliza el valor predeterminado <code>0</code>.
</td>
</tr>
<tr>
<td><code>timeout</code></td>
<td>Sustituya <code>&lt;<em>time</em>&gt;</code> con el máximo de tiempo, en segundos, que ALB debe intentar pasar una solicitud al siguiente servidor en sentido ascendente. Por ejemplo, para establecer un tiempo de 30 segundos, especifique <code>30s</code>. Para desactivar esta limitación, utilice <code>0</code>. Si no especifica un valor, se utiliza el valor predeterminado <code>0</code>.
</td>
</tr>
<tr>
<td><code>error</code></td>
<td>Si se establece en <code>true</code>, el ALB pasa una solicitud al siguiente servidor en sentido ascendente cuando se produce un error al establecer una conexión con el primer servidor en sentido ascendente, pasando una solicitud al mismo, o leyendo la cabecera de respuesta.
</td>
</tr>
<tr>
<td><code>invalid_header</code></td>
<td>Si se establece en <code>true</code>, el ALB pasa una solicitud al siguiente servidor en sentido ascendente cuando el primer servidor en sentido ascendente devuelve una respuesta vacía o no válida.
</td>
</tr>
<tr>
<td><code>http_502</code></td>
<td>Si se establece en <code>true</code>, el ALB pasa una solicitud al siguiente servidor en sentido ascendente cuando el primer servidor en sentido ascendente devuelve una respuesta con el código 502. Puede designar los siguientes códigos de respuesta HTTP: <code>500</code>, <code>502</code>, <code>503</code>, <code>504</code>, <code>403</code>, <code>404</code>, <code>429</code>.
</td>
</tr>
<tr>
<td><code>non_idempotent</code></td>
<td>Si se establece en <code>true</code>, el ALB puede pasar solicitudes con un método no idempotente al siguiente servidor en sentido ascendente. De forma predeterminada, el ALB no pasa estas solicitudes al siguiente servidor en sentido ascendente.
</td>
</tr>
<tr>
<td><code>off</code></td>
<td>Establezca este valor en <code>true</code> para evitar que el ALB pase solicitudes al siguiente servidor en sentido ascendente.
</td>
</tr>
</tbody></table>
</dd>
</dl>

<br />


### Afinidad de sesión con cookies (sticky-cookie-services)
{: #sticky-cookie-services}

Utilice la anotación de cookie sticky para añadir afinidad de sesión al ALB y direccionar siempre el tráfico de red entrante al mismo servidor en sentido ascendente.
{:shortdesc}

<dl>
<dt>Descripción</dt>
<dd>Para la alta disponibilidad, es posible que la configuración de la app requiera que despliegue varios servidores en sentido ascendente para que gestionen las solicitudes entrantes del cliente y garanticen una mayor disponibilidad. Cuando un cliente se conecta a la app de fondo, podría ser útil que un cliente recibiera el servicio del mismo servidor en sentido ascendente mientras dure la sesión o durante el tiempo que se tarda en completar una tarea. Puede configurar el ALB de modo que asegure la afinidad de sesión direccionando siempre el tráfico de red de entrada al mismo servidor en sentido ascendente.

</br></br>
El ALB asigna cada cliente que se conecta a la app de fondo a uno de los servidores en sentido ascendente disponibles. El ALB crea una cookie de sesión que se almacena en la app del cliente y que se incluye en la información de cabecera de cada solicitud entre el ALB y el cliente. La información de la cookie garantiza que todas las solicitudes se gestionan en el mismo servidor en sentido ascendente durante todo el periodo de la sesión.

</br></br>
Si incluye varios servicios, utilice únicamente un punto y coma (;) para separarlos.</dd>
<dt>YAML del recurso de Ingress de ejemplo</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/sticky-cookie-services: "serviceName=&lt;myservice1&gt; name=&lt;cookie_name1&gt; expires=&lt;expiration_time1&gt; path=&lt;cookie_path1&gt; hash=&lt;hash_algorithm1&gt;;serviceName=&lt;myservice2&gt; name=&lt;cookie_name2&gt; expires=&lt;expiration_time2&gt; path=&lt;cookie_path2&gt; hash=&lt;hash_algorithm2&gt;"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: &lt;myservice1&gt;
          servicePort: 8080
      - path: /service2_path
        backend:
          serviceName: &lt;myservice2&gt;
          servicePort: 80</code></pre>

  <table>
  <caption>Componentes de anotación</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
  </thead>
  <tbody>
  <tr>
  <td><code>serviceName</code></td>
  <td>Sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre del servicio de Kubernetes que ha creado para la app.</td>
  </tr>
  <tr>
  <td><code>name</code></td>
  <td>Sustituya <code>&lt;<em>cookie_name</em>&gt;</code> por el nombre de una "stickie cookie" que se crea durante una sesión.</td>
  </tr>
  <tr>
  <td><code>expires</code></td>
  <td>Sustituya <code>&lt;<em>expiration_time</em>&gt;</code> por el tiempo en segundos (s), minutos (m) u horas (h) antes de que caduque la "sticky cookie". Este tiempo depende de la actividad del usuario. Una vez caducada la cookie, el navegador web del cliente la suprime y se deja de enviar al ALB. Por ejemplo, para establecer un tiempo de caducidad de 1 segundo, 1 minutos o 1 hora, escriba <code>1s</code>, <code>1m</code> o <code>1h</code>.</td>
  </tr>
  <tr>
  <td><code>path</code></td>
  <td>Sustituya <code>&lt;<em>cookie_path</em>&gt;</code> por vía de acceso que se añade al subdominio de Ingress y que indica los dominios y subdominios para los que se envía la cookie al ALB. Por ejemplo, si el dominio de Ingress es <code>www.myingress.com</code> y desea enviar la cookie en cada solicitud del cliente, debe establecer <code>path=/</code>. Si desea enviar la cookie solo para <code>www.myingress.com/myapp</code> y todos sus subdominios, establezca este valor en <code>path=/myapp</code>.</td>
  </tr>
  <tr>
  <td><code>hash</code></td>
  <td>Sustituya <code>&lt;<em>hash_algorithm</em>&gt;</code> por el algoritmo hash que protege la información en la cookie. Solo se admite <code>sha1
</code>. SHA1 crea una suma de hash en función de la información de la cookie y añade esta suma de hash a la cookie. El servidor puede descifrar la información de la cookie y verificar la integridad de los datos.</td>
  </tr>
  </tbody></table>

 </dd></dl>

<br />




### Estado activo en sentido ascendente (upstream-keepalive)
{: #upstream-keepalive}

Establece el número máximo de conexiones de estado activo para un servidor en sentido ascendente.
{:shortdesc}

<dl>
<dt>Descripción</dt>
<dd>
Define el número máximo de conexiones de estado activo desocupadas para el servidor en sentido ascendente de un servicio determinado. El servidor en sentido ascendente tiene 64 conexiones de estado activo desocupadas de forma predeterminada.
</dd>


 <dt>YAML del recurso de Ingress de ejemplo</dt>
 <dd>

 <pre class="codeblock">
 <code>apiVersion: extensions/v1beta1
 kind: Ingress
 metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/upstream-keepalive: "serviceName=&lt;myservice&gt; keepalive=&lt;max_connections&gt;"
 spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

 <table>
 <caption>Componentes de anotación</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
  </thead>
  <tbody>
  <tr>
  <td><code>serviceName</code></td>
  <td>Sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre del servicio de Kubernetes que ha creado para la app.</td>
  </tr>
  <tr>
  <td><code>keepalive</code></td>
  <td>Sustituya <code>&lt;<em>max_connections</em>&gt;</code> por el número máximo de conexiones de estado activo desocupadas para el servidor en sentido ascendente. El valor predeterminado es <code>64</code>. Un valor de <code>0</code> inhabilita las conexiones de estado activo en sentido ascendente para el servicio determinado.</td>
  </tr>
  </tbody></table>
  </dd>
  </dl>

<br />




## Anotaciones de autenticación TLS/SSL y HTTPS
{: #https-auth}

### Autenticación de {{site.data.keyword.appid_short_notm}} (appid-auth)
{: #appid-auth}

  Utilice {{site.data.keyword.appid_full_notm}} para autenticarse con la aplicación.
  {:shortdesc}

  <dl>
  <dt>Descripción</dt>
  <dd>
  Autentique las solicitudes HTTP/HTTPS de API o web con {{site.data.keyword.appid_short_notm}}.

  <p>Si establece el tipo de solicitud en <code>web</code>, se valida una solicitud web que contiene una señal de acceso de {{site.data.keyword.appid_short_notm}}. Si la validación de señal falla, la solicitud web es rechazada. Si la solicitud no contiene ninguna señal de acceso, la solicitud se redirige a la página de inicio de sesión de {{site.data.keyword.appid_short_notm}}. <strong>Nota</strong>: Para que funcione la autenticación web de {{site.data.keyword.appid_short_notm}}, las cookies deben estar habilitadas en el navegador del usuario.</p>

  <p>Si establece el tipo de solicitud en <code>api</code>, se valida una solicitud de API que contiene una señal de acceso de {{site.data.keyword.appid_short_notm}}. Si la solicitud no contiene ninguna señal de acceso, el usuario recibe un mensaje de error <code>401: Unauthorized</code>.</p>

  <p>**Nota**: Por razones de seguridad, la autenticación de {{site.data.keyword.appid_short_notm}} solo da soporte a sistemas de fondos que tenga TLS/SSL habilitado.</p>
  </dd>
   <dt>YAML del recurso de Ingress de ejemplo</dt>
   <dd>

   <pre class="codeblock">
   <code>apiVersion: extensions/v1beta1
   kind: Ingress
   metadata:
    name: myingress
    annotations:
      ingress.bluemix.net/appid-auth: "bindSecret=&lt;bind_secret&gt; namespace=&lt;namespace&gt; requestType=&lt;request_type&gt; serviceName=&lt;myservice&gt;"
   spec:
    tls:
    - hosts:
      - mydomain
    secretName: mytlssecret
  rules:
    - host: mydomain
    http:
      paths:
        - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

   <table>
   <caption>Componentes de anotación</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
    </thead>
    <tbody>
    <tr>
    <td><code>bindSecret</code></td>
    <td>Sustituya <em><code>&lt;bind_secret&gt;</code></em> con el secreto de Kubernetes que almacena el secreto de enlace.</td>
    </tr>
    <tr>
    <td><code>namespace</code></td>
    <td>Sustituya <em><code>&lt;namespace&gt;</code></em> con el espacio de nombres del secreto de enlace. El valor predeterminado de este campo es el espacio de nombres `default`.</td>
    </tr>
    <tr>
    <td><code>requestType</code></td>
    <td>Sustituya <code><em>&lt;request_type&gt;</em></code> con el tipo de solicitud que desea enviar a {{site.data.keyword.appid_short_notm}}. Los valores aceptados son `web` o `api`. El valor predeterminado es `api`.</td>
    </tr>
    <tr>
    <td><code>serviceName</code></td>
    <td>Sustituya <code><em>&lt;myservice&gt;</em></code> por el nombre del servicio de Kubernetes que ha creado para la app. Este campo es opcional. Si no se incluye un nombre de servicio, la anotación se habilita para todos los servicios.  Si se incluye un nombre de servicio, la anotación se habilita solo para ese servicio. Separe varios servicios con punto y coma (;).</td>
    </tr>
    </tbody></table>
    </dd>
    <dt>Uso</dt>
    <dd>Puesto que la aplicación utiliza {{site.data.keyword.appid_short_notm}} para la autenticación, debe suministrar una instancia de {{site.data.keyword.appid_short_notm}}, configurar la instancia con URI de redirección válidos y generar un secreto de enlace.
    <ol>
    <li>Suministre una [instancia de {{site.data.keyword.appid_short_notm}}](https://console.bluemix.net/catalog/services/app-id).</li>
    <li>En la consola de gestión de {{site.data.keyword.appid_short_notm}}, añada URI de redirección para la app.</li>
    <li>Cree un secreto de enlace.
    <pre class="pre"><code>bx cs cluster-service-bind &lt;my_cluster&gt; &lt;my_namespace&gt; &lt;my_service_instance_GUID&gt;</code></pre> </li>
    <li>Configure la anotación <code>appid-auth</code>.</li>
    </ol></dd>
    </dl>

<br />



### Puertos HTTP y HTTPS personalizados (custom-port)
{: #custom-port}

Cambie los puertos predeterminados para el tráfico de red HTTP (puerto 80) y HTTPS (puerto 443).
{:shortdesc}

<dl>
<dt>Descripción</dt>
<dd>De forma predeterminada, el ALB de Ingress está configurado para escuchar el tráfico de red de entrada HTTP en el puerto 80 y el tráfico de red de entrada HTTPS en el puerto 443. Puede cambiar los puertos predeterminados para añadir seguridad a su dominio del ALB o para habilitar solo un puerto HTTPS.
</dd>


<dt>YAML del recurso de Ingress de ejemplo</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/custom-port: "protocol=&lt;protocol1&gt; port=&lt;port1&gt;;protocol=&lt;protocol2&gt;port=&lt;port2&gt;"
spec:
 tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
     paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>Componentes de anotación</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;protocol&gt;</code></td>
 <td>Especifique <code>http</code> o <code>https</code> para cambiar el puerto predeterminado para el tráfico de red de entrada HTTP o HTTPS.</td>
 </tr>
 <tr>
 <td><code>&lt;port&gt;</code></td>
 <td>Especifique el número de puerto que desea utilizar para el tráfico de red de entrada HTTP o HTTPS.  <p><strong>Nota:</strong> Cuando se especifica un puerto personalizado para HTTP o HTTPS, los puertos predeterminados dejan de ser válidos para HTTP y HTTPS. Por ejemplo, para cambiar el puerto predeterminado de HTTPS por 8443, pero utilizar el puerto predeterminado para HTTP, debe definir ciertos puertos personalizados para ambos: <code>custom-port: "protocol=http port=80; protocol=https port=8443"</code>.</p></td>
 </tr>
 </tbody></table>

 </dd>
 <dt>Uso</dt>
 <dd><ol><li>Revise los puertos abiertos en el ALB.
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
La salida de la CLI se parecerá a la siguiente:
<pre class="screen">
<code>NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                      AGE
public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx 80:30416/TCP,443:32668/TCP   109d</code></pre></li>
<li>Abra el mapa de configuración del ALB.
<pre class="pre">
<code>kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system</code></pre></li>
<li>Añada los puertos HTTP y HTTPS no predeterminados al mapa de configuración. Sustituya &lt;port&gt; por el puerto HTTP o HTTPS que desea abrir. <b>Nota</b>: De forma predeterminada, los puertos 80 y 443 están abiertos. Si desea mantener los puertos 80 y 443 abiertos, también debe incluirlos, además de cualquier otro puerto TCP que especifique en el campo `public-ports`. Si ha habilitado un ALB privado, también debe especificar los puertos que desea mantener abiertos en el campo `private-ports`. Para obtener más información, consulte <a href="cs_ingress.html#opening_ingress_ports">Apertura de puertos en el ALB de Ingress</a>.
<pre class="codeblock">
<code>apiVersion: v1
kind: ConfigMap
data:
 public-ports: &lt;port1&gt;;&lt;port2&gt;
metadata:
 creationTimestamp: 2017-08-22T19:06:51Z
 name: ibm-cloud-provider-ingress-cm
 namespace: kube-system
 resourceVersion: "1320"
 selfLink: /api/v1/namespaces/kube-system/configmaps/ibm-cloud-provider-ingress-cm
 uid: &lt;uid&gt;</code></pre></li>
 <li>Verifique que el ALB se ha vuelto a configurar con los puertos no predeterminados.
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
La salida de la CLI se parecerá a la siguiente:
<pre class="screen">
<code>NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                      AGE
public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx  169.xx.xxx.xxx &lt;port1&gt;:30776/TCP,&lt;port2&gt;:30412/TCP   109d</code></pre></li>
<li>Configure Ingress para que utilice los puertos no predeterminados cuando direccione el tráfico de entrada de la red a sus servicios. Utilice el archivo YAML de ejemplo de esta referencia. </li>
<li>Actualice la configuración del ALB.
<pre class="pre">
<code>kubectl apply -f myingress.yaml</code></pre>
</li>
<li>Abra el navegador web preferido para acceder a la app. Ejemplo: <code>https://&lt;ibmdomain&gt;:&lt;port&gt;/&lt;service_path&gt;/</code></li></ol></dd></dl>

<br />



### HTTP redirige a HTTPS (redirect-to-https)
{: #redirect-to-https}

Convierte las solicitudes de cliente HTTP no seguras en HTTPS.
{:shortdesc}

<dl>
<dt>Descripción</dt>
<dd>Puede configurar el ALB de Ingress de modo que proteja el dominio con el certificado TLS proporcionado por IBM o por un certificado TLS personalizado. Es posible que algunos usuarios intenten acceder a sus apps mediante una solicitud <code>http</code> no segura al dominio del ALB, por ejemplo <code>http://www.myingress.com</code>, en lugar hacerlo de mediante <code>https</code>. Puede utilizar la anotación redirect para convertir siempre las solicitudes HTTP no seguras en HTTPS. Si no utiliza esta anotación, las solicitudes HTTP no seguras no se convertirán en solicitud HTTPS de forma predeterminada y se podría exponer información confidencial sin cifrado al público.

</br></br>
La redirección de solicitudes HTTP a HTTPS está inhabilitada de forma predeterminada.</dd>

<dt>YAML del recurso de Ingress de ejemplo</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/redirect-to-https: "True"
spec:
 tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
     paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

</dd>

</dl>

<br />


### HTTP con seguridad de transporte estricta (hsts)
{: #hsts}

<dl>
<dt>Descripción</dt>
<dd>
HSTS indica al navegador que acceda sólo a un dominio mediante HTTPS. Incluso si el usuario entra o sigue un enlace HTTP sin formato, el navegador actualiza estrictamente la conexión a HTTPS.
</dd>


<dt>YAML del recurso de Ingress de ejemplo</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/hsts: enabled=true maxAge=&lt;31536000&gt; includeSubdomains=true
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: myservice1
          servicePort: 8443
      - path: /service2_path
        backend:
          serviceName: myservice2
          servicePort: 8444
          </code></pre>

<table>
<caption>Componentes de anotación</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
  </thead>
  <tbody>
  <tr>
  <td><code>enabled</code></td>
  <td>Utilice <code>true</code> para habilitar HSTS.</td>
  </tr>
    <tr>
  <td><code>maxAge</code></td>
  <td>Sustituya <code>&lt;<em>31536000</em>&gt;</code> con un entero que represente cuántos segundos almacenará en caché el navegador el envío de solicitudes directamente a HTTPS. El valor predeterminado es <code>31536000</code>, que equivale a 1 año.</td>
  </tr>
  <tr>
  <td><code>includeSubdomains</code></td>
  <td>Utilice <code>true</code> para indicar al navegador que la política de HSTS también se aplica a todos los subdominios del dominio actual. El valor predeterminado es <code>true</code>. </td>
  </tr>
  </tbody></table>

  </dd>
  </dl>


<br />


### Autenticación mutual (mutual-auth)
{: #mutual-auth}

Configure la autenticación mutua para el ALB.
{:shortdesc}

<dl>
<dt>Descripción</dt>
<dd>
Configure la autenticación mutua para el tráfico en sentido descendente para el ALB de Ingress. El cliente externo autentica el servidor y el servidor también autentica el cliente mediante certificados. La autenticación mutua también se conoce como autenticación basada en certificado o autenticación bidireccional.
</dd>

<dt>Requisitos previos</dt>
<dd>
<ul>
<li>[Debe tener un secreto válido que contenga la autoridad de certificado necesaria (CA)](cs_app.html#secrets). Para autenticarse con la autenticación mutua también son necesarios <code>client.key</code> y <code>client.crt</code>.</li>
<li>Para habilitar la autenticación mutua en un puerto distinto de 443, [configure el ALB para abrir el puerto válido](cs_ingress.html#opening_ingress_ports).</li>
</ul>
</dd>


<dt>YAML del recurso de Ingress de ejemplo</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
name: myingress
annotations:
  ingress.bluemix.net/mutual-auth: "secretName=&lt;mysecret&gt; port=&lt;port&gt; serviceName=&lt;servicename1&gt;,&lt;servicename2&gt;"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>Componentes de anotación</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>secretName</code></td>
<td>Sustituya <code>&lt;<em>mysecret</em>&gt;</code> por el nombre del recurso de secreto.</td>
</tr>
<tr>
<td><code>port</code></td>
<td>Sustituya <code>&lt;<em>port</em>&gt;</code> con el número de puerto del ALB.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>Sustituya <code>&lt;<em>servicename</em>&gt;</code> con el nombre de uno o varios recursos de Ingress. Este parámetro
es opcional.</td>
</tr>
</tbody></table>

</dd>
</dl>

<br />



### Soporte de servicios SSL (ssl-services)
{: #ssl-services}

Permite las solicitudes HTTPS y cifra el tráfico en las apps ascendentes.
{:shortdesc}

<dl>
<dt>Descripción</dt>
<dd>
Cifra el tráfico que Ingress envía a las apps en sentido ascendente que precisan HTTPS. Si sus apps en sentido ascendente pueden manejar TLS, puede proporcionar de forma opcional un certificado contenido en un secreto TLS.<br></br>**Opcional**: Puede añadir [autenticación unidireccional o autenticación mutua](#ssl-services-auth) a esta anotación.</dd>


<dt>YAML del recurso de Ingress de ejemplo</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: &lt;myingressname&gt;
  annotations:
    ingress.bluemix.net/ssl-services: "ssl-service=&lt;myservice1&gt; [ssl-secret=&lt;service1-ssl-secret&gt;];ssl-service=&lt;myservice2&gt; [ssl-secret=&lt;service2-ssl-secret&gt;]
spec:
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: myservice1
          servicePort: 8443
      - path: /service2_path
        backend:
          serviceName: myservice2
          servicePort: 8444</code></pre>

<table>
<caption>Componentes de anotación</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
  </thead>
  <tbody>
  <tr>
  <td><code>ssl-service</code></td>
  <td>Sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre del servicio que precisa HTTPS. Se cifrará el tráfico entre el ALB y este servicio de app.</td>
  </tr>
  <tr>
  <td><code>ssl-secret</code></td>
  <td>Opcional: Si desea utilizar un secreto TLS y su app en sentido ascendente puede manejar TLS, sustituya <code>&lt;<em>service-ssl-secret</em>&gt;</code> con el secreto para el servicio. Si desea proporcionar un secreto, el valor debe contener el <code>trusted.crt</code>, <code>client.crt</code> y <code>client.key</code> que su app espera del cliente. Para crear un secreto TLS, primero [convierta los certificados y claves en base-64 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.base64encode.org/). A continuación, consulte [Creación de secretos](cs_app.html#secrets).</td>
  </tr>
  </tbody></table>

  </dd>
</dl>

<br />


#### Soporte de servicios SSL con autenticación
{: #ssl-services-auth}

<dl>
<dt>Descripción</dt>
<dd>
Permita las solicitudes HTTPS y cifre el tráfico a las apps ascendentes con autenticación unidireccional o mutua para la seguridad adicional.
</dd>


<dt>YAML del recurso de Ingress de ejemplo</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: &lt;myingressname&gt;
  annotations:
    ingress.bluemix.net/ssl-services: |
      ssl-service=&lt;myservice1&gt; ssl-secret=&lt;service1-ssl-secret&gt;;
      ssl-service=&lt;myservice2&gt; ssl-secret=&lt;service2-ssl-secret&gt;
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: myservice1
          servicePort: 8443
      - path: /service2_path
        backend:
          serviceName: myservice2
          servicePort: 8444
          </code></pre>

<table>
<caption>Componentes de anotación</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
  </thead>
  <tbody>
  <tr>
  <td><code>ssl-service</code></td>
  <td>Sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre del servicio que precisa HTTPS. Se cifrará el tráfico entre el ALB y este servicio de app.</td>
  </tr>
  <tr>
  <td><code>ssl-secret</code></td>
  <td>Sustituya <code>&lt;<em>service-ssl-secret</em>&gt;</code> con el secreto de autenticación mutua para el servicio. El valor debe contener el certificado de la entidad emisora de certificados (CA) que su app espera del cliente. Para crear un secreto de autenticación mutuo, primero [convierta el certificado y la clave en base-64 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.base64encode.org/). A continuación, consulte [Creación de secretos](cs_app.html#secrets).</td>
  </tr>
  </tbody></table>

  </dd>
  </dl>

<br />


## Anotaciones de Istio
{: #istio-annotations}

### Servicios de Istio (istio-services)
{: #istio-services}

  Direcciona el tráfico a los servicios gestionados por Istio.
  {:shortdesc}

  <dl>
  <dt>Descripción</dt>
  <dd>
  Si tiene servicios gestionados por Istio, puede utilizar un clúster ALB para direccionar las solicitudes HTTP/HTTPS al controlador de Ingress Istio. A continuación, el controlador de Istio Ingress direcciona las solicitudes a los servicios de app. Para poder direccionar el tráfico, es necesario cambiar los recursos de Ingress tanto para el clúster ALB como al controlador de Istio Ingress.
    <br><br>En el recurso Ingress para el clúster ALB debe:
      <ul>
        <li>especificar la anotación `istio-services`</li>
        <li>definir la vía de acceso del servicio como la vía de acceso actual en la que la app está a la escucha</li>
        <li>definir el puerto de servicio como el puerto del controlador de Ingress Istio</li>
      </ul>
    <br>En el recurso Ingress para el controlador Istio Ingress debe:
      <ul>
        <li>definir la vía de acceso del servicio como la vía de acceso actual en la que la app está a la escucha</li>
        <li>definir el puerto de servicio como el puerto HTTP/HTTPS del servicio de app que expone el controlador de Istio Ingress</li>
    </ul>
  </dd>

   <dt>YAML del recurso de Ingress de ejemplo para el clúster ALB</dt>
   <dd>

   <pre class="codeblock">
   <code>apiVersion: extensions/v1beta1
   kind: Ingress
   metadata:
    name: myingress
    annotations:
      ingress.bluemix.net/istio-services: "enabled=true serviceName=&lt;myservice1&gt; istioServiceNamespace=&lt;istio-namespace&gt; istioServiceName=&lt;istio-ingress-service&gt;"
   spec:
    tls:
    - hosts:
      - mydomain
    secretName: mytlssecret
  rules:
    - host: mydomain
    http:
      paths:
        - path: &lt;/myapp1&gt;
          backend:
            serviceName: &lt;myservice1&gt;
            servicePort: &lt;istio_ingress_port&gt;
        - path: &lt;/myapp2&gt;
          backend:
            serviceName: &lt;myservice2&gt;
            servicePort: &lt;istio_ingress_port&gt;</code></pre>

   <table>
   <caption>Visión general de los componentes del archivo YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono Idea"/> Visión general de los componentes del archivo YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code>enabled</code></td>
      <td>Para habilitar el direccionamiento del tráfico de servicios gestionados de Istio, establézcalo en <code>True</code>.</td>
    </tr>
    <tr>
    <td><code>serviceName</code></td>
    <td>Sustituya <code><em>&lt;myservice1&gt;</em></code> por el nombre del servicio de Kubernetes que ha creado para la app gestionada de Istio. Separe varios servicios con punto y coma (;). Este campo es opcional. Si no especifica un nombre de servicio, se habilitan todos los servicios gestionados de Istio para el direccionamiento del tráfico.</td>
    </tr>
    <tr>
    <td><code>istioServiceNamespace</code></td>
    <td>Sustituya <code><em>&lt;istio-namespace&gt;</em></code> con el espacio de nombres de Kubernetes en donde Istio está instalado. Este campo es opcional. Si no especifica un espacio de nombres, se utiliza el espacio de nombres <code>istio-system</code>.</td>
    </tr>
    <tr>
    <td><code>istioServiceName</code></td>
    <td>Sustituya <code><em>&lt;istio-ingress-service&gt;</em></code> con el nombre del servicio de Istio Ingress. Este campo es opcional. Si no especifica el nombre del servicio de Istio Ingress, se utiliza el nombre de servicio <code>istio-ingress</code>.</td>
    </tr>
    <tr>
    <td><code>path</code></td>
      <td>Para cada servicio gestionado de Istio para el que desee direccionar el tráfico, sustituya <code><em>&lt;/myapp1&gt;</em></code> con la vía de acceso del sistema de fondo en el que el servicio gestionado de Istio está a la escucha. La vía de acceso debe corresponder a la vía de acceso que haya definido en el recurso de Ingress.</td>
    </tr>
    <tr>
    <td><code>servicePort</code></td>
    <td>Para cada servicio gestionado de Istio para el que desee direccionar el tráfico, sustituya <code><em>&lt;istio_ingress_port&gt;</em></code> con el puerto del controlador de Istio Ingress.</td>
    </tr>
    </tbody></table>
    </dd>
    </dl>

## Anotaciones de almacenamiento intermedio de proxy
{: #proxy-buffer}


### Colocación en almacenamiento intermedio de datos de respuesta del cliente (proxy-buffering)
{: #proxy-buffering}

Utilice la anotación buffer para inhabilitar el almacenamiento de datos de respuesta en el ALB mientras los datos se envían al cliente.
{:shortdesc}

<dl>
<dt>Descripción</dt>
<dd>El ALB de Ingress actúa como un proxy entre la app de fondo y el navegador web del cliente. Cuando se envía una respuesta de la app de fondo al cliente, los datos de la respuesta se colocan en almacenamiento intermedio en el ALB de forma predeterminada. El ALB procesa mediante proxy la respuesta del cliente y empieza a enviar la respuesta al cliente al ritmo del cliente. Cuando el ALB ha recibido todos los datos procedentes de la app de fondo, la conexión con la app de fondo se cierra. La conexión entre el ALB y el cliente permanece abierta hasta que el cliente haya recibido todos los datos.

</br></br>
Si se inhabilita la colocación en almacenamiento intermedio de los datos de la respuesta en el ALB, los datos se envían inmediatamente del ALB al cliente. El cliente debe ser capaz de manejar los datos de entrada al ritmo del ALB. Si el cliente es demasiado lento, es posible que se pierdan datos.
</br></br>
La colocación en almacenamiento intermedio de datos en el ALB está habilitada de forma predeterminada.</dd>
<dt>YAML del recurso de Ingress de ejemplo</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/proxy-buffering: "enabled=&lt;false&gt; serviceName=&lt;myservice1&gt;"
spec:
 tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
     paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>Componentes de anotación</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
 </thead>
 <tbody>
 <tr>
 <td><code>enabled</code></td>
   <td>Para inhabilitar la creación de almacenamientos intermedios de datos de respuesta en el ALB, establezca el valor en <code>false</code>.</td>
 </tr>
 <tr>
 <td><code>serviceName</code></td>
 <td>Sustituya <code><em>&lt;myservice1&gt;</em></code> por el nombre del servicio de Kubernetes que ha creado para la app. Separe varios servicios con punto y coma (;). Este campo es opcional. Si no especifica un nombre de servicio, todos los servicios utilizarán esta anotación.</td>
 </tr>
 </tbody></table>
 </dd>
 </dl>

<br />



### Almacenamiento intermedio de proxy (proxy-buffers)
{: #proxy-buffers}

Configure el número y el tamaño de los almacenamientos intermedios de proxy para el ALB.
{:shortdesc}

<dl>
<dt>Descripción</dt>
<dd>
Establece el número y el tamaño de los almacenamientos intermedios que leen una respuesta de una única conexión desde el servidor mediante proxy. La configuración se aplica a todos los servicios del host de Ingress a menos que se especifique un servicio. Por ejemplo, si se especifica una configuración como <code>serviceName=SERVICE number=2 size=1k</code>, se aplica 1 k al servicio. Si se especifica una configuración como <code>number=2 size=1k</code>, se aplica 1 k a todos los servicios del host de Ingress.
</dd>
<dt>YAML del recurso de Ingress de ejemplo</dt>
<dd>
<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: proxy-ingress
 annotations:
   ingress.bluemix.net/proxy-buffers: "serviceName=&lt;myservice&gt; number=&lt;number_of_buffers&gt; size=&lt;size&gt;"
spec:
 tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
     paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>Componentes de anotación</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
 </thead>
 <tbody>
 <tr>
 <td><code>serviceName</code></td>
 <td>Sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre de un servicio para aplicar almacenamientos intermedios de proxy.</td>
 </tr>
 <tr>
 <td><code>number</code></td>
 <td>Sustituya <code>&lt;<em>number_of_buffers</em>&gt;</code> por un número, como <em>2</em>.</td>
 </tr>
 <tr>
 <td><code>size</code></td>
 <td>Sustituya <code>&lt;<em>size</em>&gt;</code> por el tamaño de cada almacenamiento intermedio en kilobytes (k o K), como <em>1K</em>.</td>
 </tr>
 </tbody>
 </table>
 </dd></dl>

<br />


### Tamaño de almacenamiento intermedio de proxy (proxy-buffer-size)
{: #proxy-buffer-size}

Configure el tamaño del almacenamiento intermedio de proxy que lee la primera parte de la respuesta.
{:shortdesc}

<dl>
<dt>Descripción</dt>
<dd>
Establezca el tamaño del almacenamiento intermedio que lee la primera parte de la respuesta que se recibe del servidor proxy. Esta parte de la respuesta normalmente contiene una cabecera de respuesta pequeña. La configuración se aplica a todos los servicios del host de Ingress a menos que se especifique un servicio. Por ejemplo, si se especifica una configuración como <code>serviceName=SERVICE size=1k</code>, se aplica 1 k al servicio. Si se especifica una configuración como <code>size=1k</code>, se aplica 1 k a todos los servicios del host de Ingress.
</dd>


<dt>YAML del recurso de Ingress de ejemplo</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: proxy-ingress
 annotations:
   ingress.bluemix.net/proxy-buffer-size: "serviceName=&lt;myservice&gt; size=&lt;size&gt;"
spec:
 tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
     paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
 </code></pre>

<table>
<caption>Componentes de anotación</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
 </thead>
 <tbody>
 <tr>
 <td><code>serviceName</code></td>
 <td>Sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre de un servicio para aplicar proxy-buffers-size.</td>
 </tr>
 <tr>
 <td><code>size</code></td>
 <td>Sustituya <code>&lt;<em>size</em>&gt;</code> por el tamaño de cada almacenamiento intermedio en kilobytes (k o K), como <em>1K</em>.</td>
 </tr>
 </tbody></table>

 </dd>
 </dl>

<br />



### Tamaño de los almacenamientos intermedios ocupados del proxy (proxy-busy-buffers-size)
{: #proxy-busy-buffers-size}

Configure el tamaño de los almacenamientos intermedios de proxy que puedan estar ocupados.
{:shortdesc}

<dl>
<dt>Descripción</dt>
<dd>
Limita el tamaño de los almacenamientos intermedios que están enviando una respuesta al cliente mientras la respuesta aún no está totalmente leída. Mientras tanto, el resto de los almacenamientos intermedios pueden leer la respuesta y, si es necesario, almacenar en el almacenamiento intermedio parte de la respuesta en un archivo temporal. La configuración se aplica a todos los servicios del host de Ingress a menos que se especifique un servicio. Por ejemplo, si se especifica una configuración como <code>serviceName=SERVICE size=1k</code>, se aplica 1 k al servicio. Si se especifica una configuración como <code>size=1k</code>, se aplica 1 k a todos los servicios del host de Ingress.
</dd>


<dt>YAML del recurso de Ingress de ejemplo</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: proxy-ingress
 annotations:
   ingress.bluemix.net/proxy-busy-buffers-size: "serviceName=&lt;myservice&gt; size=&lt;size&gt;"
spec:
 tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
     paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>Componentes de anotación</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre de un servicio para aplicar proxy-busy-buffers-size.</td>
</tr>
<tr>
<td><code>size</code></td>
<td>Sustituya <code>&lt;<em>size</em>&gt;</code> por el tamaño de cada almacenamiento intermedio en kilobytes (k o K), como <em>1K</em>.</td>
</tr>
</tbody></table>

 </dd>
 </dl>

<br />



## Anotaciones de solicitud y respuesta
{: #request-response}


### Cabecera de respuesta o solicitud de cliente adicional (proxy-add-headers, response-add-headers)
{: #proxy-add-headers}

Añada información de cabecera adicional a una solicitud de cliente antes de enviar la solicitud a la app de fondo o a una respuesta del cliente antes de enviar la respuesta al cliente.
{:shortdesc}

<dl>
<dt>Descripción</dt>
<dd>El ALB de Ingress actúa como un proxy entre la app del cliente y la app de fondo. Las solicitudes del cliente que se envían al ALB se procesan (mediante proxy) y se colocan en una nueva solicitud que se envía a la app de fondo. Asimismo, las respuestas de la app de fondo que se envían al ALB se procesan (mediante proxy) y se colocan en una nueva respuesta que se envía al cliente. El proceso mediante proxy de una solicitud o respuesta elimina la información de cabecera HTTP, como por ejemplo el nombre de usuario, que se envió inicialmente desde el cliente o la app de fondo.

<br><br>
Si la app de fondo necesita la información de cabecera HTTP, puede utilizar la anotación <code>proxy-add-headers</code> para añadir la información de cabecera a la solicitud del cliente antes de que el ALB reenvíe la solicitud a la app de fondo.

<br>
<ul><li>Por ejemplo, puede que necesite añadir la información de cabecera X-Forward siguiente a la solicitud antes de reenviarla a la app:

<pre class="screen">
<code>proxy_set_header Host $host;
proxy_set_header X-Forwarded-Proto $scheme;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;</code></pre>

</li>

<li>Para añadir la información de cabecera X-Forward a la solicitud enviada a la app, utilice la anotación `proxy-add-headers` de la siguiente forma:

<pre class="screen">
<code>ingress.bluemix.net/proxy-add-headers: |
  serviceName=<myservice1> {
  Host $host;
  X-Forwarded-Proto $scheme;
  X-Forwarded-For $proxy_add_x_forwarded_for;
  }</code></pre>

</li></ul><br>

Si la app web del cliente necesita la información de cabecera HTTP, puede utilizar la anotación <code>response-add-headers</code> para añadir la información de cabecera a la respuesta antes de que el ALB reenvíe la respuesta a la app web del cliente.</dd>

<dt>YAML del recurso de Ingress de ejemplo</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/proxy-add-headers: |
      serviceName=&lt;myservice1&gt; {
      &lt;header1&gt; &lt;value1&gt;;
      &lt;header2&gt; &lt;value2&gt;;
      }
      serviceName=&lt;myservice2&gt; {
      &lt;header3&gt; &lt;value3&gt;;
      }
    ingress.bluemix.net/response-add-headers: |
      serviceName=&lt;myservice1&gt; {
      &lt;header1&gt;: &lt;value1&gt;;
      &lt;header2&gt;: &lt;value2&gt;;
      }
      serviceName=&lt;myservice2&gt; {
      &lt;header3&gt;: &lt;value3&gt;;
      }
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: &lt;myservice1&gt;
          servicePort: 8080
      - path: /service2_path
        backend:
          serviceName: &lt;myservice2&gt;
          servicePort: 80</code></pre>

 <table>
 <caption>Componentes de anotación</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
  </thead>
  <tbody>
  <tr>
  <td><code>service_name</code></td>
  <td>Sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre del servicio de Kubernetes que ha creado para la app.</td>
  </tr>
  <tr>
  <td><code>&lt;header&gt;</code></td>
  <td>La clave de la información de cabecera que se va a añadir a la solicitud del cliente o a la respuesta del cliente.</td>
  </tr>
  <tr>
  <td><code>&lt;value&gt;</code></td>
  <td>El valor de la información de cabecera que se va a añadir a la solicitud del cliente o a la respuesta del cliente.</td>
  </tr>
  </tbody></table>
 </dd></dl>

<br />



### Eliminación de cabecera de respuesta de cliente (response-remove-headers)
{: #response-remove-headers}

Elimine la información de cabecera que se incluye en la respuesta del cliente de la app de fondo antes de que la respuesta se envíe al cliente.
 {:shortdesc}

 <dl>
 <dt>Descripción</dt>
 <dd>El ALB de Ingress actúa como un proxy entre la app de fondo y el navegador web del cliente. Las respuestas del cliente procedentes de la app de fondo que se envían al ALB se procesan (mediante proxy) y se colocan en una nueva respuesta que se envía del ALB al navegador web del cliente. Aunque el proceso mediante proxy de una respuesta elimina la información de cabecera http que se envió inicialmente desde la app de fondo, es posible que este proceso no elimine todas las cabeceras específicas de la app de fondo. Elimine la información de cabecera de la respuesta del cliente antes de que la respuesta se reenvíe del ALB al navegador web del cliente.</dd>
 <dt>YAML del recurso de Ingress de ejemplo</dt>
 <dd>
 <pre class="codeblock">
 <code>apiVersion: extensions/v1beta1
 kind: Ingress
 metadata:
   name: myingress
   annotations:
     ingress.bluemix.net/response-remove-headers: |
       serviceName=&lt;myservice1&gt; {
       "&lt;header1&gt;";
       "&lt;header2&gt;";
       }
       serviceName=&lt;myservice2&gt; {
       "&lt;header3&gt;";
       }
 spec:
   tls:
   - hosts:
     - mydomain
    secretName: mytlssecret
  rules:
   - host: mydomain
    http:
      paths:
       - path: /service1_path
         backend:
           serviceName: &lt;myservice1&gt;
           servicePort: 8080
       - path: /service2_path
         backend:
           serviceName: &lt;myservice2&gt;
           servicePort: 80</code></pre>

  <table>
  <caption>Componentes de anotación</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
   </thead>
   <tbody>
   <tr>
   <td><code>service_name</code></td>
   <td>Sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre del servicio de Kubernetes que ha creado para la app.</td>
   </tr>
   <tr>
   <td><code>&lt;header&gt;</code></td>
   <td>La clave de la cabecera que se eliminará de la respuesta del cliente.</td>
   </tr>
   </tbody></table>
   </dd></dl>

<br />


### Tamaño de cuerpo de solicitud del cliente (client-max-body-size)
{: #client-max-body-size}

Defina el tamaño máximo del cuerpo que puede enviar el cliente como parte de una solicitud.
{:shortdesc}

<dl>
<dt>Descripción</dt>
<dd>Para mantener el rendimiento esperado, el tamaño máximo del cuerpo de la solicitud del cliente se establece en 1 megabyte. Cuando se envía una solicitud del cliente con un tamaño de cuerpo que supera el límite al ALB de Ingress y el cliente no permite dividir los datos, el ALB devuelve al cliente una respuesta HTTP 413 (la entidad de la solicitud es demasiado grande). No se puede establecer una conexión entre el cliente y el ALB hasta que se reduce el tamaño del cuerpo de la solicitud. Si el cliente puede dividir los datos en varios bloques, los datos se dividen en paquetes de 1 megabyte y se envían al ALB.

</br></br>
Es posible que desee aumentar el tamaño máximo del cuerpo porque espera solicitudes del cliente con un tamaño de cuerpo superior a 1 megabyte. Por ejemplo, si desea que el cliente pueda cargar archivos grandes. El hecho de aumentar el tamaño máximo del cuerpo de la solicitud puede afectar el rendimiento del ALB, ya que la conexión con el cliente debe permanecer abierta hasta que se recibe la solicitud.
</br></br>
<strong>Nota:</strong> Algunos navegadores web de cliente no pueden mostrar correctamente el mensaje de respuesta de HTTP 413.</dd>
<dt>YAML del recurso de Ingress de ejemplo</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/client-max-body-size: "size=&lt;size&gt;"
spec:
 tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
     paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>Componentes de anotación</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;size&gt;</code></td>
 <td>El tamaño máximo del cuerpo de la respuesta del cliente. Por ejemplo, para establecer el tamaño máximo en 200 megabytes, defina <code>200m</code>.  <strong>Nota:</strong> Puede establecer el tamaño en 0 para inhabilitar la comprobación del tamaño del cuerpo de la solicitud del cliente.</td>
 </tr>
 </tbody></table>

 </dd></dl>

<br />


### Almacenamientos intermedios de cabeceras de cliente largas (large-client-header-buffers)
{: #large-client-header-buffers}

Establezca el tamaño y el número máximo de los almacenamientos intermedios que leen cabeceras de solicitud de cliente largas.
{:shortdesc}

<dl>
<dt>Descripción</dt>
<dd>Los almacenamientos intermedios que leen cabeceras largas de solicitud de cliente sólo se asignan bajo demanda: si una conexión se cambia al estado activo al final del proceso de la solicitud, estos almacenamientos intermedios se liberan. De forma predeterminada, el tamaño del almacenamiento intermedio es igual a <code>8K</code> bytes. Si una línea de la solicitud supera el tamaño máximo de un almacenamiento intermedio, el cliente recibe el error <code>414 Request-URI Too Large</code>. Además, si un campo de la cabecera de la solicitud supera el tamaño máximo de un almacenamiento intermedio, el cliente recibe el error <code>400 Bad Request</code>. Puede ajustar el tamaño y el número máximo de los almacenamientos intermedios que se utilizan para leer cabeceras de solicitud de cliente largas.

<dt>YAML del recurso de Ingress de ejemplo</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/large-client-header-buffers: "number=&lt;number&gt; size=&lt;size&gt;"
spec:
 tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
     paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>Componentes de anotación</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;number&gt;</code></td>
 <td>El número máximo de almacenamientos intermedios que deben asignarse para leer cabeceras largas de solicitud del cliente. Por ejemplo, para establecerlo en 4, defina <code>4</code>.</td>
 </tr>
 <tr>
 <td><code>&lt;size&gt;</code></td>
 <td>El tamaño máximo de los almacenamientos intermedios que leen cabeceras largas de solicitud del cliente. Por ejemplo, para establecerlo en 16 kilobytes, defina <code>16k</code>.
   <strong>Nota:</strong> El tamaño debe terminar en <code>k</code> para kilobytes o <code>m</code> para megabytes.</td>
 </tr>
</tbody></table>
</dd>
</dl>

<br />


## Anotaciones de límite de servicio
{: #service-limit}


### Límites de velocidad global (global-rate-limit)
{: #global-rate-limit}

Limite la velocidad de procesamiento de solicitudes y el número de conexiones por una clave definida para todos los servicios.
{:shortdesc}

<dl>
<dt>Descripción</dt>
<dd>
Para todos los servicios, limite la velocidad de servicio de solicitudes y el número de conexiones por una clave definida que provienen de una única dirección IP para todas las vías de acceso de los backends seleccionados.
</dd>


 <dt>YAML del recurso de Ingress de ejemplo</dt>
 <dd>

 <pre class="codeblock">
 <code>apiVersion: extensions/v1beta1
 kind: Ingress
 metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/global-rate-limit: "key=&lt;key&gt; rate=&lt;rate&gt; conn=&lt;number_of_connections&gt;"
 spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

 <table>
 <caption>Componentes de anotación</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
  </thead>
  <tbody>
  <tr>
  <td><code>key</code></td>
  <td>Para establecer un límite global para las solicitudes entrantes basado en la ubicación o servicio, utilice `key=location`. Para establecer un límite global para las solicitudes entrantes basado en la cabecera, utilice `X-USER-ID key=$http_x_user_id`.</td>
  </tr>
  <tr>
  <td><code>rate</code></td>
  <td>Sustituya <code>&lt;<em>rate</em>&gt;</code> por la velocidad de procesamiento. Especifique un valor como velocidad por segundo (r/s) o velocidad por minuto (r/m). Por ejemplo: <code>50r/m</code>.</td>
  </tr>
  <tr>
  <td><code>number-of_connections</code></td>
  <td>Sustituya <code>&lt;<em>conn</em>&gt;</code> por el número de conexiones.</td>
  </tr>
  </tbody></table>

  </dd>
  </dl>

<br />



### Límites de velocidad de servicio (service-rate-limit)
{: #service-rate-limit}

Limita la velocidad de procesamiento de solicitudes y el número de conexiones para servicios específicos.
{:shortdesc}

<dl>
<dt>Descripción</dt>
<dd>Para servicios específicos, limite la velocidad de servicio de solicitudes y el número de conexiones por una clave definida que provienen de una única dirección IP para todas las vías de acceso de los backends seleccionados.
</dd>


 <dt>YAML del recurso de Ingress de ejemplo</dt>
 <dd>

 <pre class="codeblock">
 <code>apiVersion: extensions/v1beta1
 kind: Ingress
 metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/service-rate-limit: "serviceName=&lt;myservice&gt; key=&lt;key&gt; rate=&lt;rate&gt; conn=&lt;number_of_connections&gt;"
 spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

 <table>
 <caption>Componentes de anotación</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Componentes de anotación</th>
  </thead>
  <tbody>
  <tr>
  <td><code>serviceName</code></td>
  <td>Sustituya <code>&lt;<em>myservice</em>&gt;</code> por el nombre del servicio para el que desea limitar la velocidad de procesamiento.</li>
  </tr>
  <tr>
  <td><code>key</code></td>
  <td>Para establecer un límite global para las solicitudes entrantes basado en la ubicación o servicio, utilice `key=location`. Para establecer un límite global para las solicitudes entrantes basado en la cabecera, utilice `X-USER-ID key=$http_x_user_id`.</td>
  </tr>
  <tr>
  <td><code>rate</code></td>
  <td>Sustituya <code>&lt;<em>rate</em>&gt;</code> por la velocidad de procesamiento. Para definir una velocidad por segundo, utilice r/s: <code>10r/s</code>. Para definir una velocidad por minuto, utilice r/m: <code>50r/m</code>.</td>
  </tr>
  <tr>
  <td><code>number-of_connections</code></td>
  <td>Sustituya <code>&lt;<em>conn</em>&gt;</code> por el número de conexiones.</td>
  </tr>
  </tbody></table>
  </dd>
  </dl>

  <br />



