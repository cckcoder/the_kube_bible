# Deployment

> Provides

* Easy scalability
* Rolling updates
* Versioned rollback

Deployment object build on top **ReplicaSets**

## Introducing

> Types of workloads:
  
* Stateless: they do not store any application state inside the container or attached volume.

  > Stateless workloads are easy to scale up and down

* Stateful: store any modifiable data inside themselves, such as a Pod is a MySQL or MongoDB
that reads and write the data to a PV

  > Stateful workloads are much harder to manage
  > try to keep stateful workloads outside your cluster

* Batch: Performs job or task processing, either schedule or on demand.
* Node-local

## Creating a Deployment object

[deployment-demo](./nginx-deployment.yaml)

> Deployment has four main components:

* replicas
* selector
* template
* strategy

> There are two important actions can perform on existing Deployment

* Modify template
* Modify replicas number

Let's apply deployment

`kubectl apply -f <DEPLOYMENT_MANIFEST> --record`

`kubectl apply -f ./nginx-deployment.yaml`

> Listing deployment

`kubectl get deployment`

`kubectl get deploy`

## Exposing Deployment Pods using Service object

[lb-svc](./nginx-service.yaml)

configuration **selector** match deployment selector

create deployment with imperative

`kubectl expose deployment --type=LoadBalancer <SVC_NAME>`

## Deleting a Deployment object

> you can do two things:

* Delete the Deployment object, along with Pods that it owns.

  `kubectl delete deploy <DEPLOYMENT_NAME>`

* Delete the Deployment object and leave the other Pods unaffected.

  `kubectl delete deploy <DEPLOYMENT_NAME> --cascade=orphan`

## Deployment object manage revisions and version rollout

> kube support two strategies out of the box

* RollingUpdate: default strategy
* Recreate: terminate and delete any existing ReplicaSet and replace it with a new one. **Should not use this strategy in production**

> Check rollout status

`kubectl rollout status deployment <DEPLOYMENT_NAME>`

roll back in imperative way:

`kubectl set image deployment <DEPLOYMENT_NAME> <CONTAINER_NAME>=<CONTAINER_IMAGE>`

`kubectl set image deployment nginx-deployment-demo nginx=nginx:1.18`
