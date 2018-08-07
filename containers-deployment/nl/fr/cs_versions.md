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



# Informations de version et actions de mise à jour
{: #cs_versions}

## Types de version Kubernetes
{: #version_types}

{{site.data.keyword.containerlong}} prend en charge plusieurs versions de Kubernetes simultanément. Lorsque la version la plus récente (n) est publiée, jusqu'à 2 versions antérieures (n-2) sont prises en charge. Les versions au-delà de deux versions avant la version la plus récente (n-3) sont d'abord dépréciées, puis finissent par ne plus être prises en charge.
{:shortdesc}

Versions Kubernetes actuellement prises en charge :

- Version la plus récente : 1.10.1
- Version par défaut : 1.9.7
- Version prise en charge : 1.8.11

**Versions dépréciées** : Lorsque des clusters s'exécutent sur une version Kubernetes dépréciée, vous disposez de 30 jours pour vérifier et passer à une version Kubernetes prise en charge. Passé ce délai, votre version ne sera plus prise en charge. Lors de cette période de dépréciation, vous pouvez exécuter un nombre limité de commandes dans vos clusters pour ajouter ou recharger des noeuds worker et mettre à jour le cluster. Vous ne pouvez pas créer de cluster dans la version dépréciée.

**Versions non prises en charge** : Si vous exécutez des clusters sur une version Kubernetes qui n'est pas prise en charge, [consultez les impacts potentiels](#version_types) concernant les mises à jour, puis [mettez à jour le cluster](cs_cluster_update.html#update) immédiatement pour continuer à recevoir les mises à jour de sécurité importantes et l'aide du support.

Pour vérifier la version du serveur d'un cluster, exécutez la commande suivante.

```
kubectl version  --short | grep -i server
```
{: pre}

Exemple de sortie :

```
Server Version: v1.9.7+9d6e0610086578
```
{: screen}


## Types de mise à jour
{: #update_types}

Votre cluster Kubernetes dispose de trois types de mise à jour : principale, secondaire et correctif.
{:shortdesc}

|Type de mise à jour|Exemples de libellé de version|Mis à jour par|Impact
|-----|-----|-----|-----|
|Principale|1.x.x|Vous|Modifications d'opérations pour des clusters, y compris scripts ou déploiements.|
|Secondaire|x.9.x|Vous|Modifications d'opérations pour des clusters, y compris scripts ou déploiements.|
|Correctif|x.x.4_1510|IBM et vous|Correctifs Kubernetes, ainsi que d'autres mises à jour du composant {{site.data.keyword.Bluemix_notm}} Provider, tels que des correctifs de sécurité et de système d'exploitation. IBM met automatiquement à jour les maîtres, mais vous appliquez les correctifs à vos noeuds worker.|
{: caption="Impacts des mises à jour Kubernetes" caption-side="top"}

A mesure que des mises à jour deviennent disponibles, vous en êtes informé lorsque vous affichez des informations sur les noeuds worker, par exemple avec les commandes `bx cs workers <cluster>` ou `bx cs worker-get <cluster> <worker>`.
-  **Mises à jour principale et secondaire** : commencez d'abord par [mettre à jour votre noeud maître](cs_cluster_update.html#master), puis [mettez à jour vos noeuds worker](cs_cluster_update.html#worker_node). 
   - Par défaut, vous ne pouvez pas mettre à jour le maître Kubernetes de plus de trois niveaux de version secondaire. Par exemple, si votre version maître actuelle est la version 1.5 et que vous voulez passer à la version 1.8, vous devez d'abord passer en version 1.7. Vous pouvez forcer la mise à jour pour continuer, mais effectuer une mise à jour au-delà de deux niveaux de version secondaire par rapport à la version en cours risque d'entraîner des résultats inattendus.
   - Si vous utilisez une version d'interface CLI `kubectl` qui ne correspond pas au moins à la version principale et secondaire (`major.minor`) de vos clusters, vous risquez d'obtenir des résultats inattendus. Assurez-vous de maintenir votre cluster Kubernetes et les [versions CLI](cs_cli_install.html#kubectl) à jour.
-  **Mises à jour de type correctif** : vérifiez tous les mois pour voir s'il y a une mise à jour disponible et utilisez la [commande](cs_cli_reference.html#cs_worker_update) `bx cs worker-update` ou la [commande](cs_cli_reference.html#cs_worker_reload) `bx cs worker-reload` pour appliquer ces correctifs de sécurité et de système d'exploitation. Pour plus d'informations, voir [Journal des modifications de version](cs_versions_changelog.html).

<br/>

Ces informations récapitulent les mises à jour susceptibles d'avoir un impact sur les applications déployées lorsque vous mettez à jour un cluster vers une nouvelle version à partir de la version précédente. 
-  [Actions de migration](#cs_v110) - Version 1.10.
-  [Actions de migration](#cs_v19) - Version 1.9.
-  [Actions de migration](#cs_v18) - Version 1.8.
-  [Archive](#k8s_version_archive) des versions dépréciées ou non prises en charge.

<br/>

Pour obtenir la liste complète des modifications, consultez les informations suivantes :
* [Journal des modifications Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md).
* [Journal des modifications de version IBM](cs_versions_changelog.html).

## Version 1.10
{: #cs_v110}

<p><img src="images/certified_kubernetes_1x10.png" style="padding-right: 10px;" align="left" alt="Ce badge indique une certification Kubernetes version 1.10 pour IBM Cloud Container Service."/> {{site.data.keyword.containerlong_notm}} est un produit certifié Kubernetes pour la version 1.10 sous le programme CNCF de certification de conformité logicielle de Kubernetes. _Kubernetes® est une marque de la Fondation Linux aux Etats-Unis et dans d'autres pays et est utilisé dans le cadre d'une licence de la Fondation Linux._</p>

Passez en revue les modifications que vous devrez peut-être apporter lors d'une mise à jour de la version Kubernetes précédente vers la version 1.10.

**Important** : pour pouvoir effectuer la mise à jour vers Kubernetes 1.10, vous devez suivre les étapes indiquées dans [Préparation à la mise à jour vers Calico v3](#110_calicov3).

<br/>

### Mise à jour avant le maître
{: #110_before}

<table summary="Mises à jour Kubernetes pour la version 1.10">
<caption>Modifications à effectuer avant la mise à jour du maître vers Kubernetes 1.10</caption>
<thead>
<tr>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico v3</td>
<td>La mise à jour vers Kubernetes version 1.10 fait également passer Calico de la version 2.6.5 à la version 3.1.1. <strong>Important</strong> : pour pouvoir effectuer la mise à jour vers Kubernetes v1.10, vous devez suivre les étapes indiquées dans [Préparation à la mise à jour vers Calico v3](#110_calicov3).</td>
</tr>
<tr>
<td>Règle réseau du tableau de bord Kubernetes</td>
<td>Dans Kubernetes 1.10, la règle réseau <code>kubernetes-dashboard</code> dans l'espace de nom <code>kube-system</code> bloque l'accès de tous les pods au tableau de bord Kubernetes. Mais elle <strong>n'affecte pas</strong> la possibilité d'accéder au tableau de bord à partir de la console {{site.data.keyword.Bluemix_notm}} ou via le proxy <code>kubectl proxy</code>. Si un pod nécessite l'accès au tableau de bord, vous pouvez ajouter un libellé <code>kubernetes-dashboard-policy: allow</code> à un espace de nom puis déployer le pod sur cet espace de nom.</td>
</tr>
<tr>
<td>Accès à l'API Kubelet</td>
<td>L'autorisation d'accès à l'API Kubelet est désormais déléguée au <code>serveur d'API Kubernetes</code>. L'accès à l'API Kubelet est basée sur les rôles de cluster (<code>ClusterRoles</code>) qui octroient le droit d'accès aux sous-ressources de <strong>noeud</strong>. Par défaut, Kubernetes Heapster dispose des rôles <code>ClusterRole</code> et <code>ClusterRoleBinding</code>. Cependant, si l'API Kubelet est utilisée par d'autres utilisateurs ou applications, vous devez leur accorder le droit d'utiliser l'API. Consultez la documentation Kubernetes sur l'[autorisation d'accès à Kubelet![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/admin/kubelet-authentication-authorization/#kubelet-authorization).</td>
</tr>
<tr>
<td>Suites de chiffrement</td>
<td>Les suites de chiffrement prises en charge sur le <code>serveur d'API Kubernetes</code> et l'API Kubelet sont désormais limitées à un sous-ensemble avec un niveau de chiffrement élevé (128 bits ou plus). Si vous disposez d'automatisation ou de ressources utilisant un niveau de chiffrement plus faible et qui s'appuient sur la communication avec le <code>serveur d'API Kubernetes</code> ou l'API Kubelet, activez un support de chiffrement plus fort avant d'effectuer la mise à jour du maître.</td>
</tr>
</tbody>
</table>

### Mise à jour après le maître
{: #110_after}

<table summary="Mises à jour Kubernetes pour la version 1.10">
<caption>Modifications à effectuer après la mise à jour du maître vers Kubernetes 1.10</caption>
<thead>
<tr>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico v3</td>
<td>Lorsque le cluster est mis à jour, toutes les données existantes de Calico appliquées au cluster sont automatiquement migrées pour utiliser la syntaxe de Calico v3. Pour afficher, ajouter ou modifier des ressources Calico avec la syntaxe de Calico v3, mettez à jour la [configuration de l'interface de ligne de commande de Calico à la version 3.1.1](#110_calicov3).</td>
</tr>
<tr>
<td>Adresse IP externe (<code>ExternalIP</code>) du noeud</td>
<td>La zone <code>ExternalIP</code> d'un noeud est désormais définie avec la valeur de l'adresse IP publique du noeud. Vérifiez et mettez à jour toutes les ressources qui dépendent de cette valeur.</td>
</tr>
<tr>
<td><code>kubectl port-forward</code></td>
<td>Désormais quand vous utilisez la commande <code>kubectl port-forward</code>, elle ne prend plus en charge l'indicateur <code>-p</code>. Si vos scripts s'appuient sur le comportement précédent, mettez-les à jour pour remplacer l'indicateur <code>-p</code> par le nom du pod.</td>
</tr>
<tr>
<td>Volumes de données d'API en lecture seule</td>
<td>Désormais les éléments `secret`, `configMap`, `downwardAPI` et les volumes projetés sont montés en lecture seule.
Auparavant, les applications étaient autorisées à écrire des données dans ces volumes qui pouvaient être
automatiquement rétablies par le système. Cette action de migration est requise pour corriger
la vulnérabilité de sécurité [CVE-2017-1002102![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=2017-1002102).
Si vos applications s'appuient sur ce comportement peu fiable en matière de sécurité, modifiez-les en conséquence.</td>
</tr>
</tbody>
</table>

### Préparation à la mise à jour vers Calico v3
{: #110_calicov3}

Avant de commencer, votre maître de cluster, ainsi que tous les noeuds worker doivent exécuter Kubernetes version 1.8 ou ultérieure et comporter au moins un noeud worker.

**Important** : préparez la mise à jour vers Calico v3 avant de mettre à jour le maître. Durant la mise à niveau du maître vers Kubernetes v1.10, les nouveaux pods et les nouvelles règles réseau de Kubernetes ou Calico ne sont plus planifiés. La durée pendant laquelle la mise à jour empêche toute nouvelle planification varie. Cette durée peut être de quelques minutes pour les clusters de petite taille avec des minutes supplémentaires pour chaque lot de 10 noeuds. Les règles réseau et les pods poursuivent leur exécution.

1.  Vérifiez que vos pods Calico sont sains.
    ```
    kubectl get pods -n kube-system -l k8s-app=calico-node -o wide
    ```
    {: pre}
    
2.  Si un pod n'est pas à l'état **Running**, supprimez-le ou attendez qu'il soit à l'état **Running** avant de continuer.

3.  Si vous générez automatiquement des règles Calico ou d'autres ressources Calico, mettez à jour vos outils d'automatisation pour générer ces ressources avec la [syntaxe de Calico v3 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/).

4.  Si vous utilisez [strongSwan](cs_vpn.html#vpn-setup) pour la connectivité VPN, la charte Helm 2.0.0 strongSwan ne fonctionne pas avec Calico v3 ou Kubernetes 1.10. [Mettez à jour strongSwan](cs_vpn.html#vpn_upgrade) avec la charte Helm 2.1.0, qui offre une compatibilité en amont avec Calico 2.6 et Kubernetes 1.7, 1.8 et 1.9.

5.  [Mettez à jour votre maître de cluster vers Kubernetes v1.10](cs_cluster_update.html#master).

<br />


## Version 1.9
{: #cs_v19}

<p><img src="images/certified_kubernetes_1x9.png" style="padding-right: 10px;" align="left" alt="Ce badge indique une certification Kubernetes version 1.9 pour IBM Cloud Container Service."/> {{site.data.keyword.containerlong_notm}} est un produit certifié Kubernetes pour la version 1.9 sous le programme CNCF de certification de conformité logicielle de Kubernetes. _Kubernetes® est une marque de la Fondation Linux aux Etats-Unis et dans d'autres pays et est utilisé dans le cadre d'une licence de la Fondation Linux._</p>

Passez en revue les modifications que vous devrez peut-être apporter lors d'une mise à jour de la version Kubernetes précédente vers la version 1.9.

<br/>

### Mise à jour avant le maître
{: #19_before}

<table summary="Mises à jour Kubernetes pour la version 1.9">
<caption>Modifications à apporter avant la mise à jour du maître vers la version Kubernetes 1.9</caption>
<thead>
<tr>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>API d'admission webhook</td>
<td>L'API d'admission, qui est utilisée lorsque le serveur d'API appelle des webhooks de contrôle d'admission, a été transférée de <code>admission.v1alpha1</code> à <code>admission.v1beta1</code>. <em>Vous devez supprimer les éventuels webhooks existants avant de mettre à niveau votre cluster</em> et mettre à jour les fichiers de configuration webhook afin d'utiliser l'API la plus récente. Cette modification n'est pas compatible en amont.</td>
</tr>
</tbody>
</table>

### Mise à jour après le maître
{: #19_after}

<table summary="Mises à jour Kubernetes pour la version 1.9">
<caption>Modifications à apporter après la mise à jour du maître vers la version Kubernetes 1.9</caption>
<thead>
<tr>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Sortie `kubectl`</td>
<td>A présent, lorsque vous utilisez la commande `kubectl` en spécifiant `-o custom-columns` et que la colonne est introuvable dans l'objet, la sortie indique `<none>`.<br>
Auparavant, l'opération échouait et un message d'erreur `xxx is not found` (xxx introuvable) était affiché. Si vos scripts reposent sur le comportement antérieur, mettez-les à jour.</td>
</tr>
<tr>
<td>`kubectl patch`</td>
<td>Maintenant, lorsqu'aucune modification n'est apportée à la ressource concernée, la commande `kubectl patch` échoue avec `exit code 1`. Si vos scripts reposent sur le comportement antérieur, mettez-les à jour.</td>
</tr>
<tr>
<td>Droits sur le tableau de bord Kubernetes</td>
<td>Les utilisateurs doivent se connecter au tableau de bord Kubernetes avec leurs données d'identification pour afficher les ressources du cluster. L'autorisation RBAC `ClusterRoleBinding` du tableau de bord Kubernetes par défaut est supprimée. Pour les instructions correspondantes, voir [Lancement du tableau de bord Kubernetes](cs_app.html#cli_dashboard).</td>
</tr>
<tr>
<td>Volumes de données d'API en lecture seule</td>
<td>Désormais les éléments `secret`, `configMap`, `downwardAPI` et les volumes projetés sont montés en lecture seule.
Auparavant, les applications étaient autorisées à écrire des données dans ces volumes qui pouvaient être
automatiquement rétablies par le système. Cette action de migration est requise pour corriger la vulnérabilité
de sécurité [CVE-2017-1002102](https://cve.mitre.org/cgi-bin/cvename.cgi?name=2017-1002102).
Si vos applications s'appuient sur ce comportement peu fiable en matière de sécurité, modifiez-les en conséquence.</td>
</tr>
<tr>
<td>Annotations Taints et tolerations</td>
<td>Les annotations taint `node.alpha.kubernetes.io/notReady` et `node.alpha.kubernetes.io/unreachable` sont remplacées respectivement par `node.kubernetes.io/not-ready` et `node.kubernetes.io/unreachable`.<br>
Bien que ces annotations taint soient mises à jour automatiquement, vous devez mettre à jour manuellement leurs annotations tolerations. Pour chaque espace de nom à l'exception d'`ibm-system` et de `kube-system`, déterminez si vous devez modifier les annotations tolerations :<br>
<ul><li><code>kubectl get pods -n &lt;namespace&gt; -o yaml | grep "node.alpha.kubernetes.io/notReady" && echo "Action required"</code></li><li>
<code>kubectl get pods -n &lt;namespace&gt; -o yaml | grep "node.alpha.kubernetes.io/unreachable" && echo "Action required"</code></li></ul><br>
Si `Action required` est renvoyé, modifiez les annotations tolerations de pod en conséquence.</td>
</tr>
<tr>
<td>API d'admission webhook</td>
<td>Si vous avez supprimé des webhooks existants avant de mettre à jour le cluster, créez de nouveaux webhooks.</td>
</tr>
</tbody>
</table>

<br />



## Version 1.8
{: #cs_v18}

<p><img src="images/certified_kubernetes_1x8.png" style="padding-right: 10px;" align="left" alt="Ce badge indique la certification Kubernetes version 1.8 pour IBM Cloud Container Service."/> {{site.data.keyword.containerlong_notm}} est un produit certifié par Kubernetes pour la version 1.8 sous le programme CNCF de certification de conformité logicielle de Kubernetes. _Kubernetes® est une marque de la Fondation Linux aux Etats-Unis et dans d'autres pays et est utilisé dans le cadre d'une licence de la Fondation Linux._</p>

Passez en revue les modifications que vous devrez peut-être apporter lors d'une mise à jour de la version Kubernetes précédente vers la version 1.8.

<br/>

### Mise à jour avant le maître
{: #18_before}

<table summary="Mises à jour Kubernetes pour les versions 1.8">
<caption>Modifications à effectuer avant la mise à jour du maître vers Kubernetes 1.8</caption>
<thead>
<tr>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Aucune</td>
<td>Aucune modification n'est requise avant la mise à jour du maître</td>
</tr>
</tbody>
</table>

### Mise à jour après le maître
{: #18_after}

<table summary="Mises à jour Kubernetes pour les versions 1.8">
<caption>Modifications à effectuer après la mise à jour du maître vers Kubernetes 1.8</caption>
<thead>
<tr>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Connexion au tableau de bord Kubernetes</td>
<td>L'URL d'accès au tableau de bord Kubernetes en version 1.8 a changé et le processus de connexion inclut une nouvelle étape d'authentification. Pour plus d'informations, voir [Accès au tableau de bord Kubernetes](cs_app.html#cli_dashboard).</td>
</tr>
<tr>
<td>Droits sur le tableau de bord Kubernetes</td>
<td>Pour forcer les utilisateurs à se connecter avec leurs données d'identification pour afficher des ressources de cluster en version 1.8, supprimez l'autorisation RBAC 1.7 ClusterRoleBinding. Exécutez `kubectl delete clusterrolebinding -n kube-system kubernetes-dashboard`.</td>
</tr>
<tr>
<td>`kubectl delete`</td>
<td>La commande `kubectl delete` ne réduit plus les objets d'API de charge de travail, tels que les pods, avant suppression de l'objet. Si vous avez besoin de réduire l'objet, utilisez la commande [`kubectl scale ` ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/reference/kubectl/overview/#scale) avant de supprimer l'objet.</td>
</tr>
<tr>
<td>`kubectl run`</td>
<td>La commande `kubectl run` doit utiliser plusieurs indicateurs pour `--env` au lieu d'arguments séparés par une virgule. Par exemple, exécutez <code>kubectl run --env &lt;x&gt;=&lt;y&gt; --env &lt;z&gt;=&lt;a&gt;</code> et non pas <code>kubectl run --env &lt;x&gt;=&lt;y&gt;,&lt;z&gt;=&lt;a&gt;</code>. </td>
</tr>
<tr>
<td>`kubectl stop`</td>
<td>La commande `kubectl stop` n'est plus disponible.</td>
</tr>
<tr>
<td>Volumes de données d'API en lecture seule</td>
<td>Désormais les éléments `secret`, `configMap`, `downwardAPI` et les volumes projetés sont montés en lecture seule.
Auparavant, les applications étaient autorisées à écrire des données dans ces volumes qui pouvaient être
automatiquement rétablies par le système. Cette action de migration est requise pour corriger la vulnérabilité
de sécurité [CVE-2017-1002102](https://cve.mitre.org/cgi-bin/cvename.cgi?name=2017-1002102).
Si vos applications s'appuient sur ce comportement peu fiable en matière de sécurité, modifiez-les en conséquence.</td>
</tr>
</tbody>
</table>

<br />



## Archive
{: #k8s_version_archive}

### Version 1.7 (dépréciée)
{: #cs_v17}

**A partir du 22 mai 2018, les clusters {{site.data.keyword.containershort_notm}} qui exécutent Kubernetes version 1.7 sont dépréciés**. Après le 21 juin 2018, les clusters de version 1.7 ne peuvent pas recevoir de mises à jour de sécurité ou d'aide du support sauf s'ils sont mis à jour vers la version suivante la plus récente ([Kubernetes 1.8](#cs_v18)).

[Consultez l'impact potentiel](cs_versions.html#cs_versions) de chaque mise à jour de version Kubernetes, puis [mettez à jour vos clusters](cs_cluster_update.html#update) immédiatement.

Vous exécutez toujours Kubernetes version 1.5 ? Consultez les informations suivantes pour évaluer l'impact de la mise à jour de votre cluster de la version 1.5 à 1.7. [Mettez à jour vos clusters](cs_cluster_update.html#update) vers la version 1.7, puis mettez-les immédiatement à jour à une version supérieure ou égale à 1.8.
{: tip}

<p><img src="images/certified_kubernetes_1x7.png" style="padding-right: 10px;" align="left" alt="Ce badge indique une certification Kubernetes version 1.7 pour IBM Cloud Container Service."/> {{site.data.keyword.containerlong_notm}} est un produit certifié Kubernetes pour la version 1.7 sous le programme CNCF de certification de conformité logicielle de Kubernetes.</p>

Passez en revue les modifications que vous devrez peut-être apporter lors d'une mise à jour de la version Kubernetes précédente vers la version 1.7.

<br/>

#### Mise à jour avant le maître
{: #17_before}

<table summary="Mises à jour de Kubernetes pour les versions 1.7 et 1.6">
<caption>Modifications à effectuer avant la mise à jour du maître vers Kubernetes 1.7</caption>
<thead>
<tr>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Stockage</td>
<td>Les scripts de configuration avec les éléments `hostPath` et `mountPath` avec des références de répertoire parent, telles que `../to/dir` ne sont pas autorisés. Remplacez les chemins d'accès par des chemins absolus simples, par exemple, `/path/to/dir`.
<ol>
  <li>Déterminez si vous devez modifier des chemins de stockage :</br>
  ```
  kubectl get pods --all-namespaces -o yaml | grep "\.\." && echo "Action required"
  ```
  </br>

  <li>Si `Action required` est renvoyé, modifiez les pods de manière à référencer le chemin absolu avant de mettre à jour tous vos noeuds worker. Si le pod est détenu par une autre ressource, par exemple un déploiement, modifiez [_PodSpec_ ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/api-reference/v1.7/#podspec-v1-core) dans cette ressource.
</ol>
</td>
</tr>
</tbody>
</table>

#### Mise à jour après le maître
{: #17_after}

<table summary="Mises à jour de Kubernetes pour les versions 1.7 et 1.6">
<caption>Modifications à effectuer après la mise à jour du maître vers Kubernetes 1.7</caption>
<thead>
<tr>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>`apiVersion` dans le fichier Deployment</td>
<td>Après avoir mis à jour le cluster à partir de Kubernetes 1.5, utilisez `apps/v1beta1` pour la zone `apiVersion` dans les nouveaux fichiers YAML `Deployment`. Continuez à utiliser `extensions/v1beta1` pour les autres ressources, par exemple `Ingress`.</td>
</tr>
<tr>
<td>'kubectl'</td>
<td>Après avoir mis à jour l'interface CLI `kubectl`, ces commandes `kubectl create` doivent utiliser plusieurs indicateurs à la place d'arguments séparés par des virgules :<ul>
 <li>`role`
 <li>`clusterrole`
 <li>`rolebinding`
 <li>`clusterrolebinding`
 <li>`secret`
 </ul>
</br>  Par exemple, exécutez la commande `kubectl create role --resource-name <x> --resource-name <y>` et non pas `kubectl create role --resource-name <x>,<y>`.</td>
</tr>
<tr>
<td>Règles réseau</td>
<td>L'annotation `net.beta.kubernetes.io/network-policy` n'est plus disponible.
<ol>
  <li>Déterminez si vous devez modifier des règles :</br>
  ```
  kubectl get ns -o yaml | grep "net.beta.kubernetes.io/network-policy" | grep "DefaultDeny" && echo "Action required"
  ```
  <li>Si `"Action required"` est renvoyé, ajoutez la règle réseau suivante à chaque espace de nom Kubernetes répertorié :</br>

  <pre class="codeblock">
  <code>
  kubectl create -n &lt;namespace&gt; -f - &lt;&lt;EOF
  kind: NetworkPolicy
  apiVersion: networking.k8s.io/v1
  metadata:
    name: default-deny
    namespace: &lt;namespace&gt;
  spec:
    podSelector: {}
  EOF
  </code>
  </pre>

  <li> Après avoir ajouté la règle réseau, retirez l'annotation `net.beta.kubernetes.io/network-policy` :
  ```
  kubectl annotate ns <namespace> --overwrite "net.beta.kubernetes.io/network-policy-"
  ```
  </li></ol>
</td></tr>
<tr>
<td>Planification d'affinité de pods</td>
<td> L'annotation `scheduler.alpha.kubernetes.io/affinity` est obsolète.
<ol>
  <li>Pour chaque espace de nom à l'exception d'`ibm-system` et de `kube-system`, déterminez si vous devez mettre à jour la planification d'affinité des pods :</br>
  ```
  kubectl get pods -n <namespace> -o yaml | grep "scheduler.alpha.kubernetes.io/affinity" && echo "Action required"
  ```
  </br></li>
  <li>Si `"Action required"` es renvoyé, utilisez la zone [_PodSpec_ ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/api-reference/v1.7/#podspec-v1-core) _affinity_ au lieu de l'annotation `scheduler.alpha.kubernetes.io/affinity`.</li>
</ol>
</td></tr>
<tr>
<td>RBAC pour `ServiceAccount` `default`</td>
<td><p>L'autorisation `ClusterRoleBinding` de l'administrateur pour `default` `ServiceAccount` dans l'espace de nom `default` est supprimée pour renforcer la sécurité du cluster. Les applications qui s'exécutent dans l'espace de nom `default` ne disposent plus des privilèges d'administrateur de cluster sur l'API Kubernetes et pourront obtenir des messages d'erreur de type `RBAC DENY`. Vérifiez votre application et le fichier `.yaml` correspondant pour voir si elle s'exécute dans l'espace de nom `default`, utilise le compte `default ServiceAccount` et accède à l'API Kubernetes.</p>
<p>Si vos applications dépendent de ces privilèges, [créez des ressources d'autorisation RBAC![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview) pour vos applications.</p>
  <p>Lorsque vous mettez à jour les règles RBAC de votre application, vous souhaiterez peut-être revenir temporairement à l'espace de nom `default` précédent. Copiez, sauvegardez et appliquez les fichiers suivants avec la commande `kubectl apply -f FILENAME`. <strong>Remarque</strong> : Revenez à 'default' pour vous laisser le temps de mettre à jour l'ensemble des ressources de votre application et non pas comme solution à long terme.</p>

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
<td>Volumes de données d'API en lecture seule</td>
<td>Désormais les éléments `secret`, `configMap`, `downwardAPI` et les volumes projetés sont montés en lecture seule.
Auparavant, les applications étaient autorisées à écrire des données dans ces volumes qui pouvaient être
automatiquement rétablies par le système. Cette action de migration est requise pour corriger la vulnérabilité
de sécurité [CVE-2017-1002102](https://cve.mitre.org/cgi-bin/cvename.cgi?name=2017-1002102).
Si vos applications s'appuient sur ce comportement peu fiable en matière de sécurité, modifiez-les en conséquence.</td>
</tr>
<tr>
<td>DNS de pod StatefulSet</td>
<td>Les pods StatefulSet perdent leurs entrées DNS Kubernetes après la mise à jour du maître. Pour restaurer les entrées DNS, supprimez les pods StatefulSet. Kubernetes recrée les pods et restaure automatiquement les entrées DNS. Pour plus d'informations, voir [Problèmes Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/kubernetes/kubernetes/issues/48327).</td>
</tr>
<tr>
<td>Annotation tolerations</td>
<td>L'annotation `scheduler.alpha.kubernetes.io/tolerations` n'est plus disponible.
<ol>
  <li>Pour chaque espace de nom à l'exception d'`ibm-system` et de `kube-system`, déterminez si vous devez modifier les annotations tolerations :</br>
  ```
  kubectl get pods -n <namespace> -o yaml | grep "scheduler.alpha.kubernetes.io/tolerations" && echo "Action required"
  ```
  </br>

  <li>Si `"Action required"` es renvoyé, utilisez la zone [_PodSpec_ ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/api-reference/v1.7/#podspec-v1-core) _tolerations_ au lieu de l'annotation `scheduler.alpha.kubernetes.io/tolerations`
</ol>
</td></tr>
<tr>
<td>Annotation taints</td>
<td>L'annotation `scheduler.alpha.kubernetes.io/taints` n'est plus disponible.
<ol>
  <li>Déterminez si vous devez modifier des annotations taints :</br>
  ```
  kubectl get nodes -o yaml | grep "scheduler.alpha.kubernetes.io/taints" && echo "Action required"
  ```
  <li>Si `"Action required"` est renvoyé, supprimez l'annotation `scheduler.alpha.kubernetes.io/taints` pour chaque noeud :</br>
  `kubectl annotate nodes <node> scheduler.alpha.kubernetes.io/taints-`
  <li>Ajoutez une annotation taint à chaque noeud :</br>
  `kubectl taint node <node> <taint>`
  </li></ol>
</td></tr>
</tbody>
</table>

<br />


### Version 1.5 (non prise en charge)
{: #cs_v1-5}

A partir du 4 avril 2018, les clusters {{site.data.keyword.containershort_notm}} qui exécutent [Kubernetes version 1.5](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.5.md) ne sont plus pris en charge. Les clusters de la version 1.5 ne peuvent pas recevoir des mises à jour ou du support sauf s'ils ont été mis à jour à la version suivante la plus récente ([Kubernetes 1.7](#cs_v17)).

[Consultez l'impact potentiel](cs_versions.html#cs_versions) de chaque mise à jour de version Kubernetes, puis [mettez à jour vos clusters](cs_cluster_update.html#update) immédiatement. Vous devez effectuer la mise à jour d'une version à la version suivante la plus récente, par exemple de 1.5 à 1.7 ou 1.8 à 1.9.
