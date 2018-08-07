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




# Exposing apps with NodePorts
{: #nodeport}

Make your containerized app available to internet access by using the public IP address of any worker node in a Kubernetes cluster and exposing a NodePort. Use this option for testing in {{site.data.keyword.containerlong}} and for short-term public access.
{:shortdesc}

## Managing network traffic by using NodePorts
{: #planning}

Expose a public port on your worker node and use the public IP address of the worker node to access your service in the cluster publicly from the internet.
{:shortdesc}

When you expose your app by creating a Kubernetes service of type NodePort, a NodePort in the range of 30000 - 32767 and an internal cluster IP address is assigned to the service. The NodePort service serves as the external entry point for incoming requests for your app. The assigned NodePort is publicly exposed in the kubeproxy settings of each worker node in the cluster. Every worker node starts listening on the assigned NodePort for incoming requests for the service. To access the service from the internet, you can use the public IP address of any worker node that was assigned during cluster creation and the NodePort in the format `<IP_address>:<nodeport>`. In addition to the public IP address, a NodePort service is available over the private IP address of a worker node.

The following diagram shows how communication is directed from the internet to an app when a NodePort service is configured:

<img src="images/cs_nodeport_planning.png" width="550" alt="Expose an app in {{site.data.keyword.containershort_notm}} by using NodePort" style="width:550px; border-style: none"/>

1. A request is sent to your app by using the public IP address of your worker node and the NodePort on the worker node.

2. The request is automatically forwarded to the NodePort service's internal cluster IP address and port. The internal cluster IP address is accessible inside the cluster only.

3. `kube-proxy` routes the request to the Kubernetes NodePort service for the app.

4. The request is forwarded to the private IP address of the pod where the app is deployed. If multiple app instances are deployed in the cluster, the NodePort service routes the requests between the app pods.

**Note:** The public IP address of the worker node is not permanent. When a worker node is removed or re-created, a new public IP address is assigned to the worker node. You can use the NodePort service for testing the public access for your app or when public access is needed for a short amount of time only. When you require a stable public IP address and more availability for your service, expose your app by using a [LoadBalancer service](cs_loadbalancer.html) or [Ingress](cs_ingress.html).

<br />


## Enabling access to an app by using a NodePort service
{: #config}

You can expose your app as a Kubernetes NodePort service for free or standard clusters.
{:shortdesc}

If you do not already have an app ready, you can use a Kubernetes example app called [Guestbook ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/examples/blob/master/guestbook/all-in-one/guestbook-all-in-one.yaml).

1.  In the configuration file for your app, define a [service ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/services-networking/service/) section. **Note**: For the Guestbook example, a front-end service section exists in the configuration file. To make the Guestbook app available externally, add the NodePort type and a NodePort in the range 30000 - 32767 to the front-end service section.

    Example:

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
    <caption>Understanding the NodePort service components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the NodePort service section components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>metadata.name</code></td>
    <td>Replace <code><em>&lt;my-nodeport-service&gt;</em></code> with a name for your NodePort service.<p>Learn more about [securing your personal information](cs_secure.html#pi) when you work with Kubernetes resources.</p></td>
    </tr>
    <tr>
    <td><code>metadata.labels</code></td>
    <td>Replace <code><em>&lt;my-label-key&gt;</em></code> and <code><em>&lt;my-label-value&gt;</em></code> with the label that you want to use for your service.</td>
    </tr>
    <tr>
      <td><code>spec.selector</code></td>
      <td>Replace <code><em>&lt;my-selector-key&gt;</em></code> and <code><em>&lt;my-selector-value&gt;</em></code> with the key/value pair that you used in the <code>spec.template.metadata.labels</code> section of your deployment yaml. To associate the service with the deployment, the selector must match the deployment labels.
      </tr>
    <tr>
    <td><code>ports.port</code></td>
    <td>Replace <code><em>&lt;8081&gt;</em></code> with the port that your service listens on. </td>
     </tr>
     <tr>
     <td><code>ports.nodePort</code></td>
     <td>Optional: Replace <code><em>&lt;31514&gt;</em></code> with a NodePort in the 30000 - 32767 range. Do not specify a NodePort that is already in use by another service. If no NodePort is assigned, a random one is assigned for you.<br><br>To specify a NodePort and see which NodePorts are already in use, run the following command: <pre class="pre"><code>kubectl get svc</code></pre><p>Any NodePorts in use appear under the **Ports** field.</p></td>
     </tr>
     </tbody></table>

2.  Save the updated configuration file.

3.  Repeat these steps to create a NodePort service for each app that you want to expose to the internet.

**What's next:**

When the app is deployed, you can use the public IP address of any worker node and the NodePort to form the public URL to access the app in a browser. If your worker nodes are connected to a private VLAN only, then a private NodePort service was created and can be accessible through q worker node's private IP address.

1.  Get the public IP address for a worker node in the cluster. If you want to access the worker node on a private network, get the private IP address instead.

    ```
    ibmcloud ks workers <cluster_name>
    ```
    {: pre}

    Output:

    ```
    ID                                                Public IP   Private IP    Size     State    Status
    prod-dal10-pa215dcf5bbc0844a990fa6b0fcdbff286-w1  192.0.2.23  10.100.10.10  u2c.2x4  normal   Ready
    prod-dal10-pa215dcf5bbc0844a990fa6b0fcdbff286-w2  192.0.2.27  10.100.10.15  u2c.2x4  normal   Ready
    ```
    {: screen}

2.  If a random NodePort was assigned, find out which one was assigned.

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

    In this example, the NodePort is `30872`.</br>
    **Note:** If the **Endpoints** section displays `<none>`, check the `<selectorkey>` and `<selectorvalue>` that you use in the `spec.selector` section of the NodePort service. Ensure that it is the same as the _key/value_ pair that you used in the `spec.template.metadata.labels` section of your deployment yaml.

3.  Form the URL with one of the worker node IP addresses and the NodePort. Example: `http://192.0.2.23:30872`
