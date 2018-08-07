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




# Sicherheit für {{site.data.keyword.containerlong_notm}}
{: #security}

Sie können die integrierten Sicherheitsfunktionen in {{site.data.keyword.containerlong}} für die Risikoanalyse und den Sicherheitsschutz verwenden. Diese Funktionen helfen Ihnen, die Kubernetes-Clusterinfrastruktur und Netzkommunikation zu schützen, die Rechenressourcen zu isolieren und die Einhaltung von Sicherheitsbestimmungen über die einzelnen Infrastrukturkomponenten und Containerbereitstellungen hinweg sicherzustellen.
{: shortdesc}



## Sicherheit nach Clusterkomponente
{: #cluster}

Jeder {{site.data.keyword.containerlong_notm}}-Cluster verfügt über Sicherheitsfunktionen, die in seine [Master](#master)- und [Worker](#worker)knoten integriert sind.
{: shortdesc}

Wenn Sie eine Firewall haben oder `kubectl`-Befehle aus Ihrem lokalen System ausführen möchten, aber Unternehmensnetzrichtlinien den Zugriff auf öffentliche Internetendpunkte verhindern, [öffnen Sie Ports in Ihrer Firewall](cs_firewall.html#firewall). Wenn Sie Apps in Ihrem Cluster mit einem lokalen Netz oder mit anderen Apps, die sich außerhalb Ihres Netzes befinden, verbinden möchten, [richten Sie VPN-Konnektivität ein](cs_vpn.html#vpn).

Das folgende Diagramm zeigt die Sicherheitsfunktionen gruppiert nach Kubernetes-Master, Workerknoten und Container-Images.

<img src="images/cs_security.png" width="400" alt="{{site.data.keyword.containershort_notm}}-Clustersicherheit" style="width:400px; border-style: none"/>


<table summary="Die erste Zeile in der Tabelle erstreckt sich über beide Spalten. Die verbleibenden Zeilen enthalten von links nach rechts die Sicherheitskomponenten in der ersten Spalte und die entsprechenden Features in der zweiten Spalte.">
<caption>Sicherheit nach Komponente</caption>
  <thead>
    <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Integrierte Clustersicherheitseinstellungen in {{site.data.keyword.containershort_notm}}</th>
  </thead>
  <tbody>
    <tr>
      <td>Kubernetes-Master</td>
      <td>Der Kubernetes-Master in jedem Cluster wird von IBM verwaltet und ist hoch verfügbar. Der Master enthält Sicherheitseinstellungen für {{site.data.keyword.containershort_notm}}, mit denen die Einhaltung der Sicherheitsbestimmungen und eine sichere Kommunikation von und zu den Workerknoten sichergestellt wird. Sicherheitsupdates werden von IBM bei Bedarf durchgeführt. Vom dedizierten Kubernetes-Master werden alle Kubernetes-Ressourcen im Cluster zentral gesteuert und überwacht. Abhängig von den Bereitstellungsvoraussetzungen und der Kapazität im Cluster werden die containerisierten Apps vom Kubernetes-Master automatisch zur Bereitstellung für die verfügbaren Workerknoten geplant. Weitere Informationen finden Sie unter [Sicherheit des Kubernetes-Masters](#master).</td>
    </tr>
    <tr>
      <td>Workerknoten</td>
      <td>Container werden auf Workerknoten bereitgestellt, die der Nutzung durch einen Cluster vorbehalten sind und gegenüber IBM Kunden die Rechen-, Netz- und Speicherisolation sicherstellen. {{site.data.keyword.containershort_notm}} stellt integrierte Sicherheitsfunktionen zur Verfügung, um die Sicherheit
Ihrer Workerknoten im privaten und im öffentlichen Netz sicherzustellen und für die Einhaltung von Sicherheitsbestimmungen für Workerknoten zu sorgen. Weitere Informationen enthält [Sicherheit von Workerknoten](#worker). Zusätzlich können Sie [Calico-Netzrichtlinien](cs_network_policy.html#network_policies) hinzufügen, um den Netzverkehr anzugeben, den Sie zu und von einem Pod in einem Workerknoten zulassen oder blockieren.</td>
    </tr>
    <tr>
      <td>Images</td>
      <td>Als Clusteradministrator können Sie ein eigenes sicheres Docker-Image-Repository in {{site.data.keyword.registryshort_notm}} einrichten, in dem Sie Docker-Images sicher speichern und zur gemeinsamen Nutzung durch Ihre Clusterbenutzer freigeben können. Jedes Image in Ihrer privaten Registry wird von Vulnerability Advisor überprüft, um die Bereitstellung sicherer Container sicherzustellen. Vulnerability Advisor ist eine Komponente von {{site.data.keyword.registryshort_notm}}, die nach potenziellen Sicherheitslücken sucht, Sicherheitsempfehlungen ausgibt und Anweisungen zur Schließung von Sicherheitslücken bereitstellt. Weitere Informationen finden Sie unter [Imagesicherheit in {{site.data.keyword.containershort_notm}}](#images).</td>
    </tr>
  </tbody>
</table>

<br />


## Kubernetes-Master
{: #master}

Prüfen Sie die integrierten Sicherheitsfeatures, die den Kubernetes-Master schützt und die Netzkommunikation für den Cluster sichert.
{: shortdesc}

<dl>
  <dt>Vollständig verwalteter und dedizierter Kubernetes-Master</dt>
    <dd>Jeder Kubernetes-Cluster in {{site.data.keyword.containershort_notm}} wird von einem dedizierten Kubernetes-Master gesteuert, der von IBM in einem IBM eigenen Konto von IBM Cloud Infrastructure (SoftLayer) verwaltet wird. Der Kubernetes-Master ist mit den folgenden dedizierten Komponenten konfiguriert, die nicht mit anderen IBM Kunden oder von anderen Clustern im selben IBM Konto gemeinsam genutzt werden.
      <ul><li>Datenspeicher 'etcd': Speichert alle Kubernetes-Ressourcen eines Clusters, z. B. Services, Bereitstellungen und Pods. Kubernetes-Konfigurationszuordnungen und geheime Kubernetes-Schlüssel sind App-Daten, die in Form von Schlüssel/Wert-Paaren gespeichert werden, damit sie von einer in einem Pod ausgeführten App verwendet werden können. Daten in 'etcd' werden auf einem verschlüsselten
Datenträger gespeichert, der von IBM verwaltet wird. Wenn die Daten an einen Pod gesendet werden, werden sie über TLS verschlüsselt, um den Datenschutz und die Datenintegrität sicherzustellen. </li>
      <li>'kube-apiserver': Dient als Haupteinstiegspunkt für alle Anforderungen vom Workerknoten zum Kubernetes-Master. Die Komponente 'kube-apiserver' validiert und verarbeitet Anforderungen und besitzt Lese- und Schreibberechtigungen für den Datenspeicher 'etcd'.</li>
      <li>'kube-scheduler': Entscheidet unter Berücksichtigung von Kapazitätsbedarf und Leistungsanforderungen, Beschränkungen durch Hardware- und Softwarerichtlinien, Anti-Affinitäts-Spezifikationen und Anforderungen für Workloads darüber, wo die Bereitstellung von Pods erfolgt. Wird kein Workerknoten gefunden, der mit den Anforderungen übereinstimmt, so wird der Pod nicht im Cluster bereitgestellt.</li>
      <li>'kube-controller-manager': Verantwortlich für die Überwachung von Replikatgruppen und für die Erstellung entsprechender Pods, um den gewünschten Zustand (Soll-Status) zu erreichen.</li>
      <li>'OpenVPN': Für {{site.data.keyword.containershort_notm}} spezifische Komponente, die sichere Netzkonnektivität für die gesamte Kommunikation zwischen dem Kubernetes-Master und dem Workerknoten bereitstellt.</li></ul></dd>
  <dt>Mit TLS geschützte Netzkonnektivität für die gesamte Kommunikation von den Workerknoten zum Kubernetes-Master</dt>
    <dd>{{site.data.keyword.containershort_notm}} schützt die Verbindung zum Kubernetes-Master durch Generieren von TLS-Zertifikaten, mit denen für jeden Cluster die Kommunikation zu und von dem Datenspeicher 'kube-apiserver' und 'etcd' verschlüsselt wird. Diese Zertifikate werden zu keinem Zeitpunkt von mehreren Clustern oder Komponenten des Kubernetes-Masters gemeinsam genutzt.</dd>
  <dt>Mit OpenVPN geschützte Netzkonnektivität für die gesamte Kommunikation vom Kubernetes-Master zu den Workerknoten</dt>
    <dd>Kubernetes schützt die Kommunikation zwischen dem Kubernetes-Master und den Workerknoten zwar durch die Verwendung des Protokolls `https`, doch auf dem Workerknoten wird keine standardmäßige Authentifizierung bereitgestellt. Zum Schutz dieser Kommunikation richtet {{site.data.keyword.containershort_notm}} bei der Erstellung des Clusters automatisch eine OpenVPN-Verbindung zwischen dem Kubernetes-Master und den Workerknoten ein.</dd>
  <dt>Kontinuierliche Überwachung des Kubernetes-Master-Netzes</dt>
    <dd>Jeder Kubernetes-Master wird kontinuierlich von IBM überwacht, um Denial-of-Service-Attacken (DoS) auf Prozessebene zu steuern und zu korrigieren.</dd>
  <dt>Einhaltung der Sicherheitsbestimmungen durch die Kubernetes-Master-Knoten</dt>
    <dd>{{site.data.keyword.containershort_notm}} überprüft automatisch jeden Knoten, auf dem der Kubernetes-Master bereitgestellt ist, auf Schwachstellen bzw. Sicherheitslücken, die in Kubernetes festgestellt wurden, und auf betriebssystemspezifische Korrekturen (Fixes) für die Sicherheit. Werden Schwachstellen bzw. Sicherheitslücken festgestellt, wendet {{site.data.keyword.containershort_notm}} automatisch entsprechende Korrekturen (Fixes) und beseitigt Schwachstellen bzw. Sicherheitslücken zugunsten des Benutzers, um den Schutz des Masterknotens sicherzustellen. </dd>
</dl>

<br />


## Workerknoten
{: #worker}

Prüfen Sie die integrierten Sicherheitsfeatures für Workerknoten, die die Workerknotenumgebung schützen und die Ressourcen-, Netz- und
Speicherisolation sicherstellen.
{: shortdesc}

<dl>
  <dt>Eigentumsrechte für Workerknoten</dt>
    <dd>Die Eigentumsrechte für Workerknoten sind vom Typ des Clusters abhängig, den Sie erstellen. <p> Workerknoten in kostenlosen Clustern werden im Konto der IBM Cloud-Infrastruktur (SoftLayer) bereitgestellt, dessen Kontoeigner IBM ist. Benutzer können zwar Apps in den Workerknoten bereitstellen, sie können in den Workerknoten jedoch weder Einstellungen ändern noch zusätzliche Software installieren.</p>
    <p>Workerknoten in Standardclustern werden im Konto der IBM Cloud-Infrastruktur (SoftLayer) bereitgestellt, das dem öffentlichen oder dedizierten IBM Cloud-Konto des Kunden zugeordnet ist. Die Workerknoten sind Eigentum des Kunden. Die Kunden können bei Bedarf Sicherheitseinstellungen ändern oder zusätzliche Software auf den Workerknoten installieren, die von {{site.data.keyword.containerlong}} bereitgestellt werden.</p> </dd>
  <dt>Isolation der Rechen-, Netz- und Speicherinfrastruktur</dt>
    <dd><p>Beim Erstellen eines Clusters werden Workerknoten von IBM als virtuelle Maschinen bereitgestellt. Workerknoten sind einem Cluster zugeordnet und hosten nicht die Arbeitslast anderer Cluster.</p>
    <p> Jedes {{site.data.keyword.Bluemix_notm}}-Konto wird mit VLANs von IBM Cloud Infrastructure (SoftLayer) eingerichtet, um die Qualität der Netzleistung und Isolation auf den Workerknoten sicherzustellen. Darüber hinaus können Sie Workerknoten als privat deklarieren, indem Sie sie nur mit einem privaten VLAN verbinden.</p> <p>Damit Daten in Ihrem Cluster als persistent erhalten bleiben, können Sie den dedizierten NFS-basierten Dateispeicher von IBM Cloud Infrastructure (SoftLayer) einrichten und die integrierten Funktionen für Datensicherheit dieser Plattform nutzen.</p></dd>
  <dt>Einrichtung geschützter Workerknoten</dt>
    <dd><p>Jeder Workerknoten wird mit dem Betriebssystem Ubuntu eingerichtet, das vom Eigner des Workerknotens nicht geändert werden kann. Da das Betriebssystem des Workerknotens Ubuntu ist, müssen alle Container, die auf dem Workerknoten bereitgestellt werden, eine Linux-Distribution mit dem Ubuntu-Kernel verwenden. Linux-Distributionen, die auf andere Weise mit dem Kernel kommunizieren müssen, können nicht verwendet werden. Um das Betriebssystem der Workerknoten gegen potenzielle Angriffe zu schützen, wird jeder Workerknoten mit extrem fortgeschrittenen Firewalleinstellungen konfiguriert, die durch iptables-Regeln von Linux umgesetzt werden.</p>
    <p>Alle Container, die auf Kubernetes ausgeführt werden, sind durch vordefinierte Calico-Netzrichtlinieneinstellungen geschützt, die bei der Clustererstellung auf jedem Workerknoten konfiguriert werden. Diese Konstellation stellt die sichere Netzkommunikation zwischen Workerknoten und Pods sicher.</p>
    <p>SSH-Zugriff ist auf dem Workerknoten inaktiviert. Wenn Sie über einen Standardcluster verfügen und weitere Features auf Ihrem Workerknoten installieren möchten, können Sie [Kubernetes-Dämon-Sets ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset) für Vorgänge verwenden, die auf jedem Workerknoten ausgeführt werden sollen. Verwenden Sie für alle einmaligen Aktionen, die ausgeführt werden müssen, [Kubernetes-Jobs ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/).</p></dd>
  <dt>Einhaltung der Sicherheitsbestimmungen für die Kubernetes-Workerknoten</dt>
    <dd>IBM arbeitet mit internen und externen Beratungsteams für Sicherheit zusammen, um potenzielle Schwachstellen in Bezug auf die Einhaltung von Sicherheitsbestimmungen zu beseitigen. <b>Wichtig</b>: Setzen Sie den [Befehl](cs_cli_reference.html#cs_worker_update) `bx cs worker-update` in regelmäßigen Abständen (z. B. monatlich) ab, um Updates und Sicherheitspatches für das Betriebssystem bereitzustellen und um die Kubernetes-Version zu aktualisieren. Wenn Updates verfügbar sind, werden Sie benachrichtigt, wenn Sie Informationen zu den Workerknoten anzeigen, z. B. mit den Befehlen `bx cs workers <cluster_name>` oder `bx cs worker-get <cluster_name> <worker_ID>`.</dd>
  <dt>Option zum Bereitstellen der Workerknoten auf physischen Servern (Bare-Metal-Servern)</dt>
    <dd>Wenn Sie Ihre Workerknoten auf physischen Bare-Metal-Servern bereitstellen möchten (und nicht auf virtuellen Serverinstanzen), haben Sie mehr Kontrolle über den Rechenhost, wie z. B. über den Speicher oder die CPU. Durch diese Konfiguration wird der Hypervisor der virtuellen Maschine entfernt, der physische Ressourcen zu virtuellen Maschinen zuordnet, die auf dem Host ausgeführt werden. Stattdessen sind alle Ressourcen der Bare-Metal-Maschine ausschließlich dem Worker gewidmet, also müssen Sie sich keine Sorgen machen, dass "lärmende Nachbarn" Ressourcen gemeinsam nutzen oder die Leistung verlangsamen. Bare-Metal-Server sind Ihnen mit allen Ressourcen zugeordnet, die für die Clusternutzung zur Verfügung stehen.</dd>
  <dt id="trusted_compute">{{site.data.keyword.containershort_notm}} mit Trusted Compute</dt>
    <dd><p>Wenn Sie [Ihren Cluster auf einem Bare-Metal-Server bereitstellen](cs_clusters.html#clusters_ui), der Trusted Compute unterstützt, können Sie Trusted Compute aktivieren. Der TPM-Chip (TPM, Trusted Platform Module) ist auf jedem Bare-Metal-Workerknoten im Cluster aktiviert, der Trusted Compute unterstützt (einschließlich zukünftiger Knoten, die Sie dem Cluster hinzufügen). Nachdem Sie Trusted Compute aktiviert haben, können Sie es später nicht mehr inaktivieren. Ein Trust-Server wird auf dem Masterknoten bereitgestellt, und ein Trust-Agent wird als Pod auf dem Workerknoten implementiert. Wenn der Workerknoten gestartet wird, überwacht der Trust-Agent-Pod die einzelnen Phasen des Prozesses.</p>
    <p>Wenn zum Beispiel ein nicht berechtigter Benutzer Zugriff auf Ihr System erlangt und den OS-Kernel durch zusätzliche Logik zum Sammeln von Daten verändert, erkennt der Trust-Agent diese Änderung und markiert die Knoten als nicht vertrauenswürdig. Mit Trusted Compute können Sie Ihre Workerknoten auf Manipulation überprüfen.</p>
    <p><strong>Hinweis</strong>: Trusted Compute ist für Bare-Metal-Maschinentypen verfügbar. Beispielsweise unterstützen die GPU-Typen `mgXc` Trusted Compute nicht.</p></dd>
  <dt id="encrypted_disks">Verschlüsselte Platte</dt>
    <dd><p>Standardmäßig stellt {{site.data.keyword.containershort_notm}} zwei lokale SSD-verschlüsselte Datenpartitionen für alle Workerknoten bereit, wenn diese Workerknoten bereitgestellt werden. Die erste Partition ist nicht verschlüsselt, und die zweite Partition, die an _/var/lib/docker_ angehängt ist, wird mithilfe von LUKS-Verschlüsselungsschlüsseln entsperrt. Alle Worker in den einzelnen Kubernetes-Clustern haben ihren eigenen eindeutigen LUKS-Verschlüsselungsschlüssel, der von {{site.data.keyword.containershort_notm}} verwaltet wird. Wenn Sie einen Cluster erstellen oder einen Workerknoten zu einem vorhandenen Cluster hinzufügen, werden die Schlüssel sicher extrahiert und nach dem Entsperren der verschlüsselten Platte gelöscht.</p>
    <p><b>Hinweis</b>: Verschlüsselung kann sich negativ auf die Platten-E/A-Leistung auswirken. Für Arbeitslasten, die eine eine leistungsfähige Platten-E/A erfordern, sollten Sie einen Cluster mit aktivierter und mit inaktivierter Verschlüsselung testen, um besser entscheiden zu können, ob die Verschlüsselung ausgeschaltet werden muss.</p></dd>
  <dt>Unterstützung für Netzfirewalls von IBM Cloud Infrastructure (SoftLayer)</dt>
    <dd>{{site.data.keyword.containershort_notm}} ist mit allen [Firewallangeboten von IBM Cloud Infrastructure (SoftLayer) ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") kompatibel](https://www.ibm.com/cloud-computing/bluemix/network-security). Sie können unter {{site.data.keyword.Bluemix_notm}} Public eine Firewall mit angepassten Netzrichtlinien einrichten, um für Ihren Standardcluster dedizierte Netzsicherheit bereitzustellen und unbefugten Zugriff zu erkennen und zu unterbinden. Sie können beispielsweise [Virtual Router Appliance](/docs/infrastructure/virtual-router-appliance/about.html) als Ihre Firewall und zum Blockieren unerwünschten Datenverkehrs einrichten. Wenn Sie eine Firewall einrichten, [müssen Sie auch die erforderlichen Ports und IP-Adressen für die einzelnen Regionen öffnen](cs_firewall.html#firewall), damit der Master und die Workerknoten kommunizieren können.</dd>
  <dt>Services privat belassen oder Service und Apps selektiv dem öffentlichen Internet zugänglich machen</dt>
    <dd>Sie können entscheiden, ob Sie Ihre Services und Apps privat belassen wollen. Durch Nutzung der integrierten Sicherheitsfeatures können Sie außerdem die geschützte Kommunikation zwischen Workerknoten und Pods sicherstellen. Um Services und Apps dem öffentlichen Internet zugänglich zu machen, können Sie die Ingress-Unterstützung und die
Unterstützung der Lastausgleichsfunktion nutzen, um Ihre Services auf sichere Weise öffentlich verfügbar zu machen.</dd>
  <dt>Sichere Verbindung Ihrer Workerknoten und Apps mit einem lokalen Rechenzentrum</dt>
    <dd><p>Um eine Verbindung Ihrer Workerknoten und Apps mit einem lokalen Rechenzentrum einzurichten, können Sie einen VPN-IPSec-Endpunkt mit einem StrongSwan-Service, einer Virtual Router Appliance oder einer Fortigate Security Appliance konfigurieren.</p>
    <ul><li><b>StrongSwan-IPSec-VPN-Service</b>: Sie können einen [StrongSwan-IPSec-VPN-Service ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.strongswan.org/) konfigurieren, der Ihren Kubernetes-Cluster sicher mit einem lokalen Netz verbindet. Der StrongSwan-IPSec-VPN-Service stellt einen sicheren End-to-End-Kommunikationskanal über das Internet bereit, der auf der standardisierten IPsec-Protokollsuite (IPsec - Internet Protocol Security) basiert. Um eine sichere Verbindung zwischen Ihrem Cluster und einem lokalen Netz einzurichten, [konfigurieren und implementieren Sie den StrongSwan-IPSec-VPN-Service](cs_vpn.html#vpn-setup) direkt in einem Pod in Ihrem Cluster.
    </li>
    <li><b>Virtual Router Appliance (VRA) oder Fortigate Security Appliance (FSA)</b>: Sie können eine [VRA](/docs/infrastructure/virtual-router-appliance/about.html) oder eine [FSA](/docs/infrastructure/fortigate-10g/about.html) zum Konfigurieren eines IPSec-VPN-Endpunkts einrichten. Diese Option ist hilfreich, wenn der Cluster größer ist, Sie über das VPN auf Nicht-Kubernetes-Ressourcen oder über ein einzelnes VPN auf mehrere Cluster zugreifen möchten. Informationen zum Konfigurieren einer VRA finden Sie unter [VPN-Konnektivität mit VRA konfigurieren](cs_vpn.html#vyatta). </li></ul></dd>

  <dt>Kontinuierliche Überwachung und Protokollierung der Clusteraktivität</dt>
    <dd>Bei Standardclustern können alle clusterbezogenen Ereignisse protokolliert und an {{site.data.keyword.loganalysislong_notm}} und {{site.data.keyword.monitoringlong_notm}} gesendet werden. Zu diesen Ereignissen zählen das Hinzufügen eines Workerknotens, der Fortschritt bei einer rollierenden Aktualisierung oder Informationen zur Kapazitätsnutzung. Sie können die [Protokollierung von Clustern](/docs/containers/cs_health.html#logging) und [die Überwachung von Clustern konfigurieren](/docs/containers/cs_health.html#view_metrics), um die Ereignisse auszuwählen, die Sie überwachen möchten. </dd>
</dl>

<br />


## Images
{: #images}

Verwenden Sie integrierte Sicherheitsfunktionen, um die Sicherheit und Integrität Ihrer Images zu verwalten.
{: shortdesc}

<dl>
<dt>Geschütztes privates Docker-Image-Repository in {{site.data.keyword.registryshort_notm}}</dt>
  <dd>Richten Sie Ihr eigenes Docker-Image-Repository in einer hoch verfügbaren, skalierbaren und privaten Multi-Tenant-Image-Registry ein, die von IBM gehostet und verwaltet wird. Durch die Verwendung der Registry können Sie Docker-Images erstellen, sicher speichern und gemeinsam mit anderen Clusterbenutzern nutzen.
  <p>Erfahren Sie mehr über das [Sichern der persönliche Daten](cs_secure.html#pi) bei der Arbeit mit Container-Images.</p></dd>
<dt>Einhaltung von Sicherheitsbestimmungen für Images</dt>
  <dd>Wenn Sie {{site.data.keyword.registryshort_notm}} verwenden, können Sie die integrierte Sicherheitsfunktion zum Scannen einsetzen, die Vulnerability Advisor bereitstellt. Jedes Image, das mit einer Push-Operation an Ihren Namensbereich übertragen wird, wird automatisch auf Schwachstellen und Sicherheitslücken gescannt. Dieser Scanvorgang erfolgt im Abgleich mit einer Datenbank mit bekannten CentOS-, Debian-, Red Hat- und Ubuntu-Problemen. Werden derartige Schwachstellen oder Sicherheitslücken festgestellt, stellt Vulnerability Advisor Anweisung dazu bereit, wie Sie diese beseitigen bzw. schließen, um die Integrität und Sicherheit des Image sicherzustellen.</dd>
</dl>

Informationen darüber, wie Sie die Schwachstellenanalyse für Ihre Images anzeigen, finden Sie in der [Dokumentation zu Vulnerability Advisor](/docs/services/va/va_index.html#va_registry_cli).

<br />


## Netzbetrieb in Clustern
{: #in_cluster_network}

Die geschützte clusterinterne Netzkommunikation zwischen Workerknoten und Pods erfolgt über private virtuelle LANs (VLANs). Ein VLAN konfiguriert eine Gruppe von Workerknoten und Pods so, als wären diese an dasselbe physische Kabel angeschlossen.
{:shortdesc}

Wenn Sie einen Cluster erstellen, wird jeder Cluster automatisch mit einem privaten VLAN von IBM verbunden. Das private VLAN bestimmt, welche private IP-Adresse einem Workerknoten bei der Clustererstellung zugewiesen wird.

|Clustertyp|Manager des privaten VLANs für den Cluster|
|------------|-------------------------------------------|
|Kostenlose Cluster in {{site.data.keyword.Bluemix_notm}}|{{site.data.keyword.IBM_notm}}|
|Standardcluster in {{site.data.keyword.Bluemix_notm}}|Sie bei Ihrem Konto von IBM Cloud Infrastructure (SoftLayer) <p>**Tipp:** Wenn
Sie Zugriff auf alle VLANs in Ihrem Konto haben wollen, aktivieren Sie [VLAN-Spanning](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning).</p>|
{: caption="Manager von privaten VLANs" caption-side="top"}

Allen Pods, die auf einem Workerknoten bereitgestellt werden, wird ebenfalls eine private IP-Adresse zugewiesen. Pods wird eine
IP im privaten Adressbereich '172.30.0.0/16' zugewiesen und eine Weiterleitung findet nur zwischen Workerknoten statt. Vermeiden Sie Konflikte, indem Sie diesen IP-Bereich nicht auf Knoten verwenden, die mit Ihren Workerknoten kommunizieren. Workerknoten und Pods können im privaten Netz durch die Verwendung der privaten IP-Adressen sicher kommunizieren. Wenn ein Pod ausfällt oder ein Workerknoten neu erstellt werden muss, wird jedoch eine neue private IP-Adresse zugewiesen.

Standardmäßig ist es schwierig, sich ändernde private IP-Adressen für Apps nachzuverfolgen, die hoch verfügbar sein müssen. Um dies zu vermeiden, können Sie die integrierten Erkennungsfunktionen des Kubernetes-Service nutzen und Apps als Cluster-IP-Services auf dem privaten Netz im Cluster zugänglich machen. Ein Kubernetes-Service fasst eine Gruppe von Pods zusammen und stellt diesen Pods eine Netzverbindung für andere Services im Cluster zur Verfügung, ohne hierbei die tatsächlichen, privaten IP-Adressen der einzelnen Pods preiszugeben. Wenn Sie einen Cluster-IP-Service erstellen, wird dem Service eine private IP-Adresse aus dem privaten Adressbereich '10.10.10.0/24' zugewiesen. Verwenden Sie, wie beim privaten Pod-Adressbereich, diesen IP-Bereich nicht auf Knoten, die mit Ihren Workerknoten kommunizieren. Auf diese IP-Adresse kann nur aus dem Cluster selbst heraus zugegriffen werden. Vom Internet aus ist der Zugriff auf diese IP-Adresse nicht möglich. Zur gleichen Zeit wird für den Service ein Eintrag für die DNS-Suche erstellt und in der Komponente 'kube-dns' des Clusters gespeichert. Der DNS-Eintrag enthält den Namen des Service, den Namensbereich, in dem der Service erstellt wurde und den Link zur zugewiesenen privaten IP-Adresse des Clusters.

Um auf einen Pod zuzugreifen, der sich hinter einem Cluster-IP-Service befindet, kann die App entweder die private Cluster-IP-Adresse des Service verwenden oder eine Anforderung unter Nennung des Servicenamens senden. Wenn Sie den Namen des Service nennen, wird dieser in der 'kube-dns'-Komponente gesucht und an die private Cluster-IP-Adresse des Service weitergeleitet. Wenn
eine Anforderung den Service erreicht, stellt der Service sicher, dass alle Anforderungen im gleichen Maße an die Pods weitergeleitet werden, und zwar unabhängig von ihren jeweiligen privaten IP-Adressen und dem Workerknoten, auf dem sie bereitgestellt sind.

Weitere Informationen zum Erstellen eines Service vom Typ 'Cluster-IP' finden Sie unter [Kubernetes-Services ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services---service-types).

Informationen über sichere Verbindungen von Apps in einem Kubernetes-Cluster zu einem lokalen Netz finden Sie in [VPN-Konnektivität einrichten](cs_vpn.html#vpn). Informationen zum Bereitstellen Ihrer Apps für die externe Netzkommunikation finden Sie unter [Öffentlichen Zugriff auf Apps zulassen](cs_network_planning.html#public_access).


<br />


## Cluster-Vertrauensbeziehung
{: cs_trust}

Standardmäßig stellt {{site.data.keyword.containerlong_notm}} viele [Features für Ihre Clusterkomponenten](#cluster) bereit, sodass Sie Ihre containerisierten Apps in einer Umgebung mit umfassender Sicherheit bereitstellen können. Optimieren Sie die Vertrauensbeziehung in Ihrem Cluster, damit Sie noch sicherer sein können, dass die Vorgänge, die in Ihrem Cluster passieren, beabsichtigt sind. Sie können die Vertrauensbeziehung auf verschiedene Weisen in Ihrem Cluster implementieren, wie im folgenden Diagramm gezeigt.
{:shortdesc}

![Container mit vertrauenswürdigen Inhalten bereitstellen](images/trusted_story.png)

1.  **{{site.data.keyword.containerlong_notm}} mit Trusted Compute**: Auf Bare-Metal-Clustern können Sie eine Vertrauensbeziehung aktivieren. Der Trust Agent überwacht Hardwarestartprozesse und berichtet alle Änderungen, damit Sie Ihre Bare-Metal-Workerknoten auf Manipulation überprüfen können. Mit Trusted Compute können Sie Ihre Container auf überprüften Bare-Metal-Hosts bereitstellen, sodass Ihre Workloads auf vertrauenswürdiger Hardware ausgeführt werden. Beachten Sie, dass einige Bare-Metal-Maschinen, wie GPU, Trusted Compute nicht unterstützen. [Weitere Informationen zur Funktionsweise von Trusted Compute](#trusted_compute).

2.  **Content Trust für Ihre Images**: Stellen Sie die Integrität Ihrer Images durch Aktivieren von Content Trust in Ihrer {{site.data.keyword.registryshort_notm}} sicher. Mit vertrauenswürdigen Inhalten können Sie steuern, wer Images als vertrauenswürdig signieren kann. Nachdem vertrauenswürdige Unterzeichner ein Image in Ihre Registry übertragen haben, können die Benutzer den signierten Inhalt extrahieren, um die Quelle des Images zu überprüfen. Weitere Informationen finden Sie unter [Images für vertrauenswürdige Inhalte unterzeichnen](/docs/services/Registry/registry_trusted_content.html#registry_trustedcontent).

3.  **Container Image Security Enforcement (beta)**: Erstellen Sie einen Admission Controller mit angepassten Richtlinien, sodass Sie Container-Images überprüfen können, bevor sie bereitgestellt werden. Mit Container Image Security Enforcement steuern Sie, von wo die Images bereitgestellt werden, und stellen sicher, dass sie [Vulnerability Advisor](/docs/services/va/va_index.html)-Richtlinien oder [Content Trust](/docs/services/Registry/registry_trusted_content.html#registry_trustedcontent)-Anforderungen erfüllen. Falls eine Bereitstellung die festgelegten Richtlinien nicht erfüllt, verhindern Sicherheitsfunktionen Änderungen an Ihrem Cluster. Weitere Informationen finden Sie unter [Container Image Security Enforcement (beta)](/docs/services/Registry/registry_security_enforce.html#security_enforce).

4.  **Anfälligkeitsscanner für Container**: Standardmäßig scannt Vulnerability Advisor in {{site.data.keyword.registryshort_notm}} gespeicherte Images. Um den Status von Live-Containern in Ihrem Cluster zu prüfen, können Sie den Container-Scanner installieren. Weitere Informationen finden Sie unter [Container-Scanner installieren](/docs/services/va/va_index.html#va_install_livescan).

5.  **Netzanalyse mit Security Advisor (Vorschau)**: Mit {{site.data.keyword.Bluemix_notm}} Security Advisor können Sie Sicherheits-Know-how aus {{site.data.keyword.Bluemix_notm}}-Services wie Vulnerability Advisor und {{site.data.keyword.cloudcerts_short}} zentral zusammenführen. Wenn Sie in Ihrem Cluster Security Advisor aktivieren, können Sie Berichte zu verdächtigem eingehenden und ausgehenden Netzverkehr anzeigen. Weitere Informationen finden Sie unter [Netzanalysen](/docs/services/security-advisor/network-analytics.html#network-analytics). Informationen zur Installation finden Sie unter [Überwachung verdächtiger Clients und Server-IP-Adressen für einen Kubernetes-Cluster einrichten](/docs/services/security-advisor/setup_cluster.html). 

6.  **{{site.data.keyword.cloudcerts_long_notm}} (beta)**: Wenn Sie über ein Cluster in der Region 'Vereinigte Staaten (Süden)' verfügen und [Ihre App mithilfe einer angepassten Domäne mit TLS zugänglich machen](https://console.bluemix.net/docs/containers/cs_ingress.html#ingress_expose_public) möchten, können Sie Ihr TLS-Zertifikat in {{site.data.keyword.cloudcerts_short}} speichern. Auf bereits oder bald abgelaufene Zertifikate kann auch in Ihrem Security Advisor-Dashboard hingewiesen werden. Weitere Informationen finden Sie unter [Einführung in {{site.data.keyword.cloudcerts_short}}](/docs/services/certificate-manager/index.html#gettingstarted).

<br />


## Persönliche Daten sichern
{: #pi}

Sie sind für den Schutz Ihrer persönlichen Daten in Kubernetes-Ressourcen und Container-Images verantwortlich. Zu solchen Daten zählen Name, Adresse, Telefonnummer, E-Mail-Adresse oder andere Informationen, anhand derer Sie, Ihre Kunden oder Dritte identifiziert, kontaktiert oder lokalisiert werden können.
{: shortdesc}

<dl>
  <dt>Geheimen Kubernetes-Schlüssel zum Speichern persönlicher Daten verwenden</dt>
  <dd>Speichern Sie nur persönliche Daten in Kubernetes-Ressourcen, die zum Speichern personenbezogenen Daten konzipiert sind. Verwenden Sie beispielsweise nicht Ihren Namen im Namen eines Kubernetes-Namensbereichs, einer Bereitstellung, eines Service oder einer Konfigurationszuordnung. Um einen angemessen Schutz und eine optimale Verschlüsselung zu erzielen, speichern Sie stattdessen persönliche Daten in <a href="cs_app.html#secrets">geheimen Kubernetes-Schlüsseln</a>.</dd>

  <dt>Geheime Kubernetes-Schlüssel für Image-Pulloperationen (`imagePullSecret`) zum Speichern von Berechtigungsnachweisen für Image-Registrys verwenden</dt>
  <dd>Speichern Sie persönliche Daten nicht in Container-Images oder Registry-Namensbereichen. Um einen angemessen Schutz und eine optimale Verschlüsselung zu erzielen, speichern Sie Berechtigungsnachweise für Image-Registrys in <a href="cs_images.html#other">geheimen Kubernetes-Schlüssel für Image-Pulloperationen (imagePullSecret)</a> und andere persönliche Daten in <a href="cs_app.html#secrets">geheimen Kubernetes-Schlüsseln</a>. Wenn persönliche Daten in einem vorherigen Layer eines Image gespeichert werden, kann das Löschen eines Image möglicherweise nicht ausreichen, wenn diese persönlichen Daten gelöscht werden sollen.</dd>
  </dl>

<br />






