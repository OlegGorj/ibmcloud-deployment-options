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



# Perché {{site.data.keyword.containerlong_notm}}
{: #cs_ov}

{{site.data.keyword.containerlong}} offre potenti strumenti combinando le tecnologie Docker e Kubernetes, un'esperienza utente intuitiva e la sicurezza e l'isolamento integrati per automatizzare la distribuzione, il funzionamento, il ridimensionamento e il monitoraggio di applicazioni caricate nei contenitori in un cluster di host di calcolo.
{:shortdesc}


## Vantaggi dell'utilizzo del servizio
{: #benefits}

I cluster vengono distribuiti su host di calcolo che forniscono funzionalità Kubernetes native e funzionalità specifiche di {{site.data.keyword.IBM_notm}}.
{:shortdesc}

|Vantaggi|Descrizione|
|-------|-----------|
|Cluster Kubernetes a singolo tenant con calcolo, rete e isolamento dell'infrastruttura di archiviazione|<ul><li>Crea la tua propria infrastruttura personalizzata che soddisfi i requisiti della tua organizzazione.</li><li>Esegui il provisioning di un master Kubernetes sicuro e dedicato, dei nodi di lavoro, delle reti virtuali e dell'archiviazione utilizzando le risorse fornite dall'infrastruttura IBM Cloud (SoftLayer).</li><li>Master Kubernetes completamente gestito e continuamente monitorato da {{site.data.keyword.IBM_notm}} per mantenere il tuo cluster disponibile.</li><li>Opzione per il provisioning dei nodi di lavoro come server bare metal con Trusted Compute.</li><li>Archivia i dati persistenti, i dati condivisi tra i pod Kubernetes e ripristinali quando necessario
con il servizio del volume protetto e integrato.</li><li>Vantaggi del supporto completo per le API Kubernetes native.</li></ul>|
|Conformità della sicurezza dell'immagine con il Controllo vulnerabilità|<ul><li>Configura il tuo registro delle immagini privato Docker in cui le immagini vengono archiviate e condivise da tutti gli utenti
nell'organizzazione.</li><li>Vantaggi dalla scansione automatica delle immagini nel tuo registro {{site.data.keyword.Bluemix_notm}} privato.</li><li>Rivedi la raccomandazioni specifiche del sistema operativo utilizzato nell'immagine per risolvere le vulnerabilità potenziali.</li></ul>|
|Monitoraggio continuo dell'integrità del cluster|<ul><li>Utilizza il dashboard del cluster per visualizzare e gestire rapidamente l'integrità del tuo cluster, dei nodi di lavoro e delle distribuzioni del contenitore.</li><li>Trova le metriche di consumo dettagliate utilizzando {{site.data.keyword.monitoringlong}} ed
espandi velocemente il tuo cluster per soddisfare i carichi di lavoro.</li><li>Esamina le informazioni di registrazione utilizzando {{site.data.keyword.loganalysislong}} per visualizzare le attività del cluster dettagliate.</li></ul>|
|Esposizione protetta delle applicazioni al pubblico|<ul><li>Scegli tra un indirizzo IP pubblico, una rotta fornita da {{site.data.keyword.IBM_notm}} o il tuo proprio dominio personale per accedere
ai servizi nel tuo cluster da internet.</li></ul>|
|Integrazione servizio {{site.data.keyword.Bluemix_notm}}|<ul><li>Aggiungi ulteriori funzionalità alla tua applicazione tramite l'integrazione dei servizi {{site.data.keyword.Bluemix_notm}}, come le API Watson, Blockchain, i servizi dati o Internet of Things.</li></ul>|
{: caption="Vantaggi di {{site.data.keyword.containerlong_notm}} " caption-side="top"}



<br />


## Confronto delle offerte e le loro combinazioni
{: #differentiation}

Puoi eseguire {{site.data.keyword.containershort_notm}} in {{site.data.keyword.Bluemix_notm}} Public o Dedicated, in {{site.data.keyword.Bluemix_notm}} Private o in una configurazione ibrida.
{:shortdesc}


<table>
<caption>Differenze tra le configurazioni {{site.data.keyword.containershort_notm}} </caption>
<col width="22%">
<col width="78%">
 <thead>
 <th>Configurazione di {{site.data.keyword.containershort_notm}}</th>
 <th>Descrizione</th>
 </thead>
 <tbody>
 <tr>
 <td>{{site.data.keyword.Bluemix_notm}} Public
 </td>
 <td>Con {{site.data.keyword.Bluemix_notm}} Public sull'[hardware condiviso o dedicato o sulle macchine bare metal](cs_clusters.html#shared_dedicated_node), puoi ospitare le tue applicazioni nei cluster sul cloud utilizzando {{site.data.keyword.containershort_notm}}. {{site.data.keyword.containershort_notm}} su {{site.data.keyword.Bluemix_notm}} Public offre potenti strumenti combinando le tecnologie Docker e Kubernetes, un'esperienza utente intuitiva e la sicurezza e l'isolamento integrati per automatizzare la distribuzione, il funzionamento, il ridimensionamento e il monitoraggio di applicazioni caricate nei contenitori in un cluster di host di calcolo.<br><br>Per ulteriori informazioni, vedi la [tecnologia {{site.data.keyword.containershort_notm}}](cs_tech.html).
 </td>
 </tr>
 <tr>
 <td>{{site.data.keyword.Bluemix_notm}} Dedicated
 </td>
 <td>{{site.data.keyword.Bluemix_notm}} Dedicated offre le stesse capacità {{site.data.keyword.containershort_notm}} sul cloud di {{site.data.keyword.Bluemix_notm}} Public. Tuttavia, con un account {{site.data.keyword.Bluemix_notm}} Dedicated, le [risorse fisiche disponibili sono dedicate solo al tuo cluster](cs_clusters.html#shared_dedicated_node) e non vengono condivise con i cluster provenienti dagli altri clienti {{site.data.keyword.IBM_notm}}. Potresti scegliere di impostare un ambiente {{site.data.keyword.Bluemix_notm}} Dedicated quando richiedi l'isolamento per il tuo cluster e gli altri servizi {{site.data.keyword.Bluemix_notm}} che utilizzi.<br><br>Per ulteriori informazioni, vedi [Introduzione ai cluster in {{site.data.keyword.Bluemix_notm}} Dedicated](cs_dedicated.html#dedicated).
 </td>
 </tr>
 <tr>
 <td>{{site.data.keyword.Bluemix_notm}} Private
 </td>
 <td>{{site.data.keyword.Bluemix_notm}} Private è una piattaforma applicativa che può essere installata localmente sulle tue macchine. Puoi scegliere di utilizzare {{site.data.keyword.containershort_notm}} in {{site.data.keyword.Bluemix_notm}} Private quando hai bisogno di sviluppare applicazioni inserite nel contenitore in loco nel tuo ambiente controllato dietro un firewall. <br><br>Per ulteriori informazioni, vedi le [informazioni sul prodotto privato {{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/cloud/private) e la documentazione [ ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_1.2.0/kc_welcome_containers.html).
 </td>
 </tr>
 <tr>
 <td>Configurazione ibrida
 </td>
 <td>La configurazione ibrida consiste nell'uso combinato dei servizi che vengono eseguiti in {{site.data.keyword.Bluemix_notm}} Public o Dedicated e altri servizi che vengono eseguiti in loco, come ad esempio un'applicazione in {{site.data.keyword.Bluemix_notm}} Private. Esempi per una configurazione ibrida: <ul><li>Eseguire il provisioning di un cluster con {{site.data.keyword.containershort_notm}} in {{site.data.keyword.Bluemix_notm}} Public ma connettendo tale cluster a un database in loco.</li><li>Esecuzione del provisioning di un cluster con {{site.data.keyword.containershort_notm}} in {{site.data.keyword.Bluemix_notm}} Private e distribuzione di un'applicazione in tale cluster. Tuttavia, questa applicazione potrebbe utilizzare un servizio {{site.data.keyword.ibmwatson}}, ad esempio {{site.data.keyword.toneanalyzershort}}, in {{site.data.keyword.Bluemix_notm}} Public.</li></ul><br>Per abilitare la comunicazione tra i servizi che sono in esecuzione in {{site.data.keyword.Bluemix_notm}} Public o Dedicated e i servizi che sono in esecuzione in loco, devi [configurare una connessione VPN](cs_vpn.html).
 </td>
 </tr>
 </tbody>
</table>

<br />


## Confronto tra i cluster standard e gratuito
{: #cluster_types}

Puoi creare un cluster gratuito o un qualsiasi numero di cluster standard. Prova i cluster gratuiti per acquisire familiarità e testare alcune funzionalità di Kubernetes o crea i cluster standard per utilizzare le funzionalità complete di Kubernetes per distribuire le applicazioni.
{:shortdesc}

|Caratteristiche|Cluster gratuiti|Cluster standard|
|---------------|-------------|-----------------|
|[Rete in cluster](cs_secure.html#in_cluster_network)
|[Archiviazione basata sul file NFS persistente con i volumi](cs_storage.html#planning)| |<img src="images/confirm.svg" width="32" alt="Funzione disponibile" style="width:32px;" />|
|[Accesso dell'applicazione della rete pubblica o privata da un servizio di bilanciamento del carico a un indirizzo IP stabile](cs_loadbalancer.html#planning)| |<img src="images/confirm.svg" width="32" alt="Funzione disponibile" style="width:32px;" />|
|[Accesso dell'applicazione della rete pubblica da un servizio Ingress a un indirizzo IP stabile e URL personalizzabile](cs_ingress.html#planning)| |<img src="images/confirm.svg" width="32" alt="Funzione disponibile" style="width:32px;" />|
|[Indirizzi IP pubblici portatili](cs_subnets.html#manage)| |<img src="images/confirm.svg" width="32" alt="Funzione disponibile" style="width:32px;" />|
|[Registrazione e monitoraggio](cs_health.html#logging)| |<img src="images/confirm.svg" width="32" alt="Funzione disponibile" style="width:32px;" />|
|[Opzione per il provisioning dei nodi di lavoro su server fisici (bare metal)](cs_clusters.html#shared_dedicated_node) | |<img src="images/confirm.svg" width="32" alt="Funzione disponibile" style="width:32px;" />|
|[Opzione per il provisioning dei nodi di lavoro bare metal con Trusted Compute](cs_clusters.html#shared_dedicated_node) | |<img src="images/confirm.svg" width="32" alt="Funzione disponibile" style="width:32px;" />|
|[Disponibile in {{site.data.keyword.Bluemix_dedicated_notm}}](cs_dedicated.html#dedicated_environment)| |<img src="images/confirm.svg" width="32" alt="Funzione disponibile" style="width:32px;" />|
{: caption="Caratteristiche dei cluster standard e gratuito" caption-side="top"}

<br />




{: #responsibilities}
**Nota**: cerchi le tue responsabilità e i termini {{site.data.keyword.containerlong}} quando utilizzi il servizio?

{: #terms}
Vedi [Responsabilità di {{site.data.keyword.containershort_notm}}](cs_responsibilities.html).
