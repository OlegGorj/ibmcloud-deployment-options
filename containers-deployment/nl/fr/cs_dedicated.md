---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-24"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}



# Initiation aux clusters dans {{site.data.keyword.Bluemix_dedicated_notm}}
{: #dedicated}

Si vous disposez d'un compte {{site.data.keyword.Bluemix_dedicated}}, vous pouvez déployer des clusters Kubernetes dans un environnement de cloud dédié (`https://<my-dedicated-cloud-instance>.bluemix.net`) et vous connecter aux services {{site.data.keyword.Bluemix_notm}} présélectionnés qui s'exécutent également à cet emplacement.
{:shortdesc}

Si vous ne disposez pas d'un compte {{site.data.keyword.Bluemix_dedicated_notm}}, vous pouvez vous [initier à {{site.data.keyword.containershort_notm}}](container_index.html) dans un compte {{site.data.keyword.Bluemix_notm}} public.

## A propos de l'environnement de cloud Dedicated
{: #dedicated_environment}

Avec un compte {{site.data.keyword.Bluemix_dedicated_notm}}, les ressources physiques disponibles sont dédiées exclusivement à votre cluster et ne sont pas partagées avec des clusters d'autres clients {{site.data.keyword.IBM_notm}}. Vous pouvez opter pour la mise en place d'un environnement {{site.data.keyword.Bluemix_dedicated_notm}} si vous voulez un isolement de votre cluster et que vous devez isoler les autres services {{site.data.keyword.Bluemix_notm}} que vous utilisez. Si vous ne disposez pas d'un compte Dedicated, vous pouvez [créer des clusters avec du matériel dédié dans un compte {{site.data.keyword.Bluemix_notm}} public](cs_clusters.html#clusters_ui).

Avec la version {{site.data.keyword.Bluemix_dedicated_notm}}, vous pouvez créer des clusters depuis le catalogue dans la console Dedicated ou à l'aide de l'interface de ligne de commande d'{{site.data.keyword.containershort_notm}}. Pour utiliser la console Dedicated, vous vous connectez simultanément à vos comptes Dedicated et public en utilisant votre IBMid. Vous pouvez utiliser cette double connexion pour accéder à vos clusters publics en utilisant votre console Dedicated. Pour utiliser l'interface de ligne de commande, vous vous connectez via votre noeud final Dedicated (`api.<my-dedicated-cloud-instance>.bluemix.net.`). Vous ciblez ensuite le noeud final d'API {{site.data.keyword.containershort_notm}} de la région publique associée à l'environnement Dedicated.

Les différences les plus significatives entre {{site.data.keyword.Bluemix_notm}} Public et Dedicated sont les suivantes.

*   Dans {{site.data.keyword.Bluemix_dedicated_notm}}, {{site.data.keyword.IBM_notm}} est propriétaire et gère le compte d'infrastructure IBM Cloud (SoftLayer) dans lequel sont déployés les noeuds worker, les réseaux locaux virtuels (VLAN) et les sous-réseaux. Dans {{site.data.keyword.Bluemix_notm}} Public, c'est vous qui êtes propriétaire du compte d'infrastructure IBM Cloud (SoftLayer).
*   Dans {{site.data.keyword.Bluemix_dedicated_notm}}, les spécifications pour les VLAN et les sous-réseaux dans le compte d'infrastructure IBM Cloud (SoftLayer) géré par {{site.data.keyword.IBM_notm}} sont déterminées lorsque l'environnement Dedicated est activé. Dans {{site.data.keyword.Bluemix_notm}} Public, les spécifications pour les VLAN et les sous-réseaux sont déterminées lorsque le cluster est créé.

### Différences en matière de gestion de cluster entre les environnements de cloud
{: #dedicated_env_differences}

<table>
<caption>Différences en matière de gestion de cluster</caption>
<col width="20%">
<col width="40%">
<col width="40%">
 <thead>
 <th>Zone</th>
 <th>{{site.data.keyword.Bluemix_notm}} Public</th>
 <th>{{site.data.keyword.Bluemix_dedicated_notm}}</th>
 </thead>
 <tbody>
 <tr>
 <td>Création de cluster</td>
 <td>Créer un cluster gratuit ou un cluster standard.</td>
 <td>Créer un cluster standard.</td>
 </tr>
 <tr>
 <td>Matériel et propriété du cluster</td>
 <td>Dans les clusters standard, le matériel peut être partagé avec d'autres clients {{site.data.keyword.IBM_notm}} ou vous être dédié exclusivement. Vous êtes propriétaire et gérez les réseaux locaux virtuels (VLAN) publics et privés dans votre compte d'infrastructure IBM Cloud (SoftLayer).</td>
 <td>Dans les clusters sur {{site.data.keyword.Bluemix_dedicated_notm}}, le matériel est toujours dédié. Les réseaux locaux virtuels (VLAN) publics et privés disponibles pour la création du cluster sont prédéfinis lorsque l'environnement {{site.data.keyword.Bluemix_dedicated_notm}} est configuré et IBM en est propriétaire et les gère pour vous. L'emplacement disponible lors de la création du cluster est également prédéfini pour l'environnement {{site.data.keyword.Bluemix_notm}}.</td>
 </tr>
 <tr>
 <td>Mise en réseau de l'équilibreur de charge et d'Ingress</td>
 <td>Lors de la mise à disposition de clusters standard, les actions suivantes interviennent automatiquement.<ul><li>Un sous-réseau public portable et un sous-réseau portable privé sont liés à votre cluster et affectés à votre compte d'infrastructure IBM Cloud (SoftLayer). Des sous-réseaux supplémentaires peuvent être demandés via votre compte d'infrastructure IBM Cloud (SoftLayer).</li></li><li>Une adresse IP publique portable est utilisée pour un équilibreur de charge d'application (ALB) Ingress à haute disponibilité et une route publique est affectée au format <code>&lt;cluster_name&gt;.containers.appdomain.cloud</code>. Vous pouvez utiliser cette route pour exposer plusieurs applications au public. Une adresse IP privée portable est utilisée pour un équilibreur de charge ALB privé.</li><li>Quatre adresses IP publiques portables et quatre adresses IP privées portables sont affectées au cluster et peuvent être utilisées pour les services d'équilibreur de charge.</ul></td>
 <td>Lorsque vous créez votre compte Dedicated, vous aboutissez à une décision de connectivité quant à la manière d'exposer et d'accéder à vos services de cluster. Pour utiliser vos propres plages d'adresses IP d'entreprise (adresses IP gérées par l'utilisateur), vous devez les indiquer lorsque vous [configurez un environnement {{site.data.keyword.Bluemix_dedicated_notm}}](/docs/dedicated/index.html#setupdedicated). <ul><li>Par défaut, aucun sous-réseau public portable n'est lié à des clusters que vous créez dans votre compte Dedicated. A la place, vous pouvez choisir le modèle de connectivité le mieux adapté à votre entreprise.</li><li>Après avoir créé le cluster, sélectionnez le type de sous-réseaux que vous désirez lier et utiliser avec votre cluster pour la connectivité Ingress ou avec l'équilibreur de charge.<ul><li>Pour les sous-réseaux portables publics ou privés, vous pouvez [ajouter des sous-réseaux aux clusters](cs_subnets.html#subnets)</li><li>Pour les adresses IP gérées par l'utilisateur que vous avez fournies à IBM lors de l'intégration à Dedicated, vous pouvez [ajouter aux clusters des sous-réseaux gérés par l'utilisateur](#dedicated_byoip_subnets).</li></ul></li><li>Après avoir lié un sous-réseau à votre cluster, l'équilibreur de charge ALB Ingress est créé. Une route Ingress publique n'est créée que si vous utilisez un sous-réseau public portable.</li></ul></td>
 </tr>
 <tr>
 <td>Mise en réseau de NodePort</td>
 <td>Vous pouvez exposer un port public sur votre noeud worker et utiliser l'adresse IP publique de ce noeud pour accès public au service dans le cluster.</td>
 <td>Toutes les adresses IP publiques des noeuds worker sont bloquées par un pare-feu. Toutefois, pour les services {{site.data.keyword.Bluemix_notm}} qui sont ajoutés au cluster, le service NodePort est accessible au moyen d'une adresse IP publique ou d'une adresse IP privée.</td>
 </tr>
 <tr>
 <td>Stockage persistant</td>
 <td>Utilisez [un provisionnement dynamique](cs_storage.html#create) ou un [provisionnement statique](cs_storage.html#existing) des volumes.</td>
 <td>Utilisez un [provisionnement dynamique](cs_storage.html#create) des volumes. [Ouvrez un ticket de demande de service](/docs/get-support/howtogetsupport.html#getting-customer-support) pour demander une sauvegarde de vos volumes, une restauration de vos volumes et effectuer d'autres fonctions de stockage.</li></ul></td>
 </tr>
 <tr>
 <td>URL du registre d'images dans {{site.data.keyword.registryshort_notm}}</td>
 <td><ul><li>Sud et Est des Etats-Unis : <code>registry.ng bluemix.net</code></li><li>Sud du Royaume-Uni : <code>registry.eu-gb.bluemix.net</code></li><li>Europe centrale (Francfort) : <code>registry.eu-de.bluemix.net</code></li><li>Australie (Sydney) : <code>registry.au-syd.bluemix.net</code></li></ul></td>
 <td><ul><li>Pour les nouveaux espaces de nom, utilisez les mêmes registres basés région que ceux définis pour {{site.data.keyword.Bluemix_notm}} public.</li><li>Pour les espaces de nom configurés pour les conteneurs uniques et évolutifs dans {{site.data.keyword.Bluemix_dedicated_notm}}, utilisez <code>registry.&lt;dedicated_domain&gt;</code></li></ul></td>
 </tr>
 <tr>
 <td>Accès au registre</td>
 <td>Voir les options dans [Utilisation de registres d'images privés et publics avec {{site.data.keyword.containershort_notm}}](cs_images.html).</td>
 <td><ul><li>Pour les nouveaux espaces de nom, examinez les options dans [Utilisation de registres d'images privés et publics avec {{site.data.keyword.containershort_notm}}](cs_images.html).</li><li>Pour les espaces de nom configurés pour des groupes uniques et évolutifs, [utilisez un jeton et créez une valeur confidentielle Kubernetes](cs_dedicated_tokens.html#cs_dedicated_tokens) pour l'authentification.</li></ul></td>
 </tr>
</tbody></table>
{: caption="Comparaison des fonctions entre {{site.data.keyword.Bluemix_notm}} public et {{site.data.keyword.Bluemix_dedicated_notm}}" caption-side="top"}

<br />


### Architecture de service
{: #dedicated_ov_architecture}

Chaque noeud worker est configuré avec un moteur Docker géré par {{site.data.keyword.IBM_notm}}, des ressources de traitement, un réseau et un service de volume distincts.
{:shortdesc}

Des fonctions de sécurité intégrées fournissent l'isolement, les capacités de gestion des ressources et la conformité en matière de sécurité du noeud worker. Le noeud worker communique avec le maître par l'entremise de certificats TLS sécurisés et d'une connexion openVPN.


*Architecture Kubernetes et opération réseau dans {{site.data.keyword.Bluemix_dedicated_notm}}*

![{{site.data.keyword.containershort_notm}} Architecture Kubernetes sur {{site.data.keyword.Bluemix_dedicated_notm}}](images/cs_dedicated_arch.png)

<br />


## Configuration d'{{site.data.keyword.containershort_notm}} sur l'environnement Dedicated
{: #dedicated_setup}

Chaque environnement {{site.data.keyword.Bluemix_dedicated_notm}} dispose d'un compte d'entreprise public, dont est propriétaire le client, dans {{site.data.keyword.Bluemix_notm}}. Pour que les utilisateurs dans l'environnement Dedicated puissent créer des clusters, l'administrateur doit ajouter ces utilisateurs à un compte d'entreprise public.
{:shortdesc}

Avant de commencer :
  * [Configurez un environnement {{site.data.keyword.Bluemix_dedicated_notm}}](/docs/dedicated/index.html#setupdedicated).
  * Si votre système local ou votre réseau d'entreprise contrôle des noeuds finaux sur l'Internet public en utilisant des proxies ou des pare-feux, vous devez [ouvrir les ports requis et les adresses IP sur votre pare-feu](cs_firewall.html#firewall).
  * [Téléchargez l'interface de ligne de commande (CLI) de Cloud Foundry ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/cloudfoundry/cli/releases) et [Ajoutez le plug-in de l'interface CLI d'IBM Cloud Admin](/docs/cli/plugins/bluemix_admin/index.html#adding-the-ibm-cloud-admin-cli-plug-in).

Pour permettre aux utilisateurs {{site.data.keyword.Bluemix_dedicated_notm}} d'accéder aux clusters :

1.  Le propriétaire de votre compte {{site.data.keyword.Bluemix_notm}} public doit générer une clé d'API.
    1.  Connectez-vous au noeud final pour votre instance {{site.data.keyword.Bluemix_dedicated_notm}}. Entrez les données d'identification {{site.data.keyword.Bluemix_notm}} pour le propriétaire du compte public et, à l'invite, sélectionnez votre compte.

        ```
        bx login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net
        ```
        {: pre}

        **Remarque :** si vous disposez d'un ID fédéré, utilisez `bx login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net --sso` pour vous connecter à l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}. Entrez votre nom d'utilisateur et utilisez l'URL mentionnée dans la sortie CLI pour extraire votre code d'accès à usage unique. Si la connexion échoue lorsque vous omettez l'option `--sso` et aboutit en incluant l'option `--sso`, ceci indique que votre ID est fédéré.

    2.  Générez une clé d'API pour inviter des utilisateurs sur le compte public. Notez la valeur de la clé d'API, que l'administrateur du compte Dedicated doit utiliser à la prochaine étape.

        ```
        bx iam api-key-create <key_name> -d "Key to invite users to <dedicated_account_name>"
        ```
        {: pre}

    3.  Notez l'identificateur global unique (GUID) de l'organisation de compte public à laquelle vous désirez inviter des utilisateurs et que l'administrateur du compte Dedicated doit utiliser à la prochaine étape.

        ```
        bx account orgs
        ```
        {: pre}

2.  Le propriétaire de votre compte {{site.data.keyword.Bluemix_dedicated_notm}} peut inviter un ou plusieurs utilisateurs dans votre compte public.
    1.  Connectez-vous au noeud final pour votre instance {{site.data.keyword.Bluemix_dedicated_notm}}. Entrez les données d'identification {{site.data.keyword.Bluemix_notm}} pour le propriétaire du compte Dedicated et, à l'invite, sélectionnez votre compte.

        ```
        bx login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net
        ```
        {: pre}

        **Remarque :** si vous disposez d'un ID fédéré, utilisez `bx login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net --sso` pour vous connecter à l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}. Entrez votre nom d'utilisateur et utilisez l'URL mentionnée dans la sortie CLI pour extraire votre code d'accès à usage unique. Si la connexion échoue lorsque vous omettez l'option `--sso` et aboutit en incluant l'option `--sso`, ceci indique que votre ID est fédéré.

    2.  Invitez les utilisateurs au compte public.
        * Pour inviter un utilisateur unique :

            ```
            bx cf bluemix-admin invite-users-to-public -userid=<user_email> -apikey=<public_API_key> -public_org_id=<public_org_ID>
            ```
            {: pre}

            Remplacez <em>&lt;user_IBMid&gt;</em> par l'adresse e-mail de l'utilisateur à inviter, <em>&lt;public_API_key&gt;</em> par la clé d'API générée à l'étape précédente et <em>&lt;public_org_ID&gt;</em> par l'identificateur global unique (GUID) de l'organisation du compte public. Pour plus d'informations sur cette commande, voir [Invitation d'un utilisateur depuis IBM Cloud Dedicated](/docs/cli/plugins/bluemix_admin/index.html#admin_dedicated_invite_public).

        * Pour inviter tous les utilisateurs actuels d'une organisation de compte Dedicated :

            ```
            bx cf bluemix-admin invite-users-to-public -organization=<dedicated_org_ID> -apikey=<public_API_key> -public_org_id=<public_org_ID>
            ```

            Remplacez <em>&lt;dedicated_org_ID&gt;</em> par l'ID d'organisation du compte Dedicated, <em>&lt;public_API_key&gt;</em> par la clé d'API générée à l'étape précédente et <em>&lt;public_org_ID&gt;</em> par l'identificateur global unique (GUID) de l'organisation du compte public. Pour plus d'informations sur cette commande, voir [Invitation d'un utilisateur depuis IBM Cloud Dedicated](/docs/cli/plugins/bluemix_admin/index.html#admin_dedicated_invite_public).

    3.  Si un IBMid existe pour un utilisateur, l'utilisateur est automatiquement ajouté à l'organisation spécifiée dans le compte public. Si l'utilisateur ne dispose pas d'un IBMid, une invitation lui est envoyée à son adresse électronique. Une fois que l'utilisateur accepte l'invitation, un IBMid est créé pour lui et cet utilisateur est ajouté dans l'organisation indiquée dans le compte public.

    4.  Vérifiez que les utilisateurs ont été ajoutés au compte.

        ```
        bx cf bluemix-admin invite-users-status -apikey=<public_API_key>
        ```
        {: pre}

        Les utilisateurs invités avec un IBMid existant ont le statut `ACTIVE`. Les utilisateurs invités qui n'ont pas d'IBMid ont le statut `PENDING` avant d'accepter l'invitation et le statut `ACTIVE` une fois qu'ils ont accepté l'invitation.

3.  Si un utilisateur a besoin d'autorisations de création de cluster, vous devez lui attribuer le rôle Administrateur.

    1.  Dans la barre de menu de la console publique, cliquez sur **Gérer > Sécurité > Identity and Access**, puis sur **Utilisateurs**.

    2.  Sur la ligne de l'utilisateur auquel vous voulez affecter un accès, sélectionnez le menu **Actions**, puis cliquez sur **Affecter un accès**.

    3.  Sélectionnez **Affecter l'accès aux ressources**.

    4.  Dans la liste **Services**, sélectionnez **{{site.data.keyword.containerlong}}**.

    5.  Dans la liste **Région**, sélectionnez **Toutes les régions en cours** ou une région spécifique si vous y êtes invité.

    6. Sous **Sélectionnez des rôles**, sélectionnez Administrateur.

    7. Cliquez sur **Affecter**.

4.  Les utilisateurs peuvent à présent se connecter au noeud final du compte Dedicated pour commencer à créer des clusters.

    1.  Connectez-vous au noeud final pour votre instance {{site.data.keyword.Bluemix_dedicated_notm}}. A l'invite, entrez votre IBMid.

        ```
        bx login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net
        ```
        {: pre}

        **Remarque :** si vous disposez d'un ID fédéré, utilisez `bx login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net --sso` pour vous connecter à l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}. Entrez votre nom d'utilisateur et utilisez l'URL mentionnée dans la sortie CLI pour extraire votre code d'accès à usage unique. Si la connexion échoue lorsque vous omettez l'option `--sso` et aboutit en incluant l'option `--sso`, ceci indique que votre ID est fédéré.

    2.  Si vous vous connectez pour la première fois, à l'invite, soumettez votre ID utilisateur Dedicated et votre mot de passe. Votre compte Dedicated est authentifié et les comptes Dedicated et public sont liés entre eux. Après cette première connexion, chaque fois que vous vous connecterez par la suite, vous n'utiliserez que votre IBMid. Pour plus d'informations, voir [Connexion d'un ID dédié à votre IBMid public](/docs/cli/connect_dedicated_id.html#connect_dedicated_id).

        **Remarque **: vous devez vous connecter à la fois à votre compte Dedicated et à votre compte public pour créer des clusters. Si vous désirez vous connecter seulement à votre compte Dedicated, utilisez l'indicateur `--no-iam` lorsque vous vous connectez au noeud final Dedicated.

    3.  Pour créer ou accéder à des clusters dans l'environnement dédié, vous devez définir la région qui est associée à cet environnement.

        ```
        bx cs region-set
        ```
        {: pre}

5.  Si vous désirez annuler le lien de votre compte, vous pouvez déconnecter votre IBMid de votre ID utilisateur Dedicated . Pour plus d'informations, voir [Déconnexion de votre ID dédié de l'IBMid public](/docs/cli/connect_dedicated_id.html#disconnect-your-dedicated-id-from-the-public-ibmid).

    ```
    bx iam dedicated-id-disconnect
    ```
    {: pre}

<br />


## Création de clusters
{: #dedicated_administering}

Concevez la configuration de votre cluster {{site.data.keyword.Bluemix_dedicated_notm}} en vue d'une disponibilité et d'une capacité maximale.
{:shortdesc}

### Création de clusters depuis l'interface graphique
{: #dedicated_creating_ui}

1. Ouvrez votre console Dedicated : `https://<my-dedicated-cloud-instance>.bluemix.net`.

2. Cochez la case indiquant de vous **connecter aussi à {{site.data.keyword.Bluemix_notm}} Public** et cliquez sur **Connexion**.

3. Suivez les invites pour vous connecter avec votre IBMid. S'il s'agit de votre toute première connexion à votre compte Dedicated, suivez les invites pour vous connecter à {{site.data.keyword.Bluemix_dedicated_notm}}.

4. Dans le catalogue, sélectionnez **Conteneurs** et cliquez sur **Cluster Kubernetes**.

5. Configurez les détails de votre cluster.

    1. Entrez un **nom de cluster**. Le nom doit commencer par une lettre, peut contenir des lettres, des nombres et des tirets (-) et ne doit pas dépasser 35 caractères. Le nom du cluster et la région dans laquelle est déployé le cluster constituent le nom de domaine qualifié complet du sous-domaine Ingress. Pour garantir que ce sous-domaine est unique dans une région, le nom de cluster peut être tronqué et complété par une valeur aléatoire dans le nom de domaine Ingress.

    2. Sélectionnez l'**Emplacement** dans lequel déployer votre cluster. L'emplacement disponible a été prédéfini lorsque l'environnement {{site.data.keyword.Bluemix_dedicated_notm}} a été configuré.

    3. Sélectionnez la version du serveur d'API Kubernetes pour le noeud maître du cluster.

    4. Sélectionnez un type d'isolement matériel. Le type virtuel est facturé à l'heure et le type bare metal est facturé au mois.

        - **Virtuel - Dédié** : vos noeuds worker sont hébergés sur l'infrastructure réservée à votre compte. Vos ressources physiques sont complètement isolées.

        - **Bare Metal** : facturés au mois, les serveurs bare metal sont mis à disposition par interaction manuelle avec l'infrastructure IBM Cloud (SoftLayer) et cette opération peut prendre plus d'un jour ouvrable. La configuration bare metal convient le mieux aux applications à hautes performances qui nécessitent plus de ressources et de contrôle hôte. 

        Assurez-vous de vouloir mettre à disposition une machine bare metal. Comme elle est facturée au mois, si vous l'annulez immédiatement après l'avoir commandée par erreur, vous serez toujours redevable pour le mois complet.
        {:tip}

    5. Sélectionnez un **type de machine**. Le type de machine définit le nombre d'UC virtuelles, la mémoire et l'espace disque configurés dans chaque noeud worker et rendus disponibles aux conteneurs. Les types de machines virtuelles et bare metal disponibles varient en fonction de l'emplacement de déploiement du cluster. Pour plus d'informations, voir la documentation correspondant à la [commande](cs_cli_reference.html#cs_machine_types) `bx cs machine-type`. Après avoir créé votre cluster, vous pouvez ajouter différents types de machine en ajoutant un noeud worker au cluster.

    6. Sélectionnez le **nombre de noeuds worker** dont vous avez besoin. Sélectionnez `3` pour assurer une haute disponibilité de votre cluster.

    7. Sélectionnez un **VLAN public** (facultatif) et un **VLAN privé** (obligatoire). Les VLAN public et privé sont prédéfinis lorsque l'environnement {{site.data.keyword.Bluemix_dedicated_notm}} est configuré. Ces deux réseaux VLAN communiquent entre les noeuds worker mais le réseau VLAN public communique également avec le noeud maître Kubernetes géré par IBM. Vous pouvez utiliser le même VLAN pour plusieurs clusters.
        **Remarque** : si les noeuds worker sont configurés uniquement avec un VLAN privé, vous devez configurer une autre solution pour la connectivité du réseau. Pour plus d'informations, voir [Connexion de réseau local virtuel (VLAN) pour noeuds worker](cs_clusters.html#worker_vlan_connection).

    8. Par défaut, l'option **Chiffrer le disque local** est sélectionnée. Si vous décochez cette case, les données Docker de l'hôte ne sont pas chiffrées. [En savoir plus sur le chiffrement](cs_secure.html#encrypted_disks).

6. Cliquez sur **Créer un cluster**. Vous pouvez voir la progression du déploiement du noeud worker dans l'onglet **Noeuds worker**. Une fois le déploiement terminé, vous pouvez voir si le cluster est prêt dans l'onglet **Présentation**.
    **Remarque :** à chaque noeud worker sont affectés un ID de noeud worker unique et un nom de domaine qui ne doivent pas être modifiés manuellement après la création du cluster. La modification de l'ID ou du domaine empêcherait le maître Kubernetes de gérer votre cluster.

### Création de clusters depuis l'interface CLI
{: #dedicated_creating_cli}

1.  Installez l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}} et le [plug-in {{site.data.keyword.containershort_notm}}](cs_cli_install.html#cs_cli_install).
2.  Connectez-vous au noeud final pour votre instance {{site.data.keyword.Bluemix_dedicated_notm}}. A l'invite, entrez vos données d'identification {{site.data.keyword.Bluemix_notm}} et sélectionnez votre compte.

    ```
    bx login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net
    ```
    {: pre}

    **Remarque :** si vous disposez d'un ID fédéré, utilisez `bx login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net --sso` pour vous connecter à l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}. Entrez votre nom d'utilisateur et utilisez l'URL mentionnée dans la sortie CLI pour extraire votre code d'accès à usage unique. Si la connexion échoue lorsque vous omettez l'option `--sso` et aboutit en incluant l'option `--sso`, ceci indique que votre ID est fédéré.

3.  Pour cibler une région, exécutez la commande `bx cs region-set`.

4.  Créez un cluster avec la commande `cluster-create`. Lorsque vous créez un cluster standard, le matériel du noeud worker est facturé par heures d'utilisation.

    Exemple :

    ```
    bx cs cluster-create --location <location> --machine-type <machine_type> --name <cluster_name> --workers <number>
    ```
    {: pre}

    <table>
    <caption>Description des composantes de cette commande</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composantes de cette commande</th>
    </thead>
    <tbody>
    <tr>
    <td><code>cluster-create</code></td>
    <td>Commande de création d'un cluster dans votre organisation {{site.data.keyword.Bluemix_notm}}.</td>
    </tr>
    <tr>
    <td><code>--location <em>&lt;location&gt;</em></code></td>
    <td>Entrez l'ID d'emplacement {{site.data.keyword.Bluemix_notm}} que votre environnement dédié est configuré pour utiliser.</td>
    </tr>
    <tr>
    <td><code>--machine-type <em>&lt;machine_type&gt;</em></code></td>
    <td>Entrez un type de machine. Vous pouvez déployer vos noeuds worker en tant que machines virtuelles sur du matériel dédié ou en tant que machines physiques sur un serveur bare metal. Les types de machines virtuelles et physiques disponibles varient en fonction de l'emplacement de déploiement du cluster. Pour plus d'informations, voir la documentation correspondant à la [commande](cs_cli_reference.html#cs_machine_types) `bx cs machine-type`.</td>
    </tr>
    <tr>
    <td><code>--public-vlan <em>&lt;machine_type&gt;</em></code></td>
    <td>Entrez l'ID du VLAN public que votre environnement dédié est configuré pour utiliser. Si vous désirez connecter vos noeuds worker uniquement à un VLAN privé, n'indiquez pas cette option. **Remarque** : si les noeuds worker sont configurés uniquement avec un VLAN privé, vous devez configurer une autre solution pour la connectivité du réseau. Pour plus d'informations, voir [Connexion de réseau local virtuel (VLAN) pour noeuds worker](cs_clusters.html#worker_vlan_connection).</td>
    </tr>
    <tr>
    <td><code>--private-vlan <em>&lt;machine_type&gt;</em></code></td>
    <td>Entrez l'ID du VLAN privé que votre environnement dédié est configuré pour utiliser.</td>
    </tr>  
    <tr>
    <td><code>--name <em>&lt;name&gt;</em></code></td>
    <td>Entrez un nom pour votre cluster. Le nom doit commencer par une lettre, peut contenir des lettres, des nombres et des tirets (-) et ne doit pas dépasser 35 caractères. Le nom du cluster et la région dans laquelle est déployé le cluster constituent le nom de domaine qualifié complet du sous-domaine Ingress. Pour garantir que ce sous-domaine est unique dans une région, le nom de cluster peut être tronqué et complété par une valeur aléatoire dans le nom de domaine Ingress.
</td>
    </tr>
    <tr>
    <td><code>--workers <em>&lt;number&gt;</em></code></td>
    <td>Entrez le nombre de noeuds worker à inclure dans le cluster. Si l'option <code>--workers</code> n'est pas spécifiée, un noeud worker est créé.</td>
    </tr>
    <tr>
    <td><code>--kube-version <em>&lt;major.minor.patch&gt;</em></code></td>
    <td>Version Kubernetes du noeud maître du cluster. Cette valeur est facultative. Lorsque la version n'est pas spécifiée, le cluster est créé avec la valeur par défaut des versions Kubernetes prises en charge. Pour voir les versions disponibles, exécutez la commande <code>bx cs kube-versions</code>.
</td>
    </tr>
    <tr>
    <td><code>--disable-disk-encrypt</code></td>
    <td>Les noeuds worker disposent par défaut du [chiffrement de disque](cs_secure.html#encrypted_disks). Incluez cette option si vous désirez désactiver le chiffrement.</td>
    </tr>
    <tr>
    <td><code>--trusted</code></td>
    <td>Activez la fonction [Calcul sécurisé](cs_secure.html#trusted_compute) pour vérifier que vous noeuds worker bare metal ne font pas l'objet de falsification. Si vous n'activez pas cette fonction lors de la création du cluster mais souhaitez le faire ultérieurement, vous pouvez utiliser la [commande](cs_cli_reference.html#cs_cluster_feature_enable) `bx cs feature-enable`. Après avoir activé cette fonction, vous ne pourrez plus la désactiver par la suite.</td>
    </tr>
    </tbody></table>

5.  Vérifiez que la création du cluster a été demandée.

    ```
    bx cs clusters
    ```
    {: pre}

    **Remarque :**
    * Pour les machines virtuelles, la commande des postes de noeud worker et la mise à disposition et la configuration du cluster dans votre compte peuvent prendre quelques minutes. Les machines physiques bare metal sont mises à disposition par interaction manuelle avec l'infrastructure IBM Cloud (SoftLayer) et cette opération peut prendre plus d'un jour ouvrable.
    * Si vous voyez s'afficher le message d'erreur suivant, [ouvrez un ticket de demande de service](/docs/get-support/howtogetsupport.html#getting-customer-support).
        ```
        Exception liée à l'infrastructure {{site.data.keyword.Bluemix_notm}} : impossible de passer la commande. Les ressources derrière le routeur 'router_name' ne sont pas suffisantes pour satisfaire la demande pour les invités suivants : 'worker_id'.
        ```

    Lorsque la mise à disposition de votre cluster est finalisée, le statut du cluster passe à **deployed**.

    ```
    Name         ID                                   State      Created          Workers   Location   Version
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   1         mil01      1.9.7
    ```
    {: screen}

6.  Vérifiez le statut des noeuds worker.

    ```
    bx cs workers <cluster_name_or_ID>
    ```
    {: pre}

    Lorsque les noeuds worker sont prêts, l'état passe à **normal** et le statut indique **Ready**. Lorsque le statut du noeud indique **Ready**, vous pouvez accéder au cluster.

    **Remarque :** à chaque noeud worker sont affectés un ID de noeud worker unique et un nom de domaine qui ne doivent pas être modifiés manuellement après la création du cluster. La modification de l'ID ou du domaine empêcherait le maître Kubernetes de gérer votre cluster.

    ```
    ID                                                 Public IP       Private IP       Machine Type   State    Status   Location   Version
    kube-mil01-paf97e8843e29941b49c598f516de72101-w1   169.xx.xxx.xxx  10.xxx.xx.xxx    free           normal   Ready    mil01      1.9.7
    ```
    {: screen}

7.  Définissez le cluster que vous avez créé comme contexte de cette session. Effectuez ces étapes de configuration à chaque fois que vous utilisez votre cluster.

    1.  Obtenez la commande permettant de définir la variable d'environnement et téléchargez les fichiers de configuration Kubernetes.

        ```
        bx cs cluster-config <cluster_name_or_ID>
        ```
        {: pre}

        Une fois les fichiers de configuration téléchargés, une commande s'affiche ; elle vous permet de définir le chemin vers le fichier de configuration Kubernetes local en tant que variable d'environnement.

        Exemple pour OS X :

        ```
        export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
        ```
        {: screen}

    2.  Copiez et collez la commande dans la sortie afin de définir la variable d'environnement `KUBECONFIG`.
    3.  Vérifiez que la variable d'environnement `KUBECONFIG` est correctement définie.

        Exemple pour OS X :

        ```
        echo $KUBECONFIG
        ```
        {: pre}

        Sortie :

        ```
        /Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml

        ```
        {: screen}

8.  Accédez à votre tableau de bord Kubernetes via le port par défaut (8001).
    1.  Définissez le proxy avec le numéro de port par défaut.

        ```
        kubectl proxy
        ```
        {: pre}

        ```
        Starting to serve on 127.0.0.1:8001
        ```
        {: screen}

    2.  Ouvrez l'URL suivante dans un navigateur Web pour accéder au tableau de bord Kubernetes.

        ```
        http://localhost:8001/ui
        ```
        {: codeblock}

### Utilisation de registres d'images privés et publics
{: #dedicated_images}

Découvrez comment [sécuriser vos informations personnelles](cs_secure.html#pi) lorsque vous utilisez des images de conteneur.

Pour les nouveaux espaces de nom, examinez les options dans [Utilisation de registres d'images privés et publics avec {{site.data.keyword.containershort_notm}}](cs_images.html). Pour les espaces de nom configurés pour des groupes uniques et évolutifs, [utilisez un jeton et créez une valeur confidentielle Kubernetes](cs_dedicated_tokens.html#cs_dedicated_tokens) pour l'authentification.

### Ajout de sous-réseaux à des clusters
{: #dedicated_cluster_subnet}

Vous pouvez modifier le pool d'adresses IP portables publiques disponibles en ajoutant des sous-réseaux à votre cluster. Pour plus d'informations, voir [Ajout de sous-réseaux à des clusters](cs_subnets.html#subnets). Examinez les différences suivantes en cas d'ajout de sous-réseaux à des clusters dans la version Dedicated.

#### Ajout à vos clusters Kubernetes de sous-réseaux et d'adresses IP supplémentaires gérés par l'utilisateur
{: #dedicated_byoip_subnets}

Vous pouvez ajouter un nombre supplémentaire de vos propres sous-réseaux depuis un réseau sur site, que vous désirez utiliser pour accéder à {{site.data.keyword.containershort_notm}}. Vous pouvez ajouter des adresses IP de ces sous-réseaux à Ingress et aux services d'équilibreur de charge dans votre cluster Kubernetes. Les sous-réseaux gérés par l'utilisateur sont configurés de deux manières selon le format du sous-réseau que vous désirez utiliser.

Conditions requises :
- Les sous-réseaux gérés par l'utilisateur peuvent être ajoutés uniquement à des réseaux locaux virtuels (VLAN) privés.
- La limite de longueur du préfixe de sous-réseau est /24 à /30. Par exemple, `203.0.113.0/24` indique 253 adresses IP privées utilisables, alors que `203.0.113.0/30` indique 1 adresse IP privée utilisable.
- La première adresse IP dans le sous-réseau doit être utilisée comme passerelle du sous-réseau.

Avant de commencer : Configurez le routage entrant et sortant du trafic réseau de votre entreprise afin d'utiliser le réseau {{site.data.keyword.Bluemix_dedicated_notm}} qui exploitera le sous-réseau géré par l'utilisateur.

1. Pour utiliser votre propre sous-réseau, [ouvrez un ticket de demande de service](/docs/get-support/howtogetsupport.html#getting-customer-support) et fournissez la liste des routages de sous-réseaux CIDR que vous désirez utiliser.
    **Remarque **: la manière dont les ALB et les équilibreurs de charge sont gérés pour la connectivité sur site et la connectivité de compte interne diffère selon le format du CIDR de sous-réseau. Voir la dernière étape pour les différences de configuration.

2. Après qu'{{site.data.keyword.IBM_notm}} provisionne les sous-réseaux gérés par l'utilisateur, rendez le sous-réseau disponible pour votre cluster Kubernetes.

    ```
    bx cs cluster-user-subnet-add <cluster_name> <subnet_CIDR> <private_VLAN>
    ```
    {: pre}
    Remplacez <em>&lt;cluster_name&gt;</em> par le nom ou l'ID de votre cluster, <em>&lt;subnet_CIDR&gt;</em> par l'un des CIDR de sous-réseau indiqués dans le ticket de demande de service et <em>&lt;private_VLAN&gt;</em> par un ID de VLAN privé disponible. Pour obtenir l'ID d'un VLAN privé disponible, exécutez la commande `bx cs vlans`.

3. Vérifiez que les sous-réseaux ont été ajoutés à votre cluster. La zone **User-managed** pour les sous-réseaux fournis par l'utilisateur indique _`true`_.

    ```
    bx cs cluster-get --showResources <cluster_name>
    ```
    {: pre}

    ```
    VLANs
    VLAN ID   Subnet CIDR         Public       User-managed
    1555503   169.xx.xxx.xxx/24   true         false
    1555505   10.xxx.xx.xxx/24    false        false
    1555505   10.xxx.xx.xxx/24    false        true
    ```
    {: screen}

4. Facultatif : [Activez le routage entre les sous-réseaux sur le même VLAN](cs_subnets.html#vlan-spanning).

5. Pour configurer une connectivité sur site et de compte interne, sélectionnez entre les options suivantes :
  - Si vous avez sélectionné une plage d'adresses IP privées 10.x.x.x pour le sous-réseau, utilisez des adresses IP valides sur cette plage pour configurer la connectivité sur site et du compte interne avec Ingress et un équilibreur de charge. Pour plus d'informations, voir [Planification réseau avec les services NodePort, LoadBalancer ou Ingress](cs_network_planning.html#planning).
  - Si vous n'avez pas utilisé une plage d'adresses IP privées 10.x.x.x pour le sous-réseau, utilisez des adresses IP valides sur cette plage pour configurer une connectivité sur site avec Ingress et un équilibreur de charge. Pour plus d'informations, voir [Planification réseau avec les services NodePort, LoadBalancer ou Ingress](cs_network_planning.html#planning). Toutefois, vous devez utiliser un sous-réseau privé portable de l'infrastructure IBM Cloud (SoftLayer) pour configurer la connectivité de compte interne entre votre cluster et d'autres services basés Cloud Foundry. Vous pouvez créer un sous-réseau privé portable à l'aide de la commande [`bx cs cluster-subnet-add`](cs_cli_reference.html#cs_cluster_subnet_add). Pour ce scénario, votre cluster dispose à la fois d'un sous-réseau géré par l'utilisateur pour la connectivité sur site et d'un sous-réseau privé portable de l'infrastructure IBM Cloud (SoftLayer) pour connectivité de compte interne.

### Autres configurations de cluster
{: #dedicated_other}

Examinez les options suivantes pour d'autres configurations de cluster :
  * [Gestion de l'accès au cluster](cs_users.html#access_policies)
  * [Mise à jour du maître Kubernetes](cs_cluster_update.html#master)
  * [Mise à jour des noeuds worker](cs_cluster_update.html#worker_node)
  * [Configuration de la consignation de cluster](cs_health.html#logging)
      * **Remarque** : L'activation des journaux n'est pas prise en charge à partir du noeud final Dedicated. Vous devez vous connecter au noeud final {{site.data.keyword.cloud_notm}} public et cibler votre organisation et votre espace de nom publics pour activer l'acheminement des journaux.
  * [Configuration de la surveillance de cluster](cs_health.html#view_metrics)
      * **Remarque** : un cluster `ibm-monitoring` existe dans chaque compte {{site.data.keyword.Bluemix_dedicated_notm}}. Cet environnement de cluster surveille en continu la santé du service {{site.data.keyword.containerlong_notm}} dans l'environnement Dedicated, en vérifiant la stabilité et la connectivité de l'environnement. Ne retirez-pas ce cluster de l'environnement.
  * [Visualisation de ressources de cluster Kubernetes](cs_integrations.html#weavescope)
  * [Suppression de clusters](cs_clusters.html#remove)

<br />


## Déploiement d'applications dans des clusters
{: #dedicated_apps}

Vous pouvez utiliser des techniques Kubernetes pour déployer des applications dans des clusters {{site.data.keyword.Bluemix_dedicated_notm}} et vous assurer que vos applications soient toujours opérationnelles.
{:shortdesc}

Pour déployer des applications dans des clusters, suivez les instructions de la rubrique [Déploiement d'applications dans des clusters {{site.data.keyword.Bluemix_notm}} publics](cs_app.html#app). Examinez les différences suivantes pour les clusters {{site.data.keyword.Bluemix_dedicated_notm}}.

Découvrez comment [sécuriser vos informations personnelles](cs_secure.html#pi) lorsque vous utilisez des ressources Kubernetes.

### Autorisation d'accès public aux applications
{: #dedicated_apps_public}

Pour les environnements {{site.data.keyword.Bluemix_dedicated_notm}}, les adresses IP primaires publiques sont bloquées par un pare-feu. Pour rendre une application accessible au public, utilisez un [service LoadBalancer](#dedicated_apps_public_load_balancer) ou [Ingress](#dedicated_apps_public_ingress) au lieu d'un service NodePort. Si vous avez besoin d'un accès à un service LoadBalancer ou Ingress utilisant des adresses IP publiques portables, fournissez à IBM une liste blanche de pare-feu d'entreprise au moment de l'intégration du service.

#### Configuration de l'accès public à une application à l'aide du type de service LoadBalancer
{: #dedicated_apps_public_load_balancer}

Si vous désirez utiliser des adresses IP publiques pour l'équilibreur de charge, vérifiez qu'une liste blanche de pare-feu d'entreprise a été fournie à IBM ou [ouvrez un ticket de demande de service](/docs/get-support/howtogetsupport.html#getting-customer-support) pour configurer la liste blanche de pare-feu. Suivez ensuite les étapes de la rubrique [Exposition d'applications avec des services LoadBalancer](cs_loadbalancer.html).

#### Configuration de l'accès public à une application à l'aide d'Ingress
{: #dedicated_apps_public_ingress}

Si vous désirez utiliser des adresses IP publiques pour l'équilibreur de charge ALB Ingress, vérifiez qu'une liste blanche de pare-feu d'entreprise a été fournie à IBM ou [ouvrez un ticket de demande de service](/docs/get-support/howtogetsupport.html#getting-customer-support) pour configurer la liste blanche de pare-feu. Suivez ensuite les étapes de la rubrique [Exposition d'applications au public](cs_ingress.html#ingress_expose_public).

### Création de stockage persistant
{: #dedicated_apps_volume_claim}

Pour examiner les options de création de stockage persistant, voir [Options de stockage de données persistant](cs_storage.html#planning). Pour demander une sauvegarde de vos volumes, et pour d'autres fonctions de stockage, vous devez [ouvrir un ticket de demande de service](/docs/get-support/howtogetsupport.html#getting-customer-support).
