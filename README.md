# Multi-regional monitoring using Observatorium

## Overview

This proof-of-concept (POC) deploys Observatorium + Prometheus + Grafana. See also [architecture diagrams](docs/diagrams).

Components used:
* [Observatorium](https://github.com/observatorium)
* [Prometheus operator](https://github.com/prometheus-operator/prometheus-operator)
* [Grafana operator](https://github.com/integr8ly/grafana-operator)

## Deploying

You will need TLS certificates for Observatorium API, Prometheus and Grafana. See [Creating TLS certificates](docs/creating_tls_certificates.md) for more details on how to generate them. Alternatively, Observatorium also supports [OIDC (OpenID Connect) for authentication](https://github.com/observatorium/deployments/tree/master/environments/local).

Create `observatorium` namespace. Everything else will be deployed into this namespace:

```
$ oc apply --kustomize observatorium-namespace/base
```

Observatorium requires an S3-compatible storage. If you don't have an existing S3 storage, you can leverage [MinIO](https://min.io/). Deploy MinIO S3 compatible object storage:

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

For testing purposes, you can expose some of the endpoints. Note that the commands below create OpenShift routes which expose unprotected services to the outside of the cluster. If this is an issue, use `oc port-forward` instead.

Create route for Prometheus UI:

```
$ oc expose svc prometheus-operated
```

Create route for Thanos Query UI:

```
$ oc expose svc observatorium-thanos-query --port http
```

Create route for Thanos Alerts Web UI (inactive, pending, firing alerts):

```
$ oc expose svc observatorium-thanos-rule --port http
```

## Related documentation

* [Observatorium Operator deployment documentation](https://github.com/observatorium/operator/blob/master/docs/deploy-operator.md)
* [Observatorium architecture diagram](https://github.com/observatorium/docs/blob/master/architecture/architecture.md)
* [Introducing Thanos: Prometheus at scale](https://www.improbable.io/blog/thanos-prometheus-at-scale)
* [Thanos documentation](https://thanos.io/tip/thanos/getting-started.md/)
* [Prometheus documentation](https://prometheus.io/docs/introduction/overview/)
