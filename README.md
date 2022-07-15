# The Kubernetes bible

## Install Minikube
* [Minikube](https://minikube.sigs.k8s.io/docs/start/)

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64

chmod +x minikube

sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

> Make sure minikube install complete with
> `minikube status`

## Minikube (Single node cluster)

* `minikube start`

* `minikube stop`

* `minikube status`

* `minikube delete`

## Kind (Multiple node cluster)
* [kind](https://kind.sigs.k8s.io/)

```bash
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.14.0/kind-linux-amd64

chmod +x ./kind

mv ./kind /usr/local/bin/kind
```

### Cli for kind
* `kind create cluster --config ~/.kube/kind_cluster.yaml`

* `kind stop`

* `kind delete cluster`

## Essential kubectl
* `kubectl config view`

* `kubectl config current-context`

* `kubectl get nodes`

* `kubectl get cs`
  * `kubectl get componentstatuses`

* `kubectl get configmaps`

* `kubectl describe pods <pod name>`
> the describe cmd intended to retrieve a complete of information for one specific object

## How should design Pods
> Respect two simple design rules when building Pods

* A Pod should contain everything required to launch a microservice

* A Pod should be stateless (When possible)

### Create a Pod with imperative syntax
> We need two parameters to create a Pod:
  * The Pod's name
  * The Docker image(s)

> Example:
`kubectl run nginx-pod --image nginx:latest`

### Create a Pod with declarative syntax
> Exmaple:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod # Pod's name
spec:
  containers:
    - name: nginx-container
      image: nginx-latest
```

* `kubectl create -f nginx-pod.yaml`

or

* `kubectl apply -f nginx-pod.yaml`

==Remember Pod's name is unique==

### Listing the object in JSON or YAML
> The `-o` option is one of the most useful option
> You can retrieve this information in JSON or YAML

```bash
kubectl get pods -o yaml

kubectl get pods -o json

kubectl get pods <POD_NAME> -o yaml
```

#### Backing up resource using the list operation

`kubectl get pods <POD_NAME> -o yaml > nginx-pod.yaml`

> In case created pod using the imperative way
this way, you have a YAML backup of nginx-pod

#### Getting more information fron list operation

> you'll get name of worker node where pod is running

`kubectl get pods -o wide`

### Accessing a pod from the outside world
>`kubectl port-forward`

`kubectl port-forward pod/nginx-pod 8081:80`

this cmd will tell kubectl forward port 8081 port to the 80 one of the Pod

### Entering a container inside a Pod

> Same like a docker exec

`kubectl exec nginx-pod -it /bin/bash`


### Deleting a Pod

> `kubectl delete Pods <POD_NAME>`

`kubectl delete Pods nginx-pod`

> If you have built Pod with declarative and still have YAML configuration file,
you can delete your Pod with out know the name of the container bacause it's contain in YAML file
run the follow command to delete Pod

`kubectl delete -f nginx-pod.yaml`

## Labels

### Create labels

> Create labels (imperative)

* `kubectl run nginx-pod1 --image nginx:latest --labels='environment=prod' --labels='tier=frontend'`

> Create labels (declarative)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod-1 # Pod's name
  labels: # Create labels
    tier: frontend
    environment: prod
spec:
  containers:
    - name: nginx-container
      image: nginx:stable-alpine
```

> Listing labels attached to a Pod

* `kubectl get pods --show-labels`

* `k get po --show-labels`

> Get Pod with labels

* `kubectl get pods -l "tier=frontend"`

* `kubectl get pods --show-labels -o wide`

* `kubectl get pods nginx-pod --show-labels -o wide`

### Add, Update and Delete labels

> Adding or updating a label to/of a running Pod

`kubectl label pods <POD_NAME> <LABEL_NAME>`

`kubectl label pods nginx-pod stack=blue`

> update existing label

`kubectl label pods nginx-pod stack=green --overwrite `

⚠️ IMPORTANT NOTE

> it's better to add labels when a Pod is created and keep your Kube configuration immutable

> Deleting label

`kubectl label pods nginx-pod stack-`

we use (-) symbol after label key

## What are jobs?

> A job is another kind of resource that's exposed by the Kube API
in the end job will create one or multiple Pods to execute a command defined by you.

* Exmaples of typical use case
  * Taking a backup of a database
  * Sending an email
  * Consuming some messages in a queue

These are task you do not want run forever.

The restartPolicy parameter can take two options:
* Never
* OnFailure

### How to make sure job work well?

> we can read its log!

`kubectl logs <POD_NAME>`
