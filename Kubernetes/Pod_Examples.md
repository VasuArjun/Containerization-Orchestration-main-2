# Kubernetes Pod Examples

This document provides a collection of Kubernetes Pod configurations, each tailored for different applications and uses. These examples are great for training and learning how to set up various environments within Kubernetes.

## 1. Example Tomcat Server Pod

Deploy a Tomcat server for hosting Java applications:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: example-tomcat-pod
  labels:
    app: tomcat
    function: web-server
spec:
  containers:
  - name: tomcat-container
    image: tomcat:9.0
    ports:
    - containerPort: 8080
```
Explanation: This Pod runs a Tomcat server using the Tomcat 9.0 image and exposes port 8080 for web traffic.

2. Example Nginx Server Pod
Set up an Nginx server, ideal for serving static content or as a reverse proxy:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: example-nginx-pod
  labels:
    app: nginx
    function: web-server
spec:
  containers:
  - name: nginx-container
    image: nginx:1.19
    ports:
    - containerPort: 80
```
Explanation: Uses Nginx version 1.19 and serves content over HTTP default port 80.

3. Example Database Pod (MySQL)
Set up a MySQL database server:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: example-mysql-pod
  labels:
    app: mysql
    function: database
spec:
  containers:
  - name: mysql-container
    image: mysql:5.7
    ports:
    - containerPort: 3306
    env:
    - name: MYSQL_ROOT_PASSWORD
      value: "yourpassword"
```
Explanation: Runs MySQL 5.7, exposes port 3306, and sets the root password for the database.

4. Example Redis Pod
Deploy a Redis data store:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: example-redis-pod
  labels:
    app: redis
    function: cache
spec:
  containers:
  - name: redis-container
    image: redis:6.0
    ports:
    - containerPort: 6379
```
Explanation: This Pod runs Redis 6.0 and opens port 6379, which is the default Redis port.

5. Example PostgreSQL Pod
Deploying a PostgreSQL database:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: example-postgres-pod
  labels:
    app: postgres
    function: database
spec:
  containers:
  - name: postgres-container
    image: postgres:13
    ports:
    - containerPort: 5432
    env:
    - name: POSTGRES_PASSWORD
      value: "yourpassword"
```
Explanation: This Pod runs PostgreSQL 13, exposes port 5432, and sets the database password.

6. Example MongoDB Pod
Run a MongoDB database:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: example-mongodb-pod
  labels:
    app: mongodb
    function: database
spec:
  containers:
  - name: mongodb-container
    image: mongo:4.4
    ports:
    - containerPort: 27017
    env:
    - name: MONGO_INITDB_ROOT_USERNAME
      value: "mongoadmin"
    - name: MONGO_INITDB_ROOT_PASSWORD
      value: "mongopassword"
```
Explanation: Uses MongoDB 4.4, exposes port 27017, and sets user credentials through environment variables.

7. Example Python Application Pod
Run a simple Python Flask application:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: example-flask-pod
  labels:
    app: flask
    function: web-application
spec:
  containers:
  - name: flask-container
    image: pypy:3-7-slim
    ports:
    - containerPort: 5000
    command: ["pypy3"]
    args: ["-m", "flask", "run", "--host=0.0.0.0"]
    env:
    - name: FLASK_APP
      value: "app.py"
```
Explanation: Runs a Flask application using PyPy, accessible over port 5000.

8. Example PHP Pod
Setting up a PHP environment running Apache:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: example-php-pod
  labels:
    app: php
    function: web-application
spec:
  containers:
  - name: php-container
    image: php:7.4-apache
    ports:
    - containerPort: 80
```
Explanation: This Pod runs PHP 7.4 with Apache and is configured to serve PHP files over port 80.

9. Example .NET Core Application Pod
Run a .NET Core application:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: example-dotnet-pod
  labels:
    app: dotnet
    function: web-application
spec:
  containers:
  - name: dotnet-container
    image: mcr.microsoft.com/dotnet/core/aspnet:3.1
    ports:
    - containerPort: 80
```
Explanation: Hosts a .NET Core (ASP.NET Core 3.1) application and exposes it on port 80.

10. Example Node.js Application Pod
Deploying a Node.js application:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: example-nodejs-pod
  labels:
    app: nodejs
    function: web-application
spec:
  containers:
  - name: nodejs-container
    image: node:14
    ports:
    - containerPort: 3000
    command: ["node"]
    args: ["app.js"]
```
Explanation: Uses Node.js 14 to run an application (app.js) and exposes it on port 3000.
