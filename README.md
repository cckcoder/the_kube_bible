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

