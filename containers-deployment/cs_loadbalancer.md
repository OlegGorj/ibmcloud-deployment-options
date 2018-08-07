---

copyright:
  years: 2014, 2018
lastupdated: "2018-08-06"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}



# Exposing apps with LoadBalancers
{: #loadbalancer}

Expose a port and use a portable IP address for a Layer 4 load balancer to access a containerized app.
{:shortdesc}

## Load balancer components and architecture
{: #planning}

When you create a standard cluster, {{site.data.keyword.containershort_notm}} automatically provisions a portable public subnet and a portable private subnet.

* The portable public subnet provides 1 portable public IP address that is used by the default [public Ingress ALB](cs_ingress.html). The remaining 4 portable public IP addresses can be used to expose single apps to the internet by creating a public load balancer service.
* The portable private subnet provides 1 portable private IP address that is used by the default [private Ingress ALB](cs_ingress.html#private_ingress). The remaining 4 portable private IP addresses can be used to expose single apps to a private network by creating a private load balancer service.

Portable public and private IP addresses are static and do not change when a worker node is removed. If the worker node that the load balancer IP address is on is removed, a keepalived daemon that constantly monitors the IP automatically moves the IP to another worker node. You can assign any port to your load balancer and are not bound to a certain port range.

A load balancer service also makes your app available over the service's NodePorts. [NodePorts](cs_nodeport.html) are accessible on every public and private IP address for every node within the cluster. To block traffic to NodePorts while you are using a load balancer service, see [Controlling inbound traffic to LoadBalancer or NodePort services](cs_network_policy.html#block_ingress).

The LoadBalancer service serves as the external entry point for incoming requests for the app. To access the LoadBalancer service from the internet, use the public IP address of your load balancer and the assigned port in the format `<IP_address>:<port>`. The following diagram shows how a load balancer directs communication from the internet to an app.

<img src="images/cs_loadbalancer_planning.png" width="550" alt="Expose an app in {{site.data.keyword.containershort_notm}} by using a load balancer" style="width:550px; border-style: none"/>

1. A request to your app uses the public IP address of your load balancer and the assigned port on the worker node.

2. The request is automatically forwarded to the load balancer service's internal cluster IP address and port. The internal cluster IP address is accessible inside the cluster only.

3. `kube-proxy` routes the request to the Kubernetes load balancer service for the app.

4. The request is forwarded to the private IP address of the app pod. The source IP address of the request package is changed to the public IP address of the worker node where the app pod is running. If multiple app instances are deployed in the cluster, the load balancer routes the requests between the app pods.

**Multizone clusters**:

If you have a multizone cluster, app instances are deployed in pods on workers across the different zones. Review these LoadBalancer setups for load balancing requests to your app instances in multiple zones.

<img src="images/cs_loadbalancer_planning_multizone.png" width="800" alt="Use a LoadBalancer service to load balance apps in multizone clusters" style="width:700px; border-style: none"/>

1. **Lower availability: load balancer that is deployed in one zone.** By default, each load balancer is set up in one zone only. When only one load balancer is deployed, the load balancer must route requests to the app instances in its own zone and to app instances in other zones.

2. **Higher availability: load balancers that are deployed in each zone.** You can achieve higher availability when you deploy a load balancer in every zone where you have app instances. Requests are handled by the load balancers in various zones in a round-robin cycle. Additionally, each load balancer routes requests to the app instances in its own zone and to app instances in other zones.


<br />



## Enabling public or private access to an app in a multizone cluster
{: #multi_zone_config}

Note:
  * This feature is available for standard clusters only.
  * LoadBalancer services do not support TLS termination. If your app requires TLS termination, you can expose your app by using [Ingress](cs_ingress.html), or configure your app to manage the TLS termination.

Before you begin:
  * A load balancer service with a portable private IP address still has a public NodePort open on every worker node. To add a network policy to prevent public traffic, see [Blocking incoming traffic](cs_network_policy.html#block_ingress).
  * In each zone, at least one public VLAN must have portable subnets available for Ingress and LoadBalancer services. To add private Ingress and LoadBalancer services, you must specify at least one private VLAN with available portable subnets. To add subnets, see [Configuring subnets for clusters](cs_subnets.html).
  * If you restrict network traffic to edge worker nodes, ensure that at least 2 [edge worker nodes](cs_edge.html#edge) are enabled in each zone. If edge worker nodes are enabled in some zones but not in others, load balancers will not deploy uniformly. Load balancers will be deployed onto edge nodes in some zones but on regular worker nodes in other zones.
  * To enable communication on the private network between workers that are in different zones, you must enable [VLAN spanning](/docs/infrastructure/vlans/vlan-spanning.html#vlan-spanning).


To set up a LoadBalancer service in a multizone cluster:
1.  [Deploy your app to the cluster](cs_app.html#app_cli). When you deploy your app to the cluster, one or more pods are created for you that run your app in a container. Ensure that you add a label to your deployment in the metadata section of your configuration file. This label is needed to identify all of the pods where your app is running so that they can be included in the load balancing.

2.  Create a load balancer service for the app that you want to expose. To make your app available on the public internet or on a private network, create a Kubernetes service for your app. Configure your service to include all the pods that make up your app into the load balancing.
  1. Create a service configuration file that is named, for example, `myloadbalancer.yaml`.
  2. Define a load balancer service for the app that you want to expose. You can specify an IP address from your private or public portable subnet, and a zone.
      - To choose both a zone and an IP address, use the `ibm-load-balancer-cloud-provider-zone` annotation to specify the zone and the `loadBalancerIP` field to specify a public or private IP address that is located in that zone.
      - To choose an IP address only, use the `loadBalancerIP` field to specify a public or private IP address. The load balancer is created in the zone where the IP address's VLAN is located.
      - To choose a zone only, use the `ibm-load-balancer-cloud-provider-zone` annotation to specify the zone. A portable IP address from the specified zone is used.
      - If you do not specify an IP address or zone, and your cluster is on a public VLAN, a portable public IP address is used. Most clusters are on a public VLAN. If your cluster is available on a private VLAN only, then a portable private IP address is used. The load balancer is created in the zone where the VLAN is located.

      LoadBalancer service that uses annotations to specify a private or public load balancer and a zone, and the `loadBalancerIP` section to specify an IP address:

      ```
      apiVersion: v1
      kind: Service
      metadata:
        name: myloadbalancer
        annotations:
          service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type: <public_or_private>
          service.kubernetes.io/ibm-load-balancer-cloud-provider-zone: "<zone>"
      spec:
        type: LoadBalancer
        selector:
          <selector_key>: <selector_value>
        ports:
         - protocol: TCP
           port: 8080
        loadBalancerIP: <IP_address>
      ```
      {: codeblock}

      <table>
      <caption>Understanding the YAML file components</caption>
      <thead>
      <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
      </thead>
      <tbody>
      <tr>
        <td><code>service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type:</code>
        <td>Annotation to specify the type of LoadBalancer. Accepted values are <code>private</code> and <code>public</code>. If you are creating a public LoadBalancer in clusters on public VLANs, this annotation is not required.</td>
      </tr>
      <tr>
        <td><code>service.kubernetes.io/ibm-load-balancer-cloud-provider-zone:</code>
        <td>Annotation to specify the zone. To see zones, run <code>ibmcloud ks zones</code>.</td>
      </tr>
      <tr>
        <td><code>selector</code></td>
        <td>Enter the label key (<em>&lt;selector_key&gt;</em>) and value (<em>&lt;selector_value&gt;</em>) pair to use to target the pods where your app runs. To target your pods and include them in the service load balancing, check the <em>&lt;selectorkey&gt;</em> and <em>&lt;selectorvalue&gt;</em> values. Make sure that they are the same as the <em>key/value</em> pair that you used in the <code>spec.template.metadata.labels</code> section of your deployment yaml.</td>
      </tr>
      <tr>
        <td><code>port</code></td>
        <td>The port that the service listens on.</td>
      </tr>
      <tr>
        <td><code>loadBalancerIP</code></td>
        <td>To create a private LoadBalancer or to use a specific portable IP address for a public LoadBalancer, replace <em>&lt;IP_address&gt;</em> with the IP address that you want to use. For more information, see the [Kubernetes documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/services-networking/service/#type-loadbalancer).</td>
      </tr>
      </tbody></table>

  3. Optional: Configure a firewall by specifying the `loadBalancerSourceRanges` in the **spec** section. For more information, see the [Kubernetes documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/access-application-cluster/configure-cloud-provider-firewall/).

  4. Create the service in your cluster.

      ```
      kubectl apply -f myloadbalancer.yaml
      ```
      {: pre}

3. Verify that the load balancer service was created successfully. Replace _&lt;myservice&gt;_ with the name of the load balancer service that you created in the previous step.

    ```
    kubectl describe service myloadbalancer
    ```
    {: pre}

    **Note:** It might take a few minutes for the load balancer service to be created properly and for the app to be available.

    Example CLI output:

    ```
    Name:                   myloadbalancer
    Namespace:              default
    Labels:                 <none>
    Selector:               app=liberty
    Type:                   LoadBalancer
    Zone:                   dal10
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

    The **LoadBalancer Ingress** IP address is the portable IP address that was assigned to your load balancer service.

4.  If you created a public load balancer, access your app from the internet.
    1.  Open your preferred web browser.
    2.  Enter the portable public IP address of the load balancer and port.

        ```
        http://169.xx.xxx.xxx:8080
        ```
        {: codeblock}        

5. If you choose to [enable source IP preservation for a load balancer service ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tutorials/services/source-ip/#source-ip-for-services-with-typeloadbalancer), ensure that app pods are scheduled onto the edge worker nodes by [adding edge node affinity to app pods](cs_loadbalancer.html#edge_nodes). App pods must be scheduled onto edge nodes to receive incoming requests.

6. To handle incoming requests to your app from other zones, repeat the above steps to add a load balancer in each zone.

7. Optional: A load balancer service also makes your app available over the service's NodePorts. [NodePorts](cs_nodeport.html) are accessible on every public and private IP address for every node within the cluster. To block traffic to NodePorts while you are using a load balancer service, see [Controlling inbound traffic to LoadBalancer or NodePort services](cs_network_policy.html#block_ingress).

## Enabling public or private access to an app in a single-zone cluster
{: #config}

Before you begin:

-   This feature is available for standard clusters only.
-   You must have a portable public or private IP address available to assign to the load balancer service.
-   A load balancer service with a portable private IP address still has a public NodePort open on every worker node. To add a network policy to prevent public traffic, see [Blocking incoming traffic](cs_network_policy.html#block_ingress).

To create a load balancer service:

1.  [Deploy your app to the cluster](cs_app.html#app_cli). When you deploy your app to the cluster, one or more pods are created for you that run your app in a container. Ensure that you add a label to your deployment in the metadata section of your configuration file. This label is needed to identify all pods where your app is running so that they can be included in the load balancing.
2.  Create a load balancer service for the app that you want to expose. To make your app available on the public internet or on a private network, create a Kubernetes service for your app. Configure your service to include all the pods that make up your app into the load balancing.
    1.  Create a service configuration file that is named, for example, `myloadbalancer.yaml`.

    2.  Define a load balancer service for the app that you want to expose.
        - If your cluster is on a public VLAN, a portable public IP address is used. Most clusters are on a public VLAN.
        - If your cluster is available on a private VLAN only, then a portable private IP address is used.
        - You can request a portable public or private IP address for a LoadBalancer service by adding an annotation to the configuration file.

        LoadBalancer service that uses a default IP address:

        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: myloadbalancer
        spec:
          type: LoadBalancer
          selector:
            <selector_key>:<selector_value>
          ports:
           - protocol: TCP
             port: 8080
        ```
        {: codeblock}

        LoadBalancer service that uses an annotation to specify a private or public IP address:

        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: myloadbalancer
          annotations:
            service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type: <public_or_private>
        spec:
          type: LoadBalancer
          selector:
            <selector_key>: <selector_value>
          ports:
           - protocol: TCP
             port: 8080
          loadBalancerIP: <IP_address>
        ```
        {: codeblock}

        <table>
        <caption>Understanding the YAML file components</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
        </thead>
        <tbody>
        <tr>
          <td><code>selector</code></td>
          <td>Enter the label key (<em>&lt;selector_key&gt;</em>) and value (<em>&lt;selector_value&gt;</em>) pair to use to target the pods where your app runs. To target your pods and include them in the service load balancing, check the <em>&lt;selector_key&gt;</em> and <em>&lt;selector_value&gt;</em> values. Make sure that they are the same as the <em>key/value</em> pair that you used in the <code>spec.template.metadata.labels</code> section of your deployment yaml.</td>
        </tr>
        <tr>
          <td><code>port</code></td>
          <td>The port that the service listens on.</td>
        </tr>
        <tr>
          <td>`service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type:`
          <td>Annotation to specify the type of LoadBalancer. Accepted values are `private` and `public`. If you are creating a public LoadBalancer in clusters on public VLANs, this annotation is not required.</td>
        </tr>
        <tr>
          <td><code>loadBalancerIP</code></td>
          <td>To create a private LoadBalancer or to use a specific portable IP address for a public LoadBalancer, replace <em>&lt;IP_address&gt;</em> with the IP address that you want to use. For more information, see the [Kubernetes documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/services-networking/service/#type-loadbalancer).</td>
        </tr>
        </tbody></table>

    3.  Optional: Configure a firewall by specifying the `loadBalancerSourceRanges` in the **spec** section. For more information, see the [Kubernetes documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/access-application-cluster/configure-cloud-provider-firewall/).

    4.  Create the service in your cluster.

        ```
        kubectl apply -f myloadbalancer.yaml
        ```
        {: pre}

        When your load balancer service is created, a portable IP address is automatically assigned to the load balancer. If no portable IP address is available, the load balancer service cannot be created.

3.  Verify that the load balancer service was created successfully.

    ```
    kubectl describe service myloadbalancer
    ```
    {: pre}

    **Note:** It might take a few minutes for the load balancer service to be created properly and for the app to be available.

    Example CLI output:

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

    The **LoadBalancer Ingress** IP address is the portable IP address that was assigned to your load balancer service.

4.  If you created a public load balancer, access your app from the internet.
    1.  Open your preferred web browser.
    2.  Enter the portable public IP address of the load balancer and port.

        ```
        http://169.xx.xxx.xxx:8080
        ```
        {: codeblock}

5. If you choose to [enable source IP preservation for a load balancer service ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tutorials/services/source-ip/#source-ip-for-services-with-typeloadbalancer), ensure that app pods are scheduled onto the edge worker nodes by [adding edge node affinity to app pods](cs_loadbalancer.html#edge_nodes). App pods must be scheduled onto edge nodes to receive incoming requests.

6. Optional: A load balancer service also makes your app available over the service's NodePorts. [NodePorts](cs_nodeport.html) are accessible on every public and private IP address for every node within the cluster. To block traffic to NodePorts while you are using a load balancer service, see [Controlling inbound traffic to LoadBalancer or NodePort services](cs_network_policy.html#block_ingress).

<br />


## Adding node affinity and tolerations to app pods for source IP
{: #node_affinity_tolerations}

When a client request to your app is sent to your cluster, the request is routed to the load balancer service pod that exposes your app. If no app pod exists on the same worker node as the load balancer service pod, the load balancer forwards the request to an app pod on a different worker node. The source IP address of the package is changed to the public IP address of the worker node where the app pod is running.

To preserve the original source IP address of the client request, you can [enable source IP ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/access-application-cluster/create-external-load-balancer/#preserving-the-client-source-ip) for load balancer services. The TCP connection continues all the way to the app pods so that the app can see the actual source IP address of the initiator. Preserving the client’s IP is useful, for example, when app servers have to apply security and access-control policies.

After you enable the source IP, load balancer service pods must forward requests to app pods that are deployed to the same worker node only. Typically, load balancer service pods are also deployed to the worker nodes that the app pods are deployed to. However, some situations exist where the load balancer pods and app pods might not be scheduled onto the same worker node:

* You have edge nodes that are tainted so that only load balancer service pods can deploy to them. App pods are not permitted to deploy to those nodes.
* Your cluster is connected to multiple public or private VLANs, and your app pods might deploy to worker nodes that are connected only to one VLAN. Load balancer service pods might not deploy to those worker nodes because the load balancer IP address is connected to a different VLAN than the worker nodes.

To force your app to deploy to specific worker nodes where load balancer service pods can also deploy to, you must add affinity rules and tolerations to your app deployment.

### Adding edge node affinity rules and tolerations
{: #edge_nodes}

When you [label worker nodes as edge nodes](cs_edge.html#edge_nodes) and also [taint the edge nodes](cs_edge.html#edge_workloads), load balancer service pods deploy only to those edge nodes, and app pods cannot deploy to edge nodes. When source IP is enabled for the load balancer service, the load balancer pods on the edge nodes cannot forward incoming requests to your app pods on other worker nodes.
{:shortdesc}

To force your app pods to deploy to edge nodes, add an edge node [affinity rule ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#node-affinity-beta-feature) and [toleration ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/#concepts) to the app deployment.

Example deployment yaml with edge node affinity and edge node toleration:

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

Both the **affinity** and **tolerations** sections have `dedicated` as the `key` and `edge` as the `value`.

### Adding affinity rules for multiple public or private VLANs
{: #edge_nodes_multiple_vlans}

When your cluster is connected to multiple public or private VLANs, your app pods might deploy to worker nodes that are connected only to one VLAN. If the load balancer IP address is connected to a different VLAN than these worker nodes, load balancer service pods won't deploy to those worker nodes.
{:shortdesc}

When source IP is enabled, schedule app pods on worker nodes that are the same VLAN as the load balancer's IP address by adding an affinity rule to the app deployment.

Before you begin, [target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster.

1. Get the IP address of the load balancer service. Look for the IP address in the **LoadBalancer Ingress** field.
    ```
    kubectl describe service <loadbalancer_service_name>
    ```
    {: pre}

2. Retrieve the VLAN ID that your load balancer service is connected to.

    1. List portable public VLANs for your cluster.
        ```
        ibmcloud ks cluster-get <cluster_name_or_ID> --showResources
        ```
        {: pre}

        Example output:
        ```
        ...

        Subnet VLANs
        VLAN ID   Subnet CIDR       Public   User-managed
        2234947   10.xxx.xx.xxx/29  false    false
        2234945   169.36.5.xxx/29   true     false
        ```
        {: screen}

    2. In the output under **Subnet VLANs**, look for the subnet CIDR that matches the load balancer IP address that you retrieved earlier and note the VLAN ID.

        For example, if the load balancer service IP address is `169.36.5.xxx`, the matching subnet in the example output of the previous step is `169.36.5.xxx/29`. The VLAN ID that the subnet is connected to is `2234945`.

3. [Add an affinity rule ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#node-affinity-beta-feature) to the app deployment for the VLAN ID that you noted in the previous step.

    For example, if you have multiple VLANs but want your app pods to deploy to worker nodes on the `2234945` public VLAN only:

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

    In the exmaple YAML, the **affinity** section has `publicVLAN` as the `key` and `"2234945"` as the `value`.

4. Apply the updated deployment configuration file.
    ```
    kubectl apply -f with-node-affinity.yaml
    ```
    {: pre}

5. Verify that the app pods deployed to worker nodes connected to the designated VLAN.

    1. List the pods in your cluster. Replace `<selector>` with the label that you used for the app.
        ```
        kubectl get pods -o wide app=<selector>
        ```
        {: pre}

        Example output:
        ```
        NAME                   READY     STATUS              RESTARTS   AGE       IP               NODE
        cf-py-d7b7d94db-vp8pq  1/1       Running             0          15d       172.30.xxx.xxx   10.176.48.78
        ```
        {: screen}

    2. In the output, identify a pod for your app. Note the **NODE** ID of the worker node that the pod is on.

        In the example output of the previous step, the app pod `cf-py-d7b7d94db-vp8pq` is on worker node `10.176.48.78`.

    3. List the details for the worker node.

        ```
        kubectl describe node <worker_node_ID>
        ```
        {: pre}

        Example output:

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

    4. In the **Labels** section of the output, verify that the public or private VLAN is the VLAN that you designated in previous steps.
