# Kubernetes Configuration (kubeconfig)

📚 [Official Documentation: Configure Access to Multiple Clusters](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/)

## Overview

The kubeconfig file configures access to Kubernetes clusters, defining clusters, users, contexts, and preferences. It's essential for managing multiple clusters and contexts.

## Key Concepts

### 1. File Structure

- Clusters: Define API server endpoints
- Users: Define authentication credentials
- Contexts: Combine cluster and user with namespace
- Current-context: Active context being used

### 2. Default Location

```bash
$HOME/.kube/config  # Default kubeconfig path
```

## Basic Operations

### 1. Viewing Configuration

```bash
# View merged config
kubectl config view

# View current context
kubectl config current-context

# List all contexts
kubectl config get-contexts

# View specific kubeconfig
kubectl config view --kubeconfig=/path/to/config
```

### 2. Context Management

```bash
# Switch context
kubectl config use-context <context-name>

# Create new context
kubectl config set-context <context-name> \
  --cluster=<cluster-name> \
  --user=<user-name> \
  --namespace=<namespace>

# Set namespace for current context
kubectl config set-context --current --namespace=<namespace>

# Using specific kubeconfig
kubectl config --kubeconfig=/root/my-kube-config use-context research
```

## Example Configurations

### 1. Basic kubeconfig

```yaml
apiVersion: v1
kind: Config
current-context: dev-frontend
clusters:
  - name: development
    cluster:
      server: https://dev.example.com
      certificate-authority: dev-ca.crt
contexts:
  - name: dev-frontend
    context:
      cluster: development
      user: developer
      namespace: frontend
users:
  - name: developer
    user:
      client-certificate: dev-cert.crt
      client-key: dev-key.key
```

### 2. Multiple Clusters

```yaml
apiVersion: v1
kind: Config
contexts:
  - name: prod-user
    context:
      cluster: production
      user: prod-admin
      namespace: prod
  - name: dev-user
    context:
      cluster: development
      user: dev-admin
      namespace: dev
```

## Best Practices

1. 🔒 **Security**

   - Protect kubeconfig files
   - Use absolute paths
   - Rotate credentials
   - Use RBAC properly

2. 🎯 **Organization**

   - Use meaningful names
   - Document contexts
   - Maintain clean configs
   - Regular review

3. ⚡ **Usage**

   - Set default namespace
   - Use context aliases
   - Regular backups
   - Version control

4. 📊 **Management**
   - Regular cleanup
   - Audit access
   - Monitor usage
   - Update documentation

## Common Operations

```bash
# Modify current context
kubectl config set current-context <context-name>

# Set credentials
kubectl config set-credentials <user-name> \
  --client-certificate=<path/to/cert> \
  --client-key=<path/to/key>

# Set cluster details
kubectl config set-cluster <cluster-name> \
  --server=https://cluster.example.com \
  --certificate-authority=<path/to/ca>

# Delete context
kubectl config delete-context <context-name>
```

## Additional Resources

### Official Documentation

- 📚 [Organizing Cluster Access](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/)
- 📚 [Managing kubeconfig Files](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/#managing-kubeconfig-files)
- 📚 [Configure Access to Multiple Clusters](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/)

### Security

- 📚 [Authentication](https://kubernetes.io/docs/reference/access-authn-authz/authentication/)
- 📚 [Certificate Management](https://kubernetes.io/docs/tasks/administer-cluster/certificates/)
