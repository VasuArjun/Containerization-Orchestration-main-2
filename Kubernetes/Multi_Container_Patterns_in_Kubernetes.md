
# Multi-Container Patterns in Kubernetes

This document provides an overview of various container patterns used in Kubernetes, including practical examples and common usage scenarios. 

## 1. Init Containers
**Description**: Init Containers are specialized containers that run before the main containers in a Pod. They are ideal for tasks such as setting up permissions, adjusting configuration files.

**Example**: Initializing a data directory that will be used by the main app container.
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: example-pod
spec:
  containers:
  - name: main-app
    image: nginx
    volumeMounts:
    - name: data-volume
      mountPath: /usr/share/nginx/html
  initContainers:
  - name: init-permissions
    image: busybox
    command: ['sh', '-c', 'chmod -R 777 /usr/share/nginx/html']
    volumeMounts:
    - name: data-volume
      mountPath: /usr/share/nginx/html
  volumes:
  - name: data-volume
    emptyDir: {}
```

## 2. Sidecar Containers
**Description**: Sidecar containers are used to support the main container by handling additional tasks like logging, monitoring, or configuration updates without modifying the application.

**Example**: Adding a logging sidecar to collect logs from a main app.
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: logging-pod
spec:
  containers:
  - name: main-app
    image: myapp:latest
  - name: log-collector
    image: fluentd:latest
    volumeMounts:
    - name: log-volume
      mountPath: /var/log/myapp
  volumes:
  - name: log-volume
    emptyDir: {}
```

## 3. Ambassador Containers
**Description**: Ambassador containers act as proxies between the main container and external services. They simplify networking challenges like splitting or sharding data to different databases based on request type.

**Example**: Using an ambassador container to manage database connections.
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: db-ambassador-pod
spec:
  containers:
  - name: main-app
    image: myapp:latest
  - name: db-ambassador
    image: ambassador:latest
    env:
    - name: DB_SHARD
      value: "shard1"
```

## 4. Adapter Containers
**Description**: Adapter containers transform the output from the main container to suit external systems, making integration easier without changing the main application.

**Example**: Formatting log output to match an external monitoring tool's requirements.
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: adapter-pod
spec:
  containers:
  - name: main-app
    image: myapp:latest
  - name: log-adapter
    image: log-adapter:latest
    volumeMounts:
    - name: log-volume
      mountPath: /var/log/myapp
  volumes:
  - name: log-volume
    emptyDir: {}
```

These patterns provide powerful ways to enhance and manage the behavior of applications running in Kubernetes environments by leveraging multiple containers within a single pod.
