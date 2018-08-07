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





# CLI-Referenz des {{site.data.keyword.containerlong_notm}}
{: #cs_cli_reference}

Verwenden Sie diese Befehle, um Kubernetes-Cluster im {{site.data.keyword.containerlong}} zu erstellen und zu verwalten.
{:shortdesc}

Informationen zum Installieren des CLI-Plug-ins finden Sie im Abschnitt [CLI installieren](cs_cli_install.html#cs_cli_install_steps).

Suchen Sie nach `bx cr`-Befehlen? Werfen Sie einen Blick in die [{{site.data.keyword.registryshort_notm}}-CLI-Referenz ](/docs/cli/plugins/registry/index.html). Suchen Sie nach `kubectl`-Befehlen? Werfen Sie einen Blick in die [Kubernetes-Dokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/reference/kubectl/overview/).
{:tip}

## 'bx cs'-Befehle
{: #cs_commands}

**Tipp:** Sie können die Version des {{site.data.keyword.containershort_notm}}-Plug-ins abrufen, indem Sie den folgenden Befehl ausführen.

```
bx plugin list
```
{: pre}



<table summary="Tabelle mit API-Befehlen">
<caption>API-Befehle</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>API-Befehle</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs api](#cs_api)</td>
    <td>[bx cs api-key-info](#cs_api_key_info)</td>
    <td>[bx cs api-key-reset](#cs_api_key_reset)</td>
    <td>[bx cs apiserver-config-get](#cs_apiserver_config_get)</td>
  </tr>
  <tr>
    <td>[bx cs apiserver-config-set](#cs_apiserver_config_set)</td>
    <td>[bx cs apiserver-config-unset](#cs_apiserver_config_unset)</td>
    <td>[bx cs apiserver-refresh](#cs_apiserver_refresh)</td>
    <td></td>
 </tr>
</tbody>
</table>

<br>

<table summary="Tabelle mit Befehlen für die Verwendung des CLI-Plug-ins">
<caption>Befehle für die Verwendung des CLI-Plug-ins</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Befehle für die Verwendung des CLI-Plug-ins</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs help](#cs_help)</td>
    <td>[bx cs init](#cs_init)</td>
    <td>[bx cs messages](#cs_messages)</td>
    <td></td>
  </tr>
</tbody>
</table>

<br>

<table summary="Clusterbefehle: Tabelle mit Managementbefehlen">
<caption>Clusterbefehle: Managementbefehle</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Clusterbefehle: Management</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs cluster-config](#cs_cluster_config)</td>
    <td>[bx cs cluster-create](#cs_cluster_create)</td>
    <td>[bx cs cluster-feature-enable](#cs_cluster_feature_enable)</td>
    <td>[bx cs cluster-get](#cs_cluster_get)</td>
  </tr>
  <tr>
    <td>[bx cs cluster-rm](#cs_cluster_rm)</td>
    <td>[bx cs cluster-update](#cs_cluster_update)</td>
    <td>[bx cs clusters](#cs_clusters)</td>
    <td>[bx cs kube-versions](#cs_kube_versions)</td>
  </tr>
</tbody>
</table>

<br>

<table summary="Clusterbefehle: Tabelle mit Befehlen für Services und Integrationen">
<caption>Clusterbefehle: Befehle für Services und Integrationen</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Clusterbefehle: Services und Integrationen</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs cluster-service-bind](#cs_cluster_service_bind)</td>
    <td>[bx cs cluster-service-unbind](#cs_cluster_service_unbind)</td>
    <td>[bx cs cluster-services](#cs_cluster_services)</td>
    <td>[bx cs webhook-create](#cs_webhook_create)</td>
  </tr>
</tbody>
</table>

</br>

<table summary="Clusterbefehle: Tabelle mit Befehlen für Teilnetze">
<caption>Clusterbefehle: Befehle für Teilnetze</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Clusterbefehle: Teilnetze</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs cluster-subnet-add](#cs_cluster_subnet_add)</td>
    <td>[bx cs cluster-subnet-create](#cs_cluster_subnet_create)</td>
    <td>[bx cs cluster-user-subnet-add](#cs_cluster_user_subnet_add)</td>
    <td>[bx cs cluster-user-subnet-rm](#cs_cluster_user_subnet_rm)</td>
  </tr>
  <tr>
    <td>[bx cs subnets](#cs_subnets)</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
</tbody>
</table>

</br>

<table summary="Tabelle mit Befehlen für die Infrastruktur">
<caption>Clusterbefehle: Befehle für die Infrastruktur</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Befehle für die Infrastruktur</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs credentials-set](#cs_credentials_set)</td>
    <td>[bx cs credentials-unset](#cs_credentials_unset)</td>
    <td>[bx cs machine-types](#cs_machine_types)</td>
    <td>[bx cs vlans](#cs_vlans)</td>
  </tr>
</tbody>
</table>

</br>

<table summary="Tabelle mit Ingress-Befehlen für die Lastausgleichsfunktion für Anwendungen (ALB)">
<caption>Ingress-Befehle für die Lastausgleichsfunktion für Anwendungen (ALB)</caption>
<col width = 25%>
<col width = 25%>
<col width = 25%>
  <thead>
    <tr>
      <th colspan=4>Ingress-Befehle für die Lastausgleichsfunktion für Anwendungen (ALB)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>[bx cs alb-cert-deploy](#cs_alb_cert_deploy)</td>
      <td>[bx cs alb-cert-get](#cs_alb_cert_get)</td>
      <td>[bx cs alb-cert-rm](#cs_alb_cert_rm)</td>
      <td>[bx cs alb-certs](#cs_alb_certs)</td>
    </tr>
    <tr>
      <td>[bx cs alb-configure](#cs_alb_configure)</td>
      <td>[bx cs alb-get](#cs_alb_get)</td>
      <td>[bx cs alb-types](#cs_alb_types)</td>
      <td>[bx cs albs](#cs_albs)</td>
    </tr>
  </tbody>
</table>

</br>

<table summary="Tabelle mit Protokollierungsbefehlen">
<caption>Protokollierungsbefehle</caption>
<col width = 25%>
<col width = 25%>
<col width = 25%>
  <thead>
    <tr>
      <th colspan=4>Protokollierungsbefehle</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>[bx cs logging-config-create](#cs_logging_create)</td>
      <td>[bx cs logging-config-get](#cs_logging_get)</td>
      <td>[bx cs logging-config-refresh](#cs_logging_refresh)</td>
      <td>[bx cs logging-config-rm](#cs_logging_rm)</td>
    </tr>
    <tr>
      <td>[bx cs logging-config-update](#cs_logging_update)</td>
      <td>[bx cs logging-filter-create](#cs_log_filter_create)</td>
      <td>[bx cs logging-filter-update](#cs_log_filter_update)</td>
      <td>[bx cs logging-filter-get](#cs_log_filter_view)</td>
    </tr>
    <tr>
      <td>[bx cs logging-filter-rm](#cs_log_filter_delete)</td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>

</br>

<table summary="Tabelle mit Regionsbefehlen">
<caption>Regionsbefehle</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Regionsbefehle</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs locations](#cs_datacenters)</td>
    <td>[bx cs region](#cs_region)</td>
    <td>[bx cs region-set](#cs_region-set)</td>
    <td>[bx cs regions](#cs_regions)</td>
  </tr>
</tbody>
</table>

</br>

<table summary="Tabelle mit Befehlen für Workerknoten">
<caption>Befehle für Workerknoten</caption>
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Befehle für Workerknoten</th>
 </thead>
 <tbody>
    <tr>
      <td>[bx cs worker-add](#cs_worker_add)</td>
      <td>[bx cs worker-get](#cs_worker_get)</td>
      <td>[bx cs worker-reboot](#cs_worker_reboot)</td>
      <td>[bx cs worker-reload](#cs_worker_reload)</td></staging>
    </tr>
    <tr>
      <td>[bx cs worker-rm](#cs_worker_rm)</td>
      <td>[bx cs worker-update](#cs_worker_update)</td>
      <td>[bx cs workers](#cs_workers)</td>
      <td></td>
    </tr>
  </tbody>
</table>

## API-Befehle
{: #api_commands}

### bx cs api ENDPUNKT [--insecure][--skip-ssl-validation] [--api-version WERT][-s]
{: #cs_api}

Legen Sie den API-Endpunkt als ziel für {{site.data.keyword.containershort_notm}} fest. Wenn Sie keinen Endpunkt angeben, können Sie Informationen zu dem aktuellen Endpunkt anzeigen, der als Ziel angegeben wurde.

Zwischen Regionen wechseln? Verwenden Sie stattdessen den Befehl `bx cs region-set` [command](#cs_region-set).
{: tip}

<strong>Befehlsoptionen</strong>:

   <dl>
   <dt><code><em>ENDPOINT</em></code></dt>
   <dd>Der {{site.data.keyword.containershort_notm}}-API-Endpunkt. Beachten Sie, dass sich dieser Endpunkt von {{site.data.keyword.Bluemix_notm}}-Endpunkten unterscheidet. Dieser Wert ist zum Festlegen der API-Endpunkte erforderlich. Folgende Werte sind zulässig:<ul>
   <li>Globaler Endpunkt: https://containers.bluemix.net</li>
   <li>Endpunkt für Asien-Pazifik (Norden): https://ap-north.containers.bluemix.net</li>
   <li>Endpunkt für Asien-Pazifik (Süden): https://ap-south.containers.bluemix.net</li>
   <li>Endpunkt für Zentraleuropa: https://eu-central.containers.bluemix.net</li>
   <li>Endpunkt für Großbritannien (Süden): https://uk-south.containers.bluemix.net</li>
   <li>Endpunkt für Vereinigte Staaten (Osten): https://us-east.containers.bluemix.net</li>
   <li>Endpunkt für Vereinigte Staaten (Süden): https://us-south.containers.bluemix.net</li></ul>
   </dd>

   <dt><code>--insecure</code></dt>
   <dd>Lässt eine unsichere HTTP-Verbindung zu. Dieses Flag ist optional.</dd>

   <dt><code>--skip-ssl-validation</code></dt>
   <dd>Lässt unsichere SSL-Zertifikate zu. Dieses Flag ist optional.</dd>

   <dt><code>--api-version VALUE</code></dt>
   <dd>Gibt die API-Version des Service an, den Sie verwenden möchten. Dieser Wert ist optional.</dd>

   <dt><code>-s</code></dt>
   <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>

   </dl>

**Beispiel**: Zeigen Sie Informationen zu dem aktuellen Endpunkt an, der als Ziel angegeben wurde.
```
bx cs api
```
{: pre}

```
API Endpoint:          https://containers.bluemix.net   
API Version:           v1   
Skip SSL Validation:   false   
Region:                us-south
```
{: screen}


### bx cs api-key-info CLUSTER [--json][-s]
{: #cs_api_key_info}

Zeigen Sie den Namen und die E-Mail-Adresse für den Eigner des IAM-API-Schlüssels in einer {{site.data.keyword.containershort_notm}}-Region an.

Der IAM-API-Schlüssel (IAM, Identity and Access Management) wird automatisch für eine Region festgelegt, wenn die erste Aktion ausgeführt wird, für die die {{site.data.keyword.containershort_notm}}-Administratorzugriffsrichtlinie ausgeführt werden muss. Zum Beispiel erstellt einer Ihrer Benutzer mit Administratorberechtigung den ersten Cluster in der Region `Vereinigte Staaten (Süden)`. Dadurch wird der IAM-API-Schlüssel für diesen Benutzer in dem Konto für diese Region gespeichert. Der API-Schlüssel wird verwendet, um Ressourcen in der IBM Cloud-Infrastruktur (SoftLayer) zu bestellen, z. B. neue Workerknoten oder VLANs.

Wenn ein anderer Benutzer eine Aktion in dieser Region ausführt, die eine Interaktion mit dem Portfolio der IBM Cloud-Infrastruktur (SoftLayer) erfordert, wie z. B. die Erstellung eines neuen Clusters oder das erneute Laden eines Workerknotens, wird der gespeicherte API-Schlüssel verwendet, um festzustellen, ob ausreichende Berechtigungen vorhanden sind, um diese Aktion auszuführen. Um sicherzustellen, dass infrastrukturbezogene Aktionen in Ihrem Cluster erfolgreich ausgeführt werden können, weisen Sie Ihre {{site.data.keyword.containershort_notm}}-Benutzer mit Administratorberechtigung der Infrastrukturzugriffsrichtlinie **Superuser** zu. Weitere Informationen finden unter [Benutzerzugriff verwalten](cs_users.html#infra_access).

Wenn Sie feststellen, dass Sie den API-Schlüssel aktualisieren müssen, der für eine Region gespeichert ist, können Sie dies tun, indem Sie den Befehl [bx cs api-key-reset](#cs_api_key_reset) ausführen. Dieser Befehl erfordert die {{site.data.keyword.containershort_notm}}-Administratorzugriffsrichtlinie und speichert den API-Schlüssel des Benutzers, der diesen Befehl ausführt, im Konto.

**Tipp:** Der API-Schlüssel, der in diesem Befehl zurückgegeben wird, wird möglicherweise nicht verwendet, falls die Berechtigungsnachweise für die IBM Cloud-Infrastruktur (SoftLayer) manuell durch Verwendung des Befehls [bx cs credentials-set](#cs_credentials_set) festgelegt wurden.

<strong>Befehlsoptionen</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Der Name oder die ID des Clusters. Dieser Wert ist erforderlich.</dd>

   <dt><code>--json</code></dt>
   <dd>Druckt die Befehlsausgabe im JSON-Format. Dieser Wert ist optional.</dd>

   <dt><code>-s</code></dt>
   <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>

   </dl>

**Beispiel**:

  ```
  bx cs api-key-info mein_cluster
  ```
  {: pre}


### bx cs api-key-reset [-s]
{: #cs_api_key_reset}

Ersetzen Sie den aktuellen IAM-API-Schlüssel in einer {{site.data.keyword.containershort_notm}}-Region.

Dieser Befehl erfordert die {{site.data.keyword.containershort_notm}}-Administratorzugriffsrichtlinie und speichert den API-Schlüssel des Benutzers, der diesen Befehl ausführt, im Konto. Der IAM-API-Schlüssel ist erforderlich, um Infrastruktur aus dem Portfolio der IBM Cloud-Infrastruktur (SoftLayer) zu bestellen. Einmal gespeichert, wird der API-Schlüssel unabhängig vom Benutzer, der diesen Befehl ausführt, für jede Aktion in einer Region verwendet, die Infrastrukturberechtigungen erfordert. Weitere Informationen dazu, wie IAM-API-Schlüssel funktionieren, finden Sie beim [Befehl `bx cs api-key-info`](#cs_api_key_info).

**Wichtig** Stellen Sie vor Verwendung dieses Befehls sicher, dass der Benutzer, der diesen Befehl ausführt, über die erforderlichen Berechtigungen für [{{site.data.keyword.containershort_notm}} und die IBM Cloud-Infrastruktur (SoftLayer) verfügt.](cs_users.html#users).

<strong>Befehlsoptionen</strong>:

   <dl>
   <dt><code>-s</code></dt>
   <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>
   </dl>


**Beispiel**:

  ```
  bx cs api-key-reset
  ```
  {: pre}


### bx cs apiserver-config-get
{: #cs_apiserver_config_get}

Abrufen von Informationen zu einer Option für die Kubernetes-API-Serverkonfiguration eines Clusters. Dieser Befehl muss mit einem der folgenden Unterbefehle für die Konfigurationsoption kombiniert werden, zu der Sie Informationen abrufen möchten.

#### bx cs apiserver-config-get audit-webhook CLUSTER
{: #cs_apiserver_api_webhook_get}

Zeigen Sie die URL für den fernen Protokollierungsservice an, an den Sie API-Serverauditprotokolle senden. Die URL wurde angegeben, als Sie das Webhook-Back-End für die API-Serverkonfiguration erstellt haben.

<strong>Befehlsoptionen</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Der Name oder die ID des Clusters. Dieser Wert ist erforderlich.</dd>
   </dl>

**Beispiel**:

  ```
  bx cs apiserver-config-get audit-webhook mein_cluster
  ```
  {: pre}

### bx cs apiserver-config-set
{: #cs_apiserver_config_set}

Legen Sie eine Option für die Kubernetes-API-Serverkonfiguration eines Clusters fest. Dieser Befehl muss mit einem der folgenden Unterbefehle für die Konfigurationsoption, die Sie festlegen möchten, kombiniert werden.

#### bx cs apiserver-config-set audit-webhook CLUSTER [--remoteServer SERVER-URL_ODER_-IP][--caCert CA_CERT_PATH] [--clientCert PFAD_DES_CLIENTZERTIFIKATS][--clientKey CLIENT_KEY_PATH]
{: #cs_apiserver_api_webhook_set}

Legen Sie das Webhook-Back-End für die API-Serverkonfiguration fest. Das Webhook-Back-End leitet API-Serverauditprotokolle an einen fernen Server weiter. Eine Webhook-Konfiguration wird auf der Grundlage der Informationen erstellt, die Sie in den Flags dieses Befehls bereitstellen. Wenn Sie keine Informationen in den Flags bereitstellen, wird eine Standard-Webhook-Konfiguration verwendet.

<strong>Befehlsoptionen</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Der Name oder die ID des Clusters. Dieser Wert ist erforderlich.</dd>

   <dt><code>--remoteServer <em>SERVER-URL</em></code></dt>
   <dd>Die URL oder IP-Adresse für den fernen Protokollierungsservice, an den Sie Auditprotokolle senden möchten. Wenn Sie eine unsichere Server-URL angeben, werden alle Zertifikate ignoriert. Dieser Wert ist optional.</dd>

   <dt><code>--caCert <em>PFAD_DES_CA-ZERTIFIKATS</em></code></dt>
   <dd>Der Dateipfad für das CA-Zertifikat, das zum Überprüfen des fernen Protokollierungsservice verwendet wird. Dieser Wert ist optional.</dd>

   <dt><code>--clientCert <em>PFAD_DES_CLIENTZERTIFIKATS</em></code></dt>
   <dd>Der Dateipfad des Clientzertifikats, das zum Authentifizieren beim fernen Protokollierungsservice verwendet wird. Dieser Wert ist optional.</dd>

   <dt><code>--clientKey <em>PFAD_DES_CLIENTSCHLÜSSELS</em></code></dt>
   <dd>Der Dateipfad für den entsprechenden Clientschlüssel, der zum Verbinden mit dem fernen Protokollierungsservice verwendet wird. Dieser Wert ist optional.</dd>
   </dl>

**Beispiel**:

  ```
  bx cs apiserver-config-set audit-webhook mein_cluster --remoteServer https://audit.example.com/audit --caCert /mnt/etc/kubernetes/apiserver-audit/ca.pem --clientCert /mnt/etc/kubernetes/apiserver-audit/cert.pem --clientKey /mnt/etc/kubernetes/apiserver-audit/key.pem
  ```
  {: pre}


### bx cs apiserver-config-unset
{: #cs_apiserver_config_unset}

Inaktivieren Sie eine Option für die Kubernetes-API-Serverkonfiguration eines Clusters. Dieser Befehl muss mit einem der folgenden Unterbefehle für die Konfigurationsoption kombiniert werden, die Sie inaktivieren möchten.

#### bx cs apiserver-config-unset audit-webhook CLUSTER
{: #cs_apiserver_api_webhook_unset}

Inaktivieren Sie die Webhook-Back-End-Konfiguration für den API-Server des Clusters. Wenn Sie das Webhook-Back-End inaktivieren, wird die Weiterleitung von API-Serverauditprotokollen an einen fernen Server gestoppt.

<strong>Befehlsoptionen</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Der Name oder die ID des Clusters. Dieser Wert ist erforderlich.</dd>
   </dl>

**Beispiel**:

  ```
  bx cs apiserver-config-unset audit-webhook mein_cluster
  ```
  {: pre}

### bx cs apiserver-refresh CLUSTER [-s]
{: #cs_apiserver_refresh}

Starten Sie den Kubernetes-Master im Cluster, um Änderungen an der API-Serverkonfiguration anzuwenden.

<strong>Befehlsoptionen</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Der Name oder die ID des Clusters. Dieser Wert ist erforderlich.</dd>

   <dt><code>-s</code></dt>
   <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>

   </dl>

**Beispiel**:

  ```
  bx cs apiserver-refresh mein_cluster
  ```
  {: pre}


<br />


## Befehle für die Verwendung des CLI-Plug-ins
{: #cli_plug-in_commands}

### bx cs help
{: #cs_help}

Anzeigen einer Liste der unterstützten Befehle und Parameter.

<strong>Befehlsoptionen</strong>:

   Keine

**Beispiel**:

  ```
  bx cs help
  ```
  {: pre}


### bx cs init [--host HOST][--insecure] [-p][-u] [-s]
{: #cs_init}

Initialisieren des {{site.data.keyword.containershort_notm}}-Plug-ins oder Angeben der Region, in der Sie Kubernetes-Cluster erstellen oder darauf zugreifen möchten.

<strong>Befehlsoptionen</strong>:

   <dl>
   <dt><code>--host <em>HOST</em></code></dt>
   <dd>Der zu verwendende {{site.data.keyword.containershort_notm}}-API-Endpunkt.  Dieser Wert ist optional. [Zeigen Sie die verfügbaren API-Endpunktwerte an.](cs_regions.html#container_regions)</dd>

   <dt><code>--insecure</code></dt>
   <dd>Lässt eine unsichere HTTP-Verbindung zu. </dd>

   <dt><code>-p</code></dt>
   <dd>Ihr IBM Cloud-Kennwort.</dd>

   <dt><code>-u</code></dt>
   <dd>Ihr IBM Cloud-Benutzername.</dd>

   <dt><code>-s</code></dt>
   <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>

   </dl>

**Beispiel**:


```
bx cs init --host https://uk-south.containers.bluemix.net
```
{: pre}


### bx cs messages
{: #cs_messages}

Aktuelle Nachrichten für den Benutzer der IBMid anzeigen.

**Beispiel**:

```
bx cs messages
```
{: pre}


<br />


## Clusterbefehle: Management
{: #cluster_mgmt_commands}


### bx cs cluster-config CLUSTER [--admin][--export] [-s][--yaml]
{: #cs_cluster_config}

Laden Sie nach erfolgter Anmeldung Kubernetes-Konfigurationsdaten und -Zertifikate herunter, um eine Verbindung zum Cluster herzustellen und `kubectl`-Befehle auszuführen. Die Dateien werden in `ausgangsverzeichnis_des_benutzers/.bluemix/plugins/container-service/clusters/<cluster_name>`.

**Befehlsoptionen**:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Der Name oder die ID des Clusters. Dieser Wert ist erforderlich.</dd>

   <dt><code>--admin</code></dt>
   <dd>Laden Sie die TLS-Zertifikate und die entsprechenden Berechtigungsdateien für die Rolle 'Superuser' herunter. Sie können die Zertifikate verwenden, um Tasks in einem Cluster zu automatisieren, ohne eine erneute Authentifizierung durchführen zu müssen. Die Dateien werden in  `<user_home_directory>/.bluemix/plugins/container-service/clusters/<clustername>-admin` heruntergeladen. Dieser Wert ist optional.</dd>

   <dt><code>--export</code></dt>
   <dd>Laden Sie die Kubernetes-Konfigurationsdaten und -Zertifikate ohne Nachrichten außer dem Exportbefehl herunter. Da keine Nachrichten angezeigt werden, können Sie dieses Flag verwenden, wenn Sie automatisierte Scripts erstellen. Dieser Wert ist optional.</dd>

  <dt><code>-s</code></dt>
  <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>

  <dt><code>--yaml</code></dt>
  <dd>Druckt die Befehlsausgabe im YALM-Format. Dieser Wert ist optional.</dd>

   </dl>

**Beispiel**:

```
bx cs cluster-config mein_cluster
```
{: pre}


### bx cs cluster-create [--file DATEISTANDORT][--hardware HARDWARE] --location STANDORT --machine-type MASCHINENTYP --name NAME [--kube-version MAJOR.MINOR.PATCH][--no-subnet] [--private-vlan PRIVATES_VLAN][--public-vlan PUBLIC_VLAN] [--workers WORKER][--disable-disk-encrypt] [--trusted][-s]
{: #cs_cluster_create}

Erstellen Sie einen Cluster in Ihrer Organisation. Für kostenlose Cluster geben Sie den Clusternamen an; alles andere ist auf einen Standardwert gesetzt. Ein kostenloser Cluster wird nach 21 Tagen automatisch gelöscht. Sie können zu einem Zeitpunkt jeweils nur über einen kostenlosen Cluster verfügen. Wenn Sie die volle Funktionalität von Kubernetes nutzen möchten, erstellen Sie einen Standardcluster.

<strong>Befehlsoptionen</strong>

<dl>
<dt><code>--file <em>DATEISTANDORT</em></code></dt>

<dd>Der Pfad zur YAML-Datei für die Erstellung Ihres Standardclusters. Statt die Merkmale Ihres Clusters mithilfe der in diesem Befehl bereitgestellten Optionen zu definieren, können Sie eine YAML-Datei verwenden.  Dieser Wert ist für Standardcluster optional und steht für kostenlose Cluster nicht zur Verfügung.

<p><strong>Hinweis:</strong> Wenn Sie dieselbe Option wie im Befehl als Parameter in der YAML-Datei bereitstellen, hat der Wert im Befehl Vorrang vor dem Wert in der YAML. Beispiel: Sie definieren eine Position in Ihrer YAML-Datei und verwenden die Option <code>--location</code> im Befehl. Der Wert, den Sie in die Befehlsoption eingegeben haben, überschreibt den Wert in der YAML-Datei.

<pre class="codeblock">
<code>name: <em>&lt;clustername&gt;</em>
location: <em>&lt;standort&gt;</em>
no-subnet: <em>&lt;kein_teilnetz&gt;</em>
machine-type: <em>&lt;maschinentyp&gt;</em>
private-vlan: <em>&lt;privates_vlan&gt;</em>
public-vlan: <em>&lt;öffentliches_vlan&gt;</em>
hardware: <em>&lt;shared_oder_dedicated&gt;</em>
workerNum: <em>&lt;anzahl_worker&gt;</em>
kube-version: <em>&lt;kube-version&gt;</em>
diskEncryption: <em>false</em>
trusted: <em>true</em>
</code></pre>


<table>
    <caption>Erklärung der Komponenten der YAML-Datei</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der YAML-Dateikomponenten</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>name</em></code></td>
    <td>Ersetzen Sie <code><em>&lt;clustername&gt;</em></code> durch den Namen Ihres Clusters. Der Name muss mit einem Buchstaben beginnen, darf Buchstaben, Ziffern und den Bindestrich (-) enthalten und darf maximal 35 Zeichen lang sein. Der vollständig qualifizierte Domänenname für die Ingress-Unterdomäne setzt sich aus dem Clusternamen und der Region zusammen, in der der Cluster bereitgestellt wird. Um sicherzustellen, dass die Ingress-Unterdomäne innerhalb einer Region eindeutig ist, wird der Clustername möglicherweise abgeschnitten und es wird ein beliebiger Wert innerhalb des Ingress-Domänennamens angehängt.
</td>
    </tr>
    <tr>
    <td><code><em>standort</em></code></td>
    <td>Ersetzen Sie <code><em>&lt;standort&gt;</em></code> durch den Standort, an dem Sie Ihren Cluster erstellen möchten. Welche Standorte verfügbar sind, hängt von der Region ab, bei der Sie angemeldet sind. Führen Sie den Befehl <code>bx cs locations</code> aus, um die verfügbaren Standorte aufzulisten. </td>
     </tr>
     <tr>
     <td><code><em>kein_teilnetz</em></code></td>
     <td>Standardmäßig werden sowohl ein öffentliches als auch ein privates portierbares Teilnetz in dem VLAN erstellt, das dem Cluster zugeordnet ist. Ersetzen Sie <code><em>&lt;kein_teilnetz&gt;</em></code> durch <code><em>true</em></code>, um zu verhindern, dass Teilnetze für den Cluster erstellt werden. Sie können Teilnetze zu einem späteren Zeitpunkt [erstellen](#cs_cluster_subnet_create) oder zu einem Cluster [hinzufügen](#cs_cluster_subnet_add).</td>
      </tr>
     <tr>
     <td><code><em>maschinentyp</em></code></td>
     <td>Ersetzen Sie <code><em>&lt;maschinentyp&gt;</em></code> durch den Maschinentyp, auf dem Sie Ihre Workerknoten bereitstellen möchten. Sie können Ihre Workerknoten als virtuelle Maschinen auf gemeinsam genutzter oder dedizierter Hardware oder als physische Maschinen auf Bare-Metal-Systemen bereitstellen. Die verfügbaren physischen und virtuellen Maschinentypen variieren je nach dem Standort, an dem Sie den Cluster bereitstellen. Weitere Informationen finden Sie in der Dokumentation zum [Befehl](cs_cli_reference.html#cs_machine_types) `bx cs machine-type`.</td>
     </tr>
     <tr>
     <td><code><em>privates_vlan</em></code></td>
     <td>Ersetzen Sie <code><em>&lt;privates_vlan&gt;</em></code> durch die ID des privaten VLANs, das Sie für Ihre Workerknoten verwenden möchten. Führen Sie <code>bx cs vlans <em>&lt;standort&gt;</em></code> aus und suchen Sie nach VLAN-Routern, die mit <code>bcr</code> (Back-End-Router) beginnen, um verfügbare VLANs aufzulisten.</td>
     </tr>
     <tr>
     <td><code><em>öffentliches_vlan</em></code></td>
     <td>Ersetzen Sie <code><em>&lt;öffentliches_vlan&gt;</em></code> durch die ID des öffentlichen VLANs, das Sie für Ihre Workerknoten verwenden möchten. Führen Sie <code>bx cs vlans <em>&lt;standort&gt;</em></code> aus und suchen Sie nach VLAN-Routern, die mit <code>fcr</code> (Front-End-Router) beginnen, um verfügbare VLANs aufzulisten.</td>
     </tr>
     <tr>
     <td><code><em>hardware</em></code></td>
     <td>Für virtuelle Maschinentypen: Der Grad an Hardware-Isolation für Ihren Workerknoten. Verwenden Sie 'dedicated', wenn
Sie verfügbare physische Ressourcen haben möchten, die nur Sie nutzen können, oder 'shared', um zuzulassen, dass
physische Ressourcen mit anderen IBM Kunden gemeinsam genutzt werden können. Die Standardeinstellung ist
<code>shared</code>.</td>
     </tr>
     <tr>
     <td><code><em>anzahl_worker</em></code></td>
     <td>Ersetzen Sie <code><em>&lt;anzahl_worker&gt;</em></code> durch die Anzahl von Workerknoten, die Sie implementieren wollen.</td>
     </tr>
     <tr>
      <td><code><em>kube-version</em></code></td>
      <td>Die Kubernetes-Version für den Cluster-Masterknoten. Dieser Wert ist optional. Wenn die Version nicht angegeben ist, wird der Cluster mit dem Standard für unterstützte Kubernetes-Versionen erstellt. Führen Sie den Befehl <code>bx cs kube-versions</code> aus, um die verfügbaren Versionen anzuzeigen.
</td></tr>
      <tr>
      <td><code>diskEncryption: <em>false</em></code></td>
      <td>Workerknoten weisen standardmäßig Verschlüsselung auf. [Weitere Informationen finden Sie hier](cs_secure.html#worker). Um die Verschlüsselung zu inaktivieren, schließen Sie diese Option ein und legen Sie den Wert auf <code>false</code> fest.</td></tr>
      <tr>
      <td><code>trusted: <em>true</em></code></td>
      <td>**Nur für Bare-Metal**: Aktivieren Sie [Trusted Compute](cs_secure.html#trusted_compute), um Ihre Bare-Metal-Workerknoten auf Manipulation zu überprüfen. Wenn Sie Trusted Compute während der Clustererstellung nicht aktivieren, dies jedoch später nachholen möchten, können Sie den [Befehl](cs_cli_reference.html#cs_cluster_feature_enable) `bx cs feature-enable` verwenden. Nachdem Sie Trusted Compute aktiviert haben, können Sie es später nicht mehr inaktivieren.</td></tr>
     </tbody></table>
    </p></dd>

<dt><code>--hardware <em>HARDWARE</em></code></dt>
<dd>Der Grad an Hardware-Isolation für Ihren Workerknoten. Verwenden Sie 'dedicated', wenn Sie verfügbare physische Ressourcen haben möchten, die nur Sie nutzen können, oder 'shared', um zuzulassen, dass physische Ressourcen mit anderen IBM Kunden gemeinsam genutzt werden können. Die Standardeinstellung ist
'shared'.  Dieser Wert ist für Standardcluster optional und steht für kostenlose Cluster nicht zur Verfügung.</dd>

<dt><code>--location <em>STANDORT</em></code></dt>
<dd>Der Standort, an dem Sie den Cluster erstellen möchten. Welche Standorte Ihnen zur Verfügung stehen, hängt von der {{site.data.keyword.Bluemix_notm}}-Region ab, bei der Sie angemeldet sind. Wählen Sie die Region aus,
die Ihrem Standort am nächsten ist, um eine optimale Leistung zu erhalten.  Dieser Wert ist für Standardcluster erforderlich und für kostenlose Cluster optional.

<p>Überprüfen Sie die [verfügbaren Standorte](cs_regions.html#locations).
</p>

<p><strong>Hinweis:</strong> Beachten Sie bei der Auswahl eines Standorts außerhalb Ihres Landes, dass Sie gegebenenfalls eine gesetzliche Genehmigung benötigen, bevor Daten physisch in einem anderen Land gespeichert werden können.</p>
</dd>

<dt><code>--machine-type <em>MASCHINENTYP</em></code></dt>
<dd>Wählen Sie einen Maschinentyp aus. Sie können Ihre Workerknoten als virtuelle Maschinen auf gemeinsam genutzter oder dedizierter Hardware oder als physische Maschinen auf Bare-Metal-Systemen bereitstellen. Die verfügbaren physischen und virtuellen Maschinentypen variieren je nach dem Standort, an dem Sie den Cluster bereitstellen. Weitere Informationen finden Sie in der Dokumentation zum [Befehl](cs_cli_reference.html#cs_machine_types) `bx cs machine-types`. Dieser Wert ist für Standardcluster erforderlich und steht für kostenlose Cluster nicht zur Verfügung.</dd>

<dt><code>--name <em>NAME</em></code></dt>
<dd>Der Name für den Cluster.  Dieser Wert ist erforderlich. Der Name muss mit einem Buchstaben beginnen, darf Buchstaben, Ziffern und den Bindestrich (-) enthalten und darf maximal 35 Zeichen lang sein. Der vollständig qualifizierte Domänenname für die Ingress-Unterdomäne setzt sich aus dem Clusternamen und der Region zusammen, in der der Cluster bereitgestellt wird. Um sicherzustellen, dass die Ingress-Unterdomäne innerhalb einer Region eindeutig ist, wird der Clustername möglicherweise abgeschnitten und es wird ein beliebiger Wert innerhalb des Ingress-Domänennamens angehängt.
</dd>

<dt><code>--kube-version <em>MAJOR.MINOR.PATCH</em></code></dt>
<dd>Die Kubernetes-Version für den Cluster-Masterknoten. Dieser Wert ist optional. Wenn die Version nicht angegeben ist, wird der Cluster mit dem Standard für unterstützte Kubernetes-Versionen erstellt. Führen Sie den Befehl <code>bx cs kube-versions</code> aus, um die verfügbaren Versionen anzuzeigen.
</dd>

<dt><code>--no-subnet</code></dt>
<dd>Standardmäßig werden sowohl ein öffentliches als auch ein privates portierbares Teilnetz in dem VLAN erstellt, das dem Cluster zugeordnet ist. Schließen Sie das Flag <code>--no-subnet</code> ein, um zu verhindern, dass Teilnetze für den Cluster erstellt werden. Sie können Teilnetze zu einem späteren Zeitpunkt [erstellen](#cs_cluster_subnet_create) oder zu einem Cluster [hinzufügen](#cs_cluster_subnet_add).</dd>

<dt><code>--private-vlan <em>PRIVATES_VLAN</em></code></dt>
<dd>

<ul>
<li>Dieser Parameter ist für kostenlose Cluster nicht verfügbar.</li>
<li>Wenn dieser Standardcluster der erste Standardcluster ist, den Sie an diesem Standort erstellen, schließen Sie dieses Flag nicht ein. Ein privates VLAN wird zusammen mit den Clustern für Sie erstellt.</li>
<li>Wenn Sie bereits einen Standardcluster an diesem Standort oder ein privates VLAN in IBM Cloud Infrastructure (SoftLayer) erstellt haben, müssen Sie dieses private VLAN angeben.

<p><strong>Hinweis:</strong> Private VLAN-Router beginnen immer mit <code>bcr</code> (Back-End-Router) und öffentliche VLAN-Router immer mit <code>fcr</code> (Front-End-Router). Wenn Sie einen Cluster erstellen und die öffentlichen und privaten VLANs angeben, müssen die Zahlen- und Buchstabenkombinationen nach diesen Präfixen übereinstimmen.</p></li>
</ul>

<p>Führen Sie <code>bx cs vlans <em>&lt;standort&gt;</em></code> aus, um herauszufinden, ob Sie bereits über ein privates VLAN für einen bestimmten Standort verfügen, oder um den Namen eines vorhandenen
privaten VLANs zu erfahren.</p></dd>

<dt><code>--public-vlan <em>ÖFFENTLICHES_VLAN</em></code></dt>
<dd>
<ul>
<li>Dieser Parameter ist für kostenlose Cluster nicht verfügbar.</li>
<li>Wenn dieser Standardcluster der erste Standardcluster ist, den Sie an diesem Standort erstellen, verwenden Sie dieses Flag nicht. Ein öffentliches VLAN wird zusammen mit dem Cluster für Sie erstellt.</li>
<li>Wenn Sie bereits einen Standardcluster an diesem Standort oder ein öffentliches VLAN in IBM Cloud Infrastructure (SoftLayer) erstellt haben, geben Sie dieses öffentliche VLAN an. Wenn Sie Ihre Workerknoten nur mit einem private VLAN verbinden möchten, geben Sie diese Option nicht an.

<p><strong>Hinweis:</strong> Private VLAN-Router beginnen immer mit <code>bcr</code> (Back-End-Router) und öffentliche VLAN-Router immer mit <code>fcr</code> (Front-End-Router). Wenn Sie einen Cluster erstellen und die öffentlichen und privaten VLANs angeben, müssen die Zahlen- und Buchstabenkombinationen nach diesen Präfixen übereinstimmen.</p></li>
</ul>

<p>Führen Sie <code>bx cs vlans <em>&lt;standort&gt;</em></code> aus, um herauszufinden, ob Sie bereits über ein öffentliches VLAN für einen bestimmten Standort verfügen, oder um den Namen eines vorhandenen
öffentlichen VLANs zu erfahren.</p></dd>

<dt><code>--workers WORKER</code></dt>
<dd>Die Anzahl von Workerknoten, die Sie in Ihrem Cluster bereitstellen wollen. Wenn Sie diese Option nicht angeben, wird ein Cluster mit einem Workerknoten erstellt. Dieser Wert ist für Standardcluster optional und steht für kostenlose Cluster nicht zur Verfügung.

<p><strong>Hinweis:</strong> Jedem Workerknoten werden eine eindeutige Workerknoten-ID und ein Domänenname zugewiesen, die nach dem Erstellen des Clusters nicht manuell geändert
werden dürfen. Wenn die ID oder der Domänenname geändert wird, kann der Kubernetes-Master Ihren Cluster nicht mehr verwalten.</p></dd>

<dt><code>--disable-disk-encrypt</code></dt>
<dd>Workerknoten weisen standardmäßig Verschlüsselung auf. [Weitere Informationen finden Sie hier](cs_secure.html#worker). Wenn Sie die Verschlüsselung inaktivieren möchten, schließen Sie diese Option ein.</dd>

<dt><code>--trusted</code></dt>
<dd><p>**Nur für Bare-Metal**: Aktivieren Sie [Trusted Compute](cs_secure.html#trusted_compute), um Ihre Bare-Metal-Workerknoten auf Manipulation zu überprüfen. Wenn Sie Trusted Compute während der Clustererstellung nicht aktivieren, dies jedoch später nachholen möchten, können Sie den [Befehl](cs_cli_reference.html#cs_cluster_feature_enable) `bx cs feature-enable` verwenden. Nachdem Sie Trusted Compute aktiviert haben, können Sie es später nicht mehr inaktivieren.</p>
<p>Um zu überprüfen, ob der Bare-Metal-Maschinentyp Trusted Compute unterstützt, überprüfen Sie das Feld `Trustable` in der Ausgabe des [Befehls](#cs_machine_types) `bx cs machine-types <location>`. Um zu überprüfen, ob für ein Cluster Trusted Compute aktiviert wurde, sehen Sie sich das Feld **Trust ready** in der Ausgabe des [Befehls](#cs_cluster_get) `bx cs cluster-get` an. Um zu überprüfen, ob für einen Bare-Metal-Workerknoten Trusted Compute aktiviert wurde, sehen Sie sich das Feld **Trust** in der Ausgabe des [Befehls](#cs_worker_get)`bx cs worker-get` an.</p></dd>

<dt><code>-s</code></dt>
<dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>
</dl>

**Beispiele**:

  

  **Kostenlosen Cluster erstellen**: Geben Sie nur den Clusternamen an; alles andere ist auf einen Standardwert gesetzt. Ein kostenloser Cluster wird nach 21 Tagen automatisch gelöscht. Sie können zu einem Zeitpunkt jeweils nur über einen kostenlosen Cluster verfügen. Wenn Sie die volle Funktionalität von Kubernetes nutzen möchten, erstellen Sie einen Standardcluster.

  ```
  bx cs cluster-create --name mein_cluster
  ```
  {: pre}

  **Den ersten Standardcluster erstellen**: Für den ersten Standardcluster, der an einem Standardort erstellt wird, wird auch ein privates VLAN erstellt. Schließen Sie daher nicht das Flag `--public-vlan` ein.
  {: #example_cluster_create}

  ```
  bx cs cluster-create --location dal10 --private-vlan meine_private_vlan-id --machine-type u2c.2x4 --name mein_cluster --hardware shared --workers 2
  ```
  {: pre}

  **Nachfolgende Standardcluster erstellen**: Wenn Sie bereits einen Standardcluster an diesem Standort oder ein öffentliches VLAN in IBM Cloud Infrastructure (SoftLayer) erstellt haben, geben Sie dieses öffentliche VLAN mit dem Flag `--public-vlan` an. Führen Sie  `bx cs vlans <location>` aus, um herauszufinden, ob Sie bereits über ein öffentliches VLAN für einen bestimmten Standort verfügen, oder um den Namen eines vorhandenen öffentlichen VLANs zu erfahren.

  ```
  bx cs cluster-create --location dal10 --public-vlan meine_öffentliche_vlan-id --private-vlan meine_private_vlan-id --machine-type u2c.2x4 --name mein_cluster --hardware shared --workers 2
  ```
  {: pre}

  **Erstellen Sie einen Cluster in einer {{site.data.keyword.Bluemix_dedicated_notm}}-Umgebung**:

  ```
  bx cs cluster-create --machine-type maschinentyp --workers anzahl --name clustername
  ```
  {: pre}

### bx cs cluster-feature-enable [-f] CLUSTER [--trusted][-s]
{: #cs_cluster_feature_enable}

Funktion auf einem vorhandenen Cluster aktivieren.

<strong>Befehlsoptionen</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Der Name oder die ID des Clusters. Dieser Wert ist erforderlich.</dd>

   <dt><code>-f</code></dt>
   <dd>Verwenden Sie diese Option, um die Option <code>--trusted</code> ohne Benutzereingabeaufforderungen zu erzwingen. Dieser Wert ist optional.</dd>

   <dt><code><em>--trusted</em></code></dt>
   <dd><p>Das Flag zum Aktivieren von [Trusted Compute](cs_secure.html#trusted_compute) für alle unterstützten Bare-Metal-R einschließen, die sich im Cluster befinden. Nachdem Sie Trusted Compute aktiviert haben, können Sie es später nicht mehr inaktivieren.</p>
   <p>Um zu überprüfen, ob der Bare-Metal-Maschinentyp Trusted Compute unterstützt, überprüfen Sie das Feld **Trustable** in der Ausgabe des [Befehls](#cs_machine_types) `bx cs machine-types <location>`. Um zu überprüfen, ob für ein Cluster Trusted Compute aktiviert wurde, sehen Sie sich das Feld **Trust ready** in der Ausgabe des [Befehls](#cs_cluster_get) `bx cs cluster-get` an. Um zu überprüfen, ob für einen Bare-Metal-Workerknoten Trusted Compute aktiviert wurde, sehen Sie sich das Feld **Trust** in der Ausgabe des [Befehls](#cs_worker_get)`bx cs worker-get` an.</p></dd>

  <dt><code>-s</code></dt>
   <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>
</dl>

**Beispielbefehl**:

  ```
  bx cs cluster-feature-enable mein_cluster --trusted=true
  ```
  {: pre}

### bx cs cluster-get CLUSTER [--json][--showResources] [-s]
{: #cs_cluster_get}

Anzeigen von Informationen zu einem Cluster in Ihrer Organisation.

<strong>Befehlsoptionen</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Der Name oder die ID des Clusters. Dieser Wert ist erforderlich.</dd>

   <dt><code>--json</code></dt>
   <dd>Druckt die Befehlsausgabe im JSON-Format. Dieser Wert ist optional.</dd>

   <dt><code><em>--showResources</em></code></dt>
   <dd>Weitere Clusterressourcen wie z. B. Add-ons, VLANs, Teilnetze und Speicher anzeigen.</dd>


  <dt><code>-s</code></dt>
  <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>
  </dl>



**Beispielbefehl**:

  ```
  bx cs cluster-get mein_cluster --showResources
  ```
  {: pre}

**Beispielausgabe**:

  ```
  Name:        mein_cluster
  ID:          abc1234567
  State:       normal
  Trust ready: false
  Created:     2018-01-01T17:19:28+0000
  Location:    dal10
  Master URL:  https://169.xx.xxx.xxx:xxxxx
  Ingress subdomain: mein_cluster.us-south.containers.appdomain.cloud
  Ingress secret:    mein_cluster
  Workers:     3
  Version:     1.7.16_1511* (1.8.11_1509 latest)
  Owner Email: name@example.com
  Monitoring dashboard: https://metrics.ng.bluemix.net/app/#/grafana4/dashboard/db/link

  Addons
  Name                   Enabled
  customer-storage-pod   true
  basic-ingress-v2       true
  storage-watcher-pod    true

  Subnet VLANs
  VLAN ID   Subnet CIDR         Public   User-managed
  2234947   10.xxx.xx.xxx/29    false    false
  2234945   169.xx.xxx.xxx/29  true    false

  ```
  {: screen}

### bx cs cluster-rm [-f] CLUSTER [-s]
{: #cs_cluster_rm}

Entfernen eines Clusters aus der Organisation.

<strong>Befehlsoptionen</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Der Name oder die ID des Clusters. Dieser Wert ist erforderlich.</dd>

   <dt><code>-f</code></dt>
   <dd>Geben Sie diese Option an, um das Entfernen eines Clusters ohne Benutzereingabeaufforderungen zu erzwingen. Dieser Wert ist optional.</dd>

   <dt><code>-s</code></dt>
   <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>

   </dl>

**Beispiel**:

  ```
  bx cs cluster-rm mein_cluster
  ```
  {: pre}


### bx cs cluster-update [-f] CLUSTER [--kube-version MAJOR.MINOR.PATCH][--force-update] [-s]
{: #cs_cluster_update}

Aktualisieren des Kubernetes-Masters auf die API-Standardversion. Während der Aktualisierung können Sie weder auf den Cluster zugreifen noch eine Änderung am Cluster vornehmen. Workerknoten, Apps und Ressourcen, die vom Benutzer bereitgestellt wurden, werden nicht geändert und weiterhin ausgeführt.

Möglicherweise müssen Sie Ihre YAML-Dateien für zukünftige Bereitstellungen ändern. Lesen Sie die detaillierten Informationen in diesen [Releaseinformationen](cs_versions.html).

<strong>Befehlsoptionen</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Der Name oder die ID des Clusters. Dieser Wert ist erforderlich.</dd>

   <dt><code>--kube-version <em>MAJOR.MINOR.PATCH</em></code></dt>
   <dd>Die Kubernetes-Version des Clusters. Wenn Sie keine Version angeben, wird der Kubernetes-Master auf die API-Standardversion aktualisiert. Führen Sie den Befehl [bx cs kube-versions](#cs_kube_versions) aus, um die verfügbaren Versionen anzuzeigen. Dieser Wert ist optional.</dd>

   <dt><code>-f</code></dt>
   <dd>Geben Sie diese Option an, um die Aktualisierung des Masters ohne Benutzereingabeaufforderungen zu erzwingen. Dieser Wert ist optional.</dd>

   <dt><code>--force-update</code></dt>
   <dd>Versuch einer Aktualisierung, selbst wenn die Änderung sich über mehr als zwei Nebenversionen erstreckt. Dieser Wert ist optional.</dd>

   <dt><code>-s</code></dt>
   <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>
   </dl>

**Beispiel**:

  ```
  bx cs cluster-update mein_cluster
  ```
  {: pre}


### bx cs clusters [--json][-s]
{: #cs_clusters}

Anzeigen einer Liste der Cluster in Ihrer Organisation.

<strong>Befehlsoptionen</strong>:

  <dl>
  <dt><code>--json</code></dt>
  <dd>Druckt die Befehlsausgabe im JSON-Format. Dieser Wert ist optional.</dd>

  <dt><code>-s</code></dt>
  <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>
  </dl>

**Beispiel**:

  ```
  bx cs clusters
  ```
  {: pre}


### bx cs kube-versions [--json][-s]
{: #cs_kube_versions}

Anzeigen einer Liste der Kubernetes-Versionen, die in {{site.data.keyword.containershort_notm}} unterstützt werden. Aktualisieren Sie den [Cluster-Master](#cs_cluster_update) und die [Workerknoten](cs_cli_reference.html#cs_worker_update) auf die Standardversion für die aktuellen und stabilen Leistungsmerkmale.

**Befehlsoptionen**:

  <dl>
  <dt><code>--json</code></dt>
  <dd>Druckt die Befehlsausgabe im JSON-Format. Dieser Wert ist optional.</dd>

  <dt><code>-s</code></dt>
  <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>
  </dl>

**Beispiel**:

  ```
  bx cs kube-versions
  ```
  {: pre}



<br />



## Clusterbefehle: Services und Integrationen
{: #cluster_services_commands}


### bx cs cluster-service-bind CLUSTER KUBERNETES-NAMENSBEREICH SERVICEINSTANZNAME [-s]
{: #cs_cluster_service_bind}

Hinzufügen eines {{site.data.keyword.Bluemix_notm}}-Service zu einem Cluster. Um die verfügbaren {{site.data.keyword.Bluemix_notm}}-Services aus dem {{site.data.keyword.Bluemix_notm}}-Katalog anzuzeigen, führen Sie den Befehl `bx service offerings` aus. **Hinweis**: Es können nur {{site.data.keyword.Bluemix_notm}}-Services hinzugefügt werden, die Serviceschlüssel unterstützen.

<strong>Befehlsoptionen</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Der Name oder die ID des Clusters. Dieser Wert ist erforderlich.</dd>

   <dt><code><em>KUBERNETES-NAMENSBEREICH</em></code></dt>
   <dd>Der Name des Kubernetes-Namensbereichs. Dieser Wert ist erforderlich.</dd>

   <dt><code><em>SERVICEINSTANZNAME</em></code></dt>
   <dd>Der Name der {{site.data.keyword.Bluemix_notm}}-Serviceinstanz, die Sie binden möchten. Führen Sie <code>bx service list</code> aus, um den Namen Ihrer Serviceinstanz zu ermitteln. Wenn mehr als eine Instanz im Account denselben Namen hat, verwenden Sie die Serviceinstanz-ID anstelle des Namens. Um die ID zu suchen, führen Sie den Befehl <code>bx service show <serviceinstanzname> --guid</code> aus. Einer dieser Werte ist erforderlich.</dd>

   <dt><code>-s</code></dt>
   <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>

   </dl>

**Beispiel**:

  ```
  bx cs cluster-service-bind mein_cluster mein_namensbereich meine_serviceinstanz
  ```
  {: pre}


### bx cs cluster-service-unbind CLUSTER KUBERNETES-NAMENSBEREICH GUID_DER_SERVICEINSTANZ [-s]
{: #cs_cluster_service_unbind}

Entfernen eines {{site.data.keyword.Bluemix_notm}}-Service aus einem Cluster.

**Hinweis:** Wenn Sie einen {{site.data.keyword.Bluemix_notm}}-Service entfernen, werden die Serviceberechtigungsnachweise aus dem Cluster entfernt. Wenn ein Pod den Service noch verwendet,
schlägt er fehl, weil die Serviceberechtigungsnachweise nicht gefunden werden können.

<strong>Befehlsoptionen</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Der Name oder die ID des Clusters. Dieser Wert ist erforderlich.</dd>

   <dt><code><em>KUBERNETES-NAMENSBEREICH</em></code></dt>
   <dd>Der Name des Kubernetes-Namensbereichs. Dieser Wert ist erforderlich.</dd>

   <dt><code><em>GUID_DER_SERVICEINSTANZ</em></code></dt>
   <dd>Die ID der {{site.data.keyword.Bluemix_notm}}-Serviceinstanz, die Sie entfernen möchten. Führen Sie `bx cs cluster-services <cluster_name_or_ID>` aus, um die ID der Serviceinstanz abzurufen. Dieser Wert ist erforderlich.</dd>

   <dt><code>-s</code></dt>
   <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>

   </dl>

**Beispiel**:

  ```
  bx cs cluster-service-unbind mein_cluster mein_namensbereich 8567221
  ```
  {: pre}


### bx cs cluster-services CLUSTER [--namespace KUBERNETES-NAMENSBEREICH][--all-namespaces] [--json][-s]
{: #cs_cluster_services}

Auflisten der Services, die an einen oder an alle Kubernetes-Namensbereiche in einem Cluster gebunden sind. Sind keine Optionen angegeben, werden die Services für den Standardnamensbereich angezeigt.

<strong>Befehlsoptionen</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Der Name oder die ID des Clusters. Dieser Wert ist erforderlich.</dd>

   <dt><code>--namespace <em>KUBERNETES-NAMENSBEREICH</em></code>, <code>-n
<em>KUBERNETES-NAMENSBEREICH</em></code></dt>
   <dd>Schließen Sie die Services ein, die an einen bestimmten Namensbereich in einem Cluster gebunden sind. Dieser Wert ist optional.</dd>

   <dt><code>--all-namespaces</code></dt>
    <dd>Schließen Sie die Services ein, die an alle Namensbereiche in einem Cluster gebunden sind. Dieser Wert ist optional.</dd>

    <dt><code>--json</code></dt>
    <dd>Druckt die Befehlsausgabe im JSON-Format. Dieser Wert ist optional.</dd>

    <dt><code>-s</code></dt>
    <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>

    </dl>

**Beispiel**:

  ```
  bx cs cluster-services mein_cluster --namespace mein_namensbereich
  ```
  {: pre}



### bx cs webhook-create --cluster CLUSTER --level STUFE --type slack --url URL  [-s]
{: #cs_webhook_create}

Registrieren Sie einen Webhook.

<strong>Befehlsoptionen</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>Der Name oder die ID des Clusters. Dieser Wert ist erforderlich.</dd>

   <dt><code>--level <em>STUFE</em></code></dt>
   <dd>Die Benachrichtigungsstufe wie etwa <code>Normal</code> oder <code>Warnung</code>. <code>Warnung</code> ist der Standardwert. Dieser Wert ist optional.</dd>

   <dt><code>--type <em>slack</em></code></dt>
   <dd>Der Typ des Webhook. Gegenwärtig wird 'slack' unterstützt. Dieser Wert ist erforderlich.</dd>

   <dt><code>--url <em>URL</em></code></dt>
   <dd>Die URL für den Webhook. Dieser Wert ist erforderlich.</dd>

   <dt><code>-s</code></dt>
   <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>
   </dl>

**Beispiel**:

  ```
  bx cs webhook-create --cluster mein_cluster --level Normal --type slack --url http://github.com/mywebhook
  ```
  {: pre}


<br />


## Clusterbefehle: Teilnetze
{: #cluster_subnets_commands}

### bx cs cluster-subnet-add CLUSTER TEILNETZ [-s]
{: #cs_cluster_subnet_add}

Zurverfügungstellung eines Teilnetzes in einem Konto von IBM Cloud Infrastructure (SoftLayer) für einen angegebenen Cluster.

**Hinweis:**
* Wenn Sie ein Teilnetz in einem Cluster verfügbar machen, werden IP-Adressen dieses Teilnetzes zum Zweck von Clusternetzen verwendet. Vermeiden Sie IP-Adresskonflikte, indem Sie ein Teilnetz mit nur einem Cluster verwenden. Verwenden Sie kein Teilnetz für mehrere Cluster oder für andere
Zwecke außerhalb von {{site.data.keyword.containershort_notm}} gleichzeitig.
* Um zwischen Teilnetzen in demselben VLAN weiterzuleiten, müssen Sie [VLAN-Spanning aktivieren](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning).

<strong>Befehlsoptionen</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Der Name oder die ID des Clusters. Dieser Wert ist erforderlich.</dd>

   <dt><code><em>TEILNETZ</em></code></dt>
   <dd>Die ID des Teilnetzes. Dieser Wert ist erforderlich.</dd>

   <dt><code>-s</code></dt>
   <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>

   </dl>

**Beispiel**:

  ```
  bx cs cluster-subnet-add mein_cluster 1643389
  ```
  {: pre}


### bx cs cluster-subnet-create CLUSTER GRÖSSE VLAN-ID [-s]
{: #cs_cluster_subnet_create}

Erstellung eines Teilnetzes in einem Konto von IBM Cloud Infrastructure (SoftLayer) und Zurverfügungstellung dieses Teilnetzes für einen angegebenen Cluster in {{site.data.keyword.containershort_notm}}.

**Hinweis:**
* Wenn Sie ein Teilnetz in einem Cluster verfügbar machen, werden IP-Adressen dieses Teilnetzes zum Zweck von Clusternetzen verwendet. Vermeiden Sie IP-Adresskonflikte, indem Sie ein Teilnetz mit nur einem Cluster verwenden. Verwenden Sie kein Teilnetz für mehrere Cluster oder für andere
Zwecke außerhalb von {{site.data.keyword.containershort_notm}} gleichzeitig.
* Um zwischen Teilnetzen in demselben VLAN weiterzuleiten, müssen Sie [VLAN-Spanning aktivieren](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning).

<strong>Befehlsoptionen</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Der Name oder die ID des Clusters. Dieser Wert ist erforderlich. Zum Auflisten Ihrer Cluster verwenden Sie den [Befehl](#cs_clusters) `bx cs clusters`.</dd>

   <dt><code><em>GRÖSSE</em></code></dt>
   <dd>Die Anzahl der IP-Teilnetzadressen. Dieser Wert ist erforderlich. Gültige Werte sind 8, 16, 32 oder 64.</dd>

   <dt><code><em>VLAN-ID</em></code></dt>
   <dd>Das VLAN, in dem das Teilnetz erstellt werden soll. Dieser Wert ist erforderlich. Verwenden Sie zum Auflisten der verfügbaren VLANs den [Befehl](#cs_vlans) `bx cs vlans <location>`. </dd>

   <dt><code>-s</code></dt>
   <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>

   </dl>

**Beispiel**:

  ```
  bx cs cluster-subnet-create mein_cluster 8 1764905
  ```
  {: pre}


### bx cs cluster-user-subnet-add CLUSTER TEILNETZ-CIDR PRIVATES_VLAN
{: #cs_cluster_user_subnet_add}

Verwenden Sie das eigene private Teilnetz in Ihren {{site.data.keyword.containershort_notm}}-Clustern.

Dieses private Teilnetz wird nicht von IBM Cloud Infrastructure (SoftLayer) bereitgestellt. Deshalb müssen Sie das gesamte Routing für ein- und ausgehenden Netzverkehr für das Teilnetz konfigurieren. Wenn Sie ein Teilnetz von IBM Cloud Infrastructure (SoftLayer) hinzufügen möchten, dann verwenden Sie den [Befehl](#cs_cluster_subnet_add) `bx cs cluster-subnet-add`.

**Hinweis**:
* Wenn Sie ein privates Benutzerteilnetz zu einem Cluster hinzufügen, werden die IP-Adressen dieses Teilnetzes für private Lastausgleichsfunktionen im Cluster verwendet. Vermeiden Sie IP-Adresskonflikte, indem Sie ein Teilnetz mit nur einem Cluster verwenden. Verwenden Sie kein Teilnetz für mehrere Cluster oder für andere
Zwecke außerhalb von {{site.data.keyword.containershort_notm}} gleichzeitig.
* Um zwischen Teilnetzen in demselben VLAN weiterzuleiten, müssen Sie [VLAN-Spanning aktivieren](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning).

<strong>Befehlsoptionen</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Der Name oder die ID des Clusters. Dieser Wert ist erforderlich.</dd>

   <dt><code><em>TEILNETZ_CIDR</em></code></dt>
   <dd>Das CIDR (Classless InterDomain Routing) des Teilnetzes. Dieser Wert ist erforderlich und darf keinen Konflikt mit einem anderen Teilnetz verursachen, das von IBM Cloud Infrastructure (SoftLayer) verwendet wird.

   Die unterstützten Präfixe liegen zwischen `/30` (1 IP-Adresse) und `/24` (253 IP-Adressen). Wenn Sie das CIDR mit einer Präfixlänge festlegen und später ändern müssen, dann sollten Sie zuerst das neue CIDR hinzufügen und anschließend das [alte CIDR entfernen](#cs_cluster_user_subnet_rm).</dd>

   <dt><code><em>PRIVATES_VLAN</em></code></dt>
   <dd>Die ID des privaten VLAN. Dieser Wert ist erforderlich. Er muss mit der ID des privaten VLAN für mindestens einen Workerknoten im Cluster übereinstimmen.</dd>
   </dl>

**Beispiel**:

  ```
  bx cs cluster-user-subnet-add mein_cluster 169.xx.xxx.xxx/29 1502175
  ```
  {: pre}


### bx cs cluster-user-subnet-rm CLUSTER TEILNETZ-CIDR PRIVATES_VLAN
{: #cs_cluster_user_subnet_rm}

Entfernen des eigenen privaten Teilnetzes aus einem angegebenen Cluster.

**Hinweis:** Jeder Service, der für eine IP-Adresse aus dem eigenen privaten Teilnetz bereitgestellt wurde, bleibt nach der Entfernung des Teilnetzes weiterhin aktiv.

<strong>Befehlsoptionen</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Der Name oder die ID des Clusters. Dieser Wert ist erforderlich.</dd>

   <dt><code><em>TEILNETZ_CIDR</em></code></dt>
   <dd>Das CIDR (Classless InterDomain Routing) des Teilnetzes. Dieser Wert ist erforderlich und muss mit dem CIDR übereinstimmen, das mit dem [Befehl](#cs_cluster_user_subnet_add) `bx cs cluster-user-subnet-add` festgelegt wurde.</dd>

   <dt><code><em>PRIVATES_VLAN</em></code></dt>
   <dd>Die ID des privaten VLAN. Dieser Wert ist erforderlich und muss mit der VLAN-ID übereinstimmen, die mit dem [Befehl](#cs_cluster_user_subnet_add) `bx cs cluster-user-subnet-add` festgelegt wurde.</dd>
   </dl>

**Beispiel**:

  ```
  bx cs cluster-user-subnet-rm mein_cluster 169.xx.xxx.xxx/29 1502175
  ```
  {: pre}

### bx cs subnets [--json][-s]
{: #cs_subnets}

Anzeigen einer Liste der Teilnetze, die in einem Konto von IBM Cloud Infrastructure (SoftLayer) verfügbar sind.

<strong>Befehlsoptionen</strong>:

  <dl>
  <dt><code>--json</code></dt>
  <dd>Druckt die Befehlsausgabe im JSON-Format. Dieser Wert ist optional.</dd>

  <dt><code>-s</code></dt>
  <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>
  </dl>

**Beispiel**:

  ```
  bx cs subnets
  ```
  {: pre}


<br />


## Ingress-Befehle für die Lastausgleichsfunktion für Anwendungen (ALB)
{: #alb_commands}

### bx cs alb-cert-deploy [--update] --cluster CLUSTER --secret-name NAME_DES_GEHEIMEN_SCHLÜSSELS --cert-crn CRN_DES_ZERTIFIKATS [-s]
{: #cs_alb_cert_deploy}

Stellen Sie für die Lastausgleichsfunktion für Anwendungen (ALB) in einem Cluster ein Zertifikat bereit oder aktualisieren Sie ein Zertifikat aus der verwendeten Instanz von {{site.data.keyword.cloudcerts_long_notm}}.

**Hinweis:**
* Dieser Befehl kann nur von Benutzern mit der Zugriffsrolle eines Administrators ausgeführt werden.
* Sie können lediglich Zertifikate aktualisieren, die aus derselben Instanz von {{site.data.keyword.cloudcerts_long_notm}} importiert werden.

<strong>Befehlsoptionen</strong>

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>Der Name oder die ID des Clusters. Dieser Wert ist erforderlich.</dd>

   <dt><code>--update</code></dt>
   <dd>Aktualisieren Sie das Zertifikat für einen geheimen Schlüssel der Lastausgleichsfunktion für Anwendungen (ALB) in einem Cluster. Dieser Wert ist optional.</dd>

   <dt><code>--secret-name <em>NAME_DES_GEHEIMEN_SCHLÜSSELS</em></code></dt>
   <dd>Der Name des geheimen Schlüssels der Lastausgleichsfunktion für Anwendungen (Application Load Balancer, ALB). Dieser Wert ist erforderlich.</dd>

   <dt><code>--cert-crn <em>CRN_DES_ZERTIFIKATS</em></code></dt>
   <dd>Die CRN des Zertifikats. Dieser Wert ist erforderlich.</dd>

   <dt><code>-s</code></dt>
   <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>
   </dl>

**Beispiele**:

Beispiel für die Bereitstellung eines geheimen ALB-Schlüssels:

   ```
   bx cs alb-cert-deploy --secret-name mein_geheimer_schlüssel_der_alb --cluster mein_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
   ```
   {: pre}

Beispiel für die Aktualisierung eines vorhandenen geheimen ALB-Schlüssels:

 ```
 bx cs alb-cert-deploy --update --secret-name mein_geheimer_schlüssel_der_alb --cluster mein_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:7e21fde8ee84a96d29240327daee3eb2
 ```
 {: pre}


### bx cs alb-cert-get --cluster CLUSTER [--secret-name NAME_DES_GEHEIMEN_SCHLÜSSELS][--cert-crn CERTIFICATE_CRN] [--json][-s]
{: #cs_alb_cert_get}

Informationen über einen geheimen ALB-Schlüssel in einem Cluster anzeigen.

**Hinweis:** Dieser Befehl kann nur von Benutzern mit der Zugriffsrolle eines Administrators ausgeführt werden.

<strong>Befehlsoptionen</strong>

  <dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
  <dd>Der Name oder die ID des Clusters. Dieser Wert ist erforderlich.</dd>

  <dt><code>--secret-name <em>NAME_DES_GEHEIMEN_SCHLÜSSELS</em></code></dt>
  <dd>Der Name des geheimen Schlüssels der Lastausgleichsfunktion für Anwendungen (Application Load Balancer, ALB). Dieser Wert ist erforderlich, um Informationen zu einem bestimmten geheimen ALB-Schlüssel im Cluster abzurufen.</dd>

  <dt><code>--cert-crn <em>CRN_DES_ZERTIFIKATS</em></code></dt>
  <dd>Die CRN des Zertifikats. Dieser Wert ist erforderlich, um Informationen zu allen geheimen ALB-Schlüsseln abzurufen, die mit der CRN eines bestimmten Zertifikats im Cluster übereinstimmen.</dd>

  <dt><code>--json</code></dt>
  <dd>Druckt die Befehlsausgabe im JSON-Format. Dieser Wert ist optional.</dd>

  <dt><code>-s</code></dt>
  <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>
  </dl>

**Beispiele**:

 Beispiel für das Abrufen von Informationen zu einem geheimen ALB-Schlüssel:

 ```
 bx cs alb-cert-get --cluster mein_cluster --secret-name mein_geheimer_schlüssel_der_alb
 ```
 {: pre}

 Beispiel für das Abrufen von Informationen zu allen geheimen ALB-Schlüsseln, die mit der CRN eines bestimmten Zertifikats übereinstimmen:

 ```
 bx cs alb-cert-get --cluster mein_cluster --cert-crn  crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
 ```
 {: pre}


### bx cs alb-cert-rm --cluster CLUSTER [--secret-name NAME_DES_GEHEIMEN_SCHLÜSSELS][--cert-crn CERTIFICATE_CRN] [-s]
{: #cs_alb_cert_rm}

Entfernen eines geheimen ALB-Schlüssels in einem Cluster.

**Hinweis:** Dieser Befehl kann nur von Benutzern mit der Zugriffsrolle eines Administrators ausgeführt werden.

<strong>Befehlsoptionen</strong>

  <dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
  <dd>Der Name oder die ID des Clusters. Dieser Wert ist erforderlich.</dd>

  <dt><code>--secret-name <em>NAME_DES_GEHEIMEN_SCHLÜSSELS</em></code></dt>
  <dd>Der Name des geheimen Schlüssels der Lastausgleichsfunktion für Anwendungen (Application Load Balancer, ALB). Dieser Wert ist erforderlich, um einen bestimmten geheimen ALB-Schlüssel im Cluster zu entfernen.</dd>

  <dt><code>--cert-crn <em>CRN_DES_ZERTIFIKATS</em></code></dt>
  <dd>Die CRN des Zertifikats. Dieser Wert ist erforderlich, um alle geheimen ALB-Schlüssel zu entfernen, die mit der CRN eines bestimmten Zertifikats im Cluster übereinstimmen.</dd>

  <dt><code>-s</code></dt>
  <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>

  </dl>

**Beispiele**:

 Beispiel für das Entfernen eines geheimen ALB-Schlüssels:

 ```
 bx cs alb-cert-rm --cluster mein_cluster --secret-name mein_geheimer_schlüssel_der_alb
 ```
 {: pre}

 Beispiel für das Entfernen aller geheimen ALB-Schlüssel, die mit der CRN eines bestimmten Zertifikats übereinstimmen:

 ```
 bx cs alb-cert-rm --cluster mein_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
 ```
 {: pre}


### bx cs alb-certs --cluster CLUSTER [--json][-s]
{: #cs_alb_certs}

Anzeigen einer Liste der geheimen ALB-Schlüssel in einem Cluster.

**Hinweis:** Dieser Befehl kann nur von Benutzern mit der Zugriffsrolle eines Administrators ausgeführt werden.

<strong>Befehlsoptionen</strong>

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>Der Name oder die ID des Clusters. Dieser Wert ist erforderlich.</dd>
   <dt><code>--json</code></dt>
   <dd>Druckt die Befehlsausgabe im JSON-Format. Dieser Wert ist optional.</dd>
   <dt><code>-s</code></dt>
   <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>
   </dl>

**Beispiel**:

 ```
 bx cs alb-certs --cluster mein_cluster
 ```
 {: pre}

### bx cs alb-configure --albID ALB-ID [--enable][--disable][--user-ip BENUTZER-IP][-s]
{: #cs_alb_configure}

Aktivieren oder Inaktivieren einer Lastausgleichsfunktion für Anwendungen (ALB) in Ihrem Standardcluster. Die öffentliche Lastausgleichsfunktion für Anwendungen (ALB) ist standardmäßig aktiviert.

**Befehlsoptionen**:

   <dl>
   <dt><code><em>--albID </em>ALB-ID</code></dt>
   <dd>Die ID für eine ALB. Führen Sie den Befehl <code>bx cs albs <em>--cluster </em>CLUSTER</code> aus, um die IDs für die ALBs in einem Cluster anzuzeigen. Dieser Wert ist erforderlich.</dd>

   <dt><code>--enable</code></dt>
   <dd>Schließen Sie dieses Flag ein, um eine ALB in einem Cluster zu aktivieren.</dd>

   <dt><code>--disable</code></dt>
   <dd>Schließen Sie dieses Flag ein, um eine ALB in einem Cluster zu inaktivieren.</dd>

   <dt><code>--user-ip <em>BENUTZER-IP</em></code></dt>
   <dd>

   <ul>
    <li>Dieser Parameter ist nur für die Aktivierung einer privaten Lastausgleichsfunktion für Anwendungen (ALB) verfügbar.</li>
    <li>Die private ALB wird mit einer IP-Adresse aus einem von einem Benutzer bereitgestellten privaten Teilnetz implementiert. Wird keine IP-Adresse bereitgestellt, wird die Lastausgleichsfunktion für Anwendungen (ALB) mit einer privaten IP-Adresse aus dem portierbaren, privaten Teilnetz bereitgestellt, die beim Erstellen des Clusters automatisch bereitgestellt wurde.</li>
   </ul>
   </dd>

   <dt><code>-s</code></dt>
   <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>

   </dl>

**Beispiele**:

  Beispiel für die Aktivierung einer ALB:

  ```
  bx cs alb-configure --albID private-cr18a61a63a6a94b658596aa93a087aaa9-alb1 --enable
  ```
  {: pre}

  Beispiel für die Aktivierung einer ALB mit einer von einem Benutzer bereitgestellten IP-Adresse:

  ```
  bx cs alb-configure --albID private-cr18a61a63a6a94b658596aa93a087aaa9-alb1 --enable --user-ip benutzer-ip
  ```
  {: pre}

  Beispiel für die Inaktivierung einer ALB:

  ```
  bx cs alb-configure --albID public-cr18a61a63a6a94b658596aa93a087aaa9-alb1 --disable
  ```
  {: pre}

### bx cs alb-get --albID ALB-ID [--json][-s]
{: #cs_alb_get}

Zeigen Sie die Details einer Lastausgleichsfunktion für Anwendungen (ALB) an.

<strong>Befehlsoptionen</strong>:

   <dl>
   <dt><code><em>--albID </em>ALB-ID</code></dt>
   <dd>Die ID für eine ALB. Führen Sie den Befehl <code>bx cs albs --cluster <em>CLUSTER</em></code> aus, um die IDs für die ALBs in einem Cluster anzuzeigen. Dieser Wert ist erforderlich.</dd>

   <dt><code>--json</code></dt>
   <dd>Druckt die Befehlsausgabe im JSON-Format. Dieser Wert ist optional.</dd>

  <dt><code>-s</code></dt>
  <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>

   </dl>

**Beispiel**:

  ```
  bx cs alb-get --albID public-cr18a61a63a6a94b658596aa93a087aaa9-alb1
  ```
  {: pre}

### bx cs alb-types [--json][-s]
{: #cs_alb_types}

Anzeigen der ALB-Typen, die in der Region unterstützt werden.

<strong>Befehlsoptionen</strong>:

  <dl>
  <dt><code>--json</code></dt>
  <dd>Druckt die Befehlsausgabe im JSON-Format. Dieser Wert ist optional.</dd>

  <dt><code>-s</code></dt>
  <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>
  </dl>

**Beispiel**:

  ```
  bx cs alb-types
  ```
  {: pre}


### bx cs albs --cluster CLUSTER [--json][-s]
{: #cs_albs}

Anzeigen des Status aller Lastausgleichsfunktionen für Anwendungen (ALBs) in einem Cluster. Wenn keine ALB-IDs zurückgegeben werden, verfügt der Cluster nicht über ein portierbares Teilnetz. Sie können Teilnetze [erstellen](#cs_cluster_subnet_create) oder zu einem Cluster [hinzufügen](#cs_cluster_subnet_add).

<strong>Befehlsoptionen</strong>:

   <dl>
   <dt><code><em>--cluster </em>CLUSTER</code></dt>
   <dd>Der Name oder die ID des Clusters, in dem Sie verfügbare Lastausgleichsfunktionen für Anwendungen (ALBs) auflisten. Dieser Wert ist erforderlich.</dd>

   <dt><code>--json</code></dt>
   <dd>Druckt die Befehlsausgabe im JSON-Format. Dieser Wert ist optional.</dd>

   <dt><code>-s</code></dt>
   <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>

   </dl>

**Beispiel**:

  ```
  bx cs albs --cluster mein_cluster
  ```
  {: pre}


<br />


## Befehle für die Infrastruktur
{: #infrastructure_commands}

### bx cs credentials-set --infrastructure-api-key API-SCHLÜSSEL --infrastructure-username BENUTZERNAME [-s]
{: #cs_credentials_set}

Festlegen von Berechtigungsnachweisen für das Konto von IBM Cloud Infrastructure (SoftLayer) für Ihr {{site.data.keyword.containershort_notm}}-Konto.

Wenn Sie über ein nutzungsabhängiges {{site.data.keyword.Bluemix_notm}}-Konto verfügen, haben Sie standardmäßig Zugriff auf das Portfolio der IBM Cloud-Infrastruktur. Es kann jedoch sein, dass Sie ein anderes IBM Cloud-Infrastrukturkonto (SoftLayer) verwenden möchten, das Sie bereits für die Infrastrukturbestellung verwenden. Sie können dieses Infrastrukturkonto über den folgenden Befehl mit Ihrem {{site.data.keyword.Bluemix_notm}}-Konto verknüpfen.

Falls die Berechtigungsnachweise für die IBM Cloud-Infrastruktur (SoftLayer) manuell definiert wurden, werden diese Berechtigungsnachweise für die Bestellung von Infrastruktur verwendet, selbst wenn bereits ein [IAM-API-Schlüssel](#cs_api_key_info) für das Konto vorhanden ist. Wenn der Benutzer, dessen Berechtigungsnachweise gespeichert wurden, nicht über die erforderlichen Berechtigungen zum Bestellen von Infrastruktur verfügt, können infrastrukturbezogene Aktionen, wie z. B. das Erstellen eines Clusters oder das erneute Laden eines Workerknotens, fehlschlagen.

Sie können nicht mehrere Berechtigungsnachweise für ein {{site.data.keyword.containershort_notm}}-Konto festlegen. Jedes {{site.data.keyword.containershort_notm}}-Konto ist nur mit einem Portfolio von IBM Cloud Infrastructure (SoftLayer) verbunden.

**Wichtig:** Stellen Sie vor Verwendung dieses Befehls sicher, dass der Benutzer, dessen Berechtigungsnachweise verwendet werden, über die erforderlichen Berechtigungen für [{{site.data.keyword.containershort_notm}} und IBM Cloud Infrastructure (SoftLayer) verfügt](cs_users.html#users).

<strong>Befehlsoptionen</strong>:

   <dl>
   <dt><code>--infrastructure-username <em>BENUTZERNAME</em></code></dt>
   <dd>Der Benutzername für ein Konto von IBM Cloud Infrastructure (SoftLayer). Dieser Wert ist erforderlich.</dd>


   <dt><code>--infrastructure-api-key <em>API-SCHLÜSSEL</em></code></dt>
   <dd>Der API-Schlüssel für ein Konto von IBM Cloud Infrastructure (SoftLayer). Dieser Wert ist erforderlich.

 <p>
  Gehen Sie wie folgt vor, um einen API-Schlüssel zu generieren:

  <ol>
  <li>Melden Sie sich beim [Portal von IBM Cloud Infrastructure (SoftLayer) ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/) an.</li>
  <li>Wählen Sie <strong>Konto</strong> und dann <strong>Benutzer</strong> aus.</li>
  <li>Klicken Sie auf <strong>Generieren</strong>, um einen API-Schlüssel von IBM Cloud Infrastructure (SoftLayer) für Ihr Konto zu generieren.</li>
  <li>Kopieren Sie den API-Schlüssel, um diesen Befehl verwenden zu können.</li>
  </ol>

  Gehen Sie wie folgt vor, um Ihren API-Schlüssel anzuzeigen:
  <ol>
  <li>Melden Sie sich beim [Portal von IBM Cloud Infrastructure (SoftLayer) ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/) an.</li>
  <li>Wählen Sie <strong>Konto</strong> und dann <strong>Benutzer</strong> aus.</li>
  <li>Klicken Sie auf <strong>Anzeigen</strong>, um den bestehenden API-Schlüssel anzuzeigen.</li>
  <li>Kopieren Sie den API-Schlüssel, um diesen Befehl verwenden zu können.</li>
  </ol>
  </p></dd>

  <dt><code>-s</code></dt>
  <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>

  </dl>

**Beispiel**:

  ```
  bx cs credentials-set --infrastructure-api-key <api-schlüssel> --infrastructure-username dbmanager
  ```
  {: pre}


### bx cs credentials-unset
{: #cs_credentials_unset}

Entfernen Sie die Kontoberechtigungsnachweise von IBM Cloud Infrastructure (SoftLayer) aus Ihrem {{site.data.keyword.containershort_notm}}-Konto.

Nachdem Sie die Berechtigungsnachweise entfernt haben, wird der [IAM-API-Schlüssel](#cs_api_key_info) verwendet, um Ressourcen in IBM Cloud Infrastructure (SoftLayer) zu bestellen.

<strong>Befehlsoptionen</strong>:

  <dl>
  <dt><code>-s</code></dt>
  <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>
  </dl>

**Beispiel**:

  ```
  bx cs credentials-unset
  ```
  {: pre}


### bx cs machine-types STANDORT [--json][-s]
{: #cs_machine_types}

Anzeige einer Liste der für Ihre Workerknoten verfügbaren Maschinentypen. Maschinentypen variieren je nach Standort. Jeder Maschinentyp enthält die Menge an virtueller CPU, an Hauptspeicher und an Plattenspeicher für jeden Workerknoten im Cluster. Das Verzeichnis `/var/lib/docker`, in dem alle Containerdaten gespeichert sind, ist mit der LUKS-Verschlüsselung verschlüsselt. Wenn die Option `disable-disk-encrypt` während der Clustererstellung eingeschlossen wird, werden die Docker-Daten des Hosts nicht verschlüsselt. [Weitere Informationen zur Verschlüsselung](cs_secure.html#encrypted_disks)
{:shortdesc}

Sie können Ihre Workerknoten als virtuelle Maschine auf gemeinsam genutzter oder dedizierter Hardware bereitstellen oder als physische Maschine auf Bare-Metal-Systemen.

<dl>
<dt>Warum sollte ich physische Maschinen (Bare-Metal) verwenden?</dt>
<dd><p><strong>Mehr Berechnungsressourcen</strong>: Sie können Ihre Workerknoten als physischen Single-Tenant Server bereitstellen, der auch als Bare-Metal-Server bezeichnet wird. Mit Bare-Metal haben Sie direkten Zugriff auf die physischen Ressourcen auf der Maschine, wie z. B. Speicher oder CPU. Durch diese Konfiguration wird der Hypervisor der virtuellen Maschine entfernt, der physische Ressourcen zu virtuellen Maschinen zuordnet, die auf dem Host ausgeführt werden. Stattdessen sind alle Ressourcen der Bare-Metal-Maschine ausschließlich dem Worker gewidmet, also müssen Sie sich keine Sorgen machen, dass "lärmende Nachbarn" Ressourcen gemeinsam nutzen oder die Leistung verlangsamen. Physische Maschinentypen verfügen über einen größeren lokalen Speicher als virtuelle und einige verfügen zudem über RAID für die Sicherung lokaler Daten.</p>
<p><strong>Monatliche Abrechnung</strong>: Die Nutzung von Bare-Metal-Servern ist teurer als die Nutzung virtueller Server. Bare-Metal-Server eignen sich für Hochleistungsanwendungen, die mehr Ressourcen und mehr Hoststeuerung benötigen. Die Abrechnung für Bare-Metal-Server erfolgt monatlich. Wenn Sie einen Bare-Metal-Server vor Monatsende stornieren, werden Ihnen die Kosten bis zum Ende dieses Monats belastet. Das Buchen und Stornieren von Bare-Metal-Servern ist ein manueller Prozess in Ihrem IBM Cloud Infrastructure-Konto (SoftLayer). Er kann länger als einen Geschäftstag dauern.</p>
<p><strong>Option zum Aktivieren von Trusted Compute</strong>: Aktivieren Sie Trusted Compute, um Ihre Workerknoten auf Manipulation zu überprüfen. Wenn Sie Trusted Compute während der Clustererstellung nicht aktivieren, dies jedoch später nachholen möchten, können Sie den [Befehl](cs_cli_reference.html#cs_cluster_feature_enable) `bx cs feature-enable` verwenden. Nachdem Sie Trusted Compute aktiviert haben, können Sie es später nicht mehr inaktivieren. Sie können einen neuen Cluster ohne Trusted Compute erstellen. Weitere Informationen zur Funktionsweise von Trusted Compute während des Startprozesses für den Knoten finden Sie in [{{site.data.keyword.containershort_notm}} mit Trusted Compute](cs_secure.html#trusted_compute). Trusted Compute ist nur auf Clustern mit Kubernetes Version 1.9 oder höher verfügbar, die bestimmte Bare-Metal-Maschinentypen aufweisen. Bei der Ausführung des[ Befehls](cs_cli_reference.html#cs_machine_types) `bx cs machine-types <location>` können Sie im Feld **Trustable** ablesen, welche Maschinen Trusted Compute unterstützen. Beispielsweise unterstützen die GPU-Typen `mgXc` Trusted Compute nicht.</p></dd>
<dt>Warum sollte ich virtuelle Maschinen verwenden?</dt>
<dd><p>Virtuelle Maschinen bieten eine größere Flexibilität, schnellere Bereitstellungszeiten und mehr automatische Skalierbarkeitsfunktionen als Bare-Metal-Maschinen und sind zudem kostengünstiger. Sie können virtuelle Maschinen für die meisten allgemeinen Anwendungsfälle, wie Test- und Entwicklungsumgebungen, Staging- und Produktionsumgebungen, Mikroservices und Business-Apps, verwenden. Allerdings müssen bei der Leistung Kompromisse gemacht werden. Wenn Sie für RAM-, GPU- oder datenintensive Arbeitslasten eine Datenverarbeitung mit hoher Leistung benötigen, verwenden Sie Bare-Metal-Maschinen.</p>
<p><strong>Auswahl zwischen Single-Tenant- oder Multi-Tenant-Konfiguration</strong>: Wenn Sie einen virtuellen Standardcluster erstellen, müssen Sie auswählen, ob die zugrunde liegende Hardware von mehreren {{site.data.keyword.IBM_notm}} Kunden gemeinsam genutzt werden kann (Multi-Tenant-Konfiguration) oder ob Sie die ausschließlich Ihnen vorbehaltene, dedizierte Nutzung vorziehen (Single-Tenant-Konfiguration).</p>
<p>Bei einer Multi-Tenant-Konfiguration werden physische Ressourcen wie CPU und Speicher von allen virtuellen Maschinen, die auf derselben physischen Hardware bereitgestellt wurden, gemeinsam genutzt. Um sicherzustellen, dass jede virtuelle Maschine unabhängig von anderen Maschinen ausgeführt werden kann, segmentiert ein VM-Monitor, d. h. eine Überwachungsfunktion für virtuelle Maschinen, die auch als Hypervisor bezeichnet wird, die physischen Ressourcen in isolierte Entitäten und ordnet diese einer virtuellen Maschine als dedizierte Ressourcen zu. Dies wird als Hypervisor-Isolation bezeichnet.</p>
<p>Bei einer Single-Tenant-Konfiguration ist die Nutzung aller physischen Ressourcen ausschließlich Ihnen vorbehalten. Sie können mehrere Workerknoten als virtuelle Maschinen auf demselben physischen Host bereitstellen. Ähnlich wie bei der Multi-Tenant-Konfiguration stellt der Hypervisor auch hier sicher, dass jeder Workerknoten seinen Anteil an den verfügbaren physischen Ressourcen erhält.</p>
<p>Gemeinsam genutzte Knoten sind in der Regel kostengünstiger als dedizierte Knoten, weil die Kosten für die ihnen zugrunde liegende Hardware von mehreren Kunden getragen werden. Bei der Entscheidungsfindung hinsichtlich gemeinsam genutzter Knoten versus dedizierter Knoten sollten Sie mit Ihrer Rechtsabteilung Rücksprache halten, um zu klären, welcher Grad an Infrastrukturisolation und Compliance für Ihre App-Umgebung erforderlich ist.</p>
<p><strong>Virtuelle `u2c`- oder `b2c`-Maschinentypen</strong>: Diese Maschinen verwenden lokale Platten anstelle von SAN (Storage Area Networking) für die Zuverlässigkeit. Zu den Vorteilen zählen ein höherer Durchsatz beim Serialisieren von Bytes für die lokale Festplatte und weniger Beeinträchtigungen des Dateisystems aufgrund von Netzausfällen. Diese Maschinentypen weisen 25 GB primären lokalen Plattenspeicher für das Dateisystem des Betriebssystems auf und 100 GB sekundären lokalen Plattenspeicher für das Verzeichnis `/var/lib/docker`, in das alle Containerdaten geschrieben werden.</p>
<p><strong>Was passiert, wenn ich über veraltete `u1c`- oder `b1c`-Maschinentypen verfüge?</strong> Zu Beginn der Verwendung von `u2c`- und `b2c`-Maschinentypen [aktualisieren Sie die Maschinentypen durch Hinzufügen von Workerknoten](cs_cluster_update.html#machine_type).</p></dd>
<dt>Welche Typen von virtuellen und physischen Maschinen stehen zur Auswahl?</dt>
<dd><p>Viele! Wählen Sie den Maschinentyp aus, der am besten für Ihren Anwendungsfall geeignet ist. Denken Sie daran, dass sich ein Worker-Pool aus Maschinen desselben Typs zusammensetzt. Wenn Sie mit einer Mischung aus Maschinentypen in Ihrem Cluster arbeiten möchten, erstellen Sie für jeden Typ einen eigenen Worker-Pool.</p>
<p>Maschinentypen variieren je nach Zone. Um die in Ihrer Zone verfügbaren Maschinentypen anzuzeigen, führen Sie den Befehl `bx cs machine-types <zone_name>` aus.</p>
<p><table>
<caption>Verfügbare physische (Bare-Metal) und virtuelle Maschinentypen in {{site.data.keyword.containershort_notm}}.</caption>
<thead>
<th>Name und Anwendungsfall</th>
<th>Kerne/Speicher</th>
<th>Primäre/Sekundäre Platte</th>
<th>Netzgeschwindigkeit</th>
</thead>
<tbody>
<tr>
<td><strong>Virtuell, u2c.2x4</strong>: Verwenden Sie diese kompakte virtuelle Maschine für Schnelltests, Machbarkeitsnachweise und weitere leichte Workloads.</td>
<td>2/4 GB</td>
<td>25 GB/100 GB</td>
<td>1000 MB/s</td>
</tr>
<tr>
<td><strong>Virtuell, b2c.4x16</strong>: Wählen Sie diese ausgeglichene virtuelle Maschine für Tests und Entwicklung und weitere leichte Workloads.</td>
<td>4/16 GB</td>
<td>25 GB/100 GB</td>
<td>1000 MB/s</td>
</tr>
<tr>
<td><strong>Virtuell, b2c.16x64</strong>: Wählen Sie diese ausgeglichene virtuelle Maschine für mittlere Workloads aus.</td></td>
<td>16/64 GB</td>
<td>25 GB/100 GB</td>
<td>1000 MB/s</td>
</tr>
<tr>
<td><strong>Virtuell, b2c.32x128</strong>: Wählen Sie diese ausgeglichene virtuelle Maschine für mittlere bis große Workloads, wie eine Datenbank und eine dynamische Website mit vielen gleichzeitigen Benutzern, aus.</td></td>
<td>32/128 GB</td>
<td>25 GB/100 GB</td>
<td>1000 MB/s</td>
</tr>
<tr>
<td><strong>Virtuell, b2c.56x242</strong>: Wählen Sie diese ausgeglichene virtuelle Maschine für große Workloads, wie eine Datenbank und mehrere Apps mit vielen gleichzeitigen Benutzern, aus.</td></td>
<td>56/242 GB</td>
<td>25 GB/100 GB</td>
<td>1000 MB/s</td>
</tr>
<tr>
<td><strong>RAM-intensiv, Bare-Metal, mr1c.28x512</strong>: Maximieren Sie den verfügbaren RAM-Speicher für Ihre Workerknoten.</td>
<td>28/512 GB</td>
<td>2 TB SATA/960 GB SSD</td>
<td>10000 MB/s</td>
</tr>
<tr>
<td><strong>GPU, Bare-Metal, mg1c.16x128</strong>: Wählen Sie diesen Typ für rechenintensive Workloads, wie Datenverarbeitungen mit hoher Leistung, maschinelles Lernen oder 3D-Anwendungen, aus. Dieser Typ verfügt über eine physische Tesla K80-Karte mit zwei GPUs (Graphics Processing Units) pro Karte für insgesamt zwei GPUs.</td>
<td>16/128 GB</td>
<td>2 TB SATA/960 GB SSD</td>
<td>10000 MB/s</td>
</tr>
<tr>
<td><strong>GPU, Bare-Metal, mg1c.28x256</strong>: Wählen Sie diesen Typ für rechenintensive Workloads, wie Datenverarbeitungen mit hoher Leistung, maschinelles Lernen oder 3D-Anwendungen, aus. Dieser Typ verfügt über zwei physische Tesla K80-Karten, die zwei GPUs (Graphics Processing Units) pro Karte für insgesamt vier GPUs aufweisen.</td>
<td>28/256 GB</td>
<td>2 TB SATA/960 GB SSD</td>
<td>10000 MB/s</td>
</tr>
<tr>
<td><strong>Datenintensiv, Bare-Metal, md1c.16x64.4x4tb</strong>: Wählen Sie diesen Typ aus, wenn Sie eine bedeutende Menge an lokalem Festplattenspeicherplatz, einschließlich RAID, für die Sicherung der lokal auf der Maschine gespeicherten Daten benötigen. Dieser Typ eignet sich für Anwendungsfälle wie verteilte Dateisysteme, große Datenbanken und Workloads für Big-Data-Analysen.</td>
<td>16/64 GB</td>
<td>2 x 2 TB RAID1/4 x 4 TB SATA RAID10</td>
<td>10000 MB/s</td>
</tr>
<tr>
<td><strong>Datenintensiv, Bare-Metal, md1c.28x512.4x4tb</strong>: Wählen Sie diesen Typ aus, wenn Sie eine bedeutende Menge an lokalem Festplattenspeicherplatz, einschließlich RAID, für die Sicherung der lokal auf der Maschine gespeicherten Daten benötigen. Dieser Typ eignet sich für Anwendungsfälle wie verteilte Dateisysteme, große Datenbanken und Workloads für Big-Data-Analysen.</td>
<td>28/512 GB</td>
<td>2 x 2 TB RAID1/4 x 4 TB SATA RAID10</td>
<td>10000 MB/s</td>
</tr>
<tr>
<td><strong>Ausgeglichen, Bare-Metal, mb1c.4x32</strong>: Verwenden Sie diesen Typ für ausgeglichene Workloads, für die mehr Ressourcen für die Datenverarbeitung benötigt werden, als virtuelle Maschinen bieten können.</td>
<td>4/32 GB</td>
<td>2 TB SATA/2 TB SATA</td>
<td>10000 MB/s</td>
</tr>
<tr>
<td><strong>Ausgeglichen, Bare-Metal, mb1c.16x64</strong>: Verwenden Sie diesen Typ für ausgeglichene Workloads, für die mehr Ressourcen für die Datenverarbeitung benötigt werden, als virtuelle Maschinen bieten können.</td>
<td>16/64 GB</td>
<td>2 TB SATA/960 GB SSD</td>
<td>10000 MB/s</td>
</tr>
</tbody>
</table>
</p>
</dd>
</dl>


<strong>Befehlsoptionen</strong>:

   <dl>
   <dt><code><em>STANDORT</em></code></dt>
   <dd>Geben Sie den Standort ein, an dem Sie verfügbare Maschinentypen auflisten möchten. Dieser Wert ist erforderlich. Überprüfen Sie die [verfügbaren Standorte](cs_regions.html#locations).</dd>

   <dt><code>--json</code></dt>
  <dd>Druckt die Befehlsausgabe im JSON-Format. Dieser Wert ist optional.</dd>

  <dt><code>-s</code></dt>
  <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>
  </dl>

**Beispielbefehl**:

  ```
  bx cs machine-types dal10
  ```
  {: pre}

**Beispielausgabe**:

  ```
  Getting machine types list...
  OK
  Machine Types
  Name                 Cores   Memory   Network Speed   OS             Server Type   Storage   Secondary Storage   Trustable
  u2c.2x4              2       4GB      1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  b2c.4x16             4       16GB     1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  b2c.16x64            16      64GB     1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  b2c.32x128           32      128GB    1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  b2c.56x242           56      242GB    1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  mb1c.4x32            4       32GB     10000Mbps       UBUNTU_16_64   physical      1000GB    2000GB              False
  mb1c.16x64           16      64GB     10000Mbps       UBUNTU_16_64   physical      1000GB    1700GB              False
  mr1c.28x512          28      512GB    10000Mbps       UBUNTU_16_64   physical      1000GB    1700GB              False
  md1c.16x64.4x4tb     16      64GB     10000Mbps       UBUNTU_16_64   physical      1000GB    8000GB              False
  md1c.28x512.4x4tb    28      512GB    10000Mbps       UBUNTU_16_64   physical      1000GB    8000GB              False
  
  ```
  {: screen}


### bx cs vlans STANDORT [--all][--json] [-s]
{: #cs_vlans}

Auflisten der öffentlichen und der privaten VLANs, die für einen Standort in Ihrem Konto von IBM Cloud Infrastructure (SoftLayer) zur Verfügung stehen. Um verfügbare VLANs auflisten zu können, müssen Sie über ein gebührenpflichtiges Konto verfügen.

<strong>Befehlsoptionen</strong>:

   <dl>
   <dt><code><em>STANDORT</em></code></dt>
   <dd>Geben Sie den Standort ein, an dem Sie Ihre privaten und öffentlichen VLANs auflisten möchten. Dieser Wert ist erforderlich. Überprüfen Sie die [verfügbaren Standorte](cs_regions.html#locations).</dd>

   <dt><code>--all</code></dt>
   <dd>Listet alle verfügbaren VLANs auf. VLANs werden standardmäßig gefiltert, um nur diejenigen anzuzeigen, die gültig sind. Damit ein VLAN gültig ist, muss es einer Infrastruktur zugeordnet sein, die einen Worker mit lokalem Plattenspeicher hosten kann.</dd>

   <dt><code>--json</code></dt>
  <dd>Druckt die Befehlsausgabe im JSON-Format. Dieser Wert ist optional.</dd>

  <dt><code>-s</code></dt>
  <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>
   </dl>

**Beispiel**:

  ```
  bx cs vlans dal10
  ```
  {: pre}


<br />


## Protokollierungsbefehle
{: #logging_commands}

### bx cs logging-config-create CLUSTER --logsource PROTOKOLLQUELLE [--namespace KUBERNETES-NAMENSBEREICH][--hostname LOG_SERVER_HOSTNAME_OR_IP] [--port PROTOKOLLSERVER-PORT][--space CLUSTER_SPACE] [--org CLUSTERORG][--app-containers CONTAINERS] [--app-paths PROTOKOLLPFAD][--syslog-protocol PROTOCOL] --type PROTOKOLLTYP [--json][--skip-validation] [-s]
{: #cs_logging_create}

Erstellen Sie eine Protokollierungskonfiguration. Sie können diesen Befehl verwenden, um Protokolle für Container, Anwendungen, Workerknoten, Kubernetes-Cluster und Ingress-Lastausgleichsfunktionen für Anwendungen an {{site.data.keyword.loganalysisshort_notm}} oder an einen externen Systemprotokollserver weiterzuleiten.

<strong>Befehlsoptionen</strong>:

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>Der Name oder die ID des Clusters.</dd>

  <dt><code>--logsource <em>PROTOKOLLQUELLE</em></code></dt>    
    <dd>Die Protokollquelle, für die die Protokollweiterleitung aktiviert werden soll. Dieses Argument unterstützt eine durch Kommas getrennte Liste mit Protokollquellen, auf die die Konfiguration angewendet werden soll. Gültige Werte sind <code>container</code>, <code>application</code>, <code>worker</code>, <code>kubernetes</code>, <code>ingress</code> und <code>kube-audit</code>. Wenn Sie keine Protokollquelle bereitstellen, werden Protokollkonfigurationen für die Protokollquellen <code>container</code> und <code>ingress</code> erstellt.</dd>

  <dt><code>--namespace <em>KUBERNETES-NAMENSBEREICH</em></code></dt>
    <dd>Der Kubernetes-Namensbereich, von dem aus Protokolle weitergeleitet werden sollen. Die Weiterleitung von Protokollen wird für die Kubernetes-Namensbereiche <code>ibm-system</code> und <code>kube-system</code> nicht unterstützt. Dieser Wert ist nur für die Containerprotokollquelle gültig und optional. Wenn Sie keinen Namensbereich angeben, verwenden alle Namensbereiche im Cluster diese Konfiguration.</dd>

  <dt><code>--hostname <em>PROTOKOLLSERVER-HOSTNAME</em></code></dt>
    <dd>Wenn der Protokollierungstyp <code>syslog</code> lautet, der Hostname oder die IP-Adresse des Protokollcollector-Servers. Dieser Wert ist für <code>syslog</code> erforderlich. Wenn der Protokollierungstyp <code>ibm</code> lautet, die {{site.data.keyword.loganalysislong_notm}}-Einpflege-URL. Sie finden die Liste von verfügbaren Einpflege-URLs [hier](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls). Wenn Sie keine Einpflege-URL angeben, wird der Endpunkt für die Region, in der Ihr Cluster erstellt wurde, verwendet.</dd>

  <dt><code>--port <em>PROTOKOLLSERVER-PORT</em></code></dt>
    <dd>Der Port des Protokollcollector-Servers. Dieser Wert ist optional. Wenn Sie keinen Port angeben, wird der Standardport <code>514</code> für <code>syslog</code> und der Standardport <code>9091</code> für <code>ibm</code> verwendet.</dd>

  <dt><code>--space <em>CLUSTERBEREICH</em></code></dt>
    <dd>Der Name des Cloud Foundry-Bereichs, an den Protokolle gesendet werden sollen. Dieser Wert ist nur für den Protokolltyp <code>ibm</code> gültig und optional. Wenn Sie keinen Bereich angeben, werden Protokolle an die Kontoebene gesendet.</dd>

  <dt><code>--org <em>CLUSTERORG</em></code></dt>
    <dd>Der Name der Cloud Foundry-Organisation, in der sich der Bereich befindet. Dieser Wert ist nur für den Protokolltyp <code>ibm</code> gültig und erforderlich, wenn Sie einen Bereich angegeben haben.</dd>

  <dt><code>--app-paths</code></dt>
    <dd>Der Pfad zu dem Container, an den die Apps Protokolle senden. Wenn Sie Protokolle mit dem Quellentyp <code>application</code> weiterleiten möchten, müssen Sie einen Pfad angeben. Wenn Sie mehr als einen Pfad angeben, verwenden Sie eine durch Kommas getrennte Liste. Dieser Wert ist für <code>application</code> der Protokollquelle erforderlich. Beispiel: <code>/var/log/myApp1/&ast;,/var/log/myApp2/&ast;</code></dd>

  <dt><code>--syslog-protocol</code></dt>
    <dd>Das Netzprotokoll (Transportschicht), das verwendet wird, wenn der Protokollierungstyp <code>syslog</code> lautet. Unterstützte Werte sind <code>TCP</code> und der Standardwert <code>UDP</code>. Bei einer Weiterleitung an einen rsyslog-Server mit dem <code>udp</code>-Protokoll werden Protokolle, die größer sind als 1 KB, abgeschnitten.</dd>

  <dt><code>--type <em>PROTOKOLLTYP</em></code></dt>
    <dd>Gibt an, wohin Sie Ihre Protokolle weiterleiten möchten. Optionen sind <code>ibm</code>, wodurch Ihre Protokolle an {{site.data.keyword.loganalysisshort_notm}} weitergeleitet werden, und <code>syslog</code>, wodurch Ihre Protokolle an externe Server weitergeleitet werden.</dd>

  <dt><code>--app-containers</code></dt>
    <dd>Zum Weiterleiten von Protokollen von Apps können Sie den Namen des Containers angeben, der Ihre App enthält. Sie können mehr als einen Container angeben, indem Sie eine durch Kommas getrennte Liste verwenden. Falls keine Container angegeben sind, werden Protokolle von allen Containern weitergeleitet, die die von Ihnen bereitgestellten Pfade enthalten. Diese Option ist nur für <code>application</code> der Protokollquelle gültig.</dd>

  <dt><code>--json</code></dt>
    <dd>Druckt die Befehlsausgabe im JSON-Format. Dieser Wert ist optional.</dd>

  <dt><code>--skip-validation</code></dt>
    <dd>Überspringt die Validierung der Organisations- und Bereichsnamen, wenn sie angegeben werden. Das Überspringen der Validierung verringert die Bearbeitungszeit. Eine ungültige Protokollierungskonfiguration aber führt dazu, dass Protokolle nicht ordnungsgemäß weitergeleitet werden. Dieser Wert ist optional.</dd>

    <dt><code>-s</code></dt>
    <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>
</dl>

**Beispiele**:

Beispiel für Protokolltyp `ibm`, der aus einer `container`-Protokollquelle am Standardport 9091 weiterleitet:

  ```
  bx cs logging-config-create mein_cluster --logsource container --namespace mein_namensbereich --hostname ingest.logging.ng.bluemix.net --type ibm
  ```
  {: pre}

Beispiel für Protokolltyp `syslog` für Weiterleitung aus der Protokollquelle `container` am Standardport 514:

  ```
  bx cs logging-config-create mein_cluster --logsource container --namespace mein_namensbereich  --hostname 169.xx.xxx.xxx --type syslog
  ```
  {: pre}

Beispiel für Protokolltyp `syslog`, der Protokolle aus einer `ingress`-Quelle an einen anderen Port als den Standardport weiterleitet:

  ```
  bx cs logging-config-create mein_cluster --logsource container --hostname 169.xx.xxx.xxx --port 5514 --type syslog
  ```
  {: pre}

### bx cs logging-config-get CLUSTER [--logsource PROTOKOLLQUELLE][--json] [-s]
{: #cs_logging_get}

Zeigen Sie alle Protokollweiterleitungskonfigurationen für einen Cluster an oder filtern Sie die Protokollierungskonfigurationen auf Basis der Protokollquelle.

<strong>Befehlsoptionen</strong>:

 <dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>Der Name oder die ID des Clusters. Dieser Wert ist erforderlich.</dd>

  <dt><code>--logsource <em>PROTOKOLLQUELLE</em></code></dt>
    <dd>Die Art der Protokollquelle, für die die Filterung durchgeführt werden soll. Nur Protokollierungskonfigurationen dieser Protokollquelle im Cluster werden zurückgegeben. Gültige Werte sind <code>container</code>, <code>application</code>, <code>worker</code>, <code>kubernetes</code> <code>ingress</code> und <code>kube-audit</code>. Dieser Wert ist optional.</dd>

  <dt><code>--show-covering-filters</code></dt>
    <dd>Zeigt die Protokollierungsfilter an, wodurch die vorherige Filter obsolet werden.</dd>

  <dt><code>--json</code></dt>
    <dd>Druckt die Befehlsausgabe im JSON-Format. Dieser Wert ist optional.</dd>

  <dt><code>-s</code></dt>
    <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>
 </dl>

**Beispiel**:

  ```
  bx cs logging-config-get mein_cluster --logsource worker
  ```
  {: pre}


### bx cs logging-config-refresh CLUSTER [-s]
{: #cs_logging_refresh}

Aktualisierung der Protokollierungskonfiguration für den Cluster. Dadurch wird das Protokollierungstoken für alle Protokollierungskonfigurationen aktualisiert, bei denen die Weiterleitung an die Bereichsebene in Ihrem Cluster erfolgt.

<strong>Befehlsoptionen</strong>:

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
   <dd>Der Name oder die ID des Clusters. Dieser Wert ist erforderlich.</dd>

   <dt><code>-s</code></dt>
     <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>
</dl>

**Beispiel**:

  ```
  bx cs logging-config-refresh mein_cluster
  ```
  {: pre}


### bx cs logging-config-rm CLUSTER [--id PROTOKOLLKONFIGURATIONS-ID][--all] [-s]
{: #cs_logging_rm}

Löschen einer Protokollweiterleitungskonfiguration oder aller Protokollierungskonfigurationen für einen Cluster. Dadurch wird die Protokollweiterleitung an einen fernen Systemprotokollserver bzw. an {{site.data.keyword.loganalysisshort_notm}} gestoppt.

<strong>Befehlsoptionen</strong>:

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
   <dd>Der Name oder die ID des Clusters. Dieser Wert ist erforderlich.</dd>

  <dt><code>--id <em>PROTOKOLLKONFIGURATIONS-ID</em></code></dt>
   <dd>Wenn Sie eine einzelne Protokollierungskonfiguration entfernen möchten, die ID der Protokollierungskonfiguration.</dd>

  <dt><code>--all</code></dt>
   <dd>Das Flag zum Entfernen aller Protokollierungskonfigurationen in einem Cluster.</dd>

   <dt><code>-s</code></dt>
     <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>
</dl>

**Beispiel**:

  ```
  bx cs logging-config-rm mein_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e
  ```
  {: pre}


### bx cs logging-config-update CLUSTER --id PROTOKOLLKONFIGURATIONS-ID [--namespace NAMENSBEREICH][--hostname LOG_SERVER_HOSTNAME_OR_IP] [--port LOG_SERVER_PORT][--space CLUSTER_SPACE] [--org CLUSTER_ORG][--app-paths PATH] [--app-containers PATH] --type LOG_TYPE [--json][--skipValidation] [-s]
{: #cs_logging_update}

Aktualisieren Sie die Details einer Protokollweiterleitungskonfiguration.

<strong>Befehlsoptionen</strong>:

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
   <dd>Der Name oder die ID des Clusters. Dieser Wert ist erforderlich.</dd>

  <dt><code>--id <em>PROTOKOLLKONFIGURATIONS-ID</em></code></dt>
   <dd>Die ID der Protokollierungskonfiguration, die aktualisiert werden soll. Dieser Wert ist erforderlich.</dd>

  <dt><code>--namespace <em>NAMENSBEREICH</em></code>
    <dd>Der Kubernetes-Namensbereich, von dem aus Protokolle weitergeleitet werden sollen. Die Weiterleitung von Protokollen wird für die Kubernetes-Namensbereiche <code>ibm-system</code> und <code>kube-system</code> nicht unterstützt. Dieser Wert ist nur für die Protokollquelle <code>container</code> gültig. Wenn Sie keinen Namensbereich angeben, verwenden alle Namensbereiche im Cluster diese Konfiguration.</dd>

  <dt><code>--hostname <em>PROTOKOLLSERVER-HOSTNAME</em></code></dt>
   <dd>Wenn der Protokollierungstyp <code>syslog</code> lautet, der Hostname oder die IP-Adresse des Protokollcollector-Servers. Dieser Wert ist für <code>syslog</code> erforderlich. Wenn der Protokollierungstyp <code>ibm</code> lautet, die {{site.data.keyword.loganalysislong_notm}}-Einpflege-URL. Sie finden die Liste von verfügbaren Einpflege-URLs [hier](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls). Wenn Sie keine Einpflege-URL angeben, wird der Endpunkt für die Region, in der Ihr Cluster erstellt wurde, verwendet.</dd>

   <dt><code>--port <em>PROTOKOLLSERVER-PORT</em></code></dt>
   <dd>Der Port des Protokollcollector-Servers. Dieser Wert ist optional, wenn der Protokollierungstyp <code>syslog</code> lautet. Wenn Sie keinen Port angeben, wird der Standardport <code>514</code> für <code>syslog</code> und <code>9091</code> für <code>ibm</code> verwendet.</dd>

   <dt><code>--space <em>CLUSTERBEREICH</em></code></dt>
   <dd>Der Name des Bereichs, an den Protokolle gesendet werden sollen. Dieser Wert ist nur für den Protokolltyp <code>ibm</code> gültig und optional. Wenn Sie keinen Bereich angeben, werden Protokolle an die Kontoebene gesendet.</dd>

   <dt><code>--org <em>CLUSTERORG</em></code></dt>
   <dd>Der Name der Organisation, in der sich der Bereich befindet. Dieser Wert ist nur für den Protokolltyp <code>ibm</code> gültig und erforderlich, wenn Sie einen Bereich angegeben haben.</dd>

   <dt><code>--app-paths <em>PFAD</em>,<em>PFAD</em></code></dt>
     <dd>Ein absoluter Dateipfad in dem Container, aus dem Protokolle erfasst werden. Platzhalter, wie '/var/log/*.log', können zwar verwendet werden, nicht jedoch rekursive Globale, wie '/var/log/**/test.log'. Wenn Sie mehr als einen Pfad angeben, verwenden Sie eine durch Kommas getrennte Liste. Dieser Wert ist erforderlich, wenn Sie 'application' für die Protokollquelle angeben. </dd>

   <dt><code>--app-containers <em>PFAD</em>,<em>PFAD</em></code></dt>
     <dd>Der Pfad für die Container, in die die Apps die Protokolle schreiben. Wenn Sie Protokolle mit dem Quellentyp <code>application</code> weiterleiten möchten, müssen Sie einen Pfad angeben. Wenn Sie mehr als einen Pfad angeben, verwenden Sie eine durch Kommas getrennte Liste. Beispiel: <code>/var/log/myApp1/&ast;,/var/log/myApp2/&ast;</code></dd>

   <dt><code>--type <em>PROTOKOLLTYP</em></code></dt>
   <dd>Das Protokollweiterleitungsprotokoll, das Sie verwenden möchten. Momentan werden <code>syslog</code> und <code>ibm</code> unterstützt. Dieser Wert ist erforderlich.</dd>

   <dt><code>--json</code></dt>
   <dd>Druckt die Befehlsausgabe im JSON-Format. Dieser Wert ist optional.</dd>

   <dt><code>--skipValidation</code></dt>
   <dd>Überspringt die Validierung der Organisations- und Bereichsnamen, wenn sie angegeben werden. Das Überspringen der Validierung verringert die Bearbeitungszeit. Eine ungültige Protokollierungskonfiguration aber führt dazu, dass Protokolle nicht ordnungsgemäß weitergeleitet werden. Dieser Wert ist optional.</dd>

   <dt><code>-s</code></dt>
     <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>
     </dl>

**Beispiel für Protokolltyp `ibm`**:

  ```
  bx cs logging-config-update mein_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e --type ibm
  ```
  {: pre}

**Beispiel für Protokolltyp `syslog`**:

  ```
  bx cs logging-config-update mein_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e --hostname localhost --port 5514 --type syslog
  ```
  {: pre}


### bx cs logging-filter-create CLUSTER --type PROTOKOLLTYP [--logging-configs KONFIGURATIONEN][--namespace KUBERNETES_NAMESPACE] [--container CONTAINER_NAME][--level LOGGING_LEVEL] [--regex-message MESSAGE][--json] [-s]
{: #cs_log_filter_create}

Erstellen Sie einen Protokollierungsfilter. Mit diesem Befehl können Sie Protokolle herausfiltern, die von Ihrer Protokollierungskonfiguration weitergeleitet werden.

<strong>Befehlsoptionen</strong>:

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>Der Name oder die ID des Clusters, für das Sie einen Protokollierungsfilter erstellen möchten. Dieser Wert ist erforderlich.</dd>

  <dt><code>--type <em>PROTOKOLLTYP</em></code></dt>
    <dd>Der Typ von Protokollen, auf die Sie den Filter anwenden möchten. Momentan werden <code>all</code>, <code>container</code> und <code>host</code> unterstützt.</dd>

  <dt><code>--logging-configs <em>KONFIGURATIONEN</em></code></dt>
    <dd>Eine durch Kommas getrennte Liste Ihrer Protokollierungskonfigurations-IDs. Wird sie nicht bereitgestellt, wird der Filter auf alle Clusterprotokollierungskonfigurationen angewendet, die an den Filter übergeben werden. Sie können mit dem Filter übereinstimmende Protokollkonfigurationen anzeigen, indem Sie das Flag <code>--show-matching-configs</code> mit dem Befehl verwenden. Dieser Wert ist optional.</dd>

  <dt><code>--namespace <em>KUBERNETES-NAMENSBEREICH</em></code></dt>
    <dd>Der Kubernetes-Namensbereich, in dem Sie Protokolle filtern möchten. Dieser Wert ist optional.</dd>

  <dt><code>--container <em>CONTAINERNAME</em></code></dt>
    <dd>Der Name des Containers, in dem Sie Protokolle filtern möchten. Dieses Flag ist nur mit dem Protokolltyp <code>container</code> anwendbar. Dieser Wert ist optional.</dd>

  <dt><code>--level <em>PROTOKOLLIERUNGSSTUFE</em></code></dt>
    <dd>Filtert Protokolle auf einer angegebenen Stufe oder den darunterliegenden Stufen heraus. Zulässige Werte in ihrer kanonischen Reihenfolge sind <code>fatal</code>, <code>error</code>, <code>warn/warning</code>, <code>info</code>, <code>debug</code> und <code>trace</code>. Dieser Wert ist optional. Beispiel: Wenn Sie Protokolle auf der Stufe <code>info</code> filtern, wird auch auf den Stufen <code>debug</code> und <code>trace</code> gefiltert. **Hinweis**: Sie können dieses Flag nur dann verwenden, wenn Protokollnachrichten das JSON-Format aufweisen und ein Feld für die Stufe enthalten. Beispielausgabe: <code>{"log": "hello", "level": "info"}</code></dd>

  <dt><code>--regex-message <em>NACHRICHT</em></code></dt>
    <dd>Optional: Filtert alle Protokolle heraus, die eine angegebene Nachricht enthalten, die als regulärer Ausdruck an einer beliebigen Stelle im Protokoll geschrieben wird. Dieser Wert ist optional.</dd>

  <dt><code>--json</code></dt>
    <dd>Druckt die Befehlsausgabe im JSON-Format. Dieser Wert ist optional.</dd>

  <dt><code>-s</code></dt>
    <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>
</dl>

**Beispiele**:

In diesem Beispiel werden alle Protokolle herausgefiltert, die von Containern mit dem Namen `test-container` im Standardnamensbereich weitergeleitet werden, die sich auf der Stufe 'debug' oder einer niedrigeren Stufe befinden und eine Protokollnachricht haben, die 'GET request' enthält.

  ```
  bx cs logging-filter-create example-cluster --type container --namespace default --container test-container --level debug --message "GET request"
  ```
  {: pre}

In diesem Beispiel werden alle Protokolle herausgefiltert, die von einem bestimmten Cluster auf der Stufe 'info' oder einer niedrigeren Stufe weitergeleitet werden. Die Ausgabe wird als JSON zurückgegeben.

  ```
  bx cs logging-filter-create example-cluster --type all --level info --json
  ```
  {: pre}



### bx cs logging-filter-get CLUSTER [--id FILTER-ID][--show-matching-configs] [--show-covering-filters][--json] [-s]
{: #cs_log_filter_view}

Zeigen Sie eine Protokollierungsfilterkonfiguration an. Mit diesem Befehl können Sie die selbst erstellten Protokollierungsfilter anzeigen.

<strong>Befehlsoptionen</strong>:

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>Der Name oder die ID des Clusters, für das Sie Filter anzeigen möchten. Dieser Wert ist erforderlich.</dd>

  <dt><code>--id <em>FILTER-ID</em></code></dt>
    <dd>Die ID des Protokollfilters, den Sie anzeigen möchten.</dd>

  <dt><code>--show-matching-configs</code></dt>
    <dd>Zeigt die Protokollierungskonfigurationen an, die mit der überprüften Konfiguration übereinstimmen. Dieser Wert ist optional.</dd>

  <dt><code>--show-covering-filters</code></dt>
    <dd>Zeigt die Protokollierungsfilter an, wodurch die vorherigen Filter obsolet werden. Dieser Wert ist optional.</dd>

  <dt><code>--json</code></dt>
    <dd>Druckt die Befehlsausgabe im JSON-Format. Dieser Wert ist optional.</dd>

   <dt><code>-s</code></dt>
     <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>
</dl>


### bx cs logging-filter-rm CLUSTER [--id FILTER-ID][--all] [-s]
{: #cs_log_filter_delete}

Löschen Sie einen Protokollierungsfilter. Mit diesem Befehl können Sie einen selbst erstellten Protokollierungsfilter entfernen.

<strong>Befehlsoptionen</strong>:

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>Der Name oder die ID des Clusters, aus dem Sie einen Protokollierungsfilter löschen möchten.</dd>

  <dt><code>--id <em>FILTER-ID</em></code></dt>
    <dd>Die ID des Protokollfilters, der gelöscht werden soll. </dd>

  <dt><code>--all</code></dt>
    <dd>Löscht alle Protokollweiterleitungsfilter. Dieser Wert ist optional.</dd>

  <dt><code>-s</code></dt>
    <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>
</dl>


### bx cs logging-filter-update CLUSTER --id FILTER-ID --type PROTOKOLLTYP [--logging-configs KONFIGURATIONEN][--namespace KUBERNETES_NAMESPACE] [--container CONTAINERNAME][--level LOGGING_LEVEL] [--message NACHRICHT][--json] [-s]
{: #cs_log_filter_update}

Aktualisieren Sie einen Protokollierungsfilter. Mit diesem Befehl können Sie einen selbst erstellten Protokollierungsfilter aktualisieren.

<strong>Befehlsoptionen</strong>:

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>Der Name oder die ID des Clusters, für den Sie einen Protokollierungsfilter aktualisieren möchten. Dieser Wert ist erforderlich.</dd>

 <dt><code>--id <em>FILTER-ID</em></code></dt>
    <dd>Die ID des Protokollfilters, der aktualisiert werden soll.</dd>

  <dt><code>--type <em>PROTOKOLLTYP</em></code></dt>
    <dd>Der Typ von Protokollen, auf die Sie den Filter anwenden möchten. Momentan werden <code>all</code>, <code>container</code> und <code>host</code> unterstützt.</dd>

  <dt><code>--logging-configs <em>KONFIGURATIONEN</em></code></dt>
    <dd>Eine durch Kommas getrennte Liste Ihrer Protokollierungskonfigurations-IDs. Wird sie nicht bereitgestellt, wird der Filter auf alle Clusterprotokollierungskonfigurationen angewendet, die an den Filter übergeben werden. Sie können mit dem Filter übereinstimmende Protokollkonfigurationen anzeigen, indem Sie das Flag <code>--show-matching-configs</code> mit dem Befehl verwenden. Dieser Wert ist optional.</dd>

  <dt><code>--namespace <em>KUBERNETES-NAMENSBEREICH</em></code></dt>
    <dd>Der Kubernetes-Namensbereich, in dem Sie Protokolle filtern möchten. Dieser Wert ist optional.</dd>

  <dt><code>--container <em>CONTAINERNAME</em></code></dt>
    <dd>Der Name des Containers, in dem Sie Protokolle filtern möchten. Dieses Flag ist nur mit dem Protokolltyp <code>container</code> anwendbar. Dieser Wert ist optional.</dd>

  <dt><code>--level <em>PROTOKOLLIERUNGSSTUFE</em></code></dt>
    <dd>Filtert Protokolle auf einer angegebenen Stufe oder den darunterliegenden Stufen heraus. Zulässige Werte in ihrer kanonischen Reihenfolge sind <code>fatal</code>, <code>error</code>, <code>warn/warning</code>, <code>info</code>, <code>debug</code> und <code>trace</code>. Dieser Wert ist optional. Beispiel: Wenn Sie Protokolle auf der Stufe <code>info</code> filtern, wird auch auf den Stufen <code>debug</code> und <code>trace</code> gefiltert. **Hinweis**: Sie können dieses Flag nur dann verwenden, wenn Protokollnachrichten das JSON-Format aufweisen und ein Feld für die Stufe enthalten. Beispielausgabe: <code>{"log": "hello", "level": "info"}</code></dd>

  <dt><code>--message <em>NACHRICHT</em></code></dt>
    <dd>Filtert alle Protokolle mit einer bestimmten Nachricht heraus. Die Nachricht wird buchstabengetreu abgeglichen und nicht als Ausdruck. Beispiel: Die Nachrichten “Hello”, “!”und “Hello, World!”würden im Protokoll “Hello, World!” wiedergefunden. Dieser Wert ist optional.</dd>

  <dt><code>--json</code></dt>
    <dd>Druckt die Befehlsausgabe im JSON-Format. Dieser Wert ist optional.</dd>

  <dt><code>-s</code></dt>
    <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>
</dl>




<br />


## Regionsbefehle
{: #region_commands}

### bx cs locations [--json][-s]
{: #cs_datacenters}

Zeigen Sie eine Liste von verfügbaren Standorten an, in denen Sie einen Cluster erstellen können. Welche Standorte verfügbar sind, hängt von der Region ab, bei der Sie angemeldet sind. Um zwischen Regionen zu wechseln, führen Sie den Befehl `bx cs region-set` aus.

<strong>Befehlsoptionen</strong>:

   <dl>
   <dt><code>--json</code></dt>
   <dd>Druckt die Befehlsausgabe im JSON-Format. Dieser Wert ist optional.</dd>

   <dt><code>-s</code></dt>
   <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>
   </dl>

**Beispiel**:

  ```
  bx cs locations
  ```
  {: pre}


### bx cs region
{: #cs_region}

Suchen Sie nach der {{site.data.keyword.containershort_notm}}-Region, in der Sie sich aktuell befinden. Sie erstellen und verwalten Cluster, die für diese Region spezifisch sind. Verwenden Sie den Befehl `bx cs region-set`, um die Region zu ändern.

**Beispiel**:

```
bx cs region
```
{: pre}

**Ausgabe**:
```
Region: us-south
```
{: screen}

### bx cs region-set [REGION]
{: #cs_region-set}

Legen Sie die Region für {{site.data.keyword.containershort_notm}} fest. Sie erstellen und verwalten Cluster, die für die Region spezifisch sind, und benötigen zum Zwecke der Hochverfügbarkeit unter Umständen Cluster in mehreren Regionen.

Sie können sich beispielsweise bei {{site.data.keyword.Bluemix_notm}} in der Region 'Vereinigte Staaten (Süden)' anmelden und einen Cluster erstellen. Anschließend können Sie `bx cs region-set eu-central` verwenden, um die Region 'Zentraleuropa' als Ziel festzulegen, und einen weiteren Cluster erstellen. Und schließlich können Sie `bx cs region-set us-south` verwenden, um zur Region 'Vereinigte Staaten (Süden)' zurückzukehren und Ihren Cluster in dieser Region zu verwalten.

**Befehlsoptionen**:

<dl>
<dt><code><em>REGION</em></code></dt>
<dd>Geben Sie die Region ein, die Sie als Ziel festlegen möchten. Dieser Wert ist optional. Wenn Sie die Region nicht angeben, können Sie sie aus der Liste in der Ausgabe auswählen.

Eine Liste von verfügbaren Regionen finden Sie unter [Regionen und Standorte](cs_regions.html). Oder setzen Sie den [Befehl](#cs_regions) `bx cs regions` ab.</dd></dl>

**Beispiel**:

```
bx cs region-set eu-central
```
{: pre}

```
bx cs region-set
```
{: pre}

**Ausgabe**:
```
Choose a region:
1. ap-north
2. ap-south
3. eu-central
4. uk-south
5. us-east
6. us-south
Enter a number> 3
OK
```
{: screen}

### bx cs regions
{: #cs_regions}

Listet die verfügbaren Regionen auf. Der `Region Name` ist der {{site.data.keyword.containershort_notm}}-Name und der `Region Alias` ist der allgemeine {{site.data.keyword.Bluemix_notm}}-Name für die Region.

**Beispiel**:

```
bx cs regions
```
{: pre}

**Ausgabe**:
```
Region Name   Region Alias
ap-north      jp-tok
ap-south      au-syd
eu-central    eu-de
uk-south      eu-gb
us-east       us-east
us-south      us-south
```
{: screen}


<br />


## Befehle für Workerknoten
{: worker_node_commands}


### bx cs worker-add --cluster CLUSTER [--file DATEISTANDORT][--hardware HARDWARE] --machine-type MASCHINENTYP --number NUMMER --private-vlan PRIVATES_VLAN --public-vlan ÖFFENTLICHES_VLAN [--disable-disk-encrypt][-s]
{: #cs_worker_add}

Fügen Sie Ihrem Standardcluster Workerknoten hinzu.



<strong>Befehlsoptionen</strong>:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Der Name oder die ID des Clusters. Dieser Wert ist erforderlich.</dd>

<dt><code>--file <em>DATEISTANDORT</em></code></dt>
<dd>Der Pfad zur YAML-Datei für das Hinzufügen von Workerknoten zum Cluster. Statt zusätzliche Workerknoten mithilfe der in diesem Befehl bereitgestellten Optionen zu definieren, können Sie eine YAML-Datei verwenden. Dieser Wert ist optional.

<p><strong>Hinweis:</strong> Wenn Sie dieselbe Option wie im Befehl als Parameter in der YAML-Datei bereitstellen, hat der Wert im Befehl Vorrang vor dem Wert in der YAML. Beispiel: Sie definieren einen Maschinentyp in Ihrer YAML-Datei und verwenden die Option '--machine-type' im Befehl. Der Wert, den Sie in der Befehlsoption eingegeben haben, überschreibt den Wert in der YAML-Datei.

<pre class="codeblock">
<code>name: <em>&lt;clustername_oder_-id&gt;</em>
location: <em>&lt;standort&gt;</em>
machine-type: <em>&lt;maschinentyp&gt;</em>
private-vlan: <em>&lt;privates_vlan&gt;</em>
public-vlan: <em>&lt;öffentliches_vlan&gt;</em>
hardware: <em>&lt;shared_oder_dedicated&gt;</em>
workerNum: <em>&lt;anzahl_worker&gt;</em>
diskEncryption: <em>false</em></code></pre>

<table>
<caption>Erklärung der Komponenten der YAML-Datei</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der YAML-Dateikomponenten</th>
</thead>
<tbody>
<tr>
<td><code><em>name</em></code></td>
<td>Ersetzen Sie <code><em>&lt;clustername_oder_-id&gt;</em></code> durch den Namen oder die ID des Clusters, in dem Sie Workerknoten hinzufügen möchten.</td>
</tr>
<tr>
<td><code><em>standort</em></code></td>
<td>Ersetzen Sie <code><em>&lt;standort&gt;</em></code> durch den Standort, an dem Sie Ihre Workerknoten bereitstellen möchten. Welche Standorte verfügbar sind, hängt von der Region ab, bei der Sie angemeldet sind. Führen Sie den Befehl <code>bx cs locations</code> aus, um die verfügbaren Standorte aufzulisten.</td>
</tr>
<tr>
<td><code><em>maschinentyp</em></code></td>
<td>Ersetzen Sie <code><em>&lt;maschinentyp&gt;</em></code> durch den Maschinentyp, auf dem Sie Ihre Workerknoten bereitstellen möchten. Sie können Ihre Workerknoten als virtuelle Maschinen auf gemeinsam genutzter oder dedizierter Hardware oder als physische Maschinen auf Bare-Metal-Systemen bereitstellen. Die verfügbaren physischen und virtuellen Maschinentypen variieren je nach dem Standort, an dem Sie den Cluster bereitstellen. Weitere Informationen finden Sie im [Befehl](cs_cli_reference.html#cs_machine_types) `bx cs machine-types`.</td>
</tr>
<tr>
<td><code><em>privates_vlan</em></code></td>
<td>Ersetzen Sie <code><em>&lt;privates_vlan&gt;</em></code> durch die ID des privaten VLANs, das Sie für Ihre Workerknoten verwenden möchten. Führen Sie <code>bx cs vlans <em>&lt;standort&gt;</em></code> aus und suchen Sie nach VLAN-Routern, die mit <code>bcr</code> (Back-End-Router) beginnen, um verfügbare VLANs aufzulisten.</td>
</tr>
<tr>
<td><code>public-vlan</code></td>
<td>Ersetzen Sie <code>&lt;öffentliches_vlan&gt;</code> durch die ID des öffentlichen VLANs, das Sie für Ihre Workerknoten verwenden möchten. Führen Sie <code>bx cs vlans &lt;standort&gt;</code> aus und suchen Sie nach VLAN-Routern, die mit <code>fcr</code> (Front-End-Router) beginnen, um verfügbare VLANs aufzulisten. <br><strong>Hinweis</strong>: {[privates_vlan_vyatta]}</td>
</tr>
<tr>
<td><code>hardware</code></td>
<td>Für virtuelle Maschinentypen: Der Grad an Hardware-Isolation für Ihren Workerknoten. Verwenden Sie 'dedicated', wenn
Sie verfügbare physische Ressourcen haben möchten, die nur Sie nutzen können, oder 'shared', um zuzulassen, dass
physische Ressourcen mit anderen IBM Kunden gemeinsam genutzt werden können. Die Standardeinstellung ist
'shared'.</td>
</tr>
<tr>
<td><code>anzahl_worker</code></td>
<td>Ersetzen Sie <code><em>&lt;anzahl_worker&gt;</em></code> durch die Anzahl von Workerknoten, die Sie implementieren wollen.</td>
</tr>
<tr>
<td><code>diskEncryption: <em>false</em></code></td>
<td>Workerknoten weisen standardmäßig Verschlüsselung auf. [Weitere Informationen finden Sie hier](cs_secure.html#worker). Um die Verschlüsselung zu inaktivieren, schließen Sie diese Option ein und legen Sie den Wert auf <code>false</code> fest.</td></tr>
</tbody></table></p></dd>

<dt><code>--hardware <em>HARDWARE</em></code></dt>
<dd>Der Grad an Hardware-Isolation für Ihren Workerknoten. Verwenden Sie 'dedicated', wenn
Sie verfügbare physische Ressourcen haben möchten, die nur Sie nutzen können, oder 'shared', um zuzulassen, dass
physische Ressourcen mit anderen IBM Kunden gemeinsam genutzt werden können. Die Standardeinstellung ist
'shared'. Dieser Wert ist optional.</dd>

<dt><code>--machine-type <em>MASCHINENTYP</em></code></dt>
<dd>Wählen Sie einen Maschinentyp aus. Sie können Ihre Workerknoten als virtuelle Maschinen auf gemeinsam genutzter oder dedizierter Hardware oder als physische Maschinen auf Bare-Metal-Systemen bereitstellen. Die verfügbaren physischen und virtuellen Maschinentypen variieren je nach dem Standort, an dem Sie den Cluster bereitstellen. Weitere Informationen finden Sie in der Dokumentation zum [Befehl](cs_cli_reference.html#cs_machine_types) `bx cs machine-types`. Dieser Wert ist für Standardcluster erforderlich und steht für kostenlose Cluster nicht zur Verfügung.</dd>

<dt><code>--number <em>ANZAHL</em></code></dt>
<dd>Die durch eine Ganzzahl angegebene Anzahl der Workerknoten, die im Cluster erstellt werden sollen. Der Standardwert ist 1. Dieser Wert ist optional.</dd>

<dt><code>--private-vlan <em>PRIVATES_VLAN</em></code></dt>
<dd>Das private VLAN, das bei der Erstellung des Clusters angegeben wurde. Dieser Wert ist erforderlich.

<p><strong>Hinweis:</strong> Private VLAN-Router beginnen immer mit <code>bcr</code> (Back-End-Router) und öffentliche VLAN-Router immer mit <code>fcr</code> (Front-End-Router). Wenn Sie einen Cluster erstellen und die öffentlichen und privaten VLANs angeben, müssen die Zahlen- und Buchstabenkombinationen nach diesen Präfixen übereinstimmen.</p></dd>

<dt><code>--public-vlan <em>ÖFFENTLICHES_VLAN</em></code></dt>
<dd>Das öffentliche VLAN, das bei der Erstellung des Clusters angegeben wurde. Dieser Wert ist optional. Wenn Ihre Workerknoten ausschließlich in einem privaten VLAN bereitgestellt werden sollen, geben Sie keine öffentliche VLAN-ID an. <strong>Hinweis</strong>: {[privates_vlan_vyatta]}

<p><strong>Hinweis:</strong> Private VLAN-Router beginnen immer mit <code>bcr</code> (Back-End-Router) und öffentliche VLAN-Router immer mit <code>fcr</code> (Front-End-Router). Wenn Sie einen Cluster erstellen und die öffentlichen und privaten VLANs angeben, müssen die Zahlen- und Buchstabenkombinationen nach diesen Präfixen übereinstimmen.</p></dd>

<dt><code>--disable-disk-encrypt</code></dt>
<dd>Workerknoten weisen standardmäßig eine Verschlüsselung auf. [Weitere Informationen finden Sie hier](cs_secure.html#worker). Wenn Sie die Verschlüsselung inaktivieren möchten, schließen Sie diese Option ein.</dd>

<dt><code>-s</code></dt>
<dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>

</dl>

**Beispiele**:

  ```
  bx cs worker-add --cluster mein_cluster --number 3 --public-vlan meine_öffentliche_vlan-id --private-vlan meine_private_vlan-id --machine-type u2c.2x4 --hardware shared
  ```
  {: pre}

  Beispiel für {{site.data.keyword.Bluemix_dedicated_notm}}:

  ```
  bx cs worker-add --cluster mein_cluster --number 3 --maschinentyp u2c.2x4
  ```
  {: pre}

 Dort wird der Cluster bereitgestellt. Weitere Informationen finden Sie in der Dokumentation zum [Befehl](cs_cli_reference.html#cs_machine_types) `bx cs machine-types`. Dieser Wert ist für Standardcluster erforderlich und steht für kostenlose Cluster nicht zur Verfügung.</dd>

  <dt><code>--size-per-zone <em>WORKER_PRO_ZONE</em></code></dt>
    <dd>Die Anzahl der Worker, die in jeder Zone erstellt werden sollen. Dieser Wert ist erforderlich.</dd>

  <dt><code>--kube-version <em>VERSION</em></code></dt>
    <dd>Die Version von Kubernetes, in der die Workerknoten erstellt werden sollen. Falls dieser Wert nicht angegeben ist, wird die Standardversion verwendet.</dd>

  <dt><code>--hardware <em>HARDWARE</em></code></dt>
    <dd>Der Grad an Hardware-Isolation für Ihren Workerknoten. Verwenden Sie 'dedicated', wenn
Sie verfügbare physische Ressourcen haben möchten, die nur Sie nutzen können, oder 'shared', um zuzulassen, dass
physische Ressourcen mit anderen IBM Kunden gemeinsam genutzt werden können. Die Standardeinstellung ist
'shared'. Dieser Wert ist optional.</dd>

  <dt><code>--labels <em>BEZEICHNUNGEN</em></code></dt>
    <dd>Die Bezeichnungen, die Sie den Workern im Pool zuweisen möchten. Beispiel: <schlüssel1>=<wert1>,<schlüssel2>=<wert2></dd>

  <dt><code>--private-only </code></dt>
    <dd>Gibt an, dass es im Worker-Pool keine öffentlichen VLANS gibt. Der Standardwert ist <code>false</code>.</dd>

  <dt><code>--diable-disk-encrpyt</code></dt>
    <dd>Gibt an, dass die Platte nicht verschlüsselt ist. Der Standardwert ist <code>false</code>.</dd>

</dl>

**Beispielbefehl**:

  ```
  bx cs worker-pool-add mein_cluster --machine-type u2c.2x4 --size-per-zone 6
  ```
  {: pre}

### bx cs worker-pools --cluster CLUSTER
{: #cs_worker_pools}

Zeigen Sie die Worker-Pools in einem Cluster an.

<strong>Befehlsoptionen</strong>:

<dl>
  <dt><code>--cluster <em>CLUSTERNAME_ODER_-ID</em></code></dt>
    <dd>Der Name oder die ID des Clusters, für den Worker-Pools aufgelistet werden sollen. Dieser Wert ist erforderlich.</dd>
</dl>

**Beispielbefehl**:

  ```
  bx cs worker-pools --cluster mein_cluster
  ```
  {: pre}

### bx cs worker-pool-get --worker-pool WORKER-POOL --cluster CLUSTER
{: #cs_worker_pool_get}

Zeigen Sie die Details eines Worker-Pools an.

<strong>Befehlsoptionen</strong>:

<dl>
  <dt><code>--worker-pool <em>WORKER-POOL</em></code></dt>
    <dd>Der Name des Workerknoten-Pools, für den die Details angezeigt werden sollen. Dieser Wert ist erforderlich.</dd>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>Der Name oder die ID des Clusters, in dem sich der Worker-Pool befindet. Dieser Wert ist erforderlich.</dd>
</dl>

**Beispielbefehl**:

  ```
  bx cs worker-pool-get --worker-pool pool1 --cluster mein_cluster
  ```
  {: pre}

### bx cs worker-pool-update --worker-pool WORKER-POOL --cluster CLUSTER
{: #cs_worker_pool_update}

Aktualisieren Sie alle Workerknoten im Pool auf die aktuelle Kubernetes-Version, die dem angegebenen Master entspricht.

<strong>Befehlsoptionen</strong>:

<dl>
  <dt><code>--worker-pool <em>WORKER-POOL</em></code></dt>
    <dd>Der Name des Workerknoten-Pools, der aktualisiert werden soll. Dieser Wert ist erforderlich.</dd>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>Der Name oder die ID des Clusters, für den Worker-Pools aktualisiert werden sollen. Dieser Wert ist erforderlich.</dd>
</dl>

**Beispielbefehl**:

  ```
  bx cs worker-pool-update --worker-pool pool1 --cluster mein_cluster
  ```
  {: pre}



### bx cs worker-pool-resize --worker-pool WORKER-POOL --cluster CLUSTER --size-per-zone WORKER_PRO_ZONE
{: #cs_worker_pool_resize}

Ändern Sie die Größe des Worker-Pools, um die Anzahl der Workerknoten zu erhöhen oder zu verringern, die sich in den einzelnen Zonen des Clusters befinden.

<strong>Befehlsoptionen</strong>:

<dl>
  <dt><code>--worker-pool <em>WORKER-POOL</em></code></dt>
    <dd>Der Name des Workerknoten-Pools, der aktualisiert werden soll. Dieser Wert ist erforderlich.</dd>

  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>Der Name oder die ID des Clusters, für den die Größe von Worker-Pools geändert werden soll. Dieser Wert ist erforderlich.</dd>

  <dt><code>--size-per-zone <em>WORKER_PRO_ZONE</em></code></dt>
    <dd>Die Anzahl der Worker, die in jeder Zone erstellt werden sollen. Dieser Wert ist erforderlich.</dd>
</dl>

**Beispielbefehl**:

  ```
  bx cs worker-pool-update --cluster mein_cluster --worker-pool pool1,pool2 --size-per-zone 3
  ```
  {: pre}

### bx cs worker-pool-rm --worker-pool WORKER-POOL --cluster CLUSTER
{: #cs_worker_pool_rm}

Entfernen Sie einen Worker-Pool aus dem Cluster. Alle Workerknoten im Pool werden gelöscht. Die Pods werden beim Löschvorgang neu terminiert. Um Ausfallzeiten zu vermeiden, stellen Sie sicher, dass Sie über genügend Worker zum Ausführen Ihrer Workload verfügen.

<strong>Befehlsoptionen</strong>:

<dl>
  <dt><code>--worker-pool <em>WORKER-POOL</em></code></dt>
    <dd>Der Name des Workerknoten-Pools, der entfernt werden soll. Dieser Wert ist erforderlich.</dd>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
    <dd>Der Name oder die ID des Clusters, aus dem der Worker-Pool entfernt werden soll. Dieser Wert ist erforderlich.</dd>
</dl>

**Beispielbefehl**:

  ```
  bx cs worker-pool-rm --cluster mein_cluster --worker-pool pool1
  ```
  {: pre}

</staging>

### bx cs worker-get [CLUSTERNAME_ODER_-ID] WORKERKNOTEN-ID [--json][-s]
{: #cs_worker_get}

Zeigen Sie die Details eines Workerknotens an.

<strong>Befehlsoptionen</strong>:

   <dl>
   <dt><code><em>CLUSTERNAME_ODER_-ID</em></code></dt>
   <dd>Der Name oder die ID des Workerknotenclusters. Dieser Wert ist optional.</dd>

   <dt><code><em>WORKERKNOTEN-ID</em></code></dt>
   <dd>Der Name Ihres Workerknotens. Führen Sie den Befehl <code>bx cs workers <em>CLUSTER</em></code> aus, um die IDs für die Workerknoten in einem Cluster anzuzeigen. Dieser Wert ist erforderlich.</dd>

   <dt><code>--json</code></dt>
   <dd>Druckt die Befehlsausgabe im JSON-Format. Dieser Wert ist optional.</dd>

   <dt><code>-s</code></dt>
   <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>
   </dl>

**Beispielbefehl**:

  ```
  bx cs worker-get mein_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1
  ```
  {: pre}

**Beispielausgabe**:

  ```
  ID:           kube-dal10-123456789-w1
  State:        normal
  Status:       Ready
  Trust:        disabled
  Private VLAN: 223xxxx
  Public VLAN:  223xxxx
  Private IP:   10.xxx.xx.xxx
  Public IP:    169.xx.xxx.xxx
  Hardware:     shared
  Zone:         dal10
  Version:      1.8.11_1509
  ```
  {: screen}

### bx cs worker-reboot [-f][--hard] CLUSTER WORKER [WORKER][-s]
{: #cs_worker_reboot}

Führen Sie einen Warmstart eines Workerknotens in einem Cluster durch. Während des Warmstarts ändert sich der Status Ihres Workerknotens nicht.

**Achtung:** Der Warmstart eines Workerknotens kann zu Datenverlust auf dem Workerknoten führen. Verwenden Sie diesen Befehl mit Bedacht und wenn Sie wissen, dass ein Warmstart die Wiederherstellung Ihres Workerknotens unterstützen kann. In allen anderen Fällen sollten Sie stattdessen [Ihren Workerknoten erneut laden](#cs_worker_reload).

Stellen Sie, bevor Sie einen Warmstart für Ihren Workerknoten durchführen, sicher, dass die Pods erneut auf anderen Workerknoten geplant werden, um Ausfallzeiten für Ihre App oder Datenverlust auf Ihrem Workerknoten zu vermeiden.

1. Listen Sie die Workerknoten in Ihrem Cluster auf und notieren Sie sich den **Namen** des Workerknotens, für den Sie einen Warmstart durchführen möchten.
   ```
   kubectl get nodes
   ```
   Der **Name**, der in diesem Befehl zurückgegeben wird, ist die private IP-Adresse, die Ihrem Workerknoten zugewiesen ist. Weitere Informationen zu Ihrem Workerknoten finden Sie, wenn Sie den Befehl `bx cs workers <cluster_name_or_ID>` ausführen und nach dem Workerknoten mit derselben **privaten IP**-Adresse suchen.
2. Markieren Sie den Workerknoten in einem Prozess, der als Abriegelung oder "Cordoning" bezeichnet wird, als nicht planbar ("unschedulable"). Wenn Sie einen Workerknoten abriegeln, ist er für die künftige Pod-Planung nicht mehr verfügbar. Verwenden Sie den **Namen** des Workerknotens, den Sie im vorherigen Schritt erhalten haben.
   ```
   kubectl cordon <workername>
   ```
   {: pre}

3. Überprüfen Sie, ob die Pod-Planung für Ihren Workerknoten inaktiviert ist
   ```
   kubectl get nodes
   ```
   {: pre}
   Ihr Workerknoten ist für die Pod-Planung inaktiviert, wenn der Status **SchedulingDisabled** angezeigt wird.
 4. Pods müssen aus Ihrem Workerknoten entfernt und auf den verbleibenden Workerknoten im Cluster erneut geplant werden.
    ```
    kubectl drain <workername>
    ```
    {: pre}
    Dieser Prozess kann einige Minuten dauern.
 5. Führen Sie einen Warmstart für den Workerknoten durch. Verwenden Sie die Worker-ID, die vom Befehl `bx cs workers <cluster_name_or_ID>` zurückgegeben wird.
    ```
    bx cs worker-reboot <clustername_oder_-id> <workername_oder_-id>
    ```
    {: pre}
 6. Warten Sie ungefähr 5 Minuten, bevor Sie Ihre Workerknoten für die Pod-Planung zur Verfügung stellen, um sicherzustellen, dass der Warmstart abgeschlossen ist. Während des Warmstarts ändert sich der Status Ihres Workerknotens nicht. Der Warmstart eines Workerknotens ist in der Regel in wenigen Sekunden abgeschlossen.
 7. Stellen Sie Ihre Workerknoten für die Pod-Planung zur Verfügung. Verwenden Sie für Ihren Workerknoten den **Namen**, der vom Befehl `kubectl get nodes` zurückgegeben wurde.
    ```
    kubectl uncordon <workername>
    ```
    {: pre}
    </br>

<strong>Befehlsoptionen</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Der Name oder die ID des Clusters. Dieser Wert ist erforderlich.</dd>

   <dt><code>-f</code></dt>
   <dd>Geben Sie diese Option an, um den Neustart des Workerknotens ohne Benutzereingabeaufforderungen zu erzwingen. Dieser Wert ist optional.</dd>

   <dt><code>--hard</code></dt>
   <dd>Geben Sie diese Option an, um einen plötzlichen Neustart eines Workerknotens zu erzwingen, indem Sie die Stromversorgung zum Workerknoten kappen. Verwenden Sie diese Option, wenn der Workerknoten nicht antwortet oder Docker blockiert ist. Dieser Wert ist optional.</dd>

   <dt><code><em>WORKERKNOTEN</em></code></dt>
   <dd>Der Name oder die ID einzelner oder mehrerer Workerknoten. Trennen Sie bei einer Auflistung mehrerer Workerknoten die einzelnen Auflistungselemente jeweils durch ein Leerzeichen. Dieser Wert ist erforderlich.</dd>

   <dt><code>-s</code></dt>
   <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>
   </dl>

**Beispiel**:

  ```
  bx cs worker-reboot mein_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}


### bx cs worker-reload [-f] CLUSTER WORKER [WORKER][-s]
{: #cs_worker_reload}

Laden Sie bei Bedarf die erforderlichen Konfigurationen für einen Workerknoten erneut. Ein erneutes Laden kann sinnvoll sein, wenn Ihr Workerknoten Probleme wie zum Beispiel langsames Laden aufweist oder wenn Ihr Knoten in einem nicht einwandfreien Zustand verharrt.

Durch das erneute Laden eines Workerknotens werden Aktualisierungen von Patchversionen auf den Workerknoten angewendet; dies gilt jedoch nicht für Aktualisierungen von Haupt- oder Nebenversionen. Informationen zu den Änderungen von einer Patchversion zur nächsten finden Sie in der Dokumentation zum [Änderungsprotokoll der Version](cs_versions_changelog.html#changelog).
{: tip}

Stellen Sie, bevor Sie Ihren Workerknoten erneut laden, sicher, dass die Pods erneut auf anderen Workerknoten geplant werden, um Ausfallzeiten für Ihre App oder Datenverlust auf Ihrem Workerknoten zu vermeiden.

1. Listen Sie die Workerknoten in Ihrem Cluster auf und notieren Sie sich den **Namen** des Workerknotens, den Sie erneut laden möchten.
   ```
   kubectl get nodes
   ```
   Der **Name**, der in diesem Befehl zurückgegeben wird, ist die private IP-Adresse, die Ihrem Workerknoten zugewiesen ist. Weitere Informationen zu Ihrem Workerknoten finden Sie, wenn Sie den Befehl `bx cs workers <cluster_name_or_ID>` ausführen und nach dem Workerknoten mit derselben **privaten IP**-Adresse suchen.
2. Markieren Sie den Workerknoten in einem Prozess, der als Abriegelung oder "Cordoning" bezeichnet wird, als nicht planbar ("unschedulable"). Wenn Sie einen Workerknoten abriegeln, ist er für die künftige Pod-Planung nicht mehr verfügbar. Verwenden Sie den **Namen** des Workerknotens, den Sie im vorherigen Schritt erhalten haben.
   ```
   kubectl cordon <workername>
   ```
   {: pre}

3. Überprüfen Sie, ob die Pod-Planung für Ihren Workerknoten inaktiviert ist
   ```
   kubectl get nodes
   ```
   {: pre}
   Ihr Workerknoten ist für die Pod-Planung inaktiviert, wenn der Status **SchedulingDisabled** angezeigt wird.
 4. Pods müssen aus Ihrem Workerknoten entfernt und auf den verbleibenden Workerknoten im Cluster erneut geplant werden.
    ```
    kubectl drain <workername>
    ```
    {: pre}
    Dieser Prozess kann einige Minuten dauern.
 5. Laden Sie den Workerknoten erneut. Verwenden Sie die Worker-ID, die vom Befehl `bx cs workers <cluster_name_or_ID>` zurückgegeben wird.
    ```
    bx cs worker-reload <clustername_oder_-id> <workername_oder_-id>
    ```
    {: pre}
 6. Warten Sie, bis das erneute Laden abgeschlossen ist.
 7. Stellen Sie Ihre Workerknoten für die Pod-Planung zur Verfügung. Verwenden Sie für Ihren Workerknoten den **Namen**, der vom Befehl `kubectl get nodes` zurückgegeben wurde.
    ```
    kubectl uncordon <workername>
    ```
</br>
<strong>Befehlsoptionen</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Der Name oder die ID des Clusters. Dieser Wert ist erforderlich.</dd>

   <dt><code>-f</code></dt>
   <dd>Geben Sie diese Option ein, um das Neuladen eines Workerknotens ohne Benutzereingabeaufforderungen zu erzwingen. Dieser Wert ist optional.</dd>

   <dt><code><em>WORKERKNOTEN</em></code></dt>
   <dd>Der Name oder die ID einzelner oder mehrerer Workerknoten. Trennen Sie bei einer Auflistung mehrerer Workerknoten die einzelnen Auflistungselemente jeweils durch ein Leerzeichen. Dieser Wert ist erforderlich.</dd>

   <dt><code>-s</code></dt>
   <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>
   </dl>

**Beispiel**:

  ```
  bx cs worker-reload mein_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}


### bx cs worker-rm [-f] CLUSTER WORKER [WORKER][-s]
{: #cs_worker_rm}

Entfernen Sie einen oder mehrere Workerknoten von einem Cluster. Wenn Sie einen Workerknoten entfernen, ist Ihr Cluster nicht mehr ausgeglichen. 

Stellen Sie, bevor Sie Ihren Workerknoten entfernen, sicher, dass die Pods erneut auf anderen Workerknoten geplant werden, um Ausfallzeiten für Ihre App oder Datenverlust auf Ihrem Workerknoten zu vermeiden.
{: tip}

1. Listen Sie die Workerknoten in Ihrem Cluster auf und notieren Sie sich den **Namen** des Workerknotens, den Sie entfernen möchten.
   ```
   kubectl get nodes
   ```
   Der **Name**, der in diesem Befehl zurückgegeben wird, ist die private IP-Adresse, die Ihrem Workerknoten zugewiesen ist. Weitere Informationen zu Ihrem Workerknoten finden Sie, wenn Sie den Befehl `bx cs workers <cluster_name_or_ID>` ausführen und nach dem Workerknoten mit derselben **privaten IP**-Adresse suchen.
2. Markieren Sie den Workerknoten in einem Prozess, der als Abriegelung oder "Cordoning" bezeichnet wird, als nicht planbar ("unschedulable"). Wenn Sie einen Workerknoten abriegeln, ist er für die künftige Pod-Planung nicht mehr verfügbar. Verwenden Sie den **Namen** des Workerknotens, den Sie im vorherigen Schritt erhalten haben.
   ```
   kubectl cordon <workername>
   ```
   {: pre}

3. Überprüfen Sie, ob die Pod-Planung für Ihren Workerknoten inaktiviert ist
   ```
   kubectl get nodes
   ```
   {: pre}
   Ihr Workerknoten ist für die Pod-Planung inaktiviert, wenn der Status **SchedulingDisabled** angezeigt wird.
4. Pods müssen aus Ihrem Workerknoten entfernt und auf den verbleibenden Workerknoten im Cluster erneut geplant werden.
   ```
   kubectl drain <workername>
   ```
   {: pre}
   Dieser Prozess kann einige Minuten dauern.
5. Entfernen Sie den Worker-Knoten. Verwenden Sie die Worker-ID, die vom Befehl `bx cs workers <cluster_name_or_ID>` zurückgegeben wird.
   ```
   bx cs worker-rm <clustername_oder_-id> <workername_oder_-id>
   ```
   {: pre}

6. Überprüfen Sie, ob der Worker-Knoten entfernt wurde.
   ```
   bx cs workers <clustername_oder_-id>
   ```
</br>
<strong>Befehlsoptionen</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Der Name oder die ID des Clusters. Dieser Wert ist erforderlich.</dd>

   <dt><code>-f</code></dt>
   <dd>Geben Sie diese Option ein, um die Entfernung eines Workerknotens ohne Benutzereingabeaufforderungen zu erzwingen. Dieser Wert ist optional.</dd>

   <dt><code><em>WORKERKNOTEN</em></code></dt>
   <dd>Der Name oder die ID einzelner oder mehrerer Workerknoten. Trennen Sie bei einer Auflistung mehrerer Workerknoten die einzelnen Auflistungselemente jeweils durch ein Leerzeichen. Dieser Wert ist erforderlich.</dd>

   <dt><code>-s</code></dt>
   <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>
   </dl>

**Beispiel**:

  ```
  bx cs worker-rm mein_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}


###bx cs worker-update [-f] CLUSTER WORKER [WORKER][--kube-version MAJOR.MINOR.PATCH] [--force-update][-s]
{: #cs_worker_update}

Aktualisieren Sie Workerknoten, um die neuesten Sicherheitsupdates und Patches auf das Betriebssystem anzuwenden und um die Kubernetes-Version auf die Version des Masterknotens zu aktualisieren. Sie können die Kubernetes-Version des Masterknotens mit dem [Befehl](cs_cli_reference.html#cs_cluster_update) `bx cs cluster-update` aktualisieren.



**Wichtig**: Durch die Ausführung von `bx cs worker-update` kann es zu Ausfallzeiten bei Ihren Apps und Services kommen. Während der Aktualisierung wird die Planung aller Pods auf anderen Workerknoten neu erstellt und die Daten werden gelöscht, wenn sie nicht außerhalb des Pods gespeichert wurden. Um Ausfallzeiten zu vermeiden, sollten Sie [sicherstellen, dass genügend Workerknoten vorhanden sind, um die Workload zu verarbeiten, während die ausgewählten Workerknoten aktualisiert werden](cs_cluster_update.html#worker_node).

Möglicherweise müssen Sie Ihre YAML-Dateien für Bereitstellungen vor der Aktualisierung ändern. Lesen Sie die detaillierten Informationen in diesen [Releaseinformationen](cs_versions.html).

<strong>Befehlsoptionen</strong>:

   <dl>

   <dt><em>CLUSTER</em></dt>
   <dd>Der Name oder die ID des Clusters, in dem Sie verfügbare Workerknoten auflisten. Dieser Wert ist erforderlich.</dd>

   <dt><code>-f</code></dt>
   <dd>Geben Sie diese Option an, um die Aktualisierung des Masters ohne Benutzereingabeaufforderungen zu erzwingen. Dieser Wert ist optional.</dd>

   <dt><code>--force-update</code></dt>
   <dd>Versuch einer Aktualisierung, selbst wenn die Änderung sich über mehr als zwei Nebenversionen erstreckt. Dieser Wert ist optional.</dd>

   <dt><code>--kube-version <em>MAJOR.MINOR.PATCH</em></code></dt>
     <dd>Die Version von Kubernetes, mit der die Workerknoten aktualisiert werden sollen. Falls dieser Wert nicht angegeben ist, wird die Standardversion verwendet.</dd>

   <dt><code><em>WORKERKNOTEN</em></code></dt>
   <dd>Die ID einzelner oder mehrerer Workerknoten. Trennen Sie bei einer Auflistung mehrerer Workerknoten die einzelnen Auflistungselemente jeweils durch ein Leerzeichen. Dieser Wert ist erforderlich.</dd>

   <dt><code>-s</code></dt>
   <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>

   </dl>

**Beispiel**:

  ```
  bx cs worker-update mein_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}


### bx cs workers CLUSTER [--show-deleted][--json] [-s]
{: #cs_workers}

Anzeigen einer Liste der der Workerknoten und ihres jeweiligen Status in einem Cluster.

<strong>Befehlsoptionen</strong>:

   <dl>
   <dt><em>CLUSTER</em></dt>
   <dd>Der Name oder die ID des Clusters für die verfügbaren Workerknoten. Dieser Wert ist erforderlich.</dd>

   <dt><em>--show-deleted</em></dt>
   <dd>Zeigen Sie Workerknoten an, die aus dem Cluster gelöscht wurden, und den Grund für das Löschen. Dieser Wert ist optional.</dd>

   <dt><code>--json</code></dt>
   <dd>Druckt die Befehlsausgabe im JSON-Format. Dieser Wert ist optional.</dd>

  <dt><code>-s</code></dt>
  <dd>Die Tagesnachricht wird nicht angezeigt oder es werden keine Erinnerungen aktualisiert. Dieser Wert ist optional.</dd>
   </dl>

**Beispiel**:

  ```
  bx cs workers mein_cluster
  ```
  {: pre}
