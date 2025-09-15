# Kubernetes Ingress

📚 [Official Documentation: Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/)

## Overview

Ingress exposes HTTP and HTTPS routes from outside the cluster to services within the cluster. Traffic routing is controlled by rules defined on the Ingress resource.

## Key Concepts

### 1. Ingress Components

- Ingress Resource: Defines routing rules
- Ingress Controller: Implements the rules (e.g., NGINX)
- Backend Services: Target services for traffic

### 2. Common Features

- Path-based routing
- Host-based routing
- TLS/SSL termination
- Name-based virtual hosting
- Load balancing

## Basic Ingress Example

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: test-ingress
  namespace: critical-space
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: / # URL rewriting
    nginx.ingress.kubernetes.io/ssl-redirect: "false" # Disable SSL redirect
spec:
  rules:
    - http:
        paths:
          - path: /pay # URL path to match
            pathType: Prefix # Path matching type
            backend:
              service:
                name: pay-service # Target service
                port:
                  number: 8282 # Service port
```

## Advanced Examples

### 1. Multi-Path Routing

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-wear-watch
  namespace: app-space
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: / # Rewrite the URL path
    nginx.ingress.kubernetes.io/ssl-redirect: "false" # Disable HTTPS redirect
spec:
  rules:
    - http:
        paths:
          - path: /wear # First path
            pathType: Prefix # Match path prefix
            backend:
              service:
                name: wear-service # Target service for /wear
                port:
                  number: 8080
          - path: /watch # Second path
            pathType: Prefix
            backend:
              service:
                name: video-service # Target service for /watch
                port:
                  number: 8080
```

### 2. NGINX Ingress Controller Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: ingress-nginx-controller
  namespace: ingress-nginx
  labels:
    app.kubernetes.io/component: controller
    app.kubernetes.io/name: ingress-nginx
spec:
  type: NodePort # Expose via NodePort
  ports:
    - port: 80 # Service port
      protocol: TCP
      targetPort: 80 # Container port
      nodePort: 30080 # External port
  selector:
    app.kubernetes.io/component: controller
    app.kubernetes.io/name: ingress-nginx
```

## Common Configurations

### 1. Host-Based Routing

```yaml
spec:
  rules:
    - host: foo.example.com # Route based on hostname
      http:
        paths:
          - path: /
            backend:
              service:
                name: foo-service
                port:
                  number: 80
```

### 2. TLS Configuration

```yaml
spec:
  tls:
    - hosts:
        - secure.example.com
      secretName: tls-secret # TLS certificate secret
  rules:
    - host: secure.example.com
      http:
        paths:
          - path: /
            backend:
              service:
                name: secure-service
                port:
                  number: 443
```

## Best Practices

1. 🔒 **Security**

   - Enable TLS where possible
   - Use appropriate annotations
   - Control access with authentication
   - Regular certificate rotation

2. 🎯 **Configuration**

   - Use meaningful path names
   - Document rewrite rules
   - Monitor resource usage
   - Plan for scaling

3. ⚡ **Performance**

   - Configure appropriate timeouts
   - Enable compression when needed
   - Use caching appropriately
   - Monitor backend health

4. 📊 **Monitoring**
   - Track ingress metrics
   - Monitor error rates
   - Set up alerting
   - Log important events

## Common Operations

```bash
# List all ingresses
kubectl get ingress --all-namespaces

# Describe ingress
kubectl describe ingress <ingress-name>

# Get ingress details
kubectl get ingress <ingress-name> -o yaml

# Delete ingress
kubectl delete ingress <ingress-name>
```

## Additional Resources

### Official Documentation

- 📚 [Ingress Controllers](https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/)
- 📚 [Ingress TLS](https://kubernetes.io/docs/concepts/services-networking/ingress/#tls)
- 📚 [NGINX Ingress Controller](https://kubernetes.github.io/ingress-nginx/)

### Troubleshooting

- 📚 [Debug Ingress](https://kubernetes.io/docs/tasks/debug/debug-application/debug-service/)
- 📚 [Common Issues](https://kubernetes.github.io/ingress-nginx/troubleshooting/)

```

```
