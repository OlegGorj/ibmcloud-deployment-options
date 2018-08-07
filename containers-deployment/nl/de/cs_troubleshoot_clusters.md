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



# Fehlerbehebung für Cluster und Workerknoten
{: #cs_troubleshoot_clusters}

Ziehen Sie bei der Verwendung von {{site.data.keyword.containerlong}} die folgenden Verfahren für die Fehlerbehebung Ihrer Cluster und Workerknoten in Betracht.
{: shortdesc}

Wenn Sie ein allgemeineres Problem haben, testen Sie das [Cluster-Debugging](cs_troubleshoot.html).
{: tip}

## Verbindung mit dem Infrastrukturkonto kann nicht hergestellt werden
{: #cs_credentials}

{: tsSymptoms}
Wenn Sie einen neuen Kubernetes-Cluster erstellen, wird die folgende Nachricht angezeigt.

```
Es konnte keine Verbindung zu Ihrem Konto von IBM Cloud Infrastructure (SoftLayer) hergestellt werden.
Zur Erstellung eines Standardclusters ist es erforderlich, dass Sie entweder über ein nutzungsabhängiges Konto
mit einer Verbindung zum Konto von IBM Cloud Infrastructure (SoftLayer) verfügen oder dass Sie die {{site.data.keyword.containerlong}}-CLI zum Einrichten der API-Schlüssel für {{site.data.keyword.Bluemix_notm}} Infrastructure verwendet haben.
```
{: screen}

{: tsCauses}
Nutzungsabhängige {{site.data.keyword.Bluemix_notm}}-Konten, die erstellt wurden, nachdem die automatische Kontoverknüpfung aktiviert wurde, haben bereits Zugriff auf das Portfolio von IBM Cloud Infrastructure (SoftLayer). Somit können Sie Infrastrukturressourcen für Ihren Cluster ohne weitere Konfiguration kaufen.

Benutzer mit anderen {{site.data.keyword.Bluemix_notm}}-Kontotypen oder Benutzer, die bereits ein Konto von IBM Cloud Infrastructure (SoftLayer) haben, das mit dem {{site.data.keyword.Bluemix_notm}}-Konto noch nicht verknüpft ist, müssen ihre Konten konfigurieren, um Standardcluster zu erstellen.

{: tsResolve}
Das Konfigurieren des Kontos für den Zugriff auf das Portfolio von IBM Cloud Infrastructure (SoftLayer) hängt von Ihrem Kontotyp ab. Sehen Sie sich die folgende Tabelle an, um die verfügbaren Optionen für die einzelnen Kontotypen zu finden.

|Kontotyp|Beschreibung|Verfügbare Optionen zum Erstellen eines Standardclusters|
|------------|-----------|----------------------------------------------|
|Lite-Konten|Lite-Konten können keine Cluster bereitstellen.|[Aktualisieren Sie Ihr Lite-Konto auf ein nutzungsabhängiges {{site.data.keyword.Bluemix_notm}}-Konto](/docs/account/index.html#paygo), das mit Zugriff auf das Portfolio von IBM Cloud Infrastructure (SoftLayer) eingerichtet ist.|
|Ältere nutzungsabhängige Konten|Nutzungsabhängige Konten, die erstellt wurden, bevor die automatische Kontoverknüpfung verfügbar war, haben keinen Zugriff auf das Portfolio von IBM Cloud Infrastructure (SoftLayer).<p>Wenn Sie ein Konto von IBM Cloud Infrastructure (SoftLayer) haben, können Sie dieses Konto nicht mit einem älteren nutzungsabhängigen Konto verknüpfen.</p>|<strong>Option 1:</strong> [Erstellen Sie ein neues nutzungsabhängiges Konto](/docs/account/index.html#paygo), das mit Zugriff auf das Portfolio von IBM Cloud Infrastructure (SoftLayer) eingerichtet wird. Wenn Sie diese Option auswählen, erhalten Sie zwei separate {{site.data.keyword.Bluemix_notm}}-Konten und -Rechnungen.<p>Um Ihr altes nutzungsabhängiges Konto weiterhin zu verwenden, können Sie mit Ihrem neuen nutzungsabhängigen Konto einen API-Schlüssel generieren. Mit diesem API-Schlüssel greifen Sie auf das Portfolio von IBM Cloud Infrastructure (SoftLayer) zu. Dann müssen Sie [den API-Schlüssel von IBM Cloud Infrastructure (SoftLayer) für Ihr altes nutzungsabhängiges Konto einrichten](cs_cli_reference.html#cs_credentials_set). </p><p><strong>Option 2:</strong> Wenn Sie bereits ein Konto von IBM Cloud Infrastructure (SoftLayer) haben, das Sie verwenden möchten, können Sie [Ihre Berechtigungsnachweise](cs_cli_reference.html#cs_credentials_set) für Ihr {{site.data.keyword.Bluemix_notm}}-Konto festlegen.</p><p>**Hinweis:** Wenn Sie eine manuelle Verbindung zu einem Konto von IBM Cloud-Infrastructure (SoftLayer) herstellen, werden die Berechtigungsnachweise für jede für IBM Cloud infrastructure (SoftLayer) spezifische Aktion in Ihrem {{site.data.keyword.Bluemix_notm}}-Konto verwendet. Sie müssen Sie sicherstellen, dass der von Ihnen angegebene API-Schlüssel über [ausreichende Infrastrukturberechtigungen](cs_users.html#infra_access) verfügt, damit die Benutzer Cluster erstellen und mit diesen arbeiten können.</p>|
|Abonnementkonten|Abonnementkonten werden ohne Zugriff auf das Portfolio von IBM Cloud Infrastructure (SoftLayer) eingerichtet.|<strong>Option 1:</strong> [Erstellen Sie ein neues nutzungsabhängiges Konto](/docs/account/index.html#paygo), das mit Zugriff auf das Portfolio von IBM Cloud Infrastructure (SoftLayer) eingerichtet wird. Wenn Sie diese Option auswählen, erhalten Sie zwei separate {{site.data.keyword.Bluemix_notm}}-Konten und -Rechnungen.<p>Wenn Sie Ihr Abonnementkonto weiterhin verwenden möchten, können Sie mit Ihrem neuen nutzungsabhängigen Konto einen API-Schlüssel in IBM Cloud Infrastructure (SoftLayer) generieren. Dann müssen Sie manuell [den API-Schlüssel von IBM Cloud Infrastructure (SoftLayer) für Ihr Abonnementenkonto einrichten](cs_cli_reference.html#cs_credentials_set). Beachten Sie, dass Ressourcen von IBM Cloud Infrastructure (SoftLayer) über Ihr neues nutzungsabhängiges Konto in Rechnung gestellt werden.</p><p><strong>Option 2:</strong> Wenn Sie bereits ein Konto von IBM Cloud Infrastructure (SoftLayer) haben, das Sie verwenden möchten, können Sie manuell [Berechtigungsnachweise von IBM Cloud Infrastructure (SoftLayer)](cs_cli_reference.html#cs_credentials_set) für Ihr {{site.data.keyword.Bluemix_notm}}-Konto festlegen.<p>**Hinweis:** Wenn Sie eine manuelle Verbindung zu einem Konto von IBM Cloud-Infrastructure (SoftLayer) herstellen, werden die Berechtigungsnachweise für jede für IBM Cloud infrastructure (SoftLayer) spezifische Aktion in Ihrem {{site.data.keyword.Bluemix_notm}}-Konto verwendet. Sie müssen Sie sicherstellen, dass der von Ihnen angegebene API-Schlüssel über [ausreichende Infrastrukturberechtigungen](cs_users.html#infra_access) verfügt, damit die Benutzer Cluster erstellen und mit diesen arbeiten können.</p>|
|Konten von IBM Cloud Infrastructure (SoftLayer), kein {{site.data.keyword.Bluemix_notm}}-Konto|Um einen Standardcluster zu erstellen, müssen Sie ein {{site.data.keyword.Bluemix_notm}}-Konto haben.|<p>[Erstellen Sie ein nutzungsabhängiges Konto](/docs/account/index.html#paygo), das mit Zugriff auf das Portfolio von IBM Cloud Infrastructure (SoftLayer) eingerichtet wird. Wenn Sie diese Option auswählen, wird ein Konto von IBM Cloud Infrastructure (SoftLayer) für Sie erstellt. Sie haben zwei separate Konten von IBM Cloud Infrastructure (SoftLayer) und eine jeweils separate Abrechnung.</p>|
{: caption="Erstellungsoptionen für Standardcluster nach Kontotyp" caption-side="top"}


<br />


## Firewall verhindert das Ausführen von CLI-Befehlen
{: #ts_firewall_clis}

{: tsSymptoms}
Wenn Sie die Befehle `bx`, `kubectl` oder `calicoctl` über die Befehlszeilenschnittstelle ausführen, schlagen sie fehl.

{: tsCauses}
Möglicherweise verhindern Unternehmensnetzrichtlinien den Zugriff von Ihrem lokalen System auf öffentliche Endpunkte über Proxys oder Firewalls.

{: tsResolve}
[Lassen Sie TCP-Zugriff zu, damit die CLI-Befehle ausgeführt werden können](cs_firewall.html#firewall). Für diese Task ist die [Zugriffsrichtlinie 'Administrator'](cs_users.html#access_policies) erforderlich. Überprüfen Sie Ihre aktuelle [Zugriffsrichtlinie](cs_users.html#infra_access).


## Firewall verhindert die Verbindung des Clusters mit Ressourcen
{: #cs_firewall}

{: tsSymptoms}
Wenn die Workerknoten keine Verbindung herstellen können, dann werden Sie möglicherweise eine Vielzahl unterschiedlicher Symptome feststellen. Das System zeigt eventuell eine der folgenden Nachrichten an, wenn die Ausführung des Befehls 'kubectl proxy' fehlschlägt oder wenn Sie versuchen, auf einen Service in Ihrem Cluster zuzugreifen und diese Verbindung fehlschlägt.

  ```
  Connection refused
  ```
  {: screen}

  ```
  Connection timed out
  ```
  {: screen}

  ```
  Unable to connect to the server: net/http: TLS handshake timeout
  ```
  {: screen}

Wenn Sie 'kubectl exec', kubectl attach' oder 'kubectl logs' ausführen, dann wird möglicherweise die folgende Nachricht angezeigt.

  ```
  Error from server: error dialing backend: dial tcp XXX.XXX.XXX:10250: getsockopt: connection timed out
  ```
  {: screen}

Wenn die Ausführung von 'kubectl proxy' erfolgreich verläuft, das Dashboard jedoch nicht verfügbar ist, dann wird möglicherweise die folgende Nachricht angezeigt.

  ```
  timeout on 172.xxx.xxx.xxx
  ```
  {: screen}



{: tsCauses}
Möglicherweise wurde eine weitere Firewall eingerichtet oder Sie haben die vorhandenen Firewalleinstellungen für Ihr Konto von IBM Cloud Infrastructure (SoftLayer) angepasst. {{site.data.keyword.containershort_notm}} erfordert, dass bestimmte IP-Adressen und Ports geöffnet sind, damit die Kommunikation vom Workerknoten zum Kubernetes-Master und umgekehrt möglich ist. Ein weiterer möglicher Grund kann sein, dass Ihre Workerknoten in einer Neuladen-Schleife hängen.

{: tsResolve}
[Gewähren Sie dem Cluster den Zugriff auf Infrastrukturressourcen und andere Services](cs_firewall.html#firewall_outbound). Für diese Task ist die [Zugriffsrichtlinie 'Administrator'](cs_users.html#access_policies) erforderlich. Überprüfen Sie Ihre aktuelle [Zugriffsrichtlinie](cs_users.html#infra_access).

<br />



## Zugreifen auf Workerknoten mit SSH schlägt fehl
{: #cs_ssh_worker}

{: tsSymptoms}
Es ist kein Zugriff auf Ihren Workerknoten über eine SSH-Verbindung möglich.

{: tsCauses}
Auf den Workerknoten ist SSH über Kennwort inaktiviert.

{: tsResolve}
Verwenden Sie [Dämon-Sets ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) für Vorgänge, die auf jedem Knoten ausgeführt werden müssen. Verwenden Sie Jobs für alle einmaligen Aktionen, die Sie durchführen müssen.

<br />


## `kubectl exec` und `kubectl logs` funktionieren nicht
{: #exec_logs_fail}

{: tsSymptoms}
Wenn Sie `kubectl exec` oder `kubectl logs` ausführen, wird die folgende Nachricht angezeigt.

  ```
  <worker-ip>:10250: getsockopt: connection timed out
  ```
  {: screen}

{: tsCauses}
Die OpenVPN-Verbindung zwischen dem Masterknoten und dem Workerknoten funktioniert möglicherweise nicht ordnungsgemäß.

{: tsResolve}
1. Aktivieren Sie [VLAN-Spanning](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning) für Ihr IBM Cloud-Infrastructure-Konto (SoftLayer).
2. Starten Sie den OpenVPN-Client-Pod erneut.
  ```
  kubectl delete pod -n kube-system -l app=vpn
  ```
  {: pre}
3. Wenn weiterhin dieselbe Fehlernachricht angezeigt wird, kann es sein, dass der Workerknoten, auf dem sich der VPN-Pod befindet, nicht ordnungsgemäß funktioniert. Um den VPN-Pod erneut zu starten und ihn für einen anderen Workerknoten zu terminieren, [riegeln Sie den Workerknoten, auf dem sich der VPN-Pod befindet, ab, entleeren sie ihn und starten Sie ihn neu](cs_cli_reference.html#cs_worker_reboot).

<br />


## Bindung eines Service an einen Cluster führt zu Fehler wegen identischer Namen
{: #cs_duplicate_services}

{: tsSymptoms}
Bei der Ausführung des Befehls `bx cs cluster-service-bind <cluster_name> <namespace> <service_instance_name>` wird die folgende Nachricht angezeigt:

```
Multiple services with the same name were found.
Run 'bx service list' to view available Bluemix service instances...
```
{: screen}

{: tsCauses}
Unter Umständen haben mehrere Serviceinstanzen in unterschiedlichen Regionen denselben Namen.

{: tsResolve}
Verwenden Sie im Befehl `bx cs cluster-service-bind` die GUID des Service anstelle des Serviceinstanznamens.

1. [Melden Sie sich in der Region an, in der sich die Serviceinstanz befindet, die gebunden werden soll.](cs_regions.html#bluemix_regions)

2. Rufen Sie die GUID für die Serviceinstanz ab.
  ```
  bx service show <serviceinstanzname> --guid
  ```
  {: pre}

  Ausgabe:
  ```
  Invoking 'cf service <serviceinstanzname> --guid'...
  <serviceinstanz-GUID>
  ```
  {: screen}
3. Binden Sie den Service erneut an den Cluster.
  ```
  bx cs cluster-service-bind <clustername> <namensbereich> <serviceinstanz-GUID>
  ```
  {: pre}

<br />


## Bindung eines Service an einen Cluster führt zu einem Fehler des Typs 'Service nicht gefunden'
{: #cs_not_found_services}

{: tsSymptoms}
Bei der Ausführung des Befehls `bx cs cluster-service-bind <cluster_name> <namespace> <service_instance_name>` wird die folgende Nachricht angezeigt:

```
Binding service to a namespace...
FAILED

The specified IBM Cloud service could not be found. If you just created the service, wait a little and then try to bind it again. To view available IBM Cloud service instances, run 'bx service list'. (E0023)
```
{: screen}

{: tsCauses}
Um Services an einen Cluster zu binden, müssen Sie über die Benutzerrolle des Cloud Foundry-Entwicklers für den Bereich verfügen, in dem die Serviceinstanz bereitgestellt wurde. Außerdem müssen Sie über die IAM-Editorzugriffsrechte auf {{site.data.keyword.containerlong}} verfügen. Um auf die Serviceinstanz zugreifen zu können, müssen Sie an dem Bereich angemeldet sein, in dem die Serviceinstanz bereitgestellt wurde. 

{: tsResolve}

**Als Benutzer:**

1. Melden Sie sich bei {{site.data.keyword.Bluemix_notm}} an. 
   ```
   bx login
   ```
   {: pre}
   
2. Geben Sie als Ziel die Organisation und den Bereich an, wo die Serviceinstanz bereitgestellt wird. 
   ```
   bx target -o <organisation> -s <bereich>
   ```
   {: pre}
   
3. Überprüfen Sie, dass sich sich in dem richtigen Bereich befinden, indem Sie Ihre Serviceinstanzen aufführen. 
   ```
   bx service list 
   ```
   {: pre}
   
4. Versuchen Sie, den Service erneut zu binden. Wenn der gleiche Fehler angezeigt wird, wenden Sie sich an den Kontoadministrator und überprüfen Sie, dass Sie über ausreichende Berechtigungen zum Binden von Services verfügen (siehe die folgenden Schritte für den Kontoadministrator). 

**Als Kontoadministrator:**

1. Stellen Sie sicher, dass der Benutzer, bei dem dieses Problem auftritt, über [Editorberechtigungen für {{site.data.keyword.containerlong}}](/docs/iam/mngiam.html#editing-existing-access) verfügt. 

2. Stellen Sie sicher, dass der Benutzer, bei dem dieses Problem auftritt, über die [Cloud Foundry-Entwicklerrolle für den Bereich](/docs/iam/mngcf.html#updating-cloud-foundry-access) verfügt, für den der Service bereitgestellt wird. 

3. Wenn die korrekten Berechtigungen vorhanden sind, versuchen Sie, eine andere Berechtigung zuzuweisen und dann die erforderliche Berechtigung erneut zuzuweisen. 

4. Warten Sie einige Minuten und lassen Sie dann den Benutzer versuchen, den Service erneut zu binden. 

5. Wenn dadurch das Problem nicht behoben wird, sind die IAM-Berechtigungen nicht synchron und Sie können das Problem nicht selbst lösen. [Wenden Sie sich an IBM Support](/docs/get-support/howtogetsupport.html#getting-customer-support), indem Sie ein Support-Ticket öffnen. Stellen Sie sicher, dass Sie die Cluster-ID, die Benutzer-ID und die Serviceinstanz-ID bereitstellen. 
   1. Rufen Sie die Cluster-ID ab.
      ```
      bx cs clusters
      ```
      {: pre}
      
   2. Rufen Sie die Serviceinstanz-ID ab.
      ```
      bx service show <servicename> --guid
      ```
      {: pre}


<br />



## Nachdem ein Workerknoten aktualisiert oder erneut geladen wurde, werden doppelte Knoten und Pods angezeigt
{: #cs_duplicate_nodes}

{: tsSymptoms}
Wenn Sie `kubectl get nodes` ausführen, werden doppelte Workerknoten mit dem Status **NotReady** (Nicht bereit) angezeigt. Die Workerknoten mit dem Status **NotReady** verfügen über öffentliche IP-Adressen, während die Workerknoten mit dem Status **Ready** (Bereit) private IP-Adressen haben.

{: tsCauses}
Bei älteren Clustern wurden Workerknoten anhand der öffentlichen IP-Adresse des Clusters aufgeführt. Nun werden Workerknoten über die private IP-Adresse des Clusters aufgelistet. Wenn Sie einen Knoten erneut laden oder aktualisieren, dann wird die IP-Adresse geändert, der Verweis auf die öffentliche IP-Adresse bleibt jedoch erhalten.

{: tsResolve}
Es treten keine Serviceunterbrechungen aufgrund dieser Duplikate auf, Sie können die alten Workerknotenverweise jedoch vom API-Server entfernen.

  ```
  kubectl delete node <knotenname1> <knotenname2>
  ```
  {: pre}

<br />


## Nachdem ein Workerknoten aktualisiert oder erneut geladen wurde, erhalten Anwendungen RBAC DENY-Fehler
{: #cs_rbac_deny}

{: tsSymptoms}
Nach der Aktualisierung auf Kubernetes Version 1.7 erhalten Anwendungen `RBAC DENY`-Fehler.

{: tsCauses}
Ab [Kubernetes Version 1.7](cs_versions.html#cs_v17) haben Anwendungen, die im Namensbereich `default` ausgeführt werden, aus Gründen der verbesserten Sicherheit keine Clusteradministratorberechtigungen für die Kubernetes-API mehr.

Wenn Ihre App im Namensbereich `default` ausgeführt wird, das Standardservicekonto (`default ServiceAccount`) verwendet und auf die Kubernetes-API zugreift, ist sie von dieser Kubernetes-Änderung betroffen. Weitere Informationen finden Sie in der [Kubernetes-Dokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/admin/authorization/rbac/#upgrading-from-15).

{: tsResolve}
Führen Sie zunächst den folgenden Schritt aus: [Geben Sie als Zile der CLI](cs_cli_install.html#cs_cli_configure) Ihren Cluster an.

1.  **Temporäre Aktion**: Wenn Sie Ihre App-RBAC-Richtlinien aktualisieren, möchten Sie möglicherweise vorübergehend zur vorherigen Clusterrollenbindung (`ClusterRoleBinding`) für das Standardservicekonto (`default ServiceAccount`) im Namensbereich `default` zurückkehren.

    1.  Kopieren Sie die folgende `.yaml`-Datei.

        ```yaml
        kind: ClusterRoleBinding
        apiVersion: rbac.authorization.k8s.io/v1beta1
        metadata:
         name: admin-binding-nonResourceURLSs-default
        subjects:
          - kind: ServiceAccount
            name: default
            namespace: default
        roleRef:
         kind: ClusterRole
         name: admin-role-nonResourceURLSs
         apiGroup: rbac.authorization.k8s.io
        ---
        kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
 name: admin-binding-resourceURLSs-default
subjects:
          - kind: ServiceAccount
      name: default
      namespace: default
  roleRef:
   kind: ClusterRole
   name: admin-role-resourceURLSs
   apiGroup: rbac.authorization.k8s.io
        ```

    2.  Wenden Sie die `.yaml`-Dateien auf Ihr Cluster an.

        ```
        kubectl apply -f DATEINAME
        ```
        {: pre}

2.  [Erstellen Sie RBAC-Autorisierungsressourcen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview), um den Administratorzugriff für `ClusterRoleBinding` zu aktualisieren.

3.  Wenn Sie eine temporäre Clusterrollenbindung erstellt haben, entfernen Sie sie.

<br />


## Zugriff auf einen Pod auf einem Workerknoten schlägt mit einer Zeitlimitüberschreitung fehl
{: #cs_nodes_duplicate_ip}

{: tsSymptoms}
Sie haben einen Workerknoten in Ihrem Cluster gelöscht und dann einen Workerknoten hinzugefügt. Wenn Sie einen Pod oder einen Kubernetes-Service bereitgestellt haben, kann die Ressource nicht auf den neu erstellten Workerknoten zugreifen, und die Verbindung überschreitet das Zeitlimit.

{: tsCauses}
Wenn Sie einen Workerknoten aus Ihrem Cluster löschen und dann einen Workerknoten hinzufügen, kann es sein, dass der neue Workerknoten der privaten IP-Adresse des gelöschten Workerknotens zugeordnet wird. Calico verwendet diese private IP-Adresse als Tag und versucht weiterhin, den gelöschten Knoten zu erreichen.

{: tsResolve}
Aktualisieren Sie die Referenz der privaten IP-Adresse manuell, um auf den korrekten Knoten zu zeigen.

1.  Stellen Sie sicher, dass Sie zwei Workerknoten mit derselben **privaten IP**-Adresse haben. Notieren Sie sich die **private IP** und die **ID** des gelöschten Workers.

  ```
  bx cs workers <CLUSTERNAME>
  ```
  {: pre}

  ```
  ID                                                 Public IP       Private IP       Machine Type   State     Status   Location   Version
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w1   169.xx.xxx.xxx  10.xxx.xx.xxx    b2c.4x16       normal    Ready    dal10      1.9.7
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w2   169.xx.xxx.xxx  10.xxx.xx.xxx    b2c.4x16       deleted    -       dal10      1.9.7
  ```
  {: screen}

2.  Installieren Sie die [Calico-CLI](cs_network_policy.html#adding_network_policies).
3.  Listen Sie die verfügbaren Workerknoten in Calico auf. Ersetzen Sie <dateipfad> durch den lokalen Pfad zur Calico-Konfigurationsdatei.

  ```
  calicoctl get nodes --config=filepath/calicoctl.cfg
  ```
  {: pre}

  ```
  NAME
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w1
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w2
  ```
  {: screen}

4.  Löschen Sie den doppelten Workerknoten in Calico. Ersetzen Sie KNOTEN-ID durch die Workerknoten-ID.

  ```
  calicoctl delete node KNOTEN-ID --config=<dateipfad>/calicoctl.cfg
  ```
  {: pre}

5.  Starten Sie den Workerknoten, der nicht gelöscht wurde, neu.

  ```
  bx cs worker-reboot CLUSTER-ID KNOTEN-ID
  ```
  {: pre}


Der gelöschte Knoten wird nicht mehr in Calico aufgelistet.

<br />


## Cluster verbleibt im Wartestatus ('Pending')
{: #cs_cluster_pending}

{: tsSymptoms}
Wenn Sie Ihren Cluster bereitstellen, bleibt dieser im Wartestatus und startet nicht.

{: tsCauses}
Wenn Sie den Cluster gerade erst erstellt haben, werden die Workerknoten möglicherweise noch konfiguriert. Wenn Sie bereits eine Weile warten, ist das VLAN möglicherweise nicht gültig.

{: tsResolve}

Sie können eine der folgenden Lösungen ausprobieren:
  - Überprüfen Sie den Status Ihres Clusters, indem Sie den folgenden Befehl ausführen: `bx cs clusters`. Überprüfen Sie dann, dass Ihre Workerknoten bereitgestellt wurden, indem Sie den folgenden Befehl ausgeben: `bx cs workers <cluster_name>`.
  - Überprüfen Sie, ob Ihr VLAN gültig ist. Damit ein VLAN gültig ist, muss es einer Infrastruktur zugeordnet sein, die einen Worker mit lokalem Plattenspeicher hosten kann. Sie können [Ihre VLANs auflisten](/docs/containers/cs_cli_reference.html#cs_vlans), indem Sie den Befehl `bx cs vlans <location>` ausführen. Wenn das VLAN nicht in der Liste aufgeführt wird, dann ist es nicht gültig. Wählen Sie ein anderes VLAN aus.

<br />


## Pods verweilen in  im Status 'Pending' (Anstehend)
{: #cs_pods_pending}

{: tsSymptoms}
Beim Ausführen von `kubectl get pods` verbleiben Pods weiterhin im Status **Pending** (Anstehend).

{: tsCauses}
Wenn Sie den Kubernetes-Cluster gerade erst erstellt haben, werden die Workerknoten möglicherweise noch konfiguriert. Falls dieser Cluster bereits vorhanden ist, steht unter Umständen nicht ausreichend Kapazität für die Bereitstellung des Pods in Ihrem Cluster zur Verfügung.

{: tsResolve}
Für diese Task ist die [Zugriffsrichtlinie 'Administrator'](cs_users.html#access_policies) erforderlich. Überprüfen Sie Ihre aktuelle [Zugriffsrichtlinie](cs_users.html#infra_access).

Führen Sie den folgenden Befehl aus, wenn Sie den Kubernetes-Cluster gerade erst erstellt haben. Warten Sie, bis die Workerknoten initialisiert werden.

```
kubectl get nodes
```
{: pre}

Falls dieser Cluster bereits vorhanden ist, prüfen Sie Ihre Clusterkapazität.

1.  Legen Sie die Standardportnummer für den Proxy fest.

  ```
  kubectl proxy
  ```
   {: pre}

2.  Öffnen Sie das Kubernetes-Dashboard.

  ```
  http://localhost:8001/ui
  ```
  {: pre}

3.  Überprüfen Sie, ob in Ihrem Cluster ausreichend Kapazität verfügbar ist, um Ihren Pod bereitzustellen.

4.  Falls Ihr Cluster nicht genügend freie Kapazität bietet, fügen Sie einen weiteren Workerknoten zu ihm hinzu.

    ```
    bx cs worker-add <clustername_oder_-id> 1
    ```
    {: pre}

5.  Falls Ihre Pods auch weiterhin im Status **Pending** (Anstehend) verweilen, obwohl der Workerknoten voll bereitgestellt wurde, ziehen Sie die [Kubernetes-Dokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-pod-replication-controller/#my-pod-stays-pending) zurate, um die Ursache für den andauernden Status 'Pending' Ihres Pods zu ermitteln und den Fehler zu beheben.

<br />




## Container werden nicht gestartet
{: #containers_do_not_start}

{: tsSymptoms}
Die Pods wurden erfolgreich auf den Clustern bereitgestellt, aber die Container können nicht gestartet werden.

{: tsCauses}
Die Container werden möglicherweise nicht gestartet, wenn die Registry-Quote erreicht ist.

{: tsResolve}
[Geben Sie Speicherplatz in {{site.data.keyword.registryshort_notm}} frei.](../services/Registry/registry_quota.html#registry_quota_freeup)

<br />



## Ein Helm-Diagramm kann nicht mit aktualisierten Konfigurationswerten installiert werden.
{: #cs_helm_install}

{: tsSymptoms}
Wenn Sie versuchen, ein aktualisiertes Helm-Diagramm zu installieren, indem Sie den Befehl `helm install -f config.yaml --namespace=kube-system --name=<releasename> ibm/<diagrammname>` ausführen, erhalten Sie die Fehlernachricht `Error: failed to download "ibm/<chart_name>"`.

{: tsCauses}
Die URL für das {{site.data.keyword.Bluemix_notm}}-Repository in Ihrer Helm-Instanz ist möglicherweise nicht korrekt.

{: tsResolve}
Gehen Sie wie folgt vor, um Fehler in Ihrem Helm-Diagramm zu beheben:

1. Listen Sie die Repository auf, die aktuell in Ihrer Helm-Instanz verfügbar sind.

    ```
    helm repo list
    ```
    {: pre}

2. Überprüfen Sie in der Ausgabe, dass die URL für das {{site.data.keyword.Bluemix_notm}}-Repository, `ibm`, wie folgt lautet: `https://registry.bluemix.net/helm/ibm`.

    ```
    NAME    URL
    stable  https://kubernetes-charts.storage.googleapis.com
    local   http://127.0.0.1:8888/charts
    ibm     https://registry.bluemix.net/helm/ibm
    ```
    {: screen}

    * Falls die URL falsch ist:

        1. Entfernen Sie das {{site.data.keyword.Bluemix_notm}}-Repository.

            ```
            helm repo remove ibm
            ```
            {: pre}

        2. Fügen Sie das {{site.data.keyword.Bluemix_notm}}-Repository erneut hinzu.

            ```
            helm repo add ibm  https://registry.bluemix.net/helm/ibm
            ```
            {: pre}

    * Wenn die URL richtig ist, rufen Sie die aktuellsten Aktualisierung vom Repository ab:

        ```
        helm repo update
        ```
        {: pre}

3. Installieren Sie das Helm-Diagramm mit Ihren Aktualisierungen.

    ```
    helm install -f config.yaml --namespace=kube-system --name=<releasename> ibm/<diagrammname>
    ```
    {: pre}

<br />


## Hilfe und Unterstützung anfordern
{: #ts_getting_help}

Haben Sie noch immer Probleme mit Ihrem Cluster?
{: shortdesc}

-   [Überprüfen Sie auf der {{site.data.keyword.Bluemix_notm}}-Statusseite ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/bluemix/support/#status), ob {{site.data.keyword.Bluemix_notm}} verfügbar ist.
-   Veröffentlichen Sie eine Frage im [{{site.data.keyword.containershort_notm}}-Slack ![External link icon](../icons/launch-glyph.svg "Symbol für externen Link")](https://ibm-container-service.slack.com).

    Wenn Sie keine IBM ID für Ihr {{site.data.keyword.Bluemix_notm}}-Konto verwenden, [fordern Sie eine Einladung](https://bxcs-slack-invite.mybluemix.net/) zu diesem Slack an.
    {: tip}
-   Suchen Sie in entsprechenden Foren, ob andere Benutzer auf das gleiche Problem gestoßen sind. Versehen Sie Ihre Fragen in den Foren mit Tags, um sie für das Entwicklungsteam von {{site.data.keyword.Bluemix_notm}} erkennbar zu machen.

    -   Wenn Sie technische Fragen zur Entwicklung oder Bereitstellung von Clustern oder Apps mit {{site.data.keyword.containershort_notm}} haben, veröffentlichen Sie Ihre Frage auf [Stack Overflow ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) und versehen Sie sie mit den Tags `ibm-cloud`, `kubernetes` und `containers`.
    -   Verwenden Sie für Fragen zum Service und zu ersten Schritten das Forum [IBM developerWorks dW Answers ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix). Geben Sie die Tags `ibm-cloud` und `containers` an.
    Weitere Details zur Verwendung der Foren finden Sie unter [Hilfe anfordern](/docs/get-support/howtogetsupport.html#using-avatar).

-   Wenden Sie sich an den IBM Support, indem Sie ein Ticket öffnen. Informationen zum Öffnen eines IBM Support-Tickets oder zu Supportstufen und zu Prioritätsstufen von Tickets finden Sie unter [Support kontaktieren](/docs/get-support/howtogetsupport.html#getting-customer-support).

{: tip}
Geben Sie beim Melden eines Problems Ihre Cluster-ID an. Führen Sie den Befehl `bx cs clusters` aus, um Ihre Cluster-ID abzurufen.

