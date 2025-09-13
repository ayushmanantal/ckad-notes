# Roles and Role Bindings

### GET

```bash
k get roles

k get rolebindings
```

### DESCRIBE

```bash
kubectl describe pod kube-apiserver-controlplane -n kube-system

k describe rolebinding kube-proxy -n kube-system
```

### Creating a role

```bash
k create role developer --namespace=default --verb=list,create,delete --resource=pods
```

### Creating rolebindings

```bash
k create rolebinding dev-user-binding --namespace=default --role=developer --user=dev-user
```

### Checking access

```bash
k auth can-i create deployments
k auth can-i delete nodes
```