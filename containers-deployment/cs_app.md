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




# Deploying apps in clusters
{: #app}

You can use Kubernetes techniques in {{site.data.keyword.containerlong}} to deploy apps in containers and ensure that those apps are up and running at all times. For example, you can perform rolling updates and rollbacks without downtime for your users.
{: shortdesc}

Learn the general steps for deploying apps by clicking an area of the following image. Want to learn the basics first? Try out the [deploying apps tutorial](cs_tutorials_apps.html#cs_apps_tutorial).

<img usemap="#d62e18" border="0" class="image" id="basic_deployment_process" src="images/basic_deployment_process.png" width="780" style="width:780px;" alt="Basic deployment process"/>
<map name="d62e18" id="d62e18">
<area href="cs_cli_install.html" target="_blank" alt="Install the CLIs." title="Install the CLIs." shape="rect" coords="30, 69, 179, 209" />
<area href="https://kubernetes.io/docs/concepts/configuration/overview/" target="_blank" alt="Create a configuration file for your app. Review the best practices from Kubernetes." title="Create a configuration file for your app. Review the best practices from Kubernetes." shape="rect" coords="254, 64, 486, 231" />
<area href="#app_cli" target="_blank" alt="Option 1: Run configuration files from the Kubernetes CLI." title="Option 1: Run configuration files from the Kubernetes CLI." shape="rect" coords="544, 67, 730, 124" />
<area href="#cli_dashboard" target="_blank" alt="Option 2: Start the Kubernetes dashboard locally and run configuration files." title="Option 2: Start the Kubernetes dashboard locally and run configuration files." shape="rect" coords="544, 141, 728, 204" />
</map>

<br />




## Planning highly available deployments
{: #highly_available_apps}

The more widely you distribute your setup across multiple worker nodes and clusters, the less likely your users are to experience downtime with your app.
{: shortdesc}

Review the following potential app setups that are ordered with increasing degrees of availability.

![Stages of high availability for an app](images/cs_app_ha_roadmap-mz.png)

1.  A deployment with n+2 pods that are managed by a replica set in a single node in a single zone cluster.
2.  A deployment with n+2 pods that are managed by a replica set and spread across multiple nodes (anti-affinity) in a single zone cluster.
3.  A deployment with n+2 pods that are managed by a replica set and spread across multiple nodes (anti-affinity) in a multizone cluster across zones.

You can also [connect multiple clusters in different regions with a global load balancer](cs_clusters.html#multiple_clusters) to increase the high availability.

### Increasing the availability of your app
{: #increase_availability}

<dl>
  <dt>Use deployments and replica sets to deploy your app and its dependencies</dt>
    <dd><p>A deployment is a Kubernetes resource that you can use to declare all of the components of your app and its dependencies. With deployments, you don't have to write down all of the steps and instead can focus on your app.</p>
    <p>When you deploy more than one pod, a replica set is automatically created for your deployments that monitors the pods and assures that the desired number of pods is up and running at all times. When a pod goes down, the replica set replaces the unresponsive pod with a new one.</p>
    <p>You can use a deployment to define update strategies for your app including the number of pods that you want to add during a rolling update and the number of pods that can be unavailable at a time. When you perform a rolling update, the deployment checks whether or not the revision is working and stops the rollout when failures are detected.</p>
    <p>With deployments you can concurrently deploy multiple revisions with different flags. For example, you can test a deployment first before you decide to push it to production.</p>
    <p>Deployments allow you to keep track of any deployed revisions. You can use this history to roll back to a previous version if you encounter that your updates are not working as expected.</p></dd>
  <dt>Include enough replicas for your app's workload, plus two</dt>
    <dd>To make your app even more highly available and more resilient to failure, consider including extra replicas than the minimum to handle the expected workload. Extra replicas can handle the workload in case a pod crashes and the replica set has not yet recovered the crashed pod. For protection against two simultaneous failures, include two extra replicas. This setup is an N+2 pattern, where N is the number of replicas to handle the incoming workload and +2 is an extra two replicas. As long as your cluster has enough space, you can have as many pods as you want.</dd>
  <dt>Spread pods across multiple nodes (anti-affinity)</dt>
    <dd><p>When you create your deployment, each pod can be deployed to the same worker node. This is known as affinity, or co-location. To protect your app against worker node failure, you can configure your deployment to spread your pods across multiple worker nodes by using the <em>podAntiAffinity</em> option with your standard clusters. You can define two types of pod anti-affinity: preferred or required. For more information, see the Kubernetes documentation on <a href="https://kubernetes.io/docs/concepts/configuration/assign-pod-node/" rel="external" target="_blank" title="(Opens in a new tab or window)">Assigning Pods to Nodes</a>.</p>
    <p><strong>Note</strong>: With required anti-affinity, you can only deploy the amount of replicas that you have worker nodes for. For example, if you have 3 worker nodes in your cluster but you define 5 replicas in your YAML file, then only 3 replicas deploy. Each replica lives on a different worker node. The leftover 2 replicas remain pending. If you add another worker node to your cluster, then one of the leftover replicas deploys to the new worker node automatically.<p>
    <p><strong>Example deployment YAML files</strong>:<ul>
    <li><a href="https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/nginx_preferredAntiAffinity.yaml" rel="external" target="_blank" title="(Opens in a new tab or window)">Nginx app with preferred pod anti-affinity.</a></li>
    <li><a href="https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/liberty_requiredAntiAffinity.yaml" rel="external" target="_blank" title="(Opens in a new tab or window)">IBM® WebSphere® Application Server Liberty app with required pod anti-affinity.</a></li></ul></p>
    
    </dd>
<dt>Distribute pods across multiple zones or regions</dt>
  <dd><p>To protect your app from a zone failure, you can create multiple clusters in separate zones or add zones to a worker pool in a multizone cluster. Multizone clusters are available only in [certain metro areas](cs_regions.html#zones), such as Dallas. If you create multiple clusters in separate zones, you must [set up a global load balancer](cs_clusters.html#multiple_clusters).</p>
  <p>When you use a replica set and specify pod anti-affinity, Kubernetes spreads your app pods across the nodes. If your nodes are in multiple zones, the pods are spread across the zones, increasing the availability of your app. If you want to limit your apps to run only in one zone, you can configure pod affinity, or create and label a worker pool in one zone. For more information, see [High availability for multizone clusters](cs_clusters.html#ha_clusters).</p>
  <p><strong>In a multizone cluster deployment, are my app pods distributed evenly across the nodes?</strong></p>
  <p>The pods are evenly distributed across zones, but not always across nodes. For example, if you have a cluster with 1 node in each of 3 zones and deploy a replica set of 6 pods, then each node gets 2 pods. However, if you have a cluster with 2 nodes in each of 3 zones and deploy a replica set of 6 pods, each zone has 2 pods scheduled, and might schedule 1 pod per node or might not. For more control over scheduling, you can [set pod affinity ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node).</p>
  <p><strong>If a zone goes down, how are pods rescheduled onto the remaining nodes in the other zones?</strong></br>It depends on your scheduling policy that you used in the deployment. If you included [node-specific pod affinity ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#node-affinity-beta-feature), your pods are not rescheduled. If you did not, pods are created on available worker nodes in other zones, but they might not be balanced. For example, the 2 pods might be spread across the 2 available nodes, or they might both be scheduled onto 1 node with available capacity. Similarly, when the unavailable zone returns, pods are not automatically deleted and rebalanced across nodes. If you want the pods to be rebalanced across zones after the zone is back up, consider using the [Kubernetes descheduler ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes-incubator/descheduler).</p>
  <p><strong>Tip</strong>: In multizone clusters, try to keep your worker node capacity at 50% per zone so that you have enough capacity left to protect your cluster against a zonal failure.</p>
  <p><strong>What if I want to spread my app across regions?</strong></br>To protect your app from a region failure, create a second cluster in another region, [set up a global load balancer](cs_clusters.html#multiple_clusters) to connect your clusters, and use a deployment YAML to deploy a duplicate replica set with [pod anti-affinity ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/) for your app.</p>
  <p><strong>What if my apps need persistent storage?</strong></p>
  <p>Use a cloud service such as [{{site.data.keyword.cloudant_short_notm}}](/docs/services/Cloudant/getting-started.html#getting-started-with-cloudant) or [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage/about-cos.html#about-ibm-cloud-object-storage).</p></dd>
</dl>



### Minimal app deployment
{: #minimal_app_deployment}

A basic app deployment in a free or standard cluster might include the following components.
{: shortdesc}

![Deployment setup](images/cs_app_tutorial_components1.png)

To deploy the components for a minimal app as depicted in the diagram, you use a configuration file similar to the following example:
```
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: ibmliberty
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: ibmliberty
    spec:
      containers:
      - name: ibmliberty
        image: registry.bluemix.net/ibmliberty:latest
        ports:
        - containerPort: 9080        
---
apiVersion: v1
kind: Service
metadata:
  name: ibmliberty-service
  labels:
    app: ibmliberty
spec:
  selector:
    app: ibmliberty
  type: NodePort
  ports:
   - protocol: TCP
     port: 9080
```
{: codeblock}

**Note:** To expose your service, make sure that the key/value pair that you use in the `spec.selector` section of the service is the same as the key/value pair that you use in the `spec.template.metadata.labels` section of your deployment yaml.
To learn more about each component, review the [Kubernetes basics](cs_tech.html#kubernetes_basics).

<br />






## Launching the Kubernetes dashboard
{: #cli_dashboard}

Open a Kubernetes dashboard on your local system to view information about a cluster and its worker nodes. [In the GUI](#db_gui), you can access the dashboard with a convenient one-click button. [With the CLI](#db_cli), you can access the dashboard or use the steps in an automation process such as for a CI/CD pipeline.
{:shortdesc}

Before you begin, [target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster.

You can use the default port or set your own port to launch the Kubernetes dashboard for a cluster.

**Launching the Kubernetes dashboard from the GUI**
{: #db_gui}

1.  Log in to the [{{site.data.keyword.Bluemix_notm}} GUI](https://console.bluemix.net/).
2.  From your profile in the menu bar, select the account that you want to use.
3.  From the menu, click **Containers**.
4.  On the **Clusters** page, click the cluster that you want to access.
5.  From the cluster detail page, click the **Kubernetes Dashboard** button.

</br>
</br>

**Launching the Kubernetes dashboard from the CLI**
{: #db_cli}

1.  Get your credentials for Kubernetes.

    ```
    kubectl config view -o jsonpath='{.users[0].user.auth-provider.config.id-token}'
    ```
    {: pre}

2.  Copy the **id-token** value that is shown in the output.

3.  Set the proxy with the default port number.

    ```
    kubectl proxy
    ```
    {: pre}

    Example output:

    ```
    Starting to serve on 127.0.0.1:8001
    ```
    {: screen}

4.  Sign in to the dashboard.

  1.  In your browser, navigate to the following URL:

      ```
      http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/
      ```
      {: codeblock}

  2.  In the sign-on page, select the **Token** authentication method.

  3.  Then, paste the **id-token** value that you previously copied into the **Token** field and click **SIGN IN**.

When you are done with the Kubernetes dashboard, use `CTRL+C` to exit the `proxy` command. After you exit, the Kubernetes dashboard is no longer available. Run the `proxy` command to restart the Kubernetes dashboard.

[Next, you can run a configuration file from the dashboard.](#app_ui)

<br />


## Creating secrets
{: #secrets}

Kubernetes secrets are a secure way to store confidential information, such as user names, passwords, or keys.
{:shortdesc}

Review the following tasks that require secrets. For more information on what you can store in secrets, see the [Kubernetes documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/configuration/secret/).

### Adding a service to a cluster
{: #secrets_service}

When you bind a service to a cluster, you don't have to create a secret. A secret is automatically created for you. For more information, see [Adding Cloud Foundry services to clusters](cs_integrations.html#adding_cluster).

### Configuring the Ingress ALB to use TLS
{: #secrets_tls}

The ALB load balances HTTP network traffic to the apps in your cluster. To also load balance incoming HTTPS connections, you can configure the ALB to decrypt the network traffic and forward the decrypted request to the apps that are exposed in your cluster.

If you are using the IBM-provided Ingress subdomain, you can [use the IBM-provided TLS certificate](cs_ingress.html#public_inside_2). To view the IBM-provided TLS secret, run the following command:
```
ibmcloud ks cluster-get <cluster_name_or_ID> | grep "Ingress secret"
```
{: pre}

If you are using a custom domain, you can use your own certificate to manage TLS termination. To create your own TLS secret:
1. Generate a key and certificate in one of the following ways:
    * Generate a certificate authority (CA) cert and key from your certificate provider. If you have your own domain, purchase an official TLS certificate for your domain.
      **Important**: Make sure the [CN ![External link icon](../icons/launch-glyph.svg "External link icon")](https://support.dnsimple.com/articles/what-is-common-name/) is different for each certificate.
    * For testing purposes, you can create a self-signed certificate by using OpenSSL. For more information, see this [self-signed SSL certificate tutorial ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.akadia.com/services/ssh_test_certificate.html).
        1. Create a `tls.key`.
            ```
            openssl genrsa -out tls.key 2048
            ```
            {: pre}
        2. Use the key to create a `tls.crt`.
            ```
            openssl req -new -x509 -key tls.key -out tls.crt
            ```
            {: pre}
2. [Convert the cert and key into base-64 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.base64encode.org/).
3. Create a secret YAML file using the cert and key.
     ```
     apiVersion: v1
     kind: Secret
     metadata:
       name: ssl-my-test
     type: Opaque
     data:
       tls.crt: <client_certificate>
       tls.key: <client_key>
     ```
     {: codeblock}

4. Create the certificate as a Kubernetes secret.
     ```
     kubectl create -f ssl-my-test
     ```
     {: pre}

### Customizing the Ingress ALB with the SSL services annotation
{: #secrets_ssl_services}

You can use the [`ingress.bluemix.net/ssl-services` annotation](cs_annotations.html#ssl-services) encrypt traffic to your upstream apps from the Ingress ALB. To create the secret:

1. Get the certificate authority (CA) key and certificate from your upstream server.
2. [Convert the cert into base-64 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.base64encode.org/).
3. Create a secret YAML file using the cert.
     ```
     apiVersion: v1
     kind: Secret
     metadata:
       name: ssl-my-test
     type: Opaque
     data:
       trusted.crt: <ca_certificate>
     ```
     {: codeblock}
     **Note**: If you want to also enforce mutual authentication for upstream traffic, you can provide a `client.crt` and `client.key` in addition to the `trusted.crt` in the data section.
4. Create the certificate as a Kubernetes secret.
     ```
     kubectl create -f ssl-my-test
     ```
     {: pre}

### Customizing the Ingress ALB with the mutual authentication annotation
{: #secrets_mutual_auth}

You can use the [`ingress.bluemix.net/mutual-auth` annotation](cs_annotations.html#mutual-auth) to configure mutual authentication of downstream traffic for the Ingress ALB. To create a mutual authentication secret:

1. Generate a key and certificate in one of the following ways:
    * Generate a certificate authority (CA) cert and key from your certificate provider. If you have your own domain, purchase an official TLS certificate for your domain.
      **Important**: Make sure the [CN ![External link icon](../icons/launch-glyph.svg "External link icon")](https://support.dnsimple.com/articles/what-is-common-name/) is different for each certificate.
    * For testing purposes, you can create a self-signed certificate by using OpenSSL. For more information, see this [self-signed SSL certificate tutorial ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.akadia.com/services/ssh_test_certificate.html).
        1. Create a `ca.key`.
            ```
            openssl genrsa -out ca.key 1024
            ```
            {: pre}
        2. Use the key to create a `ca.crt`.
            ```
            openssl req -new -x509 -key ca.key -out ca.crt
            ```
            {: pre}
        3. Use the `ca.crt` to create a self-signed certificate.
            ```
            openssl x509 -req -in example.org.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out example.org.crt
            ```
            {: pre}
2. [Convert the cert into base-64 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.base64encode.org/).
3. Create a secret YAML file using the cert.
     ```
     apiVersion: v1
     kind: Secret
     metadata:
       name: ssl-my-test
     type: Opaque
     data:
       ca.crt: <ca_certificate>
     ```
     {: codeblock}
4. Create the certificate as a Kubernetes secret.
     ```
     kubectl create -f ssl-my-test
     ```
     {: pre}

<br />


## Deploying apps with the GUI
{: #app_ui}

When you deploy an app to your cluster by using the Kubernetes dashboard, a deployment resource automatically creates, updates, and manages the pods in your cluster.
{:shortdesc}

Before you begin:

-   Install the required [CLIs](cs_cli_install.html#cs_cli_install).
-   [Target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster.

To deploy your app:

1.  Open the Kubernetes [dashboard](#cli_dashboard) and click **+ Create**.
2.  Enter your app details in one of two ways.
  * Select **Specify app details below** and enter the details.
  * Select **Upload a YAML or JSON file** to upload your app [configuration file ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/).

  Need help with your configuration file? Check out this [example YAML file ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/IBM-Cloud/kube-samples/blob/master/deploy-apps-clusters/deploy-ibmliberty.yaml). In this example, a container is deployed from the **ibmliberty** image in the US-South region. Learn more about [securing your personal information](cs_secure.html#pi) when you work with Kubernetes resources.
  {: tip}

3.  Verify that you successfully deployed your app in one of the following ways.
  * In the Kubernetes dashboard, click **Deployments**. A list of successful deployments is displayed.
  * If your app is [publicly available](cs_network_planning.html#public_access), navigate to the cluster overview page in your {{site.data.keyword.containerlong}} dashboard. Copy the subdomain, which is located in the cluster summary section and paste it into a browser to view your app.

<br />


## Deploying apps with the CLI
{: #app_cli}

After a cluster is created, you can deploy an app into that cluster by using the Kubernetes CLI.
{:shortdesc}

Before you begin:

-   Install the required [CLIs](cs_cli_install.html#cs_cli_install).
-   [Target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster.

To deploy your app:

1.  Create a configuration file based on [Kubernetes best practices ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/configuration/overview/). Generally, a configuration file contains configuration details for each of the resources you are creating in Kubernetes. Your script might include one or more of the following sections:

    -   [Deployment ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/): Defines the creation of pods and replica sets. A pod includes an individual containerized app and replica sets control multiple instances of pods.

    -   [Service ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/services-networking/service/): Provides front-end access to pods by using a worker node or load balancer public IP address, or a public Ingress route.

    -   [Ingress ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/services-networking/ingress/): Specifies a type of load balancer that provides routes to access your app publicly.

    Learn more about [securing your personal information](cs_secure.html#pi) when you work with Kubernetes resources.

2.  Run the configuration file in a cluster's context.

    ```
    kubectl apply -f config.yaml
    ```
    {: pre}

3.  If you made your app publicly available by using a nodeport service, a load balancer service, or Ingress, verify that you can access the app.

<br />


## Deploying apps to specific worker nodes by using labels
{: #node_affinity}

When you deploy an app, the app pods indiscriminately deploy to various worker nodes in your cluster. In some cases, you might want to restrict the worker nodes that the app pods to deploy to. For example, you might want app pods to only deploy to worker nodes in a certain worker pool because those worker nodes are on bare metal machines. To designate the worker nodes that app pods must deploy to, add an affinity rule to your app deployment.
{:shortdesc}

Before you begin, [target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster.

1. Get the name of the worker pool that you want to deploy app pods to.
    ```
    ibmcloud ks worker-pools <cluster_name_or_ID>
    ```
    {:pre}

    These steps use a worker pool name as an example. To deploy app pods to certain worker nodes based on another factor, get that value instead. For example, to deploy app pods only to worker nodes on a specific VLAN, get the VLAN ID by running `ibmcloud ks vlans <zone>`.
    {: tip}

2. [Add an affinity rule ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#node-affinity-beta-feature) for the worker pool name to the app deployment.

    Example yaml:

    ```
    apiVersion: extensions/v1beta1
    kind: Deployment
    metadata:
      name: with-node-affinity
    spec:
      template:
        spec:
          affinity:
            nodeAffinity:
              requiredDuringSchedulingIgnoredDuringExecution:
                nodeSelectorTerms:
                - matchExpressions:
                  - key: workerPool
                    operator: In
                    values:
                    - <worker_pool_name>
    ...
    ```
    {: codeblock}

    In the **affinity** section of the example yaml, `workerPool` is the `key` and `<worker_pool_name>` is the `value`.

3. Apply the updated deployment configuration file.
    ```
    kubectl apply -f with-node-affinity.yaml
    ```
    {: pre}

4. Verify that the app pods deployed to the correct worker nodes.

    1. List the pods in your cluster.
        ```
        kubectl get pods -o wide
        ```
        {: pre}

        Example output:
        ```
        NAME                   READY     STATUS              RESTARTS   AGE       IP               NODE
        cf-py-d7b7d94db-vp8pq  1/1       Running             0          15d       172.30.xxx.xxx   10.176.48.78
        ```
        {: screen}

    2. In the output, identify a pod for your app. Note the **NODE** private IP address of the worker node that the pod is on.

        In the above example output, the app pod `cf-py-d7b7d94db-vp8pq` is on a worker node with the IP address `10.176.48.78`.

    3. List the worker nodes in the worker pool that you designated in your app deployment.

        ```
        ibmcloud ks workers <cluster_name_or_ID> --worker-pool <worker_pool_name>
        ```
        {: pre}

        Example output:

        ```
        ID                                                 Public IP       Private IP     Machine Type      State    Status  Zone    Version
        kube-dal10-crb20b637238bb471f8b4b8b881bbb4962-w7   169.xx.xxx.xxx  10.176.48.78   b2c.4x16          normal   Ready   dal10   1.8.6_1504
        kube-dal10-crb20b637238bb471f8b4b8b881bbb4962-w8   169.xx.xxx.xxx  10.176.48.83   b2c.4x16          normal   Ready   dal10   1.8.6_1504
        kube-dal12-crb20b637238bb471f8b4b8b881bbb4962-w9   169.xx.xxx.xxx  10.176.48.69   b2c.4x16          normal   Ready   dal12   1.8.6_1504
        ```
        {: screen}

        If you created an app affinity rule based on another factor, get that value instead. For example, to verify that the app pod deployed to a worker nodes on a specific VLAN, view the VLAN that the worker node is on by running `ibmcloud ks worker-get <cluster_name_or_ID> <worker_ID>`.
        {: tip}

    4. In the output, verify that the worker node with the private IP address that you identified in the previous step is deployed in this worker pool.

<br />


## Deploying an app on a GPU machine
{: #gpu_app}

If you have a [bare metal graphics processing unit (GPU) machine type](cs_clusters.html#shared_dedicated_node), you can schedule mathematically intensive workloads onto the worker node. For example, you might run a 3D app that uses the Compute Unified Device Architecture (CUDA) platform to share the processing load across the GPU and CPU to increase performance.
{:shortdesc}

In the following steps, you learn how to deploy workloads that require the GPU. You can also [deploy apps](#app_ui) that don't need to process their workloads across both the GPU and CPU. After, you might find it useful to play around with mathematically intensive workloads such as the [TensorFlow ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.tensorflow.org/) machine learning framework with [this Kubernetes demo ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/pachyderm/pachyderm/tree/master/doc/examples/ml/tensorflow).

Before you begin:
* [Create a bare metal GPU machine type](cs_clusters.html#clusters_cli). Note that this process can take more than 1 business day to complete.
* Your cluster master and GPU worker node must run Kubernetes version 1.10 or later.

To execute a workload on a GPU machine:
1.  Create a YAML file. In this example, a `Job` YAML manages batch-like workloads by making a short-lived pod that runs until the command that it is scheduled to complete successfully terminates.

    **Important**: For GPU workloads, you must always provide the `resources: limits: nvidia.com/gpu` field in the YAML specification.

    ```yaml
    apiVersion: batch/v1
    kind: Job
    metadata:
      name: nvidia-smi
      labels:
        name: nvidia-smi
    spec:
      template:
        metadata:
          labels:
            name: nvidia-smi
        spec:
          containers:
          - name: nvidia-smi
            image: nvidia/cuda:9.1-base-ubuntu16.04
            command: [ "/usr/test/nvidia-smi" ]
            imagePullPolicy: IfNotPresent
            resources:
              limits:
                nvidia.com/gpu: 2
            volumeMounts:
            - mountPath: /usr/test
              name: nvidia0
          volumes:
            - name: nvidia0
              hostPath:
                path: /usr/bin
          restartPolicy: Never
    ```
    {: codeblock}

    <table>
    <caption>YAML components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
    </thead>
    <tbody>
    <tr>
    <td>Metadata and label names</td>
    <td>Give a name and a label for the job, and use the same name in both the file's metadata and the `spec template` metadata. For example, `nvidia-smi`.</td>
    </tr>
    <tr>
    <td><code>containers/image</code></td>
    <td>Provide the image that the container is a running instance of. In this example, the value is set to use the DockerHub CUDA image:<code>nvidia/cuda:9.1-base-ubuntu16.04</code></td>
    </tr>
    <tr>
    <td><code>containers/command</code></td>
    <td>Specify a command to run in the container. In this example, the <code>[ "/usr/test/nvidia-smi" ]</code>command refers to a binary that is on the GPU machine, so you must also set up a volume mount.</td>
    </tr>
    <tr>
    <td><code>containers/imagePullPolicy</code></td>
    <td>To pull a new image only if the image is not currently on the worker node, specify <code>IfNotPresent</code>.</td>
    </tr>
    <tr>
    <td><code>resources/limits</code></td>
    <td>For GPU machines, you must specify the resource limit. The Kubernetes [Device Plug-in![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/cluster-administration/device-plugins/) sets the default resource request to match the limit.
    <ul><li>You must specify the key as <code>nvidia.com/gpu</code>.</li>
    <li>Enter the whole number of GPUs that you request, such as <code>2</code>. <strong>Note</strong>: Container pods do not share GPUs and GPUs cannot be overcommitted. For example, if you have only 1 `mg1c.16x128` machine, then you have only 2 GPUs in that machine and can specify a maximum of `2`.</li></ul></td>
    </tr>
    <tr>
    <td><code>volumeMounts</code></td>
    <td>Name the volume that is mounted onto the container, such as <code>nvidia0</code>. Specify the <code>mountPath</code> on the container for the volume. In this example, the path <code>/usr/test</code> matches the path used in the job container command.</td>
    </tr>
    <tr>
    <td><code>volumes</code></td>
    <td>Name the job volume, such as <code>nvidia0</code>. In the GPU worker node's <code>hostPath</code>, specify the volume's <code>path</code> on the host, in this example, <code>/usr/bin</code>. The container <code>mountPath</code> is mapped to the host volume <code>path</code>, which gives this job access to the NVIDIA binaries on the GPU worker node for the container command to run.</td>
    </tr>
    </tbody></table>

2.  Apply the YAML file. For example:

    ```
    kubectl apply -f nvidia-smi.yaml
    ```
    {: pre}

3.  Check the job pod by filtering your pods by the `nvidia-sim` label. Verify that the **STATUS** is **Completed**.

    ```
    kubectl get pod -a -l 'name in (nvidia-sim)'
    ```
    {: pre}

    Example output:
    ```
    NAME                  READY     STATUS      RESTARTS   AGE
    nvidia-smi-ppkd4      0/1       Completed   0          36s
    ```
    {: screen}

4.  Describe the pod to see how the GPU device plug-in scheduled the pod.
    * In the `Limits` and `Requests` fields, see that the resource limit that you specified matches the request that the device plug-in automatically set.
    * In the events, verify that the pod is assigned to your GPU worker node.

    ```
    kubectl describe pod nvidia-smi-ppkd4
    ```
    {: pre}

    Example output:
    ```
    Name:           nvidia-smi-ppkd4
    Namespace:      default
    ...
    Limits:
     nvidia.com/gpu:  2
    Requests:
     nvidia.com/gpu:  2
    ...
    Events:
    Type    Reason                 Age   From                     Message
    ----    ------                 ----  ----                     -------
    Normal  Scheduled              1m    default-scheduler        Successfully assigned nvidia-smi-ppkd4 to 10.xxx.xx.xxx
    ...
    ```
    {: screen}

5.  To verify that the job used the GPU to compute its workload, you can check the logs. The `[ "/usr/test/nvidia-smi" ]` command from the job queried the GPU device state on the GPU worker node.

    ```
    kubectl logs nvidia-sim-ppkd4
    ```
    {: pre}

    Example output:
    ```
    +-----------------------------------------------------------------------------+
    | NVIDIA-SMI 390.12                 Driver Version: 390.12                    |
    |-------------------------------+----------------------+----------------------+
    | GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
    | Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
    |===============================+======================+======================|
    |   0  Tesla K80           Off  | 00000000:83:00.0 Off |                  Off |
    | N/A   37C    P0    57W / 149W |      0MiB / 12206MiB |      0%      Default |
    +-------------------------------+----------------------+----------------------+
    |   1  Tesla K80           Off  | 00000000:84:00.0 Off |                  Off |
    | N/A   32C    P0    63W / 149W |      0MiB / 12206MiB |      1%      Default |
    +-------------------------------+----------------------+----------------------+

    +-----------------------------------------------------------------------------+
    | Processes:                                                       GPU Memory |
    |  GPU       PID   Type   Process name                             Usage      |
    |=============================================================================|
    |  No running processes found                                                 |
    +-----------------------------------------------------------------------------+
    ```
    {: screen}

    In this example, you see that both GPUs were used to execute the job because both the GPUs were scheduled in the worker node. If the limit is set to 1, only 1 GPU is shown.

## Scaling apps
{: #app_scaling}

With Kubernetes, you can enable [horizontal pod autoscaling ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) to automatically increase or decrease the number of instances of your apps based on CPU.
{:shortdesc}

Looking for information about scaling Cloud Foundry applications? Check out [IBM Auto-Scaling for {{site.data.keyword.Bluemix_notm}}](/docs/services/Auto-Scaling/index.html). 
{: tip}

Before you begin:
- [Target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster.
- Heapster monitoring must be deployed in the cluster that you want to autoscale.

Steps:

1.  Deploy your app to a cluster from the CLI. When you deploy your app, you must request CPU.

    ```
    kubectl run <app_name> --image=<image> --requests=cpu=<cpu> --expose --port=<port_number>
    ```
    {: pre}

    <table>
    <caption>Command components for kubectl run</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding this command&apos;s components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--image</code></td>
    <td>The application that you want to deploy.</td>
    </tr>
    <tr>
    <td><code>--request=cpu</code></td>
    <td>The required CPU for the container, which is specified in milli-cores. As an example, <code>--requests=200m</code>.</td>
    </tr>
    <tr>
    <td><code>--expose</code></td>
    <td>When true, creates an external service.</td>
    </tr>
    <tr>
    <td><code>--port</code></td>
    <td>The port where your app is available externally.</td>
    </tr></tbody></table>

    For more complex deployments, you might need to create a [configuration file](#app_cli).
    {: tip}

2.  Create an autoscaler and define your policy. For more information about working with the `kubectl autoscale` command, see [the Kubernetes documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://v1-8.docs.kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#autoscale).

    ```
    kubectl autoscale deployment <deployment_name> --cpu-percent=<percentage> --min=<min_value> --max=<max_value>
    ```
    {: pre}

    <table>
    <caption>Command components for kubectl autoscale</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding this command&apos;s components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--cpu-percent</code></td>
    <td>The average CPU utilization that is maintained by the Horizontal Pod Autoscaler, which is specified as a percentage.</td>
    </tr>
    <tr>
    <td><code>--min</code></td>
    <td>The minimum number of deployed pods that are used to maintain the specified CPU utilization percentage.</td>
    </tr>
    <tr>
    <td><code>--max</code></td>
    <td>The maximum number of deployed pods that are used to maintain the specified CPU utilization percentage.</td>
    </tr>
    </tbody></table>


<br />


## Managing rolling deployments
{: #app_rolling}

You can manage the rollout of your changes in an automated and controlled fashion. If your rollout isn't going according to plan, you can roll back your deployment to the previous revision.
{:shortdesc}

Before you begin, create a [deployment](#app_cli).

1.  [Roll out ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#updating-a-deployment) a change. For example, you might want to change the image that you used in your initial deployment.

    1.  Get the deployment name.

        ```
        kubectl get deployments
        ```
        {: pre}

    2.  Get the pod name.

        ```
        kubectl get pods
        ```
        {: pre}

    3.  Get the name of the container that is running in the pod.

        ```
        kubectl describe pod <pod_name>
        ```
        {: pre}

    4.  Set the new image for the deployment to use.

        ```
        kubectl set image deployment/<deployment_name><container_name>=<image_name>
        ```
        {: pre}

    When you run the commands, the change is immediately applied and logged in the roll-out history.

2.  Check the status of your deployment.

    ```
    kubectl rollout status deployments/<deployment_name>
    ```
    {: pre}

3.  Roll back a change.
    1.  View the roll-out history for the deployment and identify the revision number of your last deployment.

        ```
        kubectl rollout history deployment/<deployment_name>
        ```
        {: pre}

        **Tip:** To see the details for a specific revision, include the revision number.

        ```
        kubectl rollout history deployment/<deployment_name> --revision=<number>
        ```
        {: pre}

    2.  Roll back to the previous version, or specify a revision. To roll back to the previous version, use the following command.

        ```
        kubectl rollout undo deployment/<depoyment_name> --to-revision=<number>
        ```
        {: pre}

<br />

