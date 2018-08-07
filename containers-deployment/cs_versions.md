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



# Version information and update actions
{: #cs_versions}

## Kubernetes version types
{: #version_types}

{{site.data.keyword.containerlong}} concurrently supports multiple versions of Kubernetes. When a latest version (n) is released, versions up to 2 behind (n-2) are supported. Versions more than 2 behind the latest (n-3) are first deprecated and then unsupported.
{:shortdesc}

**Supported Kubernetes versions**:

- Latest: 1.10.5
- Default: 1.10.5
- Other: 1.9.9, 1.8.15

</br>

**Deprecated versions**: When clusters are running on a deprecated Kubernetes version, you have 30 days to review and update to a supported Kubernetes version before the version becomes unsupported. During the deprecation period, your cluster is still fully supported. However, you cannot create new clusters that use the deprecated version.

**Unsupported versions**: If you are running clusters on a Kubernetes version that is not supported, [review potential impacts](#version_types) for updates and then immediately [update the cluster](cs_cluster_update.html#update) to continue receiving important security updates and support. 
*  **Attention**: If you wait until your cluster is three or more minor versions behind a supported version, you must force the update, which might cause unexpected results or failure. 
*  Unsupported clusters cannot add or reload existing worker nodes. 
*  After you update the cluster to a supported version, your cluster can resume normal operations and continue receiving support.

</br>

To check the server version of a cluster, run the following command.

```
kubectl version  --short | grep -i server
```
{: pre}

Example output:

```
Server Version: v1.10.5+IKS
```
{: screen}


## Update types
{: #update_types}

Your Kubernetes cluster has three types of updates: major, minor, and patch.
{:shortdesc}

|Update type|Examples of version labels|Updated by|Impact
|-----|-----|-----|-----|
|Major|1.x.x|You|Operation changes for clusters, including scripts or deployments.|
|Minor|x.9.x|You|Operation changes for clusters, including scripts or deployments.|
|Patch|x.x.4_1510|IBM and you|Kubernetes patches, as well as other {{site.data.keyword.Bluemix_notm}} Provider component updates such as security and operating system patches. IBM updates masters automatically, but you apply patches to worker nodes.|
{: caption="Impacts of Kubernetes updates" caption-side="top"}

As updates become available, you are notified when you view information about the worker nodes, such as with the `ibmcloud ks workers <cluster>` or `ibmcloud ks worker-get <cluster> <worker>` commands.
-  **Major and minor updates**: First, [update your master node](cs_cluster_update.html#master) and then [update the worker nodes](cs_cluster_update.html#worker_node).
   - By default, you cannot update a Kubernetes master three or more minor versions ahead. For example, if your current master is version 1.5 and you want to update to 1.8, you must update to 1.7 first. You can force the update to continue, but updating more than two minor versions might cause unexpected results or failure.
   - If you use a `kubectl` CLI version that does match at least the `major.minor` version of your clusters, you might experience unexpected results. Make sure to keep your Kubernetes cluster and [CLI versions](cs_cli_install.html#kubectl) up-to-date.
-  **Patch updates**: Check monthly to see whether an update is available, and use the `ibmcloud ks worker-update` [command](cs_cli_reference.html#cs_worker_update) or the `ibmcloud ks worker-reload` [command](cs_cli_reference.html#cs_worker_reload) to apply these security and operating system patches. For more information, see [Version changelog](cs_versions_changelog.html).

<br/>

This information summarizes updates that are likely to have impact on deployed apps when you update a cluster to a new version from the previous version.
-  Version 1.10 [migration actions](#cs_v110).
-  Version 1.9 [migration actions](#cs_v19).
-  Version 1.8 [migration actions](#cs_v18).
-  [Archive](#k8s_version_archive) of deprecated or unsupported versions.

<br/>

For a complete list of changes, review the following information:
* [Kubernetes changelog ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md).
* [IBM version changelog](cs_versions_changelog.html).

## Version 1.10
{: #cs_v110}

<p><img src="images/certified_kubernetes_1x10.png" style="padding-right: 10px;" align="left" alt="This badge indicates Kubernetes version 1.10 certification for IBM Cloud Container Service."/> {{site.data.keyword.containerlong_notm}} is a Certified Kubernetes product for version 1.10 under the CNCF Kubernetes Software Conformance Certification program. _Kubernetes® is a registered trademark of The Linux Foundation in the United States and other countries, and is used pursuant to a license from The Linux Foundation._</p>

Review changes that you might need to make when you are updating from the previous Kubernetes version to 1.10.

**Important**: Before you can successfully update to Kubernetes 1.10, you must follow the steps listed in [Preparing to update to Calico v3](#110_calicov3).

<br/>

### Update before master
{: #110_before}

<table summary="Kubernetes updates for version 1.10">
<caption>Changes to make before you update the master to Kubernetes 1.10</caption>
<thead>
<tr>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico v3</td>
<td>Updating to Kubernetes version 1.10 also updates Calico from v2.6.5 to v3.1.1. <strong>Important</strong>: Before you can successfully update to Kubernetes v1.10, you must follow the steps listed in [Preparing to update to Calico v3](#110_calicov3).</td>
</tr>
<tr>
<td>Kubernetes Dashboard network policy</td>
<td>In Kubernetes 1.10, the <code>kubernetes-dashboard</code> network policy in the <code>kube-system</code> namespace blocks all pods from accessing the Kubernetes dashboard. However, this does <strong>not</strong> impact the ability to access the dashboard from the {{site.data.keyword.Bluemix_notm}} console or by using <code>kubectl proxy</code>. If a pod requires access to the dashboard, you can add a <code>kubernetes-dashboard-policy: allow</code> label to a namespace and then deploy the pod to the namespace.</td>
</tr>
<tr>
<td>Kubelet API access</td>
<td>Kubelet API authorization is now delegated to the <code>Kubernetes API server</code>. Access to the Kubelet API is based on <code>ClusterRoles</code> that grant permission to access <strong>node</strong> subresources. By default, Kubernetes Heapster has <code>ClusterRole</code> and <code>ClusterRoleBinding</code>. However, if the Kubelet API is used by other users or apps, you must grant them permission to use the API. Refer to the Kubernetes documentation on [Kubelet authorization![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet-authentication-authorization/).</td>
</tr>
<tr>
<td>Cipher suites</td>
<td>The supported cipher suites to the <code>Kubernetes API server</code> and Kubelet API are now restricted to a subset with high strength encryption (128 bits or more). If you have existing automation or resources that use weaker ciphers and rely on communicating with the <code>Kubernetes API server</code> or Kubelet API, enable stronger cipher support before you update the master.</td>
</tr>
<tr>
<td>strongSwan VPN</td>
<td>If you use [strongSwan](cs_vpn.html#vpn-setup) for VPN connectivity, you must remove the chart before you update the cluster by running `helm delete --purge <release_name>`. After the cluster update is complete, reinstall the strongSwan Helm chart.</td>
</tr>
</tbody>
</table>

### Update after master
{: #110_after}

<table summary="Kubernetes updates for version 1.10">
<caption>Changes to make after you update the master to Kubernetes 1.10</caption>
<thead>
<tr>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Calico v3</td>
<td>When the cluster is updated, all existing Calico data that is applied to the cluster is automatically migrated to use Calico v3 syntax. To view, add, or modify Calico resources with Calico v3 syntax, update your [Calico CLI configuration to version 3.1.1](#110_calicov3).</td>
</tr>
<tr>
<td>Node <code>ExternalIP</code> address</td>
<td>The <code>ExternalIP</code> field of a node is now set to the public IP address value of the node. Review and update any resources that depend on this value.</td>
</tr>
<tr>
<td><code>kubectl port-forward</code></td>
<td>Now when you use the <code>kubectl port-forward</code> command, it no longer supports the <code>-p</code> flag. If your scripts rely on the previous behavior, update them to replace the <code>-p</code> flag with the pod name.</td>
</tr>
<tr>
<td>Read-only API data volumes</td>
<td>Now `secret`, `configMap`, `downwardAPI`, and projected volumes are mounted read-only.
Previously, apps were allowed to write data to these volumes that might be
reverted automatically by the system. This migration action is required to fix
security vulnerability [CVE-2017-1002102![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=2017-1002102).
If your apps rely on the previous insecure behavior, modify them accordingly.</td>
</tr>
<tr>
<td>strongSwan VPN</td>
<td>If you use [strongSwan](cs_vpn.html#vpn-setup) for VPN connectivity and deleted your chart before updating your cluster, you can now re-install your strongSwan Helm chart.</td>
</tr>
</tbody>
</table>

### Preparing to update to Calico v3
{: #110_calicov3}

Before you begin, your cluster master and all worker nodes must be running Kubernetes version 1.8 or later, and must have at least one worker node.

**Important**: Prepare for the Calico v3 update before you update the master. During the master upgrade to Kubernetes v1.10, new pods and new Kubernetes or Calico network policies are not scheduled. The amount of time that the update prevents new scheduling varies. Small clusters can take a few minutes, with a few extra minutes for every 10 nodes. Existing network policies and pods continue to run.

1.  Verify that your Calico pods are healthy.
    ```
    kubectl get pods -n kube-system -l k8s-app=calico-node -o wide
    ```
    {: pre}

2.  If any pod is not in a **Running** state, delete the pod and wait until it is in a **Running** state before you continue.

3.  If you auto-generate Calico policies or other Calico resources, update your automation tooling to generate these resources with [Calico v3 syntax ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/).

4.  If you use [strongSwan](cs_vpn.html#vpn-setup) for VPN connectivity, the strongSwan 2.0.0 Helm chart does not work with Calico v3 or Kubernetes 1.10. [Update strongSwan](cs_vpn.html#vpn_upgrade) to the 2.1.0 Helm chart, which is backward compatible with Calico 2.6, and Kubernetes 1.7, 1.8, and 1.9.

5.  [Update your cluster master to Kubernetes v1.10](cs_cluster_update.html#master).

<br />


## Version 1.9
{: #cs_v19}

<p><img src="images/certified_kubernetes_1x9.png" style="padding-right: 10px;" align="left" alt="This badge indicates Kubernetes version 1.9 certification for IBM Cloud Container Service."/> {{site.data.keyword.containerlong_notm}} is a Certified Kubernetes product for version 1.9 under the CNCF Kubernetes Software Conformance Certification program. _Kubernetes® is a registered trademark of The Linux Foundation in the United States and other countries, and is used pursuant to a license from The Linux Foundation._</p>

Review changes that you might need to make when you are updating from the previous Kubernetes version to 1.9.

<br/>

### Update before master
{: #19_before}

<table summary="Kubernetes updates for version 1.9">
<caption>Changes to make before you update the master to Kubernetes 1.9</caption>
<thead>
<tr>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Webhook admission API</td>
<td>The admission API, which is used when the API server calls admission control webhooks, is moved from <code>admission.v1alpha1</code> to <code>admission.v1beta1</code>. <em>You must delete any existing webhooks before you upgrade your cluster</em>, and update the webhook configuration files to use the latest API. This change is not backward compatible.</td>
</tr>
</tbody>
</table>

### Update after master
{: #19_after}

<table summary="Kubernetes updates for version 1.9">
<caption>Changes to make after you update the master to Kubernetes 1.9</caption>
<thead>
<tr>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>`kubectl` output</td>
<td>Now, when you use the `kubectl` command to specify `-o custom-columns` and the column is not found in the object, you see an output of `<none>`.<br>
Previously, the operation failed and you saw the error message `xxx is not found`. If your scripts rely on the previous behavior, update them.</td>
</tr>
<tr>
<td>`kubectl patch`</td>
<td>Now, when no changes are made to the resource that is patched, the `kubectl patch` command fails with `exit code 1`. If your scripts rely on the previous behavior, update them.</td>
</tr>
<tr>
<td>Kubernetes dashboard permissions</td>
<td>Users are required to log in to the Kubernetes dashboard with their credentials to view cluster resources. The default Kubernetes dashboard `ClusterRoleBinding` RBAC authorization is removed. For instructions, see [Launching the Kubernetes dashboard](cs_app.html#cli_dashboard).</td>
</tr>
<tr>
<td>Read-only API data volumes</td>
<td>Now `secret`, `configMap`, `downwardAPI`, and projected volumes are mounted read-only.
Previously, apps were allowed to write data to these volumes that might be
reverted automatically by the system. This migration action is required to fix
security vulnerability [CVE-2017-1002102](https://cve.mitre.org/cgi-bin/cvename.cgi?name=2017-1002102).
If your apps rely on the previous insecure behavior, modify them accordingly.</td>
</tr>
<tr>
<td>Taints and tolerations</td>
<td>The `node.alpha.kubernetes.io/notReady` and `node.alpha.kubernetes.io/unreachable` taints were changed to `node.kubernetes.io/not-ready` and `node.kubernetes.io/unreachable` respectively.<br>
Although the taints are updated automatically, you must manually update the tolerations for these taints. For each namespace except `ibm-system` and `kube-system`, determine whether you need to change tolerations:<br>
<ul><li><code>kubectl get pods -n &lt;namespace&gt; -o yaml | grep "node.alpha.kubernetes.io/notReady" && echo "Action required"</code></li><li>
<code>kubectl get pods -n &lt;namespace&gt; -o yaml | grep "node.alpha.kubernetes.io/unreachable" && echo "Action required"</code></li></ul><br>
If `Action required` is returned, modify the pod tolerations accordingly.</td>
</tr>
<tr>
<td>Webhook admission API</td>
<td>If you deleted existing webhooks before you updated the cluster, create new webhooks.</td>
</tr>
</tbody>
</table>

<br />



## Version 1.8
{: #cs_v18}

<p><img src="images/certified_kubernetes_1x8.png" style="padding-right: 10px;" align="left" alt="This badge indicates Kubernetes version 1.8 certification for IBM Cloud Container Service."/> {{site.data.keyword.containerlong_notm}} is a Certified Kubernetes product for version 1.8 under the CNCF Kubernetes Software Conformance Certification program. _Kubernetes® is a registered trademark of The Linux Foundation in the United States and other countries, and is used pursuant to a license from The Linux Foundation._</p>

Review changes that you might need to make when you are updating from the previous Kubernetes version to 1.8.

<br/>

### Update before master
{: #18_before}

<table summary="Kubernetes updates for versions 1.8">
<caption>Changes to make before you update the master to Kubernetes 1.8</caption>
<thead>
<tr>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>None</td>
<td>No changes are required before you update the master</td>
</tr>
</tbody>
</table>

### Update after master
{: #18_after}

<table summary="Kubernetes updates for versions 1.8">
<caption>Changes to make after you update the master to Kubernetes 1.8</caption>
<thead>
<tr>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes dashboard login</td>
<td>The URL for accessing the Kubernetes dashboard in version 1.8 changed, and the login process includes a new authentication step. For more information, see [accessing the Kubernetes dashboard](cs_app.html#cli_dashboard).</td>
</tr>
<tr>
<td>Kubernetes dashboard permissions</td>
<td>To force users to log in with their credentials to view cluster resources in version 1.8, remove the 1.7 ClusterRoleBinding RBAC authorization. Run `kubectl delete clusterrolebinding -n kube-system kubernetes-dashboard`.</td>
</tr>
<tr>
<td>`kubectl delete`</td>
<td>The `kubectl delete` command no longer scales down workload API objects, like pods, before the object is deleted. If you require the object to scale down, use [`kubectl scale` ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/reference/kubectl/overview/#scale) before you delete the object.</td>
</tr>
<tr>
<td>`kubectl run`</td>
<td>The `kubectl run` command must use multiple flags for `--env` instead of comma-separated arguments. For example, run <code>kubectl run --env &lt;x&gt;=&lt;y&gt; --env &lt;z&gt;=&lt;a&gt;</code> and not <code>kubectl run --env &lt;x&gt;=&lt;y&gt;,&lt;z&gt;=&lt;a&gt;</code>. </td>
</tr>
<tr>
<td>`kubectl stop`</td>
<td>The `kubectl stop` command is no longer available.</td>
</tr>
<tr>
<td>Read-only API data volumes</td>
<td>Now `secret`, `configMap`, `downwardAPI`, and projected volumes are mounted read-only.
Previously, apps were allowed to write data to these volumes that might be
reverted automatically by the system. This migration action is required to fix
security vulnerability [CVE-2017-1002102](https://cve.mitre.org/cgi-bin/cvename.cgi?name=2017-1002102).
If your apps rely on the previous insecure behavior, modify them accordingly.</td>
</tr>
</tbody>
</table>

<br />



## Archive
{: #k8s_version_archive}

### Version 1.7 (Unsupported)
{: #cs_v17}

As of 21 June 2018, {{site.data.keyword.containershort_notm}} clusters that run [Kubernetes version 1.7](cs_versions_changelog.html#changelog_archive) are unsupported. Version 1.7 clusters cannot receive security updates or support unless they are updated to the next most recent version ([Kubernetes 1.8](#cs_v18)).

[Review potential impact](cs_versions.html#cs_versions) of each Kubernetes version update, and then [update your clusters](cs_cluster_update.html#update) immediately to at least 1.8.

### Version 1.5 (Unsupported)
{: #cs_v1-5}

As of 4 April 2018, {{site.data.keyword.containershort_notm}} clusters that run [Kubernetes version 1.5](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.5.md) are unsupported. Version 1.5 clusters cannot receive security updates or support unless they are updated to the next most recent version ([Kubernetes 1.8](#cs_v18)).

[Review potential impact](cs_versions.html#cs_versions) of each Kubernetes version update, and then [update your clusters](cs_cluster_update.html#update) immediately to at least 1.8.
