# Kubernetes Persistent Storage

## Topics: Volumes, PersistentVolumes, PersistentVolumeClaims, StorageClasses, StatefulSets
 
 Scenario: Your organization is running a stateful application, such as a database, inside Kubernetes. Configure persistent storage so that application data remains available when a Pod is deleted and recreated.
 
## Tasks
1. Create a PersistentVolume (PV), using a storage type supported by your cluster.
2. Create a PersistentVolumeClaim (PVC) and bind it to the PV.
3. Deploy an application that mounts the PVC.
4. Write a test file or data into the mounted volume.
5. Delete the application Pod and allow its controller to recreate it.
6. Verify that the previously written data is still available.
7. Create or use a StorageClass and demonstrate dynamic volume provisioning, if supported by the cluster.
8. Deploy a StatefulSet using volumeClaimTemplates.

## Live Demonstration
- Show the PV and PVC status and explain how they are bound.
- Show the volume mounted inside the container.
- Write data to the mounted volume.
- Delete and recreate the Pod, then verify that the data remains.
- Show the PVCs created by the StatefulSet and explain their lifecycle.

## Questions to Explain
- What is the difference between a PV and a PVC?
- What is a StorageClass?
- What is the difference between static and dynamic provisioning?
- What happens to a PVC when a Pod is deleted?
- What do ReadWriteOnce, ReadOnlyMany, and ReadWriteMany mean?
- What is the difference between a Pod volume and persistent storage?