# ConfigMaps in Kubernetes

📚 [Official Documentation: ConfigMaps](https://kubernetes.io/docs/concepts/configuration/configmap/)

## Overview

ConfigMaps allow you to decouple configuration artifacts from image content to keep containerized applications portable.

## Key Concepts

### 1. Purpose

- Store non-confidential configuration data
- Separate configuration from application code
- Enable application portability

### 2. Use Cases

- Environment variables
- Command-line arguments
- Configuration files in volumes

## Common Operations

### Basic Commands

```bash
# Create ConfigMap
kubectl create configmap app-config --from-literal=APP_COLOR=blue --from-literal=APP_MODE=prod

# List ConfigMaps
kubectl get configmaps

# View ConfigMap details
kubectl describe configmap app-config

# Edit ConfigMap
kubectl edit configmap app-config

# Delete ConfigMap
kubectl delete configmap app-config
```

### ConfigMap Definition

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: game-config
data:
  game.properties: |
    enemies=aliens
    lives=3
    secret.code.passphrase=UUDDLRLRBABAS
  ui.properties: |
    color.good=purple
    color.bad=yellow
```

## Using ConfigMaps

### 1. As Environment Variables

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: config-env-pod
spec:
  containers:
    - name: test-container
      image: nginx
      envFrom:
        - configMapRef:
            name: game-config
```

### 2. As Files in Volume

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: config-vol-pod
spec:
  containers:
    - name: test-container
      image: nginx
      volumeMounts:
        - name: config-volume
          mountPath: /etc/config
  volumes:
    - name: config-volume
      configMap:
        name: game-config
```

### 3. As Single Environment Variable

```yaml
spec:
  containers:
    - env:
        - name: SPECIAL_COLOR
          valueFrom:
            configMapKeyRef:
              name: app-config
              key: APP_COLOR
```

## Best Practices

1. 🔄 **Versioning**

   - Version your ConfigMaps
   - Use labels for tracking
   - Document changes

2. 📦 **Organization**

   - Group related configurations
   - Use meaningful names
   - Keep data format consistent

3. 🔍 **Monitoring**

   - Track ConfigMap usage
   - Monitor mount success
   - Check for errors

4. ⚡ **Performance**
   - Avoid large ConfigMaps
   - Use appropriate update strategy
   - Consider rate limits

## Additional Resources

### Official Documentation

- 📚 [Configure Pods using ConfigMap](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/)
- 📚 [ConfigMap Design](https://kubernetes.io/docs/concepts/configuration/configmap/#configmap-object)

### Troubleshooting

- 📚 [Debug ConfigMaps](https://kubernetes.io/docs/tasks/debug/debug-application/debug-pods/)
