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



# Risoluzione dei problemi di rete del cluster
{: #cs_troubleshoot_network}

Mentre utilizzi {{site.data.keyword.containerlong}}, tieni presente queste tecniche per la risoluzione dei problemi di rete del cluster.
{: shortdesc}

Se hai un problema più generale, prova a [eseguire il debug del cluster](cs_troubleshoot.html).
{: tip}


## Impossibile collegarsi a un'applicazione tramite un servizio del programma di bilanciamento del carico
{: #cs_loadbalancer_fails}

{: tsSymptoms}
Hai esposto pubblicamente la tua applicazione creando un servizio del programma di bilanciamento del carico nel tuo cluster. Quando tenti di collegarti alla tua applicazione utilizzando l'indirizzo IP pubblico del programma di bilanciamento del carico, la connessione non riesce o va in timeout. 

{: tsCauses}
Il tuo servizio del programma di bilanciamento del carico potrebbe non funzionare correttamente per uno dei seguenti motivi:

-   Il cluster è un cluster gratuito o standard con solo un nodo di lavoro.
-   Il cluster non è ancora stato completamente distribuito.
-   Lo script di configurazione per il tuo servizio del programma di bilanciamento del carico include degli errori.

{: tsResolve}
Per risolvere i problemi del tuo servizio del programma di bilanciamento del carico:

1.  Verifica di aver configurato un cluster standard che è stato completamente distribuito e che abbia almeno due nodi di lavoro per garantire l'elevata disponibilità al tuo servizio del programma di bilanciamento del carico.

  ```
  bx cs workers <cluster_name_or_ID>
  ```
  {: pre}

    Nel tuo output della CLI, assicurati che lo **Stato** del tuo nodo di lavoro visualizzi **Pronto** e che il **Tipo di macchina** sia diverso da **gratuito**.

2.  Verifica l'accuratezza del file di configurazione del tuo servizio del programma di bilanciamento del carico.

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

    1.  Verifica di aver definito **LoadBalancer** come il tipo del tuo servizio.
    2.  Nella sezione `spec.selector` del servizio LoadBalancer, assicurati che `<selector_key>` e `<selector_value>` siano gli stessi della coppia chiave/valore che hai utilizzato nella sezione `spec.template.metadata.labels` del tuo file yaml di distribuzione. Se le etichette non corrispondono, la sezione **Endpoints** nel tuo servizio LoadBalancer mostra **<none>** e la tua applicazione non sarà accessibile da internet.
    3.  Verifica di aver utilizzato la **porta** su cui è in ascolto la tua applicazione.

3.  Verifica il tuo servizio del programma di bilanciamento del carico e controlla la sezione **Eventi** per trovare errori potenziali.

    ```
    kubectl describe service <myservice>
    ```
    {: pre}

    Ricerca i seguenti messaggi di errore:

    <ul><li><pre class="screen"><code>Clusters with one node must use services of type NodePort</code></pre></br>Per utilizzare il servizio del programma di bilanciamento del carico, devi disporre di un cluster standard con almeno due nodi di lavoro.</li>
    <li><pre class="screen"><code>No cloud provider IPs are available to fulfill the load balancer service request. Add a portable subnet to the cluster and try again</code></pre></br>Questo messaggio di errore indica che non è stato lasciato alcun indirizzo IP pubblico portatile che può essere assegnato al tuo servizio del programma di bilanciamento del carico. Fai riferimento a <a href="cs_subnets.html#subnets">Aggiunta di sottoreti ai cluster</a> per trovare informazioni su come richiedere gli indirizzi IP pubblici portatili per il tuo cluster. Dopo che gli indirizzi IP pubblici portatili sono disponibili per il cluster, il servizio del programma di bilanciamento del carico viene creato automaticamente.</li>
    <li><pre class="screen"><code>Requested cloud provider IP <cloud-provider-ip> is not available. The following cloud provider IPs are available: <available-cloud-provider-ips></code></pre></br>Hai definito un indirizzo IP pubblico portatile per il tuo servizio del programma di bilanciamento del carico utilizzando la sezione **loadBalancerIP**, ma questo indirizzo IP pubblico portatile non è disponibile nella tua sottorete pubblica portatile. Nella sezione **loadBalancerIP** il tuo script di configurazione rimuove l'indirizzo IP esistente e aggiunge uno degli indirizzi IP pubblici portatili. Puoi anche rimuovere la sezione **loadBalancerIP** dal tuo script in modo che possa venirne assegnato uno automaticamente. </li>
    <li><pre class="screen"><code>No available nodes for load balancer services</code></pre>Non disponi di abbastanza nodi di lavoro da distribuire a un servizio del programma di bilanciamento del carico. Un motivo potrebbe essere che hai distribuito un cluster standard con più di un nodo di lavoro ma il provisioning ha avuto esito negativo.</li>
    <ol><li>Elenca i nodi di lavoro disponibili.</br><pre class="codeblock"><code>kubectl get nodes</code></pre></li>
    <li>Se vengono trovati almeno due nodi di lavoro, elenca i dettagli del nodo di lavoro.</br><pre class="codeblock"><code>bx cs worker-get [&lt;cluster_name_or_ID&gt;] &lt;worker_ID&gt;</code></pre></li>
    <li>Assicurati che gli ID delle VLAN privata e pubblica dei nodi di lavoro restituiti dai comandi <code>kubectl get nodes</code> e <code>bx cs [&lt;cluster_name_or_ID&gt;] worker-get</code> corrispondano.</li></ol></li></ul>

4.  Se stai utilizzando un dominio personalizzato per collegarti al tuo servizio del programma di bilanciamento del carico, assicurati che sia associato all'indirizzo IP pubblico del tuo servizio del programma di bilanciamento del carico.
    1.  Trova l'indirizzo IP pubblico del servizio di bilanciamento del carico.

        ```
        kubectl describe service <service_name> | grep "LoadBalancer Ingress"
        ```
        {: pre}

    2.  Verifica che il tuo dominio personalizzato sia associato all'indirizzo IP pubblico portatile del tuo servizio di bilanciamento del carico nel record di puntatore (PTR).

<br />




## Impossibile collegarsi a un'applicazione tramite Ingress
{: #cs_ingress_fails}

{: tsSymptoms}
Hai esposto pubblicamente la tua applicazione creando una risorsa Ingress per la tua applicazione nel tuo cluster. Quando tenti di collegarti alla tua applicazione tramite il dominio secondario o l'indirizzo IP pubblico del programma di bilanciamento del carico (ALB) dell'applicazione Ingress, la connessione non riesce o va in timeout.

{: tsCauses}
Ingress potrebbe non funzionare correttamente per i seguenti motivi:
<ul><ul>
<li>Il cluster non è ancora stato completamente distribuito.
<li>Il cluster è stato configurato come un cluster gratuito o standard con solo un nodo di lavoro.
<li>Lo script di configurazione Ingress include degli errori.
</ul></ul>

{: tsResolve}
Per risolvere i problemi con Ingress:

1.  Verifica di aver configurato un cluster standard che è stato completamente distribuito e che abbia almeno due nodi di lavoro per garantire l'elevata disponibilità al tuo ALB.

  ```
  bx cs workers <cluster_name_or_ID>
  ```
  {: pre}

    Nel tuo output della CLI, assicurati che lo **Stato** del tuo nodo di lavoro visualizzi **Pronto** e che il **Tipo di macchina** sia diverso da **gratuito**.

2.  Richiama l'indirizzo IP pubblico e il dominio secondario dell'ALB e quindi esegui il ping ad entrambi.

    1.  Richiama il dominio secondario ALB.

      ```
      bx cs cluster-get <cluster_name_or_ID> | grep "Ingress subdomain"
      ```
      {: pre}

    2.  Esegui il ping del dominio secondario ALB.

      ```
      ping <ingress_subdomain>
      ```
      {: pre}

    3.  Richiama l'indirizzo IP pubblico del tuo ALB.

      ```
      nslookup <ingress_subdomain>
      ```
      {: pre}

    4.  Esegui il ping dell'indirizzo IP pubblico dell'ALB. 

      ```
      ping <ALB_IP>
      ```
      {: pre}

    Se la CLI restituisce un timeout per il dominio secondario o per l'indirizzo IP pubblico dell'ALB e hai configurato un firewall personalizzato che protegge i tuoi nodi di lavoro, apri ulteriori porte e gruppi di rete nel tuo [firewall](cs_troubleshoot_clusters.html#cs_firewall).

3.  Se stai utilizzando un dominio personalizzato, assicurati che sia associato al dominio secondario o all'indirizzo IP pubblico dell'ALB fornito da IBM con il tuo provider DNS.
    1.  Se hai utilizzato il dominio secondario dell'ALB subdomain, verifica il tuo record di nome canonico (CNAME).
    2.  Se hai utilizzato l'indirizzo IP pubblico dell'ALB, verifica che il tuo dominio personalizzato sia associato all'indirizzo IP pubblico portatile nel record di puntatore (PTR).
4.  Verifica il tuo file di configurazione della risorsa Ingress.

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

    1.  Verifica che il dominio secondario dell'ALB e il certificato TLS siano corretti. Per trovare il dominio secondario e il certificato TLS forniti da IBM, esegui `bx cs cluster-get <cluster_name_or_ID>`.
    2.  Assicurati che la tua applicazione sia in ascolto sullo stesso percorso configurato nella sezione **percorso** del tuo Ingress. Se la tua applicazione è configurata per essere in ascolto nel percorso root, includi **/** al tuo percorso.
5.  Controlla la distribuzione di Ingress e cerca potenziali messaggi di avvertenza o di errore.

    ```
    kubectl describe ingress <myingress>
    ```
    {: pre}

    Ad esempio, nella sezione **Eventi** dell'output, potresti vedere dei messaggi di avvertenza sui valori non validi nella risorsa Ingress o in alcune annotazioni che hai utilizzato.

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

6.  Verifica i log del tuo ALB.
    1.  Richiama l'ID dei pod Ingress in esecuzione nel tuo cluster.

      ```
      kubectl get pods -n kube-system | grep alb
      ```
      {: pre}

    2.  Richiama i log per ogni pod Ingress.

      ```
      kubectl logs <ingress_pod_ID> nginx-ingress -n kube-system
      ```
      {: pre}

    3.  Ricerca messaggi di errore nei log dell'ALB.

<br />


## Problemi con il segreto del programma di bilanciamento del carico dell'applicazione Ingress
{: #cs_albsecret_fails}

{: tsSymptoms}
Dopo aver distribuito un segreto del programma di bilanciamento del carico (ALB) dell'applicazione Ingress al tuo cluster, il campo `Descrizione` non viene aggiornato con il nome del segreto quando visualizzi il tuo certificato in {{site.data.keyword.cloudcerts_full_notm}}.

Quando elenchi le informazioni sul segreto dell'ALB, lo stato è `*_failed`. Ad esempio, `create_failed`, `update_failed`, `delete_failed`.

{: tsResolve}
Controlla i seguenti motivi sul perché il segreto dell'ALB può avere un malfunzionamento e i passi di risoluzione del problema corrispondenti:

<table>
<caption>Risoluzione dei problemi con i segreti del programma di bilanciamento del carico dell'applicazione Ingress</caption>
 <thead>
 <th>Perché sta succedendo</th>
 <th>Come porvi rimedio</th>
 </thead>
 <tbody>
 <tr>
 <td>Non disponi dei ruoli di accesso necessari per scaricare e aggiornare i dati del certificato.</td>
 <td>Controlla con il tuo amministratore dell'account che ti vengano assegnati i ruoli **Operatore** e **Editor** per la tua istanza {{site.data.keyword.cloudcerts_full_notm}}. Per ulteriori informazioni, vedi <a href="/docs/services/certificate-manager/access-management.html#managing-service-access-roles">Gestione dell'accesso al servizio</a> per {{site.data.keyword.cloudcerts_short}}.</td>
 </tr>
 <tr>
 <td>Il CRN del certificato fornito al momento della creazione, dell'aggiornamento o della rimozione non appartiene allo stesso account del cluster.</td>
 <td>Controlla che il CRN del certificato che hai fornito sia importato in un'istanza del servizio {{site.data.keyword.cloudcerts_short}} che viene distribuita nello stesso account del tuo cluster.</td>
 </tr>
 <tr>
 <td>Il CRN del certificato fornito al momento della creazione non è corretto.</td>
 <td><ol><li>Controlla l'accuratezza della stringa CRN del certificato che fornisci.</li><li>Se il certificato CRN risulta essere accurato, prova ad aggiornare il segreto: <code>bx cs alb-cert-deploy --update --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li><li>Se questo comando produce lo stato <code>update_failed</code>, rimuovi il segreto: <code>bx cs alb-cert-rm --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt;</code></li><li>Distribuisci di nuovo il segreto: <code>bx cs alb-cert-deploy --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li></ol></td>
 </tr>
 <tr>
 <td>Il CRN del certificato fornito al momento dell'aggiornamento non è corretto.</td>
 <td><ol><li>Controlla l'accuratezza della stringa CRN del certificato che fornisci.</li><li>Se il certificato CRN risulta essere accurato, rimuovi il segreto: <code>bx cs alb-cert-rm --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt;</code></li><li>Distribuisci di nuovo il segreto: <code>bx cs alb-cert-deploy --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li><li>Prova ad aggiornare il segreto: <code>bx cs alb-cert-deploy --update --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li></ol></td>
 </tr>
 <tr>
 <td>Il servizio {{site.data.keyword.cloudcerts_long_notm}} sta riscontrando un tempo di inattività.</td>
 <td>Controlla che il tuo servizio {{site.data.keyword.cloudcerts_short}} sia attivo e in esecuzione.</td>
 </tr>
 </tbody></table>

<br />


## Impossibile ottenere un dominio secondario per l'ALB Ingress
{: #cs_subnet_limit}

{: tsSymptoms}
Quando esegui `bx cs cluster-get <cluster>`, il tuo cluster si trova in uno stato `normal` ma non ci sono **domini secondari Ingress** disponibili.

Potresti ricevere un messaggio di errore simile al seguente.

```
There are already the maximum number of subnets permitted in this VLAN.
```
{: screen}

{: tsCauses}
Quando crei un cluster, vengono richieste 8 sottoreti portatili pubbliche e 8 private sulla VLAN che hai specificato. Per {{site.data.keyword.containershort_notm}}, le VLAN hanno un limite di 40 sottoreti. Se la VLAN del cluster ha già raggiunto questo limite, il provisioning del **dominio secondario Ingress** ha esito negativo.

Per visualizzare quante sottoreti sono presenti in una VLAN:
1.  Dalla [console dell'infrastruttura IBM Cloud (SoftLayer)](https://control.bluemix.net/), seleziona **Rete** > **Gestione IP** > **VLAN**.
2.  Fai clic sul **Numero VLAN** della VLAN che hai usato per creare il tuo cluster. Esamina la sezione **Sottoreti** per vedere se ci sono 40 o più sottoreti. 

{: tsResolve}
Se hai bisogno di una nuova VLAN, ordinane una [contattando il supporto {{site.data.keyword.Bluemix_notm}}](/docs/get-support/howtogetsupport.html#getting-customer-support). Quindi [crea un cluster](cs_cli_reference.html#cs_cluster_create) che utilizzi questa nuova VLAN. 

Se hai un'altra VLAN disponibile, puoi [configurare lo spanning VLAN](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning) nel tuo cluster esistente. Dopo, puoi aggiungere nuovi nodi di lavoro al cluster che utilizza l'altra VLAN con sottoreti disponibili.

Se non stai utilizzando tutte le sottoreti nella VLAN, puoi riutilizzare le sottoreti nel cluster.
1.  Controlla che le sottoreti che desideri utilizzare siano disponibili. **Nota**: l'account dell'infrastruttura che stai utilizzando potrebbe essere condiviso tra più account {{site.data.keyword.Bluemix_notm}}. Se è questo il caso, anche se esegui il comando `bx cs subnets` per vedere le sottoreti con **Bound Clusters**, potrai vedere solo le informazioni per i tuoi cluster. Controlla con il proprietario dell'account dell'infrastruttura per assicurarti che le sottoreti siano disponibili e non in uso da parte di un altro account o team.

2.  [Crea un cluster](cs_cli_reference.html#cs_cluster_create) con l'opzione `--no-subnet` in modo tale che il servizio non tenti di creare nuove sottoreti. Specifica l'ubicazione e la VLAN in cui le sottoreti sono disponibili per essere utilizzate.

3.  Usa il [comando](cs_cli_reference.html#cs_cluster_subnet_add) `bx cs cluster-subnet-add` per aggiungere sottoreti esistenti al tuo cluster. Per ulteriori informazioni, consulta [Aggiunta o riutilizzo di sottoreti personalizzate ed esistenti nei cluster Kubernetes](cs_subnets.html#custom).

<br />


## Impossibile stabilire la connettività VPN con il grafico Helm strongSwan
{: #cs_vpn_fails}

{: tsSymptoms}
Quando controlli la connettività VPN eseguendo `kubectl exec -n kube-system  $STRONGSWAN_POD -- ipsec status`, non vedi lo stato `ESTABLISHED` oppure il pod VPN è in uno stato `ERROR` o continua ad arrestarsi e a riavviarsi.

{: tsCauses}
Il tuo file di configurazione del grafico Helm ha valori errati, valori mancanti o errori di sintassi.

{: tsResolve}
Quando tenti di stabilire la connettività VPN con il grafico Helm strongSwan per la prima volta, è probabile che lo stato della VPN non sia `ESTABLISHED`. Potresti dover verificare diversi tipi di problemi e modificare di conseguenza il tuo file di configurazione. Per risolvere i problemi relativi alla connettività VPN strongSwan:

1. Controlla le impostazioni dell'endpoint VPN in loco rispetto alle impostazioni nel tuo file di configurazione. Se le impostazioni non corrispondono: 

    <ol>
    <li>Elimina il grafico Helm esistente.</br><pre class="codeblock"><code>helm delete --purge <release_name></code></pre></li>
    <li>Correggi i valori errati nel file <code>config.yaml</code> e salva il file aggiornato.</li>
    <li>Installa il nuovo grafico Helm.</br><pre class="codeblock"><code>helm install -f config.yaml --namespace=kube-system --name=<release_name> bluemix/strongswan</code></pre></li>
    </ol>

2. Se il pod della VPN è in uno stato `ERROR` o continua ad arrestarsi e a riavviarsi, potrebbe essere dovuto alla convalida dei parametri delle impostazioni `ipsec.conf` nella mappa di configurazione del grafico.

    <ol>
    <li>Controlla eventuali errori di convalida nei log del pod strongSwan. </br><pre class="codeblock"><code>kubectl logs -n kube-system $STRONGSWAN_POD</code></pre></li>
    <li>Se i log contengono errori di convalida, elimina il grafico Helm esistente. </br><pre class="codeblock"><code>helm delete --purge <release_name></code></pre></li>
    <li>Correggi i valori errati nel file `config.yaml` e salva il file aggiornato.</li>
    <li>Installa il nuovo grafico Helm.</br><pre class="codeblock"><code>helm install -f config.yaml --namespace=kube-system --name=<release_name> bluemix/strongswan</code></pre></li>
    </ol>

3. Esegui i 5 test Helm inclusi nella definizione del grafico strongSwan.

    <ol>
    <li>Esegui i test Helm.</br><pre class="codeblock"><code>helm test vpn</code></pre></li>
    <li>Se un test non riesce, fai riferimento a [Descrizione dei test di connettività VPN Helm](cs_vpn.html#vpn_tests_table) per informazioni su ciascun test e sul perché potrebbe non riuscire. <b>Nota</b>: alcuni dei test hanno requisiti che sono impostazioni facoltative nella configurazione VPN. Se alcuni test non riescono, gli errori potrebbero essere accettabili a seconda che queste impostazioni facoltative siano state specificate o meno.</li>
    <li>Visualizza l'output di un test non riuscito nei log del pod di test.<br><pre class="codeblock"><code>kubectl logs -n kube-system <test_program></code></pre></li>
    <li>Elimina il grafico Helm esistente.</br><pre class="codeblock"><code>helm delete --purge <release_name></code></pre></li>
    <li>Correggi i valori errati nel file <code>config.yaml</code> e salva il file aggiornato.</li>
    <li>Installa il nuovo grafico Helm.</br><pre class="codeblock"><code>helm install -f config.yaml --namespace=kube-system --name=<release_name> bluemix/strongswan</code></pre></li>
    <li>Per controllare le tue modifiche:<ol><li>Ottieni i pod di test correnti.</br><pre class="codeblock"><code>kubectl get pods -a -n kube-system -l app=strongswan-test</code></pre></li><li>Ripulisci i pod di test correnti.</br><pre class="codeblock"><code>kubectl delete pods -n kube-system -l app=strongswan-test</code></pre></li><li>Esegui di nuovo i test.</br><pre class="codeblock"><code>helm test vpn</code></pre></li>
    </ol></ol>

4. Esegui lo strumento di debug VPN fornito all'interno dell'immagine del pod VPN. 

    1. Imposta la variabile di ambiente `STRONGSWAN_POD`.

        ```
        export STRONGSWAN_POD=$(kubectl get pod -n kube-system -l app=strongswan,release=vpn -o jsonpath='{ .items[0].metadata.name }')
        ```
        {: pre}

    2. Esegui lo strumento di debug.

        ```
        kubectl exec -n kube-system  $STRONGSWAN_POD -- vpnDebug
        ```
        {: pre}

        Lo strumento emette diverse pagine di informazioni mentre esegue vari test per i comuni problemi di rete. Le righe di output che iniziano con `ERROR`, `WARNING`, `VERIFY` o `CHECK` indicano possibili errori con la connettività VPN.

    <br />


## La connettività VPN strongSwan si interrompe dopo l'aggiunta o l'eliminazione del nodo di lavoro
{: #cs_vpn_fails_worker_add}

{: tsSymptoms}
In precedenza hai stabilito una connessione VPN funzionante utilizzando il servizio VPN IPsec strongSwan. Tuttavia, dopo aver aggiunto o eliminato un nodo di lavoro nel cluster, si verificano uno o più dei seguenti sintomi:

* Non hai uno stato `ESTABLISHED` per la VPN 
* Non puoi accedere ai nuovi nodi di lavoro dalla tua rete in loco 
* Non puoi accedere alla rete remota dai pod in esecuzione nei nuovi nodi di lavoro 

{: tsCauses}
Se hai aggiunto un nodo di lavoro:

* È stato eseguito il provisioning del nodo di lavoro su una nuova sottorete privata che non è esposta tramite la connessione VPN dalle tue impostazioni `localSubnetNAT` o `local.subnet` esistenti 
* le rotte VPN non possono essere aggiunte al nodo di lavoro perché il nodo di lavoro ha corruzioni o etichette che non sono incluse nelle tue impostazioni `tolerations` o `nodeSelector` esistenti
* Il pod VPN è in esecuzione sul nuovo nodo di lavoro, ma l'indirizzo IP pubblico di tale nodo di lavoro non è consentito attraverso il firewall in loco 

Se hai eliminato il nodo di lavoro:

* Quel nodo di lavoro era l'unico nodo su cui era in esecuzione un pod VPN, a causa delle restrizioni su determinate corruzioni o etichette nelle tue impostazioni `tolerations` o `nodeSelector` esistenti 

{: tsResolve}
Aggiorna i valori del grafico Helm per riflettere le modifiche del nodo di lavoro:

1. Elimina il grafico Helm esistente.

    ```
    helm delete --purge <release_name>
    ```
    {: pre}

2. Apri il file di configurazione per il tuo servizio VPN strongSwan.

    ```
    helm inspect values ibm/strongswan > config.yaml
    ```
    {: pre}

3. Controlla le seguenti impostazioni e apporta modifiche per riflettere i nodi di lavoro eliminati o aggiunti secondo necessità. 

    Se hai aggiunto un nodo di lavoro:

    <table>
    <caption>Impostazioni</didascalia del nodo di lavoro?
     <thead>
     <th>Impostazione</th>
     <th>Descrizione</th>
     </thead>
     <tbody>
     <tr>
     <td><code>localSubnetNAT</code></td>
     <td>Il nodo di lavoro aggiunto potrebbe essere distribuito su una nuova sottorete privata diversa rispetto alle altre sottoreti esistenti su cui sono attivi altri nodi di lavoro. Se utilizzi il NAT della sottorete per riassociare gli indirizzi IP locali privati del tuo cluster e il nodo di lavoro è stato aggiunto in una nuova sottorete, aggiungi il CIDR della nuova sottorete a questa impostazione. </td>
     </tr>
     <tr>
     <td><code>nodeSelector</code></td>
     <td>Se in precedenza hai limitato la distribuzione del pod VPN ai nodi di lavoro con un'etichetta specifica, assicurati che anche il nodo di lavoro aggiunto abbia tale etichetta.</td>
     </tr>
     <tr>
     <td><code>tolerations</code></td>
     <td>Se il nodo di lavoro aggiunto è corrotto, modifica questa impostazione per consentire l'esecuzione del pod VPN su tutti i nodi di lavoro corrotti con qualsiasi corruzione o corruzioni specifiche. </td>
     </tr>
     <tr>
     <td><code>local.subnet</code></td>
     <td>Il nodo di lavoro aggiunto potrebbe essere distribuito su una nuova sottorete privata diversa rispetto alle sottoreti esistenti su cui sono attivi altri nodi di lavoro.  Se le tue applicazioni sono esposte dai servizi NodePort o LoadBalancer sulla rete privata e si trovano sul nodo di lavoro aggiunto, aggiungi il nuovo CIDR della sottorete a questa impostazione. **Nota**: se aggiungi valori a `local.subnet`, controlla le impostazioni VPN per la sottorete locale per vedere se devono essere aggiornate.</td>
     </tr>
     </tbody></table>

    Se hai eliminato il nodo di lavoro:

    <table>
    <caption>Impostazioni del nodo di lavoro </caption>
     <thead>
     <th>Impostazione</th>
     <th>Descrizione</th>
     </thead>
     <tbody>
     <tr>
     <td><code>localSubnetNAT</code></td>
     <td>Se stai utilizzando il NAT di sottorete per riassociare specifici indirizzi IP locali privati, rimuovi, da questa impostazione, gli indirizzi IP che provengono dal vecchio nodo di lavoro. Se utilizzi il NAT della sottorete per riassociare intere sottoreti e non ci sono nodi di lavoro rimanenti in una sottorete, rimuovi il CIDR di tale sottorete da questa impostazione. </td>
     </tr>
     <tr>
     <td><code>nodeSelector</code></td>
     <td>Se hai limitato la distribuzione del pod VPN a un solo nodo di lavoro ed è stato eliminato, modifica questa impostazione per consentire l'esecuzione del pod VPN su altri nodi di lavoro.</td>
     </tr>
     <tr>
     <td><code>tolerations</code></td>
     <td>Se il nodo di lavoro che hai eliminato non era corrotto, ma lo sono gli unici nodi di lavoro rimasti, modifica questa impostazione per consentire l'esecuzione del pod VPN sui nodi di lavoro con qualsiasi corruzione o con corruzioni specifiche.
     </td>
     </tr>
     </tbody></table>

4. Installa il nuovo grafico Helm con i tuoi valori aggiornati.

    ```
    helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/strongswan
    ```
    {: pre}

5. Controlla lo stato di distribuzione del grafico. Quando il grafico è pronto, il campo **STATO** vicino alla parte superiore dell'output ha un valore di `DEPLOYED`.

    ```
    helm status <release_name>
    ```
    {: pre}

6. In alcuni casi, potresti dover modificare le impostazioni in loco e le impostazioni del tuo firewall in modo che corrispondano alle modifiche che hai apportato al file di configurazione VPN.

7. Avvia la VPN.
    * Se la connessione VPN viene avviata dal cluster (`ipsec.auto` è impostato su `start`), avvia la VPN sul gateway in loco e quindi avviala sul cluster.
    * Se la connessione VPN viene avviata dal gateway in loco (`ipsec.auto` è impostato su `auto`), avvia la VPN sul cluster e quindi avviala sul gateway in loco.

8. Imposta la variabile di ambiente `STRONGSWAN_POD`.

    ```
    export STRONGSWAN_POD=$(kubectl get pod -n kube-system -l app=strongswan,release=<release_name> -o jsonpath='{ .items[0].metadata.name }')
    ```
    {: pre}

9. Controlla lo stato della VPN.

    ```
    kubectl exec -n kube-system  $STRONGSWAN_POD -- ipsec status
    ```
    {: pre}

    * Se la connessione VPN ha lo stato `ESTABLISHED`, la connessione VPN ha avuto esito positivo. Non sono necessarie ulteriori azioni.

    * Se hai ancora problemi di connessione, vedi [Impossibile stabilire la connettività VPN con il grafico Helm strongSwan](#cs_vpn_fails) per ulteriori informazioni su come risolvere i problemi con la connessione VPN.

<br />



## Impossibile richiamare le politiche di rete Calico
{: #cs_calico_fails}

{: tsSymptoms}
Quando tenti di visualizzare le politiche di rete Calico nel tuo cluster eseguendo `calicoctl get policy`, riscontri uno dei seguenti risultati non previsti o messaggi di errore:
- Un elenco vuoto
- Un elenco delle precedenti politiche Calico v2 invece delle v3
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'projectcalico.org/v3'`

Quando tenti di visualizzare le politiche di rete Calico nel tuo cluster eseguendo `calicoctl get GlobalNetworkPolicy`, riscontri uno dei seguenti risultati non previsti o messaggi di errore:
- Un elenco vuoto
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'v1'`
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'projectcalico.org/v3'`
- `Failed to get resources: Resource type 'GlobalNetworkPolicy' is not supported`

{: tsCauses}
Per utilizzare le politiche Calico, quattro fattori devono essere allineati: la tua versione del cluster Kubernetes, la versione della CLI Calico, la sintassi del file di configurazione Calico e i comandi di visualizzazione della politica. Uno o più di questi fattori non è alla versione corretta.

{: tsResolve}
Quando il tuo cluster è alla [versione Kubernetes 1.10 o successiva](cs_versions.html), devi utilizzare la CLI Calico v3.1, la sintassi del file di configurazione `calicoctl.cfg` v3 e i comandi `calicoctl get GlobalNetworkPolicy` e `calicoctl get NetworkPolicy`.

Quando il tuo cluster è alla [versione Kubernetes 1.9 o precedente](cs_versions.html), devi utilizzare la CLI Calico v1.6.3, la sintassi del file di configurazione `calicoctl.cfg` v2 e il comando `calicoctl get policy`.

Per assicurarti che tutti i fattori Calico siano allineati:

1. Visualizza la tua versione del cluster Kubernetes.
    ```
    bx cs cluster-get <cluster_name>
    ```
    {: pre}

    * Se il tuo cluster è alla versione Kubernetes 1.10 o successiva:
        1. [Installa e configura la CLI Calico versione 3.1.1](cs_network_policy.html#1.10_install). La configurazione include l'aggiornamento manuale del file `calicoctl.cfg` per utilizzare la sintassi Calico v3.
        2. Assicurati che tutte le politiche che crei e vuoi applicare al tuo cluster utilizzino la [sintassi Calico v3 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/networkpolicy). Se hai un file `.yaml` o `.json` della politica esistente alla sintassi Calico v2, puoi convertirlo a Calico v3 utilizzando il comando [`calicoctl convert` ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.projectcalico.org/v3.1/reference/calicoctl/commands/convert).
        3. Per [visualizzare le politiche](cs_network_policy.html#1.10_examine_policies), assicurati di stare utilizzando `calicoctl get GlobalNetworkPolicy` per le politiche globali e `calicoctl get NetworkPolicy --namespace <policy_namespace>` per le politiche dedicate a spazi dei nomi specifici.

    * Se il tuo cluster è alla versione Kubernetes 1.9 o precedente: 
        1. [Installa e configura la CLI Calico versione 1.6.3](cs_network_policy.html#1.9_install). Assicurati che il file `calicoctl.cfg` utilizzi la sintassi Calico v2.
        2. Assicurati che tutte le politiche che crei e vuoi applicare al tuo cluster utilizzino la [sintassi Calico v2 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.projectcalico.org/v2.6/reference/calicoctl/resources/policy). 
        3. Per [visualizzare le politiche](cs_network_policy.html#1.9_examine_policies), assicurati di stare utilizzando `calicoctl get policy`.

Prima di aggiornare il tuo cluster da Kubernetes versione 1.9 o precedente alla versione 1.10 o successiva, rivedi [Preparazione dell'aggiornamento a Calico v3](cs_versions.html#110_calicov3).
{: tip}

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

