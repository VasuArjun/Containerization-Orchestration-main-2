
# Kubernetes ConfigMaps and Secrets Management

## ConfigMaps
ConfigMaps allow you to decouple configuration artifacts from image content to keep containerized applications portable. They are used to store non-sensitive configuration data in key-value pairs. This could include environment variables, command-line arguments, configuration files, etc.

### Creating a ConfigMap
Here's an example of how you can create a ConfigMap:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: example-config
data:
  key1: value1
  key2: value2
```

### Using a ConfigMap in a Pod
To use this ConfigMap in a pod:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: example-pod
spec:
  containers:
    - name: example-container
      image: nginx
      env:
        - name: KEY1
          valueFrom:
            configMapKeyRef:
              name: example-config
              key: key1
```

In this example, the example-pod pod has an environment variable KEY1 whose value is taken from the example-config ConfigMap's key1 entry.

## Secrets
Secrets are like ConfigMaps, but are specifically designed to store sensitive information such as passwords, OAuth tokens, SSH keys, etc. They are base64 encoded by default, but are not encrypted.

### Creating a Secret
Here's how you can create a Secret:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: example-secret
type: Opaque
data:
  username: YWRtaW4=  # base64 encoded value
  password: MWYyZDFlMmU2N2Rm  # base64 encoded value
```

### Using a Secret in a Pod
Using a Secret in a pod:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: example-pod
spec:
  containers:
    - name: example-container
      image: nginx
      env:
        - name: DB_USERNAME
          valueFrom:
            secretKeyRef:
              name: example-secret
              key: username
        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: example-secret
              key: password
```

In this example, DB_USERNAME and DB_PASSWORD environment variables are populated from the example-secret Secret.

### Best Practices
- Always use Secrets for sensitive information and ConfigMaps for non-sensitive configuration data.
- Be cautious about granting access to these resources as they might contain sensitive information.
