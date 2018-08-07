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



# Traitement des incidents liés au stockage en cluster
{: #cs_troubleshoot_storage}

Lorsque vous utilisez {{site.data.keyword.containerlong}}, tenez compte des techniques décrites ci-dessous pour identifier et résoudre les incidents liés au stockage en cluster.
{: shortdesc}

Si vous rencontrez un problème d'ordre plus général, expérimentez le [débogage de cluster](cs_troubleshoot.html).
{: tip}



## Les systèmes de fichiers des noeuds worker passent en mode lecture seule
{: #readonly_nodes}

{: tsSymptoms}
{: #stuck_creating_state}
Vous pouvez voir l'un des symptômes suivants :
- Lorsque vous exécutez la commande `kubectl get pods -o wide`, vous voyez que plusieurs pods qui s'exécutent sur le même noeud worker sont bloqués à l'état `ContainerCreating`.
- Lorsque vous exécutez une commande `kubectl describe`, vous voyez s'afficher l'erreur suivante à la section **Events** : `MountVolume.SetUp failed for volume ... read-only file system`.

{: tsCauses}
Le système de fichiers sur le noeud worker est en lecture seule.

{: tsResolve}
1.  Sauvegardez les données éventuelles stockées sur le noeud worker ou dans vos conteneurs.
2.  Pour une solution à court terme sur le noeud worker existant, rechargez le noeud worker.
    <pre class="pre"><code>bx cs worker-reload &lt;cluster_name&gt; &lt;worker_ID&gt;</code></pre>

Pour une solution à long terme, [mettez à jour le type de machine en ajoutant un autre noeud worker](cs_cluster_update.html#machine_type).

<br />



## L'application échoue lorsqu'un utilisateur non root détient le chemin de montage du stockage de fichiers NFS
{: #nonroot}

{: tsSymptoms}
Une fois que vous [ajoutez du stockage NFS](cs_storage.html#app_volume_mount) dans votre déploiement, le conteneur ne parvient pas à se déployer. Lorsque vous extrayez les journaux de votre conteneur, vous pourrez voir des erreurs liées au droit d'accès en écriture ou des messages d'erreur indiquant qu'il manque des droits nécessaires. Il y a échec du pod qui est bloqué dans un cycle de rechargement.

{: tsCauses}
Par défaut, les utilisateurs non root ne disposent pas de droits en écriture sur le chemin de montage du volume correspondant au stockage sauvegardé par NFS. Certaines images d'application courantes, telles que Jenkins et Nexus3, spécifient un utilisateur non root qui détient le chemin de montage dans le fichier Dockerfile. Lorsque vous créez un conteneur à partir de ce fichier Dockerfile, la création échoue car l'utilisateur non root ne dispose pas des droits nécessaires pour accéder au chemin de montage. Pour octroyer des droits en écriture, vous pouvez modifier le fichier Dockerfile pour ajouter temporairement l'utilisateur non root au groupe d'utilisateurs root avant que les droits sur le chemin de montage soient modifiés, ou utiliser un conteneur init.

Si vous utilisez une charte Helm pour déployer l'image, modifiez le déploiement Helm pour utiliser un conteneur init.
{:tip}



{: tsResolve}
Lorsque vous ajoutez un [conteneur init![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) dans votre déploiement, vous pouvez octroyer à un utilisateur non root spécifié dans votre fichier Dockerfile les droits en écriture pour le chemin de montage du volume à l'intérieur du conteneur qui pointe vers votre partage de fichiers NFS. Le conteneur init démarre avant le conteneur de votre application. Le conteneur init crée le chemin de montage du volume dans le conteneur, modifie le chemin de montage de sorte à ce que l'utilisateur (non root) correct en soit détenteur, puis se ferme. Ensuite, le conteneur de votre application avec l'utilisateur non root qui doit écrire dans le chemin de montage démarre. Comme le chemin est déjà détenu par l'utilisateur non root, l'écriture dans le chemin de montage aboutit. Si vous ne voulez pas utiliser un conteneur init, vous pouvez modifier le fichier Dockerfile afin d'ajouter l'accès d'utilisateur non root au stockage de fichiers NFS.


Avant de commencer, [ciblez avec votre interface de ligne de commande](cs_cli_install.html#cs_cli_configure) votre cluster.

1.  Ouvrez le fichier Dockerfile de votre application et récupérez l'ID utilisateur (UID) et l'ID du groupe (GID) de l'utilisateur auquel vous voulez accorder les droits en écriture sur le chemin de montage du volume. Dans l'exemple d'un fichier Dockerfile Jenkins, les informations sont :
    - UID : `1000`
    - GID : `1000`

    **Exemple** :

    ```
    FROM openjdk:8-jdk

    RUN apt-get update &&apt-get install -y git curl &&rm -rf /var/lib/apt/lists/*

    ARG user=jenkins
    ARG group=jenkins
    ARG uid=1000
    ARG gid=1000
    ARG http_port=8080
    ARG agent_port=50000

    ENV JENKINS_HOME /var/jenkins_home
    ENV JENKINS_SLAVE_AGENT_PORT ${agent_port}
    ...
    ```
    {:screen}

2.  Ajoutez du stockage persistant à votre application en créant une réservation de volume persistant (PVC). Cet exemple utilise la classe de stockage `ibmc-file-bronze`. Pour consulter les classes de stockage disponibles, exécutez la commande `kubectl get storageclasses`.

    ```
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: mypvc
      annotations:
        volume.beta.kubernetes.io/storage-class: "ibmc-file-bronze"
    spec:
      accessModes:
        - ReadWriteMany
      resources:
        requests:
          storage: 20Gi
    ```
    {: codeblock}

3.  Créez la réservation de volume persistant (PVC).

    ```
    kubectl apply -f mypvc.yaml
    ```
    {: pre}

4.  Dans votre fichier `.yaml` de déploiement, ajoutez le conteneur init. Insérez les identificateurs UID et GID que vous avez récupérés précédemment.

    ```
    initContainers:
    - name: initContainer # Or you can replace with any name
      image: alpine:latest
      command: ["/bin/sh", "-c"]
      args:
        - chown <UID>:<GID> /mount; # Replace UID and GID with values from the Dockerfile
      volumeMounts:
      - name: volume # Or you can replace with any name
        mountPath: /mount # Must match the mount path in the args line
    ```
    {: codeblock}

    **Exemple** pour un déploiement Jenkins :

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: my_pod
    spec:
      replicas: 1
      template:
        metadata:
          labels:
            app: jenkins
        spec:
          containers:
          - name: jenkins
            image: jenkins
            volumeMounts:
            - mountPath: /var/jenkins_home
              name: volume
          volumes:
          - name: volume
            persistentVolumeClaim:
              claimName: mypvc
          initContainers:
          - name: permissionsfix
            image: alpine:latest
            command: ["/bin/sh", "-c"]
            args:
              - chown 1000:1000 /mount;
            volumeMounts:
            - name: volume
              mountPath: /mount
    ```
    {: codeblock}

5.  Créez le pod et montez la PVC dessus.

    ```
    kubectl apply -f my_pod.yaml
    ```
    {: pre}

6. Vérifiez que le montage du volume sur votre pod a réussi. Notez le nom du pod et le chemin **Containers/Mounts**.

    ```
    kubectl describe pod <my_pod>
    ```
    {: pre}

    **Exemple de sortie** :

    ```
    Name:		    mypod-123456789
    Namespace:	default
    ...
    Init Containers:
    ...
    Mounts:
      /mount from volume (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-cp9f0 (ro)
    ...
    Containers:
      jenkins:
        Container ID:
        Image:		jenkins
        Image ID:
        Port:		  <none>
        State:		Waiting
          Reason:		PodInitializing
        Ready:		False
        Restart Count:	0
        Environment:	<none>
        Mounts:
          /var/jenkins_home from volume (rw)
          /var/run/secrets/kubernetes.io/serviceaccount from default-token-cp9f0 (ro)
    ...
    Volumes:
      myvol:
        Type: PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
        ClaimName: mypvc
        ReadOnly: false

    ```
    {: screen}

7.  Connectez-vous au pod en utilisant le nom du pod que vous avez noté précédemment.

    ```
    kubectl exec -it <my_pod-123456789> /bin/bash
    ```
    {: pre}

8. Vérifiez les droits octroyés pour le chemin de montage de votre conteneur. Dans l'exemple, le chemin de montage est `/var/jenkins_home`.

    ```
    ls -ln /var/jenkins_home
    ```
    {: pre}

    **Exemple de sortie** :

    ```
    jenkins@mypod-123456789:/$ ls -ln /var/jenkins_home
    total 12
    -rw-r--r-- 1 1000 1000  102 Mar  9 19:58 copy_reference_file.log
    drwxr-xr-x 2 1000 1000 4096 Mar  9 19:58 init.groovy.d
    drwxr-xr-x 9 1000 1000 4096 Mar  9 20:16 war
    ```
    {: screen}

    Cette sortie montre que les identificateurs GID et UID de votre fichier Dockerfile (dans cet exemple, `1000` et `1000`) sont propriétaires du chemin de montage à l'intérieur du conteneur.

<br />


## Echec de l'ajout de l'accès d'utilisateur non root au stockage persistant
{: #cs_storage_nonroot}

{: tsSymptoms}
Une fois que vous avez [ajouté l'accès d'utilisateur non root au stockage persistant](#nonroot) ou déployé une charte Helm avec un ID d'utilisateur non root, l'utilisateur n'a pas accès en écriture au stockage monté.

{: tsCauses}
Le déploiement ou la configuration de la charte Helm indique le [contexte de sécurité](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/) pour `fsGroup` (ID de groupe) et `runAsUser` (ID utilisateur) du pod. Actuellement, {{site.data.keyword.containershort_notm}} ne prend pas en charge la spécification `fsGroup` et n'accepte que `runAsUser` défini avec la valeur `0` (droits de l'utilisateur root).

{: tsResolve}
Supprimez les zones `securityContext` dans la configuration pour `fsGroup` et `runAsUser` du fichier de configuration de l'image, du déploiement ou de la charte Helm, et effectuez un redéploiement. Si vous devez remplacer la propriété du chemin de montage `nobody`, [ajoutez un accès d'utilisateur non root](#nonroot). Une fois que vous avez ajouté la propriété [non-root initContainer](#nonroot), définissez `runAsUser` au niveau du conteneur et non pas au niveau du pod.

<br />




## Echec de montage d'un stockage par blocs existant sur un pod en raison d'un système de fichiers incorrect
{: #block_filesystem}

{: tsSymptoms}
Lorsque vous exécutez la commande `kubectl describe pod <pod_name>`, l'erreur suivante s'affiche :
```
failed to mount the volume as "ext4", it already contains xfs. Mount error: mount failed: exit status 32
```
{: screen}

{: tsCauses}
Vous disposez d'une unité de stockage par blocs configurée avec un système de fichiers `XFS`. Pour monter cette unité sur votre pod, vous [avez créé un volume persistant (PV)](cs_storage.html#existing_block) qui spécifiait `ext4` pour votre système de fichiers dans la section `spec/flexVolume/fsType`. Si aucun système de fichiers n'est défini, le volume persistant prend la valeur par défaut `ext4`.
Le volume persitant a été créé et lié à votre instance de stockage par blocs. Cependant, lorsque vous tentez de monter le volume persistant sur votre cluster en utilisant une réservation de volume persistant (PVC) correspondante, le montage du volume échoue. Vous ne pouvez pas monter votre instance de stockage par blocs `XFS` avec un système de fichiers `ext4` sur le pod.

{: tsResolve}
Mettez à jour le système de fichier dans le volume persistant (PV) existant en remplaçant `ext4` par `XFS`.

1. Affichez la liste des volumes persistants existants dans votre cluster et notez le nom du volume persistant que vous avez utilisé pour votre instance de stockage par blocs.
   ```
   kubectl get pv
   ```
   {: pre}

2. Sauvegardez le fichier YAML du volume persistant sur votre machine locale.
   ```
   kubectl get pv <pv_name> -o yaml > <filepath/xfs_pv.yaml>
   ```
   {: pre}

3. Ouvrez le fichier YAML et modifiez `fsType` en remplaçant `ext4` par `xfs`.
4. Replacez le volume persistant dans votre cluster.
   ```
   kubectl replace --force -f <filepath/xfs_pv.yaml>
   ```
   {: pre}

5. Connectez-vous au pod sur lequel vous avez monté le volume persistant.
   ```
   kubectl exec -it <pod_name> sh
   ```
   {: pre}

6. Vérifiez que le système de fichiers est passé à `XFS`.
   ```
   df -Th
   ```
   {: pre}

   Exemple de sortie :
   ```
   Filesystem Type Size Used Avail Use% Mounted on /dev/mapper/3600a098031234546d5d4c9876654e35 xfs 20G 33M 20G 1% /myvolumepath
   ```
   {: screen}

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

