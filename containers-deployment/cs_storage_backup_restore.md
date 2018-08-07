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


# Backing up and restoring data in persistent volumes
{: #backup_restore}

File shares and block storage are provisioned into the same zone as your cluster. The storage is hosted on clustered servers by {{site.data.keyword.IBM_notm}} to provide availability in case a server goes down. However, file shares and block storage are not backed up automatically and might be inaccessible if the entire zone fails. To protect your data from being lost or damaged, you can set up periodic backups that you can use to restore your data when needed.
{: shortdesc}

Review the following backup and restore options for your NFS file shares and block storage:

<dl>
  <dt>Set up periodic snapshots</dt>
  <dd><p>You can set up periodic snapshots for your NFS file share or block storage, which is a read-only image that captures the state of the instance at a point in time. To store the snapshot, you must request snapshot space on your NFS file share or block storage. Snapshots are stored on the existing storage instance within the same zone. You can restore data from a snapshot if a user accidentally removes important data from the volume. </br></br> <strong>To create a snapshot for your volume: </strong><ol><li>List existing PVs in your cluster. <pre class="pre"><code>kubectl get pv</code></pre></li><li>Get the details for the PV for which you want to create snapshot space and note the volume ID, the size and the IOPS. <pre class="pre"><code>kubectl describe pv &lt;pv_name&gt;</code></pre> For file storage, the volume ID, the size and the IOPS can be found in the <strong>Labels</strong> section of your CLI output. For block storage, the size and IOPS are shown in the <strong>Labels</strong> section of your CLI output. To find the volume ID, review the <code>ibm.io/network-storage-id</code> annotation of your CLI output. </li><li>Create the snapshot size for your existing volume with the parameters that you retrieved in the previous step. <pre class="pre"><code>slcli file snapshot-order --capacity &lt;size&gt; --tier &lt;iops&gt; &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-order --capacity &lt;size&gt; --tier &lt;iops&gt; &lt;volume_id&gt;</code></pre></li><li>Wait for the snapshot size to create. <pre class="pre"><code>slcli file volume-detail &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block volume-detail &lt;volume_id&gt;</code></pre>The snapshot size is successfully provisioned when the <strong>Snapshot Capacity (GB)</strong> in your CLI output changes from 0 to the size that you ordered. </li><li>Create the snapshot for your volume and note the ID of the snapshot that is created for you. <pre class="pre"><code>slcli file snapshot-create &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-create &lt;volume_id&gt;</code></pre></li><li>Verify that the snapshot is created successfully. <pre class="pre"><code>slcli file volume-detail &lt;snapshot_id&gt;</code></pre><pre class="pre"><code>slcli block volume-detail &lt;snapshot_id&gt;</code></pre></li></ol></br><strong>To restore data from a snapshot to an existing volume: </strong><pre class="pre"><code>slcli file snapshot-restore -s &lt;snapshot_id&gt; &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-restore -s &lt;snapshot_id&gt; &lt;volume_id&gt;</code></pre></br>For more information, see:<ul><li>[NFS periodic snapshots](/docs/infrastructure/FileStorage/snapshots.html)</li><li>[Block periodic snapshots](/docs/infrastructure/BlockStorage/snapshots.html#snapshots)</li></ul></p></dd>
  <dt>Replicate snapshots to another zone</dt>
 <dd><p>To protect your data from a zone failure, you can [replicate snapshots](/docs/infrastructure/FileStorage/replication.html#replicating-data) to an NFS file share or block storage instance that is set up in another zone. Data can be replicated from the primary storage to the backup storage only. You cannot mount a replicated NFS file share or block storage instance to a cluster. When your primary storage fails, you can manually set your replicated backup storage to be the primary one. Then, you can mount it to your cluster. After your primary storage is restored, you can restore the data from the backup storage.</p>
 <p>For more information, see:<ul><li>[Replicate snapshots for NFS](/docs/infrastructure/FileStorage/replication.html)</li><li>[Replicate snapshots for Block](/docs/infrastructure/BlockStorage/replication.html#replicating-data)</li></ul></p></dd>
 <dt>Duplicate storage</dt>
 <dd><p>You can duplicate your NFS file share or block storage instance in the same zone as the original storage instance. A duplicate has the same data as the original storage instance at the point in time that you create the duplicate. Unlike replicas, use the duplicate as an independent storage instance from the original. To duplicate, first set up snapshots for the volume.</p>
 <p>For more information, see:<ul><li>[NFS duplicate snapshots](/docs/infrastructure/FileStorage/how-to-create-duplicate-volume.html#creating-a-duplicate-file-storage)</li><li>[Block duplicate snapshots](/docs/infrastructure/BlockStorage/how-to-create-duplicate-volume.html#creating-a-duplicate-block-volume)</li></ul></p></dd>
  <dt>Back up data to Object Storage</dt>
  <dd><p>You can use the [**ibm-backup-restore image**](/docs/services/RegistryImages/ibm-backup-restore/index.html#ibmbackup_restore_starter) to spin up a backup and restore pod in your cluster. This pod contains a script to run a one-time or periodic backup for any persistent volume claim (PVC) in your cluster. Data is stored in your {{site.data.keyword.objectstoragefull}} instance that you set up in a zone.</p>
  <p>To make your data even more highly available and protect your app from a zone failure, set up a second {{site.data.keyword.objectstoragefull}} instance and replicate data across zones. If you need to restore data from your {{site.data.keyword.objectstoragefull}} instance, use the restore script that is provided with the image.</p></dd>
<dt>Copy data to and from pods and containers</dt>
<dd><p>You can use the `kubectl cp` [command![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/reference/kubectl/overview/#cp) to copy files and directories to and from pods or specific containers in your cluster.</p>
<p>Before you begin, [target your Kubernetes CLI](cs_cli_install.html#cs_cli_configure) to the cluster that you want to use. If you do not specify a container with <code>-c</code>, the command uses to the first available container in the pod.</p>
<p>You can use the command in various ways:</p>
<ul>
<li>Copy data from your local machine to a pod in your cluster: <pre class="pre"><code>kubectl cp <var>&lt;local_filepath&gt;/&lt;filename&gt;</var> <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var></code></pre></li>
<li>Copy data from a pod in your cluster to your local machine: <pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;/&lt;filename&gt;</var> <var>&lt;local_filepath&gt;/&lt;filename&gt;</var></code></pre></li>
<li>Copy data from a pod in your cluster to a specific container in another pod another: <pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var> <var>&lt;namespace&gt;/&lt;other_pod&gt;:&lt;pod_filepath&gt;</var> -c <var>&lt;container></var></code></pre></li>
</ul></dd>
  </dl>
