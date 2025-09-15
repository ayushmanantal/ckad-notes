# Kubernetes API Deprecation

📚 [Official Documentation: API Deprecation](https://kubernetes.io/docs/reference/using-api/deprecation-policy/)

## Overview

Kubernetes follows a structured deprecation policy for its APIs to ensure smooth transitions between versions while maintaining stability and backward compatibility.

## Key Concepts

### 1. Version Format

```plaintext
X.Y.Z (Semantic Versioning)
X - Major version (breaking changes)
Y - Minor version (features, non-breaking)
Z - Patch version (bug fixes)
```

### 2. Deprecation Rules

- Features must be supported for 12 months or 3 releases (whichever is longer)
- API elements may only be removed by incrementing the API version
- API versions in GA must be supported for 12 months

## API Version Management

### 1. Checking API Versions

````bash
# View resource API info
kubectl explain jobs

# List available API resources
kubectl api-resources

# Check API versions
kubectl api-versions

# Explore API endpoints
kubectl proxy 8001&
curl localhost:8001/apis/authorization.k8s.io

### 2. API Server Configuration
```bash
# View API server configuration
vi /etc/kubernetes/manifests/kube-apiserver.yaml

# Check running configuration
ps -ef | grep kube-apiserver
````

## Working with kubectl-convert

### 1. Installation

```bash
# Download kubectl-convert
curl -LO https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl-convert

# Set permissions
chmod +x kubectl-convert

# Move to PATH
mv kubectl-convert /usr/local/bin/kubectl-convert
```

### 2. Converting API Versions

```bash
# Convert to new version
kubectl-convert -f old-resource.yaml --output-version new.api.version/v1

# Example: Convert Ingress
kubectl-convert -f ingress-old.yaml --output-version networking.k8s.io/v1 > ingress-new.yaml

# Apply converted resource
kubectl apply -f ingress-new.yaml

# Verify conversion
kubectl get ing <ingress-name> -o yaml | grep apiVersion
```

## Common Migration Scenarios

### 1. Ingress API Migration

```yaml
# Old Version (extensions/v1beta1)
apiVersion: extensions/v1beta1
kind: Ingress

# New Version (networking.k8s.io/v1)
apiVersion: networking.k8s.io/v1
kind: Ingress
```

### 2. Deployment API Migration

```yaml
# Old Version (apps/v1beta1)
apiVersion: apps/v1beta1
kind: Deployment

# New Version (apps/v1)
apiVersion: apps/v1
kind: Deployment
```

## Best Practices

1. 🔄 **Version Management**

   - Track API versions used
   - Plan migrations early
   - Test conversions in dev
   - Document changes

2. 🎯 **Migration Strategy**

   - Identify deprecated APIs
   - Update manifests systematically
   - Use kubectl-convert
   - Verify functionality

3. ⚡ **Testing**

   - Test in non-prod first
   - Verify all functionality
   - Check for dependencies
   - Monitor for issues

4. 📊 **Monitoring**
   - Track deprecated usage
   - Monitor conversion success
   - Watch for warnings
   - Document issues

## Common Operations

```bash
# Check API versions
kubectl api-versions | grep <resource-group>

# View deprecated API usage
kubectl get <resource> --all-namespaces -o yaml | grep "apiVersion:"

# Convert multiple files
for f in *.yaml; do kubectl-convert -f $f --output-version new.api.version/v1 > converted/$f; done

# Verify resources
kubectl get <resource> -o yaml | grep -A5 apiVersion
```

## Additional Resources

### Official Documentation

- 📚 [API Deprecation Policy](https://kubernetes.io/docs/reference/using-api/deprecation-policy/)
- 📚 [API Versions](https://kubernetes.io/docs/reference/using-api/)
- 📚 [API Changes](https://kubernetes.io/docs/reference/using-api/api-changes/)

### Migration Guides

- 📚 [Deprecated API Migration Guide](https://kubernetes.io/docs/reference/using-api/deprecation-guide/)
- 📚 [Version Skew Policy](https://kubernetes.io/releases/version-skew-policy/)
