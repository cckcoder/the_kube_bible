# Design Pattern
* The ambassador design pattern
* The sidecar design pattern
* The adapter design pattern

# Accessing a specific container inside a multi-container Pod

> Get container name in Pod
`kubectl get po <POD_NAME> -o jsonpath="{.items[*].spec.containers[*].name}"`

> Access into container name
`kubectl exec -it multi-container-pod -c nginx-ctr -- /bin/sh`

## initContainers

can offload some configuration to them by keeping your main containers images
in general, `initContainers` used to pull application code from Git repo

## Access the logs
`kubectl logs <POD_NAME>` >> this cause for only one container in Pod
`kubectl logs <POD_NAME> -c <CONTAINER_NAME>`
