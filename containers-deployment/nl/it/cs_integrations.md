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




# Integrazione dei servizi
{: #integrations}

Puoi utilizzare vari servizi esterni e servizi di catalogo con un cluster Kubernetes standard in {{site.data.keyword.containerlong}}.
{:shortdesc}


## Servizi delle applicazioni
<table summary="Riepilogo dell'accessibilità">
<caption>Servizi delle applicazioni</caption>
<thead>
<tr>
<th>Servizio</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.blockchainfull}}</td>
<td>Distribuisci un ambiente di sviluppo pubblicamente per IBM Blockchain a un cluster Kubernetes in {{site.data.keyword.containerlong_notm}}. Utilizza questo ambiente per sviluppare e personalizzare la tua propria rete blockchain per distribuire le applicazioni che condividono un ledger immutabile per la registrazione della cronologia delle transazioni. Per ulteriori informazioni, consulta
<a href="https://ibm-blockchain.github.io" target="_blank">Sviluppa in un sandbox cloud della piattaforma
IBM Blockchain <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. </td>
</tr>
</tbody>
</table>

<br />



## Servizi DevOps
<table summary="Riepilogo dell'accessibilità">
<caption>Servizi DevOps</caption>
<thead>
<tr>
<th>Servizio</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Codeship</td>
<td>Puoi utilizzare <a href="https://codeship.com" target="_blank">Codeship <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a> per l'integrazione e la consegna continua dei contenitori. Per ulteriori informazioni, consulta <a href="https://www.ibm.com/blogs/bluemix/2017/10/using-codeship-pro-deploy-workloads-ibm-container-service/" target="_blank">Using Codeship Pro To Deploy Workloads to {{site.data.keyword.containershort_notm}} <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. </td>
</tr>
<tr>
<td>Helm</td>
<td> <a href="https://helm.sh/" target="_blank">Helm <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a> è un gestore del pacchetto Kubernetes. Puoi creare nuovi grafici Helm o utilizzare grafici Helm preesistenti per definire, installare e aggiornare applicazioni Kubernetes complesse eseguite nei cluster {{site.data.keyword.containerlong_notm}}. <p>Per ulteriori informazioni, vedi [Configurazione di Helm in {{site.data.keyword.containershort_notm}}](cs_integrations.html#helm).</p></td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>Automatizza le creazioni delle tue applicazioni e le distribuzioni del contenitore ai cluster Kubernetes utilizzando una toolchain. Per informazioni sulla configurazione, consulta il blog <a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">Deploy Kubernetes pods to the {{site.data.keyword.containerlong_notm}} using DevOps Pipelines <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. </td>
</tr>
<tr>
<td>Istio</td>
<td><a href="https://www.ibm.com/cloud/info/istio" target="_blank">Istio <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a> è un servizio open source che offre agli sviluppatori un modo per connettere, proteggere, gestire e monitorare una rete di microservizi, nota anche come rete (mesh) di servizi, su piattaforme di orchestrazione cloud come Kubernetes. Controlla il post del blog relativo a <a href="https://developer.ibm.com/dwblog/2017/istio/" target="_blank">come IBM ha collaborato a fondare e avviato Istio<img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a> per trovare ulteriori informazioni sul progetto open source. Per installare Istio sul tuo cluster Kubernetes in {{site.data.keyword.containershort_notm}} e iniziare ad utilizzare un'applicazione di esempio, consulta [Tutorial: Managing microservices with Istio](cs_tutorials_istio.html#istio_tutorial).</td>
</tr>
</tbody>
</table>

<br />



## Servizi di registrazione e monitoraggio
<table summary="Riepilogo dell'accessibilità">
<caption>Servizi di registrazione e monitoraggio</caption>
<thead>
<tr>
<th>Servizio</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>CoScale</td>
<td>Monitora i nodi di lavoro, i contenitori, le serie di repliche, i controller della replica e i servizi con <a href="https://www.coscale.com/" target="_blank">CoScale <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. Per ulteriori informazioni, consulta <a href="https://www.ibm.com/blogs/bluemix/2017/06/monitoring-ibm-bluemix-container-service-coscale/" target="_blank">Monitoring {{site.data.keyword.containershort_notm}} with CoScale <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. </td>
</tr>
<tr>
<td>Datadog</td>
<td>Monitora il tuo cluster e visualizza le metriche delle prestazioni dell'applicazione e dell'infrastruttura con <a href="https://www.datadoghq.com/" target="_blank">Datadog <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. Per ulteriori informazioni, consulta <a href="https://www.ibm.com/blogs/bluemix/2017/07/monitoring-ibm-bluemix-container-service-datadog/" target="_blank">Monitoring {{site.data.keyword.containershort_notm}} with Datadog <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.loganalysisfull}}</td>
<td>Espandi le tue capacità di raccolta, conservazione e ricerca dei log con {{site.data.keyword.loganalysisfull_notm}}. Per ulteriori informazioni, vedi <a href="../services/CloudLogAnalysis/containers/containers_kube_other_logs.html" target="_blank">Abilitazione della raccolta automatica dei log di cluster <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.monitoringlong}}</td>
<td>Espandi le tue capacità di raccolta e conservazione delle metriche definendo regole e avvisi con {{site.data.keyword.monitoringlong_notm}}. Per ulteriori informazioni, vedi <a href="../services/cloud-monitoring/tutorials/container_service_metrics.html" target="_blank">Analizza le metriche in Grafana per un'applicazione distribuita in un cluster Kubernetes <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. </td>
</tr>
<tr>
<td>Instana</td>
<td> <a href="https://www.instana.com/" target="_blank">Instana <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a> fornisce il monitoraggio delle prestazioni di infrastruttura e applicazioni con una GUI che rileva e associa automaticamente le tue applicazioni. Istana cattura ogni richiesta alle tue applicazioni, che puoi utilizzare per risolvere i problemi ed eseguire l'analisi delle cause principali per evitare che i problemi si ripetano. Per ulteriori informazioni, controlla il post del blog relativo alla <a href="https://www.instana.com/blog/precise-visibility-applications-ibm-bluemix-container-service/" target="_blank">distribuzione di Istana in {{site.data.keyword.containershort_notm}} <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>.</td>
</tr>
<tr>
<td>Prometheus</td>
<td>Prometheus è uno strumento di avviso, registrazione e monitoraggio open source che è stato specificatamente progettato per Kubernetes. Prometheus recupera informazioni dettagliate sul cluster, sui nodi di lavoro e sull'integrità della distribuzione in base alle informazioni di registrazione di Kubernetes. La CPU, la memoria, I/O e l'attività di rete vengono raccolti per ogni contenitore in esecuzione in un cluster. Puoi utilizzare i dati raccolti in query o avvisi personalizzati per monitorare le prestazioni e i carichi di lavoro nel tuo cluster. 

<p>Per utilizzare Prometheus, segui le <a href="https://github.com/coreos/prometheus-operator/tree/master/contrib/kube-prometheus" target="_blank">istruzioni CoreOS <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>.</p>
</td>
</tr>
<tr>
<td>Sematext</td>
<td>Visualizza le metriche e i log per le tue applicazioni inserite in un contenitore utilizzando <a href="https://sematext.com/" target="_blank">Sematext <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. Per ulteriori informazioni, consulta <a href="https://www.ibm.com/blogs/bluemix/2017/09/monitoring-logging-ibm-bluemix-container-service-sematext/" target="_blank">Monitoring and logging for containers with Sematext <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. </td>
</tr>
<tr>
<td>Sysdig</td>
<td>Acquisisci le metriche dell'applicazione, del contenitore, di statsd e dell'host con un solo punto di strumentazione utilizzando <a href="https://sysdig.com/" target="_blank">Sysdig <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. Per ulteriori informazioni, consulta <a href="https://www.ibm.com/blogs/bluemix/2017/08/monitoring-ibm-bluemix-container-service-sysdig-container-intelligence/" target="_blank">Monitoring {{site.data.keyword.containershort_notm}} with Sysdig Container Intelligence <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. </td>
</tr>
<tr>
<td>Weave Scope</td>
<td>Weave Scope fornisce un diagramma visivo delle risorse in un cluster Kubernetes,
inclusi i servizi, i pod, i contenitori, i processi, i nodi e altro. Scope fornisce metriche interattive
per la CPU e la memoria e inoltre fornisce strumenti per inserire ed eseguire in un contenitore.<p>Per ulteriori informazioni, consulta [Visualizzazione delle risorse del cluster Kubernetes con Weave Scope e {{site.data.keyword.containershort_notm}}](cs_integrations.html#weavescope).</p></li></ol>
</td>
</tr>
</tbody>
</table>

<br />



## Servizi di sicurezza
<table summary="Riepilogo dell'accessibilità">
<caption>Servizi di sicurezza</caption>
<thead>
<tr>
<th>Servizio</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
  <tr id="appid">
    <td>{{site.data.keyword.appid_full}}</td>
    <td>Aggiungi un livello di sicurezza alle tue applicazioni con [{{site.data.keyword.appid_short}}](/docs/services/appid/index.html#gettingstarted) richiedendo agli utenti di effettuare l'accesso. Per autenticare le richieste HTTP/HTTPS web o API nella tua applicazione, puoi integrare {{site.data.keyword.appid_short_notm}} con il tuo servizio Ingress utilizzando l'[annotazione Ingress per l'autenticazione {{site.data.keyword.appid_short_notm}}](cs_annotations.html#appid-auth).</td>
  </tr>
<tr>
<td>Aqua Security</td>
  <td>Come un supplemento al <a href="/docs/services/va/va_index.html" target="_blank">Controllo vulnerabilità</a>, puoi utilizzare <a href="https://www.aquasec.com/" target="_blank">Aqua Security <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a> per migliorare la sicurezza delle distribuzioni del contenitore riducendo cosa è consentito fare alla tua applicazione. Per ulteriori informazioni, consulta <a href="https://www.ibm.com/blogs/bluemix/2017/06/protecting-container-deployments-bluemix-aqua-security/" target="_blank">Protecting container deployments on {{site.data.keyword.Bluemix_notm}} with Aqua Security <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.cloudcerts_full}}</td>
<td>Puoi utilizzare <a href="../services/certificate-manager/index.html" target="_blank">{{site.data.keyword.cloudcerts_long}} <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a> per archiviare e gestire i certificati SSL per le tue applicazioni. Per ulteriori informazioni, consulta <a href="https://www.ibm.com/blogs/bluemix/2018/01/use-ibm-cloud-certificate-manager-ibm-cloud-container-service-deploy-custom-domain-tls-certificates/" target="_blank">Use {{site.data.keyword.cloudcerts_long_notm}} with {{site.data.keyword.containershort_notm}} to deploy custom domain TLS Certificates <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. </td>
</tr>
<tr>
  <td>{{site.data.keyword.registrylong}}</td>
  <td>Imposta il tuo repository delle immagini Docker protetto in cui puoi memorizzare e condividere in modo sicuro le immagini tra gli utenti cluster. Per ulteriori informazioni, vedi la <a href="/docs/services/Registry/index.html" target="_blank">{{site.data.keyword.registrylong}} documentazione <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>.</td>
</tr>
<tr>
<td>NeuVector</td>
<td>Proteggi i contenitori con un firewall nativo nel cloud utilizzando <a href="https://neuvector.com/" target="_blank">NeuVector <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. Per ulteriori informazioni, consulta <a href="https://www.ibm.com/us-en/marketplace/neuvector-container-security" target="_blank">NeuVector Container Security <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. </td>
</tr>
<tr>
<td>Twistlock</td>
<td>Come un supplemento al <a href="/docs/services/va/va_index.html" target="_blank">Controllo vulnerabilità</a>, puoi utilizzare <a href="https://www.twistlock.com/" target="_blank">Twistlock <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a> per gestire i firewall, la protezione dalle minacce e la risposta agli incidenti. Per ulteriori informazioni, consulta <a href="https://www.ibm.com/blogs/bluemix/2017/07/twistlock-ibm-bluemix-container-service/" target="_blank">Twistlock on {{site.data.keyword.containershort_notm}} <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>. </td>
</tr>
</tbody>
</table>

<br />



## Servizi di archiviazione
<table summary="Riepilogo dell'accessibilità">
<caption>Servizi di archiviazione</caption>
<thead>
<tr>
<th>Servizio</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Heptio Ark</td>
  <td>Puoi utilizzare <a href="https://github.com/heptio/ark" target="_blank">Heptio Ark <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a> per eseguire il backup e per ripristinare le risorse del cluster e i volumi persistenti. Per ulteriori informazioni, vedi Heptio Ark <a href="https://github.com/heptio/ark/blob/master/docs/use-cases.md#use-cases" target="_blank">Use cases for disaster recovery and cluster migration <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>.</td>
</tr>
<tr>
  <td>{{site.data.keyword.cos_full}}</td>
  <td>I dati memorizzati con {{site.data.keyword.cos_short}} vengono crittografati e diffusi tra più posizioni geografiche e vi si accede su HTTP utilizzando un'API REST. Puoi utilizzare l'[immagine ibm-backup-restore](/docs/services/RegistryImages/ibm-backup-restore/index.html) per configurare il servizio per creare backup monouso o pianificati per i dati nei tuoi cluster. Per informazioni generali sul servizio, vedi la <a href="/docs/services/cloud-object-storage/about-cos.html" target="_blank">{{site.data.keyword.cos_short}} documentazione <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>.</td>
</tr>
  <tr>
    <td>{{site.data.keyword.cloudantfull}}</td>
    <td>{{site.data.keyword.cloudant_short_notm}} è un DBaaS (document-oriented DataBase as a Service) che memorizza i dati come documenti in formato JSON. Il servizio viene creato per la scalabilità, l'alta disponibilità e la durabilità. Per ulteriori informazioni, vedi la <a href="/docs/services/Cloudant/getting-started.html" target="_blank">{{site.data.keyword.cloudant_short_notm}} documentazione <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>.</td>
  </tr>
  <tr>
    <td>{{site.data.keyword.composeForMongoDB_full}}</td>
    <td>{{site.data.keyword.composeForMongoDB}} offre alta disponibilità e ridondanza, backup no-stop in locale e automatizzati, strumenti di monitoraggio, integrazione nei sistemi di avviso, viste dell'analisi delle prestazioni e altro ancora. Per ulteriori informazioni, vedi la <a href="/docs/services/ComposeForMongoDB/index.html" target="_blank">{{site.data.keyword.composeForMongoDB}} documentazione <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>.</td>
  </tr>
</tbody>
</table>


<br />



## Aggiunta di servizi Cloud Foundry ai cluster
{: #adding_cluster}

Aggiungi un'istanza del servizio Cloud Foundry esistente al tuo cluster per consentire agli utenti del cluster di accedere e utilizzare il servizio quando distribuiscono un'applicazione sul cluster.
{:shortdesc}

Prima di iniziare:

1. [Indirizza la tua CLI](cs_cli_install.html#cs_cli_configure) al tuo cluster.
2. [Richiedi un'istanza del servizio {{site.data.keyword.Bluemix_notm}}](/docs/apps/reqnsi.html#req_instance).
   **Nota:** per creare un'istanza di un servizio nell'ubicazione Washington, devi utilizzare la CLI.
3. I servizi Cloud Foundry sono supportati per il bind con i cluster, ma altri servizi non lo sono. Puoi vedere i diversi tipi di servizio dopo aver creato l'istanza del servizio e i servizi sono raggruppati nel dashboard come **Servizi Cloud Foundry** e **Servizi**. Per eseguire il bind dei servizi elencati nella sezione **Servizi** ai cluster, [crea prima gli alias Cloud Foundry](#adding_resource_cluster).

**Nota:**
<ul><ul>
<li>Puoi aggiungere solo servizi {{site.data.keyword.Bluemix_notm}} che supportano le chiavi del servizio. Se il servizio non supporta le chiavi del servizio, consulta [Abilitazione di applicazioni esterne all'utilizzo dei servizi {{site.data.keyword.Bluemix_notm}}](/docs/apps/reqnsi.html#accser_external).</li>
<li>Il cluster e i nodi di lavoro devono essere distribuiti completamente prima di poter aggiungere un servizio.</li>
</ul></ul>


Per aggiungere un servizio:
2.  Elenca i servizi {{site.data.keyword.Bluemix_notm}} disponibili.

    ```
    bx service list
    ```
    {: pre}

    Output CLI di esempio:

    ```
    name                      service           plan    bound apps   last operation   
    <service_instance_name>   <service_name>    spark                create succeeded
    ```
    {: screen}

3.  Prendi nota del **nome** dell'istanza del servizio che vuoi aggiungere al tuo cluster.
4.  Identifica lo spazio dei nomi del cluster che desideri utilizzare per l'aggiunta del tuo servizio. Scegli tra le seguenti opzioni:
    -   Elenca gli spazi dei nomi esistenti e scegline uno che desideri utilizzare.

        ```
        kubectl get namespaces
        ```
        {: pre}

    -   Crea uno spazio dei nomi nel tuo cluster. 

        ```
        kubectl create namespace <namespace_name>
        ```
        {: pre}

5.  Aggiungi il servizio al tuo cluster.

    ```
    bx cs cluster-service-bind <cluster_name_or_ID> <namespace> <service_instance_name>
    ```
    {: pre}

    Dopo che
il servizio è stato correttamente associato al tuo cluster, viene creato un segreto cluster che contiene le credenziali
della tua istanza del servizio. Output CLI di esempio:

    ```
    bx cs cluster-service-bind mycluster mynamespace cleardb 
    Binding service instance to namespace...
    OK
    Namespace: mynamespace
    Secret name:     binding-<service_instance_name>
    ```
    {: screen}

6.  Verifica che il segreto sia stato creato nel tuo spazio dei nomi del cluster.

    ```
    kubectl get secrets --namespace=<namespace>
    ```
    {: pre}

Per utilizzare il servizio in un pod distribuito nel cluster, gli utenti del cluster devono accedere alle credenziali del servizio. Gli utenti del cluster possono accedere alle credenziali del servizio {{site.data.keyword.Bluemix_notm}} da [montaggio del segreto Kubernetes come un volume secreto in un pod](#adding_app). 

<br />


## Creazione di alias Cloud Foundry per altre risorse del servizio {{site.data.keyword.Bluemix_notm}}
{: #adding_resource_cluster}

I servizi Cloud Foundry sono supportati per il bind con i cluster. Per associare un servizio {{site.data.keyword.Bluemix_notm}} che non è un servizio Cloud Foundry al tuo cluster, crea un alias Cloud Foundry per l'istanza del servizio.
{:shortdesc}

Prima di iniziare, [richiedi un'istanza del servizio {{site.data.keyword.Bluemix_notm}}](/docs/apps/reqnsi.html#req_instance).

Per creare un alias Cloud Foundry per l'istanza del servizio:

1. Specifica l'organizzazione e uno spazio in cui viene creata l'istanza del servizio.

    ```
    bx target -o <org_name> -s <space_name>
    ```
    {: pre}

2. Prendi nota del nome dell'istanza del servizio.
    ```
    bx resource service-instances
    ```
    {: pre}

3. Crea un alias Cloud Foundry per l'istanza del servizio.
    ```
    bx resource service-alias-create <service_alias_name> --instance-name <service_instance>
    ```
    {: pre}

4. Verifica che l'alias di servizio sia stato creato.

    ```
    bx service list
    ```
    {: pre}

5. [Esegui il bind dell'alias Cloud Foundry al cluster](#adding_cluster).



<br />


## Aggiunta dei servizi alle applicazioni
{: #adding_app}

I segreti Kubernetes crittografati sono utilizzati per memorizzare i dettagli e le credenziali del servizio {{site.data.keyword.Bluemix_notm}}
e consentono una comunicazione sicura tra il servizio e il cluster.
{:shortdesc}

I segreti Kubernetes rappresentano un modo sicuro per memorizzare informazioni riservate, quali nome utente, password o
chiavi. Invece di esporre informazioni riservate attraverso le variabili di ambiente o direttamente nel Dockerfile, puoi montare i segreti in un pod. Quindi, un contenitore in esecuzione in un pod potrà accedere a questi segreti.

Quando monti un volume segreto nel tuo pod, un file denominato `binding` viene memorizzato nella directory di montaggio del volume. Il file `binding` include tutte le informazioni e credenziali di cui hai bisogno per accedere al servizio {{site.data.keyword.Bluemix_notm}}.

Prima di iniziare, [indirizza la tua CLI](cs_cli_install.html#cs_cli_configure) al tuo cluster. Accertati che il servizio {{site.data.keyword.Bluemix_notm}} che vuoi usare nella tua applicazione sia stato [aggiunto al cluster](cs_integrations.html#adding_cluster) dall'amministratore del cluster.

1.  Elenca i segreti disponibili nello spazio dei nomi del cluster.

    ```
    kubectl get secrets --namespace=<my_namespace>
    ```
    {: pre}

    Output di esempio:

    ```
    NAME                                    TYPE                                  DATA      AGE
    binding-<service_instance_name>         Opaque                                1         3m

    ```
    {: screen}

2.  Cerca un segreto di tipo **Opaco** e prendi nota del
**nome** del segreto. Se sono presenti più segreti, contatta il tuo amministratore cluster per identificare il segreto del servizio
corretto.

3.  Apri il tuo editor preferito.

4.  Crea un file YAML per configurare un pod che possa accedere ai dettagli del servizio tramite un volume
segreto. Se hai associato più di un servizio, verifica che ogni segreto sia associato al servizio corretto.

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
    <caption>Descrizione dei componenti del file YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del file YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code>volumeMounts/mountPath</code></td>
    <td>Il nome del volume segreto che vuoi montare nel tuo contenitore.</td>
    </tr>
    <tr>
    <td><code>volumes/name</code></td>
    <td>Immetti un nome per il volume segreto che vuoi montare nel tuo contenitore.</td>
    </tr>
    <tr>
    <td><code>secret/defaultMode</code></td>
    <td>Imposta le autorizzazioni di sola lettura per il segreto del servizio.</td>
    </tr>
    <tr>
    <td><code>secret/secretName</code></td>
    <td>Immetti il nome del segreto di cui hai preso nota in precedenza.</td>
    </tr></tbody></table>

5.  Crea il pod e monta il volume segreto.

    ```
    kubectl apply -f secret-test.yaml
    ```
    {: pre}

6.  Verifica che il pod sia stato creato.

    ```
    kubectl get pods --namespace=<my_namespace>
    ```
    {: pre}

    Output CLI di esempio:

    ```
    NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
    ```
    {: screen}

7.  Prendi nota del **NOME** del tuo pod.
8.  Ottieni i dettagli relativi al pod e cerca il nome del segreto.

    ```
    kubectl describe pod <pod_name>
    ```
    {: pre}

    Output:

    ```
    ...
    Volumes:
      service-bind-volume:
        Type:       Secret (a volume populated by a Secret)
        SecretName: binding-<service_instance_name>
    ...
    ```
    {: screen}

    

9.  Configura le tue applicazioni per trovare il file del segreto `binding` nella directory di montaggio, analizza il contenuto JSON e determina l'URL e le credenziali del servizio per accedere al tuo servizio {{site.data.keyword.Bluemix_notm}}.

Puoi ora accedere ai dettagli e alle credenziali del servizio {{site.data.keyword.Bluemix_notm}}. Per lavorare con il servizio {{site.data.keyword.Bluemix_notm}}, assicurati che la tua applicazione sia configurata per trovare il file del segreto del servizio nella directory di montaggio, analizzare il contenuto JSON e determinare i dettagli del servizio. 

<br />


## Configurazione di Helm in {{site.data.keyword.containershort_notm}}
{: #helm}

[Helm ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://helm.sh/) è un gestore pacchetti di Kubernetes. Puoi creare grafici Helm o utilizzare grafici Helm preesistenti per definire, installare e aggiornare applicazioni Kubernetes complesse eseguite nei cluster {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

Prima di utilizzare i grafici Helm con {{site.data.keyword.containershort_notm}}, devi installare e inizializzare un'istanza Helm per il tuo cluster. Puoi quindi aggiungere il repository Helm {{site.data.keyword.Bluemix_notm}} alla tua istanza Helm.

Prima di iniziare, [indirizza la tua CLI](cs_cli_install.html#cs_cli_configure) al cluster in cui vuoi utilizzare un grafico Helm.

1. Installa la <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">CLI Helm <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>.

2. **Importante**: per mantenere la sicurezza del cluster, crea un account di servizio per Tiller nello spazio dei nomi `kube-system` e un bind del ruolo cluster RBAC Kubernetes per il pod `tiller-deploy`.

    1. Nel tuo editor preferito, crea il seguente file e salvalo come `rbac-config.yaml`.
      **Nota**:
        * Il ruolo cluster `cluster-admin` viene creato nei cluster Kubernetes per impostazione predefinita, quindi non devi definirlo esplicitamente.
        * Se stai utilizzando un cluster versione 1.7.x, modifica `apiVersion` in `rbac.authorization.k8s.io/v1beta1`.

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

    2. Crea l'account di servizio e il bind del ruolo cluster.

        ```
        kubectl create -f rbac-config.yaml
        ```
        {: pre}

3. Inizializza Helm e installa `tiller` con l'account di servizio che hai creato.

    ```
    helm init --service-account tiller
    ```
    {: pre}

4. Verifica che il pod `tiller-deploy` abbia uno **Status** di `Running` nel tuo cluster.

    ```
    kubectl get pods -n kube-system -l app=helm
    ```
    {: pre}

    Output di esempio:

    ```
    NAME                            READY     STATUS    RESTARTS   AGE
    tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
    ```
    {: screen}

5. Aggiungi il repository Helm {{site.data.keyword.Bluemix_notm}} alla tua istanza Helm.

    ```
    helm repo add ibm  https://registry.bluemix.net/helm/ibm
    ```
    {: pre}

6. Elenca i grafici Helm attualmente disponibili nel repository {{site.data.keyword.Bluemix_notm}}. 

    ```
    helm search ibm
    ```
    {: pre}

7. Per ulteriori informazioni su un grafico, elencane le impostazioni e i valori predefiniti.

    Ad esempio, per visualizzare le impostazioni, la documentazione e i valori predefiniti per il grafico Helm del servizio VPN IPSec strongSwan:

    ```
    helm inspect ibm/strongswan
    ```
    {: pre}


### Link Helm correlati
{: #helm_links}

* Per utilizzare il grafico Helm strongSwan, vedi [Configurazione della connettività VPN con il grafico Helm del servizio VPN IPSec strongSwan](cs_vpn.html#vpn-setup).
* Visualizza i grafici Helm disponibili che puoi utilizzare con {{site.data.keyword.Bluemix_notm}} nella GUI [Helm Charts Catalog ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/containers-kubernetes/solutions/helm-charts).
* Ulteriori informazioni sui comandi Helm utilizzati per configurare e gestire i grafici Helm sono disponibili nella <a href="https://docs.helm.sh/helm/" target="_blank">documentazione Helm <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>.
* Ulteriori informazioni su come puoi [aumentare la velocità di distribuzione con i grafici Helm Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://developer.ibm.com/recipes/tutorials/increase-deployment-velocity-with-kubernetes-helm-charts/).

## Visualizzazione delle risorse del cluster Kubernetes
{: #weavescope}

Weave Scope fornisce un diagramma visivo delle tue risorse all'interno di un cluster Kubernetes, inclusi servizi, pod, contenitori e altro. Weave Scope fornisce metriche interattive per CPU e memoria e strumenti per l'accodamento e l'esecuzione in un contenitore.
{:shortdesc}

Prima di iniziare:

-   Ricorda di non esporre le informazioni del tuo cluster pubblicamente su internet. Completa questa procedura per distribuire in sicurezza Weave Scope e accedervi da un browser web
localmente.
-   Se non ne hai già uno, [crea un
cluster standard](cs_clusters.html#clusters_ui). Weave Scope può essere intensivo per la CPU, specialmente l'applicazione. Esegui Weave Scope con grandi cluster a pagamento, non con cluster gratuito.
-   [Indirizza la tua
CLI](cs_cli_install.html#cs_cli_configure) al tuo cluster per eseguire i comandi kubectl.


Per utilizzare Weave Scope con un cluster:
2.  Distribuisci uno dei file di configurazione delle autorizzazioni RBAC fornite nel
cluster.

    Per abilitare le autorizzazioni di lettura/scrittura:

    ```
    kubectl apply -f "https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/weave-scope/weave-scope-rbac.yaml"
    ```
    {: pre}

    Per abilitare le autorizzazioni di
sola
lettura:

    ```
    kubectl apply --namespace weave -f "https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/weave-scope/weave-scope-rbac-readonly.yaml"
    ```
    {: pre}

    Output:

    ```
    clusterrole "weave-scope-mgr" created
    clusterrolebinding "weave-scope-mgr-role-binding" created
    ```
    {: screen}

3.  Distribuisci il servizio Weave Scope, che è accessibile privatamente dall'indirizzo IP del cluster.

    <pre class="pre">
    <code>kubectl apply --namespace weave -f "https://cloud.weave.works/k8s/scope.yaml?k8s-version=$(kubectl version | base64 | tr -d '&bsol;n')"</code>
    </pre>

    Output:

    ```
    serviceaccount "weave-scope" created
    deployment "weave-scope-app" created
    service "weave-scope-app" created
    daemonset "weave-scope-agent" created
    ```
    {: screen}

4.  Esegui un comando di inoltro della porta per aprire il servizio nel tuo computer. La volta successiva che accedi a Weave Scope, puoi eseguire questo comando di inoltro della porta senza dover completare nuovamente i precedenti passi di configurazione.

    ```
    kubectl port-forward -n weave "$(kubectl get -n weave pod --selector=weave-scope-component=app -o jsonpath='{.items..metadata.name}')" 4040
    ```
    {: pre}

    Output:

    ```
    Forwarding from 127.0.0.1:4040 -> 4040
    Forwarding from [::1]: :1]:4040 -> 4040
    Handling connection for 4040
    ```
    {: screen}

5.  Apri il tuo browser web in `http://localhost:4040`. Senza i componenti predefiniti distribuiti, viene visualizzato il seguente diagramma. Puoi scegliere di visualizzare i diagrammi della topologia o le tabelle delle risorse Kubernetes nel cluster.

     <img src="images/weave_scope.png" alt="Topologia di esempio da Weave Scope" style="width:357px;" />


[Ulteriori informazioni sulle funzioni Weave Scope ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.weave.works/docs/scope/latest/features/).

<br />

