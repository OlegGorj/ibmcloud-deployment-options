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




# Rubriques les plus consultées pour {{site.data.keyword.containershort_notm}}
{: #cs_popular_topics}

Tenez-vous au courant de ce qui se passe dans {{site.data.keyword.containerlong}}. Découvrez les nouvelles fonctions à explorer, une astuce pour les expérimenter ou quelques rubriques populaires que d'autres développeurs jugent désormais utiles.
{:shortdesc}

## Rubriques les plus consultées en mai 2018
{: #may18}


<table summary="Ce tableau présente les rubriques les plus consultées. La lecture des lignes s'effectue de gauche à droite, avec la date indiquée dans la première colonne, le titre de la fonction dans la deuxième colonne et la description dans la troisième colonne.">
<caption>Rubriques les plus consultées pour les conteneurs et les clusters Kubernetes en mai 2018</caption>
<thead>
<th>Date</th>
<th>Titre</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td>24 mai</td>
<td>[Nouveau format de sous-domaine Ingress](cs_ingress.html)</td>
<td>Les clusters créés après le 24 mai ont un domaine Ingress qui leur est affecté dans un nouveau format : <code>&lt;cluster_name&gt;.&lt;region&gt;.containers.appdomain.cloud</code>. Lorsque vous utilisez Ingress pour exposer vos applications, vous pouvez recourir au nouveau sous-domaine pour accéder à vos applications sur Internet.</td>
</tr>
<tr>
<td>14 mai</td>
<td>[Mise à jour : déployez vos charges de travail sur des unités bare metal GPU dans le monde entier](cs_app.html#gpu_app)</td>
<td>Si vous disposez d'un [type de machine d'unité de traitement graphique (GPU)](cs_clusters.html#shared_dedicated_node) dans votre cluster, vous pouvez planifier des applications nécessitant de nombreux calculs mathématiques. Le noeud worker GPU peut traiter la charge de travail de votre application à la fois sur l'unité centrale (UC) et l'unité de traitement graphique (GPU) pour améliorer les performances.</td>
</tr>
<tr>
<td>3 mai</td>
<td>[Container Image Security Enforcement (bêta)](/docs/services/Registry/registry_security_enforce.html#security_enforce)</td>
<td>Besoin d'aide supplémentaire pour votre équipe pour savoir quelle image extraire dans vos conteneurs d'application ? Essayez la version bêta de Container Image Security Enforcement pour vérifier les images de conteneur avant de les déployer. Disponible pour les clusters exécutant Kubernetes 1.9 ou version ultérieure.</td>
</tr>
<tr>
<td>1er mai</td>
<td>[Déploiement du tableau de bord Kubernetes à partir de l'interface graphique](cs_app.html#cli_dashboard)</td>
<td>Avez-vous déjà envisagé de pouvoir accéder au tableau de bord Kubernetes d'un seul clic ? Vérifiez la présence du bouton **Tableau de bord Kubernetes** dans l'interface graphique d'{{site.data.keyword.Bluemix_notm}}.</td>
</tr>
</tbody></table>




## Rubriques les plus consultées en avril 2018
{: #apr18}

<table summary="Ce tableau présente les rubriques les plus consultées. La lecture des lignes s'effectue de gauche à droite, avec la date indiquée dans la première colonne, le titre de la fonction dans la deuxième colonne et la description dans la troisième colonne.">
<caption>Rubriques les plus consultées pour les conteneurs et les clusters Kubernetes en avril 2018</caption>
<thead>
<th>Date</th>
<th>Titre</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td>17 avril</td>
<td>{{site.data.keyword.Bluemix_notm}} Block Storage</td>
<td>Installez le [plug-in](cs_storage.html#install_block) {{site.data.keyword.Bluemix_notm}} Block Storage pour sauvegarder les données persistantes dans un stockage par blocs. Ensuite, vous pouvez [créer](cs_storage.html#create) ou [utiliser du stockage par blocs existant](cs_storage.html#existing_block) pour votre cluster.</td>
</tr>
<tr>
<td>13 avril</td>
<td>[Nouveau tutoriel pour la migration d'applications Cloud Foundry dans des clusters](cs_tutorials_cf.html#cf_tutorial)</td>
<td>Vous disposez d'une application Cloud Foundry ? Découvrez comment déployer le même code de cette application dans un conteneur qui s'exécute dans un cluster Kubernetes.</td>
</tr>
<tr>
<td>5 avril</td>
<td>[Filtrage des journaux](cs_health.html#filter-logs)</td>
<td>Filtrez des journaux spécifiques pour empêcher leur transfert. Les journaux peuvent être filtrés pour un espace de nom, un nom de conteneur, un niveau de consignation et une chaîne de message spécifiques.</td>
</tr>
</tbody></table>

## Rubriques les plus consultées en mars 2018
{: #mar18}

<table summary="Ce tableau présente les rubriques les plus consultées. La lecture des lignes s'effectue de gauche à droite, avec la date indiquée dans la première colonne, le titre de la fonction dans la deuxième colonne et la description dans la troisième colonne.">
<caption>Rubriques les plus consultées pour les conteneurs et les clusters Kubernetes en mars 2018</caption>
<thead>
<th>Date</th>
<th>Titre</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td>16 mars</td>
<td>[Mise à disposition d'un cluster bare metal avec calcul sécurisé](cs_clusters.html#shared_dedicated_node)</td>
<td>Créez un cluster bare metal qui exécute [Kubernetes version 1.9](cs_versions.html#cs_v19) ou ultérieure et activez la fonction de calcul sécurisé pour vérifier que vos noeuds worker ne font pas l'objet de falsification.</td>
</tr>
<tr>
<td>14 mars</td>
<td>[Connexion sécurisée avec {{site.data.keyword.appid_full}}](cs_integrations.html#appid)</td>
<td>Améliorez vos applications qui s'exécutent dans {{site.data.keyword.containershort_notm}} en obligeant les utilisateurs à se connecter.</td>
</tr>
<tr>
<td>13 mars</td>
<td>[Emplacement disponible à São Paulo](cs_regions.html)</td>
<td>Bienvenue à São Paulo au Brésil comme nouvelle implantation dans la région Sud des Etats-Unis. Si vous disposez d'un pare-feu, veillez à [ouvrir les ports de pare-feu](cs_firewall.html#firewall) pour cet emplacement ainsi que les autres ports situés dans la région où se trouve votre cluster.</td>
</tr>
<tr>
<td>12 mars</td>
<td>[Vous venez juste de rejoindre {{site.data.keyword.Bluemix_notm}} avec un compte d'essai ? Essayez gratuitement un cluster Kubernetes !](container_index.html#clusters)</td>
<td>Avec un [compte d'essai {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/registration/), vous pouvez déployer un cluster gratuit pendant 21 jours pour tester les fonctions de Kubernetes.</td>
</tr>
</tbody></table>

## Rubriques les plus consultées en février 2018
{: #feb18}

<table summary="Ce tableau présente les rubriques les plus consultées. La lecture des lignes s'effectue de gauche à droite, avec la date indiquée dans la première colonne, le titre de la fonction dans la deuxième colonne et la description dans la troisième colonne.">
<caption>Rubriques les plus consultées pour les conteneurs et les clusters Kubernetes en février 2018</caption>
<thead>
<th>Date</th>
<th>Titre</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td>27 février</td>
<td>Images HVM (Hardware virtual machine) pour les noeuds worker</td>
<td>Améliorez les performances d'E-S de vos charges de travail avec des images HVM. Activez cette fonction sur chaque noeud worker en place à l'aide de la [commande](cs_cli_reference.html#cs_worker_reload) `bx cs worker-reload` ou de la [commande](cs_cli_reference.html#cs_worker_update) `bx cs worker-update`.</td>
</tr>
<tr>
<td>26 février</td>
<td>[Mise à l'échelle automatique avec KubeDNS](https://kubernetes.io/docs/tasks/administer-cluster/dns-horizontal-autoscaling/)</td>
<td>KubeDNS s'adapte désormais à l'évolution de votre cluster. Vous pouvez ajuster les ratios de mise à l'échelle à l'aide de la commande : `kubectl -n kube-system edit cm kube-dns-autoscaler`.</td>
</tr>
<tr>
<td>23 février</td>
<td>Affichage de l'interface utilisateur Web pour la [consignation](cs_health.html#view_logs) et les [métriques](cs_health.html#view_metrics)</td>
<td>Affichez facilement les données des journaux et des métriques sur votre cluster et ses composants avec une interface utilisateur Web améliorée. Consultez la page des détails de votre cluster pour savoir comment y accéder.</td>
</tr>
<tr>
<td>20 février</td>
<td>Images chiffrées et [contenu sécurisé, signé](../services/Registry/registry_trusted_content.html#registry_trustedcontent)</td>
<td>Dans {{site.data.keyword.registryshort_notm}}, vous pouvez signer et chiffrer des images pour assurer leur intégrité lorsque vous les stockez dans votre espace de nom de registre. Exécutez vos instances de conteneur en utilisant uniquement du contenu sécurisé.</td>
</tr>
<tr>
<td>19 février</td>
<td>[Configuration du VPN IPSec strongSwan](cs_vpn.html#vpn-setup)</td>
<td>Déployez rapidement la charte Helm du VPN IPsec strongSwan pour connecter votre cluster {{site.data.keyword.containershort_notm}} de manière sécurisée à votre centre de données sur site sans dispositif de routeur virtuel (VRA).</td>
</tr>
<tr>
<td>14 février</td>
<td>[Emplacement disponible à Séoul](cs_regions.html)</td>
<td>Juste au bon moment pour les Jeux Olympiques, déployez un cluster Kubernetes à Séoul dans la région Asie-Pacifique nord. Si vous disposez d'un pare-feu, veillez à [ouvrir les ports de pare-feu](cs_firewall.html#firewall) pour cet emplacement ainsi que les autres ports situés dans la région où se trouve votre cluster.</td>
</tr>
<tr>
<td>8 février</td>
<td>[Mise à jour vers Kubernetes 1.9](cs_versions.html#cs_v19)</td>
<td>Examinez les modifications à apporter à vos clusters avant d'effectuer la mise à jour vers Kubernetes 1.9.</td>
</tr>
</tbody></table>

## Rubriques les plus consultées en janvier 2018
{: #jan18}

<table summary="Ce tableau présente les rubriques les plus consultées. La lecture des lignes s'effectue de gauche à droite, avec la date indiquée dans la première colonne, le titre de la fonction dans la deuxième colonne et la description dans la troisième colonne.">
<caption>Rubriques les plus consultées pour les conteneurs et les clusters Kubernetes en janvier 2018</caption>
<thead>
<th>Date</th>
<th>Titre</th>
<th>Description</th>
</thead>
<tbody>
<td>25 janvier</td>
<td>[Registre global disponible](../services/Registry/registry_overview.html#registry_regions)</td>
<td>Avec le registre {{site.data.keyword.registryshort_notm}}, vous pouvez utiliser le registre global `registry.bluemix.net` pour extraire les images publiques fournies par IBM.</td>
</tr>
<tr>
<td>23 janvier</td>
<td>[Emplacements disponibles à Singapour et Montréal (Canada)](cs_regions.html)</td>
<td>Singapour et Montréal sont les emplacements disponibles dans les régions {{site.data.keyword.containershort_notm}} Asie-Pacifique nord et Est des Etats-Unis. Si vous disposez d'un pare-feu, veillez à [ouvrir les ports de pare-feu](cs_firewall.html#firewall) pour ces emplacements ainsi que les autres ports situés dans la région où se trouve votre cluster.</td>
</tr>
<tr>
<td>8 janvier</td>
<td>[Versions disponibles améliorées](cs_cli_reference.html#cs_machine_types)</td>
<td>Les types de machine virtuelle de la série 2 comprennent du stockage SSD et le chiffrement de disque. [Déplacez vos charges de travail](cs_cluster_update.html#machine_type) sur ces versions pour améliorer les performances et la stabilité.</td>
</tr>
</tbody></table>

## Discussion avec des développeurs partageant les mêmes idées sur Slack
{: #slack}

Vous pouvez voir les thèmes de discussion d'autres personnes et poser vos propres questions dans [{{site.data.keyword.containershort_notm}} Slack. ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://ibm-container-service.slack.com)
{:shortdesc}

Si vous n'utilisez pas un ID IBM pour votre compte {{site.data.keyword.Bluemix_notm}}, [demandez une invitation](https://bxcs-slack-invite.mybluemix.net/) sur ce site Slack.
{: tip}
