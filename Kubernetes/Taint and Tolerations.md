# Taint and Tolerations

### Checking Taints

```bash
kubectl describe node controlplane | grep -i taints
```

### Adding a Taint

```bash
kubectl taint nodes node1 key1=value1:NoSchedule
```

### Another Example

```bash
kubectl taint nodes node01 spray=mortein:NoSchedule
```

### Removing a Taint

```bash
kubectl taint nodes node1 key1=value1:NoSchedule
```

### Definition

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: bee
spec:
  containers:
  - name: bee
    image: nginx
  tolerations:
  - key: spray
    value: mortein
    operator: Equal
    effect: NoSchedule
```

```bash
k taint nodes controlplane node-role.kubernetes.io/control-plane:NoSchedule-
```