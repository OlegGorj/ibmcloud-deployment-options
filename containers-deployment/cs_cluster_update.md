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





# Updating clusters, worker nodes, and add-ons
{: #update}

You can install updates to keep your Kubernetes clusters up-to-date in {{site.data.keyword.containerlong}}.
{:shortdesc}

## Updating the Kubernetes master
{: #master}

Periodically, Kubernetes releases [major, minor, or patch updates](cs_versions.html#version_types). Updates can affect the Kubernetes API server version or other components in your Kubernetes master. IBM updates the patch version, but you must update the master major and minor versions.
{:shortdesc}

**How do I know when to update the master?**</br>
You are notified in the GUI and CLI when updates are available, and can also check our [supported versions](cs_versions.html) page.

**How many versions behind the latest can the master be?**</br>
IBM generally supports 3 versions of Kubernetes at a given time. You can update the Kubernetes API server no more than 2 versions ahead of its current version.

For example, if your current Kubernetes API server version is 1.7 and you want to update to 1.10, you must first update to 1.8 or 1.9. You can force the update to occur, but updating three or more minor versions might cause unexpected results or failure.

If your cluster is running an unsupported Kubernetes version, you might have to force the update. Therefore, keep your cluster up to date to avoid operational impact.

**Can my worker nodes run a later version than the master?**</br>
No. First, [update your master](#update_master) to the latest Kubernetes version. Then, [update the worker nodes](#worker_node) in your cluster. Unlike the master, you also must update your workers for each patch version.

**What happens during the master update?**</br>
When you update the Kubernetes API server, the API server is down for about 5 - 10 minutes. During the update, you cannot access or change the cluster. However, worker nodes, apps, and resources that cluster users have deployed are not modified and continue to run.

**Can I roll back the update?**</br>
No, you cannot roll back a cluster to a previous version after the update process takes place. Be sure to use a test cluster and follow the instructions to address potential issues before updating your production master.

**What process can I follow to update the master?**</br>
The following diagram shows the process that you can take to update your master.

![Master update best practice](/images/update-tree.png)

Figure 1. Updating Kubernetes master process diagram

{: #update_master}
To update the Kubernetes master _major_ or _minor_ version:

1.  Review the [Kubernetes changes](cs_versions.html) and make any updates marked _Update before master_.

2.  Update your Kubernetes API server and associated Kubernetes master components by using the GUI or running the CLI `ibmcloud ks cluster-update` [command](cs_cli_reference.html#cs_cluster_update).

3.  Wait a few minutes, then confirm that the update is complete. Review the Kubernetes API server version on the {{site.data.keyword.Bluemix_notm}} Dashboard or run `ibmcloud ks clusters`.

4.  Install the version of the [`kubectl cli`](cs_cli_install.html#kubectl) that matches the Kubernetes API server version that runs in the Kubernetes master.

When the Kubernetes API server update is complete, you can update your worker nodes.

<br />


## Updating worker nodes
{: #worker_node}

You received a notification to update your worker nodes. What does that mean? As security updates and patches are put in place for the Kubernetes API server and other Kubernetes master components, you must be sure that the worker nodes remain in sync.
{: shortdesc}

Before you begin:
- [Target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster.
- [Update the Kubernetes master](#master). The worker node Kubernetes version cannot be higher than the Kubernetes API server version that runs in your Kubernetes master.
- Make any changes that are marked with _Update after master_ in the [Kubernetes changes](cs_versions.html).
- If you want to apply a patch update, review the [Kubernetes version changelog](cs_versions_changelog.html#changelog). </br>

**Attention**: Updates to worker nodes can cause downtime for your apps and services. Data is deleted if not [stored outside the pod](cs_storage_planning.html#persistent).


**What happens to my apps during an update?**</br>
If you run apps as part of a deployment on worker nodes that you update, the apps are rescheduled onto other worker nodes in the cluster. These worker nodes might be in a different worker pool, or if you have stand-alone worker nodes, apps might be scheduled onto stand-alone worker nodes. To avoid downtime for your app, you must ensure that you have enough capacity in the cluster to carry the workload.

**How can I control how many worker nodes go down at a given time during the update?**
If you need all your worker nodes to be up and running, consider [resizing your worker pool](cs_cli_reference.html#cs_worker_pool_resize) or [adding stand-alone worker nodes](cs_cli_reference.html#cs_worker_add) to add more worker nodes. You can remove the additional worker nodes after the update is completed.

In addition, you can create a Kubernetes config map that specifies the maximum number of worker nodes that can be unavailable at a time during the update. Worker nodes are identified by the worker node labels. You can use IBM-provided labels or custom labels that you added to the worker node.

**What if I choose not to define a config map?**</br>
When the config map is not defined, the default is used. By default, a maximum of 20% of all of your worker nodes in each cluster can be unavailable during the update process.

To create a config map and update worker nodes:

1.  List available worker nodes and note their private IP address.

    ```
    ibmcloud ks workers <cluster_name_or_ID>
    ```
    {: pre}

2. View the labels of a worker node. You can find the worker node labels in the **Labels** section of your CLI output. Every label consists of a `NodeSelectorKey` and a `NodeSelectorValue`.
   ```
   kubectl describe node <private_worker_IP>
   ```
   {: pre}

   Example output:
   ```
   Name:               10.184.58.3
   Roles:              <none>
   Labels:             arch=amd64
                    beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    failure-domain.beta.kubernetes.io/region=us-south
                    failure-domain.beta.kubernetes.io/zone=dal12
                    ibm-cloud.kubernetes.io/encrypted-docker-data=true
                    ibm-cloud.kubernetes.io/iaas-provider=softlayer
                    ibm-cloud.kubernetes.io/machine-type=u2c.2x4.encrypted
                    kubernetes.io/hostname=10.123.45.3
                    privateVLAN=2299001
                    publicVLAN=2299012
   Annotations:        node.alpha.kubernetes.io/ttl=0
                    volumes.kubernetes.io/controller-managed-attach-detach=true
   CreationTimestamp:  Tue, 03 Apr 2018 15:26:17 -0400
   Taints:             <none>
   Unschedulable:      false
   ```
   {: screen}

3. Create a config map and define the unavailability rules for your worker nodes. The following example shows 4 checks, the `zonecheck.json`, `regioncheck.json`, `defaultcheck.json`, and a check template. You can use these example checks to define rules for worker nodes in a specific zone (`zonecheck.json`), region (`regioncheck.json`), or for all worker nodes that do not match any of the checks that you defined in the config map (`defaultcheck.json`). Use the check template to create your own check. For every check, to identify a worker node, you must choose one of the worker node labels that you retrieved in the previous step.  

   **Note:** For every check, you can set only one value for <code>NodeSelectorKey</code> and <code>NodeSelectorValue</code>. If you want to set rules for more than one region, zone, or other worker node labels, create a new check. Define up to 10 checks in a config map. If you add more checks, they are ignored.

   Example:
   ```
   apiVersion: v1
   kind: ConfigMap
   metadata:
     name: ibm-cluster-update-configuration
     namespace: kube-system
   data:
    drain_timeout_seconds: "120"
    zonecheck.json: |
      {
        "MaxUnavailablePercentage": 30,
        "NodeSelectorKey": "failure-domain.beta.kubernetes.io/zone",
        "NodeSelectorValue": "dal13"
      }
    regioncheck.json: |
      {
        "MaxUnavailablePercentage": 20,
        "NodeSelectorKey": "failure-domain.beta.kubernetes.io/region",
        "NodeSelectorValue": "us-south"
      }
    defaultcheck.json: |
      {
        "MaxUnavailablePercentage": 20
      }
    <check_name>: |
      {
        "MaxUnavailablePercentage": <value_in_percentage>,
        "NodeSelectorKey": "<node_selector_key>",
        "NodeSelectorValue": "<node_selector_value>"
      }
   ```
   {:pre}

   <table summary="The first row in the table spans both columns. The rest of the rows should be read left to right, with the parameter in column one and the description that matches in column two.">
   <caption>ConfigMap components</caption>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the components </th>
    </thead>
    <tbody>
      <tr>
        <td><code>drain_timeout_seconds</code></td>
        <td> Optional: The timeout in seconds to wait for the [drain ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/) to complete. Draining a worker node safely removes all existing pods from the worker node and reschedules the pods onto other worker nodes in the cluster. Accepted values are integers from 1 to 180. The default value is 30.</td>
      </tr>
      <tr>
        <td><code>zonecheck.json</code></br><code>regioncheck.json</code></td>
        <td>Two checks that define a rule for a set of worker nodes that you can identify with the specified <code>NodeSelectorKey</code> and <code>NodeSelectorValue</code>. The <code>zonecheck.json</code> identifies worker nodes based on their zone label, and the <code>regioncheck.json</code> uses the region label that is added to every worker node during provisioning. In the example, 30% of all worker nodes that have <code>dal13</code> as their zone label and 20% of all the worker nodes in <code>us-south</code> can be unavailable during the update.</td>
      </tr>
      <tr>
        <td><code>defaultcheck.json</code></td>
        <td>If you do not create a config map or the map is configured incorrectly, the Kubernetes default is applied. By default, only 20% of the worker nodes in the cluster can be unavailable at a given time. You can override the default value by adding the default check to your config map. In the example, every worker node that is not specified in the zone and region checks (<code>dal13</code> or <code>us-south</code>) can be unavailable during the update. </td>
      </tr>
      <tr>
        <td><code>MaxUnavailablePercentage</code></td>
        <td>The maximum amount of nodes that are allowed to be unavailable for a specified label key and value, specified as a percentage. A worker node is unavailable when it is in the process of deploying, reloading, or provisioning. The queued worker nodes are blocked from updating if it exceeds any defined maximum unavailable percentages. </td>
      </tr>
      <tr>
        <td><code>NodeSelectorKey</code></td>
        <td>The label key of the worker node for which you want to set a rule. You can set rules for the default labels provided by IBM, as well as on worker node labels that you created. <ul><li>If you want to add a rule for worker nodes that belong to one worker pool, you can use the <code>ibm-cloud.kubernetes.io/machine-type</code> label. </li><li> If you have more than one worker pool with the same machine type, use a custom label. </li></ul></td>
      </tr>
      <tr>
        <td><code>NodeSelectorValue</code></td>
        <td>The label value that the worker node must have to be considered for the rule that you define. </td>
      </tr>
    </tbody>
   </table>

4. Create the configuration map in your cluster.
   ```
   kubectl apply -f <filepath/configmap.yaml>
   ```
   {: pre}

5. Verify that the config map is created.
   ```
   kubectl get configmap --namespace kube-system
   ```
   {: pre}

6.  Update the worker nodes.

    ```
    ibmcloud ks worker-update <cluster_name_or_ID> <worker_node1_ID> <worker_node2_ID>
    ```
    {: pre}

7. Optional: Verify the events that are triggered by the config map and any validation errors that occur. The events can be reviewed in the  **Events** section of your CLI output.
   ```
   kubectl describe -n kube-system cm ibm-cluster-update-configuration
   ```
   {: pre}

8. Confirm that the update is complete by reviewing the Kubernetes version of your worker nodes.  
   ```
   kubectl get nodes
   ```
   {: pre}

9. Verify that you do not have duplicate worker nodes. In some cases, older clusters might list duplicate worker nodes with a **NotReady** status after an update. To remove duplicates, see [troubleshooting](cs_troubleshoot_clusters.html#cs_duplicate_nodes).

Next steps:
  - Repeat the update process with other worker pools.
  - Inform developers who work in the cluster to update their `kubectl` CLI to the version of the Kubernetes master.
  - If the Kubernetes dashboard does not display utilization graphs, [delete the `kube-dashboard` pod](cs_troubleshoot_health.html#cs_dashboard_graphs).

<br />



## Updating machine types
{: #machine_type}

You can update the machine types of your worker nodes by adding new worker nodes and removing the old ones. For example, if you have virtual worker nodes on deprecated machine types with `u1c` or `b1c` in the names, create worker nodes that use machine types with `u2c` or `b2c` in the names.
{: shortdesc}

Before you begin:
- [Target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster.
- If you store data on your worker node, the data is deleted if not [stored outside the worker node](cs_storage_planning.html#persistent).


**Attention**: Updates to worker nodes can cause downtime for your apps and services. Data is deleted if not [stored outside the pod](cs_storage_planning.html#persistent).

1. List available worker nodes and note their private IP address.
   - **For worker nodes in a worker pool**:
     1. List available worker pools in your cluster.
        ```
        ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
        ```
        {: pre}

     2. List the worker nodes in the worker pool.
        ```
        ibmcloud ks workers <cluster_name_or_ID> --worker-pool <pool_name>
        ```
        {: pre}

     3. Get the details for a worker node and note the zone, the private and the public VLAN ID.
        ```
        ibmcloud ks worker-get <cluster_name_or_ID> <worker_ID>
        ```
        {: pre}

   - **Deprecated: For stand-alone worker nodes**:
     1. List available worker nodes.
        ```
        ibmcloud ks workers <cluster_name_or_ID>
        ```
        {: pre}

     2. Get the details for a worker node and note the zone, the private and the public VLAN ID.
        ```
        ibmcloud ks worker-get <cluster_name_or_ID> <worker_ID>
        ```
        {: pre}

2. List available machine types in the zone.
   ```
   ibmcloud ks machine-types <zone>
   ```
   {: pre}

3. Create a worker node with the new machine type.
   - **For worker nodes in a worker pool**:
     1. Create a worker pool with the number of worker nodes that you want to replace.
        ```
        ibmcloud ks worker-pool-create --name <pool_name> --cluster <cluster_name_or_ID> --machine-type <machine_type> --size-per-zone <number_of_workers_per_zone>
        ```
        {: pre}

     2. Verify that the worker pool is created.
        ```
        ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
        ```
        {: pre}

     3. Add the zone to your worker pool that you retrieved earlier. When you add a zone, the worker nodes that are defined in your worker pool are provisioned in the zone and considered for future workload scheduling. If you want to spread your worker nodes across multiple zones, choose a [multizone-capable zone](cs_regions.html#zones).
        ```
        ibmcloud ks zone-add --zone <zone> --cluster <cluster_name_or_ID> --worker-pools <pool_name> --private-vlan <private_VLAN_ID> --public-vlan <public_VLAN_ID>
        ```
        {: pre}

   - **Deprecated: For stand-alone worker nodes**:
       ```
       ibmcloud ks worker-add --cluster <cluster_name> --machine-type <machine_type> --number <number_of_worker_nodes> --private-vlan <private_VLAN_ID> --public-vlan <public_VLAN_ID>
       ```
       {: pre}

4. Wait for the worker nodes to be deployed.
   ```
   ibmcloud ks workers <cluster_name_or_ID>
   ```
   {: pre}

   When the worker node state changes to **Normal**, the deployment is finished.

5. Remove the old worker node. **Note**: If you are removing a machine type that is billed monthly (such as bare metal), you are charged for the entire the month.
   - **For worker nodes in a worker pool**:
     1. Remove the worker pool with the old machine type. Removing a worker pool removes all worker nodes in the pool in all zones. This process might take a few minutes to complete.
        ```
        ibmcloud ks worker-pool-rm --worker-pool <pool_name> --cluster <cluster_name_or_ID>
        ```
        {: pre}

     2. Verify that the worker pool is removed.
        ```
        ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
        ```
        {: pre}

   - **Deprecated: For stand-alone worker nodes**:
      ```
      ibmcloud ks worker-rm <cluster_name> <worker_node>
      ```
      {: pre}

6. Verify that the worker nodes are removed from your cluster.
   ```
   ibmcloud ks workers <cluster_name_or_ID>
   ```
   {: pre}

7. Repeat these steps to update other worker pools or stand-alone worker nodes to different machine types.

## Updating cluster add-ons
{: #addons}

Your {{site.data.keyword.containershort_notm}} cluster comes with **add-ons**, such as Fluentd for logging, that are installed automatically when you provision the cluster. These add-ons must be updated separately from the master and worker nodes.
{: shortdesc}

**What default add-ons do I have to update separately from the cluster?**
* [Fluentd for logging](#logging)

**Can I install other add-ons than the default?**</br>
Yes. {{site.data.keyword.containershort_notm}} provides other add-ons that you can choose from to add capabilities to your cluster. For example, you might want to [use Helm charts](cs_integrations.html#helm) to install the [block storage plug-in](cs_storage_block.html#install_block), [Istio](cs_tutorials_istio.html#istio_tutorial), or [strongSwan VPN](cs_vpn.html#vpn-setup). You must update each add-on separately by following the instructions to update the Helm charts.

### Fluentd for logging
{: #logging}

In order to make changes to your logging or filter configurations, the Fluentd add-on must be at the latest version. By default, automatic updates to the add-on are enabled.
{: shortdesc}

You can check whether automatic updates are enabled by running the `ibmcloud ks logging-autoupdate-get --cluster <cluster_name_or_ID>` [command](cs_cli_reference.html#cs_log_autoupdate_get).

To disable automatic updates, run the `ibmcloud ks logging-autoupdate-disable` [command](cs_cli_reference.html#cs_log_autoupdate_disable).

If automatic updates are disabled but you need to make a change to your configuration, you have two options.

*  Turn on automatic updates for your Fluentd pods.

    ```
    ibmcloud ks logging-autoupdate-enable --cluster <cluster_name_or_ID>
    ```
    {: pre}

*  Force a one-time update when you use a logging command that includes the `--force-update` option. **Note**: Your pods update to the latest version of the Fluentd add-on, but Fluentd does not update automatically going forward.

    Example command:

    ```
    ibmcloud ks logging-config-update --cluster <cluster_name_or_ID> --id <log_config_ID> --type <log_type> --force-update
    ```
    {: pre}

## Updating from stand-alone worker nodes to worker pools
{: #standalone_to_workerpool}

With the introduction of multizone clusters, worker nodes with the same configuration, such as the machine type, are grouped in worker pools. When you create a new cluster, a worker pool that is named `default` is automatically created for you.
{: shortdesc}

You can use worker pools to spread worker nodes evenly across zones and build a balanced cluster. Balanced clusters are more available and resilient to failures. If a worker node is removed from a zone, you can rebalance the worker pool and automatically provision new worker nodes to that zone. Worker pools are also used to install Kubernetes version updates to all of your worker nodes.  

**Important:** If you created clusters before multizone clusters became available, your worker nodes are still stand-alone and not automatically grouped into worker pools. You must update these clusters to use worker pools. If not updated, you cannot change your single zone cluster to a multizone cluster.

Review the following image to see how your cluster setup changes when you move from stand-alone worker nodes to worker pools.

<img src="images/cs_cluster_migrate.png" alt="Update your cluster from stand-alone worker nodes to worker pools" width="600" style="width:600px; border-style: none"/>

Before you begin, [target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster.

1. List existing stand-alone worker nodes in your cluster and note the **ID**, the **Machine Type**, and **Private IP**.
   ```
   ibmcloud ks workers <cluster_name_or_ID>
   ```
   {: pre}

2. Create a worker pool and decide on the machine type and the number of worker nodes that you want to add to the pool.
   ```
   ibmcloud ks worker-pool-create --name <pool_name> --cluster <cluster_name_or_ID> --machine-type <machine_type> --size-per-zone <number_of_workers_per_zone>
   ```
   {: pre}

3. List available zones and decide where you want to provision the worker nodes in your worker pool. To view the zone where your stand-alone worker nodes are provisioned, run `ibmcloud ks cluster-get <cluster_name_or_ID>`. If you want to spread your worker nodes across multiple zones, choose a [multizone-capable zone](cs_regions.html#zones).
   ```
   ibmcloud ks zones
   ```
   {: pre}

4. List available VLANs for the zone that you chose in the previous step. If you do not have a VLAN in that zone yet, the VLAN is automatically created for you when you add the zone to the worker pool.
   ```
   ibmcloud ks vlans <zone>
   ```
   {: pre}

5. Add the zone to your worker pool. When you add a zone to a worker pool, the worker nodes that are defined in your worker pool are provisioned in the zone and considered for future workload scheduling. {{site.data.keyword.containerlong}} automatically adds the `failure-domain.beta.kubernetes.io/region` label for the region and the `failure-domain.beta.kubernetes.io/zone` label for the zone to each worker node. The Kubernetes scheduler uses these labels to spread pods across zones within the same region.
   1. **To add a zone to one worker pool**: Replace `<pool_name>` with the name of your worker pool, and fill in the cluster ID, zone, and VLANs with the information you previously retrieved. If you do not have a private and a public VLAN in that zone, do not specify this option. A private and a public VLAN are automatically created for you.

      If you want to use different VLANs for different worker pools, repeat this command for each VLAN and its corresponding worker pools. Any new worker nodes are added to the VLANs that you specify, but the VLANs for any existing worker nodes are not changed.
      ```
      ibmcloud ks zone-add --zone <zone> --cluster <cluster_name_or_ID> --worker-pools <pool_name> --private-vlan <private_VLAN_ID> --public-vlan <public_VLAN_ID>
      ```
      {: pre}

   2. **To add the zone to multiple worker pools**: Add multiple worker pools to the `ibmcloud ks zone-add` command. To add multiple worker pools to a zone, you must have an existing private and public VLAN in that zone. If you do not have a public and private VLAN in that zone, consider adding the zone to one worker pool first so that a public and a private VLAN are created for you. Then, you can add the zone to other worker pools. </br></br>It is important that the worker nodes in all your worker pools are provisioned into all the zones to ensure that your cluster is balanced across zones. If you want to use different VLANs for different worker pools, repeat this command with the VLAN that you want to use for your worker pool. To enable communication on the private network between workers that are in different zones, you must enable [VLAN spanning](/docs/infrastructure/vlans/vlan-spanning.html#vlan-spanning).
      ```
      ibmcloud ks zone-add --zone <zone> --cluster <cluster_name_or_ID> --worker-pools <pool_name1,pool_name2,pool_name3> --private-vlan <private_VLAN_ID> --public-vlan <public_VLAN_ID>
      ```
      {: pre}

   3. **To add multiple zones to your worker pools**: Repeat the `ibmcloud ks zone-add` command with a different zone and specify the worker pools that you want to provision in that zone. By adding more zones to your cluster, you change your cluster from a single zone cluster to a [multizone cluster](cs_clusters.html#multi_zone).

6. Wait for the worker nodes to be deployed in each zone.
   ```
   ibmcloud ks workers <cluster_name_or_ID>
   ```
   {: pre}
   When the worker node state changes to **Normal** the deployment is finished.

7. Remove your stand-alone worker nodes. If you have multiple stand-alone worker nodes, remove one at a time.
   1. List the worker nodes in your cluster and compare the private IP address from this command with the private IP address that you retrieved in the beginning to find your stand-alone worker nodes.
      ```
      kubectl get nodes
      ```
      {: pre}
   2. Mark the worker node as unschedulable in a process that is known as cordoning. When you cordon a worker node, you make it unavailable for future pod scheduling. Use the `name` that is returned in the `kubectl get nodes` command.
      ```
      kubectl cordon <worker_name>
      ```
      {: pre}
   3. Verify that pod scheduling is disabled for your worker node.
      ```
      kubectl get nodes
      ```
      {: pre}
      Your worker node is disabled for pod scheduling if the status displays **SchedulingDisabled**.
   4. Force pods to be removed from your stand-alone worker node and rescheduled onto remaining uncordoned stand-alone worker nodes and worker nodes from your worker pool.
      ```
      kubectl drain <worker_name> --ignore-daemonsets
      ```
      {: pre}
      This process can take a few minutes.

   5. Remove your stand-alone worker node. Use the ID of the worker node that you retrieved with the `ibmcloud ks workers <cluster_name_or_ID>` command.
      ```
      ibmcloud ks worker-rm <cluster_name_or_ID> <worker_ID>
      ```
      {: pre}
   6. Repeat these steps until all your stand-alone worker nodes are removed.


**What's next?** </br>
Now that you updated your cluster to use worker pools, you can, improve availability by adding more zones to your cluster. Adding more zones to your cluster changes your cluster from a single zone cluster to a [multizone cluster](cs_clusters.html#ha_clusters). When you change your single zone cluster to a multizone cluster, your Ingress domain changes from `<cluster_name>.<region>.containers.mybluemix.net` to `<cluster_name>.<region_or_zone>.containers.appdomain.cloud`. The existing Ingress domain is still valid and can be used to send requests to your apps.
