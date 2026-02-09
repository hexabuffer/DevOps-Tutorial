# Docker Complete Learning Path: Beginner to Advanced Practitioner

> **A Comprehensive Guide to Mastering Docker Containerization Technology**

---

## 📚 Table of Contents

- [Introduction](#introduction)
- [Learning Path Overview](#learning-path-overview)
- [Phase 1: Foundations (Beginner)](#phase-1-foundations-beginner)
- [Phase 2: Core Skills (Intermediate)](#phase-2-core-skills-intermediate)
- [Phase 3: Advanced Operations (Advanced)](#phase-3-advanced-operations-advanced)
- [Phase 4: Production & Specialization (Expert)](#phase-4-production--specialization-expert)
- [Hands-On Projects](#hands-on-projects)
- [Learning Resources](#learning-resources)
- [Certification Path](#certification-path)

---

## Introduction

### What You'll Learn

This comprehensive learning path takes you from Docker basics to advanced production deployments, covering:

- **Container fundamentals** and Docker architecture
- **Image creation** and optimization techniques
- **Networking, storage**, and resource management
- **Multi-container orchestration** with Docker Compose
- **Production deployment** with Kubernetes
- **Security best practices** and performance optimization
- **Specialized applications** (CI/CD, microservices, data science)

### Prerequisites

- Basic command-line proficiency (Linux/Unix commands)
- Understanding of software development concepts
- Familiarity with version control (Git recommended)
- Basic networking knowledge helpful but not required

### Time Commitment

- **Beginner Phase**: 2-3 weeks
- **Intermediate Phase**: 3-4 weeks
- **Advanced Phase**: 4-6 weeks
- **Expert Phase**: 6-8 weeks
- **Total Estimated Time**: 15-21 weeks (3-5 months)

---

## Learning Path Overview

```
BEGINNER → INTERMEDIATE → ADVANCED → EXPERT
   ↓            ↓             ↓          ↓
Concepts    Workflows    Orchestration  Production
Basics      Images       Networking     Security
Commands    Compose      Volumes        Optimization
                         Swarm          Kubernetes
                                       CI/CD
                                       Microservices
```

---

## Phase 1: Foundations (Beginner)

**Duration**: 2-3 weeks  
**Goal**: Understand containerization concepts and basic Docker operations

### 1.1 Understanding Docker

**Topics to Master:**
- What is Docker and why use it?
- Containerization vs. virtualization
- Docker architecture (daemon, client, registry)
- Docker components overview

**Key Concepts:**
- **Containers**: Lightweight, isolated runtime environments
- **Images**: Read-only templates for creating containers
- **Docker Hub**: Public registry for container images
- **Dockerfile**: Script to build custom images

**Learning Activities:**
- Read Docker documentation on architecture
- Watch introductory videos on containerization
- Draw diagrams comparing VMs vs. containers

**Success Criteria:**
✅ Explain Docker's benefits over virtual machines  
✅ Understand client-server architecture  
✅ Identify key Docker components  

---

### 1.2 Installation & Setup

**Topics to Master:**
- Docker editions (Desktop vs. Engine)
- Platform-specific installation (Linux, macOS, Windows)
- Post-installation configuration
- Verification and troubleshooting

**Hands-On Practice:**

```bash
# Verify installation
docker --version
docker info

# Test with hello-world
docker run hello-world

# Configure user permissions (Linux)
sudo usermod -aG docker $USER

# Verify daemon status
systemctl status docker
```

**Common Issues to Resolve:**
- Permission denied errors
- Daemon connection failures
- WSL2 configuration on Windows

**Success Criteria:**
✅ Successfully install Docker on your platform  
✅ Run containers without sudo (Linux)  
✅ Access Docker Dashboard (Desktop) or verify daemon status  

---

### 1.3 Working with Containers

**Topics to Master:**
- Running your first container
- Container lifecycle (create, start, stop, remove)
- Listing and inspecting containers
- Interactive vs. detached modes
- Port mapping and exposure

**Essential Commands:**

```bash
# Run containers
docker run nginx                    # Foreground
docker run -d nginx                # Detached
docker run -it ubuntu bash         # Interactive

# List containers
docker ps                          # Running
docker ps -a                       # All containers

# Container management
docker start <container_id>
docker stop <container_id>
docker restart <container_id>
docker rm <container_id>

# Port mapping
docker run -d -p 8080:80 nginx

# View logs
docker logs <container_id>
docker logs -f <container_id>      # Follow mode

# Execute commands in running containers
docker exec -it <container_id> bash
```

**Practical Exercises:**

1. **Web Server Exercise**
   ```bash
   # Run nginx web server
   docker run -d -p 8080:80 --name my-nginx nginx
   
   # Access in browser: http://localhost:8080
   
   # View logs
   docker logs my-nginx
   
   # Stop and remove
   docker stop my-nginx
   docker rm my-nginx
   ```

2. **Interactive Container**
   ```bash
   # Start interactive Ubuntu container
   docker run -it --name my-ubuntu ubuntu bash
   
   # Inside container, explore
   apt update
   apt install curl
   curl google.com
   exit
   
   # Restart and attach
   docker start my-ubuntu
   docker attach my-ubuntu
   ```

**Success Criteria:**
✅ Run containers in foreground and detached modes  
✅ Map ports and access containerized applications  
✅ Execute commands inside running containers  
✅ Clean up containers after use  

---

### 1.4 Understanding Docker Images

**Topics to Master:**
- What are Docker images?
- Image layers and caching
- Image registries (Docker Hub, others)
- Pulling and managing images
- Image tags and versions

**Essential Commands:**

```bash
# List images
docker images
docker image ls

# Pull images
docker pull ubuntu
docker pull nginx:1.21-alpine

# Image inspection
docker inspect nginx
docker history nginx

# Remove images
docker rmi nginx
docker image prune              # Remove unused images
```

**Understanding Tags:**
```bash
# Different versions
docker pull python:3.9
docker pull python:3.9-slim
docker pull python:3.9-alpine

# Latest tag (default)
docker pull redis              # Same as redis:latest
```

**Practical Exercises:**

1. **Image Exploration**
   ```bash
   # Pull multiple versions
   docker pull ubuntu:20.04
   docker pull ubuntu:22.04
   
   # Compare image sizes
   docker images ubuntu
   
   # Inspect layers
   docker history ubuntu:22.04
   ```

2. **Image Cleanup**
   ```bash
   # List all images
   docker images
   
   # Remove specific image
   docker rmi ubuntu:20.04
   
   # Remove dangling images
   docker image prune
   
   # Remove all unused images
   docker image prune -a
   ```

**Success Criteria:**
✅ Pull images from Docker Hub  
✅ Understand image tagging conventions  
✅ Inspect image layers and metadata  
✅ Manage and clean up images  

---

### 1.5 Basic Dockerfile Creation

**Topics to Master:**
- Dockerfile syntax and structure
- Essential instructions (FROM, RUN, COPY, CMD)
- Building images from Dockerfiles
- Basic image optimization

**Fundamental Instructions:**

| Instruction | Purpose | Example |
|-------------|---------|---------|
| `FROM` | Base image | `FROM ubuntu:22.04` |
| `RUN` | Execute commands during build | `RUN apt-get update` |
| `COPY` | Copy files from host | `COPY app.py /app/` |
| `WORKDIR` | Set working directory | `WORKDIR /app` |
| `CMD` | Default command | `CMD ["python", "app.py"]` |
| `EXPOSE` | Document port | `EXPOSE 8080` |

**First Dockerfile Example:**

```dockerfile
# Simple Python web app
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

EXPOSE 5000

CMD ["python", "app.py"]
```

**Building and Running:**

```bash
# Build image
docker build -t my-python-app:1.0 .

# Run container from custom image
docker run -d -p 5000:5000 my-python-app:1.0
```

**Practical Projects:**

**Project 1: Static Website**
```dockerfile
# Dockerfile
FROM nginx:alpine
COPY ./html /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

```bash
# Build and run
docker build -t my-website .
docker run -d -p 8080:80 my-website
```

**Project 2: Node.js Application**
```dockerfile
FROM node:16-alpine

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3000
CMD ["node", "server.js"]
```

**Success Criteria:**
✅ Write basic Dockerfiles  
✅ Build custom images  
✅ Understand layer caching  
✅ Run containers from custom images  

---

### Phase 1 Milestone Project

**Goal**: Create a containerized web application with a database

**Project**: Simple Blog Application

```yaml
# docker-compose.yml (preview for next phase)
version: '3.8'
services:
  app:
    build: .
    ports:
      - "5000:5000"
  db:
    image: postgres:13
    environment:
      POSTGRES_PASSWORD: secret
```

**Deliverables:**
- Working Dockerfile for application
- Ability to run app container
- Understanding of container networking basics

---

## Phase 2: Core Skills (Intermediate)

**Duration**: 3-4 weeks  
**Goal**: Master container workflows, multi-container applications, and data persistence

### 2.1 Advanced Dockerfile Techniques

**Topics to Master:**
- Multi-stage builds
- Layer optimization
- Build arguments and environment variables
- .dockerignore files
- Dockerfile best practices

**Multi-Stage Builds:**

```dockerfile
# Build stage
FROM node:16 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Production stage
FROM node:16-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
EXPOSE 3000
CMD ["node", "dist/server.js"]
```

**Benefits:**
- Smaller final images
- Separate build and runtime dependencies
- Better security (no build tools in production)

**Build Arguments:**

```dockerfile
FROM python:3.9-slim

ARG APP_VERSION=1.0
ARG ENVIRONMENT=production

ENV VERSION=${APP_VERSION}
ENV ENV=${ENVIRONMENT}

WORKDIR /app
COPY . .

RUN echo "Building version ${VERSION} for ${ENVIRONMENT}"
CMD ["python", "app.py"]
```

```bash
# Build with arguments
docker build --build-arg APP_VERSION=2.0 --build-arg ENVIRONMENT=staging -t myapp:2.0 .
```

**Optimization with .dockerignore:**

```
# .dockerignore
node_modules
npm-debug.log
.git
.env
*.md
tests/
.dockerignore
Dockerfile
```

**Layer Optimization:**

```dockerfile
# ❌ Bad - reinstalls dependencies on any code change
FROM node:16
WORKDIR /app
COPY . .
RUN npm install
CMD ["node", "server.js"]

# ✅ Good - caches dependencies layer
FROM node:16
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
CMD ["node", "server.js"]
```

**Success Criteria:**
✅ Implement multi-stage builds  
✅ Reduce image sizes by 50%+  
✅ Use build arguments effectively  
✅ Optimize layer caching  

---

### 2.2 Docker Networking

**Topics to Master:**
- Network drivers (bridge, host, overlay, macvlan)
- Creating and managing networks
- Container communication
- DNS resolution
- Network troubleshooting

**Network Drivers:**

| Driver | Use Case | Isolation |
|--------|----------|-----------|
| **bridge** | Default, single host | Container-to-container |
| **host** | Performance-critical | None (uses host network) |
| **overlay** | Multi-host (Swarm) | Cross-host communication |
| **macvlan** | Legacy app integration | Direct MAC address |

**Essential Commands:**

```bash
# List networks
docker network ls

# Create custom network
docker network create my-network
docker network create --driver bridge --subnet 172.20.0.0/16 custom-net

# Inspect network
docker network inspect my-network

# Run containers on network
docker run -d --name web --network my-network nginx
docker run -d --name api --network my-network python:3.9

# Connect existing container
docker network connect my-network existing-container

# Disconnect
docker network disconnect my-network existing-container

# Remove network
docker network rm my-network
```

**Container Communication:**

```bash
# Create network
docker network create app-network

# Run database
docker run -d --name db --network app-network \
  -e POSTGRES_PASSWORD=secret postgres:13

# Run app (can access db by hostname "db")
docker run -d --name app --network app-network \
  -e DATABASE_URL=postgresql://postgres:secret@db:5432/mydb \
  my-python-app
```

**Testing Connectivity:**

```bash
# Enter container
docker exec -it app bash

# Test DNS resolution
nslookup db
ping db

# Test connection
curl http://db:5432
```

**Network Troubleshooting:**

```bash
# Debug with network tools container
docker run -it --network app-network nicolaka/netshoot

# Inside container
ping db
traceroute db
nmap db -p 5432
```

**Success Criteria:**
✅ Create custom bridge networks  
✅ Connect containers for inter-service communication  
✅ Understand DNS-based service discovery  
✅ Troubleshoot network connectivity issues  

---

### 2.3 Docker Volumes & Data Persistence

**Topics to Master:**
- Volume types (named, anonymous, bind mounts)
- Volume lifecycle management
- Data backup and restore
- Volume drivers
- tmpfs mounts

**Volume Types Comparison:**

| Type | Syntax | Use Case | Managed by Docker |
|------|--------|----------|-------------------|
| **Named Volume** | `my-vol:/data` | Production data | ✅ Yes |
| **Anonymous Volume** | `/data` | Temporary data | ✅ Yes |
| **Bind Mount** | `/host/path:/container/path` | Development | ❌ No |
| **tmpfs** | `--tmpfs /tmp` | Sensitive temp data | N/A (RAM) |

**Working with Volumes:**

```bash
# Create volume
docker volume create my-data

# List volumes
docker volume ls

# Inspect volume
docker volume inspect my-data

# Run container with volume
docker run -d -v my-data:/app/data my-app

# Remove volume
docker volume rm my-data

# Remove unused volumes
docker volume prune
```

**Practical Examples:**

**1. Database Persistence:**
```bash
# Create volume for PostgreSQL
docker volume create postgres-data

# Run PostgreSQL with persistent storage
docker run -d \
  --name postgres \
  -v postgres-data:/var/lib/postgresql/data \
  -e POSTGRES_PASSWORD=secret \
  postgres:13

# Data persists even after container removal
docker rm -f postgres
docker run -d --name postgres -v postgres-data:/var/lib/postgresql/data \
  -e POSTGRES_PASSWORD=secret postgres:13
```

**2. Bind Mounts for Development:**
```bash
# Mount local code into container for live development
docker run -d \
  -p 3000:3000 \
  -v $(pwd):/app \
  -v /app/node_modules \
  node:16 \
  npm start

# Changes to local files immediately reflect in container
```

**3. Read-Only Volumes:**
```bash
# Mount configuration as read-only
docker run -d \
  -v $(pwd)/config:/etc/app/config:ro \
  my-app
```

**Data Backup and Restore:**

```bash
# Backup volume to tar archive
docker run --rm \
  -v my-data:/data \
  -v $(pwd):/backup \
  ubuntu \
  tar czf /backup/backup.tar.gz /data

# Restore from backup
docker run --rm \
  -v my-data:/data \
  -v $(pwd):/backup \
  ubuntu \
  tar xzf /backup/backup.tar.gz -C /
```

**tmpfs Mounts (RAM-based):**

```bash
# Mount tmpfs for sensitive data
docker run -d \
  --tmpfs /tmp:rw,size=100m,mode=1777 \
  my-app

# Data stored in RAM, never written to disk
```

**Success Criteria:**
✅ Use named volumes for data persistence  
✅ Implement bind mounts for development workflows  
✅ Back up and restore volume data  
✅ Choose appropriate volume type for use cases  

---

### 2.4 Docker Compose

**Topics to Master:**
- docker-compose.yml syntax
- Service definitions
- Multi-container orchestration
- Environment variables and secrets
- Compose commands and workflows
- Networking in Compose
- Volume management in Compose

**Basic docker-compose.yml Structure:**

```yaml
version: '3.8'

services:
  service-name:
    image: image:tag
    # OR
    build: ./path
    ports:
      - "host:container"
    environment:
      KEY: value
    volumes:
      - volume-name:/path
    networks:
      - network-name
    depends_on:
      - other-service

volumes:
  volume-name:

networks:
  network-name:
```

**Essential Compose Commands:**

```bash
# Start services
docker-compose up
docker-compose up -d                    # Detached
docker-compose up --build               # Rebuild images

# Stop services
docker-compose down                     # Stop and remove
docker-compose down -v                  # Also remove volumes
docker-compose stop                     # Stop without removing

# View status
docker-compose ps
docker-compose logs
docker-compose logs -f service-name     # Follow logs

# Execute commands
docker-compose exec service-name bash
docker-compose run service-name command

# Scale services
docker-compose up -d --scale web=3

# Rebuild
docker-compose build
docker-compose build --no-cache
```

**Practical Example 1: WordPress Stack**

```yaml
version: '3.8'

services:
  wordpress:
    image: wordpress:latest
    ports:
      - "8080:80"
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: wpuser
      WORDPRESS_DB_PASSWORD: wppass
      WORDPRESS_DB_NAME: wordpress
    volumes:
      - wordpress-data:/var/www/html
    depends_on:
      - db
    networks:
      - wp-network

  db:
    image: mysql:8.0
    environment:
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wpuser
      MYSQL_PASSWORD: wppass
      MYSQL_ROOT_PASSWORD: rootpass
    volumes:
      - db-data:/var/lib/mysql
    networks:
      - wp-network

volumes:
  wordpress-data:
  db-data:

networks:
  wp-network:
```

**Practical Example 2: Full-Stack Application**

```yaml
version: '3.8'

services:
  # Frontend
  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
    environment:
      REACT_APP_API_URL: http://localhost:8000
    volumes:
      - ./frontend:/app
      - /app/node_modules
    depends_on:
      - backend
    networks:
      - app-network

  # Backend API
  backend:
    build: ./backend
    ports:
      - "8000:8000"
    environment:
      DATABASE_URL: postgresql://user:pass@db:5432/appdb
      REDIS_URL: redis://cache:6379
    volumes:
      - ./backend:/app
    depends_on:
      - db
      - cache
    networks:
      - app-network

  # Database
  db:
    image: postgres:13
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
      POSTGRES_DB: appdb
    volumes:
      - db-data:/var/lib/postgresql/data
    networks:
      - app-network

  # Cache
  cache:
    image: redis:6-alpine
    networks:
      - app-network

  # Nginx Reverse Proxy
  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
      - frontend
      - backend
    networks:
      - app-network

volumes:
  db-data:

networks:
  app-network:
    driver: bridge
```

**Advanced Features:**

**Environment Variables:**

```yaml
# Using .env file
version: '3.8'
services:
  app:
    image: myapp:${VERSION:-latest}
    environment:
      - DATABASE_URL=${DB_URL}
      - API_KEY=${API_KEY}
```

**.env file:**
```
VERSION=1.2.3
DB_URL=postgresql://localhost/db
API_KEY=secret123
```

**Extending Services:**

```yaml
# docker-compose.base.yml
version: '3.8'
services:
  app:
    image: myapp
    environment:
      - ENV=base

# docker-compose.override.yml (automatically loaded)
version: '3.8'
services:
  app:
    environment:
      - ENV=development
    volumes:
      - ./src:/app/src
```

**Healthchecks:**

```yaml
services:
  app:
    image: myapp
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8080/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 40s
```

**Success Criteria:**
✅ Write multi-service docker-compose.yml files  
✅ Orchestrate full-stack applications  
✅ Use environment variables and secrets  
✅ Implement service dependencies and health checks  

---

### 2.5 Image Management & Registries

**Topics to Master:**
- Image tagging strategies
- Pushing to Docker Hub
- Private registries
- Image scanning and security
- Image cleanup and management

**Tagging Best Practices:**

```bash
# Semantic versioning
docker tag myapp:latest myapp:1.2.3
docker tag myapp:latest myapp:1.2
docker tag myapp:latest myapp:1

# Environment-specific tags
docker tag myapp:latest myapp:production
docker tag myapp:latest myapp:staging

# Git commit tags
docker tag myapp:latest myapp:$(git rev-parse --short HEAD)
```

**Docker Hub Workflow:**

```bash
# Login to Docker Hub
docker login

# Tag with username
docker tag myapp:latest username/myapp:1.0

# Push to Docker Hub
docker push username/myapp:1.0
docker push username/myapp:latest

# Pull from Docker Hub
docker pull username/myapp:1.0
```

**Private Registry:**

```bash
# Run local registry
docker run -d -p 5000:5000 --name registry registry:2

# Tag for private registry
docker tag myapp:latest localhost:5000/myapp:1.0

# Push to private registry
docker push localhost:5000/myapp:1.0

# Pull from private registry
docker pull localhost:5000/myapp:1.0
```

**Image Scanning:**

```bash
# Scan with Docker (built-in)
docker scan myapp:latest

# Scan with Trivy
docker run aquasec/trivy image myapp:latest

# Scan with Snyk
snyk container test myapp:latest
```

**Image Cleanup Strategies:**

```bash
# Remove dangling images
docker image prune

# Remove all unused images
docker image prune -a

# Remove images older than 24h
docker image prune -a --filter "until=24h"

# Automated cleanup script
docker system df                    # Show disk usage
docker system prune -a --volumes    # Clean everything
```

**Success Criteria:**
✅ Implement tagging strategies  
✅ Push images to Docker Hub  
✅ Set up private registry  
✅ Scan images for vulnerabilities  
✅ Maintain clean image inventory  

---

### Phase 2 Milestone Project

**Goal**: Build and deploy a microservices application with Docker Compose

**Project**: E-commerce Platform

**Architecture:**
- Frontend (React)
- API Gateway (Node.js)
- Product Service (Python)
- Order Service (Go)
- PostgreSQL Database
- Redis Cache
- Nginx Load Balancer

**Deliverables:**
- Complete docker-compose.yml
- Dockerfiles for each service
- Network isolation
- Data persistence
- Environment configuration
- Health checks
- Scaling capability

---

## Phase 3: Advanced Operations (Advanced)

**Duration**: 4-6 weeks  
**Goal**: Master production deployment, orchestration, security, and optimization

### 3.1 Docker Security Best Practices

**Topics to Master:**
- Security scanning and hardening
- Non-root users in containers
- Secret management
- Content trust and signing
- Runtime security
- Network security
- Compliance and auditing

**1. Use Official and Minimal Base Images:**

```dockerfile
# ❌ Avoid
FROM ubuntu:latest

# ✅ Better - official slim variant
FROM python:3.9-slim

# ✅ Best - Alpine-based (smallest)
FROM python:3.9-alpine
```

**2. Non-Root User:**

```dockerfile
FROM node:16-alpine

# Create app user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

WORKDIR /app

# Copy and install as root
COPY package*.json ./
RUN npm install --production

# Copy app files
COPY . .

# Change ownership
RUN chown -R appuser:appgroup /app

# Switch to non-root user
USER appuser

EXPOSE 3000
CMD ["node", "server.js"]
```

**3. Secret Management:**

```bash
# ❌ Never hardcode secrets
docker run -e PASSWORD=mysecret myapp

# ✅ Use Docker secrets (Swarm)
echo "mysecret" | docker secret create db_password -
docker service create --secret db_password myapp

# ✅ Use environment files
docker run --env-file .env myapp

# ✅ Use external secret managers
docker run -e PASSWORD=$(aws secretsmanager get-secret-value --secret-id db_pass) myapp
```

**4. Read-Only File Systems:**

```bash
# Run container with read-only root filesystem
docker run --read-only -v /app/tmp:/tmp myapp

# In docker-compose.yml
services:
  app:
    image: myapp
    read_only: true
    tmpfs:
      - /tmp
      - /run
```

**5. Resource Limits:**

```bash
# Limit CPU and memory
docker run -d \
  --memory="512m" \
  --cpus="1.0" \
  --pids-limit=100 \
  myapp

# In docker-compose.yml
services:
  app:
    image: myapp
    deploy:
      resources:
        limits:
          cpus: '1.0'
          memory: 512M
        reservations:
          cpus: '0.5'
          memory: 256M
```

**6. Network Segmentation:**

```yaml
version: '3.8'
services:
  frontend:
    networks:
      - public
  api:
    networks:
      - public
      - backend
  database:
    networks:
      - backend

networks:
  public:
  backend:
    internal: true  # No external access
```

**7. Image Scanning in CI/CD:**

```yaml
# .github/workflows/docker.yml
name: Docker Security Scan
on: [push]
jobs:
  scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Build image
        run: docker build -t myapp:${{ github.sha }} .
      - name: Scan with Trivy
        uses: aquasecurity/trivy-action@master
        with:
          image-ref: myapp:${{ github.sha }}
          severity: 'CRITICAL,HIGH'
```

**8. Enable Content Trust:**

```bash
# Enable Docker Content Trust
export DOCKER_CONTENT_TRUST=1

# Push signed image
docker push username/myapp:1.0

# Pull only signed images
docker pull username/myapp:1.0
```

**9. Capability Dropping:**

```bash
# Drop all capabilities, add only needed ones
docker run --cap-drop=ALL --cap-add=NET_BIND_SERVICE myapp

# In docker-compose.yml
services:
  app:
    cap_drop:
      - ALL
    cap_add:
      - NET_BIND_SERVICE
```

**10. Security Auditing:**

```bash
# Audit Docker configuration
docker run -it --net host --pid host --cap-add audit_control \
  -v /var/lib:/var/lib \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v /etc:/etc \
  aquasec/docker-bench:latest

# Check container compliance
docker run -it --rm -v /var/run/docker.sock:/var/run/docker.sock \
  aquasec/docker-security-checker
```

**Security Checklist:**

- [ ] Use official, minimal base images
- [ ] Run containers as non-root users
- [ ] Scan images for vulnerabilities
- [ ] Enable Docker Content Trust
- [ ] Use secrets management
- [ ] Implement resource limits
- [ ] Enable read-only filesystems where possible
- [ ] Segment networks appropriately
- [ ] Drop unnecessary capabilities
- [ ] Keep Docker and images updated
- [ ] Enable logging and monitoring
- [ ] Regular security audits

**Success Criteria:**
✅ Implement multi-layered security  
✅ Pass security scanning with no critical vulnerabilities  
✅ Use non-root users in all containers  
✅ Implement secret management  
✅ Configure network isolation  

---

### 3.2 Performance Optimization

**Topics to Master:**
- Image optimization techniques
- Resource management
- Networking performance
- Storage optimization
- Logging efficiency
- Application-level tuning
- Benchmarking and profiling

**1. Multi-Stage Build Optimization:**

```dockerfile
# ❌ Unoptimized - 1.2GB final image
FROM node:16
WORKDIR /app
COPY . .
RUN npm install
RUN npm run build
CMD ["npm", "start"]

# ✅ Optimized - 150MB final image
# Build stage
FROM node:16-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
RUN npm run build

# Production stage
FROM node:16-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
ENV NODE_ENV=production
USER node
CMD ["node", "dist/server.js"]
```

**2. Layer Caching Optimization:**

```dockerfile
# ✅ Optimal layer ordering (least to most frequently changed)
FROM python:3.9-slim

# System dependencies (rarely change)
RUN apt-get update && apt-get install -y \
    gcc \
    && rm -rf /var/lib/apt/lists/*

# Python dependencies (change occasionally)
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Application code (changes frequently)
COPY . /app
WORKDIR /app

CMD ["python", "app.py"]
```

**3. Use .dockerignore:**

```
# .dockerignore
**/.git
**/.gitignore
**/node_modules
**/npm-debug.log
**/.env
**/dist
**/build
**/*.md
**/.DS_Store
**/coverage
**/*.test.js
```

**4. Resource Constraints:**

```yaml
# docker-compose.yml
version: '3.8'
services:
  app:
    image: myapp
    deploy:
      resources:
        limits:
          cpus: '2.0'
          memory: 2G
        reservations:
          cpus: '1.0'
          memory: 1G
    # CPU pinning for consistency
    cpuset: "0,1"
```

**5. Network Performance:**

```bash
# Use host networking for max performance (when security allows)
docker run --network host myapp

# Disable userland proxy for better performance
# In /etc/docker/daemon.json
{
  "userland-proxy": false
}
```

**6. Storage Driver Optimization:**

```json
// /etc/docker/daemon.json
{
  "storage-driver": "overlay2",
  "storage-opts": [
    "overlay2.override_kernel_check=true"
  ]
}
```

**7. Logging Optimization:**

```bash
# Limit log size
docker run -d \
  --log-driver json-file \
  --log-opt max-size=10m \
  --log-opt max-file=3 \
  myapp

# Use faster logging driver
docker run -d \
  --log-driver local \
  --log-opt max-size=10m \
  myapp
```

**8. Enable BuildKit:**

```bash
# Enable BuildKit for faster builds
export DOCKER_BUILDKIT=1

# Parallel build stages
docker build -t myapp .

# In Dockerfile, use cache mounts
# syntax=docker/dockerfile:1
FROM golang:1.18
WORKDIR /app
COPY go.* ./
RUN --mount=type=cache,target=/go/pkg/mod \
    go mod download
COPY . .
RUN --mount=type=cache,target=/go/pkg/mod \
    --mount=type=cache,target=/root/.cache/go-build \
    go build -o server
```

**9. Benchmarking:**

```bash
# Monitor resource usage
docker stats

# Specific container stats
docker stats container_name

# Benchmark with Apache Bench
docker run -d -p 8080:80 --name web nginx
ab -n 10000 -c 100 http://localhost:8080/

# Profile with cAdvisor
docker run \
  --volume=/:/rootfs:ro \
  --volume=/var/run:/var/run:ro \
  --volume=/sys:/sys:ro \
  --volume=/var/lib/docker/:/var/lib/docker:ro \
  --publish=8080:8080 \
  --detach=true \
  --name=cadvisor \
  google/cadvisor:latest
```

**10. Application-Level Optimization:**

```python
# Use production WSGI server, not development server
# ❌ Development
# CMD ["python", "app.py"]

# ✅ Production
# requirements.txt: gunicorn
CMD ["gunicorn", "--workers=4", "--bind=0.0.0.0:8000", "app:app"]
```

**Performance Metrics to Track:**

| Metric | Tool | Target |
|--------|------|--------|
| Image size | `docker images` | < 200MB |
| Build time | `time docker build` | < 5 min |
| Container startup | `docker logs` | < 5s |
| Memory usage | `docker stats` | < 512MB (depends) |
| CPU usage | `docker stats` | < 50% idle |
| Network latency | `ping` | < 1ms local |

**Success Criteria:**
✅ Reduce image sizes by 70%+  
✅ Optimize build times with caching  
✅ Implement resource constraints  
✅ Monitor and benchmark performance  
✅ Achieve sub-second container startup  

---

### 3.3 Docker Swarm Orchestration

**Topics to Master:**
- Swarm architecture
- Creating and managing swarms
- Services and stacks
- Service discovery
- Load balancing
- Rolling updates
- Secrets and configs

**Swarm Architecture:**

```
Manager Nodes (Raft consensus)
     ↓
Task Scheduling
     ↓
Worker Nodes (run containers)
```

**Initialize Swarm:**

```bash
# Initialize swarm on manager node
docker swarm init --advertise-addr <MANAGER-IP>

# Join worker nodes (run on worker machines)
docker swarm join --token <TOKEN> <MANAGER-IP>:2377

# Join additional managers
docker swarm join-token manager

# List nodes
docker node ls

# Inspect node
docker node inspect <NODE-ID>
```

**Promote/Demote Nodes:**

```bash
# Promote worker to manager
docker node promote <NODE-ID>

# Demote manager to worker
docker node demote <NODE-ID>
```

**Deploy Services:**

```bash
# Create service
docker service create \
  --name web \
  --replicas 3 \
  --publish 8080:80 \
  nginx

# List services
docker service ls

# Inspect service
docker service inspect web

# View service logs
docker service logs web

# List tasks (containers) for service
docker service ps web
```

**Scale Services:**

```bash
# Scale up
docker service scale web=5

# Scale down
docker service scale web=2

# Auto-scale with resource constraints
docker service update \
  --limit-cpu 0.5 \
  --limit-memory 512M \
  web
```

**Rolling Updates:**

```bash
# Update service image
docker service update --image nginx:1.21 web

# Configure update behavior
docker service update \
  --update-parallelism 2 \
  --update-delay 10s \
  --update-failure-action rollback \
  web

# Rollback update
docker service rollback web
```

**Deploy Stack (multi-service):**

```yaml
# docker-stack.yml
version: '3.8'

services:
  web:
    image: nginx:alpine
    ports:
      - "8080:80"
    deploy:
      replicas: 3
      update_config:
        parallelism: 1
        delay: 10s
      restart_policy:
        condition: on-failure
    networks:
      - webnet

  visualizer:
    image: dockersamples/visualizer
    ports:
      - "8081:8080"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    deploy:
      placement:
        constraints:
          - node.role == manager
    networks:
      - webnet

networks:
  webnet:
    driver: overlay
```

```bash
# Deploy stack
docker stack deploy -c docker-stack.yml myapp

# List stacks
docker stack ls

# List services in stack
docker stack services myapp

# List tasks in stack
docker stack ps myapp

# Remove stack
docker stack rm myapp
```

**Secrets Management:**

```bash
# Create secret
echo "mysecretpassword" | docker secret create db_password -

# List secrets
docker secret ls

# Use secret in service
docker service create \
  --name db \
  --secret db_password \
  -e POSTGRES_PASSWORD_FILE=/run/secrets/db_password \
  postgres:13

# In stack file
services:
  db:
    image: postgres:13
    secrets:
      - db_password
    environment:
      POSTGRES_PASSWORD_FILE: /run/secrets/db_password

secrets:
  db_password:
    external: true
```

**Configs (non-sensitive data):**

```bash
# Create config
docker config create nginx_config nginx.conf

# Use in service
docker service create \
  --name web \
  --config source=nginx_config,target=/etc/nginx/nginx.conf \
  nginx
```

**Health Checks and Placement:**

```yaml
services:
  app:
    image: myapp
    deploy:
      replicas: 3
      placement:
        constraints:
          - node.role == worker
          - node.labels.region == us-west
      update_config:
        parallelism: 1
        failure_action: rollback
        monitor: 30s
      restart_policy:
        condition: on-failure
        delay: 5s
        max_attempts: 3
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost/health"]
      interval: 30s
      timeout: 10s
      retries: 3
```

**Success Criteria:**
✅ Initialize multi-node swarm cluster  
✅ Deploy and scale services  
✅ Implement rolling updates  
✅ Use secrets and configs  
✅ Deploy multi-service stacks  

---

### 3.4 Advanced Networking

**Topics to Master:**
- Overlay networks
- Ingress load balancing
- Service mesh concepts
- Network encryption
- Custom network drivers
- DNS configuration

**Overlay Networks (Multi-host):**

```bash
# Create overlay network
docker network create \
  --driver overlay \
  --subnet 10.0.0.0/24 \
  --gateway 10.0.0.1 \
  my-overlay

# Create encrypted overlay
docker network create \
  --driver overlay \
  --opt encrypted \
  secure-overlay

# Attach service to overlay network
docker service create \
  --name web \
  --network my-overlay \
  --replicas 3 \
  nginx
```

**Ingress Load Balancing:**

```bash
# Create service with published port (automatic load balancing)
docker service create \
  --name web \
  --publish published=8080,target=80 \
  --replicas 3 \
  nginx

# Requests to ANY swarm node:8080 load-balanced across all replicas
curl http://node1:8080
curl http://node2:8080
curl http://node3:8080
```

**MacVLAN Networks (Direct MAC addresses):**

```bash
# Create macvlan network
docker network create -d macvlan \
  --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 \
  -o parent=eth0 \
  macvlan-net

# Run container with own MAC address
docker run -d \
  --network macvlan-net \
  --ip 192.168.1.100 \
  nginx
```

**Custom DNS Configuration:**

```bash
# Custom DNS servers
docker run -d \
  --dns 8.8.8.8 \
  --dns 8.8.4.4 \
  nginx

# Custom hostname and DNS search
docker run -d \
  --hostname myapp.local \
  --dns-search example.com \
  nginx

# In docker-compose.yml
services:
  app:
    dns:
      - 8.8.8.8
      - 8.8.4.4
    dns_search:
      - example.com
```

**Network Troubleshooting Tools:**

```bash
# Run debugging container with network tools
docker run -it --rm \
  --network my-network \
  nicolaka/netshoot

# Inside container
ping service-name
traceroute service-name
nslookup service-name
tcpdump -i eth0
```

**Success Criteria:**
✅ Configure overlay networks for multi-host communication  
✅ Understand ingress load balancing  
✅ Implement network encryption  
✅ Troubleshoot complex networking scenarios  

---

### 3.5 Advanced Volumes & Storage

**Topics to Master:**
- Volume plugins
- Network storage (NFS, Ceph)
- Storage drivers
- Backup strategies
- Data migration
- Performance tuning

**Volume Drivers:**

```bash
# Use local driver with options
docker volume create \
  --driver local \
  --opt type=nfs \
  --opt o=addr=192.168.1.100,rw \
  --opt device=:/path/to/share \
  nfs-volume

# Run container with NFS volume
docker run -d \
  -v nfs-volume:/data \
  myapp
```

**Volume Plugins (example: REX-Ray for AWS EBS):**

```bash
# Install volume plugin
docker plugin install rexray/ebs

# Create EBS volume
docker volume create \
  --driver rexray/ebs \
  --opt size=50 \
  ebs-volume

# Volume automatically attaches to container's host
docker run -d \
  -v ebs-volume:/data \
  myapp
```

**Automated Backup Strategy:**

```bash
#!/bin/bash
# backup-volumes.sh

BACKUP_DIR="/backups"
DATE=$(date +%Y%m%d_%H%M%S)

# Backup all volumes
for volume in $(docker volume ls -q); do
  docker run --rm \
    -v $volume:/data:ro \
    -v $BACKUP_DIR:/backup \
    alpine \
    tar czf /backup/${volume}_${DATE}.tar.gz -C /data .
done

# Retention: keep last 7 days
find $BACKUP_DIR -name "*.tar.gz" -mtime +7 -delete
```

**Cross-Host Volume Sharing:**

```yaml
# docker-stack.yml with shared volume
version: '3.8'

services:
  app:
    image: myapp
    volumes:
      - shared-data:/data
    deploy:
      replicas: 3
      placement:
        constraints:
          - node.role == worker

volumes:
  shared-data:
    driver: rexray/ebs
    driver_opts:
      size: 50
```

**Success Criteria:**
✅ Implement network-backed volumes  
✅ Configure volume plugins  
✅ Automate backup and restore  
✅ Share volumes across hosts  

---

### Phase 3 Milestone Project

**Goal**: Deploy production-ready application with high availability

**Project**: Scalable Web Application on Docker Swarm

**Requirements:**
- 3-node Swarm cluster (1 manager, 2 workers)
- Multi-tier application (frontend, backend, database)
- Load balancing across multiple replicas
- Rolling updates with zero downtime
- Secrets management
- Persistent storage with backups
- Monitoring and logging
- Security hardening

**Deliverables:**
- Swarm cluster documentation
- Stack deployment files
- Update/rollback procedures
- Backup/restore scripts
- Security audit report
- Performance benchmarks

---

## Phase 4: Production & Specialization (Expert)

**Duration**: 6-8 weeks  
**Goal**: Master Kubernetes, CI/CD integration, and specialized Docker applications

### 4.1 Kubernetes Orchestration

**Topics to Master:**
- Kubernetes architecture
- Pods, Deployments, Services
- ConfigMaps and Secrets
- Persistent Volumes
- Ingress and networking
- StatefulSets
- Helm package manager
- Kubernetes monitoring

**Kubernetes Architecture:**

```
Control Plane (Master)
├── API Server
├── Scheduler
├── Controller Manager
└── etcd (cluster state)

Worker Nodes
├── Kubelet (node agent)
├── Kube-proxy (networking)
└── Container Runtime (Docker/containerd)
```

**Setup Minikube (Local Development):**

```bash
# Install kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# Install Minikube
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

# Start cluster
minikube start --driver=docker

# Verify
kubectl cluster-info
kubectl get nodes
```

**Deploy Application to Kubernetes:**

**1. Create Deployment:**

```yaml
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
      - name: web
        image: nginx:alpine
        ports:
        - containerPort: 80
        resources:
          requests:
            memory: "128Mi"
            cpu: "100m"
          limits:
            memory: "256Mi"
            cpu: "200m"
```

```bash
# Apply deployment
kubectl apply -f deployment.yaml

# Check status
kubectl get deployments
kubectl get pods
kubectl describe pod <pod-name>
```

**2. Create Service:**

```yaml
# service.yaml
apiVersion: v1
kind: Service
metadata:
  name: web-service
spec:
  type: LoadBalancer
  selector:
    app: web
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
```

```bash
# Apply service
kubectl apply -f service.yaml

# Get service
kubectl get services

# Access service (Minikube)
minikube service web-service
```

**3. ConfigMaps and Secrets:**

```yaml
# configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  DATABASE_HOST: postgres-service
  DATABASE_NAME: mydb
---
# secret.yaml
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
type: Opaque
stringData:
  DATABASE_PASSWORD: mysecretpassword
```

```yaml
# deployment using config and secret
spec:
  containers:
  - name: app
    image: myapp
    env:
    - name: DATABASE_HOST
      valueFrom:
        configMapKeyRef:
          name: app-config
          key: DATABASE_HOST
    - name: DATABASE_PASSWORD
      valueFrom:
        secretKeyRef:
          name: app-secret
          key: DATABASE_PASSWORD
```

**4. Persistent Volumes:**

```yaml
# pv.yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mysql-pv
spec:
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /data/mysql
---
# pvc.yaml
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
```

```yaml
# Use PVC in deployment
spec:
  containers:
  - name: mysql
    image: mysql:8.0
    volumeMounts:
    - name: mysql-storage
      mountPath: /var/lib/mysql
  volumes:
  - name: mysql-storage
    persistentVolumeClaim:
      claimName: mysql-pvc
```

**5. Scaling:**

```bash
# Manual scaling
kubectl scale deployment web-app --replicas=5

# Autoscaling
kubectl autoscale deployment web-app \
  --min=2 --max=10 \
  --cpu-percent=80

# Check autoscaler
kubectl get hpa
```

**6. Rolling Updates:**

```bash
# Update image
kubectl set image deployment/web-app web=nginx:1.21

# Check rollout status
kubectl rollout status deployment/web-app

# View rollout history
kubectl rollout history deployment/web-app

# Rollback
kubectl rollout undo deployment/web-app

# Rollback to specific revision
kubectl rollout undo deployment/web-app --to-revision=2
```

**7. Helm Package Manager:**

```bash
# Install Helm
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash

# Add repository
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update

# Install application
helm install my-wordpress bitnami/wordpress

# List releases
helm list

# Upgrade release
helm upgrade my-wordpress bitnami/wordpress --set wordpressUsername=admin

# Uninstall
helm uninstall my-wordpress
```

**Create Custom Helm Chart:**

```bash
# Create chart
helm create myapp

# Chart structure
myapp/
├── Chart.yaml           # Chart metadata
├── values.yaml          # Default values
├── templates/
│   ├── deployment.yaml
│   ├── service.yaml
│   └── ingress.yaml
└── charts/              # Dependencies
```

**values.yaml:**
```yaml
replicaCount: 3

image:
  repository: myapp
  tag: "1.0"
  pullPolicy: IfNotPresent

service:
  type: LoadBalancer
  port: 80

resources:
  limits:
    cpu: 200m
    memory: 256Mi
  requests:
    cpu: 100m
    memory: 128Mi
```

```bash
# Install custom chart
helm install my-release ./myapp

# Install with overrides
helm install my-release ./myapp --set replicaCount=5
```

**8. Monitoring with Prometheus:**

```bash
# Install Prometheus via Helm
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm install prometheus prometheus-community/kube-prometheus-stack

# Access Prometheus
kubectl port-forward svc/prometheus-kube-prometheus-prometheus 9090:9090

# Access Grafana
kubectl port-forward svc/prometheus-grafana 3000:80
```

**Success Criteria:**
✅ Deploy applications to Kubernetes  
✅ Implement scaling and rolling updates  
✅ Use ConfigMaps and Secrets  
✅ Configure persistent storage  
✅ Create and use Helm charts  
✅ Set up monitoring  

---

### 4.2 Docker in CI/CD Pipelines

**Topics to Master:**
- Docker in GitHub Actions
- GitLab CI/CD with Docker
- Jenkins pipelines
- Image building and testing
- Security scanning
- Multi-environment deployments
- Artifact management

**GitHub Actions Workflow:**

```yaml
# .github/workflows/docker.yml
name: Docker CI/CD

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v2
    
    - name: Set up Docker Buildx
      uses: docker/setup-buildx-action@v1
    
    - name: Login to DockerHub
      uses: docker/login-action@v1
      with:
        username: ${{ secrets.DOCKER_USERNAME }}
        password: ${{ secrets.DOCKER_PASSWORD }}
    
    - name: Build and push
      uses: docker/build-push-action@v2
      with:
        context: .
        push: true
        tags: |
          username/myapp:latest
          username/myapp:${{ github.sha }}
        cache-from: type=registry,ref=username/myapp:latest
        cache-to: type=inline
    
    - name: Run tests
      run: |
        docker run --rm username/myapp:${{ github.sha }} npm test
    
    - name: Security scan with Trivy
      uses: aquasecurity/trivy-action@master
      with:
        image-ref: username/myapp:${{ github.sha }}
        format: 'sarif'
        output: 'trivy-results.sarif'
    
    - name: Upload scan results
      uses: github/codeql-action/upload-sarif@v1
      with:
        sarif_file: 'trivy-results.sarif'

  deploy:
    needs: build-and-test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    
    steps:
    - name: Deploy to production
      uses: appleboy/ssh-action@master
      with:
        host: ${{ secrets.PROD_HOST }}
        username: ${{ secrets.PROD_USER }}
        key: ${{ secrets.PROD_SSH_KEY }}
        script: |
          docker pull username/myapp:${{ github.sha }}
          docker stop myapp || true
          docker rm myapp || true
          docker run -d --name myapp -p 80:80 username/myapp:${{ github.sha }}
```

**GitLab CI/CD:**

```yaml
# .gitlab-ci.yml
stages:
  - build
  - test
  - scan
  - deploy

variables:
  DOCKER_DRIVER: overlay2
  DOCKER_TLS_CERTDIR: ""
  IMAGE_TAG: $CI_REGISTRY_IMAGE:$CI_COMMIT_SHORT_SHA

before_script:
  - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY

build:
  stage: build
  image: docker:latest
  services:
    - docker:dind
  script:
    - docker build -t $IMAGE_TAG .
    - docker push $IMAGE_TAG

test:
  stage: test
  image: docker:latest
  services:
    - docker:dind
  script:
    - docker pull $IMAGE_TAG
    - docker run --rm $IMAGE_TAG npm test

security-scan:
  stage: scan
  image: aquasec/trivy:latest
  script:
    - trivy image --exit-code 1 --severity CRITICAL $IMAGE_TAG

deploy-staging:
  stage: deploy
  only:
    - develop
  script:
    - docker pull $IMAGE_TAG
    - docker-compose -f docker-compose.staging.yml up -d

deploy-production:
  stage: deploy
  only:
    - main
  when: manual
  script:
    - docker pull $IMAGE_TAG
    - docker stack deploy -c docker-stack.yml myapp
```

**Jenkins Pipeline:**

```groovy
// Jenkinsfile
pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = "username/myapp"
        DOCKER_TAG = "${env.BUILD_NUMBER}"
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}")
                }
            }
        }
        
        stage('Test') {
            steps {
                script {
                    docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").inside {
                        sh 'npm test'
                    }
                }
            }
        }
        
        stage('Security Scan') {
            steps {
                script {
                    sh """
                        docker run --rm \
                          -v /var/run/docker.sock:/var/run/docker.sock \
                          aquasec/trivy image ${DOCKER_IMAGE}:${DOCKER_TAG}
                    """
                }
            }
        }
        
        stage('Push') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
                        docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").push()
                        docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").push('latest')
                    }
                }
            }
        }
        
        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                script {
                    sh """
                        docker service update \
                          --image ${DOCKER_IMAGE}:${DOCKER_TAG} \
                          myapp_web
                    """
                }
            }
        }
    }
    
    post {
        always {
            cleanWs()
        }
    }
}
```

**Blue-Green Deployment Script:**

```bash
#!/bin/bash
# blue-green-deploy.sh

NEW_VERSION=$1
BLUE_PORT=8080
GREEN_PORT=8081
PROXY_PORT=80

# Deploy new version to green
docker run -d --name green -p $GREEN_PORT:80 myapp:$NEW_VERSION

# Health check
sleep 5
if curl -f http://localhost:$GREEN_PORT/health; then
  echo "Green deployment healthy"
  
  # Switch nginx proxy to green
  docker exec nginx sed -i "s/$BLUE_PORT/$GREEN_PORT/g" /etc/nginx/nginx.conf
  docker exec nginx nginx -s reload
  
  # Stop blue
  docker stop blue
  docker rm blue
  
  # Rename green to blue
  docker rename green blue
  docker port blue | sed "s/$GREEN_PORT/$BLUE_PORT/"
  
  echo "Deployment successful"
else
  echo "Green deployment failed health check, rolling back"
  docker stop green
  docker rm green
  exit 1
fi
```

**Success Criteria:**
✅ Implement complete CI/CD pipeline with Docker  
✅ Automate testing and security scanning  
✅ Deploy to multiple environments  
✅ Implement blue-green deployments  
✅ Integrate with major CI/CD platforms  

---

### 4.3 Microservices Architecture

**Topics to Master:**
- Microservices design principles
- Service decomposition
- Inter-service communication
- Service discovery
- API gateways
- Distributed tracing
- Circuit breakers

**Microservices Stack Example:**

```
┌─────────────────────────────────────────┐
│          API Gateway (Kong/Traefik)     │
└────────────┬────────────────────────────┘
             │
    ┌────────┼────────┐
    │        │        │
┌───▼──┐ ┌──▼───┐ ┌──▼────┐
│User  │ │Order │ │Product│
│Service│ │Service│ │Service│
└───┬──┘ └──┬───┘ └──┬────┘
    │       │        │
    └───────┼────────┘
            │
      ┌─────▼─────┐
      │   Redis   │
      │PostgreSQL │
      └───────────┘
```

**docker-compose.yml for Microservices:**

```yaml
version: '3.8'

services:
  # API Gateway
  kong:
    image: kong:latest
    environment:
      KONG_DATABASE: postgres
      KONG_PG_HOST: postgres
      KONG_PG_PASSWORD: kong
    ports:
      - "8000:8000"
      - "8001:8001"
    depends_on:
      - postgres
    networks:
      - microservices

  # User Service
  user-service:
    build: ./services/user
    environment:
      DATABASE_URL: postgresql://postgres:secret@postgres:5432/users
      REDIS_URL: redis://cache:6379
    depends_on:
      - postgres
      - cache
    networks:
      - microservices

  # Order Service
  order-service:
    build: ./services/order
    environment:
      DATABASE_URL: postgresql://postgres:secret@postgres:5432/orders
      USER_SERVICE_URL: http://user-service:8000
      PRODUCT_SERVICE_URL: http://product-service:8000
    depends_on:
      - postgres
      - user-service
      - product-service
    networks:
      - microservices

  # Product Service
  product-service:
    build: ./services/product
    environment:
      DATABASE_URL: postgresql://postgres:secret@postgres:5432/products
      REDIS_URL: redis://cache:6379
    depends_on:
      - postgres
      - cache
    networks:
      - microservices

  # Shared Database (or separate DBs per service)
  postgres:
    image: postgres:13
    environment:
      POSTGRES_PASSWORD: secret
    volumes:
      - postgres-data:/var/lib/postgresql/data
    networks:
      - microservices

  # Cache
  cache:
    image: redis:6-alpine
    networks:
      - microservices

  # Service Discovery (Consul)
  consul:
    image: consul:latest
    ports:
      - "8500:8500"
    networks:
      - microservices

  # Distributed Tracing (Jaeger)
  jaeger:
    image: jaegertracing/all-in-one:latest
    ports:
      - "16686:16686"
      - "14268:14268"
    networks:
      - microservices

volumes:
  postgres-data:

networks:
  microservices:
    driver: bridge
```

**Service Example (Python Flask):**

```python
# services/user/app.py
from flask import Flask, jsonify, request
import psycopg2
import redis
import os

app = Flask(__name__)

# Database connection
db = psycopg2.connect(os.getenv('DATABASE_URL'))
cache = redis.from_url(os.getenv('REDIS_URL'))

@app.route('/users/<user_id>', methods=['GET'])
def get_user(user_id):
    # Check cache
    cached = cache.get(f'user:{user_id}')
    if cached:
        return jsonify(eval(cached))
    
    # Query database
    cursor = db.cursor()
    cursor.execute('SELECT * FROM users WHERE id = %s', (user_id,))
    user = cursor.fetchone()
    
    if user:
        user_data = {'id': user[0], 'name': user[1], 'email': user[2]}
        cache.setex(f'user:{user_id}', 300, str(user_data))
        return jsonify(user_data)
    
    return jsonify({'error': 'User not found'}), 404

@app.route('/health', methods=['GET'])
def health():
    return jsonify({'status': 'healthy'})

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8000)
```

**Dockerfile:**

```dockerfile
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8000

HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD curl -f http://localhost:8000/health || exit 1

CMD ["gunicorn", "--bind", "0.0.0.0:8000", "--workers", "4", "app:app"]
```

**Success Criteria:**
✅ Decompose monolith into microservices  
✅ Implement service-to-service communication  
✅ Set up service discovery  
✅ Implement distributed tracing  
✅ Deploy multi-service architecture  

---

### 4.4 Docker for Data Science & ML

**Topics to Master:**
- Jupyter environments
- GPU support (NVIDIA Docker)
- ML workflow containerization
- Model serving
- Distributed training
- Reproducible research

**Jupyter Notebook with Docker:**

```dockerfile
# Dockerfile.jupyter
FROM jupyter/scipy-notebook:latest

USER root

# Install additional packages
RUN apt-get update && apt-get install -y \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

USER $NB_UID

# Install Python packages
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Set working directory
WORKDIR /home/jovyan/work

EXPOSE 8888

CMD ["start-notebook.sh", "--NotebookApp.token=''", "--NotebookApp.password=''"]
```

```bash
# Run Jupyter with persistent storage
docker run -d \
  -p 8888:8888 \
  -v $(pwd)/notebooks:/home/jovyan/work \
  --name jupyter \
  my-jupyter
```

**GPU-Enabled ML Container:**

```dockerfile
# Dockerfile.ml-gpu
FROM nvidia/cuda:11.8.0-cudnn8-runtime-ubuntu22.04

RUN apt-get update && apt-get install -y \
    python3-pip \
    python3-dev

COPY requirements.txt .
RUN pip3 install --no-cache-dir -r requirements.txt

# requirements.txt
# tensorflow-gpu
# torch
# torchvision
# transformers

WORKDIR /workspace

CMD ["python3"]
```

```bash
# Run with GPU support
docker run --gpus all -it --rm \
  -v $(pwd):/workspace \
  ml-gpu python3 train.py
```

**ML Model Serving:**

```dockerfile
# Dockerfile.serve
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir flask gunicorn tensorflow

COPY model/ ./model/
COPY serve.py .

EXPOSE 5000

CMD ["gunicorn", "--bind", "0.0.0.0:5000", "--workers", "4", "serve:app"]
```

```python
# serve.py
from flask import Flask, request, jsonify
import tensorflow as tf
import numpy as np

app = Flask(__name__)
model = tf.keras.models.load_model('model/')

@app.route('/predict', methods=['POST'])
def predict():
    data = request.json
    input_data = np.array(data['input'])
    prediction = model.predict(input_data)
    return jsonify({'prediction': prediction.tolist()})

@app.route('/health', methods=['GET'])
def health():
    return jsonify({'status': 'healthy'})
```

**MLOps Pipeline with Docker Compose:**

```yaml
version: '3.8'

services:
  # Training Job
  train:
    build:
      context: .
      dockerfile: Dockerfile.train
    volumes:
      - ./data:/data
      - ./models:/models
    environment:
      - MLFLOW_TRACKING_URI=http://mlflow:5000
    depends_on:
      - mlflow

  # MLflow Tracking Server
  mlflow:
    image: python:3.9
    command: >
      bash -c "pip install mlflow psycopg2-binary &&
               mlflow server --backend-store-uri postgresql://mlflow:mlflow@postgres/mlflow
               --default-artifact-root /mlflow --host 0.0.0.0"
    ports:
      - "5000:5000"
    volumes:
      - mlflow-data:/mlflow
    depends_on:
      - postgres

  # Model Serving
  serve:
    build:
      context: .
      dockerfile: Dockerfile.serve
    ports:
      - "8080:5000"
    volumes:
      - ./models:/app/model:ro

  # Database
  postgres:
    image: postgres:13
    environment:
      POSTGRES_USER: mlflow
      POSTGRES_PASSWORD: mlflow
      POSTGRES_DB: mlflow
    volumes:
      - postgres-data:/var/lib/postgresql/data

volumes:
  mlflow-data:
  postgres-data:
```

**Success Criteria:**
✅ Set up containerized Jupyter environment  
✅ Run GPU-accelerated training  
✅ Containerize ML models for serving  
✅ Implement MLOps pipeline  

---

### 4.5 Troubleshooting & Debugging

**Topics to Master:**
- Container lifecycle issues
- Networking problems
- Storage debugging
- Performance profiling
- Log analysis
- Resource constraints

**Essential Debugging Commands:**

```bash
# Container inspection
docker inspect <container>
docker logs <container>
docker logs -f --tail 100 <container>
docker stats <container>
docker top <container>

# Execute commands in container
docker exec -it <container> bash
docker exec <container> ps aux
docker exec <container> cat /var/log/app.log

# Network debugging
docker network inspect <network>
docker exec <container> ping other-container
docker exec <container> netstat -tulpn
docker exec <container> curl http://service:port

# Volume debugging
docker volume inspect <volume>
docker run --rm -v <volume>:/data alpine ls -la /data

# System-wide issues
docker system df              # Disk usage
docker system prune -a        # Clean up
docker events                 # Real-time events

# Daemon logs
journalctl -u docker.service
tail -f /var/log/docker.log
```

**Common Issues and Solutions:**

**1. Container Exits Immediately:**

```bash
# Check logs
docker logs <container>

# Run with interactive mode
docker run -it <image> /bin/bash

# Check entrypoint
docker inspect <image> | grep -i entrypoint
```

**2. Port Already in Use:**

```bash
# Find process using port
sudo lsof -i :8080
sudo netstat -tulpn | grep 8080

# Kill process or use different port
docker run -p 8081:80 nginx
```

**3. Out of Disk Space:**

```bash
# Check disk usage
docker system df

# Remove unused data
docker system prune -a --volumes

# Remove specific items
docker container prune
docker image prune -a
docker volume prune
```

**4. Cannot Connect to Service:**

```bash
# Check if containers are on same network
docker network inspect bridge

# Test connectivity
docker exec container1 ping container2
docker exec container1 nslookup container2

# Check if port is exposed
docker ps
docker port <container>
```

**5. Permission Denied:**

```bash
# Run as different user
docker run --user $(id -u):$(id -g) <image>

# Fix volume permissions
docker run -v /host/path:/container/path:z <image>

# Add user to docker group
sudo usermod -aG docker $USER
```

**Performance Profiling:**

```bash
# CPU and memory usage
docker stats

# Detailed container metrics
docker run -d \
  -v /:/rootfs:ro \
  -v /var/run:/var/run:ro \
  -v /sys:/sys:ro \
  -v /var/lib/docker/:/var/lib/docker:ro \
  -p 8080:8080 \
  google/cadvisor:latest

# Profile application
docker exec <container> top
docker exec <container> htop
```

**Success Criteria:**
✅ Diagnose container startup failures  
✅ Debug networking connectivity issues  
✅ Resolve storage and permission problems  
✅ Profile and optimize performance  
✅ Analyze logs effectively  

---

### Phase 4 Capstone Project

**Goal**: Build and deploy a complete production system

**Project**: Cloud-Native SaaS Application

**Requirements:**

**Architecture:**
- Microservices (5+ services)
- Kubernetes deployment (GKE/EKS/AKS)
- API Gateway
- Service mesh (Istio/Linkerd)
- Database per service
- Event-driven communication (RabbitMQ/Kafka)

**DevOps:**
- Full CI/CD pipeline
- Automated testing (unit, integration, e2e)
- Security scanning
- Blue-green deployments
- Automated rollbacks

**Monitoring & Observability:**
- Centralized logging (ELK/Loki)
- Metrics (Prometheus/Grafana)
- Distributed tracing (Jaeger)
- Alerting

**Security:**
- Secrets management (Vault)
- Network policies
- Pod security policies
- Image signing

**Documentation:**
- Architecture diagrams
- API documentation
- Runbooks
- Disaster recovery plan

**Deliverables:**
- Source code repository
- Kubernetes manifests/Helm charts
- CI/CD configuration
- Monitoring dashboards
- Complete documentation
- Presentation/demo

---

## Hands-On Projects

### Beginner Projects

**1. Personal Blog**
- Static site with Nginx
- Single Dockerfile
- Port mapping

**2. Simple API**
- Flask/Express REST API
- PostgreSQL database
- docker-compose.yml

**3. WordPress Site**
- WordPress + MySQL
- Persistent volumes
- Environment variables

### Intermediate Projects

**4. Full-Stack Todo App**
- React frontend
- Node.js backend
- MongoDB database
- Redis cache
- Nginx reverse proxy

**5. CI/CD Pipeline**
- GitHub Actions
- Automated testing
- Docker Hub registry
- Deployment automation

**6. Swarm Cluster**
- 3-node cluster
- Load-balanced services
- Rolling updates
- Secrets management

### Advanced Projects

**7. E-commerce Platform**
- 5+ microservices
- Event-driven architecture
- Service discovery
- Distributed tracing

**8. Kubernetes Application**
- Multi-tier deployment
- Helm charts
- Horizontal pod autoscaling
- Ingress controller

**9. ML Pipeline**
- Jupyter notebooks
- Training pipeline
- Model serving
- MLflow tracking

### Expert Projects

**10. Cloud-Native SaaS**
- Complete production system
- Multi-region deployment
- Auto-scaling
- Disaster recovery
- Full observability

---

## Learning Resources

### Official Documentation
- [Docker Documentation](https://docs.docker.com/)
- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [Docker Hub](https://hub.docker.com/)

### Online Courses
- Docker Mastery (Udemy)
- Kubernetes for Developers (Pluralsight)
- Docker and Kubernetes: The Complete Guide (Udemy)

### Books
- "Docker Deep Dive" by Nigel Poulton
- "Kubernetes in Action" by Marko Lukša
- "The DevOps Handbook" by Gene Kim

### Practice Platforms
- [Play with Docker](https://labs.play-with-docker.com/)
- [Katacoda Docker Scenarios](https://www.katacoda.com/courses/docker)
- [Kubernetes by Example](https://kubernetesbyexample.com/)

### Communities
- Docker Community Forums
- Kubernetes Slack
- Reddit: r/docker, r/kubernetes
- Stack Overflow

---

## Certification Path

### Docker Certifications

**Docker Certified Associate (DCA)**
- **Topics**: Images, containers, networks, volumes, Swarm, security
- **Duration**: 13 questions, 90 minutes
- **Recommended Experience**: 6-12 months

### Kubernetes Certifications

**Certified Kubernetes Application Developer (CKAD)**
- **Topics**: Core concepts, configuration, multi-container pods, observability
- **Duration**: 2 hours, hands-on exam
- **Recommended Experience**: 6 months

**Certified Kubernetes Administrator (CKA)**
- **Topics**: Cluster architecture, scheduling, networking, storage, troubleshooting
- **Duration**: 2 hours, hands-on exam
- **Recommended Experience**: 1+ years

**Certified Kubernetes Security Specialist (CKS)**
- **Topics**: Cluster setup, hardening, system hardening, monitoring, supply chain security
- **Prerequisites**: CKA certification
- **Duration**: 2 hours

---

## Next Steps

### After Completing This Path

1. **Contribute to Open Source**
   - Docker projects on GitHub
   - Kubernetes ecosystem tools
   - Create public Docker images

2. **Build Portfolio**
   - GitHub repositories
   - Docker Hub profile
   - Blog about learnings
   - Video tutorials

3. **Stay Current**
   - Follow Docker/Kubernetes releases
   - Attend conferences (DockerCon, KubeCon)
   - Join local meetups
   - Read industry blogs

4. **Specialize**
   - Cloud platforms (AWS ECS, GCP GKE, Azure AKS)
   - Service mesh (Istio, Linkerd)
   - Serverless containers (AWS Fargate)
   - Edge computing

5. **Teach Others**
   - Mentor juniors
   - Write tutorials
   - Speak at meetups
   - Create courses

---

## Conclusion

Docker mastery is a journey of continuous learning. This comprehensive path provides structured progression from beginner to expert, but the key to success is **hands-on practice**. Build real projects, break things, debug, and iterate.

**Key Takeaways:**
- Start with fundamentals before advancing
- Practice consistently with real projects
- Embrace troubleshooting as learning opportunities
- Join communities and share knowledge
- Stay updated with ecosystem evolution

**Remember**: The best way to learn Docker is to use Docker. Start today!

---

*Document Version: 1.0*  
*Last Updated: 2026-02-09*  
*Source: Docker Book - Complete Learning Guide*
