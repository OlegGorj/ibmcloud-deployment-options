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




# Génération de conteneurs à partir d'images
{: #images}

Une image Docker est la base de chaque conteneur que vous créez avec {{site.data.keyword.containerlong}}.
{:shortdesc}

L'image est créée depuis un Dockerfile, lequel est un fichier contenant des instructions pour générer l'image. Un Dockerfile peut référencer dans ses instructions des artefacts de génération stockés séparément, comme une application, sa configuration, et ses dépendances.

## Planification de registres d'images
{: #planning}

Les images sont généralement stockées dans un registre d'images pouvant être accessible au public (registre public)  ou, au contraire, dont l'accès est limité à un petit groupe d'utilisateurs (registre privé).
{:shortdesc}

Des registres publics, tel que Docker Hub, peuvent être utilisés pour vous familiariser avec Docker et Kubernetes en créant créer votre première application conteneurisée dans un cluster. Dans le cas d'applications d'entreprise, utilisez un registre privé, tel que celui fourni dans {{site.data.keyword.registryshort_notm}}, pour empêcher l'utilisation et la modification de vos images par des utilisateurs non habilités. Les registres privés sont mis en place par l'administrateur du cluster pour garantir que les données d'identification pour accès au registre privé sont disponibles aux utilisateurs du cluster.


Vous pouvez utiliser plusieurs registres avec {{site.data.keyword.containershort_notm}} pour déployer des applications dans votre cluster.

|Registre|Description|Avantage|
|--------|-----------|-------|
|[{{site.data.keyword.registryshort_notm}}](/docs/services/Registry/index.html)|Cette option vous permet de mettre en place votre propre référentiel d'images Docker sécurisé dans {{site.data.keyword.registryshort_notm}} où vous pourrez stocker en sécurité vos images et les partager avec les autres utilisateurs du cluster.|<ul><li>Gestion de l'accès aux images dans votre compte.</li><li>Utilisation d'images et d'exemples d'application fournis par {{site.data.keyword.IBM_notm}}, comme {{site.data.keyword.IBM_notm}} Liberty,  en tant qu'image parent à laquelle vous ajouterez votre propre code d'application.</li><li>Analyse automatique des images pour détection de vulnérabilités potentielles par Vulnerability Advisor et soumission de recommandations spécifiques au système d'exploitation pour les corriger.</li></ul>|
|Tout autre registre privé|Connexion de n'importe quel registre privé existant à votre cluster en créant un élément [imagePullSecret ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/containers/images/). Cette valeur est utilisée pour enregistrer de manière sécurisée l'URL de votre registre URL et ses données d'identification dans une valeur confidentielle Kubernetes.|<ul><li>Utilisation de registres privés existants indépendants de leur source (Docker Hub, registres dont l'organisation est propriétaire, autres registres Cloud privés).</li></ul>|
|[Docker Hub public![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://hub.docker.com/){: #dockerhub}|Utilisez cette option pour utiliser directement des images publiques Docker Hub existantes dans votre [déploiement Kubernetes![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) lorsqu'il n'y a pas besoin d'effectuer des modifications dans Dockerfile. <p>**Remarque :** gardez à l'esprit que cette option peut ne pas satisfaire les exigences de sécurité de votre organisation (par exemple, en matière de gestion des accès, d'analyse des vulnérabilités ou de protection des données confidentielles de l'application).</p>|<ul><li>Aucune configuration supplémentaire du cluster n'est nécessaire.</li><li>Inclut un certain nombre d'applications en code source ouvert.</li></ul>|
{: caption="Options de registre d'images public et privé" caption-side="top"}

Une fois que vous avez configuré un registre d'images, les utilisateurs du cluster peuvent utiliser les images pour le déploiement de leurs applications dans le cluster.

Découvrez comment [sécuriser vos informations personnelles](cs_secure.html#pi) lorsque vous utilisez des images de conteneur.

<br />


## Configuration de contenu sécurisé pour des images de conteneur
{: #trusted_images}

Vous pouvez générer des conteneurs à partir d'images sécurisées qui sont signées et stockées dans {{site.data.keyword.registryshort_notm}} et empêcher les déploiements à partir d'images non signées ou vulnérables.
{:shortdesc}

1.  [Signez les images pour le contenu sécurisé](/docs/services/Registry/registry_trusted_content.html#registry_trustedcontent). Après avoir défini la fonction de confiance (trust) pour vos images, vous pouvez gérer du contenu sécurisé et les signataires peuvent envoyer des images par commande push dans votre registre.
2.  Pour imposer une règle stipulant que seules des images signées peuvent être utilisées pour générer des conteneurs dans votre cluster, [ajoutez Container Image Security Enforcement (bêta)](/docs/services/Registry/registry_security_enforce.html#security_enforce).
3.  Déployez votre application.
    1. [Déploiement dans l'espace de nom Kubernetes `default`](#namespace).
    2. [Déploiement dans un autre espace de nom Kubernetes ou à partir d'une autre région ou d'un autre compte {{site.data.keyword.Bluemix_notm}}](#other).

<br />


## Déploiement de conteneurs à partir d'une image {{site.data.keyword.registryshort_notm}} dans l'espace de nom Kubernetes `default`
{: #namespace}

Vous pouvez déployer dans votre cluster des conteneurs depuis une image fournie par IBM ou depuis une image privée stockée dans votre espace de nom dans {{site.data.keyword.registryshort_notm}}.
{:shortdesc}

Lorsque vous créez un cluster, des jetons de registre sans date d'expiration et des valeurs confidentielles sont créés automatiquement pour le [registre régional le plus proche, tout comme pour le registre global](/docs/services/Registry/registry_overview.html#registry_regions). Le registre global stocke de manière sécurisée des images publiques fournies par IBM auxquelles vous pouvez vous référer dans vos déploiements au lieu d'utiliser des références différentes pour les images stockées dans chaque registre régional. Le registre régional stocke de manière sécurisée vos propres images Docker privées, tout comme les mêmes images publiques hébergées dans le registre global. Les jetons sont utilisés pour autoriser un accès en lecture seule aux espaces nom de votre choix que vous configurez dans {{site.data.keyword.registryshort_notm}} afin que vous puissiez utiliser ces images publiques (registre global) et privées (registres régionaux).

Chaque jeton doit être stocké dans un élément Kubernetes `imagePullSecret` de sorte à être accessible à un cluster Kubernetes lorsque vous déployez une application conteneurisée. Lorsque votre cluster est créé, {{site.data.keyword.containershort_notm}} stocke automatiquement les jetons pour le registre global (images publiques fournies par IBM) et pour les registres régionaux dans des valeurs confidentielles Kubernetes pour extraction d'images. Les valeurs confidentielles d'extraction d'images (imagePullSecret) sont ajoutées à l'espace nom Kubernetes nommé `default`, la liste par défaut des valeurs confidentielles dans l'élément `ServiceAccount` pour cet espace nom, et l'espace nom `kube-system`.

**Remarque :** avec cette configuration initiale, vous pouvez déployer des conteneurs depuis n'importe quelle image disponible dans un espace de nom dans votre compte {{site.data.keyword.Bluemix_notm}} vers l'espace de nom nommé **default** de votre cluster. Pour déployer un conteneur dans d'autres espaces de nom de votre cluster ou utiliser une image stockée dans une autre région {{site.data.keyword.Bluemix_notm}}, ou dans un autre compte {{site.data.keyword.Bluemix_notm}}, vous devez [créer votre propre élément imagePullSecret pour votre cluster](#other).

Avant de commencer :
1. [Configurez un espace de nom dans {{site.data.keyword.registryshort_notm}} sur {{site.data.keyword.Bluemix_notm}} public ou {{site.data.keyword.Bluemix_dedicated_notm}} et transférez par commande push des images dans cet espace de nom](/docs/services/Registry/registry_setup_cli_namespace.html#registry_namespace_add).
2. [Créez un cluster](cs_clusters.html#clusters_cli).
3. [Ciblez avec votre interface CLI votre cluster](cs_cli_install.html#cs_cli_configure).

Pour déployer un conteneur dans l'espace de nom **default** de votre cluster, créez un fichier de configuration.

1.  Créez un fichier de configuration de déploiement nommé `mydeployment.yaml`.
2.  Définissez le déploiement et l'image de votre espace de nom que vous désirez utiliser dans {{site.data.keyword.registryshort_notm}}.

    Pour utiliser une image privée d'un espace de nom dans {{site.data.keyword.registryshort_notm}} :

    ```
    apiVersion: apps/v1beta1
    kind: Deployment
    metadata:
      name: ibmliberty-deployment
    spec:
      replicas: 3
      template:
        metadata:
          labels:
            app: ibmliberty
        spec:
          containers:
          - name: ibmliberty
            image: registry.<region>.bluemix.net/<namespace>/<my_image>:<tag>
    ```
    {: codeblock}

    **Astuce :** pour extraire vos informations d'espace de nom, exécutez la commande `bx cr namespace-list`.

3.  Créez le déploiement dans votre cluster.

    ```
    kubectl apply -f mydeployment.yaml
    ```
    {: pre}

    **Astuce :** vous pouvez également déployer un fichier de configuration existant, tel que l'une des images publiques fournies par IBM. Cet exemple utilise l'image **ibmliberty** dans la région Sud des Etats-Unis.

    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/deploy-ibmliberty.yaml
    ```
    {: pre}


<br />



## Création d'un élément `imagePullSecret` pour accéder à {{site.data.keyword.Bluemix_notm}} ou à des registres privés externes dans d'autres espaces de nom Kubernetes ou d'autres régions et comptes {{site.data.keyword.Bluemix_notm}}
{: #other}

Créez votre élément `imagePullSecret` pour déployer des conteneurs sur d'autres espaces de nom Kubernetes, utiliser des images stockées dans d'autres régions ou comptes {{site.data.keyword.Bluemix_notm}}, utiliser des images stockées dans {{site.data.keyword.Bluemix_dedicated_notm}} ou utiliser des images stockées dans des registres privés externes.
{:shortdesc}

Les éléments ImagePullSecret ne sont valides que pour les espaces de nom pour lesquels ils ont été créés. Répétez ces étapes pour chaque espace de nom dans lequel vous désirez déployer des conteneurs. Les images de [DockerHub](#dockerhub) ne nécessitent pas ImagePullSecrets.
{: tip}

Avant de commencer :

1.  [Configurez un espace de nom dans {{site.data.keyword.registryshort_notm}} sur {{site.data.keyword.Bluemix_notm}} public ou {{site.data.keyword.Bluemix_dedicated_notm}} et transférez par commande push des images dans cet espace de nom](/docs/services/Registry/registry_setup_cli_namespace.html#registry_namespace_add).
2.  [Créez un cluster](cs_clusters.html#clusters_cli).
3.  [Ciblez avec votre interface CLI votre cluster](cs_cli_install.html#cs_cli_configure).

<br/>
Pour créer votre propre élément imagePullSecret, vous pouvez choisir une des options suivantes :
- [Copier l'élément imagePullSecret à partir de l'espace de nom default dans d'autres espaces de nom de votre cluster](#copy_imagePullSecret).
- [Créer un élément imagePullSecret pour accéder à des images figurant dans d'autres régions ou comptes {{site.data.keyword.Bluemix_notm}}](#other_regions_accounts).
- [Créer un élément imagePullSecret pour accéder à des images figurant dans des registres privés externes](#private_images).

<br/>
Si vous avez déjà créé un élément imagePullSecret dans votre espace de nom que vous souhaitez utiliser dans votre déploiement, voir [Déploiement de conteneurs à l'aide de l'élément imagePullSecret créé](#use_imagePullSecret).

### Copier l'élément imagePullSecret à partir de l'espace de nom default dans d'autres espaces de nom de votre cluster
{: #copy_imagePullSecret}

Vous pouvez copier l'élément imagePullSecret qui est créé automatiquement pour l'espace de nom Kubernetes `default` dans d'autres espaces de nom de votre cluster.
{: shortdesc}

1. Répertoriez les espaces de nom disponibles dans votre cluster.
   ```
   kubectl get namespaces
   ```
   {: pre}

   Exemple de sortie :
   ```
   default          Active    79d
   ibm-cert-store   Active    79d
   ibm-system       Active    79d
   istio-system     Active    34d
   kube-public      Active    79d
   kube-system      Active    79d
   ```
   {: screen}

2. Facultatif : créez un espace de nom dans votre cluster.
   ```
   kubectl create namespace <namespace_name>
   ```
   {: pre}

3. Copiez les éléments imagePullSecret de l'espace de nom `default` vers l'espace de nom de votre choix. Les nouveaux éléments imagePullSecrets sont nommés `bluemix-<namespace_name>-secret-regional` et `bluemix-<namespace_name>-secret-international`.
   ```
   kubectl get secret bluemix-default-secret-regional -o yaml | sed 's/default/<namespace_name>/g' | kubectl -n <namespace_name> create -f -
   ```
   {: pre}
   
   ```
   kubectl get secret bluemix-default-secret-international -o yaml | sed 's/default/<namespace_name>/g' | kubectl -n <namespace_name> create -f -
   ```
   {: pre}

4.  Vérifiez que la création de la valeur confidentielle a abouti.
    ```
    kubectl get secrets --namespace <namespace_name>
    ```
    {: pre}

5. [Déployez un conteneur à l'aide de l'élément imagePullSecret](#use_imagePullSecret) dans votre espace de nom.


### Création d'un élément imagePullSecret pour accéder aux images figurant dans d'autres régions ou comptes {{site.data.keyword.Bluemix_notm}}
{: #other_regions_accounts}

Pour accéder à des images figurant dans d'autres régions ou comptes {{site.data.keyword.Bluemix_notm}}, vous devez créer un jeton de registre et sauvegarder vos données d'identification dans un élément imagePullSecret.
{: shortdesc}

1.  Si vous ne possédez pas de jeton, [créez un jeton pour le registre auquel vous souhaitez accéder. ](/docs/services/Registry/registry_tokens.html#registry_tokens_create)
2.  Répertoriez les jetons dans votre compte {{site.data.keyword.Bluemix_notm}}.

    ```
    bx cr token-list
    ```
    {: pre}

3.  Notez l'ID du jeton que vous désirez utiliser.
4.  Extrayez la valeur de ce jeton. Remplacez <em>&lt;token_ID&gt;</em> par l'ID du jeton que vous avez récupéré à l'étape précédente.

    ```
    bx cr token-get <token_id>
    ```
    {: pre}

    La valeur de ce jeton est affichée dans la zone **Token** de votre sortie CLI.

5.  Créez la valeur confidentielle Kubernetes pour stocker votre information de jeton.

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_URL> --docker-username=token --docker-password=<token_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>Description des composantes de cette commande</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composantes de cette commande</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>Obligatoire. Espace de nom Kubernetes de votre cluster dans lequel vous désirez utiliser la valeur confidentielle et déployer des conteneurs. Exécutez la commande <code>kubectl get namespaces</code> pour répertorier tous les espaces de nom dans votre cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>Obligatoire. Nom que vous désirez utiliser pour l'élément imagePullSecret.</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>Obligatoire. URL du registre d'images où votre espace de nom est configuré.<ul><li>Pour les espaces de nom définis au Sud et à l'Est des Etats-Unis  : registry.ng.bluemix.net</li><li>Pour les espaces de nom définis au Sud du Royaume-Uni : registry.eu-gb.bluemix.net</li><li>Pour les espaces de nom définis en Europe centrale (Francfort) : registry.eu-de.bluemix.net</li><li>Pour les espaces de nom définis en Australie (Sydney) : registry.au-syd.bluemix.net</li><li>Pour les espaces de nom définis dans le registre {{site.data.keyword.Bluemix_dedicated_notm}}.<em>&lt;dedicated_domain&gt;</em></li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>Obligatoire. Nom d'utilisateur pour connexion à votre registre privé. Pour {{site.data.keyword.registryshort_notm}}, le nom d'utilisateur est défini avec <code>token</code>.</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>Obligatoire. Valeur de votre jeton de registre extraite auparavant.</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>Obligatoire. Si vous en avez une, entrez votre adresse e-mail Docker. A défaut, indiquez une adresse e-mail fictive (par exemple, a@b.c). Cet e-mail est obligatoire pour créer une valeur confidentielle Kubernetes, mais n'est pas utilisé après la création.</td>
    </tr>
    </tbody></table>

6.  Vérifiez que la création de la valeur confidentielle a abouti. Remplacez <em>&lt;kubernetes_namespace&gt;</em> par l'espace de nom dans lequel vous avez créé l'élément imagePullSecret.

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

7.  [Déployez un conteneur à l'aide de l'élément imagePullSecret](#use_imagePullSecret) dans votre espace de nom.

### Accès aux images stockées dans d'autres registres privés
{: #private_images}

Si vous disposez déjà d'un registre privé, vous devez stocker les données d'identification du registre dans un élément Kubernetes imagePullSecret et référencer cette valeur confidentielle depuis votre fichier de configuration.
{:shortdesc}

Avant de commencer :

1.  [Créez un cluster](cs_clusters.html#clusters_cli).
2.  [Ciblez avec votre interface CLI votre cluster](cs_cli_install.html#cs_cli_configure).

Pour créer un élément imagePullSecret :

1.  Créez la valeur confidentielle Kubernetes pour stocker vos données d'identification de registre privé.

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_URL> --docker-username=<docker_username> --docker-password=<docker_password> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>Description des composantes de cette commande</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composantes de cette commande</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>Obligatoire. Espace de nom Kubernetes de votre cluster dans lequel vous désirez utiliser la valeur confidentielle et déployer des conteneurs. Exécutez la commande <code>kubectl get namespaces</code> pour répertorier tous les espaces de nom dans votre cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>Obligatoire. Nom que vous désirez utiliser pour l'élément imagePullSecret.</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>Obligatoire. URL du registre dans lequel sont stockées vos images privées.</td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>Obligatoire. Nom d'utilisateur pour connexion à votre registre privé.</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>Obligatoire. Valeur de votre jeton de registre extraite auparavant.</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>Obligatoire. Si vous en avez une, entrez votre adresse e-mail Docker. Si vous n'en avez pas, indiquez une adresse e-mail fictive (par exemple, a@b.c). Cet e-mail est obligatoire pour créer une valeur confidentielle Kubernetes, mais n'est pas utilisé après la création.</td>
    </tr>
    </tbody></table>

2.  Vérifiez que la création de la valeur confidentielle a abouti. Remplacez <em>&lt;kubernetes_namespace&gt;</em> par le nom de l'espace de nom sur lequel vous avez créé l'élément imagePullSecret.

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

3.  [Créez un pod faisant référence à l'élément imagePullSecret](#use_imagePullSecret).

## Déploiement de conteneurs à l'aide de l'élément imagePullSecret créé
{: #use_imagePullSecret}

Vous pouvez définir un élément imagePullSecret dans votre déploiement de pod ou stocker l'élément imagePullSecret dans votre compte de service Kubernetes de sorte à ce qu'il soit disponible pour tous les déploiements sans compte de service spécifié.
{: shortdesc}

Sélectionnez l'une des options suivantes :
* [Référencer l'élément imagePullSecret dans votre déploiement de pod](#pod_imagePullSecret) : utilisez cette option si, par défaut, vous ne souhaitez pas octroyer l'accès à votre registre pour tous les pods de votre espace de nom.
* [Stocker l'élément imagePullSecret dans le compte de service Kubernetes](#store_imagePullSecret) : utilisez cette option pour octroyer l'accès aux images de votre registre pour les déploiements dans les espaces de nom Kubernetes sélectionnés.

Avant de commencer :
* [Créez un élément imagePullSecret](#other) pour accéder aux images d'autres registres, d'autres espaces de nom Kubernetes ou d'autres régions ou comptes {{site.data.keyword.Bluemix_notm}}.
* [Ciblez avec votre interface CLI votre cluster](cs_cli_install.html#cs_cli_configure).

### Référencer l'élément `imagePullSecret` dans votre déploiement de pod
{: #pod_imagePullSecret}

Lorsque vous référencez l'élément imagePullSecret dans un déploiement de pod, l'élément imagePullSecret n'est valide que pour ce pod et ne peut pas être partagé entre les pods dans l'espace de nom.
{:shortdesc}

1.  Créez un fichier de configuration de pod nommé `mypod.yaml`.
2.  Définissez le pod et l'élément imagePullSecret pour accéder au registre privé d'{{site.data.keyword.registrylong_notm}}.

    Pour accéder à une image privée :
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod
    spec:
      containers:
        - name: <container_name>
          image: registry.<region>.bluemix.net/<namespace_name>/<image_name>:<tag>
      imagePullSecrets:
        - name: <secret_name>
    ```
    {: codeblock}

    Pour accéder à une image publique {{site.data.keyword.Bluemix_notm}} :
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod
    spec:
      containers:
        - name: <container_name>
          image: registry.bluemix.net/<image_name>:<tag>
      imagePullSecrets:
        - name: <secret_name>
    ```
    {: codeblock}

    <table>
    <caption>Description des composants du fichier YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants du fichier YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>&lt;container_name&gt;</em></code></td>
    <td>Nom du conteneur à déployer dans votre cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;namespace_name&gt;</em></code></td>
    <td>Espace de nom sous lequel l'image est stockée. Pour répertorier les espaces de nom disponibles, exécutez la commande `bx cr namespace-list`.</td>
    </tr>
    <tr>
    <td><code><em>&lt;image_name&gt;</em></code></td>
    <td>Nom de l'image que vous désirez utiliser. Pour répertorier les images disponibles dans un compte {{site.data.keyword.Bluemix_notm}}, exécutez la commande `bx cr image-list`.</td>
    </tr>
    <tr>
    <td><code><em>&lt;tag&gt;</em></code></td>
    <td>Version de l'image que vous désirez utiliser. Si aucune étiquette n'est spécifiée, celle correspondant à <strong>latest</strong> (la plus récente) est utilisée par défaut.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>Nom de l'élément imagePullSecret que vous avez créé plus tôt.</td>
    </tr>
    </tbody></table>

3.  Sauvegardez vos modifications.
4.  Créez le déploiement dans votre cluster.
    ```
    kubectl apply -f mypod.yaml
    ```
    {: pre}

### Stocker l'élément imagePullSecret dans le compte de service Kubernetes pour l'espace de nom sélectionné
{:#store_imagePullSecret}

Tous les espaces de nom ont un compte de service Kubernetes nommé `default`. Vous pouvez ajouter l'élément imagePullSecret à ce compte de service pour octroyer l'accès aux images de votre registre. Les déploiements pour lesquels aucun compte de service n'est spécifié utilisent automatiquement le compte de service `default` pour cet espace de nom.
{:shortdesc}

1. Vérifiez s'il existe déjà un élément imagePullSecret pour le compte de service default.
   ```
   kubectl describe serviceaccount default -n <namespace_name>
   ```
   {: pre}
   Lorsque `<none>` s'affiche dans l'entrée **ImagePullSecrets**, il n'existe aucun élément imagePullSecret.  
2. Ajoutez l'élément imagePullSecret dans votre compte de service default.
   - **Pour ajouter un élément imagePullSecret lorsqu'aucun élément de ce type n'est défini :**
       ```
       kubectl patch -n <namespace_name> serviceaccount/default -p '{"imagePullSecrets":[{"name": "bluemix-<namespace_name>-secret-regional"}]}'
       ```
       {: pre}
   - **Pour ajouter l'élément imagePullSecret lorsqu'il y en a un déjà défini :**
       ```
       kubectl patch -n <namespace_name> serviceaccount/default --type='json' -p='[{"op":"add","path":"/imagePullSecrets/-","value":{"name":"bluemix-<namespace_name>-secret-regional"}}]'
       ```
       {: pre}
3. Vérifiez que l'élément imagePullSecret a été ajouté dans votre compte de service default.
   ```
   kubectl describe serviceaccount default -n <namespace_name>
   ```
   {: pre}

   Exemple de sortie :
   ```
   Name:                default
   Namespace:           <namespace_name>
   Labels:              <none>
   Annotations:         <none>
   Image pull secrets:  bluemix-namespace_name-secret-regional
   Mountable secrets:   default-token-sh2dx
   Tokens:              default-token-sh2dx
   Events:              <none>
   ```
   {: pre}

4. Déployez un conteneur à partir d'une image dans votre registre.
   ```
   apiVersion: v1
   kind: Pod
   metadata:
     name: mypod
   spec:
     containers:
       - name: <container_name>
         image: registry.<region>.bluemix.net/<namespace_name>/<image_name>:<tag>
   ```
   {: codeblock}

5. Créez le déploiement dans le cluster.
   ```
   kubectl apply -f mypod.yaml
   ```
   {: pre}

<br />


