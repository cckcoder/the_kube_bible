# ConfigMap and Secret

> Behave like repositories for key/value pairs.

* ConfigMap: store unsecured configuration values.
* Secret: use for more sensitive configuration data such as hashes or database password.
* configuration file has other format your application can use.

## Using ConfigMap
* Listing ConfigMap
  * `kubectl get configmaps`
  * `kubectl get cm`

* Creating ConfigMap
  * `kubectl create configmap <CONFIGMAP_NAME>`
  * `kubectl create cm <CONFIGMAP_NAME>`

* Creating ConfigMap with value
  * `kubectl create configmap <CONFIGMAP_NAME> --from-literal=<KEY>=<VALUE>`
  * `kubectl create configmap third-configmap --from-literal=color=blue`
  * ```
    kubectl create configmap forth-configmap --from-literal=color=blue \
    --from-literal=version=1 --from-literal=environment=prod
    ```

* Creating ConfigMap with file
  * `kubectl create cm <CONFIGMAP_NAME> --from-file=<PATH_YOUR_FILE>`
  * `kubectl create cm sixth-configmap --from-file=$HOME/configfile.txt`

* Creating ConfigMap from an env file
  * `kubectl create cm <CONFIGMAP_NAME> --from-env-file=<PATH_YOUR_FILE>`
  * `kubectl create cm eight-configmap --from-env-file=/opt/project/the_kube_bible/04_config_maps_and_secrets/my-env-file.txt`

### How to read values inside a ConfigMap
`kubectl describe cm <CONFIGMAP_NAME>`

### Linking ConfigMap as env variable

> As follow by this example, it'll take only env name color
[nginx-with-configmap](./nginx-pod-with-configmap.yaml)

`kubectl exec nginx-pod-with-configmap -- env`
then it'll list all env inside container
you should see "COLOR=blue"

> This demo show how to inject all env variable into container
[nginx-with-all-configmap](./nginx-pod-with-all-configmap.yaml)

### Mounting a ConfigMap as a volume mount
> Demo
[nginx-with-volume](./nginx-pod-with-config-volumn.yaml)

`kubectl exec nginx-pod-with-configmap-volume -- ls /etc/conf`

### Delete a ConfigMap
* `kubectl delete cm <CONFIGMAP_NAME>`
  * `kubectl delete cm my-configmap`

### Updating a ConfigMap

> There's no real way to update a ConfigMap once it's been created.
To update a ConfigMap, you'll have to delete it


## Secret

### Creating Secret imperatively

`--from-literal`

* `kubectl create secret <SECRET_NAME> --from-literal=<KEY>=<VALUE>`
  * `kubectl create secret generic my-first-secret --from-literal='db_pass=my-db-password'`


### Creating Secret declaratively

> When create a Secret from a YAML manifest file, you will have to handle encode your string.

Example:

`echo 'my-db-password' | base64` output will be: `bXktZGItcGFzc3dvcmQK`

declarative example: [my-secret](./secrets/secret-from-file.yaml)

let's create the secret `kubectl create -f secret-from-file.yaml`

> Create secret with file
`kubectl create secret generic <SECRET_NAME> --from-file=<FILE_NAME>`

#### Reading a Secret
`kubectl describe secret <SECRET_NAME>`

### Consuming a Secret as a volume mount
> You can only do with declaratively

* [Demo1](./nginx-pod-with-config-volumn.yaml)

### Deleting Secret
`kubectl delete secret <SECRET_NAME>`
`kubectl delete secret mysql-pass`

### Update Secret

> To update a secret, you will need to delte it and then recreate it with new values.
