<!--toc:start-->
- [Design Pattern](#design-pattern)
- [Accessing a specific container inside a multi-container Pod](#accessing-a-specific-container-inside-a-multi-container-pod)
 - [initContainers](#initcontainers)
 - [Access the logs](#access-the-logs)
 - [Make some fun](#make-some-fun)
- [The ambassador design pattern](#the-ambassador-design-pattern)
<!--toc:end-->
# Design Pattern
* The ambassador design pattern
* The sidecar design pattern
* The adapter design pattern

## Accessing a specific container inside a multi-container Pod

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

## Make some fun
> It's will create text file name hello-world.txt and contain hello world message.

`kubectl exec two-ctr-with-empty-dir -c nginx-ctr -- /bin/sh -c "echo 'hello world' >> /var/i-am-empty-dir-vol/hello-world.txt"`

> Check with other container in same Pod to access volume

`kubectl exec two-ctr-with-empty-dir -c fapi-ctr -- /bin/sh -c "cat /var/i-am-empty-dir-vol/hello-world.txt"`

## The ambassador design pattern

* The first container will be called the main container.
* The other container will be called the ambassador container.

## The sidecar design pattern

* You must locate the directory where your main containers write their logs.
* You must create a volume to make this directory accessible to the log forwarder sidecar.
* You must launch the sidecar container with the proper configuration.
