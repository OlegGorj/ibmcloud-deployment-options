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



# Apps mit LoadBalancers zugänglich machen
{: #loadbalancer}

Machen Sie einen Port zugänglich und verwenden Sie eine portierbare IP-Adresse für die Lastausgleichsfunktion (Load Balancer), um auf eine containerisierte App zuzugreifen.
{:shortdesc}

## Netzverkehr mithilfe von LoadBalancers verwalten
{: #planning}

Wenn Sie einen Standardcluster erstellen, stellt {{site.data.keyword.containershort_notm}} automatisch die folgenden Teilnetze bereit:
* Ein primäres öffentliches Teilnetz, das während der Clustererstellung öffentliche IP-Adressen für Workerknoten festlegt.
* Ein primäres privates Teilnetz, das während der Clustererstellung private IP-Adressen für Workerknoten festlegt.
* Ein portierbares öffentliches Teilnetz, das fünf öffentliche IP-Adressen für Ingress-Netzservices und Netzservices für die Lastausgleichsfunktion bereitstellt.
* Ein portierbares privates Teilnetz, das fünf private IP-Adressen für Ingress-Netzservices und Netzservices für die Lastausgleichsfunktion bereitstellt.

Portierbare öffentliche und private IP-Adressen sind statisch und ändern sich nicht, wenn ein Workerknoten entfernt wird. Für jedes Teilnetz wird eine portierbare öffentliche und eine portierbare private IP-Adresse für die standardmäßig verwendeten [Ingress-Lastausgleichsfunktionen für Anwendungen](cs_ingress.html) verwendet. Die verbleibenden vier portierbaren öffentlichen und vier portierbaren privaten IP-Adressen können verwendet werden, um einzelne Apps für das öffentliche oder private Netz verfügbar zu machen, indem Sie einen Lastausgleichsservice erstellen.

Wenn Sie einen Kubernetes-LoadBalancer-Service in einem Cluster in einem öffentlichen VLAN erstellen, wird eine externe Lastausgleichsfunktion erstellt. Ihre Optionen für IP-Adressen bei Erstellung eines LoadBalancer-Service lauten wie folgt:

- Wenn sich Ihr Cluster in einem öffentlichen VLAN befindet, dann wird eine der vier verfügbaren, portierbaren, öffentlichen IP-Adressen verwendet.
- Wenn Ihr Cluster ausschließlich in einem privaten VLAN verfügbar ist, wird eine der vier verfügbaren, portierbaren, privaten IP-Adressen verwendet.
- Sie können eine portierbare öffentliche oder private IP-Adresse für einen LoadBalancer-Service anfordern, indem Sie eine Annotation zur Konfigurationsdatei hinzufügen: `service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type: <public_or_private>`.

Die dem LoadBalancer-Service zugewiesene portierbare öffentliche IP-Adresse ist dauerhaft und ändert sich nicht, wenn ein Workerknoten entfernt oder neu erstellt wird. Dadurch bietet der LoadBalancer-Service eine höhere Verfügbarkeit als der NodePort-Service. Anders als bei NodePort-Services können Sie Ihrer Lastausgleichsfunktion jeden beliebigen Port zuweisen und sind dabei nicht an einen bestimmten Portnummernbereich gebunden. Wenn Sie einen LoadBalancer-Service verwenden, steht für jede IP-Adresse aller Workerknoten auch ein Knotenport zur Verfügung. Informationen zum Blockieren des Zugriffs auf den Knotenport während der Nutzung eines LoadBalancer-Service finden Sie in [Eingehenden Datenverkehr blockieren](cs_network_policy.html#block_ingress).

Der LoadBalancer-Service fungiert als externer Einstiegspunkt für eingehende Anforderungen an die App. Um vom Internet aus auf den LoadBalancer-Service zuzugreifen, können Sie die öffentliche IP-Adresse Ihrer Lastausgleichsfunktion in Verbindung mit der zugewiesenen Portnummer im Format `<IP_address>:<port>`. Das folgende Diagramm veranschaulicht, wie eine Lastausgleichsfunktion die Kommunikation vom Internet an eine App leitet.

<img src="images/cs_loadbalancer_planning.png" width="550" alt="Stellen Sie ein App in {{site.data.keyword.containershort_notm}} bereit, indem Sie eine Lastausgleichsfunktion verwenden" style="width:550px; border-style: none"/>

1. Eine Anforderung wird an Ihre App gesendet, indem die öffentliche IP-Adresse Ihrer Lastausgleichsfunktion und der zugewiesene Port auf dem Workerknoten verwendet wird.

2. Die Anforderung wird automatisch an die interne Cluster-IP-Adresse und den internen Port des Service für die Lastausgleichsfunktion weitergeleitet. Auf die interne Cluster-IP-Adresse kann nur aus dem Cluster selbst heraus zugegriffen werden.

3. `kube-proxy` leitet die Anforderung an den Kubernetes-Service für die Lastausgleichsfunktion für die App weiter.

4. Die Anforderung wird an die private IP-Adresse des Pods weitergeleitet, auf dem die App bereitgestellt wird. Wenn mehrere App-Instanzen im Cluster bereitgestellt werden, leitet die Lastausgleichsfunktion die Anforderungen zwischen den App-Pods weiter.




<br />


s`.</td>
        </tr>
        <tr>
          <td><code>selector</code></td>
          <td>Geben Sie das Paar aus Kennzeichnungsschlüssel (<em>&lt;selektorschlüssel&gt;</em>) und Wert (<em>&lt;selektorwert&gt;</em>) ein, das Sie für die Ausrichtung auf die Pods, in denen Ihre App ausgeführt wird, verwenden möchten. Um die Pods als Ziel auszuwählen und in den Servicelastausgleich einzubeziehen, überprüfen Sie die Werte für <em>&lt;selektorschlüssel&gt;</em> und <em>&lt;selektorwert&gt;</em>. Stellen Sie sicher, dass es sich hierbei um dasselbe <em>Schlüssel/Wert</em>-Paar handelt, das Sie im Abschnitt <code>spec.template.metadata.labels</code> der YAML-Bereitstellungsdatei verwendet haben.</td>
        </tr>
        <tr>
          <td><code>port</code></td>
          <td>Der Port, den der Service überwacht.</td>
        </tr>
        <tr>
          <td><code>loadBalancerIP</code></td>
          <td>Um einen privaten LoadBalancer zu erstellen oder um eine bestimmte portierbare IP-Adresse für einen öffentlichen LoadBalancer zu verwenden, ersetzen Sie <em>&lt;ip-adresse&gt;</em> durch die IP-Adresse, die Sie verwenden wollen. Weitere Informationen enthält die [Kubernetes-Dokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/services-networking/service/#type-loadbalancer).</td>
        </tr>
        </tbody></table>

      3. Optional: Konfigurieren Sie eine Firewall, indem Sie `loadBalancerSourceRanges` im Abschnitt **spec** angeben. Weitere Informationen enthält die [Kubernetes-Dokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/tasks/access-application-cluster/configure-cloud-provider-firewall/).

      4. Erstellen Sie den Service in Ihrem Cluster.

          ```
          kubectl apply -f myloadbalancer.yaml
          ```
          {: pre}

          Wenn der Lastenausgleichsservice erstellt wird, wird der Lastenausgleichsfunktion automatisch eine portierbare IP-Adresse zugewiesen. Ist keine portierbare IP-Adresse verfügbar, schlägt die Erstellung Ihres Lastenausgleichsservice fehl.

3.  Stellen Sie sicher, dass der Lastenausgleichsservice erstellt wurde. Ersetzen Sie _&lt;mein_service&gt;_ durch den Namen des Lastausgleichsservice, den Sie im vorherigen Schritt erstellt haben.

    ```
    kubectl describe service myloadbalancer
    ```
    {: pre}

    **Hinweis:** Es kann ein paar Minuten dauern, bis der Lastenausgleichsservice ordnungsgemäß erstellt und die App verfügbar ist.

    CLI-Beispielausgabe:

    ```
    Name:                   myloadbalancer
    Namespace:              default
    Labels:                 <none>
    Selector:               app=liberty
    Type:                   LoadBalancer
    Location:               dal10
    IP:                     172.21.xxx.xxx
    LoadBalancer Ingress:   169.xx.xxx.xxx
    Port:                   <unset> 8080/TCP
    NodePort:               <unset> 32040/TCP
    Endpoints:              172.30.xxx.xxx:8080
    Session Affinity:       None
    Events:
      FirstSeen	LastSeen	Count	From			SubObjectPath	Type	 Reason			          Message
      ---------	--------	-----	----			-------------	----	 ------			          -------
      10s		    10s		    1	    {service-controller }	  Normal CreatingLoadBalancer	Creating load balancer
      10s		    10s		    1	    {service-controller }		Normal CreatedLoadBalancer	Created load balancer
    ```
    {: screen}

    Die IP-Adresse für **LoadBalancer Ingress** ist die portierbare IP-Adresse, die dem Lastenausgleichsservice zugewiesen wurde.

4.  Wenn Sie eine öffentliche Lastausgleichsfunktion erstellt haben, dann greifen Sie über das Internet auf Ihre App zu.
    1.  Öffnen Sie Ihren bevorzugten Web-Browser.
    2.  Geben Sie die portierbare öffentliche IP-Adresse der Lastausgleichsfunktion und des Ports ein.

        ```
        http://169.xx.xxx.xxx:8080
        ```
        {: codeblock}        

5. Wenn Sie die [Beibehaltung der Quellen-IP für einen Lastausgleichsservice ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") aktivieren](https://kubernetes.io/docs/tutorials/services/source-ip/#source-ip-for-services-with-typeloadbalancer), stellen Sie sicher, dass App-Pods auf den Edge-Workerknoten geplant sind, indem Sie [Edge-Knoten-Affinität zu App-Pods hinzufügen](cs_loadbalancer.html#edge_nodes). App-Pods müssen auf Edge-Knoten geplant werden, um eingehende Anforderungen empfangen zu können.

6. Optional: Um eingehende Anforderungen von anderen Zonen zu verarbeiten, wiederholen Sie diese Schritte, um in jeder Zone eine Lastausgleichsfunktion hinzuzufügen.

</staging>

## Öffentlichen oder privaten Zugriff auf eine App mithilfe eines LoadBalancer-Service aktivieren
{: #config}

Vorbemerkungen:

-   Dieses Feature ist nur für Standardcluster verfügbar.
-   Es muss eine portierbare öffentliche oder private IP-Adresse verfügbar sein, die Sie dem Service für die Lastausgleichsfunktion (Load Balancer) zuweisen können.
-   Ein Service für die Lastausgleichsfunktion mit einer portierbaren privaten IP-Adresse verfügt weiterhin auf jedem Workerknoten über einen offenen öffentlichen Knotenport. Informationen zum Hinzufügen einer Netzrichtlinie zur Verhinderung von öffentlichem Datenverkehr finden Sie unter dem Thema [Eingehenden Datenverkehr blockieren](cs_network_policy.html#block_ingress).

Gehen Sie wie folgt vor, um einen Service für die Lastausgleichsfunktion zu erstellen:

1.  [Stellen Sie die App für den Cluster bereit](cs_app.html#app_cli). Wenn Sie dem Cluster die App bereitstellen, wird mindestens ein Pod für Sie erstellt, von dem die App im Container ausgeführt wird. Stellen Sie sicher, dass Sie zur Bereitstellung im Metadatenabschnitt der Konfigurationsdatei eine Bezeichnung hinzufügen. Diese Bezeichnung ist zur Identifizierung aller Pods erforderlich, in denen Ihre App ausgeführt wird, damit sie in den Lastenausgleich aufgenommen werden können.
2.  Erstellen Sie einen Service für die App, den Sie öffentlich zugänglich machen möchten. Damit Ihre App im öffentlichen Internet oder in einem privaten Netz verfügbar wird, müssen Sie einen Kubernetes-Service für die App erstellen. Sie müssen den Service so konfigurieren, dass alle Pods, aus denen die App besteht, in den Lastenausgleich eingeschlossen sind.
    1.  Erstellen Sie eine Servicekonfigurationsdatei namens `myloadbalancer.yaml` (Beispiel).

    2.  Definieren Sie einen Lastenausgleichsservice für die App, die Sie zugänglich machen möchten.
        - Wenn sich Ihr Cluster in einem öffentlichen VLAN befindet, dann wird eine portierbare öffentliche IP-Adresse verwendet. Die meisten Cluster befinden sich in einem öffentlichen VLAN.
        - Wenn Ihr Cluster ausschließlich in einem privaten VLAN verfügbar ist, wird eine portierbare private IP-Adresse verwendet.
        - Sie können eine portierbare öffentliche oder private IP-Adresse für einen LoadBalancer-Service anfordern, indem Sie eine Annotation zur Konfigurationsdatei hinzufügen.

        LoadBalancer-Service, der eine IP-Standardadresse verwendet:

        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: myloadbalancer
        spec:
          type: LoadBalancer
          selector:
            <selektorschlüssel>:<selektorwert>
          ports:
           - protocol: TCP
             port: 8080
        ```
        {: codeblock}

        LoadBalancer-Service, der eine Annotation zur Angabe einer privaten oder öffentlichen IP-Adresse verwendet:

        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: myloadbalancer
          annotations:
            service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type: <öffentlich_oder_privat>
        spec:
          type: LoadBalancer
          selector:
            <selektorschlüssel>: <selektorwert>
          ports:
           - protocol: TCP
             port: 8080
          loadBalancerIP: <ip-adresse>
        ```
        {: codeblock}

        <table>
        <caption>Erklärung der Komponenten der YAML-Datei</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Ideensymbol"/> Erklärung der YAML-Dateikomponenten</th>
        </thead>
        <tbody>
        <tr>
          <td><code>selector</code></td>
          <td>Geben Sie das Paar aus Kennzeichnungsschlüssel (<em>&lt;selektorschlüssel&gt;</em>) und Wert (<em>&lt;selektorwert&gt;</em>) ein, das Sie für die Ausrichtung auf die Pods, in denen Ihre App ausgeführt wird, verwenden möchten. Um die Pods als Ziel auszuwählen und in den Servicelastausgleich einzubeziehen, überprüfen Sie die Werte für <em>&lt;selektorschlüssel&gt;</em> und <em>&lt;selektorwert&gt;</em>. Stellen Sie sicher, dass es sich hierbei um dasselbe <em>Schlüssel/Wert</em>-Paar handelt, das Sie im Abschnitt <code>spec.template.metadata.labels</code> der YAML-Bereitstellungsdatei verwendet haben.</td>
        </tr>
        <tr>
          <td><code>port</code></td>
          <td>Der Port, den der Service überwacht.</td>
        </tr>
        <tr>
          <td>`service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type:`
          <td>Annotation zur Angabe des LoadBalancer-Typs. Zulässige Werte sind `private` und `public`. Bei der Erstellung eines öffentlichen LoadBalancers in Clustern in öffentlichen VLANs ist diese Annotation nicht erforderlich.</td>
        </tr>
        <tr>
          <td><code>loadBalancerIP</code></td>
          <td>Um einen privaten LoadBalancer zu erstellen oder um eine bestimmte portierbare IP-Adresse für einen öffentlichen LoadBalancer zu verwenden, ersetzen Sie <em>&lt;ip-adresse&gt;</em> durch die IP-Adresse, die Sie verwenden wollen. Weitere Informationen enthält die [Kubernetes-Dokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/services-networking/service/#type-loadbalancer).</td>
        </tr>
        </tbody></table>

    3.  Optional: Konfigurieren Sie eine Firewall, indem Sie `loadBalancerSourceRanges` im Abschnitt **spec** angeben. Weitere Informationen enthält die [Kubernetes-Dokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/tasks/access-application-cluster/configure-cloud-provider-firewall/).

    4.  Erstellen Sie den Service in Ihrem Cluster.

        ```
        kubectl apply -f myloadbalancer.yaml
        ```
        {: pre}

        Wenn der Lastenausgleichsservice erstellt wird, wird der Lastenausgleichsfunktion automatisch eine portierbare IP-Adresse zugewiesen. Ist keine portierbare IP-Adresse verfügbar, schlägt die Erstellung Ihres Lastenausgleichsservice fehl.

3.  Stellen Sie sicher, dass der Lastenausgleichsservice erstellt wurde.

    ```
    kubectl describe service myloadbalancer
    ```
    {: pre}

    **Hinweis:** Es kann ein paar Minuten dauern, bis der Lastenausgleichsservice ordnungsgemäß erstellt und die App verfügbar ist.

    CLI-Beispielausgabe:

    ```
    Name:                   myloadbalancer
    Namespace:              default
    Labels:                 <none>
    Selector:               app=liberty
    Type:                   LoadBalancer
    Location:               dal10
    IP:                     172.21.xxx.xxx
    LoadBalancer Ingress:   169.xx.xxx.xxx
    Port:                   <unset> 8080/TCP
    NodePort:               <unset> 32040/TCP
    Endpoints:              172.30.xxx.xxx:8080
    Session Affinity:       None
    Events:
      FirstSeen	LastSeen	Count	From			SubObjectPath	Type	 Reason			          Message
      ---------	--------	-----	----			-------------	----	 ------			          -------
      10s		    10s		    1	    {service-controller }	  Normal CreatingLoadBalancer	Creating load balancer
      10s		    10s		    1	    {service-controller }		Normal CreatedLoadBalancer	Created load balancer
    ```
    {: screen}

    Die IP-Adresse für **LoadBalancer Ingress** ist die portierbare IP-Adresse, die dem Lastenausgleichsservice zugewiesen wurde.

4.  Wenn Sie eine öffentliche Lastausgleichsfunktion erstellt haben, dann greifen Sie über das Internet auf Ihre App zu.
    1.  Öffnen Sie Ihren bevorzugten Web-Browser.
    2.  Geben Sie die portierbare öffentliche IP-Adresse der Lastausgleichsfunktion und des Ports ein.

        ```
        http://169.xx.xxx.xxx:8080
        ```
        {: codeblock}

5. Wenn Sie die [Beibehaltung der Quellen-IP für einen Lastausgleichsservice ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") aktivieren](https://kubernetes.io/docs/tutorials/services/source-ip/#source-ip-for-services-with-typeloadbalancer), stellen Sie sicher, dass App-Pods auf den Edge-Workerknoten geplant sind, indem Sie [Edge-Knoten-Affinität zu App-Pods hinzufügen](cs_loadbalancer.html#edge_nodes). App-Pods müssen auf Edge-Knoten geplant werden, um eingehende Anforderungen empfangen zu können.

<br />


## Knotenaffinität und Tolerierungen zu App-Pods für eine Quellen-IP hinzufügen
{: #node_affinity_tolerations}

Immer wenn Sie App-Pods bereitstellen, werden auch Lastausgleichsfunktions-Pods auf den Workerknoten bereitgestellt, auf denen sich die App-Pods befinden. Es kann jedoch vorkommen, dass die Lastausgleichsfunktions-Pods und die App-Pods nicht auf demselben Workerknoten geplant sind:
{: shortdesc}

* Sie verfügen Edge-Knoten, auf die ein Taint angewendet wurde und auf denen deshalb nur Lastausgleichsservice-Pods bereitgestellt werden können. App-Pods dürfen auf diesen Knoten nicht bereitgestellt werden.
* Ihr Cluster ist mit mehreren öffentlichen oder privaten VLANs verbunden und Ihre App-Pods werden unter Umständen auf Workerknoten bereitgestellt, die nur mit einem VLAN verbunden sind. Lastausgleichsservice-Pods lassen sich möglicherweise nicht auf solchen Workerknoten bereitstellen, weil die IP-Adresse der Lastausgleichsfunktion mit einem anderen VLAN als die Workerknoten verbunden ist.

Wenn eine Clientanforderung für Ihre App an Ihren Cluster gesendet wird, wird die Anforderung an einen Pod für den Kubernetes-Lastausgleichsservice weitergeleitet, der die App zugänglich macht. Wenn ein App-Pod nicht auf demselben Workernoten vorhanden ist wie der Lastausgleichsservice-Pod, leitet die Lastausgleichsfunktion die Anforderung an einen App-Pod auf einem anderen Workerknoten weiter. Die Quellen-IP-Adresse des Pakets wird in die öffentliche IP-Adresse des Workerknotens geändert, auf dem der App-Pod ausgeführt wird.

Um die ursprüngliche Quellen-IP-Adresse der Clientanforderung beizubehalten, können Sie die [Quellen-IP ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/tutorials/services/source-ip/#source-ip-for-services-with-typeloadbalancer) für Lastausgleichsservices aktivieren. Das Beibehalten der IP des Clients ist nützlich, z. B. wenn App-Server Sicherheits- und Zugriffssteuerungsrichtlinien genügen müssen. Nachdem Sie die Quellen-IP aktiviert haben, müssen Lastausgleichsservice-Pods Anforderungen an App-Pods weiterleiten, die auf demselben Workerknoten bereitgestellt sind. Um zu erzwingen, dass Ihre App auf bestimmten Workerknoten bereitgestellt wird, auf denen auch Lastausgleichsservice-Pods bereitgestellt werden können, müssen Sie Affinitätsregeln und Tolerierungen zu Ihrer App-Bereitstellung hinzufügen.

### Affinitätsregeln und Tolerierungen für Edge-Knoten hinzufügen
{: #edge_nodes}

Wenn Sie [Workerknoten als Edge-Knoten kennzeichnen](cs_edge.html#edge_nodes) und zudem [Taints auf Edge-Knoten anwenden](cs_edge.html#edge_workloads), werden Lastausgleichsservice-Pods nur auf diesen Edge-Knoten bereitgestellt und App-Pods können nicht auf Edge-Knoten implementiert werden. Wenn die Quellen-Ip für den Lastausgleichsservice aktiviert ist, können die Lastausgleichsfunktions-Pods auf den Edge-Knoten eingehende Anforderungen nicht an die App-Pods auf anderen Workerknoten weiterleiten.
{:shortdesc}

Um zu erzwingen, dass Ihre App-Pods auf Edge-Knoten bereitgestellt werden, fügen Sie eine [Affinitätsregel ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#node-affinity-beta-feature) für den Edge-Knoten und eine [Tolerierung ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/#concepts) zur App-Bereitstellung hinzu.

Beispiel für die YAML-Bereitstellungsdatei mit Edge-Knoten-Affinität und -Tolerierung:

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: with-node-affinity
spec:
  template:
    spec:
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: dedicated
                operator: In
                values:
                - edge
      tolerations:
        - key: dedicated
          value: edge
...
```
{: codeblock}

In beiden Abschnitten **affinity** und **tolerations** ist der Wert `dedicated` für `key` und der Wert `edge` für `value` angegeben.

### Affinitätsregeln für mehrere öffentliche oder private VLANs hinzufügen
{: #edge_nodes_multiple_vlans}

Wenn Ihr Cluster mit mehreren öffentlichen oder privaten VLANs verbunden ist, werden Ihre App-Pods unter Umständen auf Workerknoten bereitgestellt, die nur mit einem VLAN verbunden sind. Wenn die IP-Adresse der Lastausgleichsfunktion mit einem anderen VLAN als diese Workerknoten verbunden ist, werden die Lastausgleichsservice-Pods nicht auf diesen Workerknoten bereitgestellt.
{:shortdesc}

Wenn die Quellen-IP aktiviert ist, planen Sie App-Pods auf Workerknoten, die dasselbe VLAN wie die IP-Adresse der Lastausgleichsfunktion aufweisen, indem Sie eine Affinitätsregel zur App-Bereitstellung hinzufügen.

Führen Sie zunächst den folgenden Schritt aus: [Richten Sie Ihre CLI](cs_cli_install.html#cs_cli_configure) auf Ihren Cluster aus.

1. Rufen Sie die IP-Adresse des Lastausgleichsservice ab. Suchen Sie im Feld **LoadBalancer Ingress** nach der IP-Adresse.
    ```
    kubectl describe service <name_des_loadbalancer-service>
    ```
    {: pre}

2. Rufen Sie die VLAN-ID ab, mit der Ihr Lastausgleichsservice verbunden ist.

    1. Listen Sie portierbare öffentliche VLANs für Ihren Cluster auf.
        ```
        bx cs cluster-get <clustername_oder_-id> --showResources
        ```
        {: pre}

        Beispielausgabe:
        ```
        ...

        Subnet VLANs
        VLAN ID   Subnet CIDR       Public   User-managed
        2234947   10.xxx.xx.xxx/29  false    false
        2234945   169.36.5.xxx/29   true     false
        ```
        {: screen}

    2. Suchen Sie in der Ausgabe unter **Subnet VLANs** nach der Teilnetz-CIDR, die mit der IP-Adresse der Lastausgleichsfunktion übereinstimmt, die Sie zuvor abgerufen haben, und notieren Sie sich die VLAN-ID.

        Wenn die IP-Adresse des Lastausgleichsservice beispielsweise `169.36.5.xxx` lautet, ist das passende Teilnetz in der Beispielausgabe aus dem vorherigen Schritt `169.36.5.xxx/29`. Die VLAN-ID, mit der das Teilnetz verbunden ist, lautet `2234945`.

3. [Fügen Sie eine Affinitätsregel ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#node-affinity-beta-feature) zu der App-Bereitstellung für die VLAN-ID hinzu, die Sie sich im vorherigen Schritt notiert haben.

    Wenn Sie beispielsweise über mehrere VLANs verfügen, aber Ihre App-Pods nur auf Workerknoten im öffentlichen VLAN `2234945` bereitstellen möchten.

    ```
    apiVersion: extensions/v1beta1
    kind: Deployment
    metadata:
      name: with-node-affinity
    spec:
      template:
        spec:
          affinity:
            nodeAffinity:
              requiredDuringSchedulingIgnoredDuringExecution:
                nodeSelectorTerms:
                - matchExpressions:
                  - key: publicVLAN
                    operator: In
                    values:
                    - "2234945"
    ...
    ```
    {: codeblock}

    In der YAML-Beispieldatei enthält der Abschnitt **affinity** den Wert `publicVLAN` für `key` und den Wert `"2234945"` für `value`.

4. Wenden Sie die aktualisierte Bereitstellungskonfigurationsdatei an.
    ```
    kubectl apply -f with-node-affinity.yaml
    ```
    {: pre}

5. Überprüfen Sie, dass die App-Pods auf Workerknoten bereitgestellt werden, die mit dem designierten VLAN verbunden sind.

    1. Listen Sie die Pods in Ihrem Cluster auf. Ersetzen Sie `<selector>` durch die Bezeichnung, die Sie für die App verwendet haben.
        ```
        kubectl get pods -o wide app=<selektor>
        ```
        {: pre}

        Beispielausgabe:
        ```
        NAME                   READY     STATUS              RESTARTS   AGE       IP               NODE
        cf-py-d7b7d94db-vp8pq  1/1       Running             0          15d       172.30.xxx.xxx   10.176.48.78
        ```
        {: screen}

    2. Geben Sie in der Ausgabe einen Pod für Ihre App an. Notieren Sie sich die Knoten-ID (**NODE**) des Workerknotens, auf dem sich der Pod befindet.

        In der Beispielausgabe aus dem vorherigen Schritt befindet sich der App-Pod `cf-py-d7b7d94db-vp8pq` auf dem Workerknoten `10.176.48.78`.

    3. Listen Sie die Details für den Workerknoten auf.

        ```
        kubectl describe node <workerknoten-ID>
        ```
        {: pre}

        Beispielausgabe:

        ```
        Name:                   10.xxx.xx.xxx
        Role:
        Labels:                 arch=amd64
                                beta.kubernetes.io/arch=amd64
                                beta.kubernetes.io/os=linux
                                failure-domain.beta.kubernetes.io/region=us-south
                                failure-domain.beta.kubernetes.io/zone=dal10
                                ibm-cloud.kubernetes.io/encrypted-docker-data=true
                                kubernetes.io/hostname=10.xxx.xx.xxx
                                privateVLAN=2234945
                                publicVLAN=2234967
        ...
        ```
        {: screen}

    4. Überprüfen Sie im Abschnitt **Labels** der Ausgabe, dass es sich bei dem öffentlichen oder privaten VLAN um das VLAN handelt, das Sie in den vorherigen Schritten angegeben haben.
