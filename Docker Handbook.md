<p style="text-align:center; font-size: 30px; font-weight: bold">Docker Handbook Guide</p>
 
### A Step-By-Step Journey Through Docker

A Comprehensive Overview of Containerization, Management, and Orchestration.

## Introduction to Docker
### What is Containerization?

- A way to package code and all its dependencies.
- Virtualizes the operating system, not the hardware.
- High efficiency and super-fast performance.
- Works consistently across different environments.

## Why Use Docker?
### Beyond Virtual Machines

- **Isolation**: Each container is its own world.
- **Portability**: "Build once, run anywhere."
- **Efficiency**: Uses fewer resources than Virtual Machines.
- **Lightweight**: Start and stop in seconds.

## Getting Started
### Installing Docker

- **macOS**: The simplest installation (Docker Desktop).
- **Windows**: Use Docker Desktop with WSL2 support.
- **Linux**: Installation via command line (the native Docker platform).
- All platforms run Docker flawlessly.

## Your First Run
### The "Hello World" Container

Run this in your terminal:
```bash
docker run hello-world
```
**What happens?**
1. Docker looks for the image locally.
2. If not found, it pulls from Docker Hub.
3. It creates and starts a container.
4. The container prints a message and exits.

## Key Concept: Images
### The Blueprint

- A read-only template for creating containers.
- Contains the OS, libraries, and application code.
- Stored in a Docker Registry (like Docker Hub).
- Think of it as a "frozen" version of your environment.

## Key Concept: Containers
### The Living Process

- An active instance of a Docker Image.
- Completely isolated from other containers and the host.
- Can be started, stopped, moved, or deleted.
- Runs only the necessary processes for the application.

## Docker Architecture
### Under the Hood

- **Docker Daemon**: The background worker (dockerd).
- **Docker Client**: The command line tool you use (docker).
- **REST API**: How the client talks to the daemon.
- **Registry**: Where images are stored (e.g., Docker Hub).

## Running Containers
### Basic Syntax

The modern way to run containers:
```bash
docker container run <image-name>
```
Example:
```bash
docker container run alpine
```
This command does both: **create** and **start**.

## Publishing Ports
### Connecting to the Web

To reach a container's web server from your host:
```bash
docker container run --publish 8080:80 nginx
```
- **8080**: Port on your computer (Host).
- **80**: Port inside the container.
- Map host ports to container ports easily.

## Detached Mode
### Running in the Background

Use the `-d` flag to run without tying up your terminal:
```bash
docker container run -d nginx
```
- Returns the full Container ID.
- The process stays alive in the background.
- Perfect for web servers or databases.

## Listing Containers
### Seeing What's Running

- **Currently running**: `docker container ls`
- **Everything (including stopped)**: `docker container ls -a`
- Helpful for checking status, ports, and names.

## Naming Containers
### Better Identification

Give yours custom names instead of random ones:
```bash
docker run --name my-web-server nginx
```
To rename an existing one:
```bash
docker container rename old-name new-name
```

## Controlling State
### Stop, Start, and Restart

- **Stop**: `docker container stop <name/id>` (Graceful).
- **Kill**: `docker container kill <name/id>` (Immediate).
- **Restart**: `docker container restart <name/id>`.
- **Start**: `docker container start <name/id>`.

## Cleanup
### Removing Containers

- **Single**: `docker container rm <name/id>`
- **Force Remove**: `docker container rm -f <name/id>`
- **Prune**: `docker container prune` (Deletes all stopped containers).
- **Auto-remove**: Use `--rm` when running to delete automatically after exit.

## Interactive Mode
### Getting Inside

To explore a container's shell:
```bash
docker container run -it ubuntu bash
```
- `-i`: Interactive mode.
- `-t`: Connects your terminal.
- Lets you run commands directly inside the environment.

## Execute Commands
### Running Inside Active Containers

If a container is already running:
```bash
docker container exec -it <name> <command>
```
Example to open bash in a running nginx:
```bash
docker container exec -it my-nginx bash
```

## Building Images
### The Dockerfile Magic

A text file with instructions to create an image:
```dockerfile
FROM ubuntu:latest
RUN apt-get update
CMD ["echo", "Hello from my custom image!"]
```
Build it with:
```bash
docker image build -t my-custom-image .
```

## Dockerfile Instructions
### Key Commands

- **FROM**: Set your base image.
- **WORKDIR**: Set the default working directory.
- **COPY**: Add files from host to image.
- **RUN**: Execute commands during build.
- **CMD**: The default command when container starts.
- **EXPOSE**: Document intended ports.

## Image Tagging
### Version Control

Tags help identify different versions:
```bash
docker image tag my-image:latest my-image:v1.0
```
Standard format: `repository:tag`.
Default tag is `latest`.

## Sharing Images
### Pushing to Docker Hub

1. Log in: `docker login`
2. Name your image: `username/image-name`
3. Push: `docker push username/image-name`
Now anyone in the world can run your container!

## Optimization
### Alpine Linux

- **Standard Ubuntu**: 70MB+
- **Alpine Linux**: ~5MB
- Using Alpine as a base image makes your images small, fast, and secure.
- Command for Alpine: `FROM alpine:latest`.

## Multi-Staged Builds
### Clean & Small Images

Build in one environment, run in another:
1. **Build Stage**: Compile code (using node/maven).
2. **Run Stage**: Copy only the binary/static files to a tiny image (like nginx/alpine).
3. Result: Tiny production-ready images without build tools.

## Volumes
### Persisting Data

Containers are temporary; volumes are permanent:
- **Anonymous**: `docker run -v /app/data ...`
- **Named**: `docker run -v my-db-data:/var/lib/mysql ...`
- Data survives even if the container is deleted.
- Essential for databases.

## Bind Mounts
### Local Syncing

Map a host folder to a container folder:
```bash
docker run -v /host/path:/container/path ...
```
- Great for development.
- Changes on your machine reflect immediately in the container.
- Use full paths for the host folder.

## Advanced Networking
### User-Defined Bridges

Create a private network for your containers:
```bash
docker network create my-custom-network
```
- Allows containers to talk to each other using their **names**.
- Safer and more reliable than using IP addresses.

## Connecting Containers
### Network Management

- **Attach**: `docker network connect <net> <container>`
- **Detach**: `docker network disconnect <net> <container>`
- **List**: `docker network ls`
- Containers can belong to multiple networks simultaneously.

## Docker Compose
### Multi-Container Orchestration

Instead of multiple `docker run` commands, use one file:
`docker-compose.yml`
- Defines services, networks, and volumes.
- Simple YAML syntax.
- Version control your whole setup.

## Compose: Core Commands
### One Tool to Rule Them All

- `docker-compose up`: Build and start everything.
- `docker-compose down`: Stop and remove everything.
- `docker-compose ps`: Show services status.
- `docker-compose logs`: See output from all containers.
- `docker-compose up -d`: Run in background.

## Practical: JavaScript App
### Containerizing Node.js

1. Choose base: `FROM node:alpine`
2. Set Workdir: `WORKDIR /app`
3. Copy Package: `COPY package.json .`
4. Install: `RUN npm install`
5. Copy Code: `COPY . .`
6. Start: `CMD ["npm", "start"]`

## Optimization Tip
### .dockerignore

Tell Docker which files to skip during COPY:
- `node_modules`
- `.git`
- `Dockerfile`
- Reduces image size and speeds up build time.

## Practical: Database Server
### Running PostgreSQL

```bash
docker run --name my-db \
  -e POSTGRES_PASSWORD=secret \
  -v my-data:/var/lib/postgresql/data \
  -d postgres
```
- Use environment variables for configuration.
- Always use a volume for data storage.

## Monitoring Containers
### Checking Logs

```bash
docker container logs <name/id>
```
- `-f`: Follow logs (real-time stream).
- `--tail N`: See last N lines.
- Vital for debugging application crashes.

## Statistics & Resources
### Checking Performance

```bash
docker stats
```
- Shows real-time CPU, Memory, and Network usage.
- Helps identify containers hogging resources.
- Essential for production monitoring.

## Executable Images
### Defining Entrypoint

Use **ENTRYPOINT** instead of **CMD** for tools:
```dockerfile
ENTRYPOINT ["git"]
```
Now, any arguments you pass to `docker run` go directly to the `git` command.
Example: `docker run my-git status` runs `git status`.

## Multi-Container Apps
### The Reality Check

Most modern apps have:
1. **Frontend**: React/Vue (Nginx)
2. **Backend**: Node/Python (API)
3. **Database**: Postgres/Redis
They must communicate over a shared network and use volumes for state.

## Full-Stack with Compose
### Example Configuration

```yaml
services:
  web:
    build: ./frontend
    ports: ["80:80"]
  api:
    build: ./backend
    networks: ["app-net"]
  db:
    image: postgres
    networks: ["app-net"]
```
Connects everything automatically!

## Port Management
### Avoiding Conflicts

- Host ports must be unique.
- Multiple containers can use the same internal port (e.g., 80) if mapped to different host ports (8081, 8082).
- Check `docker ps` to see currently used ports on your host.

## Image Management
### Listing & Removing Images

- **List Images**: `docker image ls`
- **Remove Image**: `docker image rm <id>`
- **Prune Images**: `docker image prune -a` (Deletes all unused images to free disk space).

## Understanding Layers
### Cache is King

- Each command in a Dockerfile creates a new layer.
- Docker caches layers to make builds faster.
- Place frequently changing commands (like COPY code) at the **bottom** to reuse earlier cached layers.

## Building NGINX from Source
### Advanced Customization

- Sometimes official images aren't enough.
- You can compile software like NGINX inside a container.
- Benefits: Full control over modules and security.
- Use multi-stage builds to keep the final image small.

## Entrypoint vs CMD
### The Main Difference

- **CMD**: Easy to overwrite by adding commands at the end of `docker run`.
- **ENTRYPOINT**: Hard to overwrite; your command becomes the "face" of the container.
- Best practice: Use ENTRYPOINT for the executable and CMD for default arguments.

## Management Scripts
### Automating Operations

- Use shell scripts (`.sh`) to group Docker commands.
- Example: `start.sh` could run `docker run` with all the correct flags.
- Makes it easier for other developers to start the project.

## Debugging Containers
### Inspect Command

```bash
docker container inspect <name>
```
- Returns a massive JSON with every tiny detail.
- Useful for finding IP addresses, volume paths, and environment variables.
- Use `--format` to filter the output.

## Volume Management
### Inspecting Data

- **List Volumes**: `docker volume ls`
- **Inspect Volume**: `docker volume inspect <name>`
- **Prune Volumes**: `docker volume prune` (Deletes unused data—be careful!).
- Volumes reside in `/var/lib/docker/volumes` on Linux.

## Health Checks
### Monitoring App Status

You can add health checks to your Dockerfile:
```dockerfile
HEALTHCHECK --interval=30s \
  CMD curl -f http://localhost/ || exit 1
```
Tells Docker if the app inside is actually working, not just "running".

## Production Best Practices
### Security & Stability

- **Don't run as root**: Create a user inside the Dockerfile.
- **Use generic base images**: Stick to official or verified creators.
- **Lock versions**: Use `node:14-alpine` instead of `node:latest`.
- **Minimize layers**: Combine RUN commands with `&&`.

## Docker Hub Tags
### The "Best" Tags

- **alpine**: Smallest and most secure.
- **slim**: Smaller than default, uses Debian usually.
- **buster/focal/alpine**: Tells you the underlying OS version.
- **latest**: Always risky for production (it changes!).

## Future Roadmap
### What's Next?

- Docker Swarm for simple scaling.
- Kubernetes for complex cloud orchestration.
- CI/CD integration with GitHub Actions.
- Cloud deployment (AWS ECS, Azure ACI).

## Summary & Conclusion
### Key Takeaways

- Containers transform how we build software.
- Mastering the CLI is the first step.
- Docker Compose makes multi-service apps easy.
- Focus on small images and secure practices.

**Time to Build!**
