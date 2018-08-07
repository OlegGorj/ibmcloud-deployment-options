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




# High availability for {{site.data.keyword.containerlong_notm}}
{: #ha}

Use the built-in Kubernetes and {{site.data.keyword.containerlong}} features to make your cluster more highly available and to protect your app from downtime when a component in your cluster fails.
{: shortdesc}

High availability is a core discipline in an IT infrastructure to keep your apps up and running, even after a partial or full site failure. The main purpose of high availability is to eliminate potential points of failures in an IT infrastructure. For example, you can prepare for the failure of one system by adding redundancy and setting up failover mechanisms.

You can achieve high availability on different levels in your IT infrastructure and within different components of your cluster. The level of availability that is right for you depends on several factors, such as your business requirements, the Service Level Agreements that you have with your customers, and the money that you want to spend.

## Overview of potential points of failure in {{site.data.keyword.containerlong_notm}}
{: #fault_domains} 

The {{site.data.keyword.containerlong_notm}} architecture and infrastructure is designed to ensure reliability, low processing latency, and a maximum uptime of the service. However, failures can happen. Depending on the service that you host in {{site.data.keyword.Bluemix_notm}}, you might not be able to tolerate failures, even if failures last for only a few minutes.
{: shortdesc}

{{site.data.keyword.containershort_notm}} provides several approaches to add more availability to your cluster by adding redundancy and anti-affinity. Review the following image to learn about potential points of failure and how to eliminate them.

<img src="images/cs_failure_ov.png" alt="Overview of fault domains in a high availability cluster within an {{site.data.keyword.containershort_notm}} region." width="250" style="width:250px; border-style: none"/>

<dl>
<dt> 1. Container or pod failure.</dt>
  <dd><p>Containers and pods are, by design, short-lived and can fail unexpectedly. For example, a container or pod might crash if an error occurs in your app. To make your app highly available, you must ensure that you have enough instances of your app to handle the workload plus additional instances in case of a failure. Ideally, these instances are distributed across multiple worker nodes to protect your app from a worker node failure.</p>
  <p>See [Deploying highly available apps.](cs_app.html#highly_available_apps)</p></dd>
<dt> 2. Worker node failure.</dt>
  <dd><p>A worker node is a VM that runs on top of a physical hardware. Worker node failures include hardware outages, such as power, cooling, or networking, and issues on the VM itself. You can account for a worker node failure by setting up multiple worker nodes in your cluster. <br/><br/><strong>Note:</strong> Worker nodes in one zone are not guaranteed to be on separate physical compute hosts. For example, you might have a cluster with 3 worker nodes, but all 3 worker nodes were created on the same physical compute host in the IBM zone. If this physical compute host goes down, all your worker nodes are down. To protect against this failure, you must set up a second cluster in another zone.</p>
  <p>See [Creating clusters with multiple worker nodes.](cs_cli_reference.html#cs_cluster_create)</p></dd>
<dt> 3. Cluster failure.</dt>
  <dd><p>The Kubernetes master is the main component that keeps your cluster up and running. The master stores all cluster data in the etcd database that serves as the single point of truth for your cluster. A cluster failure occurs when the master cannot be reached due to a networking failure or when data in your etcd database get corrupted. You can create multiple clusters in one zone to protect your apps from a Kubernetes master or etcd failure. To load balance between the clusters, you must set up an external load balancer. <br/><br/><strong>Note:</strong> Setting up multiple clusters in one zone does not guarantee that your worker nodes are deployed on separate physical compute hosts. To protect against this failure, you must set up a second cluster in another zone.</p>
  <p>See [Setting up highly available clusters.](cs_clusters.html#ha_clusters)</p></dd>
<dt> 4. Zone failure.</dt>
  <dd><p>A zone failure affects all physical compute hosts and NFS storage. Failures include power, cooling, networking, or storage outages, and natural disasters, like flooding, earthquakes, and hurricanes. To protect against a zone failure, you must have clusters in two different zones that are load balanced by an external load balancer.</p>
  <p>See [Setting up highly available clusters.](cs_clusters.html#ha_clusters)</p></dd>    
<dt> 5. Region failure.</dt>
  <dd><p>Every region is set up with a highly available load balancer that is accessible from the region-specific API endpoint. The load balancer routes incoming and outgoing requests to clusters in the regional zones. The likelihood of a full regional failure is low. However, to account for this failure, you can set up multiple clusters in different regions and connect them by using an external load balancer. In case an entire region fails, the cluster in the other region can take over the work load. <br/><br/><strong>Note:</strong> A multi-region cluster requires several Cloud resources, and depending on your app, can be complex and expensive. Check if you need a multi-region setup or if you can accommodate a potential service disruption. If you want to set up a multi-region cluster, make sure that your app and the data can be hosted in another region, and that your app can handle global data replication.</p>
  <p>See [Setting up highly available clusters.](cs_clusters.html#ha_clusters)</p></dd>   
<dt> 6a, 6b. Storage failure.</dt>
  <dd><p>In a stateful app, data plays an important role to keep your app up and running. You want to make sure that your data is highly available so that you can recover from a potential failure. In {{site.data.keyword.containershort_notm}} you can choose from several options to persist your data. For example, you can provision NFS storage by using Kubernetes native persistent volumes, or store your data by using an {{site.data.keyword.Bluemix_notm}} database service.</p>
  <p>See [Planning highly available data.](cs_storage_planning.html#persistent)</p></dd> 
</dl>
