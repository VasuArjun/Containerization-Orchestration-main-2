# Kubernetes Services

## Overview

Kubernetes Services are an abstract way to expose an application running on a set of Pods as a network service. With Kubernetes, you don't need to modify your application to use an unfamiliar service discovery mechanism. Kubernetes gives Pods their own IP addresses and a single DNS name for a set of Pods, and can load-balance across them.

## Types of Services

### 1. ClusterIP

- **Default type** of service.
- Gives a service its own IP address, making it only reachable within the cluster.
- Useful for internal applications or a backend service that needs to be hidden from the external environment.

### 2. NodePort

- Exposes the service on each Node’s IP at a static port (the NodePort).
- A ClusterIP service, to which the NodePort service will route, is automatically created.
- You can contact the NodePort service, from outside the cluster, by requesting `<NodeIP>:
<NodePort>`.

### 3. LoadBalancer
Exposes the service externally using a cloud provider’s load balancer.
NodePort and ClusterIP services are automatically created, and the external load balancer will route to them.
This is the simplest way to get external traffic directly to your service.



***Example: LoadBalancer Service***
File: my-loadbalancer-service.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-loadbalancer-service
spec:
  type: LoadBalancer
  selector:
    app: webapp  # This must match the labels of the pods created by your deployment.
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80  # The port on the pod where the traffic will be sent. Matches the exposed pod port.
```

Explanation:

Purpose: Distributes incoming network traffic across pods tagged with app: webapp. This service manages the external access to the web application, providing a single point of access.
Selector: Ensures the service routes traffic to the correct pods.
Ports: Configures the external and internal communication ports.


***Example: NodePort Service***
File: webapp-service.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: webapp-service
spec:
  type: NodePort
  selector:
    app: webapp
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30007  # The static port assigned on all nodes.
```

Explanation:

Purpose: Allows external traffic on a specific port on each node which is then forwarded to the pods. Ideal for development environments or internal access within a larger enterprise.
Selector: Connects the service to the correct pods.
Ports: Details the configuration of network ports, facilitating the routing of traffic from external sources directly to the designated pods.


***Example: Deployment Managing Pods***
File: webapp-deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webapp
spec:
  replicas: 2
  selector:
    matchLabels:
      app: webapp
  template:
    metadata:
      labels:
        app: webapp
    spec:
      containers:
      - name: webapp
        image: nginx
        ports:
        - containerPort: 80
```

Explanation:

Purpose: Ensures high availability by maintaining two replicas of the Nginx server, handling incoming traffic through the services described above.
Replicas: Specifies the desired number of pod instances.
Selector and Labels: Key for linking the deployment to its services.
Containers: Configures the running containers, specifying the image and ports necessary for the Nginx server to function.
These examples and explanations should provide a clear guide on how Kubernetes services function and how they are used to expose and manage access to applications in a cluster.
