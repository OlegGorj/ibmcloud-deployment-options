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




# Building containers from images
{: #images}

A Docker image is the basis for every container that you create with {{site.data.keyword.containerlong}}.
{:shortdesc}

An image is created from a Dockerfile, which is a file that contains instructions to build the image. A Dockerfile might reference build artifacts in its instructions that are stored separately, such as an app, the app's configuration, and its dependencies.

## Planning image registries
{: #planning}

Images are typically stored in a registry that can either be accessible by the public (public registry) or set up with limited access for a small group of users (private registry).
{:shortdesc}

Public registries, such as Docker Hub, can be used to get started with Docker and Kubernetes to create your first containerized app in a cluster. But when it comes to enterprise applications, use a private registry, like the one provided in {{site.data.keyword.registryshort_notm}} to protect your images from being used and changed by unauthorized users. Private registries must be set up by the cluster admin to ensure that the credentials to access the private registry are available to the cluster users.


You can use multiple registries with {{site.data.keyword.containershort_notm}} to deploy apps to your cluster.

|Registry|Description|Benefit|
|--------|-----------|-------|
|[{{site.data.keyword.registryshort_notm}}](/docs/services/Registry/index.html)|With this option, you can set up your own secured Docker image repository in {{site.data.keyword.registryshort_notm}} where you can safely store and share images between cluster users.|<ul><li>Manage access to images in your account.</li><li>Use {{site.data.keyword.IBM_notm}} provided images and sample apps, such as {{site.data.keyword.IBM_notm}} Liberty, as a parent image and add your own app code to it.</li><li>Automatic scanning of images for potential vulnerabilities by Vulnerability Advisor, including OS specific recommendations to fix them.</li></ul>|
|Any other private registry|Connect any existing private registry to your cluster by creating an [imagePullSecret ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/containers/images/). The secret is used to securely save your registry URL and credentials in a Kubernetes secret.|<ul><li>Use existing private registries independent of their source (Docker Hub, organization owned registries, or other private Cloud registries).</li></ul>|
|[Public Docker Hub![External link icon](../icons/launch-glyph.svg "External link icon")](https://hub.docker.com/){: #dockerhub}|Use this option to directly use existing public images from Docker Hub in your [Kubernetes deployment![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) when no Dockerfile changes are needed. <p>**Note:** Keep in mind that this option might not meet your organization's security requirements, like access management, vulnerability scanning, or app privacy.</p>|<ul><li>No additional setup is needed for your cluster.</li><li>Includes a variety of open-source applications.</li></ul>|
{: caption="Public and private image registry options" caption-side="top"}

After you set up an image registry, cluster users can use the images for their app deployments to the cluster.

Learn more about [securing your personal information](cs_secure.html#pi) when you work with container images.

<br />


## Setting up trusted content for container images
{: #trusted_images}

You can build containers from trusted images that are signed and stored in {{site.data.keyword.registryshort_notm}}, and prevent deployments from unsigned or vulnerable images.
{:shortdesc}

1.  [Sign images for trusted content](/docs/services/Registry/registry_trusted_content.html#registry_trustedcontent). After you set up trust for your images, you can manage trusted content and signers that can push images to your registry.
2.  To enforce a policy that only signed images can be used to build containers in your cluster, [add Container Image Security Enforcement (beta)](/docs/services/Registry/registry_security_enforce.html#security_enforce).
3.  Deploy your app.
    1. [Deploy to the `default` Kubernetes namespace](#namespace).
    2. [Deploy to a different Kubernetes namespace, or from a different {{site.data.keyword.Bluemix_notm}} region or account](#other).

<br />


## Deploying containers from an {{site.data.keyword.registryshort_notm}} image to the `default` Kubernetes namespace
{: #namespace}

You can deploy containers to your cluster from an IBM-provided public image or a private image that is stored in your namespace in {{site.data.keyword.registryshort_notm}}.
{:shortdesc}

When you create a cluster, non-expiring registry tokens and secrets are automatically created for both the [nearest regional registry and the global registry](/docs/services/Registry/registry_overview.html#registry_regions). The global registry securely stores public, IBM-provided images that you can refer to across your deployments instead of having different references for images that are stored in each regional registry. The regional registry securely stores your own private Docker images, as well as the same public images that are stored in the global registry. The tokens are used to authorize read-only access to any of your namespaces that you set up in {{site.data.keyword.registryshort_notm}} so that you can work with these public (global registry) and private (regional registries) images.

Each token must be stored in a Kubernetes `imagePullSecret` so that it is accessible to a Kubernetes cluster when you deploy a containerized app. When your cluster is created, {{site.data.keyword.containershort_notm}} automatically stores the tokens for the global (IBM-provided public images) and regional registries in Kubernetes image pull secrets. The image pull secrets are added to the `default` Kubernetes namespace, the default list of secrets in the `ServiceAccount` for that namespace, and the `kube-system` namespace.

**Note:** By using this initial setup, you can deploy containers from any image that is available in a namespace in your {{site.data.keyword.Bluemix_notm}} account into the **default** namespace of your cluster. To deploy a container into other namespaces of your cluster, or to use an image that is stored in another {{site.data.keyword.Bluemix_notm}} region or in another {{site.data.keyword.Bluemix_notm}} account, you must [create your own imagePullSecret for your cluster](#other).

Before you begin:
1. [Set up a namespace in {{site.data.keyword.registryshort_notm}} on {{site.data.keyword.Bluemix_notm}} Public or {{site.data.keyword.Bluemix_dedicated_notm}} and push images to this namespace](/docs/services/Registry/registry_setup_cli_namespace.html#registry_namespace_add).
2. [Create a cluster](cs_clusters.html#clusters_cli).
3. [Target your CLI to your cluster](cs_cli_install.html#cs_cli_configure).

To deploy a container into the **default** namespace of your cluster, create a configuration file.

1.  Create a deployment configuration file that is named `mydeployment.yaml`.
2.  Define the deployment and the image that you want to use from your namespace in {{site.data.keyword.registryshort_notm}}.

    To use a private image from a namespace in {{site.data.keyword.registryshort_notm}}:

    ```
    apiVersion: apps/v1beta1
    kind: Deployment
    metadata:
      name: ibmliberty-deployment
    spec:
      replicas: 3
      template:
        metadata:
          labels:
            app: ibmliberty
        spec:
          containers:
          - name: ibmliberty
            image: registry.<region>.bluemix.net/<namespace>/<my_image>:<tag>
    ```
    {: codeblock}

    **Tip:** To retrieve your namespace information, run `ibmcloud cr namespace-list`.

3.  Create the deployment in your cluster.

    ```
    kubectl apply -f mydeployment.yaml
    ```
    {: pre}

    **Tip:** You can also deploy an existing configuration file, such as one of the IBM-provided public images. This example uses the **ibmliberty** image in the US-South region.

    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/deploy-ibmliberty.yaml
    ```
    {: pre}


<br />



## Creating an `imagePullSecret` to access {{site.data.keyword.Bluemix_notm}} or external private registries in other Kubernetes namespaces, {{site.data.keyword.Bluemix_notm}} regions, and accounts
{: #other}

Create your own `imagePullSecret` to deploy containers to other Kubernetes namespaces, use images that are stored in other {{site.data.keyword.Bluemix_notm}} regions or accounts, use images that are stored in {{site.data.keyword.Bluemix_dedicated_notm}}, or use images that are stored in external private registries.
{:shortdesc}

ImagePullSecrets are valid only for the Kubernetes namespaces that they were created for. Repeat these steps for every namespace where you want to deploy containers. Images from [DockerHub](#dockerhub) do not require ImagePullSecrets.
{: tip}

Before you begin:

1.  [Set up a namespace in {{site.data.keyword.registryshort_notm}} on {{site.data.keyword.Bluemix_notm}} Public or {{site.data.keyword.Bluemix_dedicated_notm}} and push images to this namespace](/docs/services/Registry/registry_setup_cli_namespace.html#registry_namespace_add).
2.  [Create a cluster](cs_clusters.html#clusters_cli).
3.  [Target your CLI to your cluster](cs_cli_install.html#cs_cli_configure).

<br/>
To create your own imagePullSecret you can choose among the following options:
- [Copy the imagePullSecret from the default namespace to other namespaces in your cluster](#copy_imagePullSecret).
- [Create an imagePullSecret to access images in other {{site.data.keyword.Bluemix_notm}} regions and accounts](#other_regions_accounts).
- [Create an imagePullSecret to access images in external private registries](#private_images).

<br/>
If you already created an imagePullSecret in your namespace that you want to use in your deployment, see [Deploying containers by using the created imagePullSecret](#use_imagePullSecret).

### Copying the imagePullSecret from the default namespace to other namespaces in your cluster
{: #copy_imagePullSecret}

You can copy the imagePullSecret that is automatically created for the `default` Kubernetes namespace to other namespaces in your cluster.
{: shortdesc}

1. List available namespaces in your cluster.
   ```
   kubectl get namespaces
   ```
   {: pre}

   Example output:
   ```
   default          Active    79d
   ibm-cert-store   Active    79d
   ibm-system       Active    79d
   istio-system     Active    34d
   kube-public      Active    79d
   kube-system      Active    79d
   ```
   {: screen}

2. Optional: Create a namespace in your cluster.
   ```
   kubectl create namespace <namespace_name>
   ```
   {: pre}

3. Copy the imagePullSecrets from the `default` namespace to the namespace of your choice. The new imagePullSecrets are named `bluemix-<namespace_name>-secret-regional` and `bluemix-<namespace_name>-secret-international`.
   ```
   kubectl get secret bluemix-default-secret-regional -o yaml | sed 's/default/<namespace_name>/g' | kubectl -n <namespace_name> create -f -
   ```
   {: pre}
   
   ```
   kubectl get secret bluemix-default-secret-international -o yaml | sed 's/default/<namespace_name>/g' | kubectl -n <namespace_name> create -f -
   ```
   {: pre}

4.  Verify that the secret was created successfully.
    ```
    kubectl get secrets --namespace <namespace_name>
    ```
    {: pre}

5. [Deploy a container by using the imagePullSecret](#use_imagePullSecret) in your namespace.


### Creating an imagePullSecret to access images in other {{site.data.keyword.Bluemix_notm}} regions and accounts
{: #other_regions_accounts}

To access images in other {{site.data.keyword.Bluemix_notm}} regions or accounts, you must create a registry token and save your credentials in an imagePullSecret.
{: shortdesc}

1.  If you do not have a token, [create a token for the registry that you want to access.](/docs/services/Registry/registry_tokens.html#registry_tokens_create)
2.  List tokens in your {{site.data.keyword.Bluemix_notm}} account.

    ```
    ibmcloud cr token-list
    ```
    {: pre}

3.  Note the token ID that you want to use.
4.  Retrieve the value for your token. Replace <em>&lt;token_ID&gt;</em> with the ID of the token that you retrieved in the previous step.

    ```
    ibmcloud cr token-get <token_id>
    ```
    {: pre}

    Your token value is displayed in the **Token** field of your CLI output.

5.  Create the Kubernetes secret to store your token information.

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_URL> --docker-username=token --docker-password=<token_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>Understanding this command's components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding this command's components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>Required. The Kubernetes namespace of your cluster where you want to use the secret and deploy containers to. Run <code>kubectl get namespaces</code> to list all namespaces in your cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>Required. The name that you want to use for your imagePullSecret.</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>Required. The URL to the image registry where your namespace is set up.<ul><li>For namespaces that are set up in US-South and US-East registry.ng.bluemix.net</li><li>For namespaces that are set up in UK-South registry.eu-gb.bluemix.net</li><li>For namespaces that are set up in EU-Central (Frankfurt) registry.eu-de.bluemix.net</li><li>For namespaces that are set up in Australia (Sydney) registry.au-syd.bluemix.net</li><li>For namespaces that are set up in {{site.data.keyword.Bluemix_dedicated_notm}} registry.<em>&lt;dedicated_domain&gt;</em></li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>Required. The user name to log in to your private registry. For {{site.data.keyword.registryshort_notm}}, the user name is set to the value <strong><code>token</code></strong>.</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>Required. The value of your registry token that you retrieved earlier.</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>Required. If you have one, enter your Docker email address. If you do not, enter a fictional email address, as for example a@b.c. This email is mandatory to create a Kubernetes secret, but is not used after creation.</td>
    </tr>
    </tbody></table>

6.  Verify that the secret was created successfully. Replace <em>&lt;kubernetes_namespace&gt;</em> with the namespace where you created the imagePullSecret.

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

7.  [Deploy a container by using the imagePullSecret](#use_imagePullSecret) in your namespace.

### Accessing images that are stored in other private registries
{: #private_images}

If you already have a private registry, you must store the registry credentials in a Kubernetes imagePullSecret and reference this secret from your configuration file.
{:shortdesc}

Before you begin:

1.  [Create a cluster](cs_clusters.html#clusters_cli).
2.  [Target your CLI to your cluster](cs_cli_install.html#cs_cli_configure).

To create an imagePullSecret:

1.  Create the Kubernetes secret to store your private registry credentials.

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_URL> --docker-username=<docker_username> --docker-password=<docker_password> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>Understanding this command's components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding this command's components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>Required. The Kubernetes namespace of your cluster where you want to use the secret and deploy containers to. Run <code>kubectl get namespaces</code> to list all namespaces in your cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>Required. The name that you want to use for your imagePullSecret.</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>Required. The URL to the registry where your private images are stored.</td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>Required. The user name to log in to your private registry.</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>Required. The value of your registry token that you retrieved earlier.</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>Required. If you have one, enter your Docker email address. If you do not have one, enter a fictional email address, as for example a@b.c. This email is mandatory to create a Kubernetes secret, but is not used after creation.</td>
    </tr>
    </tbody></table>

2.  Verify that the secret was created successfully. Replace <em>&lt;kubernetes_namespace&gt;</em> with the name of the namespace where you created the imagePullSecret.

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

3.  [Create a pod that references the imagePullSecret](#use_imagePullSecret).

## Deploying containers by using the created imagePullSecret
{: #use_imagePullSecret}

You can define an imagePullSecret in your pod deployment or store the imagePullSecret in your Kubernetes service account so that it is available for all deployments that do not specify a service account.
{: shortdesc}

Choose between the following options:
* [Referring to the imagePullSecret in your pod deployment](#pod_imagePullSecret): Use this option if you do not want to grant access to your registry for all pods in your namespace by default.
* [Storing the imagePullSecret in the Kubernetes service account](#store_imagePullSecret): Use this option to grant access to images in your registry for deployments in the selected Kubernetes namespaces.

Before you begin:
* [Create an imagePullSecret](#other) to access images in other registries, Kubernetes namespaces, {{site.data.keyword.Bluemix_notm}} regions, or accounts.
* [Target your CLI to your cluster](cs_cli_install.html#cs_cli_configure).

### Referring to the `imagePullSecret` in your pod deployment
{: #pod_imagePullSecret}

When you refer to the imagePullSecret in a pod deployment, the imagePullSecret is valid for this pod only and cannot be shared across pods in the namespace.
{:shortdesc}

1.  Create a pod configuration file that is named `mypod.yaml`.
2.  Define the pod and the imagePullSecret to access the private {{site.data.keyword.registrylong_notm}}.

    To access a private image:
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod
    spec:
      containers:
        - name: <container_name>
          image: registry.<region>.bluemix.net/<namespace_name>/<image_name>:<tag>
      imagePullSecrets:
        - name: <secret_name>
    ```
    {: codeblock}

    To access an {{site.data.keyword.Bluemix_notm}} public image:
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod
    spec:
      containers:
        - name: <container_name>
          image: registry.bluemix.net/<image_name>:<tag>
      imagePullSecrets:
        - name: <secret_name>
    ```
    {: codeblock}

    <table>
    <caption>Understanding the YAML file components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>&lt;container_name&gt;</em></code></td>
    <td>The name of the container to deploy to your cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;namespace_name&gt;</em></code></td>
    <td>The namespace where the image is stored. To list available namespaces, run `ibmcloud cr namespace-list`.</td>
    </tr>
    <tr>
    <td><code><em>&lt;image_name&gt;</em></code></td>
    <td>The name of the image that you want to use. To list available images in an {{site.data.keyword.Bluemix_notm}} account, run `ibmcloud cr image-list`.</td>
    </tr>
    <tr>
    <td><code><em>&lt;tag&gt;</em></code></td>
    <td>The version of the image that you want to use. If no tag is specified, the image that is tagged <strong>latest</strong> is used by default.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>The name of the imagePullSecret that you created earlier.</td>
    </tr>
    </tbody></table>

3.  Save your changes.
4.  Create the deployment in your cluster.
    ```
    kubectl apply -f mypod.yaml
    ```
    {: pre}

### Storing the imagePullSecret in the Kubernetes service account for the selected namespace
{:#store_imagePullSecret}

Every namespace has a Kubernetes service account that is named `default`. You can add the imagePullSecret to this service account to grant access to images in your registry. Deployments that do not specify a service account automatically use the `default` service account for this namespace.
{:shortdesc}

1. Check if an imagePullSecret already exists for your default service account.
   ```
   kubectl describe serviceaccount default -n <namespace_name>
   ```
   {: pre}
   When `<none>` is displayed in the **Image pull secrets** entry, no imagePullSecret exists.  
2. Add the imagePullSecret to your default service account.
   - **To add the imagePullSecret when no imagePullSecret is defined:**
       ```
       kubectl patch -n <namespace_name> serviceaccount/default -p '{"imagePullSecrets":[{"name": "bluemix-<namespace_name>-secret-regional"}]}'
       ```
       {: pre}
   - **To add the imagePullSecret when an imagePullSecret is already defined:**
       ```
       kubectl patch -n <namespace_name> serviceaccount/default --type='json' -p='[{"op":"add","path":"/imagePullSecrets/-","value":{"name":"bluemix-<namespace_name>-secret-regional"}}]'
       ```
       {: pre}
3. Verify that your imagePullSecret was added to your default service account.
   ```
   kubectl describe serviceaccount default -n <namespace_name>
   ```
   {: pre}

   Example output:
   ```
   Name:                default
   Namespace:           <namespace_name>
   Labels:              <none>
   Annotations:         <none>
   Image pull secrets:  bluemix-namespace_name-secret-regional
   Mountable secrets:   default-token-sh2dx
   Tokens:              default-token-sh2dx
   Events:              <none>
   ```
   {: pre}

4. Deploy a container from an image in your registry.
   ```
   apiVersion: v1
   kind: Pod
   metadata:
     name: mypod
   spec:
     containers:
       - name: <container_name>
         image: registry.<region>.bluemix.net/<namespace_name>/<image_name>:<tag>
   ```
   {: codeblock}

5. Create the deployment in the cluster.
   ```
   kubectl apply -f mypod.yaml
   ```
   {: pre}

<br />


