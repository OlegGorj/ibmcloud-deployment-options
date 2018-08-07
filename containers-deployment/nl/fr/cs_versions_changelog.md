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



# Journal des modifications de version
{: #changelog}

Affichez des informations sur les modifications de version correspondant à des mises à jour principale, secondaire ou de correctif disponibles pour vos clusters Kubernetes {{site.data.keyword.containerlong}}. Les modifications comprennent des mises à jour pour les composants de Kubernetes et d'{{site.data.keyword.Bluemix_notm}} Provider. 
{:shortdesc}

IBM applique automatiquement les mises à jour du maîtres incluant des correctifs, mais vous devez [mettre à jour vos noeuds worker](cs_cluster_update.html#worker_node). Vérifiez tous les mois s'il y a des mises à jour disponibles. A mesure que des mises à jour deviennent disponibles, vous en êtes informé lorsque vous affichez des informations sur les noeuds worker, par exemple avec les commandes `bx cs workers <cluster>` ou `bx cs worker-get <cluster> <worker>`.

Pour obtenir un récapitulatif des actions de migration, voir [Versions de Kubernetes](cs_versions.html).
{: tip}

Pour plus d'informations sur les modifications apportées depuis la version précédente, voir les journaux de modifications suivants.
-  [Journal des modifications](#110_changelog) - Version 1.10.
-  [Journal des modifications](#19_changelog) - Version 1.9.
-  [Journal des modifications](#18_changelog) - Version 1.8.
-  **Version dépréciée** : [Journal des modifications](#17_changelog) Version 1.7.

## Journal des modifications - Version 1.10
{: #110_changelog}

Passez en revue les modifications suivantes.

### Journal des modifications de noeud worker - Groupe de correctifs 1.10.1_1510, publié le 18 mai 2018
{: #1101_1510}

<table summary="Modifications effectuées depuis la version 1.10.1_1509">
<caption>Modifications depuis la version 1.10.1_1509</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Correctif pour résoudre un bogue qui se produisait lorsque vous utilisiez un plug-in de stockage par blocs.</td>
</tr>
</tbody>
</table>

### Journal des modifications de noeud worker - Groupe de correctifs 1.10.1_1509, publié le 16 mai 2018
{: #1101_1509}

<table summary="Modifications effectuées depuis la version 1.10.1_1508">
<caption>Modifications depuis la version 1.10.1_1508</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Les données que vous stockez dans le répertoire racine `kubelet` sont désormais sauvegardées dans un disque secondaire plus volumineux de la machine du noeud worker.</td>
</tr>
</tbody>
</table>

### Journal des modifications de la version 10. 1_1508, publié le 1er mai 2018
{: #1101_1508}

<table summary="Modifications effectuées depuis la version 1.9.7_1510">
<caption>Modifications depuis la version 1.9.7_1510</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico</td>
<td>v2.6.5</td>
<td>v3.1.1</td>
<td>Voir les [notes sur l'édition de Calico ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.projectcalico.org/v3.1/releases/#v311).</td>
</tr>
<tr>
<td>Kubernetes Heapster</td>
<td>v1.5.0</td>
<td>v1.5.2</td>
<td>Voir les [notes sur l'édition de Kubernetes Heapster ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/heapster/releases/tag/v1.5.2).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.9.7</td>
<td>v1.10.1</td>
<td>Voir les [notes sur l'édition de Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.1).</td>
</tr>
<tr>
<td>Configuration de Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Ajout de <code>StorageObjectInUseProtection</code> à l'option <code>--enable-admission-plugins</code> pour le serveur d'API Kubernetes du cluster.</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>1.14.8</td>
<td>1.14.10</td>
<td>Voir les [notes sur l'édition de Kubernetes DNS ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/dns/releases/tag/1.14.10).</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.9.7-102</td>
<td>v1.10.1-52</td>
<td>Mis à jour pour prendre en charge la version Kubernetes 1.10.</td>
</tr>
<tr>
<td>Prise en charge de GPU</td>
<td>N/A</td>
<td>N/A</td>
<td>La prise en charge des [charges de travail de conteneur d'unité de traitement graphique (GPU)](cs_app.html#gpu_app) est désormais disponible pour la planification et l'exécution. Pour obtenir la liste des types de machine GPU disponibles, voir [Matériel pour les noeuds worker](cs_clusters.html#shared_dedicated_node). Pour plus d'informations, voir la documentation Kubernetes relative à la [planification d'unités GPU ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/).</td>
</tr>
</tbody>
</table>

## Journal des modifications - Version 1.9
{: #19_changelog}

Passez en revue les modifications suivantes.

### Journal des modifications de noeud worker - Groupe de correctifs 1.9.7_1512, publié le 18 mai 2018
{: #197_1512}

<table summary="Modifications effectuées depuis la version 1.9.7_1511">
<caption>Modifications depuis la version 1.9.7_1511</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Correctif pour résoudre un bogue qui se produisait lorsque vous utilisiez un plug-in de stockage par blocs.</td>
</tr>
</tbody>
</table>

### Journal des modifications de noeud worker - Groupe de correctifs 1.9.7_1511, publié le 16 mai 2018
{: #197_1511}

<table summary="Modifications effectuées depuis la version 1.9.7_1510">
<caption>Modifications depuis la version 1.9.7_1510</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Les données que vous stockez dans le répertoire racine `kubelet` sont désormais sauvegardées dans un disque secondaire plus volumineux de la machine du noeud worker.</td>
</tr>
</tbody>
</table>

### Journal des modifications pour la version 1.9.7_1510, publié le 30 avril 2018
{: #197_1510}

<table summary="Modifications effectuées depuis la version 1.9.3_1506">
<caption>Modifications depuis la version 1.9.3_1506</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.9.3</td>
<td>v1.9.7	</td>
<td>Voir les [notes sur l'édition de Kubernetes![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.7). Cette édition traite les vulnérabilités [CVE-2017-1002101 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) et [CVE-2017-1002102 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102).</td>
</tr>
<tr>
<td>Configuration de Kubernetes</td>
<td>N/A</td>
<td>N/A</td>
<td>Ajout de `admissionregistration.k8s.io/v1alpha1=true` à l'option `--runtime-config` pour le serveur d'API Kubernetes du cluster.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.9.3-71</td>
<td>v1.9.7-102</td>
<td>Les services `NodePort` et `LoadBalancer` prennent désormais en charge la [conservation de l'adresse IP source du client](cs_loadbalancer.html#node_affinity_tolerations) en définissant l'élément `service.spec.externalTrafficPolicy` sur `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Correction de la configuration de tolérance des [noeuds de périphérie](cs_edge.html#edge) pour les clusters plus anciens.</td>
</tr>
</tbody>
</table>

## Journal des modifications - Version 1.8
{: #18_changelog}

Passez en revue les modifications suivantes.

### Journal des modifications de noeud worker - Groupe de correctifs 1.8.11_1511, publié le 18 mai 2018
{: #1811_1511}

<table summary="Modifications effectuées depuis la version 1.8.11_1510">
<caption>Modifications depuis la version 1.8.11_1510</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Correctif pour résoudre un bogue qui se produisait lorsque vous utilisiez un plug-in de stockage par blocs.</td>
</tr>
</tbody>
</table>

### Journal des modifications de noeud worker - Groupe de correctifs 1.8.11_1510, publié le 16 mai 2018
{: #1811_1510}

<table summary="Modifications effectuées depuis la version 1.8.11_1509">
<caption>Modifications depuis la version 1.8.11_1509</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Les données que vous stockez dans le répertoire racine `kubelet` sont désormais sauvegardées dans un disque secondaire plus volumineux de la machine du noeud worker.</td>
</tr>
</tbody>
</table>


### Journal des modifications pour la version 1.8.11_1509, publié le 19 avril 2018
{: #1811_1509}

<table summary="Modifications effectuées depuis la version 1.8.8_1507">
<caption>Modifications depuis la version 1.8.8_1507</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.8.8</td>
<td>v1.8.11	</td>
<td>Voir les [notes sur l'édition de Kubernetes![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.11). Cette édition traite les vulnérabilités [CVE-2017-1002101 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) et [CVE-2017-1002102 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102).</td>
</tr>
<tr>
<td>Image de conteneur Pause</td>
<td>3.0</td>
<td>3.1</td>
<td>Supprime les processus zombie orphelins hérités.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.8.8-86</td>
<td>v1.8.11-126</td>
<td>Les services `NodePort` et `LoadBalancer` prennent désormais en charge la [conservation de l'adresse IP source du client](cs_loadbalancer.html#node_affinity_tolerations) en définissant l'élément `service.spec.externalTrafficPolicy` sur `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Correction de la configuration de tolérance des [noeuds de périphérie](cs_edge.html#edge) pour les clusters plus anciens.</td>
</tr>
</tbody>
</table>

## Archive
{: #changelog_archive}

### Journal des modifications - Version 1.7 (déprécié)
{: #17_changelog}

Passez en revue les modifications suivantes.

#### Journal des modifications de noeud worker - Groupe de correctifs 1.7.16_1513, publié le 18 mai 2018
{: #1716_1513}

<table summary="Modifications effectuées depuis la version 1.7.16_1512">
<caption>Modifications depuis la version 1.7.16_1512</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Correctif pour résoudre un bogue qui se produisait lorsque vous utilisiez un plug-in de stockage par blocs.</td>
</tr>
</tbody>
</table>

#### Journal des modifications de noeud worker - Groupe de correctifs 1.7.16_1512, publié le 16 mai 2018
{: #1716_1512}

<table summary="Modifications effectuées depuis la version 1.7.16_1511">
<caption>Modifications depuis la version 1.7.16_1511</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Les données que vous stockez dans le répertoire racine `kubelet` sont désormais sauvegardées dans un disque secondaire plus volumineux de la machine du noeud worker.</td>
</tr>
</tbody>
</table>

#### Journal des modifications pour la version 1.7.16_1511, publié le 19 avril 2018
{: #1716_1511}

<table summary="Modifications effectuées depuis la version 1.7.4_1509">
<caption>Modifications depuis la version 1.7.4_1509</caption>
<thead>
<tr>
<th>Composant</th>
<th>V. précédente</th>
<th>V. en cours</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.7.4</td>
<td>v1.7.16	</td>
<td>Voir les [Notes sur l'édition de Kubernetes![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/releases/tag/v1.7.16). Cette édition traite les vulnérabilités [CVE-2017-1002101 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) et [CVE-2017-1002102 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102).</td>
</tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.7.4-133</td>
<td>v1.7.16-17</td>
<td>Les services `NodePort` et `LoadBalancer` prennent désormais en charge la [conservation de l'adresse IP source du client](cs_loadbalancer.html#node_affinity_tolerations) en définissant l'élément `service.spec.externalTrafficPolicy` sur `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Correction de la configuration de tolérance des [noeuds de périphérie](cs_edge.html#edge) pour les clusters plus anciens.</td>
</tr>
</tbody>
</table>
