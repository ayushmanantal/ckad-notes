# Podman Basics

Podman (Pod Manager) is a daemonless container engine for developing, managing, and running OCI containers. It's a secure alternative to Docker that can run containers either as root or in rootless mode.

## Why Podman?

- 🔒 Daemonless architecture for better security
- 🐋 Drop-in replacement for Docker
- 🔄 Compatible with Docker commands and Dockerfiles
- 👥 Rootless container execution
- 🔌 No daemon required for container operations

## Common Operations

### Building and Managing Images

Build and manage container images using Podman commands:

```bash
# Image Building and Management
# Build a container image from a Dockerfile in current directory
podman build -t simpleapp .

# List all local images
podman images

# View the layer structure of an image
podman image tree localhost/simpleapp:latest

# Container Operations
# Run a container in detached mode with port mapping
podman run -d --name test -p 8080:80 localhost/simpleapp

# List running containers
podman ps

# View container logs
podman logs test

# Test container accessibility
curl 0.0.0.0:8080

# Execute commands in running container
podman exec -it test cat /usr/local/apache2/htdocs/index.html

# Registry Operations
# Tag and push image to private registry
podman tag localhost/simpleapp $registry_ip:5000/simpleapp
podman push $registry_ip:5000/simpleapp

# Container Management
# Create container without starting it
podman create busbox

# List all containers (running and stopped)
podman container ls -a

# Export container filesystem as tar
podman export test --output=output.tar

# Kubernetes Integration
# Deploy container from private registry to Kubernetes
kubectl run simpleapp --image=$registry_ip:5000/simpleapp --port=80

# Authentication and Secrets
# Log in to container registry
podman login --username $YOUR_USER --password $YOUR_PWD docker.io

# View stored credentials
cat ${XDG_RUNTIME_DIR}/containers/auth.json

# Create Kubernetes secret from registry credentials
kubectl create secret generic mysecret \
  --from-file=.dockerconfigjson=${XDG_RUNTIME_DIR}/containers/auth.json \
  --type=kubernetes.io/dockeconfigjson

```
