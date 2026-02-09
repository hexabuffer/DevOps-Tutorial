# Docker Complete Guide

---

## Table of Contents

- [Chapter 1: Introduction to Docker](#chapter-1-introduction-to-docker)
- [Chapter 2: Installing Docker](#chapter-2-installing-docker)
- [Chapter 3: Working with Docker Containers](#chapter-3-working-with-docker-containers)
- [Chapter 4: What are Docker Images](#chapter-4-what-are-docker-images)
- [Chapter 5: What is a Dockerfile](#chapter-5-what-is-a-dockerfile)
- [Chapter 6: Docker Networking](#chapter-6-docker-networking)
- [Chapter 7: Docker Volumes](#chapter-7-docker-volumes)
- [Chapter 8: Docker Compose](#chapter-8-docker-compose)
- [Chapter 9: Docker Security Best Practices](#chapter-9-docker-security-best-practices)
- [Chapter 10: Docker in Production - Orchestration with Kubernetes](#chapter-10-docker-in-production-orchestration-with-kubernetes)
- [Chapter 11: Docker Performance Optimization](#chapter-11-docker-performance-optimization)
- [Chapter 12: Docker Troubleshooting and Debugging](#chapter-12-docker-troubleshooting-and-debugging)
- [Chapter 13: Advanced Docker Concepts and Features](#chapter-13-advanced-docker-concepts-and-features)
- [Chapter 14: Docker in CI/CD Pipelines](#chapter-14-docker-in-cicd-pipelines)
- [Chapter 15: Docker and Microservices Architecture](#chapter-15-docker-and-microservices-architecture)
- [Chapter 16: Docker for Data Science and Machine Learning](#chapter-16-docker-for-data-science-and-machine-learning)
- [Chapter 17: Docker Swarm](#chapter-17-docker-swarm)

---

## Chapter 1: Introduction to Docker

### What is Docker?

Docker is an open-source platform that automates the deployment, scaling, and management of applications using containerization technology. It allows developers to package applications and their dependencies into standardized units called containers, which can run consistently across different environments.

### Key Concepts

1. **Containerization**: A lightweight form of virtualization that packages applications and their dependencies together
2. **Docker Engine**: The runtime that allows you to build and run containers
3. **Docker Image**: A read-only template used to create containers
4. **Docker Container**: A runnable instance of a Docker image
5. **Docker Hub**: A cloud-based registry for storing and sharing Docker images

### Why Use Docker?

Docker offers numerous advantages for developers and operations teams:

1. **Consistency**: Ensures applications run the same way in development, testing, and production environments
2. **Isolation**: Containers are isolated from each other and the host system, improving security and reducing conflicts
3. **Portability**: Containers can run on any system that supports Docker, regardless of the underlying infrastructure
4. **Efficiency**: Containers share the host system's OS kernel, making them more lightweight than traditional virtual machines
5. **Scalability**: Easy to scale applications horizontally by running multiple containers
6. **Version Control**: Docker images can be versioned, allowing for easy rollbacks and updates

### Docker Architecture

Docker uses a client-server architecture:

1. **Docker Client**: The primary way users interact with Docker through the command line interface (CLI)
2. **Docker Host**: The machine running the Docker daemon (dockerd)
3. **Docker Daemon**: Manages Docker objects like images, containers, networks, and volumes
4. **Docker Registry**: Stores Docker images (e.g., Docker Hub)

```
  ┌─────────────┐   ┌─────────────────────────────────────┐
  │ Docker CLI │   │      Docker Host       │
  │ (docker)  │◄───►│ ┌────────────┐   ┌───────────┐ │
  └─────────────┘   │ │  Docker  │   │ Containers│ │
          │ │  Daemon  │◄────►│  and  │ │
          │ │ (dockerd) │   │ Images  │ │
          │ └────────────┘   └───────────┘ │
          └─────────────────────────────────────┘
              ▲
              │
              ▼
          ┌─────────────────────┐
          │  Docker Registry  │
          │  (Docker Hub)   │
          └─────────────────────┘
```

### Containers vs. Virtual Machines

| Aspect | Containers | Virtual Machines |
|--------|------------|------------------|
| OS | Share host OS kernel | Run full OS and kernel |
| Resource Usage | Lightweight, minimal overhead | Higher resource usage |
| Boot Time | Seconds | Minutes |
| Isolation | Process-level isolation | Full isolation |
| Portability | Highly portable across different OSes | Less portable, OS-dependent |
| Performance | Near-native performance | Slight performance overhead |
| Storage | Typically smaller (MBs) | Larger (GBs) |

### Basic Docker Workflow

1. **Build**: Create a Dockerfile that defines your application and its dependencies
2. **Ship**: Push your Docker image to a registry like Docker Hub
3. **Run**: Pull the image and run it as a container on any Docker-enabled host

Example workflow:
```bash
# Build an image
docker build -t myapp:1.0 .

# Ship the image to Docker Hub
docker push myusername/myapp:1.0

# Run the container
docker run -d -p 8080:80 myusername/myapp:1.0
```

### Docker Components

1. **Dockerfile**: A text file containing instructions to build a Docker image
2. **Docker Compose**: A tool for defining and running multi-container Docker applications
3. **Docker Swarm**: Docker's native clustering and orchestration solution
4. **Docker Network**: Facilitates communication between Docker containers
5. **Docker Volume**: Provides persistent storage for container data

### Use Cases for Docker

1. **Microservices Architecture**: Deploy and scale individual services independently
2. **Continuous Integration/Continuous Deployment (CI/CD)**: Streamline development and deployment processes
3. **Development Environments**: Create consistent development environments across teams
4. **Application Isolation**: Run multiple versions of an application on the same host
5. **Legacy Application Migration**: Containerize legacy applications for easier management and deployment

---

## Chapter 2: Installing Docker

### Docker Editions

Before installation, understand the different Docker editions:

1. **Docker Engine - Community**: Free, open-source Docker platform suitable for developers and small teams
2. **Docker Engine - Enterprise**: Designed for enterprise development and IT teams building business-critical applications
3. **Docker Desktop**: Easy-to-install application for Mac/Windows including Docker Engine, CLI, Compose, Kubernetes

For most users, Docker Engine - Community or Docker Desktop is sufficient.

### Installing Docker on Linux

#### Method 1: Using the Docker Installation Script (Recommended for Quick Setup)

Docker provides a convenient auto-detection script:

```bash
# Download and execute installation script
wget -qO- https://get.docker.com | sh

# Start Docker service
sudo systemctl start docker

# Enable Docker on boot
sudo systemctl enable docker
```

This method is ideal for quick setups and testing environments.

#### Method 2: Manual Installation for Specific Distributions

**Ubuntu:**

```bash
# Update package index
sudo apt-get update

# Install prerequisites
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common

# Add Docker's official GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# Set up stable repository
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

# Install Docker Engine
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

**CentOS:**

```bash
# Install required packages
sudo yum install -y yum-utils

# Add Docker repository
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

# Install Docker Engine
sudo yum install docker-ce docker-ce-cli containerd.io

# Start Docker
sudo systemctl start docker
```

### Installing Docker on macOS

1. Download Docker Desktop for Mac from: https://www.docker.com/products/docker-desktop
2. Double-click the `.dmg` file and drag Docker to Applications
3. Open Docker from Applications folder
4. Follow on-screen instructions

### Installing Docker on Windows

**Windows 10 Pro/Enterprise/Education:**

1. Download Docker Desktop from: https://www.docker.com/products/docker-desktop
2. Run the installer
3. Follow the installation wizard
4. Docker Desktop starts automatically

**Windows 10 Home or older versions:**

Use Docker Toolbox with Oracle VirtualBox:
1. Download from: https://github.com/docker/toolbox/releases
2. Run installer
3. Use Docker Quickstart Terminal

### Post-Installation Steps

**Add user to docker group** (Linux):
```bash
sudo usermod -aG docker $USER
```

Log out and back in for changes to take effect.

**Verify installation:**
```bash
docker --version
docker run hello-world
```

### Docker Desktop vs Docker Engine

| Feature | Docker Desktop | Docker Engine |
|---------|----------------|---------------|
| Platform | macOS, Windows | Linux |
| GUI | Yes | No (CLI only) |
| Kubernetes | Built-in | Separate install |
| Resource Management | GUI-based | Manual configuration |
| Ideal For | Development | Production servers |

---

## Chapter 3: Working with Docker Containers

### Running Your First Container

```bash
docker run hello-world
```

This command:
1. Checks for the image locally
2. Downloads from Docker Hub if not found
3. Creates a container from the image
4. Runs the container

### Basic Docker Commands

**Listing Containers:**
```bash
# List running containers
docker ps

# List all containers (including stopped)
docker ps -a

# List only container IDs
docker ps -q
```

**Starting and Stopping Containers:**
```bash
# Start a stopped container
docker start <container_id>

# Stop a running container
docker stop <container_id>

# Restart a container
docker restart <container_id>
```

**Removing Containers:**
```bash
# Remove a stopped container
docker rm <container_id>

# Force remove a running container
docker rm -f <container_id>

# Remove all stopped containers
docker container prune
```

### Running Containers in Different Modes

**Detached Mode** (background):
```bash
docker run -d nginx
```

**Interactive Mode:**
```bash
# Interactive with terminal
docker run -it ubuntu bash

# Attach to running container
docker attach <container_id>
```

**Port Mapping:**
```bash
# Map host port 8080 to container port 80
docker run -d -p 8080:80 nginx

# Map random host port
docker run -d -P nginx
```

### Working with Container Logs

```bash
# View logs
docker logs <container_id>

# Follow log output
docker logs -f <container_id>

# Show last 100 lines
docker logs --tail 100 <container_id>

# Show logs with timestamps
docker logs -t <container_id>
```

### Executing Commands in Running Containers

```bash
# Execute command
docker exec <container_id> ls /app

# Interactive shell
docker exec -it <container_id> bash
```

### Practical Example: Running an Apache Container

```bash
# Run Apache in background with port mapping
docker run -d --name my-apache -p 8080:80 httpd:2.4

# Verify it's running
docker ps

# Access via browser at http://localhost:8080

# View logs
docker logs my-apache

# Stop and remove
docker stop my-apache
docker rm my-apache
```

### Container Resource Management

**Limiting Memory:**
```bash
# Limit to 512MB
docker run -m 512m nginx

# Limit with reservation
docker run -m 512m --memory-reservation 256m nginx
```

**Limiting CPU:**
```bash
# Limit to 1.5 CPUs
docker run --cpus=1.5 nginx

# Set CPU shares (relative weight)
docker run --cpu-shares=512 nginx
```

### Container Networking

**Listing Networks:**
```bash
docker network ls
```

**Creating a Network:**
```bash
docker network create my-network
```

**Connecting Container to Network:**
```bash
docker run -d --name web --network my-network nginx
```

### Data Persistence with Volumes

**Creating a Volume:**
```bash
docker volume create my-volume
```

**Running Container with Volume:**
```bash
docker run -d -v my-volume:/data nginx
```

### Container Health Checks

```dockerfile
HEALTHCHECK --interval=30s --timeout=3s \
  CMD curl -f http://localhost/ || exit 1
```

### Cleaning Up

```bash
# Remove stopped containers
docker container prune

# Remove unused images
docker image prune

# Remove all unused objects
docker system prune

# Remove everything (including volumes)
docker system prune -a --volumes
```

---

## Chapter 4: What are Docker Images

### Key Concepts

1. **Layers**: Images are composed of multiple layers, each representing a set of changes to the filesystem
2. **Base Image**: The foundation of an image, typically a minimal operating system
3. **Parent Image**: An image that your image is built upon
4. **Image Tags**: Labels used to version and identify images
5. **Image ID**: A unique identifier for each image

### Working with Docker Images

**Listing Images:**
```bash
docker images

# or
docker image ls

# Show all images including intermediates
docker images -a
```

**Pulling Images from Docker Hub:**
```bash
# Pull latest version
docker pull nginx

# Pull specific version
docker pull nginx:1.21

# Pull from specific registry
docker pull gcr.io/google-containers/nginx
```

**Running Containers from Images:**
```bash
docker run nginx:latest
```

**Image Information:**
```bash
# Detailed information
docker inspect nginx:latest

# View image history
docker history nginx:latest
```

**Removing Images:**
```bash
# Remove specific image
docker rmi nginx:latest

# Remove by ID
docker rmi <image_id>

# Force remove
docker rmi -f <image_id>

# Remove unused images
docker image prune
```

### Building Custom Images

**Using a Dockerfile:**

Create a `Dockerfile`:
```dockerfile
FROM ubuntu:20.04
RUN apt-get update && apt-get install -y nginx
COPY index.html /var/www/html/
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

Build the image:
```bash
docker build -t my-nginx:1.0 .
```

**Building from a Running Container:**
```bash
# Make changes to a running container
docker exec -it <container_id> bash

# Commit changes to new image
docker commit <container_id> my-custom-image:1.0
```

### Image Tagging

```bash
# Tag an image
docker tag nginx:latest myusername/nginx:v1.0

# Tag with multiple tags
docker tag myimage:latest myimage:v1.0
docker tag myimage:latest myimage:stable
```

### Pushing Images to Docker Hub

```bash
# Login to Docker Hub
docker login

# Push image
docker push myusername/nginx:v1.0

# Logout
docker logout
```

### Image Layers and Caching

Docker images are built in layers. Each instruction in a Dockerfile creates a new layer:
- Layers are cached and reused when possible
- Changing one layer invalidates cache for all subsequent layers
- Order Dockerfile instructions from least to most frequently changing

### Multi-stage Builds

Reduce image size by using multiple FROM statements:

```dockerfile
# Build stage
FROM golang:1.16 AS builder
WORKDIR /app
COPY . .
RUN go build -o main .

# Production stage
FROM alpine:latest
WORKDIR /root/
COPY --from=builder /app/main .
CMD ["./main"]
```

### Image Scanning and Security

```bash
# Scan image for vulnerabilities (Docker Desktop)
docker scan nginx:latest

# Using Trivy
trivy image nginx:latest
```

### Best Practices for Working with Images

1. **Use official images** as base images when possible
2. **Keep images small** - use Alpine-based images
3. **Use specific tags** - avoid `latest` in production
4. **One process per container** - follow single responsibility principle
5. **Use .dockerignore** - exclude unnecessary files
6. **Don't store secrets** in images
7. **Scan for vulnerabilities** regularly
8. **Use multi-stage builds** to reduce size

### Image Management and Cleanup

```bash
# View disk usage
docker system df

# Remove unused images
docker image prune

# Remove all unused images
docker image prune -a

# Remove images by pattern
docker images | grep "pattern" | awk '{print $3}' | xargs docker rmi
```

---

## Chapter 5: What is a Dockerfile

### Anatomy of a Dockerfile

A Dockerfile consists of:
1. Base image declaration
2. Metadata and label information
3. Environment setup
4. File and directory operations
5. Dependency installation
6. Application code copying
7. Execution command specification

### Dockerfile Instructions

#### FROM
Specifies the base image:
```dockerfile
FROM ubuntu:20.04

# Multi-stage build
FROM node:14 AS build
```

#### LABEL
Adds metadata:
```dockerfile
LABEL maintainer="user@example.com"
LABEL version="1.0"
LABEL description="My application"
```

#### ENV
Sets environment variables:
```dockerfile
ENV NODE_ENV=production
ENV PORT=3000
ENV APP_HOME=/app
```

#### WORKDIR
Sets working directory:
```dockerfile
WORKDIR /app
# All subsequent commands run from /app
```

#### COPY and ADD
Copy files into the image:
```dockerfile
# COPY (preferred for simple file copying)
COPY package.json .
COPY src/ /app/src/

# ADD (has additional features like tar extraction)
ADD https://example.com/file.tar.gz /tmp/
ADD archive.tar.gz /app/
```

**Difference**: Use `COPY` unless you need ADD's special features (URL download, tar extraction).

#### RUN
Execute commands during build:
```dockerfile
# Shell form
RUN apt-get update && apt-get install -y nginx

# Exec form
RUN ["apt-get", "install", "-y", "nginx"]

# Multi-line for readability
RUN apt-get update \
    && apt-get install -y \
        nginx \
        curl \
        vim \
    && rm -rf /var/lib/apt/lists/*
```

#### CMD
Default command when container starts:
```dockerfile
# Exec form (preferred)
CMD ["nginx", "-g", "daemon off;"]

# Shell form
CMD nginx -g "daemon off;"

# As parameters to ENTRYPOINT
CMD ["--help"]
```

#### ENTRYPOINT
Configure container as executable:
```dockerfile
ENTRYPOINT ["python", "app.py"]

# With CMD as default arguments
ENTRYPOINT ["python", "app.py"]
CMD ["--port", "8000"]
```

**Difference**: `CMD` can be overridden easily, `ENTRYPOINT` sets the main command.

#### EXPOSE
Document which ports the container listens on:
```dockerfile
EXPOSE 80
EXPOSE 443
EXPOSE 8080/tcp
```

Note: EXPOSE doesn't actually publish ports; use `-p` flag with `docker run`.

#### VOLUME
Create mount points:
```dockerfile
VOLUME /data
VOLUME ["/var/log", "/var/db"]
```

#### ARG
Define build-time variables:
```dockerfile
ARG VERSION=latest
ARG BUILD_DATE

FROM ubuntu:${VERSION}
LABEL build_date=${BUILD_DATE}
```

Use during build:
```bash
docker build --build-arg VERSION=20.04 --build-arg BUILD_DATE=$(date) .
```

### Best Practices for Writing Dockerfiles

1. **Use multi-stage builds** to create smaller final images:
```dockerfile
FROM node:14 AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html
```

2. **Minimize the number of layers** - combine commands where possible

3. **Leverage build cache** - order instructions from least to most frequently changing

4. **Use `.dockerignore`** to exclude unnecessary files:
```
node_modules
.git
.env
*.md
```

5. **Don't install unnecessary packages** - keep the image lean

6. **Use specific tags** - avoid `latest` for reproducibility:
```dockerfile
FROM node:14.17.0-alpine
```

7. **Set the `WORKDIR`** - always use WORKDIR instead of `RUN cd`:
```dockerfile
WORKDIR /app
```

8. **Use `COPY` instead of `ADD`** unless you need ADD's extra features

9. **Use environment variables** for flexibility:
```dockerfile
ENV APP_VERSION=1.0.0
ENV APP_PORT=8080
```

10. **Run as non-root user** for security:
```dockerfile
RUN useradd -m appuser
USER appuser
```

### Advanced Dockerfile Concepts

**Health Checks:**
```dockerfile
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD curl -f http://localhost/ || exit 1
```

**Shell and Exec Forms:**
```dockerfile
# Shell form (runs in shell, allows variable substitution)
RUN echo $HOME

# Exec form (doesn't invoke shell, preferred for CMD/ENTRYPOINT)
CMD ["nginx", "-g", "daemon off;"]
```

**BuildKit:**
Enable BuildKit for improved build performance:
```bash
DOCKER_BUILDKIT=1 docker build .
```

---

## Chapter 6: Docker Networking

### Docker Network Drivers

Docker uses pluggable networking architecture with built-in drivers:

1. **Bridge**: Default driver for standalone containers
2. **Host**: Removes network isolation, container uses host's network
3. **Overlay**: Enables multi-host networking for swarm services
4. **MacVLAN**: Assigns MAC address to container, appears as physical device
5. **None**: Disables all networking
6. **Network plugins**: Third-party network drivers

### Working with Docker Networks

**Listing Networks:**
```bash
docker network ls
```

**Inspecting Networks:**
```bash
docker network inspect bridge
docker network inspect <network_name>
```

**Creating a Network:**
```bash
# Create bridge network
docker network create my-network

# Create with specific driver
docker network create --driver bridge my-bridge-network

# Create with subnet
docker network create --subnet=172.18.0.0/16 my-network

# Create overlay network (for swarm)
docker network create --driver overlay my-overlay-network
```

**Connecting Containers to Networks:**
```bash
# Run container on specific network
docker run -d --name web --network my-network nginx

# Connect running container to network
docker network connect my-network <container_name>

# Connect with specific IP
docker network connect --ip 172.18.0.10 my-network <container_name>
```

**Disconnecting Containers from Networks:**
```bash
docker network disconnect my-network <container_name>
```

**Removing Networks:**
```bash
docker network rm my-network

# Remove all unused networks
docker network prune
```

### Deep Dive into Network Drivers

#### Bridge Networks
Default network mode, provides isolation:

```bash
# Create custom bridge network
docker network create --driver bridge \
  --subnet=172.20.0.0/16 \
  --gateway=172.20.0.1 \
  my-bridge

# Run containers on bridge network
docker run -d --name db --network my-bridge mysql
docker run -d --name web --network my-bridge nginx

# Containers can communicate using container names
```

**Benefits:**
- Containers on same bridge can communicate
- Isolated from containers on other networks
- Automatic DNS resolution by container name

#### Host Networks
Container shares host's network stack:

```bash
docker run -d --network host nginx
```

**Use cases:**
- Maximum network performance needed
- Container needs to handle many ports
- No port mapping overhead

**Limitations:**
- No port mapping (container uses host ports directly)
- Less isolation
- Port conflicts possible

#### Overlay Networks
Connect multiple Docker daemons (for swarm):

```bash
# Initialize swarm
docker swarm init

# Create overlay network
docker network create --driver overlay my-overlay

# Deploy service on overlay
docker service create --name web --network my-overlay nginx
```

**Use cases:**
- Multi-host container communication
- Swarm services
- Distributed applications

#### MacVLAN Networks
Container appears as physical device on network:

```bash
docker network create -d macvlan \
  --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 \
  -o parent=eth0 \
  my-macvlan

docker run -d --network my-macvlan --ip=192.168.1.100 nginx
```

**Use cases:**
- Direct network access required
- Legacy applications expecting physical network
- Network monitoring applications

### Network Troubleshooting

```bash
# Inspect container network settings
docker inspect <container_name> | grep -A 20 NetworkSettings

# Test connectivity between containers
docker exec <container1> ping <container2>

# Check port bindings
docker port <container_name>

# Use network debugging tools
docker run --rm -it nicolaka/netshoot

# View network traffic
docker run --rm --net container:<container_name> nicolaka/netshoot tcpdump
```

### Best Practices

1. **Use custom bridge networks** instead of default bridge
2. **Use network aliases** for service discovery
3. **Limit network exposure** - only expose necessary ports
4. **Use overlay networks** for multi-host deployments
5. **Implement network segmentation** for security
6. **Monitor network performance**
7. **Use DNS for service discovery**

### Advanced Topics

**Network Encryption:**
```bash
# Create encrypted overlay network
docker network create --driver overlay --opt encrypted my-secure-network
```

**Network Plugins:**
Third-party plugins for advanced networking:
- Weave
- Calico
- Flannel

**Service Discovery:**
Containers can discover each other using:
- DNS (container names)
- Environment variables
- Service mesh (Consul, etcd)

---

## Chapter 7: Docker Volumes

Docker volumes are the preferred mechanism for persisting data generated by and used by Docker containers. While containers can create, update, and delete files, those changes are lost when the container is removed. Volumes provide the ability to connect specific filesystem paths of the container back to the host machine.

### Why Use Docker Volumes?

1. **Data Persistence**: Data survives container deletion
2. **Data Sharing**: Share data between multiple containers
3. **Performance**: Better I/O performance than bind mounts
4. **Backup and Migration**: Easier to backup and migrate
5. **Decoupling**: Separate data from container lifecycle

### Types of Docker Volumes

#### 1. Named Volumes
Managed by Docker, stored in Docker's directory:

```bash
# Create named volume
docker volume create my-volume

# Use in container
docker run -d -v my-volume:/data nginx

# List volumes
docker volume ls

# Inspect volume
docker volume inspect my-volume
```

#### 2. Anonymous Volumes
Created automatically without a name:

```bash
# Docker creates anonymous volume
docker run -d -v /data nginx
```

#### 3. Bind Mounts
Mount host directory into container:

```bash
# Mount host directory
docker run -d -v /host/path:/container/path nginx

# Mount current directory
docker run -d -v $(pwd):/app nginx

# Read-only bind mount
docker run -d -v /host/path:/container/path:ro nginx
```

**Comparison:**

| Type | Storage Location | Management | Use Case |
|------|------------------|------------|----------|
| Named Volume | Docker directory | Docker managed | Production data |
| Anonymous Volume | Docker directory | Docker managed | Temporary data |
| Bind Mount | Any host path | User managed | Development, config files |

### Working with Docker Volumes

**Listing Volumes:**
```bash
docker volume ls

# Show size
docker volume ls --format "table {{.Name}}\t{{.Size}}"
```

**Inspecting Volumes:**
```bash
docker volume inspect my-volume
```

**Removing Volumes:**
```bash
# Remove specific volume
docker volume rm my-volume

# Remove all unused volumes
docker volume prune

# Remove with confirmation
docker volume prune -f
```

**Backing Up Volumes:**
```bash
# Backup volume to tar file
docker run --rm -v my-volume:/data -v $(pwd):/backup \
  ubuntu tar czf /backup/backup.tar.gz -C /data .
```

**Restoring Volumes:**
```bash
# Restore from tar file
docker run --rm -v my-volume:/data -v $(pwd):/backup \
  ubuntu tar xzf /backup/backup.tar.gz -C /data
```

### Volume Drivers

Docker supports volume drivers for various storage backends:

```bash
# Use specific driver
docker volume create --driver local \
  --opt type=nfs \
  --opt o=addr=192.168.1.1,rw \
  --opt device=:/path/to/dir \
  nfs-volume
```

Common volume drivers:
- **local**: Default driver (local filesystem)
- **nfs**: Network File System
- **cifs**: Common Internet File System
- **azure**: Azure File Storage
- **aws**: Amazon EBS

### Best Practices for Using Docker Volumes

1. **Use named volumes** for production data
2. **Use bind mounts** for development only
3. **Never store data in containers** - always use volumes
4. **Backup volumes regularly**
5. **Use volume labels** for organization
6. **Clean up unused volumes** periodically
7. **Use read-only volumes** when possible for security
8. **Consider using volume drivers** for cloud storage

### Advanced Volume Concepts

**1. Read-Only Volumes:**
```bash
docker run -d -v my-volume:/data:ro nginx
```

**2. Tmpfs Mounts** (memory-based):
```bash
docker run -d --tmpfs /tmp nginx

# With size limit
docker run -d --tmpfs /tmp:size=100m nginx
```

**3. Sharing Volumes Between Containers:**
```bash
# Create and use volume in first container
docker run -d --name app1 -v shared-volume:/data nginx

# Use same volume in second container
docker run -d --name app2 -v shared-volume:/data nginx
```

**4. Volume Plugins:**
- REX-Ray
- Portworx
- NetApp

### Troubleshooting Volume Issues

```bash
# Check volume mount points
docker inspect <container> | grep -A 10 Mounts

# Check volume usage
docker system df -v

# Find which containers use a volume
docker ps -a --filter volume=my-volume

# Access volume data directly
docker run --rm -it -v my-volume:/data ubuntu bash
```

---

## Chapter 8: Docker Compose

Docker Compose is a powerful tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application's services, networks, and volumes.

**Note**: Docker Compose is now integrated into Docker CLI. Use `docker compose` instead of `docker-compose`.

### Key Benefits of Docker Compose

1. **Simplified multi-container deployment**
2. **Environment reproducibility**
3. **Easy configuration management**
4. **Version control** for infrastructure
5. **Simplified networking** between containers
6. **Single command** to start/stop entire application

### The docker-compose.yml File

Basic structure:

```yaml
version: '3.8'

services:
  service_name:
    image: image_name
    # or build: ./path
    ports:
      - "host:container"
    environment:
      KEY: value
    volumes:
      - volume_name:/path
    networks:
      - network_name
    depends_on:
      - other_service

networks:
  network_name:

volumes:
  volume_name:
```

### Key Concepts in Docker Compose

- **Services**: Containers to run
- **Networks**: Communication between services
- **Volumes**: Data persistence
- **Environment**: Configuration variables
- **Dependencies**: Service startup order

### Basic Docker Compose Commands

```bash
# Start services
docker compose up

# Start in background
docker compose up -d

# Build and start
docker compose up --build

# Stop services
docker compose stop

# Stop and remove containers
docker compose down

# Stop and remove everything including volumes
docker compose down -v

# View logs
docker compose logs

# Follow logs
docker compose logs -f

# View logs for specific service
docker compose logs service_name

# List running services
docker compose ps

# Execute command in service
docker compose exec service_name bash

# Scale services
docker compose up -d --scale web=3
```

### Advanced Docker Compose Features

**1. Environment Variables:**

`.env` file:
```
DB_PASSWORD=secret
APP_PORT=8000
```

`docker-compose.yml`:
```yaml
services:
  db:
    environment:
      POSTGRES_PASSWORD: ${DB_PASSWORD}
  web:
    ports:
      - "${APP_PORT}:80"
```

**2. Extending Services:**

`docker-compose.override.yml`:
```yaml
version: '3.8'
services:
  web:
    environment:
      DEBUG: "true"
```

**3. Healthchecks:**
```yaml
services:
  web:
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 40s
```

### Practical Example: WordPress with MySQL

```yaml
version: '3.8'

services:
  db:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: somewordpress
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress

  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    ports:
      - "8000:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress

volumes:
  db_data:
```

**Components explained:**

1. **db service**:
   - Uses MySQL 5.7 image
   - Creates named volume for data persistence
   - Sets environment variables for database configuration

2. **wordpress service**:
   - Depends on db (starts after database)
   - Maps port 8000 on host to port 80 in container
   - Connects to database using service name `db`

3. **volumes**:
   - `db_data` persists MySQL data

**To run:**
```bash
docker compose up -d
# Access at http://localhost:8000
```

### Practical Example 2: Flask App with Redis and Nginx

```yaml
version: '3.8'

services:
  redis:
    image: redis:alpine
    networks:
      - backend

  web:
    build: ./web
    ports:
      - "5000:5000"
    environment:
      - REDIS_HOST=redis
    networks:
      - backend
      - frontend
    depends_on:
      - redis

  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
    networks:
      - frontend
    depends_on:
      - web

networks:
  frontend:
  backend:
```

### Best Practices for Docker Compose

1. **Use version control** for docker-compose.yml
2. **Use .env files** for environment-specific configuration
3. **Don't store secrets** in docker-compose.yml
4. **Use health checks** to ensure service readiness
5. **Use depends_on with condition** for proper startup order
6. **Pin image versions** - avoid `latest` tag
7. **Use named volumes** for data persistence
8. **Organize files** - separate dev and production configs
9. **Use profiles** for optional services

### Scaling Services

```bash
# Scale web service to 3 instances
docker compose up -d --scale web=3

# With load balancer
docker compose up -d --scale web=3 --scale nginx=1
```

### Networking in Docker Compose

```yaml
# Custom networks
networks:
  frontend:
    driver: bridge
  backend:
    driver: bridge
    internal: true  # No external access

# Connect services to networks
services:
  web:
    networks:
      - frontend
      - backend
```

### Volumes in Docker Compose

```yaml
volumes:
  # Named volume
  db_data:
  
  # With driver options
  nfs_volume:
    driver: local
    driver_opts:
      type: nfs
      o: addr=192.168.1.1,rw
      device: ":/path/to/dir"

  # External volume
  external_volume:
    external: true
```

---

## Chapter 9: Docker Security Best Practices

Security is critical when working with Docker, especially in production environments.

### 1. Keep Docker Updated

```bash
# Check Docker version
docker version

# Update Docker (Ubuntu)
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

**Why**: Security patches and bug fixes

### 2. Use Official Images

```bash
# Prefer official images
docker pull nginx  # Official

# Check image source
docker pull library/nginx
```

**Why**: Official images are maintained and regularly scanned for vulnerabilities

### 3. Scan Images for Vulnerabilities

```bash
# Docker scan
docker scan nginx:latest

# Trivy
trivy image nginx:latest

# Snyk
snyk container test nginx:latest
```

### 4. Limit Container Resources

```bash
# Memory limit
docker run -m 512m nginx

# CPU limit
docker run --cpus=1.5 nginx

# In Compose:
```yaml
services:
  web:
    image: nginx
    deploy:
      resources:
        limits:
          cpus: '0.5'
          memory: 512M
        reservations:
          cpus: '0.25'
          memory: 256M
```

### 5. Use Non-Root Users

Dockerfile:
```dockerfile
FROM nginx:alpine

# Create non-root user
RUN addgroup -g 1001 appuser && \
    adduser -D -u 1001 -G appuser appuser

# Switch to non-root user
USER appuser

CMD ["nginx", "-g", "daemon off;"]
```

**Why**: Limits damage if container is compromised

### 6. Use Secret Management

```bash
# Docker secrets (Swarm)
echo "db_password" | docker secret create db_pass -

# Use in service
docker service create --secret db_pass myapp
```

In application, read from `/run/secrets/db_pass`

**Don't**:
```dockerfile
# ❌ DON'T hardcode secrets
ENV DB_PASSWORD=secretpassword
```

### 7. Enable Content Trust

```bash
# Enable Docker Content Trust
export DOCKER_CONTENT_TRUST=1

# Now only signed images can be pulled
docker pull nginx
```

### 8. Use Read-Only Containers

```bash
# Read-only root filesystem
docker run --read-only nginx

# With tmpfs for writable directories
docker run --read-only --tmpfs /tmp nginx
```

### 9. Implement Network Segmentation

```yaml
version: '3.8'
services:
  web:
    networks:
      - frontend
  
  app:
    networks:
      - frontend
      - backend
  
  db:
    networks:
      - backend

networks:
  frontend:
  backend:
    internal: true  # No internet access
```

### 10. Regular Security Audits

```bash
# Docker bench security
docker run -it --net host --pid host --cap-add audit_control \
  -v /var/lib:/var/lib \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v /etc:/etc --label docker_bench_security \
  docker/docker-bench-security

# Check for CVEs
docker scout cves nginx:latest
```

### 11. Use Security-Enhanced Linux (SELinux) or AppArmor

**SELinux:**
```bash
# Run with SELinux label
docker run --security-opt label=level:s0:c100,c200 nginx
```

**AppArmor:**
```bash
# Use AppArmor profile
docker run --security-opt apparmor=docker-default nginx
```

### 12. Implement Logging and Monitoring

```bash
# Configure logging driver
docker run --log-driver=syslog nginx

# View logs
docker logs container_id
```

**Compose:**
```yaml
services:
  web:
    logging:
      driver: "json-file"
      options:
        max-size: "10m"
        max-file: "3"
```

### Additional Security Practices

1. **Minimize attack surface** - only install necessary packages
2. **Use .dockerignore** - prevent sensitive files in image
3. **Drop capabilities** - remove unnecessary Linux capabilities:
```bash
docker run --cap-drop=ALL --cap-add=NET_BIND_SERVICE nginx
```

4. **Limit syscalls** with seccomp:
```bash
docker run --security-opt seccomp=profile.json nginx
```

5. **No privileged containers** unless absolutely necessary:
```bash
# ❌ Avoid
docker run --privileged nginx
```

6. **Use private registries** for sensitive images
7. **Implement RBAC** (Role-Based Access Control)
8. **Regular image updates and rebuilds**
9. **Monitor container behavior** for anomalies
10. **Use minimal base images** (Alpine, Distroless)

---

## Chapter 10: Docker in Production - Orchestration with Kubernetes

Kubernetes (K8s) is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications. It provides robust features for running containers in production.

### Key Kubernetes Concepts

1. **Pods**: Smallest deployable units, containing one or more containers
2. **Services**: Abstract way to expose applications running on Pods
3. **Deployments**: Describe desired state for Pods and ReplicaSets
4. **Namespaces**: Virtual clusters within a physical cluster
5. **ConfigMaps**: Configuration data for containers
6. **Secrets**: Sensitive data storage
7. **Ingress**: HTTP/HTTPS routing to services
8. **Persistent Volumes**: Storage abstraction

### Setting Up a Kubernetes Cluster

**Using Minikube** (local development):
```bash
# Install minikube
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

# Start cluster
minikube start

# Verify
kubectl get nodes
```

**Using kubeadm** (production):
```bash
# Initialize master node
sudo kubeadm init --pod-network-cidr=10.244.0.0/16

# Join worker nodes
kubeadm join <master-ip>:6443 --token <token> --discovery-token-ca-cert-hash <hash>
```

### Deploying a Docker Container to Kubernetes

**1. Create Deployment:**

`deployment.yaml`:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.21
        ports:
        - containerPort: 80
```

Apply:
```bash
kubectl apply -f deployment.yaml
```

**2. Create Service:**

`service.yaml`:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: LoadBalancer
```

Apply:
```bash
kubectl apply -f service.yaml
```

**3. Verify Deployment:**
```bash
kubectl get deployments
kubectl get pods
kubectl get services
```

### Scaling in Kubernetes

```bash
# Manual scaling
kubectl scale deployment nginx-deployment --replicas=5

# Auto-scaling
kubectl autoscale deployment nginx-deployment --min=2 --max=10 --cpu-percent=80
```

### Rolling Updates

```bash
# Update image
kubectl set image deployment/nginx-deployment nginx=nginx:1.22

# Check rollout status
kubectl rollout status deployment/nginx-deployment

# Rollback if needed
kubectl rollout undo deployment/nginx-deployment
```

### Monitoring and Logging

**Kubernetes Dashboard:**
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
kubectl proxy
```

**View logs:**
```bash
kubectl logs <pod-name>
kubectl logs -f <pod-name>  # Follow logs
kubectl logs <pod-name> -c <container-name>  # Specific container
```

### Persistent Storage in Kubernetes

**PersistentVolume and PersistentVolumeClaim:**

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql
spec:
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mysql:5.7
        volumeMounts:
        - name: mysql-storage
          mountPath: /var/lib/mysql
      volumes:
      - name: mysql-storage
        persistentVolumeClaim:
          claimName: mysql-pvc
```

### Kubernetes Networking

**Network Policies:**
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-frontend
spec:
  podSelector:
    matchLabels:
      app: backend
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: frontend
```

### Kubernetes Secrets

```bash
# Create secret
kubectl create secret generic db-secret \
  --from-literal=username=admin \
  --from-literal=password=secretpass

# Use in deployment
```yaml
env:
- name: DB_USERNAME
  valueFrom:
    secretKeyRef:
      name: db-secret
      key: username
```

### Helm: The Kubernetes Package Manager

```bash
# Install Helm
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash

# Add repository
helm repo add bitnami https://charts.bitnami.com/bitnami

# Install chart
helm install my-release bitnami/nginx

# List releases
helm list

# Uninstall
helm uninstall my-release
```

### Best Practices for Kubernetes in Production

1. **Use namespaces** for resource isolation
2. **Set resource limits** on all containers
3. **Implement health checks** (liveness and readiness probes)
4. **Use ConfigMaps** for configuration
5. **Store secrets securely** using Kubernetes Secrets or external vault
6. **Implement RBAC** for access control
7. **Use Ingress** for HTTP routing
8. **Monitor** with Prometheus and Grafana
9. **Use Helm** for package management
10. **Implement CI/CD** pipelines

---

## Chapter 11: Docker Performance Optimization

### 1. Optimizing Docker Images

**Use Multi-Stage Builds:**
```dockerfile
# Build stage
FROM node:14 AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Final stage
FROM node:14-alpine
WORKDIR /app
COPY --from=build /app/dist ./dist
COPY package*.json ./
RUN npm install --production
CMD ["node", "dist/server.js"]
```

**Minimize Layer Count:**
```dockerfile
# ❌ Multiple layers
RUN apt-get update
RUN apt-get install -y nginx
RUN apt-get install -y curl

# ✅ Single layer
RUN apt-get update && \
    apt-get install -y nginx curl && \
    rm -rf /var/lib/apt/lists/*
```

**Use .dockerignore:**
```
node_modules
.git
.env
*.log
.DS_Store
```

### 2. Container Resource Management

**Set Memory and CPU Limits:**
```bash
docker run -m 512m --cpus=1.5 myapp

# In Compose:
```yaml
deploy:
  resources:
    limits:
      cpus: '1.5'
      memory: 512M
    reservations:
      cpus: '0.5'
      memory: 256M
```

**Use --cpuset-cpus for CPU Pinning:**
```bash
docker run --cpuset-cpus="0,1" myapp
```

### 3. Networking Optimization

**Use Host Networking Mode** (when appropriate):
```bash
docker run --network host myapp
```

**Optimize DNS Resolution:**
```bash
# Set custom DNS
docker run --dns 8.8.8.8 --dns 8.8.4.4 myapp
```

### 4. Storage Optimization

**Use Volumes Instead of Bind Mounts:**
```bash
# Better performance
docker run -v myvolume:/data myapp

# vs bind mount
docker run -v /host/path:/data myapp
```

**Consider Using tmpfs Mounts:**
```bash
docker run --tmpfs /tmp:size=100m myapp
```

### 5. Logging and Monitoring

**Use JSON-file Logging Driver with Limits:**
```yaml
logging:
  driver: "json-file"
  options:
    max-size: "10m"
    max-file: "3"
```

**Implement Proper Monitoring:**
```bash
# Use cAdvisor
docker run -d \
  --name=cadvisor \
  -p 8080:8080 \
  -v /:/rootfs:ro \
  -v /var/run:/var/run:ro \
  -v /sys:/sys:ro \
  -v /var/lib/docker/:/var/lib/docker:ro \
  google/cadvisor:latest
```

### 6. Docker Daemon Optimization

**Adjust Storage Driver:**
```json
{
  "storage-driver": "overlay2"
}
```

**Enable Live Restore:**
```json
{
  "live-restore": true
}
```

### 7. Application-Level Optimization

**Use Alpine-Based Images:**
```dockerfile
# From 900MB
FROM node:14

# To 100MB
FROM node:14-alpine
```

**Optimize Application Code:**
- Use connection pooling
- Implement caching
- Minimize dependencies
- Use production builds

### 8. Benchmarking and Profiling

**Use Docker's Built-in Stats:**
```bash
docker stats

# For specific container
docker stats container_name
```

**Benchmark with Apache Bench:**
```bash
docker run --rm jordi/ab -n 1000 -c 10 http://container_ip/
```

### 9. Orchestration-Level Optimization (Kubernetes)

**Use Horizontal Pod Autoscaler:**
```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: myapp-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: myapp
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
```

**Implement Proper Liveness and Readiness Probes:**
```yaml
livenessProbe:
  httpGet:
    path: /health
    port: 8080
  initialDelaySeconds: 30
  periodSeconds: 10

readinessProbe:
  httpGet:
    path: /ready
    port: 8080
  initialDelaySeconds: 5
  periodSeconds: 5
```

---

## Chapter 12: Docker Troubleshooting and Debugging

### 1. Container Lifecycle Issues

**Container Won't Start:**
```bash
# View container logs
docker logs container_name

# Inspect container details
docker inspect container_name

# Check container status
docker ps -a
```

**Container Exits Immediately:**
```bash
# Run in interactive mode
docker run -it image_name bash

# Check ENTRYPOINT and CMD
docker inspect image_name | grep -A 5 Cmd
```

### 2. Networking Issues

**Container Can't Connect to Network:**
```bash
# Inspect network settings
docker network inspect bridge

# Check container's network
docker inspect container_name | grep -A 20 NetworkSettings

# Use debugging container
docker run --rm -it nicolaka/netshoot
```

**Port Mapping Issues:**
```bash
# Check port mappings
docker port container_name

# Verify firewall settings
sudo iptables -L -n

# Test port on container
docker exec container_name nc -zv localhost 80
```

### 3. Storage and Volume Issues

**Data Persistence Problems:**
```bash
# List volumes
docker volume ls

# Inspect volume
docker volume inspect volume_name

# Check mounts
docker inspect container_name | grep -A 10 Mounts
```

**Disk Space Issues:**
```bash
# Check Docker disk usage
docker system df

# Remove unused data
docker system prune

# Identify large images
docker images --format "table {{.Repository}}\t{{.Tag}}\t{{.Size}}" | sort -k 3 -h
```

### 4. Resource Constraints

**Container Using Too Much CPU/Memory:**
```bash
# Monitor resource usage
docker stats

# Set resource limits
docker run -m 512m --cpus=1.0 myapp

# Update limits for running container
docker update --memory 1g --cpus 2 container_name
```

### 5. Image-Related Issues

**Image Pull Failures:**
```bash
# Check Docker Hub status
curl -s https://status.docker.com/

# Verify login
docker login

# Pull with verbose output
docker pull --debug nginx
```

**Image Build Failures:**
```bash
# Build with verbose output
docker build --progress=plain -t myapp .

# Check Dockerfile syntax
docker run --rm -i hadolint/hadolint < Dockerfile
```

### 6. Docker Daemon Issues

**Docker Daemon Won't Start:**
```bash
# Check daemon status
sudo systemctl status docker

# View daemon logs
sudo journalctl -u docker.service

# Restart daemon
sudo systemctl restart docker
```

### 7. Debugging Techniques

**Interactive Debugging:**
```bash
# Start shell in running container
docker exec -it container_name bash

# Run new container with shell
docker run -it --entrypoint /bin/bash image_name
```

**Using Docker Events:**
```bash
docker events

# Filter specific events
docker events --filter event=start --filter event=die
```

**Logging:**
```bash
# View logs
docker logs container_name

# Follow logs
docker logs -f container_name

# Adjust logging driver
docker run --log-driver json-file --log-opt max-size=10m myapp
```

### 8. Performance Debugging

**Identifying Bottlenecks:**
```bash
# Monitor resources
docker stats

# Profile processes
docker exec container_name top

# Use cAdvisor
docker run -d -p 8080:8080 \
  -v /:/rootfs:ro \
  -v /var/run:/var/run:ro \
  google/cadvisor
```

### 9. Docker Compose Troubleshooting

```bash
# View all service logs
docker compose logs

# Rebuild and recreate
docker compose up -d --build --force-recreate

# Check configuration
docker compose config
```

---

## Chapter 13: Advanced Docker Concepts and Features

### 1. Multi-stage Builds
Already covered in optimization section

### 2. Docker BuildKit

Enable BuildKit for faster builds:
```bash
export DOCKER_BUILDKIT=1
docker build .

# Or in daemon.json
{
  "features": {
    "buildkit": true
  }
}
```

**BuildKit features:**
- Parallel build steps
- Better caching
- Build secrets
- SSH forwarding

### 3. Custom Bridge Networks

```bash
# Create custom bridge
docker network create --driver bridge \
  --subnet=172.20.0.0/16 \
  --gateway=172.20.0.1 \
  my-bridge

# Use custom DNS
docker network create --driver bridge \
  --opt com.docker.network.bridge.name=my-bridge \
  --opt com.docker.network.bridge.enable_ip_masquerade=true \
  custom-bridge
```

### 4. Docker Contexts

Manage multiple Docker environments:
```bash
# Create context
docker context create remote --docker "host=ssh://user@remote-host"

# List contexts
docker context ls

# Switch context
docker context use remote

# Run commands on remote
docker ps
```

### 5. Docker Content Trust (DCT)

Sign and verify images:
```bash
# Enable DCT
export DOCKER_CONTENT_TRUST=1

# Push signed image
docker push myusername/myimage:tag

# Pull only signed images
docker pull myusername/myimage:tag
```

### 6. Docker Secrets (Swarm)

```bash
# Create secret
echo "mysecretpassword" | docker secret create db_password -

# Use in service
docker service create \
  --name myapp \
  --secret db_password \
  myimage

# Access in container at /run/secrets/db_password
```

### 7. Docker Health Checks

```dockerfile
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD curl -f http://localhost/ || exit 1
```

```bash
# Check health status
docker inspect --format='{{.State.Health.Status}}' container_name
```

### 8. Docker Plugins

```bash
# Install plugin
docker plugin install vieux/sshfs

# Use plugin for volume
docker run -d \
  --volume-driver vieux/sshfs \
  -v sshvolume:/data \
  nginx
```

### 9. Docker Buildx (Multi-platform builds)

```bash
# Create builder instance
docker buildx create --name mybuilder --use

# Build for multiple platforms
docker buildx build --platform linux/amd64,linux/arm64 -t myimage:latest --push .
```

### 10. Container Escape Protection

**Use AppArmor/SELinux:**
```bash
docker run --security-opt apparmor=docker-default nginx
```

**Drop capabilities:**
```bash
docker run --cap-drop=ALL --cap-add=NET_BIND_SERVICE nginx
```

---

## Chapter 14: Docker in CI/CD Pipelines

### 1. Docker in Continuous Integration

**GitLab CI Example:**
```yaml
# .gitlab-ci.yml
stages:
  - build
  - test
  - deploy

build:
  stage: build
  image: docker:latest
  services:
    - docker:dind
  script:
    - docker build -t myapp:$CI_COMMIT_SHA .
    - docker push myapp:$CI_COMMIT_SHA

test:
  stage: test
  image: myapp:$CI_COMMIT_SHA
  script:
    - npm test
```

**GitHub Actions Example:**
```yaml
name: CI
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Build Docker image
        run: docker build -t myapp .
      - name: Run tests
        run: docker run myapp npm test
```

### 2. Docker in Continuous Deployment

**Jenkins Pipeline:**
```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'docker build -t myapp:${BUILD_NUMBER} .'
            }
        }
        stage('Push') {
            steps {
                sh 'docker push myapp:${BUILD_NUMBER}'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker service update --image myapp:${BUILD_NUMBER} myservice'
            }
        }
    }
}
```

### 3. Security Scanning in CI/CD

```yaml
# GitLab CI with Trivy
security_scan:
  stage: test
  image: aquasec/trivy:latest
  script:
    - trivy image --exit-code 1 --severity HIGH,CRITICAL myapp:latest
```

### 4. Caching in CI/CD

```yaml
# GitHub Actions with caching
- name: Cache Docker layers
  uses: actions/cache@v2
  with:
    path: /tmp/.buildx-cache
    key: ${{ runner.os }}-buildx-${{ github.sha }}
    restore-keys: |
      ${{ runner.os }}-buildx-
```

---

## Chapter 15: Docker and Microservices Architecture

### Principles of Microservices

1. **Single Responsibility**
2. **Loose Coupling**
3. **Independent Deployment**
4. **Decentralized Data Management**
5. **Infrastructure Automation**

### Dockerizing Microservices

**Sample Microservice Dockerfile:**
```dockerfile
FROM node:14-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY . .

EXPOSE 3000

HEALTHCHECK --interval=30s CMD node healthcheck.js || exit 1

CMD ["node", "server.js"]
```

### Inter-service Communication

**REST API:**
```yaml
version: '3.8'
services:
  user-service:
    build: ./user-service
    networks:
      - backend
  
  order-service:
    build: ./order-service
    environment:
      USER_SERVICE_URL: http://user-service:3000
    networks:
      - backend
```

**Message Queues:**
```yaml
services:
  rabbitmq:
    image: rabbitmq:3-management
    ports:
      - "15672:15672"
  
  producer:
    build: ./producer
    depends_on:
      - rabbitmq
    environment:
      RABBITMQ_URL: amqp://rabbitmq
```

### Service Discovery

Use Docker DNS or tools like Consul, etcd:

```yaml
services:
  consul:
    image: consul:latest
    ports:
      - "8500:8500"
  
  web:
    build: ./web
    environment:
      CONSUL_HTTP_ADDR: consul:8500
```

### API Gateway

```yaml
services:
  nginx:
    image: nginx:alpine
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
    ports:
      - "80:80"
    depends_on:
      - service1
      - service2
```

### Database per Service

```yaml
services:
  user-db:
    image: postgres:13
    environment:
      POSTGRES_DB: users
  
  order-db:
    image: postgres:13
    environment:
      POSTGRES_DB: orders
```

---

## Chapter 16: Docker for Data Science and Machine Learning

### 1. Jupyter Notebook with Docker

```bash
docker run -p 8888:8888 \
  -v $(pwd):/home/jovyan/work \
  jupyter/scipy-notebook
```

**Custom Jupyter Dockerfile:**
```dockerfile
FROM jupyter/scipy-notebook

RUN pip install tensorflow pandas scikit-learn matplotlib
```

### 2. GPU Support with NVIDIA Docker

```bash
# Install NVIDIA Container Toolkit
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | \
  sudo tee /etc/apt/sources.list.d/nvidia-docker.list

sudo apt-get update && sudo apt-get install -y nvidia-container-toolkit
sudo systemctl restart docker

# Run with GPU
docker run --gpus all nvidia/cuda:11.0-base nvidia-smi
```

### 3. Model Serving with Flask

```dockerfile
FROM python:3.8-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY model.pkl .
COPY app.py .

EXPOSE 5000

CMD ["python", "app.py"]
```

### 4. Apache Spark with Docker

```yaml
version: '3'
services:
  spark-master:
    image: bitnami/spark:latest
    environment:
      - SPARK_MODE=master
    ports:
      - "8080:8080"
  
  spark-worker:
    image: bitnami/spark:latest
    environment:
      - SPARK_MODE=worker
      - SPARK_MASTER_URL=spark://spark-master:7077
```

---

## Chapter 17: Docker Swarm

### What is Docker Swarm Mode

Docker Swarm is Docker's native clustering and orchestration solution. A swarm is a group of machines running Docker joined into a cluster.

**Components:**
- **Manager nodes**: Dispatch tasks and manage the cluster
- **Worker nodes**: Execute tasks
- Recommended: 3 or 5 manager nodes for HA

### Docker Services

Services are containers in a swarm, with scaling and orchestration capabilities.

### Building a Swarm

**Step 1: Initialize Swarm (on manager node)**
```bash
docker swarm init --advertise-addr <MANAGER-IP>
```

**Step 2: Join workers**
```bash
# On worker nodes
docker swarm join --token <TOKEN> <MANAGER-IP>:2377
```

**Step 3: Verify cluster**
```bash
docker node ls
```

**Step 4: Deploy service**
```bash
docker service create --name web --replicas 3 -p 80:80 nginx
```

### Managing the Cluster

**Promote worker to manager:**
```bash
docker node promote <NODE-ID>
```

**Drain node for maintenance:**
```bash
docker node update --availability drain <NODE-ID>
```

### Using Services

**Create service:**
```bash
docker service create \
  --name myapp \
  --replicas 5 \
  --publish 8080:80 \
  nginx
```

**Scaling a service:**
```bash
docker service scale myapp=10
```

**Update service:**
```bash
docker service update --image nginx:1.22 myapp
```

**Remove service:**
```bash
docker service rm myapp
```

### Docker Stack

Deploy multi-service applications:

```yaml
# stack.yml
version: '3.8'
services:
  web:
    image: nginx
    deploy:
      replicas: 3
      update_config:
        parallelism: 1
        delay: 10s
      restart_policy:
        condition: on-failure
    ports:
      - "80:80"
```

```bash
docker stack deploy -c stack.yml mystack
```

---

## Conclusion

This comprehensive guide covers Docker from basics to advanced concepts:

1. **Fundamentals**: Containers, images, basic commands
2. **Installation**: Cross-platform setup
3. **Core Concepts**: Containers, images, Dockerfiles
4. **Networking**: Bridge, overlay, custom networks
5. **Storage**: Volumes and data persistence
6. **Orchestration**: Compose, Swarm, Kubernetes
7. **Production**: Security, performance, monitoring
8. **Advanced**: CI/CD, microservices, ML workloads

### Next Steps

1. **Practice**: Build and deploy your own applications
2. **Explore**: Try advanced features like BuildKit, multi-platform builds
3. **Learn**: Kubernetes for large-scale orchestration
4. **Contribute**: Join the Docker community

### Resources

- **Official Documentation**: https://docs.docker.com
- **Docker Hub**: https://hub.docker.com
- **Community**: https://forums.docker.com
- **GitHub**: https://github.com/docker

---

**License**: MIT License  
**Copyright** © 2024 Bobby Iliev

---

*Generated from "Docker Book" by Bobby Iliev*
*Last Updated: August 19, 2024*

