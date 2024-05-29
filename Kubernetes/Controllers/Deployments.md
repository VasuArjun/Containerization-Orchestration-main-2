# Kubernetes Deployments

Kubernetes Deployments are a powerful feature used to manage the state and rollout of your applications. A Deployment provides declarative updates for Pods and ReplicaSets, meaning you describe the desired state in a Deployment configuration, and Kubernetes changes the actual state to the desired state at a controlled rate. This abstraction allows you to focus on the application rather than managing individual pods.

## Typical Uses of Deployments

Deployments are typically used to:

- **Manage a Deployment's lifecycle**: Deployments enable you to specify strategies like a rolling update for updating applications. This strategy ensures zero downtime by incrementally updating pod instances with new ones.

- **Scale applications**: You can easily increase or decrease the number of pods handling your application traffic.

- **Self-heal applications**: Deployments automatically replace pods that fail, are deleted, or are terminated. This is crucial for maintaining the application's availability without manual intervention.

- **Rollback applications**: If a Deployment's current state becomes unstable, you can rollback to a previous stable version. Deployments keep a history of applied configurations which can be reverted anytime.

- **Deployment orchestration**: Deployments utilize ReplicaSets to manage the orchestration of the application pods. While you can use ReplicaSets directly, Deployments simplify many management tasks and should be preferred for scalable and resilient applications.


## Deployment YAML Example
Here's an example of a Deployment manifest file:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: example-deployment
  labels:
    app: myapp
spec:
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
```
This YAML configures a Deployment named `example-deployment` that oversees three replicas of a container using the `myapp:latest` image.

## Common Commands

### Creating and Managing Deployments
- **Create a Deployment**:
  ```bash
  kubectl create -f deployment-ex.yml  # create deployment from a file
  ```

- **Update a Deployment**:
  ```bash
  kubectl apply -f deployment-ex.yml  # apply changes to the deployment
  ```

### Querying Deployments
- **List all Deployments in the current namespace**:
  ```bash
  kubectl get deployments
  ```

- **List Deployments in a specific namespace**:
  ```bash
  kubectl get deployments -n <namespace>
  ```

- **Show labels for all Deployments**:
  ```bash
  kubectl get deployments --show-labels
  ```

- **Describe a Deployment**:
  ```bash
  kubectl describe deployment <deployment-name>
  ```

### Scaling and Updating Deployments
- **Scale a Deployment**:
  ```bash
  kubectl scale deployment <deployment-name> --replicas=x
  ```

- **Rollback a Deployment**:
  ```bash
  kubectl rollout undo deployment <deployment-name>
  ```

- **View rollout status**:
  ```bash
  kubectl rollout status deployment <deployment-name>
  ```

### Additional Operations
- **Delete a Deployment**:
  ```bash
  kubectl delete deployment <deployment-name>
  ```

## Summary
Deployments in Kubernetes facilitate robust, scalable, and resilient application management, enabling automated updates, scaling, and self-healing capabilities for modern cloud-native applications.
