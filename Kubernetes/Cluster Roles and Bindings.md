# Cluster Roles and Role Bindings in Kubernetes

📚 [Official Documentation: RBAC Authorization](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)

## Overview
Role-Based Access Control (RBAC) in Kubernetes uses Roles and ClusterRoles to define permissions, and RoleBindings and ClusterRoleBindings to assign these permissions to users.

## Key Concepts

### 1. RBAC Components
- ClusterRoles: Cluster-wide permission sets
- ClusterRoleBindings: Cluster-wide permission assignments
- Roles: Namespace-scoped permission sets
- RoleBindings: Namespace-scoped permission assignments

### 2. Key Differences
- Roles/RoleBindings: Namespace-specific
- ClusterRoles/ClusterRoleBindings: Cluster-wide

## ClusterRole Examples

### 1. Basic Node Management Role
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: node-admin
rules:
- apiGroups: [""]           # Core API group
  resources: ["nodes"]      # Resource type
  verbs:                    # Allowed actions
    - "get"                 # Read single node
    - "watch"              # Watch for changes
    - "list"               # List all nodes
    - "create"             # Create nodes
    - "delete"             # Delete nodes
```

### 2. Advanced Multi-Resource Role
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: cluster-monitor
rules:
- apiGroups: [""]
  resources: ["nodes", "pods", "services"]
  verbs: ["get", "list", "watch"]
- apiGroups: ["apps"]
  resources: ["deployments", "daemonsets"]
  verbs: ["get", "list", "watch"]
```

## ClusterRoleBinding Examples

### 1. Basic Admin Binding
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-binding
subjects:                          # Who gets the permissions
- kind: User                       # User type (User, Group, ServiceAccount)
  name: admin-user                 # Username
  apiGroup: rbac.authorization.k8s.io
roleRef:                           # What permissions they get
  kind: ClusterRole               # Type of role
  name: cluster-admin             # Name of role
  apiGroup: rbac.authorization.k8s.io
```

### 2. Group Binding
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: read-only-binding
subjects:
- kind: Group
  name: readers
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: view
  apiGroup: rbac.authorization.k8s.io
```

## Common Operations

```bash
# Create ClusterRole
kubectl create clusterrole pod-reader --verb=get,list,watch --resource=pods

# Create ClusterRoleBinding
kubectl create clusterrolebinding pod-reader-binding --clusterrole=pod-reader --user=jane

# View ClusterRoles
kubectl get clusterroles

# View ClusterRoleBindings
kubectl get clusterrolebindings

# Describe ClusterRole
kubectl describe clusterrole admin
```

## Best Practices

1. 🔒 **Security**
   - Follow least privilege principle
   - Regularly audit permissions
   - Document role assignments
   - Use groups for better management

2. 🎯 **Role Design**
   - Keep roles focused
   - Use aggregated roles
   - Version control definitions
   - Regular review cycles

3. ⚡ **Performance**
   - Minimize number of bindings
   - Use groups over individual bindings
   - Cache RBAC decisions
   - Monitor API server load

4. 📊 **Monitoring**
   - Audit role changes
   - Track binding updates
   - Monitor access patterns
   - Log authorization decisions

## Additional Resources

### Official Documentation
- 📚 [Using RBAC Authorization](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)
- 📚 [Default Roles and Role Bindings](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#default-roles-and-role-bindings)
- 📚 [Aggregated ClusterRoles](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#aggregated-clusterroles)

### Security
- 📚 [Authorization Overview](https://kubernetes.io/docs/reference/access-authn-authz/authorization/)
- 📚 [Authentication Overview](https://kubernetes.io/docs/reference/access-authn-authz/authentication/)
```