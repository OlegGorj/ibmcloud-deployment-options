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



# Risoluzione dei problemi dell'archiviazione cluster
{: #cs_troubleshoot_storage}

Mentre utilizzi {{site.data.keyword.containerlong}}, tieni presente queste tecniche per la risoluzione dei problemi dell'archiviazione del cluster.
{: shortdesc}

Se hai un problema più generale, prova a [eseguire il debug del cluster](cs_troubleshoot.html).
{: tip}



## I file system per i nodi di lavoro diventano di sola lettura
{: #readonly_nodes}

{: tsSymptoms}
{: #stuck_creating_state}
Potresti vedere uno dei seguenti sintomi:
- Quando esegui `kubectl get pods -o wide`, visualizzi che molti pod in esecuzione nello stesso nodo di lavoro sono bloccati nello stato `ContainerCreating`.
- Quando esegui un comando `kubectl describe`, vedi il seguente errore nella sezione **Eventi**: `MountVolume.SetUp failed for volume ... read-only file system`.

{: tsCauses}
Il file system nel nodo di lavoro è in sola lettura.

{: tsResolve}
1.  Esegui il backup di tutti i dati che possono venire archiviati nel nodo di lavoro o nei tuoi contenitori.
2.  Per una correzione a breve termine per il nodo di lavoro esistente, ricarica il nodo di lavoro.
    <pre class="pre"><code>bx cs worker-reload &lt;cluster_name&gt; &lt;worker_ID&gt;</code></pre>

Per una correzione a lungo termine, [aggiorna il tipo di macchina aggiungendo un altro nodo di lavoro](cs_cluster_update.html#machine_type).

<br />



## L'applicazione genera un errore quando un utente non root possiede il percorso di montaggio dell'archiviazione file NFS
{: #nonroot}

{: tsSymptoms}
Dopo aver [aggiunto l'archiviazione NFS](cs_storage.html#app_volume_mount) alla tua distribuzione, la distribuzione del tuo contenitore ha esito negativo. Una volta recuperati i log relativi al tuo contenitore, potresti vedere errori di tipo "write-permission" o "do not have required permission". Il pod genera un errore e si blocca in un ciclo di ricaricamento.

{: tsCauses}
Per impostazione predefinita, gli utenti non root non dispongono dell'autorizzazione di scrittura sul percorso di montaggio del volume per l'archiviazione con supporto NFS. Alcune immagini comuni dell'applicazione, come Jenkins e Nexus3, specificano un utente non root che possiede il percorso di montaggio nel Dockerfile. Quando crei un contenitore da questo Dockerfile, la creazione del contenitore non riesce a causa di autorizzazioni insufficienti dell'utente non root sul percorso di montaggio. Per concedere l'autorizzazione di scrittura, puoi modificare il Dockerfile per aggiungere temporaneamente l'utente non root al gruppo di utenti root prima di modificare le autorizzazioni del percorso di montaggio o utilizzare un contenitore init. 

Se utilizzi un grafico Helm per distribuire l'immagine, modifica la distribuzione Helm in modo che utilizzi un contenitore init.
{:tip}



{: tsResolve}
Quando includi un [contenitore init![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) nella tua distribuzione,  puoi fornire a un utente non root specificato nel Dockerfile le autorizzazioni di scrittura sul percorso di montaggio del volume all'interno del contenitore. Il contenitore init viene avviato prima del contenitore dell'applicazione. Il contenitore init crea il percorso di montaggio del volume all'interno del contenitore, modifica il percorso di montaggio in modo che sia di proprietà dell'utente corretto (non root) e si chiude. Quindi, viene avviato il contenitore dell'applicazione con l'utente non root che deve scrivere sul percorso di montaggio. Poiché il percorso è già di proprietà dell'utente non root, la scrittura sul percorso di montaggio ha esito positivo. Se non vuoi utilizzare un contenitore init, puoi modificare il Dockerfile per aggiungere l'accesso utente non root all'archiviazione file NFS.


Prima di iniziare, [indirizza la tua CLI](cs_cli_install.html#cs_cli_configure) al tuo cluster.

1.  Apri il Dockerfile per la tua applicazione e ottieni l'ID utente (UID) e l'ID di gruppo (GID) dell'utente a cui desideri concedere l'autorizzazione di scrittore sul percorso di montaggio del volume. Nell'esempio di un Dockerfile Jenkins, le informazioni sono:
    - UID: `1000`
    - GID: `1000`

    **Esempio**:

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

2.  Aggiungi l'archiviazione persistente alla tua applicazione creando un'attestazione del volume persistente (o PVC, persistent volume claim). Questo esempio utilizza la classe di archiviazione `ibmc-file-bronze`. Per esaminare le classi di archiviazione disponibili, esegui `kubectl get storageclasses`.

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

3.  Crea la PVC.

    ```
    kubectl apply -f mypvc.yaml
    ```
    {: pre}

4.  Nel tuo file di distribuzione `.yaml`, aggiungi il contenitore init. Includi l'UID e il GID che hai richiamato in precedenza.

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

    **Esempio** per una distribuzione Jenkins:

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

5.  Crea il pod e monta la PVC nel tuo pod.

    ```
    kubectl apply -f my_pod.yaml
    ```
    {: pre}

6. Verifica che il volume sia stato correttamente montato nel tuo pod. Prendi nota del nome del pod e del percorso **Containers/Mounts**.

    ```
    kubectl describe pod <my_pod>
    ```
    {: pre}

    **Output di esempio**:

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

7.  Accedi al pod utilizzando il nome pod che hai annotato precedentemente.

    ```
    kubectl exec -it <my_pod-123456789> /bin/bash
    ```
    {: pre}

8. Verifica le autorizzazioni del percorso di montaggio del tuo contenitore. Nell'esempio, il percorso di montaggio è `/var/jenkins_home`.

    ```
    ls -ln /var/jenkins_home
    ```
    {: pre}

    **Output di esempio**:

    ```
    jenkins@mypod-123456789:/$ ls -ln /var/jenkins_home
    total 12
    -rw-r--r-- 1 1000 1000  102 Mar  9 19:58 copy_reference_file.log
    drwxr-xr-x 2 1000 1000 4096 Mar  9 19:58 init.groovy.d
    drwxr-xr-x 9 1000 1000 4096 Mar  9 20:16 war
    ```
    {: screen}

    Questo output mostra che il GID e l'UID dal tuo Dockerfile (in questo esempio, `1000` e `1000`) possiedono il percorso di montaggio all'interno del contenitore.

<br />


## L'aggiunta di accesso utente non root all'archiviazione persistente non riesce
{: #cs_storage_nonroot}

{: tsSymptoms}
Dopo aver [aggiunto l'accesso utente non root all'archiviazione persistente](#nonroot) o la distribuzione di un grafico Helm con un ID utente non root specificato, l'utente non può scrivere nell'archiviazione montata.

{: tsCauses}
La distribuzione o la configurazione del grafico Helm specifica il [contesto di sicurezza](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/) per `fsGroup` (ID gruppo) e `runAsUser` (ID utente) del pod. Attualmente, {{site.data.keyword.containershort_notm}} non supporta la specifica `fsGroup` e supporta solo `runAsUser` impostato come `0` (autorizzazioni root). 

{: tsResolve}
Rimuovi i campi `securityContext` della configurazione per `fsGroup` e `runAsUser` dal file di configurazione dell'immagine, della distribuzione o del grafico Helm e riesegui la distribuzione. Se devi modificare la proprietà del percorso di montaggio da `nobody`, [aggiungi l'accesso utente non root](#nonroot). Una volta aggiunto [non-root initContainer](#nonroot), imposta `runAsUser` a livello di contenitore, non a livello di pod. 

<br />




## Impossibile montare l'archiviazione blocchi esistente in un pod a causa del file system sbagliato
{: #block_filesystem}

{: tsSymptoms}
Quando esegui `kubectl describe pod <pod_name>`, visualizzi il seguente errore:
```
failed to mount the volume as "ext4", it already contains xfs. Mount error: mount failed: exit status 32
```
{: screen}

{: tsCauses}
Hai un dispositivo di archiviazione blocchi esistente configurato con un file system `XFS`. Per montare questo dispositivo nel tuo pod, [hai creato un PV](cs_storage.html#existing_block) che ha specificato `ext4` come tuo file system o nessun file system nella sezione `spec/flexVolume/fsType`. Se non viene definito un file system, viene utilizzato il valore predefinito del PV `ext4`.
Il PV era stato creato correttamente e collegato alla tua istanza dell'archiviazione blocchi esistente. Tuttavia, quando tenti di montare il PV nel tuo cluster utilizzando un PVC corrispondente, il montaggio del volume non riesce. Non puoi montare la tua istanza dell'archiviazione blocchi `XFS` con un file system `ext4` nel pod.

{: tsResolve}
Aggiorna il file system nel PV esistente da `ext4` a `XFS`.

1. Elenca i PV esistenti nel tuo cluster e prendi nota del nome del PV che hai utilizzato per la tua istanza dell'archiviazione blocchi esistente.
   ```
   kubectl get pv
   ```
   {: pre}

2. Salva il file yaml del PV nella tua macchina locale.
   ```
   kubectl get pv <pv_name> -o yaml > <filepath/xfs_pv.yaml>
   ```
   {: pre}

3. Apri il file yaml e modifica `fsType` da `ext4` a `xfs`.
4. Sostituisci il PV nel tuo cluster. 
   ```
   kubectl replace --force -f <filepath/xfs_pv.yaml>
   ```
   {: pre}

5. Accedi al pod in cui hai montato il PV.
   ```
   kubectl exec -it <pod_name> sh
   ```
   {: pre}

6. Verifica che il file system sia stato modificato con `XFS`.
   ```
   df -Th
   ```
   {: pre}

   Output di esempio:
   ```
   Filesystem Type Size Used Avail Use% Mounted on /dev/mapper/3600a098031234546d5d4c9876654e35 xfs 20G 33M 20G 1% /myvolumepath
   ```
   {: screen}

<br />



## Come ottenere aiuto e supporto
{: #ts_getting_help}

Stai ancora avendo problemi con il tuo cluster?
{: shortdesc}

-   Per vedere se {{site.data.keyword.Bluemix_notm}} è disponibile, [controlla la pagina sugli stati {{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://developer.ibm.com/bluemix/support/#status).
-   Pubblica una domanda in [{{site.data.keyword.containershort_notm}} Slack ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://ibm-container-service.slack.com).

    Se non stai utilizzando un ID IBM per il tuo account {{site.data.keyword.Bluemix_notm}}, [richiedi un invito](https://bxcs-slack-invite.mybluemix.net/) a questo Slack.
    {: tip}
-   Rivedi i forum per controllare se altri utenti hanno riscontrato gli stessi problemi. Quando utilizzi i forum per fare una domanda, contrassegna con una tag la tua domanda in modo che sia visualizzabile dai team di sviluppo {{site.data.keyword.Bluemix_notm}}.

    -   Se hai domande tecniche sullo sviluppo o la distribuzione di cluster o applicazioni con
{{site.data.keyword.containershort_notm}}, inserisci la tua domanda in
[Stack Overflow ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) e contrassegnala con le tag `ibm-cloud`, `kubernetes` e `containers`.
    -   Per domande sul servizio e sulle istruzioni per l'utilizzo iniziale, utilizza il forum
[IBM developerWorks dW Answers ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix). Includi le tag `ibm-cloud`
e `containers`.
    Consulta [Come ottenere supporto](/docs/get-support/howtogetsupport.html#using-avatar) per ulteriori dettagli sull'utilizzo dei forum.

-   Contatta il supporto IBM aprendo un ticket. Per informazioni su come aprire un ticket di supporto IBM o sui livelli di supporto e sulla gravità dei ticket, consulta [Come contattare il supporto](/docs/get-support/howtogetsupport.html#getting-customer-support).

{: tip}
Quando riporti un problema, includi il tuo ID del cluster. Per ottenere il tuo ID del cluster, esegui `bx cs clusters`.

