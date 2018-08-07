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


# Assigning cluster access
{: #users}

As a cluster administrator, you can define access policies for your Kubernetes cluster to create different levels of access for different users. For example, you can authorize certain users to work with cluster resources while others can deploy containers only.
{: shortdesc}


## Understanding access policies and permissions
{: #access_policies}

<dl>
  <dt>Do I have to set access policies?</dt>
    <dd>You must define access policies for every user that works with {{site.data.keyword.containershort_notm}}. The scope of an access policy is based on a users defined role or roles that determine the actions that they are allowed to perform. Some policies are pre-defined, but others can be customized. The same policy is enforced whether the user makes the request from the {{site.data.keyword.containershort_notm}} GUI or through the CLI, even when the actions are completed in IBM Cloud infrastructure (SoftLayer).</dd>

  <dt>What are the types of permissions?</dt>
    <dd><p><strong>Platform</strong>: {{site.data.keyword.containershort_notm}} is configured to use {{site.data.keyword.Bluemix_notm}} platform roles to determine the actions that individuals can perform on a cluster. The role permissions build on each other, which means that the `Editor` role has all of the same permissions as the `Viewer` role, plus the permissions that are granted to an editor. You can set these policies by region. These policies must be set along with infrastructure policies and have corresponding RBAC roles that are automatically assigned to the default namespace. Example actions are creating or removing clusters, or adding extra worker nodes.</p> <p><strong>Infrastructure</strong>: You can determine the access levels for your infrastructure such as the cluster node machines, networking, or storage resources. You must set this type of policy along with {{site.data.keyword.containershort_notm}} platform access policies. To learn about the available roles, check out [infrastructure permissions](/docs/iam/infrastructureaccess.html#infrapermission). In addition to granting specific infrastructure roles, you must also grant device access to users that work with infrastructure. To start assigning roles, follow the steps in [Customizing infrastructure permissions for a user](#infra_access). <strong>Note</strong>: Make sure that your {{site.data.keyword.Bluemix_notm}} account is [set up with access to the IBM Cloud infrastructure (SoftLayer) portfolio](cs_troubleshoot_clusters.html#cs_credentials) so that authorized users can perform actions in the IBM Cloud infrastructure (SoftLayer) account based on the assigned permissions.</p> <p><strong>RBAC</strong>: Resource-based access control (RBAC) is a way of securing your resources that are inside of your cluster and deciding who can perform which Kubernetes actions. Every user who is assigned a platform access policy is automatically assigned a Kubernetes role. In Kubernetes, [Role Based Access Control (RBAC) ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#api-overview) determines the actions that a user can perform on the resources inside of a cluster. <strong>Note</strong>: RBAC roles are automatically set in conjunction with the platform role for the default namespace. As a cluster administrator, you can [update or assign roles](#rbac) for other namespaces.</p> <p><strong>Cloud Foundry</strong>: Not all services can be managed with Cloud IAM. If you are using one of these services, you can continue to use the [Cloud Foundry user roles](/docs/iam/cfaccess.html#cfaccess) to control access to your services. Example actions are binding a service or creating a new service instance.</p></dd>

  <dt>How can I set the permissions?</dt>
    <dd><p>When you set Platform permissions, you can assign access to a specific user, a group of users, or to the default resource group. When you set the platform permissions, RBAC roles are automatically configured for the default namespace and a RoleBinding is created.</p>
    <p><strong>Users</strong>: You might have a specific user that needs more or less permissions than the rest of your team. You can customize permissions on an individual basis so that each person has the right amount of permissions that they need to complete their task.</p>
    <p><strong>Access groups</strong>: You can create groups of users and then assign permissions to a specific group. For instance, you could group all of your team leads and give that group administrator access. While at the same time, your group of developers has only write access.</p>
    <p><strong>Resource groups</strong>: With IAM, you can create access policies for a group of resources and grant users access to this group. These resources can be part of one {{site.data.keyword.Bluemix_notm}} service or you can also group resources across service instances, such as an {{site.data.keyword.containershort_notm}} cluster and a CF app.</p> <p>**Important**: {{site.data.keyword.containershort_notm}} supports only the <code>default</code> resource group. All cluster-related resources are automatically made available in the <code>default</code> resource group. If you have other services in your {{site.data.keyword.Bluemix_notm}} account that you want to use with your cluster, the services must also be in the <code>default</code> resource group.</p></dd>
</dl>


Feeling overwhelmed? Try out this tutorial about the [best practices for organizing users, teams, and applications](/docs/tutorials/users-teams-applications.html).
{: tip}

<br />


## Accessing the IBM Cloud infrastructure (SoftLayer) portfolio
{: #api_key}

<dl>
  <dt>Why do I need access to the IBM Cloud infrastructure (SoftLayer) portfolio?</dt>
    <dd>To successfully provision and work with clusters in your account, you must ensure that your account is correctly set up to access the IBM Cloud infrastructure (SoftLayer) portfolio. Depending on your account setup, you either use the IAM API key or infrastructure credentials that you manually set by using the `ibmcloud ks credentials-set` command.</dd>

  <dt>How does the IAM API key work with the Container service?</dt>
    <dd><p>The Identity and Access Management (IAM) API key is automatically set for a region when the first action that requires the {{site.data.keyword.containershort_notm}} admin access policy is performed. For example, one of your admin users creates the first cluster in the <code>us-south</code> region. By doing that, the IAM API key for this user is stored in the account for this region. The API key is used to order IBM Cloud infrastructure (SoftLayer), such as new worker nodes or VLANs.</p> <p>When a different user performs an action in this region that requires interaction with the IBM Cloud infrastructure (SoftLayer) portfolio, such as creating a new cluster or reloading a worker node, the stored API key is used to determine whether sufficient permissions exist to perform that action. To make sure that infrastructure-related actions in your cluster can be successfully performed, assign your {{site.data.keyword.containershort_notm}} admin users the <strong>Super user</strong> infrastructure access policy.</p> <p>You can find the current API key owner by running [<code>ibmcloud ks api-key-info</code>](cs_cli_reference.html#cs_api_key_info). If you find that you need to update the API key that is stored for a region, you can do so by running the [<code>ibmcloud ks api-key-reset</code>](cs_cli_reference.html#cs_api_key_reset) command. This command requires the {{site.data.keyword.containershort_notm}} admin access policy and stores the API key of the user that executes this command in the account. The API key that is stored for the region might not be used if IBM Cloud infrastructure (SoftLayer) credentials were manually set by using the <code>ibmcloud ks credentials-set</code> command.</p> <p><strong>Note:</strong> Be sure that you want to reset the key and understand the impact to your app. The key is used in several different places and can cause breaking changes if it is unnecessarily changed.</p></dd>

  <dt>What does the <code>ibmcloud ks credentials-set</code> command do?</dt>
    <dd><p>If you have an {{site.data.keyword.Bluemix_notm}} Pay-As-You-Go account, you have access to the IBM Cloud infrastructure (SoftLayer) portfolio by default. However, you might want to use a different IBM Cloud infrastructure (SoftLayer) account that you already have to order infrastructure. You can link this infrastructure account to your {{site.data.keyword.Bluemix_notm}} account by using the [<code>ibmcloud ks credentials-set</code>](cs_cli_reference.html#cs_credentials_set) command.</p> <p>To remove IBM Cloud infrastructure (SoftLayer) credentials that were manually set, you can use the [<code>ibmcloud ks credentials-unset</code>](cs_cli_reference.html#cs_credentials_unset) command. After the credentials are removed, the IAM API key is used to order infrastructure.</p></dd>

  <dt>Is there a difference between the two?</dt>
    <dd>Both the API key and the <code>ibmcloud ks credentials-set</code> command accomplish the same task. If you manually set credentials with the <code>ibmcloud ks credentials-set</code> command, then the set credentials override any access that is granted by the API key. However, if the user whose credentials are stored does not have the required permissions to order infrastructure then infrastructure-related actions, such as creating a cluster or reloading a worker node, can fail.</dd>
</dl>


To make working with API keys a little bit easier, try creating a functional ID that you can use to set permissions.
{: tip}

<br />



## Understanding role relationships
{: #user-roles}

Before you can understand which role can perform each action, it's important to understand how the roles fit together.
{: shortdesc}

The following image shows the roles that each type of person in your organization might need. However, it is different for every organization.

![{{site.data.keyword.containershort_notm}} access roles](/images/user-policies.png)

Figure. {{site.data.keyword.containershort_notm}} access permissions by type of role

<br />



## Assigning roles with the GUI
{: #add_users}

You can add users to an {{site.data.keyword.Bluemix_notm}} account to grant access to your clusters with the GUI.
{: shortdesc}

**Before you begin**

- Verify that your user is added to the account. If not add [them](../iam/iamuserinv.html#iamuserinv).
- Verify that you are assigned the `Manager` [Cloud Foundry role](/docs/iam/mngcf.html#mngcf) for the {{site.data.keyword.Bluemix_notm}} account in which you're working.

**To assign access to a user**

1. Navigate to **Manage > Users**. A list of the users with access to the account is shown.

2. Click the name of the user that you want to set permissions for. If the user is not shown, click **Invite users** to add them to the account.

3. Assign a policy.
  * For a resource group:
    1. Select the **default** resource group. {{site.data.keyword.containershort_notm}} access can be configured for only the default resource group.
  * For a specific resource:
    1. From the **Services** list, select **{{site.data.keyword.containershort_notm}}**.
    2. From the **Region** list, select a region.
    3. From the **Service instance** list, select the cluster to invite the user to. To find the ID of a specific cluster, run `ibmcloud ks clusters`.

4. In the **Select roles** section, choose a role. 

5. Click **Assign**.

6. Assign a [Cloud Foundry role](/docs/iam/mngcf.html#mngcf).

7. Optional: Assign an [infrastructure role](/docs/iam/infrastructureaccess.html#infrapermission).

</br>

**To assign access to a group**

1. Navigate to **Manage > Access Groups**.

2. Create an access group.
  1. Click **Create** and give your group a **Name** and **Description**. Click **Create**.
  2. Click **Add users** to add people to your access group. A list of users that have access to your account is shown.
  3. Check the box next to the users that you want to add to the group. A dialog box displays.
  4. Click **Add to group**.

3. To assign access to a specific service, add a service ID.
  1. Click **Add service ID**.
  2. Check the box next to the users that you want to add to the group. A pop up displays.
  3. Click **Add to group**.

4. Assign access policies. Don't forget to double check the people that you add to your group. Everyone in the group is provided the same level of access.
    * For a resource group:
        1. Select the **default** resource group. {{site.data.keyword.containershort_notm}} access can be configured for only the default resource group.
    * For a specific resource:
        1. From the **Services** list, select **{{site.data.keyword.containershort_notm}}**.
        2. From the **Region** list, select a region.
        3. From the **Service instance** list, select the cluster to invite the user to. To find the ID of a specific cluster, run `ibmcloud ks clusters`.

5. In the **Select roles** section, choose a role. 

6. Click **Assign**.

7. Assign a [Cloud Foundry role](/docs/iam/mngcf.html#mngcf).

8. Optional: Assign an [infrastructure role](/docs/iam/infrastructureaccess.html#infrapermission).

<br />






## Authorizing users with custom Kubernetes RBAC roles
{: #rbac}

{{site.data.keyword.containershort_notm}} access policies correspond with certain Kubernetes role-based access control (RBAC) roles. To authorize other Kubernetes roles that differ from the corresponding access policy, you can customize RBAC roles and then assign the roles to individuals or groups of users.
{: shortdesc}

There are times that you might need access policies to be more granular than an IAM policy might allow. No problem! You can assign access policies for specific Kubernetes resources for users or for users. You can create a role and then bind the role to specific users or a group. For more information, see [Using RBAC Authorization ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#api-overview) in the Kubernetes documentation.

When a binding is created for a group, it affects any user that is added or removed from that group. If you add a user to the group, then they also have the additional access. If they are removed, their access is revoked.
{: tip}

If you want to assign access to a service such as for a continuous integration, continuous delivery pipeline, you can use [Kubernetes Service Accounts ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/reference/access-authn-authz/service-accounts-admin/).

**Before you begin**

- Target the [Kubernetes CLI](cs_cli_install.html#cs_cli_configure) to your cluster.
- Ensure that the user or group has a minimum of `Viewer` access at the service level.


**To customize RBAC roles**

1.  Create the role with the access that you want to assign.

    1. Make a `.yaml` file to define the role with the access that you want to assign.

        ```
        kind: Role
        apiVersion: rbac.authorization.k8s.io/v1
        metadata:
          namespace: default
          name: my_role
        rules:
        - apiGroups: [""]
          resources: ["pods"]
          verbs: ["get", "watch", "list"]
        - apiGroups: ["apps", "extensions"]
          resources: ["daemonsets", "deployments"]
          verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
        ```
        {: codeblock}

        <table>
        <caption>Understanding this YAML components</caption>
          <thead>
            <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding this YAML components</th>
          </thead>
          <tbody>
            <tr>
              <td><code>kind</code></td>
              <td>Use `Role` to grant access to resources within a single namespace, or `ClusterRole` for resources cluster-wide.</td>
            </tr>
            <tr>
              <td><code>apiVersion</code></td>
              <td><ul><li>For clusters that run Kubernetes 1.8 or later, use `rbac.authorization.k8s.io/v1`. </li><li>For earlier versions, use `apiVersion: rbac.authorization.k8s.io/v1beta1`.</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/namespace</code></td>
              <td><ul><li>For `Role` kind: Specify the Kubernetes namespace to which access is granted.</li><li>Do not use the `namespace` field if you are creating a `ClusterRole` that applies at the cluster-level.</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/name</code></td>
              <td>Name the role and use the name later when you bind the role.</td>
            </tr>
            <tr>
              <td><code>rules/apiGroups</code></td>
              <td><ul><li>Specify the Kubernetes API groups that you want users to be able to interact with, such as `"apps"`, `"batch"`, or `"extensions"`. </li><li>For access to the core API group at REST path `api/v1`, leave the group blank: `[""]`.</li><li>For more information, see [API groups![External link icon](../icons/launch-glyph.svg "External link icon")](https://v1-9.docs.kubernetes.io/docs/reference/api-overview/#api-groups) in the Kubernetes documentation.</li></ul></td>
            </tr>
            <tr>
              <td><code>rules/resources</code></td>
              <td><ul><li>Specify the Kubernetes resources to which you want to grant access, such as `"daemonsets"`, `"deployments"`, `"events"`, or `"ingresses"`.</li><li>If you specify `"nodes"`, then the role kind must be `ClusterRole`.</li><li>For a list of resources, see the table of [Resource types![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/reference/kubectl/cheatsheet/) in the Kubernetes cheat sheet.</li></ul></td>
            </tr>
            <tr>
              <td><code>rules/verbs</code></td>
              <td><ul><li>Specify the types of actions that you want users to be able to do, such as `"get"`, `"list"`, `"describe"`, `"create"`, or `"delete"`. </li><li>For a full list of verbs, see the [`kubectl` documentation![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/reference/kubectl/overview/).</li></ul></td>
            </tr>
          </tbody>
        </table>

    2.  Create the role in your cluster.

        ```
        kubectl apply -f <path_to_yaml_file>
        ```
        {: pre}

    3.  Verify that the role is created.

        ```
        kubectl get roles
        ```
        {: pre}

2.  Bind users to the role.

    1. Make a `.yaml` file to bind users to your role. Note the unique URL to use for each subject's name.

        ```
        kind: RoleBinding
        apiVersion: rbac.authorization.k8s.io/v1
        metadata:
          name: my_role_team1
          namespace: default
        subjects:
        - kind: User
          name: https://iam.ng.bluemix.net/kubernetes#user1@example.com
          apiGroup: rbac.authorization.k8s.io
        - kind: User
          name: https://iam.ng.bluemix.net/kubernetes#user2@example.com
          apiGroup: rbac.authorization.k8s.io
        - kind: User
          name: system:serviceaccount:<namespace>:<service_account_name>
          apiGroup: rbac.authorization.k8s.io
        roleRef:
          kind: Role
          name: custom-rbac-test
          apiGroup: rbac.authorization.k8s.io
        ```
        {: codeblock}

        <table>
        <caption>Understanding this YAML components</caption>
          <thead>
            <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding this YAML components</th>
          </thead>
          <tbody>
            <tr>
              <td><code>kind</code></td>
              <td>Specify the `kind` as `RoleBinding` for both types of role `.yaml` files: namespace `Role` and cluster-wide `ClusterRole`.</td>
            </tr>
            <tr>
              <td><code>apiVersion</code></td>
              <td><ul><li>For clusters that run Kubernetes 1.8 or later, use `rbac.authorization.k8s.io/v1`. </li><li>For earlier versions, use `apiVersion: rbac.authorization.k8s.io/v1beta1`.</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/namespace</code></td>
              <td><ul><li>For `Role` kind: Specify the Kubernetes namespace to which access is granted.</li><li>Do not use the `namespace` field if you are creating a `ClusterRole` that applies at the cluster-level.</li></ul></td>
            </tr>
            <tr>
              <td><code>metadata/name</code></td>
              <td>Name the role binding.</td>
            </tr>
            <tr>
              <td><code>subjects/kind</code></td>
              <td>Specify the kind as `User`.</td>
            </tr>
            <tr>
              <td><code>subjects/name</code></td>
              <td><ul><li>**For individual users**: Append the user's email address to the following URL: `https://iam.ng.bluemix.net/kubernetes#`. For example, `https://iam.ng.bluemix.net/kubernetes#user1@example.com`</li>
              <li>**For service accounts**: Specify the namespace and service name. For example: `system:serviceaccount:<namespace>:<service_account_name>`</li></ul></td>
            </tr>
            <tr>
              <td><code>subjects/apiGroup</code></td>
              <td>Use `rbac.authorization.k8s.io`.</td>
            </tr>
            <tr>
              <td><code>roleRef/kind</code></td>
              <td>Enter the same value as the `kind` in the role `.yaml` file: `Role` or `ClusterRole`.</td>
            </tr>
            <tr>
              <td><code>roleRef/name</code></td>
              <td>Enter the name of the role `.yaml` file.</td>
            </tr>
            <tr>
              <td><code>roleRef/apiGroup</code></td>
              <td>Use `rbac.authorization.k8s.io`.</td>
            </tr>
          </tbody>
        </table>

    2. Create the role binding resource in your cluster.

        ```
        kubectl apply -f filepath/my_role_team1.yaml
        ```
        {: pre}

    3.  Verify that the binding is created.

        ```
        kubectl get rolebinding
        ```
        {: pre}

Now that you created and bound a custom Kubernetes RBAC role, follow up with users. Ask them to test an action that they have permission to complete due to the role, such as deleting a pod.


<br />



## Customizing infrastructure permissions for a user
{: #infra_access}

When you set infrastructure policies in Identity and Access Management, a user is given permissions that are associated with a role. Some policies are pre-defined, but others can be customized. To customize those permissions, you must log in to IBM Cloud infrastructure (SoftLayer) and adjust the permissions there.
{: #view_access}

For example, **Basic Users** can reboot a worker node, but they cannot reload a worker node. Without giving that person **Super User** permissions, you can adjust the IBM Cloud infrastructure (SoftLayer) permissions and add the permission to run a reload command.

If you have multizone clusters, your IBM Cloud infrastructure (SoftLayer) account owner needs to turn on VLAN spanning so that the nodes in different zones can communicate within the cluster. The account owner can also assign a user the **Network > Manage Network VLAN Spanning** permission so that the user can enable VLAN spanning.
{: tip}


1.  Log in to your [{{site.data.keyword.Bluemix_notm}} account](https://console.bluemix.net/), then from the menu, select **Infrastructure**.

2.  Go to **Account** > **Users** > **User List**.

3.  To modify permissions, select a user profile's name or the **Device Access** column.

4.  In the **Portal Permissions** tab, customize the user's access. The permissions that users need depend on what infrastructure resources they need to use:

    * Use the **Quick Permissions** drop-down list to assign the **Super User** role, which gives the user all permissions.
    * Use the **Quick Permissions** drop-down list to assign the **Basic User** role, which gives the user some, but not all, needed permissions.
    * If you don't want to grant all permissions with the **Super User** role or need to add permissions beyond the **Basic User** role, review the following table that describes permissions that are needed to perform common tasks in {{site.data.keyword.containershort_notm}}.

    <table summary="Infrastructure permissions for common {{site.data.keyword.containershort_notm}} scenarios.">
     <caption>Commonly required infrastructure permissions for {{site.data.keyword.containershort_notm}}</caption>
     <thead>
      <th>Common tasks in {{site.data.keyword.containershort_notm}}</th>
      <th>Required infrastructure permissions by tab</th>
     </thead>
     <tbody>
       <tr>
         <td><strong>Minimum permissions</strong>: <ul><li>Create a cluster.</li></ul></td>
         <td><strong>Devices</strong>:<ul><li>View Virtual Server Details</li><li>Reboot server and view IPMI system information</li><li>Issue OS Reloads and Initiate Rescue Kernel</li></ul><strong>Account</strong>: <ul><li>Add/Upgrade Cloud Instances</li><li>Add Server</li></ul></td>
       </tr>
       <tr>
         <td><strong>Cluster Administration</strong>: <ul><li>Create, update, and delete clusters.</li><li>Add, reload, and reboot worker nodes.</li><li>View VLANs.</li><li>Create subnets.</li><li>Deploy pods and load balancer services.</li></ul></td>
         <td><strong>Support</strong>:<ul><li>View Tickets</li><li>Add Tickets</li><li>Edit Tickets</li></ul>
         <strong>Devices</strong>:<ul><li>View Virtual Server Details</li><li>Reboot server and view IPMI system information</li><li>Upgrade Server</li><li>Issue OS Reloads and Initiate Rescue Kernel</li></ul>
         <strong>Services</strong>:<ul><li>Manage SSH Keys</li></ul>
         <strong>Account</strong>:<ul><li>View Account Summary</li><li>Add/Upgrade Cloud Instances</li><li>Cancel Server</li><li>Add Server</li></ul></td>
       </tr>
       <tr>
         <td><strong>Storage</strong>: <ul><li>Create persistent volume claims to provision persistent volumes.</li><li>Create and manage storage infrastructure resources.</li></ul></td>
         <td><strong>Services</strong>:<ul><li>Manage Storage</li></ul><strong>Account</strong>:<ul><li>Add Storage</li></ul></td>
       </tr>
       <tr>
         <td><strong>Private Networking</strong>: <ul><li>Manage private VLANs for in-cluster networking.</li><li>Set up VPN connectivity to private networks.</li></ul></td>
         <td><strong>Network</strong>:<ul><li>Manage Network Subnet Routes</li><li>Manage IPSEC Network Tunnels</li><li>Manage Network Gateways</li><li>VPN Administration</li></ul></td>
       </tr>
       <tr>
         <td><strong>Public Networking</strong>:<ul><li>Set up public load balancer or Ingress networking to expose apps.</li></ul></td>
         <td><strong>Devices</strong>:<ul><li>Manage Load Balancers</li><li>Edit Hostname/Domain</li><li>Manage Port Control</li></ul>
         <strong>Network</strong>:<ul><li>Add Compute with Public Network Port</li><li>Manage Network Subnet Routes</li><li>Add IP Addresses</li></ul>
         <strong>Services</strong>:<ul><li>Manage DNS, Reverse DNS, and WHOIS</li><li>View Certificates (SSL)</li><li>Manage Certificates (SSL)</li></ul></td>
       </tr>
     </tbody>
    </table>

5.  To save your changes, click **Edit Portal Permissions**.

6.  In the **Device Access** tab, select the devices to grant access to.

    * In the **Device Type** drop-down list, you can grant access to **All Virtual Servers**.
    * To allow users access to new devices that are created, select **Automatically grant access when new devices are added**.
    * To save your changes, click **Update Device Access**.

Downgrading permissions? It can take a few minutes for the action to complete.
{: tip}

<br />

