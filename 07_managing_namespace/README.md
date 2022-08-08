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

