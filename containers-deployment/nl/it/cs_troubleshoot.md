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



# Debug del tuo cluster
{: #cs_troubleshoot}

Mentre utilizzi {{site.data.keyword.containerlong}}, tieni presenti queste tecniche per procedure generiche sulla risoluzione dei problemi e sull'esecuzione del debug per i tuoi cluster. Puoi anche verificare lo [stato del sistema {{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://developer.ibm.com/bluemix/support/#status).
{: shortdesc}

Puoi eseguire questi passi generali per assicurarti che i tuoi cluster siano aggiornati: 
- Controlla mensilmente la disponibilità delle patch di sicurezza e del sistema operativo per [aggiornare i tuoi nodi di lavoro](cs_cli_reference.html#cs_worker_update). 
- [Aggiorna il tuo cluster](cs_cli_reference.html#cs_cluster_update) alla [versione di Kubernetes](cs_versions.html) più recente predefinita per {{site.data.keyword.containershort_notm}}

## Debug dei cluster
{: #debug_clusters}

Rivedi le opzioni per eseguire il debug dei tuoi cluster e trovare le cause principali degli errori.

1.  Elenca il tuo cluster e trova lo `Stato` del cluster.

  ```
  bx cs clusters
  ```
  {: pre}

2.  Riesamina lo `Stato` del cluster. Se il tuo cluster si trova in uno stato **Critico**, **Eliminazione non riuscita** o **Avvertenza** o rimane bloccato nello stato **In sospeso** per molto tempo, avvia il [debug dei nodi di lavoro](#debug_worker_nodes).

    <table summary="Ogni riga della tabella deve essere letta da sinistra verso destra, con lo stato del cluster nella prima colonna e un descrizione nella seconda colonna.">
<caption>Stati cluster</caption>
   <thead>
   <th>Stato cluster</th>
   <th>Descrizione</th>
   </thead>
   <tbody>
<tr>
   <td>Interrotto</td>
   <td>L'eliminazione del cluster viene richiesta dall'utente prima che il master Kubernetes venga distribuito. Al termine dell'eliminazione del cluster, questo viene rimosso dal tuo dashboard. Se il tuo cluster rimane bloccato in questo stato per un lungo periodo, apri un [{{site.data.keyword.Bluemix_notm}} ticket di supporto](cs_troubleshoot.html#ts_getting_help).</td>
   </tr>
 <tr>
     <td>Critico</td>
     <td>Il master Kubernetes non può essere raggiunto o tutti i nodi di lavoro nel cluster sono inattivi. </td>
    </tr>
   <tr>
     <td>Eliminazione non riuscita</td>
     <td>Non è possibile eliminare il master Kubernetes o almeno uno dei nodi di lavoro.  </td>
   </tr>
   <tr>
     <td>Eliminato</td>
     <td>Il cluster viene eliminato ma non ancora rimosso dal tuo dashboard. Se il tuo cluster rimane bloccato in questo stato per un lungo periodo, apri un [{{site.data.keyword.Bluemix_notm}} ticket di supporto](cs_troubleshoot.html#ts_getting_help). </td>
   </tr>
   <tr>
   <td>Eliminazione</td>
   <td>Il cluster viene eliminato e la relativa infrastruttura viene smantellata. Non puoi accedere al cluster.  </td>
   </tr>
   <tr>
     <td>Distribuzione non riuscita</td>
     <td>Non è stato possibile completare la distribuzione del master Kubernetes. Non puoi risolvere questo stato. Contatta il supporto IBM Cloud aprendo un [{{site.data.keyword.Bluemix_notm}} ticket di supporto](cs_troubleshoot.html#ts_getting_help).</td>
   </tr>
     <tr>
       <td>Distribuzione</td>
       <td>Il master Kubernetes non è ancora stato completamente distribuito. Non puoi accedere al tuo cluster. Attendi fino alla completa distribuzione del cluster per verificarne l'integrità.</td>
      </tr>
      <tr>
       <td>Normale</td>
       <td>Tutti i nodi di lavoro in un cluster sono attivi e in esecuzione. Puoi accedere al cluster e distribuire le applicazioni al cluster. Questo stato è considerato come integro e non richiede un'azione da parte tua. **Nota**: anche se i nodi di lavoro possono essere normali, le altre risorse dell'infrastruttura, come [rete](cs_troubleshoot_network.html) e [archiviazione](cs_troubleshoot_storage.html), potrebbero necessitare ancora di attenzione.</td>
    </tr>
      <tr>
       <td>In sospeso</td>
       <td>Il master Kubernetes è stato distribuito. Sta venendo eseguito il provisioning dei nodi di lavoro e non sono ancora disponibili nel cluster. Puoi accedere al cluster, ma non puoi distribuire le applicazioni al cluster.  </td>
     </tr>
   <tr>
     <td>Richiesto</td>
     <td>Viene inviata una richiesta per creare il cluster e ordinare l'infrastruttura per il master Kubernetes e i nodi di lavoro. Quando viene avviata la distribuzione del cluster, lo stato del cluster cambia in <code>Distribuzione</code>. Se il tuo cluster rimane bloccato nello stato <code>Richiesto</code> per molto tempo, apri un [{{site.data.keyword.Bluemix_notm}}ticket di supporto](cs_troubleshoot.html#ts_getting_help). </td>
   </tr>
   <tr>
     <td>Aggiornamento in corso</td>
     <td>Il server API Kubernetes eseguito nel master Kubernetes viene aggiornato a una nuova versione dell'API Kubernetes. Durante l'aggiornamento, non è possibile accedere o modificare il cluster. I nodi di lavoro, le applicazioni e le risorse che sono state distribuite dall'utente non vengono modificate e continuano a essere eseguite. Attendi il completamento dell'aggiornamento per verificare l'integrità del tuo cluster. </td>
   </tr>
    <tr>
       <td>Avvertenza</td>
       <td>Almeno un nodo di lavoro nel cluster non è disponibile, ma altri lo sono e possono subentrare nel carico di lavoro. </td>
    </tr>
   </tbody>
 </table>


<br />


## Debug dei nodi di lavoro
{: #debug_worker_nodes}

Rivedi le opzioni per eseguire il debug dei nodi di lavoro e trovare le cause principali degli errori.


1.  Se il tuo cluster si trova in uno stato **Critico**, **Eliminazione non riuscita** o **Avvertenza** o rimane bloccato nello stato **In sospeso** per molto tempo, controlla lo stato dei tuoi nodi di lavoro.

  ```
  bx cs workers <cluster_name_or_id>
  ```
  {: pre}

2.  Controlla il campo `State` e `Status` per ogni nodo di lavoro nell'output della CLI.

  <table summary="Ogni riga della tabella deve essere letta da sinistra verso destra, con lo stato del cluster nella prima colonna e un descrizione nella seconda colonna.">
  <caption>Stati del nodo di lavoro </caption>
    <thead>
    <th>Stato nodo di lavoro</th>
    <th>Descrizione</th>
    </thead>
    <tbody>
  <tr>
      <td>Critico</td>
      <td>Un nodo di lavoro può entrare in uno stato Critico per diversi motivi: <ul><li>Hai effettuato un riavvio per il tuo nodo di lavoro senza prima delimitarlo e svuotarlo. Il riavvio di un nodo di lavoro può causare il danneggiamento dei dati in <code>docker</code>, <code>kubelet</code>, <code>kube-proxy</code> e <code>calico</code>. </li><li>I pod distribuiti sul tuo nodo di lavoro non utilizzano limiti di risorse per [memoria ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/tasks/configure-pod-container/assign-memory-resource/) e [CPU ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/tasks/configure-pod-container/assign-cpu-resource/). Senza limiti di risorse, i pod possono consumare tutte le risorse disponibili, senza lasciare risorse per altri pod da eseguire su questo nodo di lavoro. Questo eccesso del carico di lavoro causa un errore per il nodo di lavoro. </li><li><code>Docker</code>, <code>kubelet</code> o <code>calico</code> sono entrati in uno stato irreversibile dopo aver eseguito centinaia o migliaia di contenitori nel tempo. </li><li>Hai configurato un VRA (Virtual Router Appliance) per il tuo nodo di lavoro che è diventato inattivo e ha interrotto la comunicazione tra il nodo di lavoro e il master Kubernetes. </li><li> Problemi di rete correnti in {{site.data.keyword.containershort_notm}} o nell'infrastruttura IBM Cloud (SoftLayer) che comportano un errore di comunicazione tra il tuo nodo di lavoro e il master Kubernetes.</li><li>Il tuo nodo di lavoro ha esaurito la capacità. Controlla lo <strong>Stato</strong> del nodo di lavoro per vedere se indica una condizione di <strong>Spazio su disco esaurito</strong> o <strong>Memoria esaurita</strong>. Se il tuo nodo di lavoro ha esaurito la capacità, puoi ridurre il carico di lavoro sul nodo di lavoro o aggiungere un nodo di lavoro al tuo cluster per bilanciare il carico di lavoro.</li></ul> In molti casi, [ricaricare](cs_cli_reference.html#cs_worker_reload) il nodo di lavoro può risolvere il problema. Quando ricarichi il tuo nodo di lavoro, verrà applicata la [versione patch](cs_versions.html#version_types)
più recente. La versione principale e quella secondaria non cambiano. Prima di ricaricare il tuo nodo di lavoro, assicurati di delimitarlo e svuotarlo per garantire che i pod esistenti vengano terminati correttamente e ripianificati sui nodi di lavoro rimanenti. </br></br> Se il ricaricamento del nodo di lavoro non risolve il problema, vai al passo successivo per continuare a risolvere il problema. </br></br><strong>Suggerimento:</strong> puoi [configurare i controlli di integrità per il nodo di lavoro e abilitare l'Autorecovery](cs_health.html#autorecovery). Se Autorecovery rileva un nodo di lavoro non integro in base ai controlli configurati, attiva un'azione correttiva come un ricaricamento del sistema operativo sul nodo di lavoro. Per ulteriori informazioni su come funziona Autorecovery, vedi il [blog Autorecovery ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/blogs/bluemix/2017/12/autorecovery-utilizes-consistent-hashing-high-availability/).
      </td>
     </tr>
      <tr>
        <td>Distribuzione</td>
        <td>Quando aggiorni la versione di Kubernetes del tuo nodo di lavoro, il nodo di lavoro viene ridistribuito per installare gli aggiornamenti. Se il tuo nodo di lavoro rimane bloccato in questo stato per molto tempo, vai al passo successivo per controllare se si è verificato un problema durante la distribuzione. </td>
     </tr>
        <tr>
        <td>Normale</td>
        <td>È stato eseguito il provisioning completo del tuo nodo di lavoro ed è pronto per essere utilizzato nel cluster. Questo stato è considerato come integro e non richiede un'azione da parte dell'utente. **Nota**: anche se i nodi di lavoro possono essere normali, le altre risorse dell'infrastruttura, come [rete](cs_troubleshoot_network.html) e [archiviazione](cs_troubleshoot_storage.html), potrebbero necessitare ancora di attenzione.</td>
     </tr>
   <tr>
        <td>Provisioning</td>
        <td>Sta venendo eseguito il provisioning del tuo nodo di lavoro e non è ancora disponibile nel cluster. Puoi monitorare il processo di provisioning nella colonna <strong>Stato</strong> del tuo output CLI. Se il tuo nodo di lavoro rimane bloccato in questo stato per molto tempo, vai al passo successivo per controllare se si è verificato un problema durante il provisioning.</td>
      </tr>
      <tr>
        <td>Provision_failed</td>
        <td>Non è stato possibile eseguire il provisioning del tuo nodo di lavoro. Continua con il passo successivo per trovare i dettagli del malfunzionamento.</td>
      </tr>
   <tr>
        <td>Ricaricamento</td>
        <td>Il tuo nodo di lavoro sta venendo ricaricato e non è disponibile nel cluster. Puoi monitorare il processo di ricaricamento nella colonna <strong>Stato</strong> del tuo output CLI. Se il tuo nodo di lavoro rimane bloccato in questo stato per molto tempo, vai al passo successivo per controllare se si è verificato un problema durante il ricaricamento.</td>
       </tr>
       <tr>
        <td>Reloading_failed </td>
        <td>Impossibile ricaricare il tuo nodo di lavoro. Continua con il passo successivo per trovare i dettagli del malfunzionamento.</td>
      </tr>
      <tr>
        <td>Reload_pending </td>
        <td>Viene inviata una richiesta per ricaricare o aggiornare la versione Kubernetes del tuo nodo di lavoro. Quando il nodo di lavoro viene ricaricato, lo stato cambia in <code>Ricaricamento</code>. </td>
      </tr>
      <tr>
       <td>Sconosciuto</td>
       <td>Il master Kubernetes non è raggiungibile per uno dei seguenti motivi:<ul><li>Hai richiesto un aggiornamento del tuo master Kubernetes. Lo stato del nodo di lavoro non può essere richiamato durante l'aggiornamento.</li><li>Potresti avere un altro firewall che protegge i tuoi nodi di lavoro o hai modificato le impostazioni del firewall recentemente. {{site.data.keyword.containershort_notm}}
richiede che alcuni indirizzi IP e porte siano aperti per consentire la comunicazione dal nodo di lavoro al master Kubernetes e viceversa. Per ulteriori informazioni, vedi [Il firewall impedisce la connessione dei nodi di lavoro](cs_troubleshoot_clusters.html#cs_firewall).</li><li>Il master Kubernetes è inattivo. Contatta il supporto {{site.data.keyword.Bluemix_notm}} aprendo un [{{site.data.keyword.Bluemix_notm}} ticket di supporto](#ts_getting_help).</li></ul></td>
  </tr>
     <tr>
        <td>Avvertenza</td>
        <td>Il tuo nodo di lavoro ha raggiunto il limite di memoria o spazio disco. Puoi ridurre il carico di lavoro nel tuo nodo di lavoro o aggiungere un nodo di lavoro al tuo cluster per bilanciare il carico del carico di lavoro.</td>
  </tr>
    </tbody>
  </table>

5.  Elenca i dettagli del nodo di lavoro. Se i dettagli includono un messaggio di errore, rivedi l'elenco di [messaggi di errore comuni per i nodi di lavoro](#common_worker_nodes_issues) per imparare come risolvere il problema.

   ```
   bx cs worker-get <worker_id>
   ```
   {: pre}

  ```
  bx cs worker-get [<cluster_name_or_id>] <worker_node_id>
  ```
  {: pre}

<br />


## Problemi comuni con i nodi di lavoro
{: #common_worker_nodes_issues}

Controlla i messaggi di errore comuni e impara come risolverli.

  <table>
  <caption>Messaggi di errore comuni</caption>
    <thead>
    <th>Messaggio di errore</th>
    <th>Descrizione e risoluzione
    </thead>
    <tbody>
      <tr>
        <td>Eccezione infrastruttura {{site.data.keyword.Bluemix_notm}}: al tuo account è al momento vietato ordinare le 'Istanze di calcolo'.</td>
        <td>Al tuo account dell'infrastruttura IBM Cloud (SoftLayer) potrebbe essere stato impedito di ordinare le risorse di calcolo. Contatta il supporto {{site.data.keyword.Bluemix_notm}} aprendo un [{{site.data.keyword.Bluemix_notm}} ticket di supporto](#ts_getting_help).</td>
      </tr>
      <tr>
        <td>Eccezione infrastruttura {{site.data.keyword.Bluemix_notm}}: impossibile inserire l'ordine. Le risorse dietro il router 'router_name' non sono sufficienti a soddisfare la richiesta per i seguenti guest: 'worker_id'.</td>
        <td>La VLAN che hai selezionato è associata a un pod nel data center con insufficiente spazio per il provisioning del tuo nodo di lavoro. Puoi scegliere tra le seguenti opzioni:<ul><li>Utilizza un data center differente per eseguire il provisioning del tuo nodo di lavoro. Esegui <code>bx cs locations</code> per elencare i data center disponibili.<li>Se disponi di una coppia di VLAN privata e pubblica esistente che è associata a un altro pod nel data center, utilizza invece questa coppia di VLAN.<li>Contatta il supporto {{site.data.keyword.Bluemix_notm}} aprendo un [{{site.data.keyword.Bluemix_notm}} ticket di supporto](#ts_getting_help).</ul></td>
      </tr>
      <tr>
        <td>Eccezione infrastruttura {{site.data.keyword.Bluemix_notm}}: impossibile ottenere la VLAN di rete con ID: &lt;vlan id&gt;. </td>
        <td>Non è stato possibile eseguire il provisioning del tuo nodo di lavoro perché non è stato possibile trovare l'ID della VLAN selezionato per uno dei seguenti motivi:<ul><li>Potresti aver specificato un numero della VLAN invece dell'ID. Il numero della VLAN è di 3 o 4 cifre, mentre l'ID è di 7. Esegui <code>bx cs vlans &lt;location&gt;</code> per richiamare l'ID della VLAN.<li>L'ID VLAN potrebbe non essere associato all'account dell'infrastruttura IBM Cloud (SoftLayer) da te utilizzato. Esegui <code>bx cs vlans &lt;location&gt;</code> per elencare gli ID della VLAN disponibili per il tuo account. Per modificare l'account dell'infrastruttura IBM Cloud (SoftLayer), vedi [`bx cs credentials-set`](cs_cli_reference.html#cs_credentials_set). </ul></td>
      </tr>
      <tr>
        <td>SoftLayer_Exception_Order_InvalidLocation: l'ubicazione fornita per questo ordine non è valida. (HTTP 500)</td>
        <td>La tua infrastruttura IBM Cloud (SoftLayer) non è configurata per ordinare le risorse di calcolo nel data center selezionato. Contatta il [supporto {{site.data.keyword.Bluemix_notm}}](#ts_getting_help) per verificare che il tuo account sia configurato correttamente. </td>
       </tr>
       <tr>
        <td>Eccezione infrastruttura {{site.data.keyword.Bluemix_notm}}: l'utente non dispone della autorizzazioni dell'infrastruttura {{site.data.keyword.Bluemix_notm}} necessarie per aggiungere i server.
        </br></br>
        Eccezione infrastruttura {{site.data.keyword.Bluemix_notm}}: 'Elemento' deve essere ordinato con l'autorizzazione.</td>
        <td>Potresti non disporre delle autorizzazioni necessarie per eseguire il provisioning di un nodo di lavoro dal portfolio dell'infrastruttura IBM Cloud (SoftLayer). Vedi [Configurazione dell'accesso al portfolio dell'infrastruttura IBM Cloud (SoftLayer) per creare cluster Kubernetes standard](cs_troubleshoot_clusters.html#cs_credentials).</td>
      </tr>
      <tr>
       <td>Il nodo di lavoro non riesce a comunicare con i server {{site.data.keyword.containershort_notm}}. Verifica che la configurazione del firewall consenta il traffico da questo nodo di lavoro.
       <td><ul><li>Se hai un firewall, [configura le impostazioni del tuo firewall per consentire il traffico in uscita alle porte e agli indirizzi IP appropriati](cs_firewall.html#firewall_outbound).</li><li>Verifica se il tuo cluster non ha un IP pubblico eseguendo `bx cs workers <mycluster>`. Se non viene elencato alcun IP pubblico, il tuo cluster ha solo VLAN private. <ul><li>Se vuoi che il cluster abbia sono VLAN private, configura la tua [connessione VLAN](cs_clusters.html#worker_vlan_connection) e il tuo [firewall](cs_firewall.html#firewall_outbound).</li><li>Se vuoi che il cluster abbia un IP pubblico, [aggiungi nuovi nodi di lavoro](cs_cli_reference.html#cs_worker_add) con VLAN sia pubbliche che private.</li></ul></li></ul></td>
     </tr>
      <tr>
  <td>Impossibile creare il token del portale IMS, poiché nessun account IMS è collegato all'account BSS selezionato</br></br>L'utente fornito non è stato trovato o non è attivo</br></br>SoftLayer_Exception_User_Customer_InvalidUserStatus: lo stato dell'account utente è attualmente cancel_pending.</br></br>Attendere che la macchina sia visibile all'utente</td>
  <td>Il proprietario della chiave API utilizzata per accedere al portfolio dell'infrastruttura IBM Cloud (SoftLayer) non ha le autorizzazioni necessarie per eseguire l'azione o potrebbe essere in attesa di eliminazione. </br></br><strong>Come utente</strong>, completa la seguente procedura: <ol><li>Se hai accesso a più account, assicurati di essere collegato all'account in cui desideri lavorare con {{site.data.keyword.containerlong_notm}}. </li><li>Esegui <code>bx cs api-key-info</code> per visualizzare il proprietario della chiave API corrente utilizzata per accedere al portfolio dell'infrastruttura IBM Cloud (SoftLayer). </li><li>Esegui <code>bx account list</code> per visualizzare il proprietario dell'account {{site.data.keyword.Bluemix_notm}} che usi attualmente. </li><li>Contatta il proprietario dell'account {{site.data.keyword.Bluemix_notm}} e segnala che il proprietario della chiave API non dispone di autorizzazioni sufficienti nell'infrastruttura IBM Cloud (SoftLayer) o potrebbe essere in attesa di eliminazione. </li></ol></br><strong>Come proprietario dell'account</strong>, completa la seguente procedura: <ol><li>Rivedi le [autorizzazioni necessarie nell'infrastruttura IBM Cloud (SoftLayer)](cs_users.html#infra_access) per eseguire l'azione precedentemente non riuscita. </li><li>Correggi le autorizzazioni del proprietario della chiave API o crea una nuova chiave API utilizzando il comando [<code>bx cs api-key-reset</code>](cs_cli_reference.html#cs_api_key_reset). </li><li>Se tu o un altro amministratore dell'account imposta manualmente le credenziali dell'infrastruttura IBM Cloud (SoftLayer) nel tuo account, esegui [<code>bx cs credentials-unset</code>](cs_cli_reference.html#cs_credentials_unset) per rimuovere le credenziali dal tuo account.</li></ol></td>
  </tr>
    </tbody>
  </table>



<br />




## Debug delle distribuzioni dell'applicazione
{: #debug_apps}

Rivedi le opzioni di cui disponi per il debug delle tue distribuzioni dell'applicazione e per trovare le cause principali degli errori.

1. Ricerca anomalie nelle risorse di distribuzione o nel servizio eseguendo il comando `describe`.

 Esempio:
 <pre class="pre"><code>kubectl describe service &lt;service_name&gt; </code></pre>

2. [Verifica se i contenitori sono bloccati nello stato ContainerCreating](cs_troubleshoot_storage.html#stuck_creating_state). 

3. Verifica se il cluster è nello stato `Critico`. Se il cluster è in uno stato `Critico`, controlla le regole del firewall e verifica che il master possa comunicare con i nodi di lavoro.

4. Verifica che il servizio sia in ascolto sulla porta corretta.
   1. Richiama il nome del pod.
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. Accedi al contenitore.
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   3. Esegui curl per l'applicazione dall'interno del contenitore. Se la porta non è accessibile, il servizio potrebbe non essere in ascolto sulla porta corretta o l'applicazione potrebbe avere dei problemi. Aggiorna il file di configurazione per il servizio con la porta corretta e ridistribuisci o investiga sui problemi potenziali dell'applicazione.
     <pre class="pre"><code>curl localhost: &lt;port&gt;</code></pre>

5. Verifica che il servizio sia collegato correttamente ai pod.
   1. Richiama il nome del pod.
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. Accedi al contenitore.
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   3. Esegui curl per la porta e l'indirizzo IP del cluster del servizio. Se la porta o l'indirizzo IP non sono accessibili, ricerca gli endpoint del servizio. Se non vengono elencati degli endpoint, il selettore del servizio non corrisponde ai pod. Se vengono elencati degli endpoint, ricerca il campo della porta di destinazione nel servizio e assicurati che sia la stessa di quella utilizzata dai pod.
     <pre class="pre"><code>curl &lt;cluster_IP&gt;:&lt;port&gt;</code></pre>

6. Per i servizi Ingress, verifica che il servizio sia accessibile dall'interno del cluster.
   1. Richiama il nome del pod.
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. Accedi al contenitore.
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   2. Esegui curl dell'URL specificato per il servizio Ingress. Se l'URL non è accessibile, controlla se è presente un errore con il firewall tra il cluster e l'endpoint esterno. 
     <pre class="pre"><code>curl &lt;host_name&gt;.&lt;domain&gt;</code></pre>

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

