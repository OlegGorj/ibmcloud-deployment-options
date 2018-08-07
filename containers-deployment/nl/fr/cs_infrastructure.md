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


# Accès au portefeuille d'infrastructure IBM Cloud (SoftLayer)
{: #infrastructure}

Pour créer un cluster Kubernetes standard, vous devez avoir accès au portefeuille d'infrastructure IBM Cloud (SoftLayer). Cet accès est nécessaire pour demander des ressources d'infrastructure payantes, telles que des noeuds worker, des adresses IP publiques portables ou un stockage persistant pour votre cluster Kubernetes dans {{site.data.keyword.containerlong}}.
{:shortdesc}

## Accès au portefeuille d'infrastructure IBM Cloud (SoftLayer)
{: #unify_accounts}

Les comptes Paiement à la carte {{site.data.keyword.Bluemix_notm}} créés après l'activation de la liaison automatique de compte sont déjà configurés avec l'accès au portefeuille d'infrastructure IBM Cloud (SoftLayer). Vous pouvez acheter des ressources d'infrastructure pour votre cluster sans configuration supplémentaire.
{:shortdesc}

Les utilisateurs disposant d'autres types de compte {{site.data.keyword.Bluemix_notm}}, ou disposant d'un compte d'infrastructure IBM Cloud (SoftLayer) existant non lié à leur compte {{site.data.keyword.Bluemix_notm}}, doivent configurer leurs comptes pour créer des clusters standard.
{:shortdesc}

Consultez le tableau suivant pour identifier les options disponibles pour chaque type de compte.

|Type de compte|Description|Options disponibles pour création d'un cluster standard|
|------------|-----------|----------------------------------------------|
|Comptes Lite|Les comptes Lite ne peuvent pas provisionner des clusters.|[Mettez à niveau votre compte Lite vers un compte {{site.data.keyword.Bluemix_notm}}  Pay-As-You-Go](/docs/account/index.html#billableacts) (Paiement à la carte) configuré avec accès au portefeuille d'infrastructure IBM Cloud (SoftLayer).|
|Anciens comptes Paiement à la carte|Les comptes Paiement à la carte créés avant que la liaison automatique de compte ne soit disponible ne disposaient pas d'un accès au portefeuille d'infrastructure IBM Cloud (SoftLayer).<p>Si vous disposez d'un compte d'infrastructure IBM Cloud (SoftLayer) existant, vous ne pouvez pas le lier à un ancien compte Paiement à la carte.</p>|Option 1 : [Créez un compte Paiement à la carte](/docs/account/index.html#billableacts) configuré avec accès au portefeuille d'infrastructure IBM Cloud (SoftLayer). Si vous sélectionnez cette option, vous disposerez de deux comptes {{site.data.keyword.Bluemix_notm}} avec facturation distincte.<p>Si vous souhaitez continuer à utiliser votre ancien compte Paiement à la carte pour créer des clusters standard, vous pouvez utiliser le nouveau compte Paiement à la carte pour générer une clé d'API pour accéder au portefeuille d'infrastructure IBM Cloud (SoftLayer). Vous devez alors configurer la clé d'API pour votre ancien compte Paiement à la carte. Pour plus d'informations, voir [Génération d'une clé d'API pour d'anciens comptes Paiement à la carte et Abonnement](#old_account). N'oubliez pas que les ressources d'infrastructure IBM Cloud (SoftLayer) sont facturées via votre nouveau compte Paiement à la carte.</p></br><p>Option 2 : Si vous disposez déjà d'un compte d'infrastructure IBM Cloud (SoftLayer) existant que vous désirez utiliser, vous pouvez [définir vos données d'identification](cs_cli_reference.html#cs_credentials_set) pour votre compte {{site.data.keyword.Bluemix_notm}}.</p><p>**Remarque :** le compte d'infrastructure IBM Cloud (SoftLayer) que vous utilisez avec votre compte {{site.data.keyword.Bluemix_notm}} doit être configuré avec des droits superutilisateur.</p>|
|Comptes d'abonnement (Subscription)|Les comptes de type Abonnement ne sont pas configurés avec un accès au portefeuille d'infrastructure IBM Cloud (SoftLayer).|Option 1 : [Créez un compte Paiement à la carte](/docs/account/index.html#billableacts) configuré avec accès au portefeuille d'infrastructure IBM Cloud (SoftLayer). Si vous sélectionnez cette option, vous disposerez de deux comptes {{site.data.keyword.Bluemix_notm}} avec facturation distincte.<p>Si vous souhaitez continuer à utiliser votre ancien compte Abonnement pour créer des clusters standard, vous pouvez utiliser le nouveau compte Paiement à la carte pour générer une clé d'API pour accéder au portefeuille d'infrastructure IBM Cloud (SoftLayer). Vous devez ensuite configurer votre clé d'API pour votre compte Abonnement. Pour plus d'informations, voir [Génération d'une clé d'API pour d'anciens comptes Paiement à la carte et Abonnement](#old_account). N'oubliez pas que les ressources d'infrastructure IBM Cloud (SoftLayer) sont facturées via votre nouveau compte Paiement à la carte.</p></br><p>Option 2 : Si vous disposez déjà d'un compte d'infrastructure IBM Cloud (SoftLayer) existant que vous désirez utiliser, vous pouvez [définir vos données d'identification](cs_cli_reference.html#cs_credentials_set) pour votre compte {{site.data.keyword.Bluemix_notm}}.<p>**Remarque :** le compte d'infrastructure IBM Cloud (SoftLayer) que vous utilisez avec votre compte {{site.data.keyword.Bluemix_notm}} doit être configuré avec des droits superutilisateur.</p>|
|Compte d'infrastructure IBM Cloud (SoftLayer), aucun compte {{site.data.keyword.Bluemix_notm}}|Pour créer un cluster standard, vous devez disposer d'un compte {{site.data.keyword.Bluemix_notm}}.|<p>[Créez un nouveau compte de type Paiement à la carte](/docs/account/index.html#billableacts) configuré pour accès au portefeuille d'infrastructure IBM Cloud (SoftLayer). Si vous sélectionnez cette option, une infrastructure IBM Cloud (SoftLayer) est créée pour vous. Vous disposez de deux comptes d'infrastructure IBM Cloud (SoftLayer) différents avec facturation distincte.</p>|

<br />


## Génération d'une clé d'API d'infrastructure IBM Cloud (SoftLayer) à utiliser avec des comptes {{site.data.keyword.Bluemix_notm}}
{: #old_account}

Pour continuer à utiliser votre ancien compte Paiement à la carte ou Abonnement pour créer des clusters standard, générez une clé d'API avec votre nouveau compte Paiement à la carte et configurez-la pour votre ancien compte.
{:shortdesc}

Avant de commencer, créez un compte Paiement à la carte {{site.data.keyword.Bluemix_notm}} configuré automatiquement avec accès au portefeuille d'infrastructure IBM Cloud (SoftLayer).

1.  Connectez-vous au portail [d'infrastructure IBM Cloud (SoftLayer)![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com/) avec l'{{site.data.keyword.ibmid}} et le mot de passe que vous avez créés pour votre nouveau compte Paiement à la carte.
2.  Sélectionnez **Compte**, puis **Utilisateurs**.
3.  Cliquez sur **Générer** pour générer une clé d'API d'infrastructure IBM Cloud (SoftLayer) pour votre nouveau compte Paiement à la carte.
4.  Copiez la clé d'API.
5.  Depuis l'interface CLI, connectez-vous à {{site.data.keyword.Bluemix_notm}} à l'aide de l'{{site.data.keyword.ibmid}} et du mot de passe de votre ancien compte de type Paiement à la carte ou Abonnement.

  ```
  bx login
  ```
  {: pre}

6.  Configurez la clé d'API que vous avez générée pour accéder au portefeuille d'infrastructure IBM Cloud (SoftLayer). Remplacez `<API_key>` par la clé d'API et `<username>` par l'{{site.data.keyword.ibmid}} de votre nouveau compte Paiement à la carte.

  ```
  bx cs credentials-set --infrastructure-api-key <API_key> --infrastructure-username <username>
  ```
  {: pre}

7.  Commencez à [créer des clusters standard](cs_clusters.html#clusters_cli).

**Remarque :** pour examiner votre clé d'API après l'avoir générée, suivez les étapes 1 et 2, puis dans la section **Clé d'API**, cliquez sur **Afficher** pour visualiser la clé d'API pour votre ID utilisateur.

