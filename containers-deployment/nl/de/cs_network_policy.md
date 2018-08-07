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



# Datenverkehr anhand von Netzrichtlinien steuern
{: #network_policies}

Jeder Kubernetes-Cluster wird mit einem Netz-Plug-in namens Calico eingerichtet. Standardnetzrichtlinien werden zum Schutz der öffentlichen Netzschnittstelle der einzelnen Workerknoten in {{site.data.keyword.containerlong}} eingerichtet.
{: shortdesc}

Wenn Sie spezielle Sicherheitsanforderungen haben, können Sie Calico und Kubernetes zum Ersellen von Netzrichtlinien für einen Cluster verwenden. Mit Kubernetes-Netzrichtlinien können Sie den Netzverkehr angeben, den Sie zu und von einem Pod in einem Cluster zulassen oder blockieren möchten. Um erweiterte Netzrichtlinien einzurichten, beispielsweise zum Blockieren von eingehendem Datenverkehr (Ingress) an LoadBalancer-Services, verwenden Sie Calico-Netzrichtlinien.

<ul>
  <li>
  [Kubernetes-Netzrichtlinien ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/services-networking/network-policies/): Diese Richtlinien geben an, wie Pods mit anderen Pods und externen Endpunkten kommunizieren können endpoints. Ab Kubernetes Vrsion 1.8 kann eingehender und abgehender Datenverkehr basierend auf Protokoll, Port, IP-Quellenadressen oder -Zieladressen erlaubt oder blockiert werden. Der Datenverkehr kann zudem anhand von Pod- und Namensbereichsbezeichnungen gefiltert werden. Kubernetes-Netzrichtlinien werden mithilfe von `kubectl`-Befehlen oder den Kubernetes-APIs angewendet. Werden diese Richtlinien angewendet, werden sie automatisch in Calico-Netzrichtlinien konvertiert und Calico erzwingt sie.
  </li>
  <li>
  Calico-Netzrichtlinien für Kubernetes Version [1.10 und höhere Cluster ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.projectcalico.org/v3.1/getting-started/kubernetes/tutorials/advanced-policy) oder [1.9 und frühere Cluster ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.projectcalico.org/v2.6/getting-started/kubernetes/tutorials/advanced-policy): Diese Richtlinien sind eine übergeordnete Gruppe der Kubernetes-Netzrichtlinien und werden mithilfe von Befehlen des Typs `calicoctl` angewendet. Im Rahmen von Calico-Richtlinien werden die folgenden Features hinzugefügt.
    <ul>
    <li>Zulassen oder Blockieren von Netzverkehr in bestimmten Netzschnittstellen, unabhängig von der IP-Quellenadresse oder -Zieladresse von Kubernetes-Pods oder vom CIDR.</li>
    <li>Zulassen oder Blockieren von Netzverkehr für Pods über Namensbereiche hinweg.</li>
    <li>[Blockieren von eingehendem Datenverkehr (Ingress) an die LoadBalancer- oder NodePort-Services von Kubernetes](#block_ingress).</li>
    </ul>
  </li>
  </ul>

Calico erzwingt diese Richtlinien, einschließlich aller Kubernetes-Netzrichtlinien, die automatisch in Calico-Richtlinien konvertiert werden, indem 'iptables'-Regeln von Linux in den Kubernetes-Workerknoten konfiguriert werden. 'iptables'-Regeln dienen als
Firewall für den Workerknoten, um die Merkmale zu definieren, die der Netzverkehr erfüllen muss, damit er an die Zielressource weitergeleitet wird.

<br />


## Standardmäßige Calico- und Kubernetes-Netzrichtlinien
{: #default_policy}

Wenn ein Cluster mit einem öffentlichen VLAN erstellt wird, wird für jeden Workerknoten und die entsprechende öffentliche Netzschnittstelle automatisch eine Host-Endpunkt-Ressource mit der Bezeichnung `ibm.role: worker_public` erstellt. Um die öffentliche Netzschnittstelle eines Workerknotens zu schützen, werden Calico-Standardrichtlinien auf alle Host-Endpunkte mit der Bezeichnung `ibm.role: worker_public` angewendet.
{:shortdesc}

Diese Calico-Standardrichtlinien lassen sämtlichen ausgehenden und eingehenden Netzdatenverkehr zu bestimmten Clusterkomponenten, wie NodePort-, LoadBalancer- und Ingress-Services von Kubernetes, zu. Eingehender Netzdatenverkehr aus dem Internet zu den Workerknoten, der nicht in den Standardrichtlinien festgelegt ist, wird blockiert. Die Standardrichtlinien wirken sich nicht auf Datenverkehr zwischen Pods aus.

Sehen Sie sich die folgenden Calico-Standardnetzrichtlinien an, die automatisch auf Ihren Cluster angewendet werden.

**Wichtig:** Entfernen Sie keine Richtlinien, die auf einen Host-Endpunkt angewendet sind, es sei denn, Sie kennen die Richtlinie. Vergewissern Sie sich, dass Sie den Datenverkehr, der durch diese Richtlinie zugelassen wird, nicht benötigen.

 <table summary="Die erste Zeile in der Tabelle erstreckt sich über beide Spalten. Die verbleibenden Zeilen enthalten von links nach rechts den Serverstandort in der ersten Spalte und die entsprechenden IP-Adressen in der zweiten Spalte.">
 <caption>Calico-Standardrichtlinien für die einzelnen Cluster</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Calico-Standardrichtlinien für die einzelnen Cluster</th>
  </thead>
  <tbody>
    <tr>
      <td><code>allow-all-outbound</code></td>
      <td>Lässt den gesamten ausgehenden Datenverkehr zu.</td>
    </tr>
    <tr>
      <td><code>allow-bixfix-port</code></td>
      <td>Lässt eingehenden Datenverkehr an Port 52311 zur BigFix-App zu, um erforderliche Aktualisierungen für Workerknoten zu ermöglichen.</td>
    </tr>
    <tr>
      <td><code>allow-icmp</code></td>
      <td>Lässt eingehende 'ICMP-Pakete (Pings) zu.</td>
     </tr>
    <tr>
      <td><code>allow-node-port-dnat</code></td>
      <td>Lässt eingehenden Datenverkehr für den NodePort-, LoadBalancer- und Ingress-Service zu den Pods zu, die diese Services zugänglich machen. <strong>Hinweis</strong>: Sie müssen die zugänglich gemachten Ports nicht angeben, weil Kubernetes DNAT (Destination Network Address Translation, Zielnetzadressumsetzung) verwendet, um die Serviceanforderungen an die korrekten Pods weiterzuleiten. Diese Weiterleitung findet statt, bevor der Host-Endpunkt in 'iptables' angewendet wird.</td>
   </tr>
   <tr>
      <td><code>allow-sys-mgmt</code></td>
      <td>Lässt eingehende Verbindungen für bestimmte Systeme von IBM Cloud Infrastructure (SoftLayer) zu, die zum Verwalten der Workerknoten verwendet werden.</td>
   </tr>
   <tr>
    <td><code>allow-vrrp</code></td>
    <td>Lässt VRRP-Pakete zu, die zum Überwachen und Verschieben von virtuellen IP-Adressen zwischen Workerknoten verwendet werden.</td>
   </tr>
  </tbody>
</table>

In Clustern der Kubernetes Version 1.10 und höher wird auch eine Kubernetes-Standardrichtlinie erstellt, die den Zugriff auf das Kubernetes-Dashboard begrenzt. Kubernetes-Richtlinien gelten nicht für den Host-Endpunkt, sondern für den Pod `kube-dashboard`. Diese Richtlinie wird auf Cluster angewendet, die ausschließlich mit einem privaten VLAN verbunden sind, sowie Cluster, die mit einem öffentlichen und einem privaten VLAN verbunden sind.

<table>
<caption>Kubernetes-Standardrichtlinien für die einzelnen Cluster</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Kubernetes-Standardrichtlinien für die einzelnen Cluster</th>
</thead>
<tbody>
 <tr>
  <td><code>kubernetes-dashboard</code></td>
  <td><b>Nur in Kubernetes Version 1.10</b> im Namensbereich <code>kube-system</code>: Blockiert für alle Pods den Zugriff auf das Kubernetes-Dashboard. Diese Richtlinie hat keinerlei Auswirkungen auf den Zugriff auf das Dashboard über die {{site.data.keyword.Bluemix_notm}}-Benutzerschnittstelle oder mithilfe von <code>kubectl proxy</code>. Wenn ein Pod Zugriff auf das Dashboard benötigt, stellen Sie den Pod in einem Namensbereich mit der Bezeichnung <code>kubernetes-dashboard-policy: allow</code> bereit.</td>
 </tr>
</tbody>
</table>

<br />


## Calico CLI installieren und konfigurieren
{: #cli_install}

Installieren und konfigurieren Sie die Calico-CLI zum Anzeigen, Verwalten und Hinzufügen von Calico-Richtlinien.
{:shortdesc}

Die Kompatibilität von Calico-Versionen für die CLI-Konfiguration und Richtlinien hängt von der Kubernetes-Version des Clusters ab. Um die Calico-CLI zu installieren und konfigurieren, klicken Sie basierend auf Ihrer Clusterversion auf einen der nachfolgenden Links:

* [Kubernetes Version 1.10 oder höhere Cluster](#1.10_install)
* [Kubernetes Version 1.10 oder frühere Cluster](#1.9_install)

Bevor Sie Ihren Cluster von Kubernetes Version 1.9 oder einer früheren Version auf Version 1.10 oder höher aktualisieren, ziehen Sie den Abschnitt [Vorbereitungen für die Aktualisierung auf Calico Version 3](cs_versions.html#110_calicov3) zurate.
{: tip}

### Version 3.1.1 der Calico-CLI für Cluster installieren und konfigurieren, die Kubernetes Version 1.10 oder höher ausführe
{: #1.10_install}

Führen Sie zunächst folgende Schritte aus. [Richten Sie die Kubernetes-CLI auf Ihren Cluster aus](cs_cli_install.html#cs_cli_configure). Schließen Sie die Option `--admin` mit dem Befehl `bx cs cluster-config` ein, der zum Herunterladen der Zertifikate und Berechtigungsdateien verwendet wird. In diesem Download sind auch die Schlüssel für die Rolle 'Superuser' enthalten, die Sie zum Ausführen von Calico-Befehlen benötigen.

  ```
  bx cs cluster-config <clustername> --admin
  ```
  {: pre}

Gehen Sie wie folgt vor, um die Calico-CLI der Version 3.1.1 zu installieren und konfigurieren:

1. [Laden Sie die Calico-CLI ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/projectcalico/calicoctl/releases/tag/v3.1.1) herunter.

    Wenn Sie OSX verwenden, laden Sie die Version `-darwin-amd64` herunter. Wenn Sie Windows verwenden, installieren Sie die Calico-CLI in demselben Verzeichnis wie die {{site.data.keyword.Bluemix_notm}}-CLI. Diese Konstellation erspart Ihnen bei der späteren Ausführung von Befehlen einige Dateipfadänderungen. {: tip}

2. OSX- und Linux-Benutzer müssen die folgenden Schritte ausführen:
    1. Verschieben Sie die ausführbare Datei in das Verzeichnis _/usr/local/bin_.
        - Linux:

          ```
          mv filepath/calicoctl /usr/local/bin/calicoctl
          ```
          {: pre}

        - OS
X:

          ```
          mv filepath/calicoctl-darwin-amd64 /usr/local/bin/calicoctl
          ```
          {: pre}

    2. Konvertieren Sie die Datei in eine ausführbare Datei.

        ```
        chmod +x /usr/local/bin/calicoctl
        ```
        {: pre}

3. Stellen Sie sicher, dass die `calico`-Befehle ordnungsgemäß ausgeführt wurden. Überprüfen Sie dazu die Clientversion der Calico-CLI.

    ```
    calicoctl version
    ```
    {: pre}

4. Falls Unternehmensnetzrichtlinien den Zugriff von Ihrem lokalen System auf öffentliche Endpunkte über Proxys oder Firewalls verhindert, [lassen Sie TCP-Zugriff für Calico-Befehle zu](cs_firewall.html#firewall).

5. Erstellen Sie für Linux und OS X das Verzeichnis `/etc/calico`. Unter Windows kann ein beliebiges Verzeichnis verwendet werden.

  ```
  sudo mkdir -p /etc/calico/
  ```
  {: pre}

6. Erstellen Sie die Datei `calicoctl.cfg`.
    - Linux und OS X:

      ```
      sudo vi /etc/calico/calicoctl.cfg
      ```
      {: pre}

    - Windows: Erstellen Sie die Datei mit einem Texteditor.

7. Geben Sie die folgenden Informationen in der Datei <code>calicoctl.cfg</code> ein.

    ```
    apiVersion: projectcalico.org/v3
    kind: CalicoAPIConfig
    metadata:
    spec:
        datastoreType: etcdv3
        etcdEndpoints: <ETCD_URL>
        etcdKeyFile: <CERTS_DIR>/admin-key.pem
        etcdCertFile: <CERTS_DIR>/admin.pem
        etcdCACertFile: <CERTS_DIR>/<ca-*pem_file>
    ```
    {: codeblock}

    1. Rufen Sie die `<ETCD_URL>` ab.

      - Linux und OS X:

          ```
          kubectl get cm -n kube-system calico-config -o yaml | grep "etcd_endpoints:" | awk '{ print $2 }'
          ```
          {: pre}

          Beispielausgabe:

          ```
          https://169.xx.xxx.xxx:30001
          ```
          {: screen}

      - Windows:
        <ol>
        <li>Rufen Sie die Calico-Konfigurationswerte aus der Konfigurationszuordnung ab. </br><pre class="codeblock"><code>kubectl get cm -n kube-system calico-config -o yaml</code></pre></br>
        <li>Suchen Sie im Abschnitt `data` nach dem Wert für 'etcd_endpoints'. Beispiel: <code>https://169.xx.xxx.xxx:30001</code>
        </ol>

    2. Rufen Sie das Verzeichnis `<CERTS_DIR>` ab. Dies ist das Verzeichnis, in das die Kubernetes-Zertifikate heruntergeladen werden.

        - Linux und OS X:

          ```
          dirname $KUBECONFIG
          ```
          {: pre}

          Beispielausgabe:

          ```
          /home/sysadmin/.bluemix/plugins/container-service/clusters/<clustername>-admin/
          ```
          {: screen}

        - Windows:

          ```
          ECHO %KUBECONFIG%
          ```
          {: pre}

            Ausgabebeispiel:

          ```
          C:/Users/<benutzer>/.bluemix/plugins/container-service/mycluster-admin/kube-config-prod-dal10-mycluster.yml
          ```
          {: screen}

        **Hinweis**: Entfernen Sie den Dateinamen `kube-config-prod-<location>-<cluster_name>.yml` am Ende der Ausgabe, um den Verzeichnispfad zu erhalten.

    3. Rufen Sie die `ca-*pem-datei` ab.

        - Linux und OS X:

          ```
          ls `dirname $KUBECONFIG` | grep "ca-"
          ```
          {: pre}

        - Windows:
          <ol><li>Öffnen Sie das Verzeichnis, das Sie im letzten Schritt abgerufen haben.</br><pre class="codeblock"><code>C:\Benutzer\<user>\.bluemix\plugins\container-service\&lt;clustername&gt;-admin\</code></pre>
          <li> Suchen Sie die Datei <code>ca-*pem-datei</code>.</ol>

    4. Vergewissern Sie sich, dass die Calico-Konfiguration ordnungsgemäß funktioniert.

        - Linux und OS X:

          ```
          calicoctl get nodes
          ```
          {: pre}

        - Windows:

          ```
          calicoctl get nodes --config=filepath/calicoctl.cfg
          ```
          {: pre}

          Ausgabe:

          ```
          NAME
              kube-dal10-crc21191ee3997497ca90c8173bbdaf560-w1.cloud.ibm
              kube-dal10-crc21191ee3997497ca90c8173bbdaf560-w2.cloud.ibm
              kube-dal10-crc21191ee3997497ca90c8173bbdaf560-w3.cloud.ibm
          ```
          {: screen}


### Version 1.6.3 der Calico-CLI für Cluster installieren und konfigurieren, die Kubernetes Version 1.9 oder früher ausführe
{: #1.9_install}

Führen Sie zunächst folgende Schritte aus. [Richten Sie die Kubernetes-CLI auf Ihren Cluster aus](cs_cli_install.html#cs_cli_configure). Schließen Sie die Option `--admin` mit dem Befehl `bx cs cluster-config` ein, der zum Herunterladen der Zertifikate und Berechtigungsdateien verwendet wird. In diesem Download sind auch die Schlüssel für die Rolle 'Superuser' enthalten, die Sie zum Ausführen von Calico-Befehlen benötigen.

  ```
  bx cs cluster-config <clustername> --admin
  ```
  {: pre}

Gehen Sie wie folgt vor, um die Calico-CLI der Version 1.6.3 zu installieren und konfigurieren:

1. [Laden Sie die Calico-CLI ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/projectcalico/calicoctl/releases/tag/v1.6.3) herunter.

    Wenn Sie OSX verwenden, laden Sie die Version `-darwin-amd64` herunter. Wenn Sie Windows verwenden, installieren Sie die Calico-CLI in demselben Verzeichnis wie die {{site.data.keyword.Bluemix_notm}}-CLI. Diese Konstellation erspart Ihnen bei der späteren Ausführung von Befehlen einige Dateipfadänderungen. {: tip}

2. OSX- und Linux-Benutzer müssen die folgenden Schritte ausführen:
    1. Verschieben Sie die ausführbare Datei in das Verzeichnis _/usr/local/bin_.
        - Linux:

          ```
          mv filepath/calicoctl /usr/local/bin/calicoctl
          ```
          {: pre}

        - OS
X:

          ```
          mv filepath/calicoctl-darwin-amd64 /usr/local/bin/calicoctl
          ```
          {: pre}

    2. Konvertieren Sie die Datei in eine ausführbare Datei.

        ```
        chmod +x /usr/local/bin/calicoctl
        ```
        {: pre}

3. Stellen Sie sicher, dass die `calico`-Befehle ordnungsgemäß ausgeführt wurden. Überprüfen Sie dazu die Clientversion der Calico-CLI.

    ```
    calicoctl version
    ```
    {: pre}

4. Falls Unternehmensnetzrichtlinien den Zugriff von Ihrem lokalen System auf öffentliche Endpunkte über Proxys oder Firewalls verhindern, müssen Sie TCP-Zugriff für die Calico-Befehle zulassen. Weitere Informationen hierzu finden Sie unter [`calicoctl`-Befehle hinter einer Firewall ausführen](cs_firewall.html#firewall).

5. Erstellen Sie für Linux und OS X das Verzeichnis `/etc/calico`. Unter Windows kann ein beliebiges Verzeichnis verwendet werden.
    ```
    sudo mkdir -p /etc/calico/
    ```
    {: pre}

6. Erstellen Sie die Datei `calicoctl.cfg`.
    - Linux und OS X:

      ```
      sudo vi /etc/calico/calicoctl.cfg
      ```
      {: pre}

    - Windows: Erstellen Sie die Datei mit einem Texteditor.

7. Geben Sie die folgenden Informationen in der Datei <code>calicoctl.cfg</code> ein.

    ```
    apiVersion: v1
      kind: calicoApiConfig
      metadata:
      spec:
          etcdEndpoints: <ETCD-URL>
          etcdKeyFile: <ZERTIFIKATSVERZEICHNIS>/admin-key.pem
          etcdCertFile: <ZERTIFIKATSVERZEICHNIS>/admin.pem
          etcdCACertFile: <ZERTIFIKATSVERZEICHNIS>/<ca-*pem-datei>
    ```
    {: codeblock}

    1. Rufen Sie die `<ETCD_URL>` ab.

      - Linux und OS X:

          ```
          kubectl get cm -n kube-system calico-config -o yaml | grep "etcd_endpoints:" | awk '{ print $2 }'
          ```
          {: pre}

      - Ausgabebeispiel:

          ```
          https://169.xx.xxx.xxx:30001
          ```
          {: screen}

      - Windows:
        <ol>
        <li>Rufen Sie die Calico-Konfigurationswerte aus der Konfigurationszuordnung ab. </br><pre class="codeblock"><code>kubectl get cm -n kube-system calico-config -o yaml</code></pre></br>
        <li>Suchen Sie im Abschnitt `data` nach dem Wert für 'etcd_endpoints'. Beispiel: <code>https://169.xx.xxx.xxx:30001</code>
        </ol>

    2. Rufen Sie das Verzeichnis `<CERTS_DIR>` ab. Dies ist das Verzeichnis, in das die Kubernetes-Zertifikate heruntergeladen werden.

        - Linux und OS X:

          ```
          dirname $KUBECONFIG
          ```
          {: pre}

          Beispielausgabe:

          ```
          /home/sysadmin/.bluemix/plugins/container-service/clusters/<clustername>-admin/
          ```
          {: screen}

        - Windows:

          ```
          ECHO %KUBECONFIG%
          ```
          {: pre}

          Beispielausgabe:

          ```
          C:/Users/<benutzer>/.bluemix/plugins/container-service/mycluster-admin/kube-config-prod-dal10-mycluster.yml
          ```
          {: screen}

        **Hinweis**: Entfernen Sie den Dateinamen `kube-config-prod-<location>-<cluster_name>.yml` am Ende der Ausgabe, um den Verzeichnispfad zu erhalten.

    3. Rufen Sie die `ca-*pem-datei` ab.

        - Linux und OS X:

          ```
          ls `dirname $KUBECONFIG` | grep "ca-"
          ```
          {: pre}

        - Windows:
          <ol><li>Öffnen Sie das Verzeichnis, das Sie im letzten Schritt abgerufen haben.</br><pre class="codeblock"><code>C:\Benutzer\<user>\.bluemix\plugins\container-service\&lt;clustername&gt;-admin\</code></pre>
          <li> Suchen Sie die Datei <code>ca-*pem-datei</code>.</ol>

    4. Vergewissern Sie sich, dass die Calico-Konfiguration ordnungsgemäß funktioniert.

        - Linux und OS X:

          ```
          calicoctl get nodes
          ```
          {: pre}

        - Windows:

          ```
          calicoctl get nodes --config=filepath/calicoctl.cfg
          ```
          {: pre}

          Ausgabe:

          ```
          NAME
              kube-dal10-crc21191ee3997497ca90c8173bbdaf560-w1.cloud.ibm
              kube-dal10-crc21191ee3997497ca90c8173bbdaf560-w2.cloud.ibm
              kube-dal10-crc21191ee3997497ca90c8173bbdaf560-w3.cloud.ibm
          ```
          {: screen}

<br />


## Netzrichtlinien anzeigen
{: #view_policies}

Zeigen Sie die Details für Standardrichtlinien und allen weiteren hinzugefügten Netzrichtlinien an, die auf Ihren Cluster angewendet werden.
{:shortdesc}

Vorbemerkungen:
1. [Installieren und konfigurieren Sie die Calico-CLI.](#cli_install)
2. [Richten Sie die Kubernetes-CLI auf den Cluster aus](cs_cli_install.html#cs_cli_configure). Schließen Sie die Option `--admin` mit dem Befehl `bx cs cluster-config` ein, der zum Herunterladen der Zertifikate und Berechtigungsdateien verwendet wird. In diesem Download sind auch die Schlüssel für die Rolle 'Superuser' enthalten, die Sie zum Ausführen von Calico-Befehlen benötigen.
    ```
    bx cs cluster-config <clustername> --admin
    ```
    {: pre}

Die Kompatibilität von Calico-Versionen für die CLI-Konfiguration und Richtlinien hängt von der Kubernetes-Version des Clusters ab. Um die Calico-CLI zu installieren und konfigurieren, klicken Sie basierend auf Ihrer Clusterversion auf einen der nachfolgenden Links:

* [Kubernetes Version 1.10 oder höhere Cluster](#1.10_examine_policies)
* [Kubernetes Version 1.10 oder frühere Cluster](#1.9_examine_policies)

Bevor Sie Ihren Cluster von Kubernetes Version 1.9 oder einer früheren Version auf Version 1.10 oder höher aktualisieren, ziehen Sie den Abschnitt [Vorbereitungen für die Aktualisierung auf Calico Version 3](cs_versions.html#110_calicov3) zurate.
{: tip}

### Netzrichtlinien in Clustern anzeigen, die Kubernetes Version 1.10 oder höher ausführen
{: #1.10_examine_policies}

1. Zeigen Sie den Calico-Host-Endpunkt an.

    ```
    calicoctl get hostendpoint -o yaml
    ```
    {: pre}

2. Zeigen Sie alle Calico- und Kubernetes-Netzrichtlinien an, die für den Cluster erstellt wurden. Diese Liste enthält Richtlinien, die möglicherweise noch nicht auf Pods oder Hosts angewendet wurden. Damit eine Netzrichtlinie erzwungen wird, muss eine Kubernetes-Ressource gefunden werden, die mit dem Selektor übereinstimmt, der in der Calico-Netzrichtlinie definiert wurde.

    [Netzrichtlinien ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/networkpolicy) beziehen sich auf bestimmte Namensbereiche:
    ```
    calicoctl get NetworkPolicy --all-namespaces -o wide
    ```

    [Globale Netzrichtlinien ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/globalnetworkpolicy) beziehen sich nicht auf bestimmte Namensbereiche:
    ```
    calicoctl get GlobalNetworkPolicy -o wide
    ```
    {: pre}

3. Zeigen Sie Details für ein Netzrichtlinie an.

    ```
    calicoctl get NetworkPolicy -o yaml <richtlinienname> --namespace <namensbereich_für_richtlinie>
    ```
    {: pre}

4. Zeigen Sie die Details aller globalen Netzrichtlinien für den Cluster an.

    ```
    calicoctl get GlobalNetworkPolicy -o yaml
    ```
    {: pre}

### Netzrichtlinien in Clustern anzeigen, die Kubernetes Version 1.9 oder früher ausführen
{: #1.9_examine_policies}

1. Zeigen Sie den Calico-Host-Endpunkt an.

    ```
    calicoctl get hostendpoint -o yaml
    ```
    {: pre}

2. Zeigen Sie alle Calico- und Kubernetes-Netzrichtlinien an, die für den Cluster erstellt wurden. Diese Liste enthält Richtlinien, die möglicherweise noch nicht auf Pods oder Hosts angewendet wurden. Damit eine Netzrichtlinie erzwungen wird, muss eine Kubernetes-Ressource gefunden werden, die mit dem Selektor übereinstimmt, der in der Calico-Netzrichtlinie definiert wurde.

    ```
    calicoctl get policy -o wide
    ```
    {: pre}

3. Zeigen Sie Details für ein Netzrichtlinie an.

    ```
    calicoctl get policy -o yaml <richtlinienname>
    ```
    {: pre}

4. Zeigen Sie die Details aller Netzrichtlinien für den Cluster an.

    ```
    calicoctl get policy -o yaml
    ```
    {: pre}

<br />


## Netzrichtlinien hinzufügen
{: #adding_network_policies}

In den meisten Fällen müssen die Standardrichtlinien nicht geändert werden. Nur erweiterte Szenarios können unter Umständen Änderungen erforderlich machen. Wenn Sie feststellen, dass Sie Änderungen vornehmen müssen, können Sie Ihre eigenen Netzrichtlinien erstellen.
{:shortdesc}

Informationen zum Erstellen von Kubernetes-Netzrichtlinien finden Sie in der [Dokumentation zu Kubernetes-Netzrichtlinien ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/services-networking/network-policies/).

Um Calico-Richtlinien zu erstellen, führen Sie die folgenden Schritte aus.

Vorbemerkungen:
1. [Installieren und konfigurieren Sie die Calico-CLI.](#cli_install)
2. [Richten Sie die Kubernetes-CLI auf den Cluster aus](cs_cli_install.html#cs_cli_configure). Schließen Sie die Option `--admin` mit dem Befehl `bx cs cluster-config` ein, der zum Herunterladen der Zertifikate und Berechtigungsdateien verwendet wird. In diesem Download sind auch die Schlüssel für die Rolle 'Superuser' enthalten, die Sie zum Ausführen von Calico-Befehlen benötigen.
    ```
    bx cs cluster-config <clustername> --admin
    ```
    {: pre}

Die Kompatibilität von Calico-Versionen für die CLI-Konfiguration und Richtlinien hängt von der Kubernetes-Version des Clusters ab. Klicken Sie basierend auf Ihrer Clusterversion auf einen der nachfolgenden Links:

* [Kubernetes Version 1.10 oder höhere Cluster](#1.10_create_new)
* [Kubernetes Version 1.10 oder frühere Cluster](#1.9_create_new)

Bevor Sie Ihren Cluster von Kubernetes Version 1.9 oder einer früheren Version auf Version 1.10 oder höher aktualisieren, ziehen Sie den Abschnitt [Vorbereitungen für die Aktualisierung auf Calico Version 3](cs_versions.html#110_calicov3) zurate.
{: tip}

### Calico-Richtlinien in Clustern hinzufügen, die Kubernetes Version 1.10 oder höher ausführen
{: #1.10_create_new}

1. Definieren Sie die Calico-[Netzrichtlinie ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/networkpolicy) oder [globale Netzrichtline ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/globalnetworkpolicy), indem Sie ein Konfigurationsscript (`.yaml`) erstellen. Diese Konfigurationsdateien enthalten die Selektoren, die beschreiben, auf welche Pods, Namensbereiche oder Hosts diese Richtlinien angewendet werden. Ziehen Sie diese [Calico-Beispielrichtlinien ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://docs.projectcalico.org/v3.1/getting-started/kubernetes/tutorials/advanced-policy) als Hilfestellung heran, wenn Sie Ihre eigenen erstellen.
    **Hinweis**: Cluster der Kubernetes Version 1.10 oder höher müssen die Syntax für Calico Version 3-Richtlinien verwenden.

2. Wenden Sie die Richtlinien auf den Cluster an.
    - Linux und OS X:

      ```
      calicoctl apply -f policy.yaml
      ```
      {: pre}

    - Windows:

      ```
      calicoctl apply -f filepath/policy.yaml --config=filepath/calicoctl.cfg
      ```
      {: pre}

### Calico-Richtlinien in Clustern hinzufügen, die Kubernetes Version 1.9 oder früher ausführen
{: #1.9_create_new}

1. Definieren Sie Ihre [Calico-Netzrichtlinie ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://docs.projectcalico.org/v2.6/reference/calicoctl/resources/policy), indem Sie ein Konfigurationsscript erstellen (`.yaml`). Diese Konfigurationsdateien enthalten die Selektoren, die beschreiben, auf welche Pods, Namensbereiche oder Hosts diese Richtlinien angewendet werden. Ziehen Sie diese [Calico-Beispielrichtlinien ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://docs.projectcalico.org/v2.6/getting-started/kubernetes/tutorials/advanced-policy) als Hilfestellung heran, wenn Sie Ihre eigenen erstellen.
    **Hinweis**: Cluster der Kubernetes Version 1.9 oder früher müssen die Syntax für Calico Version 2-Richtlinien verwenden.


2. Wenden Sie die Richtlinien auf den Cluster an.
    - Linux und OS X:

      ```
      calicoctl apply -f policy.yaml
      ```
      {: pre}

    - Windows:

      ```
      calicoctl apply -f filepath/policy.yaml --config=filepath/calicoctl.cfg
      ```
      {: pre}

<br />


## Eingehenden Datenverkehr zu LoadBalancer- oder NodePort-Services blockieren
{: #block_ingress}

[Standardmäßig](#default_policy) sind die NodePort- und LoadBalancer-Services von Kubernetes so konzipiert, dass Ihre App in allen öffentlichen und privaten Clusterschnittstellen verfügbar ist. Allerdings können Sie den eingehenden Datenverkehr zu Ihren Services auf Basis der Datenverkehrsquelle oder des Ziels blockieren.
{:shortdesc}

Ein Kubernetes-LoadBalancer-Service stellt auch einen NodePort-Service dar. Ein LoadBalancer-Service stellt Ihre App über die IP-Adresse der Lastausgleichsfunktion und den zugehörigen Port zur Verfügung und macht sie über die Knotenports des Service verfügbar. Auf Knotenports kann über jede IP-Adresse (öffentlich und privat) für jeden Knoten innerhalb des Clusters zugegriffen werden.

Der Clusteradministrator kann die Calico-Netzrichtlinien vom Typ `preDNAT` verwenden, um folgenden Datenverkehr zu blockieren:

  - Den Datenverkehr zu den NodePort-Services. Der Datenverkehr zu LoadBalancer-Services ist zulässig.
  - Den Datenverkehr, der auf einer Quellenadresse oder einem CIDR basiert.

Nachfolgend finden Sie einige gängige Anwendungsbereiche für die Calico-Netzrichtlinien des Typs

  - Blockieren des Datenverkehrs an öffentliche Knotenports eines privaten LoadBalancer-Service.
  - Blockieren des Datenverkehrs an öffentliche Knotenports in Clustern, in denen [Edge-Workerknoten](cs_edge.html#edge) ausgeführt werden. Durch das Blockieren von Knotenports wird sichergestellt, dass die Edge-Workerknoten die einzigen Workerknoten sind, die eingehenden Datenverkehr verarbeiten.

Die standardmäßigen Kubernetes- und Calico-Richtlinien sind aufgrund der DNAT-iptables-Regeln, die für die Kubernetes-NodePort- und Kubernetes-LoadBalancer-Services generiert werden, zum Schutz dieser Services schwierig anzuwenden.

Calico-Netzrichtlinien des Typs `preDNAT` können Ihnen helfen, da sie iptables-Regeln auf Basis einer Calico-Netzrichtlinienressource
generieren. Cluster der Kubernetes Version 1.10 oder höher verwenden [Netzrichtlinien mit der Version 3-Syntax von `calicoctl.cfg` ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/networkpolicy). Cluster der Kubernetes Version 1.9 oder früher verwenden [Richtlinien mit der Version 2-Syntax von `calicoctl.cfg` ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.projectcalico.org/v2.6/reference/calicoctl/resources/policy).

1. Definieren Sie eine Calico-Netzrichtlinie vom Typ `preDNAT` für den Ingress-Zugriff (eingehenden Datenverkehr) auf Kubernetes-Services.

    * Cluster der Kubernetes Version 1.10 oder höher müssen die Syntax für Calico Version 3-Richtlinien verwenden.

        Beispielressource, die alle NodePorts-Services blockiert:

        ```
        apiVersion: projectcalico.org/v3
        kind: GlobalNetworkPolicy
        metadata:
          name: deny-kube-node-port-services
        spec:
          applyOnForward: true
          ingress:
          - action: Deny
            destination:
              ports:
              - 30000:32767
            protocol: TCP
            source: {}
          - action: Deny
            destination:
              ports:
              - 30000:32767
            protocol: UDP
            source: {}
          preDNAT: true
          selector: ibm.role in { 'worker_public', 'master_public' }
          types:
          - Ingress
        ```
        {: codeblock}

    * Cluster der Kubernetes Version 1.9 oder früher müssen die Syntax für Calico Version 2-Richtlinien verwenden.

        Beispielressource, die alle NodePorts-Services blockiert:

        ```
        apiVersion: v1
        kind: policy
        metadata:
          name: deny-kube-node-port-services
        spec:
          preDNAT: true
          selector: ibm.role in { 'worker_public', 'master_public' }
          ingress:
          - action: deny
            protocol: tcp
            destination:
              ports:
              - 30000:32767
          - action: deny
            protocol: udp
            destination:
              ports:
              - 30000:32767
        ```
        {: codeblock}

2. Wenden Sie die Calico-Netzrichtlinie vom Typ 'preDNAT' an. Es dauert ca. eine Minute, bis die Richtlinienänderungen im Cluster angewendet sind.

  ```
  calicoctl apply -f deny-kube-node-port-services.yaml
  ```
  {: pre}
