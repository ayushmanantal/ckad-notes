# Helm

### For checking the package in Hub

```bash
helm seach hub worpress
```

### Adding a helm chart repo in “controlplane”

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
```

### Searching a Package

```bash
helm search repo joomla
```

### Checking helm repositories in controlplane

```bash
helm repo list
```

### Installing a helm chart

```bash
helm install bravo bitnami/drupal
```

### Download chart

```bash
helm pull --untar bitnami/apache
```