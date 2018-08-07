---

copyright:
  years: 2014, 2018
lastupdated: "2018-08-06"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Getting started with clusters in {{site.data.keyword.Bluemix_dedicated_notm}}
{: #dedicated}

If you have an {{site.data.keyword.Bluemix_dedicated}} account, you can deploy Kubernetes clusters in a dedicated cloud environment (`https://<my-dedicated-cloud-instance>.bluemix.net`) and connect with the preselected {{site.data.keyword.Bluemix_notm}} services that are also running there.
{:shortdesc}

If you do not have an {{site.data.keyword.Bluemix_dedicated_notm}} account, you can [get started with {{site.data.keyword.containershort_notm}}](container_index.html) in a public {{site.data.keyword.Bluemix_notm}} account.

## About the Dedicated cloud environment
{: #dedicated_environment}

With an {{site.data.keyword.Bluemix_dedicated_notm}} account, available physical resources are dedicated to your cluster only and are not shared with clusters from other {{site.data.keyword.IBM_notm}} customers. You might choose to set up an {{site.data.keyword.Bluemix_dedicated_notm}} environment when you want isolation for your cluster and you require isolation for the other {{site.data.keyword.Bluemix_notm}} services that you use. If you do not have a Dedicated account, you can [create clusters with dedicated hardware in {{site.data.keyword.Bluemix_notm}} public](cs_clusters.html#clusters_ui).

With {{site.data.keyword.Bluemix_dedicated_notm}}, you can create clusters from the catalog in the Dedicated console or by using the {{site.data.keyword.containershort_notm}} CLI. To use the Dedicated console, you log in to both your Dedicated and public accounts simultaneously by using your IBMid. You can use the dual login to access your public clusters by using your Dedicated console. To use the CLI, you log in by using your Dedicated endpoint (`api.<my-dedicated-cloud-instance>.bluemix.net.`). You then target the {{site.data.keyword.containershort_notm}} API endpoint of the public region that is associated with the Dedicated environment.

The most significant differences between {{site.data.keyword.Bluemix_notm}} public and Dedicated are as follows.

*   In {{site.data.keyword.Bluemix_dedicated_notm}}, {{site.data.keyword.IBM_notm}} owns and manages the IBM Cloud infrastructure (SoftLayer) account that the worker nodes, VLANs, and subnets are deployed into. In {{site.data.keyword.Bluemix_notm}} public, you own the IBM Cloud infrastructure (SoftLayer) account.
*   In {{site.data.keyword.Bluemix_dedicated_notm}}, specifications for the VLANs and subnets in the {{site.data.keyword.IBM_notm}}-managed IBM Cloud infrastructure (SoftLayer) account are determined when the Dedicated environment is enabled. In {{site.data.keyword.Bluemix_notm}} public, specifications for VLANs and subnets are determined when the cluster is created.

### Differences in cluster management between the cloud environments
{: #dedicated_env_differences}

<table>
<caption>Differences in cluster management</caption>
<col width="20%">
<col width="40%">
<col width="40%">
 <thead>
 <th>Area</th>
 <th>{{site.data.keyword.Bluemix_notm}} public</th>
 <th>{{site.data.keyword.Bluemix_dedicated_notm}}</th>
 </thead>
 <tbody>
 <tr>
 <td>Cluster creation</td>
 <td>Create a free cluster or a standard cluster.</td>
 <td>Create a standard cluster.</td>
 </tr>
 <tr>
 <td>Cluster hardware and ownership</td>
 <td>In standard clusters, the hardware can be shared by other {{site.data.keyword.IBM_notm}} customers or dedicated to you only. The public and private VLANs are owned and managed by you in your IBM Cloud infrastructure (SoftLayer) account.</td>
 <td>In clusters on {{site.data.keyword.Bluemix_dedicated_notm}}, the hardware is always dedicated. The public and private VLANs that are available for cluster creation are pre-defined when the {{site.data.keyword.Bluemix_dedicated_notm}} environment is set up, and are owned and managed by IBM for you. The zone that is available during cluster creation is also pre-defined for the {{site.data.keyword.Bluemix_notm}} environment.</td>
 </tr>
 <tr>
 <td>Load balancer and Ingress networking</td>
 <td>During the provisioning of standard clusters, the following actions occur automatically.<ul><li>One portable public and one portable private subnet are bound to your cluster and assigned to your IBM Cloud infrastructure (SoftLayer) account. More subnets can be requested through your IBM Cloud infrastructure (SoftLayer) account.</li></li><li>One portable public IP address is used for a highly available Ingress application load balancer (ALB) and a unique public route is assigned in the format <code>&lt;cluster_name&gt;. containers.appdomain.cloud</code>. You can use this route to expose multiple apps to the public. One portable private IP address is used for a private ALB.</li><li>Four portable public and four portable private IP addresses are assigned to the cluster that can be used for load balancer services.</ul></td>
 <td>When you create your Dedicated account, you make a connectivity decision on how you want to expose and access your cluster services. To use your own enterprise IP ranges (user-manged IPs), you must provide them when you [set up an {{site.data.keyword.Bluemix_dedicated_notm}} environment](/docs/dedicated/index.html#setupdedicated). <ul><li>By default, no portable public subnets are bound to clusters that you create in your Dedicated account. Instead, you have the flexibility to choose the connectivity model which best suits your enterprise.</li><li>After you create the cluster, you choose the type of subnets you want bind to and use with your cluster for either load balancer or Ingress connectivity.<ul><li>For public or private portable subnets, you can [add subnets to clusters](cs_subnets.html#subnets)</li><li>For user-managed IP addresses that you provided to IBM at Dedicated onboarding, you can [add user-managed subnets to clusters](#dedicated_byoip_subnets).</li></ul></li><li>After you bind a subnet to your cluster, the Ingress ALB is created. A public Ingress route is created only if you use a portable public subnet.</li></ul></td>
 </tr>
 <tr>
 <td>NodePort networking</td>
 <td>Expose a public port on your worker node and use the public IP address of the worker node to publicly access your service in the cluster.</td>
 <td>All public IP addresses of the workers nodes are blocked by a firewall. However, for {{site.data.keyword.Bluemix_notm}} services that are added to the cluster, the NodePort can be accessed through a public IP address or a private IP address.</td>
 </tr>
 <tr>
 <td>Persistent storage</td>
 <td>Use [dynamic provisioning](cs_storage_basics.html#dynamic_provisioning) or [static provisioning](cs_storage_basics.html#static_provisioning) of volumes.</td>
 <td>Use [dynamic provisioning](cs_storage_basics.html#dynamic_provisioning) of volumes. [Open a support ticket](/docs/get-support/howtogetsupport.html#getting-customer-support) to request a backup for your volumes, request a restoration from your volumes, and perform other storage functions.</li></ul></td>
 </tr>
 <tr>
 <td>Image registry URL in {{site.data.keyword.registryshort_notm}}</td>
 <td><ul><li>US-South and US-East: <code>registry.ng bluemix.net</code></li><li>UK-South: <code>registry.eu-gb.bluemix.net</code></li><li>EU-Central (Frankfurt): <code>registry.eu-de.bluemix.net</code></li><li>Australia (Sydney): <code>registry.au-syd.bluemix.net</code></li></ul></td>
 <td><ul><li>For new namespaces, use the same region-based registries that are defined for {{site.data.keyword.Bluemix_notm}} public.</li><li>For namespaces that were set up for single and scalable containers in {{site.data.keyword.Bluemix_dedicated_notm}}, use <code>registry.&lt;dedicated_domain&gt;</code></li></ul></td>
 </tr>
 <tr>
 <td>Accessing the registry</td>
 <td>See the options in [Using private and public image registries with {{site.data.keyword.containershort_notm}}](cs_images.html).</td>
 <td><ul><li>For new namespaces, see the options in [Using private and public image registries with {{site.data.keyword.containershort_notm}}](cs_images.html).</li><li>For namespaces that were set up for single and scalable groups, [use a token and create a Kubernetes secret](cs_dedicated_tokens.html#cs_dedicated_tokens) for authentication.</li></ul></td>
 </tr>
 <tr>
 <td>Multizone clusters</td>
 <td>Create [multizone clusters](cs_clusters.html#multi_zone) by adding more zones to your worker pools.</td>
 <td>Create [single zone clusters](cs_clusters.html#single_zone). The available zone was pre-defined when the {{site.data.keyword.Bluemix_dedicated_notm}} environment was set up. By default, a single zone cluster is set up with a worker pool that is named `default`. The worker pool groups worker nodes with the same configuration, such as the machine type, that you defined during cluster creation. You can add more worker nodes to your cluster by [resizing an existing worker pool](cs_clusters.html#resize_pool) or by [adding a new worker pool](cs_clusters.html#add_pool). When you add a worker pool, you must add the available zone to the worker pool so that workers can deploy into the zone. However, you cannot add other zones to your worker pools.</td>
 </tr>
</tbody></table>
{: caption="Feature differences between {{site.data.keyword.Bluemix_notm}} public and {{site.data.keyword.Bluemix_dedicated_notm}}" caption-side="top"}

<br />


### Service architecture
{: #dedicated_ov_architecture}

Each worker node is set up with separate compute resources, networking, and volume service.
{:shortdesc}

Built-in security features provide isolation, resource management capabilities, and worker node security compliance. The worker node communicates with the master by using secure TLS certificates and openVPN connection.


*Kubernetes architecture and networking in the {{site.data.keyword.Bluemix_dedicated_notm}}*

![{{site.data.keyword.containershort_notm}} Kubernetes architecture on {{site.data.keyword.Bluemix_dedicated_notm}}](images/cs_dedicated_arch.png)

<br />


## Setting up {{site.data.keyword.containershort_notm}} on Dedicated
{: #dedicated_setup}

Each {{site.data.keyword.Bluemix_dedicated_notm}} environment has a public, client-owned, corporate account in {{site.data.keyword.Bluemix_notm}}. For users in the Dedicated environment to create clusters, the administrator must add the users to a public corporate account.
{:shortdesc}

Before you begin:
  * [Set up an {{site.data.keyword.Bluemix_dedicated_notm}} environment](/docs/dedicated/index.html#setupdedicated).
  * If your local system or your corporate network controls public internet endpoints by using proxies or firewalls, you must [open required ports and IP addresses in your firewall](cs_firewall.html#firewall).
  * [Download the Cloud Foundry CLI ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/cloudfoundry/cli/releases) and [Add the IBM Cloud Admin CLI plug-in](/docs/cli/plugins/bluemix_admin/index.html#adding-the-ibm-cloud-admin-cli-plug-in).

To allow {{site.data.keyword.Bluemix_dedicated_notm}} users to access clusters:

1.  The owner of your public {{site.data.keyword.Bluemix_notm}} account must generate an API key.
    1.  Log in to the endpoint for your {{site.data.keyword.Bluemix_dedicated_notm}} instance. Enter the {{site.data.keyword.Bluemix_notm}} credentials for the public account owner and select your account when prompted.

        ```
        ibmcloud login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net
        ```
        {: pre}

        **Note:** If you have a federated ID, use `ibmcloud login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net --sso` to log in to the {{site.data.keyword.Bluemix_notm}} CLI. Enter your user name and use the provided URL in your CLI output to retrieve your one-time passcode. You know that you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.

    2.  Generate an API key for inviting users to the public account. Note the API key value, which the Dedicated account administrator must use in the next step.

        ```
        ibmcloud iam api-key-create <key_name> -d "Key to invite users to <dedicated_account_name>"
        ```
        {: pre}

    3.  Note the GUID of the public account organization that you want to invite users to, which the Dedicated account administrator must use in the next step.

        ```
        ibmcloud account orgs
        ```
        {: pre}

2.  The owner of your {{site.data.keyword.Bluemix_dedicated_notm}} account can invite single or multiple users to your Public account.
    1.  Log in to the endpoint for your {{site.data.keyword.Bluemix_dedicated_notm}} instance. Enter the {{site.data.keyword.Bluemix_notm}} credentials for the Dedicated account owner and select your account when prompted.

        ```
        ibmcloud login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net
        ```
        {: pre}

        **Note:** If you have a federated ID, use `ibmcloud login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net --sso` to log in to the {{site.data.keyword.Bluemix_notm}} CLI. Enter your user name and use the provided URL in your CLI output to retrieve your one-time passcode. You know that you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.

    2.  Invite the users to the public account.
        * To invite a single user:

            ```
            ibmcloud cf bluemix-admin invite-users-to-public -userid=<user_email> -apikey=<public_API_key> -public_org_id=<public_org_ID>
            ```
            {: pre}

            Replace <em>&lt;user_IBMid&gt;</em> with the email of the user you want to invite, <em>&lt;public_API_key&gt;</em> with the API key generated in the previous step, and <em>&lt;public_org_ID&gt;</em> with the GUID of the public account organization. For more information about this command, see [Inviting a user from IBM Cloud Dedicated](/docs/cli/plugins/bluemix_admin/index.html#admin_dedicated_invite_public).

        * To invite all users currently in a Dedicated account organization:

            ```
            ibmcloud cf bluemix-admin invite-users-to-public -organization=<dedicated_org_ID> -apikey=<public_API_key> -public_org_id=<public_org_ID>
            ```

            Replace <em>&lt;dedicated_org_ID&gt;</em> with the Dedicated account organization ID, <em>&lt;public_API_key&gt;</em> with the API key generated in the previous step, and <em>&lt;public_org_ID&gt;</em> with the public account organization GUID. For more information about this command, see [Inviting a user from IBM Cloud Dedicated](/docs/cli/plugins/bluemix_admin/index.html#admin_dedicated_invite_public).

    3.  If an IBMid exists for a user, the user is automatically added to the specified organization in the public account. If an IBMid does not exist for a user, then an invitation is sent to the user's email address. After the user accepts the invitation, an IBMid is created for the user, and the user is added to the specified organization in the public account.

    4.  Verify that the users were added to the account.

        ```
        ibmcloud cf bluemix-admin invite-users-status -apikey=<public_API_key>
        ```
        {: pre}

        Invited users that have an existing IBMid have the `ACTIVE` status. Invited users that did not have an existing IBMid have the `PENDING` status before they accept the invitation the `ACTIVE` status after they accept the invitation.

3.  If any user needs cluster create privileges, you must grant the Administrator role to that user.

    1.  From the menu bar in the public console, click **Manage > Security > Identity and Access**, and then click **Users**.

    2.  From the row for the user that you want to assign access, select the **Actions** menu, and then click **Assign access**.

    3.  Select **Assign access to resources**.

    4.  From the **Services** list, select **{{site.data.keyword.containerlong}}**.

    5.  From the **Region** list, select **All current regions** or a specific region, if you are prompted.

    6. Under **Select roles**, select Administrator.

    7. Click **Assign**.

4.  Users can now log in to the Dedicated account endpoint to start creating clusters.

    1.  Log in to the endpoint for your {{site.data.keyword.Bluemix_dedicated_notm}} instance. Enter your IBMid when prompted.

        ```
        ibmcloud login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net
        ```
        {: pre}

        **Note:** If you have a federated ID, use `ibmcloud login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net --sso` to log in to the {{site.data.keyword.Bluemix_notm}} CLI. Enter your user name and use the provided URL in your CLI output to retrieve your one-time passcode. You know that you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.

    2.  If you are logging in for the first time, provide your Dedicated user ID and password when prompted. Your Dedicated account is authenticated, and the Dedicated and public accounts are linked together. Every time you log in after this first time, you use only your IBMid to log in. For more information, see [Connecting a dedicated ID to your public IBMid](/docs/cli/connect_dedicated_id.html#connect_dedicated_id).

        **Note**: You must log in to both your Dedicated account and your public account to create clusters. If you only want to log in to your Dedicated account, use the `--no-iam` flag when you log in to the Dedicated endpoint.

    3.  To create or access clusters in the dedicated environment, you must set the region that is associated with that environment.

        ```
        ibmcloud ks region-set
        ```
        {: pre}

5.  If you want to unlink your accounts, you can disconnect your IBMid from your Dedicated user ID. For more information, see [Disconnect your dedicated ID from the public IBMid](/docs/cli/connect_dedicated_id.html#disconnect-your-dedicated-id-from-the-public-ibmid).

    ```
    ibmcloud iam dedicated-id-disconnect
    ```
    {: pre}

<br />


## Creating clusters
{: #dedicated_administering}

Design your {{site.data.keyword.Bluemix_dedicated_notm}} cluster setup for maximum availability and capacity.
{:shortdesc}

### Creating clusters with the GUI
{: #dedicated_creating_ui}

1. Open your Dedicated console: `https://<my-dedicated-cloud-instance>.bluemix.net`.

2. Select the **Also log in to {{site.data.keyword.Bluemix_notm}} Public** check box and click **Log in**.

3. Follow the prompts to log in with your IBMid. If you are logging in to your Dedicated account for the first time, then follow the prompts to log in to {{site.data.keyword.Bluemix_dedicated_notm}}.

4. From the catalog, select **Containers** and click **Kubernetes cluster**.

5. Configure your cluster details.

    1. Enter a **Cluster Name**. The name must start with a letter, can contain letters, numbers, and hyphen (-), and must be 35 characters or fewer. The cluster name and the region in which the cluster is deployed form the fully qualified domain name for the Ingress subdomain. To ensure that the Ingress subdomain is unique within a region, the cluster name might be truncated and appended with a random value within the Ingress domain name.

    2. Select the **Zone** in which to deploy your cluster. The available zone was pre-defined when the {{site.data.keyword.Bluemix_dedicated_notm}} environment was set up.

    3. Choose the Kubernetes API server version for the cluster master node.

    4. Select a type of hardware isolation. Virtual is billed hourly and bare metal is billed monthly.

        - **Virtual - Dedicated**: Your worker nodes are hosted on infrastructure that is devoted to your account. Your physical resources are completely isolated.

        - **Bare Metal**: Billed monthly, bare metal servers are provisioned by manual interaction with IBM Cloud infrastructure (SoftLayer), and can take more than one business day to complete. Bare metal is best suited for high-performance applications that need more resources and host control. 

        Be sure that you want to provision a bare metal machine. Because it is billed monthly, if you cancel it immediately after an order by mistake, you are still charged the full month.
        {:tip}

    5. Select a **Machine type**. The machine type defines the amount of virtual CPU, memory, and disk space that is set up in each worker node and made available to the containers. Available bare metal and virtual machines types vary by the zone in which you deploy the cluster. For more information, see the documentation for the `ibmcloud ks machine-type` [command](cs_cli_reference.html#cs_machine_types). After you create your cluster, you can add different machine types by adding a worker node to the cluster.

    6. Choose the **Number of worker nodes** that you need. Select `3` to ensure high availability of your cluster.

    7. Select a **Public VLAN** (optional) and **Private VLAN** (required). The available public and private VLANs are pre-defined when the {{site.data.keyword.Bluemix_dedicated_notm}} environment is set up. Both VLANs communicate between worker nodes but the public VLAN also communicates with the IBM-managed Kubernetes master. You can use the same VLAN for multiple clusters.
        **Note**: If worker nodes are set up with a private VLAN only, you must configure an alternative solution for network connectivity.

    8. By default, **Encrypt local disk** is selected. If you choose to clear the check box, then the host's Docker data is not encrypted. [Learn more about the encryption](cs_secure.html#encrypted_disk).

6. Click **Create cluster**. You can see the progress of the worker node deployment in the **Worker nodes** tab. When the deployment is done, you can see that your cluster is ready in the **Overview** tab.
    **Note:** Every worker node is assigned a unique worker node ID and domain name that must not be manually changed after the cluster is created. Changing the ID or domain name prevents the Kubernetes master from managing your cluster.

### Creating clusters with the CLI
{: #dedicated_creating_cli}

1.  Install the {{site.data.keyword.Bluemix_notm}} CLI and the [{{site.data.keyword.containershort_notm}} plug-in](cs_cli_install.html#cs_cli_install).
2.  Log in to the endpoint for your {{site.data.keyword.Bluemix_dedicated_notm}} instance. Enter your {{site.data.keyword.Bluemix_notm}} credentials and select your account when prompted.

    ```
    ibmcloud login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net
    ```
    {: pre}

    **Note:** If you have a federated ID, use `ibmcloud login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net --sso` to log in to the {{site.data.keyword.Bluemix_notm}} CLI. Enter your user name and use the provided URL in your CLI output to retrieve your one-time passcode. You know that you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.

3.  To target a region, run `ibmcloud ks region-set`.

4.  Create a cluster with the `cluster-create` command. When you create a standard cluster, the hardware of the worker node is billed by hours of usage.

    Example:

    ```
    ibmcloud ks cluster-create --zone <zone> --machine-type <machine_type> --name <cluster_name> --workers <number>
    ```
    {: pre}

    <table>
    <caption>Understanding this command's components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding this command's components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>cluster-create</code></td>
    <td>The command to create a cluster in your {{site.data.keyword.Bluemix_notm}} organization.</td>
    </tr>
    <tr>
    <td><code>--zone <em>&lt;zone&gt;</em></code></td>
    <td>Enter the {{site.data.keyword.Bluemix_notm}} zone ID that your Dedicated environment is configured to use.</td>
    </tr>
    <tr>
    <td><code>--machine-type <em>&lt;machine_type&gt;</em></code></td>
    <td>Enter a machine type. You can deploy your worker nodes as virtual machines on dedicated hardware, or as physical machines on bare metal. Available physical and virtual machines types vary by the zone in which you deploy the cluster. For more information, see the documentation for the `ibmcloud ks machine-type` [command](cs_cli_reference.html#cs_machine_types).</td>
    </tr>
    <tr>
    <td><code>--public-vlan <em>&lt;machine_type&gt;</em></code></td>
    <td>Enter the ID of the public VLAN that your Dedicated environment is configured to use. If you want to connect your worker nodes to a private VLAN only, do not specify this option. **Note**: If worker nodes are set up with a private VLAN only, you must configure an alternative solution for network connectivity.</td>
    </tr>
    <tr>
    <td><code>--private-vlan <em>&lt;machine_type&gt;</em></code></td>
    <td>Enter the ID of the private VLAN that your Dedicated environment is configured to use.</td>
    </tr>  
    <tr>
    <td><code>--name <em>&lt;name&gt;</em></code></td>
    <td>Enter a name for your cluster. The name must start with a letter, can contain letters, numbers, and hyphen (-), and must be 35 characters or fewer. The cluster name and the region in which the cluster is deployed form the fully qualified domain name for the Ingress subdomain. To ensure that the Ingress subdomain is unique within a region, the cluster name might be truncated and appended with a random value within the Ingress domain name.
</td>
    </tr>
    <tr>
    <td><code>--workers <em>&lt;number&gt;</em></code></td>
    <td>Enter the number of worker nodes to include in the cluster. If the <code>--workers</code> option is not specified, one worker node is created.</td>
    </tr>
    <tr>
    <td><code>--kube-version <em>&lt;major.minor.patch&gt;</em></code></td>
    <td>The Kubernetes version for the cluster master node. This value is optional. When the version is not specified, the cluster is created with the default of supported Kubernetes versions. To see available versions, run <code>ibmcloud ks kube-versions</code>.
</td>
    </tr>
    <tr>
    <td><code>--disable-disk-encrypt</code></td>
    <td>Worker nodes feature [disk encryption](cs_secure.html#encrypted_disk) by default. If you want to disable encryption, include this option.</td>
    </tr>
    <tr>
    <td><code>--trusted</code></td>
    <td>Enable [Trusted Compute](cs_secure.html#trusted_compute) to verify your bare metal worker nodes against tampering. If you don't enable trust during cluster creation but want to later, you can use the `ibmcloud ks feature-enable` [command](cs_cli_reference.html#cs_cluster_feature_enable). After you enable trust, you cannot disable it later.</td>
    </tr>
    </tbody></table>

5.  Verify that the creation of the cluster was requested.

    ```
    ibmcloud ks clusters
    ```
    {: pre}

    **Note:**
    * For virtual machines, it can take a few minutes for the worker node machines to be ordered, and for the cluster to be set up and provisioned in your account. Bare metal physical machines are provisioned by manual interaction with IBM Cloud infrastructure (SoftLayer), and can take more than one business day to complete.
    * If you see the following error message, [open a support ticket](/docs/get-support/howtogetsupport.html#getting-customer-support).
        ```
        {{site.data.keyword.Bluemix_notm}} Infrastructure Exception: Could not place order. There are insufficient resources behind router 'router_name' to fulfill the request for the following guests: 'worker_id'.
        ```

    When the provisioning of your cluster is completed, the status of your cluster changes to **deployed**.

    ```
    Name         ID                                   State      Created          Workers   Zone   Version
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   1         mil01      1.10.5
    ```
    {: screen}

6.  Check the status of the worker nodes.

    ```
    ibmcloud ks workers <cluster_name_or_ID>
    ```
    {: pre}

    When the worker nodes are ready, the state changes to **normal** and the status is **Ready**. When the node status is **Ready**, you can then access the cluster.

    **Note:** Every worker node is assigned a unique worker node ID and domain name that must not be changed manually after the cluster is created. Changing the ID or domain name prevents the Kubernetes master from managing your cluster.

    ```
    ID                                                 Public IP       Private IP       Machine Type   State    Status   Zone   Version
    kube-mil01-paf97e8843e29941b49c598f516de72101-w1   169.xx.xxx.xxx  10.xxx.xx.xxx    free           normal   Ready    mil01      1.10.5
    ```
    {: screen}

7.  Set the cluster that you created as the context for this session. Complete these configuration steps every time that you work with your cluster.

    1.  Get the command to set the environment variable and download the Kubernetes configuration files.

        ```
        ibmcloud ks cluster-config <cluster_name_or_ID>
        ```
        {: pre}

        When the download of the configuration files is finished, a command is displayed that you can use to set the path to the local Kubernetes configuration file as an environment variable.

        Example for OS X:

        ```
        export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
        ```
        {: screen}

    2.  Copy and paste the command in the output to set the `KUBECONFIG` environment variable.
    3.  Verify that the `KUBECONFIG` environment variable is set properly.

        Example for OS X:

        ```
        echo $KUBECONFIG
        ```
        {: pre}

        Output:

        ```
        /Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml

        ```
        {: screen}

8.  Access your Kubernetes dashboard with the default port 8001.
    1.  Set the proxy with the default port number.

        ```
        kubectl proxy
        ```
        {: pre}

        ```
        Starting to serve on 127.0.0.1:8001
        ```
        {: screen}

    2.  Open the following URL in a web browser to see the Kubernetes dashboard.

        ```
        http://localhost:8001/ui
        ```
        {: codeblock}

### Adding worker nodes
{: #add_workers}

With a {{site.data.keyword.Bluemix_dedicated_notm}}, you can create only [single zone clusters](cs_clusters.html#single_zone). By default, a single zone cluster is set up with a worker pool that is named `default`. The worker pool groups worker nodes with the same configuration, such as the machine type, that you defined during cluster creation. You can add more worker nodes to your cluster by [resizing an existing worker pool](cs_clusters.html#resize_pool) or by [adding a new worker pool](cs_clusters.html#add_pool). When you add a worker pool, you must add the available zone to the worker pool so that workers can deploy into the zone. However, you cannot add other zones to your worker pools.

### Using private and public image registries
{: #dedicated_images}

Learn more about [securing your personal information](cs_secure.html#pi) when you work with container images.

For new namespaces, see the options in [Using private and public image registries with {{site.data.keyword.containershort_notm}}](cs_images.html). For namespaces that were set up for single and scalable groups, [use a token and create a Kubernetes secret](cs_dedicated_tokens.html#cs_dedicated_tokens) for authentication.

### Adding subnets to clusters
{: #dedicated_cluster_subnet}

Change the pool of available portable public IP addresses by adding subnets to your cluster. For more information, see [Adding subnets to clusters](cs_subnets.html#subnets). Review the following differences for adding subnets to Dedicated clusters.

#### Adding more user-managed subnets and IP addresses to your Kubernetes clusters
{: #dedicated_byoip_subnets}

Provide more of your own subnets from an on-premises network that you want to use to access {{site.data.keyword.containershort_notm}}. You can add private IP addresses from those subnets to Ingress and load balancer services in your Kubernetes cluster. User-managed subnets are configured in one of two ways depending on the format of the subnet that you want to use.

Requirements:
- User-managed subnets can be added to private VLANs only.
- The subnet prefix length limit is /24 to /30. For example, `203.0.113.0/24` specifies 253 usable private IP addresses, while `203.0.113.0/30` specifies 1 usable private IP address.
- The first IP address in the subnet must be used as the gateway for the subnet.

Before you begin: Configure the routing of network traffic into and out of your enterprise network to the {{site.data.keyword.Bluemix_dedicated_notm}} network that will use the user-managed subnet.

1. To use your own subnet, [open a support ticket](/docs/get-support/howtogetsupport.html#getting-customer-support) and provide the list of subnet CIDRs that you want to use.
    **Note**: The way that the ALB and load balancers are managed for on-premises and internal account connectivity differs depending on the format of the subnet CIDR. See the final step for configuration differences.

2. After {{site.data.keyword.IBM_notm}} provisions the user-managed subnets, make the subnet available to your Kubernetes cluster.

    ```
    ibmcloud ks cluster-user-subnet-add <cluster_name> <subnet_CIDR> <private_VLAN>
    ```
    {: pre}
    Replace <em>&lt;cluster_name&gt;</em> with the name or ID of your cluster, <em>&lt;subnet_CIDR&gt;</em> with one of the subnet CIDRs that you provided in the support ticket, and <em>&lt;private_VLAN&gt;</em> with an available private VLAN ID. You can find the ID of an available private VLAN by running `ibmcloud ks vlans`.

3. Verify that the subnets were added to your cluster. The field **User-managed** for user-provided subnets is _`true`_.

    ```
    ibmcloud ks cluster-get --showResources <cluster_name>
    ```
    {: pre}

    ```
    VLANs
    VLAN ID   Subnet CIDR         Public       User-managed
    1555503   169.xx.xxx.xxx/24   true         false
    1555505   10.xxx.xx.xxx/24    false        false
    1555505   10.xxx.xx.xxx/24    false        true
    ```
    {: screen}

4. **Important**: To enable communication between workers that are on different subnets on the same VLAN, you must [enable routing between subnets on the same VLAN](/docs/infrastructure/vlans/vlan-spanning.html#vlan-spanning).

5. To configure on-premises and internal account connectivity, choose between these options:
  - If you used a 10.x.x.x private IP address range for the subnet, use valid IPs from that range to configure on-premises and internal account connectivity with Ingress and a load balancer. For more information, see [Planning networking with NodePort, LoadBalancer, or Ingress services](cs_network_planning.html#planning).
  - If you did not use a 10.x.x.x private IP address range for the subnet, use valid IPs from that range to configure on-premises connectivity with Ingress and a load balancer. For more information, see [Planning networking with NodePort, LoadBalancer, or Ingress services](cs_network_planning.html#planning). However, you must use an IBM Cloud infrastructure (SoftLayer) portable private subnet to configure internal account connectivity between your cluster and other Cloud Foundry-based services. You can create a portable private subnet with the [`ibmcloud ks cluster-subnet-add`](cs_cli_reference.html#cs_cluster_subnet_add) command. For this scenario, your cluster has both a user-managed subnet for on-premises connectivity and an IBM Cloud infrastructure (SoftLayer) portable private subnet for internal account connectivity.

### Other cluster configurations
{: #dedicated_other}

Review the following options for other cluster configurations:
  * [Managing cluster access](cs_users.html#access_policies)
  * [Updating the Kubernetes master](cs_cluster_update.html#master)
  * [Updating worker nodes](cs_cluster_update.html#worker_node)
  * [Configuring cluster logging](cs_health.html#logging)
      * **Note**: Log enablement is not supported from the Dedicated endpoint. You must log in to the public {{site.data.keyword.cloud_notm}} endpoint and target your public org and space to enable log forwarding.
  * [Configuring cluster monitoring](cs_health.html#view_metrics)
      * **Note**: An `ibm-monitoring` cluster exists within each {{site.data.keyword.Bluemix_dedicated_notm}} account. This cluster continuously monitors the health of the {{site.data.keyword.containerlong_notm}} in the Dedicated environment, checking the stability and connectivity of the environment. Do not remove this cluster from the environment.
  * [Visualizing Kubernetes cluster resources](cs_integrations.html#weavescope)
  * [Removing clusters](cs_clusters.html#remove)

<br />


## Deploying apps in clusters
{: #dedicated_apps}

You can use Kubernetes techniques to deploy apps in {{site.data.keyword.Bluemix_dedicated_notm}} clusters and to ensure that your apps are always up and running.
{:shortdesc}

To deploy apps in clusters, you can follow the instructions for [deploying apps in {{site.data.keyword.Bluemix_notm}} public clusters](cs_app.html#app). Review the following differences for {{site.data.keyword.Bluemix_dedicated_notm}} clusters.

Learn more about [securing your personal information](cs_secure.html#pi) when you work with Kubernetes resources.

### Allowing public access to apps
{: #dedicated_apps_public}

For {{site.data.keyword.Bluemix_dedicated_notm}} environments, public primary IP addresses are blocked by a firewall. To make an app publicly available, use a [LoadBalancer service](#dedicated_apps_public_load_balancer) or [Ingress](#dedicated_apps_public_ingress) instead of a NodePort service. If you require access to a LoadBalancer service or Ingress that portable public IP addresses, supply an enterprise firewall whitelist to IBM at service onboarding time.

#### Configuring access to an app by using the load balancer service type
{: #dedicated_apps_public_load_balancer}

If you want to use public IP addresses for the load balancer, ensure that an enterprise firewall whitelist was provided to IBM or [open a support ticket](/docs/get-support/howtogetsupport.html#getting-customer-support) to configure the firewall whitelist. Then, follow the steps in [Exposing apps with LoadBalancers](cs_loadbalancer.html).

#### Configuring public access to an app by using Ingress
{: #dedicated_apps_public_ingress}

If you want to use public IP addresses for the Ingress ALB, ensure that an enterprise firewall whitelist was provided to IBM or [open a support ticket](/docs/get-support/howtogetsupport.html#getting-customer-support) to configure the firewall whitelist. Then, follow the steps in [Exposing apps to the public](cs_ingress.html#ingress_expose_public).

### Creating persistent storage
{: #dedicated_apps_volume_claim}

To review options for creating persistent storage, see [Persistent data storage](cs_storage_planning.html#persistent). To request a backup for your volumes, a restoration from your volumes, or a deletion of volumes, you must [open a support ticket](/docs/get-support/howtogetsupport.html#getting-customer-support).
