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




# Esposizione delle applicazioni con le NodePort
{: #nodeport}

Rendi disponibile la tua applicazione inserita in un contenitore per l'accesso a Internet utilizzando l'indirizzo IP pubblico di qualsiasi nodo di lavoro in un cluster Kubernetes ed esponendo una NodePort. Utilizza questa opzione per la verifica in {{site.data.keyword.containerlong}} e per l'accesso pubblico a breve termine.
{:shortdesc}

## Gestione del traffico di rete utilizzando le NodePort
{: #planning}

Esponi una porta pubblica sul tuo nodo di lavoro e utilizza l'indirizzo IP pubblico del nodo di lavoro per accedere al tuo servizio nel cluster pubblicamente da Internet.
{:shortdesc}

Quando esponi la tua applicazione creando un servizio Kubernetes del tipo NodePort, vengono assegnati al servizio
una NodePort nell'intervallo 30000 - 32767 e un indirizzo
IP del cluster interno. Il servizio
NodePort funge da punto di ingresso per le richieste in entrata per la tua applicazione. La NodePort assegnata è pubblicamente esposta nelle impostazioni kubeproxy di ogni nodo di lavoro nel
cluster. Ogni nodo di lavoro inizia ad ascoltare dalla NodePort assegnata per le richieste in entrata per il
servizio. Per accedere al servizio da internet, puoi utilizzare l'indirizzo IP pubblico di ogni nodo di lavoro
che è stato assegnato durante la creazione del cluster e la NodePort nel formato `<IP_address>:<nodeport>`. In aggiunta all'indirizzo IP pubblico, è disponibile un servizio NodePort nell'indirizzo IP privato di un nodo di lavoro.

Il seguente diagramma mostra come viene diretta la comunicazione da Internet a un'applicazione quando è configurato un servizio NodePort:

<img src="images/cs_nodeport_planning.png" width="550" alt="Esponi un'applicazione in {{site.data.keyword.containershort_notm}} utilizzando NodePort" style="width:550px; border-style: none"/>

1. Viene inviata una richiesta alla tua applicazione utilizzando l'indirizzo IP pubblico del tuo nodo di lavoro e la NodePort sul nodo di lavoro.

2. La richiesta viene inoltrata automaticamente alla porta e all'indirizzo IP del cluster interno del servizio NodePort. L'indirizzo IP del cluster interno è accessibile solo all'interno del cluster.

3. `kube-proxy` instrada la richiesta al servizio NodePort Kubernetes per l'applicazione.

4. La richiesta viene inoltrata all'indirizzo IP privato del pod in cui è distribuita l'applicazione. Se nel cluster vengono distribuite più istanze dell'applicazione, il servizio NodePort instrada le richieste tra i pod dell'applicazione.

**Nota:** l'indirizzo IP pubblico del nodo di lavoro non è permanente. Quando un nodo di lavoro viene rimosso
o ricreato, a tale nodo viene assegnato un nuovo indirizzo IP pubblico. Puoi utilizzare NodePort per verificare l'accesso pubblico per la tua applicazione
o se l'accesso pubblico è richiesto solo per un breve periodo. Quando hai bisogno di un indirizzo IP pubblico stabile
e di una maggiore disponibilità per il tuo servizio, esponi la tua applicazione utilizzando un [Servizio LoadBalancer](cs_loadbalancer.html) o [Ingress](cs_ingress.html).

<br />


## Abilitazione dell'accesso pubblico a un'applicazione utilizzando un servizio NodePort
{: #config}

Puoi esporre la tua applicazione come un servizio Kubernetes NodePort per i cluster gratuito o standard.
{:shortdesc}

Se ancora non hai un'applicazione pronta, puoi utilizzare un'applicazione di esempio Kubernetes denominata [Guestbook ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/examples/blob/master/guestbook/all-in-one/guestbook-all-in-one.yaml).

1.  Nel file di configurazione della tua applicazione, definisci una sezione per il [servizio ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/services-networking/service/). **Nota**: per l'esempio Guestbook, esiste una sezione del servizio di frontend nel file di configurazione. Per rendere l'applicazione Guestbook disponibile all'esterno, aggiungi il tipo di NodePort e una NodePort compresa nell'intervallo 30000 - 32767 alla sezione del servizio di frontend.

    Esempio:

    ```
    apiVersion: v1
    kind: Service
    metadata:
      name: <my-nodeport-service>
      labels:
        <my-label-key>: <my-label-value>
    spec:
      selector:
        <my-selector-key>: <my-selector-value>
      type: NodePort
      ports:
       - port: <8081>
         # nodePort: <31514>

    ```
    {: codeblock}

    <table>
    <caption>Descrizione dei componenti del servizio NodePort</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icona idea"/> Descrizione dei componenti della sezione del servizio NodePort</th>
    </thead>
    <tbody>
    <tr>
    <td><code>metadata.name</code></td>
    <td>Sostituisci <code><em>&lt;my-nodeport-service&gt;</em></code> con un nome per il tuo servizio NodePort.<p>Ulteriori informazioni sulla [protezione delle tue informazioni personali](cs_secure.html#pi) quando utilizzi le risorse Kubernetes.</p></td>
    </tr>
    <tr>
    <td><code>metadata.labels</code></td>
    <td>Sostituisci <code><em>&lt;my-label-key&gt;</em></code> e <code><em>&lt;my-label-value&gt;</em></code> con l'etichetta che desideri utilizzare per il tuo servizio.</td>
    </tr>
    <tr>
      <td><code>spec.selector</code></td>
      <td>Sostituisci <code><em>&lt;my-selector-key&gt;</em></code> e <code><em>&lt;my-selector-value&gt;</em></code> con la coppia chiave/valore che hai utilizzato nella sezione <code>spec.template.metadata.labels</code> del tuo file yaml di distribuzione.
      </tr>
    <tr>
    <td><code>ports.port</code></td>
    <td>Sostituisci <code><em>&lt;8081&gt;</em></code> con la porta su cui è in ascolto il tuo servizio. </td>
     </tr>
     <tr>
     <td><code>ports.nodePort</code></td>
     <td>Facoltativo: sostituisci <code><em>&lt;31514&gt;</em></code> con un valore NodePort compreso tra 30000 e 32767. Non specificare una NodePort già utilizzata da un altro servizio. Se non assegni una NodePort, ne verrà assegnata una casuale automaticamente.<br><br>Per specificare una NodePort e vedere quali NodePort sono già in uso, immetti il seguente comando: <pre class="pre"><code>    kubectl get svc
    </code></pre><p>Ogni NodePort in utilizzo viene visualizzata nel campo **Porte**.</p></td>
     </tr>
     </tbody></table>

2.  Salva il file di configurazione aggiornato.

3.  Ripeti questi passi per creare un servizio NodePort per ogni applicazione che vuoi esporre su internet.

**Operazioni successive:**

Quando l'applicazione viene distribuita, puoi utilizzare l'indirizzo IP pubblico di qualsiasi nodo di lavoro e la NodePort per formare l'URL pubblico per accedere all'applicazione da un browser.

1.  Ottieni l'indirizzo IP pubblico per un nodo di lavoro nel cluster.

    ```
    bx cs workers <cluster_name>
    ```
    {: pre}

    Output:

    ```
    ID                                                Public IP   Private IP    Size     State    Status
    prod-dal10-pa215dcf5bbc0844a990fa6b0fcdbff286-w1  192.0.2.23  10.100.10.10  u2c.2x4  normal   Ready
    prod-dal10-pa215dcf5bbc0844a990fa6b0fcdbff286-w2  192.0.2.27  10.100.10.15  u2c.2x4  normal   Ready
    ```
    {: screen}

2.  Se è stato assegnato un NodePort casuale, scopri quale è stato assegnato.

    ```
    kubectl describe service <service_name>
    ```
    {: pre}

    Output:

    ```
    Name:                   <service_name>
    Namespace:              default
    Labels:                 run=<deployment_name>
    Selector:               run=<deployment_name>
    Type:                   NodePort
    IP:                     10.10.10.8
    Port:                   <unset> 8080/TCP
    NodePort:               <unset> 30872/TCP
    Endpoints:              172.30.171.87:8080
    Session Affinity:       None
    No events.
    ```
    {: screen}

    In questo esempio, la NodePort è `30872`.</br>
    **Nota:** se la sezione **Endpoints** mostra `<none>`, controlla `<selectorkey>` e `<selectorvalue>` che hai utilizzato nella sezione `spec.selector` del servizio NodePort. Assicurati che siano gli stessi della coppia _chiave/valore_ che hai utilizzato nella sezione `spec.template.metadata.labels` del tuo file yaml di distribuzione. 

3.  Forma l'URL con uno degli indirizzi IP pubblici del nodo di lavoro e la NodePort. Esempio: `http://192.0.2.23:30872`
