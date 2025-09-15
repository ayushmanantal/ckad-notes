# Kubernetes Network Policies

📚 [Official Documentation: Network Policies](https://kubernetes.io/docs/concepts/services-networking/network-policies/)

## Overview

Network Policies specify how groups of pods can communicate with each other and other network endpoints. They are the Kubernetes equivalent of a firewall for pod traffic.

## Key Concepts

### 1. Policy Types

- Ingress: Incoming traffic rules
- Egress: Outgoing traffic rules
- Both can be combined

### 2. Selectors

- podSelector: Target pods
- namespaceSelector: Target namespaces
- ipBlock: Target IP ranges

## Basic Operations

```bash
# List Network Policies
kubectl get networkpolicy
kubectl get netpol

# View Policy Details
kubectl describe networkpolicy <policy-name>

# Delete Policy
kubectl delete networkpolicy <policy-name>
```

## Policy Examples

### 1. Basic Deny All Policy

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-all
spec:
  podSelector: {}
  policyTypes:
    - Ingress
    - Egress
```

### 2. Complex Policy with Multiple Rules

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: internal-policy
  namespace: default
spec:
  podSelector:
    matchLabels:
      name: internal
  policyTypes:
    - Egress
    - Ingress
  ingress:
    - {} # Allow all incoming traffic
  egress:
    # Allow MySQL access
    - to:
        - podSelector:
            matchLabels:
              name: mysql
      ports:
        - protocol: TCP
          port: 3306

    # Allow Payroll service access
    - to:
        - podSelector:
            matchLabels:
              name: payroll
      ports:
        - protocol: TCP
          port: 8080

    # Allow DNS resolution
    - ports:
        - port: 53
          protocol: UDP
        - port: 53
          protocol: TCP
```

## Common Use Cases

### 1. Isolate Environment

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: prod-isolation
spec:
  podSelector:
    matchLabels:
      environment: production
  policyTypes:
    - Ingress
  ingress:
    - from:
        - podSelector:
            matchLabels:
              environment: production
```

### 2. Allow Specific Traffic

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: api-allow
spec:
  podSelector:
    matchLabels:
      app: api
  policyTypes:
    - Ingress
  ingress:
    - from:
        - podSelector:
            matchLabels:
              role: frontend
      ports:
        - protocol: TCP
          port: 8080
```

## Best Practices

1. 🔒 **Security**

   - Start with deny-all
   - Use specific selectors
   - Document policies
   - Regular review

2. 🎯 **Implementation**

   - Test thoroughly
   - Consider DNS access
   - Monitor policy effects
   - Plan for troubleshooting

3. 🔍 **Monitoring**

   - Log denied connections
   - Track policy changes
   - Monitor performance impact
   - Alert on violations

4. 📝 **Documentation**
   - Policy purposes
   - Affected workloads
   - Dependencies
   - Change history

## Troubleshooting

### Common Issues

1. DNS resolution blocked
2. Missing egress rules
3. Incorrect selectors
4. Policy conflicts

### Debug Commands

```bash
# Check pod labels
kubectl get pods --show-labels

# Verify policy application
kubectl describe networkpolicy <policy-name>

# Test connectivity
kubectl exec -it <pod-name> -- wget -qO- http://service
```

## Additional Resources

### Official Documentation

- 📚 [Declare Network Policy](https://kubernetes.io/docs/tasks/administer-cluster/declare-network-policy/)
- 📚 [Network Policy Recipes](https://kubernetes.io/docs/concepts/services-networking/network-policies/#networkpolicy-resource)

### Security

- 📚 [Pod Security Standards](https://kubernetes.io/docs/concepts/security/pod-security-standards/)
- 📚 [Security Best Practices](https://kubernetes.io/docs/concepts/security/security-checklist/)
