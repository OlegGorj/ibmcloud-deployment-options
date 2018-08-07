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




# Esposizione delle applicazioni con Ingress
{: #ingress}

Esponi più applicazioni nel tuo cluster Kubernetes creando risorse Ingress che vengono gestite dal programma di bilanciamento del carico dell'applicazione fornito da IBM in {{site.data.keyword.containerlong}}.
{:shortdesc}

## Gestione del traffico di rete utilizzando Ingress
{: #planning}

Ingress è un servizio Kubernetes che bilancia i carichi di lavoro del traffico di rete nel tuo cluster inoltrando le richieste pubbliche o private alle tue applicazioni. Puoi utilizzare Ingress per esporre più servizi dell'applicazione ad una rete pubblica o privata utilizzando una rotta pubblica o privata univoca.
{:shortdesc}



Ingress è composto da due componenti:
<dl>
<dt>Programma di bilanciamento del carico (ALB - Application load balancer)</dt>
<dd>L'ALB è un programma di bilanciamento del carico esterno che ascolta le richieste di servizio HTTP, HTTPS, TCP o UDP in entrata e le inoltra al pod dell'applicazione appropriato. Quando crei un cluster standard, {{site.data.keyword.containershort_notm}} crea automaticamente un ALB altamente disponibile per il tuo cluster e gli assegna una rotta pubblica univoca. La rotta pubblica è collegata a un indirizzo IP pubblico portatile che viene fornito nel tuo account dell'infrastruttura IBM Cloud (SoftLayer) durante la creazione del cluster. Viene inoltre creato automaticamente un ALB privato predefinito, ma non viene automaticamente abilitato.</dd>
<dt>Risorsa Ingress</dt>
<dd>Per esporre un'applicazione utilizzando Ingress, devi creare un servizio Kubernetes per la tua applicazione e registrare questo servizio con l'ALB definendo una risorsa Ingress. La risorsa Ingress è una risorsa Kubernetes che definisce le regole su come instradare le richieste in entrata per un'applicazione. La risorsa Ingress specifica anche il percorso al tuo servizio dell'applicazione che viene aggiunto alla rotta pubblica per formare un URL univoco dell'applicazione, ad esempio `mycluster.us-south.containers.appdomain.cloud/myapp`.
<br></br><strong>Nota</strong>: a partire dal 24 maggio 2018, il formato del dominio secondario Ingress cambia per i nuovi cluster.<ul><li>I cluster creati dopo il 24 maggio 2018 vengono assegnati a un dominio secondario nel nuovo formato, <code>&lt;cluster_name&gt;.&lt;region&gt;.containers.appdomain.cloud</code>.</li><li>I cluster creati prima del 24 maggio 2018 continuano ad utilizzare il dominio secondario assegnato nel vecchio formato, <code>&lt;cluster_name&gt;.&lt;region&gt;.containers.mybluemix.net</code>.</li></ul></dd>
</dl>

Il seguente diagramma mostra come Ingress dirige la comunicazione da Internet a un'applicazione:

<img src="images/cs_ingress.png" width="550" alt="Esponi un'applicazione in {{site.data.keyword.containershort_notm}} utilizzando Ingress" style="width:550px; border-style: none"/>

1. Un utente invia una richiesta alla tua applicazione accedendo all'URL dell'applicazione. Questo URL è l'URL pubblico per la tua applicazione esposta a cui è aggiunto il percorso della risorsa Ingress, ad esempio `mycluster.us-south.containers.appdomain.cloud/myapp`.

2. Un servizio di sistema DNS che funge da programma di bilanciamento del carico globale risolve l'URL nell'indirizzo IP pubblico portatile dell'ALB pubblico predefinito nel cluster. La richiesta viene instradata al servizio ALB Kubernetes per l'applicazione.

3. Il servizio Kubernetes instrada la richiesta all'ALB.

4. L'ALB verifica se esiste una regola di instradamento per il percorso `myapp` nel cluster. Se viene trovata una regola corrispondente, la richiesta viene inoltrata in base alle regole definite nella risorsa Ingress al pod in cui è distribuita l'applicazione. Se nel cluster vengono distribuite più istanze dell'applicazione, l'ALB bilancia il carico delle richieste tra i pod dell'applicazione.



<br />


## Prerequisiti
{: #config_prereqs}

Prima di iniziare ad utilizzare Ingress, controlla i seguenti prerequisiti.
{:shortdesc}

**Prerequisiti per tutte le configurazioni Ingress:**
- Ingress è disponibile solo per i cluster standard e richiede almeno due nodi di lavoro nel cluster per garantire l'alta disponibilità e l'applicazione di aggiornamenti periodici.
- La configurazione di Ingress richiede una [politica di accesso Amministratore](cs_users.html#access_policies). Verifica la tua [politica di accesso](cs_users.html#infra_access) corrente.



<br />


## Pianificazione della rete per spazi dei nomi singoli o multipli
{: #multiple_namespaces}

È necessaria almeno una risorsa Ingress per lo spazio dei nomi in cui hai delle applicazioni da esporre.
{:shortdesc}

<dl>
<dt>Tutte le applicazioni sono in uno spazio dei nomi</dt>
<dd>Se le applicazioni nel tuo cluster sono tutte nello stesso spazio dei nomi, è necessaria almeno una risorsa Ingress per definire le regole di instradamento delle applicazioni qui esposte. Ad esempio, se hai `app1` e `app2` esposte dai servizi in uno spazio dei nomi di sviluppo, puoi creare una risorsa Ingress nello spazio dei nomi. La risorsa specifica `domain.net` come host e registra i percorsi su cui è in ascolto ogni applicazione con `domain.net`.
<br></br><img src="images/cs_ingress_single_ns.png" width="300" alt="È necessaria almeno una risorsa per spazio dei nomi" style="width:300px; border-style: none"/>
</dd>
<dt>Le applicazioni sono in più spazi dei nomi</dt>
<dd>Se le applicazioni nel tuo cluster sono in spazi dei nomi differenti, devi creare almeno una risorsa per spazio dei nomi per definire le regole delle applicazioni qui esposte. Per registrare più risorse Ingress con l'ALB Ingress del cluster, devi utilizzare un dominio jolly. Quando viene registrato un dominio jolly come `*.mycluster.us-south.containers.appdomain.cloud`, tutti i domini multipli vengono risolti dallo stesso host. Quindi, puoi creare una risorsa Ingress in ogni spazio dei nomi e specificare un dominio secondario diverso in ogni risorsa Ingress.
<br><br>
Ad esempio, considera il seguente scenario:<ul>
<li>Hai due versioni della stessa applicazione, `app1` e `app3`, per scopi di test.</li>
<li>Distribuisci le applicazioni in due spazi dei nomi differenti all'interno dello stesso cluster: `app1` nello spazio dei nomi di sviluppo e `app3` in quello di preparazione.</li></ul>
Per utilizzare lo stesso ALB del cluster per gestire il traffico a queste applicazioni, crea i seguenti servizi e risorse:<ul>
<li>Un servizio Kubernetes nello spazio dei nomi di sviluppo per esporre `app1`.</li>
<li>Una risorsa Ingress nello spazio dei nomi di sviluppo che specifica l'host come `dev.mycluster.us-south.containers.appdomain.cloud`.</li>
<li>Un servizio Kubernetes nello spazio dei nomi di preparazione per esporre `app3`.</li>
<li>Una risorsa Ingress nello spazio dei nomi di preparazione che specifica l'host come `stage.mycluster.us-south.containers.appdomain.cloud`.</li></ul></br>
<img src="images/cs_ingress_multi_ns.png" alt="All'interno di uno spazio dei nomi, utilizza i domini secondari in una o più risorse" style="border-style: none"/>
Ora, entrambi gli URL risolvono lo stesso dominio e sono anche serviti dallo stesso ALB. Tuttavia, poiché la risorsa nello spazio dei nomi di preparazione viene registrata nel dominio secondario `stage`, l'ALB Ingress correttamente instrada le richieste dall'URL `stage.mycluster.us-south.containers.appdomain.cloud/app3` solo a `app3`.</dd>
</dl>

**Nota**:
* Il dominio secondario jolly Ingress fornito da IBM, `*.<cluster_name>.<region>.containers.appdomain.cloud`, viene registrato per impostazione predefinita per il tuo cluster. Tuttavia, TLS non è supportato per il dominio secondario jolly Ingress fornito da IBM.
* Se vuoi utilizzare un dominio personalizzato, devi registrarlo come un dominio jolly come ad esempio `*.custom_domain.net`. Per utilizzare TLS, devi ottenere un certificato jolly.

### Più domini all'interno di uno spazio dei nomi.

All'interno di uno spazio dei nomi individuale, puoi utilizzare un dominio per accedere a tutte le applicazioni nello spazio dei nomi. Se vuoi utilizzare domini differenti per le applicazioni all'interno di uno spazio dei nomi individuale, utilizza un dominio jolly. Quando viene registrato un dominio jolly come `*.mycluster.us-south.containers.appdomain.cloud`, tutti i domini multipli vengono risolti dallo stesso host. Quindi, puoi utilizzare una risorsa per specificare più host del dominio secondario in tale risorsa. In alternativa, puoi creare più risorse Ingress nello spazio dei nomi e specificare un dominio secondario diverso in ogni risorsa Ingress.

<img src="images/cs_ingress_single_ns_multi_subs.png" alt="È necessaria almeno una risorsa per spazio dei nomi." style="border-style: none"/>

**Nota**:
* Il dominio secondario jolly Ingress fornito da IBM, `*.<cluster_name>.<region>.containers.appdomain.cloud`, viene registrato per impostazione predefinita per il tuo cluster. Tuttavia, TLS non è supportato per il dominio secondario jolly Ingress fornito da IBM.
* Se vuoi utilizzare un dominio personalizzato, devi registrarlo come un dominio jolly come ad esempio `*.custom_domain.net`. Per utilizzare TLS, devi ottenere un certificato jolly.

<br />


## Esposizione delle applicazioni all'interno del tuo cluster al pubblico
{: #ingress_expose_public}

Esponi le applicazioni all'interno del tuo cluster al pubblico utilizzando l'ALB Ingress pubblico.
{:shortdesc}

Prima di iniziare:

-   Esamina i [prerequisiti](#config_prereqs) Ingress.
-   Se non ne hai già uno, [crea un cluster standard](cs_clusters.html#clusters_ui).
-   [Indirizza la tua CLI](cs_cli_install.html#cs_cli_configure) al tuo cluster per eseguire i comandi `kubectl`.

### Passo 1: Distribuisci le applicazioni e crea i servizi dell'applicazione.
{: #public_inside_1}

Inizia distribuendo le tue applicazioni e creando i servizi Kubernetes per esporle.
{: shortdesc}

1.  [Distribuisci la tua applicazione al cluster](cs_app.html#app_cli). Assicurati di aggiungere un'etichetta alla tua distribuzione nella sezione dei metadati del tuo file di configurazione, ad esempio `app: code`. Questa etichetta è necessaria per identificare tutti i pod in cui è in esecuzione la tua applicazione in modo che possano essere inclusi nel bilanciamento del carico Ingress.

2.   Crea un servizio Kubernetes per ogni applicazione che desideri esporre. La tua applicazione deve essere esposta da un servizio Kubernetes per poter essere inclusa dall'ALB cluster nel bilanciamento del carico Ingress.
      1.  Apri il tuo editor preferito e crea un file di configurazione del servizio denominato, ad esempio, `myapp_service.yaml`.
      2.  Definisci un servizio per l'applicazione che l'ALB esporrà.

          ```
          apiVersion: v1
          kind: Service
          metadata:
            name: myapp_service
          spec:
            selector:
              <selector_key>: <selector_value>
            ports:
             - protocol: TCP
               port: 8080
          ```
          {: codeblock}

          <table>
          <thead>
          <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del file del servizio ALB</th>
          </thead>
          <tbody>
          <tr>
          <td><code>selector</code></td>
          <td>Immetti la coppia di chiave (<em>&lt;selector_key&gt;</em>) e valore (<em>&lt;selector_value&gt;</em>) dell'etichetta che vuoi utilizzare per indirizzare i pod in cui viene eseguita la tua applicazione. Per indirizzare i tuoi pod e includerli nel bilanciamento del carico di servizio, assicurati che i valori di <em>&lt;selector_key&gt;</em> e <em>&lt;selector_value&gt;</em> siano gli stessi della coppia chiave/valore nella sezione <code>spec.template.metadata.labels</code> del tuo yaml di distribuzione.</td>
           </tr>
           <tr>
           <td><code> port</code></td>
           <td>La porta su cui è in ascolto il servizio.</td>
           </tr>
           </tbody></table>
      3.  Salva le modifiche.
      4.  Crea il servizio nel tuo cluster. Se le applicazioni vengono distribuite in più spazi dei nomi nel tuo cluster, assicurati che il servizio venga distribuito nello stesso spazio dei nomi dell'applicazione che vuoi esporre.

          ```
          kubectl apply -f myapp_service.yaml [-n <namespace>]
          ```
          {: pre}
      5.  Ripeti questi passi per ogni applicazione che vuoi esporre.


### Passo 2: Seleziona un dominio dell'applicazione e la terminazione TLS.
{: #public_inside_2}

Quando configuri l'ALB pubblico, scegli il dominio tramite il quale saranno accessibili le tue applicazioni e se utilizzare la terminazione TLS.
{: shortdesc}

<dl>
<dt>Dominio</dt>
<dd>Puoi utilizzare il dominio fornito da IBM, come <code>mycluster-12345.us-south.containers.appdomain.cloud/myapp</code>, per accedere alla tua applicazione da internet. Per utilizzare invece un dominio personalizzato, puoi associarlo al dominio fornito da IBM o all'indirizzo IP pubblico di ALB.</dd>
<dt>Terminazione TLS</dt>
<dd>Il carico ALB bilancia il traffico di rete HTTP alle applicazioni nel tuo cluster. Per bilanciare anche il traffico delle connessioni HTTPS in entrata, puoi configurare l'ALB per decodificare il traffico di rete e inoltrare la richiesta decodificata alle applicazioni esposte nel tuo cluster. Se stai utilizzando il dominio secondario fornito da IBM, puoi utilizzare il certificato TLS fornito da IBM. TLS non è al momento supportato per i domini secondari jolly forniti da IBM. Se stai utilizzando un dominio personalizzato, puoi utilizzare il tuo certificato TLS per gestire la terminazione TLS.</dd>
</dl>

Per utilizzare il dominio Ingress fornito da IBM:
1. Ottieni i dettagli del tuo cluster. Sostituisci _&lt;cluster_name_or_ID&gt;_ con il nome del cluster in cui sono distribuite le applicazioni che vuoi esporre.

    ```
    bx cs cluster-get <cluster_name_or_ID>
    ```
    {: pre}

    Output di esempio:

    ```
    Name:                   mycluster
    ID:                     18a61a63c6a94b658596ca93d087aad9
    State:                  normal
    Created:                2018-01-12T18:33:35+0000
    Location:               dal10
    Master URL:             https://169.xx.xxx.xxx:26268
    Ingress Subdomain:      mycluster-12345.us-south.containers.appdomain.cloud
    Ingress Secret:         <tls_secret>
    Workers:                3
    Version:                1.9.7
    Owner Email:            owner@email.com
    Monitoring Dashboard:   <dashboard_URL>
    ```
    {: screen}
2. Ottieni il dominio fornito da IBM nel campo **Dominio secondario Ingress**. Se vuoi utilizzare TLS, ottieni anche il segreto TLS fornito da IBM nel campo **Segreto Ingress**.
    **Nota**: se stai utilizzando un dominio secondario jolly, TLS non è supportato.

Per utilizzare un dominio personalizzato:
1.    Crea un dominio personalizzato. Per registrare il tuo dominio personalizzato, utilizza il provider DNS (Domain Name Service) o il DNS [{{site.data.keyword.Bluemix_notm}}](/docs/infrastructure/dns/getting-started.html#getting-started-with-dns).
      * Se le applicazioni che vuoi che Ingress esponga in spazi dei nomi differenti in un cluster, registra il dominio personalizzato come un dominio jolly, ad esempio `*.custom_domain.net`.

2.  Configura il tuo dominio per instradare il traffico di rete in entrata all'ALB fornito da IBM. Scegli tra queste opzioni:
    -   Definisci un alias per il tuo dominio personalizzato specificando il dominio fornito da IBM come un record di nome canonico (CNAME). Per trovare il dominio Ingress fornito da IBM, esegui `bx cs cluster-get <cluster_name>` e cerca il campo **Dominio secondario Ingress**.
    -   Associa il tuo dominio personalizzato all'indirizzo IP pubblico portatile dell'ALB fornito da IBM aggiungendo l'indirizzo IP come record. Per trovare l'indirizzo IP pubblico portatile dell'ALB, esegui `bx cs alb-get <public_alb_ID>`.
3.   Facoltativo: se vuoi utilizzare TLS, importa o crea un certificato TLS e il segreto della chiave. Se stai utilizzando un dominio jolly, accertarti di aver importato o creato un certificato jolly.
      * Se in {{site.data.keyword.cloudcerts_long_notm}} è memorizzato un certificato TLS che vuoi utilizzare, puoi importare il suo segreto associato nel tuo cluster immettendo il seguente comando:
        ```
        bx cs alb-cert-deploy --secret-name <secret_name> --cluster <cluster_name_or_ID> --cert-crn <certificate_crn>
        ```
        {: pre}
      * Se non hai un certificato TLS pronto, segui questa procedura:
        1. Crea un certificato e una chiave TLS per il tuo dominio codificati in formato PEM.
        2. Crea un segreto che utilizza il certificato e la chiave TLS. Sostituisci <em>&lt;tls_secret_name&gt;</em> con un nome per il tuo segreto Kubernetes, <em>&lt;tls_key_filepath&gt;</em> con il percorso al tuo file della chiave TLS personalizzato e <em>&lt;tls_cert_filepath&gt;</em> con il percorso al tuo file del certificato TLS personalizzato.
          ```
          kubectl create secret tls <tls_secret_name> --key <tls_key_filepath> --cert <tls_cert_filepath>
          ```
          {: pre}


### Passo 3: Crea la risorsa Ingress
{: #public_inside_3}

Le risorse Ingress definiscono le regole di instradamento che l'ALB utilizza per instradare il traffico al tuo servizio dell'applicazione.
{: shortdesc}

**Nota:** se il tuo cluster ha più spazi dei nomi in cui sono esposte le applicazioni, è necessaria almeno una risorsa Ingress per spazio dei nomi. Tuttavia, ogni spazio dei nomi deve utilizzare un host diverso. Devi registrare un dominio jolly e specificare un dominio secondario differente in ogni risorsa. Per ulteriori informazioni, vedi [Pianificazione della rete per spazi dei nomi singoli o multipli](#multiple_namespaces).

1. Apri il tuo editor preferito e crea un file di configurazione Ingress denominato, ad esempio, `myingressresource.yaml`.

2. Definisci una risorsa Ingress nel tuo file di configurazione che utilizza il dominio fornito da IBM o il tuo dominio personalizzato per instradare il traffico di rete in entrata ai servizi che hai creato in precedenza.

    YAML di esempio che non utilizza TLS:
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingressresource
    spec:
      rules:
      - host: <domain>
        http:
          paths:
          - path: /<app1_path>
            backend:
              serviceName: <app1_service>
              servicePort: 80
          - path: /<app2_path>
            backend:
              serviceName: <app2_service>
              servicePort: 80
    ```
    {: codeblock}

    YAML di esempio che utilizza TLS:
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingressresource
    spec:
      tls:
      - hosts:
        - <domain>
        secretName: <tls_secret_name>
      rules:
      - host: <domain>
        http:
          paths:
          - path: /<app1_path>
            backend:
              serviceName: <app1_service>
              servicePort: 80
          - path: /<app2_path>
            backend:
              serviceName: <app2_service>
              servicePort: 80
    ```
    {: codeblock}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del file YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code>tls/hosts</code></td>
    <td>Per utilizzare TLS, sostituisci <em>&lt;domain&gt;</em> con il dominio secondario Ingress fornito da IBM o con il tuo dominio personalizzato.

    </br></br>
    <strong>Nota:</strong><ul><li>Se le tue applicazioni sono esposte dai servizi in spazi dei nomi differenti in un cluster, aggiungi un dominio secondario jolly all'inizio del dominio, ad esempio `subdomain1.custom_domain.net` o `subdomain1.mycluster.us-south.containers.appdomain.cloud`. Utilizza un dominio secondario univoco per ogni risorsa che crei nel cluster.</li><li>Non utilizzare &ast; per il tuo host o lascia vuota la proprietà host per evitare errori durante la creazione di Ingress.</li></ul></td>
    </tr>
    <tr>
    <td><code>tls/secretName</code></td>
    <td><ul><li>Se stai utilizzando il dominio fornito da IBM, sostituisci <em>&lt;tls_secret_name&gt;</em> con il nome del segreto Ingress fornito da IBM.</li><li>Se stai utilizzando un dominio personalizzato, sostituisci <em>&lt;tls_secret_name&gt;</em> segreto che hai creato in precedenza che contiene il tuo certificato e la tua chiave TLS personalizzati. Se hai importato un certificato da {{site.data.keyword.cloudcerts_short}}, puoi eseguire <code>bx cs alb-cert-get --cluster <cluster_name_or_ID> --cert-crn <certificate_crn></code> per visualizzare i segreti associati a un certificato TLS.</li><ul><td>
    </tr>
    <tr>
    <td><code>host</code></td>
    <td>Sostituisci <em>&lt;domain&gt;</em> con il dominio secondario Ingress fornito da IBM o con il tuo dominio personalizzato.

    </br></br>
    <strong>Nota:</strong><ul><li>Se le tue applicazioni sono esposte dai servizi in spazi dei nomi differenti in un cluster, aggiungi un dominio secondario jolly all'inizio del dominio, ad esempio `subdomain1.custom_domain.net` o `subdomain1.mycluster.us-south.containers.appdomain.cloud`. Utilizza un dominio secondario univoco per ogni risorsa che crei nel cluster.</li><li>Non utilizzare &ast; per il tuo host o lascia vuota la proprietà host per evitare errori durante la creazione di Ingress.</li></ul></td>
    </tr>
    <tr>
    <td><code>path</code></td>
    <td>Sostituisci <em>&lt;app_path&gt;</em> con una barra o il percorso su cui è in ascolto la tua applicazione. Il percorso viene aggiunto al dominio personalizzato o fornito da IBM per creare una rotta univoca alla tua applicazione. Quando immetti questa rotta in un browser web, il traffico di rete viene instradato all'ALB. L'ALB cerca il servizio associato e invia il traffico di rete al servizio. Il servizio inoltra quindi il traffico ai pod su cui è in esecuzione l'applicazione.
    </br></br>
    Molte applicazioni non sono in ascolto su uno specifico percorso, ma utilizzano il percorso root e una porta specificata. In questo caso, definisci il percorso root come <code>/</code> e non specificare un percorso individuale per la tua applicazione. Esempi: <ul><li>Per <code>http://domain/</code>, immetti <code>/</code> come percorso.</li><li>Per <code>http://domain/app1_path</code>, immetti <code>/app1_path</code> come percorso.</li></ul>
    </br>
    <strong>Suggerimento:</strong> per configurare Ingress affinché sia in ascolto su un percorso diverso da quello su cui è in ascolto la tua applicazione, puoi utilizzare l'[annotazione di riscrittura](cs_annotations.html#rewrite-path).</td>
    </tr>
    <tr>
    <td><code>serviceName</code></td>
    <td>Sostituisci <em>&lt;app1_service&gt;</em> e <em>&lt;app2_service&gt;</em> e così via, con il nome dei servizi che hai creato per esporre le tue applicazioni. Se le tue applicazioni sono esposte dai servizi in spazi dei nomi differenti nel cluster, includi solo i servizi dell'applicazione presenti nello stesso spazio dei nomi. Devi creare una risorsa Ingress per ogni spazio dei nomi in cui hai delle applicazioni da esporre.</td>
    </tr>
    <tr>
    <td><code>servicePort</code></td>
    <td>La porta su cui è in ascolto il tuo servizio. Utilizza la stessa porta che hai definito quando hai creato il servizio Kubernetes per la tua applicazione.</td>
    </tr>
    </tbody></table>

3.  Crea la risorsa Ingress per il tuo cluster. Assicurati che la risorsa venga distribuita nello stesso spazio dei nomi dei servizi dell'applicazione che hai specificato nella risorsa.

    ```
    kubectl apply -f myingressresource.yaml -n <namespace>
    ```
    {: pre}
4.   Verifica che la risorsa Ingress sia stata creata correttamente.

      ```
      kubectl describe ingress myingressresource
      ```
      {: pre}

      1. Se i messaggi negli eventi descrivono un errore nella configurazione della tua risorsa, modifica i valori nel file di risorsa e riapplica il file per la risorsa.


La tua risorsa Ingress viene creata nello stesso spazio dei nomi dei tuoi servizi dell'applicazione. Le tue applicazioni in questo spazio dei nomi vengono registrate con l'ALB Ingress del cluster.

### Passo 4: Accedi alla tua applicazione da internet
{: #public_inside_4}

In un browser web, immetti l'URL del servizio dell'applicazione a cui accedere.

```
https://<domain>/<app1_path>
```
{: pre}

Se hai esposto più applicazioni, accedi a queste applicazioni modificando il percorso accodato all'URL.

```
https://<domain>/<app2_path>
```
{: pre}

Se utilizzi un dominio jolly per esporre le applicazioni in spazi dei nomi differenti, accedi a queste applicazioni con i rispettivi domini secondari.

```
http://<subdomain1>.<domain>/<app1_path>
```
{: pre}

```
http://<subdomain2>.<domain>/<app1_path>
```
{: pre}


<br />


## Esposizione delle applicazioni all'esterno del tuo cluster al pubblico 
{: #external_endpoint}

Esponi le applicazioni all'esterno del tuo cluster al pubblico includendole nel programma di bilanciamento del carico ALB Ingress pubblico. Le richieste pubbliche in entrata sul dominio fornito da IBM o sul tuo dominio personalizzato vengono inoltrate automaticamente all'applicazione esterna.
{:shortdesc}

Prima di iniziare:

-   Esamina i [prerequisiti](#config_prereqs) Ingress.
-   Se non ne hai già uno, [crea un cluster standard](cs_clusters.html#clusters_ui).
-   [Indirizza la tua CLI](cs_cli_install.html#cs_cli_configure) al tuo cluster per eseguire i comandi `kubectl`.
-   Assicurati che l'applicazione esterna che desideri includere nel bilanciamento del carico del cluster sia accessibile mediante un indirizzo IP pubblico.

### Passo 1: Crea un servizio dell'applicazione e un endpoint esterno
{: #public_outside_1}

Inizia creando un servizio Kubernetes per esporre l'applicazione esterna e configurando un endpoint esterno Kubernetes per l'applicazione.
{: shortdesc}

1.  Crea un servizio Kubernetes per il tuo cluster che inoltrerà le richieste in entrata a un endpoint esterno che avrai già creato. 
    1.  Apri il tuo editor preferito e crea un file di configurazione del servizio denominato, ad esempio, `myexternalservice.yaml`.
    2.  Definisci un servizio per l'applicazione che l'ALB esporrà.

        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: myexternalservice
        spec:
          ports:
           - protocol: TCP
               port: 8080
        ```
        {: codeblock}

        <table>
        <caption>Descrizione dei componenti del file del servizio ALB</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del file YAML</th>
        </thead>
        <tbody>
        <tr>
        <td><code>metadata/name</code></td>
        <td>Sostituisci <em>&lt;myexternalservice&gt;</em> con un nome per il tuo servizio.<p>Ulteriori informazioni sulla [protezione delle tue informazioni personali](cs_secure.html#pi) quando utilizzi le risorse Kubernetes.</p></td>
        </tr>
        <tr>
        <td><code> port</code></td>
        <td>La porta su cui è in ascolto il servizio.</td>
        </tr></tbody></table>

    3.  Salva le modifiche.
    4.  Crea il servizio Kubernetes per il tuo cluster.

        ```
        kubectl apply -f myexternalservice.yaml
        ```
        {: pre}
2.  Configura un endpoint Kubernetes che definisce la posizione esterna dell'applicazione che desideri includere nel bilanciamento del carico del cluster.
    1.  Apri il tuo editor preferito e crea un file di configurazione endpoint denominato, ad esempio, `myexternalendpoint.yaml`.
    2.  Definisci il tuo endpoint esterno. Includi tutti gli indirizzi IP pubblici e le porte che puoi utilizzare per accedere alla tua applicazione esterna.

        ```
        kind: Endpoints
        apiVersion: v1
        metadata:
          name: myexternalendpoint
        subsets:
          - addresses:
              - ip: <external_IP1>
              - ip: <external_IP2>
            ports:
              - port: <external_port>
        ```
        {: codeblock}

        <table>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del file YAML</th>
        </thead>
        <tbody>
        <tr>
        <td><code>name</code></td>
        <td>Sostituisci <em>&lt;myexternalendpoint&gt;</em> con il nome del servizio Kubernetes che hai creato in precedenza.</td>
        </tr>
        <tr>
        <td><code>ip</code></td>
        <td>Sostituisci <em>&lt;external_IP&gt;</em> con gli indirizzi IP pubblici per la connessione alla tua applicazione esterna.</td>
         </tr>
         <td><code> port</code></td>
         <td>Sostituisci <em>&lt;external_port&gt;</em> con la porta su cui è in ascolto la tua applicazione esterna.</td>
         </tbody></table>

    3.  Salva le modifiche.
    4.  Crea l'endpoint Kubernetes per il tuo cluster.

        ```
        kubectl apply -f myexternalendpoint.yaml
        ```
        {: pre}

### Passo 2: Seleziona un dominio dell'applicazione e la terminazione TLS.
{: #public_outside_2}

Quando configuri l'ALB pubblico, scegli il dominio tramite il quale saranno accessibili le tue applicazioni e se utilizzare la terminazione TLS.
{: shortdesc}

<dl>
<dt>Dominio</dt>
<dd>Puoi utilizzare il dominio fornito da IBM, come <code>mycluster-12345.us-south.containers.appdomain.cloud/myapp</code>, per accedere alla tua applicazione da internet. Per utilizzare invece un dominio personalizzato, puoi associarlo al dominio fornito da IBM o all'indirizzo IP pubblico di ALB.</dd>
<dt>Terminazione TLS</dt>
<dd>Il carico ALB bilancia il traffico di rete HTTP alle applicazioni nel tuo cluster. Per bilanciare anche il traffico delle connessioni HTTPS in entrata, puoi configurare l'ALB per decodificare il traffico di rete e inoltrare la richiesta decodificata alle applicazioni esposte nel tuo cluster. Se stai utilizzando il dominio secondario fornito da IBM, puoi utilizzare il certificato TLS fornito da IBM. TLS non è al momento supportato per i domini secondari jolly forniti da IBM. Se stai utilizzando un dominio personalizzato, puoi utilizzare il tuo certificato TLS per gestire la terminazione TLS.</dd>
</dl>

Per utilizzare il dominio Ingress fornito da IBM:
1. Ottieni i dettagli del tuo cluster. Sostituisci _&lt;cluster_name_or_ID&gt;_ con il nome del cluster in cui sono distribuite le applicazioni che vuoi esporre.

    ```
    bx cs cluster-get <cluster_name_or_ID>
    ```
    {: pre}

    Output di esempio:

    ```
    Name:                   mycluster
    ID:                     18a61a63c6a94b658596ca93d087aad9
    State:                  normal
    Created:                2018-01-12T18:33:35+0000
    Location:               dal10
    Master URL:             https://169.xx.xxx.xxx:26268
    Ingress Subdomain:      mycluster-12345.us-south.containers.appdomain.cloud
    Ingress Secret:         <tls_secret>
    Workers:                3
    Version:                1.9.7
    Owner Email:            owner@email.com
    Monitoring Dashboard:   <dashboard_URL>
    ```
    {: screen}
2. Ottieni il dominio fornito da IBM nel campo **Dominio secondario Ingress**. Se vuoi utilizzare TLS, ottieni anche il segreto TLS fornito da IBM nel campo **Segreto Ingress**. **Nota**: se stai utilizzando un dominio secondario jolly, TLS non è supportato.

Per utilizzare un dominio personalizzato:
1.    Crea un dominio personalizzato. Per registrare il tuo dominio personalizzato, utilizza il provider DNS (Domain Name Service) o il DNS [{{site.data.keyword.Bluemix_notm}}](/docs/infrastructure/dns/getting-started.html#getting-started-with-dns).
      * Se le applicazioni che vuoi che Ingress esponga in spazi dei nomi differenti in un cluster, registra il dominio personalizzato come un dominio jolly, ad esempio `*.custom_domain.net`.

2.  Configura il tuo dominio per instradare il traffico di rete in entrata all'ALB fornito da IBM. Scegli tra queste opzioni:
    -   Definisci un alias per il tuo dominio personalizzato specificando il dominio fornito da IBM come un record di nome canonico (CNAME). Per trovare il dominio Ingress fornito da IBM, esegui `bx cs cluster-get <cluster_name>` e cerca il campo **Dominio secondario Ingress**.
    -   Associa il tuo dominio personalizzato all'indirizzo IP pubblico portatile dell'ALB fornito da IBM aggiungendo l'indirizzo IP come record. Per trovare l'indirizzo IP pubblico portatile dell'ALB, esegui `bx cs alb-get <public_alb_ID>`.
3.   Facoltativo: se vuoi utilizzare TLS, importa o crea un certificato TLS e il segreto della chiave. Se stai utilizzando un dominio jolly, accertarti di aver importato o creato un certificato jolly.
      * Se in {{site.data.keyword.cloudcerts_long_notm}} è memorizzato un certificato TLS che vuoi utilizzare, puoi importare il suo segreto associato nel tuo cluster immettendo il seguente comando:
        ```
        bx cs alb-cert-deploy --secret-name <secret_name> --cluster <cluster_name_or_ID> --cert-crn <certificate_crn>
        ```
        {: pre}
      * Se non hai un certificato TLS pronto, segui questa procedura:
        1. Crea un certificato e una chiave TLS per il tuo dominio codificati in formato PEM.
        2. Crea un segreto che utilizza il certificato e la chiave TLS. Sostituisci <em>&lt;tls_secret_name&gt;</em> con un nome per il tuo segreto Kubernetes, <em>&lt;tls_key_filepath&gt;</em> con il percorso al tuo file della chiave TLS personalizzato e <em>&lt;tls_cert_filepath&gt;</em> con il percorso al tuo file del certificato TLS personalizzato.
          ```
          kubectl create secret tls <tls_secret_name> --key <tls_key_filepath> --cert <tls_cert_filepath>
          ```
          {: pre}


### Passo 3: Crea la risorsa Ingress
{: #public_outside_3}

Le risorse Ingress definiscono le regole di instradamento che l'ALB utilizza per instradare il traffico al tuo servizio dell'applicazione.
{: shortdesc}

**Nota:** se stai esponendo più applicazioni esterne e i servizi che hai creato per le applicazioni nel [Passo 1](#public_outside_1) sono in spazi dei nomi differenti, è necessaria almeno una risorsa Ingress per spazio dei nomi. Tuttavia, ogni spazio dei nomi deve utilizzare un host diverso. Devi registrare un dominio jolly e specificare un dominio secondario differente in ogni risorsa. Per ulteriori informazioni, vedi [Pianificazione della rete per spazi dei nomi singoli o multipli](#multiple_namespaces).

1. Apri il tuo editor preferito e crea un file di configurazione Ingress denominato, ad esempio, `myexternalingress.yaml`.

2. Definisci una risorsa Ingress nel tuo file di configurazione che utilizza il dominio fornito da IBM o il tuo dominio personalizzato per instradare il traffico di rete in entrata ai servizi che hai creato in precedenza.

    YAML di esempio che non utilizza TLS:
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myexternalingress
    spec:
      rules:
      - host: <domain>
        http:
          paths:
          - path: /<external_app1_path>
            backend:
              serviceName: <app1_service>
              servicePort: 80
          - path: /<external_app2_path>
            backend:
              serviceName: <app2_service>
              servicePort: 80
    ```
    {: codeblock}

    YAML di esempio che utilizza TLS:
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myexternalingress
    spec:
      tls:
      - hosts:
        - <domain>
        secretName: <tls_secret_name>
      rules:
      - host: <domain>
        http:
          paths:
          - path: /<external_app1_path>
            backend:
              serviceName: <app1_service>
              servicePort: 80
          - path: /<external_app2_path>
            backend:
              serviceName: <app2_service>
              servicePort: 80
    ```
    {: codeblock}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del file YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code>tls/hosts</code></td>
    <td>Per utilizzare TLS, sostituisci <em>&lt;domain&gt;</em> con il dominio secondario Ingress fornito da IBM o con il tuo dominio personalizzato.

    </br></br>
    <strong>Nota:</strong><ul><li>Se i tuoi servizi dell'applicazione sono in spazi dei nomi differenti nel cluster, aggiungi un dominio secondario jolly all'inizio del dominio, ad esempio `subdomain1.custom_domain.net` o `subdomain1.mycluster.us-south.containers.appdomain.cloud`. Utilizza un dominio secondario univoco per ogni risorsa che crei nel cluster.</li><li>Non utilizzare &ast; per il tuo host o lascia vuota la proprietà host per evitare errori durante la creazione di Ingress.</li></ul></td>
    </tr>
    <tr>
    <td><code>tls/secretName</code></td>
    <td><ul><li>Se stai utilizzando il dominio fornito da IBM, sostituisci <em>&lt;tls_secret_name&gt;</em> con il nome del segreto Ingress fornito da IBM.</li><li>Se stai utilizzando un dominio personalizzato, sostituisci <em>&lt;tls_secret_name&gt;</em> segreto che hai creato in precedenza che contiene il tuo certificato e la tua chiave TLS personalizzati. Se hai importato un certificato da {{site.data.keyword.cloudcerts_short}}, puoi eseguire <code>bx cs alb-cert-get --cluster <cluster_name_or_ID> --cert-crn <certificate_crn></code> per visualizzare i segreti associati a un certificato TLS.</li><ul><td>
    </tr>
    <tr>
    <td><code>rules/host</code></td>
    <td>Sostituisci <em>&lt;domain&gt;</em> con il dominio secondario Ingress fornito da IBM o con il tuo dominio personalizzato.

    </br></br>
    <strong>Nota:</strong><ul><li>Se le tue applicazioni sono esposte dai servizi in spazi dei nomi differenti in un cluster, aggiungi un dominio secondario jolly all'inizio del dominio, ad esempio `subdomain1.custom_domain.net` o `subdomain1.mycluster.us-south.containers.appdomain.cloud`. Utilizza un dominio secondario univoco per ogni risorsa che crei nel cluster.</li><li>Non utilizzare &ast; per il tuo host o lascia vuota la proprietà host per evitare errori durante la creazione di Ingress.</li></ul></td>
    </tr>
    <tr>
    <td><code>path</code></td>
    <td>Sostituisci <em>&lt;external_app_path&gt;</em> con una barra o il percorso su cui è in ascolto la tua applicazione. Il percorso viene aggiunto al dominio personalizzato o fornito da IBM per creare una rotta univoca alla tua applicazione. Quando immetti questa rotta in un browser web, il traffico di rete viene instradato all'ALB. L'ALB cerca il servizio associato e invia il traffico di rete al servizio. Il servizio inoltra quindi il traffico all'applicazione esterna. Per poter ricevere il traffico di rete in entrata, l'applicazione deve essere configurata per l'ascolto su questo percorso.
    </br></br>
    Molte applicazioni non sono in ascolto su uno specifico percorso, ma utilizzano il percorso root e una porta specificata. In questo caso, definisci il percorso root come <code>/</code> e non specificare un percorso individuale per la tua applicazione. Esempi: <ul><li>Per <code>http://domain/</code>, immetti <code>/</code> come percorso.</li><li>Per <code>http://domain/app1_path</code>, immetti <code>/app1_path</code> come percorso.</li></ul>
    </br></br>
    <strong>Suggerimento:</strong> per configurare Ingress affinché sia in ascolto su un percorso diverso da quello su cui è in ascolto la tua applicazione, puoi utilizzare l'[annotazione di riscrittura](cs_annotations.html#rewrite-path).</td>
    </tr>
    <tr>
    <td><code>serviceName</code></td>
    <td>Sostituisci <em>&lt;app1_service&gt;</em> e <em>&lt;app2_service&gt;</em>, . con il nome dei servizi che hai creato per esporre le tue applicazioni esterne. Se le tue applicazioni sono esposte dai servizi in spazi dei nomi differenti nel cluster, includi solo i servizi dell'applicazione presenti nello stesso spazio dei nomi. Devi creare una risorsa Ingress per ogni spazio dei nomi in cui hai delle applicazioni da esporre.</td>
    </tr>
    <tr>
    <td><code>servicePort</code></td>
    <td>La porta su cui è in ascolto il tuo servizio. Utilizza la stessa porta che hai definito quando hai creato il servizio Kubernetes per la tua applicazione.</td>
    </tr>
    </tbody></table>

3.  Crea la risorsa Ingress per il tuo cluster. Assicurati che la risorsa venga distribuita nello stesso spazio dei nomi dei servizi dell'applicazione che hai specificato nella risorsa.

    ```
    kubectl apply -f myexternalingress.yaml -n <namespace>
    ```
    {: pre}
4.   Verifica che la risorsa Ingress sia stata creata correttamente.

      ```
      kubectl describe ingress myingressresource
      ```
      {: pre}

      1. Se i messaggi negli eventi descrivono un errore nella configurazione della tua risorsa, modifica i valori nel file di risorsa e riapplica il file per la risorsa.


La tua risorsa Ingress viene creata nello stesso spazio dei nomi dei tuoi servizi dell'applicazione. Le tue applicazioni in questo spazio dei nomi vengono registrate con l'ALB Ingress del cluster.

### Passo 4: Accedi alla tua applicazione da internet
{: #public_outside_4}

In un browser web, immetti l'URL del servizio dell'applicazione a cui accedere.

```
https://<domain>/<app1_path>
```
{: pre}

Se hai esposto più applicazioni, accedi a queste applicazioni modificando il percorso accodato all'URL.

```
https://<domain>/<app2_path>
```
{: pre}

Se utilizzi un dominio jolly per esporre le applicazioni in spazi dei nomi differenti, accedi a queste applicazioni con i rispettivi domini secondari.

```
http://<subdomain1>.<domain>/<app1_path>
```
{: pre}

```
http://<subdomain2>.<domain>/<app1_path>
```
{: pre}


<br />


## Abilitazione dell'ALB privato predefinito 
{: #private_ingress}

Quando crei un cluster standard, viene creato un ALB (programma di bilanciamento del carico dell'applicazione) privato fornito da IBM a cui vengono assegnati un indirizzo IP privato portatile e una rotta privata. Tuttavia, l'ALB privato predefinito non viene abilitato automaticamente. Per utilizzare l'ALB privato per bilanciare il carico del traffico di rete privato alle tue applicazioni, devi prima abilitarlo con l'indirizzo IP privato portatile fornito da IBM o con il tuo proprio indirizzo IP privato portatile.
{:shortdesc}

**Nota**: se quando hai creato il cluster hai utilizzato l'indicatore `--no-subnet`, devi aggiungere una sottorete privata portatile o una sottorete gestita dall'utente prima di poter abilitare l'ALB privato. Per ulteriori informazioni, vedi [Richiesta di più sottoreti per il tuo cluster](cs_subnets.html#request).

Prima di iniziare:

-   Se non ne hai già uno, [crea un cluster standard](cs_clusters.html#clusters_ui).
-   [Indirizza la tua CLI](cs_cli_install.html#cs_cli_configure) al tuo cluster.

Per abilitare l'ALB privato utilizzando l'indirizzo IP privato portatile pre-assegnato fornito da IBM:

1. Elenca gli ALB disponibili nel tuo cluster per ottenere l'ID dell'ALB privato. Sostituisci <em>&lt;cluser_name&gt;</em> con il nome del cluster in cui è distribuita l'applicazione che vuoi esporre.

    ```
    bx cs albs --cluster <cluser_name>
    ```
    {: pre}

    Il campo **Status** per l'ALB privato è _disabled_.
    ```
    ALB ID                                            Enabled   Status     Type      ALB IP
    private-cr6d779503319d419ea3b4ab171d12c3b8-alb1   false     disabled   private   -
    public-cr6d779503319d419ea3b4ab171d12c3b8-alb1    true      enabled    public    169.xx.xxx.xxx
    ```
    {: screen}

2. Abilita l'ALB privato. Sostituisci <em>&lt;private_ALB_ID&gt;</em> con l'ID dell'ALB privato indicato nell'output nel passo precedente.

   ```
   bx cs alb-configure --albID <private_ALB_ID> --enable
   ```
   {: pre}

<br>
Per abilitare l'ALB privato utilizzando il tuo proprio indirizzo IP privato portatile: 

1. Configura la sottorete gestita dall'utente dell'indirizzo IP che hai scelto per instradare il traffico sulla VLAN privata del tuo cluster.

   ```
   bx cs cluster-user-subnet-add <cluster_name> <subnet_CIDR> <private_VLAN_ID>
   ```
   {: pre}

   <table>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del comando</th>
   </thead>
   <tbody>
   <tr>
   <td><code>&lt;cluser_name&gt;</code></td>
   <td>Il nome o l'ID del cluster in cui l'applicazione che vuoi esporre viene distribuita. </td>
   </tr>
   <tr>
   <td><code>&lt;subnet_CIDR&gt;</code></td>
   <td>Il CIDR della tua sottorete gestita dall'utente.</td>
   </tr>
   <tr>
   <td><code>&lt;private_VLAN_ID&gt;</code></td>
   <td>Un ID della VLAN privata disponibile. Puoi trovare l'ID di una VLAN privata disponibile eseguendo `bx cs vlans`.</td>
   </tr>
   </tbody></table>

2. Elenca gli ALB disponibili nel tuo cluster per ottenere l'ID dell'ALB privato.

    ```
    bx cs albs --cluster <cluster_name>
    ```
    {: pre}

    Il campo **Status** per l'ALB privato è _disabled_.
    ```
    ALB ID                                            Enabled   Status     Type      ALB IP
    private-cr6d779503319d419ea3b4ab171d12c3b8-alb1   false     disabled   private   -
    public-cr6d779503319d419ea3b4ab171d12c3b8-alb1    true      enabled    public    169.xx.xxx.xxx
    ```
    {: screen}

3. Abilita l'ALB privato. Sostituisci <em>&lt;private_ALB_ID&gt;</em> con l'ID dell'ALB privato indicato nell'output nel passo precedente e <em>&lt;user_IP&gt;</em> con l'indirizzo IP dalla sottorete gestita dall'utente che vuoi utilizzare.

   ```
   bx cs alb-configure --albID <private_ALB_ID> --enable --user-ip <user_IP>
   ```
   {: pre}

<br />


## Esposizione delle applicazioni su una rete privata
{: #ingress_expose_private}

Esponi le applicazioni a una rete privata utilizzando l'ALB Ingress privato.
{:shortdesc}

Prima di iniziare:
* Esamina i [prerequisiti](#config_prereqs) Ingress.
* [Abilita il programma di bilanciamento del carico dell'applicazione privato](#private_ingress).
* Se hai dei nodi di lavoro privati e desideri utilizzare un provider DNS esterno, devi [configurare i nodi edge con l'accesso pubblico](cs_edge.html#edge) e [configurare un VRA (Virtual Router Appliance) ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/blogs/bluemix/2017/07/kubernetes-and-bluemix-container-based-workloads-part4/).
* Se hai nodi di lavoro privati e desideri rimanere solo su una rete privata, devi [configurare un servizio DNS in loco e privato ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/) per risolvere le richieste URL alle tue applicazioni.

### Passo 1: Distribuisci le applicazioni e crea i servizi dell'applicazione.
{: #private_1}

Inizia distribuendo le tue applicazioni e creando i servizi Kubernetes per esporle.
{: shortdesc}

1.  [Distribuisci la tua applicazione al cluster](cs_app.html#app_cli). Assicurati di aggiungere un'etichetta alla tua distribuzione nella sezione dei metadati del tuo file di configurazione, ad esempio `app: code`. Questa etichetta è necessaria per identificare tutti i pod in cui è in esecuzione la tua applicazione in modo che possano essere inclusi nel bilanciamento del carico Ingress.

2.   Crea un servizio Kubernetes per ogni applicazione che desideri esporre. La tua applicazione deve essere esposta da un servizio Kubernetes per poter essere inclusa dall'ALB cluster nel bilanciamento del carico Ingress.
      1.  Apri il tuo editor preferito e crea un file di configurazione del servizio denominato, ad esempio, `myapp_service.yaml`.
      2.  Definisci un servizio per l'applicazione che l'ALB esporrà.

          ```
          apiVersion: v1
          kind: Service
          metadata:
            name: myapp_service
          spec:
            selector:
              <selector_key>: <selector_value>
            ports:
             - protocol: TCP
               port: 8080
          ```
          {: codeblock}

          <table>
          <thead>
          <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del file del servizio ALB</th>
          </thead>
          <tbody>
          <tr>
          <td><code>selector</code></td>
          <td>Immetti la coppia di chiave (<em>&lt;selector_key&gt;</em>) e valore (<em>&lt;selector_value&gt;</em>) dell'etichetta che vuoi utilizzare per indirizzare i pod in cui viene eseguita la tua applicazione. Per indirizzare i tuoi pod e includerli nel bilanciamento del carico di servizio, assicurati che i valori di <em>&lt;selector_key&gt;</em> e <em>&lt;selector_value&gt;</em> siano gli stessi della coppia chiave/valore nella sezione <code>spec.template.metadata.labels</code> del tuo yaml di distribuzione.</td>
           </tr>
           <tr>
           <td><code> port</code></td>
           <td>La porta su cui è in ascolto il servizio.</td>
           </tr>
           </tbody></table>
      3.  Salva le modifiche.
      4.  Crea il servizio nel tuo cluster. Se le applicazioni vengono distribuite in più spazi dei nomi nel tuo cluster, assicurati che il servizio venga distribuito nello stesso spazio dei nomi dell'applicazione che vuoi esporre.

          ```
          kubectl apply -f myapp_service.yaml [-n <namespace>]
          ```
          {: pre}
      5.  Ripeti questi passi per ogni applicazione che vuoi esporre.


### Passo 2: Associa il tuo dominio personalizzato e seleziona la terminazione TLS
{: #private_2}

Quando configuri l'ALB privato, utilizzi il dominio personalizzato tramite il quale saranno accessibili le tue applicazioni e scegli se utilizzare la terminazione TLS.
{: shortdesc}

Il carico ALB bilancia il traffico di rete HTTP alle tue applicazioni. Per bilanciare anche il traffico delle connessioni HTTPS in entrata, puoi configurare l'ALB per utilizzare il tuo certificato TLS per decodificare il traffico di rete. L'ALB poi inoltra la richiesta decodificata alle applicazioni esposte nel tuo cluster.
1.   Crea un dominio personalizzato. Per registrare il tuo dominio personalizzato, utilizza il provider DNS (Domain Name Service) o il DNS [{{site.data.keyword.Bluemix_notm}}](/docs/infrastructure/dns/getting-started.html#getting-started-with-dns).
      * Se le applicazioni che vuoi che Ingress esponga in spazi dei nomi differenti in un cluster, registra il dominio personalizzato come un dominio jolly, ad esempio `*.custom_domain.net`.

2. Associa il tuo dominio personalizzato all'indirizzo IP privato portatile dell'ALB privato fornito da IBM aggiungendo l'indirizzo IP come record. Per trovare l'indirizzo IP privato portatile dell'ALB privato, esegui `bx cs albs --cluster <cluster_name>`.
3.   Facoltativo: se vuoi utilizzare TLS, importa o crea un certificato TLS e il segreto della chiave. Se stai utilizzando un dominio jolly, accertarti di aver importato o creato un certificato jolly.
      * Se in {{site.data.keyword.cloudcerts_long_notm}} è memorizzato un certificato TLS che vuoi utilizzare, puoi importare il suo segreto associato nel tuo cluster immettendo il seguente comando:
        ```
        bx cs alb-cert-deploy --secret-name <secret_name> --cluster <cluster_name_or_ID> --cert-crn <certificate_crn>
        ```
        {: pre}
      * Se non hai un certificato TLS pronto, segui questa procedura:
        1. Crea un certificato e una chiave TLS per il tuo dominio codificati in formato PEM.
        2. Crea un segreto che utilizza il certificato e la chiave TLS. Sostituisci <em>&lt;tls_secret_name&gt;</em> con un nome per il tuo segreto Kubernetes, <em>&lt;tls_key_filepath&gt;</em> con il percorso al tuo file della chiave TLS personalizzato e <em>&lt;tls_cert_filepath&gt;</em> con il percorso al tuo file del certificato TLS personalizzato.
          ```
          kubectl create secret tls <tls_secret_name> --key <tls_key_filepath> --cert <tls_cert_filepath>
          ```
          {: pre}


### Passo 3: Crea la risorsa Ingress
{: #pivate_3}

Le risorse Ingress definiscono le regole di instradamento che l'ALB utilizza per instradare il traffico al tuo servizio dell'applicazione.
{: shortdesc}

**Nota:** se il tuo cluster ha più spazi dei nomi in cui sono esposte le applicazioni, è necessaria almeno una risorsa Ingress per spazio dei nomi. Tuttavia, ogni spazio dei nomi deve utilizzare un host diverso. Devi registrare un dominio jolly e specificare un dominio secondario differente in ogni risorsa. Per ulteriori informazioni, vedi [Pianificazione della rete per spazi dei nomi singoli o multipli](#multiple_namespaces).

1. Apri il tuo editor preferito e crea un file di configurazione Ingress denominato, ad esempio, `myingressresource.yaml`.

2.  Definisci una risorsa Ingress nel tuo file di configurazione che utilizza il tuo dominio personalizzato per instradare il traffico di rete in entrata ai servizi che hai creato in precedenza. 

    YAML di esempio che non utilizza TLS:
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingressresource
      annotations:
        ingress.bluemix.net/ALB-ID: "<private_ALB_ID>"
    spec:
      rules:
      - host: <domain>
        http:
          paths:
          - path: /<app1_path>
            backend:
              serviceName: <app1_service>
              servicePort: 80
          - path: /<app2_path>
            backend:
              serviceName: <app2_service>
              servicePort: 80
    ```
    {: codeblock}

    YAML di esempio che utilizza TLS:
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingressresource
      annotations:
        ingress.bluemix.net/ALB-ID: "<private_ALB_ID>"
    spec:
      tls:
      - hosts:
        - <domain>
        secretName: <tls_secret_name>
      rules:
      - host: <domain>
        http:
          paths:
          - path: /<app1_path>
            backend:
              serviceName: <app1_service>
              servicePort: 80
          - path: /<app2_path>
            backend:
              serviceName: <app2_service>
              servicePort: 80
    ```
    {: codeblock}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti del file YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code>ingress.bluemix.net/ALB-ID</code></td>
    <td>Sostituisci <em>&lt;private_ALB_ID&gt;</em> con l'ID del tuo ALB privato. Per trovare l'ID ALB, esegui <code>bx cs albs --cluster <my_cluster></code>. Per ulteriori informazioni su questa annotazione Ingress, consulta [Instradamento del programma di bilanciamento del carico dell'applicazione privato](cs_annotations.html#alb-id).</td>
    </tr>
    <tr>
    <td><code>tls/hosts</code></td>
    <td>Per utilizzare TLS, sostituisci <em>&lt;domain&gt;</em> con il tuo dominio personalizzato.

    </br></br>
    <strong>Nota:</strong><ul><li>Se le tue applicazioni sono esposte dai servizi in spazi dei nomi differenti in un cluster, aggiungi un dominio secondario jolly all'inizio del dominio, ad esempio `subdomain1.custom_domain.net`. Utilizza un dominio secondario univoco per ogni risorsa che crei nel cluster.</li><li>Non utilizzare &ast; per il tuo host o lascia vuota la proprietà host per evitare errori durante la creazione di Ingress.</li></ul></td>
    </tr>
    <tr>
    <td><code>tls/secretName</code></td>
    <td>Sostituisci <em>&lt;tls_secret_name&gt;</em> con il nome del segreto che hai creato in precedenza e che contiene la tua chiave e il tuo certificato TLS personalizzati. Se hai importato un certificato da {{site.data.keyword.cloudcerts_short}}, puoi eseguire <code>bx cs alb-cert-get --cluster <cluster_name_or_ID> --cert-crn <certificate_crn></code> per visualizzare i segreti associati a un certificato TLS.
    </tr>
    <tr>
    <td><code>host</code></td>
    <td>Sostituisci <em>&lt;domain&gt;</em> con il tuo dominio personalizzato.
    </br></br>
    <strong>Nota:</strong><ul><li>Se le tue applicazioni sono esposte dai servizi in spazi dei nomi differenti in un cluster, aggiungi un dominio secondario jolly all'inizio del dominio, ad esempio `subdomain1.custom_domain.net`. Utilizza un dominio secondario univoco per ogni risorsa che crei nel cluster.</li><li>Non utilizzare &ast; per il tuo host o lascia vuota la proprietà host per evitare errori durante la creazione di Ingress.</li></ul></td>
    </td>
    </tr>
    <tr>
    <td><code>path</code></td>
    <td>Sostituisci <em>&lt;app_path&gt;</em> con una barra o il percorso su cui è in ascolto la tua applicazione. Il percorso viene aggiunto al dominio personalizzato per creare una rotta univoca alla tua applicazione. Quando immetti questa rotta in un browser web, il traffico di rete viene instradato all'ALB. L'ALB cerca il servizio associato e invia il traffico di rete al servizio. Il servizio inoltra quindi il traffico ai pod su cui è in esecuzione l'applicazione.
    </br></br>
    Molte applicazioni non sono in ascolto su uno specifico percorso, ma utilizzano il percorso root e una porta specificata. In questo caso, definisci il percorso root come <code>/</code> e non specificare un percorso individuale per la tua applicazione. Esempi: <ul><li>Per <code>http://domain/</code>, immetti <code>/</code> come percorso.</li><li>Per <code>http://domain/app1_path</code>, immetti <code>/app1_path</code> come percorso.</li></ul>
    </br>
    <strong>Suggerimento:</strong> per configurare Ingress affinché sia in ascolto su un percorso diverso da quello su cui è in ascolto la tua applicazione, puoi utilizzare l'[annotazione di riscrittura](cs_annotations.html#rewrite-path).</td>
    </tr>
    <tr>
    <td><code>serviceName</code></td>
    <td>Sostituisci <em>&lt;app1_service&gt;</em> e <em>&lt;app2_service&gt;</em> e così via, con il nome dei servizi che hai creato per esporre le tue applicazioni. Se le tue applicazioni sono esposte dai servizi in spazi dei nomi differenti nel cluster, includi solo i servizi dell'applicazione presenti nello stesso spazio dei nomi. Devi creare una risorsa Ingress per ogni spazio dei nomi in cui hai delle applicazioni da esporre.</td>
    </tr>
    <tr>
    <td><code>servicePort</code></td>
    <td>La porta su cui è in ascolto il tuo servizio. Utilizza la stessa porta che hai definito quando hai creato il servizio Kubernetes per la tua applicazione.</td>
    </tr>
    </tbody></table>

3.  Crea la risorsa Ingress per il tuo cluster. Assicurati che la risorsa venga distribuita nello stesso spazio dei nomi dei servizi dell'applicazione che hai specificato nella risorsa.

    ```
    kubectl apply -f myingressresource.yaml -n <namespace>
    ```
    {: pre}
4.   Verifica che la risorsa Ingress sia stata creata correttamente.

      ```
      kubectl describe ingress myingressresource
      ```
      {: pre}

      1. Se i messaggi negli eventi descrivono un errore nella configurazione della tua risorsa, modifica i valori nel file di risorsa e riapplica il file per la risorsa.


La tua risorsa Ingress viene creata nello stesso spazio dei nomi dei tuoi servizi dell'applicazione. Le tue applicazioni in questo spazio dei nomi vengono registrate con l'ALB Ingress del cluster.

### Passo 4: Accedi alla tua applicazione dalla tua rete privata 
{: #private_4}

Dall'interno del tuo firewall della rete privata, immetti l'URL del servizio dell'applicazione in un browser web.

```
https://<domain>/<app1_path>
```
{: pre}

Se hai esposto più applicazioni, accedi a queste applicazioni modificando il percorso accodato all'URL.

```
https://<domain>/<app2_path>
```
{: pre}

Se utilizzi un dominio jolly per esporre le applicazioni in spazi dei nomi differenti, accedi a queste applicazioni con i rispettivi domini secondari.

```
http://<subdomain1>.<domain>/<app1_path>
```
{: pre}

```
http://<subdomain2>.<domain>/<app1_path>
```
{: pre}


Per un'esercitazione completa su come proteggere le comunicazioni tra i microservizi nei tuoi cluster utilizzando l'ALB privato con TLS, consulta [questo post del blog ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://medium.com/ibm-cloud/secure-microservice-to-microservice-communication-across-kubernetes-clusters-using-a-private-ecbe2a8d4fe2).

<br />


## Configurazioni facoltative per i programmi di bilanciamento del carico dell'applicazione
{: #configure_alb}

Puoi ulteriormente configurare un programma di bilanciamento del carico dell'applicazione con le seguenti opzioni.

-   [Apertura di porte nel programma di bilanciamento del carico dell'applicazione Ingress](#opening_ingress_ports)
-   [Configurazione di protocolli SSL e cifrature SSL a livello di HTTP](#ssl_protocols_ciphers)
-   [Personalizzazione del formato di log Ingress](#ingress_log_format)
-   [Personalizzazione del tuo programma di bilanciamento del carico dell'applicazione con le annotazioni](cs_annotations.html)
{: #ingress_annotation}


### Apertura di porte nel programma di bilanciamento del carico dell'applicazione Ingress
{: #opening_ingress_ports}

Per impostazione predefinita, solo le porte 80 e 443 sono esposte nell'ALB Ingress. Per esporre altre porte, puoi modificare la risorsa della mappa di configurazione `ibm-cloud-provider-ingress-cm`.
{:shortdesc}

1. Crea e apri una versione locale del file di configurazione per la risorsa della mappa di configurazione `ibm-cloud-provider-ingress-cm`.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. Aggiungi una sezione <code>data</code> e specifica le porte pubbliche `80`, `443` e le altre porte che vuoi esporre separate da un punto e virgola (;).

    **Importante**: per impostazione predefinita, le porte 80 e 443 sono aperte. Se vuoi mantenere le porte 80 e 443 aperte, devi includerle in aggiunta a tutte le altre porte che specifichi nel campo `public-ports`. Qualsiasi porta che non sia specificata viene chiusa. Se hai abilitato un ALB privato, devi inoltre specificare tutte le porte che vuoi mantenere aperte nel campo `private-ports`. 

    ```
    apiVersion: v1
    data:
      public-ports: "80;443;<port3>"
      private-ports: "80;443;<port4>"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: codeblock}

    Esempio che mantiene le porte `80`, `443` e `9443` aperte:
    ```
    apiVersion: v1
 data:
   public-ports: "80;443;9443"
 kind: ConfigMap
 metadata:
   name: ibm-cloud-provider-ingress-cm
   namespace: kube-system
    ```
    {: screen}

3. Salva il file di configurazione.

4. Verifica che le modifiche alla mappa di configurazione siano state applicate.

 ```
 kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
 ```
 {: pre}

 Output:
 ```
 Name:        ibm-cloud-provider-ingress-cm
 Namespace:    kube-system
 Labels:        <none>
 Annotations:    <none>

 Data
 ====

  public-ports: "80;443;9443"
 ```
 {: screen}

Per ulteriori informazioni sulle risorse della mappa di configurazione, consulta la [documentazione di Kubernetes](https://kubernetes-v1-4.github.io/docs/user-guide/configmap/).

### Configurazione di protocolli SSL e cifrature SSL a livello di HTTP
{: #ssl_protocols_ciphers}

Abilita i protocolli e le cifrature SSL a livello globale HTTP modificando la mappa di configurazione `ibm-cloud-provider-ingress-cm`.
{:shortdesc}

Per impostazione predefinita, il protocollo 1.2 TLS viene utilizzato per tutte le configurazioni Ingress che utilizzano il dominio fornito da IBM. Puoi sovrascrivere il valore predefinito invece di utilizzare i protocolli 1.1 o 1.0 TLS seguendo questa procedura.

**Nota**: quando specifichi i protocolli abilitati per tutti gli host, i parametri TLSv1.1 e TLSv1.2 (1.1.13, 1.0.12) funzionano solo quando viene utilizzato OpenSSL 1.0.1 o superiore. Il parametro TLSv1.3 (1.13.0) funziona solo quando viene utilizzato OpenSSL 1.1.1 sviluppato con il supporto TLSv1.3.

Per modificare la mappa di configurazione per l'abilitazione di protocolli e cifrature SSL:

1. Crea e apri una versione locale del file di configurazione per la risorsa della mappa di configurazione `ibm-cloud-provider-ingress-cm`.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. Aggiungi i protocolli e le cifrature SSL. Formatta le cifrature in base al [formato dell'elenco di cifrature della libreria OpenSSL ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.openssl.org/docs/man1.0.2/apps/ciphers.html).

   ```
   apiVersion: v1
 data:
   ssl-protocols: "TLSv1 TLSv1.1 TLSv1.2"
   ssl-ciphers: "HIGH:!aNULL:!MD5"
 kind: ConfigMap
 metadata:
   name: ibm-cloud-provider-ingress-cm
   namespace: kube-system
   ```
   {: codeblock}

3. Salva il file di configurazione.

4. Verifica che le modifiche alla mappa di configurazione siano state applicate.

   ```
   kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
   ```
   {: pre}

   Output:
   ```
   Name:        ibm-cloud-provider-ingress-cm
 Namespace:    kube-system
 Labels:        <none>
 Annotations:    <none>

   Data
 ====

    ssl-protocols: "TLSv1 TLSv1.1 TLSv1.2"
  ssl-ciphers: "HIGH:!aNULL:!MD5"
   ```
   {: screen}

### Personalizzazione del formato e del contenuto del log Ingress
{: #ingress_log_format}

Puoi personalizzare il contenuto e il formato dei log raccolti per l'ALB Ingress.
{:shortdesc}

Per impostazione predefinita, il formato dei log Ingress è JSON e mostrano i campi log comuni. Tuttavia, puoi anche creare un formato di log personalizzato. Per scegliere quali componenti di log verranno inoltrati e come verranno organizzati nell'output del log: 

1. Crea e apri una versione locale del file di configurazione per la risorsa della mappa di configurazione `ibm-cloud-provider-ingress-cm`.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. Aggiungi una sezione <code>data</code>. Aggiungi il campo `log-format` e, facoltativamente, il campo `log-format-escape-json`. 

    ```
    apiVersion: v1
    data:
      log-format: '{<key1>: <log_variable1>, <key2>: <log_variable2>, <key3>: <log_variable3>}'
      log-format-escape-json: "true"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: pre}

    <table>
    <caption>Componenti del file YAML </caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione della configurazione del formato di log</th>
    </thead>
    <tbody>
    <tr>
    <td><code>log-format</code></td>
    <td>Sostituisci <code>&lt;key&gt;</code> con il nome del componente di log e <code>&lt;log_variable&gt;</code> con una variabile per il componente di cui desideri raccogliere le voci di log. Puoi includere il testo e la punteggiatura che desideri nella voce di log, ad esempio le virgolette nei valori stringa e le virgole per separare i componenti di log. Ad esempio, se formatti un componente utilizzando <code>request: "$request"</code>, generi quanto segue in una voce del log: <code>request: "GET / HTTP/1.1",</code> . Per un elenco di tutte le variabili che puoi utilizzare, vedi l'<a href="http://nginx.org/en/docs/varindex.html">elenco di variabili Nginx</a>.<br><br>Per registrare un'intestazione aggiuntiva come ad esempio <em>x-custom-ID</em>, aggiungi la seguente coppia chiave-valore al contenuto del log personalizzato: <br><pre class="pre"><code>customID: $http_x_custom_id</code></pre> <br>I trattini (<code>-</code>) vengono convertiti in caratteri di sottolineatura (<code>_</code>) e <code>$http_</code> deve essere aggiunto come prefisso al nome intestazione personalizzato. </td>
    </tr>
    <tr>
    <td><code>log-format-escape-json</code></td>
    <td>Facoltativo: per impostazione predefinita, i log vengono generati in formato testo. Per generare i log in formato JSON, aggiungi il campo <code>log-format-escape-json</code> e utilizza il valore <code>true</code>.</td>
    </tr>
    </tbody></table>
    </dd>
    </dl>

    Ad esempio, il tuo formato di log potrebbe contenere le seguenti variabili:
    ```
    apiVersion: v1
    data:
      log-format: '{remote_address: $remote_addr, remote_user: "$remote_user",
                    time_date: [$time_local], request: "$request",
                    status: $status, http_referer: "$http_referer",
                    http_user_agent: "$http_user_agent",
                    request_id: $request_id}'
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: screen}

    Una voce di log basata su questo formato sarà simile al seguente esempio:
    ```
    remote_address: 127.0.0.1, remote_user: "dbmanager", time_date: [30/Mar/2018:18:52:17 +0000], request: "GET / HTTP/1.1", status: 401, http_referer: "-", http_user_agent: "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:47.0) Gecko/20100101 Firefox/47.0", request_id: a02b2dea9cf06344a25611c1d7ad72db
    ```
    {: screen}

    Per creare un formato di log personalizzato basato sul formato predefinito per i log ALB, modifica la seguente sezione come necessario e aggiungila alla tua mappa di configurazione:
    ```
    apiVersion: v1
    data:
      log-format: '{"time_date": "$time_iso8601", "client": "$remote_addr",
                    "host": "$http_host", "scheme": "$scheme",
                    "request_method": "$request_method", "request_uri": "$uri",
                    "request_id": "$request_id", "status": $status,
                    "upstream_addr": "$upstream_addr", "upstream_status":
                    $upstream_status, "request_time": $request_time,
                    "upstream_response_time": $upstream_response_time,
                    "upstream_connect_time": $upstream_connect_time,
                    "upstream_header_time": $upstream_header_time}'
      log-format-escape-json: "true"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: codeblock}

4. Salva il file di configurazione.

5. Verifica che le modifiche alla mappa di configurazione siano state applicate.

 ```
 kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
 ```
 {: pre}

 Output di esempio:
 ```
 Name:        ibm-cloud-provider-ingress-cm
 Namespace:    kube-system
 Labels:        <none>
 Annotations:    <none>

 Data
 ====

  log-format: '{remote_address: $remote_addr, remote_user: "$remote_user", time_date: [$time_local], request: "$request", status: $status, http_referer: "$http_referer", http_user_agent: "$http_user_agent", request_id: $request_id}'
  log-format-escape-json: "true"
 ```
 {: screen}

4. Per visualizzare i log ALB Ingress, [crea una configurazione di registrazione per il servizio Ingress](cs_health.html#logging) nel tuo cluster.

<br />



