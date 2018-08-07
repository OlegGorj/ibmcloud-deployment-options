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




# Intégration de services
{: #integrations}

Vous pouvez utiliser différents services externes et services de catalogue avec un cluster Kubernetes standard dans {{site.data.keyword.containerlong}}.
{:shortdesc}


## Services d'application
<table summary="Récapitulatif de l'accessibilité">
<caption>Services d'application</caption>
<thead>
<tr>
<th>Service</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.blockchainfull}}</td>
<td>Permet de déployer un environnement de développement disponible publiquement pour IBM Blockchain sur un cluster Kubernetes dans {{site.data.keyword.containerlong_notm}}. Utilisez cet environnement pour développer et personnaliser votre propre réseau de blockchain afin de déployer des applications qui partagent un grand livre non modifiable dédié à l'enregistrement de l'historique des transactions. Pour plus d'informations, voir <a href="https://ibm-blockchain.github.io" target="_blank">Develop in a cloud sandbox IBM Blockchain Platform <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. </td>
</tr>
</tbody>
</table>

<br />



## Services DevOps
<table summary="Récapitulatif de l'accessibilité">
<caption>Services DevOps</caption>
<thead>
<tr>
<th>Service</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Codeship</td>
<td>Vous pouvez utiliser <a href="https://codeship.com" target="_blank">Codeship <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a> pour intégration continue et livraison de conteneurs. Pour plus d'informations, voir <a href="https://www.ibm.com/blogs/bluemix/2017/10/using-codeship-pro-deploy-workloads-ibm-container-service/" target="_blank">Using Codeship Pro To Deploy Workloads to {{site.data.keyword.containershort_notm}} <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. </td>
</tr>
<tr>
<td>Helm</td>
<td> <a href="https://helm.sh/" target="_blank">Helm <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a> est un gestionnaire de package Kubernetes. Vous pouvez créer de nouvelles chartes Helm ou utiliser des chartes Helm préexistantes pour définir, installer et mettre à niveau des applications Kubernetes complexes qui s'exécutent dans les clusters {{site.data.keyword.containerlong_notm}}. <p>Pour plus d'informations, voir [Configuration de Helm dans {{site.data.keyword.containershort_notm}}](cs_integrations.html#helm).</p></td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>Permet d'automatiser vos générations d'applications et vos déploiements de conteneur sur les clusters Kubernetes en utilisant une chaîne d'outils. Pour obtenir des informations de configuration, voir le blogue <a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">Deploy Kubernetes pods to the {{site.data.keyword.containerlong_notm}} using DevOps Pipelines <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. </td>
</tr>
<tr>
<td>Istio</td>
<td><a href="https://www.ibm.com/cloud/info/istio" target="_blank">Istio <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a> est un service open source qui permet aux développeurs de connecter, de sécuriser et de gérer de manière uniforme un réseau de microservices, également dénommé maillage de services, sur des plateformes d'orchestration cloud telles que Kubernetes. Pour en savoir plus sur le projet open source, consultez l'article de blogue rappelant qu'<a href="https://developer.ibm.com/dwblog/2017/istio/" target="_blank">IBM a cofondé et a lancé Istio<img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. Pour installer Istio sur votre cluster Kubernetes dans {{site.data.keyword.containershort_notm}} et commencer à utiliser l'exemple d'application, voir [Tutoriel : Gestion de microservices avec Istio](cs_tutorials_istio.html#istio_tutorial).</td>
</tr>
</tbody>
</table>

<br />



## Services de consignation et de surveillance
<table summary="Récapitulatif de l'accessibilité">
<caption>Services de consignation et de surveillance</caption>
<thead>
<tr>
<th>Service</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>CoScale</td>
<td>Vous pouvez surveiller les noeuds worker, les conteneurs, les jeux de répliques, les contrôleurs de réplication et les services à l'aide de <a href="https://www.coscale.com/" target="_blank">CoScale <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. Pour plus d'informations, voir <a href="https://www.ibm.com/blogs/bluemix/2017/06/monitoring-ibm-bluemix-container-service-coscale/" target="_blank">Monitoring {{site.data.keyword.containershort_notm}} with CoScale <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. </td>
</tr>
<tr>
<td>Datadog</td>
<td>Vous pouvez surveiller votre cluster et examiner les performances de l'infrastructure et les métriques d'application à l'aide de <a href="https://www.datadoghq.com/" target="_blank">Datadog <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. Pour plus d'informations, voir <a href="https://www.ibm.com/blogs/bluemix/2017/07/monitoring-ibm-bluemix-container-service-datadog/" target="_blank">Monitoring {{site.data.keyword.containershort_notm}} with Datadog <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.loganalysisfull}}</td>
<td>Développez vos capacités de collecte, de conservation et de recherche de journaux avec {{site.data.keyword.loganalysisfull_notm}}. Pour plus d'informations, voir <a href="../services/CloudLogAnalysis/containers/containers_kube_other_logs.html" target="_blank">Activation de la collecte automatique des journaux de cluster <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.monitoringlong}}</td>
<td>Développez vos capacités de collecte et de conservation des métriques en définissant des règles et des alertes avec {{site.data.keyword.monitoringlong_notm}}. Pour plus d'informations, voir <a href="../services/cloud-monitoring/tutorials/container_service_metrics.html" target="_blank">Analyse des métriques dans Grafana pour une application qui est déployée dans un cluster Kubernetes <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. </td>
</tr>
<tr>
<td>Instana</td>
<td> <a href="https://www.instana.com/" target="_blank">Instana <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a> permet de surveiller les performances de l'infrastructure et des applications à l'aide d'une interface graphique qui découvre et mappe automatiquement vos applications. Istana capture toutes les demandes adressées à vos applications, que vous pouvez utiliser pour identifier et résoudre les problèmes et effectuer une analyse de leur cause première pour éviter qu'ils ne se reproduisent. Pour en savoir plus, consultez l'article de blogue sur le <a href="https://www.instana.com/blog/precise-visibility-applications-ibm-bluemix-container-service/" target="_blank">déploiement d'Istana dans {{site.data.keyword.containershort_notm}} <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>.</td>
</tr>
<tr>
<td>Prometheus</td>
<td>Prometheus est un outil open source de surveillance, de consignation et d'alerte spécialement conçu pour Kubernetes. Prometheus extrait des informations détaillées sur le cluster, les noeuds worker et l'état de santé du déploiement à partir des informations de consignation de Kubernetes. L'activité de l'UC, de la mémoire, des E-S et du réseau est collectée pour chaque conteneur s'exécutant dans un cluster. Vous pouvez utiliser les données collectées dans des demandes ou des alertes personnalisées pour surveiller les performances et les charges de travail dans votre cluster.

<p>Pour utiliser Prometheus, suivez les <a href="https://github.com/coreos/prometheus-operator/tree/master/contrib/kube-prometheus" target="_blank">instructions CoreOS <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>.</p>
</td>
</tr>
<tr>
<td>Sematext</td>
<td>Vous pouvez afficher les métriques et les journaux de vos applications conteneurisées à l'aide de <a href="https://sematext.com/" target="_blank">Sematext <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. Pour plus d'informations, voir la rubrique sur <a href="https://www.ibm.com/blogs/bluemix/2017/09/monitoring-logging-ibm-bluemix-container-service-sematext/" target="_blank">la surveillance et la consignation des conteneurs avec Sematext <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. </td>
</tr>
<tr>
<td>Sysdig</td>
<td>Vous pouvez capturer des métriques d'application, de conteneur, de démon statsd et sur l'hôte depuis un point d'instrumentation unique à l'aide de <a href="https://sysdig.com/" target="_blank">Sysdig <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. Pour plus d'informations, voir <a href="https://www.ibm.com/blogs/bluemix/2017/08/monitoring-ibm-bluemix-container-service-sysdig-container-intelligence/" target="_blank">Monitoring {{site.data.keyword.containershort_notm}} with Sysdig Container Intelligence <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. </td>
</tr>
<tr>
<td>Weave Scope</td>
<td>Weave Scope produit un diagramme visuel de vos ressources dans un cluster Kubernetes, notamment des services, pods, conteneurs, processus, noeuds, et autres. Weave Scope fournit des métriques interactives sur l'utilisation de l'UC et de la mémoire, ainsi que des outils pour suivi et exécution dans un conteneur.<p>Pour plus d'informations, voir [Visualisation de ressources de cluster Kubernetes via Weave Scope et {{site.data.keyword.containershort_notm}}](cs_integrations.html#weavescope).</p></li></ol>
</td>
</tr>
</tbody>
</table>

<br />



## Services de sécurité
<table summary="Récapitulatif de l'accessibilité">
<caption>Services de sécurité</caption>
<thead>
<tr>
<th>Service</th>
<th>Description</th>
</tr>
</thead>
<tbody>
  <tr id="appid">
    <td>{{site.data.keyword.appid_full}}</td>
    <td>Ajoutez un niveau de sécurité à vos applications avec [{{site.data.keyword.appid_short}}](/docs/services/appid/index.html#gettingstarted) en obligeant les utilisateurs à se connecter. Pour l'authentification de demandes HTTP/HTTPS Web ou d'API vers votre application, vous pouvez intégrer {{site.data.keyword.appid_short_notm}} au service Ingress en utilisant l'[annotation Ingress d'authentification {{site.data.keyword.appid_short_notm}}](cs_annotations.html#appid-auth).</td>
  </tr>
<tr>
<td>Aqua Security</td>
  <td>En complément de <a href="/docs/services/va/va_index.html" target="_blank">Vulnerability Advisor</a>, vous pouvez utiliser <a href="https://www.aquasec.com/" target="_blank">Aqua Security <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a> pour renforcer la sécurité de vos déploiements de conteneurs en limitant ce que votre application est autorisée à effectuer. Pour plus d'informations, voir <a href="https://www.ibm.com/blogs/bluemix/2017/06/protecting-container-deployments-bluemix-aqua-security/" target="_blank">Protecting container deployments on {{site.data.keyword.Bluemix_notm}} with Aqua Security <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.cloudcerts_full}}</td>
<td>Vous pouvez utiliser <a href="../services/certificate-manager/index.html" target="_blank">{{site.data.keyword.cloudcerts_long}} <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a> pour stocker et gérer les certificats SSL pour vos applications. Pour plus d'informations, voir <a href="https://www.ibm.com/blogs/bluemix/2018/01/use-ibm-cloud-certificate-manager-ibm-cloud-container-service-deploy-custom-domain-tls-certificates/" target="_blank">Use {{site.data.keyword.cloudcerts_long_notm}} with {{site.data.keyword.containershort_notm}} to deploy custom domain TLS Certificates <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. </td>
</tr>
<tr>
  <td>{{site.data.keyword.registrylong}}</td>
  <td>Configurez votre propre référentiel d'images Docker sécurisé où vous pourrez stocker et partager des images en toute sécurité entre les utilisateurs du cluster. Pour plus d'informations, voir la <a href="/docs/services/Registry/index.html" target="_blank">documentation d'{{site.data.keyword.registrylong}} <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>.</td>
</tr>
<tr>
<td>NeuVector</td>
<td>Vous pouvez protéger vos conteneurs avec un pare-feu natif du cloud à l'aide de <a href="https://neuvector.com/" target="_blank">NeuVector <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. Pour plus d'informations, voir <a href="https://www.ibm.com/us-en/marketplace/neuvector-container-security" target="_blank">NeuVector Container Security <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. </td>
</tr>
<tr>
<td>Twistlock</td>
<td>En complément de <a href="/docs/services/va/va_index.html" target="_blank">Vulnerability Advisor</a>, vous pouvez utiliser <a href="https://www.twistlock.com/" target="_blank">Twistlock <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a> pour gérer les pare-feux, la protection contre les menaces et les réponses aux incidents. Pour plus d'informations, voir <a href="https://www.ibm.com/blogs/bluemix/2017/07/twistlock-ibm-bluemix-container-service/" target="_blank">Twistlock on {{site.data.keyword.containershort_notm}} <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>. </td>
</tr>
</tbody>
</table>

<br />



## Services de stockage
<table summary="Récapitulatif de l'accessibilité">
<caption>Services de stockage</caption>
<thead>
<tr>
<th>Service</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Heptio Ark</td>
  <td>Vous pouvez utiliser <a href="https://github.com/heptio/ark" target="_blank">Heptio Ark <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a> pour effectuer la sauvegarde et la restauration de vos ressources de cluster et vos volumes persistants. Pour plus d'informations, voir <a href="https://github.com/heptio/ark/blob/master/docs/use-cases.md#use-cases" target="_blank">les cas d'utilisation pour la reprise après incident et la migration de cluster <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a> de Heptio Ark.</td>
</tr>
<tr>
  <td>{{site.data.keyword.cos_full}}</td>
  <td>Les données stockées avec {{site.data.keyword.cos_short}} sont chiffrées et disséminées entre plusieurs emplacements géographiques. Elles sont accessibles via HTTP en utilisant une API REST. Vous pouvez utiliser l'[image ibm-backup-restore](/docs/services/RegistryImages/ibm-backup-restore/index.html) pour configurer le service de sorte à effectuer des sauvegardes ponctuelles ou planifiées pour les données figurant dans vos clusters. Pour obtenir des informations générales sur le service, voir la <a href="/docs/services/cloud-object-storage/about-cos.html" target="_blank">documentation d'{{site.data.keyword.cos_short}} <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>.</td>
</tr>
  <tr>
    <td>{{site.data.keyword.cloudantfull}}</td>
    <td>{{site.data.keyword.cloudant_short_notm}} est une base de données orientée document de type DBaas (DataBase as a Service) qui stocke les données sous forme de documents au format JSON. Le service est construit pour favoriser l'évolutivité, la haute disponibilité et la durabilité. Pour plus d'informations, voir la <a href="/docs/services/Cloudant/getting-started.html" target="_blank">documentation de {{site.data.keyword.cloudant_short_notm}} <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>.</td>
  </tr>
  <tr>
    <td>{{site.data.keyword.composeForMongoDB_full}}</td>
    <td>{{site.data.keyword.composeForMongoDB}} assure la redondance et la haute disponibilité, fournit des sauvegardes automatiques et à la demande en continu, ainsi que des outils de surveillances, l'intégration à des systèmes d'alerte, des vues d'analyses de performances, et bien plus encore. Pour plus d'informations, voir la <a href="/docs/services/ComposeForMongoDB/index.html" target="_blank">documentation de {{site.data.keyword.composeForMongoDB}} <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>.</td>
  </tr>
</tbody>
</table>


<br />



## Ajout de services Cloud Foundry à des clusters
{: #adding_cluster}

Ajoutez une instance de service Cloud Foundry existante à votre cluster pour permettre aux utilisateurs de votre cluster d'accéder et d'utiliser le service lorsqu'ils déploient une application sur le cluster.
{:shortdesc}

Avant de commencer :

1. [Ciblez votre interface de ligne de commande](cs_cli_install.html#cs_cli_configure) vers votre cluster.
2. [Demandez une instance du service {{site.data.keyword.Bluemix_notm}}](/docs/apps/reqnsi.html#req_instance).
   **Remarque :** pour créer une instance de service sur le site Washington DC, vous devez utiliser l'interface CLI.
3. Les services Cloud Foundry sont pris en charge pour la liaison avec des clusters, alors que d'autres services ne le sont pas. Vous pouvez voir les différents types de service une fois que vous avez créé l'instance de service. Les services sont regroupés dans le tableau de bord sous **Services Cloud Foundry** et **Services**. Pour lier les services de la section **Services** avec des clusters, [créez d'abord des alias Cloud Foundry](#adding_resource_cluster).

**Remarque :**
<ul><ul>
<li>Vous pouvez ajouter uniquement des services {{site.data.keyword.Bluemix_notm}} qui prennent en charge les clés de service. Si le service ne prend pas en charge les clés de service, voir [Utilisation des services {{site.data.keyword.Bluemix_notm}} avec des applications externes](/docs/apps/reqnsi.html#accser_external).</li>
<li>Le cluster et les noeuds worker doivent être complètement déployés pour pouvoir ajouter un service.</li>
</ul></ul>


Pour ajouter un service, procédez comme suit :
2.  Répertoriez les services {{site.data.keyword.Bluemix_notm}} disponibles.

    ```
    bx service list
    ```
    {: pre}

    Exemple de sortie d'interface CLI :

    ```
    name                      service           plan    bound apps   last operation   
    <service_instance_name>   <service_name>    spark                create succeeded
    ```
    {: screen}

3.  Notez la valeur du nom (**name**) de l'instance de service que vous souhaitez ajouter au cluster.
4.  Identifiez l'espace de nom du cluster que vous désirez ajouter à votre service. Sélectionnez l'une des options suivantes.
    -   Listez les espaces de nom existants et sélectionnez l'espace de nom à utiliser.

        ```
        kubectl get namespaces
        ```
        {: pre}

    -   Créez un espace de nom dans votre cluster.

        ```
        kubectl create namespace <namespace_name>
        ```
        {: pre}

5.  Ajoutez le service à votre cluster.

    ```
    bx cs cluster-service-bind <cluster_name_or_ID> <namespace> <service_instance_name>
    ```
    {: pre}

    Une fois que le service a été correctement ajouté à votre cluster, une valeur confidentielle de cluster est créée pour héberger les données d'identification de votre instance de service. Exemple de sortie d'interface CLI :

    ```
    bx cs cluster-service-bind mycluster mynamespace cleardb 
    Binding service instance to namespace...
    OK
    Namespace: mynamespace
    Secret name:     binding-<service_instance_name>
    ```
    {: screen}

6.  Vérifiez que la valeur confidentielle a bien été créée dans l'espace de nom de votre cluster.

    ```
    kubectl get secrets --namespace=<namespace>
    ```
    {: pre}

Pour utiliser le service dans un pod qui est déployé dans le cluster, les utilisateurs du cluster doivent accéder aux données d'identification du service. Ils peuvent accéder aux données d'identification du service {{site.data.keyword.Bluemix_notm}} en [montant la valeur confidentielle de Kubernetes en tant que volume secret](#adding_app).

<br />


## Création d'alias Cloud Foundry pour d'autres ressources de service {{site.data.keyword.Bluemix_notm}}
{: #adding_resource_cluster}

Les services Cloud Foundry sont pris en charge pour la liaison avec des clusters. Pour lier un service {{site.data.keyword.Bluemix_notm}} qui n'est pas un service Cloud Foundry à votre cluster, créez un alias Cloud Foundry pour l'instance de service.
{:shortdesc}

Avant de commencer, [demandez une instance du service {{site.data.keyword.Bluemix_notm}}](/docs/apps/reqnsi.html#req_instance).

Pour créer un alias Cloud Foundry pour l'instance de service :

1. Ciblez l'organisation et l'espace de création de l'instance de service.

    ```
    bx target -o <org_name> -s <space_name>
    ```
    {: pre}

2. Notez le nom de l'instance de service.
    ```
    bx resource service-instances
    ```
    {: pre}

3. Créez un alias Cloud Foundry pour l'instance de service.
    ```
    bx resource service-alias-create <service_alias_name> --instance-name <service_instance>
    ```
    {: pre}

4. Vérifiez que l'alias du service a été créé.

    ```
    bx service list
    ```
    {: pre}

5. [Liez l'alias Cloud Foundry au cluster](#adding_cluster).



<br />


## Ajout de services à des applications
{: #adding_app}

Des valeurs confidentielles  Kubernetes chiffrées sont utilisées pour stocker les informations détaillées du service {{site.data.keyword.Bluemix_notm}} et ses données d'identification, ainsi que pour permettre une communication sécurisée entre le service et le cluster.
{:shortdesc}

Les valeurs confidentielles Kubernetes permettent de stocker de manière sécurisée des informations sensibles, comme les noms des utilisateurs, leurs mots de passe ou leurs clés. Au lieu d'exposer des informations confidentielles via des variables d'environnement, ou directement dans le fichier Dockerfile, vous pouvez monter des valeurs confidentielles sur un pod. Ces valeurs sont alors accessibles à un conteneur en cours d'exécution sur un pod.

Lorsque vous montez un volume secret sur votre pod, un fichier nommé `binding` est stocké dans le répertoire de montage du volume. Le fichier `binding` comprend toutes les informations et données d'identification dont vous avez besoin pour accéder au service {{site.data.keyword.Bluemix_notm}}.

Avant de commencer, [ciblez avec votre interface de ligne de commande](cs_cli_install.html#cs_cli_configure) votre cluster. Vérifiez que le service {{site.data.keyword.Bluemix_notm}} que vous souhaitez utiliser dans votre application
a été [ajouté au cluster](cs_integrations.html#adding_cluster) par l'administrateur du cluster.

1.  Répertoriez les valeurs confidentielles disponibles dans l'espace de nom de votre cluster.

    ```
    kubectl get secrets --namespace=<my_namespace>
    ```
    {: pre}

    Exemple de sortie :

    ```
    NAME                                    TYPE                                  DATA      AGE
    binding-<service_instance_name>         Opaque                                1         3m

    ```
    {: screen}

2.  Recherchez une valeur confidentielle du type **Opaque** et notez le
**nom** de cette valeur confidentielle. S'il existe plusieurs valeurs confidentielles, contactez l'administrateur de votre cluster pour identifier la valeur confidentielle appropriée.

3.  Ouvrez l'éditeur de votre choix.

4.  Créez un fichier YAML pour configurer un pod pouvant accéder aux informations détaillées du service via un volume secret. Si vous avez lié plusieurs services, vérifiez que chaque valeur confidentielle est associée au service correct.

    ```
    apiVersion: apps/v1beta1
    kind: Deployment
    metadata:
      labels:
        app: secret-test
      name: secret-test
      namespace: <my_namespace>
    spec:
      selector:
        matchLabels:
          app: secret-test
      replicas: 1
      template:
        metadata:
          labels:
            app: secret-test
        spec:
          containers:
          - image: nginx
            name: secret-test
            volumeMounts:
            - mountPath: /opt/service-bind
              name: service-bind-volume
          volumes:
          - name: service-bind-volume
            secret:
              defaultMode: 420
              secretName: binding-<service_instance_name>
    ```
    {: codeblock}

    <table>
    <caption>Description des composants du fichier YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants du fichier YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code>volumeMounts/mountPath</code></td>
    <td>Nom du volume secret que vous désirez monter dans le conteneur.</td>
    </tr>
    <tr>
    <td><code>volumes/name</code></td>
    <td>Entrez un nom pour le volume secret que vous désirez monter dans votre conteneur.</td>
    </tr>
    <tr>
    <td><code>secret/defaultMode</code></td>
    <td>Affectez des droit d'accès en lecture seule à la valeur confidentielle du service.</td>
    </tr>
    <tr>
    <td><code>secret/secretName</code></td>
    <td>Entrez le nom de la valeur confidentielle que vous avez noté auparavant.</td>
    </tr></tbody></table>

5.  Créez le pod et montez le volume secret.

    ```
    kubectl apply -f secret-test.yaml
    ```
    {: pre}

6.  Vérifiez que le pod a bien été créé.

    ```
    kubectl get pods --namespace=<my_namespace>
    ```
    {: pre}

    Exemple de sortie d'interface CLI :

    ```
    NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
    ```
    {: screen}

7.  Notez la valeur **NAME** de votre pod.
8.  Obtenez les détails du pod et recherchez le nom de la clé confidentielle.

    ```
    kubectl describe pod <pod_name>
    ```
    {: pre}

    Sortie :

    ```
    ...
    Volumes:
      service-bind-volume:
        Type:       Secret (a volume populated by a Secret)
        SecretName: binding-<service_instance_name>
    ...
    ```
    {: screen}

    

9.  Configurez vos applications pour trouver le fichier de valeurs confidentielles `binding` dans le répertoire de montage, analyser le contenu JSON et déterminer l'URL et les données d'identification pour accéder à votre service {{site.data.keyword.Bluemix_notm}}.

Vous pouvez à présent accéder aux informations détaillées et aux données d'identification du service {{site.data.keyword.Bluemix_notm}}. Pour utiliser votre service {{site.data.keyword.Bluemix_notm}}, veillez à ce que votre application soit configurée pour rechercher le fichier de la valeur confidentielle du service dans le répertoire de montage, analyser le contenu JSON et examiner les informations détaillées du service.

<br />


## Configuration de Helm dans {{site.data.keyword.containershort_notm}}
{: #helm}

[Helm ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://helm.sh/) est un gestionnaire de packages Kubernetes. Vous pouvez créer des chartes Helm ou utiliser des chartes Helm préexistantes pour définir, installer et mettre à niveau des applications Kubernetes complexes qui s'exécutent dans les clusters {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

Avant d'utiliser des chartes Helm avec {{site.data.keyword.containershort_notm}}, vous devez installer et initialiser une instance Helm pour votre cluster. Vous pouvez ensuite ajouter le référentiel Helm {{site.data.keyword.Bluemix_notm}} à votre instance Helm.

Avant de commencer, [ciblez avec votre interface de ligne de commande (CLI)](cs_cli_install.html#cs_cli_configure) le cluster dans lequel vous désirez utiliser une charte Helm.

1. Installez l'<a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">interface de ligne de commande Helm  <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>.

2. **Important** : pour assurer la sécurité du cluster, créez un compte de service pour Tiller dans l'espace de nom `kube-system` et une liaison de rôle de cluster RBAC Kubernetes pour le pod `tiller-deploy`.

    1. Dans l'éditeur de votre choix, créez le fichier suivant et sauvegardez-le avec le nom `rbac-config.yaml`.
      **Remarque **:
        * Le rôle de cluster `cluster-admin` est créé par défaut dans les clusters Kubernetes, par conséquent, vous n'avez pas à le définir de manière explicite.
        * Si vous utilisez un cluster de version 1.7.x, remplacez `apiVersion` par `rbac.authorization.k8s.io/v1beta1`.

      ```
      apiVersion: v1
      kind: ServiceAccount
      metadata:
        name: tiller
        namespace: kube-system
      ---
      apiVersion: rbac.authorization.k8s.io/v1
      kind: ClusterRoleBinding
      metadata:
        name: tiller
      roleRef:
        apiGroup: rbac.authorization.k8s.io
        kind: ClusterRole
        name: cluster-admin
      subjects:
        - kind: ServiceAccount
          name: tiller
          namespace: kube-system
      ```
      {: codeblock}

    2. Créez le compte de service et la liaison de rôle de cluster.

        ```
        kubectl create -f rbac-config.yaml
        ```
        {: pre}

3. Initialisez Helm et installez `tiller` avec le compte de service que vous avez créé.

    ```
    helm init --service-account tiller
    ```
    {: pre}

4. Vérifiez que la zone **Status** du pod `tiller-deploy` indique `Running` dans votre cluster.

    ```
    kubectl get pods -n kube-system -l app=helm
    ```
    {: pre}

    Exemple de sortie :

    ```
    NAME                            READY     STATUS    RESTARTS   AGE
    tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
    ```
    {: screen}

5. Ajoutez le référentiel {{site.data.keyword.Bluemix_notm}} Helm à votre instance Helm.

    ```
    helm repo add ibm  https://registry.bluemix.net/helm/ibm
    ```
    {: pre}

6. Répertoriez les chartes Helm actuellement disponibles dans le référentiel {{site.data.keyword.Bluemix_notm}}.

    ```
    helm search ibm
    ```
    {: pre}

7. Pour en savoir plus sur une charte, affichez la liste de ses paramètres et valeurs par défaut.

    Par exemple, pour afficher les paramètres, la documentation et les valeurs par défaut de la charte Helm du service VPN IPSec strongSwan, exécutez la commande :

    ```
    helm inspect ibm/strongswan
    ```
    {: pre}


### Liens associés à Helm
{: #helm_links}

* Pour utiliser la charte Helm strongSwan, voir [Configuration de la connectivité VPN avec la charte Helm du service VPN IPsec strongSwan](cs_vpn.html#vpn-setup).
* Affichez les chartes Helm disponibles que vous pouvez utiliser avec {{site.data.keyword.Bluemix_notm}} dans l'interface graphique du [catalogue des chartes Helm ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/containers-kubernetes/solutions/helm-charts).
* Pour en savoir plus sur les commandes Helm utilisées pour configurer et gérer des chartes Helm, voir la <a href="https://docs.helm.sh/helm/" target="_blank">documentation Helm <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>.
* Pour en savoir plus sur comment augmenter la vitesse de déploiement avec les chartes Helm de Kubernetes, voir [Increase deployment velocity with Kubernetes Helm Charts ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://developer.ibm.com/recipes/tutorials/increase-deployment-velocity-with-kubernetes-helm-charts/).

## Visualisation de ressources de cluster Kubernetes
{: #weavescope}

Weave Scope produit un diagramme visuel de vos ressources dans un cluster Kubernetes, notamment des services, pods, conteneurs, etc. Weave Scope fournit des métriques interactives sur l'utilisation de l'UC et de la mémoire, ainsi que des outils pour suivi et exécution dans un conteneur.
{:shortdesc}

Avant de commencer :

-   Prenez soin de ne pas exposer les informations de votre cluster sur l'Internet public. Procédez comme suit pour déployer de manière sécurisée Weave Scope et y accéder en local depuis un navigateur Web.
-   Si ce n'est déjà fait, [créez un cluster standard](cs_clusters.html#clusters_ui). Weave Scope peut solliciter énormément l'unité centrale, surtout l'application. Exécutez Weave Scope avec des clusters standard plus volumineux et non pas des clusters gratuits.
-   [Ciblez avec votre interface CLI](cs_cli_install.html#cs_cli_configure) votre cluster pour exécuter des commandes `kubectl`.


Pour utiliser Weave Scope avec un cluster, procédez comme suit :
2.  Déployez dans le cluster l'un des fichiers de configuration d'autorisations RBAC fournis.

    Pour activer les droits d'accès en lecture / écriture, exécutez la commande suivante :

    ```
    kubectl apply -f "https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/weave-scope/weave-scope-rbac.yaml"
    ```
    {: pre}

    Pour activer les droits d'accès en lecture seule, exécutez la commande suivante :

    ```
    kubectl apply --namespace weave -f "https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/weave-scope/weave-scope-rbac-readonly.yaml"
    ```
    {: pre}

    Sortie :

    ```
    clusterrole "weave-scope-mgr" created
    clusterrolebinding "weave-scope-mgr-role-binding" created
    ```
    {: screen}

3.  Déployez le service Weave Scope, lequel est accessible en mode privé via l'adresse IP du cluster.

    <pre class="pre">
    <code>kubectl apply --namespace weave -f "https://cloud.weave.works/k8s/scope.yaml?k8s-version=$(kubectl version | base64 | tr -d '&bsol;n')"</code>
    </pre>

    Sortie :

    ```
    serviceaccount "weave-scope" created
    deployment "weave-scope-app" created
    service "weave-scope-app" created
    daemonset "weave-scope-agent" created
    ```
    {: screen}

4.  Exécutez une commande d'acheminement de port pour ouvrir le service sur votre ordinateur. La prochaine fois que vous accéderez à Weave Scope, vous pourrez exécuter cette commande d'acheminement de port sans avoir à compléter à nouveau les étapes de configuration précédentes.

    ```
    kubectl port-forward -n weave "$(kubectl get -n weave pod --selector=weave-scope-component=app -o jsonpath='{.items..metadata.name}')" 4040
    ```
    {: pre}

    Sortie :

    ```
    Forwarding from 127.0.0.1:4040 -> 4040
    Forwarding from [::1]: :1]:4040 -> 4040
    Handling connection for 4040
    ```
    {: screen}

5.  Ouvrez votre navigateur Web et accédez à l'adresse `http://localhost:4040`. Si les composants par défaut ne sont pas déployés, vous obtenez le diagramme suivant. Vous pouvez choisir d'afficher des diagrammes de topologie ou bien des tableaux des ressources Kubernetes dans le cluster.

     <img src="images/weave_scope.png" alt="Exemple de topologie Weave Scope" style="width:357px;" />


[En savoir plus sur les fonctions Weave Scope ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.weave.works/docs/scope/latest/features/).

<br />

