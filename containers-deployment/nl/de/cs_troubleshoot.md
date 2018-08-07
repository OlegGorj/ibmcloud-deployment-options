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



# Cluster debuggen
{: #cs_troubleshoot}

Ziehen Sie bei der Verwendung von {{site.data.keyword.containerlong}} die folgenden Verfahren für die allgemeine Fehlerbehebung und das Debugging Ihrer Cluster in Betracht. Sie können außerdem den [Status des {{site.data.keyword.Bluemix_notm}}-Systems ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/bluemix/support/#status) überprüfen.
{: shortdesc}

Sie können die folgenden allgemeinen Schritte ausführen, um sicherzustellen, dass Ihre Cluster auf dem neuesten Stand sind:
- Prüfen Sie einmal im Monat auf verfügbare Sicherheits- und Betriebssystempatches, um [Ihre Workerknoten zu aktualisieren](cs_cli_reference.html#cs_worker_update).
- [Aktualisieren Sie Ihren Cluster](cs_cli_reference.html#cs_cluster_update) auf die neueste [Standardversion von Kubernetes](cs_versions.html) für {{site.data.keyword.containershort_notm}}.

## Cluster debuggen
{: #debug_clusters}

Informieren Sie sich über die Optionen, die Ihnen für die Fehlerbehebung bei Ihren Clustern und zum Eingrenzen der jeweiligen Fehlerquelle zur Verfügung stehen.

1.  Listen Sie Ihren Cluster auf und suchen Sie nach `Status` des Clusters.

  ```
  bx cs clusters
  ```
  {: pre}

2.  Überprüfen Sie den `Status` Ihres Clusters. Wenn Ihr Cluster den Status **Critical**, **Delete failed** oder **Warning** aufweist oder sich seit längerer Zeit im Status **Pending** befindet, beginnen Sie mit dem [Debugging der Workerknoten](#debug_worker_nodes).

    <table summary="Jede Tabellenzeile sollte von links nach rechts gelesen werden, wobei der Clusterstatus in der ersten Spalte und eine Beschreibung in der zweiten Spalte angegeben ist.">
<caption>Clusterstatus</caption>
   <thead>
   <th>Clusterstatus</th>
   <th>Beschreibung</th>
   </thead>
   <tbody>
<tr>
   <td>Aborted (Abgebrochen)</td>
   <td>Das Löschen des Clusters wird vom Benutzer angefordert, bevor der Kubernetes Master bereitgestellt ist. Nachdem das Löschen des Clusters abgeschlossen ist, wird der Cluster aus dem Dashboard entfernt. Wenn Ihr Cluster in diesem Status lange Zeit blockiert ist, öffnen Sie ein [{{site.data.keyword.Bluemix_notm}}-Support-Ticket](cs_troubleshoot.html#ts_getting_help).</td>
   </tr>
 <tr>
     <td>Critical (Kritisch)</td>
     <td>Der Kubernetes-Master kann nicht erreicht werden oder alle Workerknoten in dem Cluster sind inaktiv. </td>
    </tr>
   <tr>
     <td>Delete failed (Löschen fehlgeschlagen)</td>
     <td>Der Kubernetes Masterknoten oder mindestens ein Workerknoten kann nicht gelöscht werden.  </td>
   </tr>
   <tr>
     <td>Deleted (Gelöscht)</td>
     <td>Der Cluster wird gelöscht, aber noch nicht aus dem Dashboard entfernt. Wenn Ihr Cluster in diesem Status lange Zeit blockiert ist, öffnen Sie ein [{{site.data.keyword.Bluemix_notm}}-Support-Ticket](cs_troubleshoot.html#ts_getting_help). </td>
   </tr>
   <tr>
   <td>Deleting (Löschen)</td>
   <td>Der Cluster wird gelöscht, und die Clusterinfrastruktur wird abgebaut. Der Zugriff auf den Cluster ist nicht möglich.  </td>
   </tr>
   <tr>
     <td>Deploy failed (Bereitstellung fehlgeschlagen)</td>
     <td>Die Bereitstellung des Kubernetes-Masters konnte nicht abgeschlossen werden. Sie können diesen Status nicht auflösen. Wenden Sie sich an den Support für IBM Cloud, indem Sie ein [{{site.data.keyword.Bluemix_notm}}-Support-Ticket](cs_troubleshoot.html#ts_getting_help) öffnen.</td>
   </tr>
     <tr>
       <td>Deploying (Wird bereitgestellt)</td>
       <td>Der Kubernetes-Master ist noch nicht vollständig implementiert. Sie können auf Ihren Cluster nicht zugreifen. Warten Sie, bis Ihr Cluster vollständig bereitgestellt wurde, und überprüfen Sie dann den Status Ihres Clusters.</td>
      </tr>
      <tr>
       <td>Normal</td>
       <td>Alle Workerknoten in einem Cluster sind betriebsbereit. Sie können auf den Cluster zugreifen und Apps auf dem Cluster bereitstellen. Dieser Status wird als einwandfreier Zustand betrachtet und erfordert keine Aktion von Ihnen. **Hinweis**: Auch wenn die Workerknoten ordnungsgemäß funktionieren, bedürfen andere Infrastrukturressourcen wie [Netz](cs_troubleshoot_network.html) und [Speicher](cs_troubleshoot_storage.html) möglicherweise Ihrer Aufmerksamkeit.</td>
    </tr>
      <tr>
       <td>Pending (Anstehend)</td>
       <td>Der Kubernetes-Master ist bereitgestellt. Die Workerknoten werden gerade eingerichtet und sind noch nicht im Cluster verfügbar. Sie können auf den Cluster zugreifen, aber Sie können keine Apps auf dem Cluster bereitstellen.  </td>
     </tr>
   <tr>
     <td>Requested (Angefordert)</td>
     <td>Eine Anforderung zum Erstellen des Clusters und zum Bestellen der Infrastruktur für die Kubernetes Master- und Worker-Knoten wird gesendet. Wenn die Bereitstellung des Clusters gestartet wird, ändert sich der Clusterstatus in <code>Deploying</code> (Wird bereitgestellt). Wenn Ihr Cluster lange Zeit im Status <code>Requested</code> (Angefordert) blockiert ist, öffnen Sie ein [{{site.data.keyword.Bluemix_notm}}-Support-Ticket](cs_troubleshoot.html#ts_getting_help). </td>
   </tr>
   <tr>
     <td>Updating (Aktualisierung)</td>
     <td>Der Kubernetes API-Server, der in Ihrem Kubernetes-Master ausgeführt wird, wird auf eine neue Version der Kubernetes-API aktualisiert. Während der Aktualisierung können Sie weder auf den Cluster zugreifen noch Änderungen am Cluster vornehmen. Workerknoten, Apps und Ressourcen, die vom Benutzer bereitgestellt wurden, werden nicht geändert und weiterhin ausgeführt. Warten Sie, bis die Aktualisierung abgeschlossen ist, und überprüfen Sie dann den Status Ihres Clusters. </td>
   </tr>
    <tr>
       <td>Warning (Warnung)</td>
       <td>Mindestens ein Workerknoten in dem Cluster ist nicht verfügbar, aber andere Workerknoten sind verfügbar und können die Workload übernehmen. </td>
    </tr>
   </tbody>
 </table>


<br />


## Debugging der Workerknoten
{: #debug_worker_nodes}

Informieren Sie sich über die Optionen, die Ihnen für die Fehlerbehebung bei Ihren Workerknoten und zum Eingrenzen der jeweiligen Fehlerquelle zur Verfügung stehen.


1.  Wenn Ihr Cluster den Status **Critical**, **Delete failed** oder **Warning** aufweist oder sich seit längerer Zeit im Status **Pending** befindet, überprüfen Sie den Status Ihrer Workerknoten.

  ```
  bx cs workers <clustername_oder_-id>
  ```
  {: pre}

2.  Überprüfen Sie die Felder für `Zustand` und `Status` für jeden Workerknoten in Ihrer CLI-Ausgabe.

  <table summary="Jede Tabellenzeile sollte von links nach rechts gelesen werden, wobei der Clusterstatus in der ersten Spalte und eine Beschreibung in der zweiten Spalte angegeben ist.">
  <caption>Status des Workerknotens</caption>
    <thead>
    <th>Status des Workerknotens</th>
    <th>Beschreibung</th>
    </thead>
    <tbody>
  <tr>
      <td>Critical (Kritisch)</td>
      <td>Ein Workerknoten kann aus vielen Gründen in einen kritischen Status wechseln: <ul><li>Sie haben einen Warmstart für Ihren Workerknoten eingeleitet, ohne den Workerknoten zu abzuriegeln und zu entleeren. Der Warmstart eines Workerknotens kann zu Datenverlust in <code>docker</code>, <code>kubelet</code>, <code>kube-proxy</code> und <code>calico</code> führen. </li><li>Die Pods, die auf Ihrem Workerknoten bereitgestellt wurden, verwenden keine Ressourcengrenzen für [Speicher ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/tasks/configure-pod-container/assign-memory-resource/) und [CPU ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/tasks/configure-pod-container/assign-cpu-resource/). Ohne Ressourcengrenzen können die Pods alle verfügbaren Ressourcen in Anspruch nehmen und keine Ressourcen mehr für andere Pods übrig lassen, die auch auf diesem Workerknoten ausgeführt werden. Diese Überbelegung durch Workload führt dazu, dass der Workerknoten fehlschlägt. </li><li><code>Docker</code>, <code>kubelet</code> oder <code>calico</code> geraten in einen nicht behebbaren kritischen Zustand, nachdem Hunderte oder Tausende von Containern ausgeführt wurden. </li><li>Sie haben eine Virtual Router Appliance für Ihren Workerknoten eingerichtet, die fehlschlug und die Kommunikation zwischen Ihrem Workerknoten und dem Kubernetes-Master unterbrochen hat. </li><li> Aktuelle Netzprobleme in {{site.data.keyword.containershort_notm}} oder IBM Cloud Infrastructure (SoftLayer), die ein Fehlschlagen der Kommunikation zwischen Ihrem Workerknoten und dem Kubernetes-Master verursachen.</li><li>Ihr Workerknoten hat keine Kapazität mehr. Überprüfen Sie den <strong>Status</strong> des Workerknotens, um zu sehen, ob <strong>Out of disk</strong> (zu wenig Plattenspeicher) oder <strong>Out of memory</strong> (zu wenig Hauptspeicher) angezeigt wird. Wenn Ihr Workerknoten die Kapazitätsgrenze erreicht hat, können Sie entweder die Arbeitslast auf Ihrem Workerknoten reduzieren oder einen Workerknoten zu Ihrem Cluster hinzufügen und so den Lastausgleich verbessern.</li></ul> In vielen Fällen kann ein [Neuladen](cs_cli_reference.html#cs_worker_reload) Ihres Workerknotens das Problem lösen. Wenn Sie Ihren Workerknoten neu laden, wird die aktuellste [Patchversion](cs_versions.html#version_types) auf Ihren Workerknoten angewendet. Die Haupt- und Nebenversion werden nicht geändert. Stellen Sie, bevor Sie Ihren Workerknoten erneut laden, sicher, dass Ihr Workerknoten abgeriegelt und entleert wurde, damit die vorhandenen Pods ordnungsgemäß beendet und auf den verbleibenden Workerknoten neu geplant werden können. </br></br> Wenn das Neuladen des Workerknotens das Problem nicht löst, wechseln Sie zum nächsten Schritt, um mit der Fehlerbehebung Ihrer Workerknoten fortzufahren. </br></br><strong>Tipp:</strong> Sie können [Statusprüfungen für Ihre Workerknoten konfigurieren und die automatische Wiederherstellung aktiveren](cs_health.html#autorecovery). Wenn die automatische Wiederherstellung basierend auf den konfigurierten Prüfungen einen nicht ordnungsgemäß funktionierenden Workerknoten erkennt, löst das System eine Korrekturmaßnahme, wie das erneute Laden des Betriebssystems, auf dem Workerknoten aus. Weitere Informationen zur Funktionsweise der automatischen Wiederherstellung finden Sie im [Blogbeitrag zur automatischen Wiederherstellung![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/blogs/bluemix/2017/12/autorecovery-utilizes-consistent-hashing-high-availability/).
      </td>
     </tr>
      <tr>
        <td>Deploying (Wird bereitgestellt)</td>
        <td>Wenn Sie die Kubernetes-Version Ihres Workerknotens aktualisieren, wird der Workerknoten erneut bereitgestellt, um die Aktualisierungen zu installieren. Falls sich Ihr Workerknoten eine längere Zeit in diesem Status befindet, fahren Sie mit dem nächsten Schritt fort, um zu sehen, ob während der Bereitstellung ein Problem aufgetreten ist. </td>
     </tr>
        <tr>
        <td>Normal</td>
        <td>Ihr Workerknoten wurde vollständig bereitgestellt und kann im Cluster verwendet werden. Dieser Status wird als einwandfreier Zustand betrachtet und erfordert keine Aktion vom Benutzer. **Hinweis**: Auch wenn die Workerknoten ordnungsgemäß funktionieren, bedürfen andere Infrastrukturressourcen wie [Netz](cs_troubleshoot_network.html) und [Speicher](cs_troubleshoot_storage.html) möglicherweise Ihrer Aufmerksamkeit.</td>
     </tr>
   <tr>
        <td>Provisioning (Wird bereitgestellt)</td>
        <td>Ihr Workerknoten wird gerade eingerichtet und ist noch nicht im Cluster verfügbar. Sie können den Bereitstellungsprozess in der Spalte <strong>Status</strong> Ihrer CLI-Ausgabe überwachen. Falls sich Ihr Workerknoten eine längere Zeit in diesem Status befindet, fahren Sie mit dem nächsten Schritt fort, um zu sehen, ob während der Bereitstellung ein Problem aufgetreten ist.</td>
      </tr>
      <tr>
        <td>Provision_failed (Bereitstellung fehlgeschlagen)</td>
        <td>Ihr Workerknoten konnte nicht bereitgestellt werden. Fahren Sie mit dem nächsten Schritt fort, um die Details des Fehlers abzurufen.</td>
      </tr>
   <tr>
        <td>Reloading (Wird neu geladen)</td>
        <td>Ihr Workerknoten wird neu geladen und ist nicht im Cluster verfügbar. Sie können den Neuladeprozess in der Spalte <strong>Status</strong> Ihrer CLI-Ausgabe überwachen. Falls sich Ihr Workerknoten eine längere Zeit in diesem Status befindet, fahren Sie mit dem nächsten Schritt fort, um zu sehen, ob während des Neuladens ein Problem aufgetreten ist.</td>
       </tr>
       <tr>
        <td>Reloading_failed (Neuladen fehlgeschlagen) </td>
        <td>Ihr Workerknoten konnte nicht neu geladen werden. Fahren Sie mit dem nächsten Schritt fort, um die Details des Fehlers abzurufen.</td>
      </tr>
      <tr>
        <td>Reload_pending (Neuladen anstehend) </td>
        <td>Eine Anforderung zum Neuladen oder Aktualisieren der Kubernetes-Version Ihrer Workerknoten wurde gesendet. Wenn der Workerknoten erneut geladen wird, ändert sich der Status in <code>Reloading</code>. </td>
      </tr>
      <tr>
       <td>Unknown (Unbekannt)</td>
       <td>Der Kubernetes-Master ist aus einem der folgenden Gründe nicht erreichbar:<ul><li>Sie haben ein Update Ihres Kubernetes-Masters angefordert. Der Status des Workerknotens kann während des Updates nicht abgerufen werden.</li><li>Sie haben möglicherweise eine weitere Firewall, die Ihre Workerknoten schützt, oder Sie haben die Firewalleinstellungen kürzlich geändert. {{site.data.keyword.containershort_notm}} erfordert, dass bestimmte IP-Adressen und Ports geöffnet sind, damit die Kommunikation vom Workerknoten zum Kubernetes-Master und umgekehrt möglich ist. Weitere Informationen finden Sie in [Firewall verhindert Verbindung für Workerknoten](cs_troubleshoot_clusters.html#cs_firewall).</li><li>Der Kubernetes-Master ist inaktiv. Wenden Sie sich an den {{site.data.keyword.Bluemix_notm}}-Support, indem Sie ein [{{site.data.keyword.Bluemix_notm}}-Support-Ticket](#ts_getting_help) öffnen.</li></ul></td>
  </tr>
     <tr>
        <td>Warning (Warnung)</td>
        <td>Ihr Workerknoten nähert sich dem Grenzwert für die Speicher- oder Plattenkapazität. Sie können entweder die Arbeitslast auf Ihrem Workerknoten reduzieren oder einen Workerknoten zu Ihrem Cluster hinzufügen und so den Lastausgleich verbessern.</td>
  </tr>
    </tbody>
  </table>

5.  Listen Sie die Details für den Workerknoten auf. Wenn die Details eine Fehlernachricht enthalten, überprüfen Sie die Liste der [gängigen Fehlernachrichten für Workerknoten](#common_worker_nodes_issues) und lernen Sie, diese Probleme zu beheben.

   ```
   bx cs worker-get <worker-id>
   ```
   {: pre}

  ```
  bx cs worker-get [<clustername_oder_-id>] <workerknoten-id>
  ```
  {: pre}

<br />


## Gängige Fehlernachrichten bei Workerknoten
{: #common_worker_nodes_issues}

Überprüfen Sie gängige Fehlernachrichten und lernen Sie, diese zu beheben.

  <table>
  <caption>Allgemeine Fehlernachrichten</caption>
    <thead>
    <th>Fehlernachricht</th>
    <th>Beschreibung und Problemlösung
    </thead>
    <tbody>
      <tr>
        <td>{{site.data.keyword.Bluemix_notm}} Infrastructure Exception: Your account is currently prohibited from ordering 'Computing Instances'.</td>
        <td>Mit Ihrem Konto von IBM Cloud Infrastructure (SoftLayer) können Sie möglicherweise keine Rechenressourcen bestellen. Wenden Sie sich an den {{site.data.keyword.Bluemix_notm}}-Support, indem Sie ein [{{site.data.keyword.Bluemix_notm}}-Support-Ticket](#ts_getting_help) öffnen.</td>
      </tr>
      <tr>
        <td>{{site.data.keyword.Bluemix_notm}} Infrastructure Exception: Could not place order. There are insufficient resources behind router 'routername' to fulfill the request for the following guests: 'worker-id'.</td>
        <td>Das ausgewählte VLAN ist einem Pod im Rechenzentrum zugeordnet, der nicht über ausreichend Speicherplatz zum Bereitstellen Ihres Workerknotens verfügt. Sie können zwischen den folgenden Optionen wählen:<ul><li>Stellen Sie Ihren Workerknoten in einem anderen Rechenzentrum bereit. Führen Sie <code>bx cs locations</code> aus, um verfügbare Rechenzentren aufzulisten.<li>Wenn Sie ein vorhandenes Paar aus öffentlichem und privatem VLAN haben, das einem anderen Pod in dem Rechenzentrum zugeordnet ist, verwenden Sie dieses VLAN stattdessen.<li>Wenden Sie sich an den {{site.data.keyword.Bluemix_notm}}-Support, indem Sie ein [{{site.data.keyword.Bluemix_notm}}-Support-Ticket](#ts_getting_help) öffnen.</ul></td>
      </tr>
      <tr>
        <td>{{site.data.keyword.Bluemix_notm}} Infrastructure Exception: Could not obtain network VLAN with ID: &lt;vlan-id&gt;.</td>
        <td>Ihr Workerknoten konnte nicht bereitgestellt werden, weil die ausgewählte VLAN-ID aus einem der folgenden Gründe nicht gefunden werden konnte:<ul><li>Möglicherweise haben Sie statt der VLAN-ID die VLAN-Nummer angegeben. Die VLAN-Nummer umfasst 3 oder 4 Ziffern, während die VLAN-ID 7 Stellen hat. Führen Sie <code>bx cs vlans &lt;standort&gt;</code> aus, um die VLAN-ID abzurufen.<li>Möglicherweise ist die VLAN-ID nicht dem von Ihnen verwendeten Konto von IBM Cloud Infrastructure (SoftLayer) zugeordnet. Führen Sie <code>bx cs vlans &lt;standort&gt;</code> aus, um verfügbare VLAN-IDs für Ihr Konto aufzulisten. Um das Konto von IBM Cloud Infrastructure (SoftLayer) zu ändern, sollten Sie die Informationen unter [`bx cs credentials-set`](cs_cli_reference.html#cs_credentials_set) nachlesen. </ul></td>
      </tr>
      <tr>
        <td>SoftLayer_Exception_Order_InvalidLocation: The location provided for this order is invalid. (HTTP 500)</td>
        <td>Ihr System von IBM Cloud Infrastructure (SoftLayer) ist nicht für das Bestellen von Rechenressourcen im ausgewählten Rechenzentrum eingerichtet. Wenden Sie sich an den [{{site.data.keyword.Bluemix_notm}}-Support](#ts_getting_help), um sicherzustellen, dass Ihr Konto korrekt eingerichtet ist.</td>
       </tr>
       <tr>
        <td>{{site.data.keyword.Bluemix_notm}} Infrastructure Exception: The user does not have the necessary {{site.data.keyword.Bluemix_notm}} Infrastructure permissions to add servers
        </br></br>
        {{site.data.keyword.Bluemix_notm}} Infrastructure Exception: 'Item' must be ordered with permission.</td>
        <td>Möglicherweise verfügen Sie nicht über die erforderlichen Berechtigungen, um einen Workerknoten aus dem Portfolio von IBM Cloud Infrastructure (SoftLayer) bereitzustellen. Weitere Informationen finden Sie in [Zugriff auf das Portfolio von IBM Cloud Infrastructure (SoftLayer) konfigurieren, um Kubernetes-Standardcluster zu erstellen](cs_troubleshoot_clusters.html#cs_credentials).</td>
      </tr>
      <tr>
       <td>Worker unable to talk to {{site.data.keyword.containershort_notm}} servers. Please verify your firewall setup is allowing traffic from this worker.
       <td><ul><li>Wenn Sie über eine Firewall verfügen, [konfigurieren Sie Ihre Firewalleinstellungen so, dass sie ausgehenden Datenverkehr zu den entsprechenden Ports und IP-Adressen zulassen](cs_firewall.html#firewall_outbound).</li><li>Überprüfen Sie, dass Ihr Cluster über keine öffentliche IP verfügt, indem Sie den folgenden Befehl ausführen: `bx cs workers <mycluster>`. Wenn keine öffentliche IP aufgelistet wird, verfügt Ihr Cluster nur über private VLANs.<ul><li>Wenn Sie möchten, dass der Cluster nur über private VLANs verfügt, konfigurieren Sie die [VLAN-Verbindung](cs_clusters.html#worker_vlan_connection) und die [Firewall](cs_firewall.html#firewall_outbound).</li><li>Wenn Sie möchten, dass der Cluster über eine öffentliche IP verfügt, [fügen Sie neue Workerknoten](cs_cli_reference.html#cs_worker_add) mit öffentlichen und privaten VLANs hinzu.</li></ul></li></ul></td>
     </tr>
      <tr>
  <td>Cannot create IMS portal token, as no IMS account is linked to the selected BSS account</br></br>Provided user not found or active</br></br>SoftLayer_Exception_User_Customer_InvalidUserStatus: User account is currently cancel_pending.</br></br>Waiting for machine to be visible to the user</td>
  <td>Der Eigner des API-Schlüssels, der für den Zugriff auf das Portfolio von IBM Cloud Infrastructure (SoftLayer) verwendet wird, verfügt nicht über die erforderlichen Berechtigungen zum Ausführen der Aktion oder es steht die Löschung der Berechtigungen an.</br></br><strong>Als Benutzer</strong> führen Sie diese Schritte aus: <ol><li>Wenn Sie Zugriff auf mehrere Konten haben, stellen Sie sicher, dass Sie bei dem Konto angemeldet sind, in dem Sie mit {{site.data.keyword.containerlong_notm}} arbeiten möchten. </li><li>Führen Sie den Befehl <code>bx cs api-key-info</code> aus, um den aktuellen API-Schlüsseleigner anzuzeigen, der für den Zugriff auf das Portfolio von IBM Cloud Infrastructure (SoftLayer) verwendet wird. </li><li>Führen Sie den Befehl <code>bx account list</code> aus, um den Eigner des {{site.data.keyword.Bluemix_notm}}-Kontos anzuzeigen, das Sie gerade verwenden. </li><li>Kontaktieren Sie den Eigner des {{site.data.keyword.Bluemix_notm}}-Kontos und melden Sie, dass der Eigner des API-Schlüssels nicht über ausreichende Berechtigungen in IBM Cloud Infrastructure (SoftLayer) verfügt oder dass die Löschung der Berechtigungen ansteht. </li></ol></br><strong>Als Kontoeigner</strong>, führen Sie diese Schritte aus: <ol><li>Überprüfen Sie die [erforderlichen Berechtigungen in IBM Cloud Infrastructure (SoftLayer)](cs_users.html#infra_access), um die Aktion auszuführen, die zuvor fehlgeschlagen ist. </li><li>Korrigieren Sie die Berechtigungen für den API-Schlüsseleigner oder erstellen Sie einen neuen API-Schlüssel mithilfe des Befehls [<code>bx cs api-key-reset</code>](cs_cli_reference.html#cs_api_key_reset). </li><li>Wenn durch Sie oder einen anderen Kontoadministrator manuell Berechtigungsnachweise für IBM Cloud Infrastructure (SoftLayer) in Ihrem Konto definiert wurden, führen Sie den Befehl [<code>bx cs credentials-unset</code>](cs_cli_reference.html#cs_credentials_unset) aus, um die Berechtigungsnachweise aus Ihrem Konto zu entfernen.</li></ol></td>
  </tr>
    </tbody>
  </table>



<br />




## App-Bereitstellungen debuggen
{: #debug_apps}

Informieren Sie sich über die Optionen, die Ihnen für das Debuggen Ihrer App-Bereitstellungen und zum Eingrenzen der jeweiligen Fehlerquelle zur Verfügung stehen.

1. Suchen Sie nach Unregelmäßigkeiten in den Service- oder Bereitstellungsressourcen, indem Sie den Befehl `describe` ausführen.

 Beispiel:
 <pre class="pre"><code>kubectl describe service &lt;servicename&gt; </code></pre>

2. [Überprüfen Sie, ob die Container im Status 'ContainerCreating' blockiert sind](cs_troubleshoot_storage.html#stuck_creating_state).

3. Überprüfen Sie, ob der Cluster sich im Status `Critical` (Kritisch) befindet. Befindet sich der Cluster im Status `Critical`, dann überprüfen Sie die Firewallregeln und verifizieren Sie, ob der Master mit den Workerknoten kommunizieren kann.

4. Vergewissern Sie sich, dass der Service am korrekten Port empfangsbereit ist.
   1. Rufen Sie den Namen eines Pods ab.
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. Melden Sie sich am Container an.
     <pre class="pre"><code>kubectl exec -it &lt;podname&gt; -- /bin/bash</code></pre>
   3. Führen Sie den Curl-Befehl für die App innerhalb des Containers aus. Wenn auf den Port nicht zugegriffen werden kann, dann ist der Service möglicherweise nicht am richtigen Port empfangsbereit oder in der App sind Probleme aufgetreten. Aktualisieren Sie die Konfigurationsdatei für den Service und geben Sie dabei den korrekten Port an und wiederholen Sie die Bereitstellung oder untersuchen Sie mögliche Probleme mit der App.
     <pre class="pre"><code>curl localhost: &lt;port&gt;</code></pre>

5. Stellen Sie sicher, dass der Service korrekt mit den Pods verbunden ist.
   1. Rufen Sie den Namen eines Pods ab.
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. Melden Sie sich am Container an.
     <pre class="pre"><code>kubectl exec -it &lt;podname&gt; -- /bin/bash</code></pre>
   3. Führen Sie den Curl-Befehl für die Cluster-IP-Adresse und den Port des Service aus. Wenn nicht auf die IP-Adresse und den Port zugegriffen werden kann, dann untersuchen Sie die Endpunkte des Service. Wenn keine Endpunkte vorhanden sind, dann stimmt der Selektor für den Service nicht mit den Pods überein. Wenn Endpunkte aufgeführt sind, dann überprüfen Sie das Feld des Zielports für den Service und vergewissern Sie sich, dass der Zielport identisch mit dem Zielport für die Pods ist.
     <pre class="pre"><code>curl &lt;cluster_IP&gt;:&lt;port&gt;</code></pre>

6. Überprüfen Sie für die Ingress-Services, ob auf den Service von innerhalb des Clusters zugegriffen werden kann.
   1. Rufen Sie den Namen eines Pods ab.
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. Melden Sie sich am Container an.
     <pre class="pre"><code>kubectl exec -it &lt;podname&gt; -- /bin/bash</code></pre>
   2. Führen Sie den Curl-Befehl für die URL aus, die für den Ingress-Service angegeben wurde. Wenn nicht auf die URL zugegriffen werden kann, dann überprüfen Sie das System auf ein Firewallproblem zwischen dem Cluster und dem externen Endpunkt. 
     <pre class="pre"><code>curl &lt;hostname&gt;.&lt;domäne&gt;</code></pre>

<br />



## Hilfe und Unterstützung anfordern
{: #ts_getting_help}

Haben Sie noch immer Probleme mit Ihrem Cluster?
{: shortdesc}

-   [Überprüfen Sie auf der {{site.data.keyword.Bluemix_notm}}-Statusseite ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/bluemix/support/#status), ob {{site.data.keyword.Bluemix_notm}} verfügbar ist.
-   Veröffentlichen Sie eine Frage im [{{site.data.keyword.containershort_notm}}-Slack ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://ibm-container-service.slack.com).

    Wenn Sie keine IBM ID für Ihr {{site.data.keyword.Bluemix_notm}}-Konto verwenden, [fordern Sie eine Einladung](https://bxcs-slack-invite.mybluemix.net/) zu diesem Slack an.
    {: tip}
-   Suchen Sie in entsprechenden Foren, ob andere Benutzer auf das gleiche Problem
gestoßen sind. Versehen Sie Ihre Fragen in den Foren mit Tags, um sie für das Entwicklungsteam
von {{site.data.keyword.Bluemix_notm}} erkennbar zu machen.

    -   Wenn Sie technische Fragen zur Entwicklung oder Bereitstellung von Clustern oder Apps mit {{site.data.keyword.containershort_notm}} haben, veröffentlichen Sie Ihre Frage auf [Stack Overflow ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) und versehen Sie sie mit den Tags `ibm-cloud`, `kubernetes` und `containers`.
    -   Verwenden Sie für Fragen zum Service und zu ersten Schritten das Forum [IBM developerWorks dW Answers ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix). Geben Sie die Tags `ibm-cloud` und `containers` an.
    Weitere Details zur Verwendung der Foren
finden Sie unter [Hilfe anfordern](/docs/get-support/howtogetsupport.html#using-avatar).

-   Wenden Sie sich an den IBM Support, indem Sie ein Ticket öffnen. Informationen zum Öffnen eines IBM Support-Tickets oder zu Supportstufen und zu Prioritätsstufen von Tickets finden Sie unter [Support kontaktieren](/docs/get-support/howtogetsupport.html#getting-customer-support).

{: tip}
Geben Sie beim Melden eines Problems Ihre Cluster-ID an. Führen Sie den Befehl `bx cs clusters` aus, um Ihre Cluster-ID abzurufen.

