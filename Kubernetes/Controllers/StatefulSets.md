
# Kubernetes StatefulSets

## Overview
StatefulSets are a Kubernetes workload API object that manages stateful applications. They are used when you need stable, unique network identifiers, stable persistent storage, and ordered, graceful deployment and scaling.

Unlike Deployments, StatefulSets ensure that the order and uniqueness of these pods are maintained, making them ideal for applications like databases, clustered applications, and any other applications that require stable identifiers.

## StatefulSet YAML Example
Here's an example of a StatefulSet manifest:
```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: example-statefulset
  labels:
    app: myapp
spec:
  serviceName: "myservice"
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp-container
        image: myapp:latest
        ports:
        - containerPort: 80
  volumeClaimTemplates:
  - metadata:
      name: storage
    spec:
      accessModes: [ "ReadWriteOnce" ]
      storageClassName: "my-storage-class"
      resources:
        requests:
          storage: 1Gi
```
This manifest creates a StatefulSet named `example-statefulset` that manages three replicas of `myapp:latest`, each with its own persistent volume claim.

## Common Commands

### Creating and Managing StatefulSets
- **Create a StatefulSet**:
  ```bash
  kubectl create -f statefulset-ex.yml  # create StatefulSet from a file
  ```

- **Update a StatefulSet**:
  ```bash
  kubectl apply -f statefulset-ex.yml  # apply changes to the StatefulSet
  ```

### Querying StatefulSets
- **List all StatefulSets in the current namespace**:
  ```bash
  kubectl get statefulsets
  ```

- **List StatefulSets in a specific namespace**:
  ```bash
  kubectl get statefulsets -n <namespace>
  ```

- **Describe a StatefulSet**:
  ```bash
  kubectl describe statefulset <statefulset-name>
  ```

### Scaling and Updating StatefulSets
- **Scale a StatefulSet**:
  ```bash
  kubectl scale statefulset <statefulset-name> --replicas=x
  ```

- **View rollout status of a StatefulSet**:
  ```bash
  kubectl rollout status statefulset <statefulset-name>
  ```

### Additional Operations
- **Delete a StatefulSet**:
  ```bash
  kubectl delete statefulset <statefulset-name>
  ```

## Summary
StatefulSets are crucial for managing stateful applications in Kubernetes, providing predictable naming and persistent storage, which are essential for applications that require careful state management and data persistence.
