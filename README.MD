````markdown
# Docker Readme

## Overview

This project utilizes Docker, a containerization platform, to streamline application development, packaging, shipping, and running processes. Docker's ability to create isolated environments makes it a valuable tool for efficient deployment and development workflows.

## Why Docker?

- **Consistency Across Environments:** Docker ensures that applications behave consistently across different environments, from local development setups to production servers. This minimizes the "it works on my machine" problem and ensures smoother deployments.
- **Isolation for Security and Bug Improvement:** By isolating applications within containers, Docker enhances security by reducing the attack surface and makes it easier to identify and fix bugs without affecting other components of the system.
- **Portability:** Docker containers can be easily moved between different environments, making it simple to transition applications from development to testing to production. This portability simplifies the deployment process and reduces the chances of configuration errors.
- **Running Multiple Applications with Different Environments:** Docker allows users to run multiple applications with different dependencies and environments on the same server. For instance, if a user needs to run two applications, one requiring Node.js v14 and the other requiring Node.js v16, Docker containers can provide isolated environments for each application, ensuring compatibility and preventing conflicts.
- **Virtual Control:** Docker provides developers with precise control over the resources allocated to each container, allowing for efficient resource management and optimization.
- **DevOps Integration:** Docker seamlessly integrates into DevOps workflows, enabling teams to automate the deployment process and promote collaboration between development and operations teams.

## Docker Workflow

1. **Docker Client:** The Docker client is a command-line tool that allows users to interact with the Docker daemon. Developers use the Docker client to build, manage, and control Docker containers and images.
2. **Docker Host (Docker Daemon):** The Docker host, also known as the Docker daemon, is responsible for managing Docker objects such as containers, images, networks, and volumes. It listens for Docker API requests and performs the necessary actions to create and manage Docker containers.

3. **Docker Registry (Docker Hub):** The Docker registry is a repository for Docker images. It stores publicly available images as well as private images uploaded by users. Docker Hub is the official Docker registry maintained by Docker, Inc., where users can discover, share, and collaborate on Docker images.

## Docker Architecture Diagram

![Docker Architecture](docker_architecture.png)

## Dockerfile for React App

```Dockerfile
# set the base image to create the image for a React app
FROM node:20-alpine

# create a user with permissions to run the app
# -S -> create a system user
# -G -> add the user to a group
# This is done to avoid running the app as the root user
# If the app runs as root, any vulnerability in the app can be exploited to gain access to the host system
# It's a good practice to run the app as a non-root user
RUN addgroup app && adduser -S -G app app

# set the user to run the app
USER app

# set the working directory to /app
WORKDIR /app

# copy package.json and package-lock.json to the working directory
# This is done before copying the rest of the files to take advantage of Docker’s cache
# If the package.json and package-lock.json files haven’t changed, Docker will use the cached dependencies
COPY package*.json ./

# sometimes the ownership of the files in the working directory is changed to the root user
# and thus the app can't access the files and throws an error -> EACCES: permission denied
# to avoid this, change the ownership of the files to the root user
USER root

# change the ownership of the /app directory to the app user
# chown -R <user>:<group> <directory>
# chown command changes the user and/or group ownership of for given file.
RUN chown -R app:app .

# change the user back to the app user
USER app

# install dependencies
RUN npm install

# copy the rest of the files to the working directory
COPY . .

# expose port 5173 to tell Docker that the container listens on the specified network ports at runtime
EXPOSE 5173

# command to run the app
CMD npm run dev
```
````

```

```

Here are the commands for building, running, tagging, and pushing a Docker image:

1. **Build Docker Image:**
   ```bash
   docker build -t foldername .
   ```

   This command builds a Docker image using the Dockerfile in the current directory (`.`) and tags it with the specified name (`foldername`).

2. **Run Docker Container:**
   ```bash
   docker run foldername
   ```

   This command runs a Docker container using the specified Docker image (`foldername`).

3. **Run Docker Container with Port Mapping:**
   ```bash
   docker run -p 5173:5173 foldername
   ```

   This command runs a Docker container with port mapping, where port `5173` of the container is mapped to port `5173` on the host machine.

4. **Tag Docker Image:**
   ```bash
   docker tag foldername username/imagename
   ```

   This command tags a Docker image with a new name (`username/imagename`). It's useful for preparing the image for publishing to a Docker registry.

5. **Push Docker Image:**
   ```bash
   docker push username/imagename
   ```

   This command pushes the tagged Docker image to a Docker registry, making it available for others to use.
