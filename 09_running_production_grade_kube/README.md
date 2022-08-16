# Topic follow on this chapter

* Ensuring HA adn fault tolerance (FT) on kube
* What is ReplicationController?
* What's ReplicaSet and how does it differ from ReplicationController

## HA techniques:

* Eliminating single points of failure (SPOF) in the system
* Falover setup
* Load balancing

## Falut Tolerance

This means that a small fault in the system will have a small impact on the overall performance of the system while serving requests from the end user.

## ReplicationController

> Imperative commands for creating ReplicationController object not available.

[demo](./replica-controller.yaml)

* listing replica controller
`kubectl get replicationcontroller`

or 

`kubectl get rc`

* get more details using `describe`
`kubectl describe rc <RC_NAME>`


Modify an existing ReplicationController and change `replicas` property in the specification to a
different number. This process is called **scaling**  if you increase the number of replicas,
you are **scaling up** and if you decrease the number of replicas you are **scaling down.  **

> Delete ReplicaSet

it can do two way
* delete ReplicationController with pods.  
`kubectl delete rc <REPLICATION_CONTROLLER_NAME>`
* delete only ReplicationController let pods running.  
`kubectl delete rc <REPLICATION_CONTROLLER_NAME> --cascade=orphan`

## ReplicaSet

### What're differnces between ReplicaSet and ReplicationController

* ReplicaSet: allows more advance, set-base(inclusion, exclusion) label selector.
* ReplicaSet: powerful building block of other Kube object such as **Deployment** and **HPA**
* In general, in the future ReplicationController will bee **deprecated**

> Demo

[ReplicaSet demo](./replicaset.yaml)

> Three main specification

* replicas: defines the number of Pod using the given `template` and matching label `selector` Pods
* selector: A label selector that defines how to identify Pods the the ReplicaSet object owns
* template: Defines a template for Pod creation.

> Listing ReplicaSet

`kubectl get replicaset`  
`kubectl get rs`  

> Observe the status of new ReplicaSet

`kubectl describe rs <REPLICASET_NAME>`

### Scaling up ReplicaSet

* Increase `replicas` in yaml file
* with imperative: `kubectl scale rs <REPLICASET_NAME> --replicas=5`

### Delete ReplicaSet

> There are two possibilities:

* Delete the ReplicaSet together with the Pods that it owns

  `kubectl delete rs <REPLICASET_NAME>`

* Delete the ReplicaSet object and leave Pods unaffected
  
  `kubectl delete rs <REPLICASET_NAME> --cascade=orphan`
