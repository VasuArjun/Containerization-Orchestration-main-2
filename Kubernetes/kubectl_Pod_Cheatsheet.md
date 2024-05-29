
# Kubernetes kubectl Cheat Sheet

## Basic Commands

- **List all running pods in the current active namespace**:
  ```bash
  kubectl get pods
  ```

- **List all running pods in a specified namespace**:
  ```bash
  kubectl get pods -n kube-system
  ```

- **List all running pods across all namespaces**:
  ```bash
  kubectl get pods --all-namespaces
  ```

- **List all running pods in current active namespace with wider output**:
  ```bash
  kubectl get pods -o wide
  ```

- **List pods with labels**:
  ```bash
  kubectl get pods --show-labels
  ```

- **List pods with matching labels**:
  ```bash
  kubectl get pods -l env=prod
  ```

- **Watch pods in a specific namespace**:
  ```bash
  kubectl get pods -n kube-system --watch
  ```

- **List running pods based on field selector**:
  ```bash
  kubectl get pods --field-selector='status.phase=Running'
  ```

- **List unhealthy pods across all namespaces**:
  ```bash
  kubectl get pods --field-selector=status.phase!=Running --all-namespaces
  ```

- **List pods by selector**:
  ```bash
  kubectl get pods --selector="app=nginx"
  ```

- **List pods and images in custom columns**:
  ```bash
  kubectl get pods -o='custom-columns=PODS:.metadata.name,Images:.spec.containers[*].image'
  ```

- **List pods and containers in custom columns**:
  ```bash
  kubectl get pods -o='custom-columns=PODS:.metadata.name,CONTAINERS:.spec.containers[*].name'
  ```

- **List pods sorted by restart count**:
  ```bash
  kubectl get pods --sort-by='.status.containerStatuses[0].restartCount'
  ```

- **Get a detailed manifest of a pod in YAML format**:
  ```bash
  kubectl get pod <podname> -o yaml
  ```

- **Get a detailed manifest of a pod in JSON format**:
  ```bash
  kubectl get pod <podname> -o json
  ```

## Create, Delete, and Update Commands

- **Create a resource from a file**:
  ```bash
  kubectl create -f <filename>
  ```

- **Delete a pod**:
  ```bash
  kubectl delete pod <podname>
  ```

- **Update a deployment**:
  ```bash
  kubectl apply -f <filename>
  ```

- **Edit a pod**:
  ```bash
  kubectl edit pod <podname>
  ```

- **Scale a deployment**:
  ```bash
  kubectl scale deployment <deploymentname> --replicas=<num>
  ```

- **Rollout restart a deployment**:
  ```bash
  kubectl rollout restart deployment <deploymentname>
  ```

### Pod Descriptions and Logs

- **Detailed Description of a Pod**
  ```bash
  kubectl describe pod <podname>
  ```

- **Get Logs for a Pod**
  ```bash
  kubectl logs my-pod
  ```

- **Stream Logs from a Pod**
  ```bash
  kubectl logs -f my-pod
  ```


- **Generate Pod YAML (Dry Run)**
  ```bash
  kubectl run nginx --image=nginx --dry-run=client -o yaml
  # Output the YAML for a pod running the nginx image without creating it.
  ```

- **Generate Pod YAML with `restartPolicy: Never`**
  ```bash
  kubectl run nginx --image=nginx --restart=Never --dry-run=client -o yaml
  ```

- **Generate Pod YAML with Labels**
  ```bash
  kubectl run nginx --image=nginx -l="app=web,env=dev" --dry-run=client -o yaml
  ```

- **Generate Pod YAML with Environment Variables**
  ```bash
  kubectl run nginx --image=nginx --env="hello=world" --env="me=naresh" --dry-run=client -o yaml
  ```

- **Generate Pod YAML with Various Parameters**
  ```bash
  kubectl run nginx --image=nginx --restart=OnFailure --env='hello=world' -l='app=web' --limits='cpu=100m,memory=150Mi' --dry-run=client -o yaml
  ```

- **Generate Pod and Service YAML**
  ```bash
  kubectl run nginx --image=nginx --port=80 --expose --dry-run=client -o yaml
  # This also creates a corresponding service object.
  ```
