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



# Affectation d'accès au cluster
{: #users}

En tant qu'administrateur de cluster, vous pouvez définir des règles d'accès à votre cluster Kubernetes pour créer différents niveaux d'accès pour différents utilisateurs. Par exemple, vous pouvez autoriser certains utilisateurs à travailler avec les ressources du cluster et en autoriser d'autres à déployer uniquement des conteneurs.
{: shortdesc}


## Planification des demandes d'accès
{: #planning_access}

En tant qu'administrateur de cluster, il peut s'avérer difficile de suivre les demandes d'accès. La mise en place d'un modèle de communication pour les demandes d'accès est essentielle pour assurer la sécurité de votre cluster.
{: shortdesc}

Afin de garantir que les bonnes personnes disposent de l'accès approprié, soyez clair en ce qui concerne vos règles de demande d'accès ou d'obtention d'aide pour effectuer les tâches courantes.

Il est possible que vous ayez déjà recours à une méthode qui fonctionne bien pour votre équipe, et c'est extra ! A défaut, envisagez d'utiliser l'une des méthodes suivantes.

*  Créer un système de tickets
*  Créer un modèle de formulaire
*  Créer une page wiki
*  Nécessiter une demande par e-mail
*  Utiliser le système de suivi des incidents auquel vous avez déjà recours pour suivre le travail quotidien de votre équipe

Vous vous sentez dépassé ? Suivez ce tutoriel consacré aux [meilleures pratiques en matière d'organisation des utilisateurs, des équipes et des applications](/docs/tutorials/users-teams-applications.html).
{: tip}

## Règles et droits d'accès
{: #access_policies}

La portée d'une règle d'accès s'appuie sur un ou plusieurs rôles définis pour les utilisateurs qui déterminent les actions qu'ils sont autorisés à effectuer. Vous pouvez définir des règles spécifiques au cluster, à l'infrastructure, aux instances de service dont vous disposez ou aux rôles Cloud Foundry.
{: shortdesc}

Vous devez définir des règles d'accès pour tous les utilisateurs qui utilisent {{site.data.keyword.containershort_notm}}. Certaines règles sont prédéfinies, mais d'autres peuvent être personnalisées. Vérifiez l'image et les définitions suivantes pour voir quels sont les rôles adaptés aux tâches courantes des utilisateurs et identifier là où vous souhaitez personnaliser une règle.

![Rôles d'accès {{site.data.keyword.containershort_notm}}](/images/user-policies.png)

Figure. Rôles d'accès {{site.data.keyword.containershort_notm}}

<dl>
  <dt>Règles IAM (Identity and Access Management)</dt>
    <dd><p><strong>Plateforme</strong> : vous pouvez déterminer les actions que peuvent effectuer des personnes sur un cluster {{site.data.keyword.containershort_notm}}. Vous pouvez définir ces règles par région. Exemples d'actions : créer ou retirer des clusters, ou ajouter des noeuds worker supplémentaires. Ces règles doivent être définies avec des règles d'infrastructure.</p>
    <p><strong>Infrastructure</strong> : vous pouvez déterminer les niveaux d'accès à votre infrastructure par exemple, les machines de noeud de cluster, les réseaux ou les ressources de stockage. La même règle s'applique que l'utilisateur fasse la demande à partir de l'interface graphique {{site.data.keyword.containershort_notm}} ou de l'interface de ligne de commande (CLI), et ce, même si les actions sont exécutées dans l'infrastructure IBM Cloud (SoftLayer). Vous devez définir ce type de règle parallèlement aux règles d'accès à la plateforme {{site.data.keyword.containershort_notm}}. Pour en savoir plus sur les rôles disponibles, consultez la rubrique [Droits Infrastructure](/docs/iam/infrastructureaccess.html#infrapermission).</p> </br></br><strong>Remarque :</strong> Assurez-vous que votre compte {{site.data.keyword.Bluemix_notm}} est [configuré avec l'accès au portefeuille d'infrastructure IBM Cloud (SoftLayer)](cs_troubleshoot_clusters.html#cs_credentials) pour que les utilisateurs autorisés puissent effectuer des actions dans le compte d'infrastructure IBM Cloud (SoftLayer) en fonction des droits qui leur sont affectés. </dd>
  <dt>Rôles RBAC (Resource Based Access Control) Kubernetes</dt>
    <dd>Tous les utilisateurs auxquels a été affectée une règle d'accès Plateforme ont un rôle Kubernetes qui leur est automatiquement affecté. Dans Kubernetes, [RBAC (Role Based Access Control) ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview) détermine les actions possibles d'un utilisateur sur les ressources d'un cluster. <strong>Remarque</strong> : Les rôles RBAC sont automatiquement configurés pour l'espace de nom <code>default</code>, mais en tant qu'administrateur de cluster, vous pouvez affecter des rôles pour d'autres espaces de nom.</dd>
  <dt>Cloud Foundry</dt>
    <dd>Certains services ne peuvent pas être gérés par Cloud IAM. Si vous utilisez l'un de ces services, vous pouvez continuer à utiliser les [rôles d'utilisateur Cloud Foundry](/docs/iam/cfaccess.html#cfaccess) pour contrôler l'accès à vos services.</dd>
</dl>


Rétromigration de droits ? Cette action peut prendre quelques minutes.
{: tip}

### Rôles de plateforme
{: #platform_roles}

{{site.data.keyword.containershort_notm}} est configuré pour utiliser les rôles de plateforme {{site.data.keyword.Bluemix_notm}}. Les droits des rôles se superposent, ce qui signifie que le rôle d'éditeur (`Editor`) dispose des mêmes droits que le rôle d'afficheur (`Viewer`) avec en plus les droits octroyés à un éditeur. Le tableau suivant présente les types d'actions que chaque rôle peut effectuer.

Les rôles RBAC correspondants sont automatiquement affectés à l'espace de nom par défaut lorsque vous affectez un rôle de plateforme. Si vous modifiez le rôle de plateforme d'un utilisateur, le rôle RBAC est également mis à jour.
{: tip}

<table>
<caption>Rôles de plateforme et actions</caption>
  <tr>
    <th>Rôles de plateforme</th>
    <th>Exemples d'actions</th>
    <th>Rôle RBAC correspondant</th>
  </tr>
  <tr>
      <td>Afficheur</td>
      <td>Afficher les détails d'un cluster ou d'autres instances de service.</td>
      <td>View</td>
  </tr>
  <tr>
    <td>Editeur</td>
    <td>Possibilité d'associer un service IBM Cloud à un cluster ou de le dissocier, ou de créer un webhook. <strong>Remarque</strong> : pour lier des services, vous devez disposer du rôle de développeur Cloud Foundry.</td>
    <td>Edit</td>
  </tr>
  <tr>
    <td>Opérateur</td>
    <td>Possibilité de créer, retirer, réamorcer ou recharger un noeud worker, ainsi que d'ajouter un sous-réseau à un cluster.</td>
    <td>Admin</td>
  </tr>
  <tr>
    <td>Administrateur</td>
    <td>Possibilité de créer et retirer des clusters, ainsi que modifier des règles d'accès pour d'autres personnes au niveau du compte pour le service et l'infrastructure. <strong>Remarque</strong> : l'accès Admin peut être affecté à un cluster spécifique ou à toutes les instances du service de votre compte. Pour supprimer des clusters, vous devez disposer de l'accès admin pour le cluster que vous voulez supprimer. Pour créer des clusters, vous devez disposer du rôle admin pour toutes les instances du service.</td>
    <td>Cluster-admin</td>
  </tr>
</table>

Pour plus d'informations sur l'affectation de rôles utilisateur dans l'interface utilisateur, voir [Gestion d'accès IAM](/docs/iam/mngiam.html#iammanidaccser).


### Rôles d'infrastructure
{: #infrastructure_roles}

Les rôles d'infrastructure autorisent les utilisateurs à effectuer des tâches sur les ressources au niveau de l'infrastructure. Le tableau suivant présente les types d'actions que chaque rôle peut effectuer. Les rôles d'infrastructure sont personnalisables. Assurez-vous d'octroyer aux utilisateurs uniquement l'accès dont ils ont besoin pour effectuer leur travail.

En plus de l'octroi de rôles d'infrastructure spécifiques, vous devez également octroyer l'accès aux unités aux utilisateurs qui travaille avec l'infrastructure.
{: tip}

<table>
<caption>Rôles d'infrastructure et actions</caption>
  <tr>
    <th>Rôle d'infrastructure</th>
    <th>Exemples d'actions</th>
  </tr>
  <tr>
    <td><i>Affichage uniquement</i></td>
    <td>Possibilité d'afficher les détails de l'infrastructure et d'obtenir le récapitulatif d'un compte, y compris les factures et les paiements</td>
  </tr>
  <tr>
    <td><i>Utilisateur de base</i></td>
    <td>Possibilité d'éditer des configurations de service, notamment les adresses IP, d'ajouter ou modifier des enregistrements DNS et d'ajouter de nouveaux utilisateurs avec accès à l'infrastructure</td>
  </tr>
  <tr>
    <td><i>Superutilisateur</i></td>
    <td>Possibilité d'effectuer toutes les actions associées à l'infrastructure</td>
  </tr>
</table>

Pour commencer à affecter des rôles, suivez les étapes indiquées dans la rubrique [Personnalisation des droits Infrastructure pour un utilisateur](#infra_access).

### Rôles RBAC
{: #rbac_roles}

Le mécanisme de contrôle d'accès basé sur des rôles (RBAC) est une méthode de sécurisation des ressources figurant dans votre cluster qui vous permet de décider qui peut effectuer des actions Kubernetes. Dans le tableau suivant, vous pouvez voir les types de rôle RBAC et les types d'action possibles pour les utilisateurs disposant de ces rôles. Les droits se superposent,ce qui signifie que le rôle `Admin` contient toutes les règles qui vont avec les rôles `View` et `Edit`. Assurez-vous de donner aux utilisateurs uniquement l'accès dont ils ont besoin.

Les rôles RBAC sont automatiquement définis en conjonction avec le rôle de plateforme pour l'espace de nom par défaut. [Vous pouvez mettre à jour le rôle, ou affecter des rôles à d'autres espaces de nom](#rbac).
{: tip}

<table>
<caption>Rôles RBAC et actions</caption>
  <tr>
    <th>Rôle RBAC</th>
    <th>Exemples d'actions</th>
  </tr>
  <tr>
    <td>View</td>
    <td>Possibilité d'afficher les ressources figurant dans l'espace de nom par défaut. Les personnes disposant de ce rôle ne peuvent pas visualiser les valeurs confidentielles de Kubernetes. </td>
  </tr>
  <tr>
    <td>Edit</td>
    <td>Possibilité de lecture/écriture dans les ressources figurant dans l'espace de nom par défaut.</td>
  </tr>
  <tr>
    <td>Admin</td>
    <td>Possibilité de lecture/écriture dans les ressources figurant dans l'espace de nom par défaut mais pas dans l'espace de nom proprement dit. Permet de créer des rôles dans un espace de nom.</td>
  </tr>
  <tr>
    <td>Cluster admin</td>
    <td>Possibilité de lecture/écriture dans les ressources figurant dans tous les espaces de nom. Permet de créer des rôles dans un espace de nom. Permet l'accès au tableau de bord Kubernetes. Possibilité de créer une ressource Ingress qui rend les applications accessibles au public.</td>
  </tr>
</table>

<br />


## Ajout d'utilisateurs à un compte {{site.data.keyword.Bluemix_notm}}
{: #add_users}

Vous pouvez ajouter des utilisateurs à un compte {{site.data.keyword.Bluemix_notm}} pour leur accorder un accès à vos clusters.
{:shortdesc}

Avant de commencer, vérifiez que vous bénéficiez du rôle `Manager` de Cloud Foundry pour un compte {{site.data.keyword.Bluemix_notm}}.

1.  [Ajoutez l'utilisateur au compte](../iam/iamuserinv.html#iamuserinv).
2.  Dans la section **Accès**, développez **Services**.
3.  Affectez un rôle de plateforme à un utilisateur pour définir l'accès pour {{site.data.keyword.containershort_notm}}.
      1. Dans la liste déroulante **Services**, sélectionnez **{{site.data.keyword.containershort_notm}}**.
      2. Dans la liste déroulante **Région**, sélectionnez celle où inviter l'utilisateur.
      3. Dans la liste déroulante **Instance de service**, sélectionnez le cluster où inviter l'utilisateur. Pour identifier l'ID d'un cluster spécifique, exécutez la commande `bx cs clusters`.
      4. Dans la section **Sélectionnez des rôles**, choisissez un rôle. Pour consulter la liste des actions prises en charge pour chaque rôle, voir [Règles et droits d'accès](#access_policies).
4. [Facultatif : affectez un rôle Cloud Foundry](/docs/iam/mngcf.html#mngcf).
5. [Facultatif : affectez un rôle Infrastructure](/docs/iam/infrastructureaccess.html#infrapermission).
6. Cliquez sur **Inviter des utilisateurs**.

<br />


## Description de la clé d'API IAM et de la commande `bx cs credentials-set`
{: #api_key}

Pour mettre à disposition et travailler avec des clusters dans votre compte, vous devez vérifier que vous avez un compte défini correctement pour accéder au portefeuille d'infrastructure IBM Cloud (SoftLayer). Selon la configuration de votre compte, vous pouvez utiliser la clé d'API IAM ou les données d'identification d'Infrastructure que vous avez définies manuellement avec la commande `bx cs credentials-set`.

<dl>
  <dt>Clé d'API IAM</dt>
    <dd><p>La clé d'API IAM (Identity and Access Management) est définie automatiquement pour une région lorsque la première action qui nécessite la politique de contrôle d'accès admin {{site.data.keyword.containershort_notm}} est effectuée. Par exemple, supposons que l'un de vos administrateurs crée le premier cluster dans la région <code>us-south</code>. Pour cette opération, la clé d'API IAM de cet utilisateur est stockée dans le compte correspondant à cette région. La clé d'API est utilisée pour commander l'infrastructure IBM Cloud (SoftLayer), par exemple de nouveaux noeuds worker ou réseaux locaux virtuels (VLAN).</p> <p>Lorsqu'un autre utilisateur effectue une action qui nécessite une interaction avec le portefeuille d'infrastructure IBM Cloud (SoftLayer) dans cette région, par exemple la création d'un nouveau cluster ou le rechargement d'un noeud worker, la clé d'API stockée est utilisée pour déterminer s'il existe des droits suffisants pour effectuer cette action. Pour vous assurer que les actions liées à l'infrastructure dans votre cluster peuvent être effectuées sans problème, affectez à vos administrateurs la politique d'accès de l'infrastructure {{site.data.keyword.containershort_notm}} <strong>Superutilisateur</strong>.</p>
    <p>Vous pouvez trouver le propriétaire actuel de la clé d'API en exécutant la commande [<code>bx cs api-key-info</code>](cs_cli_reference.html#cs_api_key_info). Si vous constatez que la clé d'API stockée pour une région nécessite une mise à jour, vous pouvez le faire en exécutant la commande [<code>bx cs api-key-reset</code>](cs_cli_reference.html#cs_api_key_reset). Cette commande nécessite la politique d'accès admin {{site.data.keyword.containershort_notm}} et stocke la clé d'API de l'utilisateur qui exécute cette commande dans le compte.</p>
    <p><strong>Remarque :</strong> la clé d'API stockée pour la région ne peut pas être utilisée si les données d'identification de l'infrastructure IBM Cloud (SoftLayer) ont été définies manuellement à l'aide de la commande <code>bx cs credentials-set</code>.</p></dd>
  <dt>Données d'identification de l'infrastructure IBM Cloud (SoftLayer) via <code>bx cs credentials-set</code></dt>
    <dd><p>Si vous disposez d'un compte de paiement à la carte {{site.data.keyword.Bluemix_notm}}, vous avez accès au portefeuille d'infrastructure IBM Cloud (SoftLayer) par défaut. Cependant, vous pouvez utiliser un autre compte d'infrastructure IBM Cloud (SoftLayer) dont vous disposez déjà pour commander l'infrastructure. Vous pouvez lier le compte de cette infrastructure à votre compte {{site.data.keyword.Bluemix_notm}} en utilisant la commande [<code>bx cs credentials-set</code>](cs_cli_reference.html#cs_credentials_set).</p>
    <p>Si les données d'identification de l'infrastructure IBM Cloud (SoftLayer) sont définies manuellement, ces données sont utilisées pour commander l'infrastructure, même s'il existe une clé d'API IAM pour ce compte. Si l'utilisateur dont les données d'identification sont stockées ne dispose pas des droits requis pour commander l'infrastructure, les actions liées à l'infrastructure, par exemple la création d'un cluster ou le rechargement d'un noeud worker risquent d'échouer.</p>
    <p>Pour supprimer les données d'identification de l'infrastructure IBM Cloud (SoftLayer) définies manuellement, vous pouvez utiliser la commande [<code>bx cs credentials-unset</code>](cs_cli_reference.html#cs_credentials_unset). Une fois ces données supprimées, la clé d'API IAM est utilisée pour commander l'infrastructure.</p></dd>
</dl>

<br />


## Personnalisation des droits Infrastructure pour un utilisateur
{: #infra_access}

Lorsque vous définissez des règles d'infrastructure dans Identity and Access Management, des droits associés à un rôle sont attribués à un utilisateur. Pour personnaliser ces droits, vous devez vous connecter à l'infrastructure IBM Cloud (SoftLayer) et adapter ces droits.
{: #view_access}

Par exemple, les **utilisateurs de base** peuvent redémarrer un noeud worker, mais ne peuvent pas recharger un noeud worker. Sans attribuer à cette personne des droits **Superutilisateur**, vous pouvez adapter les droits sur l'infrastructure IBM Cloud (SoftLayer) et autoriser l'utilisateur à exécuter une commande de rechargement.

1.  Connectez-vous à votre [compte {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/), puis sélectionnez **Infrastructure** dans le menu.

2.  Accédez à **Compte** > **Utilisateurs** > **Liste d'utilisateurs**.

3.  Pour modifier les droits, sélectionnez un nom de profil utilisateur ou la colonne **Accès à l'unité**.

4.  Dans l'onglet **Autorisations du portail**, personnalisez l'accès de l'utilisateur. Les droits dont les utilisateurs ont besoin dépendent des ressources d'infrastructure qu'ils doivent utiliser :

    * Utilisez la liste déroulante **Droits rapides** pour affecter le rôle **Superutilisateur** qui octroie tous les droits à l'utilisateur.
    * Utilisez la liste déroulante **Droits rapides** pour affecter le rôle **Utilisateur de base** qui octroie à l'utilisateur certains droits, mais pas la totalité.
    * Si vous ne voulez pas octroyer tous les droits avec le rôle **Superutilisateur** ou si vous devez ajouter des droits au-delà du rôle **Utilisateur de base**, consultez le tableau suivant qui décrit les droits nécessaires pour exécuter des tâches courantes dans {{site.data.keyword.containershort_notm}}.

    <table summary="Droits Infrastructure pour les scénarios {{site.data.keyword.containershort_notm}} courants.">
     <caption>Droits Infrastructure souvent requis pour {{site.data.keyword.containershort_notm}}</caption>
     <thead>
      <th>Tâches courantes dans {{site.data.keyword.containershort_notm}}</th>
      <th>Droits Infrastructure requis par onglet</th>
     </thead>
     <tbody>
       <tr>
         <td><strong>Droits minimum</strong> : <ul><li>Créer un cluster.</li></ul></td>
         <td><strong>Equipements</strong> :<ul><li>Afficher les détails du serveur virtuel</li><li>Réamorcer le serveur et afficher les informations système IPMI (Intelligent Platform Management Interface)</li><li>Emettre les rechargements de système d'exploitation et lancer le noyau de secours</li></ul><strong>Compte</strong> : <ul><li>Ajouter/mettre à niveau des instances cloud</li><li>Ajouter un serveur</li></ul></td>
       </tr>
       <tr>
         <td><strong>Administration de clusters</strong> : <ul><li>Créer, mettre à jour et supprimer des clusters.</li><li>Ajouter, recharger et redémarrer les noeuds worker.</li><li>Afficher les réseaux locaux virtuels (VLAN).</li><li>Créer des sous-réseaux.</li><li>Déployer des pods et charger des services d'équilibreur de charge.</li></ul></td>
         <td><strong>Support</strong> :<ul><li>Afficher les tickets</li><li>Ajouter des tickets</li><li>Editer des tickets</li></ul>
         <strong>Equipements</strong> :<ul><li>Afficher les détails du serveur virtuel</li><li>Réamorcer le serveur et afficher les informations système IPMI (Intelligent Platform Management Interface)</li><li>Mettre à niveau le serveur</li><li>Emettre les rechargements de système d'exploitation et lancer le noyau de secours</li></ul>
         <strong>Services</strong> :<ul><li>Gérer les clés SSH</li></ul>
         <strong>Compte</strong> :<ul><li>Afficher le récapitulatif du compte</li><li>Ajouter/mettre à niveau des instances cloud</li><li>Annuler le serveur</li><li>Ajouter un serveur</li></ul></td>
       </tr>
       <tr>
         <td><strong>Stockage</strong> : <ul><li>Créer des réservations de volumes persistants pour mettre à disposition des volumes persistants.</li><li>Créer et gérer des ressources d'infrastructure de stockage.</li></ul></td>
         <td><strong>Services</strong> :<ul><li>Gérer le stockage</li></ul><strong>Compte</strong> :<ul><li>Ajouter du stockage</li></ul></td>
       </tr>
       <tr>
         <td><strong>Réseau privé</strong> : <ul><li>Gérer les VLAN privés pour la mise en réseau au sein d'un cluster.</li><li>Configurer la connectivité VPN pour les réseaux privés.</li></ul></td>
         <td><strong>Réseau</strong> :<ul><li>Gérer les routes de sous-réseau du réseau</li><li>Gérer les tunnels réseau IPSEC</li><li>Gérer les passerelles réseau</li><li>Administration VPN</li></ul></td>
       </tr>
       <tr>
         <td><strong>Réseau public</strong> :<ul><li>Configurer un équilibreur de charge public ou un réseau Ingress pour exposer des applications.</li></ul></td>
         <td><strong>Equipements</strong> :<ul><li>Gérer les équilibreurs de charge</li><li>Editer le nom d'hôte/domaine</li><li>Gérer le contrôle de port</li></ul>
         <strong>Réseau</strong> :<ul><li>Ajouter la fonction de calcul avec le port réseau public</li><li>Gérer les routes de sous-réseau du réseau</li><li>Ajouter des adresses IP</li></ul>
         <strong>Services</strong> :<ul><li>Gérer DNS, RDNS et WHOIS</li><li>Afficher les certificats (SSL)</li><li>Gérer les certificats (SSL)</li></ul></td>
       </tr>
     </tbody>
    </table>

5.  Pour enregistrer vos modifications, cliquez sur **Modifier droits pour le portail**.

6.  Dans l'onglet **Accès à l'unité**, sélectionnez les unités auxquelles accorder l'accès.

    * Dans la liste déroulante **Type d'unité**, vous pouvez octroyer l'accès à **Tous les serveurs virtuels**.
    * Pour autoriser l'accès utilisateur aux nouvelles unités créées, sélectionnez **Accorder l'accès automatiquement lors de l'ajout de nouveaux appareils**.
    * Pour enregistrer vos modifications, cliquez sur **Mettre à jour l'accès à l'unité**.

<br />


## Autorisation des utilisateurs avec des droits RBAC Kubernetes personnalisés
{: #rbac}

Les règles d'accès d'{{site.data.keyword.containershort_notm}} correspondent à certains rôles RBAC (Role-Based Access Control) de Kubernetes comme indiqué à la section [Règles et droits d'accès](#access_policies). Pour autoriser d'autres rôles Kubernetes qui diffèrent de la règle d'accès correspondante, vous pouvez personnaliser les rôles RBAC, puis affecter ces rôles à des personnes ou à des groupes d'utilisateurs.
{: shortdesc}

Par exemple, vous pouvez envisager d'octroyer des droits à une équipe de développeurs pour qu'ils travaillent sur un groupe d'API spécifique ou avec des ressources au sein d'un espace de nom Kubernetes dans le cluster, mais pas sur l'ensemble du cluster. Vous créez alors un rôle, puis associez ce rôle à des utilisateurs, en utilisant un nom d'utilisateur unique à {{site.data.keyword.containershort_notm}}. Pour plus d'informations, voir [Using RBAC Authorization ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview) dans la documentation Kubernetes.

Avant de commencer, [ciblez l'interface CLI de Kubernetes sur le cluster](cs_cli_install.html#cs_cli_configure).

1.  Créez le rôle avec l'accès que vous souhaitez affecter.

    1. Créez un fichier `.yaml` pour définir le rôle avec l'accès que vous souhaitez affecter.

        ```
        kind: Role
        apiVersion: rbac.authorization.k8s.io/v1
        metadata:
          namespace: default
          name: my_role
        rules:
        - apiGroups: [""]
          resources: ["pods"]
          verbs: ["get", "watch", "list"]
        - apiGroups: ["apps", "extensions"]
          resources: ["daemonsets", "deployments"]
          verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
        ```
        {: codeblock}

        <table>
        <caption> Description des composants de ce fichier YAML</caption>
          <thead>
            <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de ce fichier YAML</th>
          </thead>
          <tbody>
            <tr>
              <td><code>kind</code></td>
              <td>Indiquez `Role` pour octroyer l'accès aux ressources au sein d'un seul espace de nom, ou `ClusterRole` pour l'accès aux ressources sur l'ensemble du cluster.</td>
            </tr>
            <tr>
              <td><code>apiVersion</code></td>
              <td><ul><li>Pour les clusters exécutant Kubernetes 1.8 ou version ultérieure, utilisez `rbac.authorization.k8s.io/v1`. </li><li>Pour les versions antérieures, utilisez `apiVersion: rbac.authorization.k8s.io/v1beta1`.</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/namespace</code></td>
              <td><ul><li>Pour kind: `Role` : Indiquez l'espace de nom Kubernetes auquel l'accès est octroyé.</li><li>N'utilisez pas la zone `namespace` si vous créez un rôle `ClusterRole` qui s'applique au niveau du cluster.</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/name</code></td>
              <td>Attribuez un nom au rôle et utilisez ce nom par la suite lorsque vous liez le rôle.</td>
            </tr>
            <tr>
              <td><code>rules/apiGroups</code></td>
              <td><ul><li>Indiquez les groupes d'API Kubernetes avec lesquels vous voulez que les utilisateurs puissent interagir, par exemple `"apps"`, `"batch"` ou `"extensions"`. </li><li>Pour accéder au groupe d'API principal sur le chemin REST `api/v1`, laissez le groupe vide : `[""]`.</li><li>Pour plus d'informations, voir [API groups![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://v1-9.docs.kubernetes.io/docs/reference/api-overview/#api-groups) dans la documentation Kubernetes.</li></ul></td>
            </tr>
            <tr>
              <td><code>rules/resources</code></td>
              <td><ul><li>Indiquez les ressources Kubernetes auxquelles vous souhaitez octroyer l'accès, par exemple `"daemonsets"`, `"deployments"`, `"events"` ou `"ingresses"`.</li><li>Si vous indiquez `"nodes"`, le rôle dans la zone kind doit être `ClusterRole`.</li><li>Pour obtenir la liste des ressources, consultez le tableau intitulé [Resource types![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/reference/kubectl/cheatsheet/) sur la page Kubernetes Cheat Sheet.</li></ul></td>
            </tr>
            <tr>
              <td><code>rules/verbs</code></td>
              <td><ul><li>Indiquez les types d'action que vous souhaitez que les utilisateurs soient en mesure d'effectuer, par exemple `"get"`, `"list"`, `"describe"`, `"create"` ou `"delete"`. </li><li>Pour obtenir la liste complète des verbes, voir la [documentation `kubectl` ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/reference/kubectl/overview/).</li></ul></td>
            </tr>
          </tbody>
        </table>

    2.  Créez le rôle dans votre cluster.

        ```
        kubectl apply -f <path_to_yaml_file>
        ```
        {: pre}

    3.  Vérifiez que le rôle est créé.

        ```
        kubectl get roles
        ```
        {: pre}

2.  Associez des utilisateurs au rôle.

    1. Créez un fichier `.yaml` pour associer les utilisateurs au rôle que vous avez créé. Notez l'unique URL à utiliser pour chaque nom dans 'subjects'.

        ```
        kind: RoleBinding
        apiVersion: rbac.authorization.k8s.io/v1
        metadata:
          name: my_role_team1
          namespace: default
        subjects:
        - kind: User
          name: https://iam.ng.bluemix.net/kubernetes#user1@example.com
          apiGroup: rbac.authorization.k8s.io
        - kind: User
          name: https://iam.ng.bluemix.net/kubernetes#user2@example.com
          apiGroup: rbac.authorization.k8s.io
        roleRef:
          kind: Role
          name: custom-rbac-test
          apiGroup: rbac.authorization.k8s.io
        ```
        {: codeblock}

        <table>
        <caption>Description des composants de ce fichier YAML</caption>
          <thead>
            <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de ce fichier YAML</th>
          </thead>
          <tbody>
            <tr>
              <td><code>kind</code></td>
              <td>Pour `kind`, indiquez `RoleBinding` pour les deux types de fichiers `.yaml` de rôle : `Role` (espace de nom) et `ClusterRole` (ensemble du cluster).</td>
            </tr>
            <tr>
              <td><code>apiVersion</code></td>
              <td><ul><li>Pour les clusters exécutant Kubernetes 1.8 ou version ultérieure, utilisez `rbac.authorization.k8s.io/v1`. </li><li>Pour les versions antérieures, utilisez `apiVersion: rbac.authorization.k8s.io/v1beta1`.</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/namespace</code></td>
              <td><ul><li>Pour kind: `Role` : Indiquez l'espace de nom Kubernetes auquel l'accès est octroyé.</li><li>N'utilisez pas la zone `namespace` si vous créez un rôle `ClusterRole` qui s'applique au niveau du cluster.</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/name</code></td>
              <td>Indiquez le nom de liaison du rôle.</td>
            </tr>
            <tr>
              <td><code>subjects/kind</code></td>
              <td>Pour kind, indiquez `User`.</td>
            </tr>
            <tr>
              <td><code>subjects/name</code></td>
              <td><ul><li>Ajoutez l'adresse e-mail de l'utilisateur à l'URL suivante : `https://iam.ng.bluemix.net/kubernetes#`.</li><li>Par exemple, `https://iam.ng.bluemix.net/kubernetes#user1@example.com`</li></ul></td>
            </tr>
            <tr>
              <td><code>subjects/apiGroup</code></td>
              <td>Utilisez `rbac.authorization.k8s.io`.</td>
            </tr>
            <tr>
              <td><code>roleRef/kind</code></td>
              <td>Entrez la même valeur que pour `kind` dans le fichier `.yaml` du rôle : `Role` ou `ClusterRole`.</td>
            </tr>
            <tr>
              <td><code>roleRef/name</code></td>
              <td>Entrez le nom du fichier `.yaml` du rôle.</td>
            </tr>
            <tr>
              <td><code>roleRef/apiGroup</code></td>
              <td>Utilisez `rbac.authorization.k8s.io`.</td>
            </tr>
          </tbody>
        </table>

    2. Créez la ressource de liaison de rôle (rolebinding) dans votre cluster.

        ```
        kubectl apply -f filepath/my_role_team1.yaml
        ```
        {: pre}

    3.  Vérifiez que la liaison est créée.

        ```
        kubectl get rolebinding
        ```
        {: pre}

Vous venez de créer et de lier un rôle RBAC Kubernetes personnalisé, effectuez à présent le suivi auprès des utilisateurs. Demandez-leur de tester une action qu'ils ont le droit d'effectuer en fonction du rôle, par exemple, supprimer un pod.
