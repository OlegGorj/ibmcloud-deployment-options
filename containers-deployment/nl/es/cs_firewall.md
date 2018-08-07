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




# Apertura de los puertos y direcciones IP necesarios en el cortafuegos
{: #firewall}

Revise estas situaciones en las que puede tener que abrir puertos específicos y direcciones IP en el cortafuegos para {{site.data.keyword.containerlong}}.
{:shortdesc}

* [Para ejecutar mandatos `bx` ](#firewall_bx)desde el sistema local cuando las políticas de red corporativas impiden el acceso a los puntos finales de internet pública mediante proxies o cortafuegos.
* [Para ejecutar mandatos `kubectl` ](#firewall_kubectl)desde el sistema local cuando las políticas de red corporativas impiden el acceso a los puntos finales de internet pública mediante proxies o cortafuegos.
* [Para ejecutar mandatos `calicoctl` ](#firewall_calicoctl)desde el sistema local cuando las políticas de red corporativas impiden el acceso a los puntos finales de internet pública mediante proxies o cortafuegos.
* [Para permitir la comunicación entre el maestro de Kubernetes y los nodos trabajadores](#firewall_outbound) cuando hay un cortafuegos configurado para los nodos trabajadores o los valores del cortafuegos están personalizados en su cuenta de infraestructura de IBM Cloud (SoftLayer).
* [Para acceder al servicio NodePort, LoadBalancer o Ingress desde fuera del clúster](#firewall_inbound).

<br />


## Ejecución de mandatos `bx cs` desde detrás de un cortafuegos
{: #firewall_bx}

Si las políticas de red corporativas impiden el acceso desde el sistema local a los puntos finales públicos mediante proxies o cortafuegos, para ejecutar mandatos `bx cs` debe permitir el acceso TCP para {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

1. Permita el acceso a `containers.bluemix.net` en el puerto 443.
2. Compruebe la conexión. Si el acceso está configurado correctamente, los envíos se visualizan en la salida.
   ```
   curl https://containers.bluemix.net/v1/
   ```
   {: pre}

   Salida de ejemplo:
   ```
                                     )___(
                              _______/__/_
                     ___     /===========|   ___
    ____       __   [\\\]___/____________|__[///]   __
    \   \_____[\\]__/___________________________\__[//]___
     \                                                    |
      \                                                  /
   ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ```
   {: screen}

<br />


## Ejecución de mandatos `kubectl` desde detrás de un cortafuegos
{: #firewall_kubectl}

Si las políticas de red corporativas impiden el acceso desde el sistema local a los puntos finales públicos mediante proxies o cortafuegos, para ejecutar mandatos `kubectl` debe permitir el acceso TCP para el clúster.
{:shortdesc}

Cuando se crea un clúster, el puerto del URL maestro se asigna aleatoriamente entre 20000 y 32767. Puede elegir abrir el rango de puerto 20000-32767 para cualquier clúster que se pueda crear o permitir el acceso a un clúster existente específico.

Antes de empezar, permita el acceso para [ejecutar mandatos `bx cs`](#firewall_bx).

Para permitir el acceso a un clúster específico:

1. Inicie la sesión en la CLI de {{site.data.keyword.Bluemix_notm}}. Escriba
sus credenciales de {{site.data.keyword.Bluemix_notm}} cuando se le solicite. Si tiene una cuenta federada, incluya la opción `--sso`.

   ```
   bx login [--sso]
   ```
   {: pre}

2. Seleccione la región en la que está el clúster.

   ```
   bx cs region-set
   ```
   {: pre}

3. Obtenga el nombre del clúster.

   ```
   bx cs clusters
   ```
   {: pre}

4. Recupere el **URL maestro** del clúster.

   ```
   bx cs cluster-get <cluster_name_or_ID>
   ```
   {: pre}

   Salida de ejemplo:
   ```
   ...
   Master URL:		https://169.xx.xxx.xxx:31142
   ...
   ```
   {: screen}

5. Permitir el acceso al **URL maestro** en el puerto, como en el puerto `31142` en el ejemplo anterior.

6. Compruebe la conexión.

   ```
   curl --insecure <master_URL>/version
   ```
   {: pre}

   Mandato de ejemplo:
   ```
   curl --insecure https://169.xx.xxx.xxx:31142/version
   ```
   {: pre}

   Salida de ejemplo:
   ```
   {
     "major": "1",
     "minor": "7+",
     "gitVersion": "v1.7.4-2+eb9172c211dc41",
     "gitCommit": "eb9172c211dc4108341c0fd5340ee5200f0ec534",
     "gitTreeState": "clean",
     "buildDate": "2017-11-16T08:13:08Z",
     "goVersion": "go1.8.3",
     "compiler": "gc",
     "platform": "linux/amd64"
   }
   ```
   {: screen}

7. Opcional: repita estos pasos para cada clúster que necesite exponer.

<br />


## Ejecución de mandatos `calicoctl` desde detrás de un cortafuegos
{: #firewall_calicoctl}

Si las políticas de red corporativas impiden el acceso desde el sistema local a los puntos finales públicos mediante proxies o cortafuegos, para ejecutar mandatos `calicoctl` debe permitir el acceso TCP para los mandatos Calico.
{:shortdesc}

Antes de empezar, permita el acceso para ejecutar mandatos [`bx`](#firewall_bx) y mandatos [`kubectl`](#firewall_kubectl).

1. Recupere la dirección IP del URL maestro que ha utilizado para permitir los mandatos [`kubectl`](#firewall_kubectl).

2. Obtenga el puerto para ETCD.

  ```
  kubectl get cm -n kube-system calico-config -o yaml | grep etcd_endpoints
  ```
  {: pre}

3. Permita el acceso para las políticas de Calico mediante la dirección IP del URL maestro y el puerto ETCD.

<br />


## Cómo permitir al clúster acceder a recursos de infraestructura y otros servicios
{: #firewall_outbound}

Permita que el clúster acceda a servicios y recursos de infraestructura desde detrás de un cortafuegos, como para las regiones de {{site.data.keyword.containershort_notm}}, {{site.data.keyword.registrylong_notm}}, {{site.data.keyword.monitoringlong_notm}}, {{site.data.keyword.loganalysislong_notm}}, IP privadas de infraestructura de IBM Cloud (SoftLayer) y de salida para reclamaciones de volumen permanente.
{:shortdesc}

  1.  Anote la dirección IP pública de todos sus nodos trabajadores del clúster.

      ```
      bx cs workers <cluster_name_or_ID>
      ```
      {: pre}

  2.  Permita el tráfico de red de salida del origen _<each_worker_node_publicIP>_ al rango de puertos TCP/UDP de destino 20000-32767 y al puerto 443, y a las siguientes direcciones IP y grupos de red. Si tiene un cortafuegos corporativo que impide a la máquina local acceder a puntos finales de Internet pública, realice este paso para los nodos trabajadores de origen y para la máquina local.
      - **Importante**: También debe permitir el tráfico de salida al puerto 443 y a todas las ubicaciones entre una región y otra para equilibrar la carga durante el proceso de arranque. Por ejemplo, si el clúster está en EE.UU. sur, debe permitir el tráfico procedente desde el puerto 443 a las direcciones IP para todas las ubicaciones (dal10, dal12 y dal13).
      <p>
  <table summary="La primera fila de la tabla abarca ambas columnas. El resto de las filas se deben leer de izquierda a derecha; la ubicación del servidor está en la columna uno y las direcciones IP correspondientes en la columna dos. ">
  <caption>Direcciones IP para abrir para el tráfico saliente</caption>
      <thead>
      <th>Región</th>
      <th>Ubicación</th>
      <th>Dirección IP</th>
      </thead>
    <tbody>
      <tr>
        <td>AP Norte</td>
        <td>hkg02<br>seo01<br>sng01<br>tok02</td>
        <td><code>169.56.132.234</code><br><code>169.56.69.242</code><br><code>161.202.186.226</code><br><code>161.202.126.210</code></td>
       </tr>
      <tr>
         <td>AP Sur</td>
         <td>mel01<br>syd01<br>syd04</td>
         <td><code>168.1.97.67</code><br><code>168.1.8.195</code><br><code>130.198.64.19, 130.198.66.34</code></td>
      </tr>
      <tr>
         <td>UE Central</td>
         <td>ams03<br>fra02<br>mil01<br>par01</td>
         <td><code>169.50.169.110, 169.50.154.194</code><br><code>169.50.56.174</code><br><code>159.122.190.98</code><br><code>159.8.86.149, 159.8.98.170</code></td>
        </tr>
      <tr>
        <td>UK Sur</td>
        <td>lon02<br>lon04</td>
        <td><code>159.122.242.78</code><br><code>158.175.65.170, 158.175.74.170, 158.175.76.2</code></td>
      </tr>
      <tr>
        <td>EE.UU. este</td>
         <td>mon01<br>tor01<br>wdc06<br>wdc07</td>
         <td><code>169.54.126.219</code><br><code>169.53.167.50</code><br><code>169.60.73.142</code><br><code>169.61.83.62</code></td>
      </tr>
      <tr>
        <td>EE.UU. Sur</td>
        <td>dal10<br>dal12<br>dal13<br>hou02<br>sao01</td>
        <td><code>169.47.234.18, 169.46.7.238</code><br><code>169.47.70.10</code><br><code>169.60.128.2</code><br><code>184.173.44.62</code><br><code>169.57.151.10</code></td>
      </tr>
      </tbody>
    </table>
</p>

  3.  Permita el tráfico de red de salida de los nodos trabajadores a regiones de [{{site.data.keyword.registrylong_notm}}](/docs/services/Registry/registry_overview.html#registry_regions):
      - `TCP port 443 FROM <each_worker_node_publicIP> TO <registry_publicIP>`
      - Sustituya <em>&lt;registry_publicIP&gt;</em> por las direcciones IP de registro a las que desea permitir el tráfico. El registro global almacena imágenes públicas proporcionadas por IBM y los registros regionales almacenan sus propias imágenes privadas o públicas.
        <p>
<table summary="La primera fila de la tabla abarca ambas columnas. El resto de las filas se deben leer de izquierda a derecha; la ubicación del servidor está en la columna uno y las direcciones IP correspondientes en la columna dos. ">
  <caption>Direcciones IP para abrir para el tráfico de registro</caption>
      <thead>
        <th>Región de {{site.data.keyword.containershort_notm}}</th>
        <th>Dirección de registro</th>
        <th>Dirección IP de registro</th>
      </thead>
      <tbody>
        <tr>
          <td>Registro global a través de regiones de {{site.data.keyword.containershort_notm}}</td>
          <td>registry.bluemix.net</td>
          <td><code>169.60.72.144/28</code><br><code>169.61.76.176/28</code></td>
        </tr>
        <tr>
          <td>AP Norte, AP Sur</td>
          <td>registry.au-syd.bluemix.net</td>
          <td><code>168.1.45.160/27</code></br><code>168.1.139.32/27</code></td>
        </tr>
        <tr>
          <td>UE Central</td>
          <td>registry.eu-de.bluemix.net</td>
          <td><code>169.50.56.144/28</code></br><code>159.8.73.80/28</code></td>
         </tr>
         <tr>
          <td>UK Sur</td>
          <td>registry.eu-gb.bluemix.net</td>
          <td><code>159.8.188.160/27</code></br><code>169.50.153.64/27</code></td>
         </tr>
         <tr>
          <td>EE.UU. Este, EE.UU. Sur</td>
          <td>registry.ng.bluemix.net</td>
          <td><code>169.55.39.112/28</code></br><code>169.46.9.0/27</code></br><code>169.55.211.0/27</code></td>
         </tr>
        </tbody>
      </table>
</p>

  4.  Opcional: Permita el tráfico de red de salida de los nodos trabajadores a {{site.data.keyword.monitoringlong_notm}} y a los servicios {{site.data.keyword.loganalysislong_notm}}:
      - `TCP port 443, port 9095 FROM <each_worker_node_public_IP> TO <monitoring_public_IP>`
      - Sustituya <em>&lt;monitoring_public_IP&gt;</em> por todas las direcciones de las regiones de supervisión a las que desea permitir el tráfico:
        <p><table summary="La primera fila de la tabla abarca ambas columnas. El resto de las filas se deben leer de izquierda a derecha; la ubicación del servidor está en la columna uno y las direcciones IP correspondientes en la columna dos. ">
  <caption>Direcciones IP para abrir para el tráfico de supervisión</caption>
        <thead>
        <th>Región de {{site.data.keyword.containershort_notm}}</th>
        <th>Dirección de supervisión</th>
        <th>Direcciones IP de supervisión</th>
        </thead>
      <tbody>
        <tr>
         <td>UE Central</td>
         <td>metrics.eu-de.bluemix.net</td>
         <td><code>159.122.78.136/29</code></td>
        </tr>
        <tr>
         <td>UK Sur</td>
         <td>metrics.eu-gb.bluemix.net</td>
         <td><code>169.50.196.136/29</code></td>
        </tr>
        <tr>
          <td>EE.UU. Este, EE.UU. Sur, AP Norte</td>
          <td>metrics.ng.bluemix.net</td>
          <td><code>169.47.204.128/29</code></td>
         </tr>
         
        </tbody>
      </table>
</p>
      - `TCP port 443, port 9091 FROM <each_worker_node_public_IP> TO <logging_public_IP>`
      - Sustituya <em>&lt;logging_public_IP&gt;</em> por todas las direcciones de las regiones de registro a las que desea permitir el tráfico:
        <p><table summary="La primera fila de la tabla abarca ambas columnas. El resto de las filas se deben leer de izquierda a derecha; la ubicación del servidor está en la columna uno y las direcciones IP correspondientes en la columna dos. ">
<caption>Direcciones IP para abrir para el tráfico de creación de registros</caption>
        <thead>
        <th>Región de {{site.data.keyword.containershort_notm}}</th>
        <th>Dirección de registro</th>
        <th>Direcciones IP de registro</th>
        </thead>
        <tbody>
          <tr>
            <td>EE.UU. Este, EE.UU. Sur</td>
            <td>ingest.logging.ng.bluemix.net</td>
            <td><code>169.48.79.236</code><br><code>169.46.186.113</code></td>
           </tr>
          <tr>
           <td>UK Sur</td>
           <td>ingest.logging.eu-gb.bluemix.net</td>
           <td><code>169.50.115.113</code></td>
          </tr>
          <tr>
           <td>UE Central</td>
           <td>ingest-eu-fra.logging.bluemix.net</td>
           <td><code>158.177.88.43</code><br><code>159.122.87.107</code></td>
          </tr>
          <tr>
           <td>AP Sur, AP Norte</td>
           <td>ingest-au-syd.logging.bluemix.net</td>
           <td><code>130.198.76.125</code><br><code>168.1.209.20</code></td>
          </tr>
         </tbody>
       </table>
</p>

  5. Para cortafuegos privados, permita los rangos de direcciones IP privadas adecuadas de infraestructura de IBM Cloud (SoftLayer). Consulte [este enlace](https://knowledgelayer.softlayer.com/faq/what-ip-ranges-do-i-allow-through-firewall) a partir de la sección **Red de fondo (privada)**.
      - Añada todas las [ubicaciones dentro de las regiones](cs_regions.html#locations) que está utilizando.
      - Tenga en cuenta que debe añadir la ubicación dal01 (centro de datos).
      - Abra los puertos 80 y 443 para permitir el proceso de arranque del clúster.

  6. {: #pvc}Para crear reclamaciones de volumen permanente para el almacenamiento de datos, permita el acceso de salida mediante el cortafuegos para las [direcciones IP de infraestructura de IBM Cloud (SoftLayer) ](https://knowledgelayer.softlayer.com/faq/what-ip-ranges-do-i-allow-through-firewall) de la ubicación (centro de datos) en la que está el clúster.
      - Para encontrar la ubicación (del centro de datos) del clúster, ejecute `bx cs clusters`.
      - Permita el acceso al rango de IP para la **red (pública) frontal** y la **red (privada) de fondo**.
      - Tenga en cuenta que debe añadir la ubicación dal01 (centro de datos) para la **red (privada) de fondo**.

<br />


## Acceso a servicios NodePort, de equilibrador de carga e Ingress desde fuera del clúster
{: #firewall_inbound}

Puede permitir el acceso a servicios NodePort, de equilibrador de carga e Ingress.
{:shortdesc}

<dl>
  <dt>Servicio NodePort</dt>
  <dd>Abra el puerto que ha configurado cuando ha desplegado el servicio a las direcciones IP públicas para permitir el tráfico a todos los nodos trabajadores. Para encontrar el puerto, ejecute `kubectl get svc`. El puerto está en el rango 20000-32000.<dd>
  <dt>Servicio LoadBalancer</dt>
  <dd>Abra el puerto que ha configurado cuando ha desplegado el servicio a la dirección IP pública del servicio de equilibrador de carga.</dd>
  <dt>Ingress</dt>
  <dd>Abra el puerto 80 para HTTP o el puerto 443 para HTTPS a la dirección IP del equilibrador de carga de aplicación de Ingress.</dd>
</dl>
