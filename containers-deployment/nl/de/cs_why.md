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



# Warum {{site.data.keyword.containerlong_notm}}?
{: #cs_ov}

{{site.data.keyword.containerlong}} bietet durch die Kombination von Docker- und Kubernetes-Technologien leistungsstarke Tools, ein intuitives Benutzererlebnis sowie integrierte Sicherheit und Isolation, um die Bereitstellung, den Betrieb, die Skalierung und die Überwachung von containerisierten Apps über einen Cluster von Rechenhosts zu automatisieren.
{:shortdesc}


## Vorteile durch die Verwendung des Service
{: #benefits}

Cluster werden auf Rechenhosts bereitgestellt, die native Kubernetes-Funktionalität und durch {{site.data.keyword.IBM_notm}} hinzugefügte Funktionen zur Verfügung stellen.
{:shortdesc}

|Vorteil|Beschreibung|
|-------|-----------|
|Single-Tenant-Kubernetes-Cluster mit Isolation der Rechen-, Netz- und Speicherinfrastruktur|<ul><li>Erstellen Sie Ihre eigene angepasste Infrastruktur, die den Anforderungen Ihrer Organisation entspricht.</li><li>Ermöglicht die Einrichtung eines dedizierten und geschützten Kubernetes-Masters sowie von Workerknoten, virtuellen Netzen und Speicher unter Nutzung der von IBM Cloud Infrastructure (SoftLayer) bereitgestellten Ressourcen.</li><li>Der komplett verwaltete Kubernetes-Master, der kontinuierlich von {{site.data.keyword.IBM_notm}} überwacht und aktualisiert wird, um Ihren Cluster verfügbar zu halten.</li><li>Option zum Bereitstellen von Workerknoten als Bare-Metal-Server mit Trusted Compute.</li><li>Ermöglicht das Speichern persistenter Daten, die gemeinsame Nutzung von Daten durch Kubernetes-Pods und bei Bedarf die Wiederherstellung von Daten mit dem integrierten und sicheren Datenträgerservice.</li><li>Bietet volle Unterstützung für alle nativen Kubernetes-APIs.</li></ul>|
|Einhaltung von Sicherheitsbestimmungen für Images mit Vulnerability Advisor|<ul><li>Ermöglicht die Einrichtung einer eigenen geschützten privaten Docker-Image-Registry, in der Images gespeichert und von allen Benutzern der Organisation gemeinsam genutzt werden können.</li><li>Bietet den Vorteil des automatischen Scannens von Images in Ihrer privaten {{site.data.keyword.Bluemix_notm}}-Registry.</li><li>Ermöglicht die Überprüfung von Empfehlungen für das im Image verwendete Betriebssystem, um potenzielle Schwachstellen zu beheben.</li></ul>|
|Kontinuierliche Überwachung des Clusterzustands|<ul><li>Über das Cluster-Dashboard können Sie den Zustand Ihrer Cluster, Workerknoten und Containerbereitstellungen rasch anzeigen und verwalten.</li><li>Ermöglicht die Feststellung detaillierter Metriken zur Auslastung mit {{site.data.keyword.monitoringlong}} und die rasche Erweiterung des Clusters als Reaktion auf die Arbeitslast.</li><li>Stellt detaillierte Protokollinformationen zu Clusteraktivitäten über den {{site.data.keyword.loganalysislong}} bereit.</li></ul>|
|Sichere Offenlegung von Apps gegenüber der Allgemeinheit|<ul><li>Sie haben die Auswahl zwischen einer öffentlichen IP-Adresse, einer von {{site.data.keyword.IBM_notm}} bereitgestellten Route oder Ihrer eigenen angepassten Domäne, um aus dem Internet auf Services in Ihrem Cluster zuzugreifen.</li></ul>|
|Integration des {{site.data.keyword.Bluemix_notm}}-Service|<ul><li>Ermöglicht das Hinzufügen zusätzlicher Funktionalität zu Ihrer App durch die Integration von {{site.data.keyword.Bluemix_notm}}-Services wie zum Beispiel Watson-APIs, Blockchain, Datenservices oder Internet of Things.</li></ul>|
{: caption="Vorteile von {{site.data.keyword.containerlong_notm}}" caption-side="top"}



<br />


## Vergleich von Angeboten und ihren Kombinationen
{: #differentiation}

Sie können {{site.data.keyword.containershort_notm}} in {{site.data.keyword.Bluemix_notm}} Public oder Dedicated, in {{site.data.keyword.Bluemix_notm}} Private oder in einem Hybridsetup ausführen.
{:shortdesc}


<table>
<caption>Unterschiede zwischen den {{site.data.keyword.containershort_notm}}-Konfigurationen</caption>
<col width="22%">
<col width="78%">
 <thead>
 <th>{{site.data.keyword.containershort_notm}}-Setup</th>
 <th>Beschreibung</th>
 </thead>
 <tbody>
 <tr>
 <td>{{site.data.keyword.Bluemix_notm}} Public
 </td>
 <td>Mit {{site.data.keyword.Bluemix_notm}} Public auf [gemeinsam genutzter oder dedizierter Hardware oder auf Bare-Metal-Maschinen](cs_clusters.html#shared_dedicated_node) können Sie Ihre Apps mithilfe von {{site.data.keyword.containershort_notm}} in Clustern in der Cloud hosten. {{site.data.keyword.containershort_notm}} auf {{site.data.keyword.Bluemix_notm}} Public bietet durch die Kombination von Docker- und Kubernetes-Technologien leistungsstarke Tools, ein intuitives Benutzererlebnis sowie integrierte Sicherheit und Isolation, um die Bereitstellung, den Betrieb, die Skalierung und die Überwachung von containerisierten Apps über einen Cluster von Rechenhosts zu automatisieren.<br><br>Weitere Informationen finden Sie unter [{{site.data.keyword.containershort_notm}}-Technologie](cs_tech.html).
 </td>
 </tr>
 <tr>
 <td>{{site.data.keyword.Bluemix_notm}} Dedicated
 </td>
 <td>{{site.data.keyword.Bluemix_notm}} Dedicated bietet dieselben {{site.data.keyword.containershort_notm}}-Funktionalitäten in der Cloud wie {{site.data.keyword.Bluemix_notm}} Public. Bei einem {{site.data.keyword.Bluemix_notm}} Dedicated-Konto sind die verfügbaren [physischen Ressourcen jedoch nur Ihrem Cluster zugeordnet](cs_clusters.html#shared_dedicated_node) und werden nicht mit Clustern von anderen {{site.data.keyword.IBM_notm}} Kunden geteilt. Sie können eine {{site.data.keyword.Bluemix_notm}} Dedicated-Umgebung einrichten, wenn Sie eine Isolation für Ihren Cluster und die anderen, von Ihnen genutzten {{site.data.keyword.Bluemix_notm}}-Services benötigen.<br><br>Weitere Informationen finden Sie unter [Einführung in Cluster in {{site.data.keyword.Bluemix_notm}} Dedicated](cs_dedicated.html#dedicated).
 </td>
 </tr>
 <tr>
 <td>{{site.data.keyword.Bluemix_notm}} Private
 </td>
 <td>{{site.data.keyword.Bluemix_notm}} Private ist eine Anwendungsplattform, die lokal auf Ihren Maschinen installiert werden kann. Sie können {{site.data.keyword.containershort_notm}} in {{site.data.keyword.Bluemix_notm}} Private verwenden, wenn Sie lokale, containerisierte Apps in Ihrer eigenen kontrollierten Umgebung hinter einer Firewall entwickeln und verwalten müssen. <br><br>Weitere Informationen finden Sie in den [Produktinformationen zu {{site.data.keyword.Bluemix_notm}} Private ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/cloud/private) und der zugehörigen [Dokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_1.2.0/kc_welcome_containers.html).
 </td>
 </tr>
 <tr>
 <td>Hybridsetup
 </td>
 <td>Hybrid ist die kombinierte Nutzung von Services, die in {{site.data.keyword.Bluemix_notm}} Public oder Dedicated ausgeführt werden, sowie anderen Services, die lokal ausgeführt werden, z. B. als App {{site.data.keyword.Bluemix_notm}} Private. Beispiele für ein Hybridsetup: <ul><li>Bereitstellung eines Clusters mit {{site.data.keyword.containershort_notm}} in {{site.data.keyword.Bluemix_notm}} Public, aber Verbindung dieses Clusters mit einer lokalen Datenbank.</li><li>Bereitstellung eines Clusters mit {{site.data.keyword.containershort_notm}} in {{site.data.keyword.Bluemix_notm}} Private und Implementierung einer App in diesem Cluster. Diese App kann jedoch einen {{site.data.keyword.ibmwatson}}-Service wie {{site.data.keyword.toneanalyzershort}} in {{site.data.keyword.Bluemix_notm}} Public nutzen.</li></ul><br>Um die Kommunikation zwischen Services zu aktivieren, die in {{site.data.keyword.Bluemix_notm}} Public oder Dedicated ausgeführt werden, und Services, die lokal ausgeführt werden, müssen Sie eine [VPN-Verbindung einrichten](cs_vpn.html).
 </td>
 </tr>
 </tbody>
</table>

<br />


## Vergleich von kostenlosen Clustern und Standardclustern
{: #cluster_types}

Sie können einen kostenlosen Cluster oder beliebig viele Standardcluster erstellen. Testen Sie kostenlose Cluster, um sich mit einigen Kubernetes-Leistungsmerkmalen vertraut zu machen. Oder erstellen Sie Standardcluster, um das vollständige Leistungsspektrum von Kubernetes zum Bereitstellen von Apps zu nutzen.
{:shortdesc}

|Merkmale|Kostenlose Cluster|Standardcluster|
|---------------|-------------|-----------------|
|[Netzbetrieb in Clustern](cs_secure.html#in_cluster_network)
|[Persistenter NFS-dateibasierter Speicher mit Datenträgern](cs_storage.html#planning)| |<img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" />|
|[Öffentlicher oder privater App-Zugriff auf eine stabile IP-Adresse durch einen Lastausgleichsservice](cs_loadbalancer.html#planning)| |<img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" />|
|[Öffentlicher App-Zugriff auf eine stabile IP-Adresse und eine anpassbare URL durch einen Ingress-Service](cs_ingress.html#planning)| |<img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" />|
|[Portierbare öffentliche IP-Adressen](cs_subnets.html#manage)| |<img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" />|
|[Protokollierung und Überwachung](cs_health.html#logging)| |<img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" />|
|[Option zum Bereitstellen der Workerknoten auf physischen Servern (Bare-Metal-Servern)](cs_clusters.html#shared_dedicated_node) | |<img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" />|
|[Option zum Bereitstellen von Bare-Metal-Workern mit Trusted Compute](cs_clusters.html#shared_dedicated_node) | |<img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" />|
|[Verfügbar in {{site.data.keyword.Bluemix_dedicated_notm}}](cs_dedicated.html#dedicated_environment)| |<img src="images/confirm.svg" width="32" alt="Funktion verfügbar" style="width:32px;" />|
{: caption="Merkmale von kostenlosen Clustern und Standardclustern" caption-side="top"}

<br />




{: #responsibilities}
**Hinweis**: Suchen Sie nach Ihren Zuständigkeiten und den Nutzungsbedingungen von {{site.data.keyword.containerlong}} bei der Verwendung des Service? 

{: #terms}
Weitere Informationen finden Sie unter [{{site.data.keyword.containershort_notm}}-Zuständigkeiten](cs_responsibilities.html).
