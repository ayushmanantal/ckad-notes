# Readiness Probes

### Readiness Probe

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp-2
spec:
  containers:
  - name: simple-webapp-2
    image: kodekloud/webapp-delayed-start
    ports:
    - containerPort: 8080
      protocol: TCP
    readinessProbe:
      httpGet:
        path: /ready
        port: 8080
```

### Liveness Probe

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp-2
spec:
  containers:
  - name: simple-webapp-2
    image: kodekloud/webapp-delayed-start
    ports:
    - containerPort: 8080
      protocol: TCP
    livenessProbe:
      httpGet:
        path: /live
        port: 8080
      initialDelaySeconds: 80
      periodSeconds: 1
```