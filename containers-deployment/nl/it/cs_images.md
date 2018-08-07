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




# Creazione dei contenitori dalle immagini
{: #images}

Un'immagine Docker è la base per ogni contenitore che crei con {{site.data.keyword.containerlong}}.
{:shortdesc}

Un'immagine viene creata da un
Dockerfile, che è un file che contiene le istruzioni per creare l'immagine. Nelle sue istruzioni, un Dockerfile potrebbe
fare riferimento alle risorse di build che vengono memorizzate separatamente, quali ad esempio un'applicazione, la configurazione
dell'applicazione e le relative dipendenze.

## Pianificazione dei registri di immagini
{: #planning}

Le immagini normalmente vengono archiviate in un registro ed è possibile accedervi pubblicamente (registro pubblico)
o configurarle con un accesso limitato a un piccolo gruppo di utenti (registro privato).
{:shortdesc}

I registri pubblici,
come Docker Hub possono essere utilizzati come un'introduzione di Docker e Kubernetes per creare la tua prima
applicazione inserita in un contenitore in un cluster. Nel caso però di applicazioni aziendali, utilizza un registro privato,
come quello fornito in {{site.data.keyword.registryshort_notm}}, per proteggere le tue immagini dall'essere utilizzate e modificate da utenti non autorizzati. I registri privati
devono essere configurati dall'amministratore del cluster per assicurare che le credenziali di accesso al registro privato
siano disponibili agli utenti del cluster.


Puoi utilizzare più registri con {{site.data.keyword.containershort_notm}} per distribuire le applicazioni al tuo cluster.

|Registro|Descrizione|Vantaggi|
|--------|-----------|-------|
|[{{site.data.keyword.registryshort_notm}}](/docs/services/Registry/index.html)|Con questa opzione, puoi configurare il tuo proprio repository delle immagini Docker protetto in {{site.data.keyword.registryshort_notm}}
dove puoi archiviare e condividere le immagini in sicurezza tra gli utenti del cluster.|<ul><li>Gestisci l'accesso alle immagini nel tuo account.</li><li>Utilizza le applicazioni di esempio e le immagini fornite da
{{site.data.keyword.IBM_notm}}, come {{site.data.keyword.IBM_notm}} Liberty, come immagine principale e aggiungi il tuo proprio codice
ad esse.</li><li>Scansione automatica delle immagini per vulnerabilità potenziali dal Controllo vulnerabilità, incluse
le raccomandazioni di correzione specifiche del sistema operativo.</li></ul>|
|Ogni altro registro privato|Collega qualsiasi registro privato esistente al tuo cluster creando un [imagePullSecret ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/containers/images/). Il segreto viene utilizzato
per salvare in modo sicuro le tue credenziali e il tuo URL del registro in un segreto Kubernetes.|<ul><li>Utilizza i registri privati esistenti indipendentemente dalle loro origini (Docker Hub, registri di proprietà dell'organizzazione
o altri registri cloud privati).</li></ul>|
|[Docker Hub pubblico![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://hub.docker.com/){: #dockerhub}|Utilizza questa opzione per utilizzare direttamente le immagini pubbliche esistenti da Docker Hub nella tua [distribuzione Kubernetes![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) quando non è necessaria alcuna
modifica al Dockerfile. <p>**Nota:** tieni presente che questa opzione non rispetta i requisiti di sicurezza della tua organizzazione,
come la gestione dell'accesso, la scansione di vulnerabilità o la privacy dell'applicazione.</p>|<ul><li>Non è necessaria alcuna ulteriore configurazione per il tuo cluster. </li><li>Include varie applicazioni open source.</li></ul>|
{: caption="Opzioni del registro delle immagini privato e pubblico" caption-side="top"}

Dopo che hai configurato un registro immagini, gli utenti del cluster possono utilizzare le immagini per le loro distribuzioni dell'applicazione al cluster.

Ulteriori informazioni sulla [protezione delle tue informazioni personali](cs_secure.html#pi) quando utilizzi le immagini del contenitore.

<br />


## Configurazione dei contenuti attendibili per le immagini di contenitore
{: #trusted_images}

Puoi creare dei contenitori dalle immagini attendibili che sono firmate e memorizzate in {{site.data.keyword.registryshort_notm}} e impedire le distribuzioni da immagini non firmate o vulnerabili.
{:shortdesc}

1.  [Firma le immagini per i contenuti attendibili](/docs/services/Registry/registry_trusted_content.html#registry_trustedcontent). Una volta configurata l'attendibilità delle tue immagini, puoi gestire i contenuti attendibili e i firmatari che possono inserire le immagini nel tuo registro.
2.  Per applicare una politica in cui possono essere utilizzate solo immagini firmate per creare i contenitori nel tuo cluster, [aggiungi Container Image Security Enforcement (beta)](/docs/services/Registry/registry_security_enforce.html#security_enforce).
3.  Distribuisci la tua applicazione.
    1. [Distribuisci nello spazio dei nomi Kubernetes `predefinito`](#namespace).
    2. [Distribuisci in uno spazio dei nomi Kubernetes diverso oppure da {{site.data.keyword.Bluemix_notm}} una regione o da un account](#other) diversi.

<br />


## Distribuzione dei contenitori da un'immagine {{site.data.keyword.registryshort_notm}} nello spazio dei nomi Kubernetes `predefinito`
{: #namespace}

Puoi distribuire i contenitori al tuo cluster da un'immagine pubblica fornita da IBM o da un'immagine privata
memorizzata nel tuo spazio dei nomi in {{site.data.keyword.registryshort_notm}}.
{:shortdesc}

Quando crei un cluster, i segreti e i token del registro senza scadenza vengono creati automaticamente per [il registro regionale più vicino e il registro globale](/docs/services/Registry/registry_overview.html#registry_regions). Il registro globale archivia in modo sicuro le immagini fornite da IBM pubbliche a cui puoi fare riferimento nelle tue distribuzioni invece di avere riferimenti differenti per le immagini archiviate in ogni registro regionale. Il registro regionale archivia in modo sicuro le tue proprie immagini Docker, così come le stesse immagini pubbliche che vengono archiviate nel registro globale. I token sono utilizzati per autorizzare l'accesso in sola lettura a tutti i tuoi spazi dei nomi che hai configurato in {{site.data.keyword.registryshort_notm}} in modo che puoi utilizzare queste immagini pubbliche (registro globale) e private (registri regionali).

Ogni token deve essere memorizzato in un `imagePullSecret` Kubernetes per poter essere accessibile da un cluster Kubernetes
quando distribuisci un'applicazione inserita in un contenitore. Quando il tuo cluster viene creato, {{site.data.keyword.containershort_notm}} memorizza automaticamente i token per il registro globale (immagini pubbliche fornite da IBM) e il regionale nei segreti di pull dell'immagine Kubernetes. I segreti di pull dell'immagine sono aggiunti allo spazio dei nomi Kubernetes `default`, all'elenco predefinito dei segreti in `ServiceAccount` per tale spazio dei nomi e allo spazio dei nomi `kube-system`.

**Nota:** utilizzando questa configurazione iniziale, puoi distribuire i contenitori da qualsiasi immagine disponibile in
uno spazio dei nomi del tuo account {{site.data.keyword.Bluemix_notm}} allo
spazio dei nomi **predefinito** del tuo cluster. Per distribuire un contenitore in altri spazi dei nomi del tuo cluster, o per utilizzare un'immagine memorizzata in un'altra regione {{site.data.keyword.Bluemix_notm}} o in un altro account {{site.data.keyword.Bluemix_notm}}, devi [creare il tuo proprio imagePullSecret per il cluster](#other). 

Prima di iniziare:
1. [Configura uno spazio dei nomi in {{site.data.keyword.registryshort_notm}} su {{site.data.keyword.Bluemix_notm}} pubblico o {{site.data.keyword.Bluemix_dedicated_notm}} e distribuisci le immagini in questo spazio dei nomi](/docs/services/Registry/registry_setup_cli_namespace.html#registry_namespace_add).
2. [Crea un cluster](cs_clusters.html#clusters_cli).
3. [Indirizza la tua CLI al tuo cluster](cs_cli_install.html#cs_cli_configure).

Per distribuire un contenitore nello spazio dei nomi **predefinito** del tuo
cluster, crea un file di configurazione.

1.  Crea un file di configurazione di distribuzione denominato `mydeployment.yaml`.
2.  Definisci la distribuzione e l'immagine che vuoi utilizzare dal tuo spazio dei nomi in {{site.data.keyword.registryshort_notm}}.

    Per
utilizzare un'immagine privata da uno spazio dei nomi in {{site.data.keyword.registryshort_notm}}:

    ```
    apiVersion: apps/v1beta1
    kind: Deployment
    metadata:
      name: ibmliberty-deployment
    spec:
      replicas: 3
      template:
        metadata:
          labels:
            app: ibmliberty
        spec:
          containers:
          - name: ibmliberty
            image: registry.<region>.bluemix.net/<namespace>/<my_image>:<tag>
    ```
    {: codeblock}

    **Suggerimento:** per richiamare le informazioni sul tuo spazio dei nomi, esegui `bx cr namespace-list`.

3.  Crea la distribuzione nel tuo cluster.

    ```
    kubectl apply -f mydeployment.yaml
    ```
    {: pre}

    **Suggerimento:** puoi anche distribuire un file di configurazione esistente, come una delle immagini pubbliche fornite da IBM. Questo esempio utilizza l'immagine **ibmliberty** nella regione stati uniti sud.

    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/deploy-ibmliberty.yaml
    ```
    {: pre}


<br />



## Creazione di una `imagePullSecret` per accedere a {{site.data.keyword.Bluemix_notm}} o ai registri privati esterni in altri spazi dei nomi Kubernetes, account e regioni {{site.data.keyword.Bluemix_notm}}
{: #other}

Crea il tuo `imagePullSecret` per distribuire i contenitori negli altri spazi dei nomi Kubernetes, utilizza le immagini memorizzate in altre regioni o in altri account {{site.data.keyword.Bluemix_notm}}, utilizza le immagini memorizzate in {{site.data.keyword.Bluemix_dedicated_notm}} oppure utilizza le immagini memorizzate nei registri privati esterni.
{:shortdesc}

I ImagePullSecret sono validi solo per gli spazi dei nomi Kubernetes per cui sono stati creati. Ripeti questa procedura
per ogni spazio dei nomi in cui vuoi distribuire i contenitori. Le immagini da [DockerHub](#dockerhub) non richiedono ImagePullSecret.
{: tip}

Prima di iniziare:

1.  [Configura uno spazio dei nomi in {{site.data.keyword.registryshort_notm}} su {{site.data.keyword.Bluemix_notm}} pubblico o {{site.data.keyword.Bluemix_dedicated_notm}} e distribuisci le immagini in questo spazio dei nomi](/docs/services/Registry/registry_setup_cli_namespace.html#registry_namespace_add).
2.  [Crea un cluster](cs_clusters.html#clusters_cli).
3.  [Indirizza la tua CLI al tuo cluster](cs_cli_install.html#cs_cli_configure).

<br/>
Per creare il tuo imagePullSecret, puoi scegliere tra le seguenti opzioni:
- [Copia l'imagePullSecret dallo spazio dei nomi predefinito negli altri spazi dei nomi nel tuo cluster](#copy_imagePullSecret).
- [Crea un imagePullSecret per accedere alle immagini in altre regioni e in altri account {{site.data.keyword.Bluemix_notm}}](#other_regions_accounts).
- [Crea un imagePullSecret per accedere alle immagini nei registri privati esterni](#private_images).

<br/>
Se nel tuo spazio dei nomi hai già creato un imagePullSecret che desideri utilizzare nella tua distribuzione, vedi [Distribuzione dei contenitori utilizzando l'imagePullSecret creato](#use_imagePullSecret).

### Copia dell'imagePullSecret dallo spazio dei nomi predefinito negli altri spazi dei nomi nel tuo cluster
{: #copy_imagePullSecret}

Puoi copiare l'imagePullSecret creato automaticamente per lo spazio dei nomi Kubernetes `predefinito` negli altri spazi dei nomi nel tuo cluster.
{: shortdesc}

1. Elenca gli spazi dei nomi disponibili nel tuo cluster.
   ```
   kubectl get namespaces
   ```
   {: pre}

   Output di esempio:
   ```
   default          Active    79d
   ibm-cert-store   Active    79d
   ibm-system       Active    79d
   istio-system     Active    34d
   kube-public      Active    79d
   kube-system      Active    79d
   ```
   {: screen}

2. Facoltativo: crea uno spazio dei nomi nel tuo cluster.
   ```
   kubectl create namespace <namespace_name>
   ```
   {: pre}

3. Copia i imagePullSecret dallo spazio dei nomi `predefinito` nello spazio dei nomi di tua scelta. I nuovi imagePullSecret sono denominati `bluemix-<namespace_name>-secret-regional` e `bluemix-<namespace_name>-secret-international`.
   ```
   kubectl get secret bluemix-default-secret-regional -o yaml | sed 's/default/<namespace_name>/g' | kubectl -n <namespace_name> create -f -
   ```
   {: pre}
   
   ```
   kubectl get secret bluemix-default-secret-international -o yaml | sed 's/default/<namespace_name>/g' | kubectl -n <namespace_name> create -f -
   ```
   {: pre}

4.  Verifica che il segreto sia stato creato correttamente.
    ```
    kubectl get secrets --namespace <namespace_name>
    ```
    {: pre}

5. [Distribuisci un contenitore utilizzando l'imagePullSecret](#use_imagePullSecret) nel tuo spazio dei nomi.


### Creazione di un imagePullSecret per accedere alle immagini in altre regioni o in altri account {{site.data.keyword.Bluemix_notm}}
{: #other_regions_accounts}

Per accedere alle immagini in altre regioni o in altri account {{site.data.keyword.Bluemix_notm}}, devi creare un token di registro e salvare le tue credenziali in un imagePullSecret.
{: shortdesc}

1.  Se non hai un token, [crea un token per il registro a cui vuoi accedere.](/docs/services/Registry/registry_tokens.html#registry_tokens_create)
2.  Elenca i token nel tuo account {{site.data.keyword.Bluemix_notm}}.

    ```
    bx cr token-list
    ```
    {: pre}

3.  Prendi nota dell'ID token che vuoi utilizzare.
4.  Richiama il valore per il tuo token. Sostituisci <em>&lt;token_ID&gt;</em> con l'ID del token che hai richiamato nel passo precedente.

    ```
    bx cr token-get <token_id>
    ```
    {: pre}

    Il valore del tuo token viene visualizzato
nel campo **Token** dell'output della tua CLI.

5.  Crea il segreto Kubernetes per memorizzare le informazioni sul token.

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_URL> --docker-username=token --docker-password=<token_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>Descrizione dei componenti di questo comando</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti di questo comando</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>Obbligatoria. Lo spazio dei nomi Kubernetes del cluster in cui vuoi utilizzare il segreto e a cui distribuire i contenitori. Esegui <code>kubectl get namespaces</code> per elencare tutti gli spazi dei nomi nel tuo cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>Obbligatoria. Il nome che vuoi utilizzare per imagePullSecret.</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>Obbligatoria. L'URL del registro delle immagini in cui è configurato il tuo spazio dei nomi.<ul><li>Per gli spazi dei nomi configurati in Stati Uniti Sud e Stati Uniti Est registry.ng.bluemix.net</li><li>Per gli spazi dei nomi configurati in Regno Unito Sud registry.eu-gb.bluemix.net</li><li>Per gli spazi dei nomi configurati in Europa centrale (Francoforte) registry.eu-de.bluemix.net</li><li>Per gli spazi dei nomi configurati in Australia (Sydney) registry.au-syd.bluemix.net</li><li>Per gli spazi dei nomi configurati nel registro {{site.data.keyword.Bluemix_dedicated_notm}}.<em>&lt;dedicated_domain&gt;</em></li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>Obbligatoria. Il nome utente per accedere al tuo registro privato. Per {{site.data.keyword.registryshort_notm}}, il nome utente è impostato su <code>token</code>.</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>Obbligatoria. Il valore del tuo token di registro che hai richiamato in precedenza.</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>Obbligatoria. Se ne hai uno, immetti il tuo indirizzo e-mail Docker. Altrimenti, immetti un indirizzo e-mail fittizio, come ad esempio a@b.c. Questa e-mail è obbligatoria per creare un segreto Kubernetes, ma non viene utilizzata dopo la creazione.</td>
    </tr>
    </tbody></table>

6.  Verifica che il segreto sia stato creato correttamente. Sostituisci <em>&lt;kubernetes_namespace&gt;</em> con lo spazio dei nomi in cui hai creato il imagePullSecret. 

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

7.  [Distribuisci un contenitore utilizzando l'imagePullSecret](#use_imagePullSecret) nel tuo spazio dei nomi.

### Accesso alle immagini memorizzate in altri registri privati
{: #private_images}

Se hai già un registro privato, devi memorizzare le credenziali del registro in un imagePullSecret Kubernetes e fare riferimento a questo segreto dal tuo file di configurazione.
{:shortdesc}

Prima di iniziare:

1.  [Crea un cluster](cs_clusters.html#clusters_cli).
2.  [Indirizza la tua CLI al tuo cluster](cs_cli_install.html#cs_cli_configure).

Per creare un imagePullSecret:

1.  Crea il segreto Kubernetes per memorizzare le credenziali del tuo registro privato.

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_URL> --docker-username=<docker_username> --docker-password=<docker_password> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>Descrizione dei componenti di questo comando</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti di questo comando</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>Obbligatoria. Lo spazio dei nomi Kubernetes del cluster in cui vuoi utilizzare il segreto e a cui distribuire i contenitori. Esegui <code>kubectl get namespaces</code> per elencare tutti gli spazi dei nomi nel tuo cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>Obbligatoria. Il nome che vuoi utilizzare per imagePullSecret.</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>Obbligatoria. L'URL del registro in cui sono memorizzate le tue immagini private.</td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>Obbligatoria. Il nome utente per accedere al tuo registro privato.</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>Obbligatoria. Il valore del tuo token di registro che hai richiamato in precedenza.</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>Obbligatoria. Se ne hai uno, immetti il tuo indirizzo e-mail Docker. Se non hai uno, immetti un indirizzo e-mail fittizio, come ad esempio a@b.c. Questa e-mail è obbligatoria per creare un segreto Kubernetes, ma non viene utilizzata dopo la creazione.</td>
    </tr>
    </tbody></table>

2.  Verifica che il segreto sia stato creato correttamente. Sostituisci <em>&lt;kubernetes_namespace&gt;</em> con il nome dello spazio in cui hai creato imagePullSecret.

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

3.  [Crea un pod che fa riferimento all'imagePullSecret](#use_imagePullSecret).

## Distribuzione dei contenitori utilizzando l'imagePullSecret creato
{: #use_imagePullSecret}

Puoi definire un imagePullSecret nella tua distribuzione pod oppure memorizzare l'imagePullSecret nel tuo account di servizio Kubernetes in modo che sia disponibile per tutte le distribuzioni che non specificano un account di servizio.
{: shortdesc}

Scegli tra le seguenti opzioni:
* [Riferimento all'imagePullSecret nella tua distribuzione pod](#pod_imagePullSecret): usa questa opzione se non desideri concedere l'accesso al tuo registro per tutti i pod presenti nel tuo spazio dei nomi per impostazione predefinita.
* [Memorizzazione dell'imagePullSecret nell'account di servizio Kubernetes](#store_imagePullSecret): usa questa opzione per concedere l'accesso alle immagini nel tuo registro per le distribuzioni negli spazi dei nomi Kubernetes selezionati.

Prima di iniziare:
* [Crea un imagePullSecret](#other) per accedere alle immagini in altri registri, spazi dei nomi Kubernetes, account o regioni {{site.data.keyword.Bluemix_notm}}.
* [Indirizza la tua CLI al tuo cluster](cs_cli_install.html#cs_cli_configure).

### Riferimento all'`imagePullSecret` nella tua distribuzione pod
{: #pod_imagePullSecret}

Quando fai riferimento all'imagePullSecret in una distribuzione pod, è valido solo per questo pod e non può essere condiviso tra i pod nello spazio dei nomi.
{:shortdesc}

1.  Crea un file di configurazione del pod denominato `mypod.yaml`.
2.  Definisci il pod e l'imagePullSecret per accedere al {{site.data.keyword.registrylong_notm}} privato.

    Per accedere a un'immagine privata:
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod
    spec:
      containers:
        - name: <container_name>
          image: registry.<region>.bluemix.net/<namespace_name>/<image_name>:<tag>
      imagePullSecrets:
        - name: <secret_name>
    ```
    {: codeblock}

    Per accedere ad un'immagine pubblica {{site.data.keyword.Bluemix_notm}}:
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod
    spec:
      containers:
        - name: <container_name>
          image: registry.bluemix.net/<image_name>:<tag>
      imagePullSecrets:
        - name: <secret_name>
    ```
    {: codeblock}

    <table>
    <caption>Descrizione dei componenti del file YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del file YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>&lt;container_name&gt;</em></code></td>
    <td>Il nome del contenitore da distribuire al tuo cluster. </td>
    </tr>
    <tr>
    <td><code><em>&lt;namespace_name&gt;</em></code></td>
    <td>Lo spazio dei nomi in cui è memorizzata l'immagine. Per elencare gli spazi dei nomi disponibili, esegui `bx cr namespace-list`.</td>
    </tr>
    <tr>
    <td><code><em>&lt;image_name&gt;</em></code></td>
    <td>Il nome dell'immagine che vuoi utilizzare. Per elencare le immagini disponibili in un account {{site.data.keyword.Bluemix_notm}}, esegui `bx cr image-list`.</td>
    </tr>
    <tr>
    <td><code><em>&lt;tag&gt;</em></code></td>
    <td>La versione dell'immagine che vuoi utilizzare. Se non si specifica una tag, viene utilizzata l'immagine contrassegnata con <strong>latest</strong> per impostazione predefinita.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>Il nome dell'imagePullSecret che hai creato in precedenza.</td>
    </tr>
    </tbody></table>

3.  Salva le modifiche.
4.  Crea la distribuzione nel tuo cluster.
    ```
    kubectl apply -f mypod.yaml
    ```
    {: pre}

### Memorizzazione dell'imagePullSecret nell'account di servizio Kubernetes per lo spazio dei nomi selezionato
{:#store_imagePullSecret}

Ogni spazio dei nomi ha un account di servizio Kubernetes denominato `default`. Puoi aggiungere l'imagePullSecret a questo account di servizio per concedere l'accesso alle immagini nel tuo registro. Le distribuzioni che non specificano un account di servizio utilizzano automaticamente l'account di servizio `default` per questo spazio dei nomi.
{:shortdesc}

1. Controlla se esiste già un imagePullSecret per il tuo account di servizio predefinito (default).
   ```
   kubectl describe serviceaccount default -n <namespace_name>
   ```
   {: pre}
   Quando `<none>` viene visualizzato nella voce **Segreti di pull dell'immagine**, non esiste alcun imagePullSecret.  
2. Aggiungi l'imagePullSecret al tuo account di servizio predefinito.
   - **Per aggiungere l'imagePullSecret quando non ci sono imagePullSecret definiti:**
       ```
       kubectl patch -n <namespace_name> serviceaccount/default -p '{"imagePullSecrets":[{"name": "bluemix-<namespace_name>-secret-regional"}]}'
       ```
       {: pre}
   - **Per aggiungere l'imagePullSecret quando è già definito un imagePullSecret:**
       ```
       kubectl patch -n <namespace_name> serviceaccount/default --type='json' -p='[{"op":"add","path":"/imagePullSecrets/-","value":{"name":"bluemix-<namespace_name>-secret-regional"}}]'
       ```
       {: pre}
3. Verifica che il tuo imagePullSecret sia stato aggiunto al tuo account di servizio predefinito.
   ```
   kubectl describe serviceaccount default -n <namespace_name>
   ```
   {: pre}

   Output di esempio:
   ```
   Name:                default
   Namespace:           <namespace_name>
   Labels:              <none>
   Annotations:         <none>
   Image pull secrets:  bluemix-namespace_name-secret-regional
   Mountable secrets:   default-token-sh2dx
   Tokens:              default-token-sh2dx
   Events:              <none>
   ```
   {: pre}

4. Distribuisci un contenitore da un'immagine nel tuo registro.
   ```
   apiVersion: v1
   kind: Pod
   metadata:
     name: mypod
   spec:
     containers:
       - name: <container_name>
         image: registry.<region>.bluemix.net/<namespace_name>/<image_name>:<tag>
   ```
   {: codeblock}

5. Crea la distribuzione nel cluster. 
   ```
   kubectl apply -f mypod.yaml
   ```
   {: pre}

<br />


