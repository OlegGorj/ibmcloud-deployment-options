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



# Tutorial: Using Calico network policies to block traffic
{: #policy_tutorial}

By default, Kubernetes NodePort, LoadBalancer, and Ingress services make your app available on all public and private cluster network interfaces. The `allow-node-port-dnat` default Calico policy permits incoming traffic from NodePort, LoadBalancer, and Ingress services to the app pods that those services expose. Kubernetes uses destination network address translation (DNAT) to forward service requests to the correct pods.

However, for security reasons, you might need to allow traffic to the networking services from certain source IP addresses only. You can use [Calico Pre-DNAT policies ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.projectcalico.org/v3.1/getting-started/bare-metal/policy/pre-dnat) to whitelist or blacklist traffic from or to certain IP addresses. Pre-DNAT policies prevent specified traffic from reaching your apps because they are applied before Kubernetes uses regular DNAT to forward traffic to pods. When you create Calico Pre-DNAT policies, you choose whether to whitelist or blacklist source IP addresses. For most scenarios, whitelisting provides the most secure configuration because all traffic is blocked except traffic from known, permitted source IP addresses. Blacklisting is typically useful only in scenarios such as preventing an attack from a small set of IP addresses.

In this scenario, you play the role of a networking administrator for a PR firm, and you notice some unusual traffic hitting your apps. The lessons in this tutorial walk you through creating a sample web server app, exposing the app by using a LoadBalancer service, and protecting the app from unwanted unusual traffic with both whitelist and blacklist Calico policies.

## Objectives

- Learn to block all incoming traffic to all NodePorts by creating a high-order Pre-DNAT policy.
- Learn to allow whitelisted source IP addresses to access the LoadBalancer public IP and port by creating a low-order Pre-DNAT policy. Lower order policies override higher-order policies.
- Learn to block blacklisted source IP addresses from accessing the LoadBalancer public IP and port by creating a low-order Pre-DNAT policy.

## Time required
1 hour

## Audience
This tutorial is intended for software developers and network administrators who want to manage network traffic to an app.

## Prerequisites

- [Create a version 1.10 cluster](cs_clusters.html#clusters_ui) or [update an existing cluster to version 1.10](cs_versions.html#cs_v110). A Kubernetes version 1.10 or later cluster is required to use the 3.1.1 Calico CLI and Calico v3 policy syntax in this tutorial.
- [Target your CLI to the cluster](cs_cli_install.html#cs_cli_configure).
- [Install and configure the Calico CLI](cs_network_policy.html#1.10_install).

<br />


## Lesson 1: Deploy an app and expose it by using a LoadBalancer
{: #lesson1}

The first lesson shows you how your app is exposed from multiple IP addresses and ports, and where public traffic is coming into your cluster.
{: shortdesc}

Start by deploying a sample web server app to use throughout the tutorial. The `echoserver` web server shows data about the connection being made to the cluster from the client, and lets you test access to the PR firm's cluster. Then, expose the app by creating a LoadBalancer service. A LoadBalancer service makes your app available over both the LoadBalancer service IP address and the worker nodes' NodePorts.

The following image shows how the webserver app will be exposed to the internet by the public NodePort and public LoadBalancer at the end of Lesson 1:

<img src="images/cs_tutorial_policies_Lesson1.png" width="450" alt="At the end of Lesson 1, the webserver app is exposed to the internet by the public NodePort and public LoadBalancer." style="width:450px; border-style: none"/>

1. Create a test namespace called `pr-firm` to use throughout this tutorial.
    ```
    kubectl create ns pr-firm
    ```
    {: pre}

2. Deploy the sample web server app. When a connection is made to the web server app, the app responds with the HTTP headers it received in the connection.
    ```
    kubectl run webserver -n pr-firm --image=k8s.gcr.io/echoserver:1.10 --replicas=3
    ```
    {: pre}

3. Verify that the web server app pods have a **STATUS** of `Running`.
    ```
    kubectl get pods -n pr-firm -o wide
    ```
    {: pre}

    Example output:
    ```
    NAME                         READY     STATUS    RESTARTS   AGE       IP               NODE
    webserver-855556f688-6dbsn   1/1       Running   0          1m        172.30.xxx.xxx   10.176.48.78
    webserver-855556f688-76rkp   1/1       Running   0          1m        172.30.xxx.xxx   10.176.48.78
    webserver-855556f688-xd849   1/1       Running   0          1m        172.30.xxx.xxx   10.176.48.78
    ```
    {: screen}

4. To expose the app to the public internet, create a LoadBalancer service configuration file called `webserver.yaml` in a text editor.
    ```
    apiVersion: v1
    kind: Service
    metadata:
      labels:
        run: webserver
      name: webserver-lb
      namespace: pr-firm
    spec:
      externalTrafficPolicy: Cluster
      ports:
      - name: webserver-port
        port: 80
        protocol: TCP
        targetPort: 8080
      selector:
        run: webserver
      type: LoadBalancer
    ```
    {: codeblock}

5. Deploy the LoadBalancer.
    ```
    kubectl apply -f filepath/webserver.yaml
    ```
    {: pre}

6. Verify that you can publicly access the app exposed by the LoadBalancer from your computer.

    1. Get the public **EXTERNAL-IP** address of the LoadBalancer.
        ```
        kubectl get svc -n pr-firm -o wide
        ```
        {: pre}

        Example output:
        ```
        NAME           CLUSTER-IP       EXTERNAL-IP        PORT(S)        AGE       SELECTOR
        webserver-lb   172.21.xxx.xxx   169.xx.xxx.xxx     80:31024/TCP   2m        run=webserver
        ```
        {: screen}

    2. Create a cheat sheet text file, and copy the LoadBalancer IP into the text file. The cheat sheet will help you more quickly use values in later lessons.

    3. Verify that you can publicly access the external IP for the LoadBalancer.
        ```
        curl --connect-timeout 10 <loadbalancer_IP>:80
        ```
        {: pre}

        The following example output confirms that the LoadBalancer exposes your app on the `169.1.1.1` public LoadBalancer IP address. The `webserver-855556f688-76rkp` app pod received the curl request:
        ```
        Hostname: webserver-855556f688-76rkp
        Pod Information:
            -no pod information available-
        Server values:
            server_version=nginx: 1.13.3 - lua: 10008
        Request Information:
            client_address=1.1.1.1
            method=GET
            real path=/
            query=
            request_version=1.1
            request_scheme=http
            request_uri=http://169.1.1.1:8080/
        Request Headers:
            accept=*/*
            host=169.1.1.1
            user-agent=curl/7.54.0
        Request Body:
            -no body in request-
        ```
        {: screen}

6. Verify that you can publicly access the app exposed by the NodePort from your computer. A LoadBalancer service makes your app available over both the LoadBalancer service IP address and the worker nodes' NodePorts.

    1. Get the NodePort that the LoadBalancer assigned to the worker nodes. The NodePort is in the 30000 - 32767 range.
        ```
        kubectl get svc -n pr-firm -o wide
        ```
        {: pre}

        In the following example output, the NodePort is `31024`:
        ```
        NAME           CLUSTER-IP       EXTERNAL-IP        PORT(S)        AGE       SELECTOR
        webserver-lb   172.21.xxx.xxx   169.xx.xxx.xxx     80:31024/TCP   2m        run=webserver
        ```
        {: screen}  

    2. Get the **Public IP** address of a worker node.
        ```
        ibmcloud ks workers <cluster_name>
        ```
        {: pre}

        Example output:
        ```
        ID                                                 Public IP        Private IP     Machine Type        State    Status   Zone    Version   
        kube-dal10-cr18e61e63c6e94b658596ca93d087eed9-w1   169.xx.xxx.xxx   10.176.48.67   u2c.2x4.encrypted   normal   Ready    dal10   1.10.5_1513*   
        kube-dal10-cr18e61e63c6e94b658596ca93d087eed9-w2   169.xx.xxx.xxx   10.176.48.79   u2c.2x4.encrypted   normal   Ready    dal10   1.10.5_1513*   
        kube-dal10-cr18e61e63c6e94b658596ca93d087eed9-w3   169.xx.xxx.xxx   10.176.48.78   u2c.2x4.encrypted   normal   Ready    dal10   1.10.5_1513*   
        ```
        {: screen}

    3. Copy the public IP of the worker node and the NodePort into your text cheat sheet to use in later lessons.

    4. Verify that you can access the public IP address the worker node through the NodePort.
        ```
        curl  --connect-timeout 10 <worker_IP>:<NodePort>
        ```
        {: pre}

        The following example output confirms that the request to your app came through the private IP address `10.1.1.1` for the worker node and the `31024` NodePort. The `webserver-855556f688-xd849` app pod received the curl request:
        ```
        Hostname: webserver-855556f688-xd849
        Pod Information:
            -no pod information available-
        Server values:
            server_version=nginx: 1.13.3 - lua: 10008
        Request Information:
            client_address=1.1.1.1
            method=GET
            real path=/
            query=
            request_version=1.1
            request_scheme=http
            request_uri=http://10.1.1.1:8080/
        Request Headers:
            accept=*/*
            host=10.1.1.1:31024
            user-agent=curl/7.60.0
        Request Body:
            -no body in request-
        ```
        {: screen}

At this point, your app is exposed from multiple IP addresses and ports. Most of these IPs are internal to the cluster and can be accessed only over the private network. Only the public NodePort and public LoadBalancer port are exposed to the public internet.

Next, you can start creating and applying Calico policies to block public traffic.

## Lesson 2: Block all incoming traffic to all NodePorts
{: #lesson2}

To secure the PR firm's cluster, you must block public access to both the LoadBalancer service and NodePorts that are exposing your app. Start by blocking access to NodePorts. The following image shows how traffic will be permitted to the LoadBalancer but not to NodePorts at the end of Lesson 2:

<img src="images/cs_tutorial_policies_Lesson2.png" width="450" alt="At the end of Lesson 2, the webserver app is exposed to the internet by public LoadBalancer only." style="width:450px; border-style: none"/>

1. In a text editor, create a high-order Pre-DNAT policy called `deny-nodeports.yaml` to deny incoming TCP and UDP traffic from any source IP to all NodePorts.
    ```
    apiVersion: projectcalico.org/v3
    kind: GlobalNetworkPolicy
    metadata:
      name: deny-nodeports
    spec:
      applyOnForward: true
      ingress:
      - action: Deny
        destination:
          ports:
          - 30000:32767
        protocol: TCP
        source: {}
      - action: Deny
        destination:
          ports:
          - 30000:32767
        protocol: UDP
        source: {}
      preDNAT: true
      selector: ibm.role=='worker_public'
      order: 1100
      types:
      - Ingress
    ```
    {: codeblock}

2. Apply the policy.
    - Linux:

      ```
      calicoctl apply -f filepath/deny-nodeports.yaml
      ```
      {: pre}

    - Windows and OS X:

      ```
      calicoctl apply -f filepath/deny-nodeports.yaml --config=filepath/calicoctl.cfg
      ```
      {: pre}
  Example output:
  ```
  Successfully applied 1 'GlobalNetworkPolicy' resource(s)
  ```
  {: screen}

3. Using the values from your cheat sheet, verify that you can't publicly access the worker node public IP address and NodePort.
    ```
    curl  --connect-timeout 10 <worker_IP>:<NodePort>
    ```
    {: pre}

    The connection times out because the Calico policy you created is blocking traffic to NodePorts.
    ```
    curl: (28) Connection timed out after 10016 milliseconds
    ```
    {: screen}

4. Change the externalTrafficPolicy of the LoadBalancer you created in the previous lesson from `Cluster` to `Local`. `Local` ensures that the source IP of your system is preserved when you curl the external IP of the LoadBalancer in the next step.
    ```
    kubectl patch svc -n pr-firm webserver -p '{"spec":{"externalTrafficPolicy":"Local"}}'
    ```
    {: pre}

5. Using the value from your cheat sheet, verify that you can still publicly access the LoadBalancer external IP address.
    ```
    curl --connect-timeout 10 <loadbalancer_IP>:80
    ```
    {: pre}

    Example output:
    ```
    Hostname: webserver-855556f688-76rkp
    Pod Information:
        -no pod information available-
    Server values:
        server_version=nginx: 1.13.3 - lua: 10008
    Request Information:
        client_address=1.1.1.1
        method=GET
        real path=/
        query=
        request_version=1.1
        request_scheme=http
        request_uri=http://<loadbalancer_IP>:8080/
    Request Headers:
        accept=*/*
        host=<loadbalancer_IP>
        user-agent=curl/7.54.0
    Request Body:
        -no body in request-
    ```
    {: screen}
    In the `Request Information` section of the output, note that the source IP address is, for example, `client_address=1.1.1.1`. The source IP address is the public IP of the system that you're using to run curl. Otherwise, if you are connecting to the internet through a proxy or VPN, the proxy or VPN might be obscuring your system's actual IP address. In either case, the LoadBalancer sees your system's source IP address as the client IP address.

6. Copy your system's source IP address (`client_address=1.1.1.1` in the previous step output) into your cheat sheet to use in later lessons.

Great! At this point, your app is exposed to the public internet from the public LoadBalancer port only. Traffic to the public NodePorts is blocked. You've partially locked down your cluster from unwanted traffic.

Next, you can create and apply Calico policies to whitelist traffic from certain source IPs.

## Lesson 3: Allow incoming traffic from a whitelisted IP to the LoadBalancer
{: #lesson3}

You now decide to completely lock down traffic to the PR firm's cluster and test access by whitelisting your own computer's IP address.
{: shortdesc}

First, in addition to the NodePorts, you must block all incoming traffic to the LoadBalancer exposing the app. Then, you can create a policy that whitelists your system's IP address. At the end of Lesson 3, all traffic to the public NodePorts and LoadBalancer will be blocked and only traffic from your whitelisted system IP will be allowed:
<img src="images/cs_tutorial_policies_L3.png" width="600" alt="The webserver app is exposed by public LoadBalancer to your system IP only." style="width:600px; border-style: none"/>

1. In a text editor, create a high-order Pre-DNAT policy called `deny-lb-port-80.yaml` to deny all incoming TCP and UDP traffic from any source IP to the LoadBalancer IP address and port. Replace `<loadbalancer_IP>` with the LoadBalancer public IP address from your cheat sheet.
    ```
    apiVersion: projectcalico.org/v3
    kind: GlobalNetworkPolicy
    metadata:
      name: deny-lb-port-80
    spec:
      applyOnForward: true
      ingress:
      - action: Deny
        destination:
          nets:
          - <loadbalancer_IP>/32
          ports:
          - 80
        protocol: TCP
        source: {}
      - action: Deny
        destination:
          nets:
          - <loadbalancer_IP>/32
          ports:
          - 80
        protocol: UDP
        source: {}
      preDNAT: true
      selector: ibm.role=='worker_public'
      order: 1100
      types:
      - Ingress
    ```
    {: codeblock}

2. Apply the policy.
    - Linux:

      ```
      calicoctl apply -f filepath/deny-lb-port-80.yaml
      ```
      {: pre}

    - Windows and OS X:

      ```
      calicoctl apply -f filepath/deny-lb-port-80.yaml --config=filepath/calicoctl.cfg
      ```
      {: pre}

3. Using the value from your cheat sheet, verify that you now can't access the public LoadBalancer IP address. The connection times out because the Calico policy you created is blocking traffic to the LoadBalancer.
    ```
    curl --connect-timeout 10 <loadbalancer_IP>:80
    ```
    {: pre}

4. In a text editor, create a low-order Pre-DNAT policy called `whitelist.yaml` to allow traffic from your system's IP to the LoadBalancer IP address and port. Using the values from your cheat sheet, replace `<loadbalancer_IP>` with the public IP address of the LoadBalancer and `<client_address>` with the public IP address of your system's source IP.
    ```
    apiVersion: projectcalico.org/v3
    kind: GlobalNetworkPolicy
    metadata:
      name: whitelist
    spec:
      applyOnForward: true
      ingress:
      - action: Allow
        destination:
          nets:
          - <loadbalancer_IP>/32
          ports:
          - 80
        protocol: TCP
        source:
          nets:
          - <client_address>/32
      preDNAT: true
      selector: ibm.role=='worker_public'
      order: 500
      types:
      - Ingress
    ```
    {: codeblock}

5. Apply the policy.
    - Linux:

      ```
      calicoctl apply -f filepath/whitelist.yaml
      ```
      {: pre}

    - Windows and OS X:

      ```
      calicoctl apply -f filepath/whitelist.yaml --config=filepath/calicoctl.cfg
      ```
      {: pre}
  Your system's IP address is now whitelisted.

6. Using the value from your cheat sheet, verify that you now can access the public LoadBalancer IP address.
    ```
    curl --connect-timeout 10 <loadbalancer_IP>:80
    ```
    {: pre}

7. If you have access to another system that has a different IP address, try to access the LoadBalancer from that system.
    ```
    curl --connect-timeout 10 <loadbalancer_IP>:80
    ```
    {: pre}
    The connection times out because that system's IP address isn't whitelisted.

At this point, all traffic to the public NodePorts and LoadBalancer is blocked. Only traffic from your whitelisted system IP is allowed.

## Lesson 4: Deny incoming traffic from blacklisted IPs to the LoadBalancer
{: #lesson4}

In the previous lesson, you blocked all traffic and whitelisted only a few IPs. That scenario works well for testing purposes when you want to limit access to only a few controlled source IP addresses. However, the PR firm has apps that need to be widely available to the public. You need to make sure that all traffic is permitted except for the unusual traffic you are seeing from a few IP addresses. Blacklisting is useful in a scenario like this because it can help you prevent an attack from a small set of IP addresses.

In this lesson, you will test blacklisting by blocking traffic from your own system's source IP address. At the end of Lesson 4, all traffic to the public NodePorts will be blocked, and all traffic to the public LoadBalancer will be allowed. Only traffic from your blacklisted system IP to the LoadBalancer will be blocked:
<img src="images/cs_tutorial_policies_L4.png" width="600" alt="The webserver app is exposed by public LoadBalancer to the internet. Traffic from your system IP only is blocked." style="width:600px; border-style: none"/>

1. Clean up the whitelist policies you created in the previous lesson.
    ```
    calicoctl delete GlobalNetworkPolicy deny-lb-port-80
    ```
    {: pre}
    ```
    calicoctl delete GlobalNetworkPolicy whitelist
    ```
    {: pre}
    Now, all incoming TCP and UDP traffic from any source IP to the LoadBalancer IP address and port is permitted again.

2. To deny all incoming TCP and UDP traffic from your system's source IP address to the LoadBalancer IP address and port, create a low-order Pre-DNAT policy called `deny-lb-port-80.yaml` in a text editor. Using the values from your cheat sheet, replace `<loadbalancer_IP>` with the public IP address of the LoadBalancer and `<client_address>` with the public IP address of your system's source IP.
    ```
    apiVersion: projectcalico.org/v3
    kind: GlobalNetworkPolicy
    metadata:
      name: blacklist
    spec:
      applyOnForward: true
      ingress:
      - action: Deny
        destination:
          nets:
          - <loadbalancer_IP>/32
          ports:
          - 80
        protocol: TCP
        source:
          nets:
          - <client_address>/32
      - action: Deny
        destination:
          nets:
          - <loadbalancer_IP>/32
          ports:
          - 80
        protocol: UDP
        source:
          nets:
          - <client_address>/32
      preDNAT: true
      selector: ibm.role=='worker_public'
      order: 500
      types:
      - Ingress
    ```
    {: codeblock}

3. Apply the policy.
    - Linux:

      ```
      calicoctl apply -f filepath/blacklist.yaml
      ```
      {: pre}

    - Windows and OS X:

      ```
      calicoctl apply -f filepath/blacklist.yaml --config=filepath/calicoctl.cfg
      ```
      {: pre}
  Your system's IP address is now blacklisted.

4. Using the value from your cheat sheet, verify from your system that you can't access the LoadBalancer IP because your system's IP is blacklisted.
    ```
    curl --connect-timeout 10 <loadbalancer_IP>:80
    ```
    {: pre}
    At this point, all traffic to the public NodePorts is blocked, and all traffic to the public LoadBalancer is allowed. Only traffic from your blacklisted system IP to the LoadBalancer is blocked.

5. To clean up this blacklist policy:
    ```
    calicoctl delete GlobalNetworkPolicy blacklist
    ```
    {: pre}

Great work! You've successfully controlled traffic into your app by using Calico Pre-DNAT policies to whitelist and blacklist source IPs.

## What's next?
{: #whats_next}

* Read more about [controlling traffic with network policies](cs_network_policy.html).
* For more example Calico network policies that control traffic to and from your cluster, you can check out the [stars policy demo ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.projectcalico.org/v3.1/getting-started/kubernetes/tutorials/stars-policy/) and the [advanced network policy ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.projectcalico.org/v3.1/getting-started/kubernetes/tutorials/advanced-policy).
