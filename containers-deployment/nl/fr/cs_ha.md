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




# Haute disponibilité pour {{site.data.keyword.containerlong_notm}}
{: #ha}

Utilisez les fonctions intégrées de Kubernetes et {{site.data.keyword.containerlong}} pour accentuer la haute disponibilité de votre cluster et éviter l'indisponibilité de votre application en cas de défaillance d'un composant dans votre cluster.
{: shortdesc}

La haute disponibilité est une discipline fondamentale dans une infrastructure informatique pour maintenir vos applications opérationnelles même après une panne affectant une partie ou l'ensemble du site. L'objectif principal de la haute disponibilité est d'éliminer les points de défaillance potentiels au sein d'une infrastructure informatique. Par exemple, vous pouvez anticiper une panne système en ajoutant des modules de redondance et en configurant des mécanismes de basculement.

Vous pouvez obtenir la haute disponibilité à différents niveaux de votre infrastructure informatique et au sein des différents composants de votre cluster. Le niveau de disponibilité qui vous convient dépend de plusieurs facteurs, par exemple de vos besoins métier, des accords sur les niveaux de service (SLA) que vous avez contractés avec vos clients et de l'argent que vous comptez dépenser.

## Présentation des points de défaillance potentiels dans {{site.data.keyword.containerlong_notm}}
{: #fault_domains} 

L'architecture et l'infrastructure d'{{site.data.keyword.containerlong_notm}} est conçue pour assurer la fiabilité, réduire les temps d'attente de traitement et favoriser la disponibilité maximale du service. Cependant, il peut y avoir des incidents. En fonction du service que vous hébergez dans {{site.data.keyword.Bluemix_notm}}, vous ne serez pas forcément en mesure de tolérer des incidents, même s'ils ne durent que quelques minutes.
{: shortdesc}

{{site.data.keyword.containershort_notm}} fournit plusieurs approches pour accentuer la disponibilité de votre cluster en ajoutant des fonctions de redondance et d'anti-affinité. Examinez l'image suivante pour en savoir plus sur les points de défaillance potentiels et sur les moyens possibles de les éliminer.

<img src="images/cs_failure_ov.png" alt="Présentation des domaines d'erreur dans un cluster à haute disponibilité au sein d'une région {{site.data.keyword.containershort_notm}}." width="250" style="width:250px; border-style: none"/>


<table summary="Ce tableau présente les points de défaillance dans {{site.data.keyword.containershort_notm}}. La lecture des lignes s'effectue de gauche à droite, le numéro correspondant au point de défaillance figurant dans la première colonne, son titre dans la deuxième colonne, une description dans la troisième colonne et un lien vers la documentation dans la quatrième colonne.">
<caption>Points de défaillance</caption>
<col width="3%">
<col width="10%">
<col width="70%">
<col width="17%">
  <thead>
  <th>#</th>
  <th>Point de défaillance</th>
  <th>Description</th>
  <th>Lien vers la documentation</th>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>Défaillance de pod ou de conteneur</td>
      <td>De par leur conception, les conteneurs ont une durée de vie brève et ne sont pas à l'abri de défaillances inattendues. Par exemple, un conteneur ou un pod peuvent tomber en panne si une erreur est survenue dans votre application. Pour assurer la haute disponibilité de votre application, vous devez vérifier que vous disposez de suffisamment d'instances de cette application pour traiter la charge de travail et d'instances supplémentaires en cas de défaillance. Dans l'idéal, ces instances sont réparties sur plusieurs noeuds worker pour protéger votre application en cas de défaillance d'un noeud worker.</td>
      <td>[Déploiement d'applications à haute disponibilité.](cs_app.html#highly_available_apps)</td>
  </tr>
  <tr>
    <td>2</td>
    <td>Défaillance de noeud worker</td>
    <td>Un noeud worker est une machine virtuelle qui s'exécute au niveau supérieur d'un matériel physique. Parmi les défaillances possibles d'un noeud worker figurent des pannes matérielles, par exemple des pannes de courant, de ventilation ou de réseau ainsi que des défaillances sur la machine virtuelle même. Vous pouvez anticiper la défaillance d'un noeud worker en configurant plusieurs noeuds worker dans votre cluster. <br/><br/><strong>Remarque :</strong> Il n'est pas garanti que les noeuds worker situés à un même emplacement se trouvent sur des hôtes de calcul physiques distincts. Par exemple, vous pouvez détenir un cluster avec 3 noeuds worker, mais ces 3 noeuds ont été créés sur le même hôte de calcul physique à l'emplacement IBM. Si cet hôte de calcul tombe en panne, tous vos noeuds worker sont hors service. Pour éviter cela, vous devez configurer un second cluster à un autre emplacement.</td>
    <td>[Création de clusters avec plusieurs noeuds worker.](cs_cli_reference.html#cs_cluster_create)</td>
  </tr>
  <tr>
    <td>3</td>
    <td>Défaillance de cluster</td>
    <td>Le maître Kubernetes est le composant principal qui permet de garder votre cluster opérationnel. Le maître stocke toutes les données de cluster dans la base de données etcd qui assure le bon fonctionnement de votre cluster. Un cluster tombe en panne lorsque le maître est inaccessible en raison d'une panne de réseau ou si les données dans votre base de données etcd sont endommagées. Vous pouvez créer plusieurs clusters au même emplacement pour protéger vos applications en cas de défaillance du maître Kubernetes ou de la base de données etcd. Pour équilibrer la charge entre les clusters, vous devez configurer un équilibreur de charge externe. <br/><br/><strong>Remarque :</strong> La configuration de plusieurs clusters au même emplacement ne garantit pas le déploiement de vos noeuds worker sur des hôtes de calcul physiques distincts. Pour éviter cela, vous devez configurer un second cluster à un autre emplacement.</td>
    <td>[Configuration de clusters à haute disponibilité.](cs_clusters.html#planning_clusters)</td>
  </tr>
  <tr>
    <td>4</td>
    <td>Défaillance d'emplacement</td>
    <td>Une défaillance d'emplacement affecte tous les hôtes de calcul physiques et le stockage NFS. Ces défaillances peuvent être des pannes de courant, de ventilation, de réseau ou de stockage ou être dues à des catastrophes naturelles, telles que des inondations, des tremblements de terre ou des ouragans. Pour vous protéger en cas de défaillance d'un emplacement, vous devez disposer de clusters à deux emplacements différents dont la charge est équilibrée au moyen d'un équilibreur de charge externe.</td>
    <td>[Configuration de clusters à haute disponibilité.](cs_clusters.html#planning_clusters)</td>
  </tr>
  <tr>
    <td>5</td>
    <td>Défaillance de région</td>
    <td>Chaque région est configurée avec un équilibreur de charge à haute disponibilité accessible à partir du noeud final d'API spécifique à la région. L'équilibreur de charge achemine les demandes entrantes et sortantes aux clusters sur les différents emplacements de la région. La probabilité d'une défaillance totale au niveau de la région est faible. Toutefois, pour prendre en compte cette défaillance, vous pouvez configurer plusieurs clusters dans différentes régions et les connecter entre eux au moyen d'un équilibreur de charge externe. En cas de défaillance d'une région dans son ensemble, le cluster situé dans l'autre région peut prendre le relais. <br/><br/><strong>Remarque :</strong> Un cluster présent dans plusieurs régions nécessite plusieurs ressources de cloud et, en fonction de votre application, peut s'avérer complexe et coûteux. Vérifiez si vous avez besoin d'une configuration sur plusieurs régions ou si vous pouvez tolérer une interruption de service potentielle. Si vous souhaitez configurer un cluster sur plusieurs régions, assurez-vous que votre application et les données peuvent être hébergées dans une autre région et que votre application peut traiter la réplication globale des données.</td>
    <td>[Configuration de clusters à haute disponibilité.](cs_clusters.html#planning_clusters)</td>
  </tr>
  <tr>
    <td>6a, 6b</td>
    <td>Défaillance de stockage</td>
    <td>Dans une application avec état, les données jouent un rôle important pour maintenir votre application opérationnelle. Vous voulez vous assurer de la haute disponibilité de vos données pour pouvoir les récupérer suite à une défaillance éventuelle. Dans {{site.data.keyword.containershort_notm}}, vous pouvez choisir parmi plusieurs options pour conserver vos données. Par exemple, vous pouvez mettre à disposition du stockage NFS en utilisant des volumes persistants Kubernetes natifs, ou stocker vos données en utilisant un service de base de données {{site.data.keyword.Bluemix_notm}}.</td>
    <td>[Planification de données à haute disponibilité.](cs_storage.html#planning)</td>
  </tr>
  </tbody>
  </table>



