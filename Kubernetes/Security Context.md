# Security Context in Kubernetes

Security Context defines privilege and access control settings for Pods and Containers. It includes settings for:

- User and group IDs
- Privileged or unprivileged execution
- Linux capabilities
- SELinux context
- AppArmor profiles

📚 [Official Documentation: Pod Security Context](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)

## Overview

Security contexts are crucial for:

- 🔒 Controlling container privileges
- 👤 Setting user/group permissions
- 🛡️ Managing Linux capabilities
- 🔐 Enforcing security policies

## Checking Container Security Context

To verify the user context in a container:

```bash
kubectl exec ubuntu-sleeper -- whoami
```

This command shows the effective user inside the container, helping verify security context settings.

## Common Security Context Configurations

### 1. Pod and Container-level User Settings

The following example demonstrates setting different user IDs at pod and container levels:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: ubuntu-sleeper
spec:
  securityContext:
    runAsUser: 1001 # Pod-level setting: all containers inherit this unless overridden
  containers:
    - image: ubuntu
      name: web
      command: ["sleep", "5000"]
      securityContext:
        runAsUser: 1010 # Container-specific override
    - image: ubuntu
      name: sidecar
      command: ["sleep", "5000"]
      # This container inherits pod-level runAsUser: 1001
```

### 2. Adding Linux Capabilities

Example of adding specific Linux capabilities to a container:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: ubuntu-sleeper
  namespace: default
spec:
  containers:
    - command:
        - sleep
        - "4800"
      image: ubuntu
      name: ubuntu-sleeper
      securityContext:
        capabilities:
          add: ["SYS_TIME"] # Allows container to modify system time
```

### 3. Multiple Capabilities

Example showing multiple Linux capabilities:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: ubuntu-sleeper
  namespace: default
spec:
  containers:
    - command:
        - sleep
        - "4800"
      image: ubuntu
      name: ubuntu-sleeper
      securityContext:
        capabilities:
          add: ["SYS_TIME", "NET_ADMIN"] # Allow system time and network admin capabilities
```

### 4. Root User with Capabilities

Example of running as root with additional capabilities:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: app-sec-kff3345
  namespace: default
spec:
  securityContext:
    runAsUser: 0 # Run as root
  containers:
    - command:
        - sleep
        - "4800"
      image: ubuntu
      name: ubuntu
      securityContext:
        capabilities:
          add: ["SYS_TIME"]
```

## Best Practices

1. 🛡️ Follow the principle of least privilege
2. ⚠️ Avoid running containers as root
3. 🔒 Only add necessary Linux capabilities
4. 📝 Document security context changes
5. 🔍 Regularly audit security settings

## Additional Resources

### Official Documentation

- 📚 [Configure Security Context](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)
- 📚 [Pod Security Standards](https://kubernetes.io/docs/concepts/security/pod-security-standards/)
- 📚 [Linux Capabilities](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/#set-capabilities-for-a-container)

### Security Guidelines

- 📚 [CIS Kubernetes Benchmark](https://www.cisecurity.org/benchmark/kubernetes)
- 📚 [Kubernetes Security Best Practices](https://kubernetes.io/docs/concepts/security/security-best-practices/)
