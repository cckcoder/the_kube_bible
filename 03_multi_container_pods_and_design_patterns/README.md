# Design Pattern
* The ambassador design pattern
* The sidecar design pattern
* The adapter design pattern

# Accessing a specific container inside a multi-container Pod

> Get container name in Pod
`kubectl get po <POD_NAME> -o jsonpath="{.items[*].spec.containers[*].name}"`

> Access into container name
`kubectl exec -it multi-container-pod -c nginx-ctr -- /bin/sh`
