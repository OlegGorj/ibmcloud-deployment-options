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


# Setting up VPN connectivity
{: #vpn}

With VPN connectivity, you can securely connect apps in a Kubernetes cluster on {{site.data.keyword.containerlong}} to an on-premises network. You can also connect apps that are external to your cluster to an app that is running inside your cluster.
{:shortdesc}

To connect your worker nodes and apps to an on-premises data center, you can configure one of the following options.

- **strongSwan IPSec VPN Service**: You can set up a [strongSwan IPSec VPN service ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.strongswan.org/about.html) that securely connects your Kubernetes cluster with an on-premises network. The strongSwan IPSec VPN service provides a secure end-to-end communication channel over the internet that is based on the industry-standard Internet Protocol Security (IPSec) protocol suite. To set up a secure connection between your cluster and an on-premises network, [configure and deploy the strongSwan IPSec VPN service](#vpn-setup) directly in a pod in your cluster.

- **Virtual Router Appliance (VRA) or Fortigate Security Appliance (FSA)**: You might choose to set up a [VRA](/docs/infrastructure/virtual-router-appliance/about.html) or [FSA](/docs/infrastructure/fortigate-10g/about.html) to configure an IPSec VPN endpoint. This option is useful when you have a larger cluster, want to access multiple clusters over a single VPN, or need a route-based VPN. To configure a VRA, see [Setting up VPN connectivity with VRA](#vyatta).

## Using the strongSwan IPSec VPN service Helm chart
{: #vpn-setup}

Use a Helm chart to configure and deploy the strongSwan IPSec VPN service inside of a Kubernetes pod.
{:shortdesc}

Because strongSwan is integrated within your cluster, you don't need an external gateway device. When VPN connectivity is established, routes are automatically configured on all of the worker nodes in the cluster. These routes allow two-way connectivity through the VPN tunnel between pods on any worker node and the remote system. For example, the following diagram shows how an app in {{site.data.keyword.containershort_notm}} can communicate with an on-premises server via a strongSwan VPN connection:

<img src="images/cs_vpn_strongswan.png" width="700" alt="Expose an app in {{site.data.keyword.containershort_notm}} by using a load balancer" style="width:700px; border-style: none"/>

1. An app in your cluster, `myapp`, receives a request from an Ingress or LoadBalancer service and needs to securely connect to data in your on-premises network.

2. The request to the on-premises data center is forwarded to the IPSec strongSwan VPN pod. The destination IP address is used to determine which network packets to send to the IPSec strongSwan VPN pod.

3. The request is encrypted and sent over the VPN tunnel to the on-premises data center.

4. The incoming request passes through the on-premises firewall and is delivered to the VPN tunnel endpoint (router) where it is decrypted.

5. The VPN tunnel endpoint (router) forwards the request to the on-premises server or mainframe, depending on the destination IP address that was specified in step 2. The necessary data is sent back over the VPN connection to `myapp` through the same process.

## strongSwan VPN service considerations
{: strongswan_limitations}

Before using the strongSwan Helm chart, review the following considerations and limitations.

* The strongSwan Helm chart requires NAT traversal to be enabled by the remote VPN endpoint. NAT traversal requires UDP port 4500 in addition to the default IPSec UDP port of 500. Both UDP ports need to be allowed through any firewall that is configured.
* The strongSwan Helm chart does not support route-based IPSec VPNs.
* The strongSwan Helm chart supports IPSec VPNs that use preshared keys, but does not support IPSec VPNs that require certificates.
* The strongSwan Helm chart does not allow multiple clusters and other IaaS resources to share a single VPN connection.
* The strongSwan Helm chart runs as a Kubernetes pod inside of the cluster. The VPN performance is affected by the memory and network usage of Kubernetes and other pods that are running in the cluster. If you have a performance-critical environment, consider using a VPN solution that runs outside of the cluster on dedicated hardware.
* The strongSwan Helm chart runs a single VPN pod as the IPSec tunnel endpoint. If the pod fails, the cluster restarts the pod. However, you might experience a short down time while the new pod starts and the VPN connection is re-established. If you require faster error recovery or a more elaborate high availability solution, consider using a VPN solution that runs outside of the cluster on dedicated hardware.
* The strongSwan Helm chart does not provide metrics or monitoring of the network traffic flowing over the VPN connection. For a list of supported monitoring tools, see [Logging and monitoring services](cs_integrations.html#health_services).

## Configuring the strongSwan Helm chart
{: #vpn_configure}

Before you begin:
* [Install an IPSec VPN gateway in your on-premises data center](/docs/infrastructure/iaas-vpn/set-up-ipsec-vpn.html#setting-up-an-ipsec-connection).
* [Create a standard cluster](cs_clusters.html#clusters_cli).
* [Target the Kubernetes CLI to the cluster](cs_cli_install.html#cs_cli_configure).

### Step 1: Get the strongSwan Helm chart
{: #strongswan_1}

1. [Install Helm for your cluster and add the {{site.data.keyword.Bluemix_notm}} repository to your Helm instance](cs_integrations.html#helm).

2. Save the default configuration settings for the strongSwan Helm chart in a local YAML file.

    ```
    helm inspect values ibm/strongswan > config.yaml
    ```
    {: pre}

3. Open the `config.yaml` file.

### Step 2: Configure basic IPSec settings
{: #strongswan_2}

To control the establishment of the VPN connection, modify the following basic IPSec settings.

For more information about each setting, read the documentation provided within the `config.yaml` file for the Helm chart.
{: tip}

1. If your on-premises VPN tunnel endpoint does not support `ikev2` as a protocol for initializing the connection, change the value of `ipsec.keyexchange` to `ikev1` or `ike`.
2. Set `ipsec.esp` to a list of ESP encryption and authentication algorithms that your on-premises VPN tunnel endpoint uses for the connection.
    * If `ipsec.keyexchange` is set to `ikev1`, this setting must be specified.
    * If `ipsec.keyexchange` is set to `ikev2`, this setting is optional.
    * If you leave this setting blank, the default strongSwan algorithms `aes128-sha1,3des-sha1` are used for the connection.
3. Set `ipsec.ike` to a list of IKE/ISAKMP SA encryption and authentication algorithms that your on-premises VPN tunnel endpoint uses for the connection. The algorithms must be specific in the format `encryption-integrity[-prf]-dhgroup`.
    * If `ipsec.keyexchange` is set to `ikev1`, this setting must be specified.
    * If `ipsec.keyexchange` is set to `ikev2`, this setting is optional.
    * If you leave this setting blank, the default strongSwan algorithms `aes128-sha1-modp2048,3des-sha1-modp1536` are used for the connection.
4. Change the value of `local.id` to any string that you want to use to identify the local Kubernetes cluster side that your VPN tunnel endpoint uses. The default is `ibm-cloud`. Some VPN implementations require that you use the public IP address for the local endpoint.
5. Change the value of `remote.id` to any string that you want to use to identify the remote on-premises side that your VPN tunnel endpoint uses. The default is `on-prem`. Some VPN implementations require that you use the public IP address for the remote endpoint.
6. Change the value of `preshared.secret` to the pre-shared secret that your on-premises VPN tunnel endpoint gateway uses for the connection. This value is stored in `ipsec.secrets`.
7. Optional: Set `remote.privateIPtoPing` to any private IP address in the remote subnet to ping as part of the Helm connectivity validation test.

### Step 3: Select inbound or outbound VPN connection
{: #strongswan_3}

When you configure a strongSwan VPN connection, you choose whether the VPN connection is inbound to the cluster or outbound from the cluster.
{: shortdesc}

<dl>
<dt>Inbound</dt>
<dd>The on-premises VPN endpoint from the remote network initiates the VPN connection, and the cluster listens for the connection.</dd>
<dt>Outbound</dt>
<dd>The cluster initiates the VPN connection, and the on-premises VPN endpoint from the remote network listens for the connection.</dd>
</dl>

To establish an inbound VPN connection, modify the following settings:
1. Verify that `ipsec.auto` is set to `add`.
2. Optional: Set `loadBalancerIP` to a portable public IP address for the strongSwan VPN service. Specifying an IP address is useful when you need a stable IP address, such as when you must designate which IP addresses are permitted through an on-premises firewall. The cluster must have at least one available public Load Balancer IP address. [You can check to see your available public IP addresses](cs_subnets.html#review_ip) or [free up a used IP address](cs_subnets.html#free).<br>**Note**:
    * If you leave this setting blank, one of the available portable public IP addresses is used.
    * You must also configure the public IP address that you select for or the public IP address that is assigned to the cluster VPN endpoint on the on-premises VPN endpoint.

To establish an outbound VPN connection, modify the following settings:
1. Change `ipsec.auto` to `start`.
2. Set `remote.gateway` to the public IP address for the on-premises VPN endpoint in the remote network.
3. Choose one of the following options for the IP address for the cluster VPN endpoint:
    * **Public IP address of the cluster's private gateway**: If your worker nodes are connected to a private VLAN only, then the outbound VPN request is routed through the private gateway in order to reach the internet. The public IP address of the private gateway is used for the VPN connection.
    * **Public IP address of the worker node where the strongSwan pod is running**: If the worker node where the strongSwan pod is running is connected to a public VLAN, then the worker node's public IP address is used for the VPN connection.
        <br>**Note**:
        * If the strongSwan pod is deleted and rescheduled onto a different worker node in the cluster, then the public IP address of the VPN changes. The on-premises VPN endpoint of the remote network must allow the VPN connection to be established from the public IP address of any of the cluster worker nodes.
        * If the remote VPN endpoint cannot handle VPN connections from multiple public IP addresses, limit the nodes that the strongSwan VPN pod deploys to. Set `nodeSelector` to the IP addresses of specific worker nodes or a worker node label. For example, the value `kubernetes.io/hostname: 10.232.xx.xx` allows the VPN pod to deploy to that worker node only. The value `strongswan: vpn` restricts the VPN pod to running on any worker nodes with that label. You can use any worker node label. To allow different worker nodes to be used with different helm chart deployments, use `strongswan: <release_name>`. For high availability, select at least two worker nodes.
    * **Public IP address of the strongSwan service**: To establish connection by using the IP address of the strongSwan VPN service, set `connectUsingLoadBalancerIP` to `true`. The strongSwan service IP address is either a portable public IP address you can specify in the `loadBalancerIP` setting, or an available portable public IP address that is automatically assigned to the service.
        <br>**Note**:
        * If you choose to select an IP address using the `loadBalancerIP` setting, the cluster must have at least one available public Load Balancer IP address. [You can check to see your available public IP addresses](cs_subnets.html#review_ip) or [free up a used IP address](cs_subnets.html#free).
        * All of the cluster worker nodes must be on the same public VLAN. Otherwise, you must use the `nodeSelector` setting to ensure that the VPN pod deploys to a worker node on the same public VLAN as the `loadBalancerIP`.
        * If `connectUsingLoadBalancerIP` is set to `true` and `ipsec.keyexchange` is set to `ikev1`, you must set `enableServiceSourceIP` to `true`.

### Step 4: Access cluster resources over the VPN connection
{: #strongswan_4}

Determine which cluster resources must be accessible by the remote network over the VPN connection.
{: shortdesc}

1. Add the CIDRs of one or more cluster subnets to the `local.subnet` setting. You must configure the local subnet CIDRs on the on-premises VPN endpoint. This list can include the following subnets:  
    * The Kubernetes pod subnet CIDR: `172.30.0.0/16`. Bidirectional communication is enabled between all cluster pods and any of the hosts in the remote network subnets that you list in the `remote.subnet` setting. If you must prevent any `remote.subnet` hosts from accessing cluster pods for security reasons, do not add the Kubernetes pod subnet to the `local.subnet` setting.
    * The Kubernetes service subnet CIDR: `172.21.0.0/16`. Service IP addresses provide a way to expose multiple app pods that are deployed on several worker nodes behind a single IP.
    * If your apps are exposed by a NodePort service on the private network or a private Ingress ALB, add the worker node's private subnet CIDR. Retrieve the first three octets of your worker's private IP address by running `ibmcloud ks worker <cluster_name>`. For example, if it is `10.176.48.xx` then note `10.176.48`. Next, get the worker private subnet CIDR by running the following command, replacing `<xxx.yyy.zz>` with the octet that you previously retrieved: `ibmcloud sl subnet list | grep <xxx.yyy.zzz>`.<br>**Note**: If a worker node is added on a new private subnet, you must add the new private subnet CIDR to the `local.subnet` setting and the on-premises VPN endpoint. Then, the VPN connection must be restarted.
    * If you have apps that are exposed by LoadBalancer services on the private network, add the cluster's private user-managed subnet CIDRs. To find these values, run `ibmcloud ks cluster-get <cluster_name> --showResources`. In the **VLANS** section, look for CIDRs that have a **Public** value of `false`.<br>
    **Note**: If `ipsec.keyexchange` is set to `ikev1`, you can specify only one subnet. However, you can use the `localSubnetNAT` setting to combine multiple cluster subnets into a single subnet.

2. Optional: Remap cluster subnets by using the `localSubnetNAT` setting. Network Address Translation (NAT) for subnets provides a workaround for subnet conflicts between the cluster network and on-premises remote network. You can use NAT to remap the cluster's private local IP subnets, the pod subnet (172.30.0.0/16), or the pod service subnet (172.21.0.0/16) to a different private subnet. The VPN tunnel sees remapped IP subnets instead of the original subnets. Remapping happens before the packets are sent over the VPN tunnel as well as after the packets arrive from the VPN tunnel. You can expose both remapped and non-remapped subnets at the same time over the VPN. To enable NAT, you can either add an entire subnet or individual IP addresses.
    * If you add an entire subnet in the format `10.171.42.0/24=10.10.10.0/24`, remapping is 1-to-1: all of the IP addresses in the internal network subnet are mapped over to external network subnet and vice versa.
    * If you add individual IP addresses in the format `10.171.42.17/32=10.10.10.2/32,10.171.42.29/32=10.10.10.3/32`, only those internal IP addresses are mapped to the specified external IP addresses.

3. Optional for version 2.2.0 and later strongSwan Helm charts: Hide all of the cluster IP addresses behind a single IP address by setting `enableSingleSourceIP` to `true`. This option provides one of the most secure configurations for the VPN connection because no connections from the remote network back into the cluster are permitted.
    <br>**Note**:
    * This setting requires that all data flow over the VPN connection must be outbound regardless of whether the VPN connection is established from the cluster or from the remote network.
    * `local.subnet` must be set to only one /32 subnet.

4. Optional for version 2.2.0 and later strongSwan Helm charts: Enable the strongSwan service to route incoming requests from the remote network to a service that exists outside of the cluster by using the `localNonClusterSubnet` setting.
    <br>**Note**:
    * The non-cluster service must exist on the same private network or on a private network that is reachable by the worker nodes.
    * The non-cluster worker node cannot initiate traffic to the remote network through the VPN connection, but the non-cluster node can be the target of incoming requests from the remote network.
    * You must list the CIDRs of the non-cluster subnets in the `local.subnet` setting.

### Step 5: Access remote network resources over the VPN connection
{: #strongswan_5}

Determine which remote network resources must be accessible by the cluster over the VPN connection.
{: shortdesc}

1. Add the CIDRs of one or more on-premises private subnets to the `remote.subnet` setting.
    <br>**Note**: If `ipsec.keyexchange` is set to `ikev1`, you can specify only one subnet.
2. Optional for version 2.2.0 and later strongSwan Helm charts: Remap remote network subnets by using the `remoteSubnetNAT` setting. Network Address Translation (NAT) for subnets provides a workaround for subnet conflicts between the cluster network and on-premises remote network. You can use NAT to remap the remote network's IP subnets to a different private subnet. The VPN tunnel sees remapped IP subnets instead of the original subnets. Remapping happens before the packets are sent over the VPN tunnel as well as after the packets arrive from the VPN tunnel. You can expose both remapped and non-remapped subnets at the same time over the VPN.

### Step 6: Deploy the Helm chart
{: #strongswan_6}

1. If you need to configure more advanced settings, follow the documentation provided for each setting in the Helm chart.

2. **Important**: If you do not need a setting in the Helm chart, comment that property out by placing a `#` in front of it.

3. Save the updated `config.yaml` file.

4. Install the Helm chart to your cluster with the updated `config.yaml` file. The updated properties are stored in a configmap for your chart.

    **Note**: If you have multiple VPN deployments in a single cluster, you can avoid naming conflicts and differentiate between your deployments by choosing more descriptive release names than `vpn`. To avoid the truncation of the release name, limit the release name to 35 characters or less.

    ```
    helm install -f config.yaml --name=vpn ibm/strongswan
    ```
    {: pre}

5. Check the chart deployment status. When the chart is ready, the **STATUS** field near the top of the output has a value of `DEPLOYED`.

    ```
    helm status vpn
    ```
    {: pre}

6. Once the chart is deployed, verify that the updated settings in the `config.yaml` file were used.

    ```
    helm get values vpn
    ```
    {: pre}

## Testing and verifying strongSwan VPN connectivity
{: #vpn_test}

After you deploy your Helm chart, test the VPN connectivity.
{:shortdesc}

1. If the VPN on the on-premises gateway is not active, start the VPN.

2. Set the `STRONGSWAN_POD` environment variable.

    ```
    export STRONGSWAN_POD=$(kubectl get pod -l app=strongswan,release=vpn -o jsonpath='{ .items[0].metadata.name }')
    ```
    {: pre}

3. Check the status of the VPN. A status of `ESTABLISHED` means that the VPN connection was successful.

    ```
    kubectl exec $STRONGSWAN_POD -- ipsec status
    ```
    {: pre}

    Example output:

    ```
    Security Associations (1 up, 0 connecting):
    k8s-conn[1]: ESTABLISHED 17 minutes ago, 172.30.xxx.xxx[ibm-cloud]...192.xxx.xxx.xxx[on-premises]
    k8s-conn{2}: INSTALLED, TUNNEL, reqid 12, ESP in UDP SPIs: c78cb6b1_i c5d0d1c3_o
    k8s-conn{2}: 172.21.0.0/16 172.30.0.0/16 === 10.91.152.xxx/26
    ```
    {: screen}

    **Note**:

    <ul>
    <li>When you try to establish VPN connectivity with the strongSwan Helm chart, it is likely that the VPN status is not `ESTABLISHED` the first time. You might need to check the on-premises VPN endpoint settings and change the configuration file several times before the connection is successful: <ol><li>Run `helm delete --purge <release_name>`</li><li>Fix the incorrect values in the configuration file.</li><li>Run `helm install -f config.yaml --name=<release_name> ibm/strongswan`</li></ol>You can also run more checks in the next step.</li>
    <li>If the VPN pod is in an `ERROR` state or continues to crash and restart, it might be due to parameter validation of the `ipsec.conf` settings in the chart's configmap.<ol><li>Check for any validation errors in the strongSwan pod logs by running `kubectl logs -n $STRONGSWAN_POD`.</li><li>If validation errors exist, run `helm delete --purge <release_name>`<li>Fix the incorrect values in the configuration file.</li><li>Run `helm install -f config.yaml --name=<release_name> ibm/strongswan`</li></ol>If your cluster has a high number of worker nodes, you can also use `helm upgrade` to more quickly apply your changes instead of running `helm delete` and `helm install`.</li>
    </ul>

4. You can further test the VPN connectivity by running the five Helm tests that are included in the strongSwan chart definition.

    ```
    helm test vpn
    ```
    {: pre}

    * If all of the tests pass, your strongSwan VPN connection is successfully set up.

    * If any of the tests fail, continue to the next step.

5. View the output of a failed test by looking at the logs of the test pod.

    ```
    kubectl logs <test_program>
    ```
    {: pre}

    **Note**: Some of the tests have requirements that are optional settings in the VPN configuration. If some of the tests fail, the failures might be acceptable depending on whether you specified these optional settings. Refer to the following table for information about each test and why it might fail.

    {: #vpn_tests_table}
    <table>
    <caption>Understanding the Helm VPN connectivity tests</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the Helm VPN connectivity tests</th>
    </thead>
    <tbody>
    <tr>
    <td><code>vpn-strongswan-check-config</code></td>
    <td>Validates the syntax of the <code>ipsec.conf</code> file that is generated from the <code>config.yaml</code> file. This test might fail due to incorrect values in the <code>config.yaml</code> file.</td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-check-state</code></td>
    <td>Checks that the VPN connection has a status of <code>ESTABLISHED</code>. This test might fail for the following reasons:<ul><li>Differences between the values in the <code>config.yaml</code> file and the on-premises VPN endpoint settings.</li><li>If the cluster is in "listen" mode (<code>ipsec.auto</code> is set to <code>add</code>), the connection is not established on the on-premises side.</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-gw</code></td>
    <td>Pings the <code>remote.gateway</code> public IP address that you configured in the <code>config.yaml</code> file. This test might fail for the following reasons:<ul><li>You did not specify an on-premises VPN gateway IP address. If <code>ipsec.auto</code> is set to <code>start</code>, the <code>remote.gateway</code> IP address is required.</li><li>The VPN connection does not have the <code>ESTABLISHED</code> status. See <code>vpn-strongswan-check-state</code> for more information.</li><li>The VPN connectivity is <code>ESTABLISHED</code>, but ICMP packets are being blocked by a firewall.</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-ip-1</code></td>
    <td>Pings the <code>remote.privateIPtoPing</code> private IP address of the on-premises VPN gateway from the VPN pod in the cluster. This test might fail for the following reasons:<ul><li>You did not specify a <code>remote.privateIPtoPing</code> IP address. If you intentionally did not specify an IP address, this failure is acceptable.</li><li>You did not specify the cluster pod subnet CIDR, <code>172.30.0.0/16</code>, in the <code>local.subnet</code> list.</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-ip-2</code></td>
    <td>Pings the <code>remote.privateIPtoPing</code> private IP address of the on-premises VPN gateway from the worker node in the cluster. This test might fail for the following reasons:<ul><li>You did not specify a <code>remote.privateIPtoPing</code> IP address. If you intentionally did not specify an IP address, this failure is acceptable.</li><li>You did not specify the cluster worker node private subnet CIDR in the <code>local.subnet</code> list.</li></ul></td>
    </tr>
    </tbody></table>

6. Delete the current Helm chart.

    ```
    helm delete --purge vpn
    ```
    {: pre}

7. Open the `config.yaml` file and fix the incorrect values.

8. Save the updated `config.yaml` file.

9. Install the Helm chart to your cluster with the updated `config.yaml` file. The updated properties are stored in a configmap for your chart.

    ```
    helm install -f config.yaml --name=<release_name> ibm/strongswan
    ```
    {: pre}

10. Check the chart deployment status. When the chart is ready, the **STATUS** field near the top of the output has a value of `DEPLOYED`.

    ```
    helm status vpn
    ```
    {: pre}

11. Once the chart is deployed, verify that the updated settings in the `config.yaml` file were used.

    ```
    helm get values vpn
    ```
    {: pre}

12. Clean up the current test pods.

    ```
    kubectl get pods -a -l app=strongswan-test
    ```
    {: pre}

    ```
    kubectl delete pods -l app=strongswan-test
    ```
    {: pre}

13. Run the tests again.

    ```
    helm test vpn
    ```
    {: pre}

<br />


## Upgrading the strongSwan Helm chart
{: #vpn_upgrade}

Make sure your strongSwan Helm chart is up-to-date by upgrading it.
{:shortdesc}

To upgrade your strongSwan Helm chart to the latest version:

  ```
  helm upgrade -f config.yaml <release_name> ibm/strongswan
  ```
  {: pre}

**Important**: The strongSwan 2.0.0 Helm chart does not work with Calico v3 or Kubernetes 1.10. Before you [update your cluster to 1.10](cs_versions.html#cs_v110), update strongSwan to the 2.2.0 Helm chart, which is backward compatible with Calico 2.6 and Kubernetes 1.8 and 1.9.

Updating your cluster to Kubernetes 1.10? Be sure to delete your strongSwan Helm chart first. Then after the update, reinstall it.
{:tip}

### Upgrading from version 1.0.0
{: #vpn_upgrade_1.0.0}

Due to some of the settings that are used in the version 1.0.0 Helm chart, you cannot use `helm upgrade` to update from 1.0.0 to the latest version.
{:shortdesc}

To upgrade from version 1.0.0, you must delete the 1.0.0 chart and install the latest version:

1. Delete the 1.0.0 Helm chart.

    ```
    helm delete --purge <release_name>
    ```
    {: pre}

2. Save the default configuration settings for the latest version of the strongSwan Helm chart in a local YAML file.

    ```
    helm inspect values ibm/strongswan > config.yaml
    ```
    {: pre}

3. Update the configuration file and save the file with your changes.

4. Install the Helm chart to your cluster with the updated `config.yaml` file.

    ```
    helm install -f config.yaml --name=<release_name> ibm/strongswan
    ```
    {: pre}

Additionally, certain `ipsec.conf` timeout settings that were hardcoded in 1.0.0 are exposed as configurable properties in later versions. The names and defaults of some of these configurable `ipsec.conf` timeout settings were also changed to be more consistent with strongSwan standards. If you are upgrading your Helm chart from 1.0.0 and want to retain the 1.0.0 version defaults for the timeout settings, add the new settings to your chart configuration file with the old default values.



  <table>
  <caption>ipsec.conf settings differences between version 1.0.0 and the latest version</caption>
  <thead>
  <th>1.0.0 setting name</th>
  <th>1.0.0 default</th>
  <th>Latest version setting name</th>
  <th>Latest version default</th>
  </thead>
  <tbody>
  <tr>
  <td><code>ikelifetime</code></td>
  <td>60m</td>
  <td><code>ikelifetime</code></td>
  <td>3h</td>
  </tr>
  <tr>
  <td><code>keylife</code></td>
  <td>20m</td>
  <td><code>lifetime</code></td>
  <td>1h</td>
  </tr>
  <tr>
  <td><code>rekeymargin</code></td>
  <td>3m</td>
  <td><code>margintime</code></td>
  <td>9m</td>
  </tr>
  </tbody></table>


## Disabling the strongSwan IPSec VPN service
{: vpn_disable}

You can disable the VPN connection by deleting the Helm chart.
{:shortdesc}

  ```
  helm delete --purge <release_name>
  ```
  {: pre}

<br />


## Using a Virtual Router Appliance
{: #vyatta}

The [Virtual Router Appliance (VRA)](/docs/infrastructure/virtual-router-appliance/about.html) provides the latest Vyatta 5600 operating system for x86 bare metal servers. You can use a VRA as VPN gateway to securely connect to an on-premises network.
{:shortdesc}

All public and private network traffic that enters or exits the cluster VLANs is routed through a VRA. You can use the VRA as a VPN endpoint to create an encrypted IPSec tunnel between servers in IBM Cloud infrastructure (SoftLayer) and on-premises resources. For example, the following diagram shows how an app on a private-only worker node in {{site.data.keyword.containershort_notm}} can communicate with an on-premises server via a VRA VPN connection:

<img src="images/cs_vpn_vyatta.png" width="725" alt="Expose an app in {{site.data.keyword.containershort_notm}} by using a load balancer" style="width:725px; border-style: none"/>

1. An app in your cluster, `myapp2`, receives a request from an Ingress or LoadBalancer service and needs to securely connect to data in your on-premises network.

2. Because `myapp2` is on a worker node that is on a private VLAN only, the VRA acts as a secure connection between the worker nodes and the on-premises network. The VRA uses the destination IP address to determine which network packets to send to the on-premises network.

3. The request is encrypted and sent over the VPN tunnel to the on-premises data center.

4. The incoming request passes through the on-premises firewall and is delivered to the VPN tunnel endpoint (router) where it is decrypted.

5. The VPN tunnel endpoint (router) forwards the request to the on-premises server or mainframe, depending on the destination IP address that was specified in step 2. The necessary data is sent back over the VPN connection to `myapp2` through the same process.

To set up a Virtual Router Appliance:

1. [Order a VRA](/docs/infrastructure/virtual-router-appliance/getting-started.html).

2. [Configure the private VLAN on the VRA](/docs/infrastructure/virtual-router-appliance/manage-vlans.html).

3. To enable a VPN connection by using the VRA, [configure VRRP on the VRA](/docs/infrastructure/virtual-router-appliance/vrrp.html#high-availability-vpn-with-vrrp).

**Note**: If you have an existing router appliance and then add a cluster, the new portable subnets that are ordered for the cluster are not configured on the router appliance. In order to use networking services, you must enable routing between the subnets on the same VLAN by [enabling VLAN spanning](cs_subnets.html#subnet-routing).
