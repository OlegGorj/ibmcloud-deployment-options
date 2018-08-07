---

copyright:
  years: 2017, 2018
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


# {{site.data.keyword.cloudaccesstrailshort}} events
{: #at_events}

You can view, manage, and audit user-initated activities in your {{site.data.keyword.containershort_notm}} cluster by using the {{site.data.keyword.cloudaccesstrailshort}} service.
{: shortdesc}

## Finding the information
{: #at-find}

{{site.data.keyword.cloudaccesstrailshort}} events are available in the {{site.data.keyword.cloudaccesstrailshort}} **account domain** that is available in the {{site.data.keyword.Bluemix_notm}} region where the events are generated. {{site.data.keyword.cloudaccesstrailshort}} events are available in the {{site.data.keyword.cloudaccesstrailshort}} **space domain** that is associated to the Cloud Foundry space where the {{site.data.keyword.cloudaccesstrailshort}} service is provisioned.

To monitor administrative activity:

1. Log in to your {{site.data.keyword.Bluemix_notm}} account.
2. From the catalog, provision an instance of the {{site.data.keyword.cloudaccesstrailshort}} service in the same account as your instance of  {{site.data.keyword.containershort_notm}}.
3. On the **Manage** tab of the {{site.data.keyword.cloudaccesstrailshort}} dashboard, click the **View in Kibana**.
4. Set the time frame that you want to view logs for. The default is 15 min.
5. In the **Available Fields** list, click **type**. Click the magnifying glass icon for **Activity Tracker** to limit the logs to only those tracked by the service.
6. You can use the other available fields to narrow your search.



## Tracking events
{: #events}

Check out the following tables for a list of the events that are sent to {{site.data.keyword.cloudaccesstrailshort}}.

For more information about how the service works, see the [{{site.data.keyword.cloudaccesstrailshort}} docs](/docs/services/cloud-activity-tracker/index.html).

For more information about the Kubernetes actions that are tracked, review the [Kubernetes documentation![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/home/).

<table>
  <tr>
    <th>Action</th>
    <th>Description</th>
  </tr>
  <tr>
    <td><code>bindings.create</code></td>
    <td>A binding is created.</td>
  </tr>
  <tr>
    <td><code>configmaps.create</code></td>
    <td>A configuration map is created.</td>
  </tr>
  <tr>
    <td><code>configmaps.delete</code></td>
    <td>A configuration map was deleted.</td>
  </tr>
  <tr>
    <td><code>configmaps.patch</code></td>
    <td>A configuration map is patched.</td>
  </tr>
  <tr>
    <td><code>configmaps.update</code></td>
    <td>A configuration map is updated.</td>
  </tr>
  <tr>
    <td><code>events.create</code></td>
    <td>An event was created.</td>
  </tr>
  <tr>
    <td><code>events.delete</code></td>
    <td>An event was deleted.</td>
  </tr>
  <tr>
    <td><code>events.patch</code></td>
    <td>An event was patched.</td>
  </tr>
  <tr>
    <td><code>events.update</code></td>
    <td>An event was updated.</td>
  </tr>
  <tr>
    <td><code>limitranges.create</code></td>
    <td>A range limit is created.</td>
  </tr>
  <tr>
    <td><code>limitranges.delete</code></td>
    <td>A range limit was deleted.</td>
  </tr>
  <tr>
    <td><code>limitranges.patch</code></td>
    <td>A range limit was patched.</td>
  </tr>
  <tr>
    <td><code>limitranges.update</code></td>
    <td>A range limit was updated.</td>
  </tr>
  <tr>
    <td><code>namespaces.create</code></td>
    <td>A namespace is created.</td>
  </tr>
  <tr>
    <td><code>namespaces.delete</code></td>
    <td>A namespace was deleted.</td>
  </tr>
  <tr>
    <td><code>namespaces.patch</code></td>
    <td>A namespace was patched.</td>
  </tr>
  <tr>
    <td><code>namespaces.update</code></td>
    <td>A namespace was updated.</td>
  </tr>
  <tr>
    <td><code>nodes.create</code></td>
    <td>A node is created.</td>
  </tr>
  <tr>
    <td><code>nodes.delete</code></td>
    <td>A node was deleted.</td>
  </tr>
  <tr>
    <td><code>nodes.delete</code></td>
    <td>A node was deleted.</td>
  </tr>
  <tr>
    <td><code>nodes.patch</code></td>
    <td>A node was patched.</td>
  </tr>
  <tr>
    <td><code>nodes.update</code></td>
    <td>A node was updated.</td>
  </tr>
  <tr>
    <td><code>persistentvolumeclaims.create</code></td>
    <td>A persistent volume claim was created.</td>
  </tr>
  <tr>
    <td><code>persistentvolumeclaims.delete</code></td>
    <td>A persistent volume claim was deleted.</td>
  </tr>
  <tr>
    <td><code>persistentvolumeclaims.patch</code></td>
    <td>A persistent volume claim was patched.</td>
  </tr>
  <tr>
    <td><code>persistentvolumeclaims.update</code></td>
    <td>A persistent volume claim was updated.</td>
  </tr>
  <tr>
    <td><code>persistentvolumes.create</code></td>
    <td>A persistent volume was created.</td>
  </tr>
  <tr>
    <td><code>persistentvolumes.delete</code></td>
    <td>A persistent volume was deleted.</td>
  </tr>
  <tr>
    <td><code>persistentvolumes.patch</code></td>
    <td>A persistent volume was patched.</td>
  </tr>
  <tr>
    <td><code>persistentvolumes.update</code></td>
    <td>A persistent volume was updated.</td>
  </tr>
  <tr>
    <td><code>pods.create</code></td>
    <td>A pod was created.</td>
  </tr>
  <tr>
    <td><code>pods.delete</code></td>
    <td>A pod was deleted.</td>
  </tr>
  <tr>
    <td><code>pods.patch</code></td>
    <td>A pod was patched.</td>
  </tr>
  <tr>
    <td><code>pods.update</code></td>
    <td>A pod was updated.</td>
  </tr>
  <tr>
    <td><code>podtemplates.create</code></td>
    <td>A pod template was created.</td>
  </tr>
  <tr>
    <td><code>podtemplates.delete</code></td>
    <td>A pod template was deleted.</td>
  </tr>
  <tr>
    <td><code>podtemplates.patch</code></td>
    <td>A pod template was patched.</td>
  </tr>
  <tr>
    <td><code>podtemplates.update</code></td>
    <td>A pod template was updated.</td>
  </tr>
  <tr>
    <td><code>replicationcontrollers.create</code></td>
    <td>A replication controller was created.</td>
  </tr>
  <tr>
    <td><code>replicationcontrollers.delete</code></td>
    <td>A replication controller was deleted.</td>
  </tr>
  <tr>
    <td><code>replicationcontrollers.patch</code></td>
    <td>A replication controller was patched.</td>
  </tr>
  <tr>
    <td><code>replicationcontrollers.update</code></td>
    <td>A replication controller was updated.</td>
  </tr>
  <tr>
    <td><code>resourcequotas.create</code></td>
    <td>A resource quota was created.</td>
  </tr>
  <tr>
    <td><code>resourcequotas.delete</code></td>
    <td>A resource quota was deleted.</td>
  </tr>
  <tr>
    <td><code>resourcequotas.patch</code></td>
    <td>A resource quota was patched.</td>
  </tr>
  <tr>
    <td><code>resourcequotas.update</code></td>
    <td>A resource quota was updated.</td>
  </tr>
  <tr>
    <td><code>secrets.create</code></td>
    <td>A secret was created.</td>
  </tr>
  <tr>
    <td><code>secrets.deleted</code></td>
    <td>A secret was deleted.</td>
  </tr>
  <tr>
    <td><code>secrets.get</code></td>
    <td>A secret was viewed.</td>
  </tr>
  <tr>
    <td><code>secrets.patch</code></td>
    <td>A secret was patched.</td>
  </tr>
  <tr>
    <td><code>secrets.updated</code></td>
    <td>A secret was updated.</td>
  </tr>
  <tr>
    <td><code>serviceaccounts.create</code></td>
    <td>A service account was created.</td>
  </tr>
  <tr>
    <td><code>serviceaccounts.deleted</code></td>
    <td>A service account was deleted.</td>
  </tr>
  <tr>
    <td><code>serviceaccounts.patch</code></td>
    <td>A service account was patched.</td>
  </tr>
  <tr>
    <td><code>serviceaccounts.updated</code></td>
    <td>A service account was updated.</td>
  </tr>
  <tr>
    <td><code>services.create</code></td>
    <td>A service was created.</td>
  </tr>
  <tr>
    <td><code>services.deleted</code></td>
    <td>A service was deleted.</td>
  </tr>
  <tr>
    <td><code>services.patch</code></td>
    <td>A service was patched.</td>
  </tr>
  <tr>
    <td><code>services.updated</code></td>
    <td>A service was updated.</td>
  </tr>
  <tr>
    <td><code>mutatingwebhookconfigurations.create</code></td>
    <td>In Kubernetes v1.9 and later, a mutating webhook configuration was created.</td>
  </tr>
  <tr>
    <td><code>mutatingwebhookconfigurations.delete</code></td>
    <td>In Kubernetes v1.9 and later, a mutating webhook configuration was deleted.</td>
  </tr>
  <tr>
    <td><code>mutatingwebhookconfigurations.patch</code></td>
    <td>In Kubernetes v1.9 and later, a mutating webhook configuration was patched.</td>
  </tr>
  <tr>
    <td><code>mutatingwebhookconfigurations.update</code></td>
    <td>In Kubernetes v1.9 and later, a mutating webhook configuration was updated.</td>
  </tr>
  <tr>
    <td><code>validatingwebhookconfigurations.create</code></td>
    <td>In Kubernetes v1.9 and later, a webhook configuration validation was created.</td>
  </tr>
  <tr>
    <td><code>validatingwebhookconfigurations.delete</code></td>
    <td>In Kubernetes v1.9 and later, a webhook configuration validation was deleted.</td>
  </tr>
  <tr>
    <td><code>validatingwebhookconfigurations.patch</code></td>
    <td>In Kubernetes v1.9 and later, a webhook configuration validation was patched.</td>
  </tr>
  <tr>
    <td><code>validatingwebhookconfigurations.update</code></td>
    <td>In Kubernetes v1.9 and later, a webhook configuration validation was updated.</td>
  </tr>
  <tr>
    <td><code>externaladmissionhookconfigurations.create</code></td>
    <td>In Kubernetes v1.8, an external admissions hook configuration was created.</td>
  </tr>
  <tr>
    <td><code>externaladmissionhookconfigurations.delete</code></td>
    <td>In Kubernetes v1.8, an external admissions hook configuration was deleted.</td>
  </tr>
  <tr>
    <td><code>externaladmissionhookconfigurations.patch</code></td>
    <td>In Kubernetes v1.8, an external admissions hook configuration was patched.</td>
  </tr>
  <tr>
    <td><code>externaladmissionhookconfigurations.update</code></td>
    <td>In Kubernetes v1.8, an external admissions hook configuration was updated.</td>
  </tr>
  <tr>
    <td><code>controllerrevisions.create</code></td>
    <td>A controller revision was created.</td>
  </tr>
  <tr>
    <td><code>controllerrevisions.delete</code></td>
    <td>A controller revision was deleted.</td>
  </tr>
  <tr>
    <td><code>controllerrevisions.patch</code></td>
    <td>A controller revision was patched.</td>
  </tr>
  <tr>
    <td><code>controllerrevisions.update</code></td>
    <td>A controller revision was updated.</td>
  </tr>
  <tr>
    <td><code>daemonsets.create</code></td>
    <td>A daemon set was created.</td>
  </tr>
  <tr>
    <td><code>daemonsets.delete</code></td>
    <td>A daemon set was deleted.</td>
  </tr>
  <tr>
    <td><code>daemonsets.patch</code></td>
    <td>A daemon set was patched.</td>
  </tr>
  <tr>
    <td><code>daemonsets.update</code></td>
    <td>A daemon set was updated.</td>
  </tr>
  <tr>
    <td><code>deployments.create</code></td>
    <td>A deployment was created.</td>
  </tr>
  <tr>
    <td><code>deployments.delete</code></td>
    <td>A deployment was deleted.</td>
  </tr>
  <tr>
    <td><code>deployments.patch</code></td>
    <td>A deployment was patched.</td>
  </tr>
  <tr>
    <td><code>deployments.update</code></td>
    <td>A deployment was updated.</td>
  </tr>
  <tr>
    <td><code>replicasets.create</code></td>
    <td>A replica set was created.</td>
  </tr>
  <tr>
    <td><code>replicasets.delete</code></td>
    <td>A replica set was deleted.</td>
  </tr>
  <tr>
    <td><code>replicasets.patch</code></td>
    <td>A replica set was patched.</td>
  </tr>
  <tr>
    <td><code>replicasets.update</code></td>
    <td>A replica set was updated.</td>
  </tr>
  <tr>
    <td><code>statefulsets.create</code></td>
    <td>A stateful set was created.</td>
  </tr>
  <tr>
    <td><code>statefulsets.delete</code></td>
    <td>A stateful set was deleted.</td>
  </tr>
  <tr>
    <td><code>statefulsets.patch</code></td>
    <td>A stateful set was patched.</td>
  </tr>
  <tr>
    <td><code>statefulsets.update</code></td>
    <td>A stateful set was updated.</td>
  </tr>
  <tr>
    <td><code>tokenreviews.create</code></td>
    <td>A token review was created.</td>
  </tr>
  <tr>
    <td><code>localsubjectaccessreviews.create</code></td>
    <td>A local subject access review was created.</td>
  </tr>
  <tr>
    <td><code>selfsubjectaccessreviews.create</code></td>
    <td>A self subject access review was created.</td>
  </tr>
  <tr>
    <td><code>selfsubjectrulesreviews.create</code></td>
    <td>A self subject rules review was created.</td>
  </tr>
  <tr>
    <td><code>subjectaccessreviews.create</code></td>
    <td>A subject access review was created.</td>
  </tr>
  <tr>
    <td><code>horizontalpodautoscalers.create</code></td>
    <td>A horizontal pod autoscaling policy was created.</td>
  </tr>
  <tr>
    <td><code>horizontalpodautoscalers.delete</code></td>
    <td>A horizontal pod autoscaling policy was deleted.</td>
  </tr>
  <tr>
    <td><code>horizontalpodautoscalers.patch</code></td>
    <td>A horizontal pod autoscaling policy was patched.</td>
  </tr>
  <tr>
    <td><code>horizontalpodautoscalers.update</code></td>
    <td>A horizontal pod autoscaling policy was updated.</td>
  </tr>
  <tr>
    <td><code>jobs.create</code></td>
    <td>A job was created.</td>
  </tr>
  <tr>
    <td><code>jobs.delete</code></td>
    <td>A job was deleted.</td>
  </tr>
  <tr>
    <td><code>jobs.patch</code></td>
    <td>A job was patched.</td>
  </tr>
  <tr>
    <td><code>jobs.update</code></td>
    <td>A job was updated.</td>
  </tr>
  <tr>
    <td><code>certificatesigningrequests.create</code></td>
    <td>A request to sign a certificate was created.</td>
  </tr>
  <tr>
    <td><code>certificatesigningrequests.delete</code></td>
    <td>A request to sign a certificate was deleted.</td>
  </tr>
  <tr>
    <td><code>certificatesigningrequests.patch</code></td>
    <td>A request to sign a certificate was patched.</td>
  </tr>
  <tr>
    <td><code>certificatesigningrequests.update</code></td>
    <td>A request to sign a certificatewas updated.</td>
  </tr>
  <tr>
    <td><code>ingresses.create</code></td>
    <td>An Ingress ALB was created.</td>
  </tr>
  <tr>
    <td><code>ingresses.delete</code></td>
    <td>A Ingress ALB was deleted.</td>
  </tr>
  <tr>
    <td><code>ingresses.patch</code></td>
    <td>A Ingress ALB was patched.</td>
  </tr>
  <tr>
    <td><code>ingresses.update</code></td>
    <td>A Ingress ALB was updated.</td>
  </tr>
  <tr>
    <td><code>networkpolicies.create</code></td>
    <td>A network policy was created.</td>
  </tr>
  <tr>
    <td><code>networkpolicies.delete</code></td>
    <td>A network policy was deleted.</td>
  </tr>
  <tr>
    <td><code>networkpolicies.patch</code></td>
    <td>A network policy was patched.</td>
  </tr>
  <tr>
    <td><code>networkpolicies.update</code></td>
    <td>A network policy was updated.</td>
  </tr>
  <tr>
    <td><code>podsecuritypolicies.create</code></td>
    <td>For Kubernetes v1.10 and higher, a pod security policy was created.</td>
  </tr>
  <tr>
    <td><code>podsecuritypolicies.delete</code></td>
    <td>For Kubernetes v1.10 and higher, a  pod security policy was deleted.</td>
  </tr>
  <tr>
    <td><code>podsecuritypolicies.patch</code></td>
    <td>For Kubernetes v1.10 and higher, a  pod security policy was patched.</td>
  </tr>
  <tr>
    <td><code>podsecuritypolicies.update</code></td>
    <td>For Kubernetes v1.10 and higher, a  pod security policy was updated.</td>
  </tr>
  <tr>
    <td><code>poddisruptionbugets.create</code></td>
    <td>A pod disruption buget was created.</td>
  </tr>
  <tr>
    <td><code>poddisruptionbugets.delete</code></td>
    <td>A pod disruption buget was deleted.</td>
  </tr>
  <tr>
    <td><code>poddisruptionbugets.patch</code></td>
    <td>A pod disruption buget was patched.</td>
  </tr>
  <tr>
    <td><code>poddisruptionbugets.update</code></td>
    <td>A pod disruption buget was updated.</td>
  </tr>
  <tr>
    <td><code>clusterbindings.create</code></td>
    <td>A cluster role binding is created.</td>
  </tr>
  <tr>
    <td><code>clusterbindings.deleted</code></td>
    <td>A cluster role binding is deleted.</td>
  </tr>
  <tr>
    <td><code>clusterbindings.patched</code></td>
    <td>A cluster role binding is patched.</td>
  </tr>
  <tr>
    <td><code>clusterbindings.updated</code></td>
    <td>A cluster role binding is updated.</td>
  </tr>
  <tr>
    <td><code>clusterroles.create</code></td>
    <td>A cluster role was created.</td>
  </tr>
  <tr>
    <td><code>clusterroles.deleted</code></td>
    <td>A cluster role was deleted.</td>
  </tr>
  <tr>
    <td><code>clusterroles.patched</code></td>
    <td>A cluster role was patched.</td>
  </tr>
  <tr>
    <td><code>clusterroles.updated</code></td>
    <td>A cluster role was updated.</td>
  </tr>
  <tr>
    <td><code>rolebindings.create</code></td>
    <td>A role binding was created.</td>
  </tr>
  <tr>
    <td><code>rolebindings.deleted</code></td>
    <td>A role binding was deleted.</td>
  </tr>
  <tr>
    <td><code>rolebindings.patched</code></td>
    <td>A role binding was patched.</td>
  </tr>
  <tr>
    <td><code>rolebindings.updated</code></td>
    <td>A role binding was updated.</td>
  </tr>
  <tr>
    <td><code>roles.create</code></td>
    <td>A role was created.</td>
  </tr>
  <tr>
    <td><code>roles.deleted</code></td>
    <td>A role was deleted.</td>
  </tr>
  <tr>
    <td><code>roles.patched</code></td>
    <td>A role was patched.</td>
  </tr>
  <tr>
    <td><code>roles.updated</code></td>
    <td>A role was updated.</td>
  </tr>
  <tr>
    <td><code>podpresets.create</code></td>
    <td>A pod preset was created.</td>
  </tr>
  <tr>
    <td><code>podpresets.deleted</code></td>
    <td>A pod preset was deleted.</td>
  </tr>
  <tr>
    <td><code>podpresets.patched</code></td>
    <td>A pod preset was patched.</td>
  </tr>
  <tr>
    <td><code>podpresets.updated</code></td>
    <td>A pod preset was updated.</td>
  </tr>
</table>
