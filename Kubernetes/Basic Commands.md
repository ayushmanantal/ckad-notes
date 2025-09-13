# Basic Commands

This guide covers the essential kubectl commands used for managing Kubernetes resources. Each section includes common use cases and examples.

## Getting Resources

Lists pods and ReplicaSets in the default namespace. The `--as` flag allows you to check permissions for other users.

```bash
k get pods
k get pod nam123
kubectl get rs

k get pods --as dev-user
```

📚 [Official Documentation: Viewing Pods and Nodes](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/)

## ALL Namespaces

View resources across all namespaces using the `-A` or `--all-namespaces` flag.

```bash
k get all -A
```

📚 [Official Documentation: Namespaces](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/)

## EDIT

Modify existing resources directly in your default editor. Changes are applied immediately upon saving.

```bash
k edit deployment name123
k edit pod redis
k edit rs/replic-1
```

📚 [Official Documentation: Update API Objects in Place](https://kubernetes.io/docs/concepts/cluster-administration/manage-deployment/#updating-your-application-without-a-service-outage)

## SCALE

Adjust the number of replicas for deployments, replicasets, or statefulsets.

```bash
kubectl scale --replicas=5 rs/repli-1
```

📚 [Official Documentation: Scaling Your Application](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#scaling-a-deployment)

## DELETE

Remove resources from your cluster. Use with caution in production environments.

```bash
k delete pod name12
k delete rs/replica-1
```

📚 [Official Documentation: Delete Resources](https://kubernetes.io/docs/tasks/run-application/delete-stateful-set/)

## DESCRIBE

Get detailed information about a specific resource, including events and status.

```bash
K describe pod name23
K describe deployment njn343
```

📚 [Official Documentation: Tools for Monitoring Resources](https://kubernetes.io/docs/tasks/debug/debug-application/debug-running-pod/)

## CREATE

Create new resources in your cluster. Useful for ConfigMaps, Secrets, and other resources.

```bash
kubectl create configmap ingress-nginx-controller --namespace ingress-nginx

kubectl create secret generic db-secret-xxdf \
  --from-literal=DB_Host=sql01 \
  --from-literal=DB_User=root \
  --from-literal=DB_Password=password123
```

📚 [Official Documentation: Managing Resources](https://kubernetes.io/docs/concepts/configuration/configmap/)

## APPLY

Create or update resources using declarative configuration files.

```bash
k apply –f test.yaml
```

📚 [Official Documentation: Managing Resources with kubectl apply](https://kubernetes.io/docs/concepts/cluster-administration/manage-deployment/#using-kubectl-apply)

## LOGS

View container logs for debugging and monitoring. Supports output redirection and filtering.

```bash
k logs pod name123
k logs deployment name123
kubectl logs dev-pod-dind-878516 -c log-x | grep -i "warn" > /opt/dind-878516_logs.txt

k logs e-com-1123 -n e-commerce  > /opt/outputs/e-com-1123.logs
```

📚 [Official Documentation: Logging Architecture](https://kubernetes.io/docs/concepts/cluster-administration/logging/)

## SERVICE

Manage and inspect services that expose applications within or outside the cluster.

```bash
k get service
kubectl get svc -n critical-space
```

📚 [Official Documentation: Service](https://kubernetes.io/docs/concepts/services-networking/service/)

## NETWORK POLICIES

View and manage network policies that control pod-to-pod communication.

```bash
k get netpol
```

📚 [Official Documentation: Network Policies](https://kubernetes.io/docs/concepts/services-networking/network-policies/)

## EXPOSE

Create a new service to expose deployments, pods, or other resources.

```bash
kubectl expose deployment redis --port=6379 --name messaging-service --namespace marketing

k -n world expose deploy europe --port 80
k -n world expose deploy asia --port 80
```

📚 [Official Documentation: Publishing Services](https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services-service-types)

## LABEL

Add, modify, or remove labels from resources for organization and selection.

```bash
kubectl label nodes controlplane app_type=beta
```

📚 [Official Documentation: Labels and Selectors](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/)

## EXPLAIN

Get detailed information about resource fields and their usage.

```bash
# Show all fields in a pod specification
kubectl explain pod --recursive

# Show all fields in a deployment
kubectl explain deployment --recursive
```

📚 [Official Documentation: kubectl explain](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#explain)

## Useful Commands

Common commands for cluster administration and troubleshooting.

```bash
#Checking current namespace
kubectl config view --minify | grep namespace

k get ingressclass

k get svc -A

cat /etc/kubernetes/manifests/kube-apiserver.yaml
```

📚 [Official Documentation: kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

## Helm

Package manager commands for installing and managing Kubernetes applications.

```bash
helm list ls

helm uninstall -n hello api-server

#dev is the release version
helm uninstall -n hello dev falcon/falco
```

📚 [Official Helm Documentation](https://helm.sh/docs/)
