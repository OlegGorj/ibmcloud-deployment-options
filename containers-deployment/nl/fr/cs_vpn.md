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


# Configuration de la connectivité VPN
{: #vpn}

La connectivité VPN vous permet de connecter de manière sécurisée les applications d'un cluster Kubernetes sur {{site.data.keyword.containerlong}} à un réseau sur site. Vous pouvez également connecter des applications externes au cluster à une application s'exécutant au sein de votre cluster.
{:shortdesc}

Pour connecter vos noeuds worker et vos applications à un centre de données sur site, vous pouvez configurer l'une des options suivantes.

- **Service VPN IPSec strongSwan** : vous pouvez définir un [service VPN IPSec strongSwan ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.strongswan.org/) connectant de manière sécurisée votre cluster Kubernetes avec un réseau sur site. Le service VPN IPSec strongSwan fournit un canal de communication de bout en bout sécurisé sur Internet basé sur la suite de protocoles IPSec (Internet Protocol Security) aux normes du l'industrie. Pour configurer une connexion sécurisée entre votre cluster et un réseau sur site, [configurez et déployez le service VPN IPSec strongSwan](#vpn-setup) directement dans un pod de votre cluster.

- **Dispositif de routeur virtuel (VRA) ou dispositif de sécurité Fortigate (FSA)** : vous pouvez choisir de configurer un dispositif [VRA](/docs/infrastructure/virtual-router-appliance/about.html) ou [FSA](/docs/infrastructure/fortigate-10g/about.html) pour configurer un noeud final VPN IPSec. Cette option est pratique si vous disposez d'un cluster plus volumineux, si vous souhaitez accéder à d'autres ressources que celles de Kubernetes via le réseau privé virtuel (VPN) ou accéder à plusieurs clusters via un seul VPN. Pour configurer un dispositif de routeur virtuel (VRA), voir la rubrique sur la [configuration d'une connectivité VPN avec un dispositif VRA](#vyatta).

## Configuration de la connectivité VPN avec la charte Helm du service VPN IPsec strongSwan
{: #vpn-setup}

Utilisez une charte Helm pour configurer et déployer le service VPN IPsec strongSwan à l'intérieur d'un pod Kubernetes.
{:shortdesc}

Etant donné que strongSwan est intégré dans votre cluster, vous n'avez pas besoin d'un dispositif de passerelle externe. Lorsque la connectivité VPN est établie, des routes sont automatiquement configurées sur tous les noeuds worker dans le cluster. Ces routes permettent d'établir une connectivité bidirectionnelle via le tunnel VPN entre les pods d'un noeud worker et le système distant. Par exemple, le diagramme suivant illustre comment une application dans {{site.data.keyword.containershort_notm}} peut communiquer avec un serveur sur site par le biais d'une connexion VPN strongSwan :

<img src="images/cs_vpn_strongswan.png" width="700" alt="Exposition d'une application dans {{site.data.keyword.containershort_notm}} à l'aide d'un équilibreur de charge" style="width:700px; border-style: none"/>

1. Une application dans votre cluster nommée `myapp` reçoit une demande d'un service Ingress ou LoadBalancer et nécessite une connexion sécurisée aux données de votre réseau sur site.

2. La demande effectuée auprès du centre de données sur site est transmise au pod VPN strongSwan IPSec. L'adresse IP de destination est utilisée pour déterminer quels sont les paquets réseau à envoyer au pod VPN strongSwan IPSec.

3. La demande est chiffrée et envoyée via le tunnel VPN au centre de données sur site.

4. La demande entrante passe par le pare-feu local et est distribuée sur le noeud final du tunnel VPN (routeur) où elle est déchiffrée.

5. Le noeud final du tunnel VPN (routeur) transfère la demande au serveur sur site ou au mainframe, en fonction de l'adresse IP de destination qui a été indiquée à l'étape 2. Les données nécessaires sont renvoyées via la connexion VPN à `myapp` en utilisant le même processus.

### Configuration de la charte Helm strongSwan
{: #vpn_configure}

Avant de commencer :
* [Installez une passerelle VPN IPsec dans votre centre de données sur site](/docs/infrastructure/iaas-vpn/set-up-ipsec-vpn.html#setting-up-an-ipsec-connection).
* Vous avez le choix entre [créer un cluster standard](cs_clusters.html#clusters_cli) ou [mettre à jour un cluster existant à la version 1.7.16 ou ultérieure](cs_cluster_update.html#master).
* Le cluster doit disposer au moins d'une adresse IP d'équilibreur de charge publique disponible. [Vous pouvez vérifier quelles sont vos adresses IP publiques disponibles](cs_subnets.html#manage) ou [libérer une adresse IP utilisée](cs_subnets.html#free).
* [Ciblez l'interface CLI de Kubernetes sur le cluster](cs_cli_install.html#cs_cli_configure).

Pour plus d'informations sur les commandes Helm utilisées pour configurer la charte strongSwan, reportez-vous à la <a href="https://docs.helm.sh/helm/" target="_blank">documentation Helm <img src="../icons/launch-glyph.svg" alt="Icône de lien externe"></a>.

Pour configurer la charte Helm :

1. [Installez Helm pour votre cluster et ajoutez le référentiel {{site.data.keyword.Bluemix_notm}} dans votre instance Helm](cs_integrations.html#helm).

2. Sauvegardez les paramètres de configuration par défaut pour la charte Helm strongSwan dans un fichier YAML local.

    ```
    helm inspect values ibm/strongswan > config.yaml
    ```
    {: pre}

3. Ouvrez le fichier `config.yaml` et apportez les modifications suivantes aux valeurs par défaut en fonction de la configuration VPN souhaitée. Vous pouvez trouver la description des paramètres plus avancés dans les commentaires du fichier de configuration.

    **Important** : si vous n'avez pas besoin de modifier une propriété, mettez celle-ci en commentaire en la précédant du signe `#`.

    <table>
    <caption>Description des composants de ce fichier YAML</caption>
    <col width="22%">
    <col width="78%">
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants du fichier YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code>localSubnetNAT</code></td>
    <td>La conversion d'adresses réseau (NAT) pour les sous-réseaux fournit une solution palliative en cas de conflits de sous-réseaux entre les réseaux locaux et sur site. Vous pouvez utiliser NAT pour remapper les sous-réseaux IP locaux privés du cluster, le sous-réseau du pod (172.30.0.0/16) ou le sous-réseau du service de pod (172.21.0.0/16) vers un autre sous-réseau privé. Le tunnel VPN voit les sous-réseaux IP remappés au lieu des sous-réseaux d'origine. Le remappage intervient avant l'envoi des paquets via le tunnel VPN et après l'arrivée des paquets en provenance du tunnel VPN. Vous pouvez exposer les sous-réseaux remappés et non remappés en même temps via le VPN.<br><br>Pour activer la conversion NAT, vous pouvez ajouter un sous-réseau complet ou des adresses IP individuelles. Si vous ajoutez un sous-réseau complet (au format <code>10.171.42.0/24=10.10.10.0/24</code>), le remappage s'effectue en mode 1 à 1 : toutes les adresses IP du sous-réseau du réseau interne sont mappées au sous-réseau du réseau externe et inversement. Si vous ajoutez des adresses IP individuelles (au format <code>10.171.42.17/32=10.10.10.2/32, 10.171.42.29/32=10.10.10.3/32</code>), seules ces adresses IP internes sont mappées aux adresses IP externes spécifiées.<br><br>Si vous utilisez cette option, le sous-réseau local exposé via la connexion VPN est le réseau "externe" auquel est mappé le réseau "interne".
    </td>
    </tr>
    <tr>
    <td><code>loadBalancerIP</code></td>
    <td>Si vous souhaitez spécifier une adresse IP publique portable pour le service VPN strongSwan pour les connexions VPN entrantes, ajoutez cette adresse IP. La spécification d'une adresse IP est utile lorsque vous avez besoin d'une adresse IP stable, par exemple lorsque vous devez désigner quelles sont les adresses IP autorisées via un pare-feu local.<br><br>Pour afficher les adresses IP publiques portables affectées à ce cluster, voir [Gestion d'adresses IP et de sous-réseaux](cs_subnets.html#manage). Si vous laissez ce paramètre vide, une adresse IP publique portable disponible est utilisée. Si la connexion VPN est initiée depuis la passerelle sur site (<code>ipsec.auto</code> est défini avec la valeur <code>add</code>), vous pouvez utiliser cette propriété pour configurer une adresse IP publique persistante sur la passerelle sur site pour le cluster.</td>
    </tr>
    <tr>
    <td><code>connectUsingLoadBalancerIP</code></td>
    <td>Utilisez l'adresse IP de l'équilibreur de charge que vous avez ajoutée dans <code>loadBalancerIP</code> pour établir également la connexion VPN sortante. Si cette option est activée, tous les noeuds worker du cluster doivent figurer sur le même VLAN public. Sinon, vous devez utiliser le paramètre <code>nodeSelector</code> pour vous assurer que le pod VPN se déploie sur un noeud worker du même VLAN public que l'adresse IP <code>loadBalancerIP</code>. Cette option est ignorée si <code>ipsec.auto</code> est défini avec <code>add</code>.<p>Valeurs admises :</p><ul><li><code>"false"</code> : ne pas connecter le VPN en utilisant l'adresse IP de l'équilibreur de charge. L'adresse IP publique du noeud worker sur lequel s'exécute le pod VPN est utilisée à la place.</li><li><code>"true"</code> : établir le VPN en utilisant l'adresse IP de l'équilibreur de charge comme adresse IP source locale. Si l'adresse IP de l'équilibreur de charge (<code>loadBalancerIP</code>) n'est pas définie, l'adresse IP externe qui est affectée au service d'équilibreur de charge est utilisée.</li><li><code>"auto"</code> : lorsque le paramètre <code>ipsec.auto</code> est défini avec <code>start</code> et que le paramètre <code>loadBalancerIP</code> est défini, établissez le VPN en utilisant l'adresse IP de l'équilibreur de charge comme adresse IP source locale.</li></ul></td>
    </tr>
    <tr>
    <td><code>nodeSelector</code></td>
    <td>Pour limiter les noeuds utilisés par le pod VPN strongSwan pour le déploiement, ajoutez l'adresse IP d'un noeud worker spécifique ou le libellé d'un noeud worker. Par exemple, la valeur <code>kubernetes.io/hostname: 10.xxx.xx.xxx</code> limite l'exécution du pod VPN uniquement à ce noeud worker. La valeur <code>strongswan: vpn</code> limite l'exécution du pod aux noeuds worker ayant ce libellé. Vous pouvez utiliser n'importe quel libellé de noeud, mais il est recommandé d'utiliser : <code>strongswan: &lt;release_name&gt;</code> pour que différents noeuds worker puissent être utilisés avec différents déploiements de cette charte.<br><br>Si la connexion VPN est initiée par le cluster (<code>ipsec.auto</code> est défini sur <code>start</code>), vous pouvez utiliser cette propriété pour limiter les adresses IP source de la connexion VPN exposées à la passerelle sur site. Cette valeur est facultative.</td>
    </tr>
    <tr>
    <td><code>ipsec.keyexchange</code></td>
    <td>Si votre noeud final de tunnel VPN sur site ne prend pas en charge <code>ikev2</code> comme protocole d'initialisation de la connexion, remplacez cette valeur par <code>ikev1</code> ou <code>ike</code>.</td>
    </tr>
    <tr>
    <td><code>ipsec.esp</code></td>
    <td>Ajoutez la liste des algorithmes de chiffrement et d'authentification ESP utilisés par votre noeud final de tunnel VPN pour la connexion.<ul><li>Si <code>ipsec.keyexchange</code> est défini avec <code>ikev1</code>, ce paramètre doit être spécifié.</li><li>Si <code>ipsec.keyexchange</code> est défini avec <code>ikev2</code>, ce paramètre est facultatif. Si vous laissez ce paramètre vide, les algorithmes strongSwan par défaut <code>aes128-sha1,3des-sha1</code> sont utilisés pour la connexion.</li></ul></td>
    </tr>
    <tr>
    <td><code>ipsec.ike</code></td>
    <td>Ajoutez la liste des algorithmes de chiffrement et d'authentification IKE/ISAKMP SA utilisés par votre noeud final de tunnel VPN pour la connexion.<ul><li>Si <code>ipsec.keyexchange</code> est défini avec <code>ikev1</code>, ce paramètre doit être spécifié.</li><li>Si <code>ipsec.keyexchange</code> est défini avec <code>ikev2</code>, ce paramètre est facultatif. Si vous laissez ce paramètre vide, les algorithmes strongSwan par défaut <code>aes128-sha1-modp2048,3des-sha1-modp1536</code> sont utilisés pour la connexion.</li></ul></td>
    </tr>
    <tr>
    <td><code>ipsec.auto</code></td>
    <td>Si vous désirez que le cluster initialise la connexion VPN, remplacez cette valeur par <code>start</code>.</td>
    </tr>
    <tr>
    <td><code>local.subnet</code></td>
    <td>Remplacez cette valeur par la liste des CIDR de sous-réseau de cluster à exposer sur la connexion VPN avec le réseau local. Cette liste peut inclure les sous-réseaux suivants : <ul><li>CIDR de sous-réseau du pod Kubernetes : <code>172.30.0.0/16</code></li><li>CIDR de sous-réseau du service Kubernetes : <code>172.21.0.0/16</code></li><li>Si vos applis sont exposées par un service NodePort sur le réseau privé, CIDR de sous-réseau privé du noeud worker. Récupérez les trois premiers octets de l'adresse IP privée de votre noeud worker en exécutant la commande <code>bx cs worker &lt;cluster_name&gt;</code>. Par exemple, pour l'adresse IP <code>&lt;10.176.48.xx&gt;</code>, notez <code>&lt;10.176.48&gt;</code>. Obtenez ensuite, le CIDR de sous-réseau privé du noeud worker à l'aide de la commande suivante en remplaçant <code>&lt;xxx.yyy.zz&gt;</code> par l'octet que vous avez récupéré précédemment : <code>bx cs subnets | grep &lt;xxx.yyy.zzz&gt;</code>.</li><li>Si vous disposez d'applis exposées par des services LoadBalancer sur le réseau privé, CIDR de sous-réseau privé ou géré par l'utilisateur du cluster. Pour identifier ces valeurs, exécutez la commande <code>bx cs cluster-get &lt;cluster_name&gt; --showResources</code>. Dans la section **VLANS**, recherchez des CIDR indiquant <code>false</code> pour **Public**.</li></ul>**Remarque** : si <code>ipsec.keyexchange</code> est défini avec <code>ikev1</code>, vous ne pouvez indiquer qu'un seul sous-réseau.</td>
    </tr>
    <tr>
    <td><code>local.id</code></td>
    <td>Remplacez cette valeur par l'identificateur de chaîne côté cluster Kubernetes local utilisé par votre noeud final de tunnel VPN pour la connexion.</td>
    </tr>
    <tr>
    <td><code>remote.gateway</code></td>
    <td>Remplacez cette valeur par l'adresse IP publique de la passerelle VPN sur site. Lorsque <code>ipsec.auto</code> est défini sur <code>start</code>, cette valeur est obligatoire.</td>
    </tr>
    <td><code>remote.subnet</code></td>
    <td>Remplacez cette valeur par la liste des CIDR de sous-réseau privé sur site auxquels les clusters Kubernetes sont autorisés à accéder. **Remarque** : si <code>ipsec.keyexchange</code> est défini avec <code>ikev1</code>, vous ne pouvez indiquer qu'un seul sous-réseau.</td>
    </tr>
    <tr>
    <td><code>remote.id</code></td>
    <td>Remplacez cette valeur par l'identificateur de chaîne côté sur site distant que votre noeud final de tunnel VPN utilise pour la connexion.</td>
    </tr>
    <tr>
    <td><code>remote.privateIPtoPing</code></td>
    <td>Ajoutez l'adresse IP privée dans le sous-réseau distant à utiliser avec les programmes de validation de test Helm pour les tests de connectivité avec ping VPN. Cette valeur est facultative.</td>
    </tr>
    <tr>
    <td><code>preshared.secret</code></td>
    <td>Remplacez cette valeur par la valeur confidentielle pré-partagée que votre passerelle de noeud final de tunnel VPN utilise pour la connexion. Cette valeur est stockée dans <code>ipsec.secrets</code>.</td>
    </tr>
    </tbody></table>

4. Enregistrez le fichier `config.yaml` mis à jour.

5. Installez la charte Helm dans votre cluster avec le fichier `config.yaml` mis à jour. Les propriétés mises à jour sont stockées dans un fichier configmap de votre charte.

    **Remarque** : si vous avez plusieurs déploiements VPN dans un seul cluster, vous pouvez éviter les conflits de noms et effectuer la distinction entre différents déploiements en choisissant des noms d'édition plus descriptifs que `vpn`. Pour éviter que le nom d'édition soit tronqué, limitez-le à 35 caractères ou moins.

    ```
    helm install -f config.yaml --name=vpn ibm/strongswan
    ```
    {: pre}

6. Vérifiez le statut de déploiement de la charte Helm. Lorsque la charte est prête, la zone **STATUS** vers le haut de la sortie a la valeur `DEPLOYED`.

    ```
    helm status vpn
    ```
    {: pre}

7. Une fois la charte Helm déployée, vérifiez que les paramètres mis à jour dans le fichier `config.yaml` ont été utilisés.

    ```
    helm get values vpn
    ```
    {: pre}


### Test et vérification de la connectivité VPN
{: #vpn_test}

Après avoir déployé la charte Helm, testez la connectivité VPN.
{:shortdesc}

1. Si le réseau privé virtuel (VPN) sur la passerelle sur site n'est pas actif, démarrez-le.

2. Définissez la variable d'environnement `STRONGSWAN_POD`.

    ```
    export STRONGSWAN_POD=$(kubectl get pod -l app=strongswan,release=vpn -o jsonpath='{ .items[0].metadata.name }')
    ```
    {: pre}

3. Vérifiez le statut du réseau privé virtuel. Un statut `ESTABLISHED` indique que la connexion VPN a abouti.

    ```
    kubectl exec $STRONGSWAN_POD -- ipsec status
    ```
    {: pre}

    Exemple de sortie :

    ```
    Security Associations (1 up, 0 connecting):
    k8s-conn[1]: ESTABLISHED 17 minutes ago, 172.30.xxx.xxx[ibm-cloud]...192.xxx.xxx.xxx[on-premises]
    k8s-conn{2}: INSTALLED, TUNNEL, reqid 12, ESP in UDP SPIs: c78cb6b1_i c5d0d1c3_o
    k8s-conn{2}: 172.21.0.0/16 172.30.0.0/16 === 10.91.152.xxx/26
    ```
    {: screen}

    **Remarque **:

    <ul>
    <li>Lorsque vous tentez d'établir la connectivité VPN avec la charte Helm strongSwan, il est probable que la première fois, le statut du VPN ne soit pas `ESTABLISHED`. Vous aurez peut-être besoin de vérifier les paramètres du noeud final VPN sur site et de modifier le fichier de configuration plusieurs fois avant d'établir une connexion opérationnelle : <ol><li>Exécutez la commande `helm delete --purge <release_name>`</li><li>Corrigez les valeurs incorrectes dans le fichier de configuration.</li><li>Exécutez la commande `helm install -f config.yaml --name=<release_name> ibm/strongswan`</li></ol>Vous pouvez également exécuter d'autres vérifications à l'étape suivante.</li>
    <li>Si le pod VPN est à l'état d'erreur (`ERROR`) ou continue à planter et à redémarrer, cela peut être dû à une validation de paramètres dans la section `ipsec.conf` du fichier configmap de la charte.<ol><li>Recherchez les erreurs de validation éventuelles dans les journaux du pod strongSwan en exécutant la commande `kubectl logs -n $STRONGSWAN_POD`.</li><li>En cas d'erreurs de validation, exécutez la commande `helm delete --purge <release_name>`<li>Corrigez les valeurs incorrectes dans le fichier de configuration.</li><li>Exécutez la commande `helm install -f config.yaml --name=<release_name> ibm/strongswan`</li></ol>Si votre cluster comporte un grand nombre de noeuds worker, vous pouvez également utiliser la commande `helm upgrade` pour appliquer plus rapidement vos modifications au lieu d'exécuter les commandes `helm delete` et `helm install`.</li>
    </ul>

4. Vous pouvez continuer à tester la connectivité VPN en exécutant les cinq tests Helm inclus dans la définition de la charte strongSwan.

    ```
    helm test vpn
    ```
    {: pre}

    * Si tous ces tests ont réussi, la connexion VPN strongSwan est configurée correctement.

    * Si l'un des tests a échoué, passez à l'étape suivante.

5. Affichez la sortie d'un test ayant échoué en consultant les journaux du pod de test.

    ```
    kubectl logs <test_program>
    ```
    {: pre}

    **Remarque** : certains de ces tests ont des conditions requises qui font partie des paramètres facultatifs dans la configuration du VPN. En cas d'échec de certains tests, les erreurs peuvent être acceptables si vous avez indiqué ces paramètres facultatifs. Consultez le tableau suivant pour obtenir des informations sur chaque test et les raisons d'échec possible pour chacun d'eux.

    {: #vpn_tests_table}
    <table>
    <caption>Description des tests Helm de connectivité VPN</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des tests Helm de connectivité VPN</th>
    </thead>
    <tbody>
    <tr>
    <td><code>vpn-strongswan-check-config</code></td>
    <td>Valide la syntaxe du fichier <code>ipsec.conf</code> généré à partir du fichier <code>config.yaml</code>. Ce test peut échouer en raison de valeurs incorrectes dans le fichier <code>config.yaml</code>.</td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-check-state</code></td>
    <td>Vérifie que le statut de la connexion VPN est <code>ESTABLISHED</code>. Ce test peut échouer pour les raisons suivantes :<ul><li>Différences entre les valeurs du fichier <code>config.yaml</code> et les paramètres du noeud final VPN sur site.</li><li>Si le cluster est en mode "écoute" (<code>ipsec.auto</code> est défini sur <code>add</code>), la connexion n'est pas établie du côté sur site.</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-gw</code></td>
    <td>Exécute une commande ping sur l'adresse IP publique de <code>remote.gateway</code> que vous avez configurée dans le fichier <code>config.yaml</code>. Ce test peut échouer pour les raisons suivantes :<ul><li>Vous n'avez pas indiqué d'adresse IP de passerelle VPN sur site. Si <code>ipsec.auto</code> est défini sur <code>start</code>, l'adresse IP indiquée dans <code>remote.gateway</code> est obligatoire.</li><li>La connexion VPN n'a pas le statut <code>ESTABLISHED</code>. Voir <code>vpn-strongswan-check-state</code> pour plus d'informations.</li><li>La connectivité VPN est établie (<code>ESTABLISHED</code>), mais les paquets ICMP sont bloqués par un pare-feu.</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-ip-1</code></td>
    <td>Exécute une commande ping sur l'adresse IP privée de <code>remote.privateIPtoPing</code> de la passerelle VPN sur site à partir du pod VPN dans le cluster. Ce test peut échouer pour les raisons suivantes :<ul><li>Vous n'avez pas indiqué d'adresse IP pour <code>remote.privateIPtoPing</code>. Si vous ne l'avez pas indiqué volontairement, cet échec est acceptable.</li><li>Vous n'avez pas indiqué de CIDR de sous-réseau de pod de cluster, <code>172.30.0.0/16</code>, dans la liste <code>local.subnet</code>.</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-ip-2</code></td>
    <td>Exécute une commande ping sur l'adresse IP privée de <code>remote.privateIPtoPing</code> de la passerelle VPN sur site à partir du noeud worker dans le cluster. Ce test peut échouer pour les raisons suivantes :<ul><li>Vous n'avez pas indiqué d'adresse IP pour <code>remote.privateIPtoPing</code>. Si vous ne l'avez pas indiqué volontairement, cet échec est acceptable.</li><li>Vous n'avez pas indiqué le CIDR de sous-réseau privé du noeud worker du cluster dans la liste <code>local.subnet</code>.</li></ul></td>
    </tr>
    </tbody></table>

6. Supprimez la charte Helm en cours.

    ```
    helm delete --purge vpn
    ```
    {: pre}

7. Ouvrez le fichier de configuration `config.yaml` et corrigez les valeurs incorrectes.

8. Enregistrez le fichier `config.yaml` mis à jour.

9. Installez la charte Helm dans votre cluster avec le fichier `config.yaml` mis à jour. Les propriétés mises à jour sont stockées dans un fichier configmap de votre charte.

    ```
    helm install -f config.yaml --name=<release_name> ibm/strongswan
    ```
    {: pre}

10. Vérifiez le statut de déploiement de la charte Helm. Lorsque la charte est prête, la zone **STATUS** vers le haut de la sortie a la valeur `DEPLOYED`.

    ```
    helm status vpn
    ```
    {: pre}

11. Une fois la charte Helm déployée, vérifiez que les paramètres mis à jour dans le fichier `config.yaml` ont été utilisés.

    ```
    helm get values vpn
    ```
    {: pre}

12. Nettoyez les pods du test en cours.

    ```
    kubectl get pods -a -l app=strongswan-test
    ```
    {: pre}

    ```
    kubectl delete pods -l app=strongswan-test
    ```
    {: pre}

13. Réexécutez les tests.

    ```
    helm test vpn
    ```
    {: pre}

<br />


## Mise à niveau de la charte Helm strongSwan
{: #vpn_upgrade}

Vérifiez que votre charte Helm strongSwan est à jour en la mettant à niveau.
{:shortdesc}

Pour mettre à niveau votre charte Helm strongSwan à la version la plus récente :

  ```
  helm upgrade -f config.yaml <release_name> ibm/strongswan
  ```
  {: pre}

**Important** : La charte Helm 2.0.0 strongSwan ne fonctionne pas avec Calico v3 ou Kubernetes 1.10. Avant de [mettre à jour votre cluster vers la version 1.10](cs_versions.html#cs_v110), mettez à jour strongSwan vers la charte Helm 2.1.0, qui offre une compatibilité en amont avec Calico 2.6 et Kubernetes 1.7, 1.8 et 1.9.


### Mise à niveau à partir de la version 1.0.0
{: #vpn_upgrade_1.0.0}

En raison de certains paramètres utilisés dans la version 1.0.0 de la charte Helm, vous ne pouvez pas utiliser la commande `helm upgrade` pour passer de la version 1.0.0 à la version la plus récente.
{:shortdesc}

Pour effectuer la mise à niveau à partir de la version 1.0.0, vous devez supprimer la charte Helm 1.0.0 et installez la version la plus récente :

1. Supprimez la charte Helm 1.0.0.

    ```
    helm delete --purge <release_name>
    ```
    {: pre}

2. Sauvegardez les paramètres de configuration par défaut de la version la plus récente de la charte Helm strongSwan dans un fichier YAML local.

    ```
    helm inspect values ibm/strongswan > config.yaml
    ```
    {: pre}

3. Mettez à jour le fichier de configuration et sauvegardez le fichier avec vos modifications.

4. Installez la charte Helm dans votre cluster avec le fichier `config.yaml` mis à jour.

    ```
    helm install -f config.yaml --name=<release_name> ibm/strongswan
    ```
    {: pre}

En outre, certains paramètres de délai d'attente `ipsec.conf` codés en dur dans la version 1.0.0 sont exposés en tant que propriétés configurables dans les versions ultérieures. Les noms et valeurs par défaut de ces paramètres de délai d'attente `ipsec.conf` configurables ont été également modifiés pour se conformer davantage aux normes strongSwan. Si vous effectuez une mise à niveau de votre charte Helm à partir de la version 1.0.0 et que vous voulez conserver les valeurs par défaut de cette version pour les paramètres de délai d'attente (timeout), ajoutez les nouvelles valeurs dans le fichier de configuration de votre charte avec les anciennes valeurs par défaut.



  <table>
  <caption>Différences des paramètres ipsec.conf entre la version 1.0.0 et la version la plus récente</caption>
  <thead>
  <th>Nom du paramètre dans la version 1.0.0</th>
  <th>Valeur par défaut dans la version 1.0.0</th>
  <th>Nom du paramètre dans la version la plus récente</th>
  <th>Valeur par défaut dans la version la plus récente</th>
  </thead>
  <tbody>
  <tr>
  <td><code>ikelifetime</code></td>
  <td>60m</td>
  <td><code>ikelifetime</code></td>
  <td>3h</td>
  </tr>
  <tr>
  <td><code>keylife</code></td>
  <td>20m</td>
  <td><code>lifetime</code></td>
  <td>1h</td>
  </tr>
  <tr>
  <td><code>rekeymargin</code></td>
  <td>3m</td>
  <td><code>margintime</code></td>
  <td>9m</td>
  </tr>
  </tbody></table>


## Désactivation du service VPN IPsec strongSwan
{: vpn_disable}

Vous pouvez désactiver la connexion VPN en supprimant la charte Helm.
{:shortdesc}

  ```
  helm delete --purge <release_name>
  ```
  {: pre}

<br />


## Configuration de la connectivité VPN avec un dispositif de routeur virtuel (VRA)
{: #vyatta}

Le [dispositif de routeur virtuel (VRA)](/docs/infrastructure/virtual-router-appliance/about.html) fournit le système d'exploitation Vyatta 5600 le plus récent pour serveurs bare metal x86. Vous pouvez utiliser un dispositif VRA comme passerelle VPN pour vous connecter de manière sécurisée à un réseau sur site.
{:shortdesc}

Tout le trafic réseau public et privé qui entre ou sort des VLAN du cluster est acheminé via le dispositif VRA. Vous pouvez utiliser le dispositif VRA comme noeud final VPN pour créer un tunnel IPSec chiffré entre les serveurs dans l'infrastructure IBM Cloud (SoftLayer) et les ressources sur site. Par exemple, le diagramme suivant illustre comment une application sur un noeud worker uniquement privé dans {{site.data.keyword.containershort_notm}} peut communiquer avec un serveur sur site par le biais d'une connexion VPN VRA :

<img src="images/cs_vpn_vyatta.png" width="725" alt="Exposition d'une application dans {{site.data.keyword.containershort_notm}} à l'aide d'un équilibreur de charge" style="width:725px; border-style: none"/>

1. Une application dans votre cluster nommée `myapp2` reçoit une demande d'un service Ingress ou LoadBalancer et nécessite une connexion sécurisée aux données de votre réseau sur site.

2. Etant donné que `myapp2` se trouve sur un noeud worker sur un réseau VLAN privé uniquement, le dispositif VRA fait office de connexion sécurisée entre les noeuds worker et le réseau sur site. Le dispositif VRA utilise l'adresse IP de destination pour déterminer quels sont les paquets réseau à envoyer au réseau sur site.

3. La demande est chiffrée et envoyée via le tunnel VPN au centre de données sur site.

4. La demande entrante passe par le pare-feu local et est distribuée sur le noeud final du tunnel VPN (routeur) où elle est déchiffrée.

5. Le noeud final du tunnel VPN (routeur) transfère la demande au serveur sur site ou au mainframe, en fonction de l'adresse IP de destination qui a été indiquée à l'étape 2. Les données nécessaires sont renvoyées via la connexion VPN à `myapp2` en utilisant le même processus.

Pour configurer un dispositif de routeur virtuel (VRA) :

1. [Commandez un dispositif VRA](/docs/infrastructure/virtual-router-appliance/getting-started.html).

2. [Configurez le VLAN privé sur le dispositif VRA](/docs/infrastructure/virtual-router-appliance/manage-vlans.html).

3. Pour activer une connexion VPN en utilisant le dispositif VRA, [configurez VRRP sur le dispositif VRA](/docs/infrastructure/virtual-router-appliance/vrrp.html#high-availability-vpn-with-vrrp).
