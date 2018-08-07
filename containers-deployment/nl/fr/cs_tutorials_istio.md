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



# Tutoriel : Installation d'Istio sur {{site.data.keyword.containerlong_notm}}
{: #istio_tutorial}

[Istio](https://www.ibm.com/cloud/info/istio) est une plateforme ouverte pour connecter, sécuriser et gérer un réseau de microservices, également dénommée maillage de services, sur des plateformes cloud telles que Kubernetes dans {{site.data.keyword.containerlong}}. Istio vous permet de gérer le trafic réseau, d'équilibrer la charge entre les microservices, d'appliquer des règles d'accès, de vérifier l'identité des services, etc.
{:shortdesc}

Dans ce tutoriel, vous découvrirez comment installer Istio de pair avec quatre microservices pour une application de librairie fictive dénommée BookInfo. Les microservices incluent une page de projet Web, les détails de livres, leurs revues et évaluations. Lorsque vous déployez des microservices BookInfo sur un cluster {{site.data.keyword.containershort}} sur lequel Istio est installé, vous injectez les proxies sidecar Istio Envoy dans les pods de chaque microservice.

**Remarque **: certaines configurations et fonctionnalités de la plateforme Istio sont encore en en cours de développement et sons susceptibles d'être modifiées compte tenu des retours d'information des utilisateurs. Patientez quelques mois pour la stabilisation d'Istio avant de l'utiliser en production. 

## Objectifs

-   Télécharger et installez Istio dans votre cluster
-   Déployer l'exemple d'application BookInfo
-   Injecter des proxies sidecar Envoy dans les pods des quatre microservices de l'application pour les connecter au maillage des services
-   Vérifiez le déploiement de l'application BookInfo à travers les trois versions du service d'évaluation

## Durée

30 minutes

## Public

Ce tutoriel est destiné aux développeurs de logiciel et aux administrateurs réseau qui utilisent Istio pour la première fois.

## Conditions prérequises

-  [Installez les interfaces de ligne de commande (CLI)](cs_cli_install.html#cs_cli_install_steps). Istio nécessite Kubernetes version 1.9 ou supérieure. Veillez à installer la version de l'interface CLI `kubectl` correspondant à la version Kubernetes de votre cluster.
-  [Créez un cluster](cs_clusters.html#clusters_cli) avec Kubernetes version 1.9 ou supérieure.
-  [Ciblez l'interface CLI sur votre cluster](cs_cli_install.html#cs_cli_configure).

## Leçon 1 : Téléchargement et installation d'Istio
{: #istio_tutorial1}

Téléchargez et installez Istio dans votre cluster.
{:shortdesc}

1. Téléchargez directement Istio depuis [https://github.com/istio/istio/releases ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/istio/istio/releases) ou obtenez la version la plus récente via curl :

   ```
   curl -L https://git.io/getLatestIstio | sh -
   ```
   {: pre}

2. Décompressez les fichiers d'installation.

3. Ajoutez le client `istioctl` à votre variable PATH. Par exemple, sur un système MacOS ou Linux, exécutez la commande suivante :

   ```
   export PATH=$PWD/istio-0.4.0/bin:$PATH
   ```
   {: pre}

4. Changez de répertoire pour accéder à l'emplacement du fichier Istio.

   ```
   cd filepath/istio-0.4.0
   ```
   {: pre}

5. Installez Istio sur le cluster Kubernetes. Istio est déployé sous l'espace de nom Kubernetes `istio-system`.

   ```
   kubectl apply -f install/kubernetes/istio.yaml
   ```
   {: pre}

   **Remarque **: si vous avez besoin d'activer l'authentification TLS mutuelle entre sidecars, vous pouvez installer à la place le fichier `istio-auth` : `kubectl apply -f install/kubernetes/istio-auth.yaml`

6. Avant de continuer, vérifiez que les services Kubernetes `istio-pilot`, `istio-mixer` et `istio-ingress` ont été complètement déployés.

   ```
   kubectl get svc -n istio-system
   ```
   {: pre}

   ```
   NAME            TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)                                                            AGE
   istio-ingress   LoadBalancer   172.21.xxx.xxx   169.xx.xxx.xxx   80:31176/TCP,443:30288/TCP                                         2m
   istio-mixer     ClusterIP      172.21.xxx.xxx     <none>           9091/TCP,15004/TCP,9093/TCP,9094/TCP,9102/TCP,9125/UDP,42422/TCP   2m
   istio-pilot     ClusterIP      172.21.xxx.xxx    <none>           15003/TCP,443/TCP                                                  2m
   ```
   {: screen}

7. Avant de continuer, vérifiez que les pods `istio-pilot-*`, `istio-mixer-*`, `istio-ingress-*` et `istio-ca-*` correspondants ont également été complètement déployés.

   ```
   kubectl get pods -n istio-system
   ```
   {: pre}

   ```
   istio-ca-3657790228-j21b9           1/1       Running   0          5m
   istio-ingress-1842462111-j3vcs      1/1       Running   0          5m
   istio-pilot-2275554717-93c43        1/1       Running   0          5m
   istio-mixer-2104784889-20rm8        2/2       Running   0          5m
   ```
   {: screen}


Bien joué ! L'installation d'Istio dans votre cluster a abouti. Déployez ensuite l'exemple d'application BookInfo dans votre cluster.


## Leçon 2 : Déploiement de l'application BookInfo
{: #istio_tutorial2}

Déployez les microservices de l'exemple d'application BookInfo dans votre cluster Kubernetes.
{:shortdesc}

Ces quatre microservices incluent une page Web de produit, les détails de livres, leurs revues (avec plusieurs versions du microservice de revue), et leurs évaluations. Tous les fichiers utilisés dans cet exemple figurent sous votre répertoire d'installation Istio `samples/bookinfo`.

Lorsque vous déployez BookInfo, des proxies sidecar Envoy sont injectés en tant que conteneurs dans les pods des microservices de votre application avant que ces pods ne soient déployés. Istio utilise une version étendue du proxy Envoy pour médiation de tout le trafic entrant et sortant des tous les microservices du maillage de services. Pour plus d'informations sur Envoy, reportez-vous à la [Documentation Istio ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://istio.io/docs/concepts/what-is-istio/overview.html#envoy).

1. Déployez l'application BookInfo. La commande `kube-inject` ajoute Envoy au fichier `bookinfo.yaml` et utilise ce fichier mis à jour pour déployer l'application. Lors du déploiement des microservices d'application, le sidecar Envoy est également déployé dans chaque pod de microservice.

   ```
   kubectl apply -f <(istioctl kube-inject -f samples/bookinfo/kube/bookinfo.yaml)
   ```
   {: pre}

2. Vérifiez que les microservices et leur pods correspondants ont été déployés :

   ```
   kubectl get svc
   ```
   {: pre}

   ```
   NAME                       CLUSTER-IP   EXTERNAL-IP   PORT(S)              AGE
   details                    10.xxx.xx.xxx    <none>        9080/TCP             6m
   kubernetes                 10.xxx.xx.xxx     <none>        443/TCP              30m
   productpage                10.xxx.xx.xxx   <none>        9080/TCP             6m
   ratings                    10.xxx.xx.xxx    <none>        9080/TCP             6m
   reviews                    10.xxx.xx.xxx   <none>        9080/TCP             6m
   ```
   {: screen}

   ```
   kubectl get pods
   ```
   {: pre}

   ```
   NAME                                        READY     STATUS    RESTARTS   AGE
   details-v1-1520924117-48z17                 2/2       Running   0          6m
   productpage-v1-560495357-jk1lz              2/2       Running   0          6m
   ratings-v1-734492171-rnr5l                  2/2       Running   0          6m
   reviews-v1-874083890-f0qf0                  2/2       Running   0          6m
   reviews-v2-1343845940-b34q5                 2/2       Running   0          6m
   reviews-v3-1813607990-8ch52                 2/2       Running   0          6m
   ```
   {: screen}

3. Pour vérifier le déploiement de l'application, extrayez l'adresse publique de votre cluster.

    * Si vous utilisez un cluster standard, exécutez la commande suivante pour extraire l'adresse IP Ingress et le port de votre cluster :

       ```
       kubectl get ingress
       ```
       {: pre}

       Exemple de sortie :

       ```
       NAME      HOSTS     ADDRESS          PORTS     AGE
       gateway   *         169.xx.xxx.xxx   80        3m
       ```
       {: screen}

       L'adresse Ingress résultant pour cet exemple est `169.48.221.218:80`. Exportez l'adresse comme URL de passerelle avec la commande suivante. Vous utiliserez l'URL de passerelle à l'étape suivante pour accéder à la page du produit BookInfo.

       ```
       export GATEWAY_URL=169.xx.xxx.xxx:80
       ```
       {: pre}

    * Si vous utilisez un cluster gratuit, vous devez utiliser l'adresse IP publique du noeud worker et le NodePort. Exécutez la commande suivante pour obtenir l'adresse IP publique du noeud worker :

       ```
       bx cs workers <cluster_name_or_ID>
       ```
       {: pre}

       Exportez l'adresse IP publique du noeud worker comme URL de passerelle avec la commande suivante. Vous utiliserez l'URL de passerelle à l'étape suivante pour accéder à la page du produit BookInfo.

       ```
       export GATEWAY_URL=<worker_node_public_IP>:$(kubectl get svc istio-ingress -n istio-system -o jsonpath='{.spec.ports[0].nodePort}')
       ```
       {: pre}

4. Utilisez curl sur la variable `GATEWAY_URL` pour vérifier que BookInfo est en exécution. Une réponse `200` signifie que BookInfo fonctionne correctement avec Istio.

   ```
   curl -I http://$GATEWAY_URL/productpage
   ```
   {: pre}

5. Dans un navigateur, accédez à `http://$GATEWAY_URL/productpage` pour afficher la page Web de BookInfo.

6. Essayez d'actualiser la page plusieurs fois. Différentes versions de la section des revues affichent des étoiles rouges, des étoiles noires, ou pas d'étoiles.

Bien joué ! Vous avez déployé l'exemple BookInfo avec des sidecars Istio Envoy. Vous pouvez ensuite nettoyer vos ressources ou continuer avec d'autres tutoriels pour explorer davantage Istio.

## Nettoyage
{: #istio_tutorial_cleanup}

Si vous avez fini d'utiliser Istio et que vous ne voulez pas [continuer à l'explorer](#istio_tutorial_whatsnext), vous pouvez procéder au nettoyage des ressources Istio dans votre cluster.
{:shortdesc}

1. Supprimez tous les services, les pods et les déploiements BookInfo dans le cluster.

   ```
   samples/bookinfo/kube/cleanup.sh
   ```
   {: pre}

2. Désinstallez Istio.

   ```
   kubectl delete -f install/kubernetes/istio.yaml
   ```
   {: pre}

## Etape suivante ?
{: #istio_tutorial_whatsnext}

Si vous désirez explorer davantage Istio, d'autres guides figurent dans la [Documentation Istio ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://istio.io/).

* [Intelligent Routing ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://istio.io/docs/guides/intelligent-routing.html) : cet exemple illustre comment acheminer le trafic vers une version spécifique de microservices de revue et d'évaluation BookInfo en utilisant les capacités de gestion de trafic d'Istio.

* [In-Depth Telemetry ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://istio.io/docs/guides/telemetry.html) : cet exemple comprend les instructions pour obtenir des métriques, des journaux et des traces uniformes à travers les microservices BookInfo en utilisant Istio Mixer et le proxy Envoy.
