# Comprehensive Docker Guide

## Table of Contents
1. [Introduction to Docker](#1-introduction-to-docker)
2. [Installation](#2-installation)
3. [Images](#3-images)
4. [Containers](#4-containers)
5. [Dockerfile](#5-dockerfile)
6. [Docker Hub](#6-docker-hub)
7. [Multi-Stage Builds](#7-multi-stage-builds)
8. [Docker Networking](#8-docker-networking)
9. [Docker Volumes](#9-docker-volumes)
10. [Docker Compose](#10-docker-compose)
11. [Docker Security Best Practices](#11-docker-security-best-practices)
12. [Docker in CI/CD](#12-docker-in-cicd)
13. [Dockerize Java and Flask App](#13-dockerize-java-and-flask-app)
14. [Docker Commands](#14-docker-commands)
15. [Docker Cheat Sheet](#15-docker-cheat-sheet)

---

## 1. Introduction to Docker

### What is Docker?

**Docker** is an open-source platform that enables developers to develop, ship, and run applications inside isolated environments called **containers**. It packages code and all its dependencies so applications run quickly and reliably across different computing environments.

**Key Benefits:**
- **Consistency**: Same environment from development to production
- **Isolation**: Applications run independently without conflicts
- **Portability**: Run anywhere - laptop, data center, cloud
- **Resource Efficiency**: Lightweight compared to virtual machines
- **Fast Deployment**: Containers start in seconds

### Architecture of Docker

Docker uses a **client-server architecture** with the following components:

#### 1. **Docker Client**
- Command-line interface (CLI) that users interact with
- Sends commands to Docker daemon via REST API
- Commands: `docker build`, `docker pull`, `docker run`, etc.

#### 2. **Docker Daemon (dockerd)**
- Background service running on host machine
- Manages Docker objects: images, containers, networks, volumes
- Listens for Docker API requests
- Communicates with other daemons to manage Docker services

#### 3. **Docker Images**
- Read-only templates with instructions for creating containers
- Built from a Dockerfile
- Composed of layers for efficiency
- Stored in Docker registries

#### 4. **Docker Containers**
- Runnable instances of Docker images
- Isolated, portable, and lightweight execution environments
- Can be created, started, stopped, moved, or deleted

#### 5. **Docker Registries**
- Storage and distribution system for Docker images
- **Docker Hub**: Public registry (default)
- **Private registries**: For organizational use
- Stores multiple versions of images using tags

#### 6. **Docker Objects**
- **Networks**: Enable container communication
- **Volumes**: Persist data beyond container lifecycle
- **Plugins**: Extend Docker capabilities

**Docker Workflow:**
```
Developer writes Dockerfile → Build Image → Push to Registry → 
Pull from Registry → Run Container
```

### Difference between Docker and Virtual Machines

| Aspect | Docker Containers | Virtual Machines |
|--------|------------------|------------------|
| **Architecture** | Share host OS kernel | Each has full guest OS |
| **Size** | Lightweight (MBs) | Heavy (GBs) |
| **Startup Time** | Seconds | Minutes |
| **Resource Usage** | Minimal overhead | Significant overhead |
| **Isolation** | Process-level isolation | Complete isolation with hypervisor |
| **Performance** | Near-native performance | Slower due to virtualization layer |
| **Portability** | Highly portable across environments | Less portable, platform-dependent |
| **Density** | Run hundreds on single host | Limited number per host |
| **OS Support** | Linux, Windows containers (separate) | Any OS on any host |
| **Use Case** | Microservices, CI/CD, rapid scaling | Legacy apps, different OS requirements |

**Visual Comparison:**
```
Virtual Machines:              Docker Containers:
┌─────────────────┐           ┌─────────────────┐
│   App A   App B │           │   App A   App B │
│  ┌───┐   ┌───┐ │           │  ┌───┐   ┌───┐ │
│  │Bin│   │Bin│ │           │  │Bin│   │Bin│ │
│  │Lib│   │Lib│ │           │  │Lib│   │Lib│ │
│  └───┘   └───┘ │           │  └───┘   └───┘ │
├─────────────────┤           ├─────────────────┤
│   Guest OS      │           │ Docker Engine   │
├─────────────────┤           ├─────────────────┤
│   Hypervisor    │           │    Host OS      │
├─────────────────┤           ├─────────────────┤
│  Infrastructure │           │  Infrastructure │
└─────────────────┘           └─────────────────┘
```

---

## 2. Installation

### Docker Installation on Windows

**System Requirements:**
- Windows 10/11 64-bit: Pro, Enterprise, or Education
- WSL 2 backend OR Hyper-V backend
- Hardware virtualization support enabled in BIOS
- At least 4GB RAM

**Installation Steps:**

1. **Download Docker Desktop**
   - Visit: https://www.docker.com/products/docker-desktop
   - Download Docker Desktop for Windows

2. **Install Docker Desktop**
   - Run `Docker Desktop Installer.exe`
   - Follow installation wizard
   - Choose WSL 2 (recommended) or Hyper-V backend
   - Complete installation and restart if prompted

3. **Configure WSL 2 (Recommended)**
   ```powershell
   # Enable WSL
   dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
   
   # Enable Virtual Machine Platform
   dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
   
   # Set WSL 2 as default
   wsl --set-default-version 2
   ```

4. **Start Docker Desktop**
   - Launch from Start menu
   - Accept service agreement
   - Wait for Docker to start

5. **Verify Installation**
   ```powershell
   docker --version
   docker run hello-world
   ```

**Post-Installation:**
- Add user to `docker-users` group (if needed)
- Configure resources in Docker Desktop settings
- Set up Docker Hub account

### Docker Installation on macOS

**System Requirements:**
- macOS 11 or newer
- Apple Silicon (M1/M2) or Intel chip
- At least 4GB RAM

**Installation Steps:**

1. **Download Docker Desktop**
   - Visit: https://www.docker.com/products/docker-desktop
   - Choose correct version:
     - **Apple Silicon**: Docker Desktop for Mac (Apple chip)
     - **Intel**: Docker Desktop for Mac (Intel chip)

2. **Install Docker Desktop**
   - Open `Docker.dmg` file
   - Drag Docker icon to Applications folder
   - Launch Docker from Applications

3. **Grant Permissions**
   - Allow Docker to access system
   - Enter password when prompted
   - Accept service agreement

4. **Verify Installation**
   ```bash
   docker --version
   docker run hello-world
   ```

**Using Homebrew (Alternative):**
```bash
brew install --cask docker
```

### Docker Installation on Ubuntu

**System Requirements:**
- Ubuntu 64-bit (20.04 LTS, 22.04 LTS, or newer)
- Kernel version 3.10 or higher

**Installation Steps:**

#### Method 1: Docker Desktop

1. **Download Package**
   ```bash
   wget https://desktop.docker.com/linux/main/amd64/docker-desktop-<version>-amd64.deb
   ```

2. **Install**
   ```bash
   sudo apt-get update
   sudo apt-get install ./docker-desktop-<version>-amd64.deb
   ```

#### Method 2: Docker Engine (CLI only - Recommended for servers)

1. **Uninstall Old Versions**
   ```bash
   sudo apt-get remove docker docker-engine docker.io containerd runc
   ```

2. **Set Up Repository**
   ```bash
   sudo apt-get update
   sudo apt-get install ca-certificates curl gnupg lsb-release
   
   # Add Docker's official GPG key
   sudo mkdir -p /etc/apt/keyrings
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
   
   # Set up repository
   echo \
     "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
     $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```

3. **Install Docker Engine**
   ```bash
   sudo apt-get update
   sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
   ```

4. **Post-Installation Setup**
   ```bash
   # Add user to docker group (avoid using sudo)
   sudo groupadd docker
   sudo usermod -aG docker $USER
   newgrp docker
   
   # Enable Docker to start on boot
   sudo systemctl enable docker.service
   sudo systemctl enable containerd.service
   ```

5. **Verify Installation**
   ```bash
   docker --version
   docker run hello-world
   ```

---

## 3. Images

### What are Docker Images?

A **Docker image** is a read-only template containing:
- Application code
- Runtime environment
- System tools and libraries
- Dependencies
- Configuration files

**Key Characteristics:**
- **Immutable**: Once created, images don't change
- **Layered**: Built in layers for efficiency and reusability
- **Portable**: Can run on any system with Docker
- **Versioned**: Tagged for version control
- **Shareable**: Stored in registries

**Image Layers:**
```
┌─────────────────────────┐
│  Application Code       │  ← Layer 4 (your app)
├─────────────────────────┤
│  Python Dependencies    │  ← Layer 3
├─────────────────────────┤
│  Python Runtime         │  ← Layer 2
├─────────────────────────┤
│  Ubuntu Base OS         │  ← Layer 1 (base)
└─────────────────────────┘
```

Each layer is cached and reused, making builds faster and storage efficient.

**Common Image Operations:**
```bash
# List images
docker images
docker image ls

# Pull image from registry
docker pull ubuntu:22.04

# Remove image
docker rmi image_name
docker image rm image_name

# Inspect image details
docker inspect image_name

# View image history/layers
docker history image_name

# Tag image
docker tag source_image:tag target_image:tag

# Save image to tar file
docker save -o myimage.tar image_name

# Load image from tar file
docker load -i myimage.tar
```

### How to Create Docker Image

**Method 1: Using Dockerfile (Recommended)**

1. **Create a Dockerfile**
   ```dockerfile
   # Dockerfile
   FROM python:3.9-slim
   
   WORKDIR /app
   
   COPY requirements.txt .
   RUN pip install --no-cache-dir -r requirements.txt
   
   COPY . .
   
   EXPOSE 5000
   
   CMD ["python", "app.py"]
   ```

2. **Build the Image**
   ```bash
   docker build -t myapp:v1 .
   
   # Build with custom Dockerfile name
   docker build -f Dockerfile.prod -t myapp:prod .
   
   # Build with build arguments
   docker build --build-arg VERSION=1.0 -t myapp:v1 .
   ```

**Method 2: From Running Container (Not Recommended)**

```bash
# Run a container
docker run -it ubuntu:22.04 bash

# Make changes inside container
apt-get update
apt-get install -y nginx

# Exit container
exit

# Commit changes to new image
docker commit container_id myimage:v1
```

**Best Practices for Creating Images:**
- Use official base images
- Minimize number of layers
- Use `.dockerignore` file
- Don't install unnecessary packages
- Use multi-stage builds for production
- Run as non-root user
- Use specific version tags, not `latest`

### How to See Docker Image Contents?

**Method 1: Using `docker history`**
```bash
docker history image_name

# Show full commands
docker history --no-trunc image_name
```

**Method 2: Using `docker inspect`**
```bash
# View detailed JSON information
docker inspect image_name

# Extract specific information
docker inspect --format='{{.Config.Env}}' image_name
docker inspect --format='{{.Config.ExposedPorts}}' image_name
```

**Method 3: Explore with Interactive Shell**
```bash
# Run container and explore filesystem
docker run -it --rm image_name sh

# Or bash
docker run -it --rm image_name bash
```

**Method 4: Using dive tool (Third-party)**
```bash
# Install dive
wget https://github.com/wagoodman/dive/releases/download/v0.10.0/dive_0.10.0_linux_amd64.deb
sudo apt install ./dive_0.10.0_linux_amd64.deb

# Analyze image
dive image_name
```

**Method 5: Export and Extract**
```bash
# Save image
docker save image_name -o image.tar

# Extract tar file
tar -xf image.tar

# Explore layer contents
```

---

## 4. Containers

### What are Docker Containers?

A **Docker container** is a runnable instance of a Docker image. It's a standard unit of software that packages code and dependencies together.

**Key Characteristics:**
- **Lightweight**: Share host OS kernel
- **Isolated**: Own filesystem, networking, process tree
- **Portable**: Run consistently across environments
- **Ephemeral**: Can be easily created and destroyed
- **Scalable**: Quickly scale up or down

**Container vs Image:**
- **Image**: Static template (like a class)
- **Container**: Running instance (like an object)

### Docker Container Lifecycle

Containers move through five states:

```
┌─────────┐  docker create   ┌─────────┐  docker start   ┌─────────┐
│  Image  │ ───────────────> │ Created │ ──────────────> │ Running │
└─────────┘                  └─────────┘                 └─────────┘
                                                              │  │
                                           docker pause       │  │ docker stop
                                           ┌───────────┐      │  │
                                           │  Paused   │ <────┘  │
                                           └───────────┘         │
                                                  │              │
                                     docker unpause│              │
                                                  v              v
                                           ┌─────────────────────┐
                                           │      Stopped        │
                                           └─────────────────────┘
                                                      │
                                           docker rm  │
                                                      v
                                           ┌─────────────────────┐
                                           │      Deleted        │
                                           └─────────────────────┘
```

**States Explained:**
1. **Created**: Container exists but not started
2. **Running**: Container is executing processes
3. **Paused**: Processes suspended (using cgroups)
4. **Stopped**: Container stopped gracefully (SIGTERM)
5. **Killed/Deleted**: Container removed

### How to Manage Docker Containers?

**Basic Container Operations:**

```bash
# Run container (create + start)
docker run image_name

# Run with options
docker run -d \                    # Detached mode
  --name mycontainer \             # Custom name
  -p 8080:80 \                     # Port mapping
  -v /host/path:/container/path \  # Volume mount
  -e ENV_VAR=value \               # Environment variable
  --rm \                           # Remove after stop
  image_name

# List running containers
docker ps

# List all containers (including stopped)
docker ps -a

# Start stopped container
docker start container_name

# Stop running container (SIGTERM)
docker stop container_name

# Force stop container (SIGKILL)
docker kill container_name

# Restart container
docker restart container_name

# Pause container
docker pause container_name

# Unpause container
docker unpause container_name

# Remove container
docker rm container_name

# Remove running container (force)
docker rm -f container_name

# Remove all stopped containers
docker container prune
```

**Monitoring and Logs:**

```bash
# View container logs
docker logs container_name

# Follow log output
docker logs -f container_name

# Last N lines
docker logs --tail 100 container_name

# View container stats
docker stats

# Specific container stats
docker stats container_name

# Inspect container details
docker inspect container_name

# View running processes
docker top container_name
```

**Executing Commands in Containers:**

```bash
# Execute command in running container
docker exec container_name command

# Interactive shell
docker exec -it container_name bash

# Run as specific user
docker exec -u username -it container_name bash
```

**Copy Files:**

```bash
# From container to host
docker cp container_name:/path/to/file /host/path

# From host to container
docker cp /host/path/file container_name:/path/to/destination
```

### Difference between Docker Image and Container

| Aspect | Docker Image | Docker Container |
|--------|-------------|------------------|
| **Definition** | Blueprint/template | Running instance |
| **State** | Immutable (read-only) | Mutable (has state) |
| **Layers** | Composed of layers | Adds writable layer on top |
| **Lifecycle** | Permanent until deleted | Can be created/destroyed frequently |
| **Storage** | Stored in registry | Exists on host filesystem |
| **Purpose** | Package application | Execute application |
| **Analogy** | Class in OOP | Object/instance in OOP |
| **Command** | `docker build`, `docker pull` | `docker run`, `docker start` |
| **Multiple instances** | One image | Many containers from same image |

**Relationship:**
```
Image (Class)           Containers (Objects)
┌──────────────┐       ┌──────────────┐
│  ubuntu:22.04│  ───> │ container_1  │
└──────────────┘       ├──────────────┤
                       │ container_2  │
                       ├──────────────┤
                       │ container_3  │
                       └──────────────┘
```

### Docker - Container and Shells

**Interactive vs Non-Interactive:**

```bash
# Non-interactive (background)
docker run -d nginx

# Interactive with terminal
docker run -it ubuntu bash

# Interactive with pseudo-TTY
docker run -it ubuntu /bin/sh
```

**Common Shell Options:**

| Flag | Purpose |
|------|---------|
| `-i` | Interactive (keep STDIN open) |
| `-t` | Allocate pseudo-TTY (terminal) |
| `-it` | Interactive terminal (combined) |

**Shell Examples:**

```bash
# Bash shell
docker run -it ubuntu bash

# Sh shell
docker run -it alpine sh

# Execute command without shell
docker run ubuntu echo "Hello World"

# Execute multiple commands
docker run ubuntu sh -c "apt-get update && apt-get install -y curl"

# Attach to running container
docker attach container_name

# Detach from container (without stopping)
# Press: Ctrl+P, Ctrl+Q
```

### Docker - Container and Hosts

**Networking Between Container and Host:**

```bash
# Access host from container
# Use: host.docker.internal (Windows/Mac)
# Use: 172.17.0.1 (Linux default bridge IP)

# Port mapping
docker run -p 8080:80 nginx
# Host:8080 → Container:80

# Multiple port mappings
docker run -p 8080:80 -p 8443:443 nginx

# Bind to specific interface
docker run -p 127.0.0.1:8080:80 nginx

# Random host port
docker run -P nginx
```

**Volume Mounting (Host ↔ Container):**

```bash
# Bind mount
docker run -v /host/path:/container/path image_name

# Read-only mount
docker run -v /host/path:/container/path:ro image_name

# Named volume
docker run -v myvolume:/container/path image_name
```

**Environment Variables:**

```bash
# Pass single variable
docker run -e DATABASE_URL=postgres://db:5432 app

# Pass multiple variables
docker run -e VAR1=value1 -e VAR2=value2 app

# From file
docker run --env-file .env app
```

**Resource Limits:**

```bash
# Limit memory
docker run -m 512m image_name

# Limit CPU
docker run --cpus=".5" image_name

# CPU shares (relative weight)
docker run --cpu-shares=512 image_name
```

### Virtualization with Docker Containers

**Container Virtualization:**
- **OS-level virtualization**: Shares host kernel
- **Process isolation**: Uses Linux namespaces
- **Resource control**: Uses cgroups

**Key Technologies:**

1. **Namespaces** (Isolation):
   - **PID**: Process isolation
   - **NET**: Network isolation
   - **IPC**: Inter-process communication isolation
   - **MNT**: Mount points isolation
   - **UTS**: Hostname and domain isolation
   - **USER**: User ID isolation

2. **Control Groups (cgroups)** (Resource Limits):
   - CPU limitation
   - Memory limitation
   - Disk I/O limitation
   - Network bandwidth

3. **Union File Systems**:
   - OverlayFS, AUFS, Btrfs
   - Layer-based storage
   - Copy-on-write mechanism

**Comparison with Traditional Virtualization:**

```
Traditional VM:                    Container:
┌──────────────────┐              ┌──────────────────┐
│      App A       │              │      App A       │
├──────────────────┤              ├──────────────────┤
│   Guest OS (GB)  │              │   Container Libs │
├──────────────────┤              │     (MBs)        │
│    Hypervisor    │              ├──────────────────┤
├──────────────────┤              │  Docker Engine   │
│     Host OS      │              ├──────────────────┤
├──────────────────┤              │     Host OS      │
│    Hardware      │              ├──────────────────┤
└──────────────────┘              │    Hardware      │
                                  └──────────────────┘

Boot Time: Minutes                Boot Time: Seconds
Size: GBs                        Size: MBs
Overhead: High                   Overhead: Low
```

---

## 5. Dockerfile

### What is a Dockerfile?

A **Dockerfile** is a text file containing a series of instructions to build a Docker image automatically. It defines:
- Base image
- Application code
- Dependencies
- Configuration
- Startup commands

**Purpose:**
- Automate image creation
- Version control for infrastructure
- Reproducible builds
- Documentation of image composition

### What is Dockerfile Extension?

**Filename Convention:**
- **Default**: `Dockerfile` (no extension)
- **Custom names**: `Dockerfile.dev`, `Dockerfile.prod`, etc.
- **File format**: Plain text file

**Using Custom Dockerfile Names:**
```bash
# Build with custom name
docker build -f Dockerfile.dev -t myapp:dev .
docker build -f Dockerfile.prod -t myapp:prod .
```

**Best Practices:**
- Use `Dockerfile` for default/production
- Use descriptive names for variants: `Dockerfile.dev`, `Dockerfile.test`
- Store in project root or `.docker/` directory

### What is Dockerfile Syntax?

**Basic Structure:**
```dockerfile
# Comment
INSTRUCTION arguments
```

**Common Instructions:**

#### 1. **FROM** - Base Image
```dockerfile
FROM ubuntu:22.04
FROM node:18-alpine
FROM python:3.9-slim AS builder
```

#### 2. **LABEL** - Metadata
```dockerfile
LABEL maintainer="you@example.com"
LABEL version="1.0"
LABEL description="My application"
```

#### 3. **ENV** - Environment Variables
```dockerfile
ENV NODE_ENV=production
ENV APP_HOME=/app
ENV PATH="/app/bin:${PATH}"
```

#### 4. **WORKDIR** - Set Working Directory
```dockerfile
WORKDIR /app
# Subsequent commands execute from /app
```

#### 5. **COPY** - Copy Files
```dockerfile
COPY package.json .
COPY . /app
COPY --chown=user:group files /destination
```

#### 6. **ADD** - Copy and Extract
```dockerfile
ADD file.tar.gz /destination
ADD https://example.com/file.txt /app/
# Prefer COPY unless you need ADD's extra features
```

#### 7. **RUN** - Execute Commands
```dockerfile
RUN apt-get update && apt-get install -y curl
RUN pip install --no-cache-dir -r requirements.txt

# Multi-line for readability
RUN apt-get update && \
    apt-get install -y \
        curl \
        vim \
        git && \
    rm -rf /var/lib/apt/lists/*
```

#### 8. **CMD** - Default Command
```dockerfile
CMD ["python", "app.py"]
CMD ["npm", "start"]
# Only one CMD per Dockerfile (last one wins)
```

#### 9. **ENTRYPOINT** - Configure Container Executable
```dockerfile
ENTRYPOINT ["python"]
CMD ["app.py"]
# Results in: python app.py
```

#### 10. **EXPOSE** - Document Ports
```dockerfile
EXPOSE 80
EXPOSE 443
EXPOSE 8000-8080
```

#### 11. **VOLUME** - Define Mount Points
```dockerfile
VOLUME /data
VOLUME ["/var/log", "/var/db"]
```

#### 12. **USER** - Set User
```dockerfile
USER appuser
USER 1000:1000
```

#### 13. **ARG** - Build-time Variables
```dockerfile
ARG VERSION=1.0
ARG PYTHON_VERSION=3.9
FROM python:${PYTHON_VERSION}
```

#### 14. **HEALTHCHECK** - Container Health
```dockerfile
HEALTHCHECK --interval=30s --timeout=3s \
  CMD curl -f http://localhost/ || exit 1
```

#### 15. **ONBUILD** - Trigger for Child Images
```dockerfile
ONBUILD COPY . /app
ONBUILD RUN make
```

**Complete Example:**
```dockerfile
# Use official Python runtime as base
FROM python:3.9-slim

# Set metadata
LABEL maintainer="dev@company.com"
LABEL version="1.0"

# Set environment variables
ENV PYTHONUNBUFFERED=1 \
    PYTHONDONTWRITEBYTECODE=1 \
    APP_HOME=/app

# Set working directory
WORKDIR ${APP_HOME}

# Install system dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        gcc \
        postgresql-client && \
    rm -rf /var/lib/apt/lists/*

# Copy requirements first (layer caching)
COPY requirements.txt .

# Install Python dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY . .

# Create non-root user
RUN useradd -m -u 1000 appuser && \
    chown -R appuser:appuser ${APP_HOME}

# Switch to non-root user
USER appuser

# Expose port
EXPOSE 8000

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s \
  CMD python healthcheck.py || exit 1

# Default command
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
```

**CMD vs ENTRYPOINT:**

| Aspect | CMD | ENTRYPOINT |
|--------|-----|------------|
| Purpose | Default command/args | Main executable |
| Override | Easily overridden | Requires --entrypoint |
| Usage | Flexible defaults | Fixed executable |
| Combination | Can complement ENTRYPOINT | Works with CMD for args |

```dockerfile
# CMD only
CMD ["python", "app.py"]
# Run: docker run image
# Override: docker run image python other.py

# ENTRYPOINT only
ENTRYPOINT ["python", "app.py"]
# Run: docker run image
# Override: docker run --entrypoint bash image

# Combined
ENTRYPOINT ["python"]
CMD ["app.py"]
# Run: docker run image → python app.py
# Override args: docker run image other.py → python other.py
```

### What is a Multi-Stage Dockerfile?

A **multi-stage build** uses multiple `FROM` statements in one Dockerfile to:
- Separate build-time dependencies from runtime
- Reduce final image size
- Improve security by excluding build tools

**Structure:**
```dockerfile
# Stage 1: Build
FROM base-image AS stage-name
# Build instructions

# Stage 2: Production
FROM smaller-base-image
COPY --from=stage-name /path/to/artifacts /destination
```

**Example: Node.js Application**

```dockerfile
# Stage 1: Build stage
FROM node:18 AS builder

WORKDIR /app

# Install dependencies
COPY package*.json ./
RUN npm ci --only=production

# Copy source code
COPY . .

# Build application
RUN npm run build

# Stage 2: Production stage
FROM node:18-alpine

WORKDIR /app

# Copy only necessary files from builder
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
COPY package*.json ./

# Run as non-root user
USER node

EXPOSE 3000

CMD ["node", "dist/server.js"]
```

**Size Comparison:**
```
Without multi-stage: 1.2 GB
With multi-stage:    150 MB
Reduction:           ~87%
```

**Advanced Multi-Stage Example: Go Application**

```dockerfile
# Build stage
FROM golang:1.20 AS builder

WORKDIR /app

COPY go.mod go.sum ./
RUN go mod download

COPY . .

# Build with optimizations
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o main .

# Production stage
FROM scratch

# Copy CA certificates for HTTPS
COPY --from=builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/

# Copy binary
COPY --from=builder /app/main /main

EXPOSE 8080

ENTRYPOINT ["/main"]
```

**Benefits:**
- **Smaller images**: Only runtime dependencies included
- **Faster deployments**: Less data to transfer
- **Better security**: No build tools in production
- **Clean separation**: Build vs runtime concerns
- **Layer caching**: Efficient rebuilds

---

## 6. Docker Hub

### What is Docker Hub?

**Docker Hub** is a cloud-based registry service that allows you to:
- Store and distribute Docker images
- Find and use official and community images
- Automate builds from source code
- Collaborate with teams

**Features:**
- **Public repositories**: Free, unlimited pulls
- **Private repositories**: Restricted access
- **Official Images**: Curated by Docker and vendors
- **Automated builds**: Build from GitHub/Bitbucket
- **Webhooks**: Trigger actions after push
- **Organizations**: Team collaboration
- **Vulnerability scanning**: Security analysis

**Website**: https://hub.docker.com

**Popular Official Images:**
- `ubuntu`, `alpine`, `debian`
- `nginx`, `httpd`, `caddy`
- `node`, `python`, `golang`, `java`
- `mysql`, `postgres`, `mongodb`, `redis`

### Publishing Images to Docker Hub

**Step-by-Step Process:**

#### 1. **Create Docker Hub Account**
- Visit https://hub.docker.com
- Sign up for free account
- Verify email

#### 2. **Create Repository**
- Click "Create Repository"
- Enter repository name: `myapp`
- Choose visibility: Public or Private
- Add description
- Click "Create"

#### 3. **Build Your Image**
```bash
docker build -t myapp:v1.0 .
```

#### 4. **Tag Image with Docker Hub Username**
```bash
# Format: username/repository:tag
docker tag myapp:v1.0 yourusername/myapp:v1.0

# Multiple tags
docker tag myapp:v1.0 yourusername/myapp:latest
```

#### 5. **Login to Docker Hub**
```bash
docker login

# Enter username and password
# Or use token for security
docker login -u yourusername -p your_token
```

#### 6. **Push Image**
```bash
# Push specific tag
docker push yourusername/myapp:v1.0

# Push all tags
docker push yourusername/myapp --all-tags
```

#### 7. **Verify on Docker Hub**
- Visit https://hub.docker.com/r/yourusername/myapp
- Check image appears

#### 8. **Pull Image (Others)**
```bash
docker pull yourusername/myapp:v1.0
```

**Complete Example:**
```bash
# 1. Build image
docker build -t flask-app:1.0 .

# 2. Tag for Docker Hub
docker tag flask-app:1.0 john/flask-app:1.0
docker tag flask-app:1.0 john/flask-app:latest

# 3. Login
docker login

# 4. Push
docker push john/flask-app:1.0
docker push john/flask-app:latest

# 5. Pull (from anywhere)
docker pull john/flask-app:latest
```

**Best Practices:**
- Use semantic versioning: `v1.0.0`, `v1.0.1`
- Tag with `latest` for most recent stable version
- Use descriptive repository names
- Add comprehensive README in Docker Hub
- Regular updates for security patches
- Scan images for vulnerabilities
- Use automated builds from Git repos
- Tag build metadata: `git-commit-hash`, `build-date`

**Automated Builds:**
1. Link Docker Hub to GitHub/Bitbucket
2. Configure build rules
3. Auto-build on code push
4. Auto-tag based on branch/tag

---

## 7. Multi-Stage Builds

### What is a Multi-Stage Dockerfile?

A **multi-stage Dockerfile** contains multiple `FROM` instructions, where each `FROM` begins a new stage. Earlier stages are used to build artifacts, and later stages copy only what's needed for production.

**Key Concepts:**
- **Build stage**: Contains compilers, build tools, dev dependencies
- **Production stage**: Minimal runtime environment
- **Selective copying**: `COPY --from=stage_name`
- **Named stages**: `FROM image AS stage_name`

**Why Use Multi-Stage Builds?**
- Dramatically reduce image size (50-90% reduction)
- Separate build-time vs runtime dependencies
- Improved security (no build tools in production)
- Single Dockerfile for entire pipeline
- Faster deployment and distribution

### How to Optimize Docker Image using Multi-Stage Builds

**Before Multi-Stage (Single Stage):**
```dockerfile
FROM node:18

WORKDIR /app

COPY package*.json ./
RUN npm install  # Installs ALL dependencies

COPY . .
RUN npm run build  # Build artifacts

EXPOSE 3000
CMD ["npm", "start"]

# Final image size: ~1.2 GB
# Contains: Build tools, dev dependencies, source code, build cache
```

**After Multi-Stage (Optimized):**
```dockerfile
# Stage 1: Build
FROM node:18 AS builder

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY . .
RUN npm run build

# Stage 2: Production
FROM node:18-alpine  # Smaller base

WORKDIR /app

# Copy only production dependencies
COPY --from=builder /app/node_modules ./node_modules

# Copy only built artifacts
COPY --from=builder /app/dist ./dist

COPY package*.json ./

USER node
EXPOSE 3000
CMD ["node", "dist/server.js"]

# Final image size: ~150 MB (87% reduction)
```

**Optimization Techniques:**

#### 1. **Use Smaller Base Images**
```dockerfile
# Before: 1.1 GB
FROM node:18

# After: 170 MB
FROM node:18-alpine

# Even smaller: 40 MB (for compiled languages)
FROM scratch
```

#### 2. **Compile to Static Binary (Go Example)**
```dockerfile
# Build stage
FROM golang:1.20-alpine AS builder

WORKDIR /app

COPY go.* ./
RUN go mod download

COPY . .

# Static compilation
RUN CGO_ENABLED=0 GOOS=linux go build -ldflags="-w -s" -o app .

# Production stage - minimal!
FROM scratch

COPY --from=builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/
COPY --from=builder /app/app /app

EXPOSE 8080
ENTRYPOINT ["/app"]

# Final size: ~10 MB!
```

#### 3. **Python Optimization**
```dockerfile
# Build stage
FROM python:3.9 AS builder

WORKDIR /app

# Install dependencies to custom path
COPY requirements.txt .
RUN pip install --user --no-cache-dir -r requirements.txt

# Production stage
FROM python:3.9-slim

WORKDIR /app

# Copy only installed packages
COPY --from=builder /root/.local /root/.local

# Copy application
COPY . .

# Update PATH
ENV PATH=/root/.local/bin:$PATH

CMD ["python", "app.py"]
```

#### 4. **Java/Maven Optimization**
```dockerfile
# Build stage
FROM maven:3.8-openjdk-17 AS builder

WORKDIR /app

# Copy POM first (layer caching)
COPY pom.xml .
RUN mvn dependency:go-offline

# Copy source and build
COPY src ./src
RUN mvn package -DskipTests

# Production stage
FROM openjdk:17-jre-slim

WORKDIR /app

# Copy only JAR file
COPY --from=builder /app/target/myapp.jar .

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "myapp.jar"]
```

#### 5. **Multiple Build Stages**
```dockerfile
# Stage 1: Dependencies
FROM node:18 AS dependencies

WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production

# Stage 2: Build
FROM node:18 AS builder

WORKDIR /app
COPY package*.json ./
RUN npm ci  # All dependencies

COPY . .
RUN npm run build
RUN npm run test

# Stage 3: Production
FROM node:18-alpine

WORKDIR /app

COPY --from=dependencies /app/node_modules ./node_modules
COPY --from=builder /app/dist ./dist
COPY package*.json ./

USER node
CMD ["node", "dist/server.js"]
```

#### 6. **Stop at Specific Stage (Testing)**
```bash
# Build only up to 'builder' stage
docker build --target builder -t myapp:builder .

# Use for CI/CD testing
docker run myapp:builder npm test
```

#### 7. **Copy from External Images**
```dockerfile
# Copy from official image
FROM alpine:latest

COPY --from=nginx:latest /etc/nginx/nginx.conf /nginx.conf
COPY --from=golang:1.20 /usr/local/go /usr/local/go

# Rest of Dockerfile...
```

**Size Comparison Examples:**

| Language | Single-Stage | Multi-Stage | Reduction |
|----------|-------------|-------------|-----------|
| Node.js | 1.2 GB | 150 MB | 87% |
| Python | 900 MB | 120 MB | 86% |
| Go | 800 MB | 10 MB | 98% |
| Java | 500 MB | 200 MB | 60% |

**Best Practices:**
1. **Name your stages** for clarity
2. **Use alpine variants** when possible
3. **Order layers** by change frequency
4. **Leverage build cache** with proper layer ordering
5. **Remove build artifacts** and caches
6. **Use .dockerignore** to exclude unnecessary files
7. **Scan images** for vulnerabilities
8. **Use specific versions** for reproducibility

**Advanced: Build Arguments Between Stages**
```dockerfile
ARG NODE_VERSION=18

FROM node:${NODE_VERSION} AS builder
ARG BUILD_ENV=production
ENV NODE_ENV=${BUILD_ENV}

# Build process...

FROM node:${NODE_VERSION}-alpine
COPY --from=builder /app/dist ./dist
```

---

## 8. Docker Networking

### Introduction to Docker Networking

**Docker networking** enables containers to communicate with:
- Each other (container-to-container)
- Host machine (container-to-host)
- External networks (container-to-internet)

**Key Concepts:**
- **Network drivers**: Different networking modes
- **Container DNS**: Built-in service discovery
- **Port publishing**: Expose container ports to host
- **Network isolation**: Separate networks for security
- **Custom networks**: User-defined networks

**Default Behavior:**
- Containers on same network can communicate by name
- Isolated from other networks
- Can access internet (NAT)

### Difference Between Docker Network and VM Network

| Aspect | Docker Network | VM Network |
|--------|----------------|------------|
| **Overhead** | Minimal (software bridge) | Higher (virtual NIC) |
| **Performance** | Near-native speed | Slower due to virtualization |
| **Isolation Level** | Namespace-based | Hardware-level |
| **Setup** | Automatic, simple | Requires configuration |
| **Resource Usage** | Lightweight | More resources |
| **DNS** | Built-in service discovery | Requires DNS server |
| **Port Conflicts** | Handled by port mapping | Separate IP spaces |
| **Network Types** | Bridge, host, overlay, etc. | Bridged, NAT, host-only |

### Docker Network Drivers

Docker provides several network drivers for different use cases:

#### 1. **Bridge (Default)**
- **Default network** for standalone containers
- **Use case**: Single host, container isolation
- **Communication**: Containers on same bridge can communicate
- **Internet access**: Via NAT

```bash
# Create container (uses default bridge)
docker run -d --name web nginx

# Create custom bridge network
docker network create my-bridge

# Run container on custom network
docker run -d --name web --network my-bridge nginx

# Connect existing container
docker network connect my-bridge container_name
```

**Characteristics:**
- Automatic DNS resolution by container name
- Isolated from other networks
- Requires port publishing for external access

#### 2. **Host**
- **Shares host's network** stack
- **Use case**: Maximum performance, no isolation needed
- **No port mapping** required
- **No network isolation**

```bash
docker run -d --network host nginx
# Nginx accessible on host's port 80 directly
```

**Characteristics:**
- Best performance (no overhead)
- Container sees host's interfaces
- Port conflicts with host possible
- Less secure (no isolation)

#### 3. **Overlay**
- **Multi-host networking** for Docker Swarm
- **Use case**: Distributed applications, microservices
- **Communication**: Across multiple Docker hosts
- **Requires**: Docker Swarm or Kubernetes

```bash
# Create overlay network (Swarm mode)
docker network create -d overlay my-overlay

# Deploy service
docker service create --network my-overlay my-service
```

**Characteristics:**
- Encrypted communication (optional)
- Service discovery across hosts
- Load balancing built-in

#### 4. **Macvlan**
- **Direct connection** to physical network
- **Use case**: Legacy apps requiring MAC address
- **Containers appear** as physical devices on network

```bash
docker network create -d macvlan \
  --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 \
  -o parent=eth0 \
  my-macvlan
```

#### 5. **None**
- **No networking**
- **Use case**: Complete isolation, custom networking
- **Communication**: Disabled

```bash
docker run -d --network none my-container
```

#### 6. **IPvlan**
- Similar to macvlan but uses single MAC
- Better for networks with MAC address limits

**Driver Comparison:**

| Driver | Use Case | Isolation | Performance | Complexity |
|--------|----------|-----------|-------------|------------|
| **Bridge** | Single host | High | Good | Low |
| **Host** | Performance-critical | None | Best | Low |
| **Overlay** | Multi-host | High | Good | Medium |
| **Macvlan** | Legacy integration | Medium | Excellent | High |
| **None** | Maximum isolation | Complete | N/A | Low |

### Docker Network Host

**Host network mode** removes network isolation between container and host.

**Advantages:**
- **Best performance**: No bridge overhead
- **No port mapping**: Direct access to host ports
- **Simplified networking**: No NAT
- **See host interfaces**: Container uses host's network stack

**Disadvantages:**
- **No isolation**: Security concern
- **Port conflicts**: Can't run multiple containers on same port
- **Not portable**: Port availability varies by host

**Usage:**
```bash
# Run with host network
docker run -d --network host nginx

# Container now listens on host's port 80
curl http://localhost:80
```

**Example: High-Performance Application**
```bash
# Database requiring max performance
docker run -d \
  --network host \
  --name postgres \
  -e POSTGRES_PASSWORD=secret \
  postgres:15
```

**When to Use:**
- Performance-critical applications
- Network monitoring tools
- Development/testing environments
- Single container on host port

**When to Avoid:**
- Production multi-container setups
- Require network isolation
- Multiple similar services
- Enhanced security needed

### Docker Network Bridge

**Bridge network** is the default network driver that creates a private internal network on the host.

**How it Works:**
```
┌─────────────────────────────────┐
│          Host Machine           │
│                                 │
│  ┌──────────┐    ┌──────────┐  │
│  │Container │    │Container │  │
│  │    A     │    │    B     │  │
│  │ 172.17.0.2│   │172.17.0.3│  │
│  └─────┬────┘    └────┬─────┘  │
│        │              │         │
│  ┌─────┴──────────────┴─────┐  │
│  │     docker0 bridge      │  │
│  │      172.17.0.1         │  │
│  └─────────────┬───────────┘  │
│                │               │
│           ┌────┴────┐          │
│           │   eth0  │          │
│           └─────────┘          │
└─────────────────────────────────┘
```

**Default Bridge Network:**
```bash
# Inspect default bridge
docker network inspect bridge

# Run container on default bridge
docker run -d nginx  # Automatically uses bridge
```

**Limitations of Default Bridge:**
- No automatic DNS resolution between containers
- Must use IP addresses or `--link` (deprecated)
- All containers share same network

**Custom Bridge Network (Recommended):**
```bash
# Create custom bridge
docker network create my-app-network

# Run containers on custom network
docker run -d --name database --network my-app-network postgres
docker run -d --name web --network my-app-network nginx

# Containers can communicate by name!
docker exec web ping database
```

**Custom Bridge Advantages:**
- **Automatic DNS**: Containers resolve each other by name
- **Better isolation**: Separate network per application
- **Dynamic attachment**: Add/remove containers anytime
- **Advanced configuration**: Custom subnet, gateway

**Network Operations:**
```bash
# Create network with custom settings
docker network create \
  --driver bridge \
  --subnet 192.168.0.0/16 \
  --gateway 192.168.0.1 \
  custom-bridge

# Connect running container
docker network connect custom-bridge container_name

# Disconnect container
docker network disconnect custom-bridge container_name

# Inspect network
docker network inspect custom-bridge

# List networks
docker network ls

# Remove network
docker network rm custom-bridge

# Remove unused networks
docker network prune
```

**Port Publishing with Bridge:**
```bash
# Publish single port
docker run -d -p 8080:80 nginx
# Host:8080 → Container:80

# Publish multiple ports
docker run -d -p 8080:80 -p 8443:443 nginx

# Publish to specific interface
docker run -d -p 127.0.0.1:8080:80 nginx

# Publish all exposed ports to random ports
docker run -d -P nginx
```

**Complete Example:**
```bash
# Create isolated network for app
docker network create webapp-network

# Run database
docker run -d \
  --name db \
  --network webapp-network \
  -e POSTGRES_PASSWORD=secret \
  postgres

# Run backend
docker run -d \
  --name api \
  --network webapp-network \
  -e DATABASE_URL=postgresql://db:5432 \
  myapi:latest

# Run frontend (exposed to host)
docker run -d \
  --name frontend \
  --network webapp-network \
  -p 8080:80 \
  myfrontend:latest

# Frontend can access API by name "api"
# API can access DB by name "db"
# Only frontend is accessible from host (port 8080)
```

---

## 9. Docker Volumes

### What is Docker Volume?

A **Docker volume** is a persistent data storage mechanism that exists outside the container filesystem.

**Why Volumes?**
- **Data persistence**: Data survives container deletion
- **Sharing data**: Multiple containers access same data
- **Backup**: Easy to backup and restore
- **Performance**: Better I/O than container filesystem
- **Decoupling**: Separate data from container lifecycle

**Types of Mounts:**

| Type | Storage Location | Management | Use Case |
|------|-----------------|------------|----------|
| **Volume** | Docker-managed (`/var/lib/docker/volumes/`) | Docker CLI | Production, persistence |
| **Bind Mount** | Any host path | User | Development, configuration |
| **tmpfs Mount** | Host memory | Temporary | Sensitive data, temporary files |

### Data Storage in Docker

**Container Filesystem:**
- **Ephemeral**: Lost when container is removed
- **Copy-on-write**: Writable layer on top of image
- **Inefficient**: Not optimized for I/O

**Volume vs Bind Mount vs tmpfs:**

```
Volume:                      Bind Mount:                tmpfs:
┌──────────────┐            ┌──────────────┐          ┌──────────────┐
│  Container   │            │  Container   │          │  Container   │
│  /app/data   │            │  /app/config │          │  /app/cache  │
└──────┬───────┘            └──────┬───────┘          └──────┬───────┘
       │                           │                         │
┌──────┴────────┐          ┌──────┴────────┐         ┌──────┴────────┐
│ Docker Volume │          │  Host Path    │         │  RAM (tmpfs)  │
│  myvolume     │          │ /home/user/   │         │  (temporary)  │
└───────────────┘          └───────────────┘         └───────────────┘
```

**When to Use Each:**

- **Volume**: Production databases, persistent app data
- **Bind Mount**: Development code, configuration files
- **tmpfs**: Passwords, tokens, cache

### Using CLI to Manage Docker Volumes

**Volume Lifecycle:**

#### 1. **Create Volume**
```bash
# Create named volume
docker volume create mydata

# Create with driver and options
docker volume create \
  --driver local \
  --opt type=nfs \
  --opt o=addr=192.168.1.1,rw \
  --opt device=:/path/to/dir \
  nfs-volume
```

#### 2. **List Volumes**
```bash
docker volume ls

# Filter volumes
docker volume ls --filter "dangling=true"
```

#### 3. **Inspect Volume**
```bash
docker volume inspect mydata

# Output shows:
# - Name
# - Driver
# - Mountpoint
# - Options
```

#### 4. **Use Volume**
```bash
# Mount named volume
docker run -d \
  --name myapp \
  -v mydata:/app/data \
  myimage

# Or using --mount (recommended, more explicit)
docker run -d \
  --name myapp \
  --mount source=mydata,target=/app/data \
  myimage
```

#### 5. **Remove Volume**
```bash
# Remove specific volume
docker volume rm mydata

# Remove all unused volumes
docker volume prune

# Force remove (even if in use - dangerous!)
docker volume rm -f mydata
```

**Bind Mount Examples:**
```bash
# Bind mount with -v
docker run -d \
  -v /host/path:/container/path \
  myimage

# Read-only bind mount
docker run -d \
  -v /host/path:/container/path:ro \
  myimage

# Bind mount with --mount
docker run -d \
  --mount type=bind,source=/host/path,target=/container/path \
  myimage
```

**tmpfs Mount Examples:**
```bash
# tmpfs mount (Linux only)
docker run -d \
  --tmpfs /app/cache \
  myimage

# With options
docker run -d \
  --mount type=tmpfs,target=/app/cache,tmpfs-size=100m \
  myimage
```

### Docker Volume Commands

**Complete Command Reference:**

```bash
# CREATE
docker volume create [OPTIONS] [VOLUME]

docker volume create mydata
docker volume create --label environment=prod dbdata

# LIST
docker volume ls [OPTIONS]

docker volume ls
docker volume ls --filter "label=environment=prod"
docker volume ls --format "table {{.Name}}\t{{.Driver}}\t{{.Mountpoint}}"

# INSPECT
docker volume inspect [OPTIONS] VOLUME [VOLUME...]

docker volume inspect mydata
docker volume inspect --format '{{.Mountpoint}}' mydata

# REMOVE
docker volume rm [OPTIONS] VOLUME [VOLUME...]

docker volume rm mydata
docker volume rm volume1 volume2 volume3

# PRUNE (remove unused volumes)
docker volume prune [OPTIONS]

docker volume prune
docker volume prune --filter "label!=keep"
docker volume prune -f  # Force, no prompt
```

**Practical Examples:**

**1. Database Persistence:**
```bash
# Create volume for PostgreSQL
docker volume create postgres-data

# Run PostgreSQL with persistent storage
docker run -d \
  --name postgres \
  -e POSTGRES_PASSWORD=secret \
  -v postgres-data:/var/lib/postgresql/data \
  postgres:15

# Data persists even if container is removed
docker rm -f postgres
docker run -d \
  --name postgres-new \
  -e POSTGRES_PASSWORD=secret \
  -v postgres-data:/var/lib/postgresql/data \
  postgres:15
# Data still there!
```

**2. Sharing Data Between Containers:**
```bash
# Create shared volume
docker volume create shared-data

# Producer container
docker run -d \
  --name producer \
  -v shared-data:/data \
  alpine \
  sh -c "while true; do date >> /data/log.txt; sleep 1; done"

# Consumer container
docker run -d \
  --name consumer \
  -v shared-data:/data \
  alpine \
  sh -c "tail -f /data/log.txt"

# Both containers share same data!
```

**3. Development with Bind Mounts:**
```bash
# Mount source code for live editing
docker run -d \
  --name dev-server \
  -p 3000:3000 \
  -v $(pwd)/src:/app/src \
  -v $(pwd)/package.json:/app/package.json \
  node:18 \
  npm run dev

# Edit code on host, changes reflect immediately in container
```

**4. Backup and Restore:**
```bash
# Backup volume
docker run --rm \
  -v mydata:/data \
  -v $(pwd):/backup \
  alpine \
  tar czf /backup/mydata-backup.tar.gz -C /data .

# Restore volume
docker run --rm \
  -v mydata:/data \
  -v $(pwd):/backup \
  alpine \
  tar xzf /backup/mydata-backup.tar.gz -C /data
```

**5. Volume with Docker Compose:**
```yaml
version: '3.8'

services:
  db:
    image: postgres:15
    volumes:
      - postgres-data:/var/lib/postgresql/data
      - ./init.sql:/docker-entrypoint-initdb.d/init.sql:ro
    environment:
      POSTGRES_PASSWORD: secret

  web:
    image: nginx
    volumes:
      - web-content:/usr/share/nginx/html
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
    ports:
      - "80:80"

volumes:
  postgres-data:
    driver: local
  web-content:
    driver: local
```

**Best Practices:**
- Use named volumes for production data
- Use bind mounts for development only
- Always backup important volumes
- Avoid storing secrets in volumes (use Docker secrets)
- Set appropriate permissions
- Use read-only mounts when possible
- Clean up unused volumes regularly
- Document volume usage in README

---

## 10. Docker Compose

### What is Docker Compose?

**Docker Compose** is a tool for defining and managing multi-container Docker applications using a YAML file.

**Purpose:**
- Orchestrate multiple containers
- Define services, networks, volumes
- Manage application lifecycle
- Simplify complex deployments

**Benefits:**
- **Single configuration file**: `docker-compose.yml`
- **Simple commands**: `up`, `down`, `start`, `stop`
- **Environment consistency**: Same setup everywhere
- **Easy scaling**: Scale services with one command
- **Service dependencies**: Control startup order

**Use Cases:**
- Development environments
- Automated testing
- Single-host deployments
- Microservices architecture

### Install Docker Compose

**Docker Compose V2** (comes with Docker Desktop):

**Check if installed:**
```bash
docker compose version
```

**Linux Installation (if not included):**
```bash
# Download latest version
DOCKER_CONFIG=${DOCKER_CONFIG:-$HOME/.docker}
mkdir -p $DOCKER_CONFIG/cli-plugins
curl -SL https://github.com/docker/compose/releases/latest/download/docker-compose-linux-x86_64 -o $DOCKER_CONFIG/cli-plugins/docker-compose

# Make executable
chmod +x $DOCKER_CONFIG/cli-plugins/docker-compose

# Verify
docker compose version
```

**Alternative: Standalone Binary (Compose V1 - Legacy):**
```bash
sudo curl -L "https://github.com/docker/compose/releases/download/v2.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

### Docker Compose YAML

**Basic Structure:**
```yaml
version: '3.8'  # Optional in newer versions

services:
  service1:
    # Service configuration
  service2:
    # Service configuration

networks:
  # Custom networks

volumes:
  # Named volumes
```

**Complete Example:**
```yaml
version: '3.8'

services:
  # Web service
  web:
    image: nginx:alpine
    container_name: my-web
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./html:/usr/share/nginx/html:ro
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
    networks:
      - frontend
    depends_on:
      - api
    environment:
      - NGINX_HOST=example.com
      - NGINX_PORT=80
    restart: unless-stopped
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost"]
      interval: 30s
      timeout: 3s
      retries: 3

  # API service
  api:
    build:
      context: ./api
      dockerfile: Dockerfile
      args:
        - VERSION=1.0
    container_name: my-api
    expose:
      - "3000"
    volumes:
      - ./api:/app
      - /app/node_modules
    networks:
      - frontend
      - backend
    depends_on:
      - db
      - redis
    environment:
      DATABASE_URL: postgresql://db:5432/myapp
      REDIS_URL: redis://redis:6379
    env_file:
      - .env
    command: npm run dev

  # Database service
  db:
    image: postgres:15-alpine
    container_name: my-db
    volumes:
      - postgres-data:/var/lib/postgresql/data
      - ./init.sql:/docker-entrypoint-initdb.d/init.sql
    networks:
      - backend
    environment:
      POSTGRES_DB: myapp
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    restart: always

  # Redis cache
  redis:
    image: redis:7-alpine
    container_name: my-redis
    networks:
      - backend
    volumes:
      - redis-data:/data
    command: redis-server --appendonly yes

networks:
  frontend:
    driver: bridge
  backend:
    driver: bridge

volumes:
  postgres-data:
  redis-data:
```

**Key YAML Fields:**

| Field | Purpose | Example |
|-------|---------|---------|
| `image` | Use pre-built image | `nginx:alpine` |
| `build` | Build from Dockerfile | `build: ./app` |
| `ports` | Publish ports | `"8080:80"` |
| `expose` | Document internal ports | `expose: [3000]` |
| `volumes` | Mount volumes | `./src:/app/src` |
| `environment` | Set env vars | `NODE_ENV=production` |
| `env_file` | Load env from file | `.env` |
| `depends_on` | Service dependencies | `depends_on: [db]` |
| `networks` | Custom networks | `networks: [frontend]` |
| `restart` | Restart policy | `always`, `unless-stopped` |
| `command` | Override CMD | `python app.py` |
| `healthcheck` | Health check config | Health check commands |

### Docker Compose for Java Applications

**Example: Spring Boot + PostgreSQL**

**Directory Structure:**
```
java-app/
├── docker-compose.yml
├── Dockerfile
├── pom.xml
└── src/
    └── main/
        ├── java/
        └── resources/
            └── application.properties
```

**Dockerfile:**
```dockerfile
FROM maven:3.8-openjdk-17 AS builder

WORKDIR /app

COPY pom.xml .
RUN mvn dependency:go-offline

COPY src ./src
RUN mvn package -DskipTests

FROM openjdk:17-jre-slim

WORKDIR /app

COPY --from=builder /app/target/*.jar app.jar

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]
```

**docker-compose.yml:**
```yaml
version: '3.8'

services:
  # Spring Boot application
  app:
    build: .
    container_name: spring-app
    ports:
      - "8080:8080"
    environment:
      SPRING_DATASOURCE_URL: jdbc:postgresql://db:5432/appdb
      SPRING_DATASOURCE_USERNAME: postgres
      SPRING_DATASOURCE_PASSWORD: ${DB_PASSWORD}
      SPRING_JPA_HIBERNATE_DDL_AUTO: update
    depends_on:
      - db
    networks:
      - app-network
    restart: unless-stopped

  # PostgreSQL database
  db:
    image: postgres:15-alpine
    container_name: postgres-db
    environment:
      POSTGRES_DB: appdb
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    volumes:
      - postgres-data:/var/lib/postgresql/data
    ports:
      - "5432:5432"
    networks:
      - app-network
    restart: always

networks:
  app-network:
    driver: bridge

volumes:
  postgres-data:
```

**Commands:**
```bash
# Start application
docker compose up -d

# View logs
docker compose logs -f app

# Rebuild and restart
docker compose up -d --build

# Stop application
docker compose down
```

### Docker Compose for Node.js Applications

**Example: Express + MongoDB + Redis**

**docker-compose.yml:**
```yaml
version: '3.8'

services:
  # Node.js API
  api:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: node-api
    ports:
      - "3000:3000"
    volumes:
      - ./src:/app/src
      - /app/node_modules
    environment:
      NODE_ENV: development
      MONGODB_URI: mongodb://mongo:27017/myapp
      REDIS_HOST: redis
      REDIS_PORT: 6379
      PORT: 3000
    depends_on:
      - mongo
      - redis
    networks:
      - app-network
    command: npm run dev

  # MongoDB database
  mongo:
    image: mongo:7
    container_name: mongodb
    ports:
      - "27017:27017"
    volumes:
      - mongo-data:/data/db
    environment:
      MONGO_INITDB_DATABASE: myapp
    networks:
      - app-network
    restart: always

  # Redis cache
  redis:
    image: redis:7-alpine
    container_name: redis
    ports:
      - "6379:6379"
    volumes:
      - redis-data:/data
    networks:
      - app-network
    restart: always

  # Nginx reverse proxy
  nginx:
    image: nginx:alpine
    container_name: nginx
    ports:
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
      - api
    networks:
      - app-network
    restart: unless-stopped

networks:
  app-network:
    driver: bridge

volumes:
  mongo-data:
  redis-data:
```

**Dockerfile:**
```dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
```

### Docker Compose for Python Applications

**Example: Flask + PostgreSQL + Celery + Redis**

**docker-compose.yml:**
```yaml
version: '3.8'

services:
  # Flask web application
  web:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: flask-web
    ports:
      - "5000:5000"
    volumes:
      - ./app:/app
    environment:
      FLASK_APP: app.py
      FLASK_ENV: development
      DATABASE_URL: postgresql://postgres:password@db:5432/flaskdb
      REDIS_URL: redis://redis:6379/0
    depends_on:
      - db
      - redis
    networks:
      - app-network
    command: flask run --host=0.0.0.0

  # Celery worker
  celery:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: celery-worker
    volumes:
      - ./app:/app
    environment:
      CELERY_BROKER_URL: redis://redis:6379/0
      CELERY_RESULT_BACKEND: redis://redis:6379/0
    depends_on:
      - redis
      - db
    networks:
      - app-network
    command: celery -A app.celery worker --loglevel=info

  # PostgreSQL database
  db:
    image: postgres:15-alpine
    container_name: postgres
    environment:
      POSTGRES_DB: flaskdb
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: password
    volumes:
      - postgres-data:/var/lib/postgresql/data
    ports:
      - "5432:5432"
    networks:
      - app-network
    restart: always

  # Redis broker
  redis:
    image: redis:7-alpine
    container_name: redis
    ports:
      - "6379:6379"
    networks:
      - app-network
    restart: always

networks:
  app-network:
    driver: bridge

volumes:
  postgres-data:
```

**Dockerfile:**
```dockerfile
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 5000

CMD ["flask", "run", "--host=0.0.0.0"]
```

### Docker Compose vs Dockerfile

| Aspect | Dockerfile | Docker Compose |
|--------|-----------|----------------|
| **Purpose** | Build single image | Orchestrate multiple containers |
| **Format** | Text file with build instructions | YAML configuration |
| **Scope** | One container | Multiple services |
| **Command** | `docker build` | `docker compose up` |
| **Output** | Docker image | Running application stack |
| **Use Case** | Define how to build image | Define how to run containers |
| **Relationship** | Used by Compose | Uses Dockerfiles |

**They are Complementary:**
```
Dockerfile (builds image) → Docker Compose (runs containers)
```

**Example:**

**Dockerfile** (defines image):
```dockerfile
FROM node:18
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]
```

**docker-compose.yml** (runs containers):
```yaml
version: '3.8'
services:
  web:
    build: .  # Uses Dockerfile above
    ports:
      - "3000:3000"
  db:
    image: postgres  # Uses existing image
```

**When to Use:**
- **Dockerfile**: Always (to define custom images)
- **Docker Compose**: When you have multiple containers that work together

### Up, Down, Stop and Start Difference

**Command Comparison:**

| Command | Action | What Happens |
|---------|--------|-------------|
| `docker compose up` | Create and start | Creates containers, networks, volumes; starts services |
| `docker compose down` | Stop and remove | Stops containers, removes containers, networks, volumes (optional) |
| `docker compose stop` | Stop only | Stops containers but keeps them |
| `docker compose start` | Start existing | Starts stopped containers (no recreation) |
| `docker compose restart` | Restart | Stops then starts |
| `docker compose pause` | Pause | Suspends processes |
| `docker compose unpause` | Resume | Resumes paused processes |

**Detailed Behavior:**

#### `docker compose up`
```bash
# Basic usage
docker compose up

# Detached mode (background)
docker compose up -d

# Rebuild images before starting
docker compose up --build

# Scale specific service
docker compose up -d --scale web=3

# Recreate containers
docker compose up -d --force-recreate

# Remove orphan containers
docker compose up --remove-orphans
```

**What it does:**
1. Creates networks
2. Creates volumes
3. Builds images (if needed)
4. Creates containers
5. Starts containers
6. Attaches to logs (unless -d)

#### `docker compose down`
```bash
# Basic usage
docker compose down

# Remove volumes too
docker compose down -v

# Remove images too
docker compose down --rmi all

# Specify timeout
docker compose down -t 30
```

**What it does:**
1. Stops containers
2. Removes containers
3. Removes networks
4. Optionally removes volumes (-v)
5. Optionally removes images (--rmi)

#### `docker compose stop`
```bash
# Stop all services
docker compose stop

# Stop specific service
docker compose stop web

# Timeout before killing
docker compose stop -t 30
```

**What it does:**
1. Stops containers (SIGTERM)
2. Keeps containers (can restart)
3. Keeps networks
4. Keeps volumes

#### `docker compose start`
```bash
# Start all stopped services
docker compose start

# Start specific service
docker compose start web
```

**What it does:**
1. Starts existing stopped containers
2. No recreation
3. No rebuild

**Workflow Examples:**

```bash
# Development workflow
docker compose up -d           # Start in background
docker compose logs -f web     # Watch logs
# Make code changes...
docker compose restart web     # Restart service
docker compose down            # Clean up

# Production deployment
docker compose up -d --build   # Build and start
docker compose ps              # Verify
docker compose logs -f         # Monitor

# Maintenance
docker compose stop            # Stop temporarily
# Perform maintenance...
docker compose start           # Resume

# Complete cleanup
docker compose down -v --rmi all  # Remove everything
```

**Quick Reference:**

```bash
# First time setup
docker compose up -d

# Daily development
docker compose start           # Resume work
docker compose stop            # End of day

# Full restart
docker compose down && docker compose up -d

# Update images
docker compose down
docker compose pull
docker compose up -d

# Clean slate
docker compose down -v
docker compose up -d --build
```

---

## 11. Docker Security Best Practices

### Security Best Practices

**1. Use Official and Verified Images**
```dockerfile
# ✅ Good: Official image
FROM python:3.9-slim

# ❌ Bad: Unknown source
FROM random-user/python:latest
```

**2. Don't Run as Root**
```dockerfile
# Create non-root user
RUN adduser -D appuser
USER appuser

# Or with specific UID
RUN useradd -m -u 1000 appuser
USER 1000
```

**3. Use Specific Image Tags**
```dockerfile
# ✅ Good: Specific version
FROM node:18.17.0-alpine

# ❌ Bad: Latest (unpredictable)
FROM node:latest
```

**4. Minimize Image Layers and Size**
```dockerfile
# ✅ Good: Combined RUN
RUN apt-get update && \
    apt-get install -y pkg1 pkg2 && \
    rm -rf /var/lib/apt/lists/*

# ❌ Bad: Multiple layers
RUN apt-get update
RUN apt-get install -y pkg1
RUN apt-get install -y pkg2
```

**5. Use Multi-Stage Builds**
```dockerfile
FROM node:18 AS builder
WORKDIR /app
COPY . .
RUN npm ci && npm run build

FROM node:18-alpine
COPY --from=builder /app/dist ./dist
CMD ["node", "dist/server.js"]
```

**6. Scan Images for Vulnerabilities**
```bash
# Docker Scout (built-in)
docker scout cves myimage:latest

# Trivy
trivy image myimage:latest

# Snyk
snyk container test myimage:latest
```

**7. Use Read-Only Filesystem**
```bash
docker run --read-only \
  --tmpfs /tmp \
  myimage
```

**8. Limit Container Resources**
```bash
docker run -d \
  --memory="512m" \
  --cpus="0.5" \
  --pids-limit 100 \
  myimage
```

**9. Drop Unnecessary Capabilities**
```bash
docker run -d \
  --cap-drop=ALL \
  --cap-add=NET_BIND_SERVICE \
  myimage
```

**10. Use Secrets Management**
```bash
# Don't embed secrets in image!
# ❌ Bad
ENV DB_PASSWORD=mysecret

# ✅ Good: Use Docker secrets or env at runtime
docker run -e DB_PASSWORD=$(cat password.txt) myimage

# Or Docker Swarm secrets
docker secret create db_password password.txt
```

**11. Enable Docker Content Trust**
```bash
export DOCKER_CONTENT_TRUST=1
docker pull nginx
```

**12. Use .dockerignore**
```
# .dockerignore
.git
.env
*.md
node_modules
__pycache__
*.pyc
.DS_Store
secrets/
```

**13. Sign and Verify Images**
```bash
# Sign image
docker trust sign myimage:v1

# Verify signature
docker trust inspect myimage:v1
```

**14. Network Isolation**
```bash
# Use custom networks
docker network create --driver bridge isolated-net

# Run container on isolated network
docker run -d --network isolated-net myimage
```

**15. Regular Updates**
```bash
# Update base images regularly
docker pull python:3.9-slim
docker build -t myapp:latest .

# Update Docker Engine
sudo apt-get update && sudo apt-get upgrade docker-ce
```

**16. Use Health Checks**
```dockerfile
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s \
  CMD curl -f http://localhost/ || exit 1
```

**17. Limit Logging**
```bash
docker run -d \
  --log-driver json-file \
  --log-opt max-size=10m \
  --log-opt max-file=3 \
  myimage
```

**18. Enable User Namespaces**
```bash
# /etc/docker/daemon.json
{
  "userns-remap": "default"
}
```

**Security Checklist:**

- [ ] Use official base images
- [ ] Run containers as non-root
- [ ] Use specific image tags
- [ ] Scan images for vulnerabilities
- [ ] Use multi-stage builds
- [ ] Minimize image size
- [ ] Don't embed secrets
- [ ] Use .dockerignore
- [ ] Limit container resources
- [ ] Drop unnecessary capabilities
- [ ] Use read-only filesystem when possible
- [ ] Enable Content Trust
- [ ] Implement health checks
- [ ] Regular security updates
- [ ] Network isolation
- [ ] Monitor and log

### Secure Docker Container Images using Security Tools

**1. Trivy (Comprehensive Scanner)**

**Installation:**
```bash
# Install Trivy
curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh | sh -s -- -b /usr/local/bin
```

**Usage:**
```bash
# Scan image
trivy image python:3.9

# Scan with severity filter
trivy image --severity HIGH,CRITICAL nginx:latest

# Output to file
trivy image --format json -o results.json myimage:latest

# Scan Dockerfile
trivy config Dockerfile

# Scan filesystem
trivy fs /path/to/project
```

**2. Docker Scout (Built-in)**

```bash
# Enable Docker Scout
docker scout quickview

# Analyze image
docker scout cves myimage:latest

# Compare images
docker scout compare myimage:v1 --to myimage:v2

# Get recommendations
docker scout recommendations myimage:latest
```

**3. Snyk**

```bash
# Install Snyk
npm install -g snyk

# Authenticate
snyk auth

# Scan image
snyk container test myimage:latest

# Monitor image
snyk container monitor myimage:latest

# Test Dockerfile
snyk iac test Dockerfile
```

**4. Anchore (Deep Analysis)**

```bash
# Pull Anchore CLI
docker pull anchore/engine-cli

# Scan image
docker run --rm \
  -v /var/run/docker.sock:/var/run/docker.sock \
  anchore/engine-cli \
  analyze --image myimage:latest
```

**5. Clair (Static Analysis)**

```bash
# Run Clair server
docker run -d -p 6060:6060 \
  --name clair \
  quay.io/coreos/clair:latest

# Scan with clairctl
clairctl analyze myimage:latest
```

**6. Grype (Fast Vulnerability Scanner)**

```bash
# Install Grype
curl -sSfL https://raw.githubusercontent.com/anchore/grype/main/install.sh | sh

# Scan image
grype myimage:latest

# Output formats
grype myimage:latest -o json
grype myimage:latest -o table
```

**CI/CD Integration Examples:**

**GitHub Actions with Trivy:**
```yaml
name: Security Scan

on: [push]

jobs:
  scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Build image
        run: docker build -t myapp:${{ github.sha }} .
      
      - name: Run Trivy scan
        uses: aquasecurity/trivy-action@master
        with:
          image-ref: myapp:${{ github.sha }}
          format: 'sarif'
          output: 'trivy-results.sarif'
          severity: 'CRITICAL,HIGH'
      
      - name: Upload results
        uses: github/codeql-action/upload-sarif@v2
        with:
          sarif_file: 'trivy-results.sarif'
```

**Jenkins Pipeline with Snyk:**
```groovy
pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                sh 'docker build -t myapp:latest .'
            }
        }
        
        stage('Security Scan') {
            steps {
                sh 'snyk container test myapp:latest --severity-threshold=high'
            }
        }
    }
}
```

**Dockerfile Linting with Hadolint:**
```bash
# Install hadolint
docker pull hadolint/hadolint

# Lint Dockerfile
docker run --rm -i hadolint/hadolint < Dockerfile

# Ignore specific rules
docker run --rm -i hadolint/hadolint \
  hadolint --ignore DL3008 --ignore DL3009 - < Dockerfile
```

### What is Docker Content Trust?

**Docker Content Trust (DCT)** provides the ability to verify the integrity and publisher of Docker images through digital signatures.

**Note**: Docker is retiring DCT in favor of newer solutions like **Sigstore** and **Notation**.

**Key Concepts:**
- **Digital signatures**: Cryptographically signed images
- **Publisher verification**: Confirm image source
- **Integrity verification**: Detect tampering
- **Trust keys**: Root, targets, snapshot, timestamp keys

**Enabling DCT:**
```bash
# Enable Content Trust
export DOCKER_CONTENT_TRUST=1

# Now all pulls/pushes require signatures
docker pull nginx  # Only signed versions allowed
```

**How it Works:**

```
Developer                    Registry                   User
    │                            │                         │
    │  1. Sign image             │                         │
    ├───────────────────────────>│                         │
    │                            │                         │
    │  2. Push signed image      │                         │
    ├───────────────────────────>│                         │
    │                            │                         │
    │                            │  3. Pull image          │
    │                            │<────────────────────────┤
    │                            │                         │
    │                            │  4. Verify signature    │
    │                            │────────────────────────>│
    │                            │                         │
    │                            │  5. Trust confirmed     │
    │                            │────────────────────────>│
```

**Signing Images:**

```bash
# Generate delegation keys
docker trust key generate mykey

# Add signer
docker trust signer add --key mykey.pub myuser myimage

# Sign and push
export DOCKER_CONTENT_TRUST=1
docker push myuser/myimage:v1
# Enter passphrase for root key
# Enter passphrase for repository key
```

**Verifying Signatures:**

```bash
# Enable DCT
export DOCKER_CONTENT_TRUST=1

# Pull will verify signature
docker pull myuser/myimage:v1

# Inspect trust data
docker trust inspect --pretty myuser/myimage:v1
```

**Viewing Trust Keys:**

```bash
# List keys
docker trust key ls

# Inspect image trust
docker trust inspect myimage:v1
```

**Rotation Keys:**

```bash
# Rotate snapshot key
docker trust key rotate snapshot myimage

# Rotate targets key
docker trust key rotate targets myimage
```

**Disabling DCT (for testing):**
```bash
export DOCKER_CONTENT_TRUST=0
```

**Best Practices:**
- Enable DCT in production environments
- Protect signing keys with passphrases
- Rotate keys periodically
- Use hardware security modules (HSM) for key storage
- Document key management procedures
- Transition to newer signing methods (Sigstore, Notation)

**Modern Alternatives:**

**Cosign (Sigstore):**
```bash
# Install cosign
curl -Lo cosign https://github.com/sigstore/cosign/releases/download/v2.0.0/cosign-linux-amd64
chmod +x cosign

# Generate key pair
cosign generate-key-pair

# Sign image
cosign sign --key cosign.key myimage:v1

# Verify signature
cosign verify --key cosign.pub myimage:v1
```

**Notation:**
```bash
# Install notation
curl -Lo notation.tar.gz https://github.com/notaryproject/notation/releases/download/v1.0.0/notation_1.0.0_linux_amd64.tar.gz

# Sign image
notation sign myregistry.io/myimage:v1

# Verify signature
notation verify myregistry.io/myimage:v1
```

---

## 12. Docker in CI/CD

### Continuous Integration with Docker

**Benefits of Docker in CI/CD:**
- **Consistent environments**: Same environment everywhere
- **Isolation**: Each build in clean container
- **Parallel testing**: Run tests in parallel containers
- **Fast setup**: Spin up dependencies quickly
- **Reproducible builds**: Same result every time
- **Easy cleanup**: Dispose containers after use

**CI Workflow with Docker:**

```
┌─────────────┐    ┌──────────────┐    ┌─────────────┐    ┌──────────────┐
│ Code Commit │ -> │ Build Image  │ -> │ Run Tests   │ -> │ Push Image   │
└─────────────┘    └──────────────┘    └─────────────┘    └──────────────┘
                          │                    │                   │
                          v                    v                   v
                   Docker Build         Test Container      Docker Registry
```

**Example: Multi-Stage CI Pipeline**

**.gitlab-ci.yml:**
```yaml
stages:
  - build
  - test
  - push
  - deploy

variables:
  DOCKER_DRIVER: overlay2
  IMAGE_NAME: $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA

build:
  stage: build
  image: docker:latest
  services:
    - docker:dind
  script:
    - docker build -t $IMAGE_NAME .
    - docker save $IMAGE_NAME -o image.tar
  artifacts:
    paths:
      - image.tar

test:
  stage: test
  image: docker:latest
  services:
    - docker:dind
  dependencies:
    - build
  script:
    - docker load -i image.tar
    - docker run $IMAGE_NAME npm test
    - docker run $IMAGE_NAME npm run lint

security-scan:
  stage: test
  image: aquasec/trivy
  dependencies:
    - build
  script:
    - trivy image --exit-code 1 --severity HIGH,CRITICAL image.tar

push:
  stage: push
  image: docker:latest
  services:
    - docker:dind
  dependencies:
    - build
  script:
    - docker load -i image.tar
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    - docker push $IMAGE_NAME
    - docker tag $IMAGE_NAME $CI_REGISTRY_IMAGE:latest
    - docker push $CI_REGISTRY_IMAGE:latest
  only:
    - main

deploy:
  stage: deploy
  image: alpine
  script:
    - apk add --no-cache curl
    - curl -X POST $DEPLOY_WEBHOOK_URL
  only:
    - main
```

**GitHub Actions Example:**
```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

env:
  REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v2
      
      - name: Build image
        uses: docker/build-push-action@v4
        with:
          context: .
          load: true
          tags: ${{ env.IMAGE_NAME }}:test
          cache-from: type=gha
          cache-to: type=gha,mode=max
      
      - name: Run tests
        run: |
          docker run --rm ${{ env.IMAGE_NAME }}:test npm test
      
      - name: Security scan
        uses: aquasecurity/trivy-action@master
        with:
          image-ref: ${{ env.IMAGE_NAME }}:test
          severity: 'CRITICAL,HIGH'
      
      - name: Login to Registry
        if: github.event_name == 'push'
        uses: docker/login-action@v2
        with:
          registry: ${{ env.REGISTRY }}
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}
      
      - name: Push image
        if: github.event_name == 'push'
        uses: docker/build-push-action@v4
        with:
          context: .
          push: true
          tags: |
            ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}:latest
            ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}:${{ github.sha }}
          cache-from: type=gha
          cache-to: type=gha,mode=max
```

### Implementing CI/CD Pipelines Using Docker and Jenkins

**Prerequisites:**
- Jenkins server
- Docker installed on Jenkins server
- Docker pipeline plugin

**Step 1: Install Docker on Jenkins Server**
```bash
# Install Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# Add Jenkins user to docker group
sudo usermod -aG docker jenkins

# Restart Jenkins
sudo systemctl restart jenkins
```

**Step 2: Install Jenkins Plugins**
- Docker Pipeline
- Docker Plugin
- Git Plugin
- Pipeline Plugin

**Step 3: Create Jenkinsfile**

**Jenkinsfile (Declarative Pipeline):**
```groovy
pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'myapp'
        DOCKER_TAG = "${env.BUILD_NUMBER}"
        REGISTRY = 'docker.io'
        REGISTRY_CREDENTIAL = 'dockerhub-credentials'
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/user/repo.git'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${REGISTRY}/${DOCKER_IMAGE}:${DOCKER_TAG}")
                }
            }
        }
        
        stage('Run Tests') {
            steps {
                script {
                    dockerImage.inside {
                        sh 'npm test'
                        sh 'npm run lint'
                    }
                }
            }
        }
        
        stage('Security Scan') {
            steps {
                sh """
                    docker run --rm \
                        -v /var/run/docker.sock:/var/run/docker.sock \
                        aquasec/trivy \
                        image ${REGISTRY}/${DOCKER_IMAGE}:${DOCKER_TAG}
                """
            }
        }
        
        stage('Push to Registry') {
            when {
                branch 'main'
            }
            steps {
                script {
                    docker.withRegistry('https://' + REGISTRY, REGISTRY_CREDENTIAL) {
                        dockerImage.push("${DOCKER_TAG}")
                        dockerImage.push("latest")
                    }
                }
            }
        }
        
        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                sh """
                    docker stop myapp || true
                    docker rm myapp || true
                    docker run -d \
                        --name myapp \
                        -p 80:3000 \
                        --restart unless-stopped \
                        ${REGISTRY}/${DOCKER_IMAGE}:${DOCKER_TAG}
                """
            }
        }
    }
    
    post {
        always {
            cleanWs()
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
```

**Advanced: Multi-Branch Pipeline with Docker Compose**

```groovy
pipeline {
    agent {
        docker {
            image 'docker:latest'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    
    stages {
        stage('Build') {
            steps {
                sh 'docker-compose build'
            }
        }
        
        stage('Test') {
            steps {
                sh 'docker-compose up -d'
                sh 'docker-compose exec -T web npm test'
                sh 'docker-compose down'
            }
        }
        
        stage('Integration Tests') {
            steps {
                sh 'docker-compose -f docker-compose.test.yml up --abort-on-container-exit'
            }
        }
        
        stage('Push Images') {
            when {
                branch 'main'
            }
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker-compose push'
                }
            }
        }
    }
}
```

**Best Practices for Docker in CI/CD:**

1. **Use Docker Layer Caching**
```groovy
stage('Build with Cache') {
    steps {
        sh '''
            docker build \
                --cache-from ${DOCKER_IMAGE}:latest \
                -t ${DOCKER_IMAGE}:${BUILD_NUMBER} \
                .
        '''
    }
}
```

2. **Parallel Testing**
```groovy
stage('Parallel Tests') {
    parallel {
        stage('Unit Tests') {
            steps {
                sh 'docker run --rm myapp:test npm run test:unit'
            }
        }
        stage('Integration Tests') {
            steps {
                sh 'docker run --rm myapp:test npm run test:integration'
            }
        }
        stage('E2E Tests') {
            steps {
                sh 'docker run --rm myapp:test npm run test:e2e'
            }
        }
    }
}
```

3. **Docker-in-Docker (DinD)**
```groovy
agent {
    docker {
        image 'docker:dind'
        args '--privileged -v /var/run/docker.sock:/var/run/docker.sock'
    }
}
```

4. **Cleanup Old Images**
```groovy
post {
    always {
        sh 'docker system prune -af --filter "until=24h"'
    }
}
```

5. **Matrix Builds (Test Multiple Versions)**
```groovy
stage('Matrix Test') {
    matrix {
        axes {
            axis {
                name 'NODE_VERSION'
                values '14', '16', '18'
            }
        }
        stages {
            stage('Test') {
                steps {
                    sh "docker run --rm node:${NODE_VERSION} npm test"
                }
            }
        }
    }
}
```

---

## 13. Dockerize Java and Flask App

### Dockerizing Java Application (Spring Boot)

**Project Structure:**
```
java-app/
├── Dockerfile
├── pom.xml
├── src/
│   └── main/
│       ├── java/
│       │   └── com/example/demo/
│       │       └── DemoApplication.java
│       └── resources/
│           └── application.properties
└── .dockerignore
```

**DemoApplication.java:**
```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@SpringBootApplication
@RestController
public class DemoApplication {
    
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
    
    @GetMapping("/")
    public String hello() {
        return "Hello from Dockerized Java App!";
    }
    
    @GetMapping("/health")
    public String health() {
        return "OK";
    }
}
```

**Dockerfile (Multi-Stage Build):**
```dockerfile
# Stage 1: Build
FROM maven:3.8-openjdk-17-slim AS builder

WORKDIR /app

# Copy pom.xml first for dependency caching
COPY pom.xml .
RUN mvn dependency:go-offline -B

# Copy source code and build
COPY src ./src
RUN mvn package -DskipTests

# Stage 2: Runtime
FROM openjdk:17-jre-slim

WORKDIR /app

# Create non-root user
RUN addgroup --system spring && \
    adduser --system --ingroup spring spring

# Copy JAR from builder
COPY --from=builder /app/target/*.jar app.jar

# Change ownership
RUN chown spring:spring app.jar

# Switch to non-root user
USER spring

# Expose port
EXPOSE 8080

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=10s \
  CMD curl -f http://localhost:8080/health || exit 1

# Run application
ENTRYPOINT ["java", "-jar", "app.jar"]
```

**pom.xml:**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.1.0</version>
    </parent>
    
    <groupId>com.example</groupId>
    <artifactId>demo</artifactId>
    <version>1.0.0</version>
    <name>demo</name>
    
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
    
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```

**.dockerignore:**
```
target/
.git/
.gitignore
*.md
.env
.DS_Store
```

**Build and Run:**
```bash
# Build image
docker build -t java-app:1.0 .

# Run container
docker run -d \
  --name java-app \
  -p 8080:8080 \
  java-app:1.0

# Test
curl http://localhost:8080/

# View logs
docker logs -f java-app
```

**docker-compose.yml (with PostgreSQL):**
```yaml
version: '3.8'

services:
  app:
    build: .
    container_name: java-app
    ports:
      - "8080:8080"
    environment:
      SPRING_DATASOURCE_URL: jdbc:postgresql://db:5432/appdb
      SPRING_DATASOURCE_USERNAME: postgres
      SPRING_DATASOURCE_PASSWORD: password
    depends_on:
      - db
    networks:
      - app-network

  db:
    image: postgres:15-alpine
    container_name: postgres
    environment:
      POSTGRES_DB: appdb
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: password
    volumes:
      - postgres-data:/var/lib/postgresql/data
    networks:
      - app-network

networks:
  app-network:
    driver: bridge

volumes:
  postgres-data:
```

### Dockerizing Flask Application (Python)

**Project Structure:**
```
flask-app/
├── Dockerfile
├── requirements.txt
├── app.py
├── .dockerignore
└── templates/
    └── index.html
```

**app.py:**
```python
from flask import Flask, render_template, jsonify
import os

app = Flask(__name__)

@app.route('/')
def home():
    return render_template('index.html')

@app.route('/api/health')
def health():
    return jsonify({"status": "healthy"}), 200

@app.route('/api/info')
def info():
    return jsonify({
        "app": "Flask Dockerized App",
        "version": "1.0.0",
        "environment": os.environ.get('FLASK_ENV', 'production')
    })

if __name__ == '__main__':
    app.run(
        host='0.0.0.0',
        port=int(os.environ.get('PORT', 5000)),
        debug=os.environ.get('FLASK_ENV') == 'development'
    )
```

**requirements.txt:**
```
Flask==2.3.2
gunicorn==21.2.0
psycopg2-binary==2.9.6
redis==4.6.0
python-dotenv==1.0.0
```

**Dockerfile (Multi-Stage Build):**
```dockerfile
# Stage 1: Build dependencies
FROM python:3.9-slim AS builder

WORKDIR /app

# Install build dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends gcc && \
    rm -rf /var/lib/apt/lists/*

# Install Python dependencies
COPY requirements.txt .
RUN pip install --user --no-cache-dir -r requirements.txt

# Stage 2: Runtime
FROM python:3.9-slim

WORKDIR /app

# Copy installed packages from builder
COPY --from=builder /root/.local /root/.local

# Copy application code
COPY . .

# Create non-root user
RUN useradd -m -u 1000 flaskuser && \
    chown -R flaskuser:flaskuser /app

# Update PATH
ENV PATH=/root/.local/bin:$PATH

# Switch to non-root user
USER flaskuser

# Expose port
EXPOSE 5000

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s \
  CMD python -c "import urllib.request; urllib.request.urlopen('http://localhost:5000/api/health')" || exit 1

# Run with gunicorn for production
CMD ["gunicorn", "--bind", "0.0.0.0:5000", "--workers", "4", "app:app"]
```

**Development Dockerfile:**
```dockerfile
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

ENV FLASK_ENV=development

EXPOSE 5000

CMD ["flask", "run", "--host=0.0.0.0"]
```

**.dockerignore:**
```
__pycache__/
*.pyc
*.pyo
*.pyd
.Python
env/
venv/
.git/
.gitignore
*.md
.env
.DS_Store
```

**Build and Run:**
```bash
# Build image
docker build -t flask-app:1.0 .

# Run container
docker run -d \
  --name flask-app \
  -p 5000:5000 \
  -e FLASK_ENV=production \
  flask-app:1.0

# Test
curl http://localhost:5000/api/info

# Development mode
docker build -f Dockerfile.dev -t flask-app:dev .
docker run -d \
  --name flask-dev \
  -p 5000:5000 \
  -v $(pwd):/app \
  flask-app:dev
```

**docker-compose.yml (Full Stack):**
```yaml
version: '3.8'

services:
  web:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: flask-web
    ports:
      - "5000:5000"
    environment:
      FLASK_ENV: production
      DATABASE_URL: postgresql://postgres:password@db:5432/flaskdb
      REDIS_URL: redis://redis:6379/0
    depends_on:
      - db
      - redis
    networks:
      - app-network
    restart: unless-stopped

  db:
    image: postgres:15-alpine
    container_name: postgres
    environment:
      POSTGRES_DB: flaskdb
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: password
    volumes:
      - postgres-data:/var/lib/postgresql/data
    networks:
      - app-network
    restart: always

  redis:
    image: redis:7-alpine
    container_name: redis
    volumes:
      - redis-data:/data
    networks:
      - app-network
    restart: always

  nginx:
    image: nginx:alpine
    container_name: nginx
    ports:
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
      - web
    networks:
      - app-network
    restart: unless-stopped

networks:
  app-network:
    driver: bridge

volumes:
  postgres-data:
  redis-data:
```

**nginx.conf:**
```nginx
events {
    worker_connections 1024;
}

http {
    upstream flask {
        server web:5000;
    }

    server {
        listen 80;
        server_name localhost;

        location / {
            proxy_pass http://flask;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }

        location /static {
            alias /app/static;
        }
    }
}
```

**Run Full Stack:**
```bash
# Start all services
docker-compose up -d

# View logs
docker-compose logs -f web

# Scale web service
docker-compose up -d --scale web=3

# Stop all services
docker-compose down

# With volume cleanup
docker-compose down -v
```

---

## 14. Docker Commands

### Essential Docker Commands

**Container Management:**
```bash
# Run container
docker run [OPTIONS] IMAGE [COMMAND]
docker run -d -p 8080:80 --name web nginx

# List containers
docker ps                    # Running containers
docker ps -a                 # All containers
docker ps -q                 # Only IDs

# Start/Stop/Restart
docker start CONTAINER
docker stop CONTAINER
docker restart CONTAINER

# Pause/Unpause
docker pause CONTAINER
docker unpause CONTAINER

# Remove container
docker rm CONTAINER
docker rm -f CONTAINER       # Force remove running
docker rm $(docker ps -aq)   # Remove all

# Execute command
docker exec CONTAINER COMMAND
docker exec -it web bash

# View logs
docker logs CONTAINER
docker logs -f CONTAINER     # Follow
docker logs --tail 100 CONTAINER

# Inspect container
docker inspect CONTAINER
docker stats CONTAINER       # Resource usage

# Copy files
docker cp CONTAINER:/path /host/path
docker cp /host/path CONTAINER:/path

# Attach to container
docker attach CONTAINER

# View processes
docker top CONTAINER
```

**Image Management:**
```bash
# List images
docker images
docker image ls

# Pull image
docker pull IMAGE:TAG
docker pull ubuntu:22.04

# Build image
docker build -t NAME:TAG .
docker build -f Dockerfile.prod -t app:prod .

# Tag image
docker tag SOURCE TARGET
docker tag app:latest app:v1.0

# Push image
docker push NAME:TAG

# Remove image
docker rmi IMAGE
docker rmi $(docker images -q)   # Remove all

# Save/Load image
docker save IMAGE -o file.tar
docker load -i file.tar

# Import/Export
docker export CONTAINER -o file.tar
docker import file.tar IMAGE:TAG

# Image history
docker history IMAGE

# Prune unused images
docker image prune
docker image prune -a        # All unused
```

**Network Management:**
```bash
# List networks
docker network ls

# Create network
docker network create NAME
docker network create --driver bridge my-net

# Inspect network
docker network inspect NAME

# Connect container
docker network connect NETWORK CONTAINER

# Disconnect container
docker network disconnect NETWORK CONTAINER

# Remove network
docker network rm NETWORK
docker network prune         # Remove unused
```

**Volume Management:**
```bash
# List volumes
docker volume ls

# Create volume
docker volume create NAME

# Inspect volume
docker volume inspect NAME

# Remove volume
docker volume rm NAME
docker volume prune          # Remove unused
```

**Docker Compose Commands:**
```bash
# Start services
docker compose up
docker compose up -d         # Detached

# Stop services
docker compose down
docker compose down -v       # With volumes

# View services
docker compose ps

# View logs
docker compose logs
docker compose logs -f SERVICE

# Execute command
docker compose exec SERVICE COMMAND

# Build services
docker compose build
docker compose up --build

# Scale service
docker compose up -d --scale web=3

# Restart service
docker compose restart SERVICE

# Stop without removing
docker compose stop
docker compose start
```

**System Commands:**
```bash
# System info
docker info
docker version

# Disk usage
docker system df

# Clean up
docker system prune          # All unused objects
docker system prune -a       # Including images
docker system prune --volumes

# Events
docker events
docker events --filter 'type=container'
```

**Registry Commands:**
```bash
# Login
docker login
docker login registry.example.com

# Logout
docker logout

# Search
docker search nginx
```

---

## 15. Docker Cheat Sheet

### Quick Reference

**Container Operations**
```bash
# Run container
docker run -d -p 8080:80 --name web nginx
docker run -it ubuntu bash
docker run --rm alpine echo "hello"

# Lifecycle
docker start|stop|restart|pause|unpause CONTAINER
docker kill CONTAINER

# Remove
docker rm CONTAINER
docker rm -f $(docker ps -aq)

# Logs & Stats
docker logs -f CONTAINER
docker stats CONTAINER
docker top CONTAINER

# Execute
docker exec -it CONTAINER bash
docker exec CONTAINER command

# Copy
docker cp CONTAINER:/path /host
docker cp /host CONTAINER:/path
```

**Image Operations**
```bash
# Build
docker build -t name:tag .
docker build --no-cache -t name:tag .

# Pull/Push
docker pull image:tag
docker push image:tag

# List/Remove
docker images
docker rmi image
docker image prune -a

# Tag
docker tag source target

# Save/Load
docker save image > file.tar
docker load < file.tar
```

**Network Operations**
```bash
# Create
docker network create net-name

# List
docker network ls

# Connect
docker run --network net-name image
docker network connect net CONTAINER

# Inspect
docker network inspect net-name

# Remove
docker network rm net-name
docker network prune
```

**Volume Operations**
```bash
# Create
docker volume create vol-name

# Mount
docker run -v vol-name:/path image
docker run -v /host/path:/container/path image

# List
docker volume ls

# Inspect
docker volume inspect vol-name

# Remove
docker volume rm vol-name
docker volume prune
```

**Docker Compose**
```bash
# Start
docker compose up -d

# Stop
docker compose down
docker compose down -v

# Logs
docker compose logs -f

# Build
docker compose build
docker compose up --build

# Scale
docker compose up -d --scale web=3

# Execute
docker compose exec service bash
```

**System Maintenance**
```bash
# Info
docker info
docker version

# Cleanup
docker system prune
docker system prune -a --volumes

# Disk usage
docker system df
```

**Common Run Options**
```bash
-d              # Detached mode
-it             # Interactive with TTY
--name          # Container name
-p 8080:80      # Port mapping
-v vol:/path    # Volume mount
-e KEY=value    # Environment variable
--network net   # Network
--rm            # Remove after stop
--restart       # Restart policy
-m 512m         # Memory limit
--cpus 2        # CPU limit
--user 1000     # Run as user
```

**Dockerfile Instructions**
```dockerfile
FROM image:tag
LABEL key=value
ENV KEY=value
WORKDIR /path
COPY src dest
ADD src dest
RUN command
EXPOSE port
VOLUME /path
USER username
CMD ["executable"]
ENTRYPOINT ["executable"]
HEALTHCHECK CMD command
```

**docker-compose.yml Template**
```yaml
version: '3.8'

services:
  web:
    image: nginx
    # or build: .
    container_name: web
    ports:
      - "80:80"
    volumes:
      - ./data:/data
    environment:
      - KEY=value
    depends_on:
      - db
    networks:
      - app-net
    restart: unless-stopped

  db:
    image: postgres:15
    volumes:
      - db-data:/var/lib/postgresql/data
    networks:
      - app-net

networks:
  app-net:

volumes:
  db-data:
```

**Troubleshooting**
```bash
# Container not starting
docker logs container_name
docker inspect container_name

# Network issues
docker network inspect network_name
docker exec container_name ping other_container

# Permission issues
docker exec -u 0 container_name command

# Disk space
docker system df
docker system prune -a

# Container stuck
docker kill container_name
docker rm -f container_name
```

**Best Practices**
- Use specific image tags, not `latest`
- Use multi-stage builds
- Run containers as non-root
- Use .dockerignore
- Minimize layers
- Scan images for vulnerabilities
- Use volumes for persistent data
- Clean up regularly
- Document with labels
- Use health checks

**Security Tips**
```bash
# Scan image
docker scout cves image:tag
trivy image image:tag

# Run as non-root
USER 1000

# Read-only filesystem
docker run --read-only image

# Drop capabilities
docker run --cap-drop=ALL image

# Limit resources
docker run -m 512m --cpus=0.5 image

# Enable Content Trust
export DOCKER_CONTENT_TRUST=1
```

---

## Additional Resources

**Official Documentation:**
- Docker Docs: https://docs.docker.com
- Docker Hub: https://hub.docker.com
- Docker Compose: https://docs.docker.com/compose

**Community:**
- Docker Forums: https://forums.docker.com
- Stack Overflow: [docker] tag
- GitHub: https://github.com/docker

**Learning:**
- Docker Training: https://www.docker.com/resources/trainings
- Play with Docker: https://labs.play-with-docker.com
- Docker Samples: https://github.com/dockersamples

**Tools:**
- Trivy: Security scanner
- Docker Scout: Vulnerability analysis
- Hadolint: Dockerfile linter
- Dive: Image layer analyzer

---

**End of Comprehensive Docker Guide**
