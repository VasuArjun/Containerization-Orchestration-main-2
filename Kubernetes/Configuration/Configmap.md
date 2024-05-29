
# ConfigMap Usage in Kubernetes

ConfigMaps are Kubernetes objects used to store non-confidential configuration data as key-value pairs. This document details the processes to create and consume ConfigMaps both from multiple files and literal values within Kubernetes Pods.

## Creating and Consuming ConfigMap from Multiple Files

### Step 1: Create ConfigMap from Files
Create configuration files and then a ConfigMap named `nginx-configmap-vol`:

```bash
# Create configuration files
echo -n 'Non-sensitive data inside app.properties' > app.properties
echo -n 'Non-sensitive data inside ui.properties' > ui.properties

# Create ConfigMap
kubectl create configmap nginx-configmap-vol --from-file=app.properties --from-file=ui.properties

# Verify ConfigMap creation
kubectl get configmaps
kubectl describe configmaps nginx-configmap-vol
```

### Step 2: Consume ConfigMap in a Pod
Define a Pod to use the ConfigMap as a volume:

```yaml
# File: nginx-pod-configmap-vol.yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod-configmap-vol
spec:
  containers:
  - name: nginx-container
    image: nginx
    volumeMounts:
    - name: config-volume
      mountPath: "/etc/config"
      readOnly: true
  volumes:
    - name: config-volume
      configMap:
        name: nginx-configmap-vol
        items:
        - key: app.properties
          path: app.properties
        - key: ui.properties
          path: ui.properties
```

### Step 3: Validate the Configuration
Deploy the Pod and validate the mounted ConfigMap:

```bash
# Deploy the Pod
kubectl create -f nginx-pod-configmap-vol.yaml

# Check Pod and ConfigMap
kubectl get pods
kubectl get configmaps
kubectl describe pod nginx-pod-configmap-vol

# Verify the configuration inside the Pod
kubectl exec nginx-pod-configmap-vol -- /bin/sh -c 'ls /etc/config && cat /etc/config/app.properties && cat /etc/config/ui.properties'
exit
```

## Creating and Consuming ConfigMap from Literal Values

### Step 1: Create ConfigMap from Literals
Create a ConfigMap `redis-configmap-env` from literal values:

```bash
# Create ConfigMap
kubectl create configmap redis-configmap-env --from-literal=file_1_content=file_a --from-literal=file_2_content=file_b

# Verify ConfigMap creation
kubectl get configmaps
kubectl describe configmap redis-configmap-env
```

### Step 2: Consume ConfigMap in a Pod via Environment Variables
Configure a Pod to use the ConfigMap values as environment variables:

```yaml
# File: redis-pod-configmap-env.yaml
apiVersion: v1
kind: Pod
metadata:
  name: redis-pod-configmap-env
spec:
  containers:
  - name: redis-container
    image: redis
    env:
    - name: FILE_1_CONTENT
      valueFrom:
        configMapKeyRef:
          name: redis-configmap-env
          key: file_1_content
    - name: FILE_2_CONTENT
      valueFrom:
        configMapKeyRef:
          name: redis-configmap-env
          key: file_2_content
  restartPolicy: Never
```

### Step 3: Validate the Environment Variables
Deploy the Pod and validate the environment variables:

```bash
# Deploy the Pod
kubectl create -f redis-pod-configmap-env.yaml

# Check Pod and ConfigMap
kubectl get pods
kubectl get configmaps
kubectl describe pod redis-pod-configmap-env

# Verify environment variables inside the Pod
kubectl exec redis-pod-configmap-env -- env | grep FILE
exit
```
