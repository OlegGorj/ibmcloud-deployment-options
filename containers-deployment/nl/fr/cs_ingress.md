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




# Exposition d'applications avec Ingress
{: #ingress}

Vous pouvez exposer plusieurs applications dans votre cluster Kubernetes en créant des ressources Ingress gérées par l'équilibreur de charge d'application (ALB) fourni par IBM dans {{site.data.keyword.containerlong}}.
{:shortdesc}

## Gestion de trafic réseau à l'aide d'Ingress
{: #planning}

Ingress est un service Kubernetes qui permet d'équilibrer les charges de travail du trafic réseau dans votre cluster en transférant des demandes publiques ou privées à vos applications. Vous pouvez utiliser Ingress pour exposer plusieurs services d'application sur un réseau public ou privé en utilisant une seule route publique ou privée.
{:shortdesc}



Ingress comporte deux composants :
<dl>
<dt>Equilibreur de charge d'application</dt>
<dd>L'équilibreur de charge d'application (ALB) est un équilibreur de charge externe qui est à l'écoute des demandes de service HTTP, HTTPS, TCP ou UDP entrantes et transmet les demandes au pod d'application approprié. Lorsque vous créez un cluster standard, {{site.data.keyword.containershort_notm}} crée automatiquement un équilibreur ALB à haute disponibilité pour votre cluster et lui affecte une route publique unique. La route publique est liée à une adresse IP publique portable allouée à votre compte d'infrastructure IBM Cloud (SoftLayer) lors de la création du cluster. Un équilibreur de charge ALB privé par défaut est également créé automatiquement, mais n'est pas activé automatiquement.</dd>
<dt>Ressource Ingress</dt>
<dd>Pour exposer une application à l'aide d'Ingress, vous devez créer un service Kubernetes pour votre application et enregistrer ce service auprès de l'équilibreur de charge ALB en définissant une ressource Ingress. La ressource Ingress est une ressource Kubernetes qui définit les règles de routage des demandes entrantes pour une application. Elle spécifie également le chemin d'accès à votre service d'application, qui est ajouté à la route publique pour composer une URL d'application unique, telle que `mycluster.us-south.containers.appdomain.cloud/myapp`.<br></br><strong>Remarque</strong> : à partir du 24 mai 2018, le format du sous-domaine Ingress a changé pour les nouveaux clusters.<ul><li>Pour les clusters créés après le 24 mai, le sous-domaine qui leur est affecté est au nouveau format, <code>&lt;cluster_name&gt;.&lt;region&gt;.containers.appdomain.cloud</code>.</li><li>Les clusters créés avant le 24 mai 2018 continuent à utiliser le sous-domaine affecté avec l'ancien format, <code>&lt;cluster_name&gt;.&lt;region&gt;.containers.mybluemix.net</code>.</li></ul></dd>
</dl>

Le diagramme suivant montre comment Ingress achemine la communication vers une application depuis Internet :

<img src="images/cs_ingress.png" width="550" alt="Exposition d'une application dans {{site.data.keyword.containershort_notm}} en utilisant Ingress" style="width:550px; border-style: none"/>

1. Un utilisateur envoie une demande à votre application en accédant à l'URL de votre application. Il s'agit de l'URL publique de votre application exposée avec le chemin de la ressource Ingress ajouté à la suite, par exemple `mycluster.us-south.containers.appdomain.cloud/myapp`.

2. Un service système DNS qui fait office d'équilibreur de charge global résout l'URL avec l'adresse IP publique portable de l'équilibreur de charge d'application (ALB) public par défaut dans le cluster. La demande est acheminée vers le service ALB Kubernetes correspondant à l'application.

3. Le service Kubernetes achemine la demande à l'équilibreur de charge ALB.

4. L'équilibreur de charge ALB vérifie s'il existe une règle de routage pour le chemin `myapp` dans le cluster. Si une règle correspondante est trouvée, la demande est transmise en fonction des règles que vous avez définies dans la ressource Ingress au pod sur lequel est déployée l'application. Si plusieurs instances sont déployées dans le cluster, l'équilibreur de charge ALB équilibre les demandes entre les pods d'application.



<br />


## Conditions prérequises
{: #config_prereqs}

Avant d'utiliser Ingress, consultez les conditions prérequises suivantes.
{:shortdesc}

**Conditions prérequises pour toutes les configurations Ingress :**
- Ingress n'est disponible que pour les clusters standard et nécessite au moins deux noeuds worker dans le cluster pour garantir une haute disponibilité et l'application régulière de mises à jour. 
- La configuration d'Ingress nécessite une [règle d'accès administrateur](cs_users.html#access_policies). Vérifiez votre [règle d'accès actuelle](cs_users.html#infra_access).



<br />


## Planification réseau pour un ou plusieurs espaces de nom
{: #multiple_namespaces}

Au moins une ressource Ingress est requise par espace de nom qui héberge les applications que vous souhaitez exposer.
{:shortdesc}

<dl>
<dt>Toutes les applications se trouvent dans un espace de nom</dt>
<dd>Si les applications de votre cluster figurent toutes dans le même espace de nom, au moins une ressource Ingress est nécessaire pour définir les règles de routage pour les applications exposées à cet endroit. Par exemple, si les applications `app1` et `app2` sont exposées par des services dans un espace de nom de développement, vous pouvez créer une ressource Ingress dans cet espace de nom. La ressource indique `domain.net` comme hôte et enregistre les chemins où chaque application est à l'écoute avec `domain.net`.
<br></br><img src="images/cs_ingress_single_ns.png" width="300" alt="Au moins une ressource est requise par espace de nom." style="width:300px; border-style: none"/>
</dd>
<dt>Les applications se trouvent dans plusieurs espaces de nom</dt>
<dd>Si les applications de votre cluster figurent dans différents espaces de nom, vous devez créer au moins une ressource par espace de nom pour définir les règles des applications exposées à cet endroit. Pour enregistrer plusieurs ressources Ingress avec l'équilibreur de charge d'application (ALB) Ingress du cluster, vous devez utiliser un domaine générique. Lorsqu'un domaine générique, tel que `*.mycluster.us-south.containers.appdomain.cloud` est enregistré, plusieurs sous-domaines sont résolus avec le même hôte. Ensuite, vous pouvez créer une ressource Ingress dans chaque espace de nom et spécifier un sous-domaine différent dans chaque ressource Ingress.
<br><br>
Vous pouvez par exemple, envisager le scénario suivant :<ul>
<li>Vous disposez de deux versions de la même application, `app1` et `app3`, à des fins de test.</li>
<li>Vous déployez les applications dans deux espaces de nom différents au sein du même cluster : `app1` dans l'espace de nom de développement et `app3` dans l'espace de nom de préproduction.</li></ul>
Pour utiliser le même équilibreur de charge ALB du cluster pour gérer le trafic vers ces applications, vous créez les ressources et services suivants :<ul>
<li>Un service Kubernetes dans votre espace de nom de développement pour exposer `app1`.</li>
<li>Une ressource Ingress dans l'espace de nom de développement qui spécifie l'hôte avec `dev.mycluster.us-south.containers.appdomain.cloud`.</li>
<li>Un service Kubernetes dans l'espace de nom de préproduction pour exposer `app3`.</li>
<li>Une ressource Ingress dans l'espace de nom de préproduction qui spécifie l'hôte avec `stage.mycluster.us-south.containers.appdomain.cloud`.</li></ul></br>
<img src="images/cs_ingress_multi_ns.png" alt="Dans un espace de nom, utilisez des sous-domaines dans une ou plusieurs ressources" style="border-style: none"/>
Désormais, les deux URL sont résolues par le même domaine et sont donc traitées par le même équilibreur de charge ALB. Cependant, comme la ressource dans l'espace de nom de préproduction est enregistrée avec le sous-domaine `stage`, l'équilibreur de charge ALB Ingress achemine correctement les demandes de l'URL `stage.mycluster.us-south.containers.appdomain.cloud/app3` uniquement vers `app3`.</dd>
</dl>

**Remarque **:
* Le domaine générique de sous-domaine Ingress fourni par IBM, `*.<cluster_name>.<region>.containers.appdomain.cloud`, est enregistré par défaut pour votre cluster. Cependant, TLS n'est pas pris en charge pour le format générique du sous-domaine Ingress fourni par IBM.
* Pour utiliser un domaine personnalisé, vous devez enregistrer le domaine personnalisé en tant que domaine générique, par exemple : `*.custom_domain.net`. Pour utiliser TLS, vous devez obtenir un certificat générique.

### Plusieurs domaines au sein d'un espace de nom

Dans un espace de nom individuel, vous pouvez utiliser un domaine pour accéder à toutes les applications dans l'espace de nom. Si vous souhaitez utiliser différents domaines pour les applications figurant dans un espace de nom, utilisez un domaine générique. Lorsqu'un domaine générique, tel que `*.mycluster.us-south.containers.appdomain.cloud` est enregistré, les sous-domaines multiples sont résolus avec le même hôte. Vous pouvez ensuite utiliser une ressource pour spécifier plusieurs hôtes de sous-domaines multiples dans cette ressource. Autrement, vous pouvez créer plusieurs ressources Ingress dans l'espace de nom et indiquer un sous-domaine différent dans chaque ressource Ingress.

<img src="images/cs_ingress_single_ns_multi_subs.png" alt="Au moins une ressource est requise par espace de nom." style="border-style: none"/>

**Remarque **:
* Le domaine générique de sous-domaine Ingress fourni par IBM, `*.<cluster_name>.<region>.containers.appdomain.cloud`, est enregistré par défaut pour votre cluster. Cependant, TLS n'est pas pris en charge pour le format générique du sous-domaine Ingress fourni par IBM.
* Pour utiliser un domaine personnalisé, vous devez enregistrer le domaine personnalisé en tant que domaine générique, par exemple : `*.custom_domain.net`. Pour utiliser TLS, vous devez obtenir un certificat générique.

<br />


## Exposition d'applications figurant dans votre cluster au public
{: #ingress_expose_public}

Exposez les applications figurant dans votre cluster au public à l'aide de l'équilibreur de charge d'application (ALB) Ingress public.
{:shortdesc}

Avant de commencer :

-   Consultez les [Conditions prérequises](#config_prereqs) d'Ingress.
-   Si ce n'est déjà fait, [créez un cluster standard](cs_clusters.html#clusters_ui).
-   [Ciblez avec votre interface CLI](cs_cli_install.html#cs_cli_configure) votre cluster pour exécuter des commandes `kubectl`.

### Etape 1 : Déployez les applications et créez des services d'application
{: #public_inside_1}

Commencez par déployer vos applications et créer des services Kubernetes pour les exposer.
{: shortdesc}

1.  [Déployez votre application sur le cluster](cs_app.html#app_cli). Prenez soin d'ajouter un libellé à votre déploiement dans la section "metadata" de votre fichier de configuration, par exemple `app: code`. Ce libellé est nécessaire pour identifier tous les pods où s'exécute votre application afin de pouvoir inclure ces pods dans l'équilibrage de charge Ingress.

2.   Créez un service Kubernetes pour chaque application que vous désirez exposer. Votre application doit être exposée par un service Kubernetes pour que l'équilibreur de charge ALB du cluster puisse l'inclure dans l'équilibrage de charge Ingress.
      1.  Ouvrez l'éditeur de votre choix et créez un fichier de configuration de service nommé, par exemple, `myapp_service.yaml`.
      2.  Définissez un service pour l'application qu'exposera l'équilibreur de charge ALB.

          ```
          apiVersion: v1
          kind: Service
          metadata:
            name: myapp_service
          spec:
            selector:
              <selector_key>: <selector_value>
            ports:
             - protocol: TCP
               port: 8080
          ```
          {: codeblock}

          <table>
          <thead>
          <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants du fichier YAML du service ALB</th>
          </thead>
          <tbody>
          <tr>
          <td><code>selector</code></td>
          <td>Entrez la paire clé de libellé (<em>&lt;selector_key&gt;</em>) et valeur (<em>&lt;selector_value&gt;</em>) que vous désirez utiliser pour cibler les pods dans lesquels s'exécute votre application. Pour cibler vos pods et les inclure dans l'équilibrage de charge du service, assurez-vous que <em>&lt;selector_key&gt;</em> et <em>&lt;selector_value&gt;</em> sont identiques à la paire clé/valeur dans la section <code>spec.template.metadata.labels</code> du fichier YALM de déploiement.</td>
           </tr>
           <tr>
           <td><code>port</code></td>
           <td>Port sur lequel le service est à l'écoute.</td>
           </tr>
           </tbody></table>
      3.  Sauvegardez vos modifications.
      4.  Créez le service dans votre cluster. Si les applications sont déployées dans plusieurs espaces de nom dans votre cluster, vérifiez que le service se déploie dans le même espace de nom que l'application que vous souhaitez exposer.

          ```
          kubectl apply -f myapp_service.yaml [-n <namespace>]
          ```
          {: pre}
      5.  Répétez ces étapes pour chaque application que vous désirez exposer.


### Etape 2 : Sélectionnez un domaine d'application et une terminaison TLS
{: #public_inside_2}

Lorsque vous configurez l'équilibreur de charge d'application public, vous choisissez le domaine sur lequel vos applications seront accessibles et indiquez s'il faut utiliser une terminaison TLS.
{: shortdesc}

<dl>
<dt>Domaine</dt>
<dd>Vous pouvez utiliser le domaine fourni par IBM, par exemple <code>mycluster-12345.us-south.containers.appdomain.cloud/myapp</code>, pour accéder à votre application sur Internet. Pour utiliser à la place un domaine personnalisé, vous pouvez mapper votre domaine personnalisé au domaine fourni par IBM ou à l'adresse IP publique de l'équilibreur de charge ALB.</dd>
<dt>Terminaison TLS</dt>
<dd>L'équilibreur de charge ALB équilibre la charge du trafic réseau HTTP vers les applications de votre cluster. Pour équilibrer la charge des connexions HTTPS entrantes, vous pouvez configurer l'équilibreur de charge ALB pour déchiffrer le trafic réseau et transférer la demande déchiffrée aux applications exposées dans votre cluster. Si vous utilisez le sous-domaine Ingress fourni par IBM, vous pouvez utiliser le certificat TLS fourni par IBM. Actuellement, TLS n'est pas pris en charge pour les sous-domaines génériques fournis par IBM. Si vous utilisez un domaine personnalisé, vous pouvez utiliser votre propre certificat TLS pour gérer la terminaison TLS.</dd>
</dl>

Pour utiliser le domaine Ingress fourni par IBM :
1. Obtenez les détails de votre cluster. Remplacez _&lt;cluster_name_or_ID&gt;_ par le nom du cluster sur lequel sont déployées les applications que vos souhaitez exposer.

    ```
    bx cs cluster-get <cluster_name_or_ID>
    ```
    {: pre}

    Exemple de sortie :

    ```
    Name:                   mycluster
    ID:                     18a61a63c6a94b658596ca93d087aad9
    State:                  normal
    Created:                2018-01-12T18:33:35+0000
    Location:               dal10
    Master URL:             https://169.xx.xxx.xxx:26268
    Ingress Subdomain:      mycluster-12345.us-south.containers.appdomain.cloud
    Ingress Secret:         <tls_secret>
    Workers:                3
    Version:                1.9.7
    Owner Email:            owner@email.com
    Monitoring Dashboard:   <dashboard_URL>
    ```
    {: screen}
2. Obtenez le domaine fourni par IBM dans la zone **Ingress Subdomain**. Si vous souhaitez utiliser TLS, récupérez également la valeur confidentielle TLS (tls_secret) fournie par IBM dans la zone **Ingress Secret**.
    **Remarque **: si vous utilisez un sous-domaine générique, TLS n'est pas pris en charge.

Pour utiliser un domaine personnalisé :
1.    Créez un domaine personnalisé. Pour enregistrer votre domaine personnalisé, travaillez en collaboration avec votre fournisseur DNS (Domain Name Service) ou [{{site.data.keyword.Bluemix_notm}} DNS](/docs/infrastructure/dns/getting-started.html#getting-started-with-dns).
      * Si les applications que vous souhaitez exposer via Ingress se trouvent dans différents espaces de nom sur un cluster, enregistrez le domaine personnalisé en tant que domaine générique, par exemple `*.custom_domain.net`.

2.  Configurez votre domaine pour acheminer le trafic réseau entrant à l'équilibreur de charge ALB fourni par IBM. Sélectionnez l'une des options suivantes :
    -   Définir un alias pour votre domaine personnalisé en spécifiant le domaine fourni par IBM sous forme d'enregistrement de nom canonique (CNAME). Pour identifier le domaine Ingress fourni par IBM, exécutez `bx cs cluster-get <cluster_name>` et recherchez la zone **Ingress subdomain**.
    -   Mapper votre domaine personnalisé à l'adresse IP publique portable de l'équilibreur de charge ALB fourni par IBM en ajoutant l'adresse IP en tant qu'enregistrement. Pour identifier l'adresse IP publique portable de l'équilibreur de charge ALB, exécutez la commande `bx cs alb-get <public_alb_ID>`.
3.   Facultatif : si vous souhaitez utiliser TLS, importez ou créez un certificat TLS et une valeur confidentielle de clé. Si vous utilisez un domaine générique, veillez à importer ou créer un certificat générique.
      * Si un certificat TLS que vous désirez utiliser est stocké dans {{site.data.keyword.cloudcerts_long_notm}}, vous pouvez importer sa valeur confidentielle associée dans votre cluster en exécutant la commande suivante :
        ```
        bx cs alb-cert-deploy --secret-name <secret_name> --cluster <cluster_name_or_ID> --cert-crn <certificate_crn>
        ```
        {: pre}
      * Si vous ne disposez pas de certificat TLS, procédez comme suit :
        1. Créez un certificat TLS et une clé pour votre domaine codés au format PEM.
        2. Créez une valeur confidentielle qui utilise le certificat et la clé TLS. Remplacez <em>&lt;tls_secret_name&gt;</em> par le nom de votre valeur confidentielle Kubernetes, <em>&lt;tls_key_filepath&gt;</em> par le chemin de votre fichier de clés TLS et <em>&lt;tls_cert_filepath&gt;</em> par le chemin d'accès à votre fichier de certificat TLS personnalisé.
          ```
          kubectl create secret tls <tls_secret_name> --key <tls_key_filepath> --cert <tls_cert_filepath>
          ```
          {: pre}


### Etape 3 : Créez la ressource Ingress
{: #public_inside_3}

Les ressources Ingress définissent les règles de routage utilisées par l'équilibreur de charge d'application (ALB) pour acheminer le trafic vers votre service d'application.
{: shortdesc}

**Remarque :** si votre cluster dispose de plusieurs espaces de nom dans lesquels sont exposées les applications, au moins une ressource Ingress est requise par espace de nom. En revanche, chaque espace de nom doit utiliser un hôte différent. Vous devez enregistrer un domaine générique et indiquer un sous-domaine différent dans chaque ressource. Pour plus d'informations, voir [Planification réseau pour un ou plusieurs espaces de nom](#multiple_namespaces).

1. Ouvrez l'éditeur de votre choix et créez un fichier de configuration Ingress nommé, par exemple, `myingressresource.yaml`.

2. Définissez dans votre fichier de configuration une ressource Ingress utilisant le domaine fourni par IBM ou votre domaine personnalisé pour acheminer le trafic réseau entrant aux services que vous avez créés auparavant.

    Exemple de fichier YAML n'utilisant pas TLS :
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingressresource
    spec:
      rules:
      - host: <domain>
        http:
          paths:
          - path: /<app1_path>
            backend:
              serviceName: <app1_service>
              servicePort: 80
          - path: /<app2_path>
            backend:
              serviceName: <app2_service>
              servicePort: 80
    ```
    {: codeblock}

    Exemple de fichier YAML utilisant TLS :
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingressresource
    spec:
      tls:
      - hosts:
        - <domain>
        secretName: <tls_secret_name>
      rules:
      - host: <domain>
        http:
          paths:
          - path: /<app1_path>
            backend:
              serviceName: <app1_service>
              servicePort: 80
          - path: /<app2_path>
            backend:
              serviceName: <app2_service>
              servicePort: 80
    ```
    {: codeblock}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants du fichier YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code>tls/hosts</code></td>
    <td>Pour utiliser TLS, remplacez <em>&lt;domain&gt;</em> par le sous-domaine Ingress fourni par IBM ou par votre domaine personnalisé.

    </br></br>
    <strong>Remarque :</strong><ul><li>Si vos applications sont exposées par des services dans différents espaces de nom, ajoutez un sous-domaine générique au début du domaine, par exemple `subdomain1.custom_domain.net` ou `subdomain1.mycluster.us-south.containers.appdomain.cloud`. Utilisez un sous-domaine unique pour chaque ressource que vous créez dans le cluster.</li><li>N'utilisez pas &ast; pour votre hôte ou laissez la propriété host vide pour éviter des erreurs lors de la création de la ressource Ingress.</li></ul></td>
    </tr>
    <tr>
    <td><code>tls/secretName</code></td>
    <td><ul><li>Si vous utilisez le domaine Ingress fourni par IBM, remplacez le nom de la valeur confidentielle TLS (<em>&lt;tls_secret_name&gt;</em>) par le nom de la valeur confidentielle Ingress (Ingress secret) fournie par IBM.</li><li>Si vous utilisez un domaine personnalisé, remplacez <em>&lt;tls_secret_name&gt;</em> par la valeur confidentielle que vous avez créée auparavant et qui contient votre certificat et votre clé TLS personnalisés. Si vous avez importé un certificat depuis {{site.data.keyword.cloudcerts_short}}, vous pouvez exécuter la commande <code>bx cs alb-cert-get --cluster <cluster_name_or_ID> --cert-crn <certificate_crn></code> pour examiner les valeurs confidentielles qui sont associées à un certificat TLS.</li><ul><td>
    </tr>
    <tr>
    <td><code>host</code></td>
    <td>Remplacez <em>&lt;domain&gt;</em> par le sous-domaine Ingress fourni par IBM ou par votre domaine personnalisé.

    </br></br>
    <strong>Remarque :</strong><ul><li>Si vos applications sont exposées par des services dans différents espaces de nom, ajoutez un sous-domaine générique au début du domaine, par exemple `subdomain1.custom_domain.net` ou `subdomain1.mycluster.us-south.containers.appdomain.cloud`. Utilisez un sous-domaine unique pour chaque ressource que vous créez dans le cluster.</li><li>N'utilisez pas &ast; pour votre hôte ou laissez la propriété host vide pour éviter des erreurs lors de la création de la ressource Ingress.</li></ul></td>
    </tr>
    <tr>
    <td><code>path</code></td>
    <td>Remplacez <em>&lt;app_path&gt;</em> par une barre oblique ou par le chemin sur lequel votre application est à l'écoute. Le chemin est ajouté au domaine fourni par IBM ou à votre domaine personnalisé de manière à créer une route unique vers votre application. Lorsque vous indiquez cette route dans un navigateur Web, le trafic réseau est acheminé vers l'équilibreur de charge ALB. L'équilibreur de charge ALB recherche le service associé et lui envoie le trafic réseau. Le service transfère ensuite le trafic aux pods sur lesquels s'exécute l'application.
    </br></br>
    De nombreuses applications ne sont pas à l'écoute sur un chemin spécifique mais utilisent le chemin racine et un port spécifique. Dans ce cas, définissez le chemin racine sous la forme <code>/</code>, sans spécifier de chemin individuel pour votre application. Exemples : <ul><li>Pour <code>http://domain/</code>, entrez <code>/</code> comme chemin.</li><li>Pour <code>http://domain/app1_path</code>, entrez <code>/app1_path</code> comme chemin.</li></ul>
    </br>
    <strong>Astuce :</strong> pour configurer Ingress pour qu'il soit à l'écoute sur un chemin différent de celui où votre application est à l'écoute, vous pouvez utiliser l'[annotation rewrite](cs_annotations.html#rewrite-path).</td>
    </tr>
    <tr>
    <td><code>serviceName</code></td>
    <td>Remplacez <em>&lt;app1_service&gt;</em> et <em>&lt;app2_service&gt;</em>, et ainsi de suite, par le nom des services que vous avez créés pour exposer vos applications. Si vos applications sont exposées par des services dans différents espaces de nom dans le cluster, incluez uniquement les services d'application figurant dans le même espace de nom. Vous devez créer une ressource Ingress pour chaque espace de nom contenant des applications que vous souhaitez exposer.</td>
    </tr>
    <tr>
    <td><code>servicePort</code></td>
    <td>Port sur lequel votre service est à l'écoute. Utilisez le même port que celui que vous avez défini lors de la création du service Kubernetes pour votre application.</td>
    </tr>
    </tbody></table>

3.  Créez la ressource Ingress pour votre cluster. Veillez à ce que la ressource se déploie dans le même espace de nom que les services d'application que vous avez spécifiés dans la ressource.

    ```
    kubectl apply -f myingressresource.yaml -n <namespace>
    ```
    {: pre}
4.   Vérifiez que la création de la ressource Ingress a abouti.

      ```
      kubectl describe ingress myingressresource
      ```
      {: pre}

      1. Si les messages compris dans les événements indiquent une erreur dans la configuration de votre ressource, modifiez les valeurs du fichier de votre ressource et réappliquez le fichier correspondant à la ressource.


Votre ressource Ingress est créée dans le même espace de nom que vos services d'application. Vos applications figurant dans cet espace de nom sont enregistrées avec l'équilibreur de charge ALB Ingress du cluster.

### Etape 4 : Accédez à votre application sur Internet
{: #public_inside_4}

Dans un navigateur Web, entrez l'URL du service d'application auquel accéder.

```
https://<domain>/<app1_path>
```
{: pre}

Si vous exposez plusieurs applications, accédez à ces applications en modifiant le chemin ajouté à l'URL.

```
https://<domain>/<app2_path>
```
{: pre}

Si vous utilisez un domaine générique pour exposer des applications dans différents espaces de nom, accédez à ces applications avec leurs sous-domaines respectifs.

```
http://<subdomain1>.<domain>/<app1_path>
```
{: pre}

```
http://<subdomain2>.<domain>/<app1_path>
```
{: pre}


<br />


## Exposition au public d'applications résidant hors de votre cluster
{: #external_endpoint}

Exposez des applications résidant hors de votre cluster au public en les incluant dans l'équilibrage de charge de l'ALB Ingress public. Les demandes publiques entrantes sur le domaine fourni par IBM ou sur votre domaine personnalisé sont transmises automatiquement à l'application externe.
{:shortdesc}

Avant de commencer :

-   Consultez les [Conditions prérequises](#config_prereqs) d'Ingress.
-   Si ce n'est déjà fait, [créez un cluster standard](cs_clusters.html#clusters_ui).
-   [Ciblez avec votre interface CLI](cs_cli_install.html#cs_cli_configure) votre cluster pour exécuter des commandes `kubectl`.
-   Vérifiez que l'application externe que vous désirez englober dans l'équilibrage de charge du cluster est accessible via une adresse IP publique.

### Etape 1 : Créez un service d'application et un noeud final externe
{: #public_outside_1}

Commencez par créer un service Kubernetes pour exposer l'application externe et par configurer un noeud final externe Kubernetes pour l'application.
{: shortdesc}

1.  Créez un service Kubernetes pour votre cluster qui transmettra les demandes entrantes au noeud final externe que vous allez créer.
    1.  Ouvrez l'éditeur de votre choix et créez un fichier de configuration de service nommé, par exemple, `myexternalservice.yaml`.
    2.  Définissez un service pour l'application qu'exposera l'équilibreur de charge ALB.

        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: myexternalservice
        spec:
          ports:
           - protocol: TCP
             port: 8080
        ```
        {: codeblock}

        <table>
        <caption>Description des composants du fichier du service ALB</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants du fichier YAML</th>
        </thead>
        <tbody>
        <tr>
        <td><code>metadata/name</code></td>
        <td>Remplacez <em>&lt;myexternalservice&gt;</em> par le nom de votre service.<p>Découvrez comment [sécuriser vos informations personnelles](cs_secure.html#pi) lorsque vous utilisez des ressources Kubernetes.</p></td>
        </tr>
        <tr>
        <td><code>port</code></td>
        <td>Port sur lequel le service est à l'écoute.</td>
        </tr></tbody></table>

    3.  Sauvegardez vos modifications.
    4.  Créez le service Kubernetes pour votre cluster.

        ```
        kubectl apply -f myexternalservice.yaml
        ```
        {: pre}
2.  Configurez un noeud final Kubernetes définissant l'emplacement externe de l'application que vous désirez inclure dans l'équilibrage de charge du cluster.
    1.  Ouvrez l'éditeur de votre choix et créez un fichier de configuration de noeud final nommé, par exemple, `myexternalendpoint.yaml`.
    2.  Définissez votre noeud final externe. Incluez toutes les adresses IP publiques et les ports pouvant être utilisés pour accéder à votre application externe.

        ```
        kind: Endpoints
        apiVersion: v1
        metadata:
          name: myexternalendpoint
        subsets:
          - addresses:
              - ip: <external_IP1>
              - ip: <external_IP2>
            ports:
              - port: <external_port>
        ```
        {: codeblock}

        <table>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants du fichier YAML</th>
        </thead>
        <tbody>
        <tr>
        <td><code>name</code></td>
        <td>Remplacez <em>&lt;myexternalendpoint&gt;</em> par le nom du service Kubernetes que vous avez créé plus tôt.</td>
        </tr>
        <tr>
        <td><code>ip</code></td>
        <td>Remplacez <em>&lt;external_IP&gt;</em> par les adresses IP publiques permettant de se connecter à votre application externe.</td>
         </tr>
         <td><code>port</code></td>
         <td>Remplacez <em>&lt;external_port&gt;</em> par le port sur lequel votre application externe est à l'écoute.</td>
         </tbody></table>

    3.  Sauvegardez vos modifications.
    4.  Créez le noeud final Kubernetes pour votre cluster.

        ```
        kubectl apply -f myexternalendpoint.yaml
        ```
        {: pre}

### Etape 2 : Sélectionnez un domaine d'application et une terminaison TLS
{: #public_outside_2}

Lorsque vous configurez l'équilibreur de charge d'application public, vous choisissez le domaine sur lequel vos applications seront accessibles et indiquez s'il faut utiliser une terminaison TLS.
{: shortdesc}

<dl>
<dt>Domaine</dt>
<dd>Vous pouvez utiliser le domaine fourni par IBM, par exemple <code>mycluster-12345.us-south.containers.appdomain.cloud/myapp</code>, pour accéder à votre application sur Internet. Pour utiliser à la place un domaine personnalisé, vous pouvez mapper votre domaine personnalisé au domaine fourni par IBM ou à l'adresse IP publique de l'équilibreur de charge ALB.</dd>
<dt>Terminaison TLS</dt>
<dd>L'équilibreur de charge ALB équilibre la charge du trafic réseau HTTP vers les applications de votre cluster. Pour équilibrer la charge des connexions HTTPS entrantes, vous pouvez configurer l'équilibreur de charge ALB pour déchiffrer le trafic réseau et transférer la demande déchiffrée aux applications exposées dans votre cluster. Si vous utilisez le sous-domaine Ingress fourni par IBM, vous pouvez utiliser le certificat TLS fourni par IBM. Actuellement, TLS n'est pas pris en charge pour les sous-domaines génériques fournis par IBM. Si vous utilisez un domaine personnalisé, vous pouvez utiliser votre propre certificat TLS pour gérer la terminaison TLS.</dd>
</dl>

Pour utiliser le domaine Ingress fourni par IBM :
1. Obtenez les détails de votre cluster. Remplacez _&lt;cluster_name_or_ID&gt;_ par le nom du cluster sur lequel sont déployées les applications que vos souhaitez exposer.

    ```
    bx cs cluster-get <cluster_name_or_ID>
    ```
    {: pre}

    Exemple de sortie :

    ```
    Name:                   mycluster
    ID:                     18a61a63c6a94b658596ca93d087aad9
    State:                  normal
    Created:                2018-01-12T18:33:35+0000
    Location:               dal10
    Master URL:             https://169.xx.xxx.xxx:26268
    Ingress Subdomain:      mycluster-12345.us-south.containers.appdomain.cloud
    Ingress Secret:         <tls_secret>
    Workers:                3
    Version:                1.9.7
    Owner Email:            owner@email.com
    Monitoring Dashboard:   <dashboard_URL>
    ```
    {: screen}
2. Obtenez le domaine fourni par IBM dans la zone **Ingress Subdomain**. Si vous souhaitez utiliser TLS, récupérez également la valeur confidentielle TLS (tls_secret) fournie par IBM dans la zone **Ingress Secret**. **Remarque **: si vous utilisez un sous-domaine générique, TLS n'est pas pris en charge.

Pour utiliser un domaine personnalisé :
1.    Créez un domaine personnalisé. Pour enregistrer votre domaine personnalisé, travaillez en collaboration avec votre fournisseur DNS (Domain Name Service) ou [{{site.data.keyword.Bluemix_notm}} DNS](/docs/infrastructure/dns/getting-started.html#getting-started-with-dns).
      * Si les applications que vous souhaitez exposer via Ingress se trouvent dans différents espaces de nom sur un cluster, enregistrez le domaine personnalisé en tant que domaine générique, par exemple `*.custom_domain.net`.

2.  Configurez votre domaine pour acheminer le trafic réseau entrant à l'équilibreur de charge ALB fourni par IBM. Sélectionnez l'une des options suivantes :
    -   Définir un alias pour votre domaine personnalisé en spécifiant le domaine fourni par IBM sous forme d'enregistrement de nom canonique (CNAME). Pour identifier le domaine Ingress fourni par IBM, exécutez `bx cs cluster-get <cluster_name>` et recherchez la zone **Ingress subdomain**.
    -   Mapper votre domaine personnalisé à l'adresse IP publique portable de l'équilibreur de charge ALB fourni par IBM en ajoutant l'adresse IP en tant qu'enregistrement. Pour identifier l'adresse IP publique portable de l'équilibreur de charge ALB, exécutez la commande `bx cs alb-get <public_alb_ID>`.
3.   Facultatif : si vous souhaitez utiliser TLS, importez ou créez un certificat TLS et une valeur confidentielle de clé. Si vous utilisez un domaine générique, veillez à importer ou créer un certificat générique.
      * Si un certificat TLS que vous désirez utiliser est stocké dans {{site.data.keyword.cloudcerts_long_notm}}, vous pouvez importer sa valeur confidentielle associée dans votre cluster en exécutant la commande suivante :
        ```
        bx cs alb-cert-deploy --secret-name <secret_name> --cluster <cluster_name_or_ID> --cert-crn <certificate_crn>
        ```
        {: pre}
      * Si vous ne disposez pas de certificat TLS, procédez comme suit :
        1. Créez un certificat TLS et une clé pour votre domaine codés au format PEM.
        2. Créez une valeur confidentielle qui utilise le certificat et la clé TLS. Remplacez <em>&lt;tls_secret_name&gt;</em> par le nom de votre valeur confidentielle Kubernetes, <em>&lt;tls_key_filepath&gt;</em> par le chemin de votre fichier de clés TLS et <em>&lt;tls_cert_filepath&gt;</em> par le chemin d'accès à votre fichier de certificat TLS personnalisé.
          ```
          kubectl create secret tls <tls_secret_name> --key <tls_key_filepath> --cert <tls_cert_filepath>
          ```
          {: pre}


### Etape 3 : Créez la ressource Ingress
{: #public_outside_3}

Les ressources Ingress définissent les règles de routage utilisées par l'équilibreur de charge d'application (ALB) pour acheminer le trafic vers votre service d'application.
{: shortdesc}

**Remarque :** si vous exposez plusieurs applications externes, et que les services que vous avez créés pour les applications à l'[étape 1](#public_outside_1) se trouvent dans différents espaces de nom, au moins une ressource Ingress est requise par espace de nom. En revanche, chaque espace de nom doit utiliser un hôte différent. Vous devez enregistrer un domaine générique et indiquer un sous-domaine différent dans chaque ressource. Pour plus d'informations, voir [Planification réseau pour un ou plusieurs espaces de nom](#multiple_namespaces).

1. Ouvrez l'éditeur de votre choix et créez un fichier de configuration Ingress nommé, par exemple, `myexternalingress.yaml`.

2. Définissez dans votre fichier de configuration une ressource Ingress utilisant le domaine fourni par IBM ou votre domaine personnalisé pour acheminer le trafic réseau entrant aux services que vous avez créés auparavant.

    Exemple de fichier YAML n'utilisant pas TLS :
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myexternalingress
    spec:
      rules:
      - host: <domain>
        http:
          paths:
          - path: /<external_app1_path>
            backend:
              serviceName: <app1_service>
              servicePort: 80
          - path: /<external_app2_path>
            backend:
              serviceName: <app2_service>
              servicePort: 80
    ```
    {: codeblock}

    Exemple de fichier YAML utilisant TLS :
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myexternalingress
    spec:
      tls:
      - hosts:
        - <domain>
        secretName: <tls_secret_name>
      rules:
      - host: <domain>
        http:
          paths:
          - path: /<external_app1_path>
            backend:
              serviceName: <app1_service>
              servicePort: 80
          - path: /<external_app2_path>
            backend:
              serviceName: <app2_service>
              servicePort: 80
    ```
    {: codeblock}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants du fichier YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code>tls/hosts</code></td>
    <td>Pour utiliser TLS, remplacez <em>&lt;domain&gt;</em> par le sous-domaine Ingress fourni par IBM ou par votre domaine personnalisé.

    </br></br>
    <strong>Remarque :</strong><ul><li>Si vos services d'application se trouvent dans différents espaces de nom dans le cluster, ajoutez un sous-domaine générique au début du domaine, par exemple : `subdomain1.custom_domain.net` ou `subdomain1.mycluster.us-south.containers.appdomain.cloud`. Utilisez un sous-domaine unique pour chaque ressource que vous créez dans le cluster.</li><li>N'utilisez pas &ast; pour votre hôte ou laissez la propriété host vide pour éviter des erreurs lors de la création de la ressource Ingress.</li></ul></td>
    </tr>
    <tr>
    <td><code>tls/secretName</code></td>
    <td><ul><li>Si vous utilisez le domaine Ingress fourni par IBM, remplacez le nom de la valeur confidentielle TLS (<em>&lt;tls_secret_name&gt;</em>) par le nom de la valeur confidentielle Ingress (Ingress secret) fournie par IBM.</li><li>Si vous utilisez un domaine personnalisé, remplacez <em>&lt;tls_secret_name&gt;</em> par la valeur confidentielle que vous avez créée auparavant et qui contient votre certificat et votre clé TLS personnalisés. Si vous avez importé un certificat depuis {{site.data.keyword.cloudcerts_short}}, vous pouvez exécuter la commande <code>bx cs alb-cert-get --cluster <cluster_name_or_ID> --cert-crn <certificate_crn></code> pour examiner les valeurs confidentielles qui sont associées à un certificat TLS.</li><ul><td>
    </tr>
    <tr>
    <td><code>rules/host</code></td>
    <td>Remplacez <em>&lt;domain&gt;</em> par le sous-domaine Ingress fourni par IBM ou par votre domaine personnalisé.

    </br></br>
    <strong>Remarque :</strong><ul><li>Si vos applications sont exposées par des services dans différents espaces de nom, ajoutez un sous-domaine générique au début du domaine, par exemple `subdomain1.custom_domain.net` ou `subdomain1.mycluster.us-south.containers.appdomain.cloud`. Utilisez un sous-domaine unique pour chaque ressource que vous créez dans le cluster.</li><li>N'utilisez pas &ast; pour votre hôte ou laissez la propriété host vide pour éviter des erreurs lors de la création de la ressource Ingress.</li></ul></td>
    </tr>
    <tr>
    <td><code>path</code></td>
    <td>Remplacez <em>&lt;external_app_path&gt;</em> par une barre oblique ou par le chemin sur lequel votre application est à l'écoute. Le chemin est ajouté au domaine fourni par IBM ou à votre domaine personnalisé de manière à créer un chemin unique vers votre application. Lorsque vous indiquez cette route dans un navigateur Web, le trafic réseau est acheminé vers l'équilibreur de charge ALB. L'équilibreur de charge ALB recherche le service associé et lui envoie le trafic réseau. Le service achemine ensuite le trafic vers l'application externe. L'application doit être configurée pour être à l'écoute sur ce chemin afin de recevoir le trafic réseau entrant.
    </br></br>
    De nombreuses applications ne sont pas à l'écoute sur un chemin spécifique mais utilisent le chemin racine et un port spécifique. Dans ce cas, définissez le chemin racine sous la forme <code>/</code>, sans spécifier de chemin individuel pour votre application. Exemples : <ul><li>Pour <code>http://domain/</code>, entrez <code>/</code> comme chemin.</li><li>Pour <code>http://domain/app1_path</code>, entrez <code>/app1_path</code> comme chemin.</li></ul>
    </br></br>
    <strong>Astuce :</strong> pour configurer Ingress pour qu'il soit à l'écoute sur un chemin différent de celui où votre application est à l'écoute, vous pouvez utiliser l'[annotation rewrite](cs_annotations.html#rewrite-path).</td>
    </tr>
    <tr>
    <td><code>serviceName</code></td>
    <td>Remplacez <em>&lt;app1_service&gt;</em> et <em>&lt;app2_service&gt;</em>, par le nom des services que vous avez créés pour exposer vos applications externes. Si vos applications sont exposées par des services dans différents espaces de nom dans le cluster, incluez uniquement les services d'application figurant dans le même espace de nom. Vous devez créer une ressource Ingress pour chaque espace de nom contenant des applications que vous souhaitez exposer.</td>
    </tr>
    <tr>
    <td><code>servicePort</code></td>
    <td>Port sur lequel votre service est à l'écoute. Utilisez le même port que celui que vous avez défini lors de la création du service Kubernetes pour votre application.</td>
    </tr>
    </tbody></table>

3.  Créez la ressource Ingress pour votre cluster. Veillez à ce que la ressource se déploie dans le même espace de nom que les services d'application que vous avez spécifiés dans la ressource.

    ```
    kubectl apply -f myexternalingress.yaml -n <namespace>
    ```
    {: pre}
4.   Vérifiez que la création de la ressource Ingress a abouti.

      ```
      kubectl describe ingress myingressresource
      ```
      {: pre}

      1. Si les messages compris dans les événements indiquent une erreur dans la configuration de votre ressource, modifiez les valeurs du fichier de votre ressource et réappliquez le fichier correspondant à la ressource.


Votre ressource Ingress est créée dans le même espace de nom que vos services d'application. Vos applications figurant dans cet espace de nom sont enregistrées avec l'équilibreur de charge ALB Ingress du cluster.

### Etape 4 : Accédez à votre application sur Internet
{: #public_outside_4}

Dans un navigateur Web, entrez l'URL du service d'application auquel accéder.

```
https://<domain>/<app1_path>
```
{: pre}

Si vous exposez plusieurs applications, accédez à ces applications en modifiant le chemin ajouté à l'URL.

```
https://<domain>/<app2_path>
```
{: pre}

Si vous utilisez un domaine générique pour exposer des applications dans différents espaces de nom, accédez à ces applications avec leurs sous-domaines respectifs.

```
http://<subdomain1>.<domain>/<app1_path>
```
{: pre}

```
http://<subdomain2>.<domain>/<app1_path>
```
{: pre}


<br />


## Activation de l'équilibreur de charge d'application (ALB) privé par défaut
{: #private_ingress}

Lorsque vous créez un cluster standard, un équilibreur de charge d'application (ALB) privé fourni par IBM est créé et une adresse IP privée portable et un chemin privé lui sont affectés. Toutefois, l'équilibreur de charge ALB privé par défaut n'est pas automatiquement activé. Pour utiliser l'équilibreur de charge ALB privé pour équilibrer la charge du trafic réseau privé vers vos applications, vous devez l'activer avec l'adresse IP privée portable fournie par IBM, ou votre propre adresse IP privée portable.
{:shortdesc}

**Remarque** : si vous avez utilisé l'indicateur `--no-subnet` lors de la création du cluster, vous devez ajouter un sous-réseau privé portable ou un sous-réseau géré par l'utilisateur avant de pouvoir activer l'équilibreur de charge ALB privé. Pour plus d'informations, voir [Demande de sous-réseaux supplémentaires pour votre cluster](cs_subnets.html#request).

Avant de commencer :

-   Si ce n'est déjà fait, [créez un cluster standard](cs_clusters.html#clusters_ui).
-   [Ciblez votre interface de ligne de commande](cs_cli_install.html#cs_cli_configure) vers votre cluster.

Pour utiliser l'équilibreur de charge ALB privé avec l'adresse IP privée portable pré-affectée fournie par IBM :

1. Répertoriez les équilibreurs de charge ALB disponibles dans votre cluster pour identifier l'ID de l'ALB privé. Remplacez <em>&lt;cluster_name&gt;</em> par le nom du cluster sur lequel l'application que vous voulez exposer est déployée.

    ```
    bx cs albs --cluster <cluster_name>
    ```
    {: pre}

    La zone **Status** pour l'ALB privé indique _disabled_.
    ```
    ALB ID                                            Enabled   Status     Type      ALB IP
    private-cr6d779503319d419ea3b4ab171d12c3b8-alb1   false     disabled   private   -
    public-cr6d779503319d419ea3b4ab171d12c3b8-alb1    true      enabled    public    169.xx.xxx.xxx
    ```
    {: screen}

2. Activez l'équilibreur de charge ALB privé. Remplacez <em>&lt;private_ALB_ID&gt;</em> par l'ID de l'ALB privé indiqué dans la sortie de l'étape précédente.

   ```
   bx cs alb-configure --albID <private_ALB_ID> --enable
   ```
   {: pre}

<br>
Pour activer l'équilibreur de charge ALB privé en utilisant votre propre adresse IP privée portable :

1. Configurez le sous-réseau géré par l'utilisateur de l'adresse IP que vous avez choisie pour acheminer le trafic sur le VLAN privé de votre cluster.

   ```
   bx cs cluster-user-subnet-add <cluster_name> <subnet_CIDR> <private_VLAN_ID>
   ```
   {: pre}

   <table>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composantes de la commande</th>
   </thead>
   <tbody>
   <tr>
   <td><code>&lt;cluster_name&gt;</code></td>
   <td>Nom ou ID du cluster dans lequel l'application que vous voulez exposer est déployée.</td>
   </tr>
   <tr>
   <td><code>&lt;subnet_CIDR&gt;</code></td>
   <td>CIDR de votre sous-réseau géré par l'utilisateur.</td>
   </tr>
   <tr>
   <td><code>&lt;private_VLAN_ID&gt;</code></td>
   <td>ID d'un VLAN privé disponible. Pour obtenir l'ID d'un VLAN privé disponible, exécutez la commande `bx cs vlans`.</td>
   </tr>
   </tbody></table>

2. Répertoriez les équilibreurs de charge ALB disponibles dans votre cluster pour identifier l'ID de l'ALB privé.

    ```
    bx cs albs --cluster <cluster_name>
    ```
    {: pre}

    La zone **Status** pour l'ALB privé indique _disabled_.
    ```
    ALB ID                                            Enabled   Status     Type      ALB IP
    private-cr6d779503319d419ea3b4ab171d12c3b8-alb1   false     disabled   private   -
    public-cr6d779503319d419ea3b4ab171d12c3b8-alb1    true      enabled    public    169.xx.xxx.xxx
    ```
    {: screen}

3. Activez l'équilibreur de charge ALB privé. Remplacez <em>&lt;private_ALB_ID&gt;</em> par l'ID de l'équilibreur de charge ALB privé indiqué dans la sortie de l'étape précédente et <em>&lt;user_IP&gt;</em> par l'adresse IP du sous-réseau géré par l'utilisateur que vous désirez utiliser.

   ```
   bx cs alb-configure --albID <private_ALB_ID> --enable --user-ip <user_IP>
   ```
   {: pre}

<br />


## Exposition d'applications sur un réseau privé
{: #ingress_expose_private}

Exposez des applications sur un réseau privé en utilisant l'équilibreur de charge ALB Ingress privé.
{:shortdesc}

Avant de commencer :
* Consultez les [Conditions prérequises](#config_prereqs) d'Ingress.
* [Activez l'équilibreur de charge d'application privé](#private_ingress).
* Si vous disposez de noeuds worker privés et que vous souhaitez utiliser un fournisseur de DNS externe, vous devez [configurer des noeuds de périphérie avec accès public](cs_edge.html#edge) et [configurer un dispositif de routeur virtuel ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/blogs/bluemix/2017/07/kubernetes-and-bluemix-container-based-workloads-part4/). 
* Si vous disposez de noeuds worker privés et que vous souhaitez rester uniquement sur un réseau privé, vous devez [configurer un service DNS sur site privé ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/) pour résoudre les demandes d'URL vers vos applications.

### Etape 1 : Déployez des applications et créez des services d'application
{: #private_1}

Commencez par déployer vos applications et créer des services Kubernetes pour les exposer.
{: shortdesc}

1.  [Déployez votre application sur le cluster](cs_app.html#app_cli). Prenez soin d'ajouter un libellé à votre déploiement dans la section "metadata" de votre fichier de configuration, par exemple `app: code`. Ce libellé est nécessaire pour identifier tous les pods où s'exécute votre application afin de pouvoir inclure ces pods dans l'équilibrage de charge Ingress.

2.   Créez un service Kubernetes pour chaque application que vous désirez exposer. Votre application doit être exposée par un service Kubernetes pour que l'équilibreur de charge ALB du cluster puisse l'inclure dans l'équilibrage de charge Ingress.
      1.  Ouvrez l'éditeur de votre choix et créez un fichier de configuration de service nommé, par exemple, `myapp_service.yaml`.
      2.  Définissez un service pour l'application qu'exposera l'équilibreur de charge ALB.

          ```
          apiVersion: v1
          kind: Service
          metadata:
            name: myapp_service
          spec:
            selector:
              <selector_key>: <selector_value>
            ports:
             - protocol: TCP
               port: 8080
          ```
          {: codeblock}

          <table>
          <thead>
          <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants du fichier YAML du service ALB</th>
          </thead>
          <tbody>
          <tr>
          <td><code>selector</code></td>
          <td>Entrez la paire clé de libellé (<em>&lt;selector_key&gt;</em>) et valeur (<em>&lt;selector_value&gt;</em>) que vous désirez utiliser pour cibler les pods dans lesquels s'exécute votre application. Pour cibler vos pods et les inclure dans l'équilibrage de charge du service, assurez-vous que <em>&lt;selector_key&gt;</em> et <em>&lt;selector_value&gt;</em> sont identiques à la paire clé/valeur dans la section <code>spec.template.metadata.labels</code> du fichier YALM de déploiement.</td>
           </tr>
           <tr>
           <td><code>port</code></td>
           <td>Port sur lequel le service est à l'écoute.</td>
           </tr>
           </tbody></table>
      3.  Sauvegardez vos modifications.
      4.  Créez le service dans votre cluster. Si les applications sont déployées dans plusieurs espaces de nom dans votre cluster, vérifiez que le service se déploie dans le même espace de nom que l'application que vous souhaitez exposer.

          ```
          kubectl apply -f myapp_service.yaml [-n <namespace>]
          ```
          {: pre}
      5.  Répétez ces étapes pour chaque application que vous désirez exposer.


### Etape 2 : Mappez votre domaine personnalisé et sélectionnez une terminaison TLS
{: #private_2}

Lorsque vous configurez l'équilibreur de charge d'application (ALB) privé, vous utilisez un domaine personnalisé sur lequel vos applications seront accessibles et déterminez s'il faut utiliser une terminaison TLS.
{: shortdesc}

L'équilibreur de charge ALB équilibre la charge du trafic réseau HTTP vers vos applications. Pour équilibrer également la charge des connexions HTTPS entrantes, vous pouvez configurer l'équilibreur de charge ALB afin d'utiliser votre propre certificat TLS pour déchiffrer le trafic réseau. L'équilibreur de charge ALB transfère ensuite la demande déchiffrée aux applications exposées dans votre cluster.
1.   Créez un domaine personnalisé. Pour enregistrer votre domaine personnalisé, travaillez en collaboration avec votre fournisseur DNS (Domain Name Service) ou [{{site.data.keyword.Bluemix_notm}} DNS](/docs/infrastructure/dns/getting-started.html#getting-started-with-dns).
      * Si les applications que vous souhaitez exposer via Ingress se trouvent dans différents espaces de nom sur un cluster, enregistrez le domaine personnalisé en tant que domaine générique, par exemple `*.custom_domain.net`.

2. Mappez votre domaine personnalisé à l'adresse IP privée portable de l'équilibreur de charge ALB privé fourni par IBM en ajoutant l'adresse IP en tant qu'enregistrement. Pour identifier l'adresse IP privée portable de l'équilibreur de charge ALB privé, exécutez la commande `bx cs albs --cluster <cluster_name>`.
3.   Facultatif : si vous souhaitez utiliser TLS, importez ou créez un certificat TLS et une valeur confidentielle de clé. Si vous utilisez un domaine générique, veillez à importer ou créer un certificat générique.
      * Si un certificat TLS que vous désirez utiliser est stocké dans {{site.data.keyword.cloudcerts_long_notm}}, vous pouvez importer sa valeur confidentielle associée dans votre cluster en exécutant la commande suivante :
        ```
        bx cs alb-cert-deploy --secret-name <secret_name> --cluster <cluster_name_or_ID> --cert-crn <certificate_crn>
        ```
        {: pre}
      * Si vous ne disposez pas de certificat TLS, procédez comme suit :
        1. Créez un certificat TLS et une clé pour votre domaine codés au format PEM.
        2. Créez une valeur confidentielle qui utilise le certificat et la clé TLS. Remplacez <em>&lt;tls_secret_name&gt;</em> par le nom de votre valeur confidentielle Kubernetes, <em>&lt;tls_key_filepath&gt;</em> par le chemin de votre fichier de clés TLS et <em>&lt;tls_cert_filepath&gt;</em> par le chemin d'accès à votre fichier de certificat TLS personnalisé.
          ```
          kubectl create secret tls <tls_secret_name> --key <tls_key_filepath> --cert <tls_cert_filepath>
          ```
          {: pre}


### Etape 3 : Créez la ressource Ingress
{: #pivate_3}

Les ressources Ingress définissent les règles de routage utilisées par l'équilibreur de charge d'application (ALB) pour acheminer le trafic vers votre service d'application.
{: shortdesc}

**Remarque :** si votre cluster dispose de plusieurs espaces de nom dans lesquels sont exposées les applications, au moins une ressource Ingress est requise par espace de nom. En revanche, chaque espace de nom doit utiliser un hôte différent. Vous devez enregistrer un domaine générique et indiquer un sous-domaine différent dans chaque ressource. Pour plus d'informations, voir [Planification réseau pour un ou plusieurs espaces de nom](#multiple_namespaces).

1. Ouvrez l'éditeur de votre choix et créez un fichier de configuration Ingress nommé, par exemple, `myingressresource.yaml`.

2.  Définissez dans votre fichier de configuration une ressource Ingress utilisant votre domaine personnalisé pour acheminer le trafic réseau entrant aux services que vous avez créés auparavant.

    Exemple de fichier YAML n'utilisant pas TLS :
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingressresource
      annotations:
        ingress.bluemix.net/ALB-ID: "<private_ALB_ID>"
    spec:
      rules:
      - host: <domain>
        http:
          paths:
          - path: /<app1_path>
            backend:
              serviceName: <app1_service>
              servicePort: 80
          - path: /<app2_path>
            backend:
              serviceName: <app2_service>
              servicePort: 80
    ```
    {: codeblock}

    Exemple de fichier YAML utilisant TLS :
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingressresource
      annotations:
        ingress.bluemix.net/ALB-ID: "<private_ALB_ID>"
    spec:
      tls:
      - hosts:
        - <domain>
        secretName: <tls_secret_name>
      rules:
      - host: <domain>
        http:
          paths:
          - path: /<app1_path>
            backend:
              serviceName: <app1_service>
              servicePort: 80
          - path: /<app2_path>
            backend:
              serviceName: <app2_service>
              servicePort: 80
    ```
    {: codeblock}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants du fichier YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code>ingress.bluemix.net/ALB-ID</code></td>
    <td>Remplacez <em>&lt;private_ALB_ID&gt;</em> par l'ID de votre équilibreur de charge ALB privé. Exécutez la commande <code>bx cs albs --cluster <my_cluster></code> pour trouver l'ID de l'ALB. Pour plus d'informations sur cette annotation Ingress, voir [Routage de l'équilibreur de charge d'application privé](cs_annotations.html#alb-id).</td>
    </tr>
    <tr>
    <td><code>tls/hosts</code></td>
    <td>Pour utiliser TLS, remplacez <em>&lt;domain&gt;</em> par votre domaine personnalisé.

    </br></br>
    <strong>Remarque :</strong><ul><li>Si vos applications sont exposées par des services dans différents espaces de nom dans un cluster, ajoutez un sous-domaine générique au début du domaine, par exemple `subdomain1.custom_domain.net`. Utilisez un sous-domaine unique pour chaque ressource que vous créez dans le cluster.</li><li>N'utilisez pas &ast; pour votre hôte ou laissez la propriété host vide pour éviter des erreurs lors de la création de la ressource Ingress.</li></ul></td>
    </tr>
    <tr>
    <td><code>tls/secretName</code></td>
    <td>Remplacez <em>&lt;tls_secret_name&gt;</em> par le nom de la valeur confidentielle que vous avez créée auparavant et qui contient votre certificat et votre clé TLS personnalisés. Si vous avez importé un certificat depuis {{site.data.keyword.cloudcerts_short}}, vous pouvez exécuter la commande <code>bx cs alb-cert-get --cluster <cluster_name_or_ID> --cert-crn <certificate_crn></code> pour examiner les valeurs confidentielles qui sont associées à un certificat TLS.
    </tr>
    <tr>
    <td><code>host</code></td>
    <td>Remplacez <em>&lt;domain&gt;</em> par votre domaine personnalisé.
    </br></br>
    <strong>Remarque :</strong><ul><li>Si vos applications sont exposées par des services dans différents espaces de nom dans un cluster, ajoutez un sous-domaine générique au début du domaine, par exemple `subdomain1.custom_domain.net`. Utilisez un sous-domaine unique pour chaque ressource que vous créez dans le cluster.</li><li>N'utilisez pas &ast; pour votre hôte ou laissez la propriété host vide pour éviter des erreurs lors de la création de la ressource Ingress.</li></ul></td>
    </td>
    </tr>
    <tr>
    <td><code>path</code></td>
    <td>Remplacez <em>&lt;app_path&gt;</em> par une barre oblique ou par le chemin sur lequel votre application est à l'écoute. Le chemin est ajouté à votre domaine personnalisé de manière à créer une route unique vers votre application. Lorsque vous indiquez cette route dans un navigateur Web, le trafic réseau est acheminé vers l'équilibreur de charge ALB. L'équilibreur de charge ALB recherche le service associé et lui envoie le trafic réseau. Le service transfère ensuite le trafic aux pods sur lesquels s'exécute l'application.
    </br></br>
    De nombreuses applications ne sont pas à l'écoute sur un chemin spécifique mais utilisent le chemin racine et un port spécifique. Dans ce cas, définissez le chemin racine sous la forme <code>/</code>, sans spécifier de chemin individuel pour votre application. Exemples : <ul><li>Pour <code>http://domain/</code>, entrez <code>/</code> comme chemin.</li><li>Pour <code>http://domain/app1_path</code>, entrez <code>/app1_path</code> comme chemin.</li></ul>
    </br>
    <strong>Astuce :</strong> pour configurer Ingress pour qu'il soit à l'écoute sur un chemin différent de celui où votre application est à l'écoute, vous pouvez utiliser l'[annotation rewrite](cs_annotations.html#rewrite-path).</td>
    </tr>
    <tr>
    <td><code>serviceName</code></td>
    <td>Remplacez <em>&lt;app1_service&gt;</em> et <em>&lt;app2_service&gt;</em>, et ainsi de suite, par le nom des services que vous avez créés pour exposer vos applications. Si vos applications sont exposées par des services dans différents espaces de nom dans le cluster, incluez uniquement les services d'application figurant dans le même espace de nom. Vous devez créer une ressource Ingress pour chaque espace de nom contenant des applications que vous souhaitez exposer.</td>
    </tr>
    <tr>
    <td><code>servicePort</code></td>
    <td>Port sur lequel votre service est à l'écoute. Utilisez le même port que celui que vous avez défini lors de la création du service Kubernetes pour votre application.</td>
    </tr>
    </tbody></table>

3.  Créez la ressource Ingress pour votre cluster. Veillez à ce que la ressource se déploie dans le même espace de nom que les services d'application que vous avez spécifiés dans la ressource.

    ```
    kubectl apply -f myingressresource.yaml -n <namespace>
    ```
    {: pre}
4.   Vérifiez que la création de la ressource Ingress a abouti.

      ```
      kubectl describe ingress myingressresource
      ```
      {: pre}

      1. Si les messages compris dans les événements indiquent une erreur dans la configuration de votre ressource, modifiez les valeurs du fichier de votre ressource et réappliquez le fichier correspondant à la ressource.


Votre ressource Ingress est créée dans le même espace de nom que vos services d'application. Vos applications figurant dans cet espace de nom sont enregistrées avec l'équilibreur de charge ALB Ingress du cluster.

### Etape 4 : Accédez à votre application depuis votre réseau privé
{: #private_4}

A l'intérieur de votre pare-feu de réseau privé, entrez l'URL du service d'application dans un navigateur Web.

```
https://<domain>/<app1_path>
```
{: pre}

Si vous exposez plusieurs applications, accédez à ces applications en modifiant le chemin ajouté à l'URL.

```
https://<domain>/<app2_path>
```
{: pre}

Si vous utilisez un domaine générique pour exposer des applications dans différents espaces de nom, accédez à ces applications avec leurs sous-domaines respectifs.

```
http://<subdomain1>.<domain>/<app1_path>
```
{: pre}

```
http://<subdomain2>.<domain>/<app1_path>
```
{: pre}


Pour obtenir un tutoriel complet sur la sécurisation d'une communication de microservice à microservice entre vos clusters en utilisant l'équilibreur de charge ALB avec TLS, consultez [cet article de blogue ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://medium.com/ibm-cloud/secure-microservice-to-microservice-communication-across-kubernetes-clusters-using-a-private-ecbe2a8d4fe2).

<br />


## Configurations facultatives de l'équilibreur de charge d'application
{: #configure_alb}

Vous pouvez poursuivre la configuration d'un équilibreur de charge d'application à l'aide des options suivantes.

-   [Ouverture de ports dans l'équilibreur de charge d'application Ingress](#opening_ingress_ports)
-   [Configuration de protocoles et de chiffrements SSL au niveau HTTP](#ssl_protocols_ciphers)
-   [Personnalisation du format des journaux Ingress](#ingress_log_format)
-   [Personnalisation de votre équilibreur de charge d'application à l'aide d'annotations](cs_annotations.html)
{: #ingress_annotation}


### Ouverture de ports dans l'équilibreur de charge d'application Ingress
{: #opening_ingress_ports}

Par défaut, seuls les ports 80 et 443 sont exposés dans l'équilibreur de charge ALB Ingress. Pour exposer d'autres ports, vous pouvez éditer la ressource configmap `ibm-cloud-provider-ingress-cm`.
{:shortdesc}

1. Créez et ouvrez une version locale du fichier de configuration pour la ressource configmap `ibm-cloud-provider-ingress-cm`.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. Ajoutez une section <code>data</code> et spécifiez les ports publics `80`, `443`, ainsi que tous les autres ports que vous voulez exposer, séparés par un point virgule (;).

    **Important** : par défaut, les ports 80 et 443 sont ouverts. Pour conserver les ports 80 et 443 ouverts, vous devez également les inclure en plus des autres ports que vous indiquez dans la zone `public-ports`. Tout port non spécifié est fermé. Si vous avez activé un équilibreur de charge ALB privé, vous devez également spécifier tous les ports que vous souhaitez garder ouverts dans la zone `private-ports`.

    ```
    apiVersion: v1
    data:
      public-ports: "80;443;<port3>"
      private-ports: "80;443;<port4>"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: codeblock}

    Exemple pour conserver les ports `80`, `443` et `9443` ouverts :
    ```
    apiVersion: v1
 data:
   public-ports: "80;443;9443"
 kind: ConfigMap
 metadata:
   name: ibm-cloud-provider-ingress-cm
   namespace: kube-system
    ```
    {: screen}

3. Sauvegardez le fichier de configuration.

4. Vérifiez que les modifications apportées à configmap ont été appliquées.

 ```
 kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
 ```
 {: pre}

 Sortie :
 ```
 Name:        ibm-cloud-provider-ingress-cm
 Namespace:    kube-system
 Labels:        <none>
 Annotations:    <none>

 Data
 ====

  public-ports: "80;443;9443"
 ```
 {: screen}

Pour plus d'informations sur les ressources configmap, voir la [documentation Kubernetes](https://kubernetes-v1-4.github.io/docs/user-guide/configmap/).

### Configuration de protocoles et de chiffrements SSL au niveau HTTP
{: #ssl_protocols_ciphers}

Activez les protocoles et chiffrements SSL au niveau HTTP global en éditant la ressource configmap `ibm-cloud-provider-ingress-cm`.
{:shortdesc}

Par défaut, le protocole TLS 1.2 est utilisé pour toutes les configurations Ingress utilisant le domaine fourni par IBM. Vous devez remplacer la valeur par défaut pour utiliser à la place les protocoles TLS 1.1 ou 1.0, comme suit :

**Remarque** : lorsque vous indiquez les protocoles activés pour tous les hôtes, les paramètres TLSv1.1 et TLSv1.2 (1.1.13, 1.0.12) fonctionnent uniquement avec l'utilisation d'OpenSSL 1.0.1 ou version supérieure. Le paramètre TLSv1.3 (1.13.0) fonctionne uniquement avec l'utilisation d'OpenSSL 1.1.1 généré avec le support TLSv1.3.

Pour éditer le fichier configmap pour activer les chiffrements et protocoles SSL :

1. Créez et ouvrez une version locale du fichier de configuration pour la ressource configmap `ibm-cloud-provider-ingress-cm`.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. Ajoutez les chiffrements et protocoles SSL. Configurez les chiffrements en fonction du [format de liste de chiffrements de la bibliothèque OpenSSL ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.openssl.org/docs/man1.0.2/apps/ciphers.html).

   ```
   apiVersion: v1
 data:
   ssl-protocols: "TLSv1 TLSv1.1 TLSv1.2"
   ssl-ciphers: "HIGH:!aNULL:!MD5"
 kind: ConfigMap
 metadata:
   name: ibm-cloud-provider-ingress-cm
   namespace: kube-system
   ```
   {: codeblock}

3. Sauvegardez le fichier de configuration.

4. Vérifiez que les modifications apportées à configmap ont été appliquées.

   ```
   kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
   ```
   {: pre}

   Sortie :
   ```
   Name:        ibm-cloud-provider-ingress-cm
 Namespace:    kube-system
 Labels:        <none>
 Annotations:    <none>

   Data
 ====

    ssl-protocols: "TLSv1 TLSv1.1 TLSv1.2"
  ssl-ciphers: "HIGH:!aNULL:!MD5"
   ```
   {: screen}

### Personnalisation du contenu et du format des journaux Ingress
{: #ingress_log_format}

Vous pouvez personnaliser le contenu et le format des journaux collectés par l'équilibreur de charge d'application (ALB) Ingress.
{:shortdesc}

Par défaut, les journaux Ingress sont au format JSON et affichent des zones de journal courantes. Vous pouvez toutefois créer un format personnalisé pour les journaux. Pour choisir les composants de journal qui sont transférés et comment ces composants sont organisés dans la sortie du journal :

1. Créez et ouvrez une version locale du fichier de configuration pour la ressource configmap `ibm-cloud-provider-ingress-cm`.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. Ajoutez une section <code>data</code>. Ajoutez la zone `log-format` et éventuellement, la zone `log-format-escape-json`.

    ```
    apiVersion: v1
    data:
      log-format: '{<key1>: <log_variable1>, <key2>: <log_variable2>, <key3>: <log_variable3>}'
      log-format-escape-json: "true"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: pre}

    <table>
    <caption>Composants du fichier YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description de la configuration du format de journal (log-format)</th>
    </thead>
    <tbody>
    <tr>
    <td><code>log-format</code></td>
    <td>Remplacez <code>&lt;key&gt;</code> par le nom du composant de journal et <code>&lt;log_variable&gt;</code> par une variable du composant de journal que vous voulez collecter dans les entrées de journal. Vous pouvez inclure le texte et la ponctuation que vous souhaitez dans l'entrée de journal, par exemple des guillemets autour des valeurs de chaîne et des virgules pour séparer les composants de journal. Par exemple, si vous formatez un composant ainsi : <code>request: "$request",</code>, vous obtenez ceci dans une entrée de journal : <code>request: "GET / HTTP/1.1",</code> . Pour obtenir la liste de toutes les variables que vous pouvez utiliser, voir le document <a href="http://nginx.org/en/docs/varindex.html">Nginx variable index</a>.<br><br>Pour consigner un en-tête supplémentaire, par exemple, <em>x-custom-ID</em>, ajoutez la paire clé-valeur suivante au contenu de journal personnalisé : <br><pre class="pre"><code>customID: $http_x_custom_id</code></pre> <br>Les traits d'union (<code>-</code>) sont convertis en traits de soulignement (<code>_</code>) et <code>$http_</code> doit être ajouté en préfixe au nom d'en-tête personnalisé.</td>
    </tr>
    <tr>
    <td><code>log-format-escape-json</code></td>
    <td>Facultatif : par défaut, les journaux sont générés en format texte. Pour générer les fichiers au format JSON, ajoutez la zone <code>log-format-escape-json</code> et utilisez la valeur <code>true</code>.</td>
    </tr>
    </tbody></table>
    </dd>
    </dl>

    Par exemple, votre format de journal peut contenir les variables suivantes :
    ```
    apiVersion: v1
    data:
      log-format: '{remote_address: $remote_addr, remote_user: "$remote_user",
                    time_date: [$time_local], request: "$request",
                    status: $status, http_referer: "$http_referer",
                    http_user_agent: "$http_user_agent",
                    request_id: $request_id}'
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: screen}

    Une entrée de journal correspondant à ce format ressemble à l'exemple suivant :
    ```
    remote_address: 127.0.0.1, remote_user: "dbmanager", time_date: [30/Mar/2018:18:52:17 +0000], request: "GET / HTTP/1.1", status: 401, http_referer: "-", http_user_agent: "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:47.0) Gecko/20100101 Firefox/47.0", request_id: a02b2dea9cf06344a25611c1d7ad72db
    ```
    {: screen}

    Pour créer un format de journal personnalisé en fonction du format par défaut de l'équilibreur de charge d'application (ALB), modifiez la section suivante selon les besoins et ajoutez-la dans configmap :
    ```
    apiVersion: v1
    data:
      log-format: '{"time_date": "$time_iso8601", "client": "$remote_addr",
                    "host": "$http_host", "scheme": "$scheme",
                    "request_method": "$request_method", "request_uri": "$uri",
                    "request_id": "$request_id", "status": $status,
                    "upstream_addr": "$upstream_addr", "upstream_status":
                    $upstream_status, "request_time": $request_time,
                    "upstream_response_time": $upstream_response_time,
                    "upstream_connect_time": $upstream_connect_time,
                    "upstream_header_time": $upstream_header_time}'
      log-format-escape-json: "true"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: codeblock}

4. Sauvegardez le fichier de configuration.

5. Vérifiez que les modifications apportées à configmap ont été appliquées.

 ```
 kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
 ```
 {: pre}

 Exemple de sortie :
 ```
 Name:        ibm-cloud-provider-ingress-cm
 Namespace:    kube-system
 Labels:        <none>
 Annotations:    <none>

 Data
 ====

  log-format: '{remote_address: $remote_addr, remote_user: "$remote_user", time_date: [$time_local], request: "$request", status: $status, http_referer: "$http_referer", http_user_agent: "$http_user_agent", request_id: $request_id}'
  log-format-escape-json: "true"
 ```
 {: screen}

4. Pour afficher les journaux de l'équilibreur de charge ALB, [créez une configuration de consignation pour le service Ingress](cs_health.html#logging) dans votre cluster.

<br />



