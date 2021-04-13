# csmr-examples

## Multi-Repo mode, unstructured format

For Config Sync multi-repo mode with unstructured format, use this [example](./root-multirepo-unstructured):

First, create 2 files with a `ConfigManagement` and a `RootSync` custom resource:

```yaml
# config-management.yaml
apiVersion: configmanagement.gke.io/v1
kind: ConfigManagement
metadata:
  name: config-management
spec:
  enableMultiRepo: true
  policyController:
    enabled: true
```

```yaml
# root-sync.yaml
# If you are using a Config Sync version earlier than 1.7,
# use: apiVersion: configsync.gke.io/v1alpha1
apiVersion: configsync.gke.io/v1beta1
kind: RootSync
metadata:
  name: root-sync
  namespace: config-management-system
spec:
  sourceFormat: unstructured
  git:
    repo: https://github.com/janetkuo/csmr-examples.git
    branch: main
    dir: "roota-multirepo-unstructured"
    auth: none
```

Then, apply them to the cluster:

```
kubectl -f config-management.yaml
kubectl -f root-sync.yaml
```

## Mono-Repo mode, unstructured format

For Config Sync mono-repo mode with unstructured format, use this [example](./root-monorepo-unstructured):

First, create a file with a `ConfigManagement` custom resource:

```yaml
# config-management.yaml
apiVersion: configmanagement.gke.io/v1
kind: ConfigManagement
metadata:
  name: config-management
spec:
  policyController:
    enabled: true
  git:
    syncRepo: https://github.com/janetkuo/csmr-examples
    syncBranch: main
    secretType: none
    policyDir: root-monorepo-unstructured
  sourceFormat: unstructured
```

Then, apply it to the cluster:

```
kubectl -f config-management.yaml
```