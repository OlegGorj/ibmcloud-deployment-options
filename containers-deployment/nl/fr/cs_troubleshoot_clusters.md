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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}



# Traitement des incidents liés aux clusters et aux noeuds worker
{: #cs_troubleshoot_clusters}

Lorsque vous utilisez {{site.data.keyword.containerlong}}, tenez compte de ces techniques pour identifier et résoudre les incidents liés à vos clusters et à vos noeuds worker.
{: shortdesc}

Si vous rencontrez un problème d'ordre plus général, expérimentez le [débogage de cluster](cs_troubleshoot.html).
{: tip}

## Impossible de se connecter à votre compte d'infrastructure
{: #cs_credentials}

{: tsSymptoms}
Lorsque vous créez un nouveau cluster Kubernetes, vous rencontrez le message suivant.

```
Nous n'avons pas pu nous connecter à votre compte d'infrastructure IBM Cloud.
La création d'un cluster standard nécessite un compte de type Paiement à la carte lié aux
conditions d'un compte de l'infrastructure IBM Cloud (SoftLayer) ou l'utilisation de l'interface de ligne de commande {{site.data.keyword.containerlong}} pour définir vos clés d'API d'infrastructure {{site.data.keyword.Bluemix_notm}}.
```
{: screen}

{: tsCauses}
Les comptes Paiement à la carte {{site.data.keyword.Bluemix_notm}} créés après l'activation de la liaison automatique de compte sont déjà configurés avec l'accès au portefeuille d'infrastructure IBM Cloud (SoftLayer). Vous pouvez acheter des ressources d'infrastructure pour votre cluster sans configuration supplémentaire.

Les utilisateurs disposant d'autres types de compte {{site.data.keyword.Bluemix_notm}}, ou disposant d'un compte d'infrastructure IBM Cloud (SoftLayer) existant non lié à leur compte {{site.data.keyword.Bluemix_notm}} doivent configurer leurs comptes pour créer des clusters standard. 

{: tsResolve}
La configuration de votre compte pour accéder au portefeuille d'infrastructure IBM Cloud (SoftLayer) dépend du type de compte dont vous disposez. Consultez ce tableau pour identifier les options disponibles pour chaque type de compte.

|Type de compte|Description|Options disponibles pour création d'un cluster standard|
|------------|-----------|----------------------------------------------|
|Comptes Lite|Les comptes Lite ne peuvent pas provisionner des clusters.|[Mettez à niveau votre compte Lite vers un compte {{site.data.keyword.Bluemix_notm}}  Pay-As-You-Go](/docs/account/index.html#paygo) (Paiement à la carte) configuré avec accès au portefeuille d'infrastructure IBM Cloud (SoftLayer).|
|Anciens comptes Paiement à la carte|Les comptes Paiement à la carte créés avant que la liaison automatique de compte ne soit disponible ne disposaient pas d'un accès au portefeuille d'infrastructure IBM Cloud (SoftLayer).<p>Si vous disposez d'un compte d'infrastructure IBM Cloud (SoftLayer) existant, vous ne pouvez pas le lier à un ancien compte Paiement à la carte.</p>|<strong>Option 1 :</strong> [Créez un compte Paiement à la carte](/docs/account/index.html#paygo) configuré avec accès au portefeuille d'infrastructure IBM Cloud (SoftLayer). Si vous sélectionnez cette option, vous disposerez de deux comptes {{site.data.keyword.Bluemix_notm}} avec facturation distincte.<p>Pour continuer à utiliser votre ancien compte Paiement à la carte, vous pouvez utiliser votre nouveau compte Paiement à la carte pour générer une clé d'API permettant d'accéder au portefeuille d'infrastructure IBM Cloud (SoftLayer). Vous devez ensuite [définir la clé d'API de l'infrastructure IBM Cloud (SoftLayer) pour votre ancien compte Paiement à la carte](cs_cli_reference.html#cs_credentials_set). </p><p><strong>Option 2 :</strong> Si vous disposez déjà d'un compte d'infrastructure IBM Cloud (SoftLayer) existant que vous désirez utiliser, vous pouvez [définir vos données d'identification](cs_cli_reference.html#cs_credentials_set) dans votre compte {{site.data.keyword.Bluemix_notm}}.</p><p>**Remarque :** lorsque vous effectuez une liaison manuelle vers un compte d'infrastructure IBM Cloud (SoftLayer), les données d'identification sont utilisées pour toutes les actions spécifiques de l'infrastructure IBM Cloud (SoftLayer) effectuées dans votre compte {{site.data.keyword.Bluemix_notm}}. Vous devez vérifier que la clé d'API que vous avez définie dispose de [droits d'infrastructure suffisants](cs_users.html#infra_access) pour que vos utilisateurs puissent créer et gérer des clusters.</p>|
|Comptes Abonnement|Les comptes de type Abonnement ne sont pas configurés avec un accès au portefeuille d'infrastructure IBM Cloud (SoftLayer).|<strong>Option 1 :</strong> [Créez un compte Paiement à la carte](/docs/account/index.html#paygo) configuré avec accès au portefeuille d'infrastructure IBM Cloud (SoftLayer). Si vous sélectionnez cette option, vous disposerez de deux comptes {{site.data.keyword.Bluemix_notm}} avec facturation distincte.<p>Si vous souhaitez continuer à utiliser votre ancien compte Abonnement, vous pouvez utiliser le nouveau compte Paiement à la carte pour générer une clé d'API dans l'infrastructure IBM Cloud (SoftLayer). Vous devez ensuite [définir manuellement la clé d'API de l'infrastructure IBM Cloud (SoftLayer) pour votre compte Abonnement](cs_cli_reference.html#cs_credentials_set). N'oubliez pas que les ressources d'infrastructure IBM Cloud (SoftLayer) sont facturées via votre nouveau compte Paiement à la carte.</p><p><strong>Option 2 :</strong> Si vous disposez déjà d'un compte d'infrastructure IBM Cloud (SoftLayer) que vous désirez utiliser, vous pouvez [définir manuellement des données d'identification d'infrastructure IBM Cloud (SoftLayer)](cs_cli_reference.html#cs_credentials_set) pour votre compte {{site.data.keyword.Bluemix_notm}}.<p>**Remarque :** lorsque vous effectuez une liaison manuelle vers un compte d'infrastructure IBM Cloud (SoftLayer), les données d'identification sont utilisées pour toutes les actions spécifiques de l'infrastructure IBM Cloud (SoftLayer) effectuées dans votre compte {{site.data.keyword.Bluemix_notm}}. Vous devez vérifier que la clé d'API que vous avez définie dispose de [droits d'infrastructure suffisants](cs_users.html#infra_access) pour que vos utilisateurs puissent créer et gérer des clusters.</p>|
|Compte d'infrastructure IBM Cloud (SoftLayer), aucun compte {{site.data.keyword.Bluemix_notm}}|Pour créer un cluster standard, vous devez disposer d'un compte {{site.data.keyword.Bluemix_notm}}.|<p>[Créez un compte de type Paiement à la carte](/docs/account/index.html#paygo) configuré pour accès au portefeuille d'infrastructure IBM Cloud (SoftLayer). Si vous sélectionnez cette option, un compte d'infrastructure IBM Cloud (SoftLayer) est créé pour vous. Vous disposez de deux comptes d'infrastructure IBM Cloud (SoftLayer) différents avec facturation distincte.</p>|
{: caption="Options de création de cluster standard par type de compte" caption-side="top"}


<br />


## Le pare-feu empêche l'exécution de commandes via la ligne de commande
{: #ts_firewall_clis}

{: tsSymptoms}
Lorsque vous exécutez des commandes `bx`, `kubectl` ou `calicoctl` depuis l'interface de ligne de commande, ces commandes échouent.

{: tsCauses}
Des règles réseau d'entreprise empêchent peut-être l'accès depuis votre système local à des noeuds finaux publics via des proxies ou des pare-feux.

{: tsResolve}
[Autorisez l'accès TCP afin que les commandes CLI fonctionnent](cs_firewall.html#firewall). Cette tâche nécessite d'utiliser une [règle d'accès administrateur](cs_users.html#access_policies). Vérifiez votre [règle d'accès actuelle](cs_users.html#infra_access).


## Le pare-feu empêche le cluster de se connecter aux ressources
{: #cs_firewall}

{: tsSymptoms}
Lorsque les connexions des noeuds worker sont impossibles, vous pouvez voir toute une variété de symptômes différents. Vous pourrez obtenir l'un des messages suivants en cas d'échec de la commande kubectl proxy ou si vous essayez d'accéder à un service dans votre cluster et que la connexion échoue.

  ```
  Connection refused
  ```
  {: screen}

  ```
  Connection timed out
  ```
  {: screen}

  ```
  Unable to connect to the server: net/http: TLS handshake timeout
  ```
  {: screen}

Si vous exécutez les commandes kubectl exec, attach ou logs, le message suivant peut s'afficher.

  ```
  Error from server: error dialing backend: dial tcp XXX.XXX.XXX:10250: getsockopt: connection timed out
  ```
  {: screen}

Si la commande kubectl proxy aboutit, mais que le tableau de bord n'est pas disponible, vous pourrez voir le message suivant.

  ```
  timeout on 172.xxx.xxx.xxx
  ```
  {: screen}



{: tsCauses}
Vous pouvez disposer d'un autre pare-feu configuré ou avoir personnalisé vos paramètres de pare-feu existants dans votre compte d'infrastructure IBM Cloud (SoftLayer). {{site.data.keyword.containershort_notm}} requiert que certaines adresses IP et certains ports soient ouverts pour permettre la communication entre le noeud worker et le maître Kubernetes et inversement. Une autre cause peut être que les noeuds worker soient bloqués dans une boucle de rechargement.

{: tsResolve}
[Autorisez le cluster à accéder aux ressources d'infrastructure et à d'autres services](cs_firewall.html#firewall_outbound). Cette tâche nécessite d'utiliser une [règle d'accès administrateur](cs_users.html#access_policies). Vérifiez votre [règle d'accès actuelle](cs_users.html#infra_access).

<br />



## L'accès à votre noeud worker à l'aide de SSH échoue
{: #cs_ssh_worker}

{: tsSymptoms}
Vous ne pouvez pas accéder à votre noeud worker à l'aide d'une connexion SSH.

{: tsCauses}
L'accès SSH via un mot de passe est désactivé sur les noeuds worker.

{: tsResolve}
Utilisez des [DaemonSets ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) pour tout ce que vous devez exécuter sur chaque noeud ou des travaux pour toutes les actions ponctuelles que vous devez exécuter.

<br />


## Les commandes `kubectl exec` et `kubectl logs` ne fonctionnent pas
{: #exec_logs_fail}

{: tsSymptoms}
Si vous exécutez une commande `kubectl exec` ou `kubectl logs`, le message suivant s'affiche :

  ```
  <workerIP>:10250: getsockopt: connection timed out
  ```
  {: screen}

{: tsCauses}
La connexion OpenVPN entre le noeud maître et les noeuds worker ne fonctionne pas correctement.

{: tsResolve}
1. Activez la fonction [Spanning VLAN](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning) pour votre compte d'infrastructure IBM Cloud (SoftLayer).
2. Redémarrez le pod du client OpenVPN.
  ```
  kubectl delete pod -n kube-system -l app=vpn
  ```
  {: pre}
3. Si le message d'erreur s'affiche toujours, le noeud worker sur lequel réside le pod VPN est peut-être défectueux. Pour redémarrer le pod VPN et le replanifier sur un autre noeud worker, utilisez les [commandes cordon, drain et réamorcez le noeud worker](cs_cli_reference.html#cs_worker_reboot) sur lequel se trouve le pod VPN.

<br />


## La liaison d'un service à un cluster renvoie une erreur due à l'utilisation du même nom
{: #cs_duplicate_services}

{: tsSymptoms}
Lorsque vous exécutez la commande `bx cs cluster-service-bind <cluster_name> <namespace> <service_instance_name>`, le message suivant s'affiche :

```
Multiple services with the same name were found.
Run 'bx service list' to view available Bluemix service instances...
```
{: screen}

{: tsCauses}
Il se peut que plusieurs instances de service aient le même nom dans différentes régions.

{: tsResolve}
Utilisez l'identificateur global unique (GUID) du service au lieu du nom de l'instance de service dans la commande `bx cs cluster-service-bind`.

1. [Connectez-vous à la région hébergeant l'instance de service à lier.](cs_regions.html#bluemix_regions)

2. Extrayez l'identificateur global unique (GUID) de l'instance de service.
  ```
  bx service show <service_instance_name> --guid
  ```
  {: pre}

  Sortie :
  ```
  Invoking 'cf service <service_instance_name> --guid'...
  <service_instance_GUID>
  ```
  {: screen}
3. Liez à nouveau le service au cluster.
  ```
  bx cs cluster-service-bind <cluster_name> <namespace> <service_instance_GUID>
  ```
  {: pre}

<br />


## La liaison d'un service à un cluster renvoie une erreur indiquant que le service est introuvable
{: #cs_not_found_services}

{: tsSymptoms}
Lorsque vous exécutez la commande `bx cs cluster-service-bind <cluster_name> <namespace> <service_instance_name>`, le message suivant s'affiche :

```
Binding service to a namespace...
FAILED

The specified IBM Cloud service could not be found. If you just created the service, wait a little and then try to bind it again. To view available IBM Cloud service instances, run 'bx service list'. (E0023)
```
{: screen}

{: tsCauses}
Pour lier des services à un cluster vous devez disposer du rôle d'utilisateur développeur Cloud Foundry pour l'espace où l'instance de service est mise à disposition. Vous devez également disposer de l'accès Editeur IAM à {{site.data.keyword.containerlong}}. Pour accéder à l'instance de service, vous devez être connecté à l'espace dans lequel l'instance de service est mise à disposition. 

{: tsResolve}

**En tant qu'utilisateur :**

1. Connectez-vous à {{site.data.keyword.Bluemix_notm}}. 
   ```
   bx login
   ```
   {: pre}
   
2. Ciblez l'organisation et l'espace où l'instance de service est mise à disposition. 
   ```
   bx target -o <org> -s <space>
   ```
   {: pre}
   
3. Vérifiez que vous êtes dans le bon espace en affichant la liste de vos instances de service. 
   ```
   bx service list 
   ```
   {: pre}
   
4. Essayez d'effectuer à nouveau la liaison du service. Si vous obtenez la même erreur, contactez l'administrateur du compte et vérifiez que vous disposez des droits suffisants pour effectuer des liaisons de services (voir la procédure suivante de l'administrateur du compte). 

**En tant qu'administrateur du compte :**

1. Vérifiez que l'utilisateur qui rencontre ce problème dispose des [droits d'éditeur pour {{site.data.keyword.containerlong}}](/docs/iam/mngiam.html#editing-existing-access). 

2. Vérifiez que l'utilisateur qui rencontre ce problème dispose du [rôle de développeur Cloud Foundry pour l'espace](/docs/iam/mngcf.html#updating-cloud-foundry-access) dans lequel le service est mis à disposition. 

3. Si les droits adéquats existent, essayez d'affecter un autre droit, puis d'affecter à nouveau le droit requis. 

4. Patientez quelques minutes, puis laissez l'utilisateur effectuer une nouvelle tentative de liaison du service. 

5. Si le problème n'est toujours pas résolu, les droits IAM ne sont pas synchronisés et vous ne pouvez pas résoudre le problème vous-même. [Contactez le support IBM](/docs/get-support/howtogetsupport.html#getting-customer-support) en ouvrant un ticket de demande de service. Veillez à fournir l'ID du cluster, l'ID utilisateur et l'ID de l'instance de service. 
   1. Récupérez l'ID du cluster.
      ```
      bx cs clusters
      ```
      {: pre}
      
   2. Récupérez l'ID d'instance de service.
      ```
      bx service show <service_name> --guid
      ```
      {: pre}


<br />



## Après la mise à jour ou le rechargement d'un noeud worker, des noeuds et des pods en double apparaissent
{: #cs_duplicate_nodes}

{: tsSymptoms}
Lorsque vous exécutez la commande `kubectl get nodes`, vous voyez des noeuds worker en double avec le statut **NotReady**. Les noeuds worker avec le statut **NotReady** ont des adresses IP publiques, alors que les noeuds worker avec le statut **Ready** ont des adresses IP privées.

{: tsCauses}
D'anciens clusters affichaient la liste des noeuds worker avec l'adresse IP publique de cluster. A présent, les noeuds worker sont répertoriés avec l'adresse IP privée du cluster. Lorsque vous rechargez ou mettez à jour un noeud, l'adresse IP est modifiée, mais la référence à l'adresse IP publique est conservée.

{: tsResolve}
Le service n'est pas interrompu en raison de ces doublons, mais vous pouvez supprimer les références aux anciens noeuds worker du serveur d'API.

  ```
  kubectl delete node <node_name1> <node_name2>
  ```
  {: pre}

<br />


## Après la mise à jour ou le rechargement d'un noeud worker, les applications reçoivent le message d'erreur RBAC DENY
{: #cs_rbac_deny}

{: tsSymptoms}
Après avoir effectué la mise à jour vers Kubernetes version 1.7, les applications reçoivent des erreurs de type `RBAC DENY`.

{: tsCauses}
A partir de [Kubernetes version 1.7](cs_versions.html#cs_v17), les applications qui s'exécutent dans l'espace de nom `default` ne disposent plus des privilèges d'administrateur de cluster sur l'API Kubernetes pour la sécurité renforcée.

Si votre application s'exécute dans l'espace de nom `default`, utilise le compte `default ServiceAccount` et accède à l'API Kubernetes, elle est affectée par cette modification de Kubernetes. Pour plus d'informations, voir [la documentation Kubernetes![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/admin/authorization/rbac/#upgrading-from-15).

{: tsResolve}
Avant de commencer, [ciblez avec votre interface de ligne de commande](cs_cli_install.html#cs_cli_configure) votre cluster.

1.  **Action temporaire** : lorsque vous mettez à jour les règles RBAC de votre application, vous souhaiterez peut-être rétablir temporairement l'autorisation `ClusterRoleBinding` précédente pour le compte `default ServiceAccount` dans l'espace de nom `default`.

    1.  Copiez le fichier `.yaml` suivant.

        ```yaml
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
        ```

    2.  Appliquez le fichier `.yaml` à votre cluster.

        ```
        kubectl apply -f FILENAME
        ```
        {: pre}

2.  [Créez des ressources d'autorisation RBAC![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview) pour mettre à jour l'accès administrateur `ClusterRoleBinding`.

3.  Si vous avez créé une liaison de rôle de cluster temporaire, supprimez-la.

<br />


## Echec de l'accès à un pod sur un nouveau noeud worker et dépassement du délai d'attente
{: #cs_nodes_duplicate_ip}

{: tsSymptoms}
Vous avez supprimé un noeud worker dans votre cluster et ajouté un noeud worker. Lorsque vous avez déployé un pod ou un service Kubernetes, la ressource n'a pas pu accéder au noeud worker nouvellement créé et un dépassement de délai d'attente s'est produit pour la connexion.

{: tsCauses}
Si vous supprimez un noeud worker de votre cluster et que vous ajoutez un noeud worker, il se peut que l'adresse IP privée du noeud worker supprimé soit affectée au nouveau noeud worker. Calico utilise cette adresse IP privée en tant que balise et continue d'essayer d'atteindre le noeud supprimé.

{: tsResolve}
Mettez manuellement à jour la référence de l'adresse IP privée pour qu'elle pointe vers le noeud approprié.

1.  Vérifiez que vous possédez deux noeuds worker associés à la même **adresse IP privée**. Notez l'**adresse IP privée** et l'**ID** du noeud worker supprimé.

  ```
  bx cs workers <CLUSTER_NAME>
  ```
  {: pre}

  ```
  ID                                                 Public IP       Private IP       Machine Type   State     Status   Location   Version
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w1   169.xx.xxx.xxx  10.xxx.xx.xxx    b2c.4x16       normal    Ready    dal10      1.9.7
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w2   169.xx.xxx.xxx  10.xxx.xx.xxx    b2c.4x16       deleted    -       dal10      1.9.7
  ```
  {: screen}

2.  Installez l'[interface de ligne de commande Calico](cs_network_policy.html#adding_network_policies).
3.  Répertoriez les noeuds worker disponibles dans Calico. Remplacez <path_to_file> par le chemin d'accès local au fichier de configuration Calico.

  ```
  calicoctl get nodes --config=filepath/calicoctl.cfg
  ```
  {: pre}

  ```
  NAME
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w1
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w2
  ```
  {: screen}

4.  Supprimez le noeud worker en double dans Calico. Remplacez NODE_ID par l'ID du noeud worker.

  ```
  calicoctl delete node NODE_ID --config=<path_to_file>/calicoctl.cfg
  ```
  {: pre}

5.  Redémarrez le noeud worker qui n'a pas été supprimé.

  ```
  bx cs worker-reboot CLUSTER_ID NODE_ID
  ```
  {: pre}


Le noeud supprimé n'apparaît plus dans Calico.

<br />


## Le cluster reste à l'état En attente
{: #cs_cluster_pending}

{: tsSymptoms}
Lorsque vous déployez votre cluster, il reste à l'état en attente (pending) et ne démarre pas.

{: tsCauses}
Si vous venez de créer le cluster, les noeuds worker sont peut-être encore en cours de configuration. Si vous patientez déjà depuis un bon moment, il se peut que votre réseau local virtuel (VLAN) soit non valide.

{: tsResolve}

Vous pouvez envisager l'une des solutions suivantes :
  - Vérifiez le statut de votre cluster en exécutant la commande `bx cs clusters`. Puis vérifiez que vos noeuds worker sont déployés en exécutant la commande `bx cs workers <cluster_name>`.
  - Vérifiez si votre réseau local virtuel (VLAN) est valide. Pour être valide, un VLAN doit être associé à une infrastructure pouvant héberger un noeud worker avec un stockage sur disque local. Vous pouvez [afficher la liste de vos VLAN](/docs/containers/cs_cli_reference.html#cs_vlans) en exécutant la commande `bx cs vlans <location>`. Si le VLAN n'apparaît pas dans la liste, il n'est pas valide. Choisissez-en un autre.

<br />


## Des pods sont toujours à l'état en attente
{: #cs_pods_pending}

{: tsSymptoms}
Lorsque vous exécutez `kubectl get pods`, vous constatez que des pods sont toujours à l'état en attente (**pending**).

{: tsCauses}
Si vous venez juste de créer le cluster Kubernetes, il se peut que les noeuds worker soient encore en phase de configuration. S'il s'agit d'un cluster existant, sa capacité est peut-être insuffisante pour y déployer le pod.

{: tsResolve}
Cette tâche nécessite d'utiliser une [règle d'accès administrateur](cs_users.html#access_policies). Vérifiez votre [règle d'accès actuelle](cs_users.html#infra_access).

Si vous venez de créer le cluster Kubernetes, exécutez la commande suivante et attendez l'initialisation des noeuds worker.

```
kubectl get nodes
```
{: pre}

S'il s'agit d'un cluster existant, vérifiez sa capacité.

1.  Définissez le proxy avec le numéro de port par défaut.

  ```
  kubectl proxy
  ```
   {: pre}

2.  Ouvrez le tableau de bord Kubernetes.

  ```
  http://localhost:8001/ui
  ```
  {: pre}

3.  Vérifiez que vous disposez de suffisamment de capacité dans votre cluster pour déployer le pod.

4.  Si vous ne disposez pas de suffisamment de capacité dans votre cluster, ajoutez un autre noeud worker à celui-ci.

    ```
    bx cs worker-add <cluster_name_or_ID> 1
    ```
    {: pre}

5.  Si vos pods sont toujours à l'état **pending** après le déploiement complet du noeud worker, consultez la [documentation Kubernetes ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-pod-replication-controller/#my-pod-stays-pending) pour effectuer d'autres tâches en vue d'identifier et de résoudre le problème.

<br />




## Les conteneurs ne démarrent pas
{: #containers_do_not_start}

{: tsSymptoms}
Les pods se déploient sur les clusters, mais les conteneurs ne démarrent pas.

{: tsCauses}
Il peut arriver que les conteneurs ne démarrent pas lorsque le quota de registre est atteint.

{: tsResolve}
[Libérez de l'espace de stockage dans {{site.data.keyword.registryshort_notm}}.](../services/Registry/registry_quota.html#registry_quota_freeup)

<br />



## Impossible d'installer une charte Helm avec les valeurs de configuration mises à jour
{: #cs_helm_install}

{: tsSymptoms}
Lorsque vous essayez d'installer une charte Helm en exécutant la commande `helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/<chart_name>`, vous obtenez le message d'erreur `Error: failed to download "ibm/<chart_name>"`.

{: tsCauses}
L'adresse URL du référentiel {{site.data.keyword.Bluemix_notm}} dans votre instance Helm est peut-être incorrecte.

{: tsResolve}
Pour identifier et résoudre les problèmes liés à votre charte Helm :

1. Affichez la liste des référentiels actuellement disponibles dans votre instance Helm.

    ```
    helm repo list
    ```
    {: pre}

2. Dans la sortie, vérifiez que l'URL du référentiel {{site.data.keyword.Bluemix_notm}}, `ibm` est `https://registry.bluemix.net/helm/ibm`.

    ```
    NAME    URL
    stable  https://kubernetes-charts.storage.googleapis.com
    local   http://127.0.0.1:8888/charts
    ibm     https://registry.bluemix.net/helm/ibm
    ```
    {: screen}

    * Si l'URL n'est pas correcte :

        1. Retirez le référentiel {{site.data.keyword.Bluemix_notm}}.

            ```
            helm repo remove ibm
            ```
            {: pre}

        2. Ajoutez à nouveau le référentiel {{site.data.keyword.Bluemix_notm}}.

            ```
            helm repo add ibm  https://registry.bluemix.net/helm/ibm
            ```
            {: pre}

    * Si l'URL est correcte, obtenez les dernières mises à jour du référentiel.

        ```
        helm repo update
        ```
        {: pre}

3. Installez la charte Helm avec vos mises à jour.

    ```
    helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/<chart_name>
    ```
    {: pre}

<br />


## Aide et assistance
{: #ts_getting_help}

Vous avez encore des problèmes avec votre cluster ?
{: shortdesc}

-   Pour déterminer si {{site.data.keyword.Bluemix_notm}} est disponible, [consultez la page de statut d'{{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://developer.ibm.com/bluemix/support/#status).
-   Publiez une question sur le site [{{site.data.keyword.containershort_notm}} Slack ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://ibm-container-service.slack.com).

    Si vous n'utilisez pas un ID IBM pour votre compte {{site.data.keyword.Bluemix_notm}}, [demandez une invitation](https://bxcs-slack-invite.mybluemix.net/) sur ce site Slack.
    {: tip}
-   Consultez les forums pour établir si d'autres utilisateurs ont rencontré le même problème. Lorsque vous utilisez les forums pour poser une question, balisez votre question de sorte que les équipes de développement {{site.data.keyword.Bluemix_notm}} la voient.

    -   Si vous avez des questions d'ordre technique sur le développement ou le déploiement de clusters ou d'applications à l'aide d'{{site.data.keyword.containershort_notm}}, publiez-les sur le site [Stack Overflow ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) en leur adjoignant les balises `ibm-cloud`, `kubernetes` et `containers`.
    -   Pour des questions relatives au service et aux instructions de mise en route, utilisez le forum [IBM developerWorks dW Answers ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix). Incluez les balises `ibm-cloud` et `containers`.
    Voir [Comment obtenir de l'aide](/docs/get-support/howtogetsupport.html#using-avatar)
pour plus d'informations sur l'utilisation des forums.

-   Contactez le support IBM en ouvrant un ticket de demande de service. Pour en savoir plus sur l'ouverture d'un ticket de demande de service IBM ou sur les niveaux de support disponibles et les gravités des tickets, voir la rubrique décrivant comment [contacter le support](/docs/get-support/howtogetsupport.html#getting-customer-support).

{: tip}
Lorsque vous signalez un problème, incluez l'ID de votre cluster. Pour identifier l'ID du cluster, exécutez la commande `bx cs clusters`.

