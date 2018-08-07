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



# Risoluzione dei problemi di registrazione e di monitoraggio
{: #cs_troubleshoot_health}

Mentre utilizzi {{site.data.keyword.containerlong}}, tieni presente queste tecniche per la risoluzione dei problemi con la registrazione e il monitoraggio.
{: shortdesc}

Se hai un problema più generale, prova a [eseguire il debug del cluster](cs_troubleshoot.html).
{: tip}

## I log non vengono visualizzati
{: #cs_no_logs}

{: tsSymptoms}
Quando accedi al dashboard Kibana, i tuoi log non vengono visualizzati.

{: tsResolve}
Controlla i seguenti motivi sul perché i tuoi log del cluster non vengono visualizzati e la procedura di risoluzione del problema corrispondente:

<table>
<caption>Risoluzione dei problemi con i log che non vengono visualizzati</caption>
  <col width="40%">
  <col width="60%">
  <thead>
    <tr>
      <th>Perché sta succedendo</th>
      <th>Come porvi rimedio</th>
    </tr>
 </thead>
 <tbody>
  <tr>
    <td>Non è impostata alcuna configurazione di registrazione.</td>
    <td>Per poter inviare i log, devi creare una configurazione di registrazione. Per eseguire tale operazione, vedi <a href="cs_health.html#logging">Configurazione della registrazione cluster</a>.</td>
  </tr>
  <tr>
    <td>Il cluster non è in uno stato <code>Normale</code>.</td>
    <td>Per controllare lo stato del tuo cluster, vedi <a href="cs_troubleshoot.html#debug_clusters">Debug dei cluster</a>.</td>
  </tr>
  <tr>
    <td>La quota di archiviazione dei log è stata raggiunta. </td>
    <td>Per aumentare i limiti della tua archiviazione di log, consulta la <a href="/docs/services/CloudLogAnalysis/troubleshooting/error_msgs.html">documentazione {{site.data.keyword.loganalysislong_notm}}</a>.</td>
  </tr>
  <tr>
    <td>Se hai specificato uno spazio durante la creazione del cluster, il proprietario dell'account non dispone delle autorizzazioni di Gestore, Sviluppatore o Revisore in quello spazio.</td>
      <td>Per modificare le autorizzazioni di accesso per il proprietario dell'account:
      <ol><li>Per trovare chi è il proprietario dell'account del cluster, esegui <code>bx cs api-key-info &lt;cluster_name_or_ID&gt;</code>.</li>
      <li>Per concedere a tale proprietario dell'account le autorizzazioni di accesso allo spazio di gestore, sviluppatore o revisore {{site.data.keyword.containershort_notm}}, consulta <a href="cs_users.html">Gestione dell'accesso al cluster</a>.</li>
      <li>Per aggiornare il token di accesso dopo che sono state modificate le autorizzazioni, esegui <code>bx cs logging-config-refresh &lt;cluster_name_or_ID&gt;</code>.</li></ol></td>
    </tr>
    <tr>
      <td>Hai una configurazione di registrazione dell'applicazione con un symlink nel tuo percorso dell'applicazione.</td>
      <td><p>Per poter inviare i log, devi utilizzare un percorso assoluto nella tua configurazione della registrazione altrimenti i log non potranno essere letti. Se il tuo percorso viene montato nel tuo nodo di lavoro, potresti aver creato un symlink.</p> <p>Esempio: se il percorso specificato è <code>/usr/local/<b>spark</b>/work/app-0546/0/stderr</code> ma vanno in <code>/usr/local/<b>spark-1.0-hadoop-1.2</b>/work/app-0546/0/stderr</code>, i log non potranno essere letti. </p></td>
    </tr>
  </tbody>
</table>

Per verificare le modifiche che hai apportato durante la risoluzione dei problemi, puoi distribuire *Noisy*, un pod di esempio che produce diversi eventi di log, su un nodo di lavoro nel tuo cluster.

  1. [Indirizza la tua CLI](cs_cli_install.html#cs_cli_configure) a un cluster in cui vuoi iniziare a produrre i log.

  2. Crea il file di configurazione `deploy-noisy.yaml`.

      ```
      apiVersion: v1
      kind: Pod
      metadata:
        name: noisy
      spec:
        containers:
        - name: noisy
          image: ubuntu:16.04
          command: ["/bin/sh"]
          args: ["-c", "while true; do sleep 10; echo 'Hello world!'; done"]
          imagePullPolicy: "Always"
        ```
        {: codeblock}

  3. Esegui il file di configurazione nel contesto del cluster.

        ```
        kubectl apply -f noisy.yaml
        ```
        {:pre}

  4. Dopo alcuni minuti, puoi visualizzare i tuoi log nel dashboard Kibana. Per accedere al dashboard Kibana, vai a uno dei seguenti URL e seleziona l'account {{site.data.keyword.Bluemix_notm}} in cui hai creato il cluster. Se hai specificato uno spazio durante la creazione del cluster, vai a tale spazio.
      - Stati Uniti Sud e Stati Uniti Est: https://logging.ng.bluemix.net
      - Regno Unito-Sud: https://logging.eu-gb.bluemix.net
      - EU-Central: https://logging.eu-fra.bluemix.net
      - AP-South: https://logging.au-syd.bluemix.net

<br />


## Il dashboard Kubernetes non visualizza i grafici di utilizzo
{: #cs_dashboard_graphs}

{: tsSymptoms}
Quando accedi al dashboard Kubernetes, i grafici di utilizzo non vengono visualizzati.

{: tsCauses}
A volte, dopo un aggiornamento del cluster o un riavvio del nodo di lavoro, il pod `kube-dashboard` non viene aggiornato.

{: tsResolve}
Elimina il pod `kube-dashboard` per forzare un riavvio. Il pod viene ricreato con le politiche RBAC in modo da accedere all'heapster per le informazioni di utilizzo.

  ```
  kubectl delete pod -n kube-system $(kubectl get pod -n kube-system --selector=k8s-app=kubernetes-dashboard -o jsonpath='{.items..metadata.name}')
  ```
  {: pre}

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

