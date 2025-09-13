# CKAD Study Guide: Comprehensive Kubernetes Notes

A comprehensive collection of study notes and practical examples for the Certified Kubernetes Application Developer (CKAD) certification. This repository contains detailed documentation on various Kubernetes concepts, commands, and best practices.

## Overview

This guide covers essential topics for the CKAD exam, including:

- Basic and advanced Kubernetes commands
- Pod management and deployment strategies
- Networking and service configuration
- Security and access control
- Storage and persistent volumes
- Container orchestration
- Resource management and monitoring

Perfect for developers preparing for the CKAD certification or anyone looking to deepen their understanding of Kubernetes application development.

# Topics

- [Basic Commands](./Kubernetes/Basic%20Commands.md)

- [Short Forms](./Kubernetes/Short%20Forms.md)

- [Short Commands](./Kubernetes/Short%20Commands.md)

- [Podman Basics](./Kubernetes/Podman%20Basics.md)

- [Docker Basics](./Kubernetes/Docker%20Basics.md)

- [Exam Tips](./Kubernetes/Exam%20Tips.md)

- [Terminal Setup](./Kubernetes/Terminal%20Setup.md)

- [Examples](./Kubernetes/Examples.md)

- [Practice Questions](./Kubernetes/Practice%20Questions.md)

- [Rolling Updates and Rollbacks](./Kubernetes/Rolling%20Updates%20and%20Rollbacks.md)

- [Monitoring](./Kubernetes/Monitoring.md)

- [Security Context](./Kubernetes/Security%20Context.md)

- [Volumes](./Kubernetes/Volumes.md)

- [Persistent Volumes, Persistent Volume Claims and Storage Class](./Kubernetes/Persistent%20Volumes,%20Persistent%20Volume%20Claims%20and%20S.md)

- [Label and Selectors](./Kubernetes/Label%20and%20Selectors.md)

- [Multi-Container Pods](./Kubernetes/Multi-Container%20Pods.md)

- [Node Affinity](./Kubernetes/Node%20Affinity.md)

- [Roles and Role Bindings](./Kubernetes/Roles%20and%20Role%20Bindings.md)

- [Readiness Probes](./Kubernetes/Readiness%20Probes.md)

- [Taint and Tolerations](./Kubernetes/Taint%20and%20Tolerations.md)

- [Service](./Kubernetes/Service.md)

- [Jobs and CronJobs](./Kubernetes/Jobs%20and%20CronJobs.md)

- [Helm](./Kubernetes/Helm.md)

- [ConfigMaps](./Kubernetes/ConfigMaps.md)

- [Secrets](./Kubernetes/Secrets.md)

- [Network Policies](./Kubernetes/Network%20Policies.md)

- [Ingress](./Kubernetes/Ingress.md)

- [Cluster Roles and Bindings](./Kubernetes/Cluster%20Roles%20and%20Bindings.md)

- [Admission Controllers](./Kubernetes/Admission%20Controllers.md)

- [API Depreciation](./Kubernetes/API%20Depreciation.md)

- [KubeConfig](./Kubernetes/KubeConfig.md)

# Study Materials

- [Kubernetes Documentation](https://kubernetes.io/docs/home/)
- [KodeKloud](https://kodekloud.com/learning-path/ckad/)

# Practice Questions

**[CKAD Exercises](https://github.com/dgkanatsios/CKAD-exercises)**

# Practice Environment Setup

## Quick Start with Minikube

For a cost-effective and efficient way to practice Kubernetes locally, I recommend using Minikube. It provides a lightweight Kubernetes implementation that runs on your local machine.

### Why Minikube?

- 🚀 Quick to set up and get started
- 💻 Runs on Linux, macOS, and Windows
- 🔧 No need for complex hardware setup
- 📚 Perfect for learning and development
- 🆓 Free and open-source

### Installation

1. Install Minikube by following the official guide: [Minikube Installation Documentation](https://minikube.sigs.k8s.io/docs/)
2. Configure shell aliases for easier usage:

#### For macOS/Linux Users:

Edit your shell configuration file (`~/.bashrc` for Bash or `~/.zshrc` for Zsh):

```bash
# Add these lines to your shell configuration
alias k="minikube kubectl --"
[[ $commands[kubectl] ]] && source <(kubectl completion zsh)
```

#### For Windows Users:

For Windows PowerShell, create a profile file if you haven't already:

```powershell
# Create PowerShell profile if it doesn't exist
if (!(Test-Path -Path $PROFILE)) {
    New-Item -ItemType File -Path $PROFILE -Force
}

# Add this line to your PowerShell profile
Set-Alias -Name k -Value 'minikube kubectl --'
```

### Verification

After setup, verify your installation:

```bash
minikube status
k get nodes
```

# References

- [Kubernetes Documentation](https://kubernetes.io/docs/home/)
- [KodeKloud](https://kodekloud.com/learning-path/ckad/)
