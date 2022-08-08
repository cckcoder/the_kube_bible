# Namespace

> They help us keep our cluster well organized by grouping our resource

Kube namespace will help to achieve the following as an adminstrator:
* Cluster partitioning to ease resource organization.
* Scoping resource names.
* Hardware sharing and consumption limitation.
* Permission access.

> Recommend create one namespace epr microservice and deploy all the resource that belong to a microservice within its namespace.

## Decide to use namespace in these ways:
* Differentiate environments
  * For example, one namespace for a production environment and another one for a development environment.

* Differentiate the tires
  * One namespace for DB, one for application Pods, and another for middleware deployment.

* Just using the default namespace
  * For the smallest cluster that only deploy a few resources, you can go for the simplest setup and just use one big default and deploy everything into it.

* List all namespace in cluster:
  * `kubectl get namespace`
  * `kubectl get ns` for short-hand version.

> Retriving the data of a specific namespace
* `kubectl get ns <NAMESPACE>`
* `kubectl get ns default`

### Create Namespace imperative
`kubectl create ns <Namespace>`

`kubectl create ns custom-ns`

### Create Namespace declarative

[ns custom](./custom-ns-2.yaml)

## Creating a resource inside a namespace with -n option

Let's try create:

### Pods

#### Imperative

* `kubectl create ns custom-ns`
* `kubectl run nginx --image nginx:latest -n custom-ns`

> `-n` is short-hand for `--namespace`

#### Declarative
[demo nginx-2](./pod-in-namespace.yaml)

### ConfigMap
* `kubectl create configmap my-configmap --from-literal=Name=Codewizz -n custom-ns`


### Listing resources inside a specific namespace
`kubectl get pods -n <NAMESPACE>`

`kubectl get pods -n custom-ns`

## Listing all the resources inside a specific namespace

`kubectl get all -n <NAMESPACE>`

### Switching between namespace with kubectl
Let's create a few more namespace
`kubectl create ns another-ns`

switch to `another-ns` namespace
`kubectl config set-context $(kubectl config current-context) --namespace=another-ns`

create nginx-pod in this namespace
`kubectl run nginx-pod --image nginx:latest`

then switch back to default namespace
`kubectl config set-context $(kubectl config current-context) --namespace=default`

### How to delete po under namespace?
`kubectl delete po <POD_NAME> -n <NAMESPACE>`

## Configuring ResourceQuota and Limit at the namespace level

* [demo_1](./pod-in-ns-request.yaml)
* [demo_2](./pod-in-ns-request-and-limit.yaml)

> If you are aware of this request and limit consideration, don't forget to add it to your Pods!

> The Pods without a limit can eat all the resources on the worker node is it launched on.

### ResourceQuota
In general, ResourceQuota is used to do the following:
* Limit CPU consumption within a namespace
* Limit memory consumption within a namespace
* Limit the absolute number of Pods operating within a namespace

[demo_1](./resource-quota.yaml)

Demo: specify that the namespace where it is created cannot hold more than 10 ConfigMap 
and 5 service.

[demo_2](./resource-quota-with-obj-count.yaml)

> Don't forget to add the `--namespace` where ResourceQuota will ber applied:
`kubectl create -f resource-quota-with-obj-count.yaml --namespace=custom-ns`

### Listing ResourceQuota

`kubectl get quota -n custom-ns`

### Deleting ResourceQuota

`kubectl delete -f resource-quota-with-obj-count.yaml -n custom-ns`

## LimitRange
