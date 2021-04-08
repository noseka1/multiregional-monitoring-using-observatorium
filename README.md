# Multi-cluster observability

```
$ oc apply --kustomize observatorium-namespace/base
```

Search for REPLACE string and customize the manifests before deploying.

```
$ oc apply --kustomize observatorium-instance/base
```

```
$ oc apply --kustomize prometheus-operator/base
```

Search for REPLACE string and customize the manifests before deploying.

```
$ kustomize build prometheus-instance/base | oc apply --filename -
```

For testing purposes, you can expose some of the endpoints:

```
$ oc expose svc prometheus-operated
```

```
$ oc expose svc observatorium-thanos-query --port http
```

```
$ oc apply --kustomize grafana-operator/base
```

```
$ oc apply --kustomize grafana-instance/base
```
