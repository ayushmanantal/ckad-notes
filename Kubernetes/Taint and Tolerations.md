# Taints and Tolerations in Kubernetes

📚 [Official Documentation: Taints and Tolerations](https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/)

## Overview

Taints and tolerations work together to ensure pods are not scheduled on inappropriate nodes. Taints allow a node to repel pods, while tolerations allow pods to schedule onto nodes with matching taints.

## Key Concepts

### 1. Taints

- Applied to nodes
- Prevent pods from scheduling
- Have three effects: NoSchedule, PreferNoSchedule, NoExecute
- Format: `key=value:effect`

### 2. Tolerations

- Applied to pods
- Allow (but don't require) pods to schedule on tainted nodes
- Must match taint's key, value, and effect

## Taint Effects

1. **NoSchedule**
   - Pods won't be scheduled on the node
   - Existing pods remain
2. **PreferNoSchedule**

   - Soft version of NoSchedule
   - System will try to avoid placing pods
   - Will place if no other option exists

3. **NoExecute**
   - New pods won't be scheduled
   - Existing pods will be evicted
   - Can specify tolerationSeconds

## Working with Taints

### Checking Node Taints

```bash
# View all taints on a node
kubectl describe node <node-name> | grep -i taints

# Get taint details for all nodes
kubectl get nodes -o custom-columns=NAME:.metadata.name,TAINTS:.spec.taints
```

### Managing Taints

```bash
# Add a taint to a node
kubectl taint nodes <node-name> key=value:NoSchedule

# Remove a taint from a node (note the minus sign at the end)
kubectl taint nodes <node-name> key=value:NoSchedule-

# Example: Add production taint
kubectl taint nodes node01 environment=production:NoSchedule

# Example: Remove control-plane taint
kubectl taint nodes controlplane node-role.kubernetes.io/control-plane:NoSchedule-
```

## Pod Tolerations

### Basic Toleration

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
    - name: nginx
      image: nginx
  tolerations:
    - key: "environment" # Match taint key
      operator: "Equal" # Equal or Exists
      value: "production" # Match taint value
      effect: "NoSchedule" # Match taint effect
```

### Advanced Toleration Examples

1. **Tolerating Multiple Values**

```yaml
spec:
  tolerations:
    - key: "environment"
      operator: "Exists" # Matches any value for this key
      effect: "NoSchedule"
```

2. **Toleration with Time Limit**

```yaml
spec:
  tolerations:
    - key: "node.kubernetes.io/not-ready"
      operator: "Exists"
      effect: "NoExecute"
      tolerationSeconds: 300 # Evict after 5 minutes
```

## Common Use Cases

### 1. Dedicated Nodes

```yaml
# Taint nodes for specific workloads
kubectl taint nodes node1 dedicated=gpu:NoSchedule

# Pod that can use GPU node
apiVersion: v1
kind: Pod
metadata:
  name: gpu-pod
spec:
  tolerations:
  - key: "dedicated"
    value: "gpu"
    effect: "NoSchedule"
```

### 2. Special Hardware

```yaml
# Taint node with special hardware
kubectl taint nodes node2 special=true:NoSchedule

# Pod requiring special hardware
apiVersion: v1
kind: Pod
metadata:
  name: special-app
spec:
  tolerations:
  - key: "special"
    value: "true"
    effect: "NoSchedule"
```

## Best Practices

1. 🎯 **Use Cases**

   - Dedicate nodes for specific workloads
   - Ensure critical workloads isolation
   - Handle special hardware requirements

2. 🔒 **Security**

   - Use taints for security boundaries
   - Control access to sensitive nodes
   - Document taint purposes

3. ⚖️ **Balance**

   - Don't over-use taints
   - Consider node affinity as alternative
   - Plan for maintenance scenarios

4. 📝 **Documentation**
   - Document taint purposes
   - Track toleration usage
   - Maintain taint standards

## Additional Resources

### Official Documentation

- 📚 [Advanced Scheduling](https://kubernetes.io/docs/concepts/scheduling-eviction/)
- 📚 [Node Selection](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/)
- 📚 [Pod Priority and Preemption](https://kubernetes.io/docs/concepts/scheduling-eviction/pod-priority-preemption/)

### Troubleshooting

- 📚 [Scheduling Debug](https://kubernetes.io/docs/tasks/debug/debug-application/debug-pod-replication-controller/)
- 📚 [Pod Scheduling Issues](https://kubernetes.io/docs/concepts/scheduling-eviction/scheduling-framework/)
