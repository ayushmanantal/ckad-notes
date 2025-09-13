# Admission Controllers

### Checking configurations in KubeConfig

```bash
vi /etc/kubernetes/manifests/kube-apiserver.yaml
```

Kubernetes have NamespaceExists admission controller enabled which rejects requests to namespaces that do not exist. 

To automatically create namespaces that don't exist, you can enable the NamespaceAutoProvision admission controller.

***Note:* Once you update kube-apiserver yaml file, please wait for a few minutes for the kube-apiserver to restart completely.**

### Disable DefaultStorageClass admission controller

This admission controller observes creation of PersistentVolumeClaim objects that do not request any specific storage class and automatically adds a default storage class to them. This way, users that do not request any special storage class do not need to care about them at all and they will get the default one.

***Note:* Once you update kube-apiserver yaml file then please wait few mins for the kube-apiserver to restart completely.**

### Answer:

Add DefaultStorageClass to disable-admission-plugins in /etc/kubernetes/manifests/kube-apiserver.yaml

**Since the kube-apiserver is running as pod you can check the process to see enabled and disabled plugins.**

```bash
ps -ef | grep kube-apiserver | grep admission-plugins
```

## **Mutating and Validation Admission Controllers**

- Mutating Admission Webhook
- Validating Admission Webhook

### Creating TLS secret

```bash
kubectl -n webhook-demo create secret tls webhook-server-tls \ --cert "/root/keys/webhook-server-tls.crt" \ --key "/root/keys/webhook-server-tls.key"
```

### Getting values from a pod:

```bash
kubectl get po pod-with-defaults -o yaml | grep -A2 " securityContext:"
```