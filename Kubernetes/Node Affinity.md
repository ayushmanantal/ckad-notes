# Node Affinity in Kubernetes

Node Affinity provides advanced ways to constrain pod placement on specific nodes based on node labels. It's more expressive than nodeSelector and offers more flexibility in pod scheduling.

📚 [Official Documentation: Node Affinity](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#node-affinity)

## Overview

### Types of Node Affinity

1. `requiredDuringSchedulingIgnoredDuringExecution`
   - Hard requirement - pods won't be scheduled if not met
   - Existing pods continue to run if labels change
2. `preferredDuringSchedulingIgnoredDuringExecution`
   - Soft requirement - scheduler tries to find matching nodes
   - Falls back to other nodes if not possible

### Node Selection Operators

- `In`: Label value must be in the specified set
- `NotIn`: Label value must not be in the specified set
- `Exists`: Label key must exist
- `DoesNotExist`: Label key must not exist
- `Gt`: Label value must be greater than specified value
- `Lt`: Label value must be less than specified value

## Node Labeling

First, label your nodes to enable affinity rules:

```bash
# Add a label to a node
kubectl label node node01 color=blue

# Verify node labels
kubectl get nodes --show-labels

# Remove a label
kubectl label node node01 color-
```

## Affinity Configurations

### 1. Required Node Affinity

Pods will only be scheduled on nodes that match these rules:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: blue
  labels:
    app: blue
spec:
  replicas: 3
  selector:
    matchLabels:
      app: blue
  template:
    metadata:
      labels:
        app: blue
    spec:
      containers:
        - name: blue
          image: nginx
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms: # List of terms - OR'd together
              - matchExpressions: # List of expressions - AND'd together
                  - key: color # Node label key to match
                    operator: In # Operator for matching
                    values: # Acceptable values
                      - blue
```

### 2. Preferred Node Affinity

Scheduler will try to honor these rules but may place pods elsewhere:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-preferred
spec:
  containers:
    - name: nginx
      image: nginx
  affinity:
    nodeAffinity:
      preferredDuringSchedulingIgnoredDuringExecution:
        - weight: 1 # Weight for this preference (1-100)
          preference:
            matchExpressions:
              - key: disk-type
                operator: In
                values:
                  - ssd
```

### 3. Multiple Rules Example

Combining multiple affinity rules:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: multi-affinity
spec:
  containers:
    - name: nginx
      image: nginx
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
          - matchExpressions:
              - key: kubernetes.io/os
                operator: In
                values:
                  - linux
      preferredDuringSchedulingIgnoredDuringExecution:
        - weight: 1
          preference:
            matchExpressions:
              - key: disk-speed
                operator: Gt
                values:
                  - "7000"
```

## Best Practices

1. 🎯 Use required affinity for critical requirements
2. 🔄 Use preferred affinity for optimization
3. 📊 Consider node maintenance implications
4. 🔍 Regular monitoring of node labels
5. 📝 Document affinity rules clearly

## Common Use Cases

1. Hardware Requirements

```yaml
nodeAffinity:
  requiredDuringSchedulingIgnoredDuringExecution:
    nodeSelectorTerms:
      - matchExpressions:
          - key: gpu-type
            operator: In
            values:
              - nvidia-tesla-p100
```

2. Geographical Distribution

```yaml
nodeAffinity:
  preferredDuringSchedulingIgnoredDuringExecution:
    - weight: 1
      preference:
        matchExpressions:
          - key: region
            operator: In
            values:
              - us-west
```

## Additional Resources

### Official Documentation

- 📚 [Advanced Scheduling](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/)
- 📚 [Node Selection](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#node-affinity)
- 📚 [Well-Known Labels](https://kubernetes.io/docs/reference/labels-annotations-taints/)

### Advanced Topics

- 📚 [Topology Spread Constraints](https://kubernetes.io/docs/concepts/workloads/pods/pod-topology-spread-constraints/)
- 📚 [Pod Scheduling](https://kubernetes.io/docs/concepts/scheduling-eviction/pod-scheduling-readiness/)
- 📚 [Resource Management](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/)
