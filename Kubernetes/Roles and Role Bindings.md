# Kubernetes RBAC: Roles and Role Bindings

Role-Based Access Control (RBAC) in Kubernetes manages authorization through Roles and RoleBindings. This system allows fine-grained control over who can access what resources within a namespace.

📚 [Official Documentation: RBAC Authorization](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)

## Overview

### Key Components

1. **Roles**: Define permissions to resources within a namespace
2. **RoleBindings**: Link roles to users, groups, or service accounts
3. **Verbs**: Actions that can be performed (get, list, create, update, delete, etc.)
4. **Resources**: Kubernetes objects that can be accessed (pods, services, etc.)

### RBAC Scope

- Roles and RoleBindings work within a namespace
- For cluster-wide permissions, use ClusterRoles and ClusterRoleBindings

## Basic Operations

### Viewing RBAC Resources

```bash
# List all roles in current namespace
kubectl get roles

# List all role bindings
kubectl get rolebindings

# List roles in specific namespace
kubectl get roles -n kube-system
```

### Inspecting RBAC Configuration

```bash
# Describe role binding details
kubectl describe rolebinding kube-proxy -n kube-system

# View API server RBAC configuration
kubectl describe pod kube-apiserver-controlplane -n kube-system
```

## Role Management

### 1. Creating Roles

Basic role creation with specific permissions:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: pod-reader
  namespace: default
spec:
  rules:
    - apiGroups: [""] # "" indicates the core API group
      resources: ["pods"]
      verbs: ["get", "watch", "list"]
```

Using imperative commands:

```bash
# Create role with specific permissions
kubectl create role developer \
  --namespace=default \
  --verb=list,create,delete \
  --resource=pods

# Create role with resource names
kubectl create role configmap-updater \
  --verb=update \
  --resource=configmaps \
  --resource-name=my-configmap
```

### 2. Creating RoleBindings

Binding a role to users or service accounts:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-pods
  namespace: default
spec:
  subjects:
    - kind: User
      name: jane # Name of the user
      apiGroup: rbac.authorization.k8s.io
  roleRef:
    kind: Role
    name: pod-reader # Name of the Role
    apiGroup: rbac.authorization.k8s.io
```

Using imperative commands:

```bash
# Bind role to user
kubectl create rolebinding dev-user-binding \
  --namespace=default \
  --role=developer \
  --user=dev-user

# Bind role to service account
kubectl create rolebinding sa-pod-reader \
  --role=pod-reader \
  --serviceaccount=default:my-sa
```

### 3. Verifying Access

Check permissions for current or other users:

```bash
# Check if current user can create deployments
kubectl auth can-i create deployments

# Check if specific user can delete nodes
kubectl auth can-i delete nodes --as dev-user

# Check namespace-specific permissions
kubectl auth can-i list pods --namespace production
```

## Common Role Configurations

### 1. Pod Management Role

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: pod-manager
rules:
  - apiGroups: [""]
    resources: ["pods"]
    verbs: ["get", "list", "create", "update", "delete"]
```

### 2. Deployment Management Role

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: deployment-manager
rules:
  - apiGroups: ["apps"]
    resources: ["deployments"]
    verbs: ["get", "list", "create", "update", "delete"]
```

## Best Practices

1. 🔒 Follow principle of least privilege
2. 📝 Document role permissions clearly
3. 🔍 Regularly audit role bindings
4. 🎯 Use specific permissions over wildcards
5. 🔄 Review and update roles periodically

## Additional Resources

### Official Documentation

- 📚 [Using RBAC Authorization](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)
- 📚 [Default Roles and Role Bindings](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#default-roles-and-role-bindings)
- 📚 [Aggregated ClusterRoles](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#aggregated-clusterroles)

### Security Guidelines

- 📚 [RBAC Good Practices](https://kubernetes.io/docs/concepts/security/rbac-good-practices/)
- 📚 [Authorization Overview](https://kubernetes.io/docs/reference/access-authn-authz/authorization/)
