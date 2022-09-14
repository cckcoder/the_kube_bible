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

### Roll out

Let's try

* First, let's imperativly roll out another version of our Deployment

  `kubectl set image deployment nginx-deployment-demo nginx=nginx:1.19`

* Using `kubectl rollout status`, wait for end of the deployment:

  `kubectl rollout status deployment nginx-deployment-demo`

* check version of application `kubectl describe deploy`

* Use the following `kubectl rollout history` to see all the revision are available

  `kubectl rollout history deploy <DEPLOYMENT_NAME>`

* Follow the command to revision

  `kubectl rollout history deploy <DEPLOYMENT_NAME> --revision=2`

* Now let's perform a rollback to this revision

  `kubectl rollout undo deploy <DEPLOYMENT_NAME> rolled back`

  or 

  `kubectl rollout undo deploy <DEPLOYMENT_NAME> --to-revision=2`

## Best practices

* Use declarative object management for Deployments

  > To delete objects, it is still better to use **imperative command**
  > it is more predictable and less prone to errors

* Do not use the **Recreate Strategy** for production workloads

  > This will mean downtime for your end users.
  > Because existing Pods old revision of the Deployment will be terminated
  > and replaced with the new Pods.
