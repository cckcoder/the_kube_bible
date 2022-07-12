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
