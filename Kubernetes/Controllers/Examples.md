
# Kubernetes Workload Examples

This document provides basic YAML examples for various Kubernetes workloads, including Pods, Deployments, StatefulSets, DaemonSets, Jobs, and CronJobs. These examples are useful for understanding the basic configurations necessary to deploy different types of applications and services in a Kubernetes environment.

## 1. Pod Example

A simple Pod running an Nginx container:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: example-pod
spec:
  containers:
  - name: nginx
    image: nginx:alpine
    ports:
    - containerPort: 80
```

## 2. Deployment Example

A Deployment managing multiple replicas of an Nginx container:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:alpine
        ports:
        - containerPort: 80
```

## 3. StatefulSet Example

A StatefulSet providing stable storage using Persistent Volumes:

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: nginx-statefulset
spec:
  serviceName: "nginx"
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:alpine
        ports:
        - containerPort: 80
  volumeClaimTemplates:
  - metadata:
      name: nginx-storage
    spec:
      accessModes: ["ReadWriteOnce"]
      storageClassName: "standard"
      resources:
        requests:
          storage: 1Gi
```

## 4. DaemonSet Example

A DaemonSet ensuring every node runs an Nginx container:

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: nginx-daemonset
spec:
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:alpine
        ports:
        - containerPort: 80
```

## 5. Job Example

A Job that executes a one-time task using BusyBox:

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: example-job
spec:
  template:
    spec:
      containers:
      - name: busybox
        image: busybox
        command: ["sh", "-c", "echo Hello Kubernetes! && sleep 30"]
      restartPolicy: Never
  backoffLimit: 4
```

## 6. CronJob Example

A CronJob that runs a task every 5 minutes:

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: example-job
spec:
  schedule: " */1 * * * *"
  jobTemplate:
    spec:
      backoffLimit: 4
      template:
       spec:
         containers:
         - name: busybox
           image: busybox
           command: ["sh", "-c", "echo Hello Kubernetes! && sleep 1 m"]
         restartPolicy: Never
```

Each of these examples is designed to be a minimal, straightforward demonstration of how to use different Kubernetes resources for deploying and managing applications and tasks.
