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



# Versionsinformationen und Aktualisierungsaktion
{: #cs_versions}

## Kubernetes-Versionstypen
{: #version_types}

{{site.data.keyword.containerlong}} unterstützt momentan mehrere Versionen von Kubernetes. Wenn die aktuellste Version (n) freigegeben wird, werden bis zu 2 Versionen davor (n-2) unterstützt. Versionen, die mehr als zwei Versionen älter sind, als die aktuellsten Version (n-3) werden zuerst nicht mehr verwendet und dann nicht weiter unterstützt.
{:shortdesc}

Die folgenden Kubernetes-Version werden gegenwärtig unterstützt:

- Aktuell: 1.10.1
- Standard: 1.9.7
- Unterstützt: 1.8.11

**Veraltete Versionen**: Wenn Cluster auf einem veralteten Kubernetes ausgeführt werden, haben Sie 30 Tage Zeit, um eine unterstützte Kubernetes-Version zu überprüfen und auf diese neuere Version zu aktualisieren, bevor die veraltete Version nicht mehr unterstützt wird. Während des Zeitraums der Einstellung der Unterstützung können Sie eingeschränkte Befehle in Ihren Clustern ausführen, um Worker hinzuzufügen, erneut zu laden und um das Cluster zu aktualisieren. In der veralteten Version können Sie keine neuen Cluster erstellen.

**Nicht unterstützte Versionen**: Wenn Sie Cluster unter einer Kubernetes-Version ausführen, die nicht unterstützt wird, [überprüfen Sie potenzielle Auswirkungen](#version_types) auf Updates und [aktualisieren Sie den Cluster](cs_cluster_update.html#update) sofort, damit Sie weiterhin wichtige Sicherheitsupdates und Support erhalten.

Führen Sie den folgenden Befehl aus, um die Serverversion eines Clusters zu überprüfen.

```
kubectl version  --short | grep -i server
```
{: pre}

Beispielausgabe:

```
Server Version: v1.9.7+9d6e0610086578
```
{: screen}


## Aktualisierungstypen
{: #update_types}

Für Ihr Kubernetes-Cluster gibt es drei Typen von Aktualisierungen: Hauptversion, Nebenversion und Patch.
{:shortdesc}

|Aktualisierungstyp|Beispiel für Versionskennzeichnungen|Aktualisierung durch|Auswirkung
|-----|-----|-----|-----|
|Hauptversion|1.x.x|Sie|Operationsänderungen für Cluster, darunter Sripts oder Bereitstellungen.|
|Nebenversion|x.9.x|Sie|Operationsänderungen für Cluster, darunter Sripts oder Bereitstellungen.|
|Patch|x.x.4_1510|IBM und Sie|Kubernetes-Patches und andere Aktualisierungen von {{site.data.keyword.Bluemix_notm}} Provider-Komponenten, z. B. Sicherheits- und Betriebssystempatches. IBM aktualisiert die Master automatisch, aber Sie wenden Patches auf Workerknoten an.|
{: caption="Auswirkungen auf Kubernetes-Aktualisierungen" caption-side="top"}

Sobald Aktualisierungen verfügbar sind, werden Sie benachrichtigt, wenn Sie Informationen zu den Workerknoten anzeigen, z. B. mit den Befehlen `bx cs workers <cluster>` oder `bx cs worker-get <cluster> <worker>`.
-  **Aktualisierungen von Haupt- und Nebenversionen**: Zuerst [aktualisieren Sie Ihren Masterknoten](cs_cluster_update.html#master) und anschließend [die Workerknoten](cs_cluster_update.html#worker_node). 
   - Standardmäßig ist für einen Kubernetes-Master eine Aktualisierung über über drei und mehr Nebenversionen hinweg nicht möglich. Wenn die aktuelle Masterversion beispielsweise 1.5 ist und Sie eine Aktualisierung auf 1.8 durchführen möchten, müssen Sie zunächst auf Version 1.7 aktualisieren. Sie können die gewünschte Aktualisierung zwar erzwingen, jedoch kann eine Aktualisierung über mehr als zwei Nebenversionen hinweg zu nicht erwarteten Ergebnissen führen.
   - Wenn Sie eine `kubectl`-CLI-Version verwenden, die nicht wenigstens mit der Version `major.minor` Ihrer Cluster übereinstimmt, kann dies zu unerwarteten Ergebnissen führen. Stellen Sie sicher, dass Ihr Kubernetes-Cluster und die [CLI-Versionen](cs_cli_install.html#kubectl) immer auf dem neuesten Stand sind.
-  **Patchaktualisierungen**: Überprüfen Sie einmal im Monat, ob eine Aktualisierung verfügbar ist, und geben Sie den [Befehl](cs_cli_reference.html#cs_worker_update) `bx cs worker-update` oder den [Befehl](cs_cli_reference.html#cs_worker_reload) `bx cs worker-reload` aus, um diese Sicherheits- und Betriebssystempatches anzuwenden. Weitere Informationen finden Sie im [Änderungsprotokoll der Version](cs_versions_changelog.html).

<br/>

Diese Informationen fassen die Aktualisierungen zusammen, die sich voraussichtlich auf die bereitgestellten Apps auswirken, wenn Sie einen Cluster von einer vorherigen Version auf eine neue Version aktualisieren.
-  Version 1.10, [Migrationsaktionen](#cs_v110).
-  Version 1.9, [Migrationsaktionen](#cs_v19).
-  Version 1.8, [Migrationsaktionen](#cs_v18).
-  [Archiv](#k8s_version_archive) veralteter oder nicht unterstützter Versionen.

<br/>

Eine vollständige Liste von Änderungen finden Sie im Rahmen der folgenden Informationen: 
* [Kubernetes-Änderungsprotokoll ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md).
* [Änderungsprotokoll der IBM Versionen](cs_versions_changelog.html).

## Version 1.10
{: #cs_v110}

<p><img src="images/certified_kubernetes_1x10.png" style="padding-right: 10px;" align="left" alt="Diese Markierung zeigt die Kubernetes Version 1.10-Zertifizierung für IBM Cloud Container Service an."/> {{site.data.keyword.containerlong_notm}} ist ein zertifiziertes Kubernetes-Produkt für Version 1.10 unter dem CNCF Kubernetes Software Conformance Certification Program. _Kubernetes® ist eine eingetragene Marke von The Linux Foundation in den USA und anderen Ländern und wird entsprechend einer Lizenz von The Linux Foundation verwendet._</p>

Überprüfen Sie Änderungen, die Sie möglicherweise bei einer Aktualisierung von der vorherigen Version auf die Kubernetes-Version 1.10 vornehmen müssen.

**Wichtig**: Bevor Sie eine erfolgreiche Aktualisierung auf Kubernetes 1.10 vornehmen können, müssen Sie zunächst die im Abschnitt mit den [Vorbereitungen zur Aktualisierung auf Calico Version 3](#110_calicov3) aufgeführten Schritte durchführen.

<br/>

### Vor Master aktualisieren
{: #110_before}

<table summary="Kubernetes-Aktualisierungen für Version 1.10">
<caption>Vorzunehmende Änderungen vor der Aktualisierung des Masters auf Kubernetes 1.10</caption>
<thead>
<tr>
<th>Typ</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico Version 3</td>
<td>Bei einer Aktualisierung auf Kubernetes Version 1.10 wird auch Calico von Version 2.6.5 auf Version 3.1.1 aktualisiert. <strong>Wichtig</strong>: Bevor Sie eine erfolgreiche Aktualisierung auf Kubernetes 1.10 vornehmen können, müssen Sie zunächst die im Abschnitt mit den [Vorbereitungen zur Aktualisierung auf Calico Version 3](#110_calicov3) aufgeführten Schritte durchführen.</td>
</tr>
<tr>
<td>Netzrichtlinie für das Kubernetes-Dashboard</td>
<td>In Kubernetes 1.10 blockiert die Netzrichtlinie <code>kubernetes-dashboard</code> im Namensbereich <code>kube-system</code> für alle Pods den Zugriff auf das Kubernetes-Dashboard. Dies hat jedoch <strong>keinerlei</strong> Auswirkungen auf den möglichen Zugriff auf das Dashboard über die {{site.data.keyword.Bluemix_notm}}-Konsole oder mithilfe von <code>kubectl proxy</code>. Wenn ein Pod Zugriff auf das Dashboard benötigt, können Sie einem Namensbereich die Bezeichnung <code>kubernetes-dashboard-policy: allow</code> hinzufügen und anschließend den Pod für den Namensbereich bereitstellen.</td>
</tr>
<tr>
<td>Kubelet-API-Zugriff</td>
<td>Die Kubelet-API-Autorisierung wird jetzt an den <code>Kubernetes-API-Server</code> delegiert. Der Zugriff auf die Kubelet-API basiert auf Clusterrollen (<code>ClusterRoles</code>), die die Berechtigung zum Zugriff auf <strong>Knoten</strong>-Unterressourcen einräumen. Standardmäßig verfügt Kubernetes Heapster über <code>ClusterRole</code> und <code>ClusterRoleBinding</code>. Wenn die Kubelet-API von anderen Benutzern oder Apps verwendet wird, müssen Sie diesen die Berechtigung für die Verwendung der API einräumen. Weitere Informationen zur [Kubelet-Autorisierung ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/admin/kubelet-authentication-authorization/#kubelet-authorization) finden Sie in der Kubernetes-Dokumentation.</td>
</tr>
<tr>
<td>Cipher-Suites</td>
<td>Die unterstützten Cipher-Suites für den <code>Kubernetes-API-Server</code> und die Kubelet-API sind nun auf eine Teilmenge mit einer starken Verschlüsselung (128 Bit oder höher) begrenzt. Wenn Sie über eine bestehende Automatisierung oder bestehende Ressourcen verfügen, die eine schwächere Verschlüsselung verwenden und auf die Kommunikation mit dem <code>Kubernetes-API-Server</code> oder der Kubelet-API angewiesen sind, aktivieren Sie eine stärkere Verschlüsselungsunterstützung, bevor Sie den Master aktualisieren.</td>
</tr>
</tbody>
</table>

### Nach Master aktualisieren
{: #110_after}

<table summary="Kubernetes-Aktualisierungen für Version 1.10">
<caption>Vorzunehmende Änderungen nach der Aktualisierung des Masters auf Kubernetes 1.10</caption>
<thead>
<tr>
<th>Typ</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico Version 3</td>
<td>Bei einer Aktualisierung des Clusters werden alle vorhandenen Calico-Daten, die auf den Cluster angewendet werden, automatisch für die Verwendung der Calico Version 3-Syntax migriert. Um Calico-Ressourcen mit der Calico Version 3-Syntax anzuzeigen, hinzuzufügen oder zu ändern, aktualisieren Sie Ihre [Calico-CLI-Konfiguration auf Version 3.1.1](#110_calicov3).</td>
</tr>
<tr>
<td>Externe IP-Adresse (<code>ExternalIP</code>) des Knotens</td>
<td>Das Feld <code>ExternalIP</code> eines Knotens ist jetzt auf den Wert der öffentlichen IP-Adresse des Knotens festgelegt. Überprüfen und aktualisieren Sie alle Ressourcen, die von diesem Wert abhängen.</td>
</tr>
<tr>
<td><code>kubectl port-forward</code></td>
<td>Wenn Sie jetzt den Befehl <code>kubectl port-forward</code> verwenden, wird das Flag <code>-p</code> nicht weiter unterstützt. Wenn Ihre Scripts das bisherige Verhalten erwarten, aktualisieren Sie die Scripts und ersetzen Sie das Flag <code>-p</code> durch den Podnamen.</td>
</tr>
<tr>
<td>Schreibgeschützte API-Datenträger</td>
<td>`secret`, `configMap`, `downwardAPI` und projizierte Datenträger werden jetzt nur schreibgeschützt angehängt.
Früher durften Apps Daten auf diese Datenträger schreiben, die vom System möglicherweise nicht automatisch zurückgesetzt werden. Diese Migrationsaktion ist erforderlich,
um die Sicherheitslücke [CVE-2017-1002102![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=2017-1002102) zu schließen.
Wenn Ihre Apps das bisherige unsichere Verhalten erwarten, ändern Sie sie entsprechend.</td>
</tr>
</tbody>
</table>

### Vorbereitungen für die Aktualisierung auf Calico Version 3
{: #110_calicov3}

Sie müssen zunächst sicherstellen, dass Ihr Cluster-Master und alle Workerknoten Kubernetes-Version 1.8 oder später ausführen und über mindestens einen Workerknoten verfügen.

**Wichtig**: Bereiten Sie sich auf die Calico Version 3-Aktualisierung vor, bevor Sie den Master aktualisieren. Während der Aktualisierung des Masters auf Kubernetes Version 1.10 werden keine neuen Pods oder neue Kubernetes- oder Calico-Netzrichtlinien terminiert. Die Zeit, während der bei der Aktualisierung keine neue Terminierung möglich ist, variiert. Für kleine Cluster kann der Vorgang einige Minuten dauern, wobei für jede 10 Knoten ein paar Minuten mehr benötigt werden. Bestehende Netzrichtlinien und Pods werden weiterhin ausgeführt.

1.  Stellen Sie sicher, dass die Calico-Pods in einwandfreiem Zustand sind.
    ```
    kubectl get pods -n kube-system -l k8s-app=calico-node -o wide
    ```
    {: pre}
    
2.  Wenn ein Pod nicht den Status **Aktiv** aufweist, löschen Sie den Pod und warten Sie, bis er den Status **Aktiv** einnimmt, bevor Sie fortfahren.

3.  Wenn Sie Calico-Richtlinien oder andere Calico-Ressourcen automatisch generieren, aktualisieren Sie Ihre Automatisierungstools, um diese Ressourcen mit der [Calico Version 3-Syntax ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/) zu generieren.

4.  Wenn Sie [strongSwan](cs_vpn.html#vpn-setup) für die VPN-Konnektivität verwenden, funktioniert das strongSwan 2.0.0-Helm-Diagramm nicht mit Calico Version 3 oder Kubernetes 1.10. [Aktualisieren Sie strongSwan](cs_vpn.html#vpn_upgrade) auf das 2.1.0-Helm-Diagramm, das abwärtskompatibel mit Calico 2.6 und Kubernetes 1.7, 1.8 und 1.9 ist.

5.  [Aktualisieren Sie den Cluster-Master auf Kubernetes Version 1.10](cs_cluster_update.html#master).

<br />


## Version 1.9
{: #cs_v19}

<p><img src="images/certified_kubernetes_1x9.png" style="padding-right: 10px;" align="left" alt="Diese Markierung zeigt die Kubernetes Version 1.9-Zertifizierung für IBM Cloud Container Service an. "/> {{site.data.keyword.containerlong_notm}} ist ein zertifiziertes Kubernetes-Produkt für Version 1.9 unter dem CNCF Kubernetes Software Conformance Certification Program. _Kubernetes® ist eine eingetragene Marke von The Linux Foundation in den USA und anderen Ländern und wird entsprechend einer Lizenz von The Linux Foundation verwendet._</p>

Überprüfen Sie Änderungen, die Sie möglicherweise bei einer Aktualisierung von der vorherigen Version auf die Kubernetes-Version 1.9 vornehmen müssen.

<br/>

### Vor Master aktualisieren
{: #19_before}

<table summary="Kubernetes-Aktualisierungen für Version 1.9">
<caption>Vorzunehmende Änderungen vor der Aktualisierung des Masters auf Kubernetes 1.9</caption>
<thead>
<tr>
<th>Typ</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Webhook-Zugangs-API</td>
<td>Die Zugangs-API, die verwendet wird, wenn der API-Server Webhooks für die Zugangssteuerung aufruft, wird von <code>admission.v1alpha1</code> nach <code>admission.v1beta1</code> verschoben. <em>Sie müssen alle vorhandenen Webhooks löschen, bevor Sie das Upgrade für Ihren Cluster durchführen</em>, und die Webhook-Konfigurationsdateien so aktualisieren, dass die aktuelle API verwendet wird. Diese Änderung ist nicht abwärtskompatibel.</td>
</tr>
</tbody>
</table>

### Nach Master aktualisieren
{: #19_after}

<table summary="Kubernetes-Aktualisierungen für Version 1.9">
<caption>Vorzunehmende Änderungen nach der Aktualisierung des Masters auf Kubernetes 1.9</caption>
<thead>
<tr>
<th>Typ</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>`kubectl` - Ausgabe</td>
<td>Wenn Sie im Befehl `kubectl` die Option `-o custom-columns` angeben und die Spalte in dem Objekt nicht gefunden wird, wird jetzt die Ausgabe `<none>`.<br>
Bisher schlug die Operation fehl und die Fehlernachricht `xxx wurde nicht gefunden` wurde angezeigt. Wenn Ihre Scripts das bisherige Verhalten erwarten, aktualisieren Sie die Scripts.</td>
</tr>
<tr>
<td>`kubectl patch`</td>
<td>Wenn beim Korrigieren der Ressource keine Änderungen vorgenommen werden, schlägt der Befehl `kubectl patch` jetzt mit `exit code 1` fehl. Wenn Ihre Scripts das bisherige Verhalten erwarten, aktualisieren Sie die Scripts.</td>
</tr>
<tr>
<td>Berechtigungen für das Kubernetes-Dashboard</td>
<td>Benutzer müssen sich mit Ihren Berechtigungsnachweisen beim Kubernetes-Dashboard anmelden, um Clusterressourcen anzuzeigen. Die RBAC-Standardberechtigung `ClusterRoleBinding` für das Kubernetes-Dashboard wurde entfernt. Entsprechende Anweisungen finden Sie unter [Kubernetes-Dashboard starten](cs_app.html#cli_dashboard).</td>
</tr>
<tr>
<td>Schreibgeschützte API-Datenträger</td>
<td>`secret`, `configMap`, `downwardAPI` und projizierte Datenträger werden jetzt nur schreibgeschützt angehängt.
Früher durften Apps Daten auf diese Datenträger schreiben, die vom System möglicherweise nicht automatisch zurückgesetzt werden. Diese Migrationsaktion ist erforderlich,
um die Sicherheitslücke [CVE-2017-1002102](https://cve.mitre.org/cgi-bin/cvename.cgi?name=2017-1002102) zu schließen.
Wenn Ihre Apps das bisherige unsichere Verhalten erwarten, ändern Sie sie entsprechend.</td>
</tr>
<tr>
<td>Taints und Tolerierungen</td>
<td>Die Taints `node.alpha.kubernetes.io/notReady` und `node.alpha.kubernetes.io/unreachable` wurden in `node.kubernetes.io/not-ready` und `node.kubernetes.io/unreachable` geändert.<br>
Die Aktualisierung der Taints erfolgt zwar automatisch, aber die Tolerierungen dieser Taints müssen manuell aktualisiert werden. Stellen Sie für jeden Namensbereich mit Ausnahme von `ibm-system` und `kube-system` fest, ob die Tolerierungen geändert werden müssen:<br>
<ul><li><code>kubectl get pods -n &lt;namensbereich&gt; -o yaml | grep "node.alpha.kubernetes.io/notReady" && echo "Action required"</code></li><li>
<code>kubectl get pods -n &lt;namensbereich&gt; -o yaml | grep "node.alpha.kubernetes.io/unreachable" && echo "Action required"</code></li></ul><br>
Wenn `Action required` zurückgegeben wird, ändern Sie die Pod-Tolerierungen entsprechend.</td>
</tr>
<tr>
<td>Webhook-Zugangs-API</td>
<td>Wenn Sie vor dem Aktualisieren des Clusters vorhandene Webhooks gelöscht hatten, erstellen Sie neue Webhooks.</td>
</tr>
</tbody>
</table>

<br />



## Version 1.8
{: #cs_v18}

<p><img src="images/certified_kubernetes_1x8.png" style="padding-right: 10px;" align="left" alt="Diese Markierung zeigt die Kubernetes Version 1.8-Zertifizierung für IBM Cloud Container Service an."/> {{site.data.keyword.containerlong_notm}} ist ein zertifiziertes Kubernetes-Produkt für Version 1.8 unter dem CNCF Kubernetes Software Conformance Certification Program. _Kubernetes® ist eine eingetragene Marke von The Linux Foundation in den USA und anderen Ländern und wird entsprechend einer Lizenz von The Linux Foundation verwendet._</p>

Überprüfen Sie Änderungen, die Sie möglicherweise bei einer Aktualisierung von der vorherigen Version auf die Kubernetes-Version 1.8 vornehmen müssen.

<br/>

### Vor Master aktualisieren
{: #18_before}

<table summary="Kubernetes-Aktualisierungen für Version 1.8">
<caption>Vorzunehmende Änderungen vor der Aktualisierung des Masters auf Kubernetes 1.8</caption>
<thead>
<tr>
<th>Typ</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Keine</td>
<td>Keine Änderungen vor der Aktualisierung des Masters erforderlich</td>
</tr>
</tbody>
</table>

### Nach Master aktualisieren
{: #18_after}

<table summary="Kubernetes-Aktualisierungen für Version 1.8">
<caption>Vorzunehmende Änderungen nach der Aktualisierung des Masters auf Kubernetes 1.8</caption>
<thead>
<tr>
<th>Typ</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Anmeldung beim Kubernetes-Dashboard</td>
<td>Die URL für den Zugriff auf das Kubernetes-Dashboard in Version 1.8 wurde geändert und für den Anmeldeprozess ist ein neuer Authentifizierungsschritt erforderlich. Weitere Informationen finden Sie im Abschnitt zum [Zugriff auf das Kubernetes-Dashboard](cs_app.html#cli_dashboard).</td>
</tr>
<tr>
<td>Berechtigungen für das Kubernetes-Dashboard</td>
<td>Damit sich Benutzer mit ihren Berechtigungsnachweisen anmelden müssen, um Clusterressourcen in Version 1.8 anzeigen zu können, entfernen Sie die RBAC-Berechtigung 'ClusterRoleBinding' aus Version 1.7. Führen Sie dazu `kubectl delete clusterrolebinding -n kube-system kubernetes-dashboard` aus.</td>
</tr>
<tr>
<td>`kubectl delete`</td>
<td>Mit dem Befehl `kubectl delete` werden nicht länger API-Objekte für Arbeitslasten, wie Pods, vor dem Löschen des Objekts nach unten skaliert. Wenn das Objekt nach unten skaliert werden soll, verwenden Sie den Befehl [`kubectl scale ` ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/reference/kubectl/overview/#scale), bevor Sie das Objekt löschen.</td>
</tr>
<tr>
<td>`kubectl run`</td>
<td>Der Befehl `kubectl run` muss anstelle von durch Kommas getrennten Argumenten mehrere Flags für `--env` verwenden. Führen Sie beispielsweise <code>kubectl run --env &lt;x&gt;=&lt;y&gt; --env &lt;z&gt;=&lt;a&gt;</code> und nicht <code>kubectl run --env &lt;x&gt;=&lt;y&gt;,&lt;z&gt;=&lt;a&gt;</code> aus. </td>
</tr>
<tr>
<td>`kubectl stop`</td>
<td>Der Befehl `kubectl stop` ist nicht mehr verfügbar.</td>
</tr>
<tr>
<td>Schreibgeschützte API-Datenträger</td>
<td>`secret`, `configMap`, `downwardAPI` und projizierte Datenträger werden jetzt nur schreibgeschützt angehängt.
Früher durften Apps Daten auf diese Datenträger schreiben, die vom System möglicherweise nicht automatisch zurückgesetzt werden. Diese Migrationsaktion ist erforderlich,
um die Sicherheitslücke [CVE-2017-1002102](https://cve.mitre.org/cgi-bin/cvename.cgi?name=2017-1002102) zu schließen.
Wenn Ihre Apps das bisherige unsichere Verhalten erwarten, ändern Sie sie entsprechend.</td>
</tr>
</tbody>
</table>

<br />



## Archiv
{: #k8s_version_archive}

### Version 1.7 (veraltet)
{: #cs_v17}

**Seit dem 22. Mai 2018 gelten {{site.data.keyword.containershort_notm}}-Cluster, die Kubernetes Version 1.7 ausführen, als veraltet**. Cluster der Version 1.7 können nach dem 21. Juni 2018 keine Sicherheitsupdates oder Unterstützung mehr erhalten, bis sie auf die nächste, aktuellste Version ([Kubernetes 1.8](#cs_v18)) aktualisiert wurden.

[Überprüfen Sie mögliche Auswirkungen](cs_versions.html#cs_versions) jeder Aktualisierung einer Kubernetes-Version und [aktualisieren Sie Ihre Cluster](cs_cluster_update.html#update) dann sofort.

Sie arbeiten immer noch mit Kubernetes Version 1.5? Ziehen Sie die folgenden Informationen zurate, um die Auswirkungen der Aktualisierung von Version 1.5 auf Version 1.7 einzuschätzen. [Aktualisieren Sie die Cluster](cs_cluster_update.html#update) auf Version 1.7 und dann sofort auf Version 1.8.
{: tip}

<p><img src="images/certified_kubernetes_1x7.png" style="padding-right: 10px;" align="left" alt="Diese Markierung zeigt die Kubernetes Version 1.7-Zertifizierung für IBM Cloud Container Service an."/> {{site.data.keyword.containerlong_notm}} ist ein zertifiziertes Kubernetes-Produkt für Version 1.7 unter dem CNCF Kubernetes Software Conformance Certification Program.</p>

Überprüfen Sie Änderungen, die Sie möglicherweise bei einer Aktualisierung von der vorherigen Version auf die Kubernetes-Version 1.7 vornehmen müssen.

<br/>

#### Vor Master aktualisieren
{: #17_before}

<table summary="Kubernetes-Aktualisierungen für die Versionen 1.7 und 1.6">
<caption>Vorzunehmende Änderungen vor der Aktualisierung des Masters auf Kubernetes 1.7</caption>
<thead>
<tr>
<th>Typ</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Speicher</td>
<td>Konfigurationsscripts mit den Angaben `hostPath` und `mountPath` und Verweisen auf das übergeordnete Verzeichnis wie beispielsweise `../to/dir` sind nicht zulässig. Ändern Sie die Pfade in einfache absolute Pfade. Beispiel: `/path/to/dir`.
<ol>
  <li>Stellen Sie fest, ob Sie Speicherpfade ändern müssen:</br>
  ```
  kubectl get pods --all-namespaces -o yaml | grep "\.\." && echo "Action required"
  ```
  </br>

  <li>Wenn vom System die Meldung `Action required` (Aktion erforderlich) zurückgegeben wird, müssen Sie die Pods so ändern, dass auf den absoluten Pfad verwiesen wird, bevor Sie alle Workerknoten aktualisieren. Wenn eine andere Ressource (z. B. eine Bereitstellung) Eigner des Pods ist, müssen Sie die [_PodSpec_ ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/api-reference/v1.7/#podspec-v1-core) in dieser Ressource ändern.
</ol>
</td>
</tr>
</tbody>
</table>

#### Nach Master aktualisieren
{: #17_after}

<table summary="Kubernetes-Aktualisierungen für die Versionen 1.7 und 1.6">
<caption>Vorzunehmende Änderungen nach der Aktualisierung des Masters auf Kubernetes 1.7</caption>
<thead>
<tr>
<th>Typ</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Bereitstellung von `apiVersion`</td>
<td>Nachdem Sie die Cluster von Kubernetes 1.5 aktualisiert haben, verwenden Sie `apps/v1beta1` für das Feld `apiVersion` in neuen `Deployment`-YAML-Dateien. Verwenden Sie weiterhin `extensions/v1beta1` für andere Ressourcen, wie z. B. `Ingress`.</td>
</tr>
<tr>
<td>'kubectl'</td>
<td>Nach der Aktualisierung der `kubectl`-Benutzerschnittstelle müssen in den folgenden `kubectl create`-Befehlen anstelle von durch Kommas getrennten Argumenten mehrere Flags verwendet werden:<ul>
 <li>`role`
 <li>`clusterrole`
 <li>`rolebinding`
 <li>`clusterrolebinding`
 <li>`secret`
 </ul>
</br>  Führen Sie z. B. `kubectl create role --resource-name <x> --resource-name <y>` und nicht `kubectl create role --resource-name <x>,<y>` aus.</td>
</tr>
<tr>
<td>Netzrichtlinie</td>
<td>Die Annotation `net.beta.kubernetes.io/network-policy` ist nicht mehr verfügbar.
<ol>
  <li>Stellen Sie fest, ob Sie Richtlinien ändern müssen:</br>
  ```
  kubectl get ns -o yaml | grep "net.beta.kubernetes.io/network-policy" | grep "DefaultDeny" && echo "Action required"
  ```
  <li>Wenn das System die Meldung `"Action required"` (Aktion erforderlich) zurückgibt, müssen Sie die folgende Netzrichtlinie zu jedem aufgeführten Kubernetes-Namensbereich hinzufügen:</br>

  <pre class="codeblock">
  <code>
  kubectl create -n &lt;namensbereich&gt; -f - &lt;&lt;EOF
  kind: NetworkPolicy
  apiVersion: networking.k8s.io/v1
  metadata:
    name: default-deny
    namespace: &lt;namensbereich&gt;
  spec:
    podSelector: {}
  EOF
  </code>
  </pre>

  <li> Entfernen Sie nach dem Hinzufügen der Netzrichtlinie die Annotation `net.beta.kubernetes.io/network-policy`:
  ```
  kubectl annotate ns <namensbereich> --overwrite "net.beta.kubernetes.io/network-policy-"
  ```
  </li></ol>
</td></tr>
<tr>
<td>Affinitätsplanung für Pod</td>
<td> Die Annotation `scheduler.alpha.kubernetes.io/affinity` wird nicht mehr verwendet.
<ol>
  <li>Stellen Sie für jeden Namensbereich mit Ausnahme von `ibm-system` und `kube-system` fest, ob die Affinitätsplanung für den Pod aktualisiert werden muss:</br>
  ```
  kubectl get pods -n <namensbereich> -o yaml | grep "scheduler.alpha.kubernetes.io/affinity" && echo "Action required"
  ```
  </br></li>
  <li>Wenn das System die Meldung `"Action required"` (Aktion erforderlich) zurückgibt, verwenden Sie anstelle der Annotation `scheduler.alpha.kubernetes.io/affinity` das Feld _affinity_ der [_PodSpec_ ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/api-reference/v1.7/#podspec-v1-core).</li>
</ol>
</td></tr>
<tr>
<td>RBAC für `default` `ServiceAccount`</td>
<td><p>Die Administratorberechtigung `ClusterRoleBinding` für den `default` `ServiceAccount` im Standardnamensbereich `default` wurde entfernt, um die Clustersicherheit zu verbessern. Anwendungen, die im Standardnamensbereich `default` ausgeführt werden, verfügen nicht mehr über Clusteradministratorberechtigungen für die Kubernetes-API und es kann bei Ihnen zu `RBAC DENY`-Berechtigungsfehlern kommen. Prüfen Sie Ihre App und die zugehörige `.yaml`-Datei, um zu sehen, ob sie im Namensbereich `default` ausgeführt wird, das Standardservicekonto (`default ServiceAccount`) verwendet und auf die Kubernetes-API zugreift.</p>
<p>Wenn Ihre Anwendungen auf diese Berechtigungen angewiesen sind, [erstellen Sie RBAC-Berechtigungsressourcen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview) für Ihre Apps.</p>
  <p>Wenn Sie die RBAC-Richtlinien für Ihre App aktualisieren, möchten Sie möglicherweise temporär zum vorherigen Standard `default` zurückkehren. Mit dem Befehl `kubectl apply -f DATEINAME` können Sie die folgenden Dateien kopieren, speichern und anwenden. <strong>Hinweis</strong>: Nutzen Sie das Zurücksetzen, damit Sie Zeit zum Aktualisieren aller Anwendungsressourcen haben. Dies ist jedoch keine langfristige Lösung.</p>

<p><pre class="codeblock">
<code>
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
</code>
</pre></p>
</td>
</tr>
<tr>
<td>Schreibgeschützte API-Datenträger</td>
<td>`secret`, `configMap`, `downwardAPI` und projizierte Datenträger werden jetzt nur schreibgeschützt angehängt.
Früher durften Apps Daten auf diese Datenträger schreiben, die vom System möglicherweise nicht automatisch zurückgesetzt werden. Diese Migrationsaktion ist erforderlich,
um die Sicherheitslücke [CVE-2017-1002102](https://cve.mitre.org/cgi-bin/cvename.cgi?name=2017-1002102) zu schließen.
Wenn Ihre Apps das bisherige unsichere Verhalten erwarten, ändern Sie sie entsprechend.</td>
</tr>
<tr>
<td>StatefulSet pod DNS</td>
<td>Pods des Typs 'StatefulSet' verlieren nach der Aktualisierung des Masters ihre Kubernetes-DNS-Einträge. Um die DNS-Einträge wiederherzustellen, löschen Sie diese Pods. Kubernetes erstellt die Pods erneut und stellt die DNS-Einträge automatisch wieder her. Weitere Informationen finden Sie im Abschnitt zu dem entsprechenden [Problem in Kubernetes ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/issues/48327).</td>
</tr>
<tr>
<td>Tolerierungen</td>
<td>Die Annotation `scheduler.alpha.kubernetes.io/tolerations` ist nicht mehr verfügbar.
<ol>
  <li>Stellen Sie für jeden Namensbereich mit Ausnahme von `ibm-system` und `kube-system` fest, ob die Tolerierungen geändert werden müssen:</br>
  ```
  kubectl get pods -n <namensbereich> -o yaml | grep "scheduler.alpha.kubernetes.io/tolerations" && echo "Action required"
  ```
  </br>

  <li>Wenn das System die Meldung `"Action required"` (Aktion erforderlich) zurückgibt, verwenden Sie anstelle der Annotation `scheduler.alpha.kubernetes.io/tolerations` das Feld _tolerations_ der [_PodSpec_ ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/api-reference/v1.7/#podspec-v1-core).
</ol>
</td></tr>
<tr>
<td>Taints</td>
<td>Die Annotation `scheduler.alpha.kubernetes.io/taints` ist nicht mehr verfügbar.
<ol>
  <li>Stellen Sie fest, ob Sie Taints ändern müssen:</br>
  ```
  kubectl get nodes -o yaml | grep "scheduler.alpha.kubernetes.io/taints" && echo "Action required"
  ```
  <li>Wenn das System die Meldung `"Action required"` (Aktion erforderlich) zurückgibt, entfernen Sie für jeden Knoten die Annotation `scheduler.alpha.kubernetes.io/taints`:</br>
  `kubectl annotate nodes <node> scheduler.alpha.kubernetes.io/taints-`
  <li>Fügen Sie zu jedem Knoten einen Taint hinzu:</br>
  `kubectl taint node <node> <taint>`
  </li></ol>
</td></tr>
</tbody>
</table>

<br />


### Version 1.5 (nicht unterstützt)
{: #cs_v1-5}

Seit dem 4. April 2018 werden {{site.data.keyword.containershort_notm}}-Cluster, die auf [Kubernetes Version 1.5](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.5.md) ausgeführt werden, nicht mehr unterstützt. Cluster der Version 1.5 können keine Sicherheitsupdates oder Unterstützung mehr erhalten, bis sie auf die nächste, aktuellste Version ([Kubernetes 1.7](#cs_v17)) aktualisiert wurden.

[Überprüfen Sie mögliche Auswirkungen](cs_versions.html#cs_versions) jeder Aktualisierung einer Kubernetes-Version und [aktualisieren Sie Ihre Cluster](cs_cluster_update.html#update) dann sofort. Ihre Aktualisierung wird von einer Version auf die nächste aktuellste Version ausgeführt, beispielsweise von Version 1.5 auf 1.7 oder von Version 1.8 auf 1.9.
