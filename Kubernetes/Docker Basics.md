# Docker Basics

```docker
Build the image:
podman build -t pinger .

podman image ls

Run the image:
podman run --name my-ping pinger

podman tag pinger local-registry:5000/pinger

podman image ls

podman push local-registry:5000/pinger

podman tag pinger pinger:v1

podman tag pinger local-registry:5000/pinger:v1

podman image ls

podman push local-registry:5000/pinger:v1
```