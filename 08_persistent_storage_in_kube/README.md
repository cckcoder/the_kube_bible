# Persistent Storage in Kubernetes

## How to listing Persistent Storage

`kubectl get persistentvolume`

or

`kubectl get pv`

## Introducing PersistenVolume types
* AWS EBS volumes
* AWS EFS filesystem
* GCP Persistent Disk (PD)
* Azure disks

## Introducing access modes
> PersistentVolumes 

## PersistenVolumeClaim

`kubectl get persistentvolumeclaims`

or

`kubectl get pvc`

> Pod cannot mount a PersistentVolume directly

PersistentVolume objects is that they are not namespace resources, but PersistentVolumeClaims objects are.

### PVC status
* Available: object is created and ready to be use
* Bound: object current mounted to one or several Pods.
* Terminating: to be destroyed and wiped out

## Dynamic Storage

### Introducing StorageClasses
> Listing StorageClasses
`kubectl get storageclass`

or

`kubectl get sc`

### Lab1

* creating namespace `kubectl create ns dynamic-storage`
* create PVC object [demo](./pvc-dynamic.yaml)

