
# My Webapp Helm Chart

This Helm chart deploys "My Webapp" on a Kubernetes cluster using the Helm package manager.

## Prerequisites
- Kubernetes 1.12+
- Helm 3.0+

## Installing the Chart
To install the chart with the release name `my-release`:
```bash
helm install my-release my-webapp/
```

## Uninstalling the Chart
To uninstall/delete the `my-release` deployment:
```bash
helm delete my-release
```

## Configuration
The following table lists the configurable parameters of the My Webapp chart and their default values.
| Parameter | Description | Default |
|-----------|-------------|---------|
| `serviceAccount.name` | The name of the service account | `my-service-account` |
| `configMap.v1.name` | The name of the first version configmap | `my-config-v1` |
| `configMap.v2.name` | The name of the second version configmap | `my-config-v2` |
| `persistentVolumeClaim.name` | The name of the PVC | `my-pvc` |
| `persistentVolumeClaim.storageClassName` | Storage class for PVC | `managed-premium` |
| `persistentVolumeClaim.size` | Storage size for PVC | `5Gi` |
| `deployments.v1.name` | Deployment name for version 1 | `my-deployment-v1` |
| `deployments.v1.replicas` | Number of replicas for version 1 | `2` |
| `deployments.v1.image` | Container image for version 1 | `nginx:latest` |
| `deployments.v2.name` | Deployment name for version 2 | `my-deployment-v2` |
| `deployments.v2.replicas` | Number of replicas for version 2 | `2` |
| `deployments.v2.image` | Container image for version 2 | `nginx:latest` |
| `service.name` | Service name | `my-loadbalancer-service` |
| `service.type` | Type of service | `LoadBalancer` |
| `service.port` | Service port | `80` |

## Additional Information
This chart also supports changing the number of replicas via the values file.
