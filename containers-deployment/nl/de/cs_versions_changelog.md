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



# Änderungsprotokoll der Version
{: #changelog}

Zeigen Sie Informationen zu Versionsänderungen für Hauptversions-, Nebenversions- und Patchaktualisierungen an, die für Ihre {{site.data.keyword.containerlong}}-Kubernetes-Cluster verfügbar sind. Diese Änderungen umfassen Updates für Kubernetes und {{site.data.keyword.Bluemix_notm}} Provider-Komponenten. 
{:shortdesc}

IBM wendet Updates auf Patch-Level automatisch auf Ihren Master an, aber Sie müssen [Ihre Workerknoten aktualisieren](cs_cluster_update.html#worker_node). Prüfen Sie einmal im Monat auf verfügbare Updates. Sobald Updates verfügbar sind, werden Sie benachrichtigt, wenn Sie Informationen zu den Workerknoten anzeigen, z. B. mit den Befehlen `bx cs workers <cluster>` oder `bx cs worker-get <cluster> <worker>`.

Eine Zusammenfassung der Migrationsaktionen finden Sie unter [Kubernetes-Versionen](cs_versions.html).
{: tip}

Weitere Informationen zu Änderungen seit der vorherigen Version finden Sie in den Änderungsprotokollen.
-  Version 1.10, [Änderungsprotokoll](#110_changelog).
-  Version 1.9, [Änderungsprotokoll](#19_changelog).
-  Version 1.8, [Änderungsprotokoll](#18_changelog).
-  **Veraltet**: Version 1.7, [Änderungsprotokoll](#17_changelog).

## Version 1.10, Änderungsprotokoll
{: #110_changelog}

Überprüfen Sie die folgenden Änderungen.

### Änderungsprotokoll für Workerknoten Fixpack 1.10.1_1510, veröffentlicht am 18. Mai 2018
{: #1101_1510}

<table summary="Seit Version 1.10.1_1509 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.1_1509</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Fix für einen Programmfehler, der bei der Verwendung des Blockspeicher-Plug-ins auftrat.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten Fixpack 1.10.1_1509, veröffentlicht am 16. Mai 2018
{: #1101_1509}

<table summary="Seit Version 1.10.1_1508 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.10.1_1508</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die im Stammverzeichnis `kubelet` gespeicherten Daten werden nun auf einer größeren sekundären Platte der Maschine mit dem Workerknoten gespeichert.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.10.1_1508, veröffentlicht am 1. Mai 2018
{: #1101_1508}

<table summary="Seit Version 1.9.7_1510 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.7_1510</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>Version 2.6.5</td>
<td>Version 3.1.1</td>
<td>Werfen Sie einen Blick auf die Calico-[Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.projectcalico.org/v3.1/releases/#v311).</td>
</tr>
<tr>
<td>Kubernetes Heapster</td>
<td>Version 1.5.0</td>
<td>Version 1.5.2</td>
<td>Werfen Sie einen Blick auf die Kubernetes Heapster-[Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/heapster/releases/tag/v1.5.2).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>Version 1.9.7</td>
<td>Version 1.10.1</td>
<td>Werfen Sie einen Blick auf die Kubernetes-[Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.1).</td>
</tr>
<tr>
<td>Kubernetes-Konfiguration</td>
<td>n.z.</td>
<td>n.z.</td>
<td><code>StorageObjectInUseProtection</code> wurde zur Option <code>--enable-admission-plugins</code> für den Kubernetes-API-Server des Clusters hinzugefügt.</td>
</tr>
<tr>
<td>Kubernetes-DNS</td>
<td>1.14.8</td>
<td>1.14.10</td>
<td>Werfen Sie einen Blick auf die Kubernetes-DNS-[Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/dns/releases/tag/1.14.10).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>Version 1.9.7-102</td>
<td>Version 1.10.1-52</td>
<td>Wurde für die Unterstützung von Kubernetes 1.10 aktualisiert.</td>
</tr>
<tr>
<td>GPU-Unterstützung</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Unterstützung für [Container-Workloads der Graphics Processing Unit (GPU)](cs_app.html#gpu_app) ist jetzt für die Terminierung und Ausführung verfügbar. Eine Liste der verfügbaren GPU-Maschinentypen finden Sie unter [Hardware für Workerknoten](cs_clusters.html#shared_dedicated_node). Weitere Informationen finden Sie in der Kubernetes-Dokumentation zum [Terminieren von GPUs ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/).</td>
</tr>
</tbody>
</table>

## Version 1.9, Änderungsprotokoll
{: #19_changelog}

Überprüfen Sie die folgenden Änderungen.

### Änderungsprotokoll für Workerknoten Fixpack 1.9.7_1512, veröffentlicht am 18. Mai 2018
{: #197_1512}

<table summary="Seit Version 1.9.7_1511 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.7_1511</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Fix für einen Programmfehler, der bei der Verwendung des Blockspeicher-Plug-ins auftrat.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten Fixpack 1.9.7_1511, veröffentlicht am 16. Mai 2018
{: #197_1511}

<table summary="Seit Version 1.9.7_1510 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.7_1510</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die im Stammverzeichnis `kubelet` gespeicherten Daten werden nun auf einer größeren sekundären Platte der Maschine mit dem Workerknoten gespeichert.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für 1.9.7_1510, veröffentlicht am 30. April 2018
{: #197_1510}

<table summary="Seit Version 1.9.3_1506 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.9.3_1506</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>Version 1.9.3</td>
<td>Version 1.9.7	</td>
<td>Werfen Sie einen Blick auf die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.7). Dieses Release befasst sich mit den Schwachstellen [CVE-2017-1002101 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) und [CVE-2017-1002102 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102).</td>
</tr>
<tr>
<td>Kubernetes-Konfiguration</td>
<td>n.z.</td>
<td>n.z.</td>
<td>`admissionregistration.k8s.io/v1alpha1=true` wurde zur Option `--runtime-config` für den Kubernetes-API-Server des Clusters hinzugefügt.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>Version 1.9.3-71</td>
<td>Version 1.9.7-102</td>
<td>`NodePort`- und `LoadBalancer`-Services unterstützen jetzt [das Beibehalten der Client-Quellen-IP](cs_loadbalancer.html#node_affinity_tolerations) durch Festlegen von `service.spec.externalTrafficPolicy` auf `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Korrigiert die [Edge-Knoten](cs_edge.html#edge)-Tolerierungskonfiguration für ältere Cluster.</td>
</tr>
</tbody>
</table>

## Version 1.8, Änderungsprotokoll
{: #18_changelog}

Überprüfen Sie die folgenden Änderungen.

### Änderungsprotokoll für Workerknoten Fixpack 1.8.11_1511, veröffentlicht am 18. Mai 2018
{: #1811_1511}

<table summary="Seit Version 1.8.11_1510 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.8.11_1510</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Fix für einen Programmfehler, der bei der Verwendung des Blockspeicher-Plug-ins auftrat.</td>
</tr>
</tbody>
</table>

### Änderungsprotokoll für Workerknoten Fixpack 1.8.11_1510, veröffentlicht am 16. Mai 2018
{: #1811_1510}

<table summary="Seit Version 1.8.11_1509 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.8.11_1509</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die im Stammverzeichnis `kubelet` gespeicherten Daten werden nun auf einer größeren sekundären Platte der Maschine mit dem Workerknoten gespeichert.</td>
</tr>
</tbody>
</table>


### Änderungsprotokoll für 1.8.11_1509, veröffentlicht am 19. April 2018
{: #1811_1509}

<table summary="Seit Version 1.8.8_1507 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.8.8_1507</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.8.8</td>
<td>v1.8.11	</td>
<td>Werfen Sie einen Blick auf die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.11). Dieses Release befasst sich mit den Schwachstellen [CVE-2017-1002101 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) und [CVE-2017-1002102 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102).</td>
</tr>
<tr>
<td>Containerimage anhalten</td>
<td>3.0</td>
<td>3.1</td>
<td>Entfernt geerbte verwaiste Zombie-Prozesse.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.8.8-86</td>
<td>v1.8.11-126</td>
<td>`NodePort`- und `LoadBalancer`-Services unterstützen jetzt [das Beibehalten der Client-Quellen-IP](cs_loadbalancer.html#node_affinity_tolerations) durch Festlegen von `service.spec.externalTrafficPolicy` auf `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Korrigiert die [Edge-Knoten](cs_edge.html#edge)-Tolerierungskonfiguration für ältere Cluster.</td>
</tr>
</tbody>
</table>

## Archiv
{: #changelog_archive}

### Version 1.7, Änderungsprotokoll (veraltet)
{: #17_changelog}

Überprüfen Sie die folgenden Änderungen.

#### Änderungsprotokoll für Workerknoten Fixpack 1.7.16_1513, veröffentlicht am 18. Mai 2018
{: #1716_1513}

<table summary="Seit Version 1.7.16_1512 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.7.16_1512</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Fix für einen Programmfehler, der bei der Verwendung des Blockspeicher-Plug-ins auftrat.</td>
</tr>
</tbody>
</table>

#### Änderungsprotokoll für Workerknoten Fixpack 1.7.16_1512, veröffentlicht am 16. Mai 2018
{: #1716_1512}

<table summary="Seit Version 1.7.16_1511 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.7.16_1511</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>n.z.</td>
<td>n.z.</td>
<td>Die im Stammverzeichnis `kubelet` gespeicherten Daten werden nun auf einer größeren sekundären Platte der Maschine mit dem Workerknoten gespeichert.</td>
</tr>
</tbody>
</table>

#### Änderungsprotokoll für 1.7.16_1511, veröffentlicht am 19. April 2018
{: #1716_1511}

<table summary="Seit Version 1.7.4_1509 vorgenommene Änderungen">
<caption>Änderungen seit Version 1.7.4_1509</caption>
<thead>
<tr>
<th>Komponente</th>
<th>Vorher</th>
<th>Aktuell</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.7.4</td>
<td>v1.7.16	</td>
<td>Werfen Sie einen Blick auf die [Kubernetes-Releaseinformationen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/kubernetes/kubernetes/releases/tag/v1.7.16). Dieses Release befasst sich mit den Schwachstellen [CVE-2017-1002101 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) und [CVE-2017-1002102 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102).</td>
</tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.7.4-133</td>
<td>v1.7.16-17</td>
<td>`NodePort`- und `LoadBalancer`-Services unterstützen jetzt [das Beibehalten der Client-Quellen-IP](cs_loadbalancer.html#node_affinity_tolerations) durch Festlegen von `service.spec.externalTrafficPolicy` auf `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Korrigiert die [Edge-Knoten](cs_edge.html#edge)-Tolerierungskonfiguration für ältere Cluster.</td>
</tr>
</tbody>
</table>
