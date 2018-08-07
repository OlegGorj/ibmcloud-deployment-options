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




# Salvataggio dei dati nel tuo cluster
{: #storage}
Puoi conservare i dati in {{site.data.keyword.containerlong}} per condividerli tra le istanze dell'applicazione e per impedirne la perdita nel caso in cui un componente nel tuo cluster Kubernetes abbia un malfunzionamento.


## Pianificazione della memorizzazione altamente disponibile
{: #planning}

In {{site.data.keyword.containerlong_notm}}, puoi scegliere tra varie opzioni per memorizzare i tuoi dati dell'applicazione e condividerli tra i pod nel tuo cluster. Tuttavia, non tutte le opzioni di archiviazione offrono lo stesso livello di persistenza e disponibilità in situazioni in cui un componente nel cluster o un intero sito abbia un malfunzionamento.
{: shortdesc}


### Opzioni di archiviazione dati non persistente
{: #non_persistent}

Puoi utilizzare le opzioni di archiviazione non persistente se i dati non devono essere memorizzati in modo persistente o se i dati non devono essere condivisi tra le istanze dell'applicazione. Le opzioni di archiviazione
non persistente possono essere utilizzate anche per la verifica dell'unità dei tuoi componenti dell'applicazione o per provare nuove funzioni.
{: shortdesc}

La seguente immagine mostra le opzioni di archiviazione dati non persistente disponibili in {{site.data.keyword.containerlong_notm}}. Queste opzioni sono disponibili per i cluster gratuito e standard.
<p>
<img src="images/cs_storage_nonpersistent.png" alt="Opzioni di archiviazione dati non persistente" width="500" style="width: 500px; border-style: none"/></p>

<table summary="La tabella mostra le opzioni di archiviazione non persistente. Le righe devono essere lette da sinistra a destra, con il numero dell'opzione nella colonna uno, il titolo delle opzioni nella colonna due e una descrizione nella colonna tre." style="width: 100%">
<caption>Opzioni di archiviazione non persistente</caption>
  <thead>
  <th>Opzione</th>
  <th>Descrizione</th>
  </thead>
  <tbody>
    <tr>
      <td>1. All'interno del contenitore o del pod</td>
      <td>I contenitori e i pod sono, come progettati, di breve durata e possono avere un malfunzionamento imprevisto. Tuttavia, puoi scrivere i dati nel file system locale del contenitore per memorizzare i dati tramite il ciclo di vita di un contenitore. I dati all'interno di un contenitore non possono essere condivisi con altri contenitori o pod e vengono persi quando il contenitore ha un arresto anomalo o viene rimosso. Per ulteriori informazioni, consulta [Memorizzazione dei dati in un contenitore](https://docs.docker.com/storage/).</td>
    </tr>
  <tr>
    <td>2. Nel nodo di lavoro</td>
    <td>Ogni nodo di lavoro è configurato con un'archiviazione primaria e secondaria determinata dal tipo di macchina che selezioni per il tuo nodo di lavoro. L'archiviazione primaria viene utilizzata per memorizzare i dati dal sistema operativo e vi si può accedere utilizzando un [volume Kubernetes <code>hostPath</code> ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/storage/volumes/#hostpath). L'archiviazione secondaria viene utilizzata per memorizzare i dati da `kubelet` e dal motore di runtime del contenitore. Puoi accedere all'archiviazione secondaria utilizzando un [volume Kubernetes <code>emptyDir</code> ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/storage/volumes/#emptydir)<br/><br/>Mentre i volumi <code>hostPath</code> vengono utilizzati per montare i file dal file system del nodo di lavoro sul tuo pod, <code>emptyDir</code> crea una directory vuota che viene assegnata a un pod nel tuo cluster. Tutti i contenitori in questo pod possono leggere e scrivere su quel volume. Poiché il volume è assegnato
a un pod specifico, i dati non possono essere condivisi con altri pod in una serie di repliche.<br/><br/><p>Un volume <code>hostPath</code> o <code>emptyDir</code> e i relativi dati vengono rimossi quando: <ul><li>Il nodo di lavoro viene eliminato.</li><li>Il nodo di lavoro viene ricaricato o aggiornato.</li><li>Il cluster viene eliminato.</li><li>L'account {{site.data.keyword.Bluemix_notm}} raggiunge uno stato sospeso. </li></ul></p><p>Inoltre, i dati in un volume <code>emptyDir</code> vengono rimossi quando: <ul><li>Il pod assegnato
viene eliminato definitivamente dal nodo di lavoro.</li><li>Il pod assegnato viene pianificato su un altro nodo di lavoro.</li></ul></p><p><strong>Nota:</strong> se il contenitore nel pod ha un arresto anomalo, i dati
nel volume sono ancora disponibili nel nodo di lavoro.</p></td>
    </tr>
    </tbody>
    </table>


### Opzioni di archiviazione dati persistente per l'elevata disponibilità
{: #persistent}

La sfida principale quando crei le applicazioni con stato altamente disponibili è di conservare i dati tra più istanze dell'applicazione in più ubicazioni e mantenere i dati sempre sincronizzati. Per i dati altamente disponibili, vuoi assicurarti di avere un database master con più istanze che vengono suddivise tra più data center
o anche tra più regioni. Questo database master deve essere continuamente replicato per mantenere una singola fonte di verità. Tutte le istanze nel tuo cluster devono leggere e scrivere in questo database master. Nel caso un'istanza del master sia inattiva, le altre istanze sostengono il carico di lavoro, per cui non riscontri del tempo di inattività per le tue applicazioni.
{: shortdesc}

La seguente immagine mostra le opzioni che hai a disposizione in {{site.data.keyword.containerlong_notm}} per rendere i tuoi dati altamente disponibili in un cluster standard. L'opzione giusta per te dipende dai seguenti fattori:
  * **Il tuo tipo di applicazione:** ad esempio, potresti avere un'applicazione che deve memorizzare i dati su una base file rispetto che all'interno di un database.
  * **I requisiti legali su dove memorizzare e instradare i dati:** ad esempio, potresti essere obbligato a memorizzare e instradare i dati solo negli Stati Uniti e non poter utilizzare un servizio ubicato in Europa.
  * **Le opzioni di backup e ripristino:** ogni opzione di memorizzazione viene fornita con le funzionalità di backup e ripristino dei dati. Controlla che le opzioni di backup e ripristino disponibili soddisfano i tuoi requisiti del piano di ripristino di emergenza, come la frequenza dei backup o le funzionalità di archiviazione dei dati all'esterno del tuo data center primario.
  * **La replica globale:** per l'elevata disponibilità, potresti voler configurare più istanze di memorizzazione distribuite e replicate tra i data center globalmente. 

<br/>
<img src="images/cs_storage_ha.png" alt="Opzioni di elevata disponibilità per l'archiviazione persistente"/>

<table summary="La tabella mostra le opzioni di archiviazione persistente. Le righe devono essere lette da sinistra a destra, con il numero dell'opzione nella colonna uno, il titolo nella colonna due e una descrizione nella colonna tre.">
<caption>Opzioni di archiviazione persistente</caption>
  <thead>
  <th>Opzione</th>
  <th>Descrizione</th>
  </thead>
  <tbody>
  <tr>
  <td>1. Archiviazione file NFS o archiviazione blocchi</td>
  <td>Con questa opzione, puoi conservare i dati del contenitore e dell'applicazione utilizzando i volumi persistenti Kubernetes. I volumi sono ospitati nell'[archiviazione file basata su NFS ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/cloud/file-storage/details) o nell'[archiviazione blocchi ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/cloud/block-storage) Endurance e Performance che possono essere utilizzate per le applicazioni che memorizzano dati su una base file o come un blocco invece che in un database. I dati che vengono salvati nell'archiviazione blocchi e file vengono codificati quando inattivi.<p>{{site.data.keyword.containershort_notm}} fornisce classi di archiviazione predefinite che
definiscono l'intervallo di dimensioni dell'archivio, gli IOPS, la politica di eliminazione e le autorizzazioni di scrittura e lettura per il volume. Per avviare una richiesta per l'archiviazione file o l'archiviazione blocchi, devi creare un'[attestazione del volume persistente (PVC o persistent volume claim)](cs_storage.html#create). Dopo aver inviato una PVC, {{site.data.keyword.containershort_notm}} esegue dinamicamente il provisioning di un volume persistente ospitato sull'archiviazione file basata su NFS o sull'archiviazione blocchi. [Puoi montare la PVC](cs_storage.html#app_volume_mount) come volume nella tua distribuzione per consentire ai contenitori di leggere e scrivere nel volume. </p><p>I volumi persistenti vengono forniti nel data center in cui è ubicato il nodo di lavoro. Puoi condividere i dati tra la stessa serie di repliche o con altre distribuzioni nello stesso cluster. Non puoi condividere i dati tra i cluster quando sono ubicati in differenti data center o regioni. </p><p>Per impostazione predefinita, il backup dell'archiviazione file NFS o dell'archiviazione blocchi non viene eseguito automaticamente. Puoi configurare un backup periodico per il tuo cluster utilizzando i [meccanismi di backup e ripristino](cs_storage.html#backup_restore) forniti. Quando un
contenitore ha un arresto anomalo o viene rimosso un pod da un nodo di lavoro, i dati non vengono rimossi ed è ancora possibile accedervi da altre distribuzioni che montano il volume. </p><p><strong>Nota:</strong> puoi scegliere di essere addebitato per l'archiviazione blocchi e l'archiviazione di condivisione file NFS persistente in modo orario o mensile. Se scegli mensile, quando rimuovi l'archiviazione persistente, pagherai comunque l'addebito mensile per essa, anche se la hai utilizzata solo per un breve periodo di tempo.</p></td>
  </tr>
  <tr id="cloud-db-service">
    <td>2. Servizio database cloud</td>
    <td>Con questa opzione, puoi conservare i dati utilizzando un servizio cloud database {{site.data.keyword.Bluemix_notm}},
come [IBM Cloudant NoSQL DB](/docs/services/Cloudant/getting-started.html#getting-started-with-cloudant). I dati che vengono archiviati con questa opzione sono accessibili nei cluster, nelle ubicazioni e nelle regioni. <p> Puoi scegliere di configurare una sola istanza database a cui possono accedere tutte le tue applicazioni o [configurare più istanze attraverso i data center e la replica](/docs/services/Cloudant/guides/active-active.html#configuring-cloudant-nosql-db-for-cross-region-disaster-recovery) tra le istanze per l'elevata disponibilità. Nel database IBM Cloudant NoSQL, non viene eseguito automaticamente il backup dei dati. Puoi utilizzare i [meccanismi di backup e ripristino](/docs/services/Cloudant/guides/backup-cookbook.html#cloudant-nosql-db-backup-and-recovery) forniti per proteggere i tuoi dati da un malfunzionamento del sito.</p> <p> Per utilizzare un servizio nel tuo cluster, devi [associare il servizio {{site.data.keyword.Bluemix_notm}}](cs_integrations.html#adding_app) a uno spazio dei nomi nel tuo cluster. Quando associ il servizio al cluster, viene creato un segreto Kubernetes. Il segreto Kubernetes ospita informazioni confidenziali sul servizio,
come ad esempio l'URL del servizio, i tuoi nome utente e password. Puoi montare il segreto
come un volume segreto nel tuo pod e accedere al servizio utilizzando le credenziali nel segreto. Montando il volume segreto in altri pod, puoi inoltre condividere i dati tra i pod. Quando un
contenitore ha un arresto anomalo o viene rimosso un pod da un nodo di lavoro, i dati non vengono rimossi ed è ancora possibile accedervi
da altri pod che montano il volume segreto. <p>Alcuni servizi database {{site.data.keyword.Bluemix_notm}} forniscono spazio su disco
per una piccola quantità di dati gratuitamente, in questo modo puoi verificarne le funzioni.</p></td>
  </tr>
  <tr>
    <td>3. Database in loco</td>
    <td>Se i tuoi dati devono essere archiviati in loco per motivi legali, puoi [configurare una connessione VPN](cs_vpn.html#vpn) nel tuo database in loco e utilizzare l'archiviazione esistente, i meccanismi di backup e ripristino nel tuo data center. </td>
  </tr>
  </tbody>
  </table>

{: caption="Tabella. Opzioni di archiviazione dati persistente per le distribuzioni nei cluster Kubernetes" caption-side="top"}

<br />



## Utilizzo di condivisioni file NFS esistenti nei cluster
{: #existing}

Se hai delle condivisioni file NFS esistenti nel tuo account dell'infrastruttura IBM Cloud (SoftLayer) che vuoi utilizzare, puoi farlo creando un volume persistente (o PV, persistent volume) per il tuo spazio di archiviazione esistente.
{:shortdesc}

Un volume persistente (PV) è una risorsa Kubernetes che rappresenta un dispositivo di archiviazione effettivo che viene fornito in un data center. I volumi persistenti astraggono i dettagli su come viene fornito uno specifico tipo di archiviazione da {{site.data.keyword.Bluemix_notm}} Storage. Per montare un PV nel tuo cluster, devi richiedere l'archiviazione persistente per il tuo pod creando un'attestazione del volume persistente (o PVC, persistent volume claim). Il seguente diagramma illustra la relazione tra i PV e le PVC.

![Crea i volumi persistenti e le attestazioni del volume persistente](images/cs_cluster_pv_pvc.png)

Come illustrato nel diagramma, per abilitare l'archiviazione NFS esistente, devi creare PV con determinate dimensioni e modalità di accesso e creare una PVC che corrisponda alla specifica del PV. Se il PV e la PVC corrispondono, vengono associati tra loro. Solo le PVC associate possono essere utilizzate dall'utente del cluster per montare il volume in una distribuzione. Questo processo viene indicato come provisioning statico di archiviazione persistente.

Prima di iniziare, assicurati di avere una condivisione file NFS esistente da utilizzare per creare il tuo PV. Ad esempio, se in precedenza [hai creato una PVC con una politica della classe di archiviazione `retain`](#create), puoi utilizzare quei messaggi conservati nella condivisione file NFS esistente per questa nuova PVC.

**Nota:** il provisioning statico di archiviazione persistente viene applicato solo alle condivisioni file NFS esistenti. Se non disponi di condivisioni file NFS esistenti, gli utenti del cluster possono utilizzare il processo di [provisioning dinamico](cs_storage.html#create) per aggiungere i PV.

Per creare un PV e una PVC corrispondente, completa la seguente procedura.

1.  Nel tuo account dell'infrastruttura IBM Cloud (SoftLayer), cerca l'ID e il percorso della condivisione file NFS in cui vuoi creare l'oggetto del PV. In aggiunta, autorizza l'archiviazione file per le sottoreti nel cluster. Questa autorizzazione fornisce al tuo cluster l'accesso all'archivio.
    1.  Accedi al tuo account dell'infrastruttura IBM Cloud (SoftLayer).
    2.  Fai clic su **Archiviazione**.
    3.  Fai clic su **Archiviazione file** e dal menu **Azioni**, seleziona **Autorizza host**.
    4.  Seleziona **Sottoreti**.
    5.  Dall'elenco a discesa, seleziona la sottorete VLAN privata a cui è connesso il nodo di lavoro. Per trovare la sottorete del tuo nodo di lavoro, esegui `bx cs workers <cluster_name>` e confronta l'`IP privato` del tuo nodo di lavoro con la sottorete che hai trovato nell'elenco a discesa. 
    6.  Fai clic su **Invia**.
    6.  Fai clic sul nome dell'archiviazione file.
    7.  Prendi nota del campo **Punto di montaggio**. Il campo viene visualizzato come `<server>:/<path>`.
2.  Crea un file di configurazione dell'archiviazione per il tuo PV. Includi il server e il percorso dal campo **punto di montaggio** dell'archiviazione file.

    ```
    apiVersion: v1
    kind: PersistentVolume
    metadata:
     name: mypv
    spec:
     capacity:
       storage: "20Gi"
     accessModes:
       - ReadWriteMany
     nfs:
       server: "nfslon0410b-fz.service.networklayer.com"
       path: "/IBM01SEV8491247_0908/data01"
    ```
    {: codeblock}

    <table>
    <caption>Descrizione dei componenti del file YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del file YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code>name</code></td>
    <td>Immetti il nome dell'oggetto del PV da creare. </td>
    </tr>
    <tr>
    <td><code>spec/capacity/storage</code></td>
    <td>Immetti la dimensione di archiviazione della condivisione file NFS esistente. La dimensione di archiviazione deve essere scritta in gigabyte, ad esempio, 20Gi (20 GB) o 1000Gi (1 TB) e deve corrispondere alla dimensione della condivisione file esistente.</td>
    </tr>
    <tr>
    <td><code>accessMode</code></td>
    <td>Le modalità di accesso definiscono il modo in cui la PVC può essere montata in un nodo di lavoro.<ul><li>ReadWriteOnce (RWO): il PV può essere montato nelle distribuzioni in un solo nodo di lavoro. I contenitori nelle distribuzioni montate su questo PV possono leggere e scrivere nel volume.</li><li>ReadOnlyMany (ROX): il PV può essere montato nelle distribuzioni ospitate su più nodi di lavoro. Le distribuzioni montate in questo PV possono solo leggere dal volume. </li><li>ReadWriteMany (RWX): questo PV può essere montato nelle distribuzioni ospitate su più nodi di lavoro. Le distribuzioni montate in questo PV possono leggere e scrivere nel volume.</li></ul></td>
    </tr>
    <tr>
    <td><code>spec/nfs/server</code></td>
    <td>Immetti l'ID server della condivisione file NFS.</td>
    </tr>
    <tr>
    <td><code>path</code></td>
    <td>Immetti il percorso della condivisione file NFS in cui vuoi creare l'oggetto del PV.</td>
    </tr>
    </tbody></table>

3.  Crea l'oggetto del PV nel tuo cluster.

    ```
    kubectl apply -f deploy/kube-config/mypv.yaml
    ```
    {: pre}

4.  Verifica che il PV sia stato creato.

    ```
    kubectl get pv
    ```
    {: pre}

5.  Crea un altro file di configurazione per creare la tua PVC. Affinché la PVC corrisponda all'oggetto del PV creato in precedenza, devi scegliere lo stesso valore per `storage` e `accessMode`. Il campo `storage-class` deve essere vuoto. Se uno di questi campi non corrisponde al PV, verrà creato automaticamente un nuovo PV.

    ```
    kind: PersistentVolumeClaim
    apiVersion: v1
    metadata:
     name: mypvc
     annotations:
       volume.beta.kubernetes.io/storage-class: ""
    spec:
     accessModes:
       - ReadWriteMany
     resources:
       requests:
         storage: "20Gi"
    ```
    {: codeblock}

6.  Crea la tua PVC.

    ```
    kubectl apply -f deploy/kube-config/mypvc.yaml
    ```
    {: pre}

7.  Verifica che la tua PVC sia stata creata e associata all'oggetto del PV. Questo processo può richiedere qualche minuto.

    ```
    kubectl describe pvc mypvc
    ```
    {: pre}

    Output di esempio:

    ```
    Name: mypvc
    Namespace: default
    StorageClass: ""
    Status: Bound
    Volume: pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    Labels: <none>
    Capacity: 20Gi
    Access Modes: RWX
    Events:
      FirstSeen LastSeen Count From        SubObjectPath Type Reason Message
      --------- -------- ----- ----        ------------- -------- ------ -------
      3m 3m 1 {ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 } Normal Provisioning External provisioner is provisioning volume for claim "default/my-persistent-volume-claim"
      3m 1m	 10 {persistentvolume-controller } Normal ExternalProvisioning cannot find provisioner "ibm.io/ibmc-file", expecting that a volume for the claim is provisioned either manually or via external software
      1m 1m 1 {ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 } Normal ProvisioningSucceeded	Successfully provisioned volume pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    ```
    {: screen}


Hai creato correttamente un oggetto del PV e lo hai associato a una PVC. Gli utenti del cluster possono ora [montare la PVC](#app_volume_mount) nelle proprie distribuzioni e iniziare a leggere e a scrivere sull'oggetto del PV.

<br />



## Utilizzo dell'archiviazione blocchi esistente nel tuo cluster
{: #existing_block}

Prima di iniziare, assicurati di avere un'istanza di archiviazione blocchi esistente che puoi utilizzare per creare il tuo PV: Ad esempio, se in precedenza [hai creato una PVC con una politica della classe di archiviazione `retain`](#create), puoi utilizzare quei dati conservati nel blocco di archiviazione blocchi esistente per questa nuova PVC.

**Nota**: l'archiviazione blocchi è un dispositivo di modalità di accesso `ReadWriteOnce`. Puoi montarlo su un solo pod in un nodo di lavoro nel cluster alla volta.

Per creare un PV e una PVC corrispondente, completa la seguente procedura.

1.  Recupera o genera una chiave API per il tuo account dell'infrastruttura IBM Cloud (SoftLayer).
    1. Accedi al [portale dell'infrastruttura IBM Cloud (SoftLayer) ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/).
    2. Seleziona **Account**, quindi **Utenti** e quindi **Elenco utenti**.
    3. Trova il tuo ID utente.
    4. Nella colonna **Chiave API**, fai clic su **Genera** per generare una chiave API oppure su **Visualizza** per visualizzare la tua chiave API esistente.
2.  Recupera il nome utente API per il tuo account dell'infrastruttura IBM Cloud (SoftLayer).
    1. Dal menu **Elenco utenti**, seleziona il tuo ID utente.
    2. Nella sezione **Informazioni di accesso API**, trova il tuo **Nome utente API**.
3.  Accedi al plug-in della CLI dell'infrastruttura IBM Cloud.
    ```
    bx sl init
    ```
    {: pre}

4.  Scegli di eseguire l'autenticazione utilizzando il nome utente e la chiave API per il tuo account dell'infrastruttura IBM Cloud (SoftLayer).
5.  Immetti il nome utente e la chiave API che hai recuperato nei passi precedenti.
6.  Elenca i dispositivi di archiviazione blocchi disponibili.
    ```
    bx sl block volume-list
    ```
    {: pre}

    Output di esempio:
    ```
    id         username            datacenter   storage_type              capacity_gb   bytes_used   ip_addr         lunId   active_transactions
    38642141   IBM02SEL1543159-1   dal10        endurance_block_storage   20            -            169.xx.xxx.xxx   170     0
    ```
    {: screen}

7.  Prendi nota di `id`, `ip_addr`, `capacity_gb` e `lunId` del dispositivo di archiviazione blocchi in cui desideri montare il tuo cluster. 
8.  Crea un file di configurazione per il tuo PV. Includi l'ID di archiviazione blocchi, l'indirizzo IP, la dimensione e l'ID lun che hai recuperato nel passo precedente. 

    ```
    apiVersion: v1
    kind: PersistentVolume
    metadata:
      name: mypv
    spec:
      capacity:
        storage: "<storage_size>"
      accessModes:
        - ReadWriteOnce
      flexVolume:
        driver: "ibm/ibmc-block"
        fsType: "<fs_type>"
        options:
          "Lun": "<lun_ID>"
          "TargetPortal": "<IP_address>"
          "VolumeID": "<volume_ID>"
          "volumeName": "<volume_name>"
      ```
      {: codeblock}

    <table>
    <caption>Descrizione dei componenti del file YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del file YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code>metadata/name</code></td>
    <td>Immetti il nome del PV che desideri creare.</td>
    </tr>
    <tr>
    <td><code>spec/flexVolume/fsType</code></td> 
    <td>Immetti il tipo di file system configurato per la tua archiviazione blocchi esistente. Scegli tra <code>ext4</code> o <code>xfs</code>. Se non specifichi questa opzione, viene utilizzato il valore predefinito del PV <code>ext4</code>. Quando viene definito il fsType sbagliato, la creazione del PV ha esito positivo ma il montaggio del PV in un pod ha esito negativo. </td></tr>	    
    <tr>
    <td><code>spec/capacity/storage</code></td>
    <td>Immetti la dimensione di archiviazione dell'archiviazione blocchi esistente che hai recuperato nel passo precedente come <code>capacity-gb</code>. La dimensione di archiviazione deve essere scritta in gigabyte, ad esempio, 20Gi (20 GB) o 1000Gi (1 TB).</td>
    </tr>
    <tr>
    <td><code>flexVolume/options/Lun</code></td>
    <td>Immetti l'ID lun per la tua archiviazione blocchi che hai recuperato nel passo precedente come <code>lunId</code>.</td>
    </tr>
    <tr>
    <td><code>flexVolume/options/TargetPortal</code></td>
    <td>Immetti l'indirizzo IP della tua archiviazione blocchi che hai recuperato nel passo precedente come <code>ip_addr</code>. </td>
    </tr>
    <tr>
	    <td><code>flexVolume/options/VolumeId</code></td>
	    <td>Immetti l'ID dell'archiviazione blocchi che hai recuperato nel passo precedente come <code>id</code>.</td>
	    </tr>
	    <tr>
		    <td><code>flexVolume/options/volumeName</code></td>
		    <td>Immetti un nome per il tuo volume.</td>
	    </tr>
    </tbody></table>

9.  Crea il PV nel tuo cluster.
    ```
    kubectl apply -f mypv.yaml
    ```
    {: pre}

10. Verifica che il PV sia stato creato.
    ```
    kubectl get pv
    ```
    {: pre}

11. Crea un altro file di configurazione per creare la tua PVC. Affinché la PVC corrisponda al PV che hai creato in precedenza, devi scegliere lo stesso valore per `storage` e `accessMode`. Il campo `storage-class` deve essere vuoto. Se uno di questi campi non corrisponde al PV, verrà creato automaticamente un nuovo PV.

     ```
     kind: PersistentVolumeClaim
    apiVersion: v1
    metadata:
     name: mypvc
     annotations:
       volume.beta.kubernetes.io/storage-class: ""
     spec:
      accessModes:
        - ReadWriteOnce
      resources:
        requests:
          storage: "20Gi"
     ```
     {: codeblock}

12.  Crea la tua PVC.
     ```
     kubectl apply -f mypvc.yaml
     ```
     {: pre}

13.  Verifica che la tua PVC sia stata creata e collegata al PV che hai creato in precedenza. Questo processo può richiedere qualche minuto.
     ```
     kubectl describe pvc mypvc
     ```
     {: pre}

     Output di esempio:

     ```
     Name: mypvc
    Namespace: default
    StorageClass: ""
     Status: Bound
     Volume: pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
     Labels: <none>
     Capacity: 20Gi
     Access Modes: RWO
     Events:
       FirstSeen LastSeen Count From        SubObjectPath Type Reason Message
       --------- -------- ----- ----        ------------- -------- ------ -------
       3m 3m 1 {ibm.io/ibmc-block 31898035-3011-11e7-a6a4-7a08779efd33 } Normal Provisioning External provisioner is provisioning volume  for claim "default/my-persistent-volume-claim"
       3m 1m	 10 {persistentvolume-controller } Normal ExternalProvisioning cannot find provisioner "ibm.io/ibmc-block", expecting that  a volume for the claim is provisioned either manually or via external software
       1m 1m 1 {ibm.io/ibmc-block 31898035-3011-11e7-a6a4-7a08779efd33 } Normal ProvisioningSucceeded	Successfully provisioned volume  pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
     ```
     {: screen}

Hai creato correttamente un PV e lo hai collegato ad una PVC. Gli utenti del cluster ora possono [montare la PVC](#app_volume_mount) nelle loro distribuzioni e iniziare a leggere e a scrivere nel PV.

<br />



## Aggiunta dell'archiviazione file NFS o dell'archiviazione blocchi alle applicazioni
{: #create}

Crea un'attestazione del volume persistente (o PVC, persistent volume claim) per eseguire il provisioning dell'archiviazione file NFS o dell'archiviazione blocchi per il tuo cluster. Quindi, monta questa attestazione in un volume persistente (o PV, persistent volume) per garantire che i dati siano disponibili anche se i pod si bloccano o si arrestano.
{:shortdesc}

L'archiviazione file NFS e l'archiviazione blocchi che supportano il PV vengono organizzate in cluster da IBM per fornire l'alta disponibilità per i tuoi dati. Le classi di archiviazione descrivono i tipi di offerte di archiviazione disponibili e definiscono aspetti come la politica di conservazione dei dati, le dimensioni in gigabyte e IOPS quando crei il tuo PV.

**Nota**: l'archiviazione blocchi è un dispositivo di modalità di accesso `ReadWriteOnce`. Puoi montarlo su un solo pod in un nodo di lavoro nel cluster alla volta. L'archiviazione file NFS è una modalità di accesso `ReadWriteMany`, per cui puoi montarla su più pod tra i nodi di lavoro all'interno di un cluster.

Prima di iniziare:
- Se hai un firewall, [consenti l'accesso in uscita](cs_firewall.html#pvc) per gli intervalli IP dell'infrastruttura IBM Cloud (SoftLayer) delle ubicazioni in cui si trovano i tuoi cluster, in modo da poter creare le PVC. 
- Se desideri montare l'archiviazione blocchi nelle tue applicazioni, devi installare innanzitutto il plug-in [{{site.data.keyword.Bluemix_notm}} Storage per l'archiviazione blocchi](#install_block).

Per aggiungere l'archiviazione persistente:

1.  Rivedi le classi di archiviazione disponibili. {{site.data.keyword.containerlong}} fornisce classi di archiviazione predefinite per l'archiviazione file NFS e l'archiviazione blocchi in modo che l'amministratore del cluster non debba creare classi di archiviazione. La classe di archiviazione `ibmc-file-bronze` è uguale a `default`. Per impostazione predefinita, l'archiviazione file viene fornita con un file system `nfs` e l'archiviazione blocchi con `ext4`. Se vuoi eseguire il provisioning dell'archiviazione blocchi con un file system `XFS`, [crea la tua classe di archiviazione personalizzata](#custom_storageclass). 

    ```
    kubectl get storageclasses
    ```
    {: pre}

    ```
    $ kubectl get storageclasses
    NAME                         TYPE
    default                      ibm.io/ibmc-file
    ibmc-file-bronze (default)   ibm.io/ibmc-file
    ibmc-file-custom             ibm.io/ibmc-file
    ibmc-file-gold               ibm.io/ibmc-file
    ibmc-file-retain-bronze      ibm.io/ibmc-file
    ibmc-file-retain-custom      ibm.io/ibmc-file
    ibmc-file-retain-gold        ibm.io/ibmc-file
    ibmc-file-retain-silver      ibm.io/ibmc-file
    ibmc-file-silver             ibm.io/ibmc-file
    ibmc-block-custom            ibm.io/ibmc-block
    ibmc-block-bronze            ibm.io/ibmc-block
    ibmc-block-gold              ibm.io/ibmc-block
    ibmc-block-silver            ibm.io/ibmc-block
    ibmc-block-retain-bronze     ibm.io/ibmc-block
    ibmc-block-retain-silver     ibm.io/ibmc-block
    ibmc-block-retain-gold       ibm.io/ibmc-block
    ibmc-block-retain-custom     ibm.io/ibmc-block
    ```
    {: screen}

    **Suggerimento:** se vuoi modificare la classe di archiviazione predefinita, esegui `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'` e sostituisci `<storageclass>` con il nome della classe di archiviazione.

2.  Decidi se desideri conservare i tuoi dati e la condivisione file NFS o l'archiviazione blocchi una volta eliminata la PVC.
    - Se vuoi conservare i tuoi dati, scegli una classe di archiviazione `retain`. Quando elimini il PVC, viene eliminato solo il PVC. Il PV è ancora presente nei dati in cui è salvato, ma non è possibile riutilizzarlo con un altro PVC. Inoltre, il file NFS o l'archiviazione blocchi e i tuoi dati saranno ancora presenti nel tuo account dell'infrastruttura IBM Cloud (SoftLayer). Successivamente, per accedere a questi dati nel tuo cluster, crea una PVC e un PV corrispondente che faccia riferimento al tuo [file NFS](#existing) o alla tua archiviazione [blocchi](#existing_block) esistente. 
    - Se vuoi che il PV, i dati e la tua archiviazione blocchi o di condivisione file NFS vengano eliminati quando elimini la PVC, scegli una classe di archiviazione senza `retain`.

3.  **Se scegli una classe di archiviazione bronze, silver o gold**: ottieni l'[archiviazione Endurance ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://knowledgelayer.softlayer.com/topic/endurance-storage) che definisce gli IOPS per GB per ogni classe. Tuttavia, puoi determinare il totale di IOPS scegliendo la dimensione nell'intervallo disponibile. Puoi selezionare qualsiasi numero intero di dimensioni di gigabyte entro l'intervallo di dimensioni consentito (come 20 Gi, 256 Gi, 11854 Gi). Ad esempio, se selezioni una dimensione di condivisione file o di archiviazione blocchi di 1000Gi nella classe di archiviazione silver di 4 IOPS per GB, il tuo volume avrà un totale di 4000 IOPS. Più IOPS ha il tuo PV, più velocemente elabora le operazioni di input e output. La seguente tabella descrive gli IOPS per gigabyte e l'intervallo di dimensioni per ogni classe di archiviazione.

    <table>
         <caption>Tabella di intervalli di dimensioni della classe di archiviazione e IOPS per gigabyte</caption>
         <thead>
         <th>Classe di archiviazione</th>
         <th>IOPS per gigabyte</th>
         <th>Intervallo di dimensioni in gigabyte</th>
         </thead>
         <tbody>
         <tr>
         <td>Bronze (predefinito)</td>
         <td>2 IOPS/GB</td>
         <td>20-12000 Gi</td>
         </tr>
         <tr>
         <td>Silver</td>
         <td>4 IOPS/GB</td>
         <td>20-12000 Gi</td>
         </tr>
         <tr>
         <td>Gold</td>
         <td>10 IOPS/GB</td>
         <td>20-4000 Gi</td>
         </tr>
         </tbody></table>

    <p>**Comando di esempio per mostrare i dettagli di una classe di archiviazione**:</p>

    <pre class="pre"><code>kubectl describe storageclasses ibmc-file-silver</code></pre>

4.  **Se scegli la classe di archiviazione personalizzata**: ottieni l'[archiviazione Performance ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://knowledgelayer.softlayer.com/topic/performance-storage) e hai maggiore controllo sulla scelta della combinazione di IOPS e dimensioni. Ad esempio, se selezioni una dimensione di 40Gi per la PVC, puoi scegliere un IOPS multiplo di 100 compreso nell'intervallo 100-2000 IOPS. Gli IOPS che scegli sono statici e non ridimensionano l'archiviazione. Se scegli 40Gi con 100 IOPS, i tuoi IOPS totali rimangono 100. La seguente tabella mostra quale intervallo di IOPS puoi scegliere in base alla dimensione selezionata.

    <table>
         <caption>Tabella di intervalli di dimensioni della classe di archiviazione personalizzata e IOPS</caption>
         <thead>
         <th>Intervallo di dimensioni in gigabyte</th>
         <th>Intervallo di IOPS in multipli di 100</th>
         </thead>
         <tbody>
         <tr>
         <td>20-39 Gi</td>
         <td>100-1000 IOPS</td>
         </tr>
         <tr>
         <td>40-79 Gi</td>
         <td>100-2000 IOPS</td>
         </tr>
         <tr>
         <td>80-99 Gi</td>
         <td>100-4000 IOPS</td>
         </tr>
         <tr>
         <td>100-499 Gi</td>
         <td>100-6000 IOPS</td>
         </tr>
         <tr>
         <td>500-999 Gi</td>
         <td>100-10000 IOPS</td>
         </tr>
         <tr>
         <td>1000-1999 Gi</td>
         <td>100-20000 IOPS</td>
         </tr>
         <tr>
         <td>2000-2999 Gi</td>
         <td>200-40000 IOPS</td>
         </tr>
         <tr>
         <td>3000-3999 Gi</td>
         <td>200-48000 IOPS</td>
         </tr>
         <tr>
         <td>4000-7999 Gi</td>
         <td>300-48000 IOPS</td>
         </tr>
         <tr>
         <td>8000-9999 Gi</td>
         <td>500-48000 IOPS</td>
         </tr>
         <tr>
         <td>10000-12000 Gi</td>
         <td>1000-48000 IOPS</td>
         </tr>
         </tbody></table>

    <p>**Comando di esempio per mostrare i dettagli di una classe di archiviazione personalizzata**:</p>

    <pre class="pre"><code>kubectl describe storageclasses ibmc-file-retain-custom</code></pre>

5.  Decidi se ricevere una fattura su base oraria o mensile. Per impostazione predefinita, la fatturazione è mensile.

6.  Crea un file di configurazione per definire la tua PVC e salva la configurazione come file `.yaml`.

    -  **Esempio per le classi di archiviazione bronze, silver, gold**:
       Il seguente file `.yaml` crea un'attestazione denominata `mypvc` della classe di archiviazione `"ibmc-file-silver"`, con fatturazione oraria (`"hourly"`) e una dimensione in gigabyte di `24Gi`. Se desideri creare una PVC per montare l'archiviazione blocchi nel tuo cluster, assicurati di immettere `ReadWriteOnce` nella sezione `accessModes`.

       ```
       apiVersion: v1
       kind: PersistentVolumeClaim
       metadata:
         name: mypvc
         annotations:
           volume.beta.kubernetes.io/storage-class: "ibmc-file-silver"
         labels:
           billingType: "hourly"
       spec:
         accessModes:
           - ReadWriteMany
         resources:
           requests:
             storage: 24Gi
        ```
        {: codeblock}

    -  **Esempio per le classi di archiviazione personalizzate**:
       Il seguente file `.yaml` crea un'attestazione denominata `mypvc` della classe di archiviazione `ibmc-file-retain-custom`, con fatturazione mensile (`"monthly"`) per impostazione predefinita, una dimensione in gigabyte di `45Gi` e IOPS di `"300"`. Se desideri creare una PVC per montare l'archiviazione blocchi nel tuo cluster, assicurati di immettere `ReadWriteOnce` nella sezione `accessModes`.

       ```
       apiVersion: v1
       kind: PersistentVolumeClaim
       metadata:
         name: mypvc
         annotations:
           volume.beta.kubernetes.io/storage-class: "ibmc-file-retain-custom"
         labels:
           billingType: "monthly"
       spec:
         accessModes:
           - ReadWriteMany
         resources:
           requests:
             storage: 45Gi
             iops: "300"
        ```
        {: codeblock}

        <table>
        <caption>Descrizione dei componenti del file YAML</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del file YAML</th>
        </thead>
        <tbody>
        <tr>
        <td><code>metadata/name</code></td>
        <td>Immetti il nome della PVC.</td>
        </tr>
        <tr>
        <td><code>metadata/annotations</code></td>
        <td>Specifica la classe di archiviazione per il PV:
          <ul>
          <li>ibmc-file-bronze / ibmc-file-retain-bronze : 2 IOPS per GB.</li>
          <li>ibmc-file-silver / ibmc-file-retain-silver: 4 IOPS per GB.</li>
          <li>ibmc-file-gold / ibmc-file-retain-gold: 10 IOPS per GB.</li>
          <li>ibmc-file-custom / ibmc-file-retain-custom: più valori di IOPS disponibili.</li>
          <li>ibmc-block-bronze / ibmc-block-retain-bronze : 2 IOPS per GB.</li>
          <li>ibmc-block-silver / ibmc-block-retain-silver: 4 IOPS per GB.</li>
          <li>ibmc-block-gold / ibmc-block-retain-gold: 10 IOPS per GB.</li>
          <li>ibmc-block-custom / ibmc-block-retain-custom: più valori di IOPS disponibili.</li></ul>
          <p>Se non specifichi alcuna classe di archiviazione, il PV viene creato con la classe di archiviazione predefinita.</p><p>**Suggerimento:** se vuoi modificare la classe di archiviazione predefinita, esegui <code>kubectl patch storageclass &lt;storageclass&gt; -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'</code> e sostituisci <code>&lt;storageclass&gt;</code> con il nome della classe di archiviazione.</p></td>
        </tr>
        <tr>
          <td><code>metadata/labels/billingType</code></td>
          <td>Specifica la frequenza per la quale viene calcolata la fattura di archiviazione, "mensile" o "oraria". Il valore predefinito è "mensile".</td>
        </tr>
        <tr>
        <td><code>spec/resources/requests/storage</code></td>
        <td>Immetti la dimensione dell'archiviazione file, in gigabyte (Gi). Scegli un numero intero entro l'intervallo di dimensioni consentite. </br></br><strong>Nota: </strong> una volta completato il provisioning della tua archiviazione, non puoi modificare la dimensione della condivisione file NFS o della tua archiviazione blocchi. Assicurati di specificare una dimensione che corrisponda alla quantità di dati che desideri memorizzare. </td>
        </tr>
        <tr>
        <td><code>spec/resources/requests/iops</code></td>
        <td>Questa opzione è valida solo per le classi di archiviazione personalizzate (`ibmc-file-custom / ibmc-file-retain-custom / ibmc-block-custom / ibmc-block-retain-custom`). Specifica l'IOPS totale per l'archiviazione, selezionando un multiplo di 100 nell'intervallo consentito. Per visualizzare tutte le opzioni, esegui `kubectl describe storageclasses <storageclass>`. Se scegli un IOPS diverso da quello elencato, viene arrotondato per eccesso.</td>
        </tr>
        </tbody></table>
	
    Se vuoi utilizzare una classe di archiviazione personalizzata, crea il tuo PVC con il nome della classe di archiviazione corrispondente, una dimensione e un IOPS validi.   
    {: tip}

7.  Crea la PVC.

    ```
    kubectl apply -f mypvc.yaml
    ```
    {: pre}

8.  Verifica che la tua PVC sia stata creata e associata al PV. Questo processo può richiedere qualche minuto.

    ```
    kubectl describe pvc mypvc
    ```
    {: pre}

    Output di esempio:

    ```
    Name: mypvc
    Namespace: default
    StorageClass: ""
    Status:		Bound
    Volume:		pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    Labels:		<none>
    Capacity:	20Gi
    Access Modes:	RWX
    Events:
      FirstSeen	LastSeen	Count	From								SubObjectPath	Type		Reason			Message
      ---------	--------	-----	----								-------------	--------	------			-------
      3m		3m		1	{ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 }			Normal		Provisioning		External provisioner is provisioning volume for claim "default/my-persistent-volume-claim"
      3m		1m		10	{persistentvolume-controller }							Normal		ExternalProvisioning	cannot find provisioner "ibm.io/ibmc-file", expecting that a volume for the claim is provisioned either manually or via external software
      1m		1m		1	{ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 }			Normal		ProvisioningSucceeded	Successfully provisioned volume pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2

    ```
    {: screen}

9.  {: #app_volume_mount}Per montare la PVC nella tua distribuzione, crea un file di configurazione `.yaml`.

    ```
    apiVersion: apps/v1beta1
    kind: Deployment
    metadata:
      name: <deployment_name>
      labels:
        app: <deployment_label>
    spec:
      selector:
        matchLabels:
          app: <app_name>
      template:
        metadata:
          labels:
            app: <app_name>
        spec:
          containers:
          - image: <image_name>
            name: <container_name>
            volumeMounts:
            - name: <volume_name>
              mountPath: /<file_path>
          volumes:
          - name: <volume_name>
            persistentVolumeClaim:
              claimName: <pvc_name>
    ```
    {: codeblock}

    <table>
    <caption>Descrizione dei componenti del file YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del file YAML</th>
    </thead>
    <tbody>
        <tr>
    <td><code>metadata/labels/app</code></td>
    <td>Un'etichetta per la distribuzione.</td>
      </tr>
      <tr>
        <td><code>spec/selector/matchLabels/app</code> <br/> <code>spec/template/metadata/labels/app</code></td>
        <td>Un'etichetta per la tua applicazione.</td>
      </tr>
    <tr>
    <td><code>template/metadata/labels/app</code></td>
    <td>Un'etichetta per la distribuzione.</td>
      </tr>
    <tr>
    <td><code>spec/containers/image</code></td>
    <td>Il nome dell'immagine che vuoi utilizzare. Per elencare le immagini disponibili nel tuo account {{site.data.keyword.registryshort_notm}}, esegui `bx cr image-list`.</td>
    </tr>
    <tr>
    <td><code>spec/containers/name</code></td>
    <td>Il nome del contenitore che vuoi distribuire al tuo cluster.</td>
    </tr>
    <tr>
    <td><code>spec/containers/volumeMounts/mountPath</code></td>
    <td>Il percorso assoluto della directory in cui viene montato il volume nel contenitore.</td>
    </tr>
    <tr>
    <td><code>spec/containers/volumeMounts/name</code></td>
    <td>Il nome del volume per montare il tuo pod.</td>
    </tr>
    <tr>
    <td><code>volumes/name</code></td>
    <td>Il nome del volume per montare il tuo pod. Normalmente questo nome è lo stesso di <code>volumeMounts/name</code>.</td>
    </tr>
    <tr>
    <td><code>volumes/persistentVolumeClaim/claimName</code></td>
    <td>Il nome della PVC che vuoi utilizzare come volume. Quando monti il volume nel pod, Kubernetes identifica il PV associato alla PVC e consente all'utente di leggere e scrivere nel PV.</td>
    </tr>
    </tbody></table>

10.  Crea la distribuzione e monta la PVC.
     ```
     kubectl apply -f <local_yaml_path>
     ```
     {: pre}

11.  Verifica che il volume sia stato correttamente montato.

     ```
     kubectl describe deployment <deployment_name>
     ```
     {: pre}

     Il punto di montaggio è nel campo **Montaggi volume** e il volume nel campo **Volumi**.

     ```
      Volume Mounts:
          /var/run/secrets/kubernetes.io/serviceaccount from default-token-tqp61 (ro)
          /volumemount from myvol (rw)
     ...
     Volumes:
      myvol:
        Type: PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
        ClaimName: mypvc
        ReadOnly: false
     ```
     {: screen}

{: #nonroot}
{: #enabling_root_permission}

**Autorizzazioni NFS**: stai cercando la documentazione relativa all'abilitazione delle autorizzazioni non root NFS? Consulta [Aggiunta di accesso utente non root all'archiviazione file NFS](cs_troubleshoot_storage.html#nonroot).

<br />

	
## Personalizzazione di una classe di archiviazione per l'archiviazione blocchi XFS
{: #custom_storageclass}

Per le classi di archiviazione predefinite da {{site.data.keyword.containerlong}} fornisce l'archiviazione blocchi con un file system `ext4` per impostazione predefinita. Puoi creare una classe di archiviazione personalizzata per eseguire il provisioning dell'archiviazione blocchi con un file system `XFS`.
{: shortdesc}

Prima di iniziare: 
- [Indirizza la CLI Kubernetes al
cluster](cs_cli_install.html#cs_cli_configure).
- Installa il [plugin {{site.data.keyword.Bluemix_notm}} Storage per l'archiviazione blocchi](#install_block).

Per creare una classe di archiviazione personalizzata:  
1. Crea un file yaml per la tua classe di archiviazione personalizzata. 
   ```
   apiVersion: storage.k8s.io/v1
   kind: StorageClass
   metadata:
     name: ibmc-block-custom-xfs
     labels:
       addonmanager.kubernetes.io/mode: Reconcile
   provisioner: ibm.io/ibmc-block
   parameters:
     type: "Performance"
     sizeIOPSRange: |-
       [20-39]Gi:[100-1000]
       [40-79]Gi:[100-2000]
       [80-99]Gi:[100-4000]
       [100-499]Gi:[100-6000]
       [500-999]Gi:[100-10000]
       [1000-1999]Gi:[100-20000]
       [2000-2999]Gi:[200-40000]
       [3000-3999]Gi:[200-48000]
       [4000-7999]Gi:[300-48000]
       [8000-9999]Gi:[500-48000]
       [10000-12000]Gi:[1000-48000]
     fsType: "xfs"
     reclaimPolicy: "Delete"
     classVersion: "2"
   ```
   {: codeblock}
   
   Se vuoi conservare i dati dopo aver rimosso l'archiviazione blocchi dal tuo cluster, modifica `reclaimPolicy` con `Retain`.
   {: tip}
   
2. Crea la classe di archiviazione nel tuo cluster.  
   ```
   kubectl apply -f <filepath/xfs_storageclass.yaml>
   ```
   {: pre}
       
3. Verifica che la classe di archiviazione personalizzata sia stata creata. 
   ```
   kubectl get storageclasses
   ```
   {: pre}

4. Esegui il provisioning dell'[archiviazione blocchi XFS](#create) con la tua classe di archiviazione personalizzata. 

<br />


## Modifica della versione dell'archiviazione file NFS predefinita
{: #nfs_version}

La versione dell'archiviazione file NFS determina il protocollo che viene utilizzato per comunicare con il server dell'archiviazione file NFS. Per impostazione predefinita, le istanze dell'archiviazione file sono configurate con NFS versione 4. Puoi modificare il PV esistente con una versione precedente di NFS se la tua applicazione richiede una versione specifica per funzionare correttamente.
{: shortdesc}

Per modificare la versione NFS predefinita, puoi creare una nuova classe di archiviazione per fornire l'archiviazione file in modo dinamico al tuo cluster oppure scegliere un PV esistente montato nel tuo pod. 

Per applicare gli ultimi aggiornamenti di sicurezza e per prestazioni migliori, utilizza la versione NFS predefinita e non modificare NFS ad una versione precedente.
{: tip}

**Per creare una classe di archiviazione personalizzata con la versione NFS desiderata:**
1. Crea un file yaml per la tua classe di archiviazione personalizzata. Sostituisci <nfs_version> con la versione NFS che vuoi utilizzare. Ad esempio, per fornire la versione NFS 3.0, immetti **3.0**.
   ```
   apiVersion: storage.k8s.io/v1
   kind: StorageClass
   metadata:
     name: ibmc-file-mount
     #annotations:
     #  storageclass.beta.kubernetes.io/is-default-class: "true"
     labels:
       kubernetes.io/cluster-service: "true"
   provisioner: ibm.io/ibmc-file
   parameters:
     type: "Endurance"
     iopsPerGB: "2"
     sizeRange: "[1-12000]Gi"
     reclaimPolicy: "Delete"
     classVersion: "2"
     mountOptions: nfsvers=<nfs_version>
   ```
   {: codeblock}
   
   Se vuoi conservare i dati dopo aver rimosso l'archiviazione blocchi dal tuo cluster, modifica `reclaimPolicy` con `Retain`.
   {: tip}
   
2. Crea la classe di archiviazione nel tuo cluster.  
   ```
   kubectl apply -f <filepath/nfsversion_storageclass.yaml>
   ```
   {: pre}
       
3. Verifica che la classe di archiviazione personalizzata sia stata creata. 
   ```
   kubectl get storageclasses
   ```
   {: pre}

4. Esegui il provisioning dell'[archiviazione file](#create) con la tua classe di archiviazione personalizzata.  

**Per modificare il tuo PV esistente in modo che utilizzi una versione NFS differente:**

1. Ottieni il PV dell'archiviazione file di cui vuoi modificare la versione NFS e prendi nota del nome del PV.
   ```
   kubectl get pv
   ```
   {: pre}

2. Aggiungi un'annotazione al tuo PV. Sostituisci `<version_number>` con la versione NFS che vuoi utilizzare. Ad esempio per modificare la versione NFS con la 3.0, immetti **3**.  
   ```
   kubectl patch pv <pv_name> -p '{"metadata": {"annotations":{"volume.beta.kubernetes.io/mount-options":"vers=<version_number>"}}}'
   ```
   {: pre}
   
3. Elimina il pod che utilizza l'archiviazione file e ricrealo. 
   1. Salva il file yaml del pod nella tua macchina locale.
      ```
      kubect get pod <pod_name> -o yaml > <filepath/pod.yaml>
      ```
      {: pre}
      
   2. Elimina il pod.
      ```
      kubectl deleted pod <pod_name>
      ```
      {: pre}
   
   3. Ricrea il pod.
      ```
      kubectl apply -f <filepath/pod.yaml>
      ```
      {: pre}

4. Attendi che il pod venga distribuito. 
   ```
   kubectl get pods
   ```
   {: pre}
   
   Il pod è completamente distribuito quando lo stato viene modificato con `In esecuzione`. 

5. Accedi al tuo pod. 
   ```
   kubectl exec -it <pod_name> sh
   ```
   {: pre}
   
6. Verifica che l'archiviazione file sia stata montata con la versione NFS che hai specificato precedentemente. 
   ```
   mount | grep "nfs" | awk -F" |," '{ print $5, $8 }'
   ```
   {: pre}
   
   Output di esempio: 
   ```
   nfs vers=3.0
   ```
   {: screen}
   
<br />




## Installazione della CLI dell'infrastruttura IBM Cloud (SoftLayer)
{: #slcli}

Installa la CLI dell'infrastruttura IBM Cloud (SoftLayer) per interagire con le tue risorse dell'infrastruttura come le istanze di archiviazione blocchi o file NFS.
{: shortdesc}

Prima di iniziare, [installa Python 3.6](https://www.python.org/downloads/).

1.  Vedi la [installation docs ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](http://softlayer-api-python-client.readthedocs.io/en/latest/install/).

    1.  Fai clic sul link nei passi **Scarica il tarball** o **Scarica il zipball**. Non utilizzare il comando `curl`.
    2.  Trova il pacchetto scaricato, decomprimilo e passa alla directory.
    3.  Installa la CLI.
    
        ```
        python3 setup.py install
        ```
        {: pre}
    
2.  Ottieni i tuoi chiave API e nome utente della tua infrastruttura IBM Cloud (SoftLayer).

    1.  Dalla [console {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/), espandi il menu e seleziona **Infrastruttura**.
    2.  Dal tuo profilo nella barra dei menu, seleziona l'account dell'infrastruttura che vuoi utilizzare. 
    3.  Seleziona **Account** > **Utenti** > **Elenco utenti**.
    4.  Dalla tabella **Utenti**, nella colonna **CHIAVE API**, fai clic su **Visualizza**. Se non visualizzi alcuna chiave API, fai clic su **Genera**.
    5.  Copia il nome utente e la chiave API nella finestra a comparsa.

3.  Configura la CLI per collegarsi al tuo account dell'infrastruttura IBM Cloud (SoftLayer).

    1.  Configura la CLI dell'infrastruttura IBM Cloud (SoftLayer).
        ```
        slcli setup
        ```
        {: pre}

    2.  Compila le informazioni obbligatorie.
    
        * **Nome utente**: immetti il nome utente API dell'infrastruttura IBM Cloud (SoftLayer) che hai precedentemente richiamato.
        * **Password o chiave API**: immetti la chiave API dell'infrastruttura IBM Cloud (SoftLayer) che hai precedentemente richiamato.
        * **Endpoint (public|private|custom) [public]**: immetti `https://api.softlayer.com/rest/v3.1`.
        * **Timeout [0]**: immetti un valore in secondi in cui la CLI attende una risposta dall'API. Un valore di `0` imposta la CLI ad aspettare per sempre.
        
        **Esempio**:
        
        ```
        $ slcli setup
        Username []: 1234567_user.name@example.com
        API Key or Password []:         Endpoint (public|private|custom) [public]: https://api.softlayer.com/rest/v3.1 
        Timeout [0]: 6000
        :..............:..................................................................:
        :         name : value                                                            :
        :..............:..................................................................:
        :     Username : 1234567_user.name@example.com                                    :
        :      API Key : 1111aa1111bbb22222b2b3c33333c3c3cc44d4444444444dd4444eee55e5e5e5 :
        : Endpoint URL : https://api.softlayer.com/xmlrpc/v3.1/                           :
        :      Timeout : 6000                                                             :
        :..............:..................................................................:
        Are you sure you want to write settings to "/Users/name/.softlayer"? [Y/n]: Y
        Configuration Updated Successfully
        ```
        {: screen}

Sei ora pronto ad utilizzare la CLI dell'infrastruttura IBM Cloud (SoftLayer).

## Installazione del plug-in {{site.data.keyword.Bluemix_notm}} Block Storage sul tuo cluster
{: #install_block}

Installa il plug-in {{site.data.keyword.Bluemix_notm}} Block Storage con un grafico Helm per impostare le classi di archiviazione predefinite per l'archiviazione blocchi. Puoi utilizzare queste classi di archiviazione per creare una PVC per eseguire il provisioning dell'archiviazione blocchi per le tue applicazioni.
{: shortdesc}

Prima di iniziare, [indirizza la tua CLI](cs_cli_install.html#cs_cli_configure) al cluster in cui desideri installare il plug-in {{site.data.keyword.Bluemix_notm}} Block Storage.

1. Installa [Helm](cs_integrations.html#helm) sul cluster in cui desideri utilizzare il plug-in {{site.data.keyword.Bluemix_notm}} Block Storage.
2. Aggiorna il repository helm per recuperare la versione più aggiornata di tutti i grafici helm in questo repository.
   ```
   helm repo update
   ```
   {: pre}

3. Installa il plug-in {{site.data.keyword.Bluemix_notm}} Block Storage. Quando installi il plug-in, al tuo cluster vengono aggiunte le classi di archiviazione blocchi predefinite.
   ```
   helm install ibm/ibmcloud-block-storage-plugin
   ```
   {: pre}

   Output di esempio:
   ```
   NAME:   bald-olm
   LAST DEPLOYED: Wed Apr 18 10:02:55 2018
   NAMESPACE: default
   STATUS: DEPLOYED

   RESOURCES:
   ==> v1beta1/DaemonSet
   NAME                           DESIRED  CURRENT  READY  UP-TO-DATE  AVAILABLE  NODE SELECTOR  AGE
   ibmcloud-block-storage-driver  0        0        0      0           0          <none>         0s

   ==> v1beta1/Deployment
   NAME                           DESIRED  CURRENT  UP-TO-DATE  AVAILABLE  AGE
   ibmcloud-block-storage-plugin  1        0        0           0          0s

   ==> v1/StorageClass
   NAME                      PROVISIONER        AGE
   ibmc-block-bronze         ibm.io/ibmc-block  0s
   ibmc-block-custom         ibm.io/ibmc-block  0s
   ibmc-block-gold           ibm.io/ibmc-block  0s
   ibmc-block-retain-bronze  ibm.io/ibmc-block  0s
   ibmc-block-retain-custom  ibm.io/ibmc-block  0s
   ibmc-block-retain-gold    ibm.io/ibmc-block  0s
   ibmc-block-retain-silver  ibm.io/ibmc-block  0s
   ibmc-block-silver         ibm.io/ibmc-block  0s

   ==> v1/ServiceAccount
   NAME                           SECRETS  AGE
   ibmcloud-block-storage-plugin  1        0s

   ==> v1beta1/ClusterRole
   NAME                           AGE
   ibmcloud-block-storage-plugin  0s

   ==> v1beta1/ClusterRoleBinding
   NAME                           AGE
   ibmcloud-block-storage-plugin  0s

   NOTES:
   Thank you for installing: ibmcloud-block-storage-plugin.   Your release is named: bald-olm
   ```
   {: screen}

4. Verifica che l'installazione sia stata eseguita correttamente.
   ```
   kubectl get pod -n kube-system | grep block
   ```
   {: pre}

   Output di esempio:
   ```
   ibmcloud-block-storage-driver-kh4mt                              1/1       Running   0          27d       10.118.98.19   10.118.98.19
   ibmcloud-block-storage-plugin-58c5f9dc86-pbl4t                   1/1       Running   0          14d       172.21.0.204   10.118.98.19
   ```
   {: screen}
   
   L'installazione ha avuto esito positivo quando visualizzi un pod `ibmcloud-block-storage-plugin` e uno o più pod `ibmcloud-block-storage-driver`. Il numero di `ibmcloud-block-storage-driver` è uguale al numero di nodi lavoro nel tuo cluster. Tutti i pod devono essere in uno stato **In esecuzione**. 

5. Verifica che le classi di archiviazione per l'archiviazione blocchi siano state aggiunte al tuo cluster.
   ```
   kubectl get storageclasses | grep block
   ```
   {: pre}

   Output di esempio:
   ```
   ibmc-block-bronze            ibm.io/ibmc-block
   ibmc-block-custom            ibm.io/ibmc-block
   ibmc-block-gold              ibm.io/ibmc-block
   ibmc-block-retain-bronze     ibm.io/ibmc-block
   ibmc-block-retain-custom     ibm.io/ibmc-block
   ibmc-block-retain-gold       ibm.io/ibmc-block
   ibmc-block-retain-silver     ibm.io/ibmc-block
   ibmc-block-silver            ibm.io/ibmc-block
   ```
   {: screen}

6. Ripeti questa procedura per ogni cluster in cui desideri eseguire il provisioning dell'archiviazione blocchi.

Ora puoi continuare a [creare una PVC](#create) per eseguire il provisioning dell'archiviazione blocchi per la tua applicazione.

<br />


### Aggiornamento del plug-in {{site.data.keyword.Bluemix_notm}} Block Storage
Puoi eseguire l'aggiornamento del plug-in {{site.data.keyword.Bluemix_notm}} Block Storage esistente alla versione più recente.
{: shortdesc}

Prima di iniziare, [indirizza la tua CLI](cs_cli_install.html#cs_cli_configure) al cluster.

1. Trova il nome del grafico helm dell'archiviazione blocchi che hai installato nel tuo cluster.
   ```
   helm ls | grep ibmcloud-block-storage-plugin
   ```
   {: pre}

   Output di esempio:
   ```
   myhelmchart 	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibmcloud-block-storage-plugin-0.1.0	default
   ```
   {: screen}

2. Aggiorna il plug-in {{site.data.keyword.Bluemix_notm}} Block Storage alla versione più recente.
   ```
   helm upgrade --force --recreate-pods <helm_chart_name>  ibm/ibmcloud-block-storage-plugin
   ```
   {: pre}

<br />


### Rimozione del plug-in {{site.data.keyword.Bluemix_notm}} Block Storage
Se non desideri eseguire il provisioning e utilizzare {{site.data.keyword.Bluemix_notm}} Block Storage per il tuo cluster, puoi disinstallare il grafico helm.
{: shortdesc}

**Nota:** la rimozione del plug-in non rimuove PVC, PV o dati esistenti. Quando rimuovi il plug-in, tutti i pod e le serie di daemon correlati vengono rimossi dal tuo cluster. Non puoi eseguire il provisioning di una nuova archiviazione blocchi per il tuo cluster o utilizzare i PV e le PVC dell'archiviazione blocchi esistente una volta rimosso il plug-in.

Prima di iniziare, [indirizza la tua CLI](cs_cli_install.html#cs_cli_configure) al cluster e assicurati di non avere PVC o PV nel tuo cluster che utilizzano l'archiviazione blocchi.

1. Trova il nome del grafico helm dell'archiviazione blocchi che hai installato nel tuo cluster.
   ```
   helm ls | grep ibmcloud-block-storage-plugin
   ```
   {: pre}

   Output di esempio:
   ```
   myhelmchart 	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibmcloud-block-storage-plugin-0.1.0	default
   ```
   {: screen}

2. Elimina il plug-in {{site.data.keyword.Bluemix_notm}} Block Storage.
   ```
   helm delete <helm_chart_name>
   ```
   {: pre}

3. Verifica che i pod dell'archiviazione blocchi siano stati rimossi.
   ```
   kubectl get pod -n kube-system | grep ibmcloud-block-storage-plugin
   ```
   {: pre}
   La rimozione dei pod è riuscita se nel tuo output CLI non vengono visualizzati pod.

4. Verifica che le classi di archiviazione dell'archiviazione blocchi siano state rimosse.
   ```
   kubectl get storageclasses | grep block
   ```
   {: pre}
   La rimozione delle classi di archiviazione è riuscita se nel tuo output CLI non vengono visualizzate classi di archiviazione.

<br />



## Configurazione delle soluzioni di backup e ripristino per le condivisioni file NFS e l'archiviazione blocchi
{: #backup_restore}

Il provisioning delle condivisioni file e dell'archiviazione blocchi viene eseguito nella stessa ubicazione del tuo cluster. L'archiviazione viene ospitata sui server in cluster da {{site.data.keyword.IBM_notm}} per offrire la disponibilità in caso di arresto di un server. Tuttavia, il backup delle condivisioni file e dell'archiviazione blocchi non viene eseguito automaticamente e potrebbero essere inaccessibili se si verifica un malfunzionamento dell'intera ubicazione. Per evitare che i tuoi dati vengano persi o danneggiati, puoi configurare dei backup periodici che puoi utilizzare per ripristinare i dati quando necessario.
{: shortdesc}

Esamina le seguenti opzioni di backup e ripristino per le tue condivisioni file NFS e la tua archiviazione blocchi:

<dl>
  <dt>Configura istantanee periodiche</dt>
  <dd><p>Puoi configurare istantanee periodiche per la tua archiviazione blocchi o di condivisione file NFS, ossia un'immagine di sola lettura che acquisisce lo stato dell'istanza in un punto nel tempo. Per archiviare l'istantanea, devi richiedere lo spazio per l'istantanea nella tua archiviazione blocchi o di condivisione file NFS. Le istantanee vengono memorizzate nell'istanza di archiviazione esistente all'interno della stessa ubicazione. Puoi ripristinare i dati da un'istantanea se un utente rimuove accidentalmente dati importanti dal volume. </br></br> <strong>Per creare un'istantanea per il tuo volume:</strong><ol><li>Elenca i PV esistenti nel tuo cluster. <pre class="pre"><code>    kubectl get pv
    </code></pre></li><li>Ottieni i dettagli del PV per cui vuoi creare uno spazio per l'istantanea e prendi nota dell'ID volume, della dimensione e degli IOPS. <pre class="pre"><code>kubectl describe pv &lt;pv_name&gt;</code></pre> Per l'archiviazione file, l'ID volume, la dimensione e gli IOPS possono essere trovati nella sezione <strong>Etichette</strong> del tuo output della CLI. Per l'archiviazione blocchi, la dimensione e gli IOPS vengono visualizzati nella sezione <strong>Etichette</strong> del tuo output della CLI. Per trovare l'ID volume, controlla l'annotazione <code>ibm.io/network-storage-id</code> del tuo output della CLI. </li><li>Crea la dimensione dell'istantanea per il tuo volume esistente con i parametri che hai richiamato nel passo precedente. <pre class="pre"><code>slcli file snapshot-order --capacity &lt;size&gt; --tier &lt;iops&gt; &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-order --capacity &lt;size&gt; --tier &lt;iops&gt; &lt;volume_id&gt;</code></pre></li><li>Attendi che la dimensione dell'istantanea venga creata. <pre class="pre"><code>slcli file volume-detail &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block volume-detail &lt;volume_id&gt;</code></pre>La dimensione dell'istantanea viene fornita correttamente quando la <strong>Capacità dell'istantanea (GB)</strong> nel tuo output della CLI viene modificata da 0 con la dimensione che hai ordinato. </li><li>Crea l'istantanea per il tuo volume e prendi nota dell'ID dell'istantanea che ti viene creata. <pre class="pre"><code>slcli file snapshot-create &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-create &lt;volume_id&gt;</code></pre></li><li>Verifica che l'istantanea sia stata creata correttamente. <pre class="pre"><code>slcli file volume-detail &lt;snapshot_id&gt;</code></pre><pre class="pre"><code>slcli block volume-detail &lt;snapshot_id&gt;</code></pre></li></ol></br><strong>Per ripristinare i dati da un'istantanea in un volume esistente: </strong><pre class="pre"><code>slcli file snapshot-restore -s &lt;snapshot_id&gt; &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-restore -s &lt;snapshot_id&gt; &lt;volume_id&gt;</code></pre></br>Per ulteriori informazioni, consulta:<ul><li>[Istantanee periodiche NFS](/docs/infrastructure/FileStorage/snapshots.html)</li><li>[Istantanee periodiche blocchi](/docs/infrastructure/BlockStorage/snapshots.html#snapshots)</li></ul></p></dd>
  <dt>Replica le istantanee in un'altra ubicazione</dt>
 <dd><p>Per proteggere i tuoi dati da un malfunzionamento dell'ubicazione, puoi [replicare le istantanee](/docs/infrastructure/FileStorage/replication.html#working-with-replication) in un'istanza di condivisione file NFS o di archiviazione blocchi configurata in un'altra ubicazione. I dati possono essere replicati solo dall'archiviazione primaria a quella di backup. Non puoi montare un'istanza di condivisione file NFS o di archiviazione blocchi replicata in un cluster. Quando la tua archiviazione primaria non funziona più, puoi impostare manualmente la tua archiviazione di backup replicata in modo che sia quella primaria. Quindi, puoi montarla nel tuo cluster. Una volta ripristinata la tua archiviazione primaria, puoi ripristinare i dati dall'archiviazione di backup.</p>
 <p>Per ulteriori informazioni, consulta:<ul><li>[Replica le istantanee per NFS](/docs/infrastructure/FileStorage/replication.html#working-with-replication)</li><li>[Replica le istantanee per Block](/docs/infrastructure/BlockStorage/replication.html#working-with-replication)</li></ul></p></dd>
 <dt>Duplica l'archiviazione</dt>
 <dd><p>Puoi duplicare la tua istanza di condivisione file NFS o di archiviazione blocchi nella stessa ubicazione dell'istanza di archiviazione originale. Un duplicato contiene gli stessi dati dell'istanza di archiviazione originale nel momento in cui è stato creato il duplicato. A differenza delle repliche, puoi utilizzare il duplicato come un'istanza di archiviazione indipendente dall'originale. Per eseguire la duplicazione, configura innanzitutto le istantanee per il volume.</p>
 <p>Per ulteriori informazioni, consulta:<ul><li>[Istantanee duplicate NFS](/docs/infrastructure/FileStorage/how-to-create-duplicate-volume.html#creating-a-duplicate-file-storage)</li><li>[Istantanee duplicate blocchi](/docs/infrastructure/BlockStorage/how-to-create-duplicate-volume.html#creating-a-duplicate-block-volume)</li></ul></p></dd>
  <dt>Esegui il backup dei dati in Object Storage </dt>
  <dd><p>Puoi utilizzare l'[**immagine ibm-backup-restore**](/docs/services/RegistryImages/ibm-backup-restore/index.html#ibmbackup_restore_starter) per avviare un pod di backup e ripristino nel tuo cluster. Questo pod contiene uno script per eseguire un backup una tantum o periodico per qualsiasi attestazione del volume persistente (PVC) nel tuo cluster. I dati vengono memorizzati nella tua istanza {{site.data.keyword.objectstoragefull}} che hai configurato in un'ubicazione.</p>
  <p>Per rendere i tuoi dati ancora più disponibili e proteggere la tua applicazione da un errore di ubicazione, configura una seconda istanza {{site.data.keyword.objectstoragefull}} e replica i dati tra le varie ubicazioni. Se devi ripristinare i dati dalla tua istanza {{site.data.keyword.objectstoragefull}}, utilizza lo script di ripristino fornito con l'immagine.</p></dd>
<dt>Copia i dati nei/dai pod e contenitori</dt>
<dd><p>Puoi utilizzare il comando `kubectl cp` [![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/reference/kubectl/overview/#cp) per copiare i file e le directory in/da pod o specifici contenitori nel tuo cluster.</p>
<p>Prima di iniziare, [indirizza la tua CLI Kubernetes](cs_cli_install.html#cs_cli_configure) al cluster che desideri utilizzare. Se non specifichi un contenitore con <code>-c</code>, il comando utilizza il primo contenitore disponibile nel pod.</p>
<p>Puoi utilizzare il comando in diversi modi:</p>
<ul>
<li>Copiare i dati dalla tua macchina locale in un pod nel tuo cluster: <pre class="pre"><code>kubectl cp <var>&lt;local_filepath&gt;/&lt;filename&gt;</var> <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var></code></pre></li>
<li>Copiare i dati da un pod nel tuo cluster nella tua macchina locale: <pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;/&lt;filename&gt;</var> <var>&lt;local_filepath&gt;/&lt;filename&gt;</var></code></pre></li>
<li>Copiare i dati da un pod nel tuo cluster in uno specifico contenitore in un altro pod: <pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var> <var>&lt;namespace&gt;/&lt;other_pod&gt;:&lt;pod_filepath&gt;</var> -c <var>&lt;container></var></code></pre></li>
</ul></dd>
  </dl>


