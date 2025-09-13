# Short Commands

```bash
#Creating a Pod and namespace together
kubectl run nginx --image=nginx --restart=Never --dry-run=client -o yaml | kubectl create -n mynamespace -f -
```

### Pods and Logs

```bash

# -it will help in seeing the output, --rm will immediately delete the pod after it exits
kubectl run busybox --image=busybox --command --restart=Never -it --rm -- env 

# or, just run it without -it
kubectl run busybox --image=busybox --command --restart=Never -- env

# and then, check its logs
kubectl logs busybox
```

### Creating a Resource Quota

```bash
kubectl create quota myrq --hard=cpu=1,memory=1G,pods=2 --dry-run=client -o yaml
```

### Setting a different image tag to the Pod

```
# kubectl set image POD/POD_NAME CONTAINER_NAME=IMAGE_NAME:TAG
kubectl set image pod/nginx nginx=nginx:1.24.0
```

```bash
# get the IP, will be something like '10.1.1.131'
kubectl get po -o wide

# create a temp busybox pod
kubectl run busybox --image=busybox --rm -it --restart=Never -- wget -O- 10.1.1.131:80

# If pod crashed and restarted, get logs about the previous instance
kubectl logs nginx -p
# or
kubectl logs nginx --previous

# Shell on Pod nginx
kubectl exec -it nginx -- /bin/sh

# Running a busybox image to call another container 
k run busybox --image=busybox --restart=Never -it --rm -- /bin/sh -c "wget -O- 10.244.1.178"

# Changing the label of a pod
kubectl label po nginx2 app=v2 --overwrite

# Specifically selecting a pod with v2 label
k get po --selector=app=v2

#Add a new label tier=web to all pods having 'app=v2' or 'app=v1' labels
k label po -l "app in(v1,v2)" tier=web

#Removing labels
k label po nginx, nginx1, nginx2, nginx3 app-

# Annotating pods
k annotate po nginx1 nginx2 nginx3 description='mydescription'

# Checking annotations for a pod
k annotate po nginx1 --list

# Removing annotations from the pods
k annotate po nginx1 nginx2 nginx3 description- owner-

# Adding a label to a node
kubectl label nodes <your-node-name> accelerator=nvidia-tesla-p100

# Tainting a node key=value:Effect
kubectl taint node node1 tier=frontend:NoSchedule

# View the taints on a node
kubectl describe node node1 

# Creating a new deployment
kubectl create deploy nginx --image=nginx:1.18.0 --replicas=2 --port=80

# Checking the deployment rollout status
kubectl rollout status deploy nginx

# Updating the nginx image for the deployment nginx
kubectl set image deploy nginx nginx=nginx:1.19.8

# Checking rollout history for a deployment
kubectl rollout history deploy nginx

# Rolling back to a specifc revision number 
k rollout undo deployment nginx --to-revision=2

# Checking a specific revision of rollout
kubectl rollout history deploy nginx --revision=4

# Scaling a deployment to 5 replicas
kubectl scale deploy nginx --replicas=5

# Autoscaling a deployment to 5 minimum, 10 maximum replicas and 80% cpu 
kubectl autoscale deploy nginx --min=5 --max=10 --cpu-percent=80

# View the horizontalpodautoscalers.autoscaling for nginx
kubectl get hpa nginx

# Pausing the rollout of a deployment
kubectl rollout pause deploy nginx

# Deleting 2 resources simultaneously
kubectl delete deploy/nginx hpa/nginx

# Creating a service of type ClusterIP
kubectl create service clusterip my-cs --tcp=5678:8080

#Creating a job
kubectl create job pi  --image=perl:5.34 -- perl -Mbignum=bpi -wle 'print bpi(2000)'

# Creating a cron job with a schedule and command
kubectl create cronjob busybox --image=busybox --schedule="*/1 * * * *" -- /bin/sh -c 'date; echo Hello from the Kubernetes cluster'

# Creating a job from Cronjob
k create job --from=cronjob/cron-job-limited sample-job

# Creating configmaps
k create configmap config --from-literal=foo=lala --from-literal=foo2=lolo

# Creating CM from a file
k create cm config2 --from-file=config.txt

# Creating a CM from a env file
k create cm config3 --from-env-file=config.env

#Creating a CM from a file with special 
kubectl create cm configmap4 --from-file=special=config4.txt

# Creating Resource Quotas in namespace=one
k -n one create quota my-rq --hard=requests.cpu=1,requests.memory=1Gi,limits.cpu=2,limits.memory=2Gi

# Getting the value of a secret
kubectl get secret mysecret2 -o yaml
echo -n YWRtaW4= | base64 -d

# Getting secret value using jsonpath
kubectl get secret mysecret2 -o jsonpath='{.data.username}' | base64 -d

#Getting all env 
k -n secret-ops exec -it consumer -- /bin/sh -c env

# Getting all service accounts for all namespaces
kubectl get sa -A

# Creating API token for a service account
k create token myuser

# Follow the logs
kubectl logs busybox -f

# Deleting a pod immediately
kubectl delete po busybox --force --grace-period=0

# Getting CPU and Memory for nodes
kubectl top nodes

# Creating a pod and exposing a port (Creates both pod and service)
kubectl run nginx --image=nginx --restart=Never --port=80 --expose

# Checking ClusterIP and Endpoints
kubectl get svc nginx
kubectl get ep

# Running a temp pod to hit a ClusterIP
k run busybox --rm --image=busybox --restart=Never -it -- wget -O- 10.106.194.216:80

# wget on NodePort
wget -O- 192.....:31987

# Running a pod temp for hitting endpoints
k run busybox --rm --image=busybox --restart=Never -it -- sh

# Running a temp pod to hit IP with timeout & labels
k run busybox --image=busybox --rm -it --restart=Never --labels=access=granted -- wget -O- http://nginx:80 --timeout 20

# kubectl copy command (Copying data from a pod and storing locally)
kubectl cp busybox:/etc/passwd ./passwd

```

## Tips

- Converting ClusterIP to NodePort just change the type in the spec.
- 

### Inside the container checking connection with another container using Netcat — USE WGET INSTEAD MUCH EASIER

```bash
nc -v -z -w 2 secure-service 80

nc: This is the Netcat command, which is used for reading and writing network connections.

-v: This flag makes the operation more verbose, providing detailed information about the connection attempt.

-z: This flag tells Netcat to scan for listening daemons without sending any data to them. It essentially checks if the port is open without establishing a full connection.

-w 2: This sets a timeout of 2 seconds for the connection attempt. If the connection cannot be established within this time, Netcat will exit.

secure-service: This is the target service to connect to.

80: This is the port number to connect to on the target service.
```

```bash

Service
Create a Service named redis-service of type ClusterIP to expose pod redis on port 6379

kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml

(This will automatically use the pod's labels as selectors)
Or
kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml (This will not use the pods' labels as selectors; instead it will assume selectors as app=redis. You cannot pass in selectors as an option. So it does not work well if your pod has a different label set. So generate the file and modify the selectors before creating the service)

Create a Service named nginx of type NodePort to expose pod nginx's port 80 on port 30080 on the nodes:
kubectl expose pod nginx --port=80 --name nginx-service --type=NodePort --dry-run=client -o yaml
(This will automatically use the pod's labels as selectors, but you cannot specify the node port. You have to generate a definition file and then add the node port in manually before creating the service with the pod.)
Or
kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml
(This will not use the pods' labels as selectors)
Both the above commands have their own challenges. While one of it cannot accept a selector the other cannot accept a node port. I would recommend going with the `kubectl expose` command. If you need to specify a node port, generate a definition file using the same command and manually input the nodeport before creating the service.

```

# Revision

```bash
k -n marketing expose deployment redis --port=6379 --name=messaging-service

k logs dev-pod-dind-878516 -c log-x | grep -i "warn" > /opt/dind-878516_logs.txt
```

HELM
PODMAN — exporting to TAR FILE