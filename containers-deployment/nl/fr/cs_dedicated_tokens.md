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





# Création d'un jeton {{site.data.keyword.registryshort_notm}} pour un registre d'images {{site.data.keyword.Bluemix_dedicated_notm}}
{: #cs_dedicated_tokens}

Créez un jeton sans date d'expiration pour un registre d'images que vous utilisez pour des groupes de conteneurs uniques et évolutifs avec des clusters dans {{site.data.keyword.containerlong}}.
{:shortdesc}

1.  Connectez-vous à l'environnement {{site.data.keyword.Bluemix_dedicated_notm}}.

    ```
    bx login -a api.<dedicated_domain>
    ```
    {: pre}

2.  Extrayez un jeton `oauth-token` pour la session actuelle et enregistrez-le en tant que variable.

    ```
    OAUTH_TOKEN=`bx iam oauth-tokens | awk 'FNR == 2 {print $3 " " $4}'`
    ```
    {: pre}

3.  Extrayez l'ID de l'organisation pour la session actuelle et enregistrez-le en tant que variable.

    ```
    ORG_GUID=`bx iam org <org_name> --guid`
    ```
    {: pre}

4.  Extrayez un jeton de registre permanent pour la session actuelle. Remplacez <dedicated_domain> par le domaine pour votre environnement {{site.data.keyword.Bluemix_dedicated_notm}}. Ce jeton permet l'accès aux images dans l'espace de nom actuel.

    ```
    curl -XPOST -H "Authorization: ${OAUTH_TOKEN}" -H "Organization: ${ORG_GUID}" https://registry.<dedicated_domain>/api/v1/tokens?permanent=true
    ```
    {: pre}

    Sortie :

    ```
    {
        "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI2MzdiM2Q4Yy1hMDg3LTVhZjktYTYzNi0xNmU3ZWZjNzA5NjciLCJpc3MiOiJyZWdpc3RyeS5jZnNkZWRpY2F0ZWQxLnVzLXNvdXRoLmJsdWVtaXgubmV0"
    }
    ```
    {: screen}

5.  Vérifiez la valeur confidentielle Kubernetes.

    ```
    kubectl describe secrets
    ```
    {: pre}

    Vous pouvez utiliser cette valeur confidentielle pour travailler dans {{site.data.keyword.containerlong}}.

6.  Créez la valeur confidentielle Kubernetes pour stocker votre information de jeton.

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_url> --docker-username=token --docker-password=<token_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>Description des composantes de cette commande</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composantes de cette commande</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace &lt;kubernetes_namespace&gt;</code></td>
    <td>Obligatoire. Espace de nom Kubernetes de votre cluster dans lequel vous désirez utiliser la valeur confidentielle et déployer des conteneurs. Exécutez la commande <code>kubectl get namespaces</code> pour répertorier tous les espaces de nom dans votre cluster.</td>
    </tr>
    <tr>
    <td><code>&lt;secret_name&gt;</code></td>
    <td>Obligatoire. Nom que vous désirez utiliser pour l'élément imagePullSecret.</td>
    </tr>
    <tr>
    <td><code>--docker-server &lt;registry_url&gt;</code></td>
    <td>Obligatoire. URL du registre d'images où votre espace de nom est configuré : registry.&lt;dedicated_domain&gt;</li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username &lt;docker_username&gt;</code></td>
    <td>Obligatoire. Nom d'utilisateur pour connexion à votre registre privé.</td>
    </tr>
    <tr>
    <td><code>--docker-password &lt;token_value&gt;</code></td>
    <td>Obligatoire. Valeur de votre jeton de registre extraite auparavant.</td>
    </tr>
    <tr>
    <td><code>--docker-email &lt;docker-email&gt;</code></td>
    <td>Obligatoire. Si vous en avez une, entrez votre adresse e-mail Docker. Si vous n'en avez pas, indiquez une adresse e-mail fictive (par exemple, a@b.c). Cet e-mail est obligatoire pour créer une valeur confidentielle Kubernetes, mais n'est pas utilisé après la création.</td>
    </tr>
    </tbody></table>

7.  Créez un pod référençant l'élément imagePullSecret.

    1.  Ouvrez l'éditeur de texte de votre choix et créez un script de configuration de pod nommé, par exemple, mypod.yaml.
    2.  Définissez le pod et la valeur imagePullSecret que vous désirez utiliser pour accéder au registre. Pour utiliser une image privée d'un espace de nom :

        ```
        apiVersion: v1
        kind: Pod
        metadata:
          name: <pod_name>
        spec:
          containers:
            - name: <container_name>
              image: registry.<dedicated_domain>/<my_namespace>/<my_image>:<tag>
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
        <td><code>&lt;pod_name&gt;</code></td>
        <td>Nom du pod que vous désirez créer.</td>
        </tr>
        <tr>
        <td><code>&lt;container_name&gt;</code></td>
        <td>Nom du conteneur que vous désirez déployer dans votre cluster.</td>
        </tr>
        <tr>
        <td><code>&lt;my_namespace&gt;</code></td>
        <td>Espace de nom sous lequel votre image est stockée. Pour répertorier les espaces de nom disponibles, exécutez la commande `bx cr namespace-list`.</td>
        </tr>
        <td><code>&lt;my_image&gt;</code></td>
        <td>Nom de l'image que vous désirez utiliser. Pour répertorier les images disponibles dans un compte {{site.data.keyword.Bluemix_notm}}, exécutez la commande <code>bx cr image-list</code>.</td>
        </tr>
        <tr>
        <td><code>&lt;tag&gt;</code></td>
        <td>Version de l'image que vous désirez utiliser. Si aucune étiquette n'est spécifiée, celle correspondant à <strong>latest</strong> (la plus récente) est utilisée par défaut.</td>
        </tr>
        <tr>
        <td><code>&lt;secret_name&gt;</code></td>
        <td>Nom de l'élément imagePullSecret que vous avez créé plus tôt.</td>
        </tr>
        </tbody></table>

    3.  Sauvegardez vos modifications.

    4.  Créez le déploiement dans votre cluster.

          ```
          kubectl apply -f mypod.yaml
          ```
          {: pre}

