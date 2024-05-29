# Docker CLI Command Guide

## Overview

This guide provides comprehensive details on using Docker CLI commands for managing containers and images effectively.

### Container Management

#### Running Containers

- **Run a Simple 'hello-world' Container**
  ```
  docker run hello-world
  ```
This command downloads the hello-world image from the public Docker Hub repository and runs a container named hello-world.

- **Interactive Ubuntu Container**
```
docker run --name myubuntu2 -it ubuntu /bin/bash
```
Creates and runs a container named myubuntu2 from the ubuntu image and connects to it interactively. The container shuts down once you exit the bash session (exit command).

- **Detached Ubuntu Container**
```
docker run -dit --name myubuntu3 ubuntu /bin/bash
```
Similar to the above but runs the container named myubuntu3 in detached mode (in the background).

- **Stopping and Starting Containers**
Stop a Container

```
docker stop myubuntu3
```
Gracefully stops the running container named myubuntu3.

Start a Container

```
docker start myubuntu3
```
Starts the previously stopped container named myubuntu3.

Kill a Container

```
docker kill myubuntu3
```
Immediately kills the running container named myubuntu3 without waiting for it to shut down gracefully.

- **Image and Repository Operations**
*Managing Images*

List Images

```
docker images
docker image ls
```
Both commands list all locally stored Docker images.

Search for Images

```
docker search hello-world
```
Searches for hello-world images first locally, then in all public repositories if not found locally.

Remove an Image

```
docker rmi hello-world
```
Removes the hello-world image from local storage, provided no running container is using it.

*Managing Containers*
List Running Containers

```
docker ps
docker container ls
```
Both commands list currently active containers.

List All Containers

```
docker ps -a
docker container ls -a
```
These commands list all containers, including those that are stopped or created but not started.

Remove Containers

```
docker rm myubuntu   # Removes the 'myubuntu' container
docker rm myubuntu1  # Removes the 'myubuntu1' container
```

- **Building and Running Custom Docker Images**

Step 1: Create a Dockerfile
Dockerfile Setup
Create a Dockerfile with the following content:
Dockerfile
```
FROM ubuntu:latest
RUN apt-get update && apt-get install -y nginx
CMD ["nginx", "-g", "daemon off;"]
```

Step 2: Build the Image
Build Command
```
docker build -t mynginx .
```
Tags the created image with the name mynginx and uses the current directory as the build context.

Step 3: Run the Container
Container Deployment
```
docker run -d -p 8080:80 mynginx
```
Runs the container in detached mode and maps port 80 of the container to port 8080 on the host, allowing you to access Nginx by visiting http://localhost:8080.

Step 4: Verify Nginx
Check Nginx
Access http://localhost:8080 in your browser to see the Nginx welcome page, confirming the setup is correct.


**Practical Tips**
- Ensure containers are stopped before attempting removal.
- Use tags to organize images effectively in Docker Hub.
- Clean up unused images and containers regularly to free up system resources.
