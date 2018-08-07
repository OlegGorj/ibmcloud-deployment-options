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


# Removing persistent storage from a cluster
{: #cleanup}

When you set up persistent storage in your cluster, you have three main components: the Kubernetes persistent volume claim (PVC) that requests storage, the Kubernetes persistent volume (PV) that is mounted to a pod and described in the PVC, and the IBM Cloud infrastructure (SoftLayer) instance, such as NFS file or block storage. Depending on how you created these, you might need to delete all three separately.
{:shortdesc}

## Cleaning up persistent storage
{: #storage_remove}

Understanding your delete options:

**I deleted my cluster. Do I have to delete anything else to remove persistent storage?**</br>
It depends. When you delete a cluster, the PVC and PV are deleted. However, you choose whether to remove the associated storage instance in IBM Cloud infrastructure (SoftLayer). If you chose not to remove it, then the storage instance still exists. Also, if you deleted your cluster in an unhealthy state, the storage might still exist even if you chose to remove it. Follow the instructions, particularly the step to [delete your storage instance](#sl_delete_storage) in IBM Cloud infrastructure (SoftLayer).

**Can I delete the PVC to remove all my storage?**</br>
Sometimes. If you [create the persistent storage dynamically](cs_storage_basics.html#dynamic_provisioning) and select a storage class without `retain` in its name, then when you delete the PVC, the PV and the IBM Cloud infrastructure (SoftLayer) storage instance are also deleted.

In all other cases, follow the instructions to check the status of your PVC, PV, and the physical storage device and delete them separately if necessary. 

**Am I still charged for storage after I delete it?**</br>
It depends on what you delete and the billing type. If you delete the PVC and PV, but not the instance in your IBM Cloud infrastructure (SoftLayer) account, that instance still exists and you are charged for it. You must delete everything to avoid charges. Further, when you specify the `billingType` in the PVC, you can choose `hourly` or `monthly`. If you chose `monthly`, your instance is billed monthly. When you delete the instance, you are charged for the remainder of the month.


**Important**:
* When you clean up persistent storage, you delete all the data that is stored in it. If you need a copy of the data, make a backup for [file storage](cs_storage_file.html#backup_restore) or [block storage](cs_storage_block.html#backup_restore).
* If you use an {{site.data.keyword.Bluemix_dedicated}} account, you must request volume deletion by [opening a support ticket](/docs/get-support/howtogetsupport.html#getting-customer-support).

Before you begin, [target your CLI](cs_cli_install.html#cs_cli_configure).

To clean up persistent data:

1.  List the PVCs in your cluster and note the **NAME** of the PVC, the **STORAGECLASS**, and the name of the PV that is bound to the PVC and shown as **VOLUME**.
    ```
    kubectl get pvc
    ```
    {: pre}

    Example output:
    ```
    NAME                  STATUS    VOLUME                                     CAPACITY   ACCESSMODES   STORAGECLASS            AGE
    claim1-block-bronze   Bound     pvc-06886b77-102b-11e8-968a-f6612bb731fb   20Gi       RWO           ibmc-block-bronze       78d
    claim-file-bronze     Bound     pvc-457a2b96-fafc-11e7-8ff9-b6c8f770356c   4Gi        RWX           ibmc-file-bronze-retain 105d
    claim-file-silve      Bound     pvc-1efef0ba-0c48-11e8-968a-f6612bb731fb   24Gi       RWX           ibmc-file-silver        83d
    ```
    {: screen}
    
2. Review the **ReclaimPolicy** and **billingType** for the storage class. 
   ```
   kubectl describe storageclass <storageclass_name>
   ```
   {: pre}
   
   If the reclaim policy says `Delete`, your PV and the physical storage are removed when you remove the PVC. If the reclaim policy says `Retain`, or if you provisioned your storage without a storage class, then your PV and physical storage are not removed when you remove the PVC. You must remove the PVC, PV, and the physical storage separately. 
   
   **Important:** If your storage is charged on a monthly basis, you still get charged for the entire month, even if you remove the storage before the end of the billing cycle. 
   
3. Remove any pods that mount the PVC. 
   1. List the pods that mount the PVC. 
      ```
      kubectl get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.volumes[*]}{.persistentVolumeClaim.claimName}{" "}{end}{end}' | grep "<pvc_name>"
      ```
      {: pre}
    
      Example output:
      ```
      blockdepl-12345-prz7b:	claim1-block-bronze  
      ```
      {: screen}
    
      If no pod is returned in your CLI output, you do not have a pod that uses the PVC. 
    
   2. Remove the pod that uses the PVC. If the pod is part of a deployment, remove the deployment. 
      ```
      kubectl delete pod <pod_name>
      ```
      {: pre}
      
   3. Verify that the pod is removed. 
      ```
      kubectl get pods
      ```
      {: pre}
   
4. Remove the PVC. 
   ```
   kubectl delete pvc <pvc_name>
   ```
   {: pre}
   
5. Review the status of your PV. Use the name of the PV that you retrieved ealier as **VOLUME**. 
   ```
   kubectl get pv <pv_name>
   ```
   {: pre}
   
   When you remove the PVC, the PV that is bound to the PVC is released. Depending on how you provisioned your storage, your PV goes into a `Deleting` state if the PV is deleted automatically, or into a `Released` state, if you must manually delete the PV. 
   
   **Note**: For PVs that are automatically deleted, the status might briefly say `Released` before it is deleted. Rerun the command after a few minutes to see if the PV is removed.
   
6. If your PV is not deleted, manually remove the PV. 
   ```
   kubectl delete pv <pv_name>
   ```
   {: pre}
   
7. Verify that the PV is removed. 
   ```
   kubectl get pv
   ```
   {: pre}
   
8. {: #sl_delete_storage}List the physical storage instance that your PV pointed to and note the **id** of the physical storage instance. 

   **File storage:** 
   ```
   ibmcloud sl file volume-list --columns id  --columns notes | grep <pv_name>
   ```
   {: pre}
   **Block storage:**
   ```
   ibmcloud sl block volume-list --columns id --columns notes | grep <pv_name>
   ```
   {: pre}
     
   If you removed your cluster and cannot retrieve the name of the PV, replace `grep <pv_name>` with `grep <cluster_id>` to list all storage devices that are associated with your cluster. 
   {: tip}
     
   Example output: 
   ```
   id         notes   
   12345678   ibmcloud-block-storage-plugin-7566ccb8d-44nff:us-south:aa1a11a1a11b2b2bb22b22222c3c3333:Performance:mypvc:pvc-457a2b96-fafc-11e7-8ff9-b6c8f770356z 
   ```
   {: screen}
     
   Understanding the **Notes** field information:
   *  **`:`**: A colon (`:`) separates information.
   *  **` ibmcloud-block-storage-plugin-7566ccb8d-44nff`**: The storage plug-in that the cluster uses.
   *  **`us-south`**: The region that your cluster is in.
   *  **`aa1a11a1a11b2b2bb22b22222c3c3333`**: The cluster ID that is associated with the storage instance.
   *  **`Performance`**: The type of file or block storage, either `Endurance` or `Performance`.
   *  **`mypvc`**: The name of the PVC that is associated with the storage instance.
   *  **`pvc-457a2b96-fafc-11e7-8ff9-b6c8f770356z`**: The PV that is associated with the storage instance.
     
9. Remove the physical storage instance. 
   
   **File storage:**
   ```
   ibmcloud sl file volume-cancel <filestorage_id>
   ```
   {: pre}
   
   **Block storage:**
   ```
   ibmcloud sl block volume-cancel <blockstorage_id>
   ```
   {: pre}
     
9. Verify that the physical storage instance is removed. This process might take a few minutes. 

   **File storage:** 
   ```
   ibmcloud sl file volume-list 
   ```
   {: pre}
   **Block storage:**
   ```
   ibmcloud sl block volume-list 
   ```
   {: pre}
 
