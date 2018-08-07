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



# Resolución de problemas de supervisión y creación de registros
{: #cs_troubleshoot_health}

Si utiliza {{site.data.keyword.containerlong}}, tenga en cuenta estas técnicas para resolver problemas relacionados con la supervisión y la creación de registros.
{: shortdesc}

Si tiene un problema más general, pruebe la [depuración del clúster](cs_troubleshoot.html).
{: tip}

## Los registros no aparecen
{: #cs_no_logs}

{: tsSymptoms}
Al acceder al panel de control de Kibana, sus registros no se visualizan.

{: tsResolve}
Revise los siguientes motivos por los que no aparecen sus registros del clúster y los pasos de resolución de problemas correspondientes:

<table>
<caption>Resolución de problemas de registros que no se muestran</caption>
  <col width="40%">
  <col width="60%">
  <thead>
    <tr>
      <th>Por qué está ocurriendo</th>
      <th>Cómo solucionarlo</th>
    </tr>
 </thead>
 <tbody>
  <tr>
    <td>No hay ninguna configuración de registro definida.</td>
    <td>Para que se envíen los registros, debe crear una configuración de registro. Para ello, consulte <a href="cs_health.html#logging">Configuración del registro de clúster</a>.</td>
  </tr>
  <tr>
    <td>El clúster no se encuentra en estado <code>Normal</code>.</td>
    <td>Para comprobar el estado del clúster, consulte <a href="cs_troubleshoot.html#debug_clusters">Depuración de clústeres</a>.</td>
  </tr>
  <tr>
    <td>Se ha alcanzado el límite de almacenamiento de registro.</td>
    <td>Para aumentar los limites del almacenamiento de registros, consulte la documentación de <a href="/docs/services/CloudLogAnalysis/troubleshooting/error_msgs.html">{{site.data.keyword.loganalysislong_notm}}</a>.</td>
  </tr>
  <tr>
    <td>Si ha especificado un espacio durante la creación del clúster, el propietario de la cuenta no tiene permisos de gestor, desarrollador o auditor para el espacio en cuestión.</td>
      <td>Para cambiar los permisos de acceso para el propietario de la cuenta:
      <ol><li>Para descubrir quién es el propietario de la cuenta del clúster, ejecute <code>bx cs api-key-info &lt;cluster_name_or_ID&gt;</code>.</li>
      <li>Para otorgar a dicho propietario de cuenta permisos de acceso de {{site.data.keyword.containershort_notm}} de Gestor, Desarrollador o Auditor al espacio, consulte <a href="cs_users.html">Gestión de acceso a clústeres</a>.</li>
      <li>Para renovar la señal de registro tras cambiar los permisos, ejecute <code>bx cs logging-config-refresh &lt;cluster_name_or_ID&gt;</code>.</li></ol></td>
    </tr>
    <tr>
      <td>Tiene una configuración de creación de registro de aplicación con un enlace simbólico en la vía de acceso de la app.</td>
      <td><p>Para que se puedan enviar los registros, se debe utilizar una vía de acceso absoluta en la configuración de registro o de lo contrario, no se podrán leer los registros. Si la vía de acceso está montada en su nodo trabajador, podría haber creado un enlace simbólico.</p> <p>Ejemplo: Si la vía de acceso especificada es <code>/usr/local/<b>spark</b>/work/app-0546/0/stderr</code> pero los registros van a <code>/usr/local/<b>spark-1.0-hadoop-1.2</b>/work/app-0546/0/stderr</code>, entonces no se podrán leer los registros.</p></td>
    </tr>
  </tbody>
</table>

Para probar los cambios que ha realizado durante la resolución de problemas, puede desplegar *Noisy*, un pod de ejemplo que produce varios sucesos de registro, en un nodo trabajador en el clúster.

  1. [Defina como objetivo de la CLI](cs_cli_install.html#cs_cli_configure) un clúster donde desee iniciar los registros de producción.

  2. Cree el archivo de configuración `deploy-noisy.yaml`.

      ```
      apiVersion: v1
      kind: Pod
      metadata:
        name: noisy
      spec:
        containers:
        - name: noisy
          image: ubuntu:16.04
          command: ["/bin/sh"]
          args: ["-c", "while true; do sleep 10; echo 'Hello world!'; done"]
          imagePullPolicy: "Always"
        ```
        {: codeblock}

  3. Ejecute el archivo de configuración en el contexto del clúster.

        ```
        kubectl apply -f noisy.yaml
        ```
        {:pre}

  4. Después de unos minutos, verá los registros en el panel. Para acceder al panel de control de Kibana, vaya a uno de los siguientes URL y seleccione la cuenta de {{site.data.keyword.Bluemix_notm}} en la que ha creado el clúster. Si ha especificado un espacio durante la creación del clúster, vaya al espacio.
      - EE.UU. Sur y EE.UU. Este: https://logging.ng.bluemix.net
      - UK Sur: https://logging.eu-gb.bluemix.net
      - UE Central: https://logging.eu-fra.bluemix.net
      - AP Sur: https://logging.au-syd.bluemix.net

<br />


## El panel de control de Kubernetes no muestra los gráficos de utilización
{: #cs_dashboard_graphs}

{: tsSymptoms}
Al acceder al panel de control de Kubernetes, los gráficos de utilización no se visualizan.

{: tsCauses}
En ocasiones después de actualizar un clúster o reiniciar un nodo trabajador, el pod `kube-dashboard` no se actualiza.

{: tsResolve}
Suprima el pod `kube-dashboard` para forzar un reinicio. El pod se volverá a crear con políticas RBAC para acceder a Heapster para obtener información de utilización.

  ```
  kubectl delete pod -n kube-system $(kubectl get pod -n kube-system --selector=k8s-app=kubernetes-dashboard -o jsonpath='{.items..metadata.name}')
  ```
  {: pre}

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

