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




# Configuration de sous-réseaux pour des clusters
{: #subnets}

Vous pouvez modifier le pool d'adresses IP portables publiques ou privées disponibles en ajoutant des sous-réseaux à votre cluster Kubernetes dans {{site.data.keyword.containerlong}}.
{:shortdesc}

Dans {{site.data.keyword.containershort_notm}}, vous pouvez ajouter des adresses IP portables stables pour les services Kubernetes en adjoignant des sous-réseaux au cluster. Dans ce cas, les sous-réseaux ne sont pas utilisés avec le masque réseau pour créer une connectivité à travers un ou plusieurs clusters. A la place, les sous-réseaux sont utilisés pour fournir des adresses IP permanentes fixes pour un service à partir d'un cluster pouvant être utilisées pour accéder à ce service.

<dl>
  <dt>Par défaut, la création d'un cluster comprend la création de sous-réseaux</dt>
  <dd>Lorsque vous créez un cluster standard, les sous-réseaux suivants sont fournis automatiquement par {{site.data.keyword.containershort_notm}} :
    <ul><li>Un sous-réseau public principal qui détermine les adresses IP publiques des noeuds worker lors de la création du cluster</li>
    <li>Un sous-réseau privé principal qui détermine les adresses IP privées des noeuds worker lors de la création du cluster</li>
    <li>Un sous-réseau public portable qui fournit 5 adresses IP publiques pour les services de réseau d'Ingress et de l'équilibreur de charge.</li>
    <li>Un sous-réseau privé portable qui fournit 5 adresses IP privées pour les services de réseau d'Ingress et de l'équilibreur de charge.</li></ul>
      Les adresses IP publiques et privées portables sont statiques et ne changent pas en cas de retrait d'un noeud worker. Pour chaque sous-réseau, une adresse IP publique portable et une adresse IP privée portable sont utilisées pour les [équilibreurs de charge d'application Ingress](cs_ingress.html) par défaut. Vous pouvez utiliser l'équilibreur de charge d'application Ingress pour exposer plusieurs applications dans votre cluster. Les quatre autres adresses IP publiques portables et les quatre autres adresses IP privées portables peuvent être utilisées pour des applications individuelles sur le réseau public ou privé en [créant un service d'équilibreur de charge](cs_loadbalancer.html).</dd>
  <dt>[Commande et gestion de vos propres sous-réseaux existants](#custom)</dt>
  <dd>Vous pouvez commander et gérer des sous-réseaux portables existants dans votre compte d'infrastructure IBM Cloud (SoftLayer) au lieu d'utiliser les sous-réseaux automatiquement fournis. Utilisez cette option pour conserver des adresses IP statiques stables lors de création ou de suppression de clusters ou pour commander des blocs d'adresses IP plus importants. Créez d'abord un cluster sans sous-réseaux en utilisant la commande `cluster-create --no-subnet`, puis en ajoutant le sous-réseau au cluster avec la commande `cluster-subnet-add`. </dd>
</dl>

**Remarque :** les adresses IP publiques portables sont facturées au mois. Si vous retirez des adresses IP publiques portables après la mise en place de votre cluster, vous devez quand même payer les frais mensuels, même si vous ne les avez utilisées que brièvement.

## Demande de sous-réseaux supplémentaires pour votre cluster
{: #request}

Vous pouvez ajouter des adresses IP publiques ou privées portables stables au cluster en lui affectant des sous-réseaux.
{:shortdesc}

**Remarque :** lorsque vous rendez un sous-réseau disponible pour un cluster, les adresses IP du sous-réseau sont utilisées pour l'opération réseau du cluster. Pour éviter des conflits d'adresse IP, prenez soin d'utiliser un sous-réseau avec un seul cluster. N'utilisez pas en même temps un sous-réseau pour plusieurs clusters ou à d'autres fins hors d'{{site.data.keyword.containershort_notm}}.

Avant de commencer, [ciblez avec votre interface de ligne de commande](cs_cli_install.html#cs_cli_configure) votre cluster.

Pour créer un sous-réseau dans un compte d'infrastructure IBM Cloud (SoftLayer) et le rendre disponible pour un cluster spécifié :

1. Provisionnez un nouveau sous-réseau.

    ```
    bx cs cluster-subnet-create <cluster_name_or_id> <subnet_size> <VLAN_ID>
    ```
    {: pre}

    <table>
    <caption>Description des composantes de cette commande</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composantes de cette commande</th>
    </thead>
    <tbody>
    <tr>
    <td><code>cluster-subnet-create</code></td>
    <td>Commande de création d'un sous-réseau pour votre cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;cluster_name_or_id&gt;</em></code></td>
    <td>Remplacez <code>&lt;cluster_name_or_id&gt;</code> par le nom ou l'ID du cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;subnet_size&gt;</em></code></td>
    <td>Remplacez <code>&lt;subnet_size&gt;</code> par le nombre d'adresses IP que vous désirez ajouter depuis votre sous-réseau portable. Valeurs admises : 8, 16, 32 ou 64. <p>**Remarque :** lorsque vous ajoutez des adresses IP portables à votre sous-réseau, trois adresses IP sont utilisées pour les opérations de réseau interne au cluster. Vous ne pouvez pas utiliser ces trois adresses IP pour votre équilibreur de charge d'application ou pour créer un service d'équilibreur de charge. Par exemple, si vous demandez huit adresses IP publiques portables, vous pouvez en utiliser cinq pour exposer vos applications au public.</p> </td>
    </tr>
    <tr>
    <td><code><em>&lt;VLAN_ID&gt;</em></code></td>
    <td>Remplacez <code>&lt;VLAN_ID&gt;</code> par l'ID du réseau local virtuel (VLAN) privé ou public sur lequel vous désirez allouer les adresses IP portables publiques ou privées. Vous devez sélectionner le réseau local virtuel public ou privé auquel un noeud worker existant est connecté. Pour examiner le VLAN privé ou public d'un noeud worker, exécutez la commande <code>bx cs worker-get &lt;worker_id&gt;</code>. </td>
    </tr>
    </tbody></table>

2.  Vérifiez que le sous-réseau a bien été créé et ajouté à votre cluster. Le CIDR de sous-réseau est répertorié dans la section **VLAN**.

    ```
    bx cs cluster-get --showResources <cluster_name_or_ID>
    ```
    {: pre}

3. Facultatif : [Activez le routage entre les sous-réseaux sur le même VLAN](#vlan-spanning).

<br />


## Ajout ou réutilisation de sous-réseaux personnalisés et existants dans les clusters Kubernetes
{: #custom}

Vous pouvez ajouter des sous-réseaux publics ou privés portables existants à votre cluster Kubernetes ou réutiliser des sous-réseaux provenant d'un cluster supprimé.
{:shortdesc}

Avant de commencer
- [Ciblez votre interface de ligne de commande](cs_cli_install.html#cs_cli_configure) vers votre cluster.
- Pour réutiliser des sous-réseaux d'un cluster que vous n'utilisez plus, supprimez le cluster inutile. Les sous-réseaux sont supprimés dans les 24 heures.

   ```
   bx cs cluster-rm <cluster_name_or_ID
   ```
   {: pre}

Pour utiliser un sous-réseau existant dans votre portefeuille d'infrastructure IBM Cloud (SoftLayer) avec des règles de pare-feu personnalisées ou des adresses IP disponibles :

1.  Identifiez le sous-réseau à utiliser. Notez l'ID du sous-réseau et l'ID du réseau local virtuel. Dans cet exemple, l'ID du sous-réseau est `1602829` et l'ID du VLAN est `2234945`.

    ```
    bx cs subnets
    ```
    {: pre}

    ```
    Getting subnet list...
    OK
    ID        Network             Gateway          VLAN ID   Type      Bound Cluster
    1550165   10.xxx.xx.xxx/26    10.xxx.xx.xxx    2234947   private
    1602829   169.xx.xxx.xxx/28   169.xx.xxx.xxx   2234945   public

    ```
    {: screen}

2.  Vérifiez l'emplacement du réseau local virtuel. Dans cet exemple, son emplacement est dal10.

    ```
    bx cs vlans dal10
    ```
    {: pre}

    ```
    Getting VLAN list...
    OK
    ID        Name   Number   Type      Router         Supports Virtual Workers
    2234947          1813     private   bcr01a.dal10   true
    2234945          1618     public    fcr01a.dal10   true
    ```
    {: screen}

3.  Créez un cluster en utilisant l'emplacement et l'ID du réseau local virtuel que vous avez identifiés. Pour réutiliser un sous-réseau existant, incluez l'indicateur `--no-subnet` pour empêcher la création automatique d'un nouveau sous-réseau d'adresses IP publiques portables et d'un nouveau sous-réseau d'adresses IP privées portables.

    ```
    bx cs cluster-create --location dal10 --machine-type u2c.2x4 --no-subnet --public-vlan 2234945 --private-vlan 2234947 --workers 3 --name my_cluster
    ```
    {: pre}

4.  Vérifiez que la création du cluster a été demandée.

    ```
    bx cs clusters
    ```
    {: pre}

    **Remarque :** la commande des postes de noeud worker et la configuration et la mise à disposition du cluster dans votre compte peuvent prendre jusqu'à 15 minutes.

    Lorsque la mise à disposition de votre cluster est finalisée, le statut du cluster passe à **deployed**.

    ```
    Name         ID                                   State      Created          Workers   Location   Version
    mycluster    aaf97a8843a29941b49a598f516da72101   deployed   20170201162433   3         dal10      1.9.7
    ```
    {: screen}

5.  Vérifiez le statut des noeuds worker.

    ```
    bx cs workers <cluster>
    ```
    {: pre}

    Lorsque les noeuds worker sont prêts, l'état passe à **normal** et le statut indique **Ready**. Lorsque le statut du noeud indique **Ready**, vous pouvez accéder au cluster.

    ```
    ID                                                  Public IP        Private IP     Machine Type   State      Status   Location   Version
    prod-dal10-pa8dfcc5223804439c87489886dbbc9c07-w1    169.xx.xxx.xxx   10.xxx.xx.xxx  free           normal     Ready    dal10      1.9.7
    ```
    {: screen}

6.  Ajoutez le sous-réseau à votre cluster en spécifiant l'ID du sous-réseau. Lorsque vous rendez disponible un sous-réseau pour un cluster, un fichier configmap Kubernetes est créé pour vous et inclut toutes les adresses IP publiques portables disponibles que vous pouvez utiliser. S'il n'existe aucun équilibreur de charge d'application pour votre cluster, une adresse IP portable publique et une adresse IP portable privée sont automatiquement utilisées pour créer les équilibreurs de charge d'application privés et publics. Toutes les autres adresses peuvent être utilisées pour créer des services d'équilibreur de charge pour vos applications.

    ```
    bx cs cluster-subnet-add mycluster 807861
    ```
    {: pre}

7. Facultatif : [Activez le routage entre les sous-réseaux sur le même VLAN](#vlan-spanning).

<br />


## Ajout de sous-réseaux et d'adresses IP gérés par l'utilisateur aux clusters Kubernetes
{: #user_managed}

Fournissez un sous-réseau à partir d'un réseau sur site que vous souhaitez accessible à {{site.data.keyword.containershort_notm}}. Vous pouvez ensuite ajouter des adresses IP privées de ce sous-réseau aux services d'équilibreur de charge dans votre cluster Kubernetes.
{:shortdesc}

Conditions requises :
- Les sous-réseaux gérés par l'utilisateur peuvent être ajoutés uniquement à des réseaux locaux virtuels (VLAN) privés.
- La limite de longueur du préfixe de sous-réseau est /24 à /30. Par exemple, `169.xx.xxx.xxx/24` indique 253 adresses IP privées utilisables, alors que `169.xx.xxx.xxx/30` indique 1 adresse IP privée utilisable.
- La première adresse IP dans le sous-réseau doit être utilisée comme passerelle du sous-réseau.

Avant de commencer :
- Configurez le routage du trafic réseau vers et depuis le sous-réseau externe.
- Confirmez que vous disposez bien d'une connectivité VPN entre la passerelle réseau du centre de données sur site et le dispositif de routeur virtuel du réseau privé ou le service VPN strongSwan qui s'exécute dans votre cluster. Pour plus d'informations, voir [Configuration de la connectivité VPN](cs_vpn.html).

Pour ajouter un sous-réseau à partir d'un réseau sur site :

1. Affichez l'ID du réseau local virtuel privé de votre cluster. Localisez la section **VLANs**. Dans la zone **User-managed**, identifiez l'ID VLAN avec la valeur _false_.

    ```
    bx cs cluster-get --showResources <cluster_name>
    ```
    {: pre}

    ```
    VLANs
    VLAN ID   Subnet CIDR       Public   User-managed
    2234947   10.xxx.xx.xxx/29  false    false
    2234945   169.xx.xxx.xxx/29 true     false
    ```
    {: screen}

2. Ajoutez le sous-réseau externe à votre VLAN privé. Les adresses IP privées portables sont ajoutées à la mappe de configuration (configmap) du cluster.

    ```
    bx cs cluster-user-subnet-add <cluster_name> <subnet_CIDR> <VLAN_ID>
    ```
    {: pre}

    Exemple :

    ```
    bx cs cluster-user-subnet-add mycluster 10.xxx.xx.xxx/24 2234947
    ```
    {: pre}

3. Vérifiez que le sous-réseau fourni par l'utilisateur a été ajouté. La zone **User-managed** a la valeur _true_.

    ```
    bx cs cluster-get --showResources <cluster_name>
    ```
    {: pre}

    ```
    VLANs
    VLAN ID   Subnet CIDR       Public   User-managed
    2234947   10.xxx.xx.xxx/29  false    false
    2234945   169.xx.xxx.xxx/29 true   false
    2234947   10.xxx.xx.xxx/24  false    true
    ```
    {: screen}

4. Facultatif : [Activez le routage entre les sous-réseaux sur le même VLAN](#vlan-spanning).

5. Ajoutez un service d'équilibreur de charge privé ou un équilibreur de charge d'application Ingress privé pour accéder à votre application à travers le réseau privé. Pour utiliser une adresse IP privée du sous-réseau que vous venez d'ajouter, vous devez spécifier une adresse IP. Autrement, une adresse IP est sélectionnée de manière aléatoire dans les sous-réseaux d'infrastructure IBM Cloud (SoftLayer) ou dans les sous-réseaux fournis par l'utilisateur sur le VLAN privé. Pour plus d'informations, voir [Activation de l'accès public ou privé à une application à l'aide d'un service LoadBalancer](cs_loadbalancer.html#config) ou [Activation de l'équilibreur de charge d'application privé](cs_ingress.html#private_ingress).

<br />


## Gestion d'adresses IP et de sous-réseaux
{: #manage}

Vérifiez les options suivantes pour répertorier les adresses IP publiques disponibles, libérer des adresses IP utilisées et activer le routage entre plusieurs sous-réseaux sur le même VLAN.
{:shortdesc}

### Affichage des adresses IP publiques portables disponibles
{: #review_ip}

Pour répertorier toutes les adresses IP utilisées et disponibles dans votre cluster, vous pouvez exécuter cette commande :

  ```
  kubectl get cm ibm-cloud-provider-vlan-ip-config -n kube-system -o yaml
  ```
  {: pre}

Pour répertorier uniquement les adresses IP publiques disponibles pour votre cluster, procédez comme suit :

Avant de commencer, [définissez le contexte du cluster que vous désirez utiliser.](cs_cli_install.html#cs_cli_configure)

1.  Créez un fichier de configuration de service Kubernetes nommé `myservice.yaml` et définissez un service de type `LoadBalancer` avec une adresse IP d'équilibreur de charge factice. L'exemple suivant utilise l'adresse IP 1.1.1.1 comme adresse IP de l'équilibreur de charge.

    ```
    apiVersion: v1
    kind: Service
    metadata:
      labels:
        run: myservice
      name: myservice
      namespace: default
    spec:
      ports:
      - port: 80
        protocol: TCP
        targetPort: 80
      selector:
        run: myservice
      sessionAffinity: None
      type: LoadBalancer
      loadBalancerIP: 1.1.1.1
    ```
    {: codeblock}

2.  Créez le service dans votre cluster.

    ```
    kubectl apply -f myservice.yaml
    ```
    {: pre}

3.  Inspectez le service.

    ```
    kubectl describe service myservice
    ```
    {: pre}

    **Remarque :** la création de ce service échoue car le maître Kubernetes ne parvient pas à localiser l'adresse IP de l'équilibreur de charge spécifié dans le fichier configmap Kubernetes. Lorsque vous exécutez cette commande, le message d'erreur s'affiche, ainsi que la liste des adresses IP publiques disponibles pour le cluster.

    ```
    Erreur sur l'équilibreur de charge cloud a8bfa26552e8511e7bee4324285f6a4a pour le service default/myservice portant l'UID 8bfa2655-2e85-11e7-bee4-324285f6a4af: L'adresse IP 1.1.1.1 du fournisseur cloud demandée n'est pas disponible. Les adresses IP suivantes de fournisseur cloud sont disponibles : <list_of_IP_addresses>
    ```
    {: screen}

### Libération des adresses IP utilisées
{: #free}

Vous pouvez libérer une adresse IP portable utilisée en supprimant le service d'équilibreur de charge qui l'utilise.
{:shortdesc}

Avant de commencer, [définissez le contexte du cluster que vous désirez utiliser.](cs_cli_install.html#cs_cli_configure)

1.  Répertoriez les services disponibles dans votre cluster.

    ```
    kubectl get services
    ```
    {: pre}

2.  Supprimez le service d'équilibreur de charge qui utilise une adresse IP publique ou privée.

    ```
    kubectl delete service <service_name>
    ```
    {: pre}

### Activation du routage entre les sous-réseaux sur le même VLAN
{: #vlan-spanning}

Lorsque vous créez un cluster, un sous-réseau se terminant par `/26` est mis à disposition dans le même réseau local virtuel (VLAN) hébergeant le cluster. Ce sous-réseau principal peut contenir jusqu'à 62 noeuds worker.
{:shortdesc}

Cette limite de 62 noeuds worker peut être dépassée par un cluster volumineux ou par plusieurs clusters plus petits dans une seule région situés dans le même VLAN. Lorsque la limite de 62 noeuds worker est atteinte, un autre sous-réseau principal dans le même VLAN est commandé.

Pour effectuer le routage entre les sous-réseaux sur le même VLAN, vous devez activer la fonction Spanning VLAN. Pour obtenir les instructions nécessaires, voir [Activer ou désactiver le spanning VLAN](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning).
