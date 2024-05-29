
# Kubernetes Probes

## Overview of Probes
Probes are used by Kubernetes to manage the health of containers within a pod. There are three types of probes:

1. **Liveness Probes**: Ensure that containers are running as expected. If these fail, the kubelet kills and restarts the container.
2. **Readiness Probes**: Ensure that containers are ready to serve traffic. Containers failing readiness will not receive traffic from services.
3. **Startup Probes**: Used during the initial start of a container to ensure it doesn't get killed before it's fully operational.

## Probe Configuration Parameters
- **`initialDelaySeconds`**: Number of seconds after the container has started before liveness or readiness probes are initiated.
- **`periodSeconds`**: Frequency in seconds to perform the probe.
- **`failureThreshold`**: Number of times the probe needs to fail to take action (e.g., restart the pod for liveness probes).

## Probe Examples

### Liveness Probe
A liveness probe that checks if a web server is up by sending an HTTP GET request:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: liveness-pod
spec:
  containers:
  - name: web-server
    image: nginx
    livenessProbe:
      httpGet:
        path: /
        port: 80
      initialDelaySeconds: 15
      timeoutSeconds: 2
      periodSeconds: 5
      failureThreshold: 3
```

### Readiness Probe
A readiness probe that checks if a web server is ready to handle requests:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: readiness-pod
spec:
  containers:
  - name: web-server
    image: nginx
    readinessProbe:
      httpGet:
        path: /
        port: 80
      initialDelaySeconds: 5
      periodSeconds: 5
      successThreshold: 1
      failureThreshold: 3
```

### Startup Probe
A startup probe used for applications that take longer to start, using a command:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: startup-pod
spec:
  containers:
  - name: web-server
    image: nginx
    startupProbe:
      exec:
        command:
        - cat
        - /app/ready
      initialDelaySeconds: 10
      periodSeconds: 5
      failureThreshold: 30
```

These examples provide a basis for configuring probes in Kubernetes to ensure the health and readiness of containers.
