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


# Configuring subnets for clusters
{: #subnets}

Change the pool of available portable public or private IP addresses for load balancer services by adding subnets to your Kubernetes cluster.
{:shortdesc}

## Default VLANs, subnets, and IPs for clusters
{: #default_vlans_subnets}

During cluster creation, the cluster worker nodes and default subnets are automatically connected to a VLAN.

### VLANs
{: #vlans}

When you create a cluster, the cluster's worker nodes are connected automatically to a VLAN. A VLAN configures a group of worker nodes and pods as if they were attached to the same physical wire and provides a channel for connectivity among the workers and pods.

<dl>
<dt>VLANs for free clusters</dt>
<dd>In free clusters, the cluster's worker nodes are connected to an IBM-owned public VLAN and private VLAN by default. Because IBM controls the VLANs, subnets, and IP addresses, you cannot create multizone clusters or add subnets to your cluster, and can use only NodePort services to expose your app.</dd>
<dt>VLANs for standard clusters</dt>
<dd>In standard clusters, the first time that you create a cluster in a zone, a public VLAN and a private VLAN in that zone are automatically provisioned for you in your IBM Cloud infrastructure (SoftLayer) account. For every subsequent cluster that you create in that zone, you can reuse the same public and private VLAN because multiple clusters can share VLANs.</br></br>You can either connect your worker nodes to both a public VLAN and the private VLAN, or to the private VLAN only. If you want to connect your worker nodes to a private VLAN only, you can use the ID of an existing private VLAN or [create a private VLAN](/docs/cli/reference/softlayer/index.html#sl_vlan_create) and use the ID during cluster creation.</dd></dl>

To see the VLANs that are provisioned in each zone for your account, run `ibmcloud ks vlans <zone>.` To see the VLANs that one cluster is provisioned on, run `ibmcloud ks cluster-get <cluster_name_or_ID> --showResources` and look for the **Subnet VLANs** section.

**Note**: If you have a multizone cluster, multiple VLANs for a single zone cluster, or multiple subnets on the same VLAN, you must turn on VLAN spanning so that your worker nodes can communicate with each other on the private network. For instructions, see [Enable or disable VLAN spanning](/docs/infrastructure/vlans/vlan-spanning.html#vlan-spanning).

### Subnets and IP addresses
{: #subnets_ips}

In addition to worker nodes and pods, subnets are also automatically provisioned onto VLANs. Subnets provide network connectivity to your cluster components by assigning IP addresses to them.

The following subnets are automatically provisioned on the default public and private VLANs:

**Public VLAN subnets**
* The primary public subnet determines the public IP addresses that are assigned to worker nodes during cluster creation. Multiple clusters in on the same VLAN can share one primary public subnet.
* The portable public subnet is bound to one cluster only and provides the cluster with 8 public IP addresses. 3 IPs are reserved for Softlayer functions. 1 IP is used by the default public Ingress ALB and 4 IPs can be used to create public load balancer networking services. Portable public IPs are permanent, fixed IP addresses that can be used to access load balancer services over the internet. If you need more than 4 IPs for public load balancers, see [Adding portable IP addresses](#adding_ips).

**Private VLAN subnets**
* The primary private subnet determines the private IP addresses that are assigned to worker nodes during cluster creation. Multiple clusters in on the same VLAN can share one primary private subnet.
* The portable private subnet is bound to one cluster only and provides the cluster with 8 private IP addresses. 3 IPs are reserved for Softlayer functions. 1 IP is used by the default private Ingress ALB and 4 IPs can be used to create private load balancer networking services. Portable private IPs are permanent, fixed IP addresses that can be used to access load balancer services over the internet. If you need more than 4 IPs for private load balancers, see [Adding portable IP addresses](#adding_ips).

To see all of the subnets provisioned in your account, run `ibmcloud ks subnets`. To see the portable public and portable private subnets that are bound to one cluster, you can run `ibmcloud ks cluster-get <cluster_name_or_ID> --showResources` and look for the **Subnet VLANs** section.

**Note**: In {{site.data.keyword.containershort_notm}}, VLANs have a limit of 40 subnets. If you reach this limit, first check to see whether you can [reuse subnets in the VLAN to create new clusters](#custom). If you need a new VLAN, order one by [contacting {{site.data.keyword.Bluemix_notm}} support](/docs/infrastructure/vlans/order-vlan.html#order-vlans). Then, [create a cluster](cs_cli_reference.html#cs_cluster_create) that uses this new VLAN.

<br />


## Using custom or existing subnets to create a cluster
{: #custom}

When you create a standard cluster, subnets are automatically created for you. However, instead of using the automatically provisioned subnets, you can use existing portable subnets from your IBM Cloud infrastructure (SoftLayer) account or reuse subnets from a deleted cluster.
{:shortdesc}

Use this option to retain stable static IP addresses across cluster removals and creations, or to order larger blocks of IP addresses.

**Note:** Portable public IP addresses are charged monthly. If you remove portable public IP addresses after your cluster is provisioned, you still must pay the monthly charge, even if you used them only for a short amount of time.

Before you begin,
- [Target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster.
- To reuse subnets from a cluster that you no longer need, delete the unneeded cluster. Create the new cluster immediately because the subnets are deleted within 24 hours if you do not reuse them.

   ```
   ibmcloud ks cluster-rm <cluster_name_or_ID>
   ```
   {: pre}

To use an existing subnet in your IBM Cloud infrastructure (SoftLayer) portfolio with custom firewall rules or available IP addresses:

1. Get the ID of the subnet that you want to use and the ID of the VLAN that the subnet is on.

    ```
    ibmcloud ks subnets
    ```
    {: pre}

    In this example output, the subnet ID is `1602829` and the VLAN ID is `2234945`:
    ```
    Getting subnet list...
    OK
    ID        Network             Gateway          VLAN ID   Type      Bound Cluster
    1550165   10.xxx.xx.xxx/26    10.xxx.xx.xxx    2234947   private
    1602829   169.xx.xxx.xxx/28   169.xx.xxx.xxx   2234945   public
    ```
    {: screen}

2. [Create a cluster](cs_clusters.html#clusters_cli) by using the VLAN ID that you identified. Include the `--no-subnet` flag to prevent a new portable public IP subnet and a new portable private IP subnet from being created automatically.

    ```
    ibmcloud ks cluster-create --zone dal10 --machine-type b2c.4x16 --no-subnet --public-vlan 2234945 --private-vlan 2234947 --workers 3 --name my_cluster
    ```
    {: pre}
    If you can't remember which zone the VLAN is in for the `--zone` flag, you can check whether the VLAN is in a certain zone by running `ibmcloud ks vlans <zone>`.
    {: tip}

3.  Verify that the cluster was created. **Note:** It can take up to 15 minutes for the worker node machines to be ordered and for the cluster to be set up and provisioned in your account.

    ```
    ibmcloud ks clusters
    ```
    {: pre}

    When your cluster is fully provisioned, the **State** changes to `deployed`.

    ```
    Name         ID                                   State      Created          Workers   Zone   Version
    mycluster    aaf97a8843a29941b49a598f516da72101   deployed   20170201162433   3         dal10      1.10.5
    ```
    {: screen}

4.  Check the status of the worker nodes.

    ```
    ibmcloud ks workers <cluster_name_or_ID>
    ```
    {: pre}

    Before continuing to the next step, the worker nodes must be ready. The **State** changes to `normal` and the **Status** is `Ready`.

    ```
    ID                                                  Public IP        Private IP     Machine Type   State      Status   Zone   Version
    prod-dal10-pa8dfcc5223804439c87489886dbbc9c07-w1    169.xx.xxx.xxx   10.xxx.xx.xxx  free           normal     Ready    dal10      1.10.5
    ```
    {: screen}

5.  Add the subnet to your cluster by specifying the subnet ID. When you make a subnet available to a cluster, a Kubernetes configmap is created for you that includes all available portable public IP addresses that you can use. If no Ingress ALBs exist in the zone where the subnet's VLAN is located, one portable public and one portable private IP address is automatically used to create the public and private ALBs for that zone. You can use all other portable public and private IP addresses from the subnet to create load balancer services for your apps.

    ```
    ibmcloud ks cluster-subnet-add mycluster 807861
    ```
    {: pre}

6. **Important**: To enable communication between workers that are on different subnets on the same VLAN, you must [enable routing between subnets on the same VLAN](#subnet-routing).

<br />


## Managing existing portable IP addresses
{: #managing_ips}

By default, 4 portable public and 4 portable private IP addresses can be used to expose single apps to the public or private network by [creating a load balancer service](cs_loadbalancer.html). To create a load balancer service, you must have at least 1 portable IP address of the correct type available. You can view portable IP addresses that are available or free up a used portable IP address.

### Viewing available portable public IP addresses
{: #review_ip}

To list all of the portable IP addresses in your cluster, both used and available, you can run:

```
kubectl get cm ibm-cloud-provider-vlan-ip-config -n kube-system -o yaml
```
{: pre}

To list only portable public IP addresses that are available to create load balancers, you can use the following steps:

Before you begin, [set the context for the cluster you want to use.](cs_cli_install.html#cs_cli_configure)

1.  Create a Kubernetes service configuration file that is named `myservice.yaml` and define a service of type `LoadBalancer` with a dummy load balancer IP address. The following example uses the IP address 1.1.1.1 as the load balancer IP address.

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

2.  Create the service in your cluster.

    ```
    kubectl apply -f myservice.yaml
    ```
    {: pre}

3.  Inspect the service.

    ```
    kubectl describe service myservice
    ```
    {: pre}

    **Note:** The creation of this service fails because the Kubernetes master cannot find the specified load balancer IP address in the Kubernetes configmap. When you run this command, you can see the error message and the list of available public IP addresses for the cluster.

    ```
    Error on cloud load balancer a8bfa26552e8511e7bee4324285f6a4a for service default/myservice with UID 8bfa2655-2e85-11e7-bee4-324285f6a4af: Requested cloud provider IP 1.1.1.1 is not available. The following cloud provider IP addresses are available: <list_of_IP_addresses>
    ```
    {: screen}

<br />


### Freeing up used IP addresses
{: #free}

You can free up a used portable IP address by deleting the load balancer service that is using the portable IP address.
{:shortdesc}

Before you begin, [set the context for the cluster you want to use.](cs_cli_install.html#cs_cli_configure)

1.  List available services in your cluster.

    ```
    kubectl get services
    ```
    {: pre}

2.  Remove the load balancer service that uses a public or private IP address.

    ```
    kubectl delete service <service_name>
    ```
    {: pre}

<br />


## Adding portable IP addresses
{: #adding_ips}

By default, 4 portable public and 4 portable private IP addresses can be used to expose single apps to the public or private network by [creating a load balancer service](cs_loadbalancer.html). To create more than 4 public or 4 private load balancers, you can get more portable IP addresses by adding network subnets to the cluster.

**Note:**
* When you make a subnet available to a cluster, IP addresses of this subnet are used for cluster networking purposes. To avoid IP address conflicts, make sure to use a subnet with one cluster only. Do not use a subnet for multiple clusters or for other purposes outside of {{site.data.keyword.containershort_notm}} at the same time.
* Portable public IP addresses are charged monthly. If you remove portable public IP addresses after your subnet is provisioned, you still must pay the monthly charge, even if you used them only for a short amount of time.

### Adding portable IPs by ordering more subnets
{: #request}

You can get more portable IPs for load balancer services by creating a new subnet in an IBM Cloud infrastructure (SoftLayer) account and making it available to your specified cluster.
{:shortdesc}

Before you begin, [target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster.

1. Provision a new subnet.

    ```
    ibmcloud ks cluster-subnet-create <cluster_name_or_id> <subnet_size> <VLAN_ID>
    ```
    {: pre}

    <table>
    <caption>Understanding this command's components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding this command's components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>cluster-subnet-create</code></td>
    <td>The command to provision a subnet for your cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;cluster_name_or_id&gt;</em></code></td>
    <td>Replace <code>&lt;cluster_name_or_id&gt;</code> with the name or ID of the cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;subnet_size&gt;</em></code></td>
    <td>Replace <code>&lt;subnet_size&gt;</code> with the number of IP addresses that you want to add from your portable subnet. Accepted values are 8, 16, 32, or 64. <p>**Note:** When you add portable IP addresses for your subnet, three IP addresses are used to establish cluster-internal networking. You cannot use these three IP addresses for your application load balancer or to create a load balancer service. For example, if you request eight portable public IP addresses, you can use five of them to expose your apps to the public.</p> </td>
    </tr>
    <tr>
    <td><code><em>&lt;VLAN_ID&gt;</em></code></td>
    <td>Replace <code>&lt;VLAN_ID&gt;</code> with the ID of the public or private VLAN on which you want to allocate the portable public or private IP addresses. You must select the public or private VLAN that an existing worker node is connected to. To review the public or private VLAN for a worker node, run the <code>ibmcloud ks worker-get &lt;worker_id&gt;</code> command. <The subnet is provisioned in the same zone that the VLAN is in.</td>
    </tr>
    </tbody></table>

2. Verify that the subnet was successfully created and added to your cluster. The subnet CIDR is listed in the **Subnet VLANs** section.

    ```
    ibmcloud ks cluster-get --showResources <cluster_name_or_ID>
    ```
    {: pre}

    In this example output, a second subnet was added to the `2234945` public VLAN:
    ```
    Subnet VLANs
    VLAN ID   Subnet CIDR          Public   User-managed
    2234947   10.xxx.xx.xxx/29     false    false
    2234945   169.xx.xxx.xxx/29    true     false
    2234945   169.xx.xxx.xxx/29    true     false
    ```
    {: screen}

3. **Important**: To enable communication between workers that are on different subnets on the same VLAN, you must [enable routing between subnets on the same VLAN](#subnet-routing).

<br />


### Adding portable private IPs by using user-managed subnets
{: #user_managed}

You can get more portable private IPs for load balancer services by making a subnet from an on-premises network available to your specified cluster.
{:shortdesc}

Requirements:
- User-managed subnets can be added to private VLANs only.
- The subnet prefix length limit is /24 to /30. For example, `169.xx.xxx.xxx/24` specifies 253 usable private IP addresses, while `169.xx.xxx.xxx/30` specifies 1 usable private IP address.
- The first IP address in the subnet must be used as the gateway for the subnet.

Before you begin:
- Configure the routing of network traffic into and out of the external subnet.
- Confirm that you have VPN connectivity between the on-premises data center network gateway and either the private network Virtual Router Appliance or the strongSwan VPN service that runs in your cluster. For more information, see [Setting up VPN connectivity](cs_vpn.html).

To add a subnet from an on-premises network:

1. View the ID of your cluster's private VLAN. Locate the **Subnet VLANs** section. In the field **User-managed**, identify the VLAN ID with _false_.

    ```
    ibmcloud ks cluster-get --showResources <cluster_name>
    ```
    {: pre}

    ```
    Subnet VLANs
    VLAN ID   Subnet CIDR       Public   User-managed
    2234947   10.xxx.xx.xxx/29  false    false
    2234945   169.xx.xxx.xxx/29 true     false
    ```
    {: screen}

2. Add the external subnet to your private VLAN. The portable private IP addresses are added to the cluster's configmap.

    ```
    ibmcloud ks cluster-user-subnet-add <cluster_name> <subnet_CIDR> <VLAN_ID>
    ```
    {: pre}

    Example:

    ```
    ibmcloud ks cluster-user-subnet-add mycluster 10.xxx.xx.xxx/24 2234947
    ```
    {: pre}

3. Verify that the user-provided subnet is added. The field **User-managed** is _true_.

    ```
    ibmcloud ks cluster-get --showResources <cluster_name>
    ```
    {: pre}

    In this example output, a second subnet was added to the `2234947` private VLAN:
    ```
    Subnet VLANs
    VLAN ID   Subnet CIDR       Public   User-managed
    2234947   10.xxx.xx.xxx/29  false    false
    2234945   169.xx.xxx.xxx/29 true     false
    2234947   10.xxx.xx.xxx/24  false    true
    ```
    {: screen}

4. [Enable routing between subnets on the same VLAN](#subnet-routing).

5. Add a [private load balancer service](cs_loadbalancer.html) or enable a [private Ingress ALB](cs_ingress.html#private_ingress) to access your app over the private network. To use a private IP address from the subnet that you added, you must specify an IP address from the subnet CIDR. Otherwise, an IP address is chosen at random from the IBM Cloud infrastructure (SoftLayer) subnets or user-provided subnets on the private VLAN.

<br />


## Managing subnet routing
{: #subnet-routing}

If you have a multizone cluster, multiple VLANs for a single zone cluster, or multiple subnets on the same VLAN, you must turn on VLAN spanning so that your worker nodes can communicate with each other on the private network. For instructions, see [Enable or disable VLAN spanning](/docs/infrastructure/vlans/vlan-spanning.html#vlan-spanning).

Review the following scenarios in which VLAN spanning is also required.

### Enabling routing between primary subnets on the same VLAN
{: #vlan-spanning}

When you create a cluster, a subnet that ends in `/26` is provisioned on the default private primary VLAN. A private primary subnet can provide IPs for up to 62 worker nodes.
{:shortdesc}

This 62 worker node limit might be exceeded by a large cluster or by several smaller clusters in a single region that are on the same VLAN. When the 62 worker node limit is reached, a second private primary subnet in the same VLAN is ordered.

To ensure that workers in these primary subnets on the same VLAN can communicate, you must turn on VLAN spanning. For instructions, see [Enable or disable VLAN spanning](/docs/infrastructure/vlans/vlan-spanning.html#vlan-spanning).

### Managing subnet routing for gateway appliances
{: #vra-routing}

When you create a cluster, a portable public and a portable private subnet are ordered on the VLANs that the cluster is connected to. These subnets provide IP addresses for Ingress and load balancer networking services.

However, if you have an existing router appliance, such as a [Virtual Router Appliance (VRA)](/docs/infrastructure/virtual-router-appliance/about.html#about), the newly added portable subnets from those VLANs that the cluster is connected to are not configured on the router. To use Ingress or load balancer networking services, you must ensure that network devices can route between different subnets on the same VLAN by [enabling VLAN spanning](/docs/infrastructure/vlans/vlan-spanning.html#vlan-spanning).
