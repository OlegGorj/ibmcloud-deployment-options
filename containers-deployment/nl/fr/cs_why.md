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



# Pourquoi utiliser {{site.data.keyword.containerlong_notm}}
{: #cs_ov}

{{site.data.keyword.containerlong}} propose des outils puissants en combinant les technologies Docker et Kubernetes, une expérience utilisateur intuitive et une sécurité et un isolement intégrés pour automatiser le déploiement, l'exploitation, la mise à l'échelle et la surveillance d'applications conteneurisées dans un cluster d'hôtes de calcul.
{:shortdesc}


## Avantages de l'utilisation du service
{: #benefits}

Les clusters sont déployés sur des hôtes de calcul qui fournissent des capacités Kubernetes natives et des capacités spécifiques à {{site.data.keyword.IBM_notm}}.
{:shortdesc}

|Avantage|Description|
|-------|-----------|
|Clusters Kubernetes à service exclusif avec isolement de l'infrastructure de traitement, de réseau et de stockage|<ul><li>Créez votre propre infrastructure personnalisée afin de répondre aux besoins de votre organisation.</li><li>Allouez à un maître Kubernetes dédié et sécurisé, des noeuds worker, des réseaux virtuels et un espace de stockage en utilisant les ressources fournies par l'infrastructure IBM Cloud (SoftLayer).</li><li>Le maître Kubernetes entièrement géré est constamment surveillé et mis à jour par {{site.data.keyword.IBM_notm}} pour que votre cluster soit toujours disponible.</li><li>Option permettant de mettre à disposition des noeuds worker en tant que serveurs bare metal avec la fonction Calcul sécurisé.</li><li>Stockez les données persistantes, partagez les données entre les pods Kubernetes et restaurez les données en cas de besoin avec le service de volumes intégré et sécurisé.</li><li>Tirez parti de la prise en charge complète de toutes les API Kubernetes natives.</li></ul>|
|Conformité en matière de sécurité d'image avec Vulnerability Advisor|<ul><li>Configurez votre propre registre d'images Docker privé et sécurisée où les images sont stockées et partagés par tous les utilisateurs dans l'organisation.</li><li>Tirez parti de l'analyse automatique des images dans votre registre {{site.data.keyword.Bluemix_notm}} privé.</li><li>Examinez les recommandations spécifiques au système d'exploitation utilisé dans l'image afin de corriger les vulnérabilités potentielles.</li></ul>|
|Surveillance en continu de l'état de santé du cluster|<ul><li>Utilisez le tableau de bord du cluster pour déterminer rapidement et gérer l'état de santé de votre cluster, des noeuds worker et des déploiements de conteneurs.</li><li>Accédez à des métriques de consommation détaillées en utilisant {{site.data.keyword.monitoringlong}} et élargissez rapidement votre cluster pour répondre aux charges de travail.</li><li>Examinez les informations de consignation à l'aide d'{{site.data.keyword.loganalysislong}} pour voir les activités détaillées du cluster.</li></ul>|
|Exposition sécurisée des applications au public|<ul><li>Sélectionnez une adresse IP publique, une route fournie par {{site.data.keyword.IBM_notm}} ou votre propre domaine personnalisé pour accéder à des services dans votre cluster depuis Internet.</li></ul>|
|Intégration de services {{site.data.keyword.Bluemix_notm}}|<ul><li>Vous pouvez ajouter des fonctionnalités supplémentaires à votre application via l'intégration de services {{site.data.keyword.Bluemix_notm}}, tels que les API Watson, Blockchain, services de données ou Internet of Things.</li></ul>|
{: caption="Avantages d'{{site.data.keyword.containerlong_notm}}" caption-side="top"}



<br />


## Comparaison d'offres et de leurs combinaisons
{: #differentiation}

Vous pouvez exécuter {{site.data.keyword.containershort_notm}} dans {{site.data.keyword.Bluemix_notm}} Public ou Dedicated, dans {{site.data.keyword.Bluemix_notm}} Private ou dans une configuration hybride.
{:shortdesc}


<table>
<caption>Différences entre les configurations d'{{site.data.keyword.containershort_notm}}</caption>
<col width="22%">
<col width="78%">
 <thead>
 <th>Configuration d'{{site.data.keyword.containershort_notm}}</th>
 <th>Description</th>
 </thead>
 <tbody>
 <tr>
 <td>{{site.data.keyword.Bluemix_notm}} Public
 </td>
 <td>Avec {{site.data.keyword.Bluemix_notm}} Public sur du [matériel partagé ou dédié ou sur des machines bare metal](cs_clusters.html#shared_dedicated_node), vous pouvez héberger vos applications dans des clusters sur le cloud à l'aide d'{{site.data.keyword.containershort_notm}}. {{site.data.keyword.containershort_notm}} sur {{site.data.keyword.Bluemix_notm}} Public fournit des outils performants en combinant les technologies de Docker et Kubernetes, une expérience utilisateur intuitive, ainsi qu'une sécurité et un isolement intégrés pour automatiser le déploiement, l'exploitation, la mise à l'échelle et la surveillance d'applications conteneurisées dans un cluster d'hôtes de calcul.<br><br>Pour plus d'informations, voir [Technologie d'{{site.data.keyword.containershort_notm}}](cs_tech.html).
 </td>
 </tr>
 <tr>
 <td>{{site.data.keyword.Bluemix_notm}} Dedicated
 </td>
 <td>{{site.data.keyword.Bluemix_notm}} Dedicated offre les mêmes fonctions qu'{{site.data.keyword.containershort_notm}} sur le cloud que {{site.data.keyword.Bluemix_notm}} Public. Cependant, avec un compte {{site.data.keyword.Bluemix_notm}} Dedicated, les [ressources physiques disponibles sont dédiées exclusivement à votre cluster](cs_clusters.html#shared_dedicated_node) et ne sont pas partagées par d'autres clients {{site.data.keyword.IBM_notm}}. Vous pouvez opter pour la mise en place d'un environnement {{site.data.keyword.Bluemix_notm}} Dedicated si vous avez besoin d'isoler votre cluster et les autres services {{site.data.keyword.Bluemix_notm}} que vous utilisez.<br><br>Pour plus d'informations, voir [Initiation aux clusters dans {{site.data.keyword.Bluemix_notm}} Dedicated](cs_dedicated.html#dedicated).
 </td>
 </tr>
 <tr>
 <td>{{site.data.keyword.Bluemix_notm}} Private
 </td>
 <td>{{site.data.keyword.Bluemix_notm}} Private est une plateforme applicative pouvant être installée en local sur vos propres machines. Vous pouvez choisir d'utiliser {{site.data.keyword.containershort_notm}} dans {{site.data.keyword.Bluemix_notm}} Private lorsque vous devez développer et gérer des applications conteneurisées sur site dans votre propre environnement contrôlé et protégé derrière un pare-feu. <br><br>Pour plus d'informations, voir les [informations produit d'{{site.data.keyword.Bluemix_notm}} Private ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/cloud/private) ainsi que la [documentation associée ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_1.2.0/kc_welcome_containers.html).
 </td>
 </tr>
 <tr>
 <td>Configuration hybride
 </td>
 <td>Une configuration hybride est l'utilisation combinée de services qui s'exécutent dans l'environnement {{site.data.keyword.Bluemix_notm}} Public ou Dedicated et d'autres services exécutés sur site, par exemple une application dans l'environnement {{site.data.keyword.Bluemix_notm}} Private. Exemples de configuration hybride : <ul><li>Mise à disposition d'un cluster avec {{site.data.keyword.containershort_notm}} dans l'environnement {{site.data.keyword.Bluemix_notm}} Public, mais en connectant ce cluster à une base de données sur site.</li><li>Mise à disposition d'un cluster avec {{site.data.keyword.containershort_notm}} dans l'environnement {{site.data.keyword.Bluemix_notm}} Private et déploiement d'une application dans ce cluster. Cependant, cette application peut utiliser un service {{site.data.keyword.ibmwatson}}, tel que {{site.data.keyword.toneanalyzershort}}, dans l'environnement {{site.data.keyword.Bluemix_notm}} Public.</li></ul><br>Pour activer la communication entre les services qui s'exécutent dans {{site.data.keyword.Bluemix_notm}} Public ou Dedicated et les services qui s'exécutent sur site, vous devez [configurer une connexion VPN](cs_vpn.html).
 </td>
 </tr>
 </tbody>
</table>

<br />


## Comparaison des clusters gratuits et standard
{: #cluster_types}

Vous pouvez créer un cluster gratuit ou n'importe quel nombre de clusters standard. Faites-vous la main avec les clusters gratuits pour vous familiariser et tester diverses fonctionnalités Kubernetes ou créez des clusters standard pour exploiter les pleines capacités Kubernetes pour déployer des applications.
{:shortdesc}

|Caractéristiques|Clusters gratuits|Clusters standard|
|---------------|-------------|-----------------|
|[Mise en réseau au sein d'un cluster](cs_secure.html#in_cluster_network)
|[Stockage de fichiers NFS persistant avec volumes](cs_storage.html#planning)| |<img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" />|
|[Accès à des applications réseau publiques ou privées par un service d'équilibreur de charge avec une adresse IP stable](cs_loadbalancer.html#planning)| |<img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" />|
|[Accès à des applications réseau publiques par un service Ingress avec une adresse IP stable et une URL personnalisable](cs_ingress.html#planning)| |<img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" />|
|[Adresses IP publiques portables](cs_subnets.html#manage)| |<img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" />|
|[Consignation et surveillance](cs_health.html#logging)| |<img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" />|
|[Option permettant de mettre à disposition vos noeuds worker sur des serveurs physiques (bare metal)](cs_clusters.html#shared_dedicated_node) | |<img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" />|
|[Option permettant de mettre à disposition des noeuds worker bare metal avec la fonction de calcul sécurisé](cs_clusters.html#shared_dedicated_node) | |<img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" />|
|[Disponible dans {{site.data.keyword.Bluemix_dedicated_notm}}](cs_dedicated.html#dedicated_environment)| |<img src="images/confirm.svg" width="32" alt="Fonction disponible" style="width:32px;" />|
{: caption="Caractéristiques des clusters gratuits et standard" caption-side="top"}

<br />




{: #responsibilities}
**Remarque** : vous recherchez quelles sont vos responsabilités et les dispositions relatives à {{site.data.keyword.containerlong}} quand vous utilisez le service ?

{: #terms}
Voir [Responsabilités liées à l'utilisation d'{{site.data.keyword.containershort_notm}}](cs_responsibilities.html).
