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




# Configurazione delle sottoreti per i cluster
{: #subnets}

Modifica il pool di indirizzi IP pubblici o privati portatili disponibili aggiungendo sottoreti al tuo cluster Kubernetes in {{site.data.keyword.containerlong}}.
{:shortdesc}

In {{site.data.keyword.containershort_notm}}, puoi aggiungere gli indirizzi IP portatili stabili per i servizi Kubernetes aggiungendo delle sottoreti di rete al cluster. In questo caso, le sottoreti non stanno venendo utilizzate con il netmasking per creare la connettività tra uno o più cluster. Invece, le sottoreti sono utilizzate per fornire gli indirizzi IP fissi permanenti per un servizio da un cluster che può essere utilizzato per accedere a tale servizio. 

<dl>
  <dt>Creazione di un cluster che include la creazione della sottorete per impostazione predefinita</dt>
  <dd>Quando crei un cluster standard, {{site.data.keyword.containershort_notm}} esegue automaticamente il provisioning delle seguenti sottoreti:
    <ul><li>Una sottorete pubblica primaria che determina gli indirizzi IP pubblici per i nodi di lavoro durante la creazione del cluster </li>
    <li>Una sottorete privata primaria che determina gli indirizzi IP privati per i nodi di lavoro durante la creazione del cluster</li>
    <li>Una sottorete pubblica portatile che fornisce 5 indirizzi IP pubblici per i servizi di rete di bilanciamento del carico e Ingress</li>
    <li>Una sottorete privata portatile che fornisce 5 indirizzi IP privati per i servizi di rete di bilanciamento del carico e Ingress</li></ul>
      Gli indirizzi IP pubblici e privati portatili sono statici e non cambiano quando viene rimosso un nodo di lavoro. Per ogni sottorete, viene utilizzato un indirizzo IP pubblico e un indirizzo IP privato portatili per i [programmi di bilanciamento del carico dell'applicazione Ingress](cs_ingress.html) predefiniti. Puoi utilizzare il programma di bilanciamento del carico dell'applicazione Ingress per esporre più applicazioni nel tuo cluster. I restanti quattro indirizzi IP pubblici portatili e i restanti quattro privati portatili possono essere utilizzati per esporre le singole applicazioni alla rete pubblica o privata [creando un servizio di bilanciamento del carico](cs_loadbalancer.html).</dd>
  <dt>[Ordinazione e gestione delle tue sottoreti esistenti](#custom)</dt>
  <dd>Puoi ordinare e gestire le sottoreti portatili esistenti nel tuo account dell'infrastruttura IBM Cloud (SoftLayer) invece di utilizzare le sottoreti di cui viene eseguito automaticamente il provisioning. Utilizza questa opzione per mantenere stabili gli indirizzi IP statici tra le rimozioni e le creazioni di cluster o per ordinare blocchi di indirizzi IP più grandi. Crea innanzitutto un cluster senza sottoreti utilizzando il comando `cluster-create --no-subnet` e poi aggiungi la sottorete al cluster con il comando `cluster-subnet-add`. </dd>
</dl>

**Nota:** gli indirizzi IP pubblici portatili vengono addebitati mensilmente. Se rimuovi gli indirizzi IP pubblici portatili dopo aver eseguito il provisioning del tuo cluster, devi comunque pagare l'addebito mensile anche se li hai utilizzati solo per un breve periodo di tempo.

## Richiesta di più sottoreti per il tuo cluster
{: #request}

Puoi aggiungere gli indirizzi IP privati o pubblici portatili stabili al cluster assegnando le sottoreti al cluster.
{:shortdesc}

**Nota:** quando rendi disponibile una sottorete a un cluster, gli indirizzi IP di questa sottorete vengono utilizzati per scopi di rete cluster. Per evitare conflitti di indirizzi IP, assicurati di utilizzare una sottorete con un solo cluster. Non utilizzare una sottorete per più cluster o per altri scopi al di fuori di {{site.data.keyword.containershort_notm}} contemporaneamente.

Prima di iniziare, [indirizza la tua CLI](cs_cli_install.html#cs_cli_configure) al tuo cluster.

Per creare una sottorete in un account dell'infrastruttura IBM Cloud (SoftLayer) e renderla disponibile per un cluster specificato:

1. Esegui il provisioning di una sottorete.

    ```
    bx cs cluster-subnet-create <cluster_name_or_id> <subnet_size> <VLAN_ID>
    ```
    {: pre}

    <table>
    <caption>Descrizione dei componenti di questo comando</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti di questo comando</th>
    </thead>
    <tbody>
    <tr>
    <td><code>cluster-subnet-create</code></td>
    <td>Il comando per eseguire il provisioning di una sottorete per il tuo cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;cluster_name_or_id&gt;</em></code></td>
    <td>Sostituisci <code>&lt;cluster_name_or_id&gt;</code> con il nome o l'ID del cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;subnet_size&gt;</em></code></td>
    <td>Sostituisci <code>&lt;subnet_size&gt;</code> con il numero di indirizzi IP che vuoi aggiungere dalla tua sottorete portatile. I valori accettati sono 8, 16, 32 o 64. <p>**Nota:** quando aggiungi gli indirizzi IP portatili per la tua sottorete, tre indirizzi IP vengono utilizzati per stabilire un collegamento di rete interno al cluster. Non puoi utilizzare questi tre indirizzi IP per il tuo programma di bilanciamento del carico dell'applicazione o per creare un servizio di bilanciamento del carico. Ad esempio, se richiedi otto indirizzi IP pubblici portatili, puoi utilizzarne cinque per esporre le tue applicazioni pubblicamente.</p> </td>
    </tr>
    <tr>
    <td><code><em>&lt;VLAN_ID&gt;</em></code></td>
    <td>Sostituisci <code>&lt;VLAN_ID&gt;</code> con l'ID della VLAN pubblica o privata su cui vuoi assegnare gli indirizzi IP pubblici o privati portatili. Devi selezionare la VLAN pubblica o privata a cui è connesso un nodo di lavoro esistente. Per controllare la VLAN pubblica o privata per un nodo di lavoro, esegui il comando <code>bx cs worker-get &lt;worker_id&gt;</code>. </td>
    </tr>
    </tbody></table>

2.  Verifica che la sottorete sia stata creata e aggiunta correttamente al tuo cluster. Il CIDR della sottorete viene elencato nella sezione **VLAN**.

    ```
    bx cs cluster-get --showResources <cluster_name_or_ID>
    ```
    {: pre}

3. Facoltativo: [abilita l'instradamento tra sottoreti sulla stessa VLAN](#vlan-spanning).

<br />


## Aggiunta o riutilizzo di sottoreti personalizzate ed esistenti nei cluster Kubernetes
{: #custom}

Puoi aggiungere sottoreti pubbliche o private esistenti al tuo cluster Kubernetes oppure puoi riutilizzare le sottoreti da un cluster eliminato.
{:shortdesc}

Prima di iniziare,
- [Indirizza la tua CLI](cs_cli_install.html#cs_cli_configure) al tuo cluster.
- Per riutilizzare le sottoreti da un cluster di cui non hai più bisogno, elimina il cluster non necessario. Le sottoreti vengono eliminate entro 24 ore.

   ```
   bx cs cluster-rm <cluster_name_or_ID
   ```
   {: pre}

Per utilizzare una sottorete esistente nel tuo portfolio dell'infrastruttura IBM Cloud (SoftLayer) con regole firewall personalizzate o indirizzi IP disponibili:

1.  Identifica la sottorete da utilizzare. Nota l'ID della sottorete e l'ID della VLAN. In questo esempio, l'ID sottorete è `1602829` e l'ID VLAN è `2234945`.

    ```
    bx cs subnets
    ```
    {: pre}

    ```
    Getting subnet list...
    OK
    ID        Network             Gateway          VLAN ID   Type      Bound Cluster
    1550165   10.xxx.xx.xxx/26    10.xxx.xx.xxx    2234947   private
    1602829   169.xx.xxx.xxx/28   169.xx.xxx.xxx   2234945   public

    ```
    {: screen}

2.  Conferma l'ubicazione della VLAN. In questo esempio, l'ubicazione è dal10.

    ```
    bx cs vlans dal10
    ```
    {: pre}

    ```
    Getting VLAN list...
    OK
    ID        Name   Number   Type      Router         Supports Virtual Workers
    2234947          1813     private   bcr01a.dal10   true
    2234945          1618     public    fcr01a.dal10   true
    ```
    {: screen}

3.  Crea un cluster utilizzando l'ubicazione e l'ID VLAN che hai identificato. Per riutilizzare una sottorete esistente, inclusi l'indicatore `--no-subnet` per impedire la creazione automatica di una nuova sottorete IP pubblica portatile e di una nuova sottorete IP privata portatile.

    ```
    bx cs cluster-create --location dal10 --machine-type u2c.2x4 --no-subnet --public-vlan 2234945 --private-vlan 2234947 --workers 3 --name my_cluster
    ```
    {: pre}

4.  Verifica che la creazione del cluster sia stata richiesta.

    ```
    bx cs clusters
    ```
    {: pre}

    **Nota:** servono fino a 15 minuti perché le macchine del nodo di lavoro siano ordinate e che il cluster
sia configurato e ne sia stato eseguito il provisioning al tuo account.

    Quando è stato terminato il provisioning del tuo cluster,
lo stato del tuo cluster viene modificato in **distribuito**.

    ```
    Name         ID                                   State      Created          Workers   Location   Version
    mycluster    aaf97a8843a29941b49a598f516da72101   deployed   20170201162433   3         dal10      1.9.7
    ```
    {: screen}

5.  Controlla lo stato dei nodi di lavoro.

    ```
    bx cs workers <cluster>
    ```
    {: pre}

    Quando i nodi di lavoro sono pronti, la condizione
passa a **normale** e lo stato è **Pronto**. Quando lo stato del nodo è **Pronto**,
puoi accedere al
cluster.

    ```
    ID                                                  Public IP        Private IP     Machine Type   State      Status   Location   Version
    prod-dal10-pa8dfcc5223804439c87489886dbbc9c07-w1    169.xx.xxx.xxx   10.xxx.xx.xxx  free           normal     Ready    dal10      1.9.7
    ```
    {: screen}

6.  Aggiungi la sottorete al tuo cluster specificando l'ID sottorete. Quando rendi disponibile una sottorete a un cluster, per te viene creata una mappa di configurazione Kubernetes che include tutti gli indirizzi IP pubblici portatili che puoi utilizzare. Se non esiste alcun programma di bilanciamento del carico dell'applicazione per il tuo cluster, viene utilizzato automaticamente un indirizzo IP pubblico e un indirizzo IP privato portatili per creare i programmi di bilanciamento del carico dell'applicazione pubblici e privati. Tutti gli altri indirizzi IP pubblici e privati portatili possono essere utilizzati per creare i servizi del programma di bilanciamento del carico per le tue applicazioni.

    ```
    bx cs cluster-subnet-add mycluster 807861
    ```
    {: pre}

7. Facoltativo: [abilita l'instradamento tra sottoreti sulla stessa VLAN](#vlan-spanning).

<br />


## Aggiunta delle sottoreti gestite dall'utente e degli indirizzi IP ai cluster Kubernetes
{: #user_managed}

Fornisci una sottorete da una rete in loco a cui desideri che {{site.data.keyword.containershort_notm}} acceda. Dopodiché, puoi aggiungere indirizzi IP privati da tale sottorete ai servizi del programma di bilanciamento del carico nel tuo cluster Kubernetes.
{:shortdesc}

Requisiti:
- Le sottoreti gestite dall'utente possono essere aggiunte solo a VLAN private.
- Il limite di lunghezza del prefisso della sottorete è da /24 a /30. Ad esempio, `169.xx.xxx.xxx/24` specifica 253 indirizzi IP privati utilizzabili, mentre `169.xx.xxx.xxx/30` specifica 1 indirizzo IP privato utilizzabile.
- Il primo indirizzo IP nella sottorete deve essere utilizzato come gateway della sottorete.

Prima di iniziare:
- Configura l'instradamento del traffico di rete in entrata e in uscita della sottorete esterna.
- Conferma di disporre di connettività VPN tra il gateway di rete del data center in loco e il VRA (Virtual Router Appliance) o il servizio VPN strongSwan in esecuzione nel tuo cluster. Per ulteriori informazioni, vedi [Configurazione della connettività VPN](cs_vpn.html).

Per aggiungere una sottorete da una rete in loco:

1. Visualizza l'ID della VLAN privata del tuo cluster. Individua la sezione **VLAN**. Nel campo **User-managed**, identifica l'ID della VLAN con _false_.

    ```
    bx cs cluster-get --showResources <cluster_name>
    ```
    {: pre}

    ```
    VLANs
    VLAN ID   Subnet CIDR       Public   User-managed
    2234947   10.xxx.xx.xxx/29  false    false
    2234945   169.xx.xxx.xxx/29 true     false
    ```
    {: screen}

2. Aggiungi la sottorete esterna alla tua VLAN privata. Gli indirizzi IP privati portatili vengono aggiunti alla mappa di configurazione del cluster.

    ```
    bx cs cluster-user-subnet-add <cluster_name> <subnet_CIDR> <VLAN_ID>
    ```
    {: pre}

    Esempio:

    ```
    bx cs cluster-user-subnet-add mycluster 10.xxx.xx.xxx/24 2234947
    ```
    {: pre}

3. Verifica che la sottorete fornita dall'utente sia stata aggiunta. Il campo **User-managed** è _true_.

    ```
    bx cs cluster-get --showResources <cluster_name>
    ```
    {: pre}

    ```
    VLANs
    VLAN ID   Subnet CIDR       Public   User-managed
    2234947   10.xxx.xx.xxx/29  false    false
    2234945   169.xx.xxx.xxx/29 true   false
    2234947   10.xxx.xx.xxx/24  false    true
    ```
    {: screen}

4. Facoltativo: [abilita l'instradamento tra sottoreti sulla stessa VLAN](#vlan-spanning).

5. Aggiungi un servizio del programma di bilanciamento del carico dell'applicazione a un programma di bilanciamento del carico Ingress privato per accedere alla tua applicazione su una rete privata. Per utilizzare un indirizzo IP privato dalla sottorete che hai aggiunto, devi specificare un indirizzo IP. Diversamente, viene scelto a caso un indirizzo IP dalle sottoreti dell'infrastruttura IBM Cloud (SoftLayer) o dalle sottoreti fornite dall'utente sulla VLAN privata. Per ulteriori informazioni, vedi [Abilitazione dell'accesso pubblico o privato a un'applicazione utilizzando un servizio LoadBalancer](cs_loadbalancer.html#config) o [Abilitazione del programma di bilanciamento del carico dell'applicazione privato](cs_ingress.html#private_ingress).

<br />


## Gestione di sottoreti e indirizzi IP
{: #manage}

Esamina le seguenti opzioni per elencare gli indirizzi IP pubblici disponibili, liberare gli indirizzi IP utilizzati e instradare tra più sottoreti sulla stessa VLAN.
{:shortdesc}

### Visualizzazione degli indirizzi IP pubblici portatili disponibili
{: #review_ip}

Per elencare tutti gli indirizzi IP nel tuo cluster, sia utilizzati che disponibili, puoi eseguire:

  ```
  kubectl get cm ibm-cloud-provider-vlan-ip-config -n kube-system -o yaml
  ```
  {: pre}

Per elencare solo gli indirizzi IP pubblici disponibili per il tuo cluster, puoi utilizzare la seguente procedura:

Prima di iniziare, [imposta il contesto per il cluster che desideri utilizzare.](cs_cli_install.html#cs_cli_configure)

1.  Crea un file di configurazione del servizio Kubernetes denominato
`myservice.yaml` e definisci un servizio di tipo `LoadBalancer` con
un indirizzo IP del programma di bilanciamento del carico fittizio. Il seguente esempio utilizza l'indirizzo IP 1.1.1.1 come
indirizzo IP del programma di bilanciamento del carico.

    ```
    apiVersion: v1
    kind: Service
    metadata:
      labels:
        run: myservice
      name: myservice
      namespace: default
    spec:
      ports:
      - port: 80
        protocol: TCP
        targetPort: 80
      selector:
        run: myservice
      sessionAffinity: None
      type: LoadBalancer
      loadBalancerIP: 1.1.1.1
    ```
    {: codeblock}

2.  Crea il servizio nel tuo cluster.

    ```
    kubectl apply -f myservice.yaml
    ```
    {: pre}

3.  Controlla il servizio.

    ```
    kubectl describe service myservice
    ```
    {: pre}

    **Nota:** la creazione di questo servizio non riesce perché il master Kubernetes non può trovare l'indirizzo IP del programma di bilanciamento del carico specificato nella mappa di configurazione Kubernetes. Quando esegui questo comando, puoi visualizzare il messaggio di errore e l'elenco di indirizzi IP pubblici disponibili per il cluster.

    ```
    Error on cloud load balancer a8bfa26552e8511e7bee4324285f6a4a for service default/myservice with UID 8bfa2655-2e85-11e7-bee4-324285f6a4af: Requested cloud provider IP 1.1.1.1 is not available. The following cloud provider IP addresses are available: <list_of_IP_addresses>
    ```
    {: screen}

### Liberazione degli indirizzi IP utilizzati
{: #free}

Puoi liberare un indirizzo IP portatile utilizzato eliminando il servizio di bilanciamento del carico
che utilizza tale indirizzo IP.
{:shortdesc}

Prima di iniziare, [imposta il contesto per il cluster che desideri utilizzare.](cs_cli_install.html#cs_cli_configure)

1.  Elenca i servizi disponibili nel tuo cluster.

    ```
    kubectl get services
    ```
    {: pre}

2.  Rimuovi il servizio di bilanciamento del carico che utilizza un indirizzo IP pubblico o privato.

    ```
    kubectl delete service <service_name>
    ```
    {: pre}

### Abilitazione dell'instradamento tra sottoreti sulla stessa VLAN
{: #vlan-spanning}

Quando crei un cluster, una sottorete che termina con `/26` viene fornita nella stessa VLAN su cui si trova il cluster. Questa sottorete primaria può contenere fino a 62 nodi di lavoro.
{:shortdesc}

Questo limite di 62 nodi di lavoro potrebbe essere superato da un cluster di grandi dimensioni o da diversi cluster più piccoli di una singola regione che si trovano sulla stessa VLAN. Quando viene raggiunto il limite di 62 nodi di lavoro, viene ordinata una seconda sottorete primaria nella stessa VLAN.

Per instradare tra le sottoreti sulla stessa VLAN, devi attivare lo spanning della VLAN. Per istruzioni, vedi [Abilita o disabilita lo spanning della VLAN](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning).
