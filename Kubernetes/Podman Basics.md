# Podman Basics

```bash
# Building a image
podman build -t simpleapp .

#Checking the images
podman images

#Checking image tree
podman image tree localhost/simpleapp:latest

#Running a docker image
podman run -d --name test -p 8080:80 localhost/simpleapp

#Checking the running image
podman ps

# Checking the logs
podman logs test

# Checking the output of the container
curl 0.0.0.0:8080

# Running a command inside a container
podman exec -it test cat /usr/local/apache2/htdocs/index.html

# Tag the image with ip and port of a private local registry and then push the image to this registry
podman tag localhost/simpleapp $registry_ip:5000/simpleapp
podman push $registry_ip:5000/simpleapp

# Creating a container without starting it 
podman create busbox

# Viewing the container 
podman container ls -a

# Getting the container output in a tar file
podman export test --output=output.tar

# Running a pod with image from registry
kubectl run simpleapp --image=$registry_ip:5000/simpleapp --port=80

# Reading the credentials from default file
podman login --username $YOUR_USER --password $YOUR_PWD docker.io
cat ${XDG_RUNTIME_DIR}/containers/auth.json

# Creating secret using existing login credentials
kubectl create secret generic mysecret --from-file=.dockerconfigjson=${XDG_RUNTIME_DIR}/containers/auth.json --type=kubernetes.io/dockeconfigjson

```