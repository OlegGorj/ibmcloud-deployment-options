---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-24"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}



# Regiones y ubicaciones
{{site.data.keyword.Bluemix}} está ubicado en todo el mundo. Una región es un área geográfica a la que accede un punto final. Las ubicaciones son centros de datos de una región. Los servicios de {{site.data.keyword.Bluemix_notm}} pueden estar disponibles a nivel global o dentro de una región específica. Cuando se crea un clúster de Kubernetes en {{site.data.keyword.containerlong}}, sus recursos permanecen en la región en la que se despliega el clúster.
{:shortdesc}

Las [regiones de {{site.data.keyword.Bluemix_notm}}](#bluemix_regions) difieren de las [regiones de {{site.data.keyword.containershort_notm}}](#container_regions).

![{{site.data.keyword.containershort_notm}} - Regiones y ubicaciones](/images/regions.png)

_Regiones y ubicaciones de {{site.data.keyword.containershort_notm}}_

Las regiones soportadas de {{site.data.keyword.containershort_notm}} son las siguientes:
  * AP Norte
  * AP Sur
  * UE Central
  * UK Sur
  * EE.UU. Este
  * EE.UU. Sur


## Puntos finales de API de regiones de {{site.data.keyword.Bluemix_notm}}
{: #bluemix_regions}

Puede organizar los recursos entre servicios de {{site.data.keyword.Bluemix_notm}} mediante regiones de {{site.data.keyword.Bluemix_notm}}. Por ejemplo, puede crear un clúster de Kubernetes utilizando una imagen de Docker privada almacenada en {{site.data.keyword.registryshort_notm}} de la misma región.
{:shortdesc}

Para comprobar la región de {{site.data.keyword.Bluemix_notm}} en la que está actualmente, ejecute `bx info` y revise el campo **Región**.

Se puede acceder a las regiones de {{site.data.keyword.Bluemix_notm}} especificando el punto final de API cuando se inicia la sesión. Si no especifica ninguna región, la sesión se iniciará automáticamente en la región más cercana.

Por ejemplo, utilice los siguientes mandatos para iniciar una sesión en los puntos finales de API de región de {{site.data.keyword.Bluemix_notm}}:

  * EE.UU. Sur y EE.UU. Este
      ```
      bx login -a api.ng.bluemix.net
      ```
      {: pre}

  * Sídney y AP Norte
      ```
      bx login -a api.au-syd.bluemix.net
      ```
      {: pre}

  * Alemania
      ```
      bx login -a api.eu-de.bluemix.net
      ```
      {: pre}

  * Reino Unido
      ```
      bx login -a api.eu-gb.bluemix.net
      ```
      {: pre}



<br />


## Puntos finales de API de regiones y ubicaciones de {{site.data.keyword.containershort_notm}}
{: #container_regions}

Mediante regiones de {{site.data.keyword.containershort_notm}}, puede crear o acceder a clústeres de Kubernetes de una región distinta de la región de {{site.data.keyword.Bluemix_notm}} en la que ha iniciado la sesión. Los puntos finales de regiones de {{site.data.keyword.containershort_notm}} hacen referencia específicamente a {{site.data.keyword.containershort_notm}}, no a {{site.data.keyword.Bluemix_notm}} en general.
{:shortdesc}

Puede acceder a {{site.data.keyword.containershort_notm}} mediante un punto final global: `https://containers.bluemix.net/`.
* Para comprobar la región de {{site.data.keyword.containershort_notm}} en la que está actualmente, ejecute `bx cs region`.
* Para recuperar una lista de regiones disponibles y sus puntos finales, ejecute `bx cs regions`.

Para utilizar la API con el punto final global, en todas las solicitudes, pase el nombre de región en una cabecera `X-Region`.
{: tip}

### Inicio de sesión en una región distinta de {{site.data.keyword.containerlong}_notm}
{: #container_login_endpoints}

Puede cambiar las ubicaciones utilizando la CLI de {{site.data.keyword.containershort_notm}}.
{:shortdesc}

Supongamos que desea iniciar una sesión en otra región de {{site.data.keyword.containershort_notm}} por las siguientes razones:
  * Ha creado servicios de {{site.data.keyword.Bluemix_notm}} o imágenes de Docker privadas en una región y desea utilizarlos con {{site.data.keyword.containershort_notm}} en otra región.
  * Desea acceder a un clúster de una región distinta de la región de {{site.data.keyword.Bluemix_notm}} predeterminada en la que ha iniciado la sesión.

</br>

Para cambiar regiones rápidamente, ejecute `bx cs region-set`.

### Utilización de mandatos de API de {{site.data.keyword.containerlong_notm}}
{: #containers_api}

Para interactuar con la API de {{site.data.keyword.containershort_notm}}, especifique el tipo de mandato y añada `/v1/command` al punto final global.
{:shortdesc}

Ejemplo de API `GET /clusters`:
  ```
  GET https://containers.bluemix.net/v1/clusters
  ```
  {: codeblock}

</br>

Para utilizar la API con el punto final global, en todas las solicitudes, pase el nombre de región en una cabecera `X-Region`. Para ver una lista de las regiones disponibles, ejecute `bx cs regions`.
{: tip}

Para ver la documentación sobre los mandatos de API, consulte [https://containers.bluemix.net/swagger-api/](https://containers.bluemix.net/swagger-api/).

## Ubicaciones disponibles en {{site.data.keyword.containershort_notm}}
{: #locations}

Las ubicaciones son centros de datos físicos que están disponibles dentro de una región de {{site.data.keyword.Bluemix_notm}}. Las regiones son una herramienta conceptual para organizar ubicaciones y pueden incluir ubicaciones (centros de datos) en diferentes países. La tabla siguiente muestra las ubicaciones disponibles por región.
{:shortdesc}

| Región | Ubicación | Ciudad |
|--------|----------|------|
| AP Norte | hkg02, seo01, sng01, tok02 | Hong Kong R.A.E. de la República Popular de China, Seúl, Singapur, Tokio |
| AP Sur     | mel01, syd01, syd04        | Melbourne, Sidney |
| UE Central     | ams03, fra02, par01        | Ámsterdam, Frankfurt, París |
| UK Sur      | lon02, lon04         | Londres |
| EE.UU. este      | mon01, tor01, wdc06, wdc07        | Montreal, Toronto, Washington DC |
| EE.UU. Sur     | dal10, dal12, dal13, sao01       | Dallas, São Paulo |
{: caption="Ubicaciones y regiones disponibles" caption-side="top"}

Los recursos del clúster permanecen en la ubicación (centro de datos) en la que se despliega el clúster. La imagen siguiente muestra la relación de su clúster dentro de una región de ejemplo de EE.UU. este:

1.  Los recursos del clúster, incluidos los nodos maestro y trabajador, están en la misma ubicación en la que se ha desplegado el clúster. Al crear acciones de orquestación del contenedor local, como por ejemplo mandatos `kubectl`, la información se intercambia entre los nodos maestro y trabajador dentro de la misma ubicación.

2.  Si ha configurado otros recursos de clúster, como almacenamiento, redes, de cálculo o apps que se ejecutan en pods, los recursos y sus datos permanecen en la ubicación en la que ha desplegado el clúster.

3.  Cuando crea acciones de gestión de clústeres, como el uso de mandatos `bx cs`, se direcciona información básica sobre el clúster (como el nombre, el ID, el usuario, el mandato) a un punto final regional.

![Ubicación de los recursos de un clúster](/images/region-cluster-resources.png)

_Ubicación de los recursos de un clúster. _



