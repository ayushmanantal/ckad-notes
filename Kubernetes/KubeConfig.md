# KubeConfig

## Basic Configuration Commands

### View Configuration

```bash
kubectl config view                    # Display merged kubeconfig settings
kubectl config current-context        # Show current context
kubectl config get-contexts          # List all available contexts
```

### Context Management

```bash
kubectl config use-context <context-name>       # Switch to a different context
kubectl config set-context <context-name> --cluster=<cluster-name> --user=<user-name>    # Create a new context
kubectl config set-context --current --namespace=<namespace>   # Change namespace in current context
```

```bash
kubectl config --kubeconfig=/root/my-kube-config use-context research

kubectl config --kubeconfig=/root/my-kube-config current-context
```