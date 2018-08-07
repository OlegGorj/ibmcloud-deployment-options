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



# Troubleshooting cluster networking
{: #cs_troubleshoot_network}

As you use {{site.data.keyword.containerlong}}, consider these techniques for troubleshooting cluster networking.
{: shortdesc}

If you have a more general issue, try out [cluster debugging](cs_troubleshoot.html).
{: tip}


## Cannot connect to an app via a load balancer service
{: #cs_loadbalancer_fails}

{: tsSymptoms}
You publicly exposed your app by creating a load balancer service in your cluster. When you tried to connect to your app by using the public IP address of the load balancer, the connection failed or timed out.

{: tsCauses}
Your load balancer service might not be working properly for one of the following reasons:

-   The cluster is a free cluster or a standard cluster with only one worker node.
-   The cluster is not fully deployed yet.
-   The configuration script for your load balancer service includes errors.

{: tsResolve}
To troubleshoot your load balancer service:

1.  Check that you set up a standard cluster that is fully deployed and has at least two worker nodes to assure high availability for your load balancer service.

  ```
  ibmcloud ks workers <cluster_name_or_ID>
  ```
  {: pre}

    In your CLI output, make sure that the **Status** of your worker nodes displays **Ready** and that the **Machine Type** shows a machine type other than **free**.

2.  Check the accuracy of the configuration file for your load balancer service.

    ```
    apiVersion: v1
    kind: Service
    metadata:
      name: myservice
    spec:
      type: LoadBalancer
      selector:
        <selector_key>:<selector_value>
      ports:
       - protocol: TCP
         port: 8080
    ```
    {: pre}

    1.  Check that you defined **LoadBalancer** as the type for your service.
    2.  In the `spec.selector` section of the LoadBalancer service, ensure that the `<selector_key>` and `<selector_value>` is the same as the key/value pair that you used in the `spec.template.metadata.labels` section of your deployment yaml. If labels do not match, the **Endpoints** section in your LoadBalancer service displays **<none>** and your app is not accessible from the internet.
    3.  Check that you used the **port** that your app listens on.

3.  Check your load balancer service and review the **Events** section to find potential errors.

    ```
    kubectl describe service <myservice>
    ```
    {: pre}

    Look for the following error messages:

    <ul><li><pre class="screen"><code>Clusters with one node must use services of type NodePort</code></pre></br>To use the load balancer service, you must have a standard cluster with at least two worker nodes.</li>
    <li><pre class="screen"><code>No cloud provider IPs are available to fulfill the load balancer service request. Add a portable subnet to the cluster and try again</code></pre></br>This error message indicates that no portable public IP addresses are left to be allocated to your load balancer service. Refer to <a href="cs_subnets.html#subnets">Adding subnets to clusters</a> to find information about how to request portable public IP addresses for your cluster. After portable public IP addresses are available to the cluster, the load balancer service is automatically created.</li>
    <li><pre class="screen"><code>Requested cloud provider IP <cloud-provider-ip> is not available. The following cloud provider IPs are available: <available-cloud-provider-ips></code></pre></br>You defined a portable public IP address for your load balancer service by using the **loadBalancerIP** section, but this portable public IP address is not available in your portable public subnet. In the **loadBalancerIP** section your configuration script, remove the existing IP address and add one of the available portable public IP addresses. You can also remove the **loadBalancerIP** section from your script so that an available portable public IP address can be allocated automatically.</li>
    <li><pre class="screen"><code>No available nodes for load balancer services</code></pre>You do not have enough worker nodes to deploy a load balancer service. One reason might be that you deployed a standard cluster with more than one worker node, but the provisioning of the worker nodes failed.</li>
    <ol><li>List available worker nodes.</br><pre class="codeblock"><code>kubectl get nodes</code></pre></li>
    <li>If at least two available worker nodes are found, list the worker node details.</br><pre class="codeblock"><code>ibmcloud ks worker-get [&lt;cluster_name_or_ID&gt;] &lt;worker_ID&gt;</code></pre></li>
    <li>Make sure that the public and private VLAN IDs for the worker nodes that were returned by the <code>kubectl get nodes</code> and the <code>ibmcloud ks [&lt;cluster_name_or_ID&gt;] worker-get</code> commands match.</li></ol></li></ul>

4.  If you are using a custom domain to connect to your load balancer service, make sure that your custom domain is mapped to the public IP address of your load balancer service.
    1.  Find the public IP address of your load balancer service.

        ```
        kubectl describe service <service_name> | grep "LoadBalancer Ingress"
        ```
        {: pre}

    2.  Check that your custom domain is mapped to the portable public IP address of your load balancer service in the Pointer record (PTR).

<br />




## Cannot connect to an app via Ingress
{: #cs_ingress_fails}

{: tsSymptoms}
You publicly exposed your app by creating an Ingress resource for your app in your cluster. When you tried to connect to your app by using the public IP address or subdomain of the Ingress application load balancer (ALB), the connection failed or timed out.

{: tsCauses}
Ingress might not be working properly for the following reasons:
<ul><ul>
<li>The cluster is not fully deployed yet.
<li>The cluster was set up as a free cluster or as a standard cluster with only one worker node.
<li>The Ingress configuration script includes errors.
</ul></ul>

{: tsResolve}
To troubleshoot your Ingress:

1.  Check that you set up a standard cluster that is fully deployed and has at least two worker nodes to assure high availability for your ALB.

  ```
  ibmcloud ks workers <cluster_name_or_ID>
  ```
  {: pre}

    In your CLI output, make sure that the **Status** of your worker nodes displays **Ready** and that the **Machine Type** shows a machine type other than **free**.

2.  Retrieve the ALB subdomain and public IP address, and then ping each one.

    1.  Retrieve the ALB subdomain.

      ```
      ibmcloud ks cluster-get <cluster_name_or_ID> | grep "Ingress subdomain"
      ```
      {: pre}

    2.  Ping the ALB subdomain.

      ```
      ping <ingress_subdomain>
      ```
      {: pre}

    3.  Retrieve the public IP address of your ALB.

      ```
      nslookup <ingress_subdomain>
      ```
      {: pre}

    4.  Ping the ALB public IP address.

      ```
      ping <ALB_IP>
      ```
      {: pre}

    If the CLI returns a timeout for the public IP address or subdomain of the ALB, and you have set up a custom firewall that is protecting your worker nodes, open more ports and networking groups in your [firewall](cs_troubleshoot_clusters.html#cs_firewall).

3.  If you are using a custom domain, make sure that your custom domain is mapped to the public IP address or subdomain of the IBM-provided ALB with your DNS provider.
    1.  If you used the ALB subdomain, check your Canonical Name record (CNAME).
    2.  If you used the ALB public IP address, check that your custom domain is mapped to the portable public IP address in the Pointer record (PTR).
4.  Check your Ingress resource configuration file.

    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingress
    spec:
      tls:
      - hosts:
        - <ingress_subdomain>
        secretName: <ingress_tls_secret>
      rules:
      - host: <ingress_subdomain>
        http:
          paths:
          - path: /
            backend:
              serviceName: myservice
              servicePort: 80
    ```
    {: codeblock}

    1.  Check that the ALB subdomain and TLS certificate are correct. To find the IBM provided subdomain and TLS certificate, run `ibmcloud ks cluster-get <cluster_name_or_ID>`.
    2.  Make sure that your app listens on the same path that is configured in the **path** section of your Ingress. If your app is set up to listen on the root path, include **/** as your path.
5.  Check your Ingress deployment and look for potential warning or error messages.

    ```
    kubectl describe ingress <myingress>
    ```
    {: pre}

    For example, in the **Events** section of the output, you might see warning messages about invalid values in your Ingress resource or in certain annotations you used.

    ```
    Name:             myingress
    Namespace:        default
    Address:          169.xx.xxx.xxx,169.xx.xxx.xxx
    Default backend:  default-http-backend:80 (<none>)
    Rules:
      Host                                             Path  Backends
      ----                                             ----  --------
      mycluster.us-south.containers.appdomain.cloud
                                                       /tea      myservice1:80 (<none>)
                                                       /coffee   myservice2:80 (<none>)
    Annotations:
      custom-port:        protocol=http port=7490; protocol=https port=4431
      location-modifier:  modifier='~' serviceName=myservice1;modifier='^~' serviceName=myservice2
    Events:
      Type     Reason             Age   From                                                            Message
      ----     ------             ----  ----                                                            -------
      Normal   Success            1m    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  TLSSecretNotFound  1m    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress resource.
      Normal   Success            59s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  AnnotationError    40s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress.bluemix.net/custom-port annotation. Error annotation format error : One of the mandatory fields not valid/missing for annotation ingress.bluemix.net/custom-port
      Normal   Success            40s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  AnnotationError    2s    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress.bluemix.net/custom-port annotation. Invalid port 7490. Annotation cannot use ports 7481 - 7490
      Normal   Success            2s    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
    ```
    {: screen}

6.  Check the logs for your ALB.
    1.  Retrieve the ID of the Ingress pods that are running in your cluster.

      ```
      kubectl get pods -n kube-system | grep alb
      ```
      {: pre}

    2.  Retrieve the logs for each Ingress pod.

      ```
      kubectl logs <ingress_pod_ID> nginx-ingress -n kube-system
      ```
      {: pre}

    3.  Look for error messages in the ALB logs.

<br />


## Ingress application load balancer secret issues
{: #cs_albsecret_fails}

{: tsSymptoms}
After you deploy an Ingress application load balancer (ALB) secret to your cluster, the `Description` field is not updating with the secret name when you view your certificate in {{site.data.keyword.cloudcerts_full_notm}}.

When you list information about the ALB secret, the status says `*_failed`. For example, `create_failed`, `update_failed`, `delete_failed`.

{: tsResolve}
Review the following reasons why the ALB secret might fail and the corresponding troubleshooting steps:

<table>
<caption>Troubleshooting Ingress application load balancer secrets</caption>
 <thead>
 <th>Why it's happening</th>
 <th>How to fix it</th>
 </thead>
 <tbody>
 <tr>
 <td>You do not have the required access roles to download and update certificate data.</td>
 <td>Check with your account Administrator to assign you both the **Manager** and **Writer** roles for your {{site.data.keyword.cloudcerts_full_notm}} instance. For more information, see <a href="/docs/services/certificate-manager/access-management.html#managing-service-access-roles">Managing service access</a> for {{site.data.keyword.cloudcerts_short}}.</td>
 </tr>
 <tr>
 <td>The certificate CRN provided at time of create, update, or remove does not belong to the same account as the cluster.</td>
 <td>Check that the certificate CRN you provided is imported to an instance of the {{site.data.keyword.cloudcerts_short}} service that is deployed in the same account as your cluster.</td>
 </tr>
 <tr>
 <td>The certificate CRN provided at time of create is incorrect.</td>
 <td><ol><li>Check the accuracy of the certificate CRN string you provide.</li><li>If the certificate CRN is found to be accurate, then try to update the secret: <code>ibmcloud ks alb-cert-deploy --update --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li><li>If this command results in the <code>update_failed</code> status, then remove the secret: <code>ibmcloud ks alb-cert-rm --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt;</code></li><li>Deploy the secret again: <code>ibmcloud ks alb-cert-deploy --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li></ol></td>
 </tr>
 <tr>
 <td>The certificate CRN provided at time of update is incorrect.</td>
 <td><ol><li>Check the accuracy of the certificate CRN string you provide.</li><li>If the certificate CRN is found to be accurate, then remove the secret: <code>ibmcloud ks alb-cert-rm --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt;</code></li><li>Deploy the secret again: <code>ibmcloud ks alb-cert-deploy --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li><li>Try to update the secret: <code>ibmcloud ks alb-cert-deploy --update --cluster &lt;cluster_name_or_ID&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li></ol></td>
 </tr>
 <tr>
 <td>The {{site.data.keyword.cloudcerts_long_notm}} service is experiencing downtime.</td>
 <td>Check that your {{site.data.keyword.cloudcerts_short}} service is up and running.</td>
 </tr>
 </tbody></table>

<br />


## Cannot get a subdomain for Ingress ALB
{: #cs_subnet_limit}

{: tsSymptoms}
When you run `ibmcloud ks cluster-get <cluster>`, your cluster is in a `normal` state but no **Ingress Subdomain** is available.

You might see an error message similar to the following.

```
There are already the maximum number of subnets permitted in this VLAN.
```
{: screen}

{: tsCauses}
In standard clusters, the first time that you create a cluster in a zone, a public VLAN and a private VLAN in that zone are automatically provisioned for you in your IBM Cloud infrastructure (SoftLayer) account. In that zone, 1 public portable subnet is requested on the public VLAN that you specify and 1 private portable subnet is requested on the private VLAN that you specify. For {{site.data.keyword.containershort_notm}}, VLANs have a limit of 40 subnets. If the cluster's VLAN in a zone already reached that limit, the **Ingress Subdomain** fails to provision.

To view how many subnets a VLAN has:
1.  From the [IBM Cloud infrastructure (SoftLayer) console](https://control.bluemix.net/), select **Network** > **IP Management** > **VLANs**.
2.  Click the **VLAN Number** of the VLAN that you used to create your cluster. Review the **Subnets** section to see whether 40 or more subnets exist.

{: tsResolve}
If you need a new VLAN, order one by [contacting {{site.data.keyword.Bluemix_notm}} support](/docs/infrastructure/vlans/order-vlan.html#order-vlans). Then, [create a cluster](cs_cli_reference.html#cs_cluster_create) that uses this new VLAN.

If you have another VLAN that is available, you can [set up VLAN spanning](/docs/infrastructure/vlans/vlan-spanning.html#vlan-spanning) in your existing cluster. After, you can add new worker nodes to the cluster that use the other VLAN with available subnets.

If you are not using all the subnets in the VLAN, you can reuse subnets in the cluster.
1.  Check that the subnets that you want to use are available. **Note**: The infrastructure account that you are using might be shared across multiple {{site.data.keyword.Bluemix_notm}} accounts. If so, even if you run the `ibmcloud ks subnets` command to see subnets with **Bound Clusters**, you can see information only for your clusters. Check with the infrastructure account owner to make sure that the subnets are available and not in use by any other account or team.

2.  [Create a cluster](cs_cli_reference.html#cs_cluster_create) with the `--no-subnet` option so that the service does not try to create new subnets. Specify the zone and VLAN that has the subnets that are available for reuse.

3.  Use the `ibmcloud ks cluster-subnet-add` [command](cs_cli_reference.html#cs_cluster_subnet_add) to add existing subnets to your cluster. For more information, see [Adding or reusing custom and existing subnets in Kubernetes clusters](cs_subnets.html#custom).

<br />


## Ingress ALB does not deploy in a zone
{: #cs_multizone_subnet_limit}

{: tsSymptoms}
When you have a multizone cluster and run `ibmcloud ks albs <cluster>`, no ALB is deployed in a zone. For example, if you have worker nodes in 3 zones, you might see an output similar to the following in which a public ALB did not deploy to the third zone.
```
ALB ID                                            Enabled   Status     Type      ALB IP   
private-cr96039a75fddb4ad1a09ced6699c88888-alb1   false     disabled   private   -   
private-cr96039a75fddb4ad1a09ced6699c88888-alb2   false     disabled   private   -   
private-cr96039a75fddb4ad1a09ced6699c88888-alb3   false     disabled   private   -   
public-cr96039a75fddb4ad1a09ced6699c88888-alb1    true      enabled    public    169.xx.xxx.xxx
public-cr96039a75fddb4ad1a09ced6699c88888-alb2    true      enabled    public    169.xx.xxx.xxx
```
{: screen}

{: tsCauses}
In each zone, 1 public portable subnet is requested on the public VLAN that you specify and 1 private portable subnet is requested on the private VLAN that you specify. For {{site.data.keyword.containershort_notm}}, VLANs have a limit of 40 subnets. If the cluster's public VLAN in a zone already reached that limit, the public Ingress ALB for that zone fails to provision.

{: tsResolve}
To check the number of subnets on a VLAN and for steps on how to get another VLAN, see [Cannot get a subdomain for Ingress ALB](#cs_subnet_limit).

<br />


## Cannot establish VPN connectivity with the strongSwan Helm chart
{: #cs_vpn_fails}

{: tsSymptoms}
When you check VPN connectivity by running `kubectl exec -n kube-system  $STRONGSWAN_POD -- ipsec status`, you do not see a status of `ESTABLISHED`, or the VPN pod is in an `ERROR` state or continues to crash and restart.

{: tsCauses}
Your Helm chart configuration file has incorrect values, missing values, or syntax errors.

{: tsResolve}
When you try to establish VPN connectivity with the strongSwan Helm chart, it is likely that the VPN status will not be `ESTABLISHED` the first time. You might need to check for several types of issues and change your configuration file accordingly. To troubleshoot your strongSwan VPN connectivity:

1. Check the on-prem VPN endpoint settings against the settings in your configuration file. If the settings don't match:

    <ol>
    <li>Delete the existing Helm chart.</br><pre class="codeblock"><code>helm delete --purge <release_name></code></pre></li>
    <li>Fix the incorrect values in the <code>config.yaml</code> file and save the updated file.</li>
    <li>Install the new Helm chart.</br><pre class="codeblock"><code>helm install -f config.yaml --namespace=kube-system --name=<release_name> bluemix/strongswan</code></pre></li>
    </ol>

2. If the VPN pod is in an `ERROR` state or continues to crash and restart, it might be due to parameter validation of the `ipsec.conf` settings in the chart's config map.

    <ol>
    <li>Check for any validation errors in the strongSwan pod logs.</br><pre class="codeblock"><code>kubectl logs -n kube-system $STRONGSWAN_POD</code></pre></li>
    <li>If the logs contain validation errors, delete the existing Helm chart.</br><pre class="codeblock"><code>helm delete --purge <release_name></code></pre></li>
    <li>Fix the incorrect values in the `config.yaml` file and save the updated file.</li>
    <li>Install the new Helm chart.</br><pre class="codeblock"><code>helm install -f config.yaml --namespace=kube-system --name=<release_name> bluemix/strongswan</code></pre></li>
    </ol>

3. Run the 5 Helm tests included in the strongSwan chart definition.

    <ol>
    <li>Run the Helm tests.</br><pre class="codeblock"><code>helm test vpn</code></pre></li>
    <li>If any test fails, refer to [Understanding the Helm VPN connectivity tests](cs_vpn.html#vpn_tests_table) for information about each test and why it might fail. <b>Note</b>: Some of the tests have requirements that are optional settings in the VPN configuration. If some of the tests fail, the failures might be acceptable depending on whether you specified these optional settings.</li>
    <li>View the output of a failed test by looking at the logs of the test pod.<br><pre class="codeblock"><code>kubectl logs -n kube-system <test_program></code></pre></li>
    <li>Delete the existing Helm chart.</br><pre class="codeblock"><code>helm delete --purge <release_name></code></pre></li>
    <li>Fix the incorrect values in the <code>config.yaml</code> file and save the updated file.</li>
    <li>Install the new Helm chart.</br><pre class="codeblock"><code>helm install -f config.yaml --namespace=kube-system --name=<release_name> bluemix/strongswan</code></pre></li>
    <li>To check your changes:<ol><li>Get the current test pods.</br><pre class="codeblock"><code>kubectl get pods -a -n kube-system -l app=strongswan-test</code></pre></li><li>Clean up the current test pods.</br><pre class="codeblock"><code>kubectl delete pods -n kube-system -l app=strongswan-test</code></pre></li><li>Run the tests again.</br><pre class="codeblock"><code>helm test vpn</code></pre></li>
    </ol></ol>

4. Run the VPN debugging tool that is packaged inside of the VPN pod image.

    1. Set the `STRONGSWAN_POD` environment variable.

        ```
        export STRONGSWAN_POD=$(kubectl get pod -n kube-system -l app=strongswan,release=vpn -o jsonpath='{ .items[0].metadata.name }')
        ```
        {: pre}

    2. Run the debugging tool.

        ```
        kubectl exec -n kube-system  $STRONGSWAN_POD -- vpnDebug
        ```
        {: pre}

        The tool outputs several pages of information as it runs various tests for common networking issues. Output lines that begin with `ERROR`, `WARNING`, `VERIFY`, or `CHECK` indicate possible errors with the VPN connectivity.

    <br />


## Cannot install a new strongSwan Helm chart release
{: #cs_strongswan_release}

{: tsSymptoms}
You modify your strongSwan Helm chart and try to install your new release by running `helm install -f config.yaml --namespace=kube-system --name=<new_release_name> bluemix/strongswan`. However, you see the following error:
```
Error: release <new_release_name> failed: deployments.extensions "vpn-strongswan" already exists
```
{: screen}

{: tsCauses}
This error indicates that the previous release of the strongSwan chart was not completely uninstalled.

{: tsResolve}

1. Delete the previous chart release.
    ```
    helm delete --purge <old_release_name>
    ```
    {: pre}

2. Delete the deployment for the previous release. Deletion of the deployment and associated pod takes up to 1 minute.
    ```
    kubectl delete deploy -n kube-system vpn-strongswan
    ```
    {: pre}

3. Verify that the deployment has been deleted. The deployment `vpn-strongswan` does not appear in the list.
    ```
    kubectl get deployments -n kube-system
    ```
    {: pre}

4. Re-install the updated strongSwan Helm chart with a new release name.
    ```
    helm install -f config.yaml --namespace=kube-system --name=<new_release_name> bluemix/strongswan
    ```
    {: pre}

<br />


## strongSwan VPN connectivity fails after worker node addition or deletion
{: #cs_vpn_fails_worker_add}

{: tsSymptoms}
You previously established a working VPN connection by using the strongSwan IPSec VPN service. However, after you added or deleted a worker node on your cluster, you experience one or more of the following symptoms:

* You do not have a VPN status of `ESTABLISHED`
* You cannot access new worker nodes from your on-prem network
* You cannot access the remote network from pods that are running on new worker nodes

{: tsCauses}
If you added a worker node to a worker pool:

* The worker node was provisioned on a new private subnet that is not exposed over the VPN connection by your existing `localSubnetNAT` or `local.subnet` settings
* VPN routes cannot be added to the worker node because the worker has taints or labels that are not included in your existing `tolerations` or `nodeSelector` settings
* The VPN pod is running on the new worker node, but the public IP address of that worker node is not allowed through the on-premises firewall

If you deleted a worker node:

* That worker node was the only node where a VPN pod was running, due to restrictions on certain taints or labels in your existing `tolerations` or `nodeSelector` settings

{: tsResolve}
Update the Helm chart values to reflect the worker node changes:

1. Delete the existing Helm chart.

    ```
    helm delete --purge <release_name>
    ```
    {: pre}

2. Open the configuration file for your strongSwan VPN service.

    ```
    helm inspect values ibm/strongswan > config.yaml
    ```
    {: pre}

3. Check the following settings and change the settings to reflect the deleted or added worker nodes as necessary.

    If you added a worker node:

    <table>
    <caption>Worker node settings</caption?
     <thead>
     <th>Setting</th>
     <th>Description</th>
     </thead>
     <tbody>
     <tr>
     <td><code>localSubnetNAT</code></td>
     <td>The added worker might be deployed on a new, different private subnet than the other existing subnets that other worker nodes are on. If you are using subnet NAT to remap your cluster's private local IP addresses and the worker was added on a new subnet, add the new subnet CIDR to this setting.</td>
     </tr>
     <tr>
     <td><code>nodeSelector</code></td>
     <td>If you previously limited VPN pod deployment to workers with a specific label, ensure the added worker node also has that label.</td>
     </tr>
     <tr>
     <td><code>tolerations</code></td>
     <td>If the added worker node is tainted, then change this setting to allow the VPN pod to run on all tainted workers with any taints or specific taints.</td>
     </tr>
     <tr>
     <td><code>local.subnet</code></td>
     <td>The added worker might be deployed on a new, different private subnet than the existing subnets that other workers are on. If your apps are exposed by NodePort or LoadBalancer services on the private network and the apps are on the added worker, add the new subnet CIDR to this setting. **Note**: If you add values to `local.subnet`, check the VPN settings for the on-premises subnet to see whether they also must be updated.</td>
     </tr>
     </tbody></table>

    If you deleted a worker node:

    <table>
    <caption>Worker node settings</caption>
     <thead>
     <th>Setting</th>
     <th>Description</th>
     </thead>
     <tbody>
     <tr>
     <td><code>localSubnetNAT</code></td>
     <td>If you are using subnet NAT to remap specific private local IP addresses, remove any IP addresses from this setting that are from the old worker. If you are using subnet NAT to remap entire subnets and no workers remain on a subnet, remove that subnet CIDR from this setting.</td>
     </tr>
     <tr>
     <td><code>nodeSelector</code></td>
     <td>If you previously limited VPN pod deployment to a single worker and that worker was deleted, change this setting to allow the VPN pod to run on other workers.</td>
     </tr>
     <tr>
     <td><code>tolerations</code></td>
     <td>If the worker that you deleted was not tainted, but the only workers that remain are tainted, change this setting to allow the VPN pod to run on workers with any taints or specific taints.
     </td>
     </tr>
     </tbody></table>

4. Install the new Helm chart with your updated values.

    ```
    helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/strongswan
    ```
    {: pre}

5. Check the chart deployment status. When the chart is ready, the **STATUS** field near the top of the output has a value of `DEPLOYED`.

    ```
    helm status <release_name>
    ```
    {: pre}

6. In some cases, you might need to change your on-prem settings and your firewall settings to match the changes you made to the VPN config file.

7. Start the VPN.
    * If the VPN connection is initiated by the cluster (`ipsec.auto` is set to `start`), start the VPN on the on-prem gateway, and then start the VPN on the cluster.
    * If the VPN connection is initiated by the on-prem gateway (`ipsec.auto` is set to `auto`), start the VPN on the cluster, and then start the VPN on the on-prem gateway.

8. Set the `STRONGSWAN_POD` environment variable.

    ```
    export STRONGSWAN_POD=$(kubectl get pod -n kube-system -l app=strongswan,release=<release_name> -o jsonpath='{ .items[0].metadata.name }')
    ```
    {: pre}

9. Check the status of the VPN.

    ```
    kubectl exec -n kube-system  $STRONGSWAN_POD -- ipsec status
    ```
    {: pre}

    * If the VPN connection has a status of `ESTABLISHED`, the VPN connection was successful. No further action is needed.

    * If you are still having connection issues, see [Cannot establish VPN connectivity with the strongSwan Helm chart](#cs_vpn_fails) to further troubleshoot your VPN connection.

<br />



## Cannot retrieve Calico network policies
{: #cs_calico_fails}

{: tsSymptoms}
When you try to view Calico network policies in your cluster by running `calicoctl get policy`, you get one of the following unexpected results or error messages:
- An empty list
- A list of old Calico v2 policies instead of v3 policies
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'projectcalico.org/v3'`

When you try to view Calico network policies in your cluster by running `calicoctl get GlobalNetworkPolicy`, you get one of the following unexpected results or error messages:
- An empty list
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'v1'`
- `Failed to create Calico API client: syntax error in calicoctl.cfg: invalid config file: unknown APIVersion 'projectcalico.org/v3'`
- `Failed to get resources: Resource type 'GlobalNetworkPolicy' is not supported`

{: tsCauses}
To use Calico policies, four factors must all align: your cluster Kubernetes version, Calico CLI version, Calico config file syntax, and view policy commands. One or more of these factors is not at the correct version.

{: tsResolve}
When your cluster is at [Kubernetes version 1.10 or later](cs_versions.html), you must use Calico CLI v3.1, `calicoctl.cfg` v3 config file syntax, and the `calicoctl get GlobalNetworkPolicy` and `calicoctl get NetworkPolicy` commands.

When your cluster is at [Kubernetes version 1.9 or earlier](cs_versions.html), you must use Calico CLI v1.6.3, `calicoctl.cfg` v2 config file syntax, and the `calicoctl get policy` command.

To ensure that all Calico factors align:

1. View your cluster Kubernetes version.
    ```
    ibmcloud ks cluster-get <cluster_name>
    ```
    {: pre}

    * If your cluster is at Kubernetes version 1.10 or later:
        1. [Install and configure the version 3.1.1 Calico CLI](cs_network_policy.html#1.10_install). Configuration includes manually updating the `calicoctl.cfg` file to use Calico v3 syntax.
        2. Ensure that any policies you create and want to apply to your cluster use [Calico v3 syntax ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.projectcalico.org/v3.1/reference/calicoctl/resources/networkpolicy). If you have an existing policy `.yaml` or `.json` file in Calico v2 syntax, you can convert it to Calico v3 syntax by using the [`calicoctl convert` command ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.projectcalico.org/v3.1/reference/calicoctl/commands/convert).
        3. To [view policies](cs_network_policy.html#1.10_examine_policies), ensure that you are using `calicoctl get GlobalNetworkPolicy` for global policies and `calicoctl get NetworkPolicy --namespace <policy_namespace>` for policies that are scoped to specific namespaces.

    * If your cluster is at Kubernetes version 1.9 or earlier:
        1. [Install and configure the version 1.6.3 Calico CLI](cs_network_policy.html#1.9_install). Ensure that the `calicoctl.cfg` file uses Calico v2 syntax.
        2. Ensure that any policies you create and want to apply to your cluster use [Calico v2 syntax ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.projectcalico.org/v2.6/reference/calicoctl/resources/policy).
        3. To [view policies](cs_network_policy.html#1.9_examine_policies), ensure that you are using `calicoctl get policy`.

Before you update your cluster from Kubernetes version 1.9 or earlier to version 1.10 or later, review [Preparing to update to Calico v3](cs_versions.html#110_calicov3).
{: tip}

<br />


## Cannot add worker nodes due to an invalid VLAN ID
{: #suspended}

{: tsSymptoms}
Your {{site.data.keyword.Bluemix_notm}} account was suspended, or all worker nodes in your cluster were deleted. After the account is reactivated, you cannot add worker nodes when you try to resize or rebalance your worker pool. You see an error message similar to the following:

```
SoftLayerAPIError(SoftLayer_Exception_Public): Could not obtain network VLAN with id #123456.
```
{: screen}

{: tsCauses}
When an account is suspended, the worker nodes within the account are deleted. If a cluster has no worker nodes, IBM Cloud infrastructure (SoftLayer) reclaims the associated public and private VLANs. However, the cluster worker pool still has the previous VLAN IDs in its metadata and uses these unavailable IDs when you rebalance or resize the pool. The nodes fail to create because the VLANs are no longer associated with the cluster.

{: tsResolve}

You can [delete your existing worker pool](cs_cli_reference.html#cs_worker_pool_rm), then [create a new worker pool](cs_cli_reference.html#cs_worker_pool_create).

Alternatively, you can keep your existing worker pool by ordering new VLANs and using these to create new worker nodes in the pool.

Before you begin, [target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster.

1.  To get the zones that you need new VLAN IDs for, note the **Location** in the following command output. **Note**: If your cluster is a multizone, you need VLAN IDs for each zone.

    ```
    ibmcloud ks clusters
    ```
    {: pre}

2.  Get a new private and public VLAN for each zone that your cluster is in by [contacting {{site.data.keyword.Bluemix_notm}} support](/docs/infrastructure/vlans/order-vlan.html#order-vlans).

3.  Note the new private and public VLAN IDs for each zone.

4.  Note the name of your worker pools.

    ```
    ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
    ```
    {: pre}

5.  Use the `zone-network-set` [command](cs_cli_reference.html#cs_zone_network_set) to change the worker pool network metadata.

    ```
    ibmcloud ks zone-network-set --zone <zone> --cluster <cluster_name_or_ID> -- worker-pools <worker-pool> --private-vlan <private_vlan_ID> --public-vlan <public_vlan_ID>
    ```
    {: pre}

6.  **Multizone cluster only**: Repeat **Step 5** for each zone in your cluster.

7.  Rebalance or resize your worker pool to add worker nodes that use the new VLAN IDs. For example:

    ```
    ibmcloud ks worker-pool-resize --cluster <cluster_name_or_ID> --worker-pool <worker_pool> --size-per-zone <number_of_workers_per_zone>
    ```
    {: pre}

8.  Verify that your worker nodes are created.

    ```
    ibmcloud ks workers <cluster_name_or_ID> --worker-pool <worker_pool>
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

