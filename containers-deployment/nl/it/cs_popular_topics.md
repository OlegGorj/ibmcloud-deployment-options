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




# Argomenti popolari di {{site.data.keyword.containershort_notm}}
{: #cs_popular_topics}

Resta al passo con cosa succede in {{site.data.keyword.containerlong}}. Ulteriori informazioni sulle nuove funzioni da esplorare, su un suggerimento per la sperimentazione o su alcuni argomenti popolari che gli altri sviluppatori stanno trovando utili al momento.
{:shortdesc}

## Argomenti popolari nel maggio 2018
{: #may18}


<table summary="La tabella mostra gli argomenti popolari. Le righe devono essere lette da sinistra a destra, con la data nella colonna uno, il titolo della funzione nella colonna due e una descrizione nella colonna tre.">
<caption>Argomenti popolari per i contenitori e i cluster Kubernetes nel maggio 2018 </caption>
<thead>
<th>Data</th>
<th>Titolo</th>
<th>Descrizione</th>
</thead>
<tbody>
<tr>
<td>24 maggio</td>
<td>[Nuovo formato del dominio secondario Ingress](cs_ingress.html)</td>
<td>I cluster creati dopo il 24 maggio vengono assegnati a un dominio secondario Ingress in un nuovo formato: <code>&lt;cluster_name&gt;.&lt;region&gt;.containers.appdomain.cloud</code>. Quando utilizzi Ingress per esporre le tue applicazioni, puoi utilizzare il nuovo dominio secondario per accedere alle tue applicazioni da internet.</td>
</tr>
<tr>
<td>14 maggio</td>
<td>[Aggiornamento: distribuisci i carichi di lavoro nel bare metal GPU nel mondo](cs_app.html#gpu_app)</td>
<td>Se hai un [tipo di macchina GPU (graphics processing unit) bare metal](cs_clusters.html#shared_dedicated_node) nel tuo cluster, puoi pianificare le applicazioni intensive in modo matematico. Il nodo di lavoro GPU può elaborare il carico di lavoro della tua applicazione tramite la CPU e la GPU per aumentare le prestazioni.</td>
</tr>
<tr>
<td>03 maggio</td>
<td>[Container Image Security Enforcement (beta)](/docs/services/Registry/registry_security_enforce.html#security_enforce)</td>
<td>Il tuo team ha bisogno di un piccolo ulteriore aiuto per sapere quale immagine trasmettere nei tuoi contenitori dell'applicazione? Prova Container Image Security Enforcement beta per verificare le immagini del contenitore prima di distribuirle. Disponibile per i cluster che eseguono Kubernetes 1.9 o successiva.</td>
</tr>
<tr>
<td>01 maggio</td>
<td>[Distribuisci il dashboard Kubernetes dalla GUI](cs_app.html#cli_dashboard)</td>
<td>Hai mai voluto accedere al dashboard Kubernetes con un clic? Controlla il pulsante **Dashboard Kubernetes** nella GUI {{site.data.keyword.Bluemix_notm}}.</td>
</tr>
</tbody></table>




## Argomenti popolari nell'aprile 2018
{: #apr18}

<table summary="La tabella mostra gli argomenti popolari. Le righe devono essere lette da sinistra a destra, con la data nella colonna uno, il titolo della funzione nella colonna due e una descrizione nella colonna tre.">
<caption>Argomenti popolari per i contenitori e i cluster Kubernetes  nell'aprile 2018</caption>
<thead>
<th>Data</th>
<th>Titolo</th>
<th>Descrizione</th>
</thead>
<tbody>
<tr>
<td>17 aprile</td>
<td>{{site.data.keyword.Bluemix_notm}} Block Storage</td>
<td>Installa il [plug-in](cs_storage.html#install_block) di {{site.data.keyword.Bluemix_notm}} Block Storage per salvare i dati persistenti nell'archiviazione blocchi. Quindi puoi [creare una nuova](cs_storage.html#create) archiviazione blocchi o [utilizzarne una esistente](cs_storage.html#existing_block) per il tuo cluster.</td>
</tr>
<tr>
<td>13 aprile</td>
<td>[Nuova esercitazione per la migrazione delle applicazioni Cloud Foundry nei cluster](cs_tutorials_cf.html#cf_tutorial)</td>
<td>Hai un'applicazione Cloud Foundry? Impara a distribuire lo stesso codice proveniente da tale applicazione ad un'applicazione che viene eseguita in un cluster Kubernetes.</td>
</tr>
<tr>
<td>5 aprile</td>
<td>[Filtraggio dei log](cs_health.html#filter-logs)</td>
<td>Filtra specifici log per impedire che vengano inoltrati. I log possono essere filtrati per uno specifico spazio dei nomi, nome contenitore, livello di log e stringa di messaggio.</td>
</tr>
</tbody></table>

## Argomenti popolari nel marzo 2018
{: #mar18}

<table summary="La tabella mostra gli argomenti popolari. Le righe devono essere lette da sinistra a destra, con la data nella colonna uno, il titolo della funzione nella colonna due e una descrizione nella colonna tre.">
<caption>Argomenti popolari per i contenitori e i cluster Kubernetes nel marzo 2018</caption>
<thead>
<th>Data</th>
<th>Titolo</th>
<th>Descrizione</th>
</thead>
<tbody>
<tr>
<td>16 marzo</td>
<td>[Provisioning di un cluster bare metal con Trusted Compute](cs_clusters.html#shared_dedicated_node)</td>
<td>Crea un cluster bare metal che esegue [Kubernetes versione 1.9](cs_versions.html#cs_v19) o successiva e abilita Trusted Compute per verificare possibili tentativi di intrusione nei tuoi nodi di lavoro.</td>
</tr>
<tr>
<td>14 marzo</td>
<td>[Accesso sicuro con {{site.data.keyword.appid_full}}](cs_integrations.html#appid)</td>
<td>Migliora le tue applicazioni in esecuzione in {{site.data.keyword.containershort_notm}} richiedendo agli utenti di effettuare l'accesso.</td>
</tr>
<tr>
<td>13 marzo</td>
<td>[Ubicazione disponibile a San Paolo](cs_regions.html)</td>
<td>San Paolo, in Brasile, è la nuova ubicazione nella regione Stati Uniti Sud. Se hai un firewall, assicurati di [aprire le porte del firewall](cs_firewall.html#firewall) per questa ubicazione e le altre all'interno della regione in cui si trova il tuo cluster.</td>
</tr>
<tr>
<td>12 marzo</td>
<td>[Ti sei appena registrato a {{site.data.keyword.Bluemix_notm}} con un account di prova? Prova un cluster Kubernetes gratuito!](container_index.html#clusters)</td>
<td>Con un [account {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/registration/) di prova, puoi distribuire un cluster gratuito per 21 giorni per testare le funzionalità di Kubernetes.</td>
</tr>
</tbody></table>

## Argomenti popolari nel febbraio 2018
{: #feb18}

<table summary="La tabella mostra gli argomenti popolari. Le righe devono essere lette da sinistra a destra, con la data nella colonna uno, il titolo della funzione nella colonna due e una descrizione nella colonna tre.">
<caption>Argomenti popolari per i contenitori e i cluster Kubernetes nel febbraio 2018</caption>
<thead>
<th>Data</th>
<th>Titolo</th>
<th>Descrizione</th>
</thead>
<tbody>
<tr>
<td>27 febbraio</td>
<td>Immagini HVM (hardware virtual machine) per i nodi di lavoro</td>
<td>Aumenta le prestazioni I/O dei tuoi carichi di lavoro con le immagini HVM. Attivale su ogni nodo di lavoro esistente utilizzando il [comando](cs_cli_reference.html#cs_worker_reload) `bx cs worker-reload` o il [comando](cs_cli_reference.html#cs_worker_update) `bx cs worker-update`.</td>
</tr>
<tr>
<td>26 febbraio</td>
<td>[Ridimensionamento automatico di KubeDNS](https://kubernetes.io/docs/tasks/administer-cluster/dns-horizontal-autoscaling/)</td>
<td>KubeDNS adesso si ridimensiona con l'espandersi del tuo cluster. Puoi regolare le razioni di ridimensionamento utilizzando il seguente comando: `kubectl -n kube-system edit cm kube-dns-autoscaler`.</td>
</tr>
<tr>
<td>23 febbraio</td>
<td>Visualizza l'IU web per la [registrazione](cs_health.html#view_logs) e le [metriche](cs_health.html#view_metrics)</td>
<td>Visualizza facilmente dati di log e metriche sul tuo cluster e i suoi componenti con un'IU web migliorata. Per l'accesso, vedi la pagina dei dettagli del tuo cluster.</td>
</tr>
<tr>
<td>20 febbraio</td>
<td>Immagini crittografate e [contenuti firmati e attendibili](../services/Registry/registry_trusted_content.html#registry_trustedcontent)</td>
<td>In {{site.data.keyword.registryshort_notm}}, puoi firmare e crittografare le immagini per garantirne l'integrità durante la memorizzazione nello spazio dei nomi del tuo registro. Esegui le tue istanze del contenitore utilizzando solo contenuti attendibili. </td>
</tr>
<tr>
<td>19 febbraio</td>
<td>[Configura la VPN IPSec strongSwan](cs_vpn.html#vpn-setup)</td>
<td>Distribuisci rapidamente il grafico Helm della VPN IPSec strongSwan per connettere in modo sicuro il cluster {{site.data.keyword.containershort_notm}} al tuo data center in loco senza un VRA (Virtual Router Appliance).</td>
</tr>
<tr>
<td>14 febbraio</td>
<td>[Ubicazione disponibile a Seoul](cs_regions.html)</td>
<td>Giusto in tempo per le Olimpiadi, distribuisci un cluster Kubernetes a Seoul nella regione Asia Pacifico Nord. Se hai un firewall, assicurati di [aprire le porte del firewall](cs_firewall.html#firewall) per questa ubicazione e le altre all'interno della regione in cui si trova il tuo cluster.</td>
</tr>
<tr>
<td>8 febbraio</td>
<td>[Aggiorna Kubernetes 1.9](cs_versions.html#cs_v19)</td>
<td>Rivedi le modifiche da apportare ai tuoi cluster prima di aggiornare a Kubernetes 1.9. </td>
</tr>
</tbody></table>

## Argomenti popolari nel gennaio 2018
{: #jan18}

<table summary="La tabella mostra gli argomenti popolari. Le righe devono essere lette da sinistra a destra, con la data nella colonna uno, il titolo della funzione nella colonna due e una descrizione nella colonna tre.">
<caption>Argomenti popolari per i contenitori e i cluster Kubernetes nel gennaio 2018</caption>
<thead>
<th>Data</th>
<th>Titolo</th>
<th>Descrizione</th>
</thead>
<tbody>
<td>25 gennaio</td>
<td>[Registro globale disponibile](../services/Registry/registry_overview.html#registry_regions)</td>
<td>Con il {{site.data.keyword.registryshort_notm}}, puoi utilizzare il registro globale `registry.bluemix.net` per estrarre le immagini pubbliche fornite da IBM.</td>
</tr>
<tr>
<td>23 gennaio</td>
<td>[Ubicazioni disponibili a Singapore e Montreal, CA](cs_regions.html)</td>
<td>Singapore e Montreal sono le ubicazioni disponibili nelle regioni Asia Pacifico Nord e Stati Uniti Est di {{site.data.keyword.containershort_notm}}. Se hai un firewall, assicurati di [aprire le porte del firewall](cs_firewall.html#firewall) per queste ubicazioni e le altre all'interno della regione in cui si trova il tuo cluster.</td>
</tr>
<tr>
<td>8 gennaio</td>
<td>[Disponibili caratteristiche avanzate](cs_cli_reference.html#cs_machine_types)</td>
<td>I tipi di macchine virtuali della serie 2 includono l'archiviazione SSD locale e la crittografia del disco. [Sposta i tuoi carichi di lavoro](cs_cluster_update.html#machine_type) a queste caratteristiche per migliorare prestazioni e stabilità.</td>
</tr>
</tbody></table>

## Utilizza la chat per parlare con sviluppatori con interessi simili su Slack
{: #slack}

Puoi vedere di cosa parlano gli altri utenti e porre le tue domande in [{{site.data.keyword.containershort_notm}} Slack. ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://ibm-container-service.slack.com)
{:shortdesc}

Se non stai utilizzando un ID IBM per il tuo account {{site.data.keyword.Bluemix_notm}}, [richiedi un invito](https://bxcs-slack-invite.mybluemix.net/) a questo Slack.
{: tip}
