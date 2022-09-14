# StatefulSet

## Managing state in containers

> Notice is that after restarting the container, you would lose the data stored in the DB
> each time it is restarted. Fortunately, containers come with the option to mount data **volumes**

## Managing state in Kube Pods

* In kube concept of container volume is extended by
  * PersistentVolumes (PV)
  * PersistentVolumeClaims(PVC)
  * StorageClass(SC)
