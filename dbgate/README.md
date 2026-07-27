# DbGate

[DbGate](https://dbgate.org/) is a fast, modern, open-source database client and SQL editor.
It supports MySQL, PostgreSQL, SQL Server, MongoDB, Redis, SQLite, Oracle, and more.
This chart deploys DbGate as a web application on Kubernetes.

## Prerequisites

- Kubernetes 1.25+
- Helm 3.12+
- A default StorageClass, or set `persistence.storageClass` explicitly
- An Ingress controller when `ingress.enabled=true`

## Install

```bash
helm repo add dbgate https://antiantiops.github.io/dbgate-helm-chart
helm repo update

helm upgrade --install dbgate dbgate/dbgate \
  --namespace dbgate --create-namespace
```

## Configuration

### Important values

| Value | Default | Description |
| --- | --- | --- |
| `image.repository` | `dbgate/dbgate` | Container image |
| `image.tag` | `5.5.5` | Image tag |
| `service.type` | `ClusterIP` | Service type |
| `service.port` | `3000` | Service port |
| `ingress.enabled` | `false` | Create an Ingress resource |
| `ingress.className` | `""` | IngressClass, e.g. `nginx` |
| `persistence.enabled` | `true` | Persist DbGate data |
| `persistence.size` | `1Gi` | PVC size |
| `persistence.storageClass` | `""` | StorageClass for PVC |
| `resources.limits.cpu` | `500m` | CPU limit |
| `resources.limits.memory` | `512Mi` | Memory limit |

### With Ingress

```yaml
# values-production.yaml
ingress:
  enabled: true
  className: nginx
  hosts:
    - host: dbgate.example.com
      paths:
        - path: /
          pathType: Prefix
  tls:
    - secretName: dbgate-tls
      hosts:
        - dbgate.example.com

persistence:
  storageClass: longhorn
```

Install with `helm upgrade --install dbgate dbgate/dbgate -n dbgate -f values-production.yaml`.

### Without persistence

```bash
helm upgrade --install dbgate dbgate/dbgate \
  --set persistence.enabled=false
```

### Environment variables

Pass database connection defaults or DbGate configuration via `env`:

```yaml
env:
  - name: WEB_ROOT
    value: /dbgate
```

## Persistence

| Component | Default | Purpose |
| --- | ---: | --- |
| `persistence` | 1Gi | DbGate connections, queries, and settings (`/root/.dbgate`) |

## Accessing DbGate

After installation with default `ClusterIP` service:

```bash
kubectl port-forward svc/dbgate 3000:3000 -n dbgate
# Open http://localhost:3000
```

## Uninstall

```bash
helm uninstall dbgate -n dbgate
```
