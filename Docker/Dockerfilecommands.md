*Docker is a powerful platform for developers and sysadmins to develop, deploy, and run applications with containers. The heart of the Docker platform is the Dockerfile, a text document that contains all the commands a user could call on the command line to assemble an image. Using docker build users can create an automated build that executes several command-line instructions in succession. Here’s a comprehensive guide to Dockerfile commands (instructions) along with examples and explanations for each.*

FROM
Purpose: Sets the base image for subsequent instructions. A valid Dockerfile must start with a FROM instruction.
Example:

```dockerfile
FROM ubuntu:18.04
```
Explanation: This line sets the base image to Ubuntu 18.04.

LABEL
Purpose: Adds metadata to an image such as version, description, and author.
Example:

```dockerfile
LABEL maintainer="john.doe@example.com"
LABEL version="1.0"
```
Explanation: These lines add a maintainer and a version label to the image.

RUN
Purpose: Executes commands in a new layer on top of the current image and commits the results.
Example:

```dockerfile
RUN apt-get update && apt-get install -y git
```
Explanation: This RUN instruction updates the package lists and installs git in Ubuntu.

CMD
Purpose: Provides defaults for executing a container.
Example:

```dockerfile
CMD ["echo", "Hello world"]
```
Explanation: This is the default command executed when you run the container. It prints "Hello world" to the console.

EXPOSE
Purpose: Informs Docker that the container listens on the specified network ports at runtime.
Example:

```dockerfile
EXPOSE 80
```
Explanation: This tells Docker that the container will listen on port 80 at runtime.

ENV
Purpose: Sets environment variables.
Example:

```dockerfile
ENV MY_NAME="John Doe"
```
Explanation: This sets an environment variable MY_NAME with the value "John Doe".

ADD
Purpose: Copies new files, directories, or remote file URLs from <src> and adds them to the filesystem of the image at the path <dest>.
Example:

```dockerfile
ADD . /app
```
Explanation: This copies everything in the current directory into the /app directory in the image.

COPY
Purpose: Copies new files or directories from <src> and adds them to the filesystem of the container at the path <dest>.
Example:

```dockerfile
COPY ./homework /homework
```
Explanation: This copies the homework directory from the host into the /homework directory in the image.

ENTRYPOINT
Purpose: Allows you to configure a container that will run as an executable.
Example:

```dockerfile
ENTRYPOINT ["python3", "-m", "http.server"]
```
Explanation: This configures the container to run as a simple HTTP server.

VOLUME
Purpose: Creates a mount point with the specified name and marks it as holding externally mounted volumes from native host or other containers.
Example:

```dockerfile
VOLUME /data
```
Explanation: This instruction creates a mount point in the container at /data.

USER
Purpose: Sets the user name or UID to use when running the image and for any RUN, CMD, and ENTRYPOINT instructions that follow it in the Dockerfile.
Example:

```dockerfile
USER nobody:nogroup
```
Explanation: This sets the user and group to nobody and nogroup, respectively.

WORKDIR
Purpose: Sets the working directory for any RUN, CMD, ENTRYPOINT, COPY, and ADD instructions that follow it in the Dockerfile.
Example:

```dockerfile
WORKDIR /app
```
Explanation: This sets the working directory to /app for any instructions that follow.

ARG
Purpose: Defines a variable that users can pass at build-time to the builder with the docker build command using the --build-arg <varname>=<value> flag.
Example:

```dockerfile
ARG ENV=prod
```
Explanation: This defines a variable ENV that can be overridden with a build argument.

ONBUILD
Purpose: Adds a trigger instruction that will be executed at a later time, when the image is used as the base for another build.
Example:

```dockerfile
ONBUILD RUN echo "This will run when the image is used as a base."
```
Explanation: This sets a command that will echo a message whenever the image built from this Dockerfile is used as a base in another Dockerfile.

STOPSIGNAL
Purpose: Sets the system call
