# DbGate Helm Chart

Helm chart for deploying [DbGate](https://github.com/dbgate/dbgate) - a modern database management client that supports MySQL, PostgreSQL, MongoDB, Redis, and more.

## Installation

```bash
helm repo add antiantiops https://antiantiops.github.io/dbgate-helm-chart
helm repo update
helm install dbgate antiantiops/dbgate
```

Or install from source:

```bash
git clone https://github.com/antiantiops/dbgate-helm-chart.git
cd dbgate-helm-chart
helm install dbgate ./dbgate
```

## Configuration

Key configuration options in `values.yaml`:

| Parameter | Description | Default |
|-----------|-------------|---------|
| `image.repository` | DbGate image repository | `dbgate/dbgate` |
| `image.tag` | DbGate image tag | `5.5.5` |
| `service.type` | Kubernetes service type | `ClusterIP` |
| `service.port` | Service port | `3000` |
| `ingress.enabled` | Enable ingress | `false` |
| `persistence.enabled` | Enable persistent storage | `true` |
| `persistence.size` | PVC size | `1Gi` |
| `resources.limits.cpu` | CPU limit | `500m` |
| `resources.limits.memory` | Memory limit | `512Mi` |

## Examples

### Basic deployment
```bash
helm install dbgate ./dbgate
```

### With ingress
```bash
helm install dbgate ./dbgate \
  --set ingress.enabled=true \
  --set ingress.hosts[0].host=dbgate.example.com
```

### Custom resources
```bash
helm install dbgate ./dbgate \
  --set resources.limits.memory=1Gi \
  --set resources.requests.cpu=200m
```

### Without persistence
```bash
helm install dbgate ./dbgate \
  --set persistence.enabled=false
```

## Accessing DbGate

After installation:

```bash
# Port-forward to local machine
kubectl port-forward svc/dbgate 3000:3000

# Open browser
open http://localhost:3000
```

## Features

- ✅ Persistent storage for database connections
- ✅ Configurable resource limits
- ✅ Ingress support
- ✅ Horizontal pod autoscaling
- ✅ Security context
- ✅ Liveness/readiness probes

## Uninstall

```bash
helm uninstall dbgate
```

## Links

- [DbGate GitHub](https://github.com/dbgate/dbgate)
- [DbGate Documentation](https://dbgate.org/)
