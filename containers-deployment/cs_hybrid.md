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


# Using {{site.data.keyword.containerlong_notm}} with {{site.data.keyword.Bluemix_notm}} Private
{: #hybrid_iks_icp}

If you have an {{site.data.keyword.Bluemix}} Private account, you can use it with select {{site.data.keyword.Bluemix_notm}} services, including {{site.data.keyword.containerlong}}. For more information, see the blog on [Hybrid experience across {{site.data.keyword.Bluemix_notm}} Private and IBM Public Cloud![External link icon](../icons/launch-glyph.svg "External link icon")](http://ibm.biz/hybridJune2018).
{: shortdesc}

You understand the [{{site.data.keyword.Bluemix_notm}} offerings](cs_why.html#differentiation). Now, you can [connect your public and private cloud](#hybrid_vpn) and [reuse your private packages for public containers](#hybrid_ppa_importer).

## Connecting your public and private cloud with the strongSwan VPN
{: #hybrid_vpn}

Establish VPN connectivity between your public Kubernetes cluster and your {{site.data.keyword.Bluemix}} Private instance to allow two-way communication.
{: shortdesc}

1.  [Create a cluster in {{site.data.keyword.Bluemix}} Private![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/installing/installing.html).

2.  Create a standard cluster with {{site.data.keyword.containerlong}} in {{site.data.keyword.Bluemix}} Public or use an existing one. To create a cluster, choose between the following options: 
    - [Create a standard cluster from the GUI](cs_clusters.html#clusters_ui). 
    - [Create a standard cluster from the CLI](cs_clusters.html#clusters_cli). 
    - [Use the Cloud Automation Manager (CAM) to create a cluster by using a pre-defined template![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SS2L37_2.1.0.3/cam_deploy_IKS.html). When you deploy a cluster with CAM, the Helm tiller is automatically installed for you.

3.  In your {{site.data.keyword.Bluemix}} Private cluster, deploy the strongSwan IPSec VPN service.

    1.  [Complete the strongSwan IPSec VPN workarounds ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SS2L37_2.1.0.3/cam_strongswan.html). 

    2.  [Install the strongSwan VPN Helm chart![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/app_center/create_release.html) in your private cluster.

4.  Get the public IP address of the {{site.data.keyword.Bluemix}} Private VPN gateway. The IP address was part of your [private cluster preliminary setup![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/installing/prep_cluster.html).

5.  In your {{site.data.keyword.Bluemix}} Public cluster, [deploy the strongSwan IPSec VPN service](cs_vpn.html#vpn-setup). Use the public IP address from the previous step and make sure to configure your VPN gateway in {{site.data.keyword.Bluemix}} Public for [outbound connection](cs_vpn.html#strongswan_3) so that the VPN connection is initiated from the cluster in {{site.data.keyword.Bluemix}} Public. 

6.  [Test the VPN connection](cs_vpn.html#vpn_test) between your clusters.

7.  Repeat these steps for each cluster that you want to connect. 


## Running {{site.data.keyword.Bluemix_notm}} Private images in public Kubernetes containers
{: #hybrid_ppa_importer}

You can run select licensed IBM products that were packaged for {{site.data.keyword.Bluemix_notm}} Private in a cluster in {{site.data.keyword.Bluemix_notm}} Public.  
{: shortdesc}

Licensed software is available in [IBM Passport Advantage ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www-01.ibm.com/software/passportadvantage/index.html). To use this software in a cluster in {{site.data.keyword.Bluemix_notm}} Public, you must download the software, extract the image, and upload the image to your namespace in {{site.data.keyword.registryshort}}. Independent of the environment where you plan to use the software, you must obtain the required license for the product first. 

The following table is an overview of available {{site.data.keyword.Bluemix_notm}} Private products that you can use in your cluster in {{site.data.keyword.Bluemix_notm}} Public.

| Product Name | Version | Part Number |
| --- | --- | --- |
| IBM Db2 Direct Advanced Edition Server | 11.1 | CNU3TML |
| IBM Db2 Advanced Enterprise Server Edition Server | 11.1 | CNU3SML |
| IBM MQ Advanced | 9.0.5 | CNU1VML |
| IBM WebSphere Application Server Liberty | 16.0.0.3 | Docker Hub image |
{: caption="Table. Supported {{site.data.keyword.Bluemix_notm}} Private products to be used in {{site.data.keyword.Bluemix_notm}} Public." caption-side="top"}

Before you begin: 
- [Install the {{site.data.keyword.registryshort}} CLI plug-in (`ibmcloud cr`)](/docs/services/Registry/registry_setup_cli_namespace.html#registry_cli_install). 
- [Set up a namespace in {{site.data.keyword.registryshort}}](/docs/services/Registry/registry_setup_cli_namespace.html#registry_namespace_add) or retrieve your existing namespace by running `ibmcloud cr namespaces`. 
- [Target your `kubectl` CLI to your cluster](/docs/containers/cs_cli_install.html#cs_cli_configure). 
- [Install the Helm CLI and set up tiller in your cluster](/docs/containers/cs_integrations.html#helm). 

To deploy an {{site.data.keyword.Bluemix_notm}} Private image in a cluster in {{site.data.keyword.Bluemix_notm}} Public:

1.  Follow the steps in the [{{site.data.keyword.registryshort}} documentation](/docs/services/Registry/ts_index.html#ts_ppa) to download the licensed software from IBM Passport Advantage, push the image to your namespace, and install the Helm chart in your cluster. 

    **For IBM WebSphere Application Server Liberty**:
    
    1.  Instead of obtaining the image from IBM Passport Advantage, use the [Docker Hub image ![External link icon](../icons/launch-glyph.svg "External link icon")](https://hub.docker.com/_/websphere-liberty/). For instructions on getting a production license, see [Upgrading the image from Docker Hub to a production image![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/WASdev/ci.docker/tree/master/ga/production-upgrade).
    
    2.  Follow the [Liberty Helm chart instructions![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/en/SSEQTP_liberty/com.ibm.websphere.wlp.doc/ae/rwlp_icp_helm.html). 

2.  Verify that the **STATUS** of the Helm chart shows `DEPLOYED`. If not, wait a few minutes, then try again.
    ```
    helm status <helm_chart_name>
    ```
    {: pre}
   
3.  Refer to the product-specific documentation for more information about how to configure and use the product with your cluster. 

    - [IBM Db2 Direct Advanced Edition Server ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.licensing.doc/doc/c0070181.html) 
    - [IBM MQ Advanced ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/en/SSFKSJ_9.0.0/com.ibm.mq.helphome.v90.doc/WelcomePagev9r0.html)
    - [IBM WebSphere Application Server Liberty ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/en/SSEQTP_liberty/as_ditamaps/was900_welcome_liberty.html)
