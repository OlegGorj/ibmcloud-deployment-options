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



# Pianificazione della rete con i servizi NodePort, LoadBalancer o Ingress
{: #planning}

Quando crei un cluster Kubernetes in {{site.data.keyword.containerlong}}, ogni cluster deve essere collegato a una VLAN pubblica. La VLAN pubblica
determina l'indirizzo IP pubblico assegnato a un nodo di lavoro durante la creazione del cluster.
{:shortdesc}

L'interfaccia di rete pubblica dei nodi di lavoro nei cluster gratuito e standard è protetta dalle politiche di rete Calico. Queste politiche bloccano la maggior parte del traffico in entrata per impostazione predefinita. Tuttavia, è consentito il traffico in entrata necessario al funzionamento di Kubernetes, così come le connessioni ai servizi NodePort, LoadBalancer e Ingress. Per ulteriori informazioni su queste politiche, incluso su come modificarle, vedi [Politiche di rete](cs_network_policy.html#network_policies).

|Tipo di cluster|Gestore della VLAN pubblica del cluster|
|------------|------------------------------------------|
|Cluster gratuiti|{{site.data.keyword.IBM_notm}}|
|Cluster standard|Tu nel tuo account dell'infrastruttura IBM Cloud (SoftLayer)|
{: caption="Gestori delle VLAN pubbliche per tipo di cluster" caption-side="top"}

Per ulteriori informazioni sulla comunicazione della rete in cluster tra i nodi di lavoro e i pod, consulta [Rete in cluster](cs_secure.html#in_cluster_network). Per ulteriori informazioni sulla connessione sicura delle applicazioni eseguite in un cluster Kubernetes a una rete in loco o ad applicazioni esterne al cluster, vedi [Configurazione della connettività VPN](cs_vpn.html).

## Consentire l'accesso pubblico alle applicazioni
{: #public_access}

Per rendere pubblicamente disponibile un'applicazione su Internet, devi aggiornare il tuo file di configurazione prima di distribuire l'applicazione in un cluster.
{:shortdesc}

*Piano di dati Kubernetes in {{site.data.keyword.containershort_notm}}*

![{{site.data.keyword.containerlong_notm}} architettura Kubernetes](images/networking.png)

Il diagramma mostra come Kubernetes esegue il traffico di rete dell'utente in {{site.data.keyword.containershort_notm}}. Per rendere la tua applicazione accessibile da internet, i modi variano a seconda se hai creato un cluster gratuito o standard.

<dl>
<dt><a href="cs_nodeport.html#planning" target="_blank">Servizio NodePort</a> (cluster gratuito e standard)</dt>
<dd>
 <ul>
  <li>Esponi una porta pubblica su ogni nodo di lavoro e utilizza l'indirizzo IP pubblico di ognuno di questi nodi
per accedere pubblicamente al tuo servizio nel cluster.</li>
  <li>Iptables è una funzione del kernel Linux che bilancia il carico delle richieste tra i pod dell'applicazione, offre un instradamento di rete ad alte prestazioni e fornisce il controllo dell'accesso alla rete.</li>
  <li>L'indirizzo IP pubblico del nodo di lavoro non è
permanente. Quando un nodo di lavoro viene rimosso
o ricreato, a tale nodo viene assegnato un nuovo indirizzo IP pubblico.</li>
  <li>Il servizio NodePort è migliore per la verifica dell'accesso pubblico. Può anche essere utilizzato se hai bisogno di un accesso pubblico solo per un breve periodo di tempo.</li>
 </ul>
</dd>
<dt><a href="cs_loadbalancer.html#planning" target="_blank">Servizio LoadBalancer</a> (solo cluster standard)</dt>
<dd>
 <ul>
  <li>Ogni cluster standard viene fornito con quattro indirizzi IP pubblici portatili e quattro indirizzi IP privati portatili che puoi utilizzare per creare un programma di bilanciamento del carico TCP/UDP esterno per la tua applicazione.</li>
  <li>Iptables è una funzione del kernel Linux che bilancia il carico delle richieste tra i pod dell'applicazione, offre un instradamento di rete ad alte prestazioni e fornisce il controllo dell'accesso alla rete.</li>
  <li>L'indirizzo IP pubblico portatile che viene assegnato al programma di bilanciamento del carico è permanente
e non cambia quando un nodo di lavoro viene ricreato nel cluster.</li>
  <li>Puoi personalizzare il tuo programma di bilanciamento del carico
esponendo una qualsiasi porta richiesta dalla tua applicazione.</li></ul>
</dd>
<dt><a href="cs_ingress.html#planning" target="_blank">Ingress</a> (solo cluster
standard)</dt>
<dd>
 <ul>
  <li>Esponi più applicazioni nel tuo cluster creando un programma di bilanciamento del carico HTTP o HTTPS, TCP o UDP esterno. Il programma di bilanciamento del carico utilizza un punto di ingresso pubblico protetto e univoco per instradare le richieste in entrata alle tue applicazioni. </li>
  <li>Puoi utilizzare una rotta pubblica per esporre più applicazioni nel tuo cluster come servizi.</li>
  <li>Ingress è composto da due componenti:
   <ul>
    <li>La risorsa Ingress definisce
le regole su come instradare e bilanciare il carico delle richieste in entrata per un'applicazione.</li>
    <li>Il programma di bilanciamento del carico dell'applicazione (ALB) rimane in ascolto delle richieste di servizio HTTP o HTTPS, TCP o UDP in entrata. Inoltra le richieste tra i pod delle applicazioni in base alle regole che hai definito nella risorsa Ingress.</li>
   </ul>
  <li>Utilizza Ingress per implementare il tuo ALB con regole di instradamento personalizzate e hai bisogno della terminazione SSL per le tue applicazioni. </li>
 </ul>
</dd></dl>

Per scegliere la migliore opzione di rete per la tua applicazione, puoi seguire questa struttura ad albero delle decisioni. Per informazioni sulla pianificazione e istruzioni di configurazione, fai clic sull'opzione del servizio di rete che hai scelto. 

<img usemap="#networking_map" border="0" class="image" src="images/networkingdt.png" width="500px" alt="Questa immagine ti guida nella scelta della migliore opzione di rete per la tua applicazione. Se questa immagine non viene visualizzata, le informazioni possono ancora essere trovate nella documentazione." style="width:500px;" />
<map name="networking_map" id="networking_map">
<area href="/docs/containers/cs_nodeport.html" alt="Servizio Nodeport" shape="circle" coords="52, 283, 45"/>
<area href="/docs/containers/cs_loadbalancer.html" alt="Servizio LoadBalancer" shape="circle" coords="247, 419, 44"/>
<area href="/docs/containers/cs_ingress.html" alt="Servizio Ingress" shape="circle" coords="445, 420, 45"/>
</map>
