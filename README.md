# DbGate Helm Chart

<p align="center">
  <img src="dbgate/icon.png" alt="DbGate" width="128" />
</p>

<p align="center">
  <a href="https://github.com/antiantiops/dbgate-helm-chart/releases"><img src="https://img.shields.io/github/v/release/antiantiops/dbgate-helm-chart?label=chart&sort=semver" alt="Chart Version"></a>
  <a href="https://github.com/antiantiops/dbgate-helm-chart/actions/workflows/ci.yaml"><img src="https://github.com/antiantiops/dbgate-helm-chart/actions/workflows/ci.yaml/badge.svg" alt="CI"></a>
  <a href="https://github.com/dbgate/dbgate"><img src="https://img.shields.io/badge/app-DbGate-blue" alt="DbGate"></a>
  <a href="https://github.com/antiantiops/dbgate-helm-chart/blob/main/LICENSE"><img src="https://img.shields.io/github/license/antiantiops/dbgate-helm-chart" alt="License"></a>
</p>

Helm chart to deploy [DbGate](https://dbgate.org/) — a fast, modern, open-source database client and SQL editor supporting MySQL, PostgreSQL, SQL Server, MongoDB, Redis, SQLite, Oracle, and more — on Kubernetes.

## Quick Start

```bash
helm repo add dbgate https://antiantiops.github.io/dbgate-helm-chart
helm repo update
helm upgrade --install dbgate dbgate/dbgate \
  --namespace dbgate --create-namespace
```

Then access DbGate:

```bash
kubectl port-forward svc/dbgate 3000:3000 -n dbgate
# Open http://localhost:3000
```

## Production Example

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
  size: 5Gi

resources:
  limits:
    cpu: "1"
    memory: 1Gi
  requests:
    cpu: 200m
    memory: 256Mi
```

```bash
helm upgrade --install dbgate dbgate/dbgate -n dbgate -f values-production.yaml
```

## Configuration

| Value | Default | Description |
| --- | --- | --- |
| `image.repository` | `dbgate/dbgate` | Container image |
| `image.tag` | `5.5.5` | Image tag |
| `service.type` | `ClusterIP` | Service type |
| `service.port` | `3000` | Service port |
| `ingress.enabled` | `false` | Create Ingress |
| `ingress.className` | `""` | IngressClass |
| `persistence.enabled` | `true` | Enable PVC |
| `persistence.size` | `1Gi` | PVC size |
| `persistence.storageClass` | `""` | StorageClass |
| `resources.limits.cpu` | `500m` | CPU limit |
| `resources.limits.memory` | `512Mi` | Memory limit |
| `env` | `[]` | Extra environment variables |

See [`dbgate/values.yaml`](dbgate/values.yaml) for the full list.

## Features

- ✅ Persistent storage for connections, queries, and settings
- ✅ Configurable resource limits and requests
- ✅ Ingress with TLS support
- ✅ Horizontal Pod Autoscaling
- ✅ Security context (non-root)
- ✅ Liveness and readiness probes
- ✅ Automated releases via GitHub Actions

## Links

- [DbGate Website](https://dbgate.org/)
- [DbGate GitHub](https://github.com/dbgate/dbgate)
- [Artifact Hub](https://artifacthub.io/) *(pending registration)*

## License

This chart is open-source. DbGate itself is licensed under the [MIT License](https://github.com/dbgate/dbgate/blob/master/LICENSE).
