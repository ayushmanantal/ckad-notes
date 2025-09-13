# Exam Tips

**Setting Context**

```bash
kubectl config set-context --current --namespace=test
```

### Writing the Pod configuration into a file

```bash
kubectl get po elephant -o yaml > elephant.yaml
```

### Forcing a YAML file to change pod configuration

```bash
kubectl replace -f elephant.yaml --force
```

### Getting pods in all namespaces

```bash
k get pods --all-namespaces
```

**Deployment**

```bash
kubectl run nginx--image=nginx
```

**Pod**

```bash
kubect run nginx --image=nginx --restart=Never
```

**Job**

```bash
kubectl run nginx--image=nginx --restart=OnFailure
```

**CronJob**

```bash
kubectl run nginx-image=nginx --restart=OnFailure -schedule="*****"
```

**One** **Liners**

```bash
kubect| run nginx-image=nginx -restart=Never -port=80 -namespace=myname --command --serviceaccount=mysal --env=HOSTNAME=local --labels=bu=finance,env=dev --
requests='cpu=100m,memory=256M' --limits='cpu=200m,memory=512Mi'--dry-run -o yaml -- /bin/sh -c
'echo hello world'

kubectl run frontend --replicas=2 --labels=run=load-balancer-example --image=busybox --port=8080

kubectl expose deployment frontend --type=NodePort -name=frontend-service --port=6262 -target-port=8080

kubectl set serviceaccount deployment frontend myuser

kubectl create service clusterip my-cs --tcp=5678:8080 --dry-run -o yaml
```

**Use** **GREP**

```bash
kubectl describe pods | grep --context=10 annotations:

kubectl describe pods | grep --context=10 Events:
```

### VIM Setup

```bash
set tabstop=2
set expandtab
set shiftwidth=2
```

### VIM Useful Tips

```bash
i — Inserting before the cursor
ZZ — save and exit (equivalent to :wq)
dd — delete the line
V — select lines (you can use arrow sign)
> — indent right (after selecting lines)
< — indent left (after selecting lines)
A — go and edit at end of line
I — got and edit at beginning of line
D — deleting text from the cursir to the end of the selected line
```

## Tip 1

Whenever pod should mount a volume that lasts the lifetime of this pod is mentioned use emptyDir as the volume of the pod.