# Kubernetes Secrets

Kubernetes Secrets let you store and manage sensitive information, such as passwords, OAuth tokens, and SSH keys. Using Secrets is more secure and flexible than putting confidential data directly in Pod specifications or container images.

> **Official Documentation**: [Kubernetes Secrets](https://kubernetes.io/docs/concepts/configuration/secret/)

## Types of Secrets

1. **Generic/Opaque**: Default Secret type for arbitrary user-defined data
2. **kubernetes.io/service-account-token**: Service account token
3. **kubernetes.io/dockercfg**: Serialized ~/.dockercfg file
4. **kubernetes.io/ssh-auth**: SSH credentials
5. **kubernetes.io/tls**: TLS client or server data
6. **bootstrap.kubernetes.io/token**: Bootstrap token data

## Creating Secrets

### 1. Using Command Line

```bash
# Create secret from literal values
kubectl create secret generic db-secret \
    --from-literal=DB_Host=sql01 \
    --from-literal=DB_User=root \
    --from-literal=DB_Password=password123

# Create secret from file
kubectl create secret generic my-secret \
    --type=kubernetes.io/ssh-auth \
    --from-file=ssh-privatekey=/root/.ssh/id_rsa
```

### 2. Using YAML Definition

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-credentials
type: Opaque
data:
  username: YWRtaW4= # base64 encoded 'admin'
  password: cGFzc3dvcmQ= # base64 encoded 'password'
```

> **Note**: Values in the data field must be base64 encoded. Use `echo -n 'mystring' | base64` to encode.

## Using Secrets in Pods

There are three main ways to use Secrets in Pods:

1. As environment variables
2. As files in a volume
3. As container image registry credentials

### 1. Using Secrets as Environment Variables

#### a. Using All Keys from a Secret

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: webapp-pod
spec:
  containers:
    - name: webapp
      image: kodekloud/simple-webapp-mysql
      envFrom: # Mount all keys from the secret
        - secretRef:
            name: db-secret # Name of the secret
```

#### b. Using Specific Keys from a Secret

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: webapp-pod
spec:
  containers:
    - name: webapp
      image: nginx
      env:
        - name: DATABASE_USER # Environment variable name
          valueFrom:
            secretKeyRef:
              name: db-secret # Secret name
              key: username # Key in the secret
```

### 2. Using Secrets as Files

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
    - name: mypod
      image: redis
      volumeMounts:
        - name: secret-volume # Must match volume name below
          mountPath: "/etc/secrets" # Mount path in container
          readOnly: true # Best practice for secrets
  volumes:
    - name: secret-volume # Volume name
      secret:
        secretName: mysecret # Secret to mount
        optional: false # Is the secret required?
```

## Best Practices

1. **Enable Encryption at Rest**

   - Enable encryption for Secret data in etcd
   - Use appropriate key rotation policies

2. **Limit Access**

   - Use RBAC to control access to Secrets
   - Minimize Secret distribution to nodes
   - Use service accounts judiciously

3. **Security Considerations**
   - Don't commit Secrets to source control
   - Rotate Secrets regularly
   - Use namespaces to isolate Secrets
   - Consider using external secret management systems

## Common Operations

```bash
# View secrets in a namespace
kubectl get secrets

# Describe a secret (metadata only)
kubectl describe secret mysecret

# Delete a secret
kubectl delete secret mysecret

# Decode a secret
kubectl get secret mysecret -o jsonpath='{.data.username}' | base64 --decode
```
