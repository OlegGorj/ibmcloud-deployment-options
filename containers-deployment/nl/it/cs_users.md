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



# Assegnazione dell'accesso al cluster
{: #users}

Come amministratore del cluster, puoi definire le politiche di accesso per il tuo cluster Kubernetes per creare livelli diversi di accesso per utenti differenti. Ad esempio, puoi autorizzare determinati utenti a lavorare con le risorse cluster mentre altri possono solo distribuire i contenitori.
{: shortdesc}


## Pianificazione delle richieste di accesso
{: #planning_access}

Come amministratore del cluster, potrebbe essere difficile tenere traccia delle richieste di accesso. Stabilire un modello di comunicazione per le richieste di accesso è essenziale per mantenere la sicurezza del tuo cluster.
{: shortdesc}

Per essere sicuro che le persone giuste abbiano l'accesso corretto, sii chiaro circa le tue politiche per la richiesta di accesso o la richiesta di assistenza per le attività comuni. 

Potresti avere già un metodo che funziona per il tuo team ed è ottimo! Se stai cercando da dove iniziare, prova con uno dei seguenti metodi.

*  Crea un sistema di ticket
*  Crea un template di modulo
*  Crea una pagina wiki
*  Richiedi una richiesta tramite e-mail
*  Utilizza il sistema di tracciamento dei problemi che già usi per tracciare il lavoro quotidiano del tuo team.

Ti senti sopraffatto? Prova questa esercitazione sulle [procedure consigliate per organizzare utenti, team e applicazioni](/docs/tutorials/users-teams-applications.html).
{: tip}

## Autorizzazioni e politiche di accesso
{: #access_policies}

L'ambito di una politica di accesso si basa su un ruolo o sui ruoli definiti dagli utenti che determinano le azioni che essi possono eseguire. Puoi impostare politiche che siano specifiche per i tuoi ruoli cluster, infrastruttura, istanze dell'infrastruttura o Cloud Foundry.
{: shortdesc}

Devi definire le politiche di accesso per ogni utente che lavora con {{site.data.keyword.containershort_notm}}. Alcune politiche sono predefinite, ma altre possono essere personalizzate. Controlla l'immagine e le definizioni riportate di seguito per vedere quali ruoli sono allineati alle attività utente comuni e identificare dove potresti voler personalizzare una politica.

![{{site.data.keyword.containershort_notm}} - ruoli di accesso](/images/user-policies.png)

Figura. Ruoli di accesso {{site.data.keyword.containershort_notm}}

<dl>
  <dt>Politiche IAM (Identity and Access Management)</dt>
    <dd><p><strong>Piattaforma</strong>: puoi determinare le azioni che possono essere eseguite dai singoli utenti su un cluster {{site.data.keyword.containershort_notm}}. Puoi impostare queste politiche in base alla regione. Alcune azioni di esempio sono la creazione o la rimozione di cluster oppure l'aggiunta di nodi di lavoro extra. Queste politiche devono essere configurate insieme alle politiche dell'infrastruttura. </p>
    <p><strong>Infrastruttura</strong>: puoi determinare i livelli di accesso per la tua infrastruttura come ad esempio le macchine del nodo cluster, la rete o le risorse di archiviazione. Si applica la stessa politica se l'utente esegue la richiesta dalla GUI {{site.data.keyword.containershort_notm}} oppure tramite la CLI, anche quando le azioni vengono completate nell'infrastruttura IBM Cloud (SoftLayer). Devi impostare questo tipo di politica insieme alle politiche di accesso alla piattaforma {{site.data.keyword.containershort_notm}}. Per informazioni sui ruoli disponibili, controlla le [autorizzazioni dell'infrastruttura](/docs/iam/infrastructureaccess.html#infrapermission).</p> </br></br><strong>Nota:</strong> assicurati che il tuo account {{site.data.keyword.Bluemix_notm}} sia [configurato con l'accesso al portfolio dell'infrastruttura IBM Cloud (SoftLayer)](cs_troubleshoot_clusters.html#cs_credentials) in modo che gli utenti autorizzati possano eseguire azioni nell'account dell'infrastruttura IBM Cloud (SoftLayer) in base alle autorizzazioni assegnate. </dd>
  <dt>Ruoli RBAC (Kubernetes Resource Based Access Control)</dt>
    <dd>A ogni utente a cui è assegnata una politica di accesso alla piattaforma viene assegnato automaticamente un ruolo Kubernetes. In Kubernetes, [Role Based Access Control (RBAC) ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview) determina le azioni che un utente può eseguire sulle risorse all'interno di un cluster. <strong>Nota</strong>: i ruoli RBAC vengono configurati automaticamente per lo spazio dei nomi <code>default</code>, ma come amministratore del cluster, puoi assegnare i ruoli per gli altri spazi dei nomi.</dd>
  <dt>Cloud Foundry</dt>
    <dd>Non tutti i servizi possono essere gestiti con Cloud IAM. Se stai utilizzando uno di questi servizi, puoi continuare a utilizzare i [ruoli utente Cloud Foundry](/docs/iam/cfaccess.html#cfaccess) per controllare l'accesso ai tuoi servizi.</dd>
</dl>


Downgrade delle autorizzazioni? Il completamento di questa operazione può richiedere alcuni minuti.
{: tip}

### Ruoli della piattaforma
{: #platform_roles}

{{site.data.keyword.containershort_notm}} è configurato per utilizzare i ruoli della piattaforma {{site.data.keyword.Bluemix_notm}}. Le autorizzazioni ruolo si basano l'una sull'altra, ciò significa che il ruolo `Editor` ha le stesse autorizzazioni del ruolo `Visualizzatore`, più le autorizzazioni concesse ad un editor. La tabella riportata di seguito spiega i tipi di azioni che ciascun ruolo può eseguire.

I ruoli RBAC corrispondenti vengono automaticamente assegnati allo spazio dei nomi predefinito quando si assegna un ruolo della piattaforma. Se modifichi un ruolo della piattaforma dell'utente, viene aggiornato anche il ruolo RBAC.
{: tip}

<table>
<caption>Azioni e ruoli della piattaforma</caption>
  <tr>
    <th>Ruoli della piattaforma</th>
    <th>Azioni di esempio</th>
    <th>Ruolo RBAC corrispondente</th>
  </tr>
  <tr>
      <td>Visualizzatore</td>
      <td>Visualizza i dettagli per un cluster o altre istanze di servizio.</td>
      <td>Visualizzatore</td>
  </tr>
  <tr>
    <td>Editor</td>
    <td>Può eseguire o annullare il bind di un servizio IBM Cloud a un cluster oppure creare un webhook. <strong>Nota</strong>: per associare i servizi, devi anche avere assegnato il ruolo di sviluppatore Cloud Foundry.</td>
    <td>Editor</td>
  </tr>
  <tr>
    <td>Operatore</td>
    <td>Può creare, rimuovere, riavviare o ricaricare un nodo di lavoro. Può aggiungere una sottorete a un cluster.</td>
    <td>Amministratore</td>
  </tr>
  <tr>
    <td>Amministratore</td>
    <td>Può creare e rimuovere i cluster. Può modificare le politiche di accesso per altri utenti a livello di account per il servizio e l'infrastruttura. <strong>Nota</strong>: l'accesso da amministratore può essere assegnato a un cluster specifico o a tutte le istanze del servizio nel tuo account. Per eliminare i cluster, devi avere l'accesso da amministratore al cluster che vuoi eliminare. Per creare i cluster devi avere il ruolo di amministratore a tutte le istanze del servizio.</td>
    <td>Amministratore cluster</td>
  </tr>
</table>

Per ulteriori informazioni sull'assegnazione dei ruoli utente nell'IU, vedi [Gestione dell'accesso IAM](/docs/iam/mngiam.html#iammanidaccser).


### Ruoli dell'infrastruttura
{: #infrastructure_roles}

I ruoli dell'infrastruttura consentono agli utenti di eseguire attività sulle risorse a livello dell'infrastruttura. La tabella riportata di seguito spiega i tipi di azioni che ciascun ruolo può eseguire. I ruoli dell'infrastruttura sono personalizzabili; assicurati di fornire agli utenti solo l'accesso di cui necessitano per eseguire le loro attività. 

In aggiunta alla concessione di ruoli dell'infrastruttura specifici, devi anche concedere l'accesso al dispositivo agli utenti che utilizzano l'infrastruttura.
{: tip}

<table>
<caption>Azioni e ruoli dell'infrastruttura</caption>
  <tr>
    <th>Ruolo dell'infrastruttura</th>
    <th>Azioni di esempio</th>
  </tr>
  <tr>
    <td><i>Solo visualizzazione</i></td>
    <td>Puoi visualizzare i dettagli dell'infrastruttura e vedere un riepilogo dell'account, compresi le fatture e i pagamenti </td>
  </tr>
  <tr>
    <td><i>Utente di base</i></td>
    <td>Può modificare le configurazioni del servizio, inclusi gli indirizzi IP, aggiungere o modificare i record DNS e aggiungere i nuovi utenti con l'accesso all'infrastruttura </td>
  </tr>
  <tr>
    <td><i>Super utente</i></td>
    <td>Può eseguire tutte le azioni correlate all'infrastruttura </td>
  </tr>
</table>

Per iniziare ad assegnare i ruoli, segui la procedura presente in [Personalizzazione delle autorizzazioni dell'infrastruttura per un utente](#infra_access).

### Ruoli RBAC
{: #rbac_roles}

RBAC (Resource-based access control) costituisce un modo per proteggere le tue risorse che si trovano all'interno del cluster e per decidere quali utenti possono eseguire i diversi tipi di azioni Kubernetes. Nella seguente tabella, puoi vedere i tipi di ruoli RBAC e i tipi di azioni che gli utenti possono eseguire con ciascun ruolo. Le autorizzazioni si basano l'una sull'altra, ciò vuol dire che un `Amministratore` ha anche tutte le politiche fornite con i ruoli `Visualizzatore` ed `Editor`. Assicurati di fornire agli utenti solo l'accesso di cui necessitano.

I ruoli RBAC sono automaticamente configurati insieme al ruolo della piattaforma dello spazio dei nomi predefinito. [Puoi aggiornare il ruolo o assegnare ruoli da altri spazi dei nomi](#rbac).
{: tip}

<table>
<caption>Azioni e ruoli RBAC</caption>
  <tr>
    <th>Ruolo RBAC</th>
    <th>Azioni di esempio</th>
  </tr>
  <tr>
    <td>Visualizzatore</td>
    <td>Può visualizzare le risorse all'interno dello spazio dei nomi predefinito. I visualizzatori non possono vedere i segreti Kubernetes. </td>
  </tr>
  <tr>
    <td>Editor</td>
    <td>Può leggere e scrivere le risorse all'interno dello spazio dei nomi predefinito.</td>
  </tr>
  <tr>
    <td>Amministratore</td>
    <td>Può leggere e scrivere le risorse all'interno dello spazio dei nomi predefinito ma non lo spazio dei nomi stesso. Può creare i ruoli all'interno di uno spazio dei nomi.</td>
  </tr>
  <tr>
    <td>Amministratore cluster</td>
    <td>Può leggere e scrivere le risorse in ogni spazio dei nomi. Può creare i ruoli all'interno di uno spazio dei nomi. Può accedere al dashboard Kubernetes. Può creare una risorsa Ingress che rende disponibili pubblicamente le applicazioni.</td>
  </tr>
</table>

<br />


## Aggiunta di utenti a un account {{site.data.keyword.Bluemix_notm}}
{: #add_users}

Puoi aggiungere utenti a un account {{site.data.keyword.Bluemix_notm}} per concedere l'accesso ai tuoi cluster.
{:shortdesc}

Prima di iniziare, verifica che ti sia stato assegnato il ruolo di `Gestore` Cloud Foundry per un account {{site.data.keyword.Bluemix_notm}}. 

1.  [Aggiungi l'utente all'account](../iam/iamuserinv.html#iamuserinv).
2.  Nella sezione **Accesso**, espandi **Servizi**.
3.  Assegna un ruolo della piattaforma ad un utente per impostare l'accesso per {{site.data.keyword.containershort_notm}}.
      1. Dall'elenco a discesa **Servizi**, seleziona **{{site.data.keyword.containershort_notm}}**.
      2. Dall'elenco a discesa **Regione**, seleziona la regione in cui invitare l'utente.
      3. Dall'elenco a discesa **Istanza del servizio**, seleziona il cluster da cui invitare l'utente. Per trovare l'ID di uno specifico cluster, esegui `bx cs clusters`.
      4. Nella sezione **Seleziona ruoli**, scegli un ruolo. Per trovare un elenco di azioni supportate per ciascun ruolo, vedi [Autorizzazioni e politiche di accesso](#access_policies).
4. [Facoltativo: assegna un ruolo Cloud Foundry](/docs/iam/mngcf.html#mngcf).
5. [Facoltativo: assegna un ruolo dell'infrastruttura](/docs/iam/infrastructureaccess.html#infrapermission).
6. Fai clic su **Invita utenti**.

<br />


## Descrizione della chiave API IAM e del comando `bx cs credentials-set`
{: #api_key}

Per eseguire il provisioning e lavorare correttamente con i cluster nel tuo account, devi assicurarti che il tuo account sia configurato in modo appropriato per accedere al portfolio dell'infrastruttura IBM Cloud (SoftLayer). A seconda della configurazione del tuo account, puoi utilizzare la chiave API IAM o le credenziali dell'infrastruttura che hai impostato manualmente usando il comando `bx cs credentials-set`.

<dl>
  <dt>Chiave API IAM</dt>
    <dd><p>La chiave API IAM (Identity and Access Management) viene impostata automaticamente per una regione quando viene eseguita la prima azione che richiede la politica di accesso come amministratore {{site.data.keyword.containershort_notm}}. Ad esempio, uno dei tuoi utenti amministratore crea il primo cluster nella regione <code>us-south</code>. In questo modo, la chiave API IAM di questo utente viene memorizzata nell'account per questa regione. La chiave API viene utilizzata per ordinare l'infrastruttura IBM Cloud (SoftLayer), come nuovi nodi di lavoro o VLAN.</p> <p>Quando un altro utente esegue un'azione in questa regione che richiede l'interazione con il portfolio dell'infrastruttura IBM Cloud (SoftLayer), come la creazione di un nuovo cluster o il ricaricamento di un nodo di lavoro, la chiave API memorizzata viene utilizzata per determinare se esistono autorizzazioni sufficienti per eseguire tale azione. Per garantire che le azioni relative all'infrastruttura nel tuo cluster possano essere eseguite correttamente, assegna ai tuoi utenti amministratore {{site.data.keyword.containershort_notm}} la politica di accesso all'infrastruttura <strong>Super utente</strong>.</p>
    <p>Puoi trovare il proprietario della chiave API corrente eseguendo [<code>bx cs api-key-info</code>](cs_cli_reference.html#cs_api_key_info). Se ritieni di dover aggiornare la chiave API memorizzata per una regione, puoi farlo eseguendo il comando [<code>bx cs api-key-reset</code>](cs_cli_reference.html#cs_api_key_reset). Questo comando richiede la politica di accesso come amministratore {{site.data.keyword.containershort_notm}} e memorizza la chiave API dell'utente che esegue questo comando nell'account.</p>
    <p><strong>Nota:</strong> la chiave API memorizzata per la regione potrebbe non essere utilizzata se le credenziali dell'infrastruttura IBM Cloud (SoftLayer) sono state impostate manualmente utilizzando il comando <code>bx cs credentials-set</code>.</p></dd>
  <dt>Credenziali dell'infrastruttura IBM Cloud (SoftLayer) tramite <code>bx cs credentials-set</code></dt>
    <dd><p>Se hai un account Pagamento a consumo {{site.data.keyword.Bluemix_notm}}, hai accesso al portfolio dell'infrastruttura IBM Cloud (SoftLayer) per impostazione predefinita. Tuttavia, potresti voler utilizzare un altro account dell'infrastruttura IBM Cloud (SoftLayer) di cui già disponi per ordinare l'infrastruttura. Puoi collegare questo account dell'infrastruttura al tuo account {{site.data.keyword.Bluemix_notm}} utilizzando il comando [<code>bx cs credentials-set</code>](cs_cli_reference.html#cs_credentials_set).</p>
    <p>Se le credenziali dell'infrastruttura IBM Cloud (SoftLayer) vengono impostate manualmente, queste credenziali sono utilizzate per ordinare l'infrastruttura, anche se esiste una chiave API IAM per l'account. Se l'utente di cui sono state memorizzate le credenziali non dispone delle autorizzazioni necessarie per ordinare l'infrastruttura, le azioni relative all'infrastruttura, come la creazione di un cluster o il ricaricamento di un nodo di lavoro, possono avere esito negativo.</p>
    <p>Per rimuovere le credenziali dell'infrastruttura IBM Cloud (SoftLayer) che erano state impostate manualmente, puoi utilizzare il comando [<code>bx cs credentials-unset</code>](cs_cli_reference.html#cs_credentials_unset). Dopo aver rimosso le credenziali, viene utilizzata la chiave API IAM per ordinare l'infrastruttura.</p></dd>
</dl>

<br />


## Personalizzazione delle autorizzazioni dell'infrastruttura per un utente
{: #infra_access}

Quando imposti le politiche dell'infrastruttura in IAM (Identity and Access Management), a un utente vengono assegnate le autorizzazioni associate a un ruolo. Per personalizzare tali autorizzazioni, devi accedere all'infrastruttura IBM Cloud (SoftLayer) e modificare lì le autorizzazioni.
{: #view_access}

Ad esempio, gli **Utenti di base** possono riavviare un nodo di lavoro ma non possono ricaricarlo. Senza concedere le autorizzazioni di **Super utente** a tale persona, puoi modificare le autorizzazioni dell'infrastruttura IBM Cloud (SoftLayer) e aggiungere l'autorizzazione per eseguire un comando di ricaricamento.

1.  Accedi al tuo [account {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/) e seleziona **Infrastruttura** dal menu. 

2.  Vai a **Account** > **Utenti** > **Elenco utenti**.

3.  Per modificare le autorizzazioni, seleziona il nome di un profilo utente o la colonna **Accesso al dispositivo**.

4.  Nella scheda **Autorizzazioni portale**, personalizza l'accesso dell'utente. Le autorizzazioni necessarie agli utenti dipendono dalle risorse di infrastruttura di cui hanno bisogno:

    * Utilizza l'elenco a discesa **Autorizzazioni rapide** per assegnare il ruolo di **Super utente**, che concede all'utente tutte le autorizzazioni. 
    * Utilizza l'elenco a discesa **Autorizzazioni rapide** per assegnare il ruolo di **Utente di base**, che concede all'utente solo alcune autorizzazioni necessarie. 
    * Se non vuoi concedere tutte le autorizzazioni con il ruolo di **Super utente** o devi aggiungere autorizzazioni al di sopra del ruolo di **Utente di base**, esamina la seguente tabella che descrive le autorizzazioni necessarie per eseguire attività comuni in {{site.data.keyword.containershort_notm}}. 

    <table summary="Autorizzazioni dell'infrastruttura per scenari comuni di {{site.data.keyword.containershort_notm}}.">
     <caption>Autorizzazioni dell'infrastruttura comunemente richieste per {{site.data.keyword.containershort_notm}}</caption>
     <thead>
      <th>Attività comuni in {{site.data.keyword.containershort_notm}}</th>
      <th>Autorizzazioni dell'infrastruttura richieste per scheda</th>
     </thead>
     <tbody>
       <tr>
         <td><strong>Autorizzazioni minime</strong>: <ul><li>Crea un cluster.</li></ul></td>
         <td><strong>Dispositivi</strong>:<ul><li>Visualizza i dettagli di Virtual Server</li><li>Riavvia il server e visualizza le informazioni di sistema IPMI</li><li>Emetti i ricaricamenti SO e avvia il kernel di salvataggio</li></ul><strong>Account</strong>: <ul><li>Aggiungi/Aggiorna istanze cloud</li><li>Aggiungi server</li></ul></td>
       </tr>
       <tr>
         <td><strong>Amministrazione cluster</strong>: <ul><li>Crea, aggiorna ed elimina i cluster.</li><li>Aggiungi, ricarica e riavvia i nodi di lavoro.</li><li>Visualizza le VLAN.</li><li>Crea sottoreti.</li><li>Distribuisci i pod e i servizi di bilanciamento del carico.</li></ul></td>
         <td><strong>Supporto</strong>:<ul><li>Visualizza ticket</li><li>Aggiungi ticket</li><li>Modifica ticket</li></ul>
         <strong>Dispositivi</strong>:<ul><li>Visualizza i dettagli di Virtual Server</li><li>Riavvia il server e visualizza le informazioni di sistema IPMI</li><li>Aggiorna server</li><li>Emetti i ricaricamenti SO e avvia il kernel di salvataggio</li></ul>
         <strong>Servizi</strong>:<ul><li>Gestisci chiavi SSH</li></ul>
         <strong>Account</strong>:<ul><li>Visualizza riepilogo account</li><li>Aggiungi/Aggiorna istanze cloud</li><li>Annulla server</li><li>Aggiungi server</li></ul></td>
       </tr>
       <tr>
         <td><strong>Archiviazione</strong>: <ul><li>Crea attestazioni del volume persistente per il provisioning di volumi persistenti.</li><li>Crea e gestisci risorse dell'infrastruttura di archiviazione.</li></ul></td>
         <td><strong>Servizi</strong>:<ul><li>Gestisci archiviazione</li></ul><strong>Account</strong>:<ul><li>Aggiungi archiviazione</li></ul></td>
       </tr>
       <tr>
         <td><strong>Rete privata</strong>: <ul><li>Gestisci le VLAN private per la rete in cluster</li><li>Configura la connettività VPN per le reti private.</li></ul></td>
         <td><strong>Rete</strong>:<ul><li>Gestisci le rotte di sottorete della rete</li><li>Gestisci i tunnel di rete IPSEC</li><li>Gestisci gateway di rete</li><li>Amministrazione VPN</li></ul></td>
       </tr>
       <tr>
         <td><strong>Rete pubblica</strong>:<ul><li>Configura la rete Ingress o programma di bilanciamento del carico pubblica per esporre le applicazioni.</li></ul></td>
         <td><strong>Dispositivi</strong>:<ul><li>Gestisci Load Balancers</li><li>Modifica nome host/dominio</li><li>Gestisci controllo porta</li></ul>
         <strong>Rete</strong>:<ul><li>Aggiungi calcoli con la porta di rete pubblica</li><li>Gestisci le rotte di sottorete della rete</li><li>Aggiungi indirizzi IP</li></ul>
         <strong>Servizi</strong>:<ul><li>Gestisci DNS, DNS inverso e WHOIS</li><li>Visualizza certificati (SSL)</li><li>Gestisci certificati (SSL)</li></ul></td>
       </tr>
     </tbody>
    </table>

5.  Per salvare le modifiche, fai clic su **Modifica autorizzazioni del portale**.

6.  Nella scheda **Accesso al dispositivo**, seleziona i dispositivi a cui concedere l'accesso.

    * Nel menu a discesa **Tipo di dispositivo**, puoi concedere l'accesso a **Tutti i server virtuali**. 
    * Per consentire agli utenti di accedere ai nuovi dispositivi che vengono creati, seleziona **Accesso concesso automaticamente quando vengono aggiunti nuovi dispositivi**. 
    * Per salvare le modifiche, fai clic su **Aggiorna accesso dispositivo**.

<br />


## Autorizzazione di utenti con ruoli personalizzati di RBAC Kubernetes
{: #rbac}

Le politiche di accesso di {{site.data.keyword.containershort_notm}} corrispondono ad alcuni ruoli RBAC (role-based access control) di Kubernetes, come descritto in [Autorizzazioni e politiche di accesso](#access_policies). Per autorizzare altri ruoli Kubernetes che differiscono dalla politica di accesso corrispondente, puoi personalizzare i ruoli RBAC e quindi assegnare i ruoli a singoli individui o gruppi di utenti.
{: shortdesc}

Ad esempio, potresti voler concedere le autorizzazioni a un gruppo di sviluppatori affinché possano lavorare su un determinato gruppo API o con risorse all'interno di uno spazio dei nomi Kubernetes nel cluster, ma non nell'intero cluster. Dovrai creare un ruolo e quindi associare il ruolo agli utenti utilizzando un nome utente che sia univoco per {{site.data.keyword.containershort_notm}}. Per ulteriori informazioni, vedi [Using RBAC Authorization ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview) nella documentazione di Kubernetes.

Prima di iniziare, [indirizza la CLI Kubernetes al cluster](cs_cli_install.html#cs_cli_configure).

1.  Crea un ruolo con l'accesso che vuoi assegnare.

    1. Crea un file `.yaml` per definire un ruolo con l'accesso che vuoi assegnare.

        ```
        kind: Role
        apiVersion: rbac.authorization.k8s.io/v1
        metadata:
          namespace: default
          name: my_role
        rules:
        - apiGroups: [""]
          resources: ["pods"]
          verbs: ["get", "watch", "list"]
        - apiGroups: ["apps", "extensions"]
          resources: ["daemonsets", "deployments"]
          verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
        ```
        {: codeblock}

        <table>
        <caption>Descrizione dei componenti di questo YAML</caption>
          <thead>
            <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti di questo YAML</th>
          </thead>
          <tbody>
            <tr>
              <td><code>kind</code></td>
              <td>Utilizza `Role` per concedere l'accesso alle risorse all'interno di un singolo spazio dei nomi o `ClusterRole` per le risorse a livello di cluster.</td>
            </tr>
            <tr>
              <td><code>apiVersion</code></td>
              <td><ul><li>Per i cluster che eseguono Kubernetes 1.8 o successiva, utilizza `rbac.authorization.k8s.io/v1`. </li><li>Per le versioni precedenti, utilizza `apiVersion: rbac.authorization.k8s.io/v1beta1`.</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/namespace</code></td>
              <td><ul><li>Per il tipo `Role`: specifica lo spazio dei nomi Kubernetes a cui è concesso l'accesso.</li><li>Non utilizzare il campo `namespace` se stai creando un `ClusterRole` che si applica a livello di cluster.</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/name</code></td>
              <td>Assegna un nome al ruolo e utilizza il nome in seguito quando associ il ruolo.</td>
            </tr>
            <tr>
              <td><code>rules/apiGroups</code></td>
              <td><ul><li>Specifica i gruppi di API Kubernetes con i quali vuoi che gli utenti possano interagire, ad esempio `"apps"`, `"batch"` o `"extensions"`. </li><li>Per l'accesso al gruppo API principale nel percorso REST `api/v1`, lascia il gruppo vuoto: `[""]`.</li><li>Per ulteriori informazioni, vedi [API groups![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://v1-9.docs.kubernetes.io/docs/reference/api-overview/#api-groups) nella documentazione di Kubernetes.</li></ul></td>
            </tr>
            <tr>
              <td><code>rules/resources</code></td>
              <td><ul><li>Specifica le risorse Kubernetes a cui vuoi concedere l'accesso, ad esempio `"daemonsets"`, `"deployments"`, `"events"` o `"ingresses"`.</li><li>Se specifichi `"nodes"`, il tipo di ruolo deve essere `ClusterRole`.</li><li>Per un elenco di risorse, vedi la tabella di [Tipi di risorse ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/reference/kubectl/cheatsheet/) nella pagina di aiuto di Kubernetes. </li></ul></td>
            </tr>
            <tr>
              <td><code>rules/verbs</code></td>
              <td><ul><li>Specifica i tipi di azioni che vuoi che gli utenti siano in grado di eseguire, ad esempio `"get"`, `"list"`, `"describe"`, `"create"` o `"delete"`. </li><li>Per un elenco completo di verbi, vedi la [documentazione `kubectl`![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/reference/kubectl/overview/).</li></ul></td>
            </tr>
          </tbody>
        </table>

    2.  Crea il ruolo nel tuo cluster.

        ```
        kubectl apply -f <path_to_yaml_file>
        ```
        {: pre}

    3.  Verifica che il ruolo sia stato creato.

        ```
        kubectl get roles
        ```
        {: pre}

2.  Associa gli utenti al ruolo.

    1. Crea un file `.yaml` per associare gli utenti al ruolo. Nota l'URL univoco da utilizzare per il nome di ogni soggetto.

        ```
        kind: RoleBinding
        apiVersion: rbac.authorization.k8s.io/v1
        metadata:
          name: my_role_team1
          namespace: default
        subjects:
        - kind: User
          name: https://iam.ng.bluemix.net/kubernetes#user1@example.com
          apiGroup: rbac.authorization.k8s.io
        - kind: User
          name: https://iam.ng.bluemix.net/kubernetes#user2@example.com
          apiGroup: rbac.authorization.k8s.io
        roleRef:
          kind: Role
          name: custom-rbac-test
          apiGroup: rbac.authorization.k8s.io
        ```
        {: codeblock}

        <table>
        <caption>Descrizione dei componenti di questo YAML</caption>
          <thead>
            <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti di questo YAML</th>
          </thead>
          <tbody>
            <tr>
              <td><code>kind</code></td>
              <td>Specifica il `kind` come `RoleBinding` per entrambi i tipi di file `.yaml` dei ruoli: `Role` per lo spazio dei nomi e `ClusterRole` a livello di cluster.</td>
            </tr>
            <tr>
              <td><code>apiVersion</code></td>
              <td><ul><li>Per i cluster che eseguono Kubernetes 1.8 o successiva, utilizza `rbac.authorization.k8s.io/v1`. </li><li>Per le versioni precedenti, utilizza `apiVersion: rbac.authorization.k8s.io/v1beta1`.</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/namespace</code></td>
              <td><ul><li>Per il tipo `Role`: specifica lo spazio dei nomi Kubernetes a cui è concesso l'accesso.</li><li>Non utilizzare il campo `namespace` se stai creando un `ClusterRole` che si applica a livello di cluster.</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/name</code></td>
              <td>Fornisci un nome per l'associazione del ruolo.</td>
            </tr>
            <tr>
              <td><code>subjects/kind</code></td>
              <td>Specifica il tipo come `User`.</td>
            </tr>
            <tr>
              <td><code>subjects/name</code></td>
              <td><ul><li>Aggiungi l'indirizzo e-mail dell'utente al seguente URL: `https://iam.ng.bluemix.net/kubernetes#`.</li><li>Ad esempio, `https://iam.ng.bluemix.net/kubernetes#user1@example.com`</li></ul></td>
            </tr>
            <tr>
              <td><code>subjects/apiGroup</code></td>
              <td>Utilizza `rbac.authorization.k8s.io`.</td>
            </tr>
            <tr>
              <td><code>roleRef/kind</code></td>
              <td>Immetti lo stesso valore del `kind` nel file `.yaml` del ruolo: `Role` o `ClusterRole`.</td>
            </tr>
            <tr>
              <td><code>roleRef/name</code></td>
              <td>Immetti il nome del file `.yaml` del ruolo.</td>
            </tr>
            <tr>
              <td><code>roleRef/apiGroup</code></td>
              <td>Utilizza `rbac.authorization.k8s.io`.</td>
            </tr>
          </tbody>
        </table>

    2. Crea la risorsa di associazione del ruolo nel tuo cluster.

        ```
        kubectl apply -f filepath/my_role_team1.yaml
        ```
        {: pre}

    3.  Verifica che l'associazione sia stata creata.

        ```
        kubectl get rolebinding
        ```
        {: pre}

Ora che hai creato e associato un ruolo RBAC Kubernetes personalizzato, occupati degli utenti. Chiedi loro di testare un'azione per la quale dispongono dell'autorizzazione grazie al ruolo, come l'eliminazione di un pod.
