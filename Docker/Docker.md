# Docker Training Guide

Welcome to the Docker Training Guide! This document is designed to introduce you to Docker, a powerful tool for creating, deploying, and running applications by using containers. Containers allow you to package up an application with all of the parts it needs, such as libraries and other dependencies, and ship it all out as one package.

## Table of Contents

- [1. Introduction to Docker](#1-introduction-to-docker)
  - [1.1 What is Docker?](#11-what-is-docker)
  - [1.2 Docker Architecture](#12-docker-architecture)
- [2. Installing Docker](#2-installing-docker)
  - [2.1 System Requirements](#21-system-requirements)
  - [2.2 Installation Steps](#22-installation-steps)
  - [2.3 Post-Installation Steps](#23-post-installation-steps)
- [3. Working with Docker Containers](#3-working-with-docker-containers)
  - [3.1 Understanding Docker Containers](#31-understanding-docker-containers)
  - [3.2 Basic Docker Commands](#32-basic-docker-commands)
  - [3.3 Managing Containers](#33-managing-containers)
- [4. Docker Images](#4-docker-images)
  - [4.1 Understanding Docker Images](#41-understanding-docker-images)
  - [4.2 Managing Images](#42-managing-images)
  - [4.3 Building Images with Dockerfile](#43-building-images-with-dockerfile)
  - [4.4 Example Dockerfiles](#44-example-dockerfiles)

## 1. Introduction to Docker

### 1.1 What is Docker?

Docker is an open-source platform that automates the deployment of applications inside lightweight, portable containers. It simplifies the process of managing applications by using containers, fostering both flexibility and scalability.

### 1.2 Docker Architecture

Docker uses a client-server architecture. The Docker *client* talks to the Docker *daemon*, which does the heavy lifting of building, running, and distributing your Docker containers. Docker uses a REST API for interaction and can communicate using a CLI or applications using Docker SDKs.

- **Docker Daemon** (dockerd): Manages Docker objects such as images, containers, networks, and volumes.
- **Docker Client** (`docker`): Enables users to interact with Docker.
- **Docker Registries**: Store Docker images. Docker Hub is a public registry that anyone can use, and Docker is configured to look for images on Docker Hub by default.
- **Docker Objects**:
  - **Images**: The blueprints of our application which form the basis of containers.
  - **Containers**: They run the applications and can be started, stopped, moved, and deleted.
  - **Services**: Allows you to scale containers across multiple Docker daemons, which all work together as a swarm with multiple managers and workers.

## 2. Installing Docker

### 2.1 System Requirements

- Windows 10 64-bit: Pro, Enterprise, or Education (1607 Anniversary Update, Build 14393 or later).
- macOS must be version 10.12.6 or newer. Hardware must be a 2010 or newer model.
- Linux distributions supported include CentOS, Debian, Fedora, and Ubuntu. Preferably use the latest version available.

### 2.2 Installation Steps

#### Windows

1. Download *Docker Desktop* from Docker Hub.
2. Run the installer and follow the on-screen instructions.

#### macOS

1. Download *Docker Desktop* from Docker Hub.
2. Drag and drop the Docker icon to your Applications folder.


#### Ubuntu

 **Update your package index**:
   
   ```bash
   sudo apt update
   ```
**Install packages to allow apt to use a repository over HTTPS**:

   ```bash
   sudo apt install apt-transport-https ca-certificates curl software-properties-common
   ```
**Add Docker’s official GPG key:**
   ```bash
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
   ```
**Verify that you now have the key with the fingerprint:**
   ```bash
   sudo apt-key fingerprint 0EBFCD88
   ```
**Set up the stable repository:**
  ```bash
  sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
  ```
**Update the apt package index:**
  ```bash
  sudo apt update
  ```
**Install the latest version of Docker CE and Containerd:**
```bash
sudo apt install docker-ce docker-ce-cli containerd.io
```
**Verify that Docker CE is installed correctly by running the Hello World image:**
```bash
sudo docker run hello-world

```

To create a user named docker_user and add that user to the docker group on a Linux system, follow these detailed steps. These steps will cover user creation, password setting, and adding the user to the necessary group.

Step 1: Create the User
First, create the user docker_user:

```
sudo useradd -m docker_user
```
The -m flag ensures that a home directory is created for the user.

Step 2: Set a Password for the User
Set a password for the new user by running:

```
sudo passwd docker_user
```
You will be prompted to enter and confirm the password.

Step 3: Add the User to the Docker Group
Before adding the user to the docker group, make sure the docker group exists. You can check this with:

```
getent group docker
```
If the docker group does not exist (no output), you can create it with:

```
sudo groupadd docker
```
Now, add docker_user to the docker group:

```
sudo usermod -aG docker docker_user
```
The -aG options are critical:

-a appends the user to the given group(s).
-G specifies the group name.
Step 4: Verify the User and Group Configuration
Check that the user was added to the docker group successfully:

```
groups docker_user
```
The output should list docker among the groups for docker_user.

Step 5: Apply Permissions
For the user to start using Docker with the new group, they must log out and back in, or you can switch to that user in the terminal to refresh their group memberships:

```
su - docker_user
```
Step 6: Test Docker Command (Optional)
As docker_user, you should now be able to run Docker commands without sudo. Test this with:

```
docker run hello-world
```
This command downloads a test image and runs it in a container, which prints a message if Docker is set up correctly.

## 3. Working with Docker Containers

### 3.1 Understanding Docker Containers

Containers are an abstraction at the app layer that packages code and dependencies together. Multiple containers can run on the same machine and share the OS kernel with other containers, each running as isolated processes in user space.

### 3.2 Basic Docker Commands

- `docker run` - Create a new container and start it.
- `docker stop` - Stop a running container.
- `docker ps` - List running containers.
- `docker rm` - Remove one or more containers.

### 3.3 Managing Containers

- `docker logs [container_id]` - Fetch the logs of a container.
- `docker exec -it [container_id] sh` - Execute an interactive bash shell inside the container.

## 4. Docker Images

### 4.1 Understanding Docker Images

A Docker image provides a convenient way to package up applications and pre-configured server environments, which you can use privately or share publicly. Images are built from a Dockerfile using the `docker build` command.

### 4.2 Managing Images

- `docker images` - List all local images.
- `docker rmi [image_id]` - Remove one or more images.
- `docker build -t [username]/[imagename]:[tag] .` - Build an image from a Dockerfile in the current directory and tag it.

### 4.3 Building Images with Dockerfile

Dockerfiles define what goes on in the environment inside your container. Access to resources like networking interfaces and disk drives is virtualized inside this environment, which is isolated from the rest of your system, so you have to map ports to the outside world, and be specific about what files you want to "copy" into that environment.

### 4.4 Example Dockerfiles

#### Example 1: Helloworld

```dockerfile
# Use a basic image as a parent
FROM ubuntu:latest

# Set the working directory in the container
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Run a simple command when the container launches
CMD ["echo", "Hello, world!"]
```
Explanation:

FROM ubuntu:latest: This line specifies the base image to use for building this Docker image. In this case, it's using the latest version of the Ubuntu Linux distribution.

WORKDIR /app: This line sets the working directory within the container to /app. Subsequent commands will be executed from this directory unless otherwise specified.

COPY . /app: This line copies the contents of the current directory (where the Dockerfile is located) into the /app directory in the container. The first dot (.) represents the current directory on the host machine, and the second dot (/app) represents the /app directory in the container.

CMD ["echo", "Hello, world!"]: This line specifies the command to run when the container starts. In this case, it runs the echo command with the argument "Hello, world!", which simply prints "Hello, world!" to the console when the container is launched.

This Dockerfile sets up a basic environment where the contents of the current directory are copied into a directory named /app within the container. When the container starts, it echoes "Hello, world!" to the console. This Dockerfile can serve as a starting point for building Docker images for various applications.

Steps to run above file .

Create a Dockerfile: Copy the provided Dockerfile content into a file named Dockerfile.

Build the Docker Image: Open a terminal or command prompt, navigate to the directory containing the Dockerfile, and run the following command to build the Docker image:

```
docker build -t my_ubuntu_image .
Replace my_ubuntu_image with the desired name for your Docker image.
```

Run the Docker Container: After the Docker image has been built, you can run a container based on the image using the following command:

```
docker run my_ubuntu_image
```

This command will execute the command specified in the Dockerfile's CMD instruction, which in this case is echo "Hello, world!".
View the Output: The command specified in the CMD instruction will run inside the Docker container, and you'll see the output "Hello, world!" printed to the console.


Example 2: Python
```dockerfile
# Use an official Python runtime as a parent image
FROM python:3

# Set the working directory in the container
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install Flask
RUN pip install Flask

# Expose port 5000 to the outside world
EXPOSE 5000

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```
```python app
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    return 'Hello, World!'

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0')
```

Dockerfile Explanation:
FROM python:3: Specifies the base image to use for building this Docker image. It uses the official Python 3 image available from Docker Hub.

WORKDIR /app: Sets the working directory within the container to /app. Subsequent commands will be executed from this directory.

COPY . /app: Copies the contents of the current directory (where the Dockerfile is located) into the /app directory in the container.

RUN pip install Flask: Installs the Flask web framework using the pip package manager inside the container.

EXPOSE 5000: Informs Docker that the container will listen on port 5000 at runtime.

ENV NAME World: Sets an environment variable named NAME with the value World.

CMD ["python", "app.py"]: Specifies the command to run when the container starts. It starts the Flask application defined in app.py.

app.py Explanation:
from flask import Flask: Imports the Flask class from the flask module.

app = Flask(name): Creates a Flask application instance called app.

@app.route('/'): Defines a route / using the @app.route decorator, which responds with "Hello, World!" when accessed.

return 'Hello, World!': Returns the string "Hello, World!" when the / route is accessed.

if name == 'main': This conditional block ensures that the Flask development server is only started when the script is executed directly (not when it's imported as a module).

app.run(debug=True, host='0.0.0.0'): Starts the Flask application on host 0.0.0.0 and enables debugging mode.

This setup creates a simple Python Flask web application inside a Docker container. The Dockerfile defines the container environment, installs necessary dependencies, and specifies the command to run the application. The app.py file contains the Flask application code.


Certainly! Below are the steps to run the Dockerfile and access the Flask app in Markdown format:

Steps to Run Dockerfile and Access Flask App

Build the Docker Image: Open a terminal or command prompt, navigate to the directory containing the Dockerfile and app.py, and run the following command to build the Docker 
```
docker build -t my_flask_app .
```

Run the Docker Container: After the Docker image has been built, you can run a container based on the image using the following command:
```
docker run -d -p 5000:5000 my_flask_app
```

Access the Flask App: Once the container is running, you can access the Flask app by opening a web browser and navigating to http://localhost:5000 or http://<docker_host_ip>:5000 if you're using Docker Toolbox or Docker Machine.

Verify the App: The Flask app should display "Hello, World!" in your web browser, indicating that the application is running successfully.

By following these steps, you can build and run the Docker image containing the Flask app, and access the app through a web browser.

