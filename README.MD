# Docker  Readme

## Overview

This project utilizes Docker to streamline application development, packaging, shipping, and running processes through containerization. Docker's isolation capabilities enhance security and promote consistent deployment and development workflows.

## Why Docker?

- **Consistency Across Environments:** Docker ensures consistent application behavior across different environments, eliminating the "it works on my machine" issue.
- **Isolation for Security and Bug Improvement:** Containerization isolates applications, enhancing security and simplifying bug identification and fixes.
- **Portability:** Docker allows easy transition of applications between development, testing, and production environments, reducing configuration errors.
- **Running Multiple Applications with Different Environments:** Docker enables running multiple applications with different dependencies on the same server, preventing conflicts.
- **Virtual Control:** Docker provides precise control over container resources for efficient management and optimization.
- **DevOps Integration:** Docker seamlessly integrates into DevOps workflows, automating deployment processes and fostering collaboration between development and operations teams.

## Docker Workflow

1. **Docker Client:** Command-line tool for building, managing, and controlling Docker containers and images.
2. **Docker Host (Docker Daemon):** Manages Docker objects like containers, images, networks, and volumes.
3. **Docker Registry (Docker Hub):** Repository for Docker images, allowing users to share, discover, and collaborate.

## Docker Architecture Diagram

![Docker Architecture](https://4.bp.blogspot.com/-rDJcsEIRfQQ/WfgYpKl-C_I/AAAAAAAAAMQ/D5xa8ydWxzIw7EnDPAJHHvDYVhxHZ3ylQCLcBGAs/s1600/docker-architecture.png)

## Dockerfile for React App

```Dockerfile
FROM node:20-alpine

RUN addgroup app && adduser -S -G app app
USER app

WORKDIR /app

COPY package*.json ./

USER root
RUN chown -R app:app .
USER app

RUN npm install

COPY . .

EXPOSE 5173

CMD npm run dev
```

## Commands

1. **Build Docker Image:**
   ```bash
   docker build -t foldername .
   ```

2. **Run Docker Container:**
   ```bash
   docker run foldername
   ```

3. **Run Docker Container with Port Mapping:**
   ```bash
   docker run -p 5173:5173 foldername
   ```

4. **Tag Docker Image:**
   ```bash
   docker tag foldername username/imagename
   ```

5. **Push Docker Image:**
   ```bash
   docker push username/imagename
   ```

Replace `foldername` with the name you used when building the Docker image, `username` with your Docker Hub username or the name of the registry, and `imagename` with the desired name for your Docker image.
```

