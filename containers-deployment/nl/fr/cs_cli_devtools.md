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





# Guide de référence de l'interface de ligne de commande pour la gestion de clusters
{: #cs_cli_reference}

Reportez-vous aux commandes suivantes pour créer et gérer des clusters dans {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}

## Commandes bx cs
{: #cs_commands}

**Astuce :** pour identifier la version du plug-in {{site.data.keyword.containershort_notm}}, exécutez la commande suivante :

```
bx plugin list
```
{: pre}



<table summary="Commandes d'API">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Commandes d'API</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs api-key-info](#cs_api_key_info)</td>
    <td>[bx cs api-key-reset](#cs_api_key_reset)</td>
    <td>[bx cs apiserver-config-get](#cs_apiserver_config_get)</td>
    <td>[bx cs apiserver-config-set](#cs_apiserver_config_set)</td>
  </tr>
  <tr>
    <td>[bx cs apiserver-config-unset](#cs_apiserver_config_unset)</td>
    <td>[bx cs apiserver-refresh](#cs_apiserver_refresh)</td>
    <td></td>
    <td></td>
 </tr>
</tbody>
</table>

<br>

<table summary="Commandes d'utilisation du plug-in de l'interface CLI">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Commandes d'utilisation du plug-in de l'interface CLI</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs help](#cs_help)</td>
    <td>[bx cs init](#cs_init)</td>
    <td>[bx cs messages](#cs_messages)</td>
    <td></td>
  </tr>
</tbody>
</table>

<br>

<table summary="Commandes de cluster : Gestion">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Commandes de cluster : Gestion</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs cluster-config](#cs_cluster_config)</td>
    <td>[bx cs cluster-create](#cs_cluster_create)</td>
    <td>[bx cs cluster-feature-enable](#cs_cluster_feature_enable)</td>
    <td>[bx cs cluster-get](#cs_cluster_get)</td>
  </tr>
  <tr>
    <td>[bx cs cluster-rm](#cs_cluster_rm)</td>
    <td>[bx cs cluster-update](#cs_cluster_update)</td>
    <td>[bx cs clusters](#cs_clusters)</td>
    <td>[bx cs kube-versions](#cs_kube_versions)</td>
  </tr>
</tbody>
</table>

<br>

<table summary="Commandes de cluster : Services et intégrations">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Commandes de cluster : Services et intégrations</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs cluster-service-bind](#cs_cluster_service_bind)</td>
    <td>[bx cs cluster-service-unbind](#cs_cluster_service_unbind)</td>
    <td>[bx cs cluster-services](#cs_cluster_services)</td>
    <td>[bx cs webhook-create](#cs_webhook_create)</td>
  </tr>
</tbody>
</table>

</br>

<table summary="Commandes de cluster : Sous-réseaux">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Commandes de cluster : Sous-réseaux</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs cluster-subnet-add](#cs_cluster_subnet_add)</td>
    <td>[bx cs cluster-subnet-create](#cs_cluster_subnet_create)</td>
    <td>[bx cs cluster-user-subnet-add](#cs_cluster_user_subnet_add)</td>
    <td>[bx cs cluster-user-subnet-rm](#cs_cluster_user_subnet_rm)</td>
  </tr>
  <tr>
    <td>[bx cs subnets](#cs_subnets)</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
</tbody>
</table>

</br>

<table summary="Commandes de l'infrastructure">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Commandes de l'infrastructure</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs credentials-set](#cs_credentials_set)</td>
    <td>[bx cs credentials-unset](#cs_credentials_unset)</td>
    <td>[bx cs machine-types](#cs_machine_types)</td>
    <td>[bx cs vlans](#cs_vlans)</td>
  </tr>
</tbody>
</table>

</br>

<table summary="Commandes de l'équilibreur de charge d'application (ALB) Ingress">
<col width = 25%>
<col width = 25%>
<col width = 25%>
  <thead>
    <tr>
      <th colspan=4>Commandes de l'équilibreur de charge d'application (ALB) Ingress</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>[bx cs alb-cert-deploy](#cs_alb_cert_deploy)</td>
      <td>[bx cs alb-cert-get](#cs_alb_cert_get)</td>
      <td>[bx cs alb-cert-rm](#cs_alb_cert_rm)</td>
      <td>[bx cs alb-certs](#cs_alb_certs)</td>
    </tr>
    <tr>
      <td>[bx cs alb-configure](#cs_alb_configure)</td>
      <td>[bx cs alb-get](#cs_alb_get)</td>
      <td>[bx cs alb-types](#cs_alb_types)</td>
      <td>[bx cs albs](#cs_albs)</td>
    </tr>
  </tbody>
</table>

</br>

<table summary="Commandes de consignation">
<col width = 25%>
<col width = 25%>
<col width = 25%>
  <thead>
    <tr>
      <th colspan=4>Commandes de consignation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>[bx cs logging-config-create](#cs_logging_create)</td>
      <td>[bx cs logging-config-get](#cs_logging_get)</td>
      <td>[bx cs logging-config-refresh](#cs_logging_refresh)</td>
      <td>[bx cs logging-config-rm](#cs_logging_rm)</td>
    </tr>
    <tr>
      <td>[bx cs logging-config-update](#cs_logging_update)</td>
      <td>[bx cs logging-filter-create](#cs_log_filter_create)</td>
      <td>[bx cs logging-filter-update](#cs_log_filter_update)</td>
      <td>[bx cs logging-filter-get](#cs_log_filter_view)</td>
    </tr>
    <tr>
      <td>[bx cs logging-filter-rm](#cs_log_filter_delete)</td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>

</br>

<table summary="Commandes de région">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Commandes de région</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs locations](#cs_datacenters)</td>
    <td>[bx cs region](#cs_region)</td>
    <td>[bx cs region-set](#cs_region-set)</td>
    <td>[bx cs regions](#cs_regions)</td>
  </tr>
</tbody>
</table>

</br>

<table summary="Commandes de noeud worker">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Commandes de noeud worker</th>
 </thead>
 <tbody>
    <tr>
      <td>[bx cs worker-add](#cs_worker_add)</td>
      <td>[bx cs worker-get](#cs_worker_get)</td>
      <td>[bx cs worker-reboot](#cs_worker_reboot)</td>
      <td>[bx cs worker-reload](#cs_worker_reload)</td></staging>
    </tr>
    <tr>
      <td>[bx cs worker-rm](#cs_worker_rm)</td>
      <td>[bx cs worker-update](#cs_worker_update)</td>
      <td>[bx cs workers](#cs_workers)</td>
      <td></td>
    </tr>
  </tbody>
</table>

## Commandes d'API
{: #api_commands}

### bx cs api-key-info CLUSTER
{: #cs_api_key_info}

Permet d'afficher le nom et l'adresse e-mail du propriétaire de la clé d'API IAM dans une région {{site.data.keyword.containershort_notm}}.

La clé d'API IAM (Identity and Access Management) est définie automatiquement pour une région lorsque la première action qui nécessite la politique de contrôle d'accès admin {{site.data.keyword.containershort_notm}} est effectuée. Par exemple, supposons que l'un de vos administrateurs crée le premier cluster dans la région `us-south`. Pour cette opération, la clé d'API IAM de cet utilisateur est stockée dans le compte correspondant à cette région. La clé d'API est utilisée pour commander des ressources dans l'infrastructure IBM Cloud (SoftLayer), par exemple de nouveaux noeuds worker ou réseaux locaux virtuels (VLAN).

Lorsqu'un autre utilisateur effectue une action qui nécessite une interaction avec le portefeuille d'infrastructure IBM Cloud (SoftLayer) dans cette région, par exemple la création d'un nouveau cluster ou le rechargement d'un noeud worker, la clé d'API stockée est utilisée pour déterminer s'il dispose des droits suffisants requis pour effectuer cette action. Pour vous assurer que les actions liées à l'infrastructure dans votre cluster peuvent être effectuées sans problème, affectez à vos administrateurs la politique d'accès à l'infrastructure {{site.data.keyword.containershort_notm}} **Superutilisateur**. Pour plus d'informations, voir [Gestion de l'accès utilisateur](cs_users.html#infra_access).

Si vous constatez que la clé d'API stockée pour une région nécessite une mise à jour, vous pouvez le faire en exécutant la commande [bx cs api-key-reset](#cs_api_key_reset). Cette commande nécessite la politique d'accès admin {{site.data.keyword.containershort_notm}} et stocke la clé d'API de l'utilisateur qui exécute cette commande dans le compte.

**Astuce :** la clé d'API renvoyée par cette commande ne peut pas être utilisée si les données d'identification ont été définies manuellement à l'aide de la commande [bx cs credentials-set](#cs_credentials_set).

<strong>Options de commande</strong> :

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>
   </dl>

**Exemple** :

  ```
  bx cs api-key-info my_cluster
  ```
  {: pre}


### bx cs api-key-reset
{: #cs_api_key_reset}

Permet de remplacer la clé d'API IAM actuelle dans une région {{site.data.keyword.containershort_notm}}.

Cette commande nécessite la politique d'accès admin {{site.data.keyword.containershort_notm}} et stocke la clé d'API de l'utilisateur qui exécute cette commande dans le compte. La clé d'API IAM est nécessaire pour commander l'infrastructure depuis le portefeuille d'infrastructure IBM Cloud (SoftLayer). Une fois stockée, la clé d'API est utilisée pour toutes les actions dans une région qui nécessite des droits d'accès à l'infrastructure indépendamment de l'utilisateur qui exécute cette commande. Pour plus d'informations sur le mode de fonctionnement des clés d'API IAM, voir la [commande `bx cs api-key-info`](#cs_api_key_info).

**Important** : avant d'utiliser cette commande, assurez-vous que l'utilisateur qui l'exécute dispose des droits [{{site.data.keyword.containershort_notm}} et des droit de l'infrastructure IBM Cloud (SoftLayer)](cs_users.html#users) requis.

**Exemple** :

  ```
  bx cs api-key-reset
  ```
  {: pre}


### bx cs apiserver-config-get
{: #cs_apiserver_config_get}

Extrait des informations sur une option pour une configuration du serveur d'API Kubernetes du cluster. Cette commande doit être combinée avec l'une des sous-commandes suivantes pour l'option de configuration sur laquelle vous désirez des informations.

#### bx cs apiserver-config-get audit-webhook CLUSTER
{: #cs_apiserver_api_webhook_get}

Affiche l'URL du service de consignation distant auquel vous envoyez les journaux d'audit de serveur d'API. L'URL a été spécifiée lorsque vous avez créé le back end du webhook pour la configuration de serveur d'API.

<strong>Options de commande</strong> :

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>
   </dl>

**Exemple** :

  ```
  bx cs apiserver-config-get audit-webhook my_cluster
  ```
  {: pre}

### bx cs apiserver-config-set
{: #cs_apiserver_config_set}

Définit une option pour la configuration du serveur d'API Kubernetes d'un cluster. Cette commande doit être associée à l'une des sous-commandes suivantes pour l'option de configuration que vous voulez définir.

#### bx cs apiserver-config-set audit-webhook CLUSTER [--remoteServer SERVER_URL_OR_IP][--caCert CA_CERT_PATH] [--clientCert CLIENT_CERT_PATH][--clientKey CLIENT_KEY_PATH]
{: #cs_apiserver_api_webhook_set}

Définissez le back end du webhook pour la configuration de serveur d'API. Le back end du webhook achemine les journaux d'audit de serveur d'API à un serveur distant. Une configuration webhook est créée compte tenu des informations que vous soumettez dans les indicateurs de cette commande. Si vous ne soumettez pas d'informations dans les indicateurs, une configuration webhook par défaut est utilisée.

<strong>Options de commande</strong> :

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

   <dt><code>--remoteServer <em>SERVER_URL_OR_IP</em></code></dt>
   <dd>URL ou adresse IP du service de consignation distant auquel vous désirez envoyer les journaux d'audit. Si vous indiquez une URL de serveur non sécurisée, les certificats éventuels sont ignorés. Cette valeur est facultative.</dd>

   <dt><code>--caCert <em>CA_CERT_PATH</em></code></dt>
   <dd>Chemin de fichier du certificat d'autorité de certification utilisé pour vérifier le service de consignation distant. Cette valeur est facultative.</dd>

   <dt><code>--clientCert <em>CLIENT_CERT_PATH</em></code></dt>
   <dd>Chemin de fichier du certificat d'autorité de certification utilisé pour l'authentification auprès du service de consignation distant. Cette valeur est facultative.</dd>

   <dt><code>--clientKey <em> CLIENT_KEY_PATH</em></code></dt>
   <dd>Chemin de fichier de la clé client correspondante utilisée pour la connexion au service de consignation distant. Cette valeur est facultative.</dd>
   </dl>

**Exemple** :

  ```
  bx cs apiserver-config-set audit-webhook my_cluster --remoteServer https://audit.example.com/audit --caCert /mnt/etc/kubernetes/apiserver-audit/ca.pem --clientCert /mnt/etc/kubernetes/apiserver-audit/cert.pem --clientKey /mnt/etc/kubernetes/apiserver-audit/key.pem
  ```
  {: pre}


### bx cs apiserver-config-unset
{: #cs_apiserver_config_unset}

Désactive une option pour une configuration de serveur d'API Kubernetes d'un cluster. Cette commande doit être combinée avec l'une des sous-commandes suivantes pour l'option de configuration que vous désirez désactiver.

#### bx cs apiserver-config-unset audit-webhook CLUSTER
{: #cs_apiserver_api_webhook_unset}

Désactivez la configuration de back end du webhook pour le serveur d'API du cluster. La désactivation de back end du webhook arrête le transfert des journaux d'audit du serveur d'API à un serveur distant.

<strong>Options de commande</strong> :

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>
   </dl>

**Exemple** :

  ```
  bx cs apiserver-config-unset audit-webhook my_cluster
  ```
  {: pre}

### bx cs apiserver-refresh CLUSTER
{: #cs_apiserver_refresh}

Redémarrez le maître Kubernetes dans le cluster pour appliquer les modifications éventuelles de la configuration de serveur d'API.

<strong>Options de commande</strong> :

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>
   </dl>

**Exemple** :

  ```
  bx cs apiserver-refresh my_cluster
  ```
  {: pre}


<br />


## Commandes d'utilisation du plug-in de l'interface CLI
{: #cli_plug-in_commands}

### bx cs help
{: #cs_help}

Affichez la liste des commandes et des paramètres pris en charge.

<strong>Options de commande</strong> :

   Aucune

**Exemple** :

  ```
  bx cs help
  ```
  {: pre}


### bx cs init [--host HOST]
{: #cs_init}

Initialisez le plug-in {{site.data.keyword.containershort_notm}} ou spécifiez la région dans laquelle vous souhaitez créer ou accéder à des clusters Kubernetes.

<strong>Options de commande</strong> :

   <dl>
   <dt><code>--host <em>HOST</em></code></dt>
   <dd>Noeud final d'API {{site.data.keyword.containershort_notm}} à utiliser.  Cette valeur est facultative. [Afficher les valeurs de noeud final d'API disponibles.](cs_regions.html#container_regions)</dd>
   </dl>

**Exemple** :


```
bx cs init --host https://uk-south.containers.bluemix.net
```
{: pre}


### bx cs messages
{: #cs_messages}

Affiche les messages en cours pour l'utilisateur IBMid.

**Exemple** :

```
bx cs messages
```
{: pre}


<br />


## Commandes de cluster : Gestion
{: #cluster_mgmt_commands}


### bx cs cluster-config CLUSTER [--admin][--export]
{: #cs_cluster_config}

Après la connexion, téléchargez les données de configuration et les certificats Kubernetes pour vous connecter à votre cluster et exécuter des commandes `kubectl`. Les fichiers sont téléchargés sous `user_home_directory/.bluemix/plugins/container-service/clusters/<cluster_name>`.

**Options de commande** :

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

   <dt><code>--admin</code></dt>
   <dd>Téléchargez les fichiers de certificat et d'autorisations TLS pour le rôle Superutilisateur. Vous pouvez utiliser les certificats pour automatiser les tâches d'un cluster sans avoir à vous authentifier à nouveau. Les fichiers sont téléchargés dans `<user_home_directory>/.bluemix/plugins/container-service/clusters/<cluster_name>-admin`. Cette valeur est facultative.</dd>

   <dt><code>--export</code></dt>
   <dd>Téléchargez les certificats et les données de configuration Kubernetes sans autre message que la commande export. Comme aucun message ne s'affiche, vous pouvez utiliser cet indicateur lorsque vous créez des scripts automatisés. Cette valeur est facultative.</dd>
   </dl>

**Exemple** :

```
bx cs cluster-config my_cluster
```
{: pre}


### bx cs cluster-create [--file FILE_LOCATION][--hardware HARDWARE] --location LOCATION --machine-type MACHINE_TYPE --name NAME [--kube-version MAJOR.MINOR.PATCH][--no-subnet] [--private-vlan PRIVATE_VLAN][--public-vlan PUBLIC_VLAN] [--workers WORKER][--disable-disk-encrypt] [--trusted]
{: #cs_cluster_create}

Permet de créer un cluster dans votre organisation. Pour les clusters gratuits, indiquez le nom du cluster, tout le reste est défini avec des valeurs par défaut. Un cluster gratuit est supprimé automatiquement au bout de 21 jours. Vous ne pouvez disposer que d'un cluster gratuit à la fois. Pour tirer parti de toutes les fonctions de Kubernetes, créez un cluster standard.

<strong>Options de commande</strong>

<dl>
<dt><code>--file <em>FILE_LOCATION</em></code></dt>

<dd>Chemin d'accès au fichier YAML pour créer votre cluster standard. Au lieu de définir les caractéristiques de votre cluster à l'aide des options fournies dans cette commande, vous pouvez utiliser un fichier YAML.  Cette valeur est facultative pour les clusters standard et n'est pas disponible pour les clusters gratuits.

<p><strong>Remarque :</strong> si vous indiquez la même option dans la commande comme paramètre dans le fichier YAML, la valeur de l'option de la commande est prioritaire sur la valeur définie dans le fichier YAML. Par exemple, si vous indiquez un emplacement dans votre fichier YAML et utilisez l'option <code>--location</code> dans la commande, la valeur que vous avez entrée dans l'option de commande se substitue à la valeur définie dans le fichier YAML.

<pre class="codeblock">
<code>name: <em>&lt;cluster_name&gt;</em>
location: <em>&lt;location&gt;</em>
no-subnet: <em>&lt;no-subnet&gt;</em>
machine-type: <em>&lt;machine_type&gt;</em>
private-vlan: <em>&lt;private_VLAN&gt;</em>
public-vlan: <em>&lt;public_VLAN&gt;</em>
hardware: <em>&lt;shared_or_dedicated&gt;</em>
workerNum: <em>&lt;number_workers&gt;</em>
kube-version: <em>&lt;kube-version&gt;</em>
diskEncryption: <em>false</em>
trusted: <em>true</em>
</code></pre>


<table>
    <caption>Tableau. Description des composants du fichier YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants du fichier YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>name</em></code></td>
    <td>Remplacez <code><em>&lt;cluster_name&gt;</em></code> par le nom de votre cluster. Le nom doit commencer par une lettre, peut contenir des lettres, des nombres et des tirets (-) et ne doit pas dépasser 35 caractères. Le nom du cluster et la région dans laquelle est déployé le cluster constituent le nom de domaine qualifié complet du sous-domaine Ingress. Pour garantir que ce sous-domaine est unique dans une région, le nom de cluster peut être tronqué et complété par une valeur aléatoire dans le nom de domaine Ingress.
</td>
    </tr>
    <tr>
    <td><code><em>location</em></code></td>
    <td>Remplacez <code><em>&lt;location&gt;</em></code> par l'emplacement où vous souhaitez créer votre cluster. Les emplacements disponibles dépendent de la région à laquelle vous êtes connecté. Pour afficher la liste des emplacements disponibles, exécutez la commande <code>bx cs locations</code>. </td>
     </tr>
     <tr>
     <td><code><em>no-subnet</em></code></td>
     <td>Par défaut, un sous-réseau public et un sous-réseau privé portables sont créés sur le réseau local virtuel (VLAN) associé au cluster. Remplacez <code><em>&lt;no-subnet&gt;</em></code> par <code><em>true</em></code> afin d'éviter de créer des sous-réseaux avec le cluster. Vous pouvez [créer](#cs_cluster_subnet_create) ou [ajouter](#cs_cluster_subnet_add) des sous-réseaux à un cluster ultérieurement.</td>
      </tr>
     <tr>
     <td><code><em>machine-type</em></code></td>
     <td>Remplacez <code><em>&lt;machine_type&gt;</em></code> par le type de machine sur lequel vous envisagez de déployer vos noeuds worker. Vous pouvez déployer vos noeuds worker en tant que machines virtuelles sur du matériel partagé ou dédié ou en tant que machines physiques sur un serveur bare metal. Les types de machines virtuelles et physiques disponibles varient en fonction de l'emplacement de déploiement du cluster. Pour plus d'informations, voir la documentation correspondant à la [commande](cs_cli_reference.html#cs_machine_types) `bx cs machine-type`.</td>
     </tr>
     <tr>
     <td><code><em>private-vlan</em></code></td>
     <td>Remplacez <code><em>&lt;private_VLAN&gt;</em></code> par l'ID du réseau local virtuel privé que vous souhaitez utiliser pour vos noeuds worker. Pour afficher la liste des réseaux locaux virtuels disponibles, exécutez la commande <code>bx cs vlans <em>&lt;location&gt;</em></code> et recherchez les routeurs VLAN débutant par <code>bcr</code> (routeur de back-end).</td>
     </tr>
     <tr>
     <td><code><em>public-vlan</em></code></td>
     <td>Remplacez <code><em>&lt;public_VLAN&gt;</em></code> par l'ID du réseau local virtuel public que vous souhaitez utiliser pour vos noeuds worker. Pour afficher la liste des réseaux locaux virtuels disponibles, exécutez la commande <code>bx cs vlans <em>&lt;location&gt;</em></code> et recherchez les routeurs VLAN débutant par <code>fcr</code> (routeur de front-end).</td>
     </tr>
     <tr>
     <td><code><em>hardware</em></code></td>
     <td>Pour les types de machine virtuelle : niveau d'isolement de votre noeud worker. Utilisez un cluster dédié si vous désirez que toutes les ressources physiques vous soient dédiées exclusivement ou un cluster partagé pour permettre leur partage avec d'autres clients IBM. La valeur par défaut est <code>shared</code>.</td>
     </tr>
     <tr>
     <td><code><em>workerNum</em></code></td>
     <td>Remplacez <code><em>&lt;number_workers&gt;</em></code> par le nombre de noeuds worker que vous souhaitez déployer.</td>
     </tr>
     <tr>
      <td><code><em>kube-version</em></code></td>
      <td>Version Kubernetes du noeud maître du cluster. Cette valeur est facultative. Lorsque la version n'est pas spécifiée, le cluster est créé avec la valeur par défaut des versions Kubernetes prises en charge. Pour voir les versions disponibles, exécutez la commande <code>bx cs kube-versions</code>.
</td></tr>
      <tr>
      <td><code>diskEncryption: <em>false</em></code></td>
      <td>Les noeuds worker disposent par défaut du chiffrement de disque. [En savoir plus](cs_secure.html#worker). Pour désactiver le chiffrement, incluez cette option en lui attribuant la valeur <code>false</code>.</td></tr>
      <tr>
      <td><code>trusted: <em>true</em></code></td>
      <td>**Serveur bare metal uniquement** : activez la fonction [Calcul sécurisé](cs_secure.html#trusted_compute) pour vérifier que vos noeuds worker bare metal ne font pas l'objet de falsification. Si vous n'activez pas cette fonction lors de la création du cluster mais souhaitez le faire ultérieurement, vous pouvez utiliser la [commande](cs_cli_reference.html#cs_cluster_feature_enable) `bx cs feature-enable`. Après avoir activé cette fonction, vous ne pourrez plus la désactiver par la suite.</td></tr>
     </tbody></table>
    </p></dd>

<dt><code>--hardware <em>HARDWARE</em></code></dt>
<dd>Niveau d'isolation du matériel pour votre noeud worker. Utilisez dedicated pour que toutes les ressources physiques vous soient dédiées exclusivement ou shared pour permettre leur partage avec d'autres clients IBM. La valeur par défaut est shared.  Cette valeur est facultative pour les clusters standard et n'est pas disponible pour les clusters gratuits.</dd>

<dt><code>--location <em>LOCATION</em></code></dt>
<dd>Emplacement sous lequel vous désirez créer le cluster. Les emplacements disponibles dépendent de la région
{{site.data.keyword.Bluemix_notm}} à laquelle vous êtes connecté. Pour des performances optimales, sélectionnez la région physiquement la plus proche.  Cette valeur est obligatoire pour les clusters standard et facultative pour les clusters gratuits.

<p>Passez en revue les [emplacements disponibles](cs_regions.html#locations).
</p>

<p><strong>Remarque :</strong> si vous sélectionnez un emplacement à l'étranger, il se peut que vous ayez besoin d'une autorisation légale pour stocker physiquement les données dans un autre pays.</p>
</dd>

<dt><code>--machine-type <em>MACHINE_TYPE</em></code></dt>
<dd>Choisissez un type de machine. Vous pouvez déployer vos noeuds worker en tant que machines virtuelles sur du matériel partagé ou dédié ou en tant que machines physiques sur un serveur bare metal. Les types de machines virtuelles et physiques disponibles varient en fonction de l'emplacement de déploiement du cluster. Pour plus d'informations, voir la documentation correspondant à la [commande](cs_cli_reference.html#cs_machine_types) `bx cs machine-types`. Cette valeur est obligatoire pour les clusters standard et n'est pas disponible pour les clusters gratuits.</dd>

<dt><code>--name <em>NAME</em></code></dt>
<dd>Nom du cluster.  Cette valeur est obligatoire. Le nom doit commencer par une lettre, peut contenir des lettres, des nombres et des tirets (-) et ne doit pas dépasser 35 caractères. Le nom du cluster et la région dans laquelle est déployé le cluster constituent le nom de domaine qualifié complet du sous-domaine Ingress. Pour garantir que ce sous-domaine est unique dans une région, le nom de cluster peut être tronqué et complété par une valeur aléatoire dans le nom de domaine Ingress.
</dd>

<dt><code>--kube-version <em>MAJOR.MINOR.PATCH</em></code></dt>
<dd>Version Kubernetes du noeud maître du cluster. Cette valeur est facultative. Lorsque la version n'est pas spécifiée, le cluster est créé avec la valeur par défaut des versions Kubernetes prises en charge. Pour voir les versions disponibles, exécutez la commande <code>bx cs kube-versions</code>.
</dd>

<dt><code>--no-subnet</code></dt>
<dd>Par défaut, un sous-réseau public et un sous-réseau privé portables sont créés sur le réseau local virtuel (VLAN) associé au cluster. Incluez l'indicateur <code>--no-subnet</code> afin d'éviter la création de sous-réseaux avec le cluster. Vous pouvez [créer](#cs_cluster_subnet_create) ou [ajouter](#cs_cluster_subnet_add) des sous-réseaux à un cluster ultérieurement.</dd>

<dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
<dd>

<ul>
<li>Ce paramètre n'est pas disponible pour les clusters gratuits.</li>
<li>S'il s'agit du premier cluster standard que vous créez à cet emplacement, n'incluez pas cet indicateur. Un VLAN privé est créé pour vous lorsque le cluster est créé.</li>
<li>Si vous avez créé un cluster standard auparavant à cet emplacement ou créé un VLAN privé dans l'infrastructure IBM Cloud (SoftLayer), vous devez spécifier ce VLAN privé.

<p><strong>Remarque</strong> : les routeurs de VLAN privé commencent toujours par <code>bcr</code> (routeur de back-end) et les routeurs de VLAN public par <code>fcr</code> (routeur de front-end). Lors de la création d'un cluster et de la spécification des VLAN publics et privés, le nombre et la combinaison de lettres après ces préfixes doivent correspondre.</p></li>
</ul>

<p>Pour déterminer si vous disposez déjà d'un VLAN privé pour un emplacement spécifique ou pour identifier le nom d'un VLAN privé existant, exécutez la commande <code>bx cs vlans <em>&lt;location&gt;</em></code>.</p></dd>

<dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
<dd>
<ul>
<li>Ce paramètre n'est pas disponible pour les clusters gratuits.</li>
<li>S'il s'agit du premier cluster standard que vous créez à cet emplacement, n'utilisez pas cet indicateur. Un VLAN public est créé pour vous lorsque le cluster est créé.</li>
<li>Si vous avez créé un cluster standard auparavant à cet emplacement ou créé un VLAN public dans l'infrastructure IBM Cloud (SoftLayer), vous devez spécifier ce VLAN public.

<p><strong>Remarque</strong> : les routeurs de VLAN privé commencent toujours par <code>bcr</code> (routeur de back-end) et les routeurs de VLAN public par <code>fcr</code> (routeur de front-end). Lors de la création d'un cluster et de la spécification des VLAN publics et privés, le nombre et la combinaison de lettres après ces préfixes doivent correspondre.</p></li>
</ul>

<p>Pour déterminer si vous disposez déjà d'un VLAN public pour un emplacement spécifique ou pour identifier le nom d'un VLAN public existant, exécutez la commande <code>bx cs vlans <em>&lt;location&gt;</em></code>.</p></dd>

<dt><code>--workers WORKER</code></dt>
<dd>Nombre de noeuds worker que vous désirez déployer dans votre cluster. Si vous ne spécifiez pas cette
option, un cluster avec 1 noeud worker est créé. Cette valeur est facultative pour les clusters standard et n'est pas disponible pour les clusters gratuits.

<p><strong>Remarque :</strong> à chaque noeud worker sont affectés un ID de noeud worker unique et un nom de domaine qui ne doivent pas être modifiés manuellement après la création du cluster. La modification de l'ID ou du domaine empêcherait le maître
Kubernetes de gérer votre cluster.</p></dd>

<dt><code>--disable-disk-encrypt</code></dt>
<dd>Les noeuds worker disposent par défaut du chiffrement de disque. [En savoir plus](cs_secure.html#worker). Pour désactiver le chiffrement, incluez cette option.</dd>

<dt><code>--trusted</code></dt>
<dd><p>**Serveur bare metal uniquement** : activez la fonction [Calcul sécurisé](cs_secure.html#trusted_compute) pour vérifier que vos noeuds worker bare metal ne font pas l'objet de falsification. Si vous n'activez pas cette fonction lors de la création du cluster mais souhaitez le faire ultérieurement, vous pouvez utiliser la [commande](cs_cli_reference.html#cs_cluster_feature_enable) `bx cs feature-enable`. Après avoir activé cette fonction, vous ne pourrez plus la désactiver par la suite.</p>
<p>Pour vérifier si le type de machine bare metal prend en charge la fonction trust, vérifiez la zone `Trustable` dans la sortie de la [commande](#cs_machine_types) `bx cs machine-types <location>`. Pour vérifier que la fonction trust est activée sur un cluster, visualisez la zone **Trust ready** dans la sortie de la [commande](#cs_cluster_get) `bx cs cluster-get`. Pour vérifier que la fonction trust est activée sur un noeud worker bare metal, visualisez la zone **Trust** dans la sortie de la [commande](#cs_worker_get) `bx cs worker-get`.</p></dd>
</dl>

**Exemples** :

  

  Exemple pour un cluster standard :
  {: #example_cluster_create}

  ```
  bx cs cluster-create --location dal10 --public-vlan my_public_VLAN_ID --private-vlan my_private_VLAN_ID --machine-type u2c.2x4 --name my_cluster --hardware shared --workers 2
  ```
  {: pre}

  Exemple pour un cluster gratuit :

  ```
  bx cs cluster-create --name my_cluster
  ```
  {: pre}

  Exemple pour un environnement {{site.data.keyword.Bluemix_dedicated_notm}} :

  ```
  bx cs cluster-create --machine-type machine-type --workers number --name cluster_name
  ```
  {: pre}

### bx cs cluster-feature-enable CLUSTER [--trusted]
{: #cs_cluster_feature_enable}

Activez une fonction sur un cluster existant.

<strong>Options de commande</strong> :

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

   <dt><code><em>--trusted</em></code></dt>
   <dd><p>Incluez cet indicateur pour activer la fonction [Calcul sécurisé](cs_secure.html#trusted_compute) pour tous les noeuds worker bare metal présents dans le cluster. Après avoir activé cette fonction, vous ne pourrez plus la désactiver pour le cluster.</p>
   <p>Pour vérifier si le type de machine bare metal prend en charge la fonction de confiance (trust), vérifiez la zone **Trustable** dans la sortie de la [commande](#cs_machine_types) `bx cs machine-types <location>`. Pour vérifier que la fonction trust est activée sur un cluster, visualisez la zone **Trust ready** dans la sortie de la [commande](#cs_cluster_get) `bx cs cluster-get`. Pour vérifier que la fonction trust est activée sur un noeud worker bare metal, visualisez la zone **Trust** dans la sortie de la [commande](#cs_worker_get) `bx cs worker-get`.</p></dd>
   </dl>

**Exemple de commande** :

  ```
  bx cs cluster-feature-enable my_cluster --trusted=true
  ```
  {: pre}

### bx cs cluster-get CLUSTER [--showResources]
{: #cs_cluster_get}

Affichez des informations sur un cluster dans votre organisation.

<strong>Options de commande</strong> :

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

   <dt><code><em>--showResources</em></code></dt>
   <dd>Affiche d'autres ressources de cluster, telles que des modules complémentaires, des réseaux locaux virtuels (VLAN), des sous-réseaux et du stockage.</dd>
   </dl>

**Exemple de commande** :

  ```
  bx cs cluster-get my_cluster --showResources
  ```
  {: pre}

**Exemple de sortie** :

  ```
  Name:        my_cluster
  ID:          abc1234567
  State:       normal
  Trust ready: false
  Created:     2018-01-01T17:19:28+0000
  Location:    dal10
  Master URL:  https://169.xx.xxx.xxx:xxxxx
  Ingress subdomain: my_cluster.us-south.containers.appdomain.cloud
  Ingress secret:    my_cluster
  Workers:     3
  Version:     1.7.16_1511* (1.8.11_1509 latest)
  Owner Email: name@example.com
  Monitoring dashboard: https://metrics.ng.bluemix.net/app/#/grafana4/dashboard/db/link

  Addons
  Name                   Enabled
  customer-storage-pod   true
  basic-ingress-v2       true
  storage-watcher-pod    true

  Subnet VLANs
  VLAN ID   Subnet CIDR         Public   User-managed
  2234947   10.xxx.xx.xxx/29    false    false
  2234945   169.xx.xxx.xxx/29  true    false

  ```
  {: screen}

### bx cs cluster-rm [-f] CLUSTER
{: #cs_cluster_rm}

Supprimez un cluster de votre organisation.

<strong>Options de commande</strong> :

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

   <dt><code>-f</code></dt>
   <dd>Utilisez cette option pour forcer la suppression d'un cluster sans invites utilisateur. Cette valeur est facultative.</dd>
   </dl>

**Exemple** :

  ```
  bx cs cluster-rm my_cluster
  ```
  {: pre}


### bx cs cluster-update [-f] CLUSTER [--kube-version MAJOR.MINOR.PATCH][--force-update]
{: #cs_cluster_update}

Mettez à jour le maître Kubernetes à la version par défaut de l'API. Pendant la mise à jour, vous ne pouvez ni accéder au cluster, ni le modifier. Les noeuds worker, les applications et les ressources déployés par l'utilisateur ne sont pas modifiés et continuent à s'exécuter.

Vous pourriez devoir modifier vos fichiers YAML en vue de déploiements ultérieurs. Consultez cette [note sur l'édition](cs_versions.html) pour plus de détails.

<strong>Options de commande</strong> :

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

   <dt><code>--kube-version <em>MAJOR.MINOR.PATCH</em></code></dt>
   <dd>Version Kubernetes du cluster. Si vous ne spécifiez pas de version, la maître Kubernetes est mis à jour vers la version d'API par défaut. Pour voir les versions disponibles, exécutez la commande [bx cs kube-versions](#cs_kube_versions). Cette valeur est facultative.</dd>

   <dt><code>-f</code></dt>
   <dd>Utilisez cette option pour forcer la mise à jour du maître sans invites utilisateur. Cette valeur est facultative.</dd>

   <dt><code>--force-update</code></dt>
   <dd>Tentative de mise à jour alors que la modification est supérieure à deux niveaux de version secondaire. Cette valeur est facultative.</dd>
   </dl>

**Exemple** :

  ```
  bx cs cluster-update my_cluster
  ```
  {: pre}


### bx cs clusters
{: #cs_clusters}

Affichez la liste des clusters dans votre organisation.

<strong>Options de commande</strong> :

  Aucune

**Exemple** :

  ```
  bx cs clusters
  ```
  {: pre}


### bx cs kube-versions
{: #cs_kube_versions}

Affichez la liste des versions Kubernetes prises en charge dans {{site.data.keyword.containershort_notm}}. Mettez à jour votre [maître de cluster](#cs_cluster_update) et vos [noeuds worker](cs_cli_reference.html#cs_worker_update) à la version par défaut pour bénéficier des fonctionnalités stables les plus récentes.

**Options de commande** :

  Aucune

**Exemple** :

  ```
  bx cs kube-versions
  ```
  {: pre}



<br />



## Commandes de cluster : Services et intégrations
{: #cluster_services_commands}


### bx cs cluster-service-bind CLUSTER KUBERNETES_NAMESPACE SERVICE_INSTANCE_NAME
{: #cs_cluster_service_bind}

Ajoutez un service {{site.data.keyword.Bluemix_notm}} à un cluster. Pour afficher les services {{site.data.keyword.Bluemix_notm}} disponibles dans le catalogue {{site.data.keyword.Bluemix_notm}}, exécutez la commande `bx service offerings`. **Remarque **: vous ne pouvez ajouter que des services {{site.data.keyword.Bluemix_notm}} qui prennent en charge les clés de service.

<strong>Options de commande</strong> :

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

   <dt><code><em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>Nom de l'espace de nom Kubernetes. Cette valeur est obligatoire.</dd>

   <dt><code><em>SERVICE_INSTANCE_NAME</em></code></dt>
   <dd>Nom de l'instance de service {{site.data.keyword.Bluemix_notm}} que vous voulez lier. Pour connaître le nom de votre instance de service, exécutez la commande <code>bx service list</code>. Si plusieurs instances ont le même nom dans le compte, utilisez l'ID d'instance à la place du nom. Pour obtenir l'ID, exécutez la commande <code>bx service show <service instance name> --guid</code>. L'une de ces valeurs est obligatoire.</dd>
   </dl>

**Exemple** :

  ```
  bx cs cluster-service-bind my_cluster my_namespace my_service_instance
  ```
  {: pre}


### bx cs cluster-service-unbind CLUSTER KUBERNETES_NAMESPACE SERVICE_INSTANCE_GUID
{: #cs_cluster_service_unbind}

Supprimez un service {{site.data.keyword.Bluemix_notm}} d'un cluster.

**Remarque :** lorsque vous supprimez un service {{site.data.keyword.Bluemix_notm}}, ses données d'identification du service sont supprimées du cluster. Si un pod utilise encore ce service, son opération échoue vu que les données d'identification du service sont introuvables.

<strong>Options de commande</strong> :

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

   <dt><code><em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>Nom de l'espace de nom Kubernetes. Cette valeur est obligatoire.</dd>

   <dt><code><em>SERVICE_INSTANCE_GUID</em></code></dt>
   <dd>ID de l'instance de service {{site.data.keyword.Bluemix_notm}} que vous désirez retirer. Pour identifier l'ID de l'instance de service, exécutez la commande `bx cs cluster-services <cluster_name_or_ID>`. Cette valeur est obligatoire.</dd>
   </dl>

**Exemple** :

  ```
  bx cs cluster-service-unbind my_cluster my_namespace 8567221
  ```
  {: pre}


### bx cs cluster-services CLUSTER [--namespace KUBERNETES_NAMESPACE][--all-namespaces]
{: #cs_cluster_services}

Répertoriez les services liés à un ou à tous les espaces de nom Kubernetes dans un cluster. Si aucune option n'est spécifiée, les services pour l'espace de nom par défaut sont affichés.

<strong>Options de commande</strong> :

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

   <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code>, <code>-n <em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>Inclut les services liés à un espace de nom spécifique dans un cluster. Cette valeur est facultative.</dd>

   <dt><code>--all-namespaces</code></dt>
    <dd>Inclut les services liés à tous les espaces de nom dans un cluster. Cette valeur est facultative.</dd>
    </dl>

**Exemple** :

  ```
  bx cs cluster-services my_cluster --namespace my_namespace
  ```
  {: pre}



### bx cs webhook-create --cluster CLUSTER --level LEVEL --type slack --url URL
{: #cs_webhook_create}

Permet d'enregistrer un webhook.

<strong>Options de commande</strong> :

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

   <dt><code>--level <em>LEVEL</em></code></dt>
   <dd>Niveau de notification, tel que <code>Normal</code> ou <code>Warning</code>. <code>Warning</code> est la valeur par défaut. Cette valeur est facultative.</dd>

   <dt><code>--type <em>slack</em></code></dt>
   <dd>Type de webhook. Seul le type slack est pris en charge actuellement. Cette valeur est obligatoire.</dd>

   <dt><code>--url <em>URL</em></code></dt>
   <dd>URL du webhook. Cette valeur est obligatoire.</dd>
   </dl>

**Exemple** :

  ```
  bx cs webhook-create --cluster my_cluster --level Normal --type slack --url http://github.com/mywebhook
  ```
  {: pre}


<br />


## Commandes de cluster : Sous-réseaux
{: #cluster_subnets_commands}

### bx cs cluster-subnet-add CLUSTER SUBNET
{: #cs_cluster_subnet_add}

Rendez un sous-réseau d'un compte d'infrastructure IBM Cloud (SoftLayer) disponible pour le cluster spécifié.

**Remarque :**
* Lorsque vous rendez un sous-réseau accessible à un cluster, les adresses IP de ce sous-réseau sont utilisées pour la mise en réseau du cluster. Pour éviter des conflits d'adresse IP, prenez soin de n'utiliser le sous-réseau qu'avec un seul cluster. N'utilisez pas en même temps un sous-réseau pour plusieurs clusters ou à d'autres fins hors d'{{site.data.keyword.containershort_notm}}.
* Pour effectuer un routage entre les sous-réseaux d'un même VLAN, vous devez [activer la fonction Spanning VLAN](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning).

<strong>Options de commande</strong> :

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

   <dt><code><em>SUBNET</em></code></dt>
   <dd>ID du sous-réseau. Cette valeur est obligatoire.</dd>
   </dl>

**Exemple** :

  ```
  bx cs cluster-subnet-add my_cluster 1643389
  ```
  {: pre}


### bx cs cluster-subnet-create CLUSTER SIZE VLAN_ID
{: #cs_cluster_subnet_create}

Créez un sous-réseau dans un compte d'infrastructure IBM Cloud (SoftLayer) et le rend disponible pour le cluster spécifié dans {{site.data.keyword.containershort_notm}}.

**Remarque :**
* Lorsque vous rendez un sous-réseau accessible à un cluster, les adresses IP de ce sous-réseau sont utilisées pour la mise en réseau du cluster. Pour éviter des conflits d'adresse IP, prenez soin de n'utiliser le sous-réseau qu'avec un seul cluster. N'utilisez pas en même temps un sous-réseau pour plusieurs clusters ou à d'autres fins hors d'{{site.data.keyword.containershort_notm}}.
* Pour effectuer un routage entre les sous-réseaux d'un même VLAN, vous devez [activer la fonction Spanning VLAN](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning).

<strong>Options de commande</strong> :

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Nom ou ID du cluster. Cette valeur est obligatoire. Pour répertorier vos clusters, utilisez la [commande](#cs_clusters) `bx cs clusters`.</dd>

   <dt><code><em>SIZE</em></code></dt>
   <dd>Nombre d'adresses IP du sous-réseau. Cette valeur est obligatoire. Les valeurs possibles sont : 8, 16, 32 ou 64.</dd>

   <dt><code><em>VLAN_ID</em></code></dt>
   <dd>Réseau local virtuel (VLAN) dans lequel créer le sous-réseau. Cette valeur est obligatoire. Pour répertorier les VLAN disponibles, utilisez la [commande](#cs_vlans) `bx cs vlans<location>`. </dd>
   </dl>

**Exemple** :

  ```
  bx cs cluster-subnet-create my_cluster 8 1764905
  ```
  {: pre}


### bx cs cluster-user-subnet-add CLUSTER SUBNET_CIDR PRIVATE_VLAN
{: #cs_cluster_user_subnet_add}

Mettez à disposition votre propre sous-réseau privé sur vos clusters {{site.data.keyword.containershort_notm}}.

Ce sous-réseau privé n'est pas celui qui est fourni par l'infrastructure IBM Cloud (SoftLayer). De ce fait, vous devez configurer tout routage de trafic entrant et sortant pour le sous-réseau. Pour ajouter un sous-réseau d'infrastructure IBM Cloud (SoftLayer), utilisez la [commande](#cs_cluster_subnet_add) `bx cs cluster-subnet-add`.

**Remarque **:
* Lorsque vous ajoutez un sous-réseau utilisateur privé à un cluster, les adresses IP de ce sous-réseau sont utilisées pour les équilibreurs de charge privés figurant dans le cluster. Pour éviter des conflits d'adresse IP, prenez soin de n'utiliser le sous-réseau qu'avec un seul cluster. N'utilisez pas en même temps un sous-réseau pour plusieurs clusters ou à d'autres fins hors d'{{site.data.keyword.containershort_notm}}.
* Pour effectuer un routage entre les sous-réseaux d'un même VLAN, vous devez [activer la fonction Spanning VLAN](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning).

<strong>Options de commande</strong> :

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

   <dt><code><em>SUBNET_CIDR</em></code></dt>
   <dd>CIDR (Classless InterDomain Routing) du sous-réseau. Cette valeur est obligatoire et ne doit pas entrer en conflit avec un sous-réseau utilisé par l'infrastructure IBM Cloud (SoftLayer).

   Les préfixes pris en charge sont compris entre `/30` (1 adresse IP) et `/24` (253 adresses IP). Si vous avez défini un CIDR avec une longueur de préfixe et que vous devez modifier sa valeur, ajoutez d'abord le nouveau CIDR, puis [supprimez l'ancien CIDR](#cs_cluster_user_subnet_rm).</dd>

   <dt><code><em>PRIVATE_VLAN</em></code></dt>
   <dd>ID du VLAN privé. Cette valeur est obligatoire. Elle doit correspondre à l'ID du VLAN privé d'un ou plusieurs noeuds worker dans le cluster.</dd>
   </dl>

**Exemple** :

  ```
  bx cs cluster-user-subnet-add my_cluster 169.xx.xxx.xxx/29 1502175
  ```
  {: pre}


### bx cs cluster-user-subnet-rm CLUSTER SUBNET_CIDR PRIVATE_VLAN
{: #cs_cluster_user_subnet_rm}

Supprimez votre propre sous-réseau privé du cluster indiqué.

**Remarque :** tout service déployé sur une adresse IP depuis votre propre sous-réseau privé reste actif une fois le sous-réseau supprimé.

<strong>Options de commande</strong> :

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

   <dt><code><em>SUBNET_CIDR</em></code></dt>
   <dd>CIDR (Classless InterDomain Routing) du sous-réseau. Cette valeur est obligatoire et doit correspondre au CIDR défini par la [commande](#cs_cluster_user_subnet_add) `bx cs cluster-user-subnet-add`.</dd>

   <dt><code><em>PRIVATE_VLAN</em></code></dt>
   <dd>ID du VLAN privé. Cette valeur est obligatoire et doit correspondre à l'ID du VLAN défini par la [commande](#cs_cluster_user_subnet_add) `bx cs cluster-user-subnet-add`.</dd>
   </dl>

**Exemple** :

  ```
  bx cs cluster-user-subnet-rm my_cluster 169.xx.xxx.xxx/29 1502175
  ```
  {: pre}

### bx cs subnets
{: #cs_subnets}

Affichez la liste des sous-réseaux disponibles dans un compte d'infrastructure IBM Cloud (SoftLayer).

<strong>Options de commande</strong> :

   Aucune

**Exemple** :

  ```
  bx cs subnets
  ```
  {: pre}


<br />


## Commandes de l'équilibreur de charge d'application (ALB) Ingress
{: #alb_commands}

### bx cs alb-cert-deploy [--update] --cluster CLUSTER --secret-name SECRET_NAME --cert-crn CERTIFICATE_CRN
{: #cs_alb_cert_deploy}

Déploiement ou mise à jour d'un certificat à partir de votre instance {{site.data.keyword.cloudcerts_long_notm}} vers l'équilibreur de charge d'application (ALB) dans un cluster.

**Remarque :**
* Seul un utilisateur affecté au rôle d'accès Administrateur peut exécuter cette commande.
* Vous ne pouvez mettre à jour que des certificats importés depuis la même instance {{site.data.keyword.cloudcerts_long_notm}}.

<strong>Options de commande</strong>

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

   <dt><code>--update</code></dt>
   <dd>Met à jour le certificat d'une valeur confidentielle d'équilibreur de charge d'application (ALB) dans un cluster. Cette valeur est facultative.</dd>

   <dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
   <dd>Nom de la valeur confidentielle ALB. Cette valeur est obligatoire.</dd>

   <dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
   <dd>CRN du certificat. Cette valeur est obligatoire.</dd>
   </dl>

**Exemples** :

Exemple de déploiement d'une valeur confidentielle ALB :

   ```
   bx cs alb-cert-deploy --secret-name my_alb_secret --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
   ```
   {: pre}

Exemple de mise à jour d'une valeur confidentielle ALB existante :

 ```
 bx cs alb-cert-deploy --update --secret-name my_alb_secret --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:7e21fde8ee84a96d29240327daee3eb2
 ```
 {: pre}


### bx cs alb-cert-get --cluster CLUSTER [--secret-name SECRET_NAME][--cert-crn CERTIFICATE_CRN]
{: #cs_alb_cert_get}

Affichage d'informations sur une valeur confidentielle ALB dans un cluster.

**Remarque :** seul un utilisateur affecté au rôle d'accès Administrateur peut exécuter cette commande.

<strong>Options de commande</strong>

  <dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
  <dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

  <dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
  <dd>Nom de la valeur confidentielle ALB. Cette valeur est requise pour obtenir des informations sur une valeur confidentielle ALB spécifique dans le cluster.</dd>

  <dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
  <dd>CRN du certificat. Cette valeur est requise pour obtenir des informations sur toutes les valeurs confidentielles ALB correspondant à un CRN de certificat spécifique dans le cluster.</dd>
  </dl>

**Exemples** :

 Exemple d'extraction d'informations sur une valeur confidentielle ALB :

 ```
 bx cs alb-cert-get --cluster my_cluster --secret-name my_alb_secret
 ```
 {: pre}

 Exemple d'extraction d'informations sur toutes les valeurs confidentielles ALB correspondant à un CRN de certificat spécifié :

 ```
 bx cs alb-cert-get --cluster my_cluster --cert-crn  crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
 ```
 {: pre}


### bx cs alb-cert-rm --cluster CLUSTER [--secret-name SECRET_NAME][--cert-crn CERTIFICATE_CRN]
{: #cs_alb_cert_rm}

Retrait d'une valeur confidentielle ALB dans un cluster.

**Remarque :** seul un utilisateur affecté au rôle d'accès Administrateur peut exécuter cette commande.

<strong>Options de commande</strong>

  <dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
  <dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

  <dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
  <dd>Nom de la valeur confidentielle ALB. Cette valeur est requise pour retirer une valeur confidentielle ALB spécifique dans le cluster.</dd>

  <dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
  <dd>CRN du certificat. Cette valeur est requise pour retirer toutes les valeurs confidentielles ALB correspondant à un CRN de certificat spécifique dans le cluster.</dd>
  </dl>

**Exemples** :

 Exemple de retrait d'une valeur confidentielle ALB :

 ```
 bx cs alb-cert-rm --cluster my_cluster --secret-name my_alb_secret
 ```
 {: pre}

 Exemple de retrait de toutes les valeurs confidentielles ALB correspondant à un CRN de certificat spécifié :

 ```
 bx cs alb-cert-rm --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
 ```
 {: pre}


### bx cs alb-certs --cluster CLUSTER
{: #cs_alb_certs}

Affichage d'une liste de valeurs confidentielles ALB dans un cluster.

**Remarque :** seuls les utilisateurs affectés au rôle d'accès Administrateur peuvent exécuter cette commande.

<strong>Options de commande</strong>

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>
   </dl>

**Exemple** :

 ```
 bx cs alb-certs --cluster my_cluster
 ```
 {: pre}

### bx cs alb-configure --albID ALB_ID [--enable][--disable][--user-ip USERIP]
{: #cs_alb_configure}

Activation ou désactivation d'un équilibreur de charge ALB dans votre cluster standard. L'ALB public est activé par défaut.

**Options de commande** :

   <dl>
   <dt><code><em>--albID </em>ALB_ID</code></dt>
   <dd>ID d'un équilibreur de charge ALB. Exécutez <code>bx cs albs <em>--cluster </em>CLUSTER</code> pour afficher les ID des équilibreurs de charge ALB dans un cluster. Cette valeur est obligatoire.</dd>

   <dt><code>--enable</code></dt>
   <dd>Incluez cet indicateur pour activer un équilibreur de charge ALB dans un cluster.</dd>

   <dt><code>--disable</code></dt>
   <dd>Incluez cet indicateur pour désactiver un équilibreur de charge ALB dans un cluster.</dd>

   <dt><code>--user-ip <em>USER_IP</em></code></dt>
   <dd>

   <ul>
    <li>Ce paramètre n'est disponible que pour activer un équilibreur de charge d'application (ALB) privé</li>
    <li>L'ALB privé est déployé avec une adresse IP à partir d'un sous-réseau privé fourni par l'utilisateur. Si aucune adresse IP n'est fournie, l'ALB est déployé avec une adresse IP privée issue du sous-réseau privé portable qui est mis à disposition automatiquement lorsque vous avez créé le cluster.</li>
   </ul>
   </dd>
   </dl>

**Exemples** :

  Exemple d'activation d'un équilibreur de charge ALB :

  ```
  bx cs alb-configure --albID private-cr18a61a63a6a94b658596aa93a087aaa9-alb1 --enable
  ```
  {: pre}

  Exemple d'activation d'un équilibreur de charge ALB avec une adresse IP fournie par l'utilisateur :

  ```
  bx cs alb-configure --albID private-cr18a61a63a6a94b658596aa93a087aaa9-alb1 --enable --user-ip user_ip
  ```
  {: pre}

  Exemple de désactivation d'un équilibreur de charge ALB :

  ```
  bx cs alb-configure --albID public-cr18a61a63a6a94b658596aa93a087aaa9-alb1 --disable
  ```
  {: pre}

### bx cs alb-get --albID ALB_ID
{: #cs_alb_get}

Affichage des détails d'un équilibreur de charge ALB.

<strong>Options de commande</strong> :

   <dl>
   <dt><code><em>--albID </em>ALB_ID</code></dt>
   <dd>ID d'un équilibreur de charge ALB. Exécutez la commande <code>bx cs albs --cluster <em>CLUSTER</em></code> pour afficher les ID des équilibreurs de charge ALB dans un cluster. Cette valeur est obligatoire.</dd>
   </dl>

**Exemple** :

  ```
  bx cs alb-get --albID public-cr18a61a63a6a94b658596aa93a087aaa9-alb1
  ```
  {: pre}

### bx cs alb-types
{: #cs_alb_types}

Affichage des types d'équilibreur de charge ALB pris en charge dans la région.

<strong>Options de commande</strong> :

   Aucune

**Exemple** :

  ```
  bx cs alb-types
  ```
  {: pre}


### bx cs albs --cluster CLUSTER
{: #cs_albs}

Affichage du statut de tous les équilibreurs de charge ALB dans un cluster. Si aucun ID ALB n'est renvoyé, le cluster n'a pas de sous-réseau portable. Vous pouvez [créer](#cs_cluster_subnet_create) ou [ajouter](#cs_cluster_subnet_add) des sous-réseaux à un cluster.

<strong>Options de commande</strong> :

   <dl>
   <dt><code><em>--cluster </em>CLUSTER</code></dt>
   <dd>Nom ou ID du cluster sur lequel répertorier les équilibreurs de charge ALB disponibles. Cette valeur est obligatoire.</dd>
   </dl>

**Exemple** :

  ```
  bx cs albs --cluster my_cluster
  ```
  {: pre}


<br />


## Commandes de l'infrastructure
{: #infrastructure_commands}

### bx cs credentials-set --infrastructure-api-key API_KEY --infrastructure-username USERNAME
{: #cs_credentials_set}

Définissez les données d'identification du compte d'infrastructure IBM Cloud (SoftLayer) pour votre compte {{site.data.keyword.containershort_notm}}.

Si vous disposez d'un compte de paiement à la carte {{site.data.keyword.Bluemix_notm}}, vous avez accès au portefeuille d'infrastructure IBM Cloud (SoftLayer) par défaut. Cependant, vous pouvez utiliser un autre compte d'infrastructure IBM Cloud (SoftLayer) dont vous disposez déjà pour commander l'infrastructure. Vous pouvez lier le compte de cette infrastructure à votre compte {{site.data.keyword.Bluemix_notm}} en utilisant cette commande.

Si les données d'identification de l'infrastructure IBM Cloud (SoftLayer) sont définies manuellement, ces données sont utilisées pour commander l'infrastructure, même s'il existe déjà une  [clé d'API IAM](#cs_api_key_info) pour le compte. Si l'utilisateur dont les données d'identification sont stockées ne dispose pas des droits requis pour commander l'infrastructure, les actions liées à l'infrastructure, par exemple la création d'un cluster ou le rechargement d'un noeud worker risquent d'échouer.

Vous ne pouvez pas définir plusieurs données d'identification pour un compte {{site.data.keyword.containershort_notm}}. Tout compte {{site.data.keyword.containershort_notm}} est lié à un seul portefeuille d'infrastructure IBM Cloud (SoftLayer).

**Important :** avant d'utiliser cette commande, assurez-vous que l'utilisateur dont les données d'identification sont utilisées dispose des droits [{{site.data.keyword.containershort_notm}} et des droits de l'infrastructure IBM Cloud (SoftLayer)](cs_users.html#users) requis.

<strong>Options de commande</strong> :

   <dl>
   <dt><code>--infrastructure-username <em>USERNAME</em></code></dt>
   <dd>Nom d'utilisateur du compte d'infrastructure IBM Cloud (SoftLayer). Cette valeur est obligatoire.</dd>


   <dt><code>--infrastructure-api-key <em>API_KEY</em></code></dt>
   <dd>Clé d'API du compte d'infrastructure IBM Cloud (SoftLayer). Cette valeur est obligatoire.

 <p>
  Pour générer une clé d'API, procédez comme suit :

  <ol>
  <li>Connectez-vous au [portail d'infrastructure IBM Cloud (SoftLayer) ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com/).</li>
  <li>Sélectionnez <strong>Compte</strong>, puis <strong>Utilisateurs</strong>.</li>
  <li>Cliquez sur <strong>Générer</strong> pour générer une clé d'API d'infrastructure IBM Cloud (SoftLayer) pour votre compte.</li>
  <li>Copiez la clé d'API à utiliser dans cette commande.</li>
  </ol>

  Pour afficher votre clé d'API existante, procédez comme suit :
  <ol>
  <li>Connectez-vous au [portail d'infrastructure IBM Cloud (SoftLayer) ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com/).</li>
  <li>Sélectionnez <strong>Compte</strong>, puis <strong>Utilisateurs</strong>.</li>
  <li>Cliquez sur <strong>Afficher</strong> pour afficher votre clé d'API existante.</li>
  <li>Copiez la clé d'API à utiliser dans cette commande.</li>
  </ol>
  </p></dd>
  </dl>

**Exemple** :

  ```
  bx cs credentials-set --infrastructure-api-key <api_key> --infrastructure-username dbmanager
  ```
  {: pre}


### bx cs credentials-unset
{: #cs_credentials_unset}

Supprimez les données d'identification du compte d'infrastructure IBM Cloud (SoftLayer) de votre compte {{site.data.keyword.containershort_notm}}.

Après avoir supprimé les données d'identification, la [clé d'API IAM](#cs_api_key_info) est utilisée pour commander des ressources dans l'infrastructure IBM Cloud (SoftLayer).

<strong>Options de commande</strong> :

   Aucune

**Exemple** :

  ```
  bx cs credentials-unset
  ```
  {: pre}


### bx cs machine-types LOCATION
{: #cs_machine_types}

Affichez la liste des types de machine disponibles pour vos noeuds worker. Chaque type de machine inclut la quantité d'UC virtuelles, de mémoire et d'espace disque pour chaque noeud worker dans le cluster. Par défaut, le répertoire `/var/lib/docker`, dans lequel sont stockées toutes les données des conteneurs, est chiffré avec le chiffrement LUKS. Si l'option `disable-disk-encrypt` est incluse lors de la création du cluster, les données Docker de l'hôte ne sont pas chiffrées. [En savoir plus sur le chiffrement.](cs_secure.html#encrypted_disks)
{:shortdesc}

Vous pouvez mettre à disposition votre noeud worker en tant que machine virtuelle sur un matériel dédié ou partagé, ou en tant que machine physique sur un serveur bare metal.

<dl>
<dt>Pourquoi utiliser des machines physiques (bare metal) ?</dt>
<dd><p><strong>Plus de ressources de calcul</strong> : vous pouvez mettre à disposition votre noeud worker en tant que serveur physique à service exclusif, également désigné par serveur bare metal. Bare metal vous permet d'accéder directement aux ressources physiques sur la machine, par exemple à la mémoire ou à l'UC. Cette configuration élimine l'hyperviseur de machine virtuelle qui alloue des ressources physiques aux machines virtuelles qui s'exécutent sur l'hôte. A la place, toutes les ressources d'une machine bare metal sont dédiées exclusivement au noeud worker, donc vous n'avez pas à vous soucier de "voisins gênants" partageant des ressources et responsables du ralentissement des performances. Les types de machine physique ont davantage de capacité de stockage local par rapport aux machines virtuelles et certaines disposent de disques RAID pour effectuer des sauvegardes de données locales.</p>
<p><strong>Facturation mensuelle</strong> : les serveurs bare metal sont plus chers que les serveurs virtuels et conviennent mieux aux applications à hautes performances qui nécessitent plus de ressources et de contrôle hôte. Les serveurs bare metal sont facturés au mois. Si vous annulez un serveur bare metal avant la fin du mois, vous êtes facturé jusqu'à la fin de ce mois. La commande et l'annulation de serveurs bare metal est un processus manuel qui s'effectue via votre compte d'infrastructure IBM Cloud (SoftLayer). Ce processus peut prendre plus d'un jour ouvrable.</p>
<p><strong>Option pour activer la fonction de calcul sécurisé (Trusted Compute)</strong> : activez la fonction de calcul sécurisé pour vérifier que vos noeuds worker ne font pas l'objet de falsification. Si vous n'activez pas cette fonction lors de la création du cluster mais souhaitez le faire ultérieurement, vous pouvez utiliser la [commande](cs_cli_reference.html#cs_cluster_feature_enable) `bx cs feature-enable`. Après avoir activé cette fonction, vous ne pourrez plus la désactiver par la suite. Vous pouvez créer un nouveau cluster sans la fonction trust. Pour plus d'informations sur le mode de fonctionnement de la fonction de confiance (trust) lors du processus de démarrage du noeud, voir [{{site.data.keyword.containershort_notm}} avec calcul sécurisé](cs_secure.html#trusted_compute). La fonction de calcul sécurisé (Trusted Compute) est activée sur les clusters qui exécutent Kubernetes version 1.9 ou ultérieure et qui ont certains types de machine bare metal. Lorsque vous exécutez la [commande](cs_cli_reference.html#cs_machine_types) `bx cs machine-types <location>`, vous pouvez voir les machines qui prennent en charge la fonction de confiance en examinant la zone **Trustable**. Par exemple, les versions GPU `mgXc` ne prennent pas en charge la fonction de calcul sécurisé (Trusted Compute).</p></dd>
<dt>Pourquoi utiliser des machines virtuelles ?</dt>
<dd><p>Avec des machines virtuelles, vous pouvez obtenir une plus grande flexibilité, des temps de mise à disposition plus rapides et plus de fonctions de mise à l'échelle automatique qu'avec des machines physiques (bare metal), à un coût plus avantageux. Vous pouvez utiliser des machines virtuelles pour la plupart des cas d'utilisation généraux, par exemple les environnements de test et de développement, les environnements de préproduction et de production, les microservices et les applications métier. Cependant, il y a un impact sur les performances. Si vous avez besoin de calcul haute performance pour des charges de travail qui consomment beaucoup de mémoire RAM, de données ou de processeur graphique (GPU), utilisez des machines bare metal.</p>
<p><strong>Choisir entre un service partagé ou exclusif</strong> : lorsque vous créez un cluster standard virtuel, vous devez décider si le matériel sous-jacent doit être partagé par plusieurs clients {{site.data.keyword.IBM_notm}} (service partagé) ou vous être dédié exclusivement (service exclusif). </p>
<p>Dans une configuration à service partagé, les ressources physiques (comme l'UC et la mémoire) sont partagées par toutes les machines virtuelles déployées sur le même matériel physique. Pour permettre à chaque machine virtuelle d'opérer indépendamment, un moniteur de machine virtuelle, également dénommé hyperviseur, segmente les ressources physiques en entités isolées et les alloue à une machine virtuelle en tant que ressources dédiées (isolement par hyperviseur).</p>
<p>Dans une configuration à service exclusif, toutes les ressources physiques vous sont dédiées en exclusivité. Vous pouvez déployer plusieurs noeuds worker en tant que machines virtuelles sur le même hôte physique. A l'instar de la configuration à service partagé, l'hyperviseur veille à ce que chaque noeud worker ait sa part des ressources physiques disponibles.</p>
<p>Les noeuds partagés sont généralement moins coûteux que les noeuds dédiés, car les coûts du matériel sous-jacent sont partagés entre plusieurs clients. Toutefois, lorsque vous choisissez entre noeuds partagés et noeud dédiés, vous devriez contacter votre service juridique pour déterminer le niveau d'isolement de l'infrastructure et de conformité requis par votre environnement d'application.</p>
<p><strong>Versions de machine virtuelle `u2c` ou `b2c`</strong> : ces machines utilisent le disque local au lieu du réseau SAN (Storage Area Networking) pour une plus grande fiabilité. Un SAN procure, entre autres, une capacité de traitement plus élevée lors de la sérialisation des octets sur le disque local et réduit les risques de dégradation du système de fichiers en cas de défaillance du réseau. Ces types de machine contiennent un stockage sur disque local principal de 25 Go pour le système de fichiers du système d'exploitation et 100 Go de stockage sur disque local secondaire pour `/var/lib/docker`, répertoire dans lequel sont écrites toutes les données des conteneurs.</p>
<p><strong>Que se passe-t-il si je dispose de types de machine `u1c` ou `b1c` obsolètes ?</strong> Pour commencer à utiliser les types de machine `u2c` et `b2c`, [mettez à jour les types de machine en ajoutant des noeuds worker](cs_cluster_update.html#machine_type).</p></dd>
<dt>Entre quelles versions de machine virtuelle et physique ai-je le choix ?</dt>
<dd><p>De nombreuses versions ! Sélectionnez le type de machine correspondant le mieux à vos besoins. N'oubliez pas qu'un pool de noeuds worker est constitué de machines de même version. Si vous souhaitez avoir un mélange de types de machine dans votre cluster, créez des pool de noeuds worker distincts pour chaque version.</p>
<p>Les types de machine varient en fonction de la zone. Pour voir les types de machine disponibles dans votre zone, exécutez la commande `bx cs machine-types <zone_name>`.</p>
<p><table>
<caption>Types de machine physique (bare metal) et virtuelle disponibles dans {{site.data.keyword.containershort_notm}}.</caption>
<thead>
<th>Nom et cas d'utilisation</th>
<th>Coeurs/ Mémoire</th>
<th>Disque principal / secondaire</th>
<th>Vitesse réseau</th>
</thead>
<tbody>
<tr>
<td><strong>Virtuel, u2c.2x4</strong> : utilisez cette machine virtuelle de petite taille à des fins de test rapide, de démonstration de faisabilité et pour d'autres charges de travail légères.</td>
<td>2 / 4 Go</td>
<td>25 Go / 100 Go</td>
<td>1000 Mbit/s</td>
</tr>
<tr>
<td><strong>Virtuel, b2c.4x16</strong> : sélectionnez cette machine virtuelle équilibrée à des fins de test et de développement et pour d'autres charges de travail légères.</td>
<td>4 / 16 Go</td>
<td>25 Go / 100 Go</td>
<td>1000 Mbit/s</td>
</tr>
<tr>
<td><strong>Virtuel, b2c.16x64</strong> : sélectionnez cette machine virtuelle équilibrée pour les charges de travail de taille moyenne.</td></td>
<td>16 / 64 Go</td>
<td>25 Go / 100 Go</td>
<td>1000 Mbit/s</td>
</tr>
<tr>
<td><strong>Virtuel, b2c.32x128</strong> : sélectionnez cette machine virtuelle équilibrée pour les charges de travail de taille moyenne à élevée, par exemple une base de données et un site Web dynamique avec de nombreux utilisateurs simultanés.</td></td>
<td>32 / 128 Go</td>
<td>25 Go / 100 Go</td>
<td>1000 Mbit/s</td>
</tr>
<tr>
<td><strong>Virtuel, b2c.56x242</strong> : sélectionnez cette machine virtuelle équilibrée pour des charges de travail volumineuses, par exemple une base de données et plusieurs applications avec de nombreux utilisateurs simultanés.</td></td>
<td>56 / 242 Go</td>
<td>25 Go / 100 Go</td>
<td>1000 Mbit/s</td>
</tr>
<tr>
<td><strong>Bare metal à forte consommation de mémoire RAM, mr1c.28x512</strong> : augmentez au maximum la mémoire RAM disponible pour vos noeuds worker.</td>
<td>28 / 512 Go</td>
<td>SATA 2 To / SSD 960 Go</td>
<td>10000 Mbit/s</td>
</tr>
<tr>
<td><strong>Bare metal GPU, mg1c.16x128</strong> : Choisissez ce type de machine pour des charges de travail nécessitant de nombreux calculs mathématiques, tels que le calcul hautes performances, l'apprentissage automatique ou les applications en 3D. Cette version comporte 1 carte physique Tesla K80 avec 2 unités de traitement graphiques (GPU) par carte pour un total de 2 GPU.</td>
<td>16 / 128 Go</td>
<td>SATA 2 To / SSD 960 Go</td>
<td>10000 Mbit/s</td>
</tr>
<tr>
<td><strong>Bare metal GPU, mg1c.28x256</strong> : Choisissez ce type de machine pour des charges de travail nécessitant de nombreux calculs mathématiques, tels que le calcul hautes performances, l'apprentissage automatique ou les applications en 3D. Cette version possède 2 cartes physiques Tesla K80 avec 2 unités GPU par carte pour un total de 4 unités GPU.</td>
<td>28 / 256 Go</td>
<td>SATA 2 To / SSD 960 Go</td>
<td>10000 Mbit/s</td>
</tr>
<tr>
<td><strong>Bare metal à forte consommation de données, md1c.16x64.4x4tb</strong> : idéal pour une quantité substantielle de stockage sur disque local, y compris RAID, pour la sauvegarde de données stockées localement sur la machine. A utiliser pour les charges de travail de systèmes de fichiers répartis, de bases de données volumineuses ou d'analyse de big data.</td>
<td>16 / 64 Go</td>
<td>RAID1 2x2 To / RAID10 SATA 4x4 To</td>
<td>10000 Mbit/s</td>
</tr>
<tr>
<td><strong>Bare metal à forte consommation de données, md1c.28x512.4x4tb</strong> : idéal pour une quantité substantielle de stockage sur disque local, y compris RAID, pour la sauvegarde de données stockées localement sur la machine. A utiliser pour les charges de travail de systèmes de fichiers répartis, de bases de données volumineuses ou d'analyse de big data.</td>
<td>28 / 512 Go</td>
<td>RAID1 2x2 To / RAID10 SATA 4x4 To</td>
<td>10000 Mbit/s</td>
</tr>
<tr>
<td><strong>Bare metal équilibré, mb1c.4x32</strong> : A utiliser pour les charges de travail équilibrées qui nécessitent plus de ressources de calcul qu'en offrent les machines virtuelles.</td>
<td>4 / 32 Go</td>
<td>SATA 2 To / SATA 2 To</td>
<td>10000 Mbit/s</td>
</tr>
<tr>
<td><strong>Bare metal équilibré, mb1c.16x64</strong> : A utiliser pour les charges de travail équilibrées qui nécessitent plus de ressources de calcul qu'en offrent les machines virtuelles.</td>
<td>16 / 64 Go</td>
<td>SATA 2 To / SSD 960 Go</td>
<td>10000 Mbit/s</td>
</tr>
</tbody>
</table>
</p>
</dd>
</dl>


<strong>Options de commande</strong> :

   <dl>
   <dt><code><em>LOCATION</em></code></dt>
   <dd>Indiquez l'emplacement où répertorier les types de machine disponibles. Cette valeur est obligatoire. Passez en revue les [emplacements disponibles](cs_regions.html#locations).</dd></dl>

**Exemple de commande** :

  ```
  bx cs machine-types dal10
  ```
  {: pre}

**Exemple de sortie** :

  ```
  Getting machine types list...
  OK
  Machine Types
  Name                 Cores   Memory   Network Speed   OS             Server Type   Storage   Secondary Storage   Trustable
  u2c.2x4              2       4GB      1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  b2c.4x16             4       16GB     1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  b2c.16x64            16      64GB     1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  b2c.32x128           32      128GB    1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  b2c.56x242           56      242GB    1000Mbps        UBUNTU_16_64   virtual       25GB      100GB               False
  mb1c.4x32            4       32GB     10000Mbps       UBUNTU_16_64   physical      1000GB    2000GB              False
  mb1c.16x64           16      64GB     10000Mbps       UBUNTU_16_64   physical      1000GB    1700GB              False
  mr1c.28x512          28      512GB    10000Mbps       UBUNTU_16_64   physical      1000GB    1700GB              False
  md1c.16x64.4x4tb     16      64GB     10000Mbps       UBUNTU_16_64   physical      1000GB    8000GB              False
  md1c.28x512.4x4tb    28      512GB    10000Mbps       UBUNTU_16_64   physical      1000GB    8000GB              False
  
  ```
  {: screen}


### bx cs vlans LOCATION [--all]
{: #cs_vlans}

Répertoriez les VLAN publics et privés disponibles pour un emplacement dans votre compte d'infrastructure IBM Cloud (SoftLayer). Pour répertorier ces réseaux, vous devez disposer d'un compte payant.

<strong>Options de commande</strong> :

   <dl>
   <dt><code><em>LOCATION</em></code></dt>
   <dd>Indiquez l'emplacement où répertorier vos VLAN privés et publics. Cette valeur est obligatoire. Passez en revue les [emplacements disponibles](cs_regions.html#locations).</dd>
   <dt><code>--all</code></dt>
   <dd>Répertorie tous les VLAN disponibles. Par défaut, les VLAN sont filtrés pour n'afficher que les VLAN valides. Pour être valide, un VLAN doit être associé à l'infrastructure qui peut héberger un noeud worker avec un stockage sur disque local.</dd>
   </dl>

**Exemple** :

  ```
  bx cs vlans dal10
  ```
  {: pre}


<br />


## Commandes de consignation
{: #logging_commands}

### bx cs logging-config-create CLUSTER --logsource LOG_SOURCE [--namespace KUBERNETES_NAMESPACE][--hostname LOG_SERVER_HOSTNAME_OR_IP] [--port LOG_SERVER_PORT][--space CLUSTER_SPACE] [--org CLUSTER_ORG][--app-containers CONTAINERS] [--app-paths PATHS_TO_LOGS] --type LOG_TYPE [--json][--skip-validation]
{: #cs_logging_create}

Créez une configuration de consignation. Vous pouvez utiliser cette commande pour acheminer des journaux de conteneurs, applications, noeuds worker, clusters Kubernetes et équilibreurs de charge d'application Ingress à {{site.data.keyword.loganalysisshort_notm}} ou à un serveur syslog externe.

<strong>Options de commande</strong> :

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>Nom ou ID du cluster.</dd>
  <dt><code>--logsource <em>LOG_SOURCE</em></code></dt>
    <dd>Source de journal pour laquelle activer l'acheminement des journaux. Cet argument prend en charge une liste séparée par des virgules de sources de journal auxquelles appliquer la configuration. Valeurs admises : <code>container</code>, <code>application</code>, <code>worker</code>, <code>kubernetes</code> et <code>ingress</code>. Si vous ne fournissez pas de source de journal, les configurations de consignation sont créées pour les sources de journal <code>container</code> et <code>ingress</code>.</dd>
  <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
    <dd>Espace de nom Kubernetes depuis lequel vous désirez acheminer des journaux. L'acheminement des journaux n'est pas pris en charge pour les espaces de nom Kubernetes <code>ibm-system</code> et <code>kube-system</code>. Cette valeur n'est valide que pour la source de journal de conteneur et est facultative. Si vous n'indiquez pas d'espace de nom, tous les espaces de nom du cluster utilisent cette configuration.</dd>
  <dt><code>--hostname <em>LOG_SERVER_HOSTNAME</em></code></dt>
    <dd>Si le type de consignation correspond à <code>syslog</code>, nom d'hôte ou adresse ID du serveur collecteur de journal. Cette valeur est obligatoire pour <code>syslog</code>. Si le type de consignation correspond à <code>ibm</code>, URL d'ingestion {{site.data.keyword.loganalysislong_notm}}. Vous trouverez [ici](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls) la liste des URL d'ingestion disponibles. Si vous n'en indiquez aucune, le noeud final de la région où a été créé votre cluster est utilisé.</dd>
  <dt><code>--port <em>LOG_SERVER_PORT</em></code></dt>
    <dd>Port du serveur collecteur de journal. Cette valeur est facultative. Si vous ne spécifiez pas de port, le port standard <code>514</code> est utilisé pour <code>syslog</code> et le port standard <code>9091</code> pour <code>ibm</code>.</dd>
  <dt><code>--space <em>CLUSTER_SPACE</em></code></dt>
    <dd>Nom de l'espace Cloud Foundry auquel envoyer les journaux. Cette valeur est facultative et n'est valide que pour le type de journal <code>ibm</code>. Si vous n'indiquez aucun espace, les journaux sont envoyés au niveau du compte.</dd>
  <dt><code>--org <em>CLUSTER_ORG</em></code></dt>
    <dd>Nom de l'organisation Cloud Foundry où réside l'espace. Cette valeur n'est valide que pour le type de journal <code>ibm</code> et est obligatoire si vous avez spécifié un espace.</dd>
  <dt><code>--app-paths</code></dt>
    <dd>Chemin dans le conteneur utilisé par les applications pour la consignation. Pour transférer des journaux avec le type de source <code>application</code>, vous devez indiquer un chemin. Pour indiquer plusieurs chemins, utilisez une liste séparée par des virgules. Cette valeur est obligatoire pour la source de journal <code>application</code>. Exemple : <code>/var/log/myApp1/&ast;,/var/log/myApp2/&ast;</code></dd>
  <dt><code>--type <em>LOG_TYPE</em></code></dt>
    <dd>Destination de transfert de vos journaux. Les options possibles sont : <code>ibm</code> pour transférer vos journaux vers {{site.data.keyword.loganalysisshort_notm}} et <code>syslog</code> pour les transférer vers un serveur externe.</dd>
  <dt><code>--app-containers</code></dt>
    <dd>Facultatif : pour transférer les journaux à partir d'une application, vous pouvez indiquer le nom du conteneur contenant votre application. Vous pouvez spécifier plusieurs conteneurs en utilisant une liste séparée par des virgules. Si aucun conteneur n'est indiqué, les journaux sont transférés à partir de tous les conteneurs contenant les chemins que vous avez fournis. Cette option n'est valide que pour la source de journal <code>application</code></dt>
  <dt><code>--json</code></dt>
    <dd>Imprime le résultat de la commande au format JSON. Cette valeur est facultative.</dd>
  <dt><code>--skip-validation</code></dt>
    <dd>Ignore la validation des noms d'organisation et d'espace lorsqu'ils sont spécifiés. Cette opération permet de réduire le temps de traitement, mais une configuration de consignation non valide ne transfère pas correctement les journaux. Cette valeur est facultative.</dd>
</dl>

**Exemples** :

Exemple pour le type de journal `ibm` qui achemine les données depuis une source de journal `container` sur le port par défaut :

  ```
  bx cs logging-config-create my_cluster --logsource container --namespace my_namespace --hostname ingest.logging.ng.bluemix.net --type ibm
  ```
  {: pre}

Exemple pour le type de journal `syslog` acheminé depuis une source de journal `container` sur le port par défaut 514 :

  ```
  bx cs logging-config-create my_cluster --logsource container --namespace my_namespace  --hostname 169.xx.xxx.xxx --type syslog
  ```
  {: pre}

Exemple pour le type de journal `syslog` qui achemine des journaux depuis une source `ingress` sur un port différent de celui par défaut :

  ```
  bx cs logging-config-create my_cluster --logsource container --hostname 169.xx.xxx.xxx --port 5514 --type syslog
  ```
  {: pre}

### bx cs logging-config-get CLUSTER [--logsource LOG_SOURCE][--json]
{: #cs_logging_get}

Affichez toutes les configurations d'acheminement de journaux d'un cluster ou filtrez les configurations de consignation en fonction de la source de journal.

<strong>Options de commande</strong> :

 <dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>
  <dt><code>--logsource <em>LOG_SOURCE</em></code></dt>
    <dd>Type de source de journal que vous voulez filtrer. Seules les configurations de consignation de cette source de journal dans le cluster sont renvoyées. Valeurs admises : <code>container</code>, <code>application</code>, <code>worker</code>, <code>kubernetes</code> et <code>ingress</code>. Cette valeur est facultative.</dd>
  <dt><code>--json</code></dt>
    <dd>(Facultatif) Imprime le résultat de la commande au format JSON.</dd>
 </dl>

**Exemple** :

  ```
  bx cs logging-config-get my_cluster --logsource worker
  ```
  {: pre}


### bx cs logging-config-refresh CLUSTER
{: #cs_logging_refresh}

Actualise la configuration de consignation pour le cluster. Ceci actualise le jeton de consignation de toute configuration de consignation qui achemine des données au niveau de l'espace dans votre cluster.

<strong>Options de commande</strong> :

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
   <dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>
</dl>

**Exemple** :

  ```
  bx cs logging-config-refresh my_cluster
  ```
  {: pre}


### bx cs logging-config-rm CLUSTER [--id LOG_CONFIG_ID][--all]
{: #cs_logging_rm}

Supprimez une configuration d'acheminement des journaux ou toutes les configurations de consignation d'un cluster. Ceci cesse l'acheminement des journaux à un serveur syslog distant ou à {{site.data.keyword.loganalysisshort_notm}}.

<strong>Options de commande</strong> :

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
   <dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>
  <dt><code>--id <em>LOG_CONFIG_ID</em></code></dt>
   <dd>Si vous souhaitez supprimer une seule configuration de consignation, ID de la configuration de consignation.</dd>
  <dt><code>--all</code></dt>
   <dd>Indicateur permettant de supprimer toutes les configurations de consignation dans un cluster.</dd>
</dl>

**Exemple** :

  ```
  bx cs logging-config-rm my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e
  ```
  {: pre}


### bx cs logging-config-update CLUSTER --id LOG_CONFIG_ID [--namespace NAMESPACE][--hostname LOG_SERVER_HOSTNAME_OR_IP] [--port LOG_SERVER_PORT][--space CLUSTER_SPACE] [--org CLUSTER_ORG] --type LOG_TYPE [--json][--skipValidation]
{: #cs_logging_update}

Mettez à jour les détails d'une configuration d'acheminement des journaux.

<strong>Options de commande</strong> :

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
   <dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>
  <dt><code>--id <em>LOG_CONFIG_ID</em></code></dt>
   <dd>ID de configuration de consignation que vous souhaitez mettre à jour. Cette valeur est obligatoire.</dd>
  <dt><code>--namespace <em>NAMESPACE</em></code>
    <dd>Espace de nom Kubernetes depuis lequel vous désirez acheminer des journaux. L'acheminement des journaux n'est pas pris en charge pour les espaces de nom Kubernetes <code>ibm-system</code> et <code>kube-system</code>. Cette valeur n'est valide que pour la source de journal <code>container</code>. Si vous n'indiquez pas d'espace de nom, tous les espaces de nom du cluster utilisent cette configuration.</dd>
  <dt><code>--hostname <em>LOG_SERVER_HOSTNAME</em></code></dt>
   <dd>Si le type de consignation correspond à <code>syslog</code>, nom d'hôte ou adresse ID du serveur collecteur de journal. Cette valeur est obligatoire pour <code>syslog</code>. Si le type de consignation correspond à <code>ibm</code>, URL d'ingestion {{site.data.keyword.loganalysislong_notm}}. Vous trouverez [ici](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls) la liste des URL d'ingestion disponibles. Si vous n'en indiquez aucune, le noeud final de la région où a été créé votre cluster est utilisé.</dd>
   <dt><code>--port <em>LOG_SERVER_PORT</em></code></dt>
   <dd>Port du serveur collecteur de journal. Cette valeur est facultative lorsque le type de consignation est <code>syslog</code>. Si vous ne spécifiez pas de port, le port standard <code>514</code> est utilisé pour <code>syslog</code> et le port <code>9091</code> pour <code>ibm</code>.</dd>
   <dt><code>--space <em>CLUSTER_SPACE</em></code></dt>
   <dd>Nom de l'espace auquel vous désirez envoyer les journaux. Cette valeur est facultative et n'est valide que pour le type de journal <code>ibm</code>. Si vous n'indiquez aucun espace, les journaux sont envoyés au niveau du compte.</dd>
   <dt><code>--org <em>CLUSTER_ORG</em></code></dt>
   <dd>Nom de l'organisation où réside l'espace. Cette valeur n'est valide que pour le type de journal <code>ibm</code> et est obligatoire si vous avez spécifié un espace.</dd>
   <dt><code>--app-paths</code></dt>
     <dd>Ignore la validation des noms d'organisation et d'espace lorsqu'ils sont spécifiés. Cette opération permet de réduire le temps de traitement, mais une configuration de consignation non valide ne transfère pas correctement les journaux. Cette valeur est facultative.</dd>
   <dt><code>--app-containers</code></dt>
     <dd>Chemin dans les conteneurs utilisés par les applications pour la consignation. Pour transférer des journaux avec le type de source <code>application</code>, vous devez indiquer un chemin. Pour indiquer plusieurs chemins, utilisez une liste séparée par des virgules. Exemple : <code>/var/log/myApp1/&ast;,/var/log/myApp2/&ast;</code></dd>
   <dt><code>--type <em>LOG_TYPE</em></code></dt>
   <dd>Protocole de transfert de journal que vous souhaitez utiliser. Actuellement, <code>syslog</code> et <code>ibm</code> sont pris en charge. Cette valeur est obligatoire.</dd>
   <dt><code>--json</code></dt>
   <dd>(Facultatif) Imprime le résultat de la commande au format JSON.</dd>
   <dt><code>--skipValidation</code></dt>
   <dd>Ignore la validation des noms d'organisation et d'espace lorsqu'ils sont spécifiés. Cette opération permet de réduire le temps de traitement, mais une configuration de consignation non valide ne transfère pas correctement les journaux. Cette valeur est facultative.</dd>
   </dl>

**Exemple pour le type de journal `ibm`** :

  ```
  bx cs logging-config-update my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e --type ibm
  ```
  {: pre}

**Exemple pour le type de journal `syslog`** :

  ```
  bx cs logging-config-update my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e --hostname localhost --port 5514 --type syslog
  ```
  {: pre}


### bx cs logging-filter-create CLUSTER --type LOG_TYPE [--logging-configs CONFIGS][--namespace KUBERNETES_NAMESPACE] [--container CONTAINER_NAME][--level LOGGING_LEVEL] [--message MESSAGE][--s] [--json]
{: #cs_log_filter_create}

Créez un filtre de consignation. Cette commande vous permet de filtrer les journaux transférés par votre configuration de consignation.

<strong>Options de commande</strong> :

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>Obligatoire : nom ou ID du cluster pour lequel vous souhaitez créer un filtre de consignation.</dd>
  <dt><code>--type <em>LOG_TYPE</em></code></dt>
    <dd>Type des journaux auquel vous voulez appliquer le filtre. Les types <code>all</code>, <code>container</code> et <code>host</code> sont actuellement pris en charge.</dd>
  <dt><code>--logging-configs <em>CONFIGS</em></code></dt>
    <dd>Facultatif : liste séparée par des virgules contenant les ID de vos configurations de consignation. Si cette liste n'est pas fournie, le filtre s'applique à toutes les configurations de consignation du cluster qui sont transmises au filtre. Vous pouvez afficher les configurations de journal qui correspondent au filtre en utilisant l'indicateur <code>--show-matching-configs</code> avec la commande.</dd>
  <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
    <dd>Facultatif : espace de nom Kubernetes depuis lequel vous souhaitez filtrer les journaux.</dd>
  <dt><code>--container <em>CONTAINER_NAME</em></code></dt>
    <dd>Facultatif : nom du conteneur depuis lequel vous voulez filtrer les journaux. Cet indicateur s'applique uniquement lorsque vous utilisez le type de journal <code>container</code>.</dd>
  <dt><code>--level <em>LOGGING_LEVEL</em></code></dt>
    <dd>Facultatif : filtre les journaux dont le niveau est inférieur ou égal au niveau spécifié. Les valeurs admises, par ordre canonique, sont : <code>fatal</code>, <code>error</code>, <code>warn/warning</code>, <code>info</code>, <code>debug</code> et <code>trace</code>. Par exemple, si vous avez filtré les journaux au niveau <code>info</code>, les niveaux <code>debug</code> et <code>trace</code> sont également filtrés. **Remarque** : vous pouvez utiliser cet indicateur uniquement si les messages de journal sont au format JSON et contiennent une zone de niveau. Exemple de sortie : <code>{"log": "hello", "level": "info"}</code></dd>
  <dt><code>--message <em>MESSAGE</em></code></dt>
    <dd>Facultatif : filtre les journaux contenant un message particulier n'importe où dans le journal. Le message est mis en correspondance littéralement, et non en tant qu'expression. Exemple : les messages “Hello”, “!”et “Hello, World!”s'appliqueront au journal “Hello, World!”.</dd>
  <dt><code>--json</code></dt>
    <dd>Facultatif : imprime le résultat de la commande au format JSON.</dd>
</dl>

**Exemples** :

L'exemple suivant permet de filtrer tous les journaux transmis à partir de conteneurs nommés `test-container` dans l'espace de nom par défaut dont le niveau est debug ou inférieur et dont le message de journal contient "GET request".

  ```
  bx cs logging-filter-create example-cluster --type container --namespace default --container test-container --level debug --message "GET request"
  ```
  {: pre}

L'exemple suivant permet de filtrer tous les journaux transférés, de niveau info ou inférieur, à partir d'un cluster spécifique. La sortie est renvoyée au format JSON.

  ```
  bx cs logging-filter-create example-cluster --type all --level info --json
  ```
  {: pre}

### bx cs logging-filter-update CLUSTER --type LOG_TYPE [--logging-configs CONFIGS][--namespace KUBERNETES_NAMESPACE] [--container CONTAINER_NAME][--level LOGGING_LEVEL] [--message MESSAGE][--s] [--json]
{: #cs_log_filter_update}

Mettez à jour un filtre de consignation. Vous pouvez utiliser cette commande pour mettre à jour un filtre de consignation que vous avez créé.

<strong>Options de commande</strong> :

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>Obligatoire : nom ou ID du cluster pour lequel vous souhaitez mettre à jour un filtre de consignation.</dd>
  <dt><code>--type <em>LOG_TYPE</em></code></dt>
    <dd>Type des journaux auquel vous voulez appliquer le filtre. Les types <code>all</code>, <code>container</code> et <code>host</code> sont actuellement pris en charge.</dd>
  <dt><code>--logging-configs <em>CONFIGS</em></code></dt>
    <dd>Facultatif : liste séparée par des virgules contenant les ID de vos configurations de consignation. Si cette liste n'est pas fournie, le filtre s'applique à toutes les configurations de consignation du cluster qui sont transmises au filtre. Vous pouvez afficher les configurations de journal qui correspondent au filtre en utilisant l'indicateur <code>--show-matching-configs</code> avec la commande.</dd>
  <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
    <dd>Facultatif : espace de nom Kubernetes depuis lequel vous souhaitez filtrer les journaux.</dd>
  <dt><code>--container <em>CONTAINER_NAME</em></code></dt>
    <dd>Facultatif : nom du conteneur depuis lequel vous voulez filtrer les journaux. Cet indicateur s'applique uniquement lorsque vous utilisez le type de journal <code>container</code>.</dd>
  <dt><code>--level <em>LOGGING_LEVEL</em></code></dt>
    <dd>Facultatif : filtre les journaux dont le niveau est inférieur ou égal au niveau spécifié. Les valeurs admises, par ordre canonique, sont : <code>fatal</code>, <code>error</code>, <code>warn/warning</code>, <code>info</code>, <code>debug</code> et <code>trace</code>. Par exemple, si vous avez filtré les journaux au niveau <code>info</code>, les niveaux <code>debug</code> et <code>trace</code> sont également filtrés. **Remarque** : vous pouvez utiliser cet indicateur uniquement si les messages de journal sont au format JSON et contiennent une zone de niveau. Exemple de sortie : <code>{"log": "hello", "level": "info"}</code></dd>
  <dt><code>--message <em>MESSAGE</em></code></dt>
    <dd>Facultatif : filtre les journaux contenant un message particulier n'importe où dans le journal. Le message est mis en correspondance littéralement, et non en tant qu'expression. Exemple : les messages “Hello”, “!”et “Hello, World!”s'appliqueront au journal “Hello, World!”.</dd>
  <dt><code>--json</code></dt>
    <dd>Facultatif : imprime le résultat de la commande au format JSON.</dd>
</dl>


### bx cs logging-filter-get CLUSTER [--id FILTER_ID][--show-matching-configs] [--json]
{: #cs_log_filter_view}

Affichez une configuration de filtre de consignation. Vous pouvez utiliser cette commande pour afficher les filtres de consignation que vous avez créés.

<strong>Options de commande</strong> :

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>Obligatoire : nom ou ID du cluster à partir duquel vous souhaitez afficher les filtres.</dd>
  <dt><code>--id <em>FILTER_ID</em></code></dt>
    <dd>ID du filtre de journal que vous désirez afficher.</dd>
  <dt><code>--show-matching-configs</code></dt>
    <dd>Facultatif : affiche les configurations de consignation qui correspondent à la configuration que vous consultez actuellement.</dd>
  <dt><code>--json</code></dt>
    <dd>Facultatif : imprime le résultat de la commande au format JSON.</dd>
</dl>


### bx cs logging-filter-rm CLUSTER [--id FILTER_ID][--json] [--all]
{: #cs_log_filter_delete}

Supprimez un filtre de consignation. Vous pouvez utiliser cette commande pour supprimer un filtre de consignation que vous avez créé.

<strong>Options de commande</strong> :

<dl>
  <dt><code><em>CLUSTER</em></code></dt>
    <dd>Nom ou ID du cluster duquel vous souhaitez supprimer un filtre.</dd>
  <dt><code>--id <em>FILTER_ID</em></code></dt>
    <dd>ID du filtre de journal à supprimer.</dd>
  <dt><code>--all</code></dt>
    <dd>Facultatif : supprime tous vos filtres d'acheminement de journaux.</dd>
  <dt><code>--json</code></dt>
    <dd>Facultatif : imprime le résultat de la commande au format JSON.</dd>
</dl>

<br />


## Commandes de région
{: #region_commands}

### bx cs locations
{: #cs_datacenters}

Affichez la liste de tous les emplacements disponibles pour créer un cluster.

<strong>Options de commande</strong> :

   Aucune

**Exemple** :

  ```
  bx cs locations
  ```
  {: pre}


### bx cs region
{: #cs_region}

Identifiez la région {{site.data.keyword.containershort_notm}} où vous êtes actuellement situé. Vous pouvez créer et gérer des clusters spécifiques à la région. Utilisez la commande `bx cs region-set` pour changer de région.

**Exemple** :

```
bx cs region
```
{: pre}

**Sortie **:
```
Region: us-south
```
{: screen}

### bx cs region-set [REGION]
{: #cs_region-set}

Définissez la région pour {{site.data.keyword.containershort_notm}}. Vous pouvez créer et gérer des clusters spécifiques à la région, et aussi désirer disposer de clusters dans plusieurs régions en vue d'une haute disponibilité.

Par exemple, vous pourriez vous connecter à {{site.data.keyword.Bluemix_notm}} dans la région Sud des Etats-Unis et y créer un cluster. Vous pourriez ensuite utiliser la commande `bx cs region-set eu-central` pour cibler la région Europe centrale et y créer un autre cluster. Enfin, vous pourriez utiliser la commande `bx cs region-set us-south` pour revenir à la région Sud des Etats-Unis et gérer votre cluster dans cette région.

**Options de commande** :

<dl>
<dt><code><em>REGION</em></code></dt>
<dd>Entrez la région que vous désirez cibler. Cette valeur est facultative. Si vous n'indiquez pas la région, vous pouvez la sélectionner dans la liste figurant dans la sortie.

Pour la liste des régions disponibles, consultez la rubrique [régions et emplacements](cs_regions.html) ou utilisez la commande `bx cs regions` [](#cs_regions).</dd></dl>

**Exemple** :

```
bx cs region-set eu-central
```
{: pre}

```
bx cs region-set
```
{: pre}

**Sortie **:
```
Choose a region:
1. ap-north
2. ap-south
3. eu-central
4. uk-south
5. us-east
6. us-south
Enter a number> 3
OK
```
{: screen}

### bx cs regions
{: #cs_regions}

Répertorie les régions disponibles. La zone `Region Name` indique le nom du {{site.data.keyword.containershort_notm}} et la zone `Region Alias` est le nom {{site.data.keyword.Bluemix_notm}} général pour la région.

**Exemple** :

```
bx cs regions
```
{: pre}

**Sortie **:
```
Region Name   Region Alias
ap-north      jp-tok
ap-south      au-syd
eu-central    eu-de
uk-south      eu-gb
us-east       us-east
us-south      us-south
```
{: screen}


<br />


## Commandes de noeud worker
{: worker_node_commands}



### bx cs worker-add --cluster CLUSTER [--file FILE_LOCATION][--hardware HARDWARE] --machine-type MACHINE_TYPE --number NUMBER --private-vlan PRIVATE_VLAN --public-vlan PUBLIC_VLAN [--disable-disk-encrypt]
{: #cs_worker_add}

Ajoutez des noeuds worker à votre cluster standard.

<strong>Options de commande</strong> :

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

<dt><code>--file <em>FILE_LOCATION</em></code></dt>
<dd>Chemin d'accès au fichier YAML pour ajouter des noeuds worker au cluster. Au lieu de définir des noeuds worker supplémentaires à l'aide des options fournies dans cette commande, vous pouvez utiliser un fichier YAML. Cette valeur est facultative.

<p><strong>Remarque :</strong> si vous indiquez la même option dans la commande comme paramètre dans le fichier YAML, la valeur de l'option de la commande est prioritaire sur la valeur définie dans le fichier YAML. Par exemple, si vous définissez un type de machine dans votre fichier YAML et que vous utilisez l'option --machine-type dans la commande, la valeur que vous avez entrée dans l'option de commande se substitue à la valeur définie dans le fichier YAML.

<pre class="codeblock">
<code>name: <em>&lt;cluster_name_or_ID&gt;</em>
location: <em>&lt;location&gt;</em>
machine-type: <em>&lt;machine_type&gt;</em>
private-vlan: <em>&lt;private_VLAN&gt;</em>
public-vlan: <em>&lt;public_VLAN&gt;</em>
hardware: <em>&lt;shared_or_dedicated&gt;</em>
workerNum: <em>&lt;number_workers&gt;</em>
diskEncryption: <em>false</em></code></pre>

<table>
<caption>Tableau 2. Description des composants du fichier YAML</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants du fichier YAML</th>
</thead>
<tbody>
<tr>
<td><code><em>name</em></code></td>
<td>Remplacez <code><em>&lt;cluster_name_or_ID&gt;</em></code> par le nom ou l'ID du cluster sur lequel vous souhaitez ajouter des noeuds worker.</td>
</tr>
<tr>
<td><code><em>location</em></code></td>
<td>Remplacez <code><em>&lt;location&gt;</em></code> par l'emplacement où déployer vos noeuds worker. Les emplacements disponibles dépendent de la région à laquelle vous êtes connecté. Pour afficher la liste des emplacements disponibles, exécutez la commande <code>bx cs locations</code>.</td>
</tr>
<tr>
<td><code><em>machine-type</em></code></td>
<td>Remplacez <code><em>&lt;machine_type&gt;</em></code> par le type de machine sur lequel vous envisagez de déployer vos noeuds worker. Vous pouvez déployer vos noeuds worker en tant que machines virtuelles sur du matériel partagé ou dédié ou en tant que machines physiques sur un serveur bare metal. Les types de machines virtuelles et physiques disponibles varient en fonction de l'emplacement de déploiement du cluster. Pour plus d'informations, voir la [commande](cs_cli_reference.html#cs_machine_types) `bx cs machine-types`.</td>
</tr>
<tr>
<td><code><em>private-vlan</em></code></td>
<td>Remplacez <code><em>&lt;private_VLAN&gt;</em></code> par l'ID du réseau local virtuel privé que vous souhaitez utiliser pour vos noeuds worker. Pour afficher la liste des réseaux locaux virtuels disponibles, exécutez la commande <code>bx cs vlans <em>&lt;location&gt;</em></code> et recherchez les routeurs VLAN débutant par <code>bcr</code> (routeur de back-end).</td>
</tr>
<tr>
<td><code>public-vlan</code></td>
<td>Remplacez <code>&lt;public_VLAN&gt;</code> par l'ID du réseau local virtuel public que vous souhaitez utiliser pour vos noeuds worker. Pour afficher la liste des réseaux locaux virtuels disponibles, exécutez la commande <code>bx cs vlans &lt;location&gt;</code> et recherchez les routeurs VLAN débutant par <code>fcr</code> (routeur de front-end). <br><strong>Remarque</strong> : {[private_VLAN_vyatta]}</td>
</tr>
<tr>
<td><code>hardware</code></td>
<td>Pour les types de machine virtuelle : niveau d'isolement de votre noeud worker. Utilisez un cluster dédié si vous désirez que toutes les ressources physiques vous soient dédiées exclusivement ou un cluster partagé pour permettre leur partage avec d'autres clients IBM. La valeur par défaut est shared.</td>
</tr>
<tr>
<td><code>workerNum</code></td>
<td>Remplacez <code><em>&lt;number_workers&gt;</em></code> par le nombre de noeuds worker que vous souhaitez déployer.</td>
</tr>
<tr>
<td><code>diskEncryption: <em>false</em></code></td>
<td>Les noeuds worker disposent par défaut du chiffrement de disque. [En savoir plus](cs_secure.html#worker). Pour désactiver le chiffrement, incluez cette option en lui attribuant la valeur <code>false</code>.</td></tr>
</tbody></table></p></dd>

<dt><code>--hardware <em>HARDWARE</em></code></dt>
<dd>Niveau d'isolation du matériel pour votre noeud worker. Utilisez un cluster dédié si vous désirez que toutes les ressources physiques vous soient dédiées exclusivement ou un cluster partagé pour permettre leur partage avec d'autres clients IBM. La valeur par défaut est shared. Cette valeur est facultative.</dd>

<dt><code>--machine-type <em>MACHINE_TYPE</em></code></dt>
<dd>Choisissez un type de machine. Vous pouvez déployer vos noeuds worker en tant que machines virtuelles sur du matériel partagé ou dédié ou en tant que machines physiques sur un serveur bare metal. Les types de machines virtuelles et physiques disponibles varient en fonction de l'emplacement de déploiement du cluster. Pour plus d'informations, voir la documentation correspondant à la [commande](cs_cli_reference.html#cs_machine_types) `bx cs machine-types`. Cette valeur est obligatoire pour les clusters standard et n'est pas disponible pour les clusters gratuits.</dd>

<dt><code>--number <em>NUMBER</em></code></dt>
<dd>Entier représentant le nombre de noeuds worker à créer dans le cluster. La valeur par défaut est 1. Cette valeur est facultative.</dd>

<dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
<dd>VLAN privé spécifié lors de la création du cluster. Cette valeur est obligatoire.

<p><strong>Remarque</strong> : les routeurs de VLAN privé commencent toujours par <code>bcr</code> (routeur de back-end) et les routeurs de VLAN public par <code>fcr</code> (routeur de front-end). Lors de la création d'un cluster et de la spécification des VLAN publics et privés, le nombre et la combinaison de lettres après ces préfixes doivent correspondre.</p></dd>

<dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
<dd>VLAN public spécifié lors de la création du cluster. Cette valeur est facultative. Si vous désirez que vos noeuds worker ne soient présents que sur un réseau virtuel local privé, n'indiquez pas d'ID de réseau virtuel local public. <strong>Remarque</strong> : {[private_VLAN_vyatta]}

<p><strong>Remarque</strong> : les routeurs de VLAN privé commencent toujours par <code>bcr</code> (routeur de back-end) et les routeurs de VLAN public par <code>fcr</code> (routeur de front-end). Lors de la création d'un cluster et de la spécification des VLAN publics et privés, le nombre et la combinaison de lettres après ces préfixes doivent correspondre.</p></dd>

<dt><code>--disable-disk-encrypt</code></dt>
<dd>Les noeuds worker disposent par défaut du chiffrement de disque. [En savoir plus](cs_secure.html#worker). Pour désactiver le chiffrement, incluez cette option.</dd>
</dl>

**Exemples** :

  ```
  bx cs worker-add --cluster my_cluster --number 3 --public-vlan my_public_VLAN_ID --private-vlan my_private_VLAN_ID --machine-type u2c.2x4 --hardware shared
  ```
  {: pre}

  Exemple pour {{site.data.keyword.Bluemix_dedicated_notm}} :

  ```
  bx cs worker-add --cluster my_cluster --number 3 --machine-type u2c.2x4
  ```
  {: pre}




### bx cs worker-get [CLUSTER_NAME_OR_ID] WORKER_NODE_ID
{: #cs_worker_get}

Affichez les détails d'un noeud worker.

<strong>Options de commande</strong> :

   <dl>
   <dt><code><em>CLUSTER_NAME_OR_ID</em></code></dt>
   <dd>Nom ou ID du cluster du noeud worker. Cette valeur est facultative.</dd>
   <dt><code><em>WORKER_NODE_ID</em></code></dt>
   <dd>Nom de votre noeud worker. Exécutez la commande <code>bx cs workers <em>CLUSTER</em></code> pour afficher les ID des noeuds worker dans un cluster. Cette valeur est obligatoire.</dd>
   </dl>

**Exemple de commande** :

  ```
  bx cs worker-get my_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1
  ```
  {: pre}

**Exemple de sortie** :

  ```
  ID:           kube-dal10-123456789-w1
  State:        normal
  Status:       Ready
  Trust:        disabled
  Private VLAN: 223xxxx
  Public VLAN:  223xxxx
  Private IP:   10.xxx.xx.xxx
  Public IP:    169.xx.xxx.xxx
  Hardware:     shared
  Zone:         dal10
  Version:      1.8.11_1509
  ```
  {: screen}

### bx cs worker-reboot [-f][--hard] CLUSTER WORKER [WORKER]
{: #cs_worker_reboot}

Redémarrez un noeud worker dans un cluster. Lors du redémarrage, l'état de votre noeud worker reste inchangé.

**Attention :** le redémarrage d'un noeud worker peut entraîner l'altération des données sur le noeud worker. Utilisez cette commande avec précaution et lorsque vous savez qu'un redémarrage pourra vous aider à restaurer le noeud worker. Dans tous les autres cas de figure,  [rechargez votre noeud worker](#cs_worker_reload) à la place.

Avant de redémarrer le noeud worker, assurez-vous que les pods sont replanifiés sur d'autres noeuds worker afin d'éviter toute indisponibilité de votre application ou l'altération des données sur votre noeud worker.

1. Répertoriez tous les noeuds worker présents dans votre cluster et notez le **nom** de celui que vous envisagez de redémarrer.
   ```
   kubectl get nodes
   ```
   Le **nom** renvoyé dans cette commande correspond à l'adresse IP privée affectée à votre noeud worker. Vous pouvez obtenir plus d'informations sur votre noeud worker lorsque vous exécutez la commande `bx cs workers <cluster_name_or_ID>` et recherchez le noeud worker avec la même adresse **IP privée**.
2. Marquez le noeud worker comme non planifiable dans un processus désigné par cordon. Lorsque vous exécutez ce processus sur un noeud worker, vous le rendez indisponible pour toute planification de pod ultérieure. Utilisez le **nom** du noeud worker que vous avez récupéré à l'étape précédente.
   ```
   kubectl cordon <worker_name>
   ```
   {: pre}

3. Vérifiez que la planification de pod est désactivée pour votre noeud worker.
   ```
   kubectl get nodes
   ```
   {: pre}
   Votre noeud worker ne sera pas activé pour la planification de pod si le statut affiché est **SchedulingDisabled**.
 4. Imposez le retrait des pods de votre noeud worker et leur replanification sur les noeuds worker restants dans le cluster.
    ```
    kubectl drain <worker_name>
    ```
    {: pre}
    Ce processus peut prendre quelques minutes.
 5. Redémarrez le noeud worker. Utilisez l'ID du noeud worker renvoyé dans la commande `bx cs workers <cluster_name_or_ID>`.
    ```
    bx cs worker-reboot <cluster_name_or_ID> <worker_name_or_ID>
    ```
    {: pre}
 6. Patientez environ 5 minutes avant de rendre votre noeud worker disponible pour la planification de pod pour vous assurer que le redémarrage est terminé. Lors du redémarrage, l'état de votre noeud worker reste inchangé. Le redémarrage d'un noeud worker s'effectue en principe en quelques secondes.
 7. Rendez votre noeud worker disponible pour la planification de pod. Utilisez le **nom** de noeud worker renvoyé par la commande `kubectl get nodes`.
    ```
    kubectl uncordon <worker_name>
    ```
    {: pre}
    </br>

<strong>Options de commande</strong> :

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

   <dt><code>-f</code></dt>
   <dd>Utilisez cette option pour forcer le redémarrage du noeud worker sans invites utilisateur. Cette valeur est facultative.</dd>

   <dt><code>--hard</code></dt>
   <dd>Utilisez cette option pour forcer un redémarrage à froid d'un noeud worker en coupant son alimentation. Utilisez cette option si le noeud worker ne répond plus ou connaît un blocage Docker. Cette valeur est facultative.</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>Nom ou ID d'un ou de plusieurs noeuds worker. Utilisez un espace pour répertorier plusieurs noeuds worker. Cette valeur est obligatoire.</dd>
   </dl>

**Exemple** :

  ```
  bx cs worker-reboot my_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}


### bx cs worker-reload [-f] CLUSTER WORKER [WORKER]
{: #cs_worker_reload}

Rechargez toutes les configurations nécessaires relatives à un noeud worker. Un rechargement peut s'avérer utile en cas de problème sur votre noeud worker, par exemple une dégradation des performances ou une immobilisation dans un mauvais état de santé.

Le rechargement d'un noeud worker applique des mises à jour de version de correctif à votre noeud worker mais pas de mise à jour principale ou secondaire. Pour voir les modifications d'une version de correctif à une autre, consultez la documentation [Journal des modifications de version](cs_versions_changelog.html#changelog).
{: tip}

Avant de recharger le noeud worker, assurez-vous que les pods sont replanifiés sur d'autres noeuds worker afin d'éviter toute indisponibilité de votre application ou l'altération des données sur votre noeud worker.

1. Répertoriez tous les noeuds worker dans votre cluster et notez le **nom** de celui que vous envisagez de recharger.
   ```
   kubectl get nodes
   ```
   Le **nom** renvoyé dans cette commande correspond à l'adresse IP privée affectée à votre noeud worker. Vous pouvez obtenir plus d'informations sur votre noeud worker lorsque vous exécutez la commande `bx cs workers <cluster_name_or_ID>` et recherchez le noeud worker avec la même adresse **IP privée**.
2. Marquez le noeud worker comme non planifiable dans un processus désigné par cordon. Lorsque vous exécutez ce processus sur un noeud worker, vous le rendez indisponible pour toute planification de pod ultérieure. Utilisez le **nom** du noeud worker que vous avez récupéré à l'étape précédente.
   ```
   kubectl cordon <worker_name>
   ```
   {: pre}

3. Vérifiez que la planification de pod est désactivée pour votre noeud worker.
   ```
   kubectl get nodes
   ```
   {: pre}
   Votre noeud worker ne sera pas activé pour la planification de pod si le statut affiché est **SchedulingDisabled**.
 4. Imposez le retrait des pods de votre noeud worker et leur replanification sur les noeuds worker restants dans le cluster.
    ```
    kubectl drain <worker_name>
    ```
    {: pre}
    Ce processus peut prendre quelques minutes.
 5. Rechargez le noeud worker. Utilisez l'ID du noeud worker renvoyé dans la commande `bx cs workers <cluster_name_or_ID>`.
    ```
    bx cs worker-reload <cluster_name_or_ID> <worker_name_or_ID>
    ```
    {: pre}
 6. Patientez jusqu'à la fin du rechargement.
 7. Rendez votre noeud worker disponible pour la planification de pod. Utilisez le **nom** de noeud worker renvoyé par la commande `kubectl get nodes`.
    ```
    kubectl uncordon <worker_name>
    ```
</br>
<strong>Options de commande</strong> :

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

   <dt><code>-f</code></dt>
   <dd>Utilisez cette option pour forcer le rechargement d'un noeud worker sans invites utilisateur. Cette valeur est facultative.</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>Nom ou ID d'un ou de plusieurs noeuds worker. Utilisez un espace pour répertorier plusieurs noeuds worker. Cette valeur est obligatoire.</dd>
   </dl>

**Exemple** :

  ```
  bx cs worker-reload my_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}


### bx cs worker-rm [-f] CLUSTER WORKER [WORKER]
{: #cs_worker_rm}

Supprimez un ou plusieurs noeuds worker d'un cluster. Si vous supprimez un noeud worker, votre cluster n'est plus équilibré. 

Avant de supprimer le noeud worker, assurez-vous que les pods sont replanifiés sur d'autres noeuds worker afin d'éviter toute indisponibilité de votre application ou l'altération des données sur votre noeud worker.
{: tip}

1. Répertoriez tous les noeuds worker dans votre cluster et notez le **nom** de celui que vous envisagez de supprimer.
   ```
   kubectl get nodes
   ```
   Le **nom** renvoyé dans cette commande correspond à l'adresse IP privée affectée à votre noeud worker. Vous pouvez obtenir plus d'informations sur votre noeud worker lorsque vous exécutez la commande `bx cs workers <cluster_name_or_ID>` et recherchez le noeud worker avec la même adresse **IP privée**.
2. Marquez le noeud worker comme non planifiable dans un processus désigné par cordon. Lorsque vous exécutez ce processus sur un noeud worker, vous le rendez indisponible pour toute planification de pod ultérieure. Utilisez le **nom** du noeud worker que vous avez récupéré à l'étape précédente.
   ```
   kubectl cordon <worker_name>
   ```
   {: pre}

3. Vérifiez que la planification de pod est désactivée pour votre noeud worker.
   ```
   kubectl get nodes
   ```
   {: pre}
   Votre noeud worker ne sera pas activé pour la planification de pod si le statut affiché est **SchedulingDisabled**.
4. Imposez le retrait des pods de votre noeud worker et leur replanification sur les noeuds worker restants dans le cluster.
   ```
   kubectl drain <worker_name>
   ```
   {: pre}
   Ce processus peut prendre quelques minutes.
5. Supprimez le noeud worker. Utilisez l'ID du noeud worker renvoyé dans la commande `bx cs workers <cluster_name_or_ID>`.
   ```
   bx cs worker-rm <cluster_name_or_ID> <worker_name_or_ID>
   ```
   {: pre}

6. Vérifiez que le noeud worker est supprimé.
   ```
   bx cs workers <cluster_name_or_ID>
   ```
</br>
<strong>Options de commande</strong> :

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>Nom ou ID du cluster. Cette valeur est obligatoire.</dd>

   <dt><code>-f</code></dt>
   <dd>Utilisez cette option pour forcer la suppression d'un noeud worker sans invites utilisateur. Cette valeur est facultative.</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>Nom ou ID d'un ou de plusieurs noeuds worker. Utilisez un espace pour répertorier plusieurs noeuds worker. Cette valeur est obligatoire.</dd>
   </dl>

**Exemple** :

  ```
  bx cs worker-rm my_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}




### bx cs worker-update [-f] CLUSTER WORKER [WORKER][--kube-version MAJOR.MINOR.PATCH] [--force-update]
{: #cs_worker_update}

Mettez à jour les noeuds worker pour appliquer les correctifs et mises à jour de sécurité les plus récents sur le système d'exploitation et mettre à jour la version Kubernetes de sorte à ce qu'elle corresponde à celle du noeud maître. Vous pouvez mettre à jour la version du noeud maître Kubernetes avec la [commande](cs_cli_reference.html#cs_cluster_update) `bx cs cluster-update`.

**Important** : l'exécution de la commande `bx cs worker-update` peut entraîner l'indisponibilité de vos services et applications. Lors de la mise à jour, tous les pods sont replanifiés sur d'autres noeuds worker et les données sont supprimées si elles elles ne sont pas stockées hors du pod. Pour éviter des temps d'indisponibilité, [vérifiez que vous disposez de suffisamment de noeuds worker pour traiter votre charge de travail alors que les noeuds worker sélectionnés sont en cours de mise à jour](cs_cluster_update.html#worker_node).

Vous pourriez devoir modifier vos fichiers YAML en vue des déploiements avant la mise à jour. Consultez cette [note sur l'édition](cs_versions.html) pour plus de détails.

<strong>Options de commande</strong> :

   <dl>

   <dt><em>CLUSTER</em></dt>
   <dd>Nom ou ID du cluster sur lequel répertorier les noeuds worker disponibles. Cette valeur est obligatoire.</dd>

   <dt><code>-f</code></dt>
   <dd>Utilisez cette option pour forcer la mise à jour du maître sans invites utilisateur. Cette valeur est facultative.</dd>

   <dt><code>--force-update</code></dt>
   <dd>Tentative de mise à jour alors que la modification est supérieure à deux niveaux de version secondaire. Cette valeur est facultative.</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>ID d'un ou de plusieurs noeuds worker. Utilisez un espace pour répertorier plusieurs noeuds worker. Cette valeur est obligatoire.</dd>
   </dl>

**Exemple** :

  ```
  bx cs worker-update my_cluster kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w1 kube-dal10-cr18a61a63a6a94b658596aa93d087aaa9-w2
  ```
  {: pre}



### bx cs workers CLUSTER [--show-deleted]
{: #cs_workers}

Affichez la liste des noeuds worker d'un cluster et le statut de chacun d'eux.

<strong>Options de commande</strong> :

   <dl>
   <dt><em>CLUSTER</em></dt>
   <dd>Nom ou ID du cluster pour les noeuds worker disponibles. Cette valeur est obligatoire.</dd>
   <dt><em>--show-deleted</em></dt>
   <dd>Affiche les noeuds worker qui ont été supprimés du cluster, y compris la raison de la suppression. Cette valeur est facultative.</dd>
   </dl>

**Exemple** :

  ```
  bx cs workers my_cluster
  ```
  {: pre}
