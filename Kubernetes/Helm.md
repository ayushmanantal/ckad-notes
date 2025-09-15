# Helm - The Kubernetes Package Manager

Helm is the package manager for Kubernetes that helps you manage Kubernetes applications. Helm Charts help you define, install, and upgrade even the most complex Kubernetes applications.

> **Official Documentation**: [Helm Documentation](https://helm.sh/docs/)

## Core Concepts

1. **Chart**: A Helm package containing all resource definitions necessary to run an application in Kubernetes
2. **Repository**: Place where charts are collected and shared
3. **Release**: An instance of a chart running in a cluster
4. **Values**: Configuration that can be supplied when installing a chart

## Basic Commands

### 1. Repository Management

```bash
# Add a repository
helm repo add bitnami https://charts.bitnami.com/bitnami

# List configured repositories
helm repo list

# Update repositories
helm repo update

# Remove a repository
helm repo remove bitnami
```

### 2. Finding Charts

````bash
# Search Helm Hub for charts
helm search hub wordpress    # Search Artifact Hub

# Search added repositories
helm search repo wordpress   # Search local reposor checking the package in Hub

```bash
helm seach hub worpress
````

### Adding a helm chart repo in “controlplane”

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
```

## Chart Installation and Management

### 1. Installing Charts

```bash
# Basic installation
helm install release-name bitnami/wordpress

# Install with custom values
helm install release-name bitnami/wordpress \
  --set wordpressUsername=admin \
  --set wordpressPassword=password

# Install with values file
helm install release-name bitnami/wordpress -f values.yaml

# Install with specific version
helm install release-name bitnami/wordpress --version 10.0.0
```

### 2. Managing Releases

```bash
# List all releases
helm list

# Get release status
helm status release-name

# Upgrade a release
helm upgrade release-name bitnami/wordpress --set parameters

# Rollback to previous version
helm rollback release-name revision-number

# Uninstall a release
helm uninstall release-name
```

### 3. Working with Charts

```bash
# Download and extract chart
helm pull --untar bitnami/wordpress

# Create a new chart
helm create mychart

# Package a chart
helm package mychart

# Validate a chart
helm lint mychart

# Show chart details
helm show chart bitnami/wordpress
```

## Custom Chart Structure

```plaintext
mychart/
  ├── Chart.yaml          # Chart metadata
  ├── values.yaml         # Default configuration values
  ├── charts/             # Chart dependencies
  ├── templates/          # Kubernetes manifest templates
  │   ├── deployment.yaml
  │   ├── service.yaml
  │   └── _helpers.tpl    # Template helpers
  └── .helmignore        # Patterns to ignore
```

## Common Chart Template Functions

```yaml
# String functions
{{ quote .Values.someString }}
{{ upper .Values.someString }}

# Flow control
{{- if .Values.enabled }}
  # content
{{- end }}

# Include named template
{{- include "mychart.labels" . | nindent 4 }}
```

## Best Practices

1. **Version Control**

   - Version your charts explicitly
   - Use semantic versioning
   - Document changes in NOTES.txt

2. **Values Management**

   - Use clear, hierarchical values structure
   - Provide good defaults
   - Document all configurable values

3. **Security**

   - Never store secrets in values.yaml
   - Use Secret management solutions
   - Implement RBAC properly

4. **Dependencies**
   - Pin dependency versions
   - Document dependencies clearly
   - Test dependency updates

## Debugging Tips

```bash
# Debug template rendering
helm template release-name . --debug

# Debug installation
helm install release-name . --dry-run

# Get manifest details
helm get manifest release-name

# Show computed values
helm get values release-name
```

## Common Issues and Solutions

1. **Chart Installation Fails**

   - Check values.yaml configuration
   - Verify Kubernetes cluster access
   - Check resource requirements

2. **Template Rendering Issues**

   - Use helm lint to check syntax
   - Debug with helm template --debug
   - Check indentation in templates

3. **Upgrade Problems**
   - Check release history
   - Use --dry-run flag
   - Consider using --atomic flag
