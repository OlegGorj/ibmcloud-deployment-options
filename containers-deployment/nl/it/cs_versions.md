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



# Azioni di aggiornamento e informazioni sulla versione
{: #cs_versions}

## Tipi di versione Kubernetes
{: #version_types}

{{site.data.keyword.containerlong}} supporta contemporaneamente più versioni di Kubernetes. Quando viene rilasciata una versione più recente (n), sono supportate fino a 2 versioni precedenti (n-2). Le versioni che sono più di 2 precedenti rispetto all'ultima (n-3) sono prima dichiarate obsolete e quindi non più supportate.
{:shortdesc}

Le versioni di Kubernetes al momento supportate sono:

- Più recente: 1.10.1
- Predefinita: 1.9.7
- Supportata: 1.8.11

**Versioni obsolete**: se i cluster sono in esecuzione su un Kubernetes deprecato, hai 30 giorni per controllare ed effettuare l'aggiornamento a una versione di Kubernetes supportata prima che la versione non sia più supportata. Durante il periodo di deprecazione, puoi eseguire comandi limitati nei cluster per aggiungere nodi di lavoro, ricaricare i nodi di lavoro e aggiornare il cluster. Non puoi creare nuovi cluster nella versione obsoleta.

**Versioni non supportate**: se esegui i cluster in una versione Kubernetes che non è supportata, [controlla i potenziali impatti](#version_types) per gli aggiornamenti e quindi [aggiorna il cluster](cs_cluster_update.html#update) immediatamente per continuare a ricevere il supporto e gli aggiornamenti di sicurezza importanti.

Per controllare la versione del server di un cluster, immetti il seguente comando.

```
kubectl version  --short | grep -i server
```
{: pre}

Output di esempio:

```
Server Version: v1.9.7+9d6e0610086578
```
{: screen}


## Tipi di aggiornamento
{: #update_types}

Per il tuo cluster Kubernetes sono disponibili tre tipi di aggiornamento: principale, secondario e patch.
{:shortdesc}

|Tipo di aggiornamento|Esempi di etichette di versione|Aggiornato da|Impatto
|-----|-----|-----|-----|
|Principale|1.x.x|Tu|Modifiche di funzionamento per i cluster, inclusi script o distribuzioni|
|Secondario|x.9.x|Tu|Modifiche di funzionamento per i cluster, inclusi script o distribuzioni|
|Patch|x.x.4_1510|IBM e tu|Patch Kubernetes e altri aggiornamenti del componente {{site.data.keyword.Bluemix_notm}} Provider come patch di sicurezza e del sistema operativo. IBM aggiorna i master automaticamente, ma tu applichi le patch ai nodi di lavoro.|
{: caption="Impatti degli aggiornamenti di Kubernetes" caption-side="top"}

Quando gli aggiornamenti diventano disponibili, riceverai una notifica quando visualizzerai le informazioni relative ai nodi di lavoro, ad esempio con i comandi `bx cs workers <cluster>` o `bx cs worker-get <cluster> <worker>`.
-  **Aggiornamenti principali e secondari**: per prima cosa, [aggiorna il tuo nodo master](cs_cluster_update.html#master) e poi [aggiorna i nodi di lavoro](cs_cluster_update.html#worker_node).  
   - Per impostazione predefinita, non puoi aggiornare un master Kubernetes a più di tre versioni secondarie in avanti. Ad esempio, se il master corrente è la versione 1.5 e vuoi aggiornare a 1.8, devi prima aggiornare a 1.7. Puoi forzare l'aggiornamento per continuare, ma l'aggiornamento di più di due versioni secondarie potrebbe causare risultati imprevisti.
   - Se utilizzi una versione della CLI `kubectl` che non corrisponde almeno alla versione `major.minor` dei tuoi cluster, potresti riscontrare risultati imprevisti. Assicurati di mantenere aggiornate le [versioni della CLI](cs_cli_install.html#kubectl) e dei cluster Kubernetes.
-  **Aggiornamenti patch**: controlla mensilmente per vedere se è disponibile un aggiornamento e usa il comando `bx cs worker-update` [](cs_cli_reference.html#cs_worker_update) o `bx cs worker-reload` [](cs_cli_reference.html#cs_worker_reload) per applicare queste patch di sicurezza e del sistema operativo. Per ulteriori informazioni, vedi [Changelog versione](cs_versions_changelog.html). 

<br/>

Queste informazioni riassumono gli aggiornamenti che potrebbero avere un impatto sulle applicazioni distribuite quando aggiorni un cluster a una nuova versione dalla versione precedente. 
-  [Azioni di migrazione](#cs_v110) Versione 1.10. 
-  [Azioni di migrazione](#cs_v19) Versione 1.9.
-  [Azioni di migrazione](#cs_v18) Versione 1.8.
-  [Archiviazione](#k8s_version_archive) di versioni obsolete o non supportate.

<br/>

Per un elenco completo delle modifiche, esamina le seguenti informazioni: 
* [Changelog Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md).
* [Changelog versione IBM](cs_versions_changelog.html).

## Versione 1.10
{: #cs_v110}

<p><img src="images/certified_kubernetes_1x10.png" style="padding-right: 10px;" align="left" alt="Questo badge indica la certificazione Kubernetes versione 1.10 per il servizio IBM Cloud Container."/> {{site.data.keyword.containerlong_notm}} è un prodotto certificato Kubernetes per la versione 1.10 con il programma Kubernetes Software Conformance Certification di CNCF. _Kubernetes® è un marchio registrato della Linux Foundation negli Stati Uniti e in altri paesi e viene utilizzato in seguito a una licenza dalla Linux Foundation._</p>

Rivedi le modifiche che potresti dover apportare quando esegui l'aggiornamento dalla precedente versione di Kubernetes alla versione 1.10.

**Importante**: prima di poter correttamente eseguire l'aggiornamento a Kubernetes 1.10, devi seguire la procedura elencata in [Preparazione dell'aggiornamento a Calico v3](#110_calicov3).

<br/>

### Aggiorna prima il master
{: #110_before}

<table summary="Aggiornamenti di Kubernetes per le versioni 1.10">
<caption>Modifiche da apportare prima di aggiornare il master a Kubernetes 1.10</caption>
<thead>
<tr>
<th>Tipo</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico v3</td>
<td>L'aggiornamento della versione Kubernetes 1.10 aggiorna anche Calico da v2.6.5 a v3.1.1. <strong>Importante</strong>: prima di poter correttamente eseguire l'aggiornamento a Kubernetes v1.10, devi seguire la procedura elencata in [Preparazione dell'aggiornamento a Calico v3](#110_calicov3).</td>
</tr>
<tr>
<td>Politica di rete del dashboard Kubernetes</td>
<td>In Kubernetes 1.10, la politica di rete <code>kubernetes-dashboard</code> nello spazio dei nomi <code>kube-system</code> blocca a tutti i pod di accedere al dashboard Kubernetes. Tuttavia, questo <strong>non</strong> influenza la possibilità di accedere al dashboard dalla console {{site.data.keyword.Bluemix_notm}} o utilizzando <code>kubectl proxy</code>. Se un pod necessita dell'accesso al dashboard, puoi aggiungere un'etichetta <code>kubernetes-dashboard-policy: allow</code> a uno spazio dei nomi e poi distribuire il pod allo spazio dei nomi.</td>
</tr>
<tr>
<td>Accesso API Kubelet</td>
<td>L'autorizzazione API Kubelet è ora delegata al <code>server API Kubernetes</code>. L'accesso all'API Kubelet si basa su <code>ClusterRoles</code> che concede l'autorizzazione ad accedere alle risorse secondarie <strong>node</strong>. Per impostazione predefinita, Kubernetes Heapster ha <code>ClusterRole</code> e <code>ClusterRoleBinding</code>. Tuttavia, se l'API Kubelet viene utilizzata da altri utenti o applicazioni, devi concedere loro l'autorizzazione ad utilizzare l'API. Fai riferimento alla documentazione Kubernetes in [Kubelet authorization![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/admin/kubelet-authentication-authorization/#kubelet-authorization).</td>
</tr>
<tr>
<td>Suite di cifratura </td>
<td>La suite di cifratura supportata dal <code>server API Kubernetes</code> e dall'API Kubelet è ora limitata a un sottoinsieme con codifica molto elevata (128 bit o più). Se hai risorse o un'automazione esistenti che utilizzano una cifratura meno sicura e che si basano sulla comunicazione con il <code>server API Kubernetes</code> o l'API Kubelet, abilita una cifratura più elevata prima di aggiornare il master.</td>
</tr>
</tbody>
</table>

### Aggiorna dopo il master
{: #110_after}

<table summary="Aggiornamenti Kubernetes per la versione 1.10">
<caption>Modifiche da apportare dopo aver aggiornato il master a Kubernetes 1.10</caption>
<thead>
<tr>
<th>Tipo</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico v3</td>
<td>Quando il cluster viene aggiornato, tutti i dati Calico esistenti applicati al cluster vengono automaticamente migrati per utilizzare la sintassi Calico v3. Per visualizzare, aggiungere o modificare le risorse Calico con la sintassi Calico v3, aggiorna la tua [configurazione della CLI Calico alla versione 3.1.1](#110_calicov3).</td>
</tr>
<tr>
<td>Indirizzo <code>ExternalIP</code> del nodo</td>
<td>Il campo <code>ExternalIP</code> di un nodo è ora impostato sul valore dell'indirizzo IP pubblico del nodo. Controlla e aggiorna tutte le risorse che dipendono da questo valore.</td>
</tr>
<tr>
<td><code>kubectl port-forward</code></td>
<td>Ora quando utilizzi il comando <code>kubectl port-forward</code>, non supporta più l'indicatore <code>-p</code>. Se i tuoi script si basano sul comportamento precedente, aggiornarli per sostituire l'indicatore <code>-p</code> con il nome del pod.</td>
</tr>
<tr>
<td>Volumi di dati API in sola lettura</td>
<td>Ora `secret`, `configMap`, `downwardAPI` e i volumi previsti sono montati in sola lettura.
Precedentemente, le applicazioni potevano scrivere i dati in questi volumi che potevano essere ripristinati
automaticamente dal sistema. Questa azione di migrazione è necessaria per risolvere la vulnerabilità di
sicurezza [CVE-2017-1002102![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=2017-1002102).
Se le tue applicazioni si basano sul comportamento non sicuro precedente, modificale.</td>
</tr>
</tbody>
</table>

### Preparazione dell'aggiornamento a Calico v3 
{: #110_calicov3}

Prima di iniziare, il tuo master del cluster e tutti i nodi di lavoro devono essere in esecuzione alla versione Kubernetes 1.8 o successiva e devono avere almeno un nodo di lavoro.

**Importante**: prepara l'aggiornamento a Calico v3 prima di aggiornare il master. Durante l'upgrade del master a Kubernetes v1.10, non vengono pianificati nuovi pod o nuove politiche di rete Kubernetes o Calico. La quantità di tempo dell'aggiornamento impedisce nuove variazioni di pianificazione. I cluster piccoli possono impiegare pochi minuti, con pochi minuti extra per ogni 10 nodi. I pod e le politiche di rete esistenti continuano ad essere in esecuzione.

1.  Verifica che i tuoi pod Calico siano integri.
    ```
    kubectl get pods -n kube-system -l k8s-app=calico-node -o wide
    ```
    {: pre}
    
2.  Se tutti i pod non sono nello stato **In esecuzione**, elimina il pod e attendi finché non è in uno stato **In esecuzione** prima di continuare.

3.  Se generi automaticamente le politiche o altre risorse Calico, aggiorna il tuo strumento di automazione per generare queste risorse con la [sintassi Calico v3 ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/).

4.  Se utilizzi [strongSwan](cs_vpn.html#vpn-setup) per la connettività VPN, il grafico Helm strongSwan 2.0.0 non funziona con Calico v3 o Kubernetes 1.10. [Aggiorna strongSwan](cs_vpn.html#vpn_upgrade) al grafico Helm 2.1.0, che è compatibile con le versioni precedenti Calico 2.6 e Kubernetes 1.7, 1.8 e 1.9.

5.  [Aggiorna il tuo master del cluster a Kubernetes v1.10](cs_cluster_update.html#master).

<br />


## Versione 1.9
{: #cs_v19}

<p><img src="images/certified_kubernetes_1x9.png" style="padding-right: 10px;" align="left" alt="Questo badge indica la certificazione Kubernetes versione 1.9 per il servizio IBM Cloud Container."/> {{site.data.keyword.containerlong_notm}} è un prodotto certificato Kubernetes per la versione 1.9 con il programma Kubernetes Software Conformance Certification di CNCF. _Kubernetes® è un marchio registrato della Linux Foundation negli Stati Uniti e in altri paesi e viene utilizzato in seguito a una licenza dalla Linux Foundation._</p>

Rivedi le modifiche che potresti dover apportare quando esegui l'aggiornamento dalla precedente versione di Kubernetes alla versione 1.9.

<br/>

### Aggiorna prima il master
{: #19_before}

<table summary="Aggiornamenti di Kubernetes per le versioni 1.9">
<caption>Modifiche da apportare prima di aggiornare il master a Kubernetes 1.9</caption>
<thead>
<tr>
<th>Tipo</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>API di ammissione webhook</td>
<td>L'API di ammissione, che viene utilizzata quando il server API richiama i webhook del controllo di ammissione, viene spostata da <code>admission.v1alpha1</code> a <code>admission.v1beta1</code>. <em>Devi eliminare tutti i webhook esistenti prima di eseguire l'upgrade del tuo cluster</em> e aggiornare i file di configurazione del webhook in modo che utilizzino l'API più recente. Questa modifica non è compatibile con le versioni precedenti.</td>
</tr>
</tbody>
</table>

### Aggiorna dopo il master
{: #19_after}

<table summary="Aggiornamenti di Kubernetes per le versioni 1.9">
<caption>Modifiche da apportare dopo aver aggiornato il master a Kubernetes 1.9</caption>
<thead>
<tr>
<th>Tipo</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Output `kubectl`</td>
<td>Ora, quando utilizzi il comando `kubectl` per specificare `-o custom-columns` e la colonna non viene trovata nell'oggetto, vedrai un output di `<none>`.<br>
In precedenza, l'operazione aveva esito negativo e visualizzavi un messaggio di errore `xxx is not found`. Se i tuoi script si basano sul comportamento precedente, aggiornarli.</td>
</tr>
<tr>
<td>`kubectl patch`</td>
<td>Ora, quando non viene effettuata alcuna modifica alla risorsa che è stata corretta, il comando `kubectl patch` ha esito negativo con `exit code 1`. Se i tuoi script si basano sul comportamento precedente, aggiornarli.</td>
</tr>
<tr>
<td>Autorizzazioni del dashboard Kubernetes</td>
<td>Agli utenti viene richiesto di accedere al dashboard Kubernetes con le proprie credenziali per visualizzare le risorse del cluster. L'autorizzazione RBAC `ClusterRoleBinding` predefinita del dashboard Kubernetes è stata rimossa. Per istruzioni, consulta [Avvio del dashboard Kubernetes](cs_app.html#cli_dashboard).</td>
</tr>
<tr>
<td>Volumi di dati API in sola lettura</td>
<td>Ora `secret`, `configMap`, `downwardAPI` e i volumi previsti sono montati in sola lettura.
Precedentemente, le applicazioni potevano scrivere i dati in questi volumi che potevano essere ripristinati
automaticamente dal sistema. Questa azione di migrazione è necessaria per risolvere la vulnerabilità di
sicurezza [CVE-2017-1002102](https://cve.mitre.org/cgi-bin/cvename.cgi?name=2017-1002102).
Se le tue applicazioni si basano sul comportamento non sicuro precedente, modificale.</td>
</tr>
<tr>
<td>Corruzioni e tolleranze</td>
<td>Le corruzioni `node.alpha.kubernetes.io/notReady` e `node.alpha.kubernetes.io/unreachable` sono state modificate rispettivamente in `node.kubernetes.io/not-ready` e `node.kubernetes.io/unreachable`.<br>
Nonostante le corruzioni siano state aggiornate automaticamente, devi aggiornare manualmente le tolleranze per queste corruzioni. Per ogni spazio dei nomi tranne `ibm-system` e `kube-system`, determina se è necessario modificare le tolleranze:<br>
<ul><li><code>kubectl get pods -n &lt;namespace&gt; -o yaml | grep "node.alpha.kubernetes.io/notReady" && echo "Action required"</code></li><li>
<code>kubectl get pods -n &lt;namespace&gt; -o yaml | grep "node.alpha.kubernetes.io/unreachable" && echo "Action required"</code></li></ul><br>
Se viene restituito `Action required`, modifica di conseguenza le tolleranze del pod.</td>
</tr>
<tr>
<td>API di ammissione webhook</td>
<td>Se hai eliminato i webhook esistenti prima di aver aggiornato il cluster, crea nuovi webhook.</td>
</tr>
</tbody>
</table>

<br />



## Versione 1.8
{: #cs_v18}

<p><img src="images/certified_kubernetes_1x8.png" style="padding-right: 10px;" align="left" alt="Questo badge indica la certificazione Kubernetes versione 1.8 per il servizio IBM Cloud Container."/> {{site.data.keyword.containerlong_notm}} è un prodotto Kubernetes certificato per la versione 1.8 nel programma CNCF Kubernetes Software Conformance Certification. _Kubernetes® è un marchio registrato della Linux Foundation negli Stati Uniti e in altri paesi e viene utilizzato in seguito a una licenza dalla Linux Foundation._</p>

Rivedi le modifiche che potresti dover apportare quando esegui l'aggiornamento dalla precedente versione di Kubernetes alla versione 1.8.

<br/>

### Aggiorna prima il master
{: #18_before}

<table summary="Aggiornamenti di Kubernetes per le versioni 1.8">
<caption>Modifiche da apportare prima di aggiornare il master a Kubernetes 1.8</caption>
<thead>
<tr>
<th>Tipo</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Nessuno</td>
<td>Non sono richieste modifiche prima di aggiornare il master</td>
</tr>
</tbody>
</table>

### Aggiorna dopo il master
{: #18_after}

<table summary="Aggiornamenti di Kubernetes per le versioni 1.8">
<caption>Modifiche da apportare dopo aver aggiornato il master a Kubernetes 1.8</caption>
<thead>
<tr>
<th>Tipo</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Accesso al dashboard Kubernetes</td>
<td>L'URL per accedere al dashboard Kubernetes nella versione 1.8 è stato modificato e la procedura di accesso include un nuovo passo di autenticazione. Per ulteriori informazioni, vedi [Accesso al dashboard Kubernetes](cs_app.html#cli_dashboard).</td>
</tr>
<tr>
<td>Autorizzazioni del dashboard Kubernetes</td>
<td>Per forzare gli utenti ad accedere con le proprie credenziali per visualizzare le risorse cluster nella versione 1.8, rimuovi l'autorizzazione RBAC ClusterRoleBinding della versione 1.7. Esegui `kubectl delete clusterrolebinding -n kube-system kubernetes-dashboard`.</td>
</tr>
<tr>
<td>`kubectl delete`</td>
<td>Il comando `kubectl delete` non ridimensiona più gli oggetti API del carico di lavoro, come i pod, prima che l'oggetto venga eliminato. Se richiedi che l'oggetto venga ridimensionato, utilizza [`kubectl scale ` ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/reference/kubectl/overview/#scale) prima di eliminare l'oggetto.</td>
</tr>
<tr>
<td>`kubectl run`</td>
<td>Il comando `kubectl run` deve utilizzare più indicatori per `--env` invece di argomenti separati da virgola. Ad esempio, esegui <code>kubectl run --env &lt;x&gt;=&lt;y&gt; --env &lt;z&gt;=&lt;a&gt;</code> e non <code>kubectl run --env &lt;x&gt;=&lt;y&gt;,&lt;z&gt;=&lt;a&gt;</code>. </td>
</tr>
<tr>
<td>`kubectl stop`</td>
<td>Il comando `kubectl stop` non è più disponibile.</td>
</tr>
<tr>
<td>Volumi di dati API in sola lettura</td>
<td>Ora `secret`, `configMap`, `downwardAPI` e i volumi previsti sono montati in sola lettura.
Precedentemente, le applicazioni potevano scrivere i dati in questi volumi che potevano essere ripristinati
automaticamente dal sistema. Questa azione di migrazione è necessaria per risolvere la vulnerabilità di
sicurezza [CVE-2017-1002102](https://cve.mitre.org/cgi-bin/cvename.cgi?name=2017-1002102).
Se le tue applicazioni si basano sul comportamento non sicuro precedente, modificale.</td>
</tr>
</tbody>
</table>

<br />



## Archivio
{: #k8s_version_archive}

### Versione 1.7 (obsoleta)
{: #cs_v17}

**A partire dal 22 maggio 2018, {{site.data.keyword.containershort_notm}} i cluster che eseguono la versione Kubernetes 1.7 sono obsoleti**. Dopo il 21 giugno 2018, i cluster della versione 1.7 non possono ricevere aggiornamenti di sicurezza o supporto a meno che non vengano aggiornati alla versione successiva più recente ([Kubernetes 1.8](#cs_v18)).

[Esamina il potenziale impatto](cs_versions.html#cs_versions) di ogni aggiornamento della versione di Kubernetes e quindi [aggiorna i tuoi cluster](cs_cluster_update.html#update) immediatamente.

Stai ancora eseguendo la versione Kubernetes 1.5? Controlla le seguenti informazioni per valutare l'impatto dell'aggiornamento dalla v1.5 alla v1.7. [Aggiorna i tuoi cluster](cs_cluster_update.html#update) alla v1.7 e poi aggiornali immediatamente all'ultima v1.8.
{: tip}

<p><img src="images/certified_kubernetes_1x7.png" style="padding-right: 10px;" align="left" alt="Questo badge indica la certificazione Kubernetes versione 1.7 per il servizio IBM Cloud Container."/> {{site.data.keyword.containerlong_notm}} è un prodotto Kubernetes certificato per la versione 1.7 nel programma CNCF Kubernetes Software Conformance Certification.</p>

Rivedi le modifiche che potresti dover apportare quando esegui l'aggiornamento dalla precedente versione di Kubernetes alla versione 1.7.

<br/>

#### Aggiorna prima il master
{: #17_before}

<table summary="Aggiornamenti Kubernetes per le versioni 1.7 e 1.6">
<caption>Modifiche da apportare prima di aggiornare il master a Kubernetes 1.7</caption>
<thead>
<tr>
<th>Tipo</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Archiviazione</td>
<td>Gli script di configurazione con `hostPath` e `mountPath` con riferimenti alle directory principali come `../to/dir` non sono consentiti. Modifica i percorsi con percorsi assoluti semplici, ad esempio, `/path/to/dir`.
<ol>
  <li>Determina se è necessario modificare i percorsi di archiviazione:</br>
  ```
  kubectl get pods --all-namespaces -o yaml | grep "\.\." && echo "Action required"
  ```
  </br>

  <li>Se viene restituito `Action required`, modifica i pod per fare riferimento al percorso assoluto prima di aggiornare tutti i tuoi nodi di lavoro. Se il pod appartiene a un'altra risorsa, ad esempio una distribuzione, modifica il [_PodSpec_ ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/api-reference/v1.7/#podspec-v1-core) all'interno di quella risorsa.
</ol>
</td>
</tr>
</tbody>
</table>

#### Aggiorna dopo il master
{: #17_after}

<table summary="Aggiornamenti Kubernetes per le versioni 1.7 e 1.6">
<caption>Modifiche da apportare dopo aver aggiornato il master a Kubernetes 1.7</caption>
<thead>
<tr>
<th>Tipo</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr>
<td>`apiVersion` della distribuzione</td>
<td>Dopo aver aggiornato il cluster da Kubernetes 1.5, utilizza `apps/v1beta1` per il campo `apiVersion` nei nuovi file YALM di `Deployment`. Continua a utilizzare `extensions/v1beta1` per le altre risorse, ad esempio `Ingress`.</td>
</tr>
<tr>
<td>'kubectl'</td>
<td>Dopo l'aggiornamento della CLI `kubectl` , questi comandi `kubectl create` devono utilizzare più indicatori anziché argomenti separati da virgola:<ul>
 <li>`role`
 <li>`clusterrole`
 <li>`rolebinding`
 <li>`clusterrolebinding`
 <li>`secret`
 </ul>
</br>  Ad esempio, esegui `kubectl create role --resource-name <x> --resource-name <y>` e non `kubectl create role --resource-name <x>,<y>`.</td>
</tr>
<tr>
<td>Politica di rete</td>
<td>L'annotazione `net.beta.kubernetes.io/network-policy` non è più disponibile.
<ol>
  <li>Determina se è necessario modificare le politiche:</br>
  ```
  kubectl get ns -o yaml | grep "net.beta.kubernetes.io/network-policy" | grep "DefaultDeny" && echo "Action required"
  ```
  <li>Se viene restituito `"Action required"`, aggiungi la seguente politica di rete a ogni spazio dei nomi Kubernetes elencato:</br>

  <pre class="codeblock">
  <code>
  kubectl create -n &lt;namespace&gt; -f - &lt;&lt;EOF
  kind: NetworkPolicy
  apiVersion: networking.k8s.io/v1
  metadata:
    name: default-deny
    namespace: &lt;namespace&gt;
  spec:
    podSelector: {}
  EOF
  </code>
  </pre>

  <li> Dopo aver aggiunto la politica di rete, rimuovi l'annotazione `net.beta.kubernetes.io/network-policy`:
  ```
  kubectl annotate ns <namespace> --overwrite "net.beta.kubernetes.io/network-policy-"
  ```
  </li></ol>
</td></tr>
<tr>
<td>Pianificazione affinità pod</td>
<td> L'annotazione `scheduler.alpha.kubernetes.io/affinity` è obsoleta.
<ol>
  <li>Per ogni spazio dei nomi tranne `ibm-system` e `kube-system`, determina se è necessario aggiornare la pianificazione dell'affinità dei pod:</br>
  ```
  kubectl get pods -n <namespace> -o yaml | grep "scheduler.alpha.kubernetes.io/affinity" && echo "Action required"
  ```
  </br></li>
  <li>Se viene restituito `"Action required"`, utilizza il campo _affinity_ [_PodSpec_ ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/api-reference/v1.7/#podspec-v1-core) invece dell'annotazione `scheduler.alpha.kubernetes.io/affinity`.</li>
</ol>
</td></tr>
<tr>
<td>RBAC per `default` `ServiceAccount`</td>
<td><p>L'amministratore `ClusterRoleBinding` per `default` `ServiceAccount` nello spazio dei nomi `default` è stato rimosso per migliorare la sicurezza del cluster. Le applicazioni eseguite nello spazio dei nomi `default` non dispongono più dei privilegi di amministratore del cluster nell'API Kubernetes e potrebbero riscontrare errori di autorizzazione `RBAC DENY`. Controlla la tua applicazione e il suo file `.yaml` per vedere se viene eseguita nello spazio dei nomi `default`, usa `default ServiceAccount` e accede all'API Kubernetes.</p>
<p>Se le tue applicazioni si basano su questi privilegi, [crea risorse di autorizzazione RBAC![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview) per le tue applicazioni.</p>
  <p>Quando aggiorni le politiche RBAC della tua applicazione, potresti voler ripristinare temporaneamente i valori di `default` precedenti. Copia, salva e applica i seguenti file con il comando `kubectl apply -f FILENAME`. <strong>Nota</strong>: considera il ripristino come un modo per darti il tempo di aggiornare tutte le risorse dell'applicazione e non come soluzione a lungo termine.</p>

<p><pre class="codeblock">
<code>
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
 name: admin-binding-nonResourceURLSs-default
subjects:
  - kind: ServiceAccount
    name: default
    namespace: default
roleRef:
 kind: ClusterRole
 name: admin-role-nonResourceURLSs
 apiGroup: rbac.authorization.k8s.io
---
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
 name: admin-binding-resourceURLSs-default
subjects:
  - kind: ServiceAccount
      name: default
      namespace: default
  roleRef:
   kind: ClusterRole
   name: admin-role-resourceURLSs
   apiGroup: rbac.authorization.k8s.io
</code>
</pre></p>
</td>
</tr>
<tr>
<td>Volumi di dati API in sola lettura</td>
<td>Ora `secret`, `configMap`, `downwardAPI` e i volumi previsti sono montati in sola lettura.
Precedentemente, le applicazioni potevano scrivere i dati in questi volumi che potevano essere ripristinati
automaticamente dal sistema. Questa azione di migrazione è necessaria per risolvere la vulnerabilità di
sicurezza [CVE-2017-1002102](https://cve.mitre.org/cgi-bin/cvename.cgi?name=2017-1002102).
Se le tue applicazioni si basano sul comportamento non sicuro precedente, modificale.</td>
</tr>
<tr>
<td>DNS pod StatefulSet</td>
<td>I pod StatefulSet perdono le loro voci DNS Kubernetes dopo l'aggiornamento del master. Per ripristinare le voci DNS, elimina i pod StatefulSet. Kubernetes ricrea i pod e ripristina automaticamente le voci DNS. Per ulteriori informazioni, vedi [Problemi di Kubernetes ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/kubernetes/kubernetes/issues/48327).</td>
</tr>
<tr>
<td>Tolleranze</td>
<td>L'annotazione `scheduler.alpha.kubernetes.io/tolerations` non è più disponibile.
<ol>
  <li>Per ogni spazio dei nomi tranne `ibm-system` e `kube-system`, determina se è necessario modificare le tolleranze:</br>
  ```
  kubectl get pods -n <namespace> -o yaml | grep "scheduler.alpha.kubernetes.io/tolerations" && echo "Action required"
  ```
  </br>

  <li>Se viene restituito `"Action required"`, utilizza il campo _tolerations_ [_PodSpec_ ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/api-reference/v1.7/#podspec-v1-core) invece dell'annotazione `scheduler.alpha.kubernetes.io/tolerations`
</ol>
</td></tr>
<tr>
<td>Corruzioni</td>
<td>L'annotazione `scheduler.alpha.kubernetes.io/taints` non è più disponibile.
<ol>
  <li>Determina se è necessario modificare le corruzioni:</br>
  ```
  kubectl get nodes -o yaml | grep "scheduler.alpha.kubernetes.io/taints" && echo "Action required"
  ```
  <li>Se viene restituito `"Action required"`, rimuovi l'annotazione `scheduler.alpha.kubernetes.io/taints` per ogni nodo:</br>
  `kubectl annotate nodes <node> scheduler.alpha.kubernetes.io/taints-`
  <li>Aggiungi una corruzione a ogni nodo:</br>
  `kubectl taint node <node> <taint>`
  </li></ol>
</td></tr>
</tbody>
</table>

<br />


### Versione 1.5 (non supportata)
{: #cs_v1-5}

A partire dal 4 aprile 2018, i cluster {{site.data.keyword.containershort_notm}} che eseguono [Kubernetes versione 1.5](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.5.md) non sono supportati. I cluster versione 1.5 non possono ricevere gli aggiornamenti di sicurezza o il supporto a meno che non vengano aggiornati alla versione successiva più recente ([Kubernetes 1.7](#cs_v17)).

[Esamina il potenziale impatto](cs_versions.html#cs_versions) di ogni aggiornamento della versione di Kubernetes e quindi [aggiorna i tuoi cluster](cs_cluster_update.html#update) immediatamente. Devi aggiornare da una versione alla successiva più recente, ad esempio da 1.5 a 1.7 o da 1.8 a 1.9. 
