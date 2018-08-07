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



# Le tue responsabilità nell'utilizzo di {{site.data.keyword.containerlong_notm}}
Ulteriori informazioni sulle responsabilità di gestione del cluster e sui termini e le condizioni derivanti dall'utilizzo di {{site.data.keyword.containerlong}}.
{:shortdesc}

## Responsabilità di gestione del cluster
{: #responsibilities}

Controlla le responsabilità che condividi con IBM per gestire i tuoi cluster.
{:shortdesc}

**IBM è responsabile per:**

- Distribuzione del master, dei nodi di lavoro e dei componenti di gestione all'interno del cluster, come il programma di bilanciamento del carico dell'applicazione Ingress, al momento della creazione del cluster
- Gestione degli aggiornamenti di sicurezza, del monitoraggio, dell'isolamento e del ripristino del master Kubernetes per il cluster 
- Monitoraggio dell'integrità dei nodi di lavoro e la fornitura dell'automazione dell'aggiornamento e del ripristino di tali nodi di lavoro
- Esecuzione delle attività di automazione nel tuo account dell'infrastruttura, inclusi l'aggiunta e la rimozione dei nodi di lavoro e la creazione di una sottorete predefinita
- Gestione, aggiornamento e ripristino dei componenti operativi all'interno del cluster, come il programma di bilanciamento del carico dell'applicazione Ingress e il plug-in di archiviazione
- Provisioning di volumi di archiviazione quando richiesto dalle attestazioni del volume persistente
- Fornitura delle impostazioni di sicurezza per tutti i nodi di lavoro

</br>

**Tu sei responsabile per:**

- [Configurazione del tuo account {{site.data.keyword.Bluemix_notm}} per accedere al portfolio dell'infrastruttura IBM Cloud (SoftLayer)](cs_troubleshoot_clusters.html#cs_credentials)
- [Distribuzione e gestione delle risorse Kubernetes, come i pod, i servizi e le distribuzioni, all'interno del cluster](cs_app.html#app_cli)
- [Utilizzo delle funzionalità del servizio e di Kubernetes per garantire l'elevata disponibilità alle applicazioni](cs_app.html#highly_available_apps)
- [Aggiunta o rimozione della capacità del cluster di utilizzare la CLI per aggiungere o rimuovere i nodi di lavoro](cs_cli_reference.html#cs_worker_add)
- [Creazione delle VLAN pubblica e privata nell'infrastruttura IBM Cloud (SoftLayer) per l'isolamento di rete del tuo cluster](/docs/infrastructure/vlans/getting-started.html#getting-started-with-vlans)
- [Verifica che tutti i nodi di lavoro dispongano di connettività di rete all'URL del master Kubernetes](cs_firewall.html#firewall) <p>**Nota**: se un nodo di lavoro ha sia una VLAN pubblica che privata, viene configurata la connettività di rete. Se i nodi di lavoro sono impostati solo con una VLAN privata, devi configurare una soluzione alternativa per la connettività di rete. Per ulteriori informazioni, vedi [Connessione VLAN per i nodi di lavoro](cs_clusters.html#worker_vlan_connection). </p>
- [Aggiornamento del kube-apiserver master quando sono disponibili aggiornamenti alla versione di Kubernetes](cs_cluster_update.html#master)
- [Mantenere i nodi di lavoro aggiornati alle versioni patch, principale e secondaria](cs_cluster_update.html#worker_node)
- [Ripristino dei nodi di lavoro con problemi eseguendo i comandi `kubectl`, come `cordon` o `drain` ed eseguendo i comandi `bx cs`, come `reboot`, `reload` o `delete`](cs_cli_reference.html#cs_worker_reboot)
- [Aggiunta o rimozione delle sottoreti nell'infrastruttura IBM Cloud (SoftLayer) secondo necessità](cs_subnets.html#subnets)
- [Backup e ripristino dei dati nell'archiviazione persistente nell'infrastruttura IBM Cloud (SoftLayer) ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](../services/RegistryImages/ibm-backup-restore/index.html)
- [Configurazione del monitoraggio dell'integrità per i nodi di lavoro con Autorecovery](cs_health.html#autorecovery)

<br />


## Abuso di {{site.data.keyword.containerlong_notm}}
{: #terms}

I clienti non possono usare impropriamente il {{site.data.keyword.containershort_notm}}.
{:shortdesc}

L'utilizzo improprio include:

*   Qualsiasi attività illegale
*   La distribuzione o l'esecuzione di malware
*   Il danneggiamento di {{site.data.keyword.containershort_notm}} o l'interferenza
con l'utilizzo di {{site.data.keyword.containershort_notm}}
*   Il danneggiamento o l'interferenza con l'utilizzo di qualsiasi altro servizio o sistema
*   L'accesso non autorizzato a qualsiasi servizio o sistema
*   La modifica non autorizzata a qualsiasi servizio o sistema
*   La violazione dei diritti altrui


Per i termini generali di utilizzo, consulta [Termini dei
servizi cloud](https://console.bluemix.net/docs/overview/terms-of-use/notices.html#terms).
