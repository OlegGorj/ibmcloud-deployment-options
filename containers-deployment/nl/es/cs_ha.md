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




# Alta disponibilidad para {{site.data.keyword.containerlong_notm}}
{: #ha}

Utilice las características incorporadas de Kubernetes y {{site.data.keyword.containerlong}} para aumentar la alta disponibilidad del clúster y proteger la app contra el tiempo de inactividad cuando falle un componente del clúster.
{: shortdesc}

La alta disponibilidad es una disciplina fundamental en una infraestructura de TI para mantener las apps en funcionamiento, incluso cuando se produce un error de sitio parcial o completo. El objetivo principal de la alta disponibilidad es eliminar posibles puntos de anomalía en una infraestructura de TI. Por ejemplo, puede prepararse por si falla un sistema añadiendo redundancia y estableciendo mecanismos de migración tras error.

Puede conseguir una alta disponibilidad en diferentes niveles en la infraestructura de TI y en diferentes componentes del clúster. El nivel de disponibilidad adecuado para cada usuario depende de varios factores, como los requisitos empresariales, los acuerdos de nivel de servicio que se tengan con los clientes y el dinero que se desee gastar.

## Visión general de posibles puntos de anomalía en {{site.data.keyword.containerlong_notm}}
{: #fault_domains} 

La arquitectura y la infraestructura de {{site.data.keyword.containerlong_notm}} están diseñadas para garantizar la fiabilidad, la baja latencia de procesamiento y el tiempo máximo de actividad del servicio. Sin embargo, se pueden producir errores. Según el servicio que aloje en {{site.data.keyword.Bluemix_notm}}, es posible que no pueda tolerar errores, aunque solo duren unos minutos.
{: shortdesc}

{{site.data.keyword.containershort_notm}} proporciona varios enfoques para añadir más disponibilidad al clúster añadiendo redundancia y antiafinidad. Revise la imagen siguiente para obtener más información sobre los posibles puntos de anomalía y sobre cómo eliminarlos.

<img src="images/cs_failure_ov.png" alt="Visión general de dominios de error en un clúster de alta disponibilidad dentro de una región de {{site.data.keyword.containershort_notm}}." width="250" style="width:250px; border-style: none"/>


<table summary="La tabla muestra los puntos de anomalía en {{site.data.keyword.containershort_notm}}. Las filas se leen de izquierda a derecha, con el número de punto de anomalía en la columna uno, el título del punto de anomalía en la columna dos, una descripción en la columna tres y un enlace a la documentación en la columna cuatro.">
<caption>Puntos de anomalía</caption>
<col width="3%">
<col width="10%">
<col width="70%">
<col width="17%">
  <thead>
  <th>#</th>
  <th>Punto de anomalía</th>
  <th>Descripción</th>
  <th>Enlace a documentación</th>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>Error de contenedor o pod</td>
      <td>Los contenedores y pods son, por diseño, efímeros y pueden fallar inesperadamente. Por ejemplo, un contenedor o pod puede colgarse si se produce un error en la app. Para que la app tenga alta disponibilidad, debe asegurarse de que tiene suficientes instancias de la app para manejar la carga de trabajo, además de instancias adicionales en caso de error. Idealmente, estas instancias se distribuyen entre varios nodos trabajadores para proteger la app ante un error del nodo trabajador.</td>
      <td>[Despliegue de apps de alta disponibilidad.](cs_app.html#highly_available_apps)</td>
  </tr>
  <tr>
    <td>2</td>
    <td>Error de nodo trabajador</td>
    <td>Un nodo trabajador es una máquina virtual que se ejecuta sobre un hardware físico. Los errores de nodo trabajador incluyen interrupciones de hardware, como alimentación, refrigeración o redes, y los problemas de la propia máquina virtual. Para estar preparado ante un error de nodo trabajador, puede configurar varios nodos trabajadores en el clúster. <br/><br/><strong>Nota:</strong> No se garantiza que los nodos trabajadores de una ubicación se alojen en hosts de cálculo físicos independientes. Por ejemplo, puede tener un clúster con 3 nodos trabajadores, pero que los 3 nodos trabajadores se hayan creado en el mismo host de cálculo físico en la ubicación de IBM. Si este host de cálculo físico pasa a estar inactivo, todos los nodos trabajadores estarán inactivos. Para estar protegido contra este error, debe configurar un segundo clúster en otra ubicación.</td>
    <td>[Creación de clústeres con varios nodos trabajadores.](cs_cli_reference.html#cs_cluster_create)</td>
  </tr>
  <tr>
    <td>3</td>
    <td>Error de clúster</td>
    <td>El maestro de Kubernetes es el componente principal que mantiene el clúster en funcionamiento. El maestro almacena todos los datos de clúster en la base de datos etcd, que sirve como único punto fiable para el clúster. Se produce un error de clúster cuando no se puede alcanzar el maestro debido a un fallo de red o cuando se corrompen los datos de la base de datos etcd. Puede crear varios clústeres en una ubicación para proteger las apps ante un error del maestro de Kubernetes o de etcd. Para equilibrar la carga entre los clústeres, debe configurar un equilibrador de carga externo. <br/><br/><strong>Nota:</strong> Configurar varios clústeres en una ubicación no garantiza que los nodos trabajadores se desplieguen en hosts de cálculo físicos independientes. Para estar protegido contra este error, debe configurar un segundo clúster en otra ubicación.</td>
    <td>[Configuración de clústeres de alta disponibilidad.](cs_clusters.html#planning_clusters)</td>
  </tr>
  <tr>
    <td>4</td>
    <td>Error de ubicación</td>
    <td>Los errores de ubicación afectan a todos los hosts de cálculo físico y al almacenamiento NFS. Los errores pueden ser interrupciones de alimentación, refrigeración, red o almacenamiento, y desastres naturales, como inundaciones, terremotos y huracanes. Para estar protegido ante un error de ubicación, debe tener clústeres en dos ubicaciones diferentes en las que equilibre la carga un equilibrador de carga externo.</td>
    <td>[Configuración de clústeres de alta disponibilidad.](cs_clusters.html#planning_clusters)</td>
  </tr>
  <tr>
    <td>5</td>
    <td>Error de región</td>
    <td>Cada región está configurada con un equilibrador de carga de alta disponibilidad al que se puede acceder desde el punto final de la API específico de la región. El equilibrador de carga direcciona solicitudes de entrada y salida a los clústeres en las ubicaciones regionales. La probabilidad de que se produzca un error global de región es baja. Sin embargo, para estar preparado ante este error, puede configurar varios clústeres en diferentes regiones y conectarlos mediante un equilibrador de carga externo. En caso de que falle toda una región, el clúster de la otra región puede asumir la carga de trabajo. <br/><br/><strong>Nota:</strong> Un clúster de varias regiones requiere varios recursos de nube y, en función de la app, puede ser complejo y costoso. Compruebe si necesita una configuración de varias regiones o si podría tolerar una interrupción potencial del servicio. Si desea configurar un clúster de varias regiones, asegúrese de que la app y los datos se pueden alojar en otra región, y que la app admite la réplica de datos globales.</td>
    <td>[Configuración de clústeres de alta disponibilidad.](cs_clusters.html#planning_clusters)</td>
  </tr>
  <tr>
    <td>6a, 6b</td>
    <td>Error de almacenamiento</td>
    <td>En una app con estado, los datos desempeñan un papel importante para mantener la app en funcionamiento. Desea asegurarse de que los datos son de alta disponibilidad para poder recuperarse de un error potencial. En {{site.data.keyword.containershort_notm}} puede elegir entre varias opciones para conservar los datos. Por ejemplo, puede suministrar almacenamiento NFS utilizando volúmenes persistentes nativos de Kubernetes, o almacenar los datos utilizando un servicio de base de datos de {{site.data.keyword.Bluemix_notm}}.</td>
    <td>[Planificación de datos de alta disponibilidad.](cs_storage.html#planning)</td>
  </tr>
  </tbody>
  </table>



