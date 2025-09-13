# Docker/Container Basics

This guide covers essential container operations using Docker or Podman. Both tools follow similar command patterns, making it easy to work with either one.

## Why Containers?

- 📦 Consistent environment across development and production
- 🔄 Easy deployment and scaling
- 🔧 Isolated dependencies and libraries
- 📱 Portable across different platforms
- 🛠️ Efficient resource utilization

## Basic Container Operations

### Building Images

Build a container image from a Dockerfile in the current directory:

```bash
# Build an image and tag it
docker build -t pinger .      # Using Docker
podman build -t pinger .      # Using Podman

# List all images
docker images                 # Using Docker
podman image ls              # Using Podman
```

📚 [Official Docker Build Reference](https://docs.docker.com/engine/reference/commandline/build/)

### Running Containers

Launch and manage containers:

```bash
# Run a container with a specific name
docker run --name my-ping pinger    # Using Docker
podman run --name my-ping pinger    # Using Podman
```

📚 [Official Docker Run Reference](https://docs.docker.com/engine/reference/run/)

### Image Registry Operations

Work with container registries:

```bash
# Tag images for registry
docker tag pinger local-registry:5000/pinger:v1
podman tag pinger local-registry:5000/pinger:v1

# Push images to registry
docker push local-registry:5000/pinger:v1
podman push local-registry:5000/pinger:v1
```

📚 [Docker Registry Documentation](https://docs.docker.com/registry/)

## Best Practices

1. Always tag your images with specific versions
2. Use multi-stage builds to reduce image size
3. Leverage build cache effectively
4. Follow the principle of least privilege
5. Regularly update base images for security

## Additional Resources

### Official Documentation

- 📚 [Docker Documentation](https://docs.docker.com/)
- 📚 [Docker Best Practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
- 📚 [Docker Security](https://docs.docker.com/engine/security/)

### Container Security

- 📚 [Container Security Best Practices](https://docs.docker.com/develop/security-best-practices/)
- 📚 [Image Scanning](https://docs.docker.com/engine/scan/)

### Podman Alternative

- 📚 [Podman Documentation](https://docs.podman.io/en/latest/)
- 📚 [Docker to Podman Migration](https://podman.io/whatis.html)
