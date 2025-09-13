# Basic Commands

## Getting all pods in default namespace

```bash
k get pods
k get pod nam123
kubectl get rs

k get pods --as dev-user
```

## ALL Namespaces

```bash
k get all -A
```

**EDIT**

```bash
k edit deployment name123
k edit pod redis
k edit rs/replic-1
```

## SCALE

```bash
kubectl scale --replicas=5 rs/repli-1
```

**DELETE**

```bash
k delete pod name12
k delete rs/replica-1
```

**DESCRIBE**

```bash
K describe pod name23
K describe deployment njn343
```

## CREATE

```bash
kubectl create configmap ingress-nginx-controller --namespace ingress-nginx

kubectl create secret generic db-secret-xxdf \
  --from-literal=DB_Host=sql01 \
  --from-literal=DB_User=root \
  --from-literal=DB_Password=password123
```

**APPLY**

```bash
k apply –f test.yaml
```

**LOGS**

```bash
k logs pod name123
k logs deployment name123
kubectl logs dev-pod-dind-878516 -c log-x | grep -i "warn" > /opt/dind-878516_logs.txt

k logs e-com-1123 -n e-commerce  > /opt/outputs/e-com-1123.logs
```

**SERVICE**

```bash
k get service
kubectl get svc -n critical-space
```

**NETWORK POLICIES**

```bash
k get netpol
```

## EXPOSE

```bash
kubectl expose deployment redis --port=6379 --name messaging-service --namespace marketing

k -n world expose deploy europe --port 80
k -n world expose deploy asia --port 80
```

## LABEL

```bash
kubectl label nodes controlplane app_type=beta
```

## EXPLAIN

```bash
# Show all fields in a pod specification
kubectl explain pod --recursive

# Show all fields in a deployment
kubectl explain deployment --recursive
```

## Some Useful Commands

```bash
#Checking current namespace
kubectl config view --minify | grep namespace

k get ingressclass

k get svc -A

cat /etc/kubernetes/manifests/kube-apiserver.yaml

```

## Helm

```docker
helm list ls

helm uninstall -n hello api-server

#dev is the release version
helm uninstall -n hello dev falcon/falco

```
