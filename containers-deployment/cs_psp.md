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

# Configuring pod security policies
{: #psp}

With [pod security policies ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/policy/pod-security-policy/), you can
configure policies to authorize who can create and update pods in {{site.data.keyword.containerlong}}. Clusters that run Kubernetes versions 1.10.3, 1.9.8, and 1.8.13 or later fixpacks support the `PodSecurityPolicy` admission controller that enforces these policies. 
{: shortdesc}

Using an older version of Kubernetes? [Update your cluster](cs_cluster_update.html) today.
{: tip}

**Why do I set pod security policies?**</br>
As a cluster admin, you want to control what happens in your cluster, especially actions that affect the cluster's security or readiness. Pod security policies can help you control usage of privileged containers, root namespaces, host networking and ports, volume types, host file systems, Linux permissions such as read-only or group IDs, and more.

With the the `PodSecurityPolicy` admission controller, no pods can be created until after you [authorize policies](#customize_psp). Setting up pod security policies can have unintended side-effects, so make sure to test out a deployment after you change the policy. To deploy apps, the user and service accounts must all be authorized by the pod security policies that are required to deploy pods. For example, if you install apps by using [Helm](cs_integrations.html#helm_links), the Helm tiller component creates pods, and so you must have the correct pod security policy authorization.

Trying to control which users have access to the {{site.data.keyword.containershort_notm}}? See [Assigning cluster access](cs_users.html#users) to set IAM and infrastructure permissions.
{: tip}

**Are any policies set by default? What can I add?**</br>
By default, {{site.data.keyword.containershort_notm}} configures the `PodSecurityPolicy` admission controller with [resources for {{site.data.keyword.IBM_notm}} cluster management](#ibm_psp) that you cannot delete or modify. You also cannot disable the admission controller. 

Pod actions are not locked down by default. Instead, two role-based access control (RBAC) resources in the cluster authorize all admins, users, services, and nodes to create privileged and unprivileged pods. If you want to prevent certain users from creating or updating pods, you can [modify these RBAC resources or create your own](#customize_psp).

**How does policy authorization work?**</br>
When you as a user create a pod directly and not by using a controller such as a deployment, your credentials are validated against the pod security policies that you are authorized to use. If no policy supports the pod security requirements, the pod is not created.

When you create a pod by using a resource controller such as a deployment, Kubernetes validates the pod's service account credentials against the pod security policies that the service account is authorized to use. If no policy supports the pod security requirements, the controller succeeds, but the pod is not created.

For common error messages, see [Pods fail to deploy because of a pod security policy](cs_troubleshoot_clusters.html#cs_psp).

## Customizing pod security policies
{: #customize_psp}

To prevent unauthorized pod actions, you can modify existing pod security policy resources or create your own. You must be a cluster admin to customize policies.
{: shortdesc}

**What existing policies can I modify?**</br>
By default, your cluster contains the following RBAC resources that enable cluster
administrators, authenticated users, service accounts, and nodes to use the
`ibm-privileged-psp` and `ibm-restricted-psp` pod security policies. These
policies allow the users to create and update privileged and unprivileged (restricted) pods.

| Name | Namespace | Type | Purpose |
|---|---|---|---|
| `privileged-psp-user` | cluster-wide | `ClusterRoleBinding` | Enables cluster administrators, authenticated users, service accounts, and nodes to use `ibm-privileged-psp` pod security policy. |
| `restricted-psp-user` | cluster-wide | `ClusterRoleBinding` | Enables cluster administrators, authenticated users, service accounts, and nodes to use `ibm-restricted-psp` pod security policy. |
{: caption="Default RBAC resources that you can modify" caption-side="top"}

You can modify these RBAC roles to remove or add admins, users, services, or nodes to the policy.

Before you begin: 
*  [Target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster.
*  Understand working with RBAC roles. For more information, see [Authorizing users with custom Kubernetes RBAC roles](cs_users.html#rbac) or the [Kubernetes documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#api-overview).
*  **Note**: When you modify the default configuration, you can prevent important cluster actions, such as pod deployments or cluster updates. Test your changes in a non-production cluster that other teams do not rely on.

**To modify the RBAC resources**:
1.  Get the name of the RBAC cluster role binding.
    ```
    kubectl get clusterrolebinding
    ```
    {: pre}
    
2.  Download the cluster role binding as a `.yaml` file that you can edit locally.
    
    ```
    kubectl get clusterrolebinding privileged-psp-user -o yaml > privileged-psp-user.yaml
    ```
    {: pre}
    
    You might want to save a copy of the existing policy so that you can revert to it if the modified policy yields unexpected results.
    {: tip}
    
    **Example cluster role binding file**:
    
    ```yaml
    apiVersion: rbac.authorization.k8s.io/v1
    kind: ClusterRoleBinding
    metadata:
      creationTimestamp: 2018-06-20T21:19:50Z
      name: privileged-psp-user
      resourceVersion: "1234567"
      selfLink: /apis/rbac.authorization.k8s.io/v1/clusterrolebindings/privileged-psp-user
      uid: aa1a1111-11aa-11a1-aaaa-1a1111aa11a1
    roleRef:
      apiGroup: rbac.authorization.k8s.io
      kind: ClusterRole
      name: ibm-privileged-psp-user
    subjects:
    - apiGroup: rbac.authorization.k8s.io
      kind: Group
      name: system:masters
    - apiGroup: rbac.authorization.k8s.io
      kind: Group
      name: system:nodes
    - apiGroup: rbac.authorization.k8s.io
      kind: Group
      name: system:serviceaccounts
    - apiGroup: rbac.authorization.k8s.io
      kind: Group
      name: system:authenticated
    ```
    {: codeblock}
    
3.  Edit the cluster role binding `.yaml` file. To understand what you can edit, review the [Kubernetes documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/policy/pod-security-policy/). Example actions:
    
    *   **Service accounts**: You might want to authorize service accounts so that deployments can occur only in specific namespaces. For example, if you scope the policy to allow actions within the `kube-system` namespace, many important actions such as cluster updates can occur. However, actions in others namespaces are no longer authorized. 
    
        To scope the policy to allow actions in a specific namespace, change the `system:serviceaccounts` to `system:serviceaccount:<namespace>`.
        ```yaml
        - apiGroup: rbac.authorization.k8s.io
          kind: Group
          name: system:serviceaccount:kube-system
        ```
        {: codeblock}
  
    *   **Users**: You might want to remove authorization for all authenticated users to deploy pods with privileged access. Remove the following `system:authenticated` entry.
        ```yaml
        - apiGroup: rbac.authorization.k8s.io
          kind: Group
          name: system:authenticated
        ```
        {: codeblock}

4.  Create the modified cluster role binding resource in your cluster.

    ```
    kubectl apply -f privileged-psp-user.yaml
    ```
    {: pre}
    
5.  Verify that the resource was modified.

    ```
    kubectl get clusterrolebinding privileged-psp-user -o yaml
    ```
    {: pre}

</br>
**To delete the RBAC resources**:
1.  Get the name of the RBAC cluster role binding.
    ```
    kubectl get clusterrolebinding
    ```
    {: pre}

2.  Delete the RBAC role that you want to remove.
    ```
    kubectl delete clusterrolebinding privileged-psp-user
    ```
    {: pre}
    
3.  Verify that the RBAC cluster role binding is no longer in your cluster.
    ```
    kubectl get clusterrolebinding
    ```
    {: pre}

</br>
**To create your own pod security policy**:</br>
To create your own pod security policy resource and authorize users with RBAC, review the [Kubernetes documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/policy/pod-security-policy/). 

Make sure that you modified the existing policies so that the new policy that you create does not conflict with the existing policy. For example, the existing policy permits users to create and update privileged pods. If you create a policy that does not permit users to create or update privileged pods, the conflict between the existing and the new policy might cause unexpected results.

## Understanding default resources for {{site.data.keyword.IBM_notm}} cluster management
{: #ibm_psp}

Your Kubernetes cluster in {{site.data.keyword.containershort_notm}} contains the following
pod security policies and related RBAC resources to allow {{site.data.keyword.IBM_notm}} to properly manage your cluster.
{: shortdesc}

The default `privileged-psp-user` and `restricted-psp-user` RBAC resources refer to the pod security policies that are set by {{site.data.keyword.IBM_notm}}. 

**Attention**: You must not delete or modify these resources.

| Name | Namespace | Type | Purpose |
|---|---|---|---|
| `ibm-privileged-psp` | cluster-wide | `PodSecurityPolicy` | Policy for privileged pod creation. |
| `ibm-privileged-psp-user` | cluster-wide | `ClusterRole` | Cluster role that allows the use of `ibm-privileged-psp` pod security policy. |
| `ibm-privileged-psp-user` | `kube-system` | `RoleBinding` | Enables cluster administrators, service accounts, and nodes to use `ibm-privileged-psp` pod security policy in the `kube-system` namespace. |
| `ibm-privileged-psp-user` | `ibm-system` | `RoleBinding` | Enables cluster administrators, service accounts, and nodes to use `ibm-privileged-psp` pod security policy in the `ibm-system` namespace. |
| `ibm-privileged-psp-user` | `kubx-cit` | `RoleBinding` | Enables cluster administrators, service accounts, and nodes to use `ibm-privileged-psp` pod security policy in the `kubx-cit` namespace. |
| `ibm-restricted-psp` | cluster-wide | `PodSecurityPolicy` | Policy for unprivileged, or restricted, pod creation. |
| `ibm-restricted-psp-user` | cluster-wide | `ClusterRole` | Cluster role that allows the use of `ibm-restricted-psp` pod security policy. |
{: caption="IBM pod security policies resources that you must not modify" caption-side="top"}
