# Kubernetes Services

📚 [Official Documentation: Services](https://kubernetes.io/docs/concepts/services-networking/service/)

## Overview

Services in Kubernetes provide a way to expose applications running in Pods as network services. They enable communication between different components within and outside of the application.

## Service Types

### 1. ClusterIP (Default)

- Internal service within the cluster
- Pods can reach the service using the cluster IP
- Default type if none specified

```yaml
apiVersion: v1
kind: Service
metadata:
  name: backend-service
spec:
  type: ClusterIP
  selector:
    app: backend
  ports:
    - port: 80 # Port exposed to the cluster
      targetPort: 8080 # Port the container accepts traffic on
```

### 2. NodePort

- Exposes the service on each node's IP at a static port
- Accessible from outside the cluster
- Port range: 30000-32767

```yaml
apiVersion: v1
kind: Service
metadata:
  name: webapp-service
spec:
  type: NodePort
  selector:
    app: webapp
  ports:
    - port: 80 # Service port
      targetPort: 8080 # Container port
      nodePort: 30080 # Port on the node
```

### 3. LoadBalancer

- Exposes service externally using cloud provider's load balancer
- Automatically creates NodePort and ClusterIP services

```yaml
apiVersion: v1
kind: Service
metadata:
  name: frontend-service
spec:
  type: LoadBalancer
  selector:
    app: frontend
  ports:
    - port: 80
      targetPort: 8080
```

### 4. ExternalName

- Maps service to external DNS name
- No proxying or port mapping

```yaml
apiVersion: v1
kind: Service
metadata:
  name: external-service
spec:
  type: ExternalName
  externalName: api.example.com
```

## Service Features

### 1. Selectors

```yaml
spec:
  selector:
    app: myapp # Match pods with label app=myapp
    tier: frontend # Multiple labels for more specific selection
```

### 2. Multi-Port Services

```yaml
spec:
  ports:
    - name: http # Name required when multiple ports
      port: 80
      targetPort: 8080
    - name: https
      port: 443
      targetPort: 8443
```

### 3. Headless Services

For direct pod-to-pod communication:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: headless-service
spec:
  clusterIP: None # Makes the service headless
  selector:
    app: database
  ports:
    - port: 5432
```

## Common Use Cases

### 1. Internal Microservices

```yaml
apiVersion: v1
kind: Service
metadata:
  name: backend-api
spec:
  type: ClusterIP
  selector:
    app: backend
    component: api
  ports:
    - port: 8080
      targetPort: 8080
```

### 2. Public Web Application

```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-frontend
spec:
  type: LoadBalancer
  selector:
    app: frontend
  ports:
    - port: 80
      targetPort: 3000
```

## Service Discovery

### 1. Environment Variables

- Automatically injected into pods
- Format: SERVICE_NAME_SERVICE_HOST and SERVICE_NAME_SERVICE_PORT

### 2. DNS

- Internal domain name format: service-name.namespace.svc.cluster.local
- Accessible within cluster

## Best Practices

1. 🎯 **Naming and Labels**

   - Use meaningful service names
   - Consistent labeling scheme
   - Document service purpose

2. 🔒 **Security**

   - Limit external exposure
   - Use appropriate service type
   - Implement network policies

3. ⚡ **Performance**

   - Choose appropriate service type
   - Consider session affinity
   - Monitor service health

4. 📊 **Monitoring**
   - Track service metrics
   - Monitor endpoints
   - Set up alerts

## Troubleshooting Commands

```bash
# List all services
kubectl get services

# Get detailed service info
kubectl describe service <service-name>

# Test service DNS
kubectl run test-pod --image=busybox -it --rm -- nslookup <service-name>

# Check service endpoints
kubectl get endpoints <service-name>
```

## Additional Resources

### Official Documentation

- 📚 [Service Types](https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services-service-types)
- 📚 [DNS for Services and Pods](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/)
- 📚 [Connecting Applications with Services](https://kubernetes.io/docs/concepts/services-networking/connect-applications-service/)

### Troubleshooting

- 📚 [Debug Services](https://kubernetes.io/docs/tasks/debug/debug-application/debug-service/)
- 📚 [Service Network Configuration](https://kubernetes.io/docs/concepts/services-networking/service/#virtual-ips-and-service-proxies)
