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


# Acceso al portafolio de infraestructura de IBM Cloud (SoftLayer)
{: #infrastructure}

Para crear un clúster de Kubernetes estándar, debe tener acceso al portafolio de infraestructura de IBM Cloud (SoftLayer). Este acceso es necesario para solicitar recursos de infraestructura de pago como, por ejemplo, nodos trabajadores, direcciones de IP públicas portátiles o almacenamiento persistente para su clúster de Kubernetes en {{site.data.keyword.containerlong}}.
{:shortdesc}

## Acceda al portafolio de infraestructura de IBM Cloud (SoftLayer)
{: #unify_accounts}

Las cuentas de Pago según uso de {{site.data.keyword.Bluemix_notm}} creadas después de haber enlazado una cuenta automática ya se han configurado con acceso a la infraestructura de IBM Cloud (SoftLayer). Es posible adquirir recursos de la infraestructura para el clúster sin configuración adicional.
{:shortdesc}

Los usuarios con otros tipos de cuenta de {{site.data.keyword.Bluemix_notm}} o los usuarios con cuentas existentes de infraestructura de IBM Cloud (SoftLayer) no enlazadas a su cuenta de {{site.data.keyword.Bluemix_notm}}, primero deben configurar sus cuentas para crear clústeres estándares.
{:shortdesc}

Revise la siguiente tabla para encontrar las opciones disponibles para cada tipo de cuenta.

|Tipo de cuenta|Descripción|Opciones disponibles para crear un clúster estándar|
|------------|-----------|----------------------------------------------|
|Cuentas Lite|Las cuentas Lite no pueden suministrar clústeres.|[Actualice su cuenta Lite a una cuenta de {{site.data.keyword.Bluemix_notm}} de Pago según uso](/docs/account/index.html#billableacts) que está configurada para acceder al portafolio de infraestructura de IBM Cloud (SoftLayer).|
|Cuenta antiguas de Pago según uso|Las cuentas de Pago según uso que se crearon antes de que estuviese disponible el enlace de cuentas automático, no tenían acceso al portafolio de infraestructura de IBM Cloud (SoftLayer).<p>Si tiene una cuenta existente de infraestructura de IBM Cloud (SoftLayer), no puede enlazarla a una cuenta antigua de Pago según uso.</p>|Opción 1: [Crear una nueva cuenta de Pago según uso](/docs/account/index.html#billableacts) que esté configurada para acceder al portafolio de infraestructura de IBM Cloud (SoftLayer). Cuando elige esta opción, tendrá dos cuentas y dos facturaciones distintas para {{site.data.keyword.Bluemix_notm}}.<p>Si desea continuar utilizando su cuenta antigua de Pago según uso para crear clústeres estándares, debe utilizar su nueva cuenta de Pago según uso para generar una clave de API para acceder al portafolio de infraestructura de IBM Cloud (SoftLayer). A continuación, debe configurar la clave de la API para su cuenta antigua de Pago según uso. Para obtener más información, consulte [Generación de una clave de API para cuentas antiguas de Suscripción y Pago según uso](#old_account). Tenga en cuenta que los recursos de infraestructura de IBM Cloud (SoftLayer) se facturarán a través de su nueva cuenta de Pago según uso.</p></br><p>Opción 2: Si ya tiene una cuenta de infraestructura de IBM Cloud (SoftLayer) existente que desea utilizar, puede [configurar sus credenciales](cs_cli_reference.html#cs_credentials_set) para su cuenta de {{site.data.keyword.Bluemix_notm}}.</p><p>**Nota:** La cuenta de infraestructura de IBM Cloud (SoftLayer) que utiliza con su cuenta de {{site.data.keyword.Bluemix_notm}} se debe configurar con permisos de superusuario.</p>|
|Cuentas de Suscripción|Las cuentas de Suscripción no se configuran con acceso al portafolio de infraestructura de IBM Cloud (SoftLayer).|Opción 1: [Crear una nueva cuenta de Pago según uso](/docs/account/index.html#billableacts) que esté configurada para acceder al portafolio de infraestructura de IBM Cloud (SoftLayer). Cuando elige esta opción, tendrá dos cuentas y dos facturaciones distintas para {{site.data.keyword.Bluemix_notm}}.<p>Si desea continuar utilizando su cuenta de Suscripción para crear clústeres estándares, debe utilizar su nueva cuenta de Pago según uso para generar una clave de API para acceder al portafolio de infraestructura de IBM Cloud (SoftLayer). A continuación, debe configurar la clave de la API para su cuenta de Suscripción. Para obtener más información, consulte [Generación de una clave de API para cuentas antiguas de Suscripción y Pago según uso](#old_account). Tenga en cuenta que los recursos de infraestructura de IBM Cloud (SoftLayer) se facturarán a través de su nueva cuenta de Pago según uso.</p></br><p>Opción 2: Si ya tiene una cuenta de infraestructura de IBM Cloud (SoftLayer) existente que desea utilizar, puede [configurar sus credenciales](cs_cli_reference.html#cs_credentials_set) para su cuenta de {{site.data.keyword.Bluemix_notm}}.<p>**Nota:** La cuenta de infraestructura de IBM Cloud (SoftLayer) que utiliza con su cuenta de {{site.data.keyword.Bluemix_notm}} se debe configurar con permisos de superusuario.</p>|
|Cuentas de infraestructura de IBM Cloud (SoftLayer), no cuenta de {{site.data.keyword.Bluemix_notm}}|Para crear un clúster estándar, debe tener una cuenta de {{site.data.keyword.Bluemix_notm}}.|<p>[Crear una nueva cuenta de Pago según uso](/docs/account/index.html#billableacts) que esté configurada para acceder al portafolio de infraestructura de IBM Cloud (SoftLayer). Cuando elige esta opción, se crea en su nombre una cuenta de infraestructura de IBM Cloud (SoftLayer). Tiene dos cuentas y facturaciones separadas de infraestructura de IBM Cloud (SoftLayer).</p>|

<br />


## Generación de una clave API de infraestructura de IBM Cloud (SoftLayer) para utilizar con cuentas de {{site.data.keyword.Bluemix_notm}}
{: #old_account}

Para continuar utilizando su cuenta antigua de Suscripción o de Pago según uso para crear clústeres estándares, genere una clave de API con la nueva cuenta de Pago según uso y establecer la clave de API para su cuenta antigua.
{:shortdesc}

Antes de continuar, cree una cuenta de {{site.data.keyword.Bluemix_notm}} de Pago según uso que está automáticamente configurada para acceder al portafolio de infraestructura de IBM Cloud (SoftLayer).

1.  Inicie una sesión en el portal de [infraestructura de IBM Cloud (SoftLayer) ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://control.softlayer.com/) con el {{site.data.keyword.ibmid}} y la contraseña que ha creado para su cuenta de pago según uso.
2.  Seleccione **Cuenta** y, a continuación, **Usuarios**.
3.  Pulse **Generar** para generar una clave de API de infraestructura de IBM Cloud (SoftLayer) para su cuenta de Pago según uso.
4.  Copia la clave de la API.
5.  Desde la interfaz de línea de mandatos, inicie una sesión en {{site.data.keyword.Bluemix_notm}}
utilizando el {{site.data.keyword.ibmid}} y la contraseña de su cuenta antigua de Pago según uso o Suscripción.

  ```
  bx login
  ```
  {: pre}

6.  Establezca la clave de API que generó con anterioridad para acceder al portafolio de infraestructura de IBM Cloud (SoftLayer). Sustituya `<API_key>` por la clave de API y `<username>` por el {{site.data.keyword.ibmid}} de la nueva cuenta de pago según uso.

  ```
  bx cs credentials-set --infrastructure-api-key <API_key> --infrastructure-username <username>
  ```
  {: pre}

7.  Empiece [creando clústeres estándares](cs_clusters.html#clusters_cli).

**Nota:** Para revisar la clave de la API después de generarla, siga el paso 1 y el paso 2 y, a continuación, en la sección de **Clave de API**, pulse **Ver** para ver la clave de API para su ID de usuario.

