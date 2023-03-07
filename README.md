# K8s Volumes Lab
In this Lab, we will investigate the use of K8s Volume object in order to store pods Data.
We work with volumes because the container File system is ephemeral: Files in container File System exists only as long as the container exists. In fact, Data in container File System is lost as soon as pods are deleted or recreated.
In real life, many Applications need a persistent data. That is why, volumes in K8s allow to store Data outside the pod while allow container in a pod to access data at runtime.
## Persistent Volumes
Persistent Volume is a bit more advanced than Volumes. It is a k8s Object that allows user to treat Storage as an Abstract Resource and consume it using Pods. 
PV is resource in the cluster just like a node is a cluster resource. It uses a set of Attribute to describe the underlying storage resources (Disk or Cloud Storage), which will be used to store data.
```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-persistnt-vol
spec:
  storageClassName: local-storage
  persistentVolumeReclaimPolicy: Recycle
  capacity:
    storage: 10Mi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /var/tmp
```
The **persistentVolumeReclaimPolicy** attribute defines how storage will be reused. The main reclaim policies are:
- Retain: Keep all the data. This require manual data cleanup and prepare for reuse.
- Delete: Delete underlying storage resources automatically (Support for Cloud Resource Only).
- Recycle: Automatically delete all data in underlying storage. Allow PVs to be reuse.
## PersistentVolumeClaim (PVC)
PersistentVolumeClaim (PVC) is a request for storage by a user. PVCs deﬁne a set of attribute Similar to those of PVs. 
The main idea is that PVCs look for a PVs that is able to meet the criteria. If it found one, it will automatically be bound to that PV.
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  storageClassName: local-storage
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 4Mi
```
## Storage Class
StorageClass allows K8s Administrator to Specify all type of Storage Service they offer on their Platform. 
```
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: local-storage
provisioner: kubernetes.io/no-provisioner
volumeBindingMode: WaitForFirstConsumer
allowVolumeExpansion: true
```
The field **allowVolumeExpansion** is the property of StorageClass and deﬁnes the ability to resize storage size after they are created. All Cloud Disk Supports this property.
## Volume Types
#### emptyDir
The emptyDir type is created when Pod is assigned to Node and Persist as long as Pod running on the Node. Multiple containers can refer the same emptyDir Volume. In fact, they can read and write the same ﬁles in the emptyDir volume, though that volume can be mounted at the same or different paths in each container.
#### hostPath
hostPath volume mounts a file or directory from the host node's filesystem into your Pod. This is not something that most Pods will need, but it offers a powerful escape hatch for some applications.
## Instructions
1. Clone the project 
```
git clone https://github.com/mehdijebali/K8s_Ingress_Lab.git
```
2. Apply manifest using **kubectl**
```
kubectl apply -f /path/to/manifest.yml
```
3. You can check the status of pods, services, deployments, and ingresses  with the following commands
```
kubecl get pods | grep <pod_name>
kubectl get sc | grep <storage_class_name>
kubectl get pv | grep <persistent_volume_name>
kubectl get pvc | grep <persistent_volume_claim_name>
```
4. You can also list additional information of specifice pod,service, deployment, and ingress for any debugging issue
```
kubectl describe pod <pod_name>
kubectl describe pv <persistent_volume_name>
kubectl describe pvc <persistent_volume_claim_name>
kubectl describe sc <storage_class_name>
```
The **<pod_name>, <dstorage_class_name>, <persistent_volume_name>), <persistent_volume_claim_name>** are the values of the key `metadata.name` in each k8s manifest yaml file.