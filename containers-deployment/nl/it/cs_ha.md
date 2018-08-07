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




# Alta disponibilità per {{site.data.keyword.containerlong_notm}}
{: #ha}

Utilizza le funzioni integrate di Kubernetes e {{site.data.keyword.containerlong}} per rendere il tuo cluster ancora più disponibile e per proteggere la tua applicazione dai tempi di inattività in caso di errore di un componente nel tuo cluster.
{: shortdesc}

L'alta disponibilità è una disciplina fondamentale in un'infrastruttura IT per mantenere operative le tue applicazioni, anche dopo un'avaria parziale o totale del sito. Lo scopo principale dell'alta disponibilità è eliminare i potenziali punti di errore in un'infrastruttura IT. Ad esempio, puoi tutelarti in caso di errore di un sistema aggiungendo la ridondanza e configurando meccanismi di failover.

Puoi ottenere l'alta disponibilità su diversi livelli della tua infrastruttura IT e all'interno dei vari componenti del tuo cluster. l livello di disponibilità giusto per te dipende da diversi fattori, come i requisiti aziendali, gli SLA (Service Level Agreement) stipulati con i tuoi clienti e il denaro che vuoi spendere.

## Panoramica dei potenziali punti di errore in {{site.data.keyword.containerlong_notm}}
{: #fault_domains} 

L'architettura e l'infrastruttura di {{site.data.keyword.containerlong_notm}} sono progettate per garantire affidabilità, bassa latenza di elaborazione e una massima operatività del servizio. Tuttavia, possono verificarsi degli errori. A seconda del servizio che ospiti in {{site.data.keyword.Bluemix_notm}}, potresti non essere in grado di tollerare gli errori, anche se questi errori durano solo pochi minuti.
{: shortdesc}

{{site.data.keyword.containershort_notm}} fornisce diversi approcci per aggiungere più disponibilità al tuo cluster aggiungendo ridondanza e anti-affinità. Controlla la seguente immagine per informazioni sui potenziali punti di errore e su come eliminarli.

<img src="images/cs_failure_ov.png" alt="Panoramica dei domini di errore in un cluster ad alta disponibilità all'interno di una regione {{site.data.keyword.containershort_notm}}." width="250" style="width:250px; border-style: none"/>


<table summary="La tabella mostra i punti di errore in {{site.data.keyword.containershort_notm}}. Le righe devono essere lette da sinistra a destra, con il numero del punto di errore nella colonna uno, il titolo del punto di errore nella colonna due, una descrizione nella colonna tre e un link alla documentazione nella colonna quattro.">
<caption>Punti di errore </caption>
<col width="3%">
<col width="10%">
<col width="70%">
<col width="17%">
  <thead>
  <th>#</th>
  <th>Punto di errore</th>
  <th>Descrizione</th>
  <th>Link alle documentazioni</th>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>Errore del contenitore o del pod</td>
      <td>I contenitori e i pod sono, come progettati, di breve durata e possono avere un malfunzionamento imprevisto. Ad esempio, un contenitore o un pod potrebbe arrestarsi se si verifica un errore nella tua applicazione. Per rendere la tua applicazione altamente disponibile, devi assicurarti di disporre di un numero sufficiente di istanze della tua applicazione per gestire il carico di lavoro oltre a delle istanze aggiuntive in caso di errore. Idealmente, queste istanze sono distribuite su più nodi di lavoro per proteggere la tua applicazione in caso di un errore del nodo di lavoro.</td>
      <td>[Distribuzione di applicazioni altamente disponibili.](cs_app.html#highly_available_apps)</td>
  </tr>
  <tr>
    <td>2</td>
    <td>Errore del nodo di lavoro</td>
    <td>Un nodo di lavoro è una VM che viene eseguita su un hardware fisico. Gli errori dei nodi di lavoro includono interruzioni hardware, come alimentazione, raffreddamento o collegamento in rete e problemi nella VM stessa. Puoi tenere conto di un errore del nodo di lavoro configurando più nodi di lavoro nel tuo cluster. <br/><br/><strong>Nota:</strong> non è garantito che i nodi di lavoro in una ubicazione siano su host di calcolo fisici separati. Ad esempio, potresti avere un cluster con 3 nodi di lavoro , ma tutti e 3 i nodi di lavoro sono stati creati sullo stesso host di calcolo fisico nella sede IBM. Se questo host di calcolo fisico si interrompe, si interromperanno tutti i tuoi nodi di lavoro. Per proteggerti da questo errore, devi configurare un secondo cluster in un'altra ubicazione.</td>
    <td>[Creazione di cluster con più nodi di lavoro.](cs_cli_reference.html#cs_cluster_create)</td>
  </tr>
  <tr>
    <td>3</td>
    <td>Errore del cluster</td>
    <td>Il master Kubernetes è il componente principale che mantiene operativo il tuo cluster. Il master memorizza tutti i dati del cluster nel database etcd che funge da unico punto di verità per il tuo cluster. Un errore del cluster si verifica quando non è possibile raggiungere il master a causa di un errore di rete o quando i dati nel database etcd vengono danneggiati. Puoi creare più cluster in un'unica ubicazione per proteggere le tue applicazioni da un errore di etcd o del master Kubernetes. Per bilanciare il carico tra i cluster, devi configurare un programma di bilanciamento del carico esterno. <br/><br/><strong>Nota:</strong> la configurazione di più cluster in un'unica ubicazione non garantisce che i tuoi nodi di lavoro vengano distribuiti su host di calcolo fisici separati. Per proteggerti da questo errore, devi configurare un secondo cluster in un'altra ubicazione.</td>
    <td>[Configurazione di cluster altamente disponibili.](cs_clusters.html#planning_clusters)</td>
  </tr>
  <tr>
    <td>4</td>
    <td>Errore di ubicazione</td>
    <td>Un errore di ubicazione riguarda tutti gli host di calcolo fisici e l'archiviazione NFS. Gli errori includono interruzioni di alimentazione, raffreddamento, collegamento in rete o di archiviazione e disastri naturali, come inondazioni, terremoti e uragani. Per proteggerti da un errore di ubicazione, devi disporre di cluster in due ubicazioni diverse il cui carico viene bilanciato dal programma di bilanciamento del carico esterno.</td>
    <td>[Configurazione di cluster altamente disponibili.](cs_clusters.html#planning_clusters)</td>
  </tr>
  <tr>
    <td>5</td>
    <td>Errore di regione</td>
    <td>Ogni regione è configurata con un programma di bilanciamento del carico altamente disponibile accessibile dall'endpoint API specifico della regione. Il programma di bilanciamento del carico instrada le richieste in entrata e in uscita ai cluster nelle ubicazioni regionali. La probabilità di un errore a livello dell'intera regione è bassa. Tuttavia, per tenere conto di questo errore, puoi configurare più cluster in regioni diverse e collegarli utilizzando un programma di bilanciamento del carico esterno. Nel caso in cui si verifichi un errore in un'intera regione, il cluster nell'altra regione può sostenere il carico di lavoro. <br/><br/><strong>Note:</strong> un cluster per più regioni richiede diverse risorse cloud e, a seconda della tua applicazione, può essere complesso e costoso. Controlla se hai bisogno di una configurazione per più regioni o se puoi gestire una potenziale interruzione del servizio. Se vuoi configurare un cluster per più regioni, assicurati che la tua applicazione e i dati possano essere ospitati in un'altra regione e che l'applicazione possa gestire la replica dei dati globali.</td>
    <td>[Configurazione di cluster altamente disponibili.](cs_clusters.html#planning_clusters)</td>
  </tr>
  <tr>
    <td>6a, 6b</td>
    <td>Errore di archiviazione</td>
    <td>In un'applicazione con stato, i dati svolgono un ruolo importante per mantenere operativa la tua applicazione. Vuoi assicurarti che i tuoi dati siano altamente disponibili in modo da poterli recuperare in caso di errore. In {{site.data.keyword.containershort_notm}}, puoi scegliere tra diverse opzioni per conservare i tuoi dati. Ad esempio, puoi eseguire il provisioning dell'archiviazione NFS utilizzando i volumi persistenti nativi di Kubernetes o memorizzare i tuoi dati utilizzando un servizio di database {{site.data.keyword.Bluemix_notm}}.</td>
    <td>[Pianificazione di dati altamente disponibili.](cs_storage.html#planning)</td>
  </tr>
  </tbody>
  </table>



