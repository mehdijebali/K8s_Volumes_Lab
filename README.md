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
## PersistentVolumeClaim
## Storage Class
## Volume Types