# Labels and Selectors in Kubernetes

Labels are key-value pairs attached to Kubernetes objects for organization and selection. Selectors help you identify and manage groups of resources based on these labels.

📚 [Official Documentation: Labels and Selectors](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/)

## Overview

### Labels

- 🏷️ Identify and organize Kubernetes resources
- 🔄 Support operations on multiple resources
- 🎯 Enable service routing and pod scheduling
- 📊 Group resources for management

### Selectors

- 🔍 Filter resources based on labels
- 🎯 Target specific resource groups
- 🔀 Support equality or set-based conditions
- 📋 Used in various Kubernetes objects (Services, Deployments, etc.)

## Common Label Operations

### Basic Selector Examples

```bash
# Select pods with environment=development
k get pods --selector env=dev

# Select pods from finance business unit
k get pods --selector bu=finance

# Select all resources in production
k get all --selector env=prod

# Multiple label selectors (AND condition)
k get pods --selector env=prod,bu=finance,tier=frontend
```

## Label Management

### Adding Labels

```bash
# Add label to a pod
kubectl label pod nginx-pod env=prod

# Add label to a node
kubectl label node worker-node gpu=true

# Add label to multiple pods
kubectl label pods -l app=nginx tier=frontend
```

### Updating Labels

```bash
# Override existing label
kubectl label pod nginx-pod env=staging --overwrite

# Remove a label
kubectl label pod nginx-pod env-
```

## Selector Types

### Equality-Based Selectors

Match resources with exact label values:

```yaml
selector:
  component: redis
  tier: backend
```

### Set-Based Selectors

More flexible matching using operators:

```yaml
selector:
  matchExpressions:
    - { key: tier, operator: In, values: [cache, backend] }
    - { key: environment, operator: NotIn, values: [dev] }
```

## Common Use Cases

### 1. Service Selection

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx # Routes traffic to pods with label app=nginx
  ports:
    - port: 80
```

### 2. Node Selection

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  nodeSelector:
    gpu: "true" # Schedule only on nodes with gpu=true
  containers:
    - name: nginx
      image: nginx
```

### 3. Deployment Selection

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx # Pods created will have this label
```

## Best Practices

1. 🏷️ Use meaningful label names and values
2. 📝 Document your labeling scheme
3. 🎯 Keep labels simple and relevant
4. 🔄 Use consistent naming conventions
5. 🔍 Avoid unnecessary labels

## Recommended Labels

Kubernetes recommends these common labels:

```yaml
app.kubernetes.io/name: mysql
app.kubernetes.io/instance: mysql-abcxzy
app.kubernetes.io/version: "5.7.21"
app.kubernetes.io/component: database
app.kubernetes.io/part-of: wordpress
app.kubernetes.io/managed-by: helm
```

## Additional Resources

### Official Documentation

- 📚 [Recommended Labels](https://kubernetes.io/docs/concepts/overview/working-with-objects/common-labels/)
- 📚 [Selecting with kubectl](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/#label-selectors)
- 📚 [Node Selection](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/)

### Label Strategy

- 📚 [Label Strategy Documentation](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/#syntax-and-character-set)
- 📚 [Service Topology](https://kubernetes.io/docs/concepts/services-networking/service-topology/)
