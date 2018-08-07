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


# Planning cluster networking
{: #planning}

With {{site.data.keyword.containerlong}}, you can manage both external networking by making apps publicly or privately accessible and internal networking within your cluster.
{: shortdesc}

## Choosing a NodePort, LoadBalancer, or Ingress service
{: #external}

To make your apps externally accessible from the [public internet](#public_access) or a [private network](#private_both_vlans), {{site.data.keyword.containershort_notm}} supports three networking services.
{:shortdesc}

**[NodePort service](cs_nodeport.html)** (free and standard clusters)
* Expose a port on every worker node and use the public or private IP address of any worker node to access your service in the cluster.
* Iptables is a Linux kernel feature that load balances requests across the app's pods, provides high-performance networking routing, and provides network access control.
* The public and private IP addresses of the worker node are not permanent. When a worker node is removed or re-created, a new public and a new private IP address are assigned to the worker node.
* The NodePort service is great for testing public or private access. It can also be used if you need public or private access for only a short amount of time.

**[LoadBalancer service](cs_loadbalancer.html)** (standard clusters only)
* Every standard cluster is provisioned with four portable public and four portable private IP addresses that you can use to create an external TCP/UDP load balancer for your app.
* Iptables is a Linux kernel feature that load balances requests across the app's pods, provides high-performance networking routing, and provides network access control.
* The portable public and private IP addresses that are assigned to the load balancer are permanent and do not change when a worker node is re-created in the cluster.
* You can customize your load balancer by exposing any port that your app requires.

**[Ingress](cs_ingress.html)** (standard clusters only)
* Expose multiple apps in a cluster by creating one external HTTP or HTTPS, TCP, or UDP application load balancer (ALB). The ALB uses a secured and unique public or private entry point to route incoming requests to your apps.
* You can use one route to expose multiple apps in your cluster as services.
* Ingress consists of three components:
  * The Ingress resource defines the rules for how to route and load balance incoming requests for an app.
  * The ALB listens for incoming HTTP or HTTPS, TCP, or UDP service requests. It forwards requests across the apps' pods based on the rules that you defined in the Ingress resource.
  * The multizone load balancer (MZLB) handles all incoming requests to your apps and load balances the requests among the ALBs in the various zones.
* Use Ingress to implement your own ALB with custom routing rules and need SSL termination for your apps.

To choose the best networking service for your app, you can follow this decision tree and click one of the options to get started.

<img usemap="#networking_map" border="0" class="image" src="images/networkingdt.png" width="500px" alt="This image walks you through choosing the best networking option for your application. If this image is not displaying, the information can still be found in the documentation." style="width:500px;" />
<map name="networking_map" id="networking_map">
<area href="/docs/containers/cs_nodeport.html" alt="Nodeport service" shape="circle" coords="52, 283, 45"/>
<area href="/docs/containers/cs_loadbalancer.html" alt="LoadBalancer service" shape="circle" coords="247, 419, 44"/>
<area href="/docs/containers/cs_ingress.html" alt="Ingress service" shape="circle" coords="445, 420, 45"/>
</map>

<br />


## Planning public external networking
{: #public_access}

When you create a Kubernetes cluster in {{site.data.keyword.containershort_notm}}, you can connect the cluster to a public VLAN. The public VLAN determines the public IP address that is assigned to each worker node, which provides each worker node with a public network interface.
{:shortdesc}

To make an app publicly available to the internet, you can create a NodePort, LoadBalancer, or Ingress service. To compare each service, see [Choosing a NodePort, LoadBalancer, or Ingress service](#external).

The following diagram shows how Kubernetes forwards public network traffic in {{site.data.keyword.containershort_notm}}.

![{{site.data.keyword.containershort_notm}} Kubernetes architecture](images/networking.png)

*Kubernetes data plane in {{site.data.keyword.containershort_notm}}*

The public network interface for the worker nodes in both free and standard clusters is protected by Calico network policies. These policies block most inbound traffic by default. However, inbound traffic that is necessary for Kubernetes to function is allowed, as are connections to NodePort, LoadBalancer, and Ingress services. For more information about these policies, including how to modify them, see [Network policies](cs_network_policy.html#network_policies).

<br />


## Planning private external networking for a public and private VLAN setup
{: #private_both_vlans}

When you create a Kubernetes cluster in {{site.data.keyword.containershort_notm}}, you must connect your cluster to a private VLAN. The private VLAN determines the private IP address that is assigned to each worker node, which provides each worker node with a private network interface.
{:shortdesc}

When you want to keep your apps connected to a private network only, you can use the private network interface for the worker nodes in standard clusters. However, when your worker nodes are connected to both a public and a private VLAN, you must also use Calico network policies to secure your cluster from unwanted public access.

The following sections describe the capabilities across {{site.data.keyword.containershort_notm}} that you can use to expose you apps to a private network and secure your cluster from unwanted public access. Optionally, you can also isolate your networking workloads and connect your cluster to resources in an on-premises network.

### Expose your apps with private networking services and secure your cluster with Calico network policies
{: #private_both_vlans_calico}

The public network interface for worker nodes is protected by [predefined Calico network policy settings](cs_network_policy.html#default_policy) that are configured on every worker node during cluster creation. By default, all outbound network traffic is allowed for all worker nodes. Inbound network traffic is blocked except for a few ports that are opened so that network traffic can be monitored by IBM and for IBM to automatically install security updates for the Kubernetes master. Access to the worker node's kubelet is secured by an OpenVPN tunnel. For more information, see the [{{site.data.keyword.containershort_notm}} architecture](cs_tech.html).

If you expose your apps with a NodePort service, a LoadBalancer service, or an Ingress application load balancer, the default Calico policies also allow inbound network traffic from the internet to these services. To make your app accessible from a private network only, you can choose to use only private NodePort, LoadBalancer, or Ingress services and block all public traffic to the services.

**NodePort**
* [Create a NodePort service](cs_nodeport.html). In addition to the public IP address, a NodePort service is available over the private IP address of a worker node.
* A NodePort service opens a port on a worker node over both the private and public IP address of the worker node. You must use a [Calico preDNAT network policy](cs_network_policy.html#block_ingress) to block the public NodePorts.

**LoadBalancer**
* [Create a private LoadBalancer service](cs_loadbalancer.html).
* A load balancer service with a portable private IP address still has a public node port open on every worker node. You must use a [Calico preDNAT network policy](cs_network_policy.html#block_ingress) to block public node ports on it.

**Ingress**
* When you create a cluster, one public and one private Ingress application load balancer (ALB) are created automatically. Because the public ALB is enabled and the private ALB is disabled by default, you must [disable the public ALB](cs_cli_reference.html#cs_alb_configure) and [enable the private ALB](cs_ingress.html#private_ingress).
* Then, [create a private Ingress service](cs_ingress.html#ingress_expose_private).

For more information about each service, see [Choosing a NodePort, LoadBalancer, or Ingress service](#external).

### Optional: Isolate networking workloads to edge worker nodes
{: #private_both_vlans_edge}

Edge worker nodes can improve the security of your cluster by allowing fewer worker nodes to be accessed externally and by isolating the networking workload. To ensure that Ingress and load balancer pods are deployed to only specified worker nodes, [label worker nodes as edge nodes](cs_edge.html#edge_nodes). To also prevent other workloads from running on edge nodes, [taint the edge nodes](cs_edge.html#edge_workloads).

Then, use a [Calico preDNAT network policy](cs_network_policy.html#block_ingress) to block traffic to public node ports on clusters that are running edge worker nodes. Blocking node ports ensures that the edge worker nodes are the only worker nodes that handle incoming traffic.

### Optional: Connect to an on-premises database by using strongSwan VPN
{: #private_both_vlans_vpn}

To securely connect your worker nodes and apps to an on-premises network, you can set up a [strongSwan IPSec VPN service ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.strongswan.org/about.html). The strongSwan IPSec VPN service provides a secure end-to-end communication channel over the internet that is based on the industry-standard Internet Protocol Security (IPSec) protocol suite. To set up a secure connection between your cluster and an on-premises network, [configure and deploy the strongSwan IPSec VPN service](cs_vpn.html#vpn-setup) directly in a pod in your cluster.

<br />


## Planning private external networking for a private VLAN setup only
{: #private_vlan}

When you create a Kubernetes cluster in {{site.data.keyword.containershort_notm}}, you must connect your cluster to a private VLAN. The private VLAN determines the private IP address that is assigned to each worker node, which provides each worker node with a private network interface.
{:shortdesc}

When your worker nodes are connected to a private VLAN only, you can use the private network interface for the worker nodes to keep apps connect to the private network only. You can then use a gateway appliance to secure your cluster from unwanted public access.

The following sections describe the capabilities across {{site.data.keyword.containershort_notm}} that you can use to secure your cluster from unwanted public access, expose you apps to a private network, and connect to resources in an on-premises network.

### Configure a gateway appliance
{: #private_vlan_gateway}

If worker nodes are set up with a private VLAN only, you must configure an alternative solution for network connectivity. You can set up a firewall with custom network policies to provide dedicated network security for your standard cluster and to detect and remediate network intrusion. For example, you might choose to set up a [Virtual Router Appliance](/docs/infrastructure/virtual-router-appliance/about.html) or a [Fortigate Security Appliance](/docs/infrastructure/fortigate-10g/about.html) to act as your firewall and block unwanted traffic. When you set up a firewall, [you must also open up the required ports and IP addresses](cs_firewall.html#firewall_outbound) for each region so that the master and the worker nodes can communicate. 

**Note**: If you have an existing router appliance and then add a cluster, the new portable subnets that are ordered for the cluster are not configured on the router appliance. In order to use networking services, you must enable routing between the subnets on the same VLAN by [enabling VLAN spanning](cs_subnets.html#vra-routing).

### Expose your apps with private networking services
{: #private_vlan_services}

To make your app accessible from a private network only, you can use private NodePort, LoadBalancer, or Ingress services. Because your worker nodes are not connected to a public VLAN, no public traffic is routed to these services.

**NodePort**:
* [Create a private NodePort service](cs_nodeport.html). The service is available over the private IP address of a worker node.
* In your private firewall, open the port that you configured when you deployed the service to the private IP addresses for all of the worker nodes to allow traffic to. To find the port, run `kubectl get svc`. The port is in the 20000-32000 range.

**LoadBalancer**
* [Create a private LoadBalancer service](cs_loadbalancer.html). If your cluster is available on a private VLAN only, one of the four available portable private IP addresses is used.
* In your private firewall, open the port that you configured when you deployed the service to the load balancer service's private IP address.

**Ingress**:
* When you create a cluster, a private Ingress application load balancer (ALB) is created automatically but is not enabled by default. You must [enable the private ALB](cs_ingress.html#private_ingress).
* Then, [create a private Ingress service](cs_ingress.html#ingress_expose_private).
* In your private firewall, open port 80 for HTTP or port 443 for HTTPS to the IP address for the private ALB.


For more information about each service, see [Choosing a NodePort, LoadBalancer, or Ingress service](#external).

### Optional: Connect to an on-premises database by using the gateway appliance
{: #private_vlan_vpn}

To securely connect your worker nodes and apps to an on-premises network, you must set up a VPN gateway. You can use the [Virtual Router Appliance (VRA)](/docs/infrastructure/virtual-router-appliance/about.html) or [Fortigate Security Appliance (FSA)](/docs/infrastructure/fortigate-10g/about.html) that you set up as a firewall to also configure an IPSec VPN endpoint. To configure a VRA, see [Setting up VPN connectivity with VRA](cs_vpn.html#vyatta).

<br />


## Planning in-cluster networking
{: #in-cluster}

All pods that are deployed to a worker node are assigned a private IP address in the 172.30.0.0/16 range and are routed between worker nodes only. To avoid conflicts, do not use this IP range on any nodes that communicate with your worker nodes. Worker nodes and pods can securely communicate on the private network by using private IP addresses. However, when a pod crashes or a worker node needs to be re-created, a new private IP address is assigned.

By default, it is difficult to track changing private IP addresses for apps that must be highly available. Instead, you can use the built-in Kubernetes service discovery features to expose apps as cluster IP services on the private network. A Kubernetes service groups a set of pods and provides a network connection to these pods for other services in the cluster without exposing the actual private IP address of each pod. Services are assigned an in-cluster IP address that is accessible inside the cluster only.
* **Older clusters**: In clusters that were created before February 2018 in the dal13 zone or before October 2017 in any other zone, the services are assigned an IP from one of 254 IPs in the 10.10.10.0/24 range. If you hit the limit of 254 services and need more services, you must create a new cluster.
* **Newer clusters**: In clusters that were created after February 2018 in the dal13 zone or after October 2017 in any other zone, the services are assigned an IP from one of the 65,000 IPs in the 172.21.0.0/16 range.

To avoid conflicts, do not use this IP range on any nodes that communicate with your worker nodes. A DNS lookup entry is also created for the service and stored in the `kube-dns` component of the cluster. The DNS entry contains the name of the service, the namespace where the service was created, and the link to the assigned in-cluster IP address.

To access a pod behind a cluster IP service, apps can either use the in-cluster IP address of the service or send a request by using the name of the service. When you use the name of the service, the name is looked up in the `kube-dns` component and routed to the in-cluster IP address of the service. When a request reaches the service, the service ensures that all requests are equally forwarded to the pods, independent of their in-cluster IP addresses and the worker node they are deployed to.
