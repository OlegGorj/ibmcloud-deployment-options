---

copyright:
  years: 2014, 2018
lastupdated: "2018-4-20"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# 访问 IBM Cloud Infrastructure (SoftLayer) 产品服务组合
{: #infrastructure}

要创建标准 Kubernetes 集群，您必须具有对 IBM Cloud infrastructure (SoftLayer) 产品服务组合的访问权。需要此访问权才能为 {{site.data.keyword.containerlong}} 中的 Kubernetes 集群请求付费基础架构资源，如工作程序节点、可移植公共 IP 地址或持久存储器。
{:shortdesc}

## 访问 IBM Cloud infrastructure (SoftLayer) 产品服务组合
{: #unify_accounts}

在自动帐户链接启用后创建的 {{site.data.keyword.Bluemix_notm}} 现买现付帐户已设置有对 IBM Cloud Infrastructure (SoftLayer) 产品服务组合的访问权。您可以为集群购买基础架构资源，而无需额外配置。
{:shortdesc}

具有其他 {{site.data.keyword.Bluemix_notm}} 帐户类型的用户或具有未链接到其 {{site.data.keyword.Bluemix_notm}} 帐户的现有 IBM Cloud infrastructure (SoftLayer) 帐户的用户必须对其帐户进行配置才能创建标准集群。
{:shortdesc}

请查看下表以查找每种帐户类型的可用选项。

|帐户类型|描述|用于创建标准集群的可用选项|
|------------|-----------|----------------------------------------------|
|Lite 帐户|Lite 帐户无法供应集群。|[将 Lite 帐户升级到 {{site.data.keyword.Bluemix_notm}} 现买现付帐户](/docs/account/index.html#billableacts)，该帐户设置为具有对 IBM Cloud infrastructure (SoftLayer) 产品服务组合的访问权。|
|较旧的现买现付帐户|在自动帐户链接可用之前创建的现买现付帐户没有对 IBM Cloud infrastructure (SoftLayer) 产品服务组合的访问权。<p>如果您有现有的 IBM Cloud infrastructure (SoftLayer) 帐户，那么无法将此帐户链接到较旧的现买现付帐户。</p>|选项 1：[创建新的现买现付帐户](/docs/account/index.html#billableacts)，该帐户设置为具有对 IBM Cloud infrastructure (SoftLayer) 产品服务组合的访问权。选择此选项时，您有两个单独的 {{site.data.keyword.Bluemix_notm}} 帐户和帐单。<p>如果要继续使用旧的现买现付帐户来创建标准集群，那么可以使用新的现买现付帐户生成 API 密钥，以用于访问 IBM Cloud infrastructure (SoftLayer) 产品服务组合。然后，必须为旧的现买现付帐户设置 API 密钥。有关更多信息，请参阅[为旧的现买现付和预订帐户生成 API 密钥](#old_account)。请记住，IBM Cloud infrastructure (SoftLayer) 资源将通过新的现买现付帐户进行计费。</p></br><p>选项 2：如果您已经拥有要使用的现有 IBM Cloud infrastructure (SoftLayer) 帐户，那么可以为 {{site.data.keyword.Bluemix_notm}} 帐户[设置凭证](cs_cli_reference.html#cs_credentials_set)。</p><p>**注**：必须使用“超级用户”许可权设置与 {{site.data.keyword.Bluemix_notm}} 帐户一起使用的 IBM Cloud infrastructure (SoftLayer) 帐户。</p>|
|预订帐户|预订帐户未设置为具有对 IBM Cloud infrastructure (SoftLayer) 产品服务组合的访问权。|选项 1：[创建新的现买现付帐户](/docs/account/index.html#billableacts)，该帐户设置为具有对 IBM Cloud infrastructure (SoftLayer) 产品服务组合的访问权。选择此选项时，您有两个单独的 {{site.data.keyword.Bluemix_notm}} 帐户和帐单。<p>如果要继续使用预订帐户来创建标准集群，那么可以使用新的现买现付帐户生成 API 密钥，以用于访问 IBM Cloud infrastructure (SoftLayer) 产品服务组合。然后，必须为预订帐户设置 API 密钥。有关更多信息，请参阅[为旧的现买现付和预订帐户生成 API 密钥](#old_account)。请记住，IBM Cloud infrastructure (SoftLayer) 资源将通过新的现买现付帐户进行计费。</p></br><p>选项 2：如果您已经拥有要使用的现有 IBM Cloud infrastructure (SoftLayer) 帐户，那么可以为 {{site.data.keyword.Bluemix_notm}} 帐户[设置凭证](cs_cli_reference.html#cs_credentials_set)。<p>**注**：必须使用“超级用户”许可权设置与 {{site.data.keyword.Bluemix_notm}} 帐户一起使用的 IBM Cloud infrastructure (SoftLayer) 帐户。</p>|
|IBM Cloud infrastructure (SoftLayer) 帐户，无 {{site.data.keyword.Bluemix_notm}} 帐户|要创建标准集群，您必须具有 {{site.data.keyword.Bluemix_notm}} 帐户。|<p>[创建新的现买现付帐户](/docs/account/index.html#billableacts)，该帐户设置为具有对 IBM Cloud infrastructure (SoftLayer) 产品服务组合的访问权。选择此选项时，将为您创建 IBM Cloud infrastructure (SoftLayer)。您有两个独立的 IBM Cloud infrastructure (SoftLayer) 帐户，两者单独进行计费。</p>|

<br />


## 生成要用于 {{site.data.keyword.Bluemix_notm}} 帐户的 IBM Cloud infrastructure (SoftLayer) API 密钥
{: #old_account}

要继续使用旧的现买现付或预订帐户来创建标准集群，请使用新的现买现付帐户生成 API 密钥，并为旧帐户设置 API 密钥。
{:shortdesc}

开始之前，请先创建一个自动设置为具有 IBM Cloud infrastructure (SoftLayer) 产品服务组合访问权的 {{site.data.keyword.Bluemix_notm}} 现买现付帐户。

1.  使用为新的现买现付帐户创建的 {{site.data.keyword.ibmid}}和密码登录到 [IBM Cloud infrastructure (SoftLayer) 门户网站 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://control.softlayer.com/)。
2.  选择**帐户**，然后选择**用户**。
3.  单击**生成**，为新的现买现付帐户生成 IBM Cloud infrastructure (SoftLayer) API 密钥。
4.  复制 API 密钥。
5.  在 CLI 中，使用旧的现买现付或预订帐户的 {{site.data.keyword.ibmid}}和密码登录到 {{site.data.keyword.Bluemix_notm}}。

  ```
  bx login
  ```
  {: pre}

6.  设置先前生成的 API 密钥，以用于访问 IBM Cloud infrastructure (SoftLayer) 产品服务组合。将 `<API_key>` 替换为 API 密钥，将 `<username>` 替换为新的现买现付帐户的 {{site.data.keyword.ibmid}}。

  ```
  bx cs credentials-set --infrastructure-api-key <API_key> --infrastructure-username <username>
  ```
  {: pre}

7.  开始[创建标准集群](cs_clusters.html#clusters_cli)。

**注**：要在生成 API 密钥后对其进行复查，请执行步骤 1 和 2，然后在 **API 密钥**部分中，单击**查看**以查看适用于您用户标识的 API 密钥。

