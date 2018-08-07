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




# Sécurité d'{{site.data.keyword.containerlong_notm}}
{: #security}

Vous pouvez utiliser les fonctions de sécurité intégrées dans {{site.data.keyword.containerlong}} pour l'analyse des risques et la protection en matière de sécurité. Ces fonctions vous aident à protéger l'infrastructure de votre cluster Kubernetes et la communication réseau, à isoler vos ressources de traitement, et à garantir la conformité aux règles de sécurité des composants de votre infrastructure et des déploiements de conteneurs.
{: shortdesc}



## Sécurité par composant du cluster
{: #cluster}

Chaque cluster {{site.data.keyword.containerlong_notm}} dispose de fonctions de sécurité intégrées dans le [maître](#master) et les noeuds [worker](#worker) associés.
{: shortdesc}

Si vous disposez d'un pare-feu ou désirez exécuter des commandes `kubectl` depuis votre système local lorsque les règles de réseau d'entreprise empêchent l'accès à des noeuds finaux Internet publics, [ouvrez des ports sur votre pare-feu](cs_firewall.html#firewall). Pour connecter des applications de votre cluster à un réseau sur site ou à d'autres applications externes à votre cluster, [configurez la connectivité VPN](cs_vpn.html#vpn).

Dans le diagramme suivant, vous voyez que les fonctions de sécurité sont regroupées par maître Kubernetes, noeuds worker et images de conteneur.

<img src="images/cs_security.png" width="400" alt="{{site.data.keyword.containershort_notm}} sécurité de cluster" style="width:400px; border-style: none"/>


<table summary="La première ligne du tableau est répartie sur deux colonnes. La lecture des autres lignes s'effectue de gauche à droite, avec le composant de sécurité dans la première colonne et les fonctions à mettre en corrélation dans la deuxième colonne.">
<caption>Sécurité par composant</caption>
  <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Paramètres de sécurité de cluster intégrés dans {{site.data.keyword.containershort_notm}}</th>
  </thead>
  <tbody>
    <tr>
      <td>Maître Kubernetes</td>
      <td>Le maître Kubernetes présent dans chaque cluster est géré par IBM et caractérisé par une haute disponibilité. Le maître inclut des paramètres de sécurité {{site.data.keyword.containershort_notm}} qui assurent la conformité aux règles de sécurité et une communication sécurisée vers et depuis les noeuds worker. Des mises à jour de sécurité sont effectuées par IBM selon les besoins. Le maître Kubernetes dédié régit et surveille depuis une position centralisée toutes les ressources Kubernetes dans le cluster. En fonction des exigences du déploiement et de la capacité du cluster, le maître Kubernetes planifie automatiquement le déploiement de vos applications conteneurisées entre les noeuds worker disponibles. Pour plus d'informations, voir [Sécurité du maître Kubernetes](#master).</td>
    </tr>
    <tr>
      <td>Noeud worker</td>
      <td>Les conteneurs sont déployés sur des noeuds worker dédiés à un cluster et qui assurent l'isolement du traitement, du réseau et du stockage pour les clients IBM. {{site.data.keyword.containershort_notm}} dispose de fonctions de sécurité intégrées pour sécuriser vos noeuds worker sur réseau privé et public et pour garantir la conformité des noeuds worker aux règles de sécurité. Pour plus d'informations, voir [Sécurité des noeuds worker](#worker). De plus, vous pouvez ajouter des [règles réseau Calico](cs_network_policy.html#network_policies) pour spécifier le trafic réseau qui est autorisé ou bloqué en provenance ou à destination d'un pod sur un noeud worker.</td>
    </tr>
    <tr>
      <td>Images</td>
      <td>En tant qu'administrateur du cluster, vous pouvez configurer votre propre registre d'images Docker sécurisé dans {{site.data.keyword.registryshort_notm}} pour y stocker et partager des images Docker avec les utilisateurs de votre cluster. Pour garantir des déploiements de conteneurs sécurisés, chaque image dans votre registre privé est analysée par Vulnerability Advisor. Vulnerability Advisor est un composant d'{{site.data.keyword.registryshort_notm}} qui détecte des vulnérabilités potentielles, soumet des recommandations de sécurité et des instructions pour résoudre ces vulnérabilités. Pour plus d'informations, voir [Sécurité des images dans {{site.data.keyword.containershort_notm}}](#images).</td>
    </tr>
  </tbody>
</table>

<br />


## Maître Kubernetes
{: #master}

Examinez les fonctions de sécurité intégrées du maître Kubernetes destinées à protéger ce dernier et à sécuriser les communications réseau du cluster.
{: shortdesc}

<dl>
  <dt>Maître Kubernetes entièrement géré et dédié</dt>
    <dd>Chaque cluster Kubernetes dans {{site.data.keyword.containershort_notm}} est régi par un maître Kubernetes dédié géré par IBM dans un compte d'infrastructure IBM Cloud (SoftLayer) sous propriété IBM. Le maître Kubernetes est configuré avec les composants dédiés suivants qui ne sont pas partagés avec d'autres clients IBM ou par différents clusters au sein du même compte IBM.
      <ul><li>Magasin  de données etcd : stocke toutes les ressources Kubernetes d'un cluster, telles que services, déploiements et pods. Les éléments Kubernetes ConfigMaps et Secrets sont des données d'application stockées sous forme de paires clé-valeur afin de les utiliser dans une application s'exécutant dans un pod. Les données etcd sont stockées sur un disque chiffré géré par IBM et sauvegardé quotidiennement. Lorsqu'elles sont envoyées à un pod, les données sont chiffrées avec TLS pour garantir leur protection et leur intégrité. </li>
      <li>kube-apiserver : constitue le point d'entrée principal pour toutes les demandes du noeud worker au maître Kubernetes. Le serveur kube-apiserver valide et traite les demandes et a accès en lecture et écriture au magasin de données etcd.</li>
      <li>kube-scheduler : décide où déployer les pods en considérant la capacité du compte et les besoins en performances, les contraintes des règles en matière de matériel et de logiciel, les spécifications d'anti-affinité et les besoins de la charge de travail. Si aucun noeud worker ne correspond à ces exigences, le pod n'est pas déployé dans le cluster.</li>
      <li>kube-controller-manager : se charge de la surveillance des jeux de répliques et de la création de pods correspondants pour atteindre l'état désiré.</li>
      <li>OpenVPN : composant spécifique à {{site.data.keyword.containershort_notm}} permettant une connectivité réseau sécurisée pour toutes les communications du maître Kubernetes avec les noeuds worker.</li></ul></dd>
  <dt>Connectivité réseau TLS sécurisée pour toutes les communications du noeud worker avec le maître Kubernetes</dt>
    <dd>Pour sécuriser la connexion au maître Kubernetes, {{site.data.keyword.containershort_notm}} génère des certificats TLS qui chiffrent les communications vers et depuis le serveur kube-apiserver et le magasin de données etcd. Ces certificats ne sont jamais partagés entre les clusters ou entre les composants du maître Kubernetes.</dd>
  <dt>Connectivité réseau OpenVPN sécurisée pour toutes les communications du maître Kubernetes vers les noeuds worker</dt>
    <dd>Bien que Kubernetes sécurise les communications entre le maître Kubernetes et les noeuds worker en utilisant le protocole `https`, aucune authentification n'est fournie par défaut sur le noeud worker. Pour sécuriser ces communications, {{site.data.keyword.containershort_notm}} configure automatiquement une connexion OpenVPN entre le maître Kubernetes et les noeuds worker lors de la création du cluster.</dd>
  <dt>Surveillance continue du réseau maître Kubernetes</dt>
    <dd>Chaque maître Kubernetes est surveillé en permanence par IBM pour contrôler et contrer les attaques par déni de service (DOS) au niveau des processus.</dd>
  <dt>Conformité de sécurité du noeud maître Kubernetes</dt>
    <dd>{{site.data.keyword.containershort_notm}} analyse automatiquement chaque noeud sur lequel le maître Kubernetes est déployé pour détecter des vulnérabilités affectant Kubernetes et identifier les correctifs de sécurité spécifiques au système d'exploitation. Si des vulnérabilités sont détectées, {{site.data.keyword.containershort_notm}} applique automatiquement les correctifs appropriés et résout les vulnérabilités pour l'utilisateur.</dd>
</dl>

<br />


## Noeuds worker
{: #worker}

Examinez les fonctions de sécurité de noeud worker intégrées destinées à protéger l'environnement de noeud worker et à assurer l'isolement des ressources, du réseau et du stockage.
{: shortdesc}

<dl>
  <dt>Propriétaire des noeuds worker</dt>
    <dd>Le propriétaire des noeuds worker dépend du type de cluster que vous créez. <p> Les noeuds worker créés dans des clusters gratuits sont provisionnés dans le compte d'infrastructure IBM Cloud (SoftLayer) dont IBM est propriétaire. Les utilisateurs peuvent déployer des applications sur les noeuds worker mais sans en changer les paramètres ou installer des logiciels supplémentaires sur le noeud worker.</p>
    <p>Les noeuds worker dans les clusters standard sont provisionnés dans le compte d'infrastructure IBM Cloud (SoftLayer) associé au compte IBM Cloud public ou dédié de l'utilisateur. Les noeuds worker appartiennent au client. Les clients ont la possibilité de modifier les paramètres de sécurité ou d'installer des logiciels supplémentaires sur les noeuds worker, tels qu'ils sont fournis par {{site.data.keyword.containerlong}}.</p> </dd>
  <dt>Isolement de l'infrastructure de traitement, de réseau et de stockage</dt>
    <dd><p>Lorsque vous créez un cluster, des noeuds worker sont provisionnés sous forme de machines virtuelles par IBM. Les noeuds worker sont dédiés à un cluster et n'hébergent pas la charge de travail d'autres clusters.</p>
    <p> Chaque compte {{site.data.keyword.Bluemix_notm}} est configuré avec des réseaux locaux virtuels d'infrastructure IBM Cloud (SoftLayer) pour garantir des performances réseau satisfaisantes et l'isolement des noeuds worker. Vous pouvez également désigner des noeuds worker comme noeuds privés en les connectant uniquement à un réseau local virtuel privé.</p> <p>Pour rendre persistantes les données dans votre cluster, vous pouvez allouer un stockage de fichiers NFS dédié depuis l'infrastructure IBM Cloud (SoftLayer) et tirer parti des fonctions intégrées de sécurité des données de cette plateforme.</p></dd>
  <dt>Configuration d'un noeud worker sécurisé</dt>
    <dd><p>Chaque noeud worker est configuré avec un système d'exploitation Ubuntu qui ne peut pas être modifié par le propriétaire du noeud. Comme le système d'exploitation du noeud worker est Ubuntu, tous les conteneurs qui sont déployés sur le noeud worker doivent utiliser une distribution Linux utilisant le noyau Ubuntu. Les distributions Linux qui doivent communiquer avec le noyau d'une autre manière ne peuvent pas être utilisées. Afin de protéger ce système d'exploitation face aux attaques potentielles, chaque noeud worker est configuré avec des paramètres de pare-feu avancés imposés par les règles iptable de Linux.</p>
    <p>Tous les conteneurs s'exécutant sur Kubernetes sont protégés par des paramètres de règles réseau Calico prédéfinies qui sont configurées sur chaque noeud worker lors de la création du cluster. Cette configuration assure une communication réseau sécurisée entre les noeuds worker et les pods.</p>
    <p>L'accès SSH est désactivé sur le noeud worker. Si vous disposez d'un cluster standard et désirez installer plus de fonctionnalités sur votre noeud worker, vous pouvez utiliser des [DaemonSets Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset) pour tous les éléments que vous désirez exécuter sur chaque noeud worker. Pour toute action ponctuelle que vous devez exécuter, utilisez des [travaux Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/).</p></dd>
  <dt>Conformité à la sécurité de noeud worker Kubernetes</dt>
    <dd>IBM collabore avec des équipes de conseil en sécurité, internes et externes, pour traiter les vulnérabilités de conformité aux règles de sécurité potentielles. <b>Important</b> : utilisez la [commande](cs_cli_reference.html#cs_worker_update) `bx cs worker-update` régulièrement (par exemple tous les mois) pour déployer des mises à jour et des correctifs de sécurité sur le système d'exploitation et mettre à jour la version Kubernetes. Lorsque des mises à jour sont disponibles, vous en êtes informé lorsque vous affichez des informations sur les noeuds worker, par exemple avec les commandes `bx cs workers <cluster_name>` ou `bx cs worker-get <cluster_name> <worker_ID>`.</dd>
  <dt>Option permettant de déployer des noeuds sur des serveurs physiques (bare metal)</dt>
    <dd>Si vous choisissez de mettre à disposition vos noeuds worker sur des serveurs physiques bare metal (au lieu d'instances de serveur virtuel), vous bénéficiez d'un contrôle accru sur l'hôte de calcul, par exemple la mémoire ou l'UC. Cette configuration élimine l'hyperviseur de machine virtuelle qui alloue des ressources physiques aux machines virtuelles qui s'exécutent sur l'hôte. A la place, toutes les ressources d'une machine bare metal sont dédiées exclusivement au noeud worker, donc vous n'avez pas à vous soucier de "voisins gênants" partageant des ressources et responsables du ralentissement des performances. Les serveurs bare metal vous sont dédiés, avec toutes leurs ressources disponibles pour l'utilisation du cluster.</dd>
  <dt id="trusted_compute">{{site.data.keyword.containershort_notm}} avec calcul sécurisé</dt>
    <dd><p>Lorsque vous [déployez votre cluster sur un serveur bare metal](cs_clusters.html#clusters_ui) qui prend en charge la fonction de calcul sécurisé (Trusted Compute), vous pouvez activer la fonction de confiance (trust). La puce TPM (Trusted Platform Module) est activée sur chaque noeud worker bare metal dans le cluster prenant en charge la fonction de calcul sécurisé (y compris les prochains noeuds que vous ajoutez au cluster). Par conséquent, après avoir activé la fonction de confiance, vous ne pourrez plus la désactiver pour le cluster. Un serveur d'accréditation est déployé sur le noeud maître et un agent de confiance est déployé sous forme de pod sur le noeud worker. Au démarrage de votre noeud worker, le pod de l'agent de confiance surveille chaque étape du processus.</p>
    <p>Par exemple, si un utilisateur non autorisé parvient à accéder à votre système et modifie le noyau du système d'exploitation avec une logique supplémentaire pour collecter des données, l'agent de confiance le détecte et change le statut du noeud pour indiquer que le noeud n'est pas fiable. Avec la fonction de calcul sécurisé, vous pouvez vérifier que vos noeuds worker ne font pas l'objet de falsification.</p>
    <p><strong>Remarque</strong> : La fonction de calcul sécurisé (Trusted Compute) est disponible pour les types de machine bare metal sélectionnés. Par exemple, les versions GPU `mgXc` ne prennent pas en charge la fonction de calcul sécurisé.</p></dd>
  <dt id="encrypted_disks">Disque chiffré</dt>
    <dd><p>Par défaut, {{site.data.keyword.containershort_notm}} fournit deux partitions de données SSD locales chiffrées pour tous les noeuds worker lorsque ceux-ci sont provisionnés. La première partition n'est pas chiffrée et la seconde partition montée sur _/var/lib/docker_ est déverrouillée à l'aide des clés de chiffrement LUKS. Chaque noeud worker dans chaque cluster Kubernetes dispose de sa propre clé de chiffrement LUKS unique, gérée par {{site.data.keyword.containershort_notm}}. Lorsque vous créez un cluster ou ajoutez un noeud worker à un cluster existant, les clés sont extraites de manière sécurisée, puis ignorées une fois que le disque chiffré a été déverrouillé.</p>
    <p><b>Remarque </b>: le chiffrement peut avoir une incidence sur les performances des entrées-sorties (E-S) de disque. Dans le cas de charges de travail exigeant de hautes performances des E-S de disque, testez un cluster avec et sans chiffrement activé pour déterminer s'il convient de désactiver le chiffrement.</p></dd>
  <dt>Support pour les pare-feux réseau d'infrastructure IBM Cloud (SoftLayer)</dt>
    <dd>{{site.data.keyword.containershort_notm}} est compatible avec toutes les [offres de pare-feu d'infrastructure IBM Cloud (SoftLayer) ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/cloud-computing/bluemix/network-security). Sur {{site.data.keyword.Bluemix_notm}} Public, vous pouvez mettre en place un pare-feu avec des règles réseau personnalisées afin de promouvoir une sécurité réseau dédiée pour votre cluster standard et détecter et parer à des intrusions réseau. Par exemple, vous pouvez choisir de configurer un [dispositif de routeur virtuel](/docs/infrastructure/virtual-router-appliance/about.html) qui fera office de pare-feu et bloquera le trafic indésirable. Lorsque vous configurez un pare-feu, [vous devez également ouvrir les adresses IP et les ports requis](cs_firewall.html#firewall) pour chaque région de manière à permettre au maître et aux noeuds worker de communiquer.</dd>
  <dt>Gardez privés les services ou exposez sélectivement des services et des applications à l'Internet public</dt>
    <dd>Vous pouvez décider de garder privés vos services et applications et d'exploiter les fonctions de sécurité intégrées pour assurer une communication sécurisée entre les noeuds worker et les pods. Si vous désirez exposer des services et des applications sur l'Internet public, vous pouvez exploiter la prise en charge d'Ingress et d'un équilibreur de charge pour rendre vos services accessibles au public de manière sécurisée.</dd>
  <dt>Connectez de manière sécurisée vos noeuds worker et vos applications à un centre de données sur site</dt>
    <dd><p>Pour connecter vos noeuds worker et vos applications à un centre de données sur site, vous pouvez configurer un noeud final VPN IPsec avec un service strongSwan, un dispositif de routeur virtuel ou un dispositif de sécurité Fortigate.</p>
    <ul><li><b>Service VPN IPsec strongSwan</b> : vous pouvez définir un [service VPN IPsec strongSwan ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.strongswan.org/) pour connecter de manière sécurisée votre cluster Kubernetes avec un réseau sur site. Le service VPN IPSec strongSwan fournit un canal de communication de bout en bout sécurisé sur Internet basé sur la suite de protocoles IPSec (Internet Protocol Security) aux normes du l'industrie. Pour configurer une connexion sécurisée entre votre cluster et un réseau sur site, [configurez et déployez le service VPN IPSec strongSwan](cs_vpn.html#vpn-setup) directement dans un pod de votre cluster.
    </li>
    <li><b>Dispositif de routeur virtuel (VRA) ou dispositif de sécurité Fortigate (FSA)</b> : vous pouvez choisir de configurer un dispositif [VRA](/docs/infrastructure/virtual-router-appliance/about.html) ou [FSA](/docs/infrastructure/fortigate-10g/about.html) pour configurer un noeud final de VPN IPSec. Cette option est pratique si vous disposez d'un cluster plus volumineux, si vous souhaitez accéder à d'autres ressources que celles de Kubernetes via le réseau privé virtuel (VPN) ou accéder à plusieurs clusters via un seul VPN. Pour configurer un dispositif de routeur virtuel (VRA), voir la rubrique sur la [configuration d'une connectivité VPN avec un dispositif VRA](cs_vpn.html#vyatta).</li></ul></dd>

  <dt>Surveillance et consignation en continu de l'activité du cluster</dt>
    <dd>Pour les clusters standard, tous les événements liés aux clusters peuvent être consignés et envoyés à {{site.data.keyword.loganalysislong_notm}} et {{site.data.keyword.monitoringlong_notm}}. Ces événements comprennent l'ajout d'un noeud worker, la progression d'une mise à jour en continu ou les informations d'utilisation des capacités. Vous pouvez [configurer la consignation du cluster](/docs/containers/cs_health.html#logging) et la [surveillance du cluster](/docs/containers/cs_health.html#view_metrics) pour déterminer les événements que vous souhaitez surveiller. </dd>
</dl>

<br />


## Images
{: #images}

Gérez la sécurité et l'intégrité de vos images via des fonctions de sécurité intégrées.
{: shortdesc}

<dl>
<dt>Référentiel d'images Docker privé et sécurisé dans {{site.data.keyword.registryshort_notm}}</dt>
  <dd>Vous pouvez mettre en place votre propre registre d'images Docker dans un registre d'images privé, à service partagé, haute disponibilité et évolutif, hébergé et géré par IBM. En utilisant le registre, vous pouvez construire des images Docker et les stocker de manière sécurisée et les partager entre les utilisateurs du cluster.
  <p>Découvrez comment [sécuriser vos informations personnelles](cs_secure.html#pi) lorsque vous utilisez des images de conteneur.</p></dd>
<dt>Conformité de l'image avec les règles de sécurité</dt>
  <dd>En utilisant {{site.data.keyword.registryshort_notm}}, vous pouvez exploiter la fonctionnalité intégrée d'analyse de sécurité offerte par Vulnerability Advisor. Chaque image envoyée par commande push à votre espace de nom est automatiquement analysée pour détection de vulnérabilités face à une base de données de problèmes CentOS, Debian, Red Hat et Ubuntu connus. Si des vulnérabilités sont détectées, Vulnerability Advisor communique des instructions pour les résoudre et garantir l'intégrité et la sécurité de l'image.</dd>
</dl>

Pour examiner l'évaluation des vulnérabilités de vos images, [consultez la documentation de Vulnerability Advisor](/docs/services/va/va_index.html#va_registry_cli).

<br />


## Mise en réseau au sein d'un cluster
{: #in_cluster_network}

La communication réseau sécurisée au sein d'un cluster entre des noeuds worker et des pods est réalisée via des réseaux locaux virtuels (VLAN) privés. Un VLAN configure un groupe de noeuds worker et de pods comme s'ils étaient reliés physiquement au même câble.
{:shortdesc}

Lorsque vous créez un cluster, chaque cluster est automatiquement connecté à un VLAN privé. Le réseau local virtuel privé détermine l'adresse IP privée qui est affectée à un noeud worker lors de la création du cluster.

|Type de cluster|Gestionnaire du VLAN privé pour le cluster|
|------------|-------------------------------------------|
|Clusters gratuits dans {{site.data.keyword.Bluemix_notm}}|{{site.data.keyword.IBM_notm}}|
|Clusters standard dans {{site.data.keyword.Bluemix_notm}}|Vous dans votre compte d'infrastructure IBM Cloud (SoftLayer) <p>**Astuce :** pour avoir accès à tous les VLAN dans votre compte, activez la fonction [Spanning VLAN](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning).</p>|
{: caption="Gestionnaires de VLAN privés" caption-side="top"}

Une adresse IP privée est également affectée à chaque pod déployé sur un noeud worker. L'adresse IP affectée aux pods est située dans la plage d'adresses IP privées 172.30.0.0/16 et les pods ne sont utilisés que pour le routage entre les noeuds worker. Pour éviter des conflits, n'utilisez pas cette plage d'adresses IP sur des noeuds qui communiquent avec vos noeuds worker. Les noeuds worker et les pods peuvent communiquer de manière sécurisée sur le réseau privé en utilisant les adresses IP privées. Toutefois, lorsqu'un pod tombe en panne ou qu'un noeud worker a besoin d'être recréé, une nouvelle adresse IP privée lui est affectée.

Par défaut, il est difficile de suivre des adresses IP privées fluctuantes pour des applications qui doivent être hautement disponibles. Pour y remédier, vous pouvez utiliser les fonctions de reconnaissance de service Kubernetes intégrées et exposer les applications sous forme de services IP de cluster sur le réseau privé. Un service Kubernetes regroupe un ensemble de pods et procure une connexion réseau vers ces pods pour d'autres services dans le cluster sans exposer l'adresse IP privée réelle de chaque pod. Lorsque vous créez un service IP de cluster, une adresse IP privée lui est affectée à partir de la plage d'adresses privées 10.10.10.0/24. Comme pour la plage d'adresses privées de pods, n'utilisez pas cette plage d'adresses sur des noeuds qui communiquent avec vos noeuds worker. Cette adresse IP n'est accessible que dans le cluster. Vous ne pouvez pas y accéder depuis Internet. En même temps, une entrée de recherche DNS est créée pour le service et stockée dans le composant kube-dns du cluster. L'entrée DNS contient le nom du service, l'espace de nom dans lequel il a été créé et le lien vers l'adresse IP privée de cluster affectée.

Pour accéder à un pod situé derrière un service IP de cluster, l'application peut utiliser l'adresse IP de cluster privée du service ou envoyer une demande en utilisant le nom du service. Lorsque vous utilisez le nom du service, ce nom est recherché dans le composant kube-dns et la demande est acheminée à l'adresse IP privée de cluster du service. Lorsqu'une demande parvient au service, celui-ci se charge d'envoyer équitablement toutes les demandes aux pods, indépendamment de leurs adresses IP privées et du noeud worker sur lequel ils sont déployés.

Pour plus d'informations sur la création d'un service de type IP de cluster, voir [Services Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services---service-types).

Pour connecter les applications de manière sécurisée dans un cluster Kubernetes à un réseau sur site, voir [Configuration de la connectivité VPN](cs_vpn.html#vpn). Pour exposer vos applications en vue de la communication avec un réseau externe, voir [Autorisation d'accès public aux applications](cs_network_planning.html#public_access).


<br />


## Approbation de cluster
{: cs_trust}

Par défaut, {{site.data.keyword.containerlong_notm}} fournit de nombreuses [fonctions pour les composants de votre cluster](#cluster) qui vous permettent de déployer vos applications conteneurisées dans un environnement où la sécurité est pleinement assurée. Augmentez le niveau de confiance de votre cluster pour vous assurer que ce qui se passe dans votre cluster correspond bien à ce que vous aviez prévu. Vous pouvez mettre en oeuvre la fonction de confiance dans votre cluster de plusieurs manières, comme illustré dans le diagramme suivant.
{:shortdesc}

![Déploiement de conteneurs avec contenu sécurisé](images/trusted_story.png)

1.  **{{site.data.keyword.containerlong_notm}} avec calcul sécurisé** : sur des clusters bare metal clusters, vous pouvez activer la fonction de confiance. L'agent de confiance surveille le processus de démarrage et signale tout changement de sorte que vous puissiez vérifier que vos noeuds worker bare metal ne font pas l'objet de falsification. Avec la fonction Calcul sécurisé, vous pouvez déployer vos conteneurs sur des hôtes bare metal vérifiés de sorte que vos charges de travail s'exécutent sur du matériel sécurisé. Notez que certaines machines bare metal, telles que les processeurs graphiques (GPU), ne prennent pas en charge la fonction de calcul sécurisé. [En savoir plus sur le fonctionnement de la fonction Calcul sécurisé](#trusted_compute).

2.  **Approbation de contenu pour vos images** : garantissez l'intégrité de vos images en activant l'approbation de contenu dans votre registre {{site.data.keyword.registryshort_notm}}. Avec un contenu sécurisé, vous pouvez vérifier que les personnes qui peuvent signer les images sont dignes de confiance. Lorsque les signataires de confiance envoient des images par commande push dans votre registre, les utilisateurs peuvent extraire le contenu signé de sorte à pouvoir vérifier la source de l'image. Pour plus d'informations, voir la rubrique sur la [signature d'images pour contenu sécurisé](/docs/services/Registry/registry_trusted_content.html#registry_trustedcontent).

3.  **Container Image Security Enforcement (bêta)** : créez un contrôleur d'admission avec des règles personnalisées de sorte à pouvoir vérifier les images de conteneur avant de les déployer. Avec Container Image Security Enforcement, vous contrôlez l'origine de déploiement des images et vérifiez si elles respectent les [règles de Vulnerability Advisor](/docs/services/va/va_index.html) ou les exigences en matière d'[approbation de contenu](/docs/services/Registry/registry_trusted_content.html#registry_trustedcontent). Si un déploiement n'est pas conforme aux règles que vous avez définies, Security Enforcement empêche les modifications sur votre cluster. Pour plus d'informations, voir [Container Image Security Enforcement (bêta)](/docs/services/Registry/registry_security_enforce.html#security_enforce).

4.  **Scanner de vulnérabilité de contenu** : par défaut, Vulnerability Advisor scanne les images stockées dans {{site.data.keyword.registryshort_notm}}. Pour vérifier le statut des conteneurs de production qui s'exécutent dans votre cluster, vous pouvez installer le scanner de conteneur. Pour plus d'informations, voir [Installation du scanner de conteneur](/docs/services/va/va_index.html#va_install_livescan).

5.  **Analyse réseau avec Security Advisor (aperçu)** : avec {{site.data.keyword.Bluemix_notm}} Security Advisor, vous pouvez centraliser les informations relatives à la sécurité à partir de services {{site.data.keyword.Bluemix_notm}}, tels que Vulnerability Advisor et {{site.data.keyword.cloudcerts_short}}. Lorsque vous activez Security Advisor dans votre cluster, vous pouvez afficher des rapports sur le trafic réseau entrant et sortant qui vous paraît suspect. Pour plus d'informations, voir [Analyse réseau](/docs/services/security-advisor/network-analytics.html#network-analytics). Pour effectuer l'installation, voir [Configuration de la surveillance des adresses IP de serveur et des clients suspects pour un cluster Kubernetes](/docs/services/security-advisor/setup_cluster.html).

6.  **{{site.data.keyword.cloudcerts_long_notm}} (bêta)** : si vous disposez d'un cluster dans la région du Sud des Etats-Unis et souhaitez [exposer votre application en utilisant un domaine personnalisé avec TLS](https://console.bluemix.net/docs/containers/cs_ingress.html#ingress_expose_public), vous pouvez stocker votre certificat TLS dans {{site.data.keyword.cloudcerts_short}}. Les certificats arrivés à expiration ou sur le point d'arriver à expiration peuvent également être indiqués dans votre tableau de bord Security Advisor. Pour plus d'informations, voir [Initiation à {{site.data.keyword.cloudcerts_short}}](/docs/services/certificate-manager/index.html#gettingstarted).

<br />


## Stockage d'informations personnelles
{: #pi}

Vous êtes chargé d'assurer la sécurité de vos informations personnelles dans les ressources et les images de conteneur Kubernetes. Ces informations comprennent vos nom, adresse, numéro de téléphone, adresse e-mail ou tout autre information permettant d'identifier, de contacter ou de localiser vous, vos clients ou d'autres personnes.
{: shortdesc}

<dl>
  <dt>Utilisez une valeur confidentielle (secret) Kubernetes pour stocker vos informations personnelles</dt>
  <dd>Ne stockez les informations personnelles que dans les ressources Kubernetes conçues pour contenir des informations personnelles. Par exemple, n'utilisez pas votre nom dans le nom d'un espace de nom, d'un déploiement ou d'un fichier configmap de Kubernetes. Pour une protection et un chiffrement appropriés, stockez plutôt les informations personnelles dans des <a href="cs_app.html#secrets">valeurs confidentielles Kubernetes</a>.</dd>

  <dt>Utilisez une valeur confidentielle Kubernetes `imagePullSecret` pour stocker les données d'identification de registre d'images</dt>
  <dd>Ne stockez pas d'informations personnelles dans des images de conteneur ou des espaces de nom de registre. Pour une protection et un chiffrement appropriés, stockez plutôt les données d'identification de registre dans des valeurs <a href="cs_images.html#other">Kubernetes imagePullSecrets</a> et d'autres informations personnelles dans des <a href="cs_app.html#secrets">valeurs confidentielles (secrets) Kubernetes</a>. Notez que si des informations personnelles sont stockées dans une couche précédente d'image, la suppression d'une image ne suffit pas forcément pour supprimer ces informations personnelles.</dd>
  </dl>

<br />






