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



# Introduzione a {{site.data.keyword.containerlong_notm}}
{: #container_index}

Parti in quarta con {{site.data.keyword.containerlong}} distribuendo applicazioni ad alta disponibilità nei contenitori Docker eseguiti nei cluster Kubernetes.
{:shortdesc}

I contenitori sono un modo standard per assemblare applicazioni e tutte le sue dipendenze in modo da poter spostare facilmente le applicazioni tra gli ambienti. A differenza delle macchine virtuali, i contenitori non includono il sistema operativo. I contenitori includono solo il codice dell'applicazione, il runtime, gli strumenti di sistema, le librerie e le impostazioni. I contenitori sono più leggeri, portatili ed efficienti rispetto alle macchine virtuali.


Fai clic su una opzione per iniziare:

<img usemap="#home_map" border="0" class="image" id="image_ztx_crb_f1b" src="images/cs_public_dedicated_options.png" width="440" alt="Fai clic su un'icona per iniziare ad utilizzare velocemente \{{site.data.keyword.containershort_notm}}. Con {{site.data.keyword.Bluemix_dedicated_notm}}, fai clic su questa icona per visualizzare le tue opzioni." style="width:440px;" />
<map name="home_map" id="home_map">
<area href="#clusters" alt="Introduzione ai cluster Kubernetes in {{site.data.keyword.Bluemix_notm}}" title="Introduzione ai cluster Kubernetes in {{site.data.keyword.Bluemix_notm}}" shape="rect" coords="-7, -8, 108, 211" />
<area href="cs_cli_install.html" alt="Installa le CLI." title="Installa le CLI." shape="rect" coords="155, -1, 289, 210" />
<area href="cs_dedicated.html#dedicated_environment" alt="{{site.data.keyword.Bluemix_dedicated_notm}} - ambiente cloud " title="{{site.data.keyword.Bluemix_notm}} - ambiente cloud" shape="rect" coords="326, -10, 448, 218" />
</map>


## Introduzione ai cluster
{: #clusters}

Vuoi distribuire un'applicazione in un contenitore? Aspetta! Crea prima un cluster Kubernetes. Kubernetes è uno strumento di orchestrazione per i contenitori. Con Kubernetes, gli sviluppatori possono distribuire in un attimo le applicazioni ad alta disponibilità sfruttando la potenza e la flessibilità dei cluster.
{:shortdesc}

E cos'è un cluster? Un cluster è un insieme di risorse, nodi di lavoro, reti e dispositivi di archiviazione che mantengono le applicazioni altamente disponibili. Dopo aver ottenuto il tuo cluster, puoi distribuire le tue applicazioni nei contenitori.

**Prima di iniziare**

Devi disporre di un [account {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/registration/) Di prova, Pagamento a consumo o Sottoscrizione.

Con un account di prova, puoi creare un cluster gratuito che puoi utilizzare per 21 giorni per prendere familiarità con il servizio. Con un account Pagamento a consumo o Sottoscrizione, puoi ancora creare un cluster di prova gratuito ma puoi anche fornire le risorse dell'infrastruttura IBM Cloud (SoftLayer) da utilizzare nei cluster standard.
{:tip}

Per creare un cluster gratuito:

1.  Nel [**Catalogo** {{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/catalog/?category=containers), seleziona **Contenitori nei cluster Kubernetes** e fai clic su **Crea**. Viene visualizzata una pagina di configurazione cluster. Per impostazione predefinita, è selezionato **Cluster gratuito**.

2. Fornisci al tuo cluster un nome univoco.

3.  Fai clic su **Crea cluster**. Viene creato un nodo di lavoro che può avere bisogno di alcuni minuti per il provisioning, ma puoi visualizzare l'andamento nella scheda **Nodi di lavoro**. Quando lo stato diventa `Pronto` puoi iniziare ad utilizzare il tuo cluster!

Ottimo lavoro! Hai creato il tuo primo cluster Kubernetes. Di seguito sono riportati alcuni dettagli sul tuo cluster gratuito:

*   **Tipo di macchina**: il cluster gratuito ha un nodo di lavoro virtuale con 2 CPU e 4 GB di memoria disponibili per le tue applicazioni. Quando crei un cluster standard, puoi scegliere tra macchine fisiche (bare metal) o virtuali, oltre a varie dimensioni della macchina.
*   **Master gestito**: il nodo di lavoro è monitorato e gestito centralmente da un master Kubernetes dedicato e altamente disponibile di proprietà di {{site.data.keyword.IBM_notm}} che controlla e monitora tutte le risorse Kubernetes presenti nel cluster. Puoi concentrarti sul tuo nodo di lavoro e sulle applicazioni distribuite nel nodo di lavoro senza preoccuparti di gestire anche questo master.
*   **Risorse dell'infrastruttura**: le risorse necessarie per eseguire il cluster, come VLAN e indirizzi IP, sono gestite in un account dell'infrastruttura IBM Cloud (SoftLayer) di proprietà di {{site.data.keyword.IBM_notm}}. Quando crei un cluster standard, gestisci queste risorse nel tuo proprio account dell'infrastruttura IBM Cloud (SoftLayer). Puoi scoprire di più su queste risorse e sulle [autorizzazioni necessarie](cs_users.html#infra_access) quando crei un cluster standard.
*   **Altre opzioni**: i cluster gratuiti vengono distribuiti nella regione da te selezionata, ma non puoi scegliere l'ubicazione (data center). Per avere il controllo sull'ubicazione, sulla rete e sull'archiviazione persistente, crea un cluster standard. [Scopri di più sui vantaggi dei cluster gratuiti e standard](cs_why.html#cluster_types).


**Operazioni successive**
Nei prossimi 21 giorni, prova ad eseguire alcune operazioni con il tuo cluster gratuito.

* [Installa le CLI per iniziare ad utilizzare il tuo cluster.](cs_cli_install.html#cs_cli_install)
* [Distribuisci un'applicazione nel tuo cluster.](cs_app.html#app_cli)
* [Crea un cluster standard con più nodi per una maggiore disponibilità.](cs_clusters.html#clusters_ui)
* [Configura un registro privato in {{site.data.keyword.Bluemix_notm}} per memorizzare e condividere le immagini Docker con altri utenti.](/docs/services/Registry/index.html)
