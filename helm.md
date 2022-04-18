# Helm

## Table of Contents

## `repo`

| Option | Description |
| :-: | :- |
|`add`|add a chart repository|
|`index`|generate an index file given a directory containing packaged charts|
|`list`|list chart repositories|
|`remove`|remove one or more chart repositories|
|`update`|update information of available charts locally from chart repositories|

```sh
$ helm repo add bitnami https://charts.bitnami.com/bitnami
"bitnami" has been added to your repositories
```

### Update repos

```sh
$ helm repo update
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "bitnami" chart repository
Update Complete. ⎈ Happy Helming!⎈
```

## `search`

| Option | Description |
| :-: | :- |
|`hub`|search for charts in the Artifact Hub or your own hub instance|
|`repo`|search repositories for a keyword in charts|

> Helm will search not just the package names, but also other fields like labels and descriptions. Thus, we could search for content and see Drupal listed there because it is a content management system:

```sh
$ helm search repo drupal
NAME            CHART VERSION   APP VERSION     DESCRIPTION
bitnami/drupal  7.0.0           9.0.0           One of the most versatile open...

$ helm search repo drupal --versions
NAME            CHART VERSION   APP VERSION     DESCRIPTION
bitnami/drupal  7.0.0           9.0.0           One of the most versatile op...
bitnami/drupal  6.2.22          8.9.0           One of the most versatile op...
bitnami/drupal  6.2.21          8.8.6           One of the most versatile op...
bitnami/drupal  6.2.20          8.8.5           One of the most versatile op...
bitnami/drupal  6.2.19          8.8.5           One of the most versatile op...
...
```

> A chart version is the version of the Helm chart. The app version is the version of the application packaged in the chart. Helm uses the chart version to make versioning decisions, such as which package is newest.

## `get`

| Option | Description |
| :-: | :- |
|`all`|download all information for a named release|
|`hooks`|download all hooks for a named release|
|`manifest`|download the manifest for a named release|
|`notes`|download the notes for a named release|
|`values`|download the values file for a named release|

```sh
helm get values [release] > values.yaml
```

## `install`

```sh
$ helm install mysite bitnami/drupal
NAME: mysite
LAST DEPLOYED: Sun Jun 14 14:46:51 2020
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
*******************************************************************
*** PLEASE BE PATIENT: Drupal may take a few minutes to install ***
*******************************************************************

1. Get the Drupal URL:

  You should be able to access your new Drupal installation through

  http://drupal.local/

2. Login with the following credentials

  echo Username: user
  echo Password: $(kubectl get secret --namespace default mysite-drupal...

```

> Instance names are scoped to Kubernetes namespaces. We could install two instances named mysite as long as they each lived in a different namespace.

```sh
kubectl create ns first
kubectl create ns second
helm install --namespace first mysite bitnami/drupal
helm install --namespace second mysite bitnami/drupal
```

## `list`

### Listing your installations

> NOTE: By default, Helm uses the namespace your Kubernetes configuration file sets as the default.

```sh
$ helm list
NAME    NAMESPACE  REVISION  UPDATED       STATUS    CHART           APP VERSION
mysite  default    1         2020-06-14... deployed  drupal-7.0.0    9.0.0
```

### Listing installations across all namespaces

```sh
$ helm list --all-namespaces
NAME    NAMESPACE  REVISION  UPDATED        STATUS     CHART         APP VERSION
mysite  default    1         2020-06-14...  deployed   drupal-7.0.0  9.0.0
mysite  first      1         2020-06-14...  deployed   drupal-7.0.0  9.0.0
mysite  second     1         2020-06-14...  deployed   drupal-7.0.0  9.0.0
```

## `upgrade`

Upgrading an installation can consist of two different kinds of changes:

- You can upgrade the version of the chart
- You can upgrade the configuration of the installation

The two are not mutually exclusive; you can do both at the same time.

## `uninstall`

### Uninstalling an Installation

```sh
helm uninstall mysite
```

### Uninstalling an Installation via namespace

```sh
helm uninstall mysite --namespace=first
```

### Retain release records

```sh
helm uninstall --keep-history
```

## Configuration at Install Time

There are several ways of telling Helm which values you want to be configured. The best way is to create a YAML file with all of the configuration overrides.

### `values.yaml` file

> NOTE: The filename does not need to be "values". This is just convention.

Since it is in a file, it is easy to reproduce the same installation. You can also check this file into a version control system to track changes to your values over time. The Helm core maintainers consider it a good practice to keep your configuration values in a YAML file. It is important to keep in mind, though, that if a configuration file has sensitive information (like a password or authentication token), you should take steps to ensure that this information is not leaked.

> NOTE: You can specify the `--values` flag multiple times. Some people use this feature to have “common” overrides in one file and specific overrides in another.

```sh
$ helm install mysite bitnami/drupal --values values.yaml
NAME: mysite
LAST DEPLOYED: Sun Jun 14 14:56:15 2020
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
*******************************************************************
*** PLEASE BE PATIENT: Drupal may take a few minutes to install ***
*******************************************************************

1. Get the Drupal URL:

  You should be able to access your new Drupal installation through

  http://drupal.local/

2. Login with the following credentials

  echo Username: admin
  echo Password: $(kubectl get secret --namespace default mysite-drupal -o js...
```

### Imperative configuration

This sets just one parameter, `drupalUsername`. This flag uses a simple `key=value` format.

> NOTE: Subsections are a little more complicated when using the `--set` flag. You will need to use a dotted notation: `--set mariadb.db.name=my-database`. This can get verbose when setting multiple values.

```sh
helm install mysite bitnami/drupal --set drupalUsername=admin
```

## How Helm Stores Release Information

By default, Helm stores these records as Kubernetes Secrets (though there are other supported storage backends).

We can see these records with `kubectl get secret`:

```sh
$ kubectl get secret
NAME                           TYPE                                  DATA   AGE
default-token-vjhx2            kubernetes.io/service-account-token   3      58m
mysite-drupal                  Opaque                                1      13m
mysite-mariadb                 Opaque                                2      13m
sh.helm.release.v1.mysite.v1   helm.sh/release.v1                    1      13m
sh.helm.release.v1.mysite.v2   helm.sh/release.v1                    1      13m
sh.helm.release.v1.mysite.v3   helm.sh/release.v1                    1      7m53s
sh.helm.release.v1.mysite.v4   helm.sh/release.v1                    1      5m30s
```
