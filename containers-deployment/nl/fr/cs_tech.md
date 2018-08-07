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



# Technologie d'{{site.data.keyword.containerlong_notm}}

Découvrez la technologie sur laquelle s'appuie {{site.data.keyword.containerlong}}.
{:shortdesc}

## Conteneurs Docker
{: #docker_containers}

Basé sur la technologie Linux Containers (LXC), le projet open source intitulé Docker est devenu une plateforme logicielle que vous pouvez utiliser pour construire, tester, déployer et mettre à l'échelle rapidement des applications. Docker livre les logiciels dans des unités standardisées dénommées conteneurs qui incluent tous les éléments dont une application a besoin pour s'exécuter.
{:shortdesc}

En savoir plus sur certains concepts Docker de base :

<dl>
<dt>Image</dt>
<dd>Une image Docker est générée à partir d'un Dockerfile, lequel est un fichier texte qui définit comment générer l'image et les artefacts de génération qu'elle doit inclure, comme l'application, la configuration de l'application et ses dépendances. Les images sont toujours construites à partir d'autres images, ce qui les rend faciles à configurer. Quelqu'un d'autre peut faire le gros du travail sur une image en vous laissant la personnaliser.</dd>
<dt>Registre</dt>
<dd>Un registre d'images est un emplacement où stocker, extraire et partager des images Docker. Les images stockées dans un registre peuvent être accessibles au public (registre public) ou seulement par un petit groupe d'utilisateurs (registre privé). {{site.data.keyword.containershort_notm}} propose des images publiques, telles que ibmliberty, que vous pouvez utiliser pour créer votre première application conteneurisée. Dans le cas d'applications d'entreprise, utilisez un registre privé tel que celui fourni dans {{site.data.keyword.Bluemix_notm}} pour protéger vos images contre une utilisation par des utilisateurs non autorisés.
</dd>
<dt>Conteneur</dt>
<dd>Chaque conteneur est créé depuis une image. Un conteneur est un package d'application avec toutes ses dépendances de sorte que l'application puisse être transférée entre des environnements et exécutée sans modifications. Contrairement aux machines virtuelles, les conteneurs ne virtualisent pas une unité, son système d'exploitation et le matériel sous-jacent. Seuls le code d'application, l'environnement d'exécution, les outils système, les bibliothèques et les paramètres sont inclus dans le conteneur. Les conteneurs opèrent sous forme de processus isolés sur des hôtes de calcul Ubuntu et partagent le système d'exploitation hôte et ses ressources matérielles. Cette approche rend le conteneur plus léger, portable, et efficace, qu'une machine virtuelle.</dd>
</dl>



### Principaux avantages de l'utilisation de conteneurs
{: #container_benefits}

<dl>
<dt>Les conteneurs sont agiles</dt>
<dd>Les conteneurs simplifient l'administration du système en fournissant des environnements standardisés pour des déploiements en espace de développement et de production. L'environnement d'exécution simple permet un dimensionnement rapide par augmentation ou diminution des déploiements. Libérez-vous de la complexité de gérer des plateformes de système d'exploitation différentes et leurs infrastructures sous-jacentes en utilisant des conteneurs qui vous aideront à déployer et à exécuter rapidement et de manière fiable n'importe quelle application sur une infrastructure quelconque.</dd>
<dt>Les conteneurs sont de taille modeste</dt>
<dd>Vous pouvez intégrer beaucoup de conteneurs dans l'espace monopolisé par une seule machine virtuelle.</dd>
<dt>Les conteneurs sont portables</dt>
<dd>
<ul>
  <li>Réutiliser des éléments d'image pour construire des conteneurs. </li>
  <li>Déplacer rapidement le code d'application de l'environnement de transfert aux environnements de production.</li>
  <li>Automatiser vos processus avec des outils de distribution continue.</li>
  </ul>
  </dd>

<p>Découvrez comment [sécuriser vos informations personnelles](cs_secure.html#pi) lorsque vous utilisez des images de conteneur.</p>

<p>Prêt à en savoir plus sur Docker ? <a href="https://developer.ibm.com/courses/all/docker-essentials-extend-your-apps-with-containers/" target="_blank">Découvrez comment Docker et {{site.data.keyword.containershort_notm}} fonctionnent ensemble en suivant ce cours.</a></p>

</dl>

<br />


## Clusters Kubernetes
{: #kubernetes_basics}

<img src="images/certified-kubernetes-resized.png" style="padding-right: 10px;" align="left" alt="Ce badge indique une certification Kubernetes pour IBM Cloud Container Service."/>Le projet open source intitulé Kubernetes combine l'exécution d'une infrastructure conteneurisée avec des charges de travail en production, des contributions open source et des outils de gestion de conteneurs Docker. L'infrastructure Kubernetes fournit une plateforme d'application isolée et sécurisée pour la gestion des conteneurs, qui est à la fois portable, extensible et dotée de fonctions de réparation spontanée en cas de basculements.
{:shortdesc}

Découvrez des concepts Kubernetes de base comme illustré dans le diagramme suivant.

![Configuration de déploiement](images/cs_app_tutorial_components1.png)

<dl>
<dt>Compte</dt>
<dd>Les termes "votre compte" font référence à votre compte {{site.data.keyword.Bluemix_notm}}.</dd>

<dt>Cluster</dt>
<dd>Un cluster Kubernetes est composé d'un ou de plusieurs hôtes de calcul dénommés noeuds worker. Les noeuds worker sont gérés par un maître Kubernetes qui assure le contrôle centralisé et la surveillance de toutes les ressources Kubernetes dans le cluster. Donc lorsque vous déployez les ressources pour une application conteneurisée, la maître Kubernetes décide sur quel noeud worker déployer ces ressources, en prenant en compte les exigences de déploiement et la capacité disponible dans le cluster. Les ressources Kubernetes incluent des services, des déploiements et des pods.</dd>

<dt>Service</dt>
<dd>Un service est une ressource Kubernetes qui regroupe un ensemble de pods et fournit la connectivité réseau à ces pods sans exposer l'adresse IP privée réelle de chaque pod. Vous pouvez utiliser un service pour rendre votre application accessible dans votre cluster ou sur l'Internet public.
</dd>

<dt>Déploiement</dt>
<dd>Un déploiement est une ressource Kubernetes où vous pouvez spécifier des informations sur d'autres ressources ou capacités requises pour exécuter votre application, telles que services, stockage persistant ou annotations. Vous documentez un déploiement dans un fichier de configuration YAML que vous appliquez au cluster. Le maître Kubernetes configure les ressources et déploie des conteneurs dans des pods sur les noeuds worker avec la capacité disponible.
</br></br>
Définissez des stratégies de mise à jour de votre application, notamment le nombre de pods que vous voulez ajouter lors d'une mise à jour en continu et le nombre de pods pouvant être indisponibles à un moment donné. Lorsque vous effectuez une mise à jour en continu, le déploiement vérifie si la mise à jour fonctionne et l'arrête si des échecs sont détectés.</dd>

<dt>Pod</dt>
<dd>Chaque application conteneurisée déployée dans un cluster est déployée, exécutée et gérée par une ressource a Kubernetes dénommée pod. Les pods sont de petites unités déployables dans un cluster Kubernetes et servent à regrouper des conteneurs devant être traités comme une seule unité. Dans la plupart des cas, chaque conteneur est déployé sur son propre pod. Toutefois, une application peut nécessiter qu'un conteneur et d'autres conteneurs auxiliaires soient déployés dans un même pod afin qu'ils soient accessibles via la même adresse IP privée.</dd>

<dt>Application</dt>
<dd>Une application peut se référer à une application complète ou à un composant d'une application. Vous pourriez déployer des composants d'une application dans des composants distincts ou des noeuds worker distincts.</dd>

<p>Découvrez comment [sécuriser vos informations personnelles](cs_secure.html#pi) lorsque vous utilisez des ressources Kubernetes.</p>

<p>Prêt à en savoir plus sur Kubernetes ?</p>
<ul><li><a href="cs_tutorials.html#cs_cluster_tutorial" target="_blank">Développez vos connaissances en termes de terminologie avec le tutoriel Création de clusters</a>.</li>
<li><a href="https://developer.ibm.com/courses/all/get-started-kubernetes-ibm-cloud-container-service/" target="_blank">Découvrez comment Kubernetes et {{site.data.keyword.containershort_notm}} fonctionnent ensemble en suivant ce cours.</a></li></ul>


</dl>

<br />


## Architecture de service
{: #architecture}

Dans un cluster Kubernetes qui s'exécute sur {{site.data.keyword.containershort_notm}}, vos applications conteneurisées sont hébergées sur des hôtes de calcul nommés noeuds worker. Plus précisément, les applications s'exécutent dans des pods et ces pods sont hébergés sur des noeuds worker. Les noeuds worker sont gérés par le maître Kubernetes. Le maître Kubernetes et les noeuds worker communiquent entre eux au moyen de certificats TLS sécurisés et d'une connexion OpenVPN pour orchestrer vos configurations de cluster.
{: shortdesc}

Quelle est la différence entre le maître Kubernetes et un noeud worker ? Bonne question !

<dl>
  <dt>Maître Kubernetes</dt>
    <dd>Le maître Kubernetes est chargé de gérer toutes les ressources de calcul, de réseau et de stockage dans le cluster. Il assure que vos applications et services conteneurisés sont déployés de manière égale sur les noeuds worker dans le cluster. En fonction de la configuration de vos applications et de vos services, le maître détermine le noeud worker qui dispose des ressources suffisantes pour répondre aux besoins de l'application.</dd>
  <dt>Noeud worker</dt>
    <dd>Chaque noeud worker correspond à une machine physique (bare metal) ou à une machine virtuelle qui s'exécute sur du matériel physique dans l'environnement de cloud. Lorsque vous mettez à disposition un noeud worker, vous déterminez les ressources disponibles dans les conteneurs qui sont hébergés sur ce noeud worker. Prêts à l'emploi, vos noeuds worker sont configurés avec un moteur Docker Engine géré par {{site.data.keyword.IBM_notm}}, ainsi que des ressources de calcul, un réseau et un service de volumes distincts. Les fonctions de sécurité intégrées assurent l'isolement, offrent des capacités de gestion des ressources et garantissent la conformité des noeuds worker en matière de sécurité.</dd>
</dl>

<p>
<figure>
 <img src="images/cs_org_ov.png" alt="{{site.data.keyword.containerlong_notm}} Kubernetes - Architecture">
 <figcaption>Architecture d'{{site.data.keyword.containershort_notm}}</figcaption>
</figure>
</p>

Vous souhaitez voir comment utiliser {{site.data.keyword.containerlong_notm}} avec d'autres produits et services ? Consultez quelques exemples d'[intégrations](cs_integrations.html#integrations).


<br />

