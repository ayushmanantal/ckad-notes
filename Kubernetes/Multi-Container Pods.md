# Multi-Container Pods in Kubernetes

Multi-container pods allow you to run multiple containers within a single pod that share the same network namespace and storage volumes. This pattern is useful for implementing various design patterns like sidecars, ambassadors, and adapters.

📚 [Official Documentation: Multi-Container Pods](https://kubernetes.io/docs/concepts/workloads/pods/#how-pods-manage-multiple-containers)

## Overview

### Common Use Cases

1. 🔄 Sidecar Pattern: Helper containers that support the main application
2. 🌐 Ambassador Pattern: Proxy containers that handle network connections
3. 🔄 Adapter Pattern: Containers that transform output or protocol
4. 📊 Logging and Monitoring: Containers that collect metrics or logs

### Key Characteristics

- Shared network (localhost)
- Shared storage volumes
- Same lifecycle management
- Co-located and co-scheduled

## Pod Configurations

### 1. Basic Multi-Container Setup

Example of a basic multi-container pod:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: yellow
spec:
  containers:
    - name: lemon # First container
      image: busybox
      command: # Keep container running
        - sleep
        - "1000"

    - name: gold # Second container
      image: redis # Redis server container
```

### 2. Sidecar Pattern Example

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: web-with-logger
spec:
  containers:
    - name: web
      image: nginx
      volumeMounts:
        - name: shared-logs
          mountPath: /var/log/nginx

    - name: logger # Sidecar container
      image: busybox
      command: ["sh", "-c", "tail -f /var/log/nginx/access.log"]
      volumeMounts:
        - name: shared-logs
          mountPath: /var/log/nginx

  volumes:
    - name: shared-logs # Shared volume
      emptyDir: {}
```

### 3. Ambassador Pattern Example

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: redis-proxy
spec:
  containers:
    - name: redis
      image: redis

    - name: proxy # Ambassador container
      image: envoyproxy/envoy
      ports:
        - containerPort: 1234
```

## Managing Multi-Container Pods

### Viewing Container Logs

```bash
# View logs of a specific container
kubectl logs pod-name -c container-name

# View logs of all containers in the pod
kubectl logs pod-name --all-containers

# View logs with namespace
kubectl -n namespace-name logs pod-name -c container-name
```

### Executing Commands

```bash
# Execute command in specific container
kubectl exec pod-name -c container-name -- command

# Get interactive shell
kubectl exec -it pod-name -c container-name -- /bin/sh
```

## Container Communication

### Inter-Container Communication

- Containers can communicate via `localhost`
- Shared volumes enable file-based communication
- Process signals are isolated between containers

Example of containers sharing a volume:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: shared-data
spec:
  containers:
    - name: container1
      image: nginx
      volumeMounts:
        - name: shared-data
          mountPath: /data

    - name: container2
      image: busybox
      volumeMounts:
        - name: shared-data
          mountPath: /data

  volumes:
    - name: shared-data
      emptyDir: {}
```

## Best Practices

1. 🎯 Keep containers focused on single responsibilities
2. 📊 Use appropriate health checks for each container
3. 🔄 Consider container startup order
4. 💾 Plan shared resource usage
5. 🔍 Monitor resource consumption per container

## Additional Resources

### Official Documentation

- 📚 [Pod Design Patterns](https://kubernetes.io/docs/concepts/workloads/pods/patterns/)
- 📚 [Inter-Container Communication](https://kubernetes.io/docs/tasks/access-application-cluster/communicate-containers-same-pod-shared-volume/)
- 📚 [Init Containers](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/)

### Design Patterns

- 📚 [Sidecar Pattern](https://kubernetes.io/docs/concepts/workloads/pods/patterns/#sidecar-containers)
- 📚 [Ambassador Pattern](https://kubernetes.io/docs/concepts/workloads/pods/patterns/#ambassador-containers)
- 📚 [Adapter Pattern](https://kubernetes.io/docs/concepts/workloads/pods/patterns/#adapter-containers)
