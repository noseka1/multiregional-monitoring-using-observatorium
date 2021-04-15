# Creating TLS certificates

Create root certificate + private key:

```
$ openssl req -new -x509 -subj "/CN=root" -newkey rsa:4096 -nodes -extensions v3_ca -days 3650 -keyout root.key.pem -out root.crt.pem
```

## Observatorium API server certificate

Create private key + CSR:

```
$ openssl req -new -subj "/CN=observatorium-api-observatorium" -newkey rsa:4096 -keyout observatorium-api-observatorium.key.pem -nodes -out observatorium-api-observatorium.csr.pem
```

Create TLS config file:

```
$ cat >observatorium-api-observatorium.ext <<EOF
subjectAltName=DNS:observatorium-api-observatorium.apps.cluster-4413.4413.example.opentlc.com, DNS:observatorium-observatorium-api.observatorium.svc.cluster.local, DNS:observatorium-api
certificatePolicies=1.3.6.1.5.5.7.3.1
EOF
```

Create certificate:

```
$ openssl x509 -req -in observatorium-api-observatorium.csr.pem -CA root.crt.pem -CAkey root.key.pem -CAcreateserial -extfile observatorium-api-observatorium.ext -days 3650 -out observatorium-api-observatorium.crt.pem
```

## Prometheus client certificate

Create private key + CSR:

```
$ openssl req -new -subj "/OU=prometheuses/CN=prometheus-1" -newkey rsa:4096 -keyout prometheus-1.key.pem -nodes -out prometheus-1.csr.pem
```

Create TLS config file:

```
$ cat >prometheus-1.ext <<EOF
subjectAltName=DNS:prometheus-1
certificatePolicies=1.3.6.1.5.5.7.3.2
EOF
```

Create certificate:

```
$ openssl x509 -req -in prometheus-1.csr.pem -CA root.crt.pem -CAkey root.key.pem -CAcreateserial -extfile prometheus-1.ext -days 3650 -out prometheus-1.crt.pem
```

## Grafana client certificate

(Same procedure as for Prometheus)

Create private key + CSR:

```
$ openssl req -new -subj "/OU=grafanas/CN=grafana-1" -newkey rsa:4096 -keyout grafana-1.key.pem -nodes -out grafana-1.csr.pem
```
Create TLS config file:

```
$ cat >grafana-1.ext <<EOF
subjectAltName=DNS:grafana-1
certificatePolicies=1.3.6.1.5.5.7.3.2
EOF
```

Create certificate:

```
$ openssl x509 -req -in grafana-1.csr.pem -CA root.crt.pem -CAkey root.key.pem -CAcreateserial -extfile grafana-1.ext -days 3650 -out grafana-1.crt.pem
```
