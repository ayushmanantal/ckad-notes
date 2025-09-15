# Rolling Updates and Rollbacks in Kubernetes

📚 [Official Documentation: Rolling Updates](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#rolling-update-deployment)
📚 [Official Documentation: Rollbacks](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#rolling-back-a-deployment)

## Overview

Rolling updates provide a way to update applications to new versions without downtime. Rollbacks allow reverting to previous versions if issues are detected.

## Deployment Strategies

### 1. RollingUpdate (Default)

- Updates pods gradually
- Maintains application availability
- Configurable update parameters
- Zero-downtime deployments

Example RollingUpdate configuration:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1 # Maximum extra pods during update
      maxUnavailable: 1 # Maximum unavailable pods
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:1.16
```

### 2. Recreate Strategy

- Terminates all existing pods before creating new ones
- Results in application downtime
- Useful for applications that don't support multiple versions

Example Recreate configuration:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
spec:
  replicas: 3
  strategy:
    type: Recreate
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
        - name: app
          image: frontend:2.0
```

## Update Commands

### View Rollout Status

```bash
kubectl rollout status deployment/nginx-deployment
```

### Pause/Resume Rollout

```bash
# Pause rollout
kubectl rollout pause deployment/nginx-deployment

# Resume rollout
kubectl rollout resume deployment/nginx-deployment
```

### View Rollout History

```bash
kubectl rollout history deployment/nginx-deployment
kubectl rollout history deployment/nginx-deployment --revision=2
```

### Rollback Commands

```bash
# Rollback to previous version
kubectl rollout undo deployment/nginx-deployment

# Rollback to specific version
kubectl rollout undo deployment/nginx-deployment --to-revision=2
```

## Best Practices

### 1. 🔄 Update Strategy

- Use RollingUpdate for zero-downtime deployments
- Configure appropriate maxSurge and maxUnavailable
- Consider resource requirements during updates

### 2. 📊 Health Checks

- Implement readiness probes
- Set appropriate probe parameters
- Ensure accurate health checking

### 3. ⚡ Resource Management

- Set resource requests and limits
- Plan for temporary resource spikes
- Monitor cluster capacity

### 4. 🔍 Monitoring

- Watch rollout status
- Monitor application metrics
- Set up alerts for failed deployments

### 5. 🔄 Rollback Planning

- Maintain deployment history
- Test rollback procedures
- Document version dependencies

## Common Scenarios

### Progressive Rollout

```yaml
spec:
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0 # Zero downtime
```

### Fast Rollout

```yaml
spec:
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 50% # Scale up to 150%
      maxUnavailable: 0 # No downtime
```

### Conservative Update

```yaml
spec:
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1 # Minimal resource usage
```

## Additional Resources

### Official Documentation

- 📚 [Deployment Concepts](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
- 📚 [Scaling Applications](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#scaling-a-deployment)
- 📚 [Deployment Strategy](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#strategy)

### Troubleshooting

- 📚 [Debug Deployments](https://kubernetes.io/docs/tasks/debug/debug-application/debug-pods/)
- 📚 [Failed Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#failed-deployment)
