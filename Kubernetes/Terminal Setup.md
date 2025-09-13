# Terminal Setup for Kubernetes

⚠️ **Important Exam Note**:
The alias `k='kubectl'` is automatically configured in CKAD exam environments. The additional aliases and shortcuts provided below are for reference and daily operations only. It is NOT recommended to spend time setting up these aliases during the exam.

## Overview

This guide provides useful terminal shortcuts and aliases for working with Kubernetes. These configurations can significantly improve your productivity in day-to-day operations.

## **General Aliases:**

```bash
alias k='kubectl'
alias kdesc='kubectl describe'
```

## **Create Resources:**

```bash
alias kcf='kubectl create -f'
alias kaf='kubectl apply -f'
```

## **Get resources:**

```bash
alias kgn='kubectl get nodes'
alias kgp='kubectl get pods'
alias kgpa='kubectl get pods -- all-namespaces'
alias kgs='kubectl get services'
alias kgd='kubectl get deployments'
```

## **Delete Resources:**

```bash
alias kd= 'kubectl delete'
alias kdp='kubectl delete pod'
alias kds='kubectl delete service'
alias kdd='kubectl delete deployment'
alias kdn='kubectl delete namespace'
```

## Other Useful Aliases

```bash

alias kn='kubectl config set-context --current --namespace'

alias kc=’k config get-contexts’

alias kg=’k get’

alias ke=”kubectl explain --recursive”

export dr=’--dry-run=client -o yaml’

export now=”--grace-period 0 -force”

alias k='kubectl'
alias kdesc='kubectl describe'
alias kcf='kubectl create -f'
alias kaf='kubectl apply -f'
alias kgn='kubectl get nodes'
alias kgp='kubectl get pods'
alias kgpa='kubectl get pods --all-namespaces'
alias kgs='kubectl get services'
alias kgd='kubectl get deployments'
```

```bash
alias kn='kubectl config set-context --current --namespace'
This sets the namespace e.g ‘kn default’ switches to default namespace.

alias kc=’k config get-contexts’
It gives you the current context. Remember, for all the questions you need to setup the cluster and context according to the question instruction.

alias kg=’k get’
To get any resource information

alias ke=”kubectl explain --recursive”
It helps to find details of field/schema of resources.

export dr=’--dry-run=client -o yaml’
To generate a yaml file from imperative command. Very very useful.

Example:
kubectl run my-nginx — image=nginx — port=80 $dr > pod.yaml

export now=”--grace-period 0 -force”
Use this to terminate the running resource
```
