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



# Zuständigkeiten bei der Verwendung von {{site.data.keyword.containerlong_notm}}
Erfahren Sie mehr über die Zuständigkeiten und Nutzungsbedingungen beim Cluster-Management, wenn Sie {{site.data.keyword.containerlong}} verwenden.
{:shortdesc}

## Zuständigkeiten beim Cluster-Management
{: #responsibilities}

Informieren Sie sich über die Zuständigkeiten, die Sie gemeinsam mit IBM bei der Verwaltung von Clustern haben.
{:shortdesc}

**IBM ist für Folgendes verantwortlich:**

- Bereitstellen des Masters, der Workerknoten und Managementkomponenten im Cluster (z. B. Ingress-Lastausgleichsfunktion für Anwendungen) bei der Clustererstellung
- Verwalten der Sicherheitsupdates, Überwachen, Isolieren und Wiederherstellen des Kubernetes-Masters für den Cluster
- Überwachen des Zustands der Workerknoten und Bereitstellen der Automatisierung für Aktualisierung und Wiederherstellung dieser Workerknoten
- Ausführen von Automatisierungstasks für das Infrastrukturkonto einschließlich Hinzufügen von Workerknoten, Entfernen von Workerknoten und Erstellen eines Standardteilnetzes
- Verwalten, Aktualisieren und Wiederherstellen der aktiven Komponenten im Cluster (z. B. Ingress-Lastausgleichsfunktion für Anwendungen und Speicher-Plug-in)
- Bereitstellen der Speicherdatenträger auf Anforderung von PVCs (Persistant Volume Claims)
- Bereitstellen der Sicherheitseinstellungen auf allen Workerknoten

</br>

**Sie sind für Folgendes verantwortlich:**

- [Konfigurieren des {{site.data.keyword.Bluemix_notm}}-Kontos für den Zugriff auf das Portfolio von IBM Cloud Infrastructure (SoftLayer)](cs_troubleshoot_clusters.html#cs_credentials)
- [Bereitstellen und Verwalten der Kubernetes-Ressourcen (z. B. Pods, Services und Bereitstellungen) im Cluster](cs_app.html#app_cli)
- [Nutzen der Funktionalität des Service und von Kubernetes zur Sicherstellung einer hohen Verfügbarkeit der Apps](cs_app.html#highly_available_apps)
- [Hinzufügen oder Entfernen von Clusterkapazitäten über die CLI durch Hinzufügen und Entfernen von Workerknoten](cs_cli_reference.html#cs_worker_add)
- [Erstellen öffentlicher und privater VLANs in IBM Cloud Infrastructure (SoftLayer) zur Netzisolation Ihres Clusters](/docs/infrastructure/vlans/getting-started.html#getting-started-with-vlans)
- [Sicherstellen der Netzkonnektivität zur Kubernetes-Master-URL für alle Workerknoten](cs_firewall.html#firewall) <p>**Hinweis**: Wenn ein Workerknoten sowohl über öffentliche als auch private VLANs verfügt, dann ist die Netzkonnektivität konfiguriert. Wenn Workerknoten nur mit einem privaten VLAN eingerichtet werden, müssen Sie eine alternative Lösung für die Netzkonnektivität konfigurieren. Weitere Informationen finden Sie unter [VLAN-Verbindung für Workerknoten](cs_clusters.html#worker_vlan_connection). </p>
- [Aktualisieren des Master-'kube-apiserver' wenn Kubernetes-Versionsaktualisierungen verfügbar sind](cs_cluster_update.html#master)
- [Aktualisieren der Workerknoten auf Haupt-, Neben- und Patchversionen](cs_cluster_update.html#worker_node)
- [Wiederherstellen von fehlerhaften Workerknoten durch Ausführen der geeigneten `kubectl`-Befehle (z. B. `cordon` oder `drain`) und durch Ausführen der geeigneten `bx cs`-Befehle (z. B. `reboot`, `reload` oder `delete`](cs_cli_reference.html#cs_worker_reboot))
- [Hinzufügen oder Entfernen von Teilnetzen in der IBM Cloud-Infrastruktur (SoftLayer) nach Bedarf](cs_subnets.html#subnets)
- [Sichern und Wiederherstellen von Daten im persistenten Speicher in IBM Cloud Infrastructure (SoftLayer) ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](../services/RegistryImages/ibm-backup-restore/index.html)
- [Zustandsüberwachung für Workerknoten mit automatischer Wiederherstellung konfigurieren](cs_health.html#autorecovery)

<br />


## Unsachgemäßer Gebrauch von {{site.data.keyword.containerlong_notm}}
{: #terms}

Der unsachgemäße Gebrauch von {{site.data.keyword.containershort_notm}} ist untersagt.
{:shortdesc}

Ein unsachgemäßer Gebrauch umfasst:

*   Illegale Aktivitäten
*   Verteilung und Ausführung von Malware
*   {{site.data.keyword.containershort_notm}} beschädigen oder Verwendung von {{site.data.keyword.containershort_notm}} durch einen anderen Benutzer stören
*   Schäden verursachen oder Verwendung durch einen anderen Benutzer stören
*   Unbefugter Zugriff auf Services oder Systeme
*   Unberechtigte Änderung von Services oder Systemen
*   Verletzung von Rechten anderer Benutzer


Alle Nutzungsbedingungen finden Sie unter [Bedingungen für Cloud-Services](https://console.bluemix.net/docs/overview/terms-of-use/notices.html#terms).
