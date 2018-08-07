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




# Integrating services
{: #integrations}

You can use various external services and catalog services with a standard Kubernetes cluster in {{site.data.keyword.containerlong}}.
{:shortdesc}


## Application services
{: #application_services}
<table summary="Summary for accessibility">
<caption>Application services</caption>
<thead>
<tr>
<th>Service</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.blockchainfull}}</td>
<td>Deploy a publicly available development environment for IBM Blockchain to a Kubernetes cluster in {{site.data.keyword.containerlong_notm}}. Use this environment to develop and customize your own blockchain network to deploy apps that share an immutable ledger for recording the history of transactions. For more information, see <a href="https://ibm-blockchain.github.io" target="_blank">Develop in a cloud sandbox
IBM Blockchain Platform <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
</tbody>
</table>

<br />



## DevOps services
{: #devops_services}
<table summary="Summary for accessibility">
<caption>DevOps services</caption>
<thead>
<tr>
<th>Service</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Codeship</td>
<td>You can use <a href="https://codeship.com" target="_blank">Codeship <img src="../icons/launch-glyph.svg" alt="External link icon"></a> for the continuous integration and delivery of containers. For more information, see <a href="https://www.ibm.com/blogs/bluemix/2017/10/using-codeship-pro-deploy-workloads-ibm-container-service/" target="_blank">Using Codeship Pro To Deploy Workloads to {{site.data.keyword.containershort_notm}} <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>Helm</td>
<td> <a href="https://helm.sh" target="_blank">Helm <img src="../icons/launch-glyph.svg" alt="External link icon"></a> is a Kubernetes package manager. You can create new Helm charts or use preexisting Helm charts to define, install, and upgrade complex Kubernetes applications that run in {{site.data.keyword.containerlong_notm}} clusters. <p>For more information, see [Setting up Helm in {{site.data.keyword.containershort_notm}}](cs_integrations.html#helm).</p></td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>Automate your app builds and container deployments to Kubernetes clusters by using a toolchain. For setup information, see the blog <a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">Deploy Kubernetes pods to the {{site.data.keyword.containerlong_notm}} using DevOps Pipelines <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>Istio</td>
<td><a href="https://www.ibm.com/cloud/info/istio" target="_blank">Istio <img src="../icons/launch-glyph.svg" alt="External link icon"></a> is an open source service that gives developers a way to connect, secure, manage, and monitor a network of microservices, also known a service mesh, on cloud orchestration platforms like Kubernetes. Check out the blog post about <a href="https://developer.ibm.com/dwblog/2017/istio/" target="_blank">how IBM co-founded and launched Istio<img src="../icons/launch-glyph.svg" alt="External link icon"></a> to find out more about the open-source project. To install Istio on your Kubernetes cluster in {{site.data.keyword.containershort_notm}} and get started with a sample app, see [Tutorial: Managing microservices with Istio](cs_tutorials_istio.html#istio_tutorial).</td>
</tr>
</tbody>
</table>

<br />



## Logging and monitoring services
{: #health_services}
<table summary="Summary for accessibility">
<caption>Logging and monitoring services</caption>
<thead>
<tr>
<th>Service</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>CoScale</td>
<td>Monitor worker nodes, containers, replica sets, replication controllers, and services with <a href="https://www.coscale.com/" target="_blank">CoScale <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. For more information, see <a href="https://www.ibm.com/blogs/bluemix/2017/06/monitoring-ibm-bluemix-container-service-coscale/" target="_blank">Monitoring {{site.data.keyword.containershort_notm}} with CoScale <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>Datadog</td>
<td>Monitor your cluster and view infrastructure and application performance metrics with <a href="https://www.datadoghq.com/" target="_blank">Datadog <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. For more information, see <a href="https://www.ibm.com/blogs/bluemix/2017/07/monitoring-ibm-bluemix-container-service-datadog/" target="_blank">Monitoring {{site.data.keyword.containershort_notm}} with Datadog <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td> {{site.data.keyword.cloudaccesstrailfull}}</td>
<td>Monitor the administrative activity made in your cluster by analyzing logs through Grafana. For more information about the service, see the [Activity Tracker](/docs/services/cloud-activity-tracker/index.html) documentation. For more information about the types of events that you can track, see [Activity Tracker events](/cs_at_events.html).</td>
</tr>
<tr>
<td>{{site.data.keyword.loganalysisfull}}</td>
<td>Expand your log collection, retention, and search abilities with {{site.data.keyword.loganalysisfull_notm}}. For more information, see <a href="../services/CloudLogAnalysis/containers/containers_kube_other_logs.html" target="_blank">Enabling automatic collection of cluster logs <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.monitoringlong}}</td>
<td>Expand your metrics collection and retention capabilities by defining rules and alerts with {{site.data.keyword.monitoringlong_notm}}. For more information, see <a href="../services/cloud-monitoring/tutorials/container_service_metrics.html" target="_blank">Analyze metrics in Grafana for an app that is deployed in a Kubernetes cluster <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>Instana</td>
<td> <a href="https://www.instana.com/" target="_blank">Instana <img src="../icons/launch-glyph.svg" alt="External link icon"></a> provides infrastructure and app performance monitoring with a GUI that automatically discovers and maps your apps. Istana captures every request to your apps, which you can use to troubleshoot and perform root cause analysis to prevent the problems from happening again. Check out the blog post about <a href="https://www.instana.com/blog/precise-visibility-applications-ibm-bluemix-container-service/" target="_blank">deploying Istana in {{site.data.keyword.containershort_notm}} <img src="../icons/launch-glyph.svg" alt="External link icon"></a> to learn more.</td>
</tr>
<tr>
<td>Prometheus</td>
<td>Prometheus is an open source monitoring, logging, and alerting tool that was specifically designed for Kubernetes. Prometheus retrieves detailed information about the cluster, worker nodes, and deployment health based on Kubernetes logging information. CPU, memory, I/O, and network activity is collected for each container running in a cluster. You can use the collected data in custom queries or alerts to monitor performance and workloads in your cluster.

<p>To use Prometheus, follow the <a href="https://github.com/coreos/prometheus-operator/tree/master/contrib/kube-prometheus" target="_blank">CoreOS instructions <img src="../icons/launch-glyph.svg" alt="External link icon"></a>.</p>
</td>
</tr>
<tr>
<td>Sematext</td>
<td>View metrics and logs for your containerized applications by using <a href="https://sematext.com/" target="_blank">Sematext <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. For more information, see <a href="https://www.ibm.com/blogs/bluemix/2017/09/monitoring-logging-ibm-bluemix-container-service-sematext/" target="_blank">Monitoring and logging for containers with Sematext <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>Sysdig</td>
<td>Capture app, container, statsd, and host metrics with a single instrumentation point by using <a href="https://sysdig.com/" target="_blank">Sysdig <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. For more information, see <a href="https://www.ibm.com/blogs/bluemix/2017/08/monitoring-ibm-bluemix-container-service-sysdig-container-intelligence/" target="_blank">Monitoring {{site.data.keyword.containershort_notm}} with Sysdig Container Intelligence <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>Weave Scope</td>
<td>Weave Scope provides a visual diagram of your resources within a Kubernetes cluster, including services, pods, containers, processes, nodes, and more. Weave Scope provides interactive metrics for CPU and memory and also provides tools to tail and exec into a container.<p>For more information, see [Visualizing Kubernetes cluster resources with Weave Scope and {{site.data.keyword.containershort_notm}}](cs_integrations.html#weavescope).</p></li></ol>
</td>
</tr>
</tbody>
</table>

<br />



## Security services
{: #security_services}
<table summary="Summary for accessibility">
<caption>Security services</caption>
<thead>
<tr>
<th>Service</th>
<th>Description</th>
</tr>
</thead>
<tbody>
  <tr id="appid">
    <td>{{site.data.keyword.appid_full}}</td>
    <td>Add a level of security to your apps with [{{site.data.keyword.appid_short}}](/docs/services/appid/index.html#gettingstarted) by requiring users to sign in. To authenticate web or API HTTP/HTTPS requests to your app, you can integrate {{site.data.keyword.appid_short_notm}} with your Ingress service by using the [{{site.data.keyword.appid_short_notm}} authentication Ingress annotation](cs_annotations.html#appid-auth).</td>
  </tr>
<tr>
<td>Aqua Security</td>
  <td>As a supplement to <a href="/docs/services/va/va_index.html" target="_blank">Vulnerability Advisor</a>, you can use <a href="https://www.aquasec.com/" target="_blank">Aqua Security <img src="../icons/launch-glyph.svg" alt="External link icon"></a> to improve the security of container deployments by reducing what your app is allowed to do. For more information, see <a href="https://blog.aquasec.com/securing-container-deployments-on-bluemix-with-aqua-security" target="_blank">Securing container deployments on {{site.data.keyword.Bluemix_notm}} with Aqua Security <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.cloudcerts_full}}</td>
<td>You can use <a href="../services/certificate-manager/index.html" target="_blank">{{site.data.keyword.cloudcerts_long}} <img src="../icons/launch-glyph.svg" alt="External link icon"></a> to store and manage SSL certificates for your apps. For more information, see <a href="https://www.ibm.com/blogs/bluemix/2018/01/use-ibm-cloud-certificate-manager-ibm-cloud-container-service-deploy-custom-domain-tls-certificates/" target="_blank">Use {{site.data.keyword.cloudcerts_long_notm}} with {{site.data.keyword.containershort_notm}} to deploy custom domain TLS Certificates <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
  <td>{{site.data.keyword.registrylong}}</td>
  <td>Set up your own secured Docker image repository where you can safely store and share images between cluster users. For more information, see the <a href="/docs/services/Registry/index.html" target="_blank">{{site.data.keyword.registrylong}} documentation <img src="../icons/launch-glyph.svg" alt="External link icon"></a>.</td>
</tr>
<tr>
<td>NeuVector</td>
<td>Protect containers with a cloud-native firewall by using <a href="https://neuvector.com/" target="_blank">NeuVector <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. For more information, see <a href="https://www.ibm.com/us-en/marketplace/neuvector-container-security" target="_blank">NeuVector Container Security <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>Twistlock</td>
<td>As a supplement to <a href="/docs/services/va/va_index.html" target="_blank">Vulnerability Advisor</a>, you can use <a href="https://www.twistlock.com/" target="_blank">Twistlock <img src="../icons/launch-glyph.svg" alt="External link icon"></a> to manage firewalls, threat protection, and incident response. For more information, see <a href="https://www.ibm.com/blogs/bluemix/2017/07/twistlock-ibm-bluemix-container-service/" target="_blank">Twistlock on {{site.data.keyword.containershort_notm}} <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
</tbody>
</table>

<br />



## Storage services
{: #storage_services}
<table summary="Summary for accessibility">
<caption>Storage services</caption>
<thead>
<tr>
<th>Service</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Heptio Ark</td>
  <td>You can use <a href="https://github.com/heptio/ark" target="_blank">Heptio Ark <img src="../icons/launch-glyph.svg" alt="External link icon"></a> to back up and restore cluster resources and persistent volumes. For more information, see the Heptio Ark <a href="https://github.com/heptio/ark/blob/master/docs/use-cases.md#use-cases" target="_blank">Use cases for disaster recovery and cluster migration <img src="../icons/launch-glyph.svg" alt="External link icon"></a>.</td>
</tr>
<tr>
  <td>{{site.data.keyword.cos_full}}</td>
  <td>Data that is stored with {{site.data.keyword.cos_short}} is encrypted and dispersed across multiple geographic locations, and accessed over HTTP by using a REST API. You can use the [ibm-backup-restore image](/docs/services/RegistryImages/ibm-backup-restore/index.html) to configure the service to make one-time or scheduled backups for data in your clusters. For general information about the service, see the <a href="/docs/services/cloud-object-storage/about-cos.html" target="_blank">{{site.data.keyword.cos_short}} documentation <img src="../icons/launch-glyph.svg" alt="External link icon"></a>.</td>
</tr>
  <tr>
    <td>{{site.data.keyword.cloudantfull}}</td>
    <td>{{site.data.keyword.cloudant_short_notm}} is a document-oriented DataBase as a Service (DBaaS) that stores data as documents in JSON format. The service is built for scalability, high availability, and durability. For more information, see the <a href="/docs/services/Cloudant/getting-started.html" target="_blank">{{site.data.keyword.cloudant_short_notm}} documentation <img src="../icons/launch-glyph.svg" alt="External link icon"></a>.</td>
  </tr>
  <tr>
    <td>{{site.data.keyword.composeForMongoDB_full}}</td>
    <td>{{site.data.keyword.composeForMongoDB}} delivers high availability and redundancy, automated and on-demand no-stop backups, monitoring tools, integration into alert systems, performance analysis views, and more. For more information, see the <a href="/docs/services/ComposeForMongoDB/index.html" target="_blank">{{site.data.keyword.composeForMongoDB}} documentation <img src="../icons/launch-glyph.svg" alt="External link icon"></a>.</td>
  </tr>
</tbody>
</table>


<br />



## Adding Cloud Foundry services to clusters
{: #adding_cluster}

Add an existing Cloud Foundry service instance to your cluster to enable your cluster users to access and use the service when they deploy an app to the cluster.
{:shortdesc}

Before you begin:

1. [Target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster.
2. [Request an instance of the {{site.data.keyword.Bluemix_notm}} service](/docs/apps/reqnsi.html#req_instance).
   **Note:** To create an instance of a service in the Washington DC zone, you must use the CLI.
3. Cloud Foundry services are supported to bind with clusters, but other services are not. You can see the different service types after you create the service instance and the services are grouped in the dashboard as **Cloud Foundry Services** and **Services**. To bind the services in the **Services** section with clusters, [first create Cloud Foundry aliases](#adding_resource_cluster).

**Note:**
<ul><ul>
<li>You can add only {{site.data.keyword.Bluemix_notm}} services that support service keys. If the service does not support service keys, see [Enabling external apps to use {{site.data.keyword.Bluemix_notm}} services](/docs/apps/reqnsi.html#accser_external).</li>
<li>The cluster and the worker nodes must be deployed fully before you can add a service.</li>
</ul></ul>


To add a service:
2.  List available {{site.data.keyword.Bluemix_notm}} services.

    ```
    ibmcloud service list
    ```
    {: pre}

    Example CLI output:

    ```
    name                      service           plan    bound apps   last operation
    <service_instance_name>   <service_name>    spark                create succeeded
    ```
    {: screen}

3.  Note the **name** of the service instance that you want to add to your cluster.
4.  Identify the cluster namespace that you want to use to add your service. Choose between the following options.
    -   List existing namespaces and choose a namespace that you want to use.

        ```
        kubectl get namespaces
        ```
        {: pre}

    -   Create a namespace in your cluster.

        ```
        kubectl create namespace <namespace_name>
        ```
        {: pre}

5.  Add the service to your cluster.

    ```
    ibmcloud ks cluster-service-bind <cluster_name_or_ID> <namespace> <service_instance_name>
    ```
    {: pre}

    When the service is successfully added to your cluster, a cluster secret is created that holds the credentials of your service instance. Example CLI output:

    ```
    ibmcloud ks cluster-service-bind mycluster mynamespace cleardb
    Binding service instance to namespace...
    OK
    Namespace:	mynamespace
    Secret name:     binding-<service_instance_name>
    ```
    {: screen}

6.  Verify that the secret was created in your cluster namespace.

    ```
    kubectl get secrets --namespace=<namespace>
    ```
    {: pre}

To use the service in a pod that is deployed in the cluster, cluster users must access the service credentials. Users can access the service credentials of the {{site.data.keyword.Bluemix_notm}} service by [mounting the Kubernetes secret as a secret volume to a pod](#adding_app).

<br />


## Creating Cloud Foundry aliases for other {{site.data.keyword.Bluemix_notm}} service resources
{: #adding_resource_cluster}

Cloud Foundry services are supported for binding with clusters. You can see the different service types after you create the service instance and the services are grouped in the dashboard as **Cloud Foundry Services** and **Services**. To bind an {{site.data.keyword.Bluemix_notm}} service that is not a Cloud Foundry service to your cluster, create a Cloud Foundry alias for the service instance.
{:shortdesc}

Before you begin, [request an instance of the {{site.data.keyword.Bluemix_notm}} service](/docs/apps/reqnsi.html#req_instance).

To create a Cloud Foundry alias for the service instance:

1. Target the org and a space where the service instance is created.

    ```
    ibmcloud target -o <org_name> -s <space_name>
    ```
    {: pre}

2. Note the service instance name.
    ```
    ibmcloud resource service-instances
    ```
    {: pre}

3. Create a Cloud Foundry alias for the service instance.
    ```
    ibmcloud resource service-alias-create <service_alias_name> --instance-name <service_instance>
    ```
    {: pre}

4. Verify that the service alias was created.

    ```
    ibmcloud service list
    ```
    {: pre}

5. [Bind the Cloud Foundry alias to the cluster](#adding_cluster).



<br />


## Adding services to apps
{: #adding_app}

Encrypted Kubernetes secrets are used to store {{site.data.keyword.Bluemix_notm}} service details and credentials and allow secure communication between the service and the cluster.
{:shortdesc}

Kubernetes secrets are a secure way to store confidential information, such as user names, passwords, or keys. Rather than exposing confidential information via environment variables or directly in the Dockerfile, you can mount secrets to a pod. Then, those secrets can be accessed by a running container in a pod.

When you mount a secret volume to your pod, a file that is named `binding` is stored in the volume mount directory. The `binding` file includes all information and credentials that you need to access the {{site.data.keyword.Bluemix_notm}} service.

Before you begin, [target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster. Make sure that the {{site.data.keyword.Bluemix_notm}} service that you want to use in your app was [added to the cluster](cs_integrations.html#adding_cluster) by the cluster admin.

1.  List available secrets in your cluster namespace.

    ```
    kubectl get secrets --namespace=<my_namespace>
    ```
    {: pre}

    Example output:

    ```
    NAME                              TYPE            DATA      AGE
    binding-<service_instance_name>   Opaque          1         3m

    ```
    {: screen}

2.  Look for a secret of type **Opaque** and note the **name** of the secret. If multiple secrets exist, contact your cluster administrator to identify the correct service secret.

3.  Open your preferred editor.

4.  Create a YAML file to configure a pod that can access the service details through a secret volume. If you bound more than one service, verify that each secret is associated with the correct service.

    ```
    apiVersion: apps/v1beta1
    kind: Deployment
    metadata:
      labels:
        app: secret-test
      name: secret-test
      namespace: <my_namespace>
    spec:
      selector:
        matchLabels:
          app: secret-test
      replicas: 1
      template:
        metadata:
          labels:
            app: secret-test
        spec:
          containers:
          - image: nginx
            name: secret-test
            volumeMounts:
            - mountPath: /opt/service-bind
              name: service-bind-volume
          volumes:
          - name: service-bind-volume
            secret:
              defaultMode: 420
              secretName: binding-<service_instance_name>
    ```
    {: codeblock}

    <table>
    <caption>Understanding the YAML file components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>volumeMounts/mountPath</code></td>
    <td>The name of the secret volume that you want to mount to your container.</td>
    </tr>
    <tr>
    <td><code>volumes/name</code></td>
    <td>Enter a name for the secret volume that you want to mount to your container.</td>
    </tr>
    <tr>
    <td><code>secret/defaultMode</code></td>
    <td>Set read-only permissions for the service secret.</td>
    </tr>
    <tr>
    <td><code>secret/secretName</code></td>
    <td>Enter the name of the secret that you noted earlier.</td>
    </tr></tbody></table>

5.  Create the pod and mount the secret volume.

    ```
    kubectl apply -f secret-test.yaml
    ```
    {: pre}

6.  Verify that the pod is created.

    ```
    kubectl get pods --namespace=<my_namespace>
    ```
    {: pre}

    Example CLI output:

    ```
    NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
    ```
    {: screen}

7.  Note the **NAME** of your pod.
8.  Get the details about the pod and look for the secret name.

    ```
    kubectl describe pod <pod_name>
    ```
    {: pre}

    Output:

    ```
    ...
    Volumes:
      service-bind-volume:
        Type:       Secret (a volume populated by a Secret)
        SecretName: binding-<service_instance_name>
    ...
    ```
    {: screen}

    

9.  Configure your apps to find the `binding` secret file in the mount directory, parse the JSON content, and determine the URL and service credentials to access your {{site.data.keyword.Bluemix_notm}} service.

You can now access the {{site.data.keyword.Bluemix_notm}} service details and credentials. To work with your {{site.data.keyword.Bluemix_notm}} service, ensure that your app is configured to find the service secret file in the mount directory, parse the JSON content and determine the service details.

<br />


## Setting up Helm in {{site.data.keyword.containershort_notm}}
{: #helm}

[Helm ![External link icon](../icons/launch-glyph.svg "External link icon")](https://helm.sh) is a Kubernetes package manager. You can create Helm charts or use preexisting Helm charts to define, install, and upgrade complex Kubernetes applications that run in {{site.data.keyword.containerlong_notm}} clusters.
{:shortdesc}

Before you use Helm charts with {{site.data.keyword.containershort_notm}}, you must install and initialize a Helm instance your cluster. You can then add the {{site.data.keyword.Bluemix_notm}} Helm repository to your Helm instance.

Before you begin, [target your CLI](cs_cli_install.html#cs_cli_configure) to the cluster where you want to use a Helm chart.

1. Install the <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm CLI <img src="../icons/launch-glyph.svg" alt="External link icon"></a>.

2. **Important**: To maintain cluster security, create a service account for Tiller in the `kube-system` namespace and a Kubernetes RBAC cluster role binding for the `tiller-deploy` pod.

    1. In your preferred editor, create the following file and save it as `rbac-config.yaml`.
      **Note**:
        * To install Tiller with the service account and cluster role binding in the `kube-system` namespace, you must have the [`cluster-admin` role](cs_users.html#access_policies). You can choose a namespace other than `kube-system`, but all IBM Helm charts must be installed in `kube-system`. Whenever you run a `helm` command, you must use the `tiller-namespace <namespace>` flag to point to the other namespace where Tiller is installed.

      ```
      apiVersion: v1
      kind: ServiceAccount
      metadata:
        name: tiller
        namespace: kube-system
      ---
      apiVersion: rbac.authorization.k8s.io/v1
      kind: ClusterRoleBinding
      metadata:
        name: tiller
      roleRef:
        apiGroup: rbac.authorization.k8s.io
        kind: ClusterRole
        name: cluster-admin
      subjects:
        - kind: ServiceAccount
          name: tiller
          namespace: kube-system
      ```
      {: codeblock}

    2. Create the service account and cluster role binding.

        ```
        kubectl create -f rbac-config.yaml
        ```
        {: pre}

3. Initialize Helm and install `tiller` with the service account that you created.

    ```
    helm init --service-account tiller
    ```
    {: pre}

4. Verify that the `tiller-deploy` pod has a **Status** of `Running` in your cluster.

    ```
    kubectl get pods -n kube-system -l app=helm
    ```
    {: pre}

    Example output:

    ```
    NAME                            READY     STATUS    RESTARTS   AGE
    tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
    ```
    {: screen}

5. Add the {{site.data.keyword.Bluemix_notm}} Helm repository to your Helm instance.

    ```
    helm repo add ibm  https://registry.bluemix.net/helm/ibm
    ```
    {: pre}

6. List the Helm charts that are currently available in the {{site.data.keyword.Bluemix_notm}} repository.

    ```
    helm search ibm
    ```
    {: pre}

7. To learn more about a chart, list its settings and default values.

    For example, to view the settings, documentation, and default values for the strongSwan IPSec VPN service Helm chart:

    ```
    helm inspect ibm/strongswan
    ```
    {: pre}


### Related Helm links
{: #helm_links}

* To use the strongSwan Helm chart, see [Setting up VPN connectivity with the strongSwan IPSec VPN service Helm chart](cs_vpn.html#vpn-setup).
* View the available Helm charts that you can use with {{site.data.keyword.Bluemix_notm}} in the [Helm Charts Catalog ![External link icon](../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/containers-kubernetes/solutions/helm-charts) GUI.
* Learn more about the Helm commands that are used to set up and manage Helm charts in the <a href="https://docs.helm.sh/helm/" target="_blank">Helm documentation <img src="../icons/launch-glyph.svg" alt="External link icon"></a>.
* Learn more about how you can [increase deployment velocity with Kubernetes Helm Charts ![External link icon](../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/recipes/tutorials/increase-deployment-velocity-with-kubernetes-helm-charts/).

## Visualizing Kubernetes cluster resources
{: #weavescope}

Weave Scope provides a visual diagram of your resources within a Kubernetes cluster, including services, pods, containers, and more. Weave Scope provides interactive metrics for CPU and memory and tools to tail and exec into a container.
{:shortdesc}

Before you begin:

-   Remember not to expose your cluster information on the public internet. Complete these steps to deploy Weave Scope securely and access it from a web browser locally.
-   If you do not have one already, [create a standard cluster](cs_clusters.html#clusters_ui). Weave Scope can be CPU intensive, especially the app. Run Weave Scope with larger standard clusters, not free clusters.
-   [Target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster to run `kubectl` commands.


To use Weave Scope with a cluster:
2.  Deploy one of the provided RBAC permissions configuration file in the cluster.

    To enable read/write permissions:

    ```
    kubectl apply -f "https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/weave-scope/weave-scope-rbac.yaml"
    ```
    {: pre}

    To enable read-only permissions:

    ```
    kubectl apply --namespace weave -f "https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/weave-scope/weave-scope-rbac-readonly.yaml"
    ```
    {: pre}

    Output:

    ```
    clusterrole "weave-scope-mgr" created
    clusterrolebinding "weave-scope-mgr-role-binding" created
    ```
    {: screen}

3.  Deploy the Weave Scope service, which is privately accessible by the cluster IP address.

    <pre class="pre">
    <code>kubectl apply --namespace weave -f "https://cloud.weave.works/k8s/scope.yaml?k8s-version=$(kubectl version | base64 | tr -d '&bsol;n')"</code>
    </pre>

    Output:

    ```
    serviceaccount "weave-scope" created
    deployment "weave-scope-app" created
    service "weave-scope-app" created
    daemonset "weave-scope-agent" created
    ```
    {: screen}

4.  Run a port forwarding command to open the service on your computer. The next time that you access Weave Scope, you can run this port forwarding command without completing the previous configuration steps again.

    ```
    kubectl port-forward -n weave "$(kubectl get -n weave pod --selector=weave-scope-component=app -o jsonpath='{.items..metadata.name}')" 4040
    ```
    {: pre}

    Output:

    ```
    Forwarding from 127.0.0.1:4040 -> 4040
    Forwarding from [::1]:4040 -> 4040
    Handling connection for 4040
    ```
    {: screen}

5.  Open your web browser to `http://localhost:4040`. Without the default components deployed, you see the following diagram. You can choose to view topology diagrams or tables of the Kubernetes resources in the cluster.

     <img src="images/weave_scope.png" alt="Example topology from Weave Scope" style="width:357px;" />


[Learn more about the Weave Scope features ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.weave.works/docs/scope/latest/features/).

<br />

