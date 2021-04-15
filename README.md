# Scalable monitoring

## Related documentation

* Observatorium Operator [deployment documentation](https://github.com/observatorium/operator/blob/master/docs/deploy-operator.md).
* Observatorium [architecture diagram](https://github.com/observatorium/docs/blob/master/architecture/architecture.md).

## Deploying

Create `observatorium` namespace. Everything else will be deployed into this namespace:

```
$ oc apply --kustomize observatorium-namespace/base
```

Deploy [MinIO](https://min.io/) S3 compatible object storage:

```
$ oc apply --kustomize minio/base
```

Install Observatorium operator:

```
$ oc apply --kustomize observatorium-operator/base
```

Deploy Observatorium. Search for REPLACE_ME string and customize manifests before deploying:

```
$ oc apply --kustomize observatorium-instance/base
```

Deploy Prometheus operator:

```
$ oc apply --kustomize prometheus-operator/base
```

Deploy Prometheus instance. Search for REPLACE_ME string and customize the manifests before deploying.

```
$ kustomize build prometheus-instance/base | oc apply --filename -
```

Deploy Grafana operator:

```
$ oc apply --kustomize grafana-operator/base
```

Deploy Grafana instance:
```
$ oc apply --kustomize grafana-instance/base
```

## Further tips

Note that the above commands create OpenShift route to unprotected services.

For testing purposes, you can expose some of the endpoints:

```
$ oc expose svc prometheus-operated
```

```
$ oc expose svc observatorium-thanos-query --port http
```

Thanos Alerts Web UI (inactive, pending, firing alerts)

```
$ oc expose svc observatorium-thanos-rule --port http
```
