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


# Configurazione dei cluster
{: #clusters}

Progetta la configurazione del tuo cluster Kubernetes per la massima capacità e disponibilità dei contenitori con {{site.data.keyword.containerlong}}.
{:shortdesc}



## Pianificazione configurazione cluster
{: #planning_clusters}

Utilizza i cluster standard per aumentare la disponibilità dell'applicazione.
{:shortdesc}

I tuoi utenti hanno meno probabilità di
riscontrare tempi di inattività quando distribuisci la configurazione su più nodi di lavoro e cluster. Le funzionalità integrate, come il bilanciamento del carico e l'isolamento, aumentano la resilienza nei confronti di
potenziali errori con host, reti o applicazioni.

Rivedi la configurazione di questi cluster potenziali ordinati per gradi di disponibilità:

![Fasi di elevata disponibilità per un cluster](images/cs_cluster_ha_roadmap.png)

1.  Un cluster con più nodi di lavoro
2.  Due cluster in esecuzione in ubicazioni diverse nella stessa regione, ognuno con
più nodi di lavoro
3.  Due cluster in esecuzione in regioni diverse, ognuno con più nodi di lavoro


### Aumenta la disponibilità del tuo cluster 

<dl>
  <dt>Espandi le applicazioni tra i nodi di lavoro</dt>
    <dd>Consenti agli sviluppatori di estendere le loro applicazioni nei contenitori tra più nodi di lavoro per cluster. Un'istanza dell'applicazione in ognuno dei tre nodi di lavoro permette il tempo di inattività di un nodo di lavoro senza interrompere l'utilizzo dell'applicazione. Puoi specificare quanti nodi di lavoro da includere quando crei un cluster dalla GUI [{{site.data.keyword.Bluemix_notm}}](cs_clusters.html#clusters_ui) o dalla CLI [](cs_clusters.html#clusters_cli). Kubernetes limita il numero massimo di nodi di lavoro che puoi avere in un cluster, per cui tieni a mente [il nodo di lavoro e le quote pod ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/admin/cluster-large/).
    <pre class="codeblock"><code>bx cs cluster-create --location dal10 --workers 3 --public-vlan &lt;public_VLAN_ID&gt; --private-vlan &lt;private_VLAN_ID&gt; --machine-type &lt;u2c.2x4&gt; --name &lt;cluster_name_or_ID&gt;</code></pre></dd>
  <dt>Espandi le applicazioni tra i cluster</dt>
    <dd>Crea più cluster, ognuno con più nodi di lavoro. Se si verifica un'interruzione con un cluster,
gli utenti possono ancora accedere a un'applicazione che viene distribuita anche in un altro cluster.
      <p>Cluster
1:</p>
        <pre class="codeblock"><code>bx cs cluster-create --location dal10 --workers 3 --public-vlan &lt;public_VLAN_ID&gt; --private-vlan &lt;private_VLAN_ID&gt; --machine-type &lt;u2c.2x4&gt; --name &lt;cluster_name_or_ID&gt;</code></pre>
      <p>Cluster
2:</p>
        <pre class="codeblock"><code>bx cs cluster-create --location dal12 --workers 3 --public-vlan &lt;public_VLAN_ID&gt; --private-vlan &lt;private_VLAN_ID&gt; --machine-type &lt;u2c.2x4&gt; --name &lt;cluster_name_or_ID&gt;</code></pre></dd>
  <dt>Espandi le applicazioni tra i cluster in regioni differenti</dt>
    <dd>Quando espandi le applicazioni tra i cluster in regioni differenti puoi permettere il bilanciamento del carico basato sulla regione in cui è l'utente. Se il cluster, l'hardware o anche un'intera ubicazione in una regione vengono disattivati, il traffico viene instradato al contenitore distribuito in un'altra ubicazione.
      <p><strong>Importante:</strong> dopo aver configurato il dominio personalizzato, puoi utilizzare questi comandi per creare i cluster.</p>
      <p>Ubicazione
1:</p>
        <pre class="codeblock"><code>bx cs cluster-create --location dal10 --workers 3 --public-vlan &lt;public_VLAN_ID&gt; --private-vlan &lt;private_VLAN_ID&gt; --machine-type &lt;u2c.2x4&gt; --name &lt;cluster_name_or_ID&gt;</code></pre>
      <p>Ubicazione
2:</p>
        <pre class="codeblock"><code>bx cs cluster-create --location ams03 --workers 3 --public-vlan &lt;public_VLAN_ID&gt; --private-vlan &lt;private_VLAN_ID&gt; --machine-type &lt;u2c.2x4&gt; --name &lt;cluster_name_or_ID&gt;</code></pre></dd>
</dl>

<br />







## Pianificazione configurazione del nodo di lavoro
{: #planning_worker_nodes}

Un cluster Kubernetes è costituito da nodi di lavoro ed è monitorato e gestito centralmente dal master Kubernetes. Gli amministratori del cluster decidono come configurare il cluster dei nodi di lavoro per garantire che gli utenti del cluster abbiano tutte le risorse per distribuire ed eseguire le applicazioni nel cluster.
{:shortdesc}

Quando crei un cluster standard, i nodi di lavoro vengono ordinati nell'infrastruttura IBM Cloud (SoftLayer) per te e aggiunti al pool di nodi di lavoro predefinito nel tuo cluster. A ogni nodo di lavoro viene
assegnato un ID e nome dominio univoco che non deve essere modificato dopo la creazione del cluster.

Puoi scegliere tra server virtuali o fisici (bare metal). A seconda del livello di isolamento hardware che scegli, i nodi di lavoro virtuali possono essere configurati come nodi condivisi o dedicati. Puoi anche scegliere se vuoi che i nodi di lavoro si colleghino a una VLAN pubblica e privata o solo a una VLAN privata. Viene eseguito il provisioning di ogni nodo di lavoro
con un tipo di macchina specifico che determina il numero di vCPU, memoria
e spazio su disco disponibili per i contenitori distribuiti al nodo di lavoro. Kubernetes limita il numero massimo di nodi di lavoro che puoi avere in un cluster. Controlla [nodo di lavoro e quote pod ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/admin/cluster-large/) per ulteriori informazioni.



### Hardware per i nodi di lavoro
{: #shared_dedicated_node}

Quando crei un cluster standard in {{site.data.keyword.Bluemix_notm}}, puoi scegliere di eseguire il provisioning dei tuoi nodi di lavoro come macchine fisiche (bare metal) o come macchine virtuali eseguite su hardware fisico. Quando crei un cluster gratuito, viene eseguito automaticamente il provisioning del tuo nodo di lavoro come nodo virtuale condiviso nell'account dell'infrastruttura IBM Cloud (SoftLayer).
{:shortdesc}

![Opzioni hardware per i nodi di lavoro in un cluster standard](images/cs_clusters_hardware.png)

Controlla le seguenti informazioni per decidere quale tipo di pool di lavoro vuoi. Come ben sai, tieni presente la [soglia minima del limite del nodo di lavoro](#resource_limit_node) del 10% di capacità di memoria totale.

<dl>
<dt>Perché dovrei utilizzare le macchine fisiche (bare metal)?</dt>
<dd><p><strong>Più risorse di calcolo</strong>: puoi eseguire il provisioning del tuo nodo di lavoro come server fisico a singolo tenant, indicato anche come bare metal. Bare metal ti dà accesso diretto alle risorse fisiche sulla macchina, come la memoria o la CPU. Questa configurazione elimina l'hypervisor della macchina virtuale che assegna risorse fisiche alle macchine virtuali eseguite sull'host. Invece, tutte le risorse di una macchina bare metal sono dedicate esclusivamente al nodo di lavoro, quindi non devi preoccuparti dei "vicini rumorosi" che condividono risorse o rallentano le prestazioni. I tipi di macchine fisiche hanno un'archiviazione locale maggiore rispetto a quelle virtuali e alcune dispongono di RAID per il backup dei dati locali.</p>
<p><strong>Fatturazione mensile</strong>: i server bare metal sono più costosi di quelli virtuali e sono più adatti per le applicazioni ad alte prestazioni che richiedono più risorse e controllo host. I server bare metal vengono fatturati mensilmente. Se annulli un server bare metal prima della fine del mese, ti viene addebitato il costo fino alla fine di quel mese. L'ordinazione e l'annullamento dei server bare metal è un processo manuale che viene eseguito tramite il tuo account dell'infrastruttura IBM Cloud (SoftLayer). Il suo completamento può richiedere più di un giorno lavorativo.</p>
<p><strong>Opzione per abilitare Trusted Compute</strong>: abilita Trusted Compute per verificare i tentativi di intrusione nei tuoi nodi di lavoro. Se non abiliti l'attendibilità durante la creazione del cluster ma vuoi farlo in seguito, puoi usare il [comando](cs_cli_reference.html#cs_cluster_feature_enable) `bx cs feature-enable`. Dopo aver abilitato l'attendibilità, non puoi disabilitarla successivamente. Puoi creare un nuovo cluster senza attendibilità. Per ulteriori informazioni su come funziona l'attendibilità durante il processo di avvio del nodo, vedi [{{site.data.keyword.containershort_notm}} con Trusted Compute](cs_secure.html#trusted_compute). Trusted Compute è disponibile sui cluster su cui è in esecuzione Kubernetes versione 1.9 o successive e che hanno alcuni tipi di macchine bare metal. Quando esegui il comando `bx cs machine-types <location>` [](cs_cli_reference.html#cs_machine_types), puoi vedere quali macchine supportano l'attendibilità controllando il campo **Trustable**. Ad esempio, le caratteristiche GPU `mgXc` non supportano Trusted Compute.</p></dd>
<dt>Perché dovrei utilizzare le macchine virtuali?</dt>
<dd><p>Con le VM, ottieni maggiore flessibilità, tempi di provisioning più veloci e ulteriori funzioni di scalabilità automatica rispetto ai bare metal, ad un prezzo più conveniente. Puoi utilizzare le VM per casi di utilizzo più generali come gli ambienti di sviluppo e test, di preparazione e di produzione, i microservizi e le applicazioni di business. Tuttavia, è un compromesso nelle prestazioni. Se hai bisogno di calcolo di prestazioni elevate per la RAM-, i dati-, o i carichi di lavoro intensivi di GPU-, utilizza i bare metal.</p>
<p><strong>Decidi tra la tenancy singola o multipla</strong>: quando crei un cluster virtuale standard, devi scegliere se desideri che l'hardware sottostante sia condiviso tra più clienti {{site.data.keyword.IBM_notm}} (più tenant) o che sia dedicato solo a te (tenant singolo).</p>
<p>In una configurazione a più tenant, le risorse fisiche, come ad esempio la CPU e la memoria, vengono condivise tra tutte le
macchine virtuali distribuite allo stesso hardware fisico. Per assicurare che ogni macchina virtuale
possa essere eseguita indipendentemente, un monitoraggio della macchina virtuale, conosciuto anche come hypervisor,
divide le risorse fisiche in entità isolate e le alloca come risorse dedicate
a una macchina virtuale (isolamento hypervisor).</p>
<p>In una configurazione a tenant singolo, tutte le risorse fisiche sono dedicate soltanto a te. Puoi distribuire
più nodi di lavoro come macchine virtuali allo stesso host fisico. In modo simile alla configurazione a più tenant,
l'hypervisor assicura che ogni nodo ottenga la propria condivisione di risorse
fisiche disponibili.</p>
<p>I nodi condivisi sono generalmente più economici dei nodi dedicati perché i costi dell'hardware sottostante
sono condivisi tra più clienti. Tuttavia, quando decidi tra nodi condivisi e dedicati,
potresti voler verificare con il tuo dipartimento legale e discutere sul livello di conformità e isolamento dell'infrastruttura
che il tuo ambiente dell'applicazione necessita.</p>
<p><strong>Caratteristiche delle macchine virtuali `u2c` o `b2c`</strong>: queste macchine utilizzano il disco locale anziché SAN (Storage Area Networking) per garantire l'affidabilità. I vantaggi dell'affidabilità includono una velocità di elaborazione più elevata durante la serializzazione dei byte sul disco locale e una riduzione del danneggiamento del file system dovuto a errori di rete. Questi tipi di macchina contengono 25 GB di archiviazione su disco locale primaria per il file system del sistema operativo e 100 GB di archiviazione su disco locale secondaria per `/var/lib/docker`, ossia la directory in cui sono scritti tutti i dati del contenitore.</p>
<p><strong>Cosa succede se ho tipi di macchine `u1c` o `b1c` obsoleti?</strong> Per iniziare a utilizzare i tipi di macchina `u2c` e `b2c`, [aggiorna i tipi di macchina aggiungendo i nodi di lavoro](cs_cluster_update.html#machine_type).</p></dd>
<dt>Tra quali caratteristiche della macchina fisica e virtuali posso scegliere?</dt>
<dd><p>Molte! Seleziona il tipo di macchina che è migliore per il tuo caso di utilizzo. Ricorda che un pool di lavoro è composto da macchine che sono della stessa caratteristica. Se vuoi una combinazione di tipi di macchina nel tuo cluster, crea pool di lavoro separati per ogni caratteristica.</p>
<p>I tipi di macchina variano per zona. Per vedere i tipi di macchina disponibili nella tua zona, esegui `bx cs machine-types <zone_name>`.</p>
<p><table>
<caption>Tipi di macchina virtuale e fisica (bare metal) disponibili in {{site.data.keyword.containershort_notm}}.</caption>
<thead>
<th>Nome e caso di utilizzo</th>
<th>Core / Memoria</th>
<th>Disco primario / secondario</th>
<th>Velocità di rete</th>
</thead>
<tbody>
<tr>
<td><strong>Virtuale, u2c.2x4</strong>: utilizza questa VM di dimensione minima per il test rapido, le prove di concetto e altri carichi di lavoro leggeri.</td>
<td>2 / 4GB</td>
<td>25GB / 100GB</td>
<td>1000Mbps</td>
</tr>
<tr>
<td><strong>Virtuale, b2c.4x16</strong>: seleziona questa VM bilanciata per il test e lo sviluppo e altri carichi di lavoro leggeri.</td>
<td>4 / 16GB</td>
<td>25GB / 100GB</td>
<td>1000Mbps</td>
</tr>
<tr>
<td><strong>Virtuale, b2c.16x64</strong>: seleziona questa VM bilanciata per carichi di lavoro di dimensione media.</td></td>
<td>16 / 64GB</td>
<td>25GB / 100GB</td>
<td>1000Mbps</td>
</tr>
<tr>
<td><strong>Virtuale, b2c.32x128</strong>: seleziona questa VM bilanciata per carichi di lavoro medi o grandi, come un database e un sito web dinamico con molti utenti simultanei.</td></td>
<td>32 / 128GB</td>
<td>25GB / 100GB</td>
<td>1000Mbps</td>
</tr>
<tr>
<td><strong>Virtuale, b2c.56x242</strong>: seleziona questa VM bilanciata per carichi di lavoro grandi, come un database e più applicazioni con molti utenti simultanei.</td></td>
<td>56 / 242GB</td>
<td>25GB / 100GB</td>
<td>1000Mbps</td>
</tr>
<tr>
<td><strong>Bare metal con RAM intensiva, mr1c.28x512</strong>: massimizza la disponibilità della RAM dei tuoi nodi di lavoro.</td>
<td>28 / 512GB</td>
<td>2TB SATA / 960GB SSD</td>
<td>10000Mbps</td>
</tr>
<tr>
<td><strong>Bare metal GPU, mg1c.16x128</strong>: scegli questo tipo per i carichi di lavoro intensivi matematicamente come il calcolo di elevate prestazioni, il machine learning o le applicazioni 3D. Questa caratteristica ha 1 scheda fisica Tesla K80 con 2 GPU (graphics processing unit) per scheda per un totale di 2 GPU.</td>
<td>16 / 128GB</td>
<td>2TB SATA / 960GB SSD</td>
<td>10000Mbps</td>
</tr>
<tr>
<td><strong>Bare metal GPU, mg1c.28x256</strong>: scegli questo tipo per i carichi di lavoro intensivi matematicamente come il calcolo di elevate prestazioni, il machine learning o le applicazioni 3D. Questa caratteristica ha 2 schede fisiche Tesla K80 con 2 GPU per scheda per un totale di 4 GPU.</td>
<td>28 / 256GB</td>
<td>2TB SATA / 960GB SSD</td>
<td>10000Mbps</td>
</tr>
<tr>
<td><strong>Bare metal con dai intensivi, md1c.16x64.4x4tb</strong>: per una quantità significativa di archiviazione su disco locale, incluso RAID per eseguire il backup dei dati memorizzati localmente sulla macchina. Utilizza per casi come i file system distribuiti, i database molto grandi e i carichi di lavoro di analisi Big Data.</td>
<td>16 / 64GB</td>
<td>2x2TB RAID1 / 4x4TB SATA RAID10</td>
<td>10000Mbps</td>
</tr>
<tr>
<td><strong>Bare metal con dai intensivi, md1c.28x512.4x4tb</strong>: per una quantità significativa di archiviazione su disco locale, incluso RAID per eseguire il backup dei dati memorizzati localmente sulla macchina. Utilizza per casi come i file system distribuiti, i database molto grandi e i carichi di lavoro di analisi Big Data.</td>
<td>28 / 512 GB</td>
<td>2x2TB RAID1 / 4x4TB SATA RAID10</td>
<td>10000Mbps</td>
</tr>
<tr>
<td><strong>Bare metal bilanciato, mb1c.4x32</strong>: utilizza per i carichi di lavoro bilanciati che richiedono ulteriori risorse di calcolo rispetto all'offerta delle macchine virtuali.</td>
<td>4 / 32GB</td>
<td>2TB SATA / 2TB SATA</td>
<td>10000Mbps</td>
</tr>
<tr>
<td><strong>Bare metal bilanciato, mb1c.16x64</strong>: utilizza per i carichi di lavoro bilanciati che richiedono ulteriori risorse di calcolo rispetto all'offerta delle macchine virtuali.</td>
<td>16 / 64GB</td>
<td>2TB SATA / 960GB SSD</td>
<td>10000Mbps</td>
</tr>
</tbody>
</table>
</p>
</dd>
</dl>


Puoi distribuire i cluster utilizzando l'[IU della console](#clusters_ui) o la [CLI](#clusters_cli).

### Connessione VLAN per i nodi di lavoro
{: #worker_vlan_connection}

Quando crei un cluster, ogni cluster viene automaticamente collegato a una VLAN dal tuo account dell'infrastruttura IBM Cloud (SoftLayer).
{:shortdesc}

Una VLAN configura un gruppo di
nodi di lavoro come se fossero collegati con lo stesso cavo fisico.
* La VLAN pubblica dispone di due sottoreti fornite automaticamente. La sottorete pubblica primaria determina l'indirizzo IP pubblico assegnato a un nodo di lavoro durante la creazione del cluster e la sottorete pubblica portatile fornisce gli indirizzi IP pubblici per i servizi di rete del programma di bilanciamento del carico e Ingress.
* Anche la VLAN privata dispone di due sottoreti fornite automaticamente. La sottorete privata primaria determina l'indirizzo IP privato assegnato a un nodo di lavoro durante la creazione del cluster e la sottorete privata portatile fornisce gli indirizzi IP privati per i servizi di rete del programma di bilanciamento del carico e Ingress.

Per i cluster gratuiti, i nodi di lavoro del cluster vengono collegati a una VLAN privata e a una VLAN pubblica di proprietà di IBM per impostazione predefinita durante la creazione del cluster.

Per i cluster standard, la prima volta che crei un cluster in un'ubicazione, ti vengono automaticamente fornite una VLAN privata e una pubblica. Per ogni cluster successivo che crei in tale ubicazione, scegli le VLAN che desideri utilizzare. Puoi collegare i tuoi nodi di lavoro a una VLAN pubblica e alla VLAN privata o solo alla VLAN privata. Se vuoi collegare i tuoi nodi di lavoro solo a una VLAN privata, puoi utilizzare l'ID di una VLAN privata esistente o [creare una VLAN privata](/docs/cli/reference/softlayer/index.html#sl_vlan_create) e utilizzare l'ID durante la creazione del cluster. Se i nodi di lavoro sono impostati solo con una VLAN privata, devi configurare una soluzione alternativa per la connettività di rete, come un [VRA (Virtual Router Appliance)](cs_vpn.html#vyatta), in modo che i nodi possano comunicare con il master.

**Nota**: se hai più VLAN per un cluster o più sottoreti sulla stessa VLAN, devi attivare lo spanning della VLAN in modo che i tuoi nodi di lavoro possano comunicare tra loro sulla rete privata. Per istruzioni, vedi [Abilita o disabilita lo spanning della VLAN](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning).

### Limiti di memoria dei nodi di lavoro
{: #resource_limit_node}

{{site.data.keyword.containershort_notm}} imposta un limite di memoria su ogni nodo di lavoro. Quando i pod in esecuzione sul nodo di lavoro superano questo limite di memoria, i pod vengono rimossi. In Kubernetes, questo limite è chiamato [soglia di rimozione rigida![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/tasks/administer-cluster/out-of-resource/#hard-eviction-thresholds).
{:shortdesc}

Se i tuoi pod vengono rimossi frequentemente, aggiungi più nodi di lavoro al tuo cluster o imposta i [limiti di risorsa ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#resource-requests-and-limits-of-pod-and-container) sui pod.

**Ogni macchina ha una soglia minima uguale al 10% della sua capacità di memoria totale**. Quando sul nodo di lavoro è disponibile meno memoria rispetto alla soglia minima consentita, Kubernetes rimuove immediatamente il pod. Il pod viene ripianificato su un altro nodo di lavoro, laddove disponibile. Ad esempio, se hai una macchina virtuale `b2c.4x16`, la sua capacità di memoria totale è 16GB. Se è disponibile meno di 1600MB (10%) di memoria, i nuovi pod non possono essere pianificati in questo nodo di lavoro ma lo sono invece in un altro. Se non è disponibile alcun nodo di lavoro, i nuovi pod rimangono senza pianificazione.

Per verificare la quantità di memoria utilizzata sul tuo nodo di lavoro, esegui [kubectl top node ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/reference/kubectl/overview/#top).

### Autorecovery per i tuoi nodi di lavoro
`Docker`, `kubelet`, `kube-proxy` e `calico` sono componenti critici che devono essere funzionali per avere un nodo di lavoro Kubernetes integro. Con il passare del tempo questi componenti possono rompersi e lasciare il tuo nodo di lavoro in uno stato non funzionale. I nodi di lavoro non funzionali riducono la capacità totale del cluster e possono causare tempi di inattività per la tua applicazione.

Puoi [configurare i controlli di integrità per il nodo di lavoro e abilitare l'Autorecovery](cs_health.html#autorecovery). Se Autorecovery rileva un nodo di lavoro non integro in base ai controlli configurati, attiva un'azione correttiva come un ricaricamento del sistema operativo sul nodo di lavoro. Per ulteriori informazioni su come funziona Autorecovery, vedi il [blog Autorecovery ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/blogs/bluemix/2017/12/autorecovery-utilizes-consistent-hashing-high-availability/).

<br />



## Creazione dei cluster con la GUI
{: #clusters_ui}

Lo scopo del cluster Kubernetes è quello di definire un insieme di risorse, nodi, reti e dispositivi di archiviazione che mantengano le applicazioni altamente disponibili. Prima di poter distribuire un'applicazione,
devi creare un cluster e configurare le definizioni per i nodi di lavoro in tale cluster.
{:shortdesc}





Prima di iniziare, devi disporre di un [account {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/registration/) Pagamento a consumo o Sottoscrizione configurato per [accedere al portfolio dell'infrastruttura IBM Cloud (SoftLayer)](cs_troubleshoot_clusters.html#cs_credentials). Per provare alcune delle funzioni, puoi creare un cluster gratuito che scade dopo 21 giorni. Puoi avere 1 cluster gratuito alla volta.

Puoi rimuovere il tuo cluster gratuito in qualsiasi momento, ma dopo 21 giorno, il cluster gratuito e i suoi dati verranno eliminati e non potranno essere ripristinati. Assicurati di eseguire un backup dei tuoi dati.
{: tip}

Per personalizzare completamente i tuoi cluster con le tue scelte per l'isolamento hardware, la zona, la versione API e altro, crea un cluster standard. 

Per creare un cluster:

1. Nel catalogo, seleziona **Cluster Kubernetes**.

2. Seleziona una regione in cui distribuire il tuo cluster.

3. Seleziona un tipo di piano del cluster. Puoi scegliere tra **Gratuito** o **Standard**. Con un cluster standard hai accesso a funzioni come più nodi di lavoro per un ambiente altamente disponibile.

4. Configura i tuoi dettagli del cluster. Completa la procedura che si applica al tipo di cluster che stai creando.

    1. **Gratuito e Standard**: dai un nome al tuo cluster. Il nome deve iniziare con una lettera, può contenere lettere, numeri e un trattino (-) e non può contenere più di 35 caratteri. Il nome cluster e la regione in cui viene distribuito il cluster formano il nome dominio completo per il dominio secondario Ingress. Per garantire che il dominio secondario Ingress sia univoco all'interno di una regione, è possibile che il nome cluster venga troncato e vi venga aggiunto un valore casuale all'interno del nome dominio Ingress.
 
    2. **Standard**: seleziona l'ubicazione in cui distribuire il tuo cluster. Per prestazioni ottimali, seleziona l'ubicazione fisicamente più vicina a te. Tieni presente che potresti aver bisogno di un'autorizzazione legale prima di poter fisicamente archiviare i dati in un paese straniero se selezioni un'ubicazione al di fuori del tuo paese.

    3. **Standard**: scegli la versione server API Kubernetes per il nodo master del cluster.

    4. **Standard**: seleziona un tipo di isolamento hardware. Il tipo virtuale ha una fatturazione oraria e il tipo bare metal ha una fatturazione mensile.

        - **Virtuale - Dedicato**: i tuoi nodi di lavoro sono ospitati sull'infrastruttura dedicata al tuo account. Le tue risorse fisiche sono completamente isolate.

        - **Virtuale - Condiviso**: le risorse dell'infrastruttura, come l'hypervisor e l'hardware fisico, sono condivise tra te e altri clienti IBM, ma ciascun nodo di lavoro è accessibile solo a te. Sebbene questa opzione sia meno costosa e sufficiente nella maggior parte dei casi, potresti voler verificare i requisiti di prestazioni e infrastruttura con le politiche aziendali.

        - **Bare Metal**: il provisioning dei server bare metal, con fatturazione mensile, viene eseguito tramite l'interazione manuale con l'infrastruttura Cloud IBM (SoftLayer) e il completamento di questo processo può richiedere più di un giorno lavorativo. Bare metal è più adatto per le applicazioni ad alte prestazioni che richiedono più risorse e controllo host.

        Assicurati di voler eseguire il provisioning di una macchina bare metal. Poiché viene fatturata mensilmente, se la annulli immediatamente dopo un ordine effettuato per errore, ti viene comunque addebitato l'intero mese.
        {:tip}

    5.  **Standard**: seleziona il tipo di macchina. Il tipo di macchina definisce la quantità di CPU virtuale, di memoria e di spazio disco configurata in ogni nodo di lavoro e resa disponibile ai contenitori. I tipi di macchine bare metal e virtuali disponibili variano in base all'ubicazione in cui viene distribuito il cluster. Dopo aver creato il tuo cluster, puoi aggiungere diversi tipi di macchina aggiungendo un nodo al cluster.

    6. **Standard**: specifica il numero di nodi di lavoro di cui hai bisogno nel cluster.

    7. **Standard**: seleziona una VLAN pubblica (facoltativo) e una VLAN privata (obbligatorio) dal tuo account dell'infrastruttura IBM Cloud (SoftLayer). Entrambe le VLAN comunicano con i nodi di lavoro ma la VLAN pubblica anche con il master Kubernetes gestito da IBM. Puoi utilizzare la stessa VLAN per più cluster.
        **Nota**: se i nodi di lavoro sono impostati solo con una VLAN privata, devi configurare una soluzione alternativa per la connettività di rete. Per ulteriori informazioni, vedi [Connessione VLAN per i nodi di lavoro](cs_clusters.html#worker_vlan_connection).

    8. Per impostazione predefinita, **Codifica disco locale** è selezionato. Se scegli di deselezionare la casella di spunta, i dati Docker dell'host non vengono codificati. 


4. Fai clic su **Crea cluster**. Puoi vedere lo stato di avanzamento della distribuzione del nodo di lavoro nella scheda **Nodi di lavoro**. Quando la distribuzione è terminata, puoi vedere se il tuo cluster è pronto nella scheda **Panoramica**.
    **Nota:** a ogni nodo di lavoro viene assegnato
un ID e nome dominio univoco che non deve essere modificato manualmente dopo la creazione del
cluster. La modifica dell'ID o del nome dominio impedisce al master Kubernetes di gestire il tuo cluster.



**Operazioni successive**

Quando il cluster è attivo e in esecuzione, puoi controllare le seguenti attività:


-   [Installa le CLI per iniziare ad utilizzare
il tuo cluster.](cs_cli_install.html#cs_cli_install)
-   [Distribuisci un'applicazione nel tuo cluster.](cs_app.html#app_cli)
-   [Configura il tuo registro privato in {{site.data.keyword.Bluemix_notm}} per memorizzare e condividere le immagini Docker con altri utenti.](/docs/services/Registry/index.html)
- Se hai più VLAN per un cluster o più sottoreti sulla stessa VLAN, devi [attivare lo spanning della VLAN](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning) in modo che i tuoi nodi di lavoro possano comunicare tra loro sulla rete privata.
- Se hai un firewall, potresti dover [aprire le porte richieste](cs_firewall.html#firewall) per utilizzare i comandi `bx`, `kubectl` o `calicotl`, per consentire il traffico in uscita dal tuo cluster o per consentire il traffico in entrata per i servizi di rete.

<br />


## Creazione dei cluster con la CLI
{: #clusters_cli}

Lo scopo del cluster Kubernetes è quello di definire un insieme di risorse, nodi, reti e dispositivi di archiviazione che mantengano le applicazioni altamente disponibili. Prima di poter distribuire un'applicazione,
devi creare un cluster e configurare le definizioni per i nodi di lavoro in tale cluster.
{:shortdesc}

Prima di iniziare:
- Devi disporre di un [account {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/registration/) Pagamento a consumo o Sottoscrizione configurato per [accedere al portfolio dell'infrastruttura IBM Cloud (SoftLayer)](cs_troubleshoot_clusters.html#cs_credentials). Puoi creare 1 cluster per provare alcune delle funzionalità per 21 giorni oppure puoi creare cluster standard completamente personalizzabili con le tue scelte di isolamento hardware.
- [Assicurati di disporre delle autorizzazioni minime richieste nell'infrastruttura IBM Cloud (SoftLayer) per eseguire il provisioning di un cluster standard](cs_users.html#infra_access).

Per creare un cluster:

1.  Installa la CLI {{site.data.keyword.Bluemix_notm}} e il plugin
[{{site.data.keyword.containershort_notm}}](cs_cli_install.html#cs_cli_install).

2.  Accedi alla CLI {{site.data.keyword.Bluemix_notm}}. Immetti le tue credenziali
{{site.data.keyword.Bluemix_notm}} quando richiesto.

    ```
    bx login
    ```
    {: pre}

    **Nota:** se disponi di un ID federato, utilizza `bx login --sso` per accedere alla CLI {{site.data.keyword.Bluemix_notm}}. Immetti il tuo nome utente e usa l'URL fornito nell'output della CLI per richiamare la tua passcode monouso. Sai che hai un ID federato quando l'accesso ha esito negativo senza
`--sso` e positivo con l'opzione `--sso`.

3. Se disponi di più account {{site.data.keyword.Bluemix_notm}}, seleziona l'account in cui vuoi creare il tuo cluster Kubernetes.

4.  Se vuoi creare o accedere ai cluster Kubernetes in un'altra regione rispetto alla regione {{site.data.keyword.Bluemix_notm}} che hai selezionato precedentemente, esegui `bx cs region-set`.

6.  Crea un cluster.

    1.  **Cluster standard**: esamina le ubicazioni disponibili. Le ubicazioni che vengono mostrate dipendono dalla regione {{site.data.keyword.containershort_notm}} a cui hai eseguito l'accesso.

        ```
        bx cs locations
        ```
        {: pre}
        
        Il tuo output della CLI corrisponde alle [ubicazioni della regione {{site.data.keyword.containerlong}}](cs_regions.html#locations).
        
    2.  **Cluster standard**: scegli un'ubicazione ed esamina i tipi di macchina disponibili in tale ubicazione. Il tipo di macchina specifica gli host di calcolo virtuali o fisici disponibili per ciascun nodo di lavoro.

        -  Visualizza il campo **Tipo di server** per scegliere macchine virtuali o fisiche (bare metal).
        -  **Virtuale**: il provisioning delle macchine virtuali, con fatturazione oraria, viene eseguito su hardware condiviso o dedicato.
        -  **Fisico**: il provisioning dei server bare metal, con fatturazione mensile, viene eseguito tramite l'interazione manuale con l'infrastruttura Cloud IBM (SoftLayer) e il completamento di questo processo può richiedere più di un giorno lavorativo. Bare metal è più adatto per le applicazioni ad alte prestazioni che richiedono più risorse e controllo host.
        - **Macchine fisiche con Trusted Compute**: per i cluster bare metal che eseguono Kubernetes versione 1.9 o successiva, puoi anche scegliere di abilitare [Trusted Compute](cs_secure.html#trusted_compute) per verificare possibili tentativi di intrusione nei tuoi nodi di lavoro bare metal. Trusted Compute è disponibile per alcuni tipi di macchina bare metal selezionati. Ad esempio, le caratteristiche GPU `mgXc` non supportano Trusted Compute. Se non abiliti l'attendibilità durante la creazione del cluster ma vuoi farlo in seguito, puoi usare il [comando](cs_cli_reference.html#cs_cluster_feature_enable) `bx cs feature-enable`. Dopo aver abilitato l'attendibilità, non puoi disabilitarla successivamente.
        -  **Tipi di macchina**: per stabilire quale tipo di macchina distribuire, esamina le combinazioni di core, memoria e archiviazione o consulta la [documentazione del comando](cs_cli_reference.html#cs_machine_types) `bx cs machine-types`. Dopo aver creato il tuo cluster, puoi aggiungere diversi tipi di macchine fisiche o virtuali utilizzando il [comando](cs_cli_reference.html#cs_worker_add) `bx cs worker-add`.

           Assicurati di voler eseguire il provisioning di una macchina bare metal. Poiché viene fatturata mensilmente, se la annulli immediatamente dopo un ordine effettuato per errore, ti viene comunque addebitato l'intero mese.
           {:tip}

        ```
        bx cs machine-types <location>
        ```
        {: pre}

    3.  **Cluster standard**: controlla se esiste già una VLAN pubblica e privata nell'infrastruttura IBM Cloud (SoftLayer) per questo account.

        ```
        bx cs vlans <location>
        ```
        {: pre}

        ```
        ID        Name                Number   Type      Router  
        1519999   vlan   1355     private   bcr02a.dal10  
        1519898   vlan   1357     private   bcr02a.dal10 
        1518787   vlan   1252     public   fcr02a.dal10 
        1518888   vlan   1254     public    fcr02a.dal10
        ```
        {: screen}

        Se esiste già una VLAN pubblica e privata, annota i router corrispondenti. I router della VLAN privata iniziano sempre con <code>bcr</code> (router back-end) e i router
della VLAN pubblica iniziano sempre con <code>fcr</code> (router front-end). Quando crei un cluster e specifichi le VLAN private e pubbliche, la combinazione di numero e lettera dopo tali prefissi deve corrispondere. Nell'output di esempio, le VLAN private
possono essere utilizzate con una qualsiasi VLAN pubblica perché i router includono tutti
`02a.dal10`.

        Devi collegare i tuoi nodi di lavoro a una VLAN privata e puoi, facoltativamente, collegarli a una VLAN pubblica **Nota**: se i nodi di lavoro sono impostati solo con una VLAN privata, devi configurare una soluzione alternativa per la connettività di rete. Per ulteriori informazioni, vedi [Connessione VLAN per i nodi di lavoro](cs_clusters.html#worker_vlan_connection).

    4.  **Cluster gratuiti e standard**: esegui il comando `cluster-create`. Puoi scegliere tra un cluster gratuito, che include un nodo di lavoro configurato con 2vCPU e 4 GB di memoria e che viene eliminato automaticamente dopo 21 giorni. Quando crei un cluster standard, per impostazione predefinita, i dischi del nodo di lavoro vengono codificati, il relativo hardware condiviso tra più clienti IBM e viene effettuato un addebito per ora di utilizzo. </br>Esempio per un cluster standard. Specifica le opzioni del cluster:

        ```
        bx cs cluster-create --location dal10 --machine-type u2c.2x4 --hardware <shared_or_dedicated> --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID> --workers 3 --name <cluster_name> --kube-version <major.minor.patch> [--disable-disk-encrypt][--trusted]
        ```
        {: pre}

        Esempio per un cluster gratuito. Specifica il nome del cluster:

        ```
        bx cs cluster-create --name my_cluster
        ```
        {: pre}

        <table>
        <caption>Componenti di cluster-create</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti di questo comando</th>
        </thead>
        <tbody>
        <tr>
        <td><code>cluster-create</code></td>
        <td>Il comando per creare un cluster nella tua organizzazione {{site.data.keyword.Bluemix_notm}}.</td>
        </tr>
        <tr>
        <td><code>--location <em>&lt;location&gt;</em></code></td>
        <td>**Cluster standard**: sostituisci <em>&lt;location&gt;</em> con l'ID ubicazione {{site.data.keyword.Bluemix_notm}} in cui vuoi creare il tuo cluster. [Le ubicazioni disponibili](cs_regions.html#locations) dipendono dalla regione {{site.data.keyword.containershort_notm}} in cui hai eseguito l'accesso.</td>
        </tr>
        <tr>
        <td><code>--machine-type <em>&lt;machine_type&gt;</em></code></td>
        <td>**Cluster standard**: scegli un tipo di macchina. Puoi distribuire i tuoi nodi di lavoro come macchine virtuali su hardware condiviso o dedicato o come macchine fisiche su bare metal. I tipi di macchine fisiche e virtuali disponibili variano in base all'ubicazione in cui viene distribuito il cluster. Per ulteriori informazioni, consulta la documentazione per il [comando](cs_cli_reference.html#cs_machine_types) `bx cs machine-type`. Per i cluster gratuiti, non devi definire il tipo di macchina.</td>
        </tr>
        <tr>
        <td><code>--hardware <em>&lt;shared_or_dedicated&gt;</em></code></td>
        <td>**Cluster standard, solo virtuali**: il livello di isolamento hardware per il tuo nodo di lavoro. Utilizza dedicato per avere risorse fisiche dedicate disponibili solo per te o condiviso per consentire la condivisione delle risorse fisiche con altri clienti IBM. L'impostazione predefinita è condiviso. Questo valore è facoltativo per i cluster standard e non è disponibile per i cluster gratuiti.</td>
        </tr>
        <tr>
        <td><code>--public-vlan <em>&lt;public_vlan_id&gt;</em></code></td>
        <td><ul>
          <li>**Cluster gratuiti**: non è necessario definire una VLAN pubblica. Il tuo cluster gratuito viene automaticamente collegato alla VLAN pubblica di proprietà di IBM.</li>
          <li>**Cluster standard**: se hai già una VLAN pubblica configurata nel tuo account dell'infrastruttura IBM Cloud (SoftLayer) per quella ubicazione, immetti l'ID della VLAN pubblica. Se vuoi collegare i tuoi nodi di lavoro solo a una VLAN privata, non specificare questa opzione. **Nota**: se i nodi di lavoro sono impostati solo con una VLAN privata, devi configurare una soluzione alternativa per la connettività di rete. Per ulteriori informazioni, vedi [Connessione VLAN per i nodi di lavoro](cs_clusters.html#worker_vlan_connection).<br/><br/>
          <strong>Nota</strong>: i router della VLAN privata iniziano sempre con <code>bcr</code> (router back-end) e i router della VLAN pubblica iniziano sempre con <code>fcr</code> (router front-end). Quando crei un cluster e specifichi le VLAN private e pubbliche, la combinazione di numero e lettera dopo tali prefissi deve corrispondere.</li>
        </ul></td>
        </tr>
        <tr>
        <td><code>--private-vlan <em>&lt;private_vlan_id&gt;</em></code></td>
        <td><ul><li>**Cluster gratuiti**: non è necessario definire una VLAN privata. Il tuo cluster gratuito viene automaticamente collegato alla VLAN privata di proprietà di IBM.</li><li>**Cluster standard**: se hai già una VLAN privata configurata nel tuo account dell'infrastruttura IBM Cloud (SoftLayer) per quella ubicazione, immetti l'ID della VLAN privata. Se non hai una VLAN privata nel tuo account, non specificare questa opzione. {{site.data.keyword.containershort_notm}} crea automaticamente una VLAN privata per te.<br/><br/><strong>Nota</strong>: i router della VLAN privata iniziano sempre con <code>bcr</code> (router back-end) e i router della VLAN pubblica iniziano sempre con <code>fcr</code> (router front-end). Quando crei un cluster e specifichi le VLAN private e pubbliche, la combinazione di numero e lettera dopo tali prefissi deve corrispondere.</li></ul></td>
        </tr>
        <tr>
        <td><code>--name <em>&lt;name&gt;</em></code></td>
        <td>**Cluster gratuiti e standard**: sostituisci <em>&lt;name&gt;</em> con un nome per il tuo cluster. Il nome deve iniziare con una lettera, può contenere lettere, numeri e un trattino (-) e non può contenere più di 35 caratteri. Il nome cluster e la regione in cui viene distribuito il cluster formano il nome dominio completo per il dominio secondario Ingress. Per garantire che il dominio secondario Ingress sia univoco all'interno di una regione, è possibile che il nome cluster venga troncato e vi venga aggiunto un valore casuale all'interno del nome dominio Ingress.
</td>
        </tr>
        <tr>
        <td><code>--workers <em>&lt;number&gt;</em></code></td>
        <td>**Cluster standard**: il numero di nodi di lavoro da includere nel cluster. Se non viene specificata l'opzione <code>--workers</code>, viene creato 1 nodo di lavoro.</td>
        </tr>
        <tr>
          <td><code>--kube-version <em>&lt;major.minor.patch&gt;</em></code></td>
          <td>**Cluster standard**: la versione di Kubernetes per il nodo master del cluster. Questo valore è facoltativo. Quando la versione non viene specificata, il cluster viene creato con l'impostazione predefinita delle versioni di Kubernetes supportate. Per visualizzare le versioni disponibili. esegui <code>bx cs kube-versions</code>.
</td>
        </tr>
        <tr>
        <td><code>--disable-disk-encrypt</code></td>
        <td>**Cluster gratuiti e standard**: i nodi di lavoro dispongono della codifica del disco per impostazione predefinita; [ulteriori informazioni](cs_secure.html#encrypted_disks). Se vuoi disabilitare la codifica, includi questa opzione.</td>
        </tr>
        <tr>
        <td><code>--trusted</code></td>
        <td>**Cluster bare metal standard**: abilita [Trusted Compute](cs_secure.html#trusted_compute) per verificare i tentativi di intrusione nei tuoi nodi di lavoro bare metal. Trusted Compute è disponibile per alcuni tipi di macchina bare metal selezionati. Ad esempio, le caratteristiche GPU `mgXc` non supportano Trusted Compute. Se non abiliti l'attendibilità durante la creazione del cluster ma vuoi farlo in seguito, puoi usare il [comando](cs_cli_reference.html#cs_cluster_feature_enable) `bx cs feature-enable`. Dopo aver abilitato l'attendibilità, non puoi disabilitarla successivamente.</td>
        </tr>
        </tbody></table>

7.  Verifica che la creazione del cluster sia stata richiesta.

    ```
    bx cs clusters
    ```
    {: pre}

    **Nota:** per le macchine virtuali, possono essere necessari alcuni minuti per l'ordine delle macchine del nodo di lavoro e per la configurazione e il provisioning del cluster nel tuo account. Il provisioning delle macchine fisiche bare metal viene eseguito tramite l'interazione manuale con l'infrastruttura Cloud IBM (SoftLayer) e il completamento di questo processo può richiedere più di un giorno lavorativo.

    Quando è stato terminato il provisioning del tuo cluster, lo stato del tuo cluster viene modificato in **distribuito**.

    ```
    Name         ID                                   State      Created          Workers   Location   Version
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   1         mil01      1.9.7
    ```
    {: screen}

8.  Controlla lo stato dei nodi di lavoro.

    ```
    bx cs workers <cluster_name_or_ID>
    ```
    {: pre}

    Quando i nodi di lavoro sono pronti, la condizione passa a **normale** e lo stato è **Pronto**. Quando lo stato del nodo è **Pronto**, puoi accedere al cluster.

    **Nota:** a ogni nodo di lavoro viene assegnato un ID e nome dominio univoco che non deve essere modificato manualmente dopo la creazione del cluster. La modifica dell'ID o del nome dominio impedisce al master Kubernetes di gestire il tuo cluster.

    ```
    ID                                                 Public IP       Private IP      Machine Type   State    Status   Location   Version
    kube-mil01-paf97e8843e29941b49c598f516de72101-w1   169.xx.xxx.xxx  10.xxx.xx.xxx   free           normal   Ready    mil01      1.9.7
    ```
    {: screen}

9. Configura il cluster che hai creato come il contesto per questa sessione. Completa questi passi di configurazione ogni volta che lavori con il tuo cluster.
    1.  Richiama il comando per impostare la variabile di ambiente e scaricare i file di configurazione Kubernetes.

        ```
        bx cs cluster-config <cluster_name_or_ID>
        ```
        {: pre}

        Quando il download dei file di configurazione è terminato, viene visualizzato un comando che puoi utilizzare per impostare il percorso al file di configurazione di Kubernetes locale come una variabile di ambiente.

        Esempio per OS X:

        ```
        export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
        ```
        {: screen}

    2.  Copia e incolla il comando visualizzato nel tuo terminale per impostare la variabile di ambiente `KUBECONFIG`.
    3.  Verifica che la variabile di ambiente `KUBECONFIG` sia impostata correttamente.

        Esempio per OS X:

        ```
        echo $KUBECONFIG
        ```
        {: pre}

        Output:

        ```
        /Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml

        ```
        {: screen}

10. Avvia il tuo dashboard Kubernetes con la porta predefinita `8001`.
    1.  Imposta il proxy con il numero di porta predefinito.

        ```
        kubectl proxy
        ```
        {: pre}

        ```
        Inizio di utilizzo su 127.0.0.1:8001
        ```
        {: screen}

    2.  Apri il seguente URL in un browser web per visualizzare il dashboard Kubernetes.

        ```
        http://localhost:8001/ui
        ```
        {: codeblock}


**Operazioni successive**


-   [Distribuisci un'applicazione nel tuo cluster.](cs_app.html#app_cli)
-   [Gestisci il tuo cluster con la riga di comando `kubectl`. ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/reference/kubectl/overview/)
-   [Configura il tuo registro privato in {{site.data.keyword.Bluemix_notm}} per memorizzare e condividere le immagini Docker con altri utenti.](/docs/services/Registry/index.html)
- Se hai più VLAN per un cluster o più sottoreti sulla stessa VLAN, devi [attivare lo spanning della VLAN](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning) in modo che i tuoi nodi di lavoro possano comunicare tra loro sulla rete privata.
- Se hai un firewall, potresti dover [aprire le porte richieste](cs_firewall.html#firewall) per utilizzare i comandi `bx`, `kubectl` o `calicotl`, per consentire il traffico in uscita dal tuo cluster o per consentire il traffico in entrata per i servizi di rete.

<br />





## Visualizzazione degli stati del cluster
{: #states}

Esamina lo stato di un cluster Kubernetes per ottenere informazioni sulla disponibilità e capacità del cluster e sui potenziali problemi che potrebbero essersi verificati.
{:shortdesc}

Per visualizzare le informazioni su un cluster specifico, come ubicazioni, URL master, dominio secondario Ingress, versione, proprietario e dashboard di monitoraggio, utilizza il comando `bx cs cluster-get <cluster_name_or_ID>` [](cs_cli_reference.html#cs_cluster_get). Includi l'indicatore `--showResources` per visualizzare più risorse del cluster come i componenti aggiuntivi per i pod di archiviazione o le VLAN di sottorete per IP pubblici e privati.

Puoi visualizzare lo stato del cluster corrente eseguendo il comando `bx cs clusters` e individuando il campo **Stato**. Per risolvere i problemi relativi al cluster e ai nodi di lavoro, vedi [Risoluzione dei problemi dei cluster](cs_troubleshoot.html#debug_clusters).

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


## Rimozione dei cluster
{: #remove}

I cluster gratuiti e standard creati con un account a Pagamento a consumo devono essere rimossi manualmente quando non sono più necessari in modo che non utilizzino più le risorse.
{:shortdesc}

**Avvertenza:**
  - Non viene creato alcun backup del tuo cluster o dei tuoi dati nella tua archiviazione persistente. L'eliminazione di un cluster o dell'archiviazione persistente è permanente e non può essere annullata.
  - Quando decidi di rimuovere un cluster, rimuovi anche le sottoreti di cui è stato eseguito automaticamente il provisioning quando hai creato il cluster e che hai creato utilizzando il comando `bx cs cluster-subnet-create`. Tuttavia, se hai aggiunto manualmente sottoreti esistenti al tuo cluster utilizzando il `comando bx cs cluster-subnet-add`, tali sottoreti non verranno rimosse dal tuo account dell'infrastruttura IBM Cloud (SoftLayer) e potrai riutilizzarle in altri cluster.

Prima di iniziare, prendi nota del tuo ID cluster. Potresti aver bisogno dell'ID cluster per investigare e rimuovere le risorse correlate all'infrastruttura IBM Cloud (SoftLayer) che non vengono automaticamente eliminate con il tuo cluster, come ad esempio la memoria persistente.
{: tip}

Per rimuovere un cluster:

-   Dalla GUI {{site.data.keyword.Bluemix_notm}}
    1.  Seleziona il tuo cluster e fai clic su **Elimina** dal menu **Ulteriori azioni...**.

-   Dalla CLI {{site.data.keyword.Bluemix_notm}}
    1.  Elenca i cluster disponibili.

        ```
        bx cs clusters
        ```
        {: pre}

    2.  Elimina il cluster.

        ```
        bx cs cluster-rm <cluster_name_or_ID
   >
        ```
        {: pre}

    3.  Segui le richieste e scegli se eliminare le risorse del cluster che includono contenitori, pod, servizi associati, archiviazione persistente e segreti.
      - **Archiviazione persistente**: l'archiviazione persistente fornisce l'alta disponibilità per i tuoi dati. Se hai creato un'attestazione del volume persistente utilizzando una [condivisione file esistente](cs_storage.html#existing), non potrai eliminare la condivisione file quando elimini il cluster. Devi eliminare manualmente in seguito la condivisione file dal tuo portfolio dell'infrastruttura IBM Cloud (SoftLayer).

          **Nota**: a causa del ciclo di fatturazione mensile, non è possibile eliminare un'attestazione del volume persistente durante l'ultimo giorno del mese. Se elimini l'attestazione del volume persistente l'ultimo giorno del mese, l'eliminazione rimane in sospeso fino all'inizio del mese successivo.

Passi successivi:
- Puoi riutilizzare il nome di un cluster rimosso una volta che non è più presente nell'elenco dei cluster disponibili quando viene eseguito il comando `bx cs clusters`.
- Se mantieni le sottoreti, puoi [riutilizzarle in un nuovo cluster](cs_subnets.html#custom) o eliminarle manualmente in un secondo momento dal tuo portfolio dell'infrastruttura IBM Cloud (SoftLayer).
- Se mantieni l'archiviazione persistente, puoi eliminarla in un secondo momento attraverso il dashboard dell'infrastruttura IBM Cloud (SoftLayer) nella GUI {{site.data.keyword.Bluemix_notm}}.
