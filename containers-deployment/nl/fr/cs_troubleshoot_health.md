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



# Traitement des incidents liés à la consignation et à la surveillance
{: #cs_troubleshoot_health}

Lorsque vous utilisez {{site.data.keyword.containerlong}}, tenez compte des techniques décrites ci-dessous pour identifier et résoudre les incidents liés à la consignation et à la surveillance.
{: shortdesc}

Si vous rencontrez un problème d'ordre plus général, expérimentez le [débogage de cluster](cs_troubleshoot.html).
{: tip}

## Les journaux ne s'affichent pas
{: #cs_no_logs}

{: tsSymptoms}
Lorsque vous accédez au tableau de bord Kibana, vos journaux ne s'affichent pas.

{: tsResolve}
Ci-dessous figurent les motifs pour lesquels les journaux de votre cluster peuvent ne pas apparaître, ainsi que les étapes de résolution correspondantes :

<table>
<caption>Traitement des incidents liés aux journaux qui ne s'affichent pas</caption>
  <col width="40%">
  <col width="60%">
  <thead>
    <tr>
      <th>Motifs</th>
      <th>Procédure de résolution du problème</th>
    </tr>
 </thead>
 <tbody>
  <tr>
    <td>Aucune configuration de consignation n'a été définie.</td>
    <td>Pour que les journaux soient envoyés, vous devez créer une configuration de consignation. Pour cela, voir <a href="cs_health.html#logging">Configuration de la consignation de cluster</a>.</td>
  </tr>
  <tr>
    <td>L'état du cluster n'indique pas <code>Normal</code>.</td>
    <td>Pour vérifier l'état de votre cluster, voir <a href="cs_troubleshoot.html#debug_clusters">Débogage des clusters</a>.</td>
  </tr>
  <tr>
    <td>Le quota de stockage des journaux a été atteint.</td>
    <td>Pour augmenter vos limites de stockage de journaux, reportez-vous à la <a href="/docs/services/CloudLogAnalysis/troubleshooting/error_msgs.html">Documentation {{site.data.keyword.loganalysislong_notm}}</a>.</td>
  </tr>
  <tr>
    <td>Si vous avez spécifié un espace lors de la création du cluster, les droits d'accès Responsable, Développeur  ou Auditeur n'ont pas été affectées au propriétaire du compte sur cet espace.</td>
      <td>Pour modifier les droits d'accès du propriétaire du compte :
      <ol><li>Pour identifier le propriétaire du compte pour le cluster, exécutez la commande <code>bx cs api-key-info &lt;cluster_name_or_ID&gt;</code>.</li>
      <li>Pour attribuer au propriétaire du compte les droits d'accès Responsable, Développeur ou Auditeur dans {{site.data.keyword.containershort_notm}} sur cet espace, voir <a href="cs_users.html">Gestion de l'accès au cluster</a>.</li>
      <li>Pour actualiser le jeton d'accès après modification des droits d'accès, exécutez la commande <code>bx cs logging-config-refresh &lt;cluster_name_or_ID&gt;</code>.</li></ol></td>
    </tr>
    <tr>
      <td>Vous avez une configuration de consignation d'application avec un lien symbolique dans le chemin de votre application.</td>
      <td><p>Pour que les journaux soient envoyés, vous devez utiliser un chemin absolu dans votre configuration de consignation autrement vos journaux ne pourront pas être lus. Si votre chemin est monté sur votre noeud worker, il peut avoir créé un lien symbolique.</p> <p>Exemple : si le chemin spécifié est <code>/usr/local/<b>spark</b>/work/app-0546/0/stderr</code> mais que les journaux sont acheminés vers <code>/usr/local/<b>spark-1.0-hadoop-1.2</b>/work/app-0546/0/stderr</code>, les journaux ne pourront pas être lus.</p></td>
    </tr>
  </tbody>
</table>

Pour tester les modifications apportées lors de la résolution des incidents, vous pouvez déployer l'exemple de pod *Noisy*, lequel génère plusieurs événements de journal sur un noeud worker dans votre cluster.

  1. [Ciblez avec votre interface de ligne de commande](cs_cli_install.html#cs_cli_configure) le cluster dans lequel vous voulez lancer la génération de journaux.

  2. Créez le fichier de configuration `deploy-noisy.yaml`.

      ```
      apiVersion: v1
      kind: Pod
      metadata:
        name: noisy
      spec:
        containers:
        - name: noisy
          image: ubuntu:16.04
          command: ["/bin/sh"]
          args: ["-c", "while true; do sleep 10; echo 'Hello world!'; done"]
          imagePullPolicy: "Always"
        ```
        {: codeblock}

  3. Exécutez le fichier de configuration dans le contexte de cluster.

        ```
        kubectl apply -f noisy.yaml
        ```
        {:pre}

  4. Au bout de quelques minutes, vos journaux s'affichent dans le tableau de bord Kibana. Pour accéder au tableau de bord Kibana, accédez à l'une des URL suivantes et sélectionnez le compte {{site.data.keyword.Bluemix_notm}} dans lequel vous avez créé le cluster. Si vous avez spécifié un espace lors de la création du cluster, accédez à la place à cet espace.
      - Sud et Est des Etats-Unis : https://logging.ng.bluemix.net
      - Sud du Royaume-Uni : https://logging.eu-gb.bluemix.net
      - Europe centrale : https://logging.eu-fra.bluemix.net
      - Asie-Pacifique sud : https://logging.au-syd.bluemix.net

<br />


## Le tableau de bord Kubernetes n'affiche pas de graphiques d'utilisation
{: #cs_dashboard_graphs}

{: tsSymptoms}
Lorsque vous accédez au tableau de bord Kubernetes, les graphiques d'utilisation ne s'affichent pas.

{: tsCauses}
Parfois, après la mise à jour d'un cluster ou le réamorçage d'un noeud worker, le pod `kube-dashboard` ne se met pas à jour.

{: tsResolve}
Supprimez le pod `kube-dashboard` pour forcer un redémarrage. Le pod est recréé avec des règles RBAC afin d'accéder à heapster pour obtenir les informations d'utilisation.

  ```
  kubectl delete pod -n kube-system $(kubectl get pod -n kube-system --selector=k8s-app=kubernetes-dashboard -o jsonpath='{.items..metadata.name}')
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

