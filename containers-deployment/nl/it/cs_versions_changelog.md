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



# Changelog versione
{: #changelog}

Visualizza le informazioni sulle modifiche alla versione per gli aggiornamenti principali, secondari e patch disponibili per i tuoi cluster Kubernetes {{site.data.keyword.containerlong}}. Le modifiche includono gli aggiornamenti ai componenti Kubernetes e {{site.data.keyword.Bluemix_notm}} Provider. 
{:shortdesc}

IBM applica automaticamente gli aggiornamenti a livello di patch al tuo master, ma devi [aggiornare i tuoi nodi di lavoro](cs_cluster_update.html#worker_node). Controlla mensilmente gli aggiornamenti. Quando gli aggiornamenti diventano disponibili, riceverai una notifica quando visualizzerai le informazioni relative ai nodi di lavoro, ad esempio con i comandi `bx cs workers <cluster>` o `bx cs worker-get <cluster> <worker>`.

Per un riepilogo delle azioni di migrazione, vedi [Versioni Kubernetes](cs_versions.html).
{: tip}

Per informazioni sulle modifiche dalla versione precedente, vedi i seguenti changelog.
-  [Changelog](#110_changelog) Versione 1.10.
-  [Changelog](#19_changelog) Versione 1.9.
-  [Changelog](#18_changelog) Versione 1.8.
-  **Obsoleto**: [Changelog](#17_changelog) Versione 1.7.

## Changelog Versione 1.10
{: #110_changelog}

Esamina le seguenti modifiche.

### Changelog per il fix pack del nodo di lavoro 1.10.1_1510, rilasciato il 18 maggio 2018
{: #1101_1510}

<table summary="Modifiche apportate dalla versione 1.10.1_1509">
<caption>Modifiche dalla versione 1.10.1_1509</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Correzione per risolvere un bug che si è verificato se hai utilizzato il plugin dell'archiviazione blocchi.</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del nodo di lavoro 1.10.1_1509, rilasciato il 16 maggio 2018
{: #1101_1509}

<table summary="Modifiche apportate dalla versione 1.10.1_1508">
<caption>Modifiche dalla versione 1.10.1_1508</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>I dati che archivi nella directory root `kubelet` sono ora salvati nel disco secondario più grande della tua macchina del nodo di lavoro.</td>
</tr>
</tbody>
</table>

### Changelog per 1.10.1_1508, rilasciato il 01 maggio 2018
{: #1101_1508}

<table summary="Modifiche apportate dalla versione 1.9.7_1510">
<caption>Modifiche dalla versione 1.9.7_1510</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v2.6.5</td>
<td>v3.1.1</td>
<td>Consulta le [note sulla release Calico ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.projectcalico.org/v3.1/releases/#v311).</td>
</tr>
<tr>
<td>Kubernetes Heapster</td>
<td>v1.5.0</td>
<td>v1.5.2</td>
<td>Vedi le [note sulla release Kubernetes Heapster ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/heapster/releases/tag/v1.5.2).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.9.7</td>
<td>v1.10.1</td>
<td>Vedi le [note sulla release Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.1).</td>
</tr>
<tr>
<td>Configurazione Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Aggiunto <code>StorageObjectInUseProtection</code> all'opzione <code>--enable-admission-plugins</code> del server dell'API Kubernetes del cluster.</td>
</tr>
<tr>
<td>DNS Kubernetes</td>
<td>1.14.8</td>
<td>1.14.10</td>
<td>Consulta le [note sulla release della DNS Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/dns/releases/tag/1.14.10).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.9.7-102</td>
<td>v1.10.1-52</td>
<td>Aggiornato per supportare la release Kubernetes 1.10.</td>
</tr>
<tr>
<td>Supporto GPU </td>
<td>N/A</td>
<td>N/A</td>
<td>Il supporto per i [carichi di lavoro del contenitore GPU (graphics processing unit)](cs_app.html#gpu_app) è ora disponibile per la pianificazione e l'esecuzione. Per un elenco di tipi di macchina GPU disponibili, consulta [Hardware per i nodi di lavoro](cs_clusters.html#shared_dedicated_node). Per ulteriori informazioni, consulta la documentazione Kubernetes in [Schedule GPUs ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/).</td>
</tr>
</tbody>
</table>

## Changelog Versione 1.9
{: #19_changelog}

Esamina le seguenti modifiche.

### Changelog per il fix pack del nodo di lavoro 1.9.7_1512, rilasciato il 18 maggio 2018
{: #197_1512}

<table summary="Modifiche apportate dalla versione 1.9.7_1511">
<caption>Modifiche dalla versione 1.9.7_1511</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Correzione per risolvere un bug che si è verificato se hai utilizzato il plugin dell'archiviazione blocchi.</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del nodo di lavoro 1.9.7_1511, rilasciato il 16 maggio 2018
{: #197_1511}

<table summary="Modifiche apportate dalla versione 1.9.7_1510">
<caption>Modifiche dalla versione 1.9.7_1510</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>I dati che archivi nella directory root `kubelet` sono ora salvati nel disco secondario più grande della tua macchina del nodo di lavoro.</td>
</tr>
</tbody>
</table>

### Changelog per 1.9.7_1510, rilasciato il 30 aprile 2018
{: #197_1510}

<table summary="Modifiche apportate dalla versione 1.9.3_1506">
<caption>Modifiche dalla versione 1.9.3_1506</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.9.3</td>
<td>v1.9.7	</td>
<td>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.7). Questa release risolve le vulnerabilità [CVE-2017-1002101 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) e [CVE-2017-1002102 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102).</td>
</tr>
<tr>
<td>Configurazione Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Aggiunto `admissionregistration.k8s.io/v1alpha1=true` all'opzione `--runtime-config` del server dell'API Kubernetes del cluster.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.9.3-71</td>
<td>v1.9.7-102</td>
<td>I servizi `NodePort` e `LoadBalancer` ora supportano [la conservazione dell'IP di origine client](cs_loadbalancer.html#node_affinity_tolerations) impostando `service.spec.externalTrafficPolicy` su `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Corregge la configurazione di tolleranza del [nodo edge](cs_edge.html#edge) per i cluster meno recenti.</td>
</tr>
</tbody>
</table>

## Changelog Versione 1.8
{: #18_changelog}

Esamina le seguenti modifiche.

### Changelog per il fix pack del nodo di lavoro 1.8.11_1511, rilasciato il 18 maggio 2018
{: #1811_1511}

<table summary="Modifiche apportate dalla versione 1.8.11_1510">
<caption>Modifiche dalla versione 1.8.11_1510</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Correzione per risolvere un bug che si è verificato se hai utilizzato il plugin dell'archiviazione blocchi.</td>
</tr>
</tbody>
</table>

### Changelog per il fix pack del nodo di lavoro 1.8.11_1510, rilasciato il 16 maggio 2018
{: #1811_1510}

<table summary="Modifiche apportate dalla versione 1.8.11_1509">
<caption>Modifiche dalla versione 1.8.11_1509</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>I dati che archivi nella directory root `kubelet` sono ora salvati nel disco secondario più grande della tua macchina del nodo di lavoro.</td>
</tr>
</tbody>
</table>


### Changelog per 1.8.11_1509, rilasciato il 19 aprile 2018
{: #1811_1509}

<table summary="Modifiche apportate dalla versione 1.8.8_1507">
<caption>Modifiche dalla versione 1.8.8_1507</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.8.8</td>
<td>v1.8.11	</td>
<td>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.11). Questa release risolve le vulnerabilità [CVE-2017-1002101 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) e [CVE-2017-1002102 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102).</td>
</tr>
<tr>
<td>Sospensione immagine contenitore</td>
<td>3.0</td>
<td>3.1</td>
<td>Rimuove i processi zombie orfani ereditati.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.8.8-86</td>
<td>v1.8.11-126</td>
<td>I servizi `NodePort` e `LoadBalancer` ora supportano [la conservazione dell'IP di origine client](cs_loadbalancer.html#node_affinity_tolerations) impostando `service.spec.externalTrafficPolicy` su `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Corregge la configurazione di tolleranza del [nodo edge](cs_edge.html#edge) per i cluster meno recenti.</td>
</tr>
</tbody>
</table>

## Archivio
{: #changelog_archive}

### Changelog versione 1.7 (obsoleto)
{: #17_changelog}

Esamina le seguenti modifiche.

#### Changelog per il fix pack del nodo di lavoro 1.7.16_1513, rilasciato il 18 maggio 2018
{: #1716_1513}

<table summary="Modifiche apportate dalla versione 1.7.16_1512">
<caption>Modifiche dalla versione 1.7.16_1512</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Correzione per risolvere un bug che si è verificato se hai utilizzato il plugin dell'archiviazione blocchi.</td>
</tr>
</tbody>
</table>

#### Changelog per il fix pack del nodo di lavoro 1.7.16_1512, rilasciato il 16 maggio 2018
{: #1716_1512}

<table summary="Modifiche apportate dalla versione 1.7.16_1511">
<caption>Modifiche dalla versione 1.7.16_1511</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>I dati che archivi nella directory root `kubelet` sono ora salvati nel disco secondario più grande della tua macchina del nodo di lavoro.</td>
</tr>
</tbody>
</table>

#### Changelog per 1.7.16_1511, rilasciato il 19 aprile 2018
{: #1716_1511}

<table summary="Modifiche apportate dalla versione 1.7.4_1509">
<caption>Modifiche dalla versione 1.7.4_1509</caption>
<thead>
<tr>
<th>Componente</th>
<th>Precedente</th>
<th>Corrente</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.7.4</td>
<td>v1.7.16	</td>
<td>Vedi le [note sulla release Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/releases/tag/v1.7.16). Questa release risolve le vulnerabilità [CVE-2017-1002101 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) e [CVE-2017-1002102 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102).</td>
</tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.7.4-133</td>
<td>v1.7.16-17</td>
<td>I servizi `NodePort` e `LoadBalancer` ora supportano [la conservazione dell'IP di origine client](cs_loadbalancer.html#node_affinity_tolerations) impostando `service.spec.externalTrafficPolicy` su `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Corregge la configurazione di tolleranza del [nodo edge](cs_edge.html#edge) per i cluster meno recenti.</td>
</tr>
</tbody>
</table>
