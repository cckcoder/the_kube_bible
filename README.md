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

## Essential kubectl
* `kubectl config view`

* `kubectl config current-context`

* `kubectl get nodes`

* `kubectl get cs`
  * `kubectl get componentstatuses`
