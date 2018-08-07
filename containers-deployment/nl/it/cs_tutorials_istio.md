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



# Esercitazione: installazione di Istio in {{site.data.keyword.containerlong_notm}}
{: #istio_tutorial}

[Istio](https://www.ibm.com/cloud/info/istio) è una piattaforma aperta per connettere, proteggere e gestire una rete di microservizi, nota anche come rete (mesh) di servizi, su piattaforme cloud come Kubernetes in {{site.data.keyword.containerlong}}. Con Istio, puoi gestire il traffico di rete, bilanciare il carico tra i microservizi, applicare le politiche di accesso, verificare l'identità del servizio e molto altro.
{:shortdesc}

In questa esercitazione, puoi vedere come installare Istio insieme a quattro microservizi per un'applicazione della libreria fittizia semplice denominata BookInfo. I microservizi includono una pagina web del prodotto, i dettagli sul libro, le recensioni e le valutazioni. Quando distribuisci i microservizi di BookInfo in un cluster {{site.data.keyword.containershort}} in cui è installato Istio, inserisci i proxy sidecar Istio Envoy nei pod di ogni microservizio.

**Nota**: alcune configurazioni e funzioni della piattaforma Istio sono ancora in fase di sviluppo e sono soggette a modifiche in base ai feedback utente. Permetti che passino alcuni mesi per il consolidamento dell'utilizzo di Istio nella produzione.  

## Obiettivi

-   Scarica e installa Istio nel tuo cluster
-   Distribuisci un'applicazione di esempio BookInfo
-   Inserisci i proxy sidecar Envoy nei pod dei quattro microservizi dell'applicazione per collegare i microservizi nella rete di servizi
-   Verifica il round robin e la distribuzione dell'applicazione BookInfo tramite le tre versioni del servizio di valutazione

## Tempo richiesto

30 minuti

## Destinatari

Questa esercitazione è destinata agli sviluppatori software e agli amministratori di rete che stanno utilizzando Istio per la prima volta.

## Prerequisiti

-  [Installa le CLI](cs_cli_install.html#cs_cli_install_steps). Istio richiede la versione Kubernetes 1.9 o superiore. Assicurati di installare la versione della CLI `kubectl` che corrisponde alla versione Kubernetes del tuo cluster.
-  [Crea un cluster](cs_clusters.html#clusters_cli) con una versione Kubernetes di 1.9 o superiore.
-  [Indirizza la CLI al tuo cluster](cs_cli_install.html#cs_cli_configure).

## Lezione 1: scarica e installa Istio
{: #istio_tutorial1}

Scarica e installa Istio nel tuo cluster.
{:shortdesc}

1. Scarica Istio direttamente da [https://github.com/istio/istio/releases ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/istio/istio/releases) o richiama l'ultima versione utilizzando curl:

   ```
   curl -L https://git.io/getLatestIstio | sh -
   ```
   {: pre}

2. Estrai i file di installazione.

3. Aggiungi il client `istioctl` al tuo PERCORSO. Ad esempio, immetti il seguente comando su un sistema MacOS o Linux:

   ```
   export PATH=$PWD/istio-0.4.0/bin:$PATH
   ```
   {: pre}

4. Modifica la directory con l'ubicazione del file Istio.

   ```
   cd filepath/istio-0.4.0
   ```
   {: pre}

5. Installa Istio sul cluster Kubernetes. Istio viene distribuito nello spazio dei nomi Kubernetes `istio-system`.

   ```
   kubectl apply -f install/kubernetes/istio.yaml
   ```
   {: pre}

   **Nota**: se hai bisogno di abilitare l'autenticazione TLS reciproca tra i sidecar, puoi invece installare il file `istio-auth`: `kubectl apply -f install/kubernetes/istio-auth.yaml`

6. Assicurati che i servizi Kubernetes `istio-pilot`, `istio-mixer` e `istio-ingress` siano stati completamente distribuiti prima di continuare.

   ```
   kubectl get svc -n istio-system
   ```
   {: pre}

   ```
   NAME            TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)                                                            AGE
   istio-ingress   LoadBalancer   172.21.xxx.xxx   169.xx.xxx.xxx   80:31176/TCP,443:30288/TCP                                         2m
   istio-mixer     ClusterIP      172.21.xxx.xxx     <none>           9091/TCP,15004/TCP,9093/TCP,9094/TCP,9102/TCP,9125/UDP,42422/TCP   2m
   istio-pilot     ClusterIP      172.21.xxx.xxx    <none>           15003/TCP,443/TCP                                                  2m
   ```
   {: screen}

7. Prima di continuare, assicurati che anche i pod corrispondenti `istio-pilot-*`, `istio-mixer-*`, `istio-ingress-*` e `istio-ca-*` siano stati completamente distribuiti.

   ```
   kubectl get pods -n istio-system
   ```
   {: pre}

   ```
   istio-ca-3657790228-j21b9           1/1       Running   0          5m
   istio-ingress-1842462111-j3vcs      1/1       Running   0          5m
   istio-pilot-2275554717-93c43        1/1       Running   0          5m
   istio-mixer-2104784889-20rm8        2/2       Running   0          5m
   ```
   {: screen}


Ottimo lavoro! Hai correttamente installato Istio nel tuo cluster. Successivamente, distribuisci l'applicazione di esempio BookInfo nel tuo cluster.


## Lezione 2: distribuisci l'applicazione BookInfo
{: #istio_tutorial2}

Distribuisci i microservizi dell'applicazione di esempio BookInfo nel tuo cluster Kubernetes.
{:shortdesc}

Questi quattro microservizi includono una pagina web del prodotto, i dettagli sul libro, le recensioni (con diverse versioni del microservizio di recensione) e le valutazioni. Puoi trovare tutti i file che sono utilizzati in questo esempio nella tua directory `samples/bookinfo` dell'installazione Istio.

Quando distribuisci BookInfo, vengono inseriti i proxy sidecar Envoy come dei contenitori nei pod dei tuoi microservizi dell'applicazione prima che i pod del microservizio vengano distribuiti. Istio utilizza una versione estesa del proxy Envoy per mediare tutto il traffico in entrata e in uscita per tutti i microservizi nella rete di servizi. Per ulteriori informazioni su Envoy, consulta la [documentazione di Istio ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://istio.io/docs/concepts/what-is-istio/overview.html#envoy).

1. Distribuisci l'applicazione BookInfo. Il comando `kube-inject` aggiunge Envoy al file `bookinfo.yaml` e utilizza questo file aggiornato per distribuire l'applicazione. Quando vengono distribuiti i microservizi dell'applicazione, viene distribuito anche il sidecar Envoy in ogni pod del microservizio.

   ```
   kubectl apply -f <(istioctl kube-inject -f samples/bookinfo/kube/bookinfo.yaml)
   ```
   {: pre}

2. Assicurati che i microservizi e loro pod corrispondenti siano stati distribuiti:

   ```
   kubectl get svc
   ```
   {: pre}

   ```
   NAME                       CLUSTER-IP   EXTERNAL-IP   PORT(S)              AGE
   details                    10.xxx.xx.xxx    <none>        9080/TCP             6m
   kubernetes                 10.xxx.xx.xxx     <none>        443/TCP              30m
   productpage                10.xxx.xx.xxx   <none>        9080/TCP             6m
   ratings                    10.xxx.xx.xxx    <none>        9080/TCP             6m
   reviews                    10.xxx.xx.xxx   <none>        9080/TCP             6m
   ```
   {: screen}

   ```
   kubectl get pods
   ```
   {: pre}

   ```
   NAME                                        READY     STATUS    RESTARTS   AGE
   details-v1-1520924117-48z17                 2/2       Running   0          6m
   productpage-v1-560495357-jk1lz              2/2       Running   0          6m
   ratings-v1-734492171-rnr5l                  2/2       Running   0          6m
   reviews-v1-874083890-f0qf0                  2/2       Running   0          6m
   reviews-v2-1343845940-b34q5                 2/2       Running   0          6m
   reviews-v3-1813607990-8ch52                 2/2       Running   0          6m
   ```
   {: screen}

3. Per verificare la distribuzione dell'applicazione, richiama l'indirizzo pubblico del tuo cluster.

    * Se stai utilizzando un cluster standard, immetti il seguente comando per richiamare la porta e l'IP Ingress del tuo cluster:

       ```
       kubectl get ingress
       ```
       {: pre}

       Output di esempio:

       ```
       NAME      HOSTS     ADDRESS          PORTS     AGE
       gateway   *         169.xx.xxx.xxx   80        3m
       ```
       {: screen}

       L'indirizzo Ingress risultante per questo esempio è `169.48.221.218:80`. Esporta l'indirizzo come l'URL del gateway con il seguente comando. Visualizzerai l'URL del gateway nel passo successivo per accedere alla pagina del prodotto BookInfo.

       ```
       export GATEWAY_URL=169.xx.xxx.xxx:80
       ```
       {: pre}

    * Se stai utilizzando un cluster gratuito, devi utilizzare un IP pubblico del nodo di lavoro e la NodePort. Immetti il seguente comando per richiamare l'IP pubblico del nodo di lavoro:

       ```
       bx cs workers <cluster_name_or_ID>
       ```
       {: pre}

       Esporta l'IP pubblico del nodo di lavoro come l'URL del gateway con il seguente comando. Visualizzerai l'URL del gateway nel passo successivo per accedere alla pagina del prodotto BookInfo.

       ```
       export GATEWAY_URL=<worker_node_public_IP>:$(kubectl get svc istio-ingress -n istio-system -o jsonpath='{.spec.ports[0].nodePort}')
       ```
       {: pre}

4. Esegui curl per la variabile `GATEWAY_URL` per verificare che BookInfo sia in esecuzione. Una risposta `200` significa che BookInfo è in esecuzione correttamente con Istio.

   ```
   curl -I http://$GATEWAY_URL/productpage
   ```
   {: pre}

5. In un browser, passa a `http://$GATEWAY_URL/productpage` per visualizzare la pagina web di BookInfo. 

6. Prova ad aggiornare la pagina diverse volte. Differenti versioni della sezione delle recensioni ruotano e visualizzano le stelle rosse, nere e nessuna stella.

Ottimo lavoro! Hai correttamente distribuito l'applicazione di esempio BookInfo con i sidecar Istio Envoy. Successivamente, puoi ripulire le tue risorse o continuare con ulteriori esercitazioni per esplorare ulteriormente Istio. 

## Ripulitura
{: #istio_tutorial_cleanup}

Se hai terminato di utilizzare Istio e non vuoi [continuare l'esplorazione](#istio_tutorial_whatsnext), puoi ripulire le risorse Istio nel tuo cluster.
{:shortdesc}

1. Elimina tutti i servizi, i pod e le distribuzioni di BookInfo nel cluster.

   ```
   samples/bookinfo/kube/cleanup.sh
   ```
   {: pre}

2. Disinstalla Istio.

   ```
   kubectl delete -f install/kubernetes/istio.yaml
   ```
   {: pre}

## Operazioni successive
{: #istio_tutorial_whatsnext}

Per esplorare ulteriormente Istio, puoi trovare ulteriori guide nella [documentazione di Istio ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://istio.io/). 

* [Intelligent Routing ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://istio.io/docs/guides/intelligent-routing.html): questo esempio mostra come instradare il traffico a una versione specifica dei microservizi delle valutazioni e delle recensioni di BookInfo utilizzando le funzionalità di gestione del traffico di Istio.

* [In-Depth Telemetry ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://istio.io/docs/guides/telemetry.html): questo esempio include come ottenere metriche, log e tracce uniformi tra i microservizi di BookInfo utilizzando Istio Mixer e il proxy Envoy.
