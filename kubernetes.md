# Kubernetes

- [Generally Helpful Info](#generally-helpful-info)
  - [Using an Alias for `kubectl`](#using-an-alias-for--kubectl-)
  - [Get API Resource Short Names](#get-api-resource-short-names)
- [`config`](#-config-)
  - [Setting a Context and Namespace](#setting-a-context-and-namespace)
- [`describe`](#-describe-)
  - [Listing of a series of events](#listing-of-a-series-of-events)
- [`explain`](#-explain-)
- [`create`](#-create-)
  - [Get help on creatable resources](#get-help-on-creatable-resources)
- [`apply`](#-apply-)
- [`delete`](#-delete-)
  - [Force kill/delete](#force-kill-delete)
- [`get`](#-get-)
  - [Get YAML manifest of existing resource](#get-yaml-manifest-of-existing-resource)
- [ConfigMap](#configmap)
  - [Creating a ConfigMap](#creating-a-configmap)
    - [*Create a new config map named my-config based on folder bar*](#-create-a-new-config-map-named-my-config-based-on-folder-bar-)
    - [*Create a new config map named my-config with specified keys instead of file basenames on disk*](#-create-a-new-config-map-named-my-config-with-specified-keys-instead-of-file-basenames-on-disk-)
    - [*Create a new config map named my-config with key1=config1 and key2=config2*](#-create-a-new-config-map-named-my-config-with-key1-config1-and-key2-config2-)
    - [*Create a new config map named my-config from the key=value pairs in the file*](#-create-a-new-config-map-named-my-config-from-the-key-value-pairs-in-the-file-)
    - [*Create a new config map named my-config from an env file*](#-create-a-new-config-map-named-my-config-from-an-env-file-)
  - [Use ConfigMap in a Pod manifest via `envFrom.configMapRef`](#use-configmap-in-a-pod-manifest-via--envfromconfigmapref-)
  - [Redefine keys from a ConfigMap](#redefine-keys-from-a-configmap)
  - [Mounting a ConfigMap as a Volume](#mounting-a-configmap-as-a-volume)
- [Secret](#secret)
  - [Creating a generic Secret](#creating-a-generic-secret)
    - [*Literal Values*](#-literal-values-)
    - [*File containing environment variables*](#-file-containing-environment-variables-)
    - [*SSH key file*](#-ssh-key-file-)
  - [A Secret with Base64-encoded values](#a-secret-with-base64-encoded-values)
  - [Injecting key-value pairs of a Secret into a container](#injecting-key-value-pairs-of-a-secret-into-a-container)
  - [Mounting a Secret as a Volume](#mounting-a-secret-as-a-volume)
- [Security Context](#security-context)
  - [Setting a security context on the container level](#setting-a-security-context-on-the-container-level)
  - [Setting a security context on the Pod level](#setting-a-security-context-on-the-pod-level)
- [ResourceQuota](#resourcequota)
  - [Defining hard resource limits with ResourceQuota](#defining-hard-resource-limits-with-resourcequota)
  - [A Pod with resource requirements](#a-pod-with-resource-requirements)
- [Service Accounts](#service-accounts)
  - [Query for available Service Accounts](#query-for-available-service-accounts)
  - [Create a new Service Account](#create-a-new-service-account)

## Generally Helpful Info

### Using an Alias for `kubectl`

```sh
alias k=kubectl
k version
```

### Get API Resource Short Names

```sh
$ kubectl api-resources
NAME                    SHORTNAMES  APIGROUP  NAMESPACED  KIND
...
persistentvolumeclaims  pvc                   true        PersistentVolumeClaim
...
```

## `config`

### Setting a Context and Namespace

```sh
kubectl config set-context <context-of-question> \
  --namespace=<namespace-of-question>
```

## `describe`

### Listing of a series of events

```sh
$ kubectl describe pod/non-root
...
Events:
Type     Reason     Age              From               Message
----     ------     ----             ----               -------
Normal   Scheduled  <unknown>        default-scheduler  Successfully assigned \
                                                        default/non-root to minikube
Normal   Pulling    18s              kubelet, minikube  Pulling image "nginx:1.18.0"
Normal   Pulled     14s              kubelet, minikube  Successfully pulled image \
                                                        "nginx:1.18.0"
Warning  Failed     0s (x3 over 14s) kubelet, minikube  Error: container has \
                                                        runAsNonRoot and image \
                                                        will run as root
```

## `explain`

```sh
$ kubectl explain pods.spec
KIND:     Pod
VERSION:  v1

RESOURCE: spec <Object>

DESCRIPTION:
  ...

FIELDS:
  ...
```

## `create`

### Get help on creatable resources

```sh
$ kubectl create --help
Create a resource from a file or from stdin.

JSON and YAML formats are accepted.

Examples:
  ...

Available Commands:
  ...

Options:
  ...
```

## `apply`

## `delete`

### Force kill/delete

> Use the command line option --grace-period=0 and --force to send a SIGKILL signal. The signal will delete a Kubernetes object immediately

```sh
kubectl delete [resource] [name] --grace-period=0 --force
```

## `get`

### Get YAML manifest of existing resource

```sh
kubectl get [resource] [name] -o yaml > manifest.yaml
```

## ConfigMap

### Creating a ConfigMap

#### *Create a new config map named my-config based on folder bar*

```sh
kubectl create configmap my-config --from-file=path/to/bar
```

#### *Create a new config map named my-config with specified keys instead of file basenames on disk*

```sh
kubectl create configmap my-config --from-file=key1=/path/to/bar/file1.txt --from-file=key2=/path/to/bar/file2.txt
```

#### *Create a new config map named my-config with key1=config1 and key2=config2*

```sh
kubectl create configmap my-config --from-literal=key1=config1 --from-literal=key2=config2
```

#### *Create a new config map named my-config from the key=value pairs in the file*

```sh
kubectl create configmap my-config --from-file=path/to/bar
```

#### *Create a new config map named my-config from an env file*

```sh
kubectl create configmap my-config --from-env-file=path/to/foo.env --from-env-file=path/to/bar.env
```

### Use ConfigMap in a Pod manifest via `envFrom.configMapRef`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: configured-pod
spec:
  containers:
  - image: nginx:1.19.0
    name: app
    envFrom:
    - configMapRef:
        name: backend-config
```

### Redefine keys from a ConfigMap

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: configured-pod
spec:
  containers:
  - image: nginx:1.19.0
    name: app
    env:
    - name: DATABASE_URL
      valueFrom:
        configMapKeyRef:
          name: backend-config
          key: database_url
    - name: USERNAME
      valueFrom:
        configMapKeyRef:
          name: backend-config
          key: user
```

### Mounting a ConfigMap as a Volume

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: configured-pod
spec:
  containers:
  - image: nginx:1.19.0
    name: app
    volumeMounts:
    - name: config-volume
      mountPath: /etc/config
  volumes:
  - name: config-volume
    configMap:
      name: backend-config
```

```sh
$ kubectl exec -it configured-pod -- /bin/sh
# ls -1 /etc/config
database_url
user
# cat /etc/config/database_url
jdbc:postgresql://localhost/test
# cat /etc/config/user
fred
```

## Secret

### Creating a generic Secret

#### *Literal Values*

```sh
kubectl create secret generic db-creds --from-literal=pwd=s3cre!
```

#### *File containing environment variables*

```sh
kubectl create secret generic db-creds --from-env-file=secret.env
```

#### *SSH key file*

```sh
kubectl create secret generic ssh-key --from-file=id_rsa=~/.ssh/id_rsa
```

### A Secret with Base64-encoded values

```sh
$ echo -n 's3cre!' | base64
czNjcmUh
````

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-creds
type: Opaque
data:
  pwd: czNjcmUh
```

### Injecting key-value pairs of a Secret into a container

> It’s important to understand that the container will make the environment variable available in a Base64-decoded value. In turn, your application running in the container will not have to implement Base64-decoding logic

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: configured-pod
spec:
  containers:
  - image: nginx:1.19.0
    name: app
    envFrom:
    - secretRef:
        name: db-creds
```

### Mounting a Secret as a Volume

> Secrets mounted as Volume will expose its values in Base64-decoded form.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: configured-pod
spec:
  containers:
  - image: nginx:1.19.0
    name: app
    volumeMounts:
    - name: secret-volume
      mountPath: /var/app
      readOnly: true
  volumes:
  - name: secret-volume
    secret:
      secretName: ssh-key
```

## Security Context

A security context defines privilege and access control settings for a Pod or a container.

- The user ID that should be used to run the Pod and/or container.
- The group ID that should be used for filesystem access.
- Granting a running process inside the container some privileges of the root user but not all of them.

The security context is not a Kubernetes primitive. It is modeled as a set of attributes under the directive `securityContext` within the Pod specification. Security settings defined on the Pod level apply to all containers running in the Pod; **however, container-level settings take precidence.**

### Setting a security context on the container level

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: non-root
spec:
  containers:
  - image: bitnami/nginx:1.18.0
    name: secured-container
    securityContext:
      runAsNonRoot: true
```

> NOTE: can fail if the container image was setup to require root user access

```sh
$ kubectl describe pod/non-root
...
Events:
Type     Reason     Age              From               Message
----     ------     ----             ----               -------
Normal   Scheduled  <unknown>        default-scheduler  Successfully assigned \
                                                        default/non-root to minikube
Normal   Pulling    18s              kubelet, minikube  Pulling image "nginx:1.18.0"
Normal   Pulled     14s              kubelet, minikube  Successfully pulled image \
                                                        "nginx:1.18.0"
Warning  Failed     0s (x3 over 14s) kubelet, minikube  Error: container has \
                                                        runAsNonRoot and image \
                                                        will run as root
```

### Setting a security context on the Pod level

> Whenever a file is created on the filesystem, the owner of the file will be the arbitrary group ID 3500

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: fs-secured
spec:
  securityContext:
    fsGroup: 3500
  containers:
  - image: bitnami/nginx:1.18.0
    name: secured-container
    volumeMounts:
    - name: data-volume
      mountPath: /data/app
  volumes:
  - name: data-volume
    emptyDir: {}
```

## ResourceQuota

The Kubernetes primitive ResourceQuota establishes the usable, maximum amount of resources per namespace.

- Setting an upper limit for the number of objects that can be created for a specific type (e.g., a maximum of 3 Pods).
- Limiting the total sum of compute resources (e.g., 3 GiB of RAM).
- Expecting a Quality of Service (QoS) class for a Pod (e.g., `BestEffort` to indicate that the Pod must not make any memory or CPU limits or requests).

<https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/#resource-units-in-kubernetes>

### Defining hard resource limits with ResourceQuota

> Because we defined minimum and maximum resource requirements for objects in the namespace, we’ll have to ensure that the YAML manifest actually defines them.

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: awesome-quota
spec:
  hard:
    pods: 2
    requests.cpu: "1"
    requests.memory: 1024m
    limits.cpu: "4"
    limits.memory: 4096m
```

```sh
$ kubectl create -f awesome-quota.yaml --namespace=team-awesome
resourcequota/awesome-quota created
$ kubectl describe resourcequota awesome-quota --namespace=team-awesome
Name:            awesome-quota
Namespace:       team-awesome
Resource         Used  Hard
--------         ----  ----
limits.cpu       0     4
limits.memory    0     4096m
pods             0     2
requests.cpu     0     1
requests.memory  0     1024m
```

### A Pod with resource requirements

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - image: nginx:1.18.0
    name: nginx
    resources:
      requests:
        cpu: "0.5"
        memory: "512m"
      limits:
        cpu: "1"
        memory: "1024m"
```

## Service Accounts

Pods use a Service Account to authenticate with the API server through an authentication token. A Kubernetes administrator assigns rules to a Service Account via role-based access control (RBAC) to authorize access to specific resources and actions.

**If not assigned explicitly, a Pod uses the default Service Account. The default Service Account has the same permissions as an unauthenticated user. This means that the Pod cannot view or modify the cluster state nor list or modify any of its resources.**

<https://kubernetes.io/docs/reference/access-authn-authz/rbac/>

### Query for available Service Accounts

```sh
$ kubectl get serviceaccounts
NAME      SECRETS   AGE
default   1         25d
```

> Kubernetes models the authentication token with the Secret primitive. It’s easy to identify the corresponding Secret for a Service Account. Retrieve the YAML representation of the Service Account and look at the attribute secrets. In the Secret, you can find the Base64-encoded values of the current namespace, the cluster certificate, and the authentication token:

```sh
$ kubectl get serviceaccount default -o yaml | grep -A 1 secrets:
secrets:
- name: default-token-bf8rh
$ kubectl get secret default-token-bf8rh -o yaml
apiVersion: v1
data:
  ca.crt: LS0tLS1CRUdJTiB...0FURS0tLS0tCg==
  namespace: ZGVmYXVsdA==
  token: ZXlKaGJHY2lPaUp...ThzU0poeFMxR013
kind: Secret
...
```

### Create a new Service Account

```sh
kubectl create serviceaccount [name]
```

> There are two ways to assign the Service Account to a Pod. You can either edit the YAML manifest and add the serviceAccountName attribute as shown above, or you can use the --serviceaccount flag in conjunction with the run command when creating the Pod

```sh
$ kubectl run nginx --image=nginx --restart=Never --serviceaccount=custom
pod/nginx created
$ kubectl get pod nginx -o yaml
apiVersion: v1
kind: Pod
metadata:
  ...
spec:
  serviceAccountName: custom
...
```