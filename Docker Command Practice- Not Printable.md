# Docker Commands & Practice Guide

> **Quick Reference for Essential Docker Commands and Hands-On Exercises**

---

## 📋 Table of Contents

- [Installation & Setup](#installation--setup)
- [Container Management](#container-management)
- [Image Management](#image-management)
- [Dockerfile Commands](#dockerfile-commands)
- [Networking](#networking)
- [Volumes & Storage](#volumes--storage)
- [Docker Compose](#docker-compose)
- [Docker Swarm](#docker-swarm)
- [System Management](#system-management)
- [Debugging & Troubleshooting](#debugging--troubleshooting)
- [Practice Exercises](#practice-exercises)

---

## Installation & Setup

```bash
# Verify Docker installation
docker --version
docker info

# Test installation
docker run hello-world

# Add user to docker group (Linux)
sudo usermod -aG docker $USER
newgrp docker

# Check daemon status
systemctl status docker
sudo systemctl start docker
sudo systemctl enable docker
```

---

## Container Management

### Running Containers

```bash
# Run container (foreground)
docker run nginx

# Run in detached mode
docker run -d nginx

# Run with name
docker run -d --name my-container nginx

# Run interactively
docker run -it ubuntu bash

# Run with port mapping
docker run -d -p 8080:80 nginx
docker run -d -p 127.0.0.1:8080:80 nginx  # Bind to localhost

# Run with environment variables
docker run -d -e MYSQL_ROOT_PASSWORD=secret mysql

# Run with restart policy
docker run -d --restart always nginx
docker run -d --restart on-failure:5 nginx

# Run with resource limits
docker run -d --memory="512m" --cpus="1.0" nginx

# Run with volume
docker run -d -v my-volume:/data nginx

# Run with bind mount
docker run -d -v $(pwd):/app nginx

# Remove container after exit
docker run --rm ubuntu echo "Hello"
```

### Listing Containers

```bash
# List running containers
docker ps

# List all containers
docker ps -a

# List container IDs only
docker ps -q

# Filter containers
docker ps --filter "status=exited"
docker ps --filter "name=web"

# Format output
docker ps --format "table {{.ID}}\t{{.Names}}\t{{.Status}}"
```

### Container Lifecycle

```bash
# Start container
docker start <container>

# Stop container
docker stop <container>

# Restart container
docker restart <container>

# Pause/unpause container
docker pause <container>
docker unpause <container>

# Kill container (force stop)
docker kill <container>

# Remove container
docker rm <container>

# Remove running container (force)
docker rm -f <container>

# Remove all stopped containers
docker container prune
```

### Container Inspection

```bash
# View container logs
docker logs <container>
docker logs -f <container>              # Follow logs
docker logs --tail 100 <container>      # Last 100 lines
docker logs --since 1h <container>      # Last hour

# Inspect container
docker inspect <container>
docker inspect <container> | jq '.[0].NetworkSettings.IPAddress'

# View container processes
docker top <container>

# View resource usage
docker stats <container>
docker stats --no-stream              # Single snapshot

# View port mappings
docker port <container>

# View container changes
docker diff <container>
```

### Executing Commands

```bash
# Execute command in running container
docker exec <container> ls /app

# Interactive shell
docker exec -it <container> bash
docker exec -it <container> sh

# Execute as specific user
docker exec -u root -it <container> bash

# Execute with environment variables
docker exec -e VAR=value <container> printenv
```

### Copying Files

```bash
# Copy from container to host
docker cp <container>:/path/file.txt ./file.txt

# Copy from host to container
docker cp ./file.txt <container>:/path/file.txt
```

---

## Image Management

### Pulling & Listing Images

```bash
# Pull image from registry
docker pull nginx
docker pull nginx:1.21
docker pull nginx:alpine

# List images
docker images
docker image ls

# List image IDs only
docker images -q

# Show image layers
docker history nginx
```

### Building Images

```bash
# Build from Dockerfile
docker build -t myapp:1.0 .

# Build with tag
docker build -t myapp:latest -t myapp:1.0 .

# Build with build arguments
docker build --build-arg VERSION=1.0 -t myapp .

# Build with no cache
docker build --no-cache -t myapp .

# Build from specific Dockerfile
docker build -f Dockerfile.prod -t myapp .

# Enable BuildKit
export DOCKER_BUILDKIT=1
docker build -t myapp .
```

### Tagging Images

```bash
# Tag image
docker tag myapp:latest myapp:1.0
docker tag myapp:latest username/myapp:latest

# Tag for private registry
docker tag myapp localhost:5000/myapp:1.0
```

### Pushing Images

```bash
# Login to Docker Hub
docker login

# Push to Docker Hub
docker push username/myapp:1.0

# Push to private registry
docker push localhost:5000/myapp:1.0

# Logout
docker logout
```

### Removing Images

```bash
# Remove image
docker rmi nginx

# Remove image by ID
docker rmi abc123

# Force remove
docker rmi -f nginx

# Remove dangling images
docker image prune

# Remove all unused images
docker image prune -a

# Remove images with filter
docker image prune -a --filter "until=24h"
```

### Saving & Loading Images

```bash
# Save image to tar file
docker save -o myapp.tar myapp:latest

# Load image from tar file
docker load -i myapp.tar

# Export container to tar
docker export <container> -o container.tar

# Import from tar as image
docker import container.tar myapp:latest
```

---

## Dockerfile Commands

### Basic Structure

```dockerfile
# Base image
FROM ubuntu:22.04

# Metadata
LABEL maintainer="you@example.com"
LABEL version="1.0"

# Environment variables
ENV APP_HOME=/app
ENV NODE_ENV=production

# Working directory
WORKDIR /app

# Copy files
COPY package.json .
COPY . .

# Add files (supports URLs and auto-extraction)
ADD https://example.com/file.tar.gz /tmp/
ADD archive.tar.gz /data/

# Run commands during build
RUN apt-get update && apt-get install -y curl
RUN npm install

# Expose ports (documentation only)
EXPOSE 8080

# Create volume mount point
VOLUME /data

# Default command
CMD ["npm", "start"]

# Entrypoint
ENTRYPOINT ["python", "app.py"]

# Build arguments
ARG VERSION=1.0
RUN echo "Building version $VERSION"

# User
USER appuser

# Health check
HEALTHCHECK --interval=30s --timeout=3s \
  CMD curl -f http://localhost/ || exit 1
```

### Multi-Stage Build

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
CMD ["node", "dist/server.js"]
```

### .dockerignore

```
node_modules
npm-debug.log
.git
.env
*.md
.DS_Store
dist
build
coverage
.dockerignore
Dockerfile
```

---

## Networking

### Network Management

```bash
# List networks
docker network ls

# Create network
docker network create my-network

# Create with driver and subnet
docker network create --driver bridge --subnet 172.20.0.0/16 my-net

# Create overlay network (Swarm)
docker network create --driver overlay my-overlay

# Inspect network
docker network inspect my-network

# Remove network
docker network rm my-network

# Remove unused networks
docker network prune
```

### Connecting Containers

```bash
# Run container on network
docker run -d --name web --network my-network nginx

# Connect running container to network
docker network connect my-network <container>

# Disconnect from network
docker network disconnect my-network <container>

# Run with multiple networks
docker run -d --network net1 nginx
docker network connect net2 <container>
```

### Network Drivers

```bash
# Bridge (default)
docker network create --driver bridge my-bridge

# Host (use host network stack)
docker run -d --network host nginx

# None (no networking)
docker run -d --network none nginx

# Overlay (multi-host)
docker network create --driver overlay --attachable my-overlay
```

### DNS & Connectivity

```bash
# Custom DNS
docker run -d --dns 8.8.8.8 --dns 8.8.4.4 nginx

# Custom hostname
docker run -d --hostname myapp.local nginx

# Add host entry
docker run -d --add-host mydb:192.168.1.100 nginx

# Test connectivity
docker exec <container> ping other-container
docker exec <container> nslookup other-container
```

---

## Volumes & Storage

### Volume Management

```bash
# Create volume
docker volume create my-volume

# List volumes
docker volume ls

# Inspect volume
docker volume inspect my-volume

# Remove volume
docker volume rm my-volume

# Remove unused volumes
docker volume prune
```

### Using Volumes

```bash
# Named volume
docker run -d -v my-volume:/data nginx

# Bind mount
docker run -d -v $(pwd):/app nginx
docker run -d -v /host/path:/container/path nginx

# Read-only mount
docker run -d -v my-volume:/data:ro nginx

# tmpfs mount (RAM storage)
docker run -d --tmpfs /tmp:rw,size=100m nginx
```

### Backup & Restore

```bash
# Backup volume
docker run --rm \
  -v my-volume:/data \
  -v $(pwd):/backup \
  alpine \
  tar czf /backup/backup.tar.gz -C /data .

# Restore volume
docker run --rm \
  -v my-volume:/data \
  -v $(pwd):/backup \
  alpine \
  tar xzf /backup/backup.tar.gz -C /data
```

---

## Docker Compose

### Basic Commands

```bash
# Start services
docker-compose up
docker-compose up -d              # Detached
docker-compose up --build         # Rebuild images

# Stop services
docker-compose down               # Stop and remove
docker-compose down -v            # Also remove volumes
docker-compose stop               # Stop without removing

# View status
docker-compose ps
docker-compose logs
docker-compose logs -f service    # Follow logs

# Execute commands
docker-compose exec service bash
docker-compose run service command

# Scale services
docker-compose up -d --scale web=3

# Rebuild
docker-compose build
docker-compose build --no-cache
```

### Compose File Example

```yaml
version: '3.8'

services:
  web:
    build: ./web
    ports:
      - "8080:80"
    environment:
      - NODE_ENV=production
    volumes:
      - ./web:/app
    depends_on:
      - db
    networks:
      - app-net

  db:
    image: postgres:13
    environment:
      POSTGRES_PASSWORD: secret
    volumes:
      - db-data:/var/lib/postgresql/data
    networks:
      - app-net

volumes:
  db-data:

networks:
  app-net:
```

### Useful Compose Commands

```bash
# Validate compose file
docker-compose config

# List containers
docker-compose ps

# View logs
docker-compose logs
docker-compose logs -f --tail=100

# Restart service
docker-compose restart web

# Pull latest images
docker-compose pull

# Remove stopped containers
docker-compose rm
```

---

## Docker Swarm

### Initialize Swarm

```bash
# Initialize swarm
docker swarm init

# Initialize with specific IP
docker swarm init --advertise-addr 192.168.1.100

# Get join token for workers
docker swarm join-token worker

# Get join token for managers
docker swarm join-token manager

# Leave swarm
docker swarm leave
docker swarm leave --force  # Manager node
```

### Node Management

```bash
# List nodes
docker node ls

# Inspect node
docker node inspect <node-id>

# Promote worker to manager
docker node promote <node-id>

# Demote manager to worker
docker node demote <node-id>

# Remove node
docker node rm <node-id>

# Update node availability
docker node update --availability drain <node-id>
```

### Service Management

```bash
# Create service
docker service create --name web --replicas 3 -p 8080:80 nginx

# List services
docker service ls

# Inspect service
docker service inspect web

# View service logs
docker service logs web

# List service tasks
docker service ps web

# Scale service
docker service scale web=5

# Update service
docker service update --image nginx:1.21 web

# Remove service
docker service rm web
```

### Stack Management

```bash
# Deploy stack
docker stack deploy -c docker-stack.yml mystack

# List stacks
docker stack ls

# List stack services
docker stack services mystack

# List stack tasks
docker stack ps mystack

# Remove stack
docker stack rm mystack
```

### Secrets & Configs

```bash
# Create secret
echo "password123" | docker secret create db_pass -

# List secrets
docker secret ls

# Inspect secret
docker secret inspect db_pass

# Remove secret
docker secret rm db_pass

# Use in service
docker service create --secret db_pass myapp

# Create config
docker config create nginx_conf nginx.conf

# List configs
docker config ls

# Use in service
docker service create --config nginx_conf nginx
```

---

## System Management

### System Information

```bash
# System info
docker info

# Version
docker version

# Disk usage
docker system df
docker system df -v  # Verbose

# View events
docker events
docker events --since 1h
```

### Cleanup

```bash
# Remove stopped containers
docker container prune

# Remove unused images
docker image prune
docker image prune -a

# Remove unused volumes
docker volume prune

# Remove unused networks
docker network prune

# Remove everything
docker system prune
docker system prune -a --volumes
```

### Resource Monitoring

```bash
# View container stats
docker stats

# View specific container
docker stats <container>

# Single snapshot
docker stats --no-stream

# Format output
docker stats --format "table {{.Container}}\t{{.CPUPerc}}\t{{.MemUsage}}"
```

---

## Debugging & Troubleshooting

### Container Debugging

```bash
# View logs
docker logs <container>
docker logs -f <container>
docker logs --tail 100 <container>

# Inspect container
docker inspect <container>

# Check processes
docker top <container>

# View changes
docker diff <container>

# Enter container
docker exec -it <container> bash

# Run debugging container
docker run -it --rm alpine sh
docker run -it --rm ubuntu bash
```

### Network Debugging

```bash
# Inspect network
docker network inspect bridge

# Check container IP
docker inspect <container> | grep IPAddress

# Test connectivity
docker exec <container> ping google.com
docker exec <container> curl http://api:8080

# Network debugging tools
docker run -it --network my-net nicolaka/netshoot
```

### Common Issues

```bash
# Permission denied (Linux)
sudo usermod -aG docker $USER
newgrp docker

# Port already in use
docker ps | grep 8080
lsof -i :8080

# Out of disk space
docker system df
docker system prune -a --volumes

# Container exits immediately
docker logs <container>
docker run -it <image> sh

# Cannot connect to daemon
systemctl status docker
sudo systemctl start docker
```

---

## Practice Exercises

### Exercise 1: Basic Container Operations

```bash
# 1. Run nginx container
docker run -d -p 8080:80 --name web nginx

# 2. Check if running
docker ps

# 3. View logs
docker logs web

# 4. Access in browser: http://localhost:8080

# 5. Stop and remove
docker stop web
docker rm web
```

### Exercise 2: Interactive Container

```bash
# 1. Start Ubuntu container
docker run -it --name myubuntu ubuntu bash

# 2. Inside container, install curl
apt update
apt install -y curl

# 3. Test curl
curl google.com

# 4. Exit container
exit

# 5. Start and attach again
docker start myubuntu
docker attach myubuntu

# 6. Clean up
docker rm -f myubuntu
```

### Exercise 3: Build Custom Image

```bash
# 1. Create directory
mkdir myapp && cd myapp

# 2. Create simple web server (app.py)
cat > app.py << 'PYTHON'
from http.server import HTTPServer, SimpleHTTPRequestHandler

class Handler(SimpleHTTPRequestHandler):
    def do_GET(self):
        self.send_response(200)
        self.send_header('Content-type', 'text/html')
        self.end_headers()
        self.wfile.write(b'Hello from Docker!')

HTTPServer(('', 8000), Handler).serve_forever()
PYTHON

# 3. Create Dockerfile
cat > Dockerfile << 'DOCKER'
FROM python:3.9-slim
WORKDIR /app
COPY app.py .
EXPOSE 8000
CMD ["python", "app.py"]
DOCKER

# 4. Build image
docker build -t myapp:1.0 .

# 5. Run container
docker run -d -p 8000:8000 --name app myapp:1.0

# 6. Test: http://localhost:8000

# 7. Clean up
docker stop app && docker rm app
```

### Exercise 4: Multi-Container App

```bash
# 1. Create docker-compose.yml
cat > docker-compose.yml << 'YAML'
version: '3.8'
services:
  web:
    image: nginx:alpine
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html
  
  db:
    image: postgres:13
    environment:
      POSTGRES_PASSWORD: secret
    volumes:
      - db-data:/var/lib/postgresql/data

volumes:
  db-data:
YAML

# 2. Create HTML file
mkdir html
echo "<h1>Hello Docker Compose!</h1>" > html/index.html

# 3. Start services
docker-compose up -d

# 4. Check status
docker-compose ps

# 5. View logs
docker-compose logs

# 6. Test: http://localhost:8080

# 7. Stop and remove
docker-compose down -v
```

### Exercise 5: Networking Practice

```bash
# 1. Create custom network
docker network create app-network

# 2. Run database
docker run -d --name db --network app-network \
  -e POSTGRES_PASSWORD=secret postgres:13

# 3. Run app that connects to db
docker run -it --rm --network app-network postgres:13 \
  psql -h db -U postgres

# 4. Test DNS resolution
docker run -it --rm --network app-network alpine \
  ping db

# 5. Clean up
docker stop db && docker rm db
docker network rm app-network
```

### Exercise 6: Volume Practice

```bash
# 1. Create volume
docker volume create mydata

# 2. Run container with volume
docker run -d --name writer -v mydata:/data alpine \
  sh -c 'echo "Hello" > /data/file.txt && sleep 3600'

# 3. Read from another container
docker run --rm -v mydata:/data alpine cat /data/file.txt

# 4. Backup volume
docker run --rm -v mydata:/data -v $(pwd):/backup alpine \
  tar czf /backup/mydata.tar.gz -C /data .

# 5. Clean up
docker stop writer && docker rm writer
docker volume rm mydata
```

### Exercise 7: Security Practice

```bash
# 1. Create Dockerfile with non-root user
cat > Dockerfile << 'DOCKER'
FROM alpine:latest
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser
WORKDIR /home/appuser
CMD ["sh"]
DOCKER

# 2. Build image
docker build -t secure-app .

# 3. Run and verify user
docker run --rm secure-app whoami  # Should show 'appuser'

# 4. Run with read-only filesystem
docker run --rm --read-only secure-app touch /tmp/test  # Should fail

# 5. Run with resource limits
docker run -d --memory="256m" --cpus="0.5" --name limited nginx

# 6. Check stats
docker stats limited --no-stream

# 7. Clean up
docker stop limited && docker rm limited
```

### Exercise 8: Health Checks

```bash
# 1. Create Dockerfile with health check
cat > Dockerfile << 'DOCKER'
FROM nginx:alpine
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD wget --quiet --tries=1 --spider http://localhost/ || exit 1
DOCKER

# 2. Build image
docker build -t nginx-health .

# 3. Run container
docker run -d --name web nginx-health

# 4. Check health status
docker ps
docker inspect web | grep -A 5 Health

# 5. Clean up
docker stop web && docker rm web
```

---

## Quick Command Reference

### Most Used Commands

```bash
# Container operations
docker run -d -p 8080:80 --name web nginx
docker ps
docker logs web
docker exec -it web bash
docker stop web
docker rm web

# Image operations
docker images
docker build -t myapp .
docker pull nginx
docker push myapp
docker rmi myapp

# Compose operations
docker-compose up -d
docker-compose ps
docker-compose logs -f
docker-compose down

# Cleanup
docker system prune -a
docker volume prune
docker network prune

# Info & debugging
docker info
docker inspect <container>
docker logs <container>
docker stats
docker events
```

### Useful Aliases

```bash
# Add to ~/.bashrc or ~/.zshrc

alias dps='docker ps'
alias dpsa='docker ps -a'
alias di='docker images'
alias dex='docker exec -it'
alias dlogs='docker logs -f'
alias dstop='docker stop $(docker ps -q)'
alias drm='docker rm $(docker ps -aq)'
alias drmi='docker rmi $(docker images -q)'
alias dprune='docker system prune -a'
alias dc='docker-compose'
alias dcu='docker-compose up -d'
alias dcd='docker-compose down'
alias dcl='docker-compose logs -f'
```

---

## Tips & Best Practices

### Performance
- Use Alpine-based images for smaller size
- Leverage build cache with proper layer ordering
- Use multi-stage builds
- Clean up unused resources regularly

### Security
- Never run containers as root (use USER instruction)
- Scan images for vulnerabilities
- Use secrets for sensitive data
- Limit container resources
- Use read-only filesystems where possible

### Organization
- Tag images with semantic versioning
- Use meaningful container and volume names
- Document port mappings and environment variables
- Use docker-compose for multi-container apps

### Development
- Use bind mounts for live code reloading
- Use docker-compose for local development
- Keep Dockerfiles in version control
- Use .dockerignore to exclude unnecessary files

---

**Quick Start Checklist:**

- [ ] Install Docker and verify with `docker run hello-world`
- [ ] Run your first web server with nginx
- [ ] Build a custom image from Dockerfile
- [ ] Create multi-container app with docker-compose
- [ ] Practice networking between containers
- [ ] Use volumes for data persistence
- [ ] Implement security best practices
- [ ] Master debugging with logs and exec

---

*For comprehensive learning, refer to the full Docker Learning Path document.*
