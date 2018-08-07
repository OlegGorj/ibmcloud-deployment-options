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




# Teilnetze für Cluster konfigurieren
{: #subnets}

Ändern Sie den Pool der verfügbaren, portierbaren, öffentlichen oder privaten IP-Adressen, indem Sie Ihrem Kubernetes-Cluster in {{site.data.keyword.containerlong}} Teilnetze hinzufügen.
{:shortdesc}

Sie können in {{site.data.keyword.containershort_notm}} stabile, portierbare IP-Adressen für Kubernetes-Services hinzufügen, indem Sie dem Cluster Teilnetze hinzufügen. In diesem Fall werden Teilnetze nicht mit Netzmasken verwendet, um Konnektivität zwischen mehreren Clustern zu erstellen. Stattdessen werden Teilnetze eingesetzt, um feste IP-Adressen für einen Service aus einem Cluster bereitzustellen, über die auf den Service zugegriffen werden kann.

<dl>
  <dt>Das Erstellen eines Clusters schließt standardmäßig das Erstellen von Teilnetzen ein.</dt>
  <dd>Wenn Sie einen Standardcluster erstellen, stellt {{site.data.keyword.containershort_notm}} automatisch die folgenden Teilnetze bereit:
    <ul><li>Ein primäres öffentliches Teilnetz, das während der Clustererstellung öffentliche IP-Adressen für Workerknoten festlegt.</li>
    <li>Ein primäres privates Teilnetz, das während der Clustererstellung private IP-Adressen für Workerknoten festlegt.</li>
    <li>Ein portierbares öffentliches Teilnetz, das fünf öffentliche IP-Adressen für Ingress-Netzservices und Netzservices für die Lastausgleichsfunktion bereitstellt.</li>
    <li>Ein portierbares privates Teilnetz, das fünf private IP-Adressen für Ingress-Netzservices und Netzservices für die Lastausgleichsfunktion bereitstellt.</li></ul>
      Portierbare öffentliche und private IP-Adressen sind statisch und ändern sich nicht, wenn ein Workerknoten entfernt wird. Für jedes Teilnetz wird eine portierbare öffentliche und eine portierbare private IP-Adresse für die standardmäßig verwendeten [Ingress-Lastausgleichsfunktionen für Anwendungen](cs_ingress.html) verwendet. Sie können die Ingress-Lastausgleichsfunktion für Anwendungen verwenden, um mehrere Apps in Ihrem Cluster zugänglich zu machen. Die verbleibenden vier portierbaren öffentlichen und vier portierbaren privaten IP-Adressen können verwendet werden, um einzelne Apps für das öffentliche oder private Netz verfügbar zu machen, indem Sie einen [Lastausgleichsservice erstellen](cs_loadbalancer.html).</dd>
  <dt>[Bestellen und Verwalten Ihrer eigenen Teilnetze](#custom)</dt>
  <dd>Sie können vorhandene portierbare Teilnetze in Ihrem IBM Cloud Infrastructure-Konto (SoftLayer) bestellen und verwalten, statt die automatisch bereitgestellten Teilnetze zu verwenden. Verwenden Sie diese Option, um stabile statische IP-Adressen beizubehalten, obwohl Cluster entfernt oder erstellt werden, oder um größere Blöcke von IP-Adressen zu bestellen. Erstellen Sie zunächst ein Cluster ohne Teilnetze mithilfe des Befehls `cluster-create --no-subnet` und fügen Sie dann mit dem Befehl `cluster-subnet-add` das Teilnetz zum Cluster hinzu. </dd>
</dl>

**Hinweis:** Portierbare öffentliche IP-Adressen werden monatlich berechnet. Wenn Sie nach der Bereitstellung Ihres Clusters portierbare öffentliche IP-Adressen entfernen, müssen Sie trotzdem die monatliche Gebühr bezahlen, auch wenn sie sie nur über einen kurzen Zeitraum genutzt haben.

## Weitere Teilnetze für Ihren Cluster anfordern
{: #request}

Sie können stabile portierbare öffentliche oder private IP-Adressen zum Cluster hinzufügen, wenn Sie dem Cluster Teilnetze zuweisen.
{:shortdesc}

**Hinweis:** Wenn Sie ein Teilnetz in einem Cluster verfügbar machen, werden IP-Adressen dieses Teilnetzes zum Zweck von Clusternetzen verwendet. Vermeiden Sie IP-Adresskonflikte, indem Sie ein Teilnetz mit nur einem Cluster verwenden. Verwenden Sie kein Teilnetz für mehrere Cluster oder für andere
Zwecke außerhalb von {{site.data.keyword.containershort_notm}} gleichzeitig.

Führen Sie zunächst den folgenden Schritt aus: [Richten Sie Ihre CLI](cs_cli_install.html#cs_cli_configure) auf Ihren Cluster aus.

Gehen Sie wie folgt vor, um ein Teilnetz in einem IBM Cloud Infrastructure (SoftLayer)-Konto zu erstellen und in einem bestimmten Cluster verfügbar zu machen:

1. Stellen Sie ein neues Teilnetz bereit.

    ```
    bx cs cluster-subnet-create <clustername_oder_-id> <teilnetzgröße> <VLAN_ID>
    ```
    {: pre}

    <table>
    <caption>Erklärung der Bestandteile dieses Befehls</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der Bestandteile dieses Befehls</th>
    </thead>
    <tbody>
    <tr>
    <td><code>cluster-subnet-create</code></td>
    <td>Der Befehl zum Bereitstellen eines Teilnetzes für Ihren Cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;clustername_oder_-id&gt;</em></code></td>
    <td>Ersetzen Sie <code>&lt;clustername_oder_-id&gt;</code> durch den Namen oder die ID des Clusters.</td>
    </tr>
    <tr>
    <td><code><em>&lt;teilnetzgröße&gt;</em></code></td>
    <td>Ersetzen Sie <code>&lt;teilnetzgröße&gt;</code> durch die Anzahl von IP-Adressen, die Sie aus Ihrem portierbaren Teilnetz hinzufügen möchten. Gültige Werte sind 8, 16, 32 oder 64. <p>**Hinweis:** Wenn Sie portierbare IP-Adressen für Ihr Teilnetz hinzufügen, werden drei IP-Adressen verwendet, um clusterinterne Netze einzurichten. Diese stehen folglich nicht für Ihre Lastausgleichsfunktionen für Anwendungen oder für das Erstellen eines Lastausgleichsservice bereit. Wenn Sie beispielsweise acht portierbare öffentliche IP-Adressen anfordern, können Sie fünf verwenden, um die Apps öffentlich verfügbar zu machen.</p> </td>
    </tr>
    <tr>
    <td><code><em>&lt;VLAN_ID&gt;</em></code></td>
    <td>Ersetzen Sie <code>&lt;VLAN_ID&gt;</code> durch die ID des öffentlichen oder privaten VLANs, in dem Sie die portierbaren öffentlichen oder privaten IP-Adressen zuweisen möchten. Sie müssen das öffentliche oder private VLAN auswählen, mit dem ein vorhandener Workerknoten verbunden ist. Führen Sie den Befehl <code>bx cs worker-get &lt;worker-id&gt;</code> aus, um das öffentliche oder private VLAN für einen Workerknoten zu überprüfen. </td>
    </tr>
    </tbody></table>

2.  Überprüfen Sie, ob das Teilnetz erfolgreich erstellt und zu Ihrem Cluster hinzugefügt wurde. Das Teilnetz-CIDR wird im Abschnitt **VLANs** aufgelistet.

    ```
    bx cs cluster-get --showResources <clustername_oder_-id>
    ```
    {: pre}

3. Optional: [Routing zwischen Teilnetzen auf demselben VLAN aktivieren](#vlan-spanning).

<br />


## Angepasste und vorhandene Teilnetze in Kubernetes-Clustern hinzufügen
{: #custom}

Sie können vorhandene portierbare öffentliche oder private Teilnetze zu Ihrem Kubernetes-Cluster hinzufügen oder Teilnetze aus einem gelöschten Cluster wiederverwenden.
{:shortdesc}

Vorbereitende Schritte
- [Richten Sie Ihre CLI](cs_cli_install.html#cs_cli_configure) (Befehlszeilenschnittstelle) auf Ihren Cluster aus.
- Wenn Sie Teilnetze aus einem Cluster wiederverwenden möchten, den Sie nicht mehr benötigen, löschen Sie den nicht benötigten Cluster. Die Teilnetze werden innerhalb von 24 Stunden gelöscht.

   ```
   bx cs cluster-rm <clustername_oder_-id
   ```
   {: pre}

Gehen Sie wie folgt vor, um ein Teilnetz in Ihrem Portfolio von IBM Cloud Infrastructure (SoftLayer) mit angepassten Firewallregeln oder verfügbaren IP-Adressen zu verwenden.

1.  Geben Sie das Teilnetz an, das Sie verwenden möchten. Beachten Sie die ID des Teilnetzes und die VLAN-ID. In diesem Beispiel lautet die Teilnetz-ID `1602829` und die VLAN-ID `2234945`.

    ```
    bx cs subnets
    ```
    {: pre}

    ```
    Getting subnet list...
    OK
    ID        Network             Gateway          VLAN ID   Type      Bound Cluster
    1550165   10.xxx.xx.xxx/26    10.xxx.xx.xxx    2234947   private
    1602829   169.xx.xxx.xxx/28   169.xx.xxx.xxx   2234945   public

    ```
    {: screen}

2.  Bestätigen Sie die Position, an der sich das VLAN befindet. In diesem Beispiel lautet die Position 'dal10'.

    ```
    bx cs vlans dal10
    ```
    {: pre}

    ```
    Getting VLAN list...
    OK
    ID        Name   Number   Type      Router         Supports Virtual Workers
    2234947          1813     private   bcr01a.dal10   true
    2234945          1618     public    fcr01a.dal10   true
    ```
    {: screen}

3.  Erstellen Sie einen Cluster mithilfe der Position und der VLAN-ID, die Sie angegeben haben. Wenn Sie ein vorhandenes Teilnetz wiederverwenden möchten, schließen Sie das Flag `--no-subnet` ein, um zu vermeiden, dass ein neues portierbares öffentliches und ein neues portierbares privates IP-Teilnetz automatisch erstellt wird.

    ```
    bx cs cluster-create --location dal10 --machine-type u2c.2x4 --no-subnet --public-vlan 2234945 --private-vlan 2234947 --workers 3 --name mein_cluster
    ```
    {: pre}

4.  Prüfen Sie, ob die Erstellung des Clusters angefordert wurde.

    ```
    bx cs clusters
    ```
    {: pre}

    **Hinweis:** Es kann bis zu 15 Minuten dauern, bis die Workerknotenmaschinen angewiesen werden und der Cluster in Ihrem
Konto eingerichtet und bereitgestellt wird.

    Nach Abschluss der Bereitstellung Ihres Clusters wird der Status des
Clusters in **deployed** (Bereitgestellt) geändert.

    ```
    Name         ID                                   State      Created          Workers   Location   Version
    mycluster    aaf97a8843a29941b49a598f516da72101   deployed   20170201162433   3         dal10      1.9.7
    ```
    {: screen}

5.  Überprüfen Sie den Status der Workerknoten.

    ```
    bx cs workers <cluster>
    ```
    {: pre}

    Wenn die Workerknoten bereit sind, wechselt der Zustand (State) zu **Normal**, während für den Status die Angabe **Bereit** angezeigt wird. Wenn der Knotenstatus **Bereit** lautet, können Sie auf den Cluster zugreifen.

    ```
    ID                                                  Public IP        Private IP     Machine Type   State      Status   Location   Version
    prod-dal10-pa8dfcc5223804439c87489886dbbc9c07-w1    169.xx.xxx.xxx   10.xxx.xx.xxx  free           normal     Ready    dal10      1.9.7
    ```
    {: screen}

6.  Fügen Sie Ihrem Cluster das Teilnetz hinzu, indem Sie die Teilnetz-ID angeben. Wenn Sie ein Teilnetz für einen Cluster verfügbar machen, wird eine Kubernetes-Konfigurationszuordnung für Sie erstellt, die alle verfügbaren portierbaren öffentlichen IP-Adressen enthält, die Sie verwenden können. Wenn für Ihr Cluster keine Lastausgleichsfunktionen für Anwendungen vorhanden sind, werden eine portierbare, öffentliche und eine portierbare, private IP-Adresse automatisch verwendet, um die öffentlichen und privaten Lastausgleichsfunktionen für Anwendungen zu erstellen. Alle anderen portierbaren öffentlichen und privaten IP-Adressen können zum Erstellen von Lastenausgleichsservices für die Apps verwendet werden.

    ```
    bx cs cluster-subnet-add mycluster 807861
    ```
    {: pre}

7. Optional: [Routing zwischen Teilnetzen auf demselben VLAN aktivieren](#vlan-spanning).

<br />


## Vom Benutzer verwaltete Teilnetze und IP-Adressen zu Kubernetes-Clustern hinzufügen
{: #user_managed}

Stellen Sie ein Teilnetz aus einem lokalen Netz zur Verfügung, auf das über {{site.data.keyword.containershort_notm}} zugegriffen werden soll. Anschließend können Sie private IP-Adressen aus diesem Teilnetz zu den Lastenausgleichsservices im Kubernetes-Cluster hinzufügen.
{:shortdesc}

Voraussetzungen:
- Vom Benutzer verwaltete Teilnetze können nur zu privaten VLANs hinzugefügt werden.
- Die Begrenzung für die Länge des Teilnetzpräfix beträgt /24 bis /30. Beispiel: `169.xx.xxx.xxx/24` gibt 253 verwendbare private IP-Adressen an, während `169.xx.xxx.xxx/30` eine verwendbare private IP-Adresse angibt.
- Die erste IP-Adresse im Teilnetz muss als Gateway für das Teilnetz verwendet werden.

Vorbemerkungen:
- Konfigurieren Sie vorab das Routing des Netzverkehrs in das externe Teilnetz bzw. aus dem externen Teilnetz.
- Vergewissern Sie sich, dass Sie über VPN-Konnektivität zwischen dem Netzgateway des lokalen Rechenzentrums und entweder der Virtual Router Appliance des privaten Netzes oder dem aktiven StrongSwan-VPN-Service in Ihrem Cluster verfügen. Weitere Informationen finden Sie unter [VPN-Konnektivität einrichten](cs_vpn.html).

Gehen Sie wie folgt vor, um ein lokales Netz im Unternehmen hinzuzufügen:

1. Zeigen Sie die ID des privaten VLAN Ihres Clusters an. Suchen Sie den Abschnitt **VLANs**. Geben Sie im Feld für die Verwaltung durch den Benutzer die VLAN-ID mit _false_ an.

    ```
    bx cs cluster-get --showResources <clustername>
    ```
    {: pre}

    ```
    VLANs
    VLAN ID   Subnet CIDR       Public   User-managed
    2234947   10.xxx.xx.xxx/29  false    false
    2234945   169.xx.xxx.xxx/29 true     false
    ```
    {: screen}

2. Fügen Sie das externe Teilnetz zu Ihrem privaten VLAN hinzu. Die portierbaren privaten IP-Adressen werden zur Konfigurationszuordnung des Clusters hinzugefügt.

    ```
    bx cs cluster-user-subnet-add <clustername> <teilnetz_CIDR> <VLAN-ID>
    ```
    {: pre}

    Beispiel:

    ```
    bx cs cluster-user-subnet-add mein_cluster 10.xxx.xx.xxx/24 2234947
    ```
    {: pre}

3. Überprüfen Sie, ob das vom Benutzer bereitgestellte Teilnetz hinzugefügt wurde. Das Feld für die Verwaltung durch den Benutzer ist auf _true_ gesetzt.

    ```
    bx cs cluster-get --showResources <clustername>
    ```
    {: pre}

    ```
    VLANs
    VLAN ID   Subnet CIDR       Public   User-managed
    2234947   10.xxx.xx.xxx/29  false    false
    2234945   169.xx.xxx.xxx/29 true   false
    2234947   10.xxx.xx.xxx/24  false    true
    ```
    {: screen}

4. Optional: [Routing zwischen Teilnetzen auf demselben VLAN aktivieren](#vlan-spanning).

5. Fügen Sie einen privaten Lastausgleichsservice oder eine private Ingress-Lastausgleichsfunktion für Anwendungen hinzu, um über das private Netz auf Ihre App zuzugreifen. Wenn Sie eine private IP-Adresse aus dem Teilnetz verwenden möchten, die Sie hinzugefügt haben, müssen Sie eine IP-Adresse angeben. Andernfalls wird eine IP-Adresse nach dem Zufallsprinzip aus den Teilnetzen von IBM Cloud Infrastructure (SoftLayer) oder aus den vom Benutzer bereitgestellten Teilnetzen im privaten VLAN ausgewählt. Weitere Informationen finden Sie unter [Öffentlichen oder privaten Zugriff auf eine App mithilfe eines LoadBalancer-Service aktivieren](cs_loadbalancer.html#config) oder [Private Lastausgleichsfunktion für Anwendungen aktivieren](cs_ingress.html#private_ingress).

<br />


## IP-Adressen und Teilnetze verwalten
{: #manage}

Überprüfen Sie die folgenden Optionen zum Auflisten der verfügbaren öffentlichen IP-Adressen, zum Freigeben von verwendeten IP-Adressen und zur Weiterleitung zwischen mehreren Teilnetzen auf demselben VLAN.
{:shortdesc}

### Verfügbare portierbare öffentliche IP-Adressen anzeigen
{: #review_ip}

Gehen Sie wie folgt vor, um alle verwendeten und verfügbaren IP-Adressen in Ihrem Cluster anzuzeigen, die Sie ausführen können,

  ```
  kubectl get cm ibm-cloud-provider-vlan-ip-config -n kube-system -o yaml
  ```
  {: pre}

Führen Sie die folgenden Schritte aus, um nur die verfügbare, öffentlichen IP-Adressen für Ihren Cluster aufzulisten:

Bevor Sie beginnen, müssen Sie den [Kontext für den zu verwendenden Cluster festlegen](cs_cli_install.html#cs_cli_configure).

1.  Erstellen Sie eine Kubernetes-Servicekonfigurationsdatei namens `myservice.yaml` und definieren Sie einen Service vom Typ `LoadBalancer` mit einer Dummy-IP-Adresse für die Lastausgleichsfunktion. Im folgenden Beispiel wird die IP-Adresse '1.1.1.1' als IP-Adresse der Lastausgleichsfunktion verwendet.

    ```
    apiVersion: v1
    kind: Service
    metadata:
      labels:
        run: myservice
      name: myservice
      namespace: default
    spec:
      ports:
      - port: 80
        protocol: TCP
        targetPort: 80
      selector:
        run: myservice
      sessionAffinity: None
      type: LoadBalancer
      loadBalancerIP: 1.1.1.1
    ```
    {: codeblock}

2.  Erstellen Sie den Service in Ihrem Cluster.

    ```
    kubectl apply -f myservice.yaml
    ```
    {: pre}

3.  Überprüfen Sie den Service.

    ```
    kubectl describe service myservice
    ```
    {: pre}

    **Hinweis:** Die Erstellung dieses Service schlägt fehl, weil der Kubernetes-Master die angegebene IP-Adresse der Lastausgleichsfunktion in der Kubernetes-Konfigurationszuordnung nicht finden kann. Wenn Sie diesen Befehl ausführen, können Sie die Fehlernachricht und die Liste von verfügbaren öffentlichen IP-Adressen für den Cluster anzeigen.

    ```
    Error on cloud load balancer a8bfa26552e8511e7bee4324285f6a4a for service default/myservice with UID 8bfa2655-2e85-11e7-bee4-324285f6a4af: Requested cloud provider IP 1.1.1.1 is not available. The following cloud provider IP addresses are available: <liste_von_IP-adressen>
    ```
    {: screen}

### Verwendete IP-Adressen freigeben
{: #free}

Sie können eine bereits verwendete portierbare IP-Adresse freigeben, indem Sie den Lastausgleichsservice löschen, der die portierbare IP-Adresse belegt.
{:shortdesc}

Bevor Sie beginnen, müssen Sie den [Kontext für den zu verwendenden Cluster festlegen](cs_cli_install.html#cs_cli_configure).

1.  Listen Sie verfügbare Services in Ihrem Cluster auf.

    ```
    kubectl get services
    ```
    {: pre}

2.  Entfernen Sie den Lastausgleichsservice, der eine öffentliche oder private IP-Adresse belegt.

    ```
    kubectl delete service <servicename>
    ```
    {: pre}

### Weiterleitung zwischen Teilnetzen auf demselben VLAN aktivieren
{: #vlan-spanning}

Wenn Sie einen Cluster erstellen, wird ein Teilnetz, das auf `/26` endet, in demselben VLAN, in dem sich auch der Cluster befindet, bereitgestellt. Dieses primäre Teilnetz kann bis zu 62 Workerknoten aufnehmen.
{:shortdesc}

Dieser Grenzwert von 62 Workerknoten kann von einem großen Cluster oder von mehreren kleineren Clustern in einer einzigen Region, die sich in demselben VLAN befinden, überschritten werden. Wenn der Grenzwert für den 62-Worker-Knoten erreicht ist, wird ein zweites primäres Teilnetz in demselben VLAN bestellt.

Für die Weiterleitung zwischen Teilnetzen auf demselben VLAN, müssen Sie VLAN-Spanning aktivieren. Entsprechende Anweisungen finden Sie in [VLAN-Spanning aktivieren oder inaktivieren](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning).
