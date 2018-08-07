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




# Hochverfügbarkeit für {{site.data.keyword.containerlong_notm}}
{: #ha}

Verwenden Sie die integrierten Kubernetes- und {{site.data.keyword.containerlong}}-Funktionen, um eine höhere Verfügbarkeit für Ihr Cluster zu erreichen und um Ihre App vor Ausfallzeiten zu schützen, wenn eine Komponenten in Ihrem Cluster ausfällt.
{: shortdesc}

Hochverfügbarkeit ist eine zentrale Disziplin in einer IT-Infrastruktur, um Ihre Apps auch nach einem partiellen oder vollständigen Ausfall der Site betriebsbereit zu halten. Der Hauptzweck der Hochverfügbarkeit ist es, potenzielle Fehlerquellen in einer IT-Infrastruktur zu eliminieren. Sie können sich zum Beispiel auf den Ausfall eines Systems vorbereiten, indem Sie Redundanz hinzufügen und Funktionsübernahmemechanismen einrichten.

Sie können eine hohe Verfügbarkeit auf verschiedenen Ebenen in Ihrer IT-Infrastruktur und innerhalb der verschiedenen Komponenten Ihres Clusters erreichen. Die für Sie geeignete Ebene der Verfügbarkeit hängt von mehreren Faktoren ab, z. B. von Ihren Geschäftsanforderungen, den Service-Level-Agreements, die Sie mit Ihren Kunden haben, und dem Geld, das Sie ausgeben möchten.

## Übersicht über mögliche Fehlerquellen in {{site.data.keyword.containerlong_notm}}
{: #fault_domains} 

Die {{site.data.keyword.containerlong_notm}}-Architektur und -Infrastruktur wurde konzipiert, um eine hohe Zuverlässigkeit, eine geringe Latenzzeit bei der Verarbeitung und eine maximale Betriebszeit des Service zu gewährleisten. Fehler können jedoch trozdem auftreten. Abhängig von dem Service, den Sie in {{site.data.keyword.Bluemix_notm}} hosten, können Sie möglicherweise keine Fehler tolerieren, selbst wenn die Fehler nur einige Minuten dauern.
{: shortdesc}

{{site.data.keyword.containershort_notm}} stellt mehrere Methoden bereit, um Ihrem Cluster mehr Verfügbarkeit hinzuzufügen, indem Redundanz und Anti-Affinität hinzugefügt werden. Sehen Sie sich die folgende Abbildung an, um mehr über mögliche Fehlerquellen zu lernen und wie Sie diese beseitigen können.

<img src="images/cs_failure_ov.png" alt="Übersicht über Fehlerbereiche in einem Hochverfügbarkeitscluster innerhalb einer {{site.data.keyword.containershort_notm}}-Region." width="250" style="width:250px; border-style: none"/>


<table summary="Die Tabelle zeigt Fehlerquellen in {{site.data.keyword.containershort_notm}}. Die Zeilen sind von links nach rechts zu lesen. In Spalte 1 finden Sie die Fehlerquelle, den Titel der Fehlerquelle in Spalte 2, eine Beschreibung in Spalte 3 und einen Link zur Dokumentation in Spalte 4. ">
<caption>Fehlerquellen</caption>
<col width="3%">
<col width="10%">
<col width="70%">
<col width="17%">
  <thead>
  <th>#</th>
  <th>Fehlerquelle</th>
  <th>Beschreibung</th>
  <th>Link zur Dokumentation</th>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>Container- oder Podfehler</td>
      <td>Container und Pods sind per Definition Komponenten mit kurzer Lebensdauer, die kurzfristig und unerwartet ausfallen können. Ein Container oder Pod kann ausfallen, wenn ein Fehler in Ihrer App auftritt. Um Ihre App hoch verfügbar zu machen, müssen Sie sicherstellen, dass Sie über genügend Instanzen Ihrer App verfügen, um die Arbeitslast zu bewältigen, sowie über zusätzliche Instanzen, die im Falle eines Fehlers zur Verfügung stehen. Im Idealfall werden diese Instanzen auf mehrere Workerknoten verteilt, um Ihre App vor einem Ausfall eines Workerknotens zu schützen.</td>
      <td>[Hoch verfügbare Apps bereitstellen](cs_app.html#highly_available_apps)</td>
  </tr>
  <tr>
    <td>2</td>
    <td>Workerknotenfehler</td>
    <td>Ein Workerknoten ist eine VM, die auf einer physischen Hardware aufsetzt. Workerknotenfehler umfassen Hardwareausfälle, wie z. B. Ausfälle bei Stromversorgung, Kühlung oder Netzbetrieb, und Probleme auf der VM selbst. Sie können einem Workerknotenfehler vorbeugen, indem Sie mehrere Workerknoten in Ihrem Cluster einrichten. <br/><br/><strong>Hinweis:</strong> Workerknoten an einem Standort befinden sich nicht zwingend auf separaten physischen Rechenhosts. Beispiel: Sie verfügen über einen Cluster mit 3 Workerknoten, aber alle 3 Workerknoten wurden auf demselben physischen Rechenhost an einem IBM Standort erstellt. Wenn dieser physische Rechenhost herunterfährt, sind alle Worker-Knoten inaktiv. Um sich vor diesem Fehler zu schützen, müssen Sie einen zweiten Cluster an einem anderen Standort konfigurieren.</td>
    <td>[Cluster mit mehreren Workerknoten erstellen](cs_cli_reference.html#cs_cluster_create)</td>
  </tr>
  <tr>
    <td>3</td>
    <td>Clusterfehler</td>
    <td>Der Kubernetes-Master ist die Hauptkomponente, die den Cluster betriebsbereit hält. Der Master speichert alle Clusterdaten in der etcd-Datenbank, die als Single Point of Truth für Ihren Cluster dient. Ein Clusterfehler tritt auf, wenn der Master aufgrund eines Netzfehlers oder wegen beschädigter Daten in Ihrer etcd-Datenbank nicht erreicht werden kann. Sie können mehrere Cluster an einem Standort erstellen, um Ihre Apps vor einem Kubernetes-Master-Fehler oder etcd-Fehler zu schützen. Zum Lastausgleich zwischen den Clustern müssen Sie eine externe Lastausgleichsfunktion einrichten. <br/><br/><strong>Hinweis:</strong> Wenn Sie mehrere Cluster an einem Standort konfigurieren, gewährleistet das nicht, dass Ihre Workerknoten auf separaten physischen Rechenhosts bereitgestellt werden. Um sich vor diesem Fehler zu schützen, müssen Sie einen zweiten Cluster an einem anderen Standort konfigurieren.</td>
    <td>[Hoch verfügbare Cluster konfigurieren](cs_clusters.html#planning_clusters)</td>
  </tr>
  <tr>
    <td>4</td>
    <td>Standortfehler</td>
    <td>Ein Standortfehler betrifft alle physischen Rechenhosts und den NFS-Speicher. Fehler umfassen Ausfälle bei Stromversorgung, Kühlung, Netzbetrieb oder Speicherausfälle sowie Naturkatastrophen wie Überschwemmungen, Erdbeben und Stürme. Um sich vor einem Standortfehler zu schützen, müssen Sie über Cluster an zwei unterschiedlichen Standorten verfügen, wobei eine externe Lastausgleichsfunktion den Lastausgleich zwischen den Standorten vornimmt.</td>
    <td>[Hoch verfügbare Cluster konfigurieren](cs_clusters.html#planning_clusters)</td>
  </tr>
  <tr>
    <td>5</td>
    <td>Regionsfehler</td>
    <td>Jede Region wird mit einer hoch verfügbaren Lastausgleichsfunktion konfiguriert, auf die von einem regionsspezifischen API-Endpunkt aus zugegriffen werden kann. Die Lastausgleichsfunktion leitet eingehende und ausgehende Anforderungen an Cluster zu den regionalen Standorten weiter. Die Wahrscheinlichkeit eines vollständigen regionalen Ausfalls ist gering. Um diesem Fehler jedoch vorzubeugen, können Sie mehrere Cluster in verschiedenen Regionen einrichten und sie mithilfe einer externen Lastausgleichsfunktion verbinden. Wenn eine ganze Region ausfällt, kann der Cluster in der anderen Region die Arbeitslast übernehmen. <br/><br/><strong>Hinweis:</strong> Ein Cluster in mehreren Regionen erfordert mehrere Cloudressourcen und kann abhängig von Ihrer App sehr komplex und kostenintensiv sein. Prüfen Sie, ob Sie eine Konfiguration über mehrere Regionen benötigen, oder ob Sie mit einer möglichen Serviceunterbrechung umgehen können. Wenn Sie einen Cluster in mehreren Regionen konfigurieren möchten, stellen Sie sicher, dass Ihre App und die Daten in einer anderen Region gehostet werden können, und dass Ihre App die Replikation von globalen Daten handhaben kann.</td>
    <td>[Hoch verfügbare Cluster konfigurieren](cs_clusters.html#planning_clusters)</td>
  </tr>
  <tr>
    <td>6a, 6b</td>
    <td>Speicherfehler</td>
    <td>In einer statusabhängigen App spielen Daten eine wichtige Rolle, um Ihre App betriebsbereit zu halten. Sie möchten sicherstellen, dass Ihre Daten hoch verfügbar sind, so dass Sie eine Wiederherstellung nach einem potenziellen Fehler vornehmen können. In {{site.data.keyword.containershort_notm}} können Sie aus mehreren Optionen auswählen, um Ihre Daten als persistent zu definieren. Sie können zum Beispiel NFS-Speicher bereitstellen, indem Sie native, persistente Kubernetes-Datenträger verwenden oder Ihre Daten mithilfe eines {{site.data.keyword.Bluemix_notm}}-Datenbankservice speichern.</td>
    <td>[Hoch verfügbare Daten planen](cs_storage.html#planning)</td>
  </tr>
  </tbody>
  </table>



