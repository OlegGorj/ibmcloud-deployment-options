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



# Traitement des incidents liés à la mise en réseau au sein d'un cluster
{: #cs_troubleshoot_network}

Lorsque vous utilisez {{site.data.keyword.containerlong}}, envisagez l'utilisation de ces techniques pour identifier et résoudre les incidents liés à la mise en réseau au sein d'un cluster.
{: shortdesc}

Si vous rencontrez un problème d'ordre plus général, expérimentez le [débogage de cluster](cs_troubleshoot.html).
{: tip}


## Impossible de se connecter à une application via un service d'équilibreur de charge
{: #cs_loadbalancer_fails}

{: tsSymptoms}
Vous avez exposé votre application au public en créant un service d'équilibreur de charge dans votre cluster. Lorsque vous avez essayé de vous connecter à votre application en utilisant l'adresse IP publique de l'équilibreur de charge, la connexion a échoué ou expiré.

{: tsCauses}
Il se peut que le service d'équilibreur de charge ne fonctionne pas correctement pour l'une des raisons suivantes :

-   Le cluster est un cluster gratuit ou un cluster standard avec un seul noeud worker.
-   Le cluster n'est pas encore complètement déployé.
-   Le script de configuration pour votre service d'équilibreur de charge comporte des erreurs.

{: tsResolve}
Pour identifier et résoudre les problèmes liés à votre service d'équilibreur de charge :

1.  Prenez soin de configurer un cluster standard qui est entièrement déployé et qui comporte au moins deux noeuds worker afin d'assurer la haute disponibilité de votre service d'équilibreur de charge.

  ```
  bx cs workers <cluster_name_or_ID>
  ```
  {: pre}

    Dans la sortie générée par votre interface de ligne de commande, vérifiez que la valeur **Ready** apparaît dans la zone **Status** pour vos noeuds worker et qu'une autre valeur que **free** est spécifiée dans la zone **Machine Type**

2.  Vérifiez que le fichier de configuration du service d'équilibreur de charge est correct.

    ```
    apiVersion: v1
    kind: Service
    metadata:
      name: myservice
    spec:
      type: LoadBalancer
      selector:
        <selector_key>:<selector_value>
      ports:
       - protocol: TCP
         port: 8080
    ```
    {: pre}

    1.  Vérifiez que vous avez défini **LoadBalancer** comme type de service.
    2.  Dans la section `spec.selector` du service LoadBalancer, vérifiez que les valeurs de `<selector_key>` et `<selector_value>` sont identiques à la paire clé/valeur que vous avez utilisée dans la section `spec.template.metadata.labels` de votre fichier YAML de déploiement. Si les libellés ne correspondent pas, la section **Endpoints** de votre service LoadBalancer affiche **<none>** et votre application n'est pas accessible sur Internet.
    3.  Vérifiez que vous avez utilisé le **port** sur lequel votre application est en mode écoute.

3.  Vérifiez votre service d'équilibreur de charge et passez en revue la section **Events** à la recherche d'éventuelles erreurs.

    ```
    kubectl describe service <myservice>
    ```
    {: pre}

    Recherchez les messages d'erreur suivants :

    <ul><li><pre class="screen"><code>Clusters with one node must use services of type NodePort</code></pre></br>Pour utiliser le service d'équilibreur de charge, vous devez disposer d'un cluster standard et d'au moins deux noeuds worker.</li>
    <li><pre class="screen"><code>No cloud provider IPs are available to fulfill the load balancer service request. Add a portable subnet to the cluster and try again</code></pre></br>Ce message d'erreur indique qu'il ne reste aucune adresse IP publique portable à attribuer à votre service d'équilibreur de charge. Pour savoir comment demander des adresses IP publiques portables pour votre cluster, voir la rubrique <a href="cs_subnets.html#subnets">Ajout de sous-réseaux à des clusters</a>. Dès lors que des adresses IP publiques portables sont disponibles pour le cluster, le service d'équilibreur de charge est automatiquement créé.</li>
    <li><pre class="screen"><code>Requested cloud provider IP <cloud-provider-ip> is not available. The following cloud provider IPs are available: <available-cloud-provider-ips></code></pre></br>Vous avez défini une adresse IP publique portable pour votre service d'équilibreur de charge à l'aide de la section **loadBalancerIP**, or, cette adresse IP publique portable n'est pas disponible dans votre sous-réseau public portable. Dans la section **loadBalancerIP** de votre script de configuration, supprimez l'adresse IP existante et ajoutez l'une des adresses IP publiques portables disponibles. Vous pouvez également retirer la section **loadBalancerIP** de votre script de sorte qu'une adresse IP publique portable disponible puisse être allouée automatiquement.</li>
    <li><pre class="screen"><code>No available nodes for load balancer services</code></pre>Vous ne disposez pas de suffisamment de noeuds worker pour déployer un service d'équilibreur de charge. Il se pourrait que vous ayez déployé un cluster standard avec plusieurs noeuds worker, mais que la mise à disposition des noeuds worker ait échoué.</li>
    <ol><li>Affichez la liste des noeuds worker disponibles.</br><pre class="codeblock"><code>kubectl get nodes</code></pre></li>
    <li>Si au moins deux noeuds worker disponibles sont trouvés, affichez les détails de ces noeuds worker.</br><pre class="codeblock"><code>bx cs worker-get [&lt;cluster_name_or_ID&gt;] &lt;worker_ID&gt;</code></pre></li>
    <li>Vérifiez que les ID de VLAN privé et public pour les noeuds worker renvoyés par les commandes <code>kubectl get nodes</code> et <code>bx cs [&lt;cluster_name_or_ID&gt;] worker-get</code> correspondent.</li></ol></li></ul>

4.  Si vous utilisez un domaine personnalisé pour vous connecter à votre service d'équilibreur de charge, assurez-vous que votre domaine personnalisé est mappé à l'adresse IP publique de votre service d'équilibreur de charge.
    1.  Identifiez l'adresse IP publique de votre service d'équilibreur de charge.

        ```
        kubectl describe service <service_name> | grep "LoadBalancer Ingress"
        ```
        {: pre}

    2.  Assurez-vous que votre domaine personnalisé est mappé à l'adresse IP publique portable de votre service d'équilibreur de charge dans le pointeur (enregistrement PTR).

<br />




## Impossible de se connecter à une application via Ingress
{: #cs_ingress_fails}

{: tsSymptoms}
Vous avez exposé votre application au public en créant une ressource Ingress pour votre application dans votre cluster. Lorsque vous avez essayé de vous connecter à votre application en utilisant l'adresse IP publique ou le sous-domaine de l'équilibreur de charge d'application (ALB) Ingress, la connexion a échoué ou expiré.

{: tsCauses}
Il se peut qu'Ingress ne fonctionne pas correctement pour les raisons suivantes :
<ul><ul>
<li>Le cluster n'est pas encore complètement déployé.
<li>Le cluster a été configuré en tant que cluster gratuit ou standard avec un seul noeud worker.
<li>Le script de configuration Ingress contient des erreurs.
</ul></ul>

{: tsResolve}
Pour identifier et résoudre les problèmes liés à votre contrôleur Ingress :

1.  Prenez soin de configurer un cluster standard qui est entièrement déployé et qui comporte au moins deux noeuds worker afin d'assurer la haute disponibilité de votre équilibreur de charge ALB.

  ```
  bx cs workers <cluster_name_or_ID>
  ```
  {: pre}

    Dans la sortie générée par votre interface de ligne de commande, vérifiez que la valeur **Ready** apparaît dans la zone **Status** pour vos noeuds worker et qu'une autre valeur que **free** est spécifiée dans la zone **Machine Type**

2.  Récupérez le sous-domaine et l'adresse IP publique de l'équilibreur de charge ALB, puis exécutez une commande PING vers chacun d'eux.

    1.  Récupérez le sous-domaine de l'équilibreur de charge ALB.

      ```
      bx cs cluster-get <cluster_name_or_ID> | grep "Ingress subdomain"
      ```
      {: pre}

    2.  Exécutez une commande ping vers le sous-domaine de l'équilibreur de charge ALB.

      ```
      ping <ingress_subdomain>
      ```
      {: pre}

    3.  Récupérez l'adresse IP publique de votre équilibreur de charge ALB.

      ```
      nslookup <ingress_subdomain>
      ```
      {: pre}

    4.  Exécutez une commande PING vers l'adresse IP publique de l'équilibreur de charge ALB.

      ```
      ping <ALB_IP>
      ```
      {: pre}

    Si l'interface CLI renvoie un dépassement de délai d'attente pour l'adresse IP publique ou le sous-domaine de l'équilibreur de charge ALB, et que vous avez configuré un pare-feu personnalisé pour protéger vos noeuds worker, ouvrez des ports et des groupes réseau supplémentaires dans votre [pare-feu](cs_troubleshoot_clusters.html#cs_firewall).

3.  Si vous utilisez un domaine personnalisé, assurez-vous qu'il est mappé à l'adresse IP publique ou au sous-domaine de l'équilibreur de charge ALB fourni par IBM avec votre fournisseur DNS.
    1.  Si vous avez utilisé le sous-domaine de l'équilibreur de charge ALB, vérifiez le nom canonique (enregistrement CNAME).
    2.  Si vous avez utilisé l'adresse IP publique de l'équilibreur de charge ALB, assurez-vous que votre domaine personnalisé est mappé à l'adresse IP publique portable dans le pointeur (enregistrement PTR).
4.  Vérifiez le fichier de configuration de ressource Ingress.

    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingress
    spec:
      tls:
      - hosts:
        - <ingress_subdomain>
        secretName: <ingress_tls_secret>
      rules:
      - host: <ingress_subdomain>
        http:
          paths:
          - path: /
        backend:
          serviceName: myservice
          servicePort: 80
    ```
    {: codeblock}

    1.  Vérifiez que le sous-domaine de l'équilibreur de charge ALB et le certificat TLS sont corrects. Pour obtenir le certificat TLS et le sous-domaine fournis par IBM, exécutez la commande `bx cs cluster-get <cluster_name_or_ID>`.
    2.  Assurez-vous que votre application est en mode écoute sur le même chemin que celui qui est configuré dans la section **path** de votre contrôleur Ingress. Si votre application est configurée pour être en mode écoute sur le chemin racine, ajoutez **/** comme chemin.
5.  Vérifiez le déploiement du contrôleur Ingress et recherchez les éventuels messages d'erreur ou d'avertissement.

    ```
    kubectl describe ingress <myingress>
    ```
    {: pre}

    Par exemple, dans la section **Events** de la sortie, vous pouvez voir des messages d'avertissement à propos de valeurs non valides dans votre ressource Ingress ou dans certaines annotations que vous avez utilisées.

    ```
    Name:             myingress
    Namespace:        default
    Address:          169.xx.xxx.xxx,169.xx.xxx.xxx
    Default backend:  default-http-backend:80 (<none>)
    Rules:
      Host                                             Path  Backends
      ----                                             ----  --------
      mycluster.us-south.containers.appdomain.cloud
                                                       /tea      myservice1:80 (<none>)
                                                       /coffee   myservice2:80 (<none>)
    Annotations:
      custom-port:        protocol=http port=7490; protocol=https port=4431
      location-modifier:  modifier='~' serviceName=myservice1;modifier='^~' serviceName=myservice2
    Events:
      Type     Reason             Age   From                                                            Message
      ----     ------             ----  ----                                                            -------
      Normal   Success            1m    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  TLSSecretNotFound  1m    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress resource.
      Normal   Success            59s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  AnnotationError    40s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress.bluemix.net/custom-port annotation. Error annotation format error : One of the mandatory fields not valid/missing for annotation ingress.bluemix.net/custom-port
      Normal   Success            40s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  AnnotationError    2s    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress.bluemix.net/custom-port annotation. Invalid port 7490. Annotation cannot use ports 7481 - 7490
      Normal   Success            2s    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
    ```
    {: screen}

6.  Vérifiez les journaux de votre équilibreur de charge ALB.
    1.  Récupérez l'ID des pods Ingress qui sont en cours d'exécution dans votre cluster.

      ```
      kubectl get pods -n kube-system | grep alb
      ```
      {: pre}

    2.  Récupérez les journaux pour chaque pod Ingress.

      ```
      kubectl logs <ingress_pod_ID> nginx-ingress -n kube-system
      ```
      {: pre}

    3.  Recherchez les messages d'erreur dans les journaux de l'équilibreur de charge ALB.

<br />


## Problèmes de valeur confidentielle de l'équilibreur de charge d'application Ingress
{: #cs_albsecret_fails}

{: tsSymptoms}
Une fois que vous avez déployé une valeur confidentielle d'équilibreur de charge ALB Ingress dans votre cluster, la zone `Description` n'est pas actualisée avec le nom de valeur confidentielle lorsque vous affichez votre certificat dans {{site.data.keyword.cloudcerts_full_notm}}.

Lorsque vous listez les informations sur la valeur confidentielle de l'équilibreur de charge ALB, son statut indique `*_failed` (Echec). Par exemple, `create_failed`, `update_failed`, `delete_failed`.

{: tsResolve}
Ci-dessous figurent les motifs pour lesquels la valeur confidentielle de l'équilibreur de charge ALB peut échouer, ainsi que les étapes de résolution correspondantes :

<table>
<caption>Traitement des incidents liés aux valeurs confidentielles de l'équilibreur de charge d'application Ingress</caption>
 <thead>
 <th>Motifs</th>
 <th>Procédure de résolution du problème</th>
 </thead>
 <tbody>
 <tr>
 <td>Les rôles d'accès requis pour télécharger et mettre à jour des données de certificat ne vous ont pas été attribués.</td>
 <td>Contactez l'administrateur de votre compte afin qu'il vous affecte les rôles **Opérateur** et **Editeur** sur votre instance {{site.data.keyword.cloudcerts_full_notm}}. Pour plus d'informations, voir la rubrique sur la <a href="/docs/services/certificate-manager/access-management.html#managing-service-access-roles">gestion des rôles d'accès au service</a> pour {{site.data.keyword.cloudcerts_short}}.</td>
 </tr>
 <tr>
 <td>Le CRN de certificat indiqué lors de la création, de la mise à jour ou de la suppression ne relève pas du même compte que le cluster.</td>
 <td>Vérifiez que le CRN de certificat que vous avez fourni est importé dans une instance du service {{site.data.keyword.cloudcerts_short}} déployée dans le même compte que votre cluster.</td>
 </tr>
 <tr>
 <td>Le CRN de certificat fourni lors de la création est incorrect.</td>
 <td><ol><li>Vérifiez l'exactitude de la chaîne de CRN de certificat soumise.</li><li>Si le CRN du certificat est exact, essayez de mettre à jour la valeur confidentielle : <code>bx cs alb-cert-deploy --update --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li><li>Si cette commande indique le statut <code>update_failed</code>, supprimez la valeur confidentielle : <code>bx cs alb-cert-rm --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt;</code></li><li>Déployez à nouveau la valeur confidentielle : <code>bx cs alb-cert-deploy --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li></ol></td>
 </tr>
 <tr>
 <td>Le CRN de certificat soumis lors de la mise à jour est incorrect.</td>
 <td><ol><li>Vérifiez l'exactitude de la chaîne de CRN de certificat soumise.</li><li>Si le CRN de certificat est exact, supprimez la valeur confidentielle : <code>bx cs alb-cert-rm --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt;</code></li><li>Déployez à nouveau la valeur confidentielle : <code>bx cs alb-cert-deploy --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li><li>Essayez de mettre à jour la valeur confidentielle : <code>bx cs alb-cert-deploy --update --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li></ol></td>
 </tr>
 <tr>
 <td>Le service {{site.data.keyword.cloudcerts_long_notm}} est confronté à un temps d'indisponibilité.</td>
 <td>Vérifiez que votre service {{site.data.keyword.cloudcerts_short}} est opérationnel.</td>
 </tr>
 </tbody></table>

<br />


## Impossible d'obtenir un sous-domaine pour l'équilibreur de charge d'application (ALB) Ingress
{: #cs_subnet_limit}

{: tsSymptoms}
Lorsque vous exécutez la commande `bx cs cluster-get <cluster>`, votre cluster est à l'état `normal` mais aucun **Sous-domaine Ingress** n'est disponible.

Vous pouvez obtenir un message d'erreur de ce type :

```
There are already the maximum number of subnets permitted in this VLAN.
```
{: screen}

{: tsCauses}
Lorsque vous créez un cluster, 8 sous-réseaux publics et 8 sous-réseaux privés portables sont demandés sur le réseau local virtuel (VLAN) que vous spécifiez. Pour {{site.data.keyword.containershort_notm}}, les VLAN sont limités à 40 sous-réseaux. Si le VLAN du cluster a déjà atteint cette limite, le **sous-domaine Ingress** ne peut pas être mis à disposition.

Pour afficher le nombre de sous-réseaux d'un VLAN :
1.  Dans la [console de l'infrastructure IBM Cloud (SoftLayer)](https://control.bluemix.net/), sélectionnez **Réseau** > **Gestion IP** > **VLAN**.
2.  Cliquez sur le **Numéro de VLAN** du VLAN que vous avez utilisé pour créer votre cluster. Examinez la section **Sous-réseaux** pour voir s'il existe 40 sous-réseaux ou plus.

{: tsResolve}
Si vous avez besoin d'un nouveau réseau local virtuel, commandez-le en [contactant le support {{site.data.keyword.Bluemix_notm}}](/docs/get-support/howtogetsupport.html#getting-customer-support). Ensuite, [créez un cluster](cs_cli_reference.html#cs_cluster_create) qui utilise ce nouveau VLAN.

Si vous avez un autre VLAN disponible, vous pouvez [configurer le spanning VLAN](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning) dans votre cluster existant. Vous pouvez ensuite ajouter de nouveaux noeuds worker au cluster qui utilise l'autre VLAN avec les sous-réseaux disponibles.

Si vous n'utilisez pas tous les sous-réseaux du VLAN, vous pouvez réutiliser des sous-réseaux dans le cluster.
1.  Vérifiez que les sous-réseaux que vous souhaitez utiliser sont disponibles. **Remarque** : le compte d'infrastructure que vous utilisez peut être partagé entre plusieurs comptes {{site.data.keyword.Bluemix_notm}}. Dans ce cas, même si vous exécutez la commande `bx cs subnets` pour voir les sous-réseaux avec les clusters liés (**Bound Clusters**), vous ne pourrez voir que les informations concernant vos clusters. Vérifiez avec le propriétaire du compte d'infrastructure que les sous-réseaux sont disponibles et qu'ils ne sont pas utilisés par un autre compte ou une autre équipe.

2.  [Créez un cluster](cs_cli_reference.html#cs_cluster_create) avec l'option `--no-subnet` de sorte que le service n'essaie pas de créer de nouveaux sous-réseaux. Indiquez l'emplacement et le VLAN qui contient les sous-réseaux disponibles pouvant être réutilisés.

3.  Utilisez la  [commande](cs_cli_reference.html#cs_cluster_subnet_add) `bx cs cluster-subnet-add` pour ajouter des sous-réseaux existants à votre cluster. Pour plus d'informations, voir [Ajout ou réutilisation de sous-réseaux personnalisés et existants dans les clusters Kubernetes](cs_subnets.html#custom).

<br />


## Impossible d'établir une connectivité VPN avec la charte Helm strongSwan
{: #cs_vpn_fails}

{: tsSymptoms}
Lorsque vous vérifiez la connectivité VPN en exécutant `kubectl exec -n kube-system  $STRONGSWAN_POD -- ipsec status`, vous ne voyez pas le statut `ESTABLISHED`, ou le pod du VPN est à l'état `ERROR` ou ne cesse de planter et redémarrer.

{: tsCauses}
Le fichier de configuration de votre charte Helm contient des valeurs incorrectes ou manquantes, ou comporte des erreurs de syntaxe.

{: tsResolve}
Lorsque vous essayez d'établir une connectivité VPN avec la charte Helm strongSwan, il est fort probable que le statut du VPN ne soit pas `ESTABLISHED` la première fois. Vous pourrez vérifier plusieurs types d'erreurs et modifier votre fichier de configuration en conséquence. Pour identifier et résoudre les incidents liés à la connectivité VPN strongSwan :

1. Comparez les paramètres du noeud final VPN sur site par rapport aux paramètres de votre fichier de configuration. Si ces paramètres ne correspondent pas :

    <ol>
    <li>Supprimez la charte Helm existante.</br><pre class="codeblock"><code>helm delete --purge <release_name></code></pre></li>
    <li>Corrigez les valeurs incorrectes dans le fichier <code>config.yaml</code> et sauvegardez le fichier mis à jour.</li>
    <li>Installez la nouvelle charte Helm.</br><pre class="codeblock"><code>helm install -f config.yaml --namespace=kube-system --name=<release_name> bluemix/strongswan</code></pre></li>
    </ol>

2. Si le pod VPN est à l'état d'erreur (`ERROR`) ou continue à planter et à redémarrer, cela peut être dû à une validation de paramètres dans la section `ipsec.conf` de la mappe de configuration de la charte.

    <ol>
    <li>Vérifiez les éventuelles erreurs de validation dans les journaux du pod strongSwan.</br><pre class="codeblock"><code>kubectl logs -n kube-system $STRONGSWAN_POD</code></pre></li>
    <li>S'il y a des erreurs de validation dans les journaux, supprimez la charte Helm existante.</br><pre class="codeblock"><code>helm delete --purge <release_name></code></pre></li>
    <li>Corrigez les valeurs incorrectes dans le fichier `config.yaml` et sauvegardez le fichier mis à jour.</li>
    <li>Installez la nouvelle charte Helm.</br><pre class="codeblock"><code>helm install -f config.yaml --namespace=kube-system --name=<release_name> bluemix/strongswan</code></pre></li>
    </ol>

3. Exécutez les 5 tests Helm inclus dans la définition de la charte strongSwan.

    <ol>
    <li>Exécutez les tests Helm.</br><pre class="codeblock"><code>helm test vpn</code></pre></li>
    <li>En cas d'échec d'un test, référez-vous à la rubrique [Description des tests Helm de connectivité VPN](cs_vpn.html#vpn_tests_table) pour obtenir des informations sur chaque test et les causes possibles de leur échec. <b>Remarque</b> : certains de ces tests ont des conditions requises qui font partie des paramètres facultatifs dans la configuration du VPN. En cas d'échec de certains tests, les erreurs peuvent être acceptables si vous avez indiqué ces paramètres facultatifs.</li>
    <li>Affichez la sortie d'un test ayant échoué en consultant les journaux du pod de test.<br><pre class="codeblock"><code>kubectl logs -n kube-system <test_program></code></pre></li>
    <li>Supprimez la charte Helm existante.</br><pre class="codeblock"><code>helm delete --purge <release_name></code></pre></li>
    <li>Corrigez les valeurs incorrectes dans le fichier <code>config.yaml</code> et sauvegardez le fichier mis à jour.</li>
    <li>Installez la nouvelle charte Helm.</br><pre class="codeblock"><code>helm install -f config.yaml --namespace=kube-system --name=<release_name> bluemix/strongswan</code></pre></li>
    <li>Pour vérifier vos modifications :<ol><li>Récupérez les pods de test actuels.</br><pre class="codeblock"><code>kubectl get pods -a -n kube-system -l app=strongswan-test</code></pre></li><li>Nettoyez les pods du test en cours.</br><pre class="codeblock"><code>kubectl delete pods -n kube-system -l app=strongswan-test</code></pre></li><li>Réexécutez les tests.</br><pre class="codeblock"><code>helm test vpn</code></pre></li>
    </ol></ol>

4. Exécutez l'outil de débogage VPN qui est inclus dans l'image du pod VPN.

    1. Définissez la variable d'environnement `STRONGSWAN_POD`.

        ```
        export STRONGSWAN_POD=$(kubectl get pod -n kube-system -l app=strongswan,release=vpn -o jsonpath='{ .items[0].metadata.name }')
        ```
        {: pre}

    2. Exécutez l'outil de débogage.

        ```
        kubectl exec -n kube-system  $STRONGSWAN_POD -- vpnDebug
        ```
        {: pre}

        L'outil renvoie plusieurs pages d'informations à mesure qu'il exécute différents tests pour détecter les problèmes réseau courants. Les lignes de sortie commençant par `ERROR`, `WARNING`, `VERIFY` ou `CHECK` indiquent des erreurs possibles liées à la connectivité VPN.

    <br />


## La connectivité VPN strongSwan échoue après l'ajout ou la suppression d'un noeud worker
{: #cs_vpn_fails_worker_add}

{: tsSymptoms}
Vous avez déjà établi une connexion VPN opérationnelle en utilisant le service VPN IPSec strongSwan. Cependant, après avoir ajouté ou supprimé un noeud worker sur votre cluster, vous expérimentez un ou plusieurs symptômes de ce type :

* Vous n'obtenez pas le statut VPN `ESTABLISHED`
* Vous ne parvenez pas à accéder aux nouveaux noeuds worker à partir de votre réseau local
* Vous ne pouvez pas accéder au réseau distant à partir des pods qui s'exécutent sur les nouveaux noeuds worker

{: tsCauses}
Si vous avez ajouté un noeud worker :

* Le noeud worker a été mis en place sur un nouveau sous-réseau privé qui n'est pas exposé via la connexion VPN avec vos paramètres `localSubnetNAT` ou `local.subnet`
* Les routes VPN ne peuvent pas être ajoutées au noeud worker car celui-ci possède des annotations taint ou des étiquettes qui ne sont pas incluses dans vos paramètres `tolerations` ou `nodeSelector`
* Le pod VPN s'exécute sur le nouveau noeud worker, mais l'adresse IP publique de ce noeud n'est pas autorisée via le pare-feu local

Si vous avez supprimé un noeud worker :

* Ce noeud worker constituait le seul noeud sur lequel un pod VPN s'exécutait, en raison des restrictions relatives à certaines annotations taint ou étiquettes dans vos paramètres `tolerations` ou `nodeSelector`

{: tsResolve}
Mettez à jour les valeurs de la charte Helm pour répercuter les modifications du noeud worker :

1. Supprimez la charte Helm existante.

    ```
    helm delete --purge <release_name>
    ```
    {: pre}

2. Ouvrez le fichier de configuration correspondant au service VPN strongSwan.

    ```
    helm inspect values ibm/strongswan > config.yaml
    ```
    {: pre}

3. Vérifiez les paramètres suivants et modifiez les paramètres pour répercuter les noeuds worker ajoutés ou supprimés selon les besoins.

    Si vous avez ajouté un noeud worker :

    <table>
    <caption>Paramètres du noeud worker</caption>
     <thead>
     <th>Paramètre</th>
     <th>Description</th>
     </thead>
     <tbody>
     <tr>
     <td><code>localSubnetNAT</code></td>
     <td>Le noeud worker ajouté peut être déployé sur un nouveau sous-réseau privé différent des autres sous-réseaux existants sur lesquels résident les autres noeuds worker. Si vous utilisez la conversion NAT de sous-réseau pour remapper les adresses IP locales privées de votre cluster et que le noeud worker a été ajouté sur un nouveau sous-réseau, ajoutez le CIDR du nouveau sous-réseau à ce paramètre.</td>
     </tr>
     <tr>
     <td><code>nodeSelector</code></td>
     <td>Si vous avez précédemment limité le déploiement du pod VPN aux noeuds worker avec un libellé spécifique, vérifiez que le noeud worker ajouté comporte également ce libellé.</td>
     </tr>
     <tr>
     <td><code>tolerations</code></td>
     <td>Si le noeud worker ajouté comporte des annotations taint, modifiez ce paramètre pour autoriser le pod VPN à s'exécuter sur tous les noeuds worker avec des annotations taint ou ayant des annotations taint spécifiques.</td>
     </tr>
     <tr>
     <td><code>local.subnet</code></td>
     <td>Le noeud worker ajouté peut être déployé sur un nouveau sous-réseau privé différent des sous-réseaux existants sur lesquels résident les autres noeuds worker. Si vos applications sont exposées par les services NodePort or LoadBalancer sur le réseau privé et qu'elles sont sur le noeud worker ajouté, ajoutez le nouveau CIDR du sous-réseau à ce paramètre. **Remarque** : si vous ajoutez des valeurs au paramètre `local.subnet`, vérifiez les paramètres VPN du sous-réseau local pour voir s'ils doivent également faire l'objet d'une mise à jour.</td>
     </tr>
     </tbody></table>

    Si vous avez supprimé un noeud worker :

    <table>
    <caption>Paramètres du noeud worker</caption>
     <thead>
     <th>Paramètre</th>
     <th>Description</th>
     </thead>
     <tbody>
     <tr>
     <td><code>localSubnetNAT</code></td>
     <td>Si vous utilisez la conversion NAT de sous-réseau pour remapper des adresses IP locales privées spécifiques, supprimez de ce paramètre les adresses IP provenant de l'ancien noeud worker. Si vous utilisez la conversion NAT de sous-réseau pour remapper des sous-réseaux entiers et qu'aucun noeud worker n'est présent sur un sous-réseau, supprimez le CIDR de sous-réseau de ce paramètre.</td>
     </tr>
     <tr>
     <td><code>nodeSelector</code></td>
     <td>Si vous avez limité le pod VPN de sorte pour qu'il s'exécute sur un noeud worker unique et que ce noeud a été supprimé, modifiez ce paramètre pour autoriser le pod VPN à s'exécuter sur d'autres noeuds worker.</td>
     </tr>
     <tr>
     <td><code>tolerations</code></td>
     <td>Si le noeud worker que vous avez supprimé n'avait pas d'annotation taint, mais que seuls les noeuds restants ont des annotations taint, modifiez ce paramètre pour autoriser le pod VPN à s'exécuter sur tous les noeuds comportant des annotations taint ou des annotations taint spécifiques.
     </td>
     </tr>
     </tbody></table>

4. Installez la nouvelle charte Helm avec vos valeurs mises à jour.

    ```
    helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/strongswan
    ```
    {: pre}

5. Vérifiez le statut de déploiement de la charte. Lorsque la charte est prête, la zone **STATUS** vers le haut de la sortie indique `DEPLOYED`.

    ```
    helm status <release_name>
    ```
    {: pre}

6. Dans certains cas, il vous faudra changer vos paramètres locaux et vos paramètres de pare-feu pour les adapter aux modifications que vous avez apportées au fichier de configuration VPN.

7. Démarrez le VPN.
    * Si la connexion VPN est initialisée par le cluster (`ipsec.auto` est défini sur `start`), démarrez le VPN sur la passerelle locale, puis démarrez le VPN sur le cluster.
    * Si la connexion VPN est initialisée par la passerelle locale (`ipsec.auto` est défini sur `auto`), démarrez le VPN sur le cluster, puis démarrez le VPN sur la passerelle locale.

8. Définissez la variable d'environnement `STRONGSWAN_POD`.

    ```
    export STRONGSWAN_POD=$(kubectl get pod -n kube-system -l app=strongswan,release=<release_name> -o jsonpath='{ .items[0].metadata.name }')
    ```
    {: pre}

9. Vérifiez le statut du réseau privé virtuel.

    ```
    kubectl exec -n kube-system  $STRONGSWAN_POD -- ipsec status
    ```
    {: pre}

    * Si la connexion VPN a le statut `ESTABLISHED`, la connexion a abouti. Aucune autre action n'est requise.

    * Si vous rencontrez toujours des problèmes de connexion, voir [Impossible d'établir une connectivité VPN avec la charte Helm strongSwan](#cs_vpn_fails) pour continuer à résoudre les problèmes de connexion VPN.

<br />



## Récupération impossible des règles réseau Calico
{: #cs_calico_fails}

{: tsSymptoms}
Lorsque vous essayez d'afficher les règles réseau Calico dans votre cluster en exécutant la commande `calicoctl get policy`, vous obtenez l'un des résultats imprévisibles ou messages d'erreur suivants :
- Une liste vide
- Une liste de règles Calico v2 au lieu de règles v3
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'projectcalico.org/v3'`

Lorsque vous essayez d'afficher les règles réseau Calico dans votre cluster en exécutant la commande `calicoctl get GlobalNetworkPolicy`, vous obtenez l'un des résultats imprévisibles ou messages d'erreur suivants :
- Une liste vide
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'v1'`
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'projectcalico.org/v3'`
- `Failed to get resources: Resource type 'GlobalNetworkPolicy' is not supported`

{: tsCauses}
Pour utiliser des règles Calico, quatre facteurs doivent être en phase : la version de votre cluster Kubernetes, la version de l'interface de ligne de commande (CLI) Calico, la syntaxe du fichier de configuration Calico et les commandes d'affichage des règles. Au moins un de ces facteurs n'est pas à la version correcte.

{: tsResolve}
Lorsque la version de votre cluster correspond à [Kubernetes version 1.10 ou ultérieure](cs_versions.html), vous devez utiliser l'interface CLI de Calico v3.1, la syntaxe du fichier de configuration `calicoctl.cfg` v3 et les commandes `calicoctl get GlobalNetworkPolicy` et `calicoctl get NetworkPolicy`.

Lorsque la version de votre cluster correspond à [Kubernetes version 1.9 ou antérieure](cs_versions.html), vous devez utiliser l'interface CLI de Calico v1.6.3, la syntaxe du fichier de configuration `calicoctl.cfg` v2 et la commande `calicoctl get policy`.

Pour vous assurer que tous les facteurs Calico sont en phase :

1. Affichez la version de votre cluster Kubernetes.
    ```
    bx cs cluster-get <cluster_name>
    ```
    {: pre}

    * Si la version de votre cluster correspond à Kubernetes version 1.10 ou ultérieure :
        1. [Installez et configurez l'interface CLI de Calico version 3.1.1](cs_network_policy.html#1.10_install). La configuration comprend la mise à jour manuelle du fichier `calicoctl.cfg` pour utiliser la syntaxe de Calico v3.
        2. Vérifiez que les règles que vous créez et que vous voulez appliquer à votre cluster utilisent la [syntaxe de Calico v3 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/networkpolicy). Si vous disposez d'un fichier `.yaml` de rège existant ou d'un fichier `.json` avec la syntaxe de Calico v2, vous pouvez le convertir en syntaxe de Calico v3 en utilisant la commande [`calicoctl convert` ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.projectcalico.org/v3.1/reference/calicoctl/commands/convert).
        3. Pour [afficher les règles](cs_network_policy.html#1.10_examine_policies), vérifiez que vous utilisez la commande `calicoctl get GlobalNetworkPolicy` pour les règles globales et `calicoctl get NetworkPolicy --namespace <policy_namespace>` pour les règles limitées à des espaces de nom spécifiques.

    * Si la version de votre cluster correspond à Kubernetes version 1.9 ou antérieure :
        1. [Installez et configurez l'interface CLI de Calico version 1.6.3](cs_network_policy.html#1.9_install). Vérifiez que le fichier `calicoctl.cfg` utilise la syntaxe de Calico v2.
        2. Vérifiez que les règles que vous créez et que vous voulez appliquer à votre cluster utilisent la [syntaxe de Calico v2 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://docs.projectcalico.org/v2.6/reference/calicoctl/resources/policy). 
        3. Pour [afficher les règles](cs_network_policy.html#1.9_examine_policies), vérifiez que vous utilisez la commande `calicoctl get policy`.

Avant de mettre à jour votre cluster de la version Kubernetes 1.9 ou antérieure à la version 1.10 ou ultérieure, consultez la rubrique [Préparation à la mise à jour vers Calico v3](cs_versions.html#110_calicov3).
{: tip}

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

