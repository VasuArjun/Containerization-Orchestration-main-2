
# Kubernetes Deployment Comparison

This document outlines how to deploy a simple web application both directly using Kubernetes manifests and using Helm, providing a comparison between the two methods.

## Part 1: Without Helm

### Manifest Files

**Service Account**
```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: my-service-account
```

**ConfigMaps**
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-config-v1
data:
  index.html: |
    <html>
    <head><title>Version 1</title></head>
    <body>
      <h1>Welcome to Version 1 of our Application</h1>
    </body>
    </html>
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-config-v2
data:
  index.html: |
    <html>
    <head><title>Version 2</title></head>
    <body>
      <h1>Welcome to Version 2 of our Application</h1>
    </body>
    </html>
```

**PersistentVolumeClaim**
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: managed-premium
  resources:
    requests:
      storage: 5Gi
```

**Deployments**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment-v1
spec:
  replicas: 2
  selector:
    matchLabels:
      app: webapp
      version: v1
  template:
    metadata:
      labels:
        app: webapp
        version: v1
    spec:
      containers:
      - name: nginx-container-v1
        image: nginx:latest
        ports:
        - containerPort: 80
        volumeMounts:
        - name: html-volume
          mountPath: "/usr/share/nginx/html"
          readOnly: true
      volumes:
      - name: html-volume
        configMap:
          name: my-config-v1
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment-v2
spec:
  replicas: 2
  selector:
    matchLabels:
      app: webapp
      version: v2
  template:
    metadata:
      labels:
        app: webapp
        version: v2
    spec:
      containers:
      - name: nginx-container-v2
        image: nginx:latest
        ports:
        - containerPort: 80
        volumeMounts:
        - name: html-volume
          mountPath: "/usr/share/nginx/html"
          readOnly: true
      volumes:
      - name: html-volume
        configMap:
          name: my-config-v2
```

**Service**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-loadbalancer-service
spec:
  type: LoadBalancer
  selector:
    app: webapp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```

## Part 2: Using Helm

### Overview
This Helm chart deploys the same web application with two versions. It includes configurations for a Service Account, ConfigMaps, a PersistentVolumeClaim, Deployments, and a LoadBalancer Service.

### Chart Structure
```plaintext
my-helm-chart/
├── Chart.yaml
├── values.yaml
├── templates/
│   ├── 01-service-account.yaml
│   ├── 02-configmaps.yaml
│   ├── 03-persistentvolumeclaim.yaml
│   ├── 04-deployments.yaml
│   └── 05-service.yaml
└── .helmignore
```

### Deployment Example
Deploy the chart with the following command:
```bash
helm install my-webapp ./my-helm-chart
```

### Clean Up
Remove the deployment with:
```bash
helm delete my-webapp
```

## Conclusion
Using Helm offers flexibility, reduces redundancy, and simplifies configuration management across different environments or releases. Without Helm, each change needs to be manually tracked and applied.

## values.yaml
```yaml

replicaCount: 2
image:
  repository: nginx
  tag: latest
  pullPolicy: IfNotPresent

serviceAccount:
  name: my-service-account

configMaps:
  v1Name: my-config-v1
  v2Name: my-config-v2

pvc:
  name: my-pvc
  accessModes:
    - ReadWriteOnce
  storageClassName: managed-premium
  size: 5Gi

deployments:
  v1:
    name: my-deployment-v1
    versionLabel: v1
  v2:
    name: my-deployment-v2
    versionLabel: v2

service:
  name: my-loadbalancer-service
  type: LoadBalancer
  port: 80
```

# Explanation of Helm Components

## Chart.yaml
This file defines the metadata about the chart like the name, version, and description. It is used by Helm to package and display information about the chart.

## values.yaml
This file contains the default values for the chart templates. These values can be overridden by the user during installation to customize the deployment as per specific needs.

## templates/
This directory contains template files for all Kubernetes resources that will be created by the Helm chart. Helm uses Go templating for dynamic content generation based on the `values.yaml` file.

## .helmignore
This file specifies patterns to ignore when packaging the chart, similar to `.gitignore` in git repositories.

## Dependencies
Helm charts can also have dependencies on other Helm charts, which would be managed through the Chart.yaml file or a separate requirements.yaml.

## Notes.txt (optional)
This file can be added to the `templates/` directory to provide usage notes to the user post-installation, which is displayed after the Helm chart is successfully deployed.
