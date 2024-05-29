
# Kubernetes DaemonSets

## Overview
DaemonSets are a Kubernetes API object that manage the deployment of replicated pods across nodes in a cluster. They ensure that all (or some) Kubernetes nodes run a copy of a specific pod, which is ideal for system-wide services such as log collection, monitoring daemons, or any service that needs to be run on all nodes.

## DaemonSet YAML Example
Here's an example of a DaemonSet manifest:
```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: example-daemonset
  labels:
    app: mydaemon
spec:
  selector:
    matchLabels:
      name: mydaemon
  template:
    metadata:
      labels:
        name: mydaemon
    spec:
      containers:
      - name: mydaemon-container
        image: mydaemon:latest
        ports:
        - containerPort: 80
```
This manifest creates a DaemonSet named `example-daemonset` that ensures every node runs a pod with the `mydaemon:latest` image.

## Common Commands

### Creating and Managing DaemonSets
- **Create a DaemonSet**:
  ```bash
  kubectl create -f daemonset-ex.yml  # create DaemonSet from a file
  ```

- **Update a DaemonSet**:
  ```bash
  kubectl apply -f daemonset-ex.yml  # apply changes to the DaemonSet
  ```

### Querying DaemonSets
- **List all DaemonSets in the current namespace**:
  ```bash
  kubectl get daemonsets
  ```

- **List DaemonSets in a specific namespace**:
  ```bash
  kubectl get daemonsets -n <namespace>
  ```

- **Describe a DaemonSet**:
  ```bash
  kubectl describe daemonset <daemonset-name>
  ```

### Additional Operations
- **Delete a DaemonSet**:
  ```bash
  kubectl delete daemonset <daemonset-name>
  ```

## Summary
DaemonSets are crucial for ensuring that certain applications are present on all or certain nodes, providing foundational services that are necessary for both the operation and management of Kubernetes clusters.
