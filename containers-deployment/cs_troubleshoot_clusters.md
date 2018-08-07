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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}



# Troubleshooting clusters and worker nodes
{: #cs_troubleshoot_clusters}

As you use {{site.data.keyword.containerlong}}, consider these techniques for troubleshooting your clusters and worker nodes.
{: shortdesc}

If you have a more general issue, try out [cluster debugging](cs_troubleshoot.html).
{: tip}

## Unable to connect to your infrastructure account
{: #cs_credentials}

{: tsSymptoms}
When you create a new Kubernetes cluster, you receive an error message similar to one of the following.

```
We were unable to connect to your IBM Cloud infrastructure (SoftLayer) account.
Creating a standard cluster requires that you have either a 
Pay-As-You-Go account that is linked to an IBM Cloud infrastructure (SoftLayer) 
account term or that you have used the {{site.data.keyword.containerlong_notm}} 
CLI to set your {{site.data.keyword.Bluemix_notm}} Infrastructure API keys.
```
{: screen}

```
{{site.data.keyword.Bluemix_notm}} Infrastructure Exception: 
'Item' must be ordered with permission.
```
{: screen}

```
{{site.data.keyword.Bluemix_notm}} Infrastructure Exception: 
The user does not have the necessary {{site.data.keyword.Bluemix_notm}} 
Infrastructure permissions to add servers
```
{: screen}

{: tsCauses}
{{site.data.keyword.Bluemix_notm}} Pay-As-You-Go accounts that were created after automatic account linking was enabled are already set up with access to the IBM Cloud infrastructure (SoftLayer) portfolio. You can purchase infrastructure resources for your cluster without additional configuration.

Users with other {{site.data.keyword.Bluemix_notm}} account types or users that have an existing IBM Cloud infrastructure (SoftLayer) account that is not linked to their {{site.data.keyword.Bluemix_notm}} account must configure their accounts to create standard clusters. 

If you have a valid Pay-As-You-Go account and receive this error message, you might not be using the correct IBM Cloud infrastructure (SoftLayer) account credentials to access infrastructure resources.

{: tsResolve}
The account owner must set up the infrastructure account credentials properly. The credentials depend on what type of infrastructure account you are using.
*  If you have a recent Pay-As-You-Go {{site.data.keyword.Bluemix_notm}} account, the account comes with a linked infrastructure account that you can use. [Verify that the infrastructure API key is set up with the correct permissions](#apikey).
*  If you have a different {{site.data.keyword.Bluemix_notm}} account type, verify that you can access the infrastructure portfolio and that [the infrastructure account credentials are set up with the correct permissions](#credentials).

To check if your cluster uses the linked infrastructure account or a different infrastructure account:
1.  Verify that you have access to an infrastructure account. Log in to the [{{site.data.keyword.Bluemix_notm}} console![External link icon](../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/) and from the expandable menu, click **Infrastructure**. If you see the infrastructure dashboard, you have access to an infrastructure account.
2.  Check if your cluster uses a different infrastructure account. From the expandable menu, click **Containers > Clusters**.
3.  From the table, select your cluster. 
4.  In the **Overview** tab, if you see an **Infrastructure User** field, the cluster uses a different infrastructure account than the one that came with your Pay-As-You-Go account.

### Configuring the infrastructure API credentials for linked accounts
{: #apikey}

1.  Verify that the user whose credentials you want to use for infrastructure actions has the correct permissions.

    1.  Log in to the [{{site.data.keyword.Bluemix_notm}} console![External link icon](../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/).
        
    2.  From the expanding menu, select **Infrastructure**.
        
    3.  From the menu bar, select **Account** > **Users** > **User List**.

    4.  In the **API Key** column, verify that the user has an API Key, or click **Generate**.

    5.  Verify or assign the user the [correct infrastructure permissions](cs_users.html#infra_access).

2.  Reset the API key for the region that the cluster is in so that it belongs to the user.
    
    1.  Log in to the terminal as the correct user.
    
    2.  Reset the API key to this user.
        ```
        ibmcloud ks api-key-reset
        ```
        {: pre}    
    
    3.  Verify that the API key is set.
        ```
        ibmcloud ks api-key-info <cluster_name_or_ID>
        ```
        {: pre}
        
    4.  **Optional**: If you previously set credentials manually with the `ibmcloud ks credentials-set` command, remove the associated infrastructure account. Now, the API key that you set in the previous substeps is used to order infrastructure.
        ```
        ibmcloud ks credentials-unset
        ```
        {: pre}

3.  **Optional**: If you connect your public cluster to on-premises resources, check your network connectivity.

    1.  Check your worker VLAN connectivity. 
    2.  If required, [set up VPN connectivity](cs_vpn.html#vpn).
    3.  [Open the required ports in your firewall](cs_firewall.html#firewall).

### Configuring the infrastructure account credentials for different accounts
{: #credentials}

1.  Get the infrastructure account that you want to use to access the IBM Cloud infrastructure (SoftLayer) portfolio. You have different options that depend on your current account type.

    <table summary="The table shows the standard cluster creation options by account type. Rows are to be read from the left to right, with the account description in column one, and the options to create a standard cluster in column two.">
    <caption>Standard cluster creation options by account type</caption>
      <thead>
      <th>Account description</th>
      <th>Options to create a standard cluster</th>
      </thead>
      <tbody>
        <tr>
          <td>**Lite accounts** cannot provision clusters.</td>
          <td>[Upgrade your Lite account to an {{site.data.keyword.Bluemix_notm}} Pay-As-You-Go account](/docs/account/index.html#paygo) that is set up with access to the IBM Cloud infrastructure (SoftLayer) portfolio.</td>
        </tr>
        <tr>
          <td>**Recent Pay-As-You-Go** accounts come with access to the infrastructure portfolio.</td>
          <td>You can create standard clusters. To troubleshoot infrastructure permissions, see [Configuring the infrastructure API credentials for linked accounts](#apikey).</td>
        </tr>
        <tr>
          <td>**Older Pay-As-You-Go accounts** that were created before automatic account linking was available did not come with access to the IBM Cloud infrastructure (SoftLayer) portfolio.<p>If you have an existing IBM Cloud infrastructure (SoftLayer) account, you cannot link this account to an older Pay-As-You-Go account.</p></td>
          <td><p><strong>Option 1:</strong> [Create a new Pay-As-You-Go account](/docs/account/index.html#paygo) that is set up with access to the IBM Cloud infrastructure (SoftLayer) portfolio. When you choose this option, you have two separate {{site.data.keyword.Bluemix_notm}} accounts and billings.</p><p>To continue using your old Pay-As-You-Go account, you can use your new Pay-As-You-Go account to generate an API key to access the IBM Cloud infrastructure (SoftLayer) portfolio.</p><p><strong>Option 2:</strong> If you already have an existing IBM Cloud infrastructure (SoftLayer) account that you want to use, you can set your credentials in your {{site.data.keyword.Bluemix_notm}} account.</p><p>**Note:** When you manually link to an IBM Cloud infrastructure (SoftLayer) account, the credentials are used for every IBM Cloud infrastructure (SoftLayer) specific action in your {{site.data.keyword.Bluemix_notm}} account. You must ensure that the API key that you set has [sufficient infrastructure permissions](cs_users.html#infra_access) so that your users can create and work with clusters.</p><p>**For both options, continue to the next step**.</p></td>
        </tr>
        <tr>
          <td>**Subscription accounts** are not set up with access to the IBM Cloud infrastructure (SoftLayer) portfolio.</td>
          <td><p><strong>Option 1:</strong> [Create a new Pay-As-You-Go account](/docs/account/index.html#paygo) that is set up with access to the IBM Cloud infrastructure (SoftLayer) portfolio. When you choose this option, you have two separate {{site.data.keyword.Bluemix_notm}} accounts and billings.</p><p>If you want to continue using your Subscription account, you can use your new Pay-As-You-Go account to generate an API key in IBM Cloud infrastructure (SoftLayer). Then, you must manually set the IBM Cloud infrastructure (SoftLayer) API key for your Subscription account. Keep in mind that IBM Cloud infrastructure (SoftLayer) resources are billed through your new Pay-As-You-Go account.</p><p><strong>Option 2:</strong> If you already have an existing IBM Cloud infrastructure (SoftLayer) account that you want to use, you can manually set IBM Cloud infrastructure (SoftLayer) credentials for your {{site.data.keyword.Bluemix_notm}} account.</p><p>**Note:** When you manually link to an IBM Cloud infrastructure (SoftLayer) account, the credentials are used for every IBM Cloud infrastructure (SoftLayer) specific action in your {{site.data.keyword.Bluemix_notm}} account. You must ensure that the API key that you set has [sufficient infrastructure permissions](cs_users.html#infra_access) so that your users can create and work with clusters.</p><p>**For both options, continue to the next step**.</p></td>
        </tr>
        <tr>
          <td>**IBM Cloud infrastructure (SoftLayer) accounts**, no {{site.data.keyword.Bluemix_notm}} account</td>
          <td><p>[Create an {{site.data.keyword.Bluemix_notm}} Pay-As-You-Go account](/docs/account/index.html#paygo) that is set up with access to the IBM Cloud infrastructure (SoftLayer) portfolio. When you choose this option, an IBM Cloud infrastructure (SoftLayer) account is created for you. You have two separate IBM Cloud infrastructure (SoftLayer) accounts and billing.</p><p>By default, your new {{site.keyword.data.Bluemix_notm}} account uses the new infrastructure account. To continue using the old infrastructure account, continue to the next step.</p></td>
        </tr>
      </tbody>
      </table>

2.  Verify that the user whose credentials you want to use for infrastructure actions has the correct permissions.

    1.  Log in to the [{{site.data.keyword.Bluemix_notm}} console![External link icon](../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/).
        
    2.  From the expanding menu, select **Infrastructure**.
        
    3.  From the menu bar, select **Account** > **Users** > **User List**.

    4.  In the **API Key** column, verify that the user has an API Key, or click **Generate**.

    5.  Verify or assign the user the [correct infrastructure permissions](cs_users.html#infra_access).

3.  Set the infrastructure API credentials with the user for the correct account.

    1.  Get the user's infrastructure API credentials. **Note**: The credentials differ from the IBMid.
            
        1.  From the [{{site.data.keyword.Bluemix_notm}} ![External link icon](../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/) console **Infrastructure** > **Account** > **Users** > **User List** table, click the **IBMid or Username**.
            
        2.  In the **API Access Information** section, view the **API Username** and **Authentication Key**.    
        
    2.  Set the infrastructure API credentials to use.
        ```
        ibmcloud ks credentials-set --infrastructure-username <infrastructure_API_username> --infrastructure-api-key <infrastructure_API_authentication_key>
  
4.  **Optional**: If you connect your public cluster to on-premises resources, check your network connectivity.

    1.  Check your worker VLAN connectivity. 
    2.  If required, [set up VPN connectivity](cs_vpn.html#vpn).
    3.  [Open the required ports in your firewall](cs_firewall.html#firewall).

<br />


## Firewall prevents running CLI commands
{: #ts_firewall_clis}

{: tsSymptoms}
When you run `ibmcloud`, `kubectl`, or `calicoctl` commands from the CLI, they fail.

{: tsCauses}
You might have corporate network policies that prevent access from your local system to public endpoints via proxies or firewalls.

{: tsResolve}
[Allow TCP access for the CLI commands to work](cs_firewall.html#firewall). This task requires an [Administrator access policy](cs_users.html#access_policies). Verify your current [access policy](cs_users.html#infra_access).


## Firewall prevents cluster from connecting to resources
{: #cs_firewall}

{: tsSymptoms}
When the worker nodes cannot connect, you might see various different symptoms. You might see one of the following messages when kubectl proxy fails or you try to access a service in your cluster and the connection fails.

  ```
  Connection refused
  ```
  {: screen}

  ```
  Connection timed out
  ```
  {: screen}

  ```
  Unable to connect to the server: net/http: TLS handshake timeout
  ```
  {: screen}

If you run kubectl exec, attach, or logs, you might see the following message.

  ```
  Error from server: error dialing backend: dial tcp XXX.XXX.XXX:10250: getsockopt: connection timed out
  ```
  {: screen}

If kubectl proxy succeeds, but the dashboard is not available, you might see the following message.

  ```
  timeout on 172.xxx.xxx.xxx
  ```
  {: screen}



{: tsCauses}
You might have another firewall set up or customized your existing firewall settings in your IBM Cloud infrastructure (SoftLayer) account. {{site.data.keyword.containershort_notm}} requires certain IP addresses and ports to be opened to allow communication from the worker node to the Kubernetes master and vice versa. Another reason might be that the worker nodes are stuck in a reloading loop.

{: tsResolve}
[Allow the cluster to access infrastructure resources and other services](cs_firewall.html#firewall_outbound). This task requires an [Administrator access policy](cs_users.html#access_policies). Verify your current [access policy](cs_users.html#infra_access).

<br />



## Accessing your worker node with SSH fails
{: #cs_ssh_worker}

{: tsSymptoms}
You cannot access your worker node by using an SSH connection.

{: tsCauses}
SSH by password is unavailable on the worker nodes.

{: tsResolve}
Use [DaemonSets ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) for actions that you must run on every node, or use jobs for one-time actions that you must run.

<br />


## Bare metal instance ID is inconsistent with worker records
{: #bm_machine_id}

{: tsSymptoms}
When you use `ibmcloud ks worker` commands with your bare metal worker node, you see a message similar to the following.

```
Instance ID inconsistent with worker records
```
{: screen}

{: tsCauses}
The machine ID can become inconsistent with the {{site.data.keyword.containershort_notm}} worker record when the machine experiences hardware issues. When IBM Cloud infrastructure (SoftLayer) resolves this issue, a component can change within the system that the service does not identify.

{: tsResolve}
For {{site.data.keyword.containershort_notm}} to re-identify the machine, [reload the bare metal worker node](cs_cli_reference.html#cs_worker_reload). **Note**: Reloading also updates the machine's [patch version](cs_versions_changelog.html).

You can also [delete the bare metal worker node](cs_cli_reference.html#cs_cluster_rm). **Note**: Bare metal instances are billed monthly.

<br />


## `kubectl exec` and `kubectl logs` do not work
{: #exec_logs_fail}

{: tsSymptoms}
If you run `kubectl exec` or `kubectl logs`, you see the following message.

  ```
  <workerIP>:10250: getsockopt: connection timed out
  ```
  {: screen}

{: tsCauses}
The OpenVPN connection between the master node and worker nodes is not functioning properly.

{: tsResolve}
1. Enable [VLAN spanning](/docs/infrastructure/vlans/vlan-spanning.html#vlan-spanning) for your IBM Cloud infrastructure (SoftLayer) account.
2. Restart the OpenVPN client pod.
  ```
  kubectl delete pod -n kube-system -l app=vpn
  ```
  {: pre}
3. If you still see the same error message, then the worker node that the VPN pod is on might be unhealthy. To restart the VPN pod and reschedule it to a different worker node, [cordon, drain, and reboot the worker node](cs_cli_reference.html#cs_worker_reboot) that the VPN pod is on.

<br />


## Binding a service to a cluster results in same name error
{: #cs_duplicate_services}

{: tsSymptoms}
When you run `ibmcloud ks cluster-service-bind <cluster_name> <namespace> <service_instance_name>`, you see the following message.

```
Multiple services with the same name were found.
Run 'ibmcloud service list' to view available Bluemix service instances...
```
{: screen}

{: tsCauses}
Multiple service instances might have the same name in different regions.

{: tsResolve}
Use the service GUID instead of the service instance name in the `ibmcloud ks cluster-service-bind` command.

1. [Log in to the region that includes the service instance to bind.](cs_regions.html#bluemix_regions)

2. Get the GUID for the service instance.
  ```
  ibmcloud service show <service_instance_name> --guid
  ```
  {: pre}

  Output:
  ```
  Invoking 'cf service <service_instance_name> --guid'...
  <service_instance_GUID>
  ```
  {: screen}
3. Bind the service to the cluster again.
  ```
  ibmcloud ks cluster-service-bind <cluster_name> <namespace> <service_instance_GUID>
  ```
  {: pre}

<br />


## Binding a service to a cluster results in service not found error
{: #cs_not_found_services}

{: tsSymptoms}
When you run `ibmcloud ks cluster-service-bind <cluster_name> <namespace> <service_instance_name>`, you see the following message.

```
Binding service to a namespace...
FAILED

The specified IBM Cloud service could not be found. If you just created the service, wait a little and then try to bind it again. To view available IBM Cloud service instances, run 'ibmcloud service list'. (E0023)
```
{: screen}

{: tsCauses}
To bind services to a cluster, you must have the Cloud Foundry developer user role for the space where the service instance is provisioned. In addition, you must have the IAM Editor access to {{site.data.keyword.containerlong}}. To access the service instance, you must be logged in to the space where the service instance is provisioned.

{: tsResolve}

**As the user:**

1. Log in to {{site.data.keyword.Bluemix_notm}}.
   ```
   ibmcloud login
   ```
   {: pre}

2. Target the org and the space where the service instance is provisioned.
   ```
   ibmcloud target -o <org> -s <space>
   ```
   {: pre}

3. Verify that you are in the right space by listing your service instances.
   ```
   ibmcloud service list
   ```
   {: pre}

4. Try binding the service again. If you get the same error, then contact the account administrator and verify that you have sufficient permissions to bind services (see the following account admin steps).

**As the account admin:**

1. Verify that the user who experiences this problem has [Editor permissions for {{site.data.keyword.containerlong}}](/docs/iam/mngiam.html#editing-existing-access).

2. Verify that the user who experiences this problem has the [Cloud Foundry developer role for the space](/docs/iam/mngcf.html#updating-cloud-foundry-access) where the service is provisioned.

3. If the correct permissions exists, try assigning a different permission and then re-assigning the required permission.

4. Wait a few minutes, then let the user try to bind the service again.

5. If this does not resolve the problem, then the IAM permissions are out of sync and you cannot resolve the issue yourself. [Contact IBM support](/docs/get-support/howtogetsupport.html#getting-customer-support) by opening a support ticket. Make sure to provide the cluster ID, the user ID, and the service instance ID.
   1. Retrieve the cluster ID.
      ```
      ibmcloud ks clusters
      ```
      {: pre}

   2. Retrieve the service instance ID.
      ```
      ibmcloud service show <service_name> --guid
      ```
      {: pre}


<br />



## After a worker node updates or reloads, duplicate nodes and pods appear
{: #cs_duplicate_nodes}

{: tsSymptoms}
When you run `kubectl get nodes`, you see duplicate worker nodes with the status **NotReady**. The worker nodes with **NotReady** have public IP addresses, while the worker nodes with **Ready** have private IP addresses.

{: tsCauses}
Older clusters listed worker nodes by the cluster's public IP address. Now, worker nodes are listed by the cluster's private IP address. When you reload or update a node, the IP address is changed, but the reference to the public IP address remains.

{: tsResolve}
Service is not disrupted due to these duplicates, but you can remove the old worker node references from the API server.

  ```
  kubectl delete node <node_name1> <node_name2>
  ```
  {: pre}

<br />


## Accessing a pod on a new worker node fails with a timeout
{: #cs_nodes_duplicate_ip}

{: tsSymptoms}
You deleted a worker node in your cluster and then added a worker node. When you deployed a pod or Kubernetes service, the resource cannot access the newly created worker node, and the connection times out.

{: tsCauses}
If you delete a worker node from your cluster and then add a worker node, the new worker node might be assigned the private IP address of the deleted worker node. Calico uses this private IP address as a tag and continues to try to reach the deleted node.

{: tsResolve}
Manually update the reference of the private IP address to point to the correct node.

1.  Confirm that you have two worker nodes with the same **Private IP** address. Note the **Private IP** and **ID** of the deleted worker.

  ```
  ibmcloud ks workers <CLUSTER_NAME>
  ```
  {: pre}

  ```
  ID                                                 Public IP       Private IP       Machine Type   State     Status   Zone   Version
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w1   169.xx.xxx.xxx  10.xxx.xx.xxx    b2c.4x16       normal    Ready    dal10      1.10.5
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w2   169.xx.xxx.xxx  10.xxx.xx.xxx    b2c.4x16       deleted    -       dal10      1.10.5
  ```
  {: screen}

2.  Install the [Calico CLI](cs_network_policy.html#adding_network_policies).
3.  List the available worker nodes in Calico. Replace <path_to_file> with the local path to the Calico configuration file.

  ```
  calicoctl get nodes --config=filepath/calicoctl.cfg
  ```
  {: pre}

  ```
  NAME
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w1
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w2
  ```
  {: screen}

4.  Delete the duplicate worker node in Calico. Replace NODE_ID with the worker node ID.

  ```
  calicoctl delete node NODE_ID --config=<path_to_file>/calicoctl.cfg
  ```
  {: pre}

5.  Reboot the worker node that was not deleted.

  ```
  ibmcloud ks worker-reboot CLUSTER_ID NODE_ID
  ```
  {: pre}


The deleted node is no longer listed in Calico.

<br />




## Pods fail to deploy because of a pod security policy
{: #cs_psp}

{: tsSymptoms}
After creating a pod or running `kubectl get events` to check on a pod deployment, you see an error message similar to the following.

```
unable to validate against any pod security policy
```
{: screen}

{: tsCauses}
[The `PodSecurityPolicy` admission controller](cs_psp.html) checks the authorization of the user or service account, such as a deployment or Helm tiller, that tried to create the pod. If no pod security policy supports the user or service account, then the `PodSecurityPolicy` admission controller prevents the pods from being created.

If you deleted one of the pod security policy resources for [{{site.data.keyword.IBM_notm}} cluster management](cs_psp.html#ibm_psp), you might experience similar issues.

{: tsResolve}
Make sure that the user or service account is authorized by a pod security policy. You might need to [modify an existing policy](cs_psp.html#customize_psp).

If you deleted an {{site.data.keyword.IBM_notm}} cluster management resource, refresh the Kubernetes master to restore it.

1.  [Target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster.
2.  Refresh the Kubernetes master to restore it.

    ```
    ibmcloud ks apiserver-refresh
    ```
    {: pre}


<br />




## Cluster remains in a pending State
{: #cs_cluster_pending}

{: tsSymptoms}
When you deploy your cluster, it remains in a pending state and doesn't start.

{: tsCauses}
If you just created the cluster, the worker nodes might still be configuring. If you already wait for a while, you might have an invalid VLAN.

{: tsResolve}

You can try one of the following solutions:
  - Check the status of your cluster by running `ibmcloud ks clusters`. Then, check to be sure that your worker nodes are deployed by running `ibmcloud ks workers <cluster_name>`.
  - Check to see whether your VLAN is valid. To be valid, a VLAN must be associated with infrastructure that can host a worker with local disk storage. You can [list your VLANs](/docs/containers/cs_cli_reference.html#cs_vlans) by running `ibmcloud ks vlans <zone>` if the VLAN does not show in the list, then it is not valid. Choose a different VLAN.

<br />


## Pods remain in pending state
{: #cs_pods_pending}

{: tsSymptoms}
When you run `kubectl get pods`, you can see pods that remain in a **Pending** state.

{: tsCauses}
If you just created the Kubernetes cluster, the worker nodes might still be configuring. If this cluster is an existing one, you might not have enough capacity in your cluster to deploy the pod.

{: tsResolve}
This task requires an [Administrator access policy](cs_users.html#access_policies). Verify your current [access policy](cs_users.html#infra_access).

If you just created the Kubernetes cluster, run the following command and wait for the worker nodes to initialize.

```
kubectl get nodes
```
{: pre}

If this cluster is an existing one, check your cluster capacity.

1.  Set the proxy with the default port number.

  ```
  kubectl proxy
  ```
   {: pre}

2.  Open the Kubernetes dashboard.

  ```
  http://localhost:8001/ui
  ```
  {: pre}

3.  Check if you have enough capacity in your cluster to deploy your pod.

4.  If you don't have enough capacity in your cluster, resize your worker pool to add more nodes.

    1.  Review the current sizes and machine types of your worker pools to decide which one to resize.

        ```
        ibmcloud ks worker-pools
        ```
        {: pre}

    2.  Resize your worker pools to add more nodes to each zone that the pool spans.

        ```
        ibmcloud ks worker-pool-resize <worker_pool> --cluster <cluster_name_or_ID> --size-per-zone <workers_per_zone>
        ```
        {: pre}

5.  If your pods still stay in a **pending** state after the worker node is fully deployed, review the [Kubernetes documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-pod-replication-controller/#my-pod-stays-pending) to further troubleshoot the pending state of your pod.

<br />




## Containers do not start
{: #containers_do_not_start}

{: tsSymptoms}
The pods deploy successfully to clusters, but the containers do not start.

{: tsCauses}
Containers might not start when the registry quota is reached.

{: tsResolve}
[Free up storage in {{site.data.keyword.registryshort_notm}}.](../services/Registry/registry_quota.html#registry_quota_freeup)

<br />



## Cannot install a Helm chart with updated configuration values
{: #cs_helm_install}

{: tsSymptoms}
When you try to install an updated Helm chart by running `helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/<chart_name>`, you get the `Error: failed to download "ibm/<chart_name>"` error message.

{: tsCauses}
The URL for the {{site.data.keyword.Bluemix_notm}} repository in your Helm instance might be incorrect.

{: tsResolve}
To troubleshoot your Helm chart:

1. List the repositories currently available in your Helm instance.

    ```
    helm repo list
    ```
    {: pre}

2. In the output, verify that the URL for the {{site.data.keyword.Bluemix_notm}} repository, `ibm`, is `https://registry.bluemix.net/helm/ibm`.

    ```
    NAME    URL
    stable  https://kubernetes-charts.storage.googleapis.com
    local   http://127.0.0.1:8888/charts
    ibm     https://registry.bluemix.net/helm/ibm
    ```
    {: screen}

    * If the URL is incorrect:

        1. Remove the {{site.data.keyword.Bluemix_notm}} repository.

            ```
            helm repo remove ibm
            ```
            {: pre}

        2. Add the {{site.data.keyword.Bluemix_notm}} repository again.

            ```
            helm repo add ibm  https://registry.bluemix.net/helm/ibm
            ```
            {: pre}

    * If the URL is correct, get the latest updates from the repository.

        ```
        helm repo update
        ```
        {: pre}

3. Install the Helm chart with your updates.

    ```
    helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/<chart_name>
    ```
    {: pre}

<br />


## Getting help and support
{: #ts_getting_help}

Still having issues with your cluster?
{: shortdesc}

-   To see whether {{site.data.keyword.Bluemix_notm}} is available, [check the {{site.data.keyword.Bluemix_notm}} status page ![External link icon](../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/bluemix/support/#status).
-   Post a question in the [{{site.data.keyword.containershort_notm}} Slack ![External link icon](../icons/launch-glyph.svg "External link icon")](https://ibm-container-service.slack.com).

    If you are not using an IBM ID for your {{site.data.keyword.Bluemix_notm}} account, [request an invitation](https://bxcs-slack-invite.mybluemix.net/) to this Slack.
    {: tip}
-   Review the forums to see whether other users ran into the same issue. When you use the forums to ask a question, tag your question so that it is seen by the {{site.data.keyword.Bluemix_notm}} development teams.

    -   If you have technical questions about developing or deploying clusters or apps with {{site.data.keyword.containershort_notm}}, post your question on [Stack Overflow ![External link icon](../icons/launch-glyph.svg "External link icon")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) and tag your question with `ibm-cloud`, `kubernetes`, and `containers`.
    -   For questions about the service and getting started instructions, use the [IBM developerWorks dW Answers ![External link icon](../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix) forum. Include the `ibm-cloud` and `containers` tags.
    See [Getting help](/docs/get-support/howtogetsupport.html#using-avatar) for more details about using the forums.

-   Contact IBM Support by opening a ticket. To learn about opening an IBM support ticket, or about support levels and ticket severities, see [Contacting support](/docs/get-support/howtogetsupport.html#getting-customer-support).

{: tip}
When you report an issue, include your cluster ID. To get your cluster ID, run `ibmcloud ks clusters`.

