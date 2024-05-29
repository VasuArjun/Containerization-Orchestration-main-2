
# Kubernetes Service Accounts

Service accounts in Kubernetes are used to provide an identity for processes that run in a Pod. These accounts help manage access to the Kubernetes API, ensuring applications can authenticate and perform actions according to set permissions.

## How Service Accounts Work

Service accounts are tied to a namespace and are created either automatically by the Kubernetes API when a namespace is set up or manually as needed. By default, pods that do not specify a service account are assigned the default service account in their namespace.

## Creating a Service Account

You can create a service account using the following command:

```bash
kubectl create serviceaccount [service-account-name] --namespace [namespace]
```

## Using Service Accounts with Pods

To specify a service account for a pod, use the `serviceAccountName` field in the pod manifest:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: my-container
    image: nginx
  serviceAccountName: my-service-account
```

## Permissions and Roles

Permissions are assigned through role-based access control (RBAC) using roles and role bindings.

### Example Role: Reading Secrets

```yaml
kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  namespace: default
  name: secret-reader
rules:
- apiGroups: [""]
  resources: ["secrets"]
  verbs: ["get", "list"]
```

### Example Role Binding

```yaml
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: read-secrets
  namespace: default
subjects:
- kind: ServiceAccount
  name: my-service-account
  namespace: default
roleRef:
  kind: Role
  name: secret-reader
  apiGroup: rbac.authorization.k8s.io
```

## Best Practices

- **Minimal Permissions**: Always grant service accounts the minimal permissions they need to function.
- **Use Distinct Service Accounts**: Avoid using the default service account; create distinct ones for each set of duties.
- **Audit and Rotate Credentials**: Regularly review and rotate service account tokens to maintain security.
