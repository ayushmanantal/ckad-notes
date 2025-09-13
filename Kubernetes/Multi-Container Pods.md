# Multi-Container Pods

### Definition

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: yellow
spec:
  containers:
  - name: lemon
    image: busybox
    command:
      - sleep
      - "1000"
 
  - name: gold
    image: redis
```

### Getting Logs

```yaml
k –n ns logs pod-name
```

### References

[https://kubernetes.io/docs/tasks/access-application-cluster/communicate-containers-same-pod-shared-volume/](https://kubernetes.io/docs/tasks/access-application-cluster/communicate-containers-same-pod-shared-volume/)