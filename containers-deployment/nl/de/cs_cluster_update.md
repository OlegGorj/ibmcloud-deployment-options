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





# Cluster und Workerknoten aktualisieren
{: #update}

Sie können Aktualisierungen installieren, um Ihre Kubernetes-Cluster in {{site.data.keyword.containerlong}} aktuell zu halten.
{:shortdesc}

## Kubernetes-Master aktualisieren
{: #master}

Kubernetes gibt regelmäßig [Hauptversionen, Nebenversionen oder Patches als Aktualisierungen heraus.](cs_versions.html#version_types). Abhängig vom Aktualisierungstyp können Sie für die Aktualisierung der Kubernetes-Masterkomponente verantwortlich sein.
{:shortdesc}

Aktualisierungen können die API-Serverversion von Kubernetes oder andere Komponenten in Ihrem Kubernetes-Master betreffen.  Auf jeden Fall sind Sie immer dafür verantwortlich, Ihre Workerknoten auf dem neuesten Stand zu halten. Bei Aktualisierungen  wird der Kubernetes-Master vor den Workerknoten aktualisiert.

Standardmäßig ist Ihre Fähigkeit den API-Server von Kubernetes zu aktualisieren in Ihrem Kubernetes-Master auf zwei Nebenversionen vor Ihrer aktuellen Version begrenzt. Beispiel: Wenn Ihre aktuelle API-Serverversion von Kubernetes die Version 1.7 ist und Sie auf Version 1.10 aktualisieren möchten, müssen Sie zuerst auf Version 1.8 oder 1.9 aktualisieren. Sie können die gewünschte Aktualisierung zwar erzwingen, doch kann eine Aktualisierung über drei oder mehr Nebenversionen hinweg unter Umständen zu nicht erwarteten Ergebnissen führen. Falls Ihr Cluster in einer nicht unterstützten Kubernetes-Version ausgeführt wird, müssen Sie die Aktualisierung möglicherweise erzwingen.

Das folgende Diagramm zeigt den Prozess, den Sie zum Aktualisieren des Masters durchlaufen können.

![Best Practice zur Aktualisierung des Masters](/images/update-tree.png)

Abbildung 1. Prozessdiagramm für die Aktualisierung des Kubernetes-Masters

**Achtung**: Nach der Durchführung des Aktualisierungsprozesses kann für einen Cluster kein Rollback auf eine Vorgängerversion durchgeführt werden. Achten Sie darauf, zunächst einen Testcluster zu verwenden und die Anweisungen für den Umgang mit potenziellen Problemen zu befolgen, bevor Sie Ihren Produktionsmaster aktualisieren.

Bei der Durchführung einer Aktualisierung für eine _Hauptversion_ oder _Nebenversion_ müssen Sie die folgenden Schritte ausführen:

1. Überprüfen Sie die [Kubernetes-Änderungen](cs_versions.html) und führen Sie alle Aktualisierungen durch, die mit der Markierung _Vor Master aktualisieren_ gekennzeichnet sind.
2. Aktualisieren Sie Ihre API-Server von Kubernetes und die zugehörigen Masterkomponenten von Kubernetes, indem Sie die GUI verwenden oder den [CLI-Befehl](cs_cli_reference.html#cs_cluster_update) ausführen. Wenn Sie den API-Server von Kubernetes aktualisieren, ist der API-Server für 5 bis 10 Minuten inaktiv. Während der Aktualisierung können Sie weder auf den Cluster zugreifen noch Änderungen am Cluster vornehmen. Allerdings werden Workerknoten, Apps und Ressourcen, die von Clusterbenutzern bereitgestellt wurden, nicht geändert und weiterhin ausgeführt.
3. Vergewissern Sie sich, dass die Aktualisierung abgeschlossen wurde. Überprüfen Sie die API-Serverversion von Kubernetes im {{site.data.keyword.Bluemix_notm}}-Dashboard oder führen Sie den folgenden Befehl aus: `bx cs clusters`.
4. Installieren Sie die Version von [`kubectl cli`](cs_cli_install.html#kubectl), die mit dem API-Server von Kubernetes übereinstimmt, der in Ihrem Kubernetes-Master ausgeführt wird.

Wenn die Aktualisierung des API-Servers von Kubernetes abgeschlossen ist, können Sie Ihre Workerknoten aktualisieren.

<br />


## Workerknoten aktualisieren
{: #worker_node}


Sie haben eine Benachrichtigung erhalten, die besagt, dass Sie Ihre Workerknoten aktualisieren müssen. Was bedeutet dies nun? Bei der Einrichtung von Sicherheitsupdates und -patchs für den API-Server von Kubernetes und anderen Komponenten des Kubernetes-Masters müssen Sie darauf achten, dass Ihre Workerknoten synchronisiert bleiben.
{: shortdesc}

Die Kubernetes-Version der Workerknoten darf nicht höher als die Version des API-Servers von Kubernetes sein. [Erstellen Sie zunächst einen Kubernetes-Master](#master).

**Achtung**:
<ul><li>Die Aktualisierung von Workerknoten kann zu Ausfallzeiten bei Ihren Apps und Services führen.</li>
<li>Die Daten werden gelöscht, wenn sie nicht außerhalb des Pods gespeichert werden.</li>
<li>Verwenden Sie [Replikate ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#replicas) in Ihren Bereitstellungen, um die erneute Zeitplanung für Pods auf den verfügbaren Knoten zu ermöglichen.</li></ul>

Was kann getan werden, um Ausfallzeiten zu vermeiden?

Während des Aktualisierungsprozesses werden bestimmte Knoten für eine gewisse Zeit nicht aktiv sein. Um Ausfallzeiten bei Ihren Anwendungen möglichst zu vermeiden, können Sie eindeutige Schlüssel in einer Konfigurationszuordnung definieren, in der Prozentsätze als Schwellenwerte für bestimmte Knotentypen für die Zeit der Durchführung des Aktualisierungsprozesses angegeben werden. Durch das Definieren von Regeln auf der Basis von standardmäßigen Kubernetes-Bezeichnungen und das Angeben eines Prozentsatzes für den maximal zulässigen Anteil an nicht verfügbaren Knoten können Sie sicherstellen, dass Ihre App betriebsbereit bleibt. Ein Knoten gilt als nicht verfügbar, wenn er den Bereitstellungsprozess noch nicht vollständig durchlaufen hat.

Wie werden die Schlüssel definiert?

Im Abschnitt für Dateninformationen der Konfigurationszuordnung können Sie bis zu 10 einzelne Regeln definieren, die an jedem angegebenen Zeitpunkt ausgeführt werden können. Damit die Aktualisierung durchgeführt werden kann, müssen die betreffenden Workerknoten jede der definierten Regeln erfüllen.

Die Schlüssel sind definiert. Was kommt als Nächstes?

Nachdem die gewünschten Regeln definiert worden sind, führen Sie den Befehl `bx cs worker-update` aus. Bei erfolgreicher Ausführung und Rückgabe einer entsprechenden Antwort werden die Workerknoten in eine Warteschlange zwecks Aktualisierung eingereiht. Die Knoten durchlaufen den Aktualisierungsprozess jedoch erst dann, wenn sämtliche Regeln erfüllt sind. Während sich die Knoten in der Warteschlange befinden, werden die Regeln in Intervallen überprüft, um festzustellen, ob die Knoten aktualisiert werden können.

Wie sieht es aus, wenn keine Konfigurationszuordnung definiert wurde?

Wenn die Konfigurationszuordnung nicht definiert wurde, wird die Standardeinstellung verwendet. Standardmäßig sind maximal 20% aller Workerknoten in den einzelnen Clustern während des Aktualisierungsprozesses nicht verfügbar.

Gehen Sie wie folgt vor, um Ihre Workerknoten zu aktualisieren:

1. Führen Sie alle Änderungen durch, die mit der Markierung _Nach Master aktualisieren_ in [Kubernetes-Änderungen](cs_versions.html) versehen sind.

2. Optional: Definieren Sie eine Konfigurationszuordnung (ConfigMap).
    Beispiel:

    ```
    apiVersion: v1
    kind: ConfigMap
    metadata:
      name: ibm-cluster-update-configuration
      namespace: kube-system
    data:
     drain_timeout_seconds: "120"
     zonecheck.json: |
       {
         "MaxUnavailablePercentage": 70,
         "NodeSelectorKey": "failure-domain.beta.kubernetes.io/zone",
         "NodeSelectorValue": "dal13"
       }
     regioncheck.json: |
       {
         "MaxUnavailablePercentage": 80,
         "NodeSelectorKey": "failure-domain.beta.kubernetes.io/region",
         "NodeSelectorValue": "us-south"
       }
     defaultcheck.json: |
       {
         "MaxUnavailablePercentage": 100
       }
    ```
    {:pre}
  <table summary="Die erste Zeile in der Tabelle erstreckt sich über beide Spalten. Die übrigen Zeilen sollten von links nach rechts gelesen werden, wobei der Parameter in der ersten Spalte und die entsprechende Beschreibung in der zweiten Spalte angegeben sind.">
  <caption>Komponenten der Konfigurationszuordnung</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Komponenten </th>
    </thead>
    <tbody>
      <tr>
        <td><code>drain_timeout_seconds</code></td>
        <td> Optional: Das Zeitlimit in Sekunden für das Entleeren, das während der Aktualisierung des Workerknotens auftritt. Beim Entleeren wird der Knoten auf `unschedulable` (nicht planbar) gesetzt, was verhindert, dass neue Pods auf diesem Knoten bereitgestellt werden. Beim Entleeren werden außerdem Pods vom Knoten gelöscht. Zulässige Werte liegen zwischen 1 und 180. Der Standardwert ist 30.</td>
      </tr>
      <tr>
        <td><code>zonecheck.json</code></br><code>regioncheck.json</code></td>
        <td> Dies sind Beispiele für eindeutige Schlüssel, für die Regeln festgelegt werden sollen. Die Namen der Schlüssel können beliebig gewählt werden; die Informationen werden von den festgelegten Konfigurationen im Schlüssel geparst. Für jeden definierten Schlüssel kann jeweils nur ein einziger Wert für <code>NodeSelectorKey</code> und <code>NodeSelectorValue</code> festgelegt werden. Wenn Sie Regeln für mehr als eine Region oder mehr als einen Standort (Rechenzentrum) festlegen möchten, müssen Sie jeweils einen neuen Schlüsseleintrag erstellen. </td>
      </tr>
      <tr>
        <td><code>defaultcheck.json</code></td>
        <td> Wenn keine gültige Definition der Zuordnung <code>ibm-cluster-update-configuration</code> vorhanden ist, dann können standardmäßig immer nur maximal 20% Ihrer Cluster gleichzeitig nicht verfügbar sein. Wird mindestens eine gültige Regel ohne globalen Standardwert definiert, dann sieht der neue Standardwert vor, dass 100 % der Worker gleichzeitig nicht verfügbar sein dürfen. Dies lässt sich entsprechend steuern, indem Sie einen Eintrag mit einem Standardprozentsatz erstellen. </td>
      </tr>
      <tr>
        <td><code>MaxUnavailablePercentage</code></td>
        <td> Dies ist für einen angegebenen Schlüssel der maximal zulässige Anteil an nicht verfügbaren Knoten. Der Wert wird als Prozentsatz angegeben. Ein Knoten ist dann nicht verfügbar, wenn er sich zum betreffenden Zeitpunkt im Bereitstellungs-, Neulade- oder Einrichtungsprozess befindet. Bei Überschreitung eines beliebigen definierten maximalen Prozentsatzes für Nichtverfügbarkeit werden die in der Warteschlange befindlichen Workerknoten blockiert und so von der Aktualisierung ausgeschlossen. </td>
      </tr>
      <tr>
        <td><code>NodeSelectorKey</code></td>
        <td> Dies ist der Typ der Bezeichnung, für die Sie eine Regel für einen angegebenen Schlüssel festlegen wollen. Sie können Regeln sowohl für die von IBM bereitgestellten Standardbezeichnungen als auch für die von Ihnen erstellten Bezeichnungen festlegen. </td>
      </tr>
      <tr>
        <td><code>NodeSelectorValue</code></td>
        <td> Dies ist die Teilmenge der Knoten in einem angegebenen Schlüssel, die von der Regel evaluiert werden sollen. </td>
      </tr>
    </tbody>
  </table>

    **Hinweis**: Es können maximal 10 Regeln definiert werden. Werden einer einzelnen Datei mehr als 10 Schlüssel hinzugefügt, wird nur eine Teilmenge der Informationen geparst.

3. Aktualisieren Sie Ihre Workerknoten über die grafische Benutzerschnittstelle (GUI) oder durch Ausführung des CLI-Befehls.
  * Zur Aktualisierung über das {{site.data.keyword.Bluemix_notm}}-Dashboard navigieren Sie zum Abschnitt für die `Workerknoten` Ihres Clusters und klicken dann auf `Worker aktualisieren`.
  * Zum Abrufen von Workerknoten-IDs müssen Sie den Befehl `bx cs workers <clustername_oder_-id>` ausführen. Wenn Sie mehrere Workerknoten auswählen, werden die Workerknoten zwecks Evaluierung der Aktualisierung in eine Warteschlange gestellt. Ergibt die Evaluierung, dass die Knoten für die Aktualisierung bereit sind, werden sie gemäß den in den Konfigurationen festgelegten Regeln aktualisiert.

    ```
    bx cs worker-update <clustername_oder_-id> <workerknoten1-id> <workerknoten2-id>
    ```
    {: pre}

4. Optional: Überprüfen Sie die Ereignisse, die von der Konfigurationszuordnung möglicherweise ausgelöst werden, sowie alle eventuell auftretenden Gültigkeitsfehler, indem Sie den folgenden Befehl ausführen und die **Ereignisse** (Events) untersuchen.
    ```
    kubectl describe -n kube-system cm ibm-cluster-update-configuration
    ```
    {: pre}

5. Vergewissern Sie sich, dass die Aktualisierung abgeschlossen wurde:
  * Überprüfen Sie die Kubernetes-Version im {{site.data.keyword.Bluemix_notm}}-Dashboard oder führen Sie den Befehl `bx cs workers <clustername_oder_-id>` aus.
  * Überprüfen Sie die Kubernetes-Version der Workerknoten, indem Sie den Befehl `kubectl get nodes` ausführen.
  * In bestimmten Fällen werden in älteren Clustern nach einer Aktualisierung doppelte Workerknoten mit dem Status **NotReady** (Nicht bereit) aufgelistet. Informationen zum Entfernen doppelter Workerknoten finden Sie im Abschnitt zur [Fehlerbehebung](cs_troubleshoot_clusters.html#cs_duplicate_nodes).

Nächste Schritte:
  - Wiederholen Sie den Aktualisierungsprozess für andere Cluster.
  - Informieren Sie die Entwickler, die im Cluster arbeiten, damit diese ihre `kubectl`-CLI auf die Version des Kubernetes-Masters aktualisieren können.
  - Wenn im Kubernetes-Dashboard keine Nutzungsdiagramme angezeigt werden, [löschen Sie den Pod `kube-dashboard`](cs_troubleshoot_health.html#cs_dashboard_graphs).
  





<br />



## Maschinentypen aktualisieren
{: #machine_type}

Sie können die Maschinentypen der Workerknoten aktualisieren, indem Sie neue Workerknoten hinzufügen und alte entfernen. Wenn Sie zum Beispiel über virtuelle Workerknoten auf nicht mehr verwendeten Maschinen mit `u1c` oder `b1c` in den jeweiligen Namen verfügen, erstellen Sie Workerknoten, die Maschinentypen mit `u2c` oder `b2c` in den Namen verwenden.
{: shortdesc}

Vorbemerkungen:
- [Richten Sie Ihre CLI](cs_cli_install.html#cs_cli_configure) (Befehlszeilenschnittstelle) auf Ihren Cluster aus.
- Wenn Sie Daten auf dem Workerknoten speichern, werden die Daten gelöscht, wenn sie nicht [außerhalb des Workerknotens gespeichert sind](cs_storage.html#storage).


**Achtung**: Die Aktualisierung von Workerknoten kann zu Ausfallzeiten bei Ihren Apps und Services führen. Die Daten werden gelöscht, wenn sie nicht [außerhalb des Pods gespeichert werden](cs_storage.html#storage).



1. Notieren Sie die Namen und Standorte der Workerknoten, die aktualisiert werden sollen.
    ```
    bx cs workers <clustername>
    ```
    {: pre}

2. Zeigen Sie die verfügbaren Maschinentypen an.
    ```
    bx cs machine-types <standort>
    ```
    {: pre}

3. Fügen Sie Workerknoten mit dem Befehl [bx cs worker-add](cs_cli_reference.html#cs_worker_add) hinzu. Geben Sie einen Maschinentyp an.

    ```
    bx cs worker-add --cluster <clustername> --machine-type <maschinentyp> --number <anzahl_der_workerknoten> --private-vlan <private_vlan-id> --public-vlan <öffentliche_vlan-id>
    ```
    {: pre}

4. Überprüfen Sie, dass die Workerknoten hinzugefügt wurden.

    ```
    bx cs workers <clustername>
    ```
    {: pre}

5. Wenn die hinzugefügten Workerknoten sich im Status `Normal` befinden, können Sie den veralteten Workerknoten entfernen. **Hinweis**: Wenn Sie einen Maschinentyp entfernen, der monatlich abgerechnet wird (zum Beispiel Bare-Metal), dann wird Ihnen der gesamte Monat noch berechnet.

    ```
    bx cs worker-rm <clustername> <workerknoten>
    ```
    {: pre}

6. Wiederholen Sie diese Schritte, um andere Workerknoten auf unterschiedliche Maschinentypen zu aktualisieren.










