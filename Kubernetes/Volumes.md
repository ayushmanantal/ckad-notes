# Volumes

```bash
kubectl exec webapp -- cat /log/app.log
```

`This command executes` cat /log/app.log`inside the`webapp`pod, displaying the contents of the`app.log` file to standard output.

### Creating a volume to store logs

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: webapp
spec:
  containers:
    - name: webapp
      image: kodekloud/event-simulator
      env:
      - name: LOG_HANDLERS
        value: file
      volumeMounts:
      - mountPath: /log
        name: log-volume

  volumes:
    - name: log-volume
      hostPath:
        path: /var/log/webapp
        type: Directory
```