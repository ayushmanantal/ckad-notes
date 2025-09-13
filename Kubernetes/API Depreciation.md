# API Depreciation

```bash
k explain jobs
```

### Releases Explained

X - major
Y - minor
Z - patch

```bash
kubectl proxy 8001&

curl localhost:8001/apis/authorization.k8s.io
```

```bash
vi /etc/kubernetes/manifests/kube-apiserver.yaml. #Kube API Server
```

### Kube Convert Installation

```bash
curl -LO https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl-convert

****chmod +x kubectl-convert  #Fixes permission

mv kubectl-convert /usr/local/bin/kubectl-convert
```

### Run the command kubectl-convert to change the deprecated API version

```bash
kubectl-convert -f ingress-old.yaml --output-version networking.k8s.io/v1
```

### Store new changes into a file

```bash
kubectl-convert -f ingress-old.yaml --output-version networking.k8s.io/v1 > ingress-new.yaml
```

### After changing the API version and storing into a file, use the kubectl create -f command to deploy the resource

```bash
kubectl create -f ingress-new.yaml
```

### Inspect the apiVersion as follows

```bash
kubectl get ing ingress-space -oyaml | grep apiVersion
```

**Note: The service and other resources mentioned in the ingress YAML may not appear on the controlplane node, as we are only deploying the ingress resource with the latest API version.**

```bash
curl localhost:8001/apis/authorization.k8s.io
```