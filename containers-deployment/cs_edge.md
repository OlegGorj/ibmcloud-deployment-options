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



# Restricting network traffic to edge worker nodes
{: #edge}

Edge worker nodes can improve the security of your Kubernetes cluster by allowing fewer worker nodes to be accessed externally and by isolating the networking workload in {{site.data.keyword.containerlong}}.
{:shortdesc}

When these worker nodes are marked for networking only, other workloads cannot consume the CPU or memory of the worker node and interfere with networking.

If you have a multizone cluster and want to restrict network traffic to edge worker nodes, at least 2 edge worker nodes must be enabled in each zone for high availability of load balancer or Ingress pods. Create an edge node worker pool that spans all the zones in your cluster, with at least 2 worker nodes per zone.
{: tip}

## Labeling worker nodes as edge nodes
{: #edge_nodes}

Add the `dedicated=edge` label to two or more worker nodes on each public VLAN in your cluster to ensure that Ingress and load balancers are deployed to those worker nodes only.
{:shortdesc}

Before you begin:

- [Create a standard cluster.](cs_clusters.html#clusters_cli)
- Ensure that your cluster has a least one public VLAN. Edge worker nodes are not available for clusters with private VLANs only.
- [Create a new worker pool](cs_clusters.html#add_pool) that spans all the zone in your cluster and has at least 2 workers per zone.
- [Target the Kubernetes CLI to the cluster](cs_cli_install.html#cs_cli_configure).

To label worker nodes as edge nodes:

1. List the worker nodes in your edge node worker pool. Use the private IP address from the **NAME** column to identify the nodes.

  ```
  ibmcloud ks workers <cluster_name_or_ID> --worker-pool <edge_pool_name>
  ```
  {: pre}

2. Label the worker nodes with `dedicated=edge`. After a worker node is marked with `dedicated=edge`, all subsequent Ingress and load balancers are deployed to an edge worker node.

  ```
  kubectl label nodes <node1_IP> <node2_IP> dedicated=edge
  ```
  {: pre}

3. Retrieve all of the existing load balancer services in the cluster.

  ```
  kubectl get services --all-namespaces -o jsonpath='{range .items[*]}kubectl get service -n {.metadata.namespace} {.metadata.name} -o yaml | kubectl apply -f - :{.spec.type},{end}' | tr "," "\n" | grep "LoadBalancer" | cut -d':' -f1
  ```
  {: pre}

  Example output:

  ```
  kubectl get service -n <namespace> <service_name> -o yaml | kubectl apply -f
  ```
  {: screen}

4. Using the output from the previous step, copy and paste each `kubectl get service` line. This command redeploys the load balancer to an edge worker node. Only public load balancers must be redeployed.

  Example output:

  ```
  service "my_loadbalancer" configured
  ```
  {: screen}

You labeled worker nodes with `dedicated=edge` and redeployed all of the existing load balancers and Ingress to the edge worker nodes. Next, prevent other [workloads from running on edge worker nodes](#edge_workloads) and [block inbound traffic to NodePorts on worker nodes](cs_network_policy.html#block_ingress).

<br />


## Preventing workloads from running on edge worker nodes
{: #edge_workloads}

A benefit of edge worker nodes is that they can be specified to run networking services only.
{:shortdesc}

Using the `dedicated=edge` toleration means that all load balancer and Ingress services are deployed to the labeled worker nodes only. However, to prevent other workloads from running on edge worker nodes and consuming worker node resources, you must use [Kubernetes taints ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/).


1. List all of the worker nodes with the `dedicated=edge` label.

  ```
  kubectl get nodes -L publicVLAN,privateVLAN,dedicated -l dedicated=edge
  ```
  {: pre}

2. Apply a taint to each worker node that prevents pods from running on the worker node and that removes pods that do not have the `dedicated=edge` label from the worker node. The pods that are removed are redeployed on other worker nodes with capacity.

  ```
  kubectl taint node <node_name> dedicated=edge:NoSchedule dedicated=edge:NoExecute
  ```
  Now, only pods with the `dedicated=edge` toleration are deployed to your edge worker nodes.

3. If you choose to [enable source IP preservation for a load balancer service ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tutorials/services/source-ip/#source-ip-for-services-with-typeloadbalancer), ensure that app pods are scheduled onto the edge worker nodes by [adding edge node affinity to app pods](cs_loadbalancer.html#edge_nodes). App pods must be scheduled onto edge nodes to receive incoming requests.
