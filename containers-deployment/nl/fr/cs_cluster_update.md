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





# Mise à jour des clusters et des noeuds worker
{: #update}

Vous pouvez installer des mises à jour pour maintenir vos clusters Kubernetes à jour dans {{site.data.keyword.containerlong}}.
{:shortdesc}

## Mise à jour du maître Kubernetes
{: #master}

Régulièrement, Kubernetes publie des [mises à jour principales, secondaires ou des correctifs](cs_versions.html#version_types). En fonction du type de mise à jour, vous pouvez être chargé de mettre à jour des composants du maître Kubernetes.
{:shortdesc}

Les mises à jour peuvent affecter la version du serveur d'API Kubernetes ou d'autres composants dans le maître Kubernetes.  C'est toujours vous qui êtes chargé de garder vos noeuds worker à jour. Lors de l'application de mises à jour, le maître Kubernetes est actualisé avant les noeuds worker.

Par défaut, votre possibilité de mettre à jour le serveur d'API Kubernetes est limitée dans le maître Kubernetes dont les versions secondaires sont plus de deux fois supérieures à la version actuelle. Par exemple, si la version actuelle de votre serveur d'API Kubernetes est 1.7 et que vous voulez le mettre à jour à la version 1.10, vous devez d'abord effectuer une mise à jour vers la version 1.8 ou 1.9. Vous pouvez forcer la mise à jour au-delà de trois versions secondaires, mais ceci peut entraîner des résultats inattendus. Si votre cluster s'exécute sur une version non prise en charge de Kubernetes, il vous faudra peut-être forcer la mise à jour.

Le diagramme suivant illustre la procédure que vous pourriez suivre pour mettre à jour votre maître.

![Procédure recommandée pour la mise à jour du maître](/images/update-tree.png)

Figure 1. Diagramme de la procédure de mise à jour du maître Kubernetes

**Attention** : Vous ne pouvez pas revenir à une version antérieure du cluster une fois réalisée la procédure de mise à jour. Prenez soin d'utiliser un cluster de test et de suivre les instructions afin d'éviter des problèmes potentiels avant de mettre à jour votre maître de production.

Pour des mises à jour _principales_ ou _secondaires_, procédez comme suit :

1. Passez en revue les [modifications Kubernetes](cs_versions.html) et effectuez les mises à jour marquées _Mise à jour avant le maître_.
2. Mettez à jour votre serveur d'API Kubernetes et les composants du maître Kubernetes associés en utilisant l'interface graphique ou en exécutant la [commande CLI](cs_cli_reference.html#cs_cluster_update). Lorsque vous mettez à jour le serveur d'API Kubernetes, il est indisponible durant  5 à 10 minutes environ. Pendant la mise à jour, vous ne pouvez ni accéder au cluster, ni le modifier. En revanche, les noeuds worker, les applications et les ressources que les utilisateurs du cluster ont déployés ne sont pas modifiés et poursuivent leur exécution.
3. Confirmez que la mise à jour est terminée. Vérifiez la version du serveur d'API Kubernetes dans le tableau de bord {{site.data.keyword.Bluemix_notm}} ou exécutez la commande `bx cs clusters`.
4. Installez la version de l'interface CLI [`kubectl cli`](cs_cli_install.html#kubectl) qui correspond à la version du serveur d'API qui s'exécute sur le maître Kubernetes.

Lorsque la mise à jour du serveur d'API Kubernetes est terminée, vous pouvez mettre à jour vos noeuds worker.

<br />


## Mise à jour des noeuds worker
{: #worker_node}


Vous avez reçu une notification vous invitant à mettre à jour vos noeuds worker. Qu'est-ce que cela signifie ? Comme les mises à jour de sécurité et les correctifs sont mis en place pour le serveur d'API Kubernetes et d'autres composants du maître Kubernetes, vous devez vérifier que vos noeuds worker soient toujours synchronisés.
{: shortdesc}

La version Kubernetes du noeud worker ne peut pas être supérieure à celle du serveur d'API Kubernetes qui s'exécute sur votre maître Kubernetes. Avant de commencer, [mettez à jour le maître Kubernetes](#master).

**Attention** :
<ul><li>Les mises à jour des noeuds worker peuvent entraîner des temps d'indisponibilité de vos applications et services.</li>
<li>Les données sont supprimées si elles ne sont pas stockées hors du pod.</li>
<li>Utilisez des [répliques ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#replicas) dans vos déploiements pour replanifier des pods sur les noeuds disponibles.</li></ul>

Que faire si je ne peux pas me permettre d'avoir des temps d'indisponibilité ?

Dans le cadre de la procédure de mise à jour, des noeuds spécifiques deviendront indisponibles pendant un certain temps. En vue d'éviter des temps d'indisponibilité de votre application, vous pouvez définir des clés uniques dans une mappe de configuration qui spécifie des pourcentages de seuil pour des types de noeud spécifiques lors de la procédure de mise à jour. En définissant des règles basées sur des labels Kubernetes standard et en attribuant un pourcentage en nombre maximal de noeuds pouvant être indisponibles, vous pouvez vous assurer que votre application demeure opérationnelle. Un noeud est considéré comme indisponible s'il lui reste à effectuer le processus de déploiement.

Comment sont définies les clés ?

Dans la section des informations sur les données de la mappe de configuration, vous pouvez définir jusqu'à 10 règles distinctes en vigueur simultanément. Pour pouvoir être mis à niveau, les noeuds worker doivent satisfaire chaque règle définie.

Les clés ont été définies. Que faire maintenant ?

Après avoir défini vos règles, exécutez la commande `bx cs worker-update`. Si une réponse positive est renvoyée, les noeuds worker sont placés en file d'attente pour être mis à jour. Cependant, la procédure de mise à jour des noeuds n'est pas engagée tant que toutes les règles ne sont pas satisfaites. Une fois les noeuds placés en file d'attente, les règles sont vérifiées périodiquement pour déterminer si l'un des noeuds peut être mis à jour.

Que se passe-t-il si j'ai choisi de ne pas définir une mappe de configuration ?

Si aucune mappe de configuration n'est définie, celle par défaut est utilisée. Par défaut, 20 % au maximum de vos noeuds worker sont indisponibles au cours de la procédure de mise à jour.

Pour mettre à jour vos noeuds worker, procédez comme suit :

1. Apportez toutes les modifications indiquées par _Mise à jour après le maître_ dans [Modifications Kubernetes](cs_versions.html).

2. Facultatif : Définissez votre mappe de configuration.
    Exemple :

    ```
    apiVersion: v1
    kind: ConfigMap
    metadata:
      name: ibm-cluster-update-configuration
      namespace: kube-system
    data:
     drain_timeout_seconds: "120"
     zonecheck.json: |
       {
         "MaxUnavailablePercentage": 70,
         "NodeSelectorKey": "failure-domain.beta.kubernetes.io/zone",
         "NodeSelectorValue": "dal13"
       }
     regioncheck.json: |
       {
         "MaxUnavailablePercentage": 80,
         "NodeSelectorKey": "failure-domain.beta.kubernetes.io/region",
         "NodeSelectorValue": "us-south"
       }
     defaultcheck.json: |
       {
         "MaxUnavailablePercentage": 100
       }
    ```
    {:pre}
  <table summary="La première ligne du tableau couvre les deux colonnes. Le reste des lignes doit être lu de gauche à droite, le paramètre figurant dans la première colonne et les descriptions correspondantes dans la seconde colonne.">
  <caption>Composants de ConfigMap</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants </th>
    </thead>
    <tbody>
      <tr>
        <td><code>drain_timeout_seconds</code></td>
        <td> Facultatif : délai en secondes dû à l'arrêt qui se produit lors d'une mise à jour de noeud worker. Cet arrêt fait passer le noeud à l'état `unschedulable`, ce qui empêche le déploiement de nouveaux pods sur ce noeud. Cela supprime également les pods hors fonction du noeud. Les valeurs admises sont des entiers compris entre 1 et 180. La valeur par défaut est 30.</td>
      </tr>
      <tr>
        <td><code>zonecheck.json</code></br><code>regioncheck.json</code></td>
        <td> Exemple de clés uniques pour lesquelles vous désirez définir des règles. Vous pouvez nommer les clés à votre gré ; les informations sont analysées par les configurations définies dans la clé. Pour chaque clé que vous définissez, vous ne pouvez affecter qu'une seule valeur à <code>NodeSelectorKey</code> et <code>NodeSelectorValue</code>. Si vous désirez définir des règles pour plusieurs régions ou emplacements (centres de données), créez une nouvelle entrée de clé. </td>
      </tr>
      <tr>
        <td><code>defaultcheck.json</code></td>
        <td> Par défaut, si la mappe <code>ibm-cluster-update-configuration</code> n'est pas définie correctement, seuls 20 % de vos clusters peuvent être indisponibles à un moment donné. Si une ou plusieurs règles valides sont définies sans mappe de configuration globale par défaut celle utilisée par défaut autorise 100 % des noeuds worker à être indisponibles à un moment donné. Vous pouvez contrôler ce comportement en créant un pourcentage par défaut. </td>
      </tr>
      <tr>
        <td><code>MaxUnavailablePercentage</code></td>
        <td> Nombre maximal de noeuds autorisés à être indisponibles pour la clé spécifiée, sous forme de pourcentage. Un noeud est indisponible lorsqu'il est en cours de déploiement, de rechargement ou de provisionnement. La mise à jour des noeuds worker placés en file d'attente est bloquée si les pourcentages maximum définis pour les noeuds indisponibles sont dépassés. </td>
      </tr>
      <tr>
        <td><code>NodeSelectorKey</code></td>
        <td> Type de label pour lequel vous désirez définir une règle pour la clé spécifiée. Vous pouvez définir des règles pour les labels par défaut fournis par IBM, tout comme pour ceux que vous créez. </td>
      </tr>
      <tr>
        <td><code>NodeSelectorValue</code></td>
        <td> Sous-ensemble de noeuds dans la clé spécifiée que la règle doit évaluer. </td>
      </tr>
    </tbody>
  </table>

    **Remarque **: Vous pouvez définir 10 règles au maximum. Si vous ajoutez plus de 10 clés à un fichier, seul un sous-ensemble des informations est analysé.

3. Mettez à jour vos noeuds worker depuis l'interface graphique ou la ligne de commande (CLI).
  * Pour effectuer la mise à jour à partir du tableau de bord {{site.data.keyword.Bluemix_notm}}, accédez à la section `Worker nodes` de votre cluster, et cliquez sur `Update Worker`.
  * Pour obtenir les ID des noeuds worker, exécutez la commande `bx cs workers <cluster_name_or_ID>`. Si vous sélectionnez plusieurs noeuds worker, ceux-ci sont placés en file d'attente pour évaluation de la mise à jour. S'ils sont considérés comme prêts au terme de l'évaluation, ils seront mis à jour d'après les règles définies dans les configurations.

    ```
    bx cs worker-update <cluster_name_or_ID> <worker_node1_ID> <worker_node2_ID>
    ```
    {: pre}

4. Facultatif : Vérifiez les événements déclenchés par la mappe de configuration et les éventuelles erreurs de validation en exécutant la commande suivante et en examinant la zone **Events**.
    ```
    kubectl describe -n kube-system cm ibm-cluster-update-configuration
    ```
    {: pre}

5. Confirmez que la mise à jour est terminée :
  * Vérifiez la version Kubernetes dans le tableau de bord {{site.data.keyword.Bluemix_notm}} ou exécutez la commande `bx cs workers <cluster_name_or_ID>`.
  * Vérifiez la version Kubernets des noeuds worker en exécutant la commande `kubectl get nodes`.
  * Dans certains cas, des clusters plus anciens peuvent répertorier des noeuds worker en double avec un statut **NotReady** après une mise à jour. Pour supprimer ces doublons, voir la section de [traitement des incidents](cs_troubleshoot_clusters.html#cs_duplicate_nodes).

Etapes suivantes :
  - Répétez le processus de mise à jour pour les autres clusters.
  - Informez les développeurs qui travaillent dans le cluster pour qu'ils mettent à jour leur interface de ligne de commande `kubectl` à la version du maître Kubernetes.
  - Si le tableau de bord Kubernetes n'affiche pas les graphiques d'utilisation, [supprimez le pod `kube-dashboard`](cs_troubleshoot_health.html#cs_dashboard_graphs).
  





<br />



## Mise à jour des types de machine
{: #machine_type}

Vous pouvez mettre à jour les types de machine de vos noeuds worker en ajoutant de nouveaux noeuds worker et en supprimant les anciens. Par exemple, si vous disposez de noeuds worker sur des types de machine dépréciés comportant `u1c` ou `b1c` dans le nom, créez des noeuds worker ayant `u2c` ou `b2c` dans le nom.
{: shortdesc}

Avant de commencer :
- [Ciblez votre interface de ligne de commande](cs_cli_install.html#cs_cli_configure) vers votre cluster.
- Si vous stockez des données sur votre noeud worker, les données sont supprimées si elles ne sont pas [stockées hors du noeud worker](cs_storage.html#storage).


**Attention** : Les mises à jour des noeuds d'agent peuvent provoquer l'indisponibilité de vos services et applications. Les données sont supprimées si elles ne sont pas [stockées hors du pool](cs_storage.html#storage).



1. Notez les noms et emplacements des noeuds worker à ajouter.
    ```
    bx cs workers <cluster_name>
    ```
    {: pre}

2. Affichez les types de machine disponibles.
    ```
    bx cs machine-types <location>
    ```
    {: pre}

3. Ajoutez des noeuds worker en exécutant la commande [bx cs worker-add](cs_cli_reference.html#cs_worker_add). Indiquez un type de machine.

    ```
    bx cs worker-add --cluster <cluster_name> --machine-type <machine_type> --number <number_of_worker_nodes> --private-vlan <private_VLAN_ID> --public-vlan <public_VLAN_ID>
    ```
    {: pre}

4. Vérifiez que les noeuds worer ont été ajoutés.

    ```
    bx cs workers <cluster_name>
    ```
    {: pre}

5. Lorsque les noeuds worker ajoutés sont à l'état `Normal`, vous pouvez supprimer le noeud worker périmé. **Remarque** : Si vous supprimez un type de machine qui est facturé au mois (par exemple bare metal), vous êtes facturé pour le mois complet.

    ```
    bx cs worker-rm <cluster_name> <worker_node>
    ```
    {: pre}

6. Répétez cette procédure pour mettre à niveau d'autres noeuds worker sur d'autres types de machine.










