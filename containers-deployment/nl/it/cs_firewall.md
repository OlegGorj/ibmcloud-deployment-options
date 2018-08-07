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




# Apertura delle porte e degli indirizzi IP necessari nel tuo firewall
{: #firewall}

Controlla le seguenti situazioni in cui potresti aver bisogno di aprire porte e indirizzi IP specifici nei tuoi firewall per {{site.data.keyword.containerlong}}.
{:shortdesc}

* [Per eseguire i comandi `bx`](#firewall_bx) dal tuo sistema locale quando le politiche di rete aziendali impediscono l'accesso agli endpoint di rete pubblici tramite proxy o firewall.
* [Per eseguire i comandi `kubectl`](#firewall_kubectl) dal tuo sistema locale quando le politiche di rete aziendali impediscono l'accesso agli endpoint di rete pubblici tramite proxy o firewall.
* [Per eseguire i comandi `calicoctl`](#firewall_calicoctl) dal tuo sistema locale quando le politiche di rete aziendali impediscono l'accesso agli endpoint di rete pubblici tramite proxy o firewall.
* [Per consentire la comunicazione tra il master Kubernetes e i nodi di lavoro](#firewall_outbound) quando è configurato un firewall per i nodi di lavoro o le impostazioni del firewall sono personalizzate nel tuo account dell'infrastruttura IBM Cloud (SoftLayer).
* [Per accedere ai servizi NodePort, LoadBalancer o Ingress al di fuori del cluster](#firewall_inbound).

<br />


## Esecuzione dei comandi `bx cs` da dietro un firewall
{: #firewall_bx}

Se le politiche di rete aziendali impediscono l'accesso dal tuo sistema locale agli endpoint pubblici tramite i proxy o i firewall, per eseguire i comandi `bx cs`, devi consentire l'accesso TCP per {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

1. Consenti l'accesso a `containers.bluemix.net` sulla porta 443.
2. Verifica la tua connessione. Se l'accesso è configurato, vengono visualizzate delle barche nell'output.
   ```
   curl https://containers.bluemix.net/v1/
   ```
   {: pre}

   Output di esempio:
   ```
                                     )___(
                              _______/__/_
                     ___     /===========|   ___
    ____       __   [\\\]___/____________|__[///]   __
    \   \_____[\\]__/___________________________\__[//]___
     \                                                    |
      \                                                  /
   ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ```
   {: screen}

<br />


## Esecuzione dei comandi `kubectl` da dietro un firewall
{: #firewall_kubectl}

Se le politiche di rete aziendali impediscono l'accesso dal tuo sistema locale agli endpoint pubblici tramite i proxy o i firewall, per eseguire i comandi `kubectl`, devi consentire l'accesso TCP per il cluster.
{:shortdesc}

Quando viene creato un cluster, la porta nell'URL master viene assegnata casualmente con un valore compreso tra 20000-32767. Puoi scegliere di aprire l'intervallo di porte 20000-32767 per tutti i cluster che possono essere creati oppure scegliere di consentire l'accesso a un cluster esistente specifico.

Prima di cominciare, consenti l'accesso a [eseguire i comandi `bx cs`](#firewall_bx).

Per concedere l'accesso per un cluster specifico:

1. Accedi alla CLI {{site.data.keyword.Bluemix_notm}}. Immetti le tue credenziali
{{site.data.keyword.Bluemix_notm}} quando richiesto. Se hai un account federato, includi l'opzione `--sso`.

   ```
   bx login [--sso]
   ```
   {: pre}

2. Seleziona la regione in cui si trova il tuo cluster.

   ```
   bx cs region-set
   ```
   {: pre}

3. Richiama il nome del tuo cluster.

   ```
   bx cs clusters
   ```
   {: pre}

4. Richiama l'**URL master** del tuo cluster.

   ```
   bx cs cluster-get <cluster_name_or_ID>
   ```
   {: pre}

   Output di esempio:
   ```
   ...
   Master URL:		https://169.xx.xxx.xxx:31142
   ...
   ```
   {: screen}

5. Consenti l'accesso all'**URL master** sulla porta, ad esempio `31142` dall'esempio precedente. 

6. Verifica la tua connessione.

   ```
   curl --insecure <master_URL>/version
   ```
   {: pre}

   Comando di esempio:
   ```
   curl --insecure https://169.xx.xxx.xxx:31142/version
   ```
   {: pre}

   Output di esempio:
   ```
   {
     "major": "1",
     "minor": "7+",
     "gitVersion": "v1.7.4-2+eb9172c211dc41",
     "gitCommit": "eb9172c211dc4108341c0fd5340ee5200f0ec534",
     "gitTreeState": "clean",
     "buildDate": "2017-11-16T08:13:08Z",
     "goVersion": "go1.8.3",
     "compiler": "gc",
     "platform": "linux/amd64"
   }
   ```
   {: screen}

7. Facoltativo: ripeti questa procedura per ogni cluster che devi esporre.

<br />


## Esecuzione dei comandi `calicoctl` da dietro un firewall
{: #firewall_calicoctl}

Se le politiche di rete aziendali impediscono l'accesso dal tuo sistema locale agli endpoint pubblici tramite i proxy o i firewall, per eseguire i comandi `calicoctl`, devi consentire l'accesso TCP per i comandi Calico.
{:shortdesc}

Prima di cominciare, consenti l'accesso a eseguire i comandi [`bx`](#firewall_bx) e [`kubectl`](#firewall_kubectl).

1. Richiama l'indirizzo IP dall'URL master che hai utilizzato per consentire i comandi [`kubectl`](#firewall_kubectl).

2. Ottieni la porta per ETCD.

  ```
  kubectl get cm -n kube-system calico-config -o yaml | grep etcd_endpoints
  ```
  {: pre}

3. Consenti l'accesso alle politiche Calico tramite l'indirizzo IP dell'URL master e la porta ETCD.

<br />


## Consentire al cluster di accedere alle risorse dell'infrastruttura e ad altri servizi
{: #firewall_outbound}

Permetti al tuo cluster di accedere ai servizi e alle risorse dell'infrastruttura da dietro un firewall, come ad esempio per le regioni {{site.data.keyword.containershort_notm}}, {{site.data.keyword.registrylong_notm}}, {{site.data.keyword.monitoringlong_notm}}, {{site.data.keyword.loganalysislong_notm}}, gli IP privati dell'infrastruttura IBM Cloud (SoftLayer) e i dati in uscita per le attestazioni del volume persistente.
{:shortdesc}

  1.  Prendi nota dell'indirizzo IP pubblico di tutti i tuoi nodi di lavoro nel cluster. 

      ```
      bx cs workers <cluster_name_or_ID>
      ```
      {: pre}

  2.  Consenti il traffico di rete in uscita da _<each_worker_node_publicIP>_ di origine alle porte TCP/UDP di destinazione comprese nell'intervallo 20000-32767 e alla porta 443, e ai seguenti indirizzi IP e gruppi di reti. Se hai un firewall aziendale che impedisce l'accesso alla tua macchina locale dagli endpoint internet pubblici, segui questa procedura per i tuoi nodi di lavoro di origine e la tua macchina locale.
      - **Importante**: devi consentire il traffico in uscita alla porta 443 per tutte le ubicazioni all'interno della regione, per bilanciare il carico durante il processo di avvio. Ad esempio, se il tuo cluster si trova in Stati Uniti Sud, devi consentire il traffico dalla porta 443 agli indirizzi IP per tutte le ubicazioni (dal10, dal12 e dal13).
      <p>
  <table summary="La prima riga nella tabella si estende su entrambe le colonne. Le rimanenti righe devono essere lette da sinistra a destra, con l'ubicazione del server nella prima colonna e gli indirizzi IP corrispondenti nella seconda colonna.">
  <caption>Gli indirizzi IP da aprire per il traffico in uscita</caption>
      <thead>
      <th>Regione</th>
      <th>Ubicazione</th>
      <th>indirizzo IP</th>
      </thead>
    <tbody>
      <tr>
        <td>Asia Pacifico Nord</td>
        <td>hkg02<br>seo01<br>sng01<br>tok02</td>
        <td><code>169.56.132.234</code><br><code>169.56.69.242</code><br><code>161.202.186.226</code><br><code>161.202.126.210</code></td>
       </tr>
      <tr>
         <td>Asia Pacifico Sud</td>
         <td>mel01<br>syd01<br>syd04</td>
         <td><code>168.1.97.67</code><br><code>168.1.8.195</code><br><code>130.198.64.19, 130.198.66.34</code></td>
      </tr>
      <tr>
         <td>Europa Centrale</td>
         <td>ams03<br>fra02<br>mil01<br>par01</td>
         <td><code>169.50.169.110, 169.50.154.194</code><br><code>169.50.56.174</code><br><code>159.122.190.98</code><br><code>159.8.86.149, 159.8.98.170</code></td>
        </tr>
      <tr>
        <td>Regno Unito Sud</td>
        <td>lon02<br>lon04</td>
        <td><code>159.122.242.78</code><br><code>158.175.65.170, 158.175.74.170, 158.175.76.2</code></td>
      </tr>
      <tr>
        <td>Stati Uniti Est</td>
         <td>mon01<br>tor01<br>wdc06<br>wdc07</td>
         <td><code>169.54.126.219</code><br><code>169.53.167.50</code><br><code>169.60.73.142</code><br><code>169.61.83.62</code></td>
      </tr>
      <tr>
        <td>Stati Uniti Sud</td>
        <td>dal10<br>dal12<br>dal13<br>hou02<br>sao01</td>
        <td><code>169.47.234.18, 169.46.7.238</code><br><code>169.47.70.10</code><br><code>169.60.128.2</code><br><code>184.173.44.62</code><br><code>169.57.151.10</code></td>
      </tr>
      </tbody>
    </table>
</p>

  3.  Consenti il traffico di rete in uscita dai nodi di lavoro alle regioni [{{site.data.keyword.registrylong_notm}}](/docs/services/Registry/registry_overview.html#registry_regions):
      - `TCP port 443 FROM <each_worker_node_publicIP> TO <registry_publicIP>`
      - Sostituisci <em>&lt;registry_publicIP&gt;</em> con gli indirizzi IP del registro per cui vuoi consentire il traffico. Il registro globale archivia le immagini pubbliche fornite da IBM e i registri regionali archiviano le tue proprie immagini pubbliche e private.
        <p>
<table summary="La prima riga nella tabella si estende su entrambe le colonne. Le rimanenti righe devono essere lette da sinistra a destra, con l'ubicazione del server nella prima colonna e gli indirizzi IP corrispondenti nella seconda colonna.">
  <caption>Gli indirizzi IP da aprire per il traffico del registro </caption>
      <thead>
        <th>Regione {{site.data.keyword.containershort_notm}}</th>
        <th>Indirizzo del registro</th>
        <th>Indirizzo IP del registro</th>
      </thead>
      <tbody>
        <tr>
          <td>Registro globale tra le regioni {{site.data.keyword.containershort_notm}} </td>
          <td>registry.bluemix.net</td>
          <td><code>169.60.72.144/28</code><br><code>169.61.76.176/28</code></td>
        </tr>
        <tr>
          <td>Asia Pacifico Nord, Asia Pacifico Sud</td>
          <td>registry.au-syd.bluemix.net</td>
          <td><code>168.1.45.160/27</code></br><code>168.1.139.32/27</code></td>
        </tr>
        <tr>
          <td>Europa Centrale</td>
          <td>registry.eu-de.bluemix.net</td>
          <td><code>169.50.56.144/28</code></br><code>159.8.73.80/28</code></td>
         </tr>
         <tr>
          <td>Regno Unito Sud</td>
          <td>registry.eu-gb.bluemix.net</td>
          <td><code>159.8.188.160/27</code></br><code>169.50.153.64/27</code></td>
         </tr>
         <tr>
          <td>Stati Uniti Est, Stati Uniti Sud</td>
          <td>registry.ng.bluemix.net</td>
          <td><code>169.55.39.112/28</code></br><code>169.46.9.0/27</code></br><code>169.55.211.0/27</code></td>
         </tr>
        </tbody>
      </table>
</p>

  4.  Facoltativo: consenti il traffico di rete in uscita dai nodi di lavoro ai servizi {{site.data.keyword.monitoringlong_notm}} e {{site.data.keyword.loganalysislong_notm}}:
      - `TCP port 443, port 9095 FROM <each_worker_node_public_IP> TO <monitoring_public_IP>`
      - Sostituisci <em>&lt;monitoring_public_IP&gt;</em> con tutti gli indirizzi delle regioni di monitoraggio per cui desideri consentire il traffico:
        <p><table summary="La prima riga nella tabella si estende su entrambe le colonne. Le rimanenti righe devono essere lette da sinistra a destra, con l'ubicazione del server nella prima colonna e gli indirizzi IP corrispondenti nella seconda colonna.">
  <caption>Gli indirizzi IP da aprire per il traffico di monitoraggio </caption>
        <thead>
        <th>Regione {{site.data.keyword.containershort_notm}}</th>
        <th>Indirizzo di monitoraggio</th>
        <th>Indirizzi IP di monitoraggio</th>
        </thead>
      <tbody>
        <tr>
         <td>Europa Centrale</td>
         <td>metrics.eu-de.bluemix.net</td>
         <td><code>159.122.78.136/29</code></td>
        </tr>
        <tr>
         <td>Regno Unito Sud</td>
         <td>metrics.eu-gb.bluemix.net</td>
         <td><code>169.50.196.136/29</code></td>
        </tr>
        <tr>
          <td>Stati Uniti Est, Stati Uniti Sud, Asia Pacifico Nord</td>
          <td>metrics.ng.bluemix.net</td>
          <td><code>169.47.204.128/29</code></td>
         </tr>
         
        </tbody>
      </table>
</p>
      - `TCP port 443, port 9091 FROM <each_worker_node_public_IP> TO <logging_public_IP>`
      - Sostituisci <em>&lt;logging_public_IP&gt;</em> con tutti gli indirizzi delle regioni di registrazione per cui desideri consentire il traffico:
        <p><table summary="La prima riga nella tabella si estende su entrambe le colonne. Le rimanenti righe devono essere lette da sinistra a destra, con l'ubicazione del server nella prima colonna e gli indirizzi IP corrispondenti nella seconda colonna.">
<caption>Gli indirizzi IP da aprire per il traffico di registrazione </caption>
        <thead>
        <th>Regione {{site.data.keyword.containershort_notm}}</th>
        <th>Indirizzo di registrazione</th>
        <th>Indirizzi IP di registrazione</th>
        </thead>
        <tbody>
          <tr>
            <td>Stati Uniti Est, Stati Uniti Sud</td>
            <td>ingest.logging.ng.bluemix.net</td>
            <td><code>169.48.79.236</code><br><code>169.46.186.113</code></td>
           </tr>
          <tr>
           <td>Regno Unito Sud</td>
           <td>ingest.logging.eu-gb.bluemix.net</td>
           <td><code>169.50.115.113</code></td>
          </tr>
          <tr>
           <td>Europa Centrale</td>
           <td>ingest-eu-fra.logging.bluemix.net</td>
           <td><code>158.177.88.43</code><br><code>159.122.87.107</code></td>
          </tr>
          <tr>
           <td>Asia Pacifico Sud, Asia Pacifico Nord</td>
           <td>ingest-au-syd.logging.bluemix.net</td>
           <td><code>130.198.76.125</code><br><code>168.1.209.20</code></td>
          </tr>
         </tbody>
       </table>
</p>

  5. Per i firewall privati, consenti gli intervalli di IP privati dell'infrastruttura IBM Cloud (SoftLayer) appropriati. Consulta [questo link](https://knowledgelayer.softlayer.com/faq/what-ip-ranges-do-i-allow-through-firewall) a iniziare dalla sezione **Backend (private) Network**.
      - Aggiungi tutte le [ubicazioni nelle regioni](cs_regions.html#locations) che stai utilizzando.
      - Tieni presente che devi aggiungere l'ubicazione (data center) dal01.
      - Apri le porte 80 e 443 per consentire il processo di avvio del cluster.

  6. {: #pvc}Per creare le attestazioni del volume persistente per l'archiviazione dei dati, consenti l'accesso in uscita tramite il tuo firewall per gli [Indirizzi IP dell'infrastruttura IBM Cloud (SoftLayer)](https://knowledgelayer.softlayer.com/faq/what-ip-ranges-do-i-allow-through-firewall) dell'ubicazione (data center) in cui si trova il tuo cluster.
      - Per trovare l'ubicazione (data center) del tuo cluster, esegui `bx cs clusters`.
      - Consenti l'accesso all'intervallo di IP per la **Rete di frontend (pubblica)** e la **Rete di backend (privata)**.
      - Tieni presente che devi aggiungere l'ubicazione (data center) dal01 per la **Rete di backend (privata)**.

<br />


## Accesso ai servizi NodePort, programma di bilanciamento del carico e Ingress dall'esterno del cluster
{: #firewall_inbound}

Puoi ora consentire l'accesso in entrata ai servizi NodePort, programma di bilanciamento del carico e Ingress.
{:shortdesc}

<dl>
  <dt>Servizio NodePort</dt>
  <dd>Apri la porta che hai configurato quando hai distribuito il servizio agli indirizzi IP pubblici per tutti i nodi di lavoro a cui consentire il traffico. Per trovare la porta, esegui `kubectl get svc`. La porta è compresa nell'intervallo 20000-32000.<dd>
  <dt>Servizio LoadBalancer</dt>
  <dd>Apri la porta che hai configurato quando hai distribuito il servizio all'indirizzo IP pubblico del servizio del programma di bilanciamento del carico.</dd>
  <dt>Ingress</dt>
  <dd>Apri la porta 80 per HTTP o la 443 per HTTPS per l'indirizzo IP per il programma di bilanciamento del carico dell'applicazione Ingress.</dd>
</dl>
