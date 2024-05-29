
# Kubernetes Secrets Management

Kubernetes Secrets provide a secure way to store and manage sensitive information, such as passwords, OAuth tokens, and SSH keys. This guide covers the basics of creating, managing, and consuming Secrets within Kubernetes.

## Creating and Consuming a Secret from Literal Values

### Step 1: Create a Secret
Create a Secret to store sensitive information securely:

```bash
# Create a Secret named my-secret
kubectl create secret generic my-secret --from-literal=username=admin --from-literal=password='securePassword123!'

# Verify Secret creation
kubectl get secrets
kubectl describe secret my-secret
```

### Step 2: Consume Secret in a Pod
Define a Pod to use the Secret values as environment variables:

```yaml
# File: my-secret-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-secret-pod
spec:
  containers:
  - name: my-container
    image: nginx
    env:
    - name: USERNAME
      valueFrom:
        secretKeyRef:
          name: my-secret
          key: username
    - name: PASSWORD
      valueFrom:
        secretKeyRef:
          name: my-secret
          key: password
  restartPolicy: Never
```

### Step 3: Validate the Secret
Deploy the Pod and validate the Secret values:

```bash
# Deploy the Pod
kubectl create -f my-secret-pod.yaml

# Check Pod and Secret
kubectl get pods
kubectl get secrets
kubectl describe pod my-secret-pod

# Verify Secret values inside the Pod
kubectl exec my-secret-pod -- env | grep USERNAME
kubectl exec my-secret-pod -- env | grep PASSWORD
exit
```

## Best Practices for Managing Secrets
- **Avoid storing plaintext secrets in your source code.**
- **Restrict access to Secrets using Kubernetes RBAC.**
- **Regularly rotate Secrets and update dependent resources.**
