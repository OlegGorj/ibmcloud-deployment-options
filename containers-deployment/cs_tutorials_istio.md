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



# Tutorial: Installing Istio on {{site.data.keyword.containerlong_notm}}
{: #istio_tutorial}

[Istio ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud/info/istio) is an open platform to connect, secure, control, and observe services on cloud platforms such as Kubernetes in {{site.data.keyword.containerlong}}. With Istio, you can manage network traffic, load balance across microservices, enforce access policies, verify service identity, and more.
{:shortdesc}

In this tutorial, you can see how to install Istio alongside four microservices for a simple mock bookstore app called BookInfo. The microservices include a product web page, book details, reviews, and ratings. When you deploy BookInfo's microservices into an {{site.data.keyword.containershort}} cluster where Istio is installed, you inject the Istio Envoy sidecar proxies in the pods of each microservice.

## Objectives

-   Deploy the Istio Helm chart in your cluster
-   Deploy the BookInfo sample app
-   Verify the BookInfo app deployment and round robin through the three versions of the ratings service

## Time required

30 minutes

## Audience

This tutorial is intended for software developers and network administrators who are using Istio for the first time.

## Prerequisites

-  [Install the IBM Cloud CLI, the {{site.data.keyword.containershort_notm}} plug-in, and the Kubernetes CLI](cs_cli_install.html#cs_cli_install_steps). Istio requires the Kubernetes version 1.9 or higher. Make sure to install the `kubectl` CLI version that matches the Kubernetes version of your cluster.
-  [Create a cluster](cs_clusters.html#clusters_cli) with a Kubernetes version of 1.9 or higher.
-  [Target the CLI to your cluster](cs_cli_install.html#cs_cli_configure).

## Lesson 1: Download and install Istio
{: #istio_tutorial1}

Download and install Istio in your cluster.
{:shortdesc}

1. Install Istio by using the [IBM Istio Helm chart ![External link icon](../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/containers-kubernetes/solutions/helm-charts/ibm/ibm-istio).
    1. [Install Helm for your cluster and add the IBM repository to your Helm instance](cs_integrations.html#helm).
    2. Install Istio’s custom resource definitions.
        ```
        kubectl apply -f https://raw.githubusercontent.com/IBM/charts/master/stable/ibm-istio/templates/crds.yaml
        ```
        {: pre}
    3. Install the Helm chart to your cluster.
        ```
        helm install ibm/ibm-istio --name=istio --namespace istio-system
        ```
        {: pre}
    4. Add the `istioctl` client to your PATH. For example, run the following command on a MacOS or Linux system:
       ```
       export PATH=$PWD/istio-1.0/bin:$PATH
       ```
       {: pre}

2. Ensure the pods for the 9 Istio services and for Prometheus are all fully deployed before you continue.
    ```
    kubectl get pods -n istio-system
    ```
    {: pre}

    ```
    NAME                                       READY     STATUS      RESTARTS   AGE
    istio-citadel-748d656b-pj9bw               1/1       Running     0          2m
    istio-egressgateway-6c65d7c98d-l54kg       1/1       Running     0          2m
    istio-galley-65cfbc6fd7-bpnqx              1/1       Running     0          2m
    istio-ingressgateway-f8dd85989-6w6nj       1/1       Running     0          2m
    istio-pilot-5fd885964b-l4df6               2/2       Running     0          2m
    istio-policy-56f4f4cbbd-2z2bk              2/2       Running     0          2m
    istio-sidecar-injector-646655c8cd-rwvsx    1/1       Running     0          2m
    istio-statsd-prom-bridge-7fdbbf769-8k42l   1/1       Running     0          2m
    istio-telemetry-8687d9d745-mwjbf           2/2       Running     0          2m
    prometheus-55c7c698d6-f4drj                1/1       Running     0          2m
    ```
    {: screen}

Good work! You successfully installed Istio into your cluster. Next, deploy the BookInfo sample app into your cluster.


## Lesson 2: Deploy the BookInfo app
{: #istio_tutorial2}

Deploy the BookInfo sample app's microservices to your Kubernetes cluster.
{:shortdesc}

These four microservices include a product web page, book details, reviews (with several versions of the review microservice), and ratings. When you deploy BookInfo, Envoy sidecar proxies are injected as containers into your app microservices' pods before the microservice pods are deployed. Istio uses an extended version of the Envoy proxy to mediate all inbound and outbound traffic for all microservices in the service mesh. For more about Envoy, see the [Istio documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://istio.io/docs/concepts/what-is-istio/overview/#envoy).

1. Download the Istio package containing the necessary BookInfo files.
    1. Either download Istio directly from [https://github.com/istio/istio/releases ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/istio/istio/releases) or get the latest version by using curl:

       ```
       curl -L https://git.io/getLatestIstio | sh -
       ```
       {: pre}

    2. Extract the installation files.

    3. Change the directory to the Istio file location.

       ```
       cd filepath/istio-1.0
       ```
       {: pre}

2. Label the `default` namespace with `istio-injection=enabled`.
    ```
    kubectl label namespace default istio-injection=enabled
    ```
    {: pre}

2. Deploy the BookInfo app. When the app microservices deploy, the Envoy sidecar is also deployed in each microservice pod.

   ```
   kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml
   ```
   {: pre}

3. Ensure that the microservices and their corresponding pods are deployed:
    ```
    kubectl get svc
    ```
    {: pre}

    ```
    NAME                      TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)          AGE
    details                   ClusterIP      172.21.19.104    <none>         9080/TCP         1m
    guestbook                 LoadBalancer   172.21.164.94    169.46.5.163   3000:32135/TCP   1m
    productpage               ClusterIP      172.21.168.196   <none>         9080/TCP         1m
    ratings                   ClusterIP      172.21.11.131    <none>         9080/TCP         1m
    reviews                   ClusterIP      172.21.117.164   <none>         9080/TCP         1m
    ```
    {: screen}

    ```
    kubectl get pods
    ```
    {: pre}

    ```
    NAME                                     READY     STATUS      RESTARTS   AGE
    details-v1-6865b9b99d-7v9h8              2/2       Running     0          2m
    guestbook-76897854cc-6zsws               1/1       Running     0          2m
    guestbook-76897854cc-pcp4v               1/1       Running     0          2m
    guestbook-76897854cc-tlqhs               1/1       Running     0          2m
    productpage-v1-f8c8fb8-tbsz9             2/2       Running     0          2m
    ratings-v1-77f657f55d-png6j              2/2       Running     0          2m
    reviews-v1-6b7f6db5c5-fdmbq              2/2       Running     0          2m
    reviews-v2-7ff5966b99-zflkv              2/2       Running     0          2m
    reviews-v3-5df889bcff-nlmjp              2/2       Running     0          2m
    ```
    {: screen}

4. To verify the app deployment, get the public address for your cluster.
    * Standard clusters:
        1. To expose your app on a public ingress IP, deploy the BookInfo gateway.
            ```
            kubectl apply -f samples/bookinfo/networking/bookinfo-gateway.yaml
            ```
            {: pre}

        2. Set the ingress host.
            ```
            export INGRESS_HOST=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
            ```
            {: pre}

        3. Set the ingress port.
            ```
            export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].port}')
            ```

        4. Create a `GATEWAY_URL` environment variable that uses the ingress host and port.

           ```
           export GATEWAY_URL=$INGRESS_HOST:$INGRESS_PORT
           ```
           {: pre}

    * Free clusters:
        1. Get the public IP address of any worker node in your cluster.
            ```
            ibmcloud ks workers <cluster_name_or_ID>
            ```
            {: pre}

        2. Create a GATEWAY_URL environment variable that uses the public IP address of the worker node.
            ```
            export GATEWAY_URL=<worker_node_public_IP>:$(kubectl get svc istio-ingressgateway -n istio-system -o jsonpath='{.spec.ports[0].nodePort}')
            ```
            {: pre}

5. Curl the `GATEWAY_URL` variable to check that the BookInfo app is running. A `200` response means that the BookInfo app is running properly with Istio.
     ```
     curl -o /dev/null -s -w "%{http_code}\n" http://${GATEWAY_URL}/productpage
     ```
     {: pre}

6. In a browser, go to `http://$GATEWAY_URL/productpage` to view the BookInfo web page.

7. Try refreshing the page several times. Different versions of the reviews section round robin through red stars, black stars, and no stars.

Good work! You successfully deployed the BookInfo sample app with Istio Envoy sidecars. Next, you can clean up your resources or continue with more tutorials to explore Istio further.

## Cleanup
{: #istio_tutorial_cleanup}

If you're finished working with Istio and don't want to [continue exploring](#istio_tutorial_whatsnext), then you can clean up the Istio resources in your cluster.
{:shortdesc}

1. Delete all BookInfo services, pods, and deployments in the cluster.
    ```
    samples/bookinfo/platform/kube/cleanup.sh
    ```
    {: pre}

2. Uninstall the Istio Helm deployment.
    ```
    helm del istio --purge
    ```
    {: pre}

3. Delete the Istio custom resource definitions.
    ```
    kubectl delete -f https://raw.githubusercontent.com/IBM/charts/master/stable/ibm-istio/templates/crds.yaml
    ```
    {: pre}

## What's next?
{: #istio_tutorial_whatsnext}

* To explore Istio further, you can find more guides in the [Istio documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://istio.io/).
    * [Intelligent Routing ![External link icon](../icons/launch-glyph.svg "External link icon")](https://istio.io/docs/guides/intelligent-routing.html): This example shows how to route traffic to a specific version of BookInfo's reviews and ratings microservices by using Istio's traffic management capabilities.
    * [In-Depth Telemetry ![External link icon](../icons/launch-glyph.svg "External link icon")](https://istio.io/docs/guides/telemetry.html): This example includes how to get uniform metrics, logs, and traces across BookInfo's microservices by using Istio Mixer and the Envoy proxy.
* Take the [Cognitive Class: Getting started with Microservices with Istio and IBM Cloud Kubernetes Service ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cognitiveclass.ai/courses/get-started-with-microservices-istio-and-ibm-cloud-container-service/). **Note**: You can skip the Istio installation section of this course.
* Check out this blog post on using [Vistio ![External link icon](../icons/launch-glyph.svg "External link icon")](https://itnext.io/vistio-visualize-your-istio-mesh-using-netflixs-vizceral-b075c402e18e) to visualize your Istio service mesh.
