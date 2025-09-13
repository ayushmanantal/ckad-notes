# Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: webapp-service
  namespace: default
spec:
  type: NodePort
  selector:
    name: simple-webapp
  ports:
    - port: 8080
      targetPort: 8080
      nodePort: 30080 
```