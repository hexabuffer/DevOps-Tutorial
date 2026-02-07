## Slide 1

# 🐳 Docker Complete Guide 🐋

### Your All-in-One Roadmap to Containerization
---
- **Mastering the cloud** ☁️
- **Professional 3D Visual Guide** 🎨
- **100 Detailed Slides** 📑
- **From Beginner to Expert** 🚀

*(Professional white background with vibrant 3D blue branding)*

## Slide 2

# 🧩 What is Docker?

### The Modern Way to Ship Apps
---
- **Containerization Platform**: An open-source tool that automates app deployment 📦
- **The "Box" Concept**: Packages code and all its dependencies into one "box" 📦
- **Consistency**: Ensures the app runs the same on your laptop or the cloud 🌍
- **Innovation**: Revolutionized how software is built and shipped today ⚡

## Slide 3

# 🌟 Why Docker Matters

### Solving the "Works on My Machine" Problem
---
- **Eliminate Conflicts**: No more library version clashes between apps ⚔️
- **Fast Deployment**: Spin up new environments in seconds, not hours ⏱️
- **Scalability**: Easily handle increased traffic by adding more containers 📈
- **Standardization**: Uniform environment for Dev, QA, and Production 🛠️

## Slide 4

# 📦 Key Concept: Containers

### The Lightweight Execution Unit
---
- **Isolated**: Contains everything needed to run: code, runtime, system tools 🛡️
- **Portable**: Move it between any OS that has Docker installed 🚚
- **Efficient**: Shares the host OS kernel, making it very fast 🚀
- **Small**: Usually measured in MBs, unlike Gigabyte-sized VMs 📏

## Slide 5

# 🖼️ Key Concept: Images

### The Immutable Blueprints
---
- **Read-Only**: Guidelines for creating a container 📜
- **Stackable**: Built in layers to save space and speed up builds 🧅
- **Portable**: Shareable via registries like Docker Hub 🌐
- **Versioned**: Tag images to keep track of different app versions 🏷️

## Slide 6

# 🚀 Benefits of Docker

### Speed, Scale, and Reliability
---
- **Speed**: Build and deploy faster than ever before ⚡
- **Density**: Run more apps on the same hardware compared to VMs 📉
- **ROI**: Lower infrastructure and maintenance costs 💰
- **Security**: Granular control over application resources 🔒

## Slide 7

# 💼 Docker Real-World Use Cases

### How Industry Leaders Use It
---
- **Microservices**: Breaking big apps into small, manageable pieces 🧩
- **CI/CD**: Automating the build-test-deploy pipeline 🔄
- **Cloud Migration**: Easily moving legacy apps to the cloud ☁️
- **Data Science**: Creating reproducible research environments 🧪

## Slide 8

# 🌍 The Docker Ecosystem

### More Than Just Containers
---
- **Docker Hub**: The world's largest library of container images 📚
- **Docker Compose**: Tool for defining multi-container applications 🎼
- **Docker Desktop**: The easy way to run Docker on your PC 💻
- **Docker Swarm**: Native orchestration for clusters 🐝

## Slide 9

# 🛤️ Getting Started Mindset

### Thinking in Containers
---
- **Immutable Infrastructure**: Don't patch; replace with a new container 🔄
- **Statelessness**: Keep your data outside the container (Volumes) 💾
- **Single Responsibility**: One process per container is the golden rule ☝️
- **Automation First**: If you do it twice, script it in a Dockerfile 🤖

## Slide 10

# 📝 Summary: Introduction

### Chapter 1 Recap
---
- ✅ Docker is about **Standardization** 📏
- ✅ Containers are **Fast and Isolated** 🏃
- ✅ Images are the **Blueprints** 📜
- ✅ It solves the **Environment Consistency** issue 🌐

*(Ready to dive into Architecture?)*

## Slide 11

# 🏗️ Docker Architecture Overview

### Understanding the Back-End
---
- **Client-Server Model**: The CLI talks to a background service 📞
- **Decoupled**: Components can be on the same or different machines 🔗
- **REST API**: Standardized communication between all parts 🌐
- **Security**: Mutual TLS used for secure remote connections 🔒

## Slide 12

# ⌨️ The Docker Client

### Command Center for Users
---
- **The CLI**: Where you type `docker` commands 💻
- **Primary Tool**: The most common way to interact with the engine 🛠️
- **Multi-Context**: Can control local or remote Docker engines 🌉
- **Format**: Commands usually follow `docker <object> <action>` 📝

```bash
docker container run ...
```

## Slide 13

# 🧠 The Docker Daemon (dockerd)

### The Brain of the Operation
---
- **Service**: Runs in the background on the Host OS ⚙️
- **Life Manager**: Creates and manages Images, Containers, and Networks 🧟
- **Resource Guard**: Allocates CPU, Memory, and Disk to containers 🛡️
- **API Listener**: Constantly waits for requests from the Client 👂

## Slide 14

# 🏠 The Docker Host

### Where the Magic Happens
---
- **Physical or Virtual**: The machine where Docker is installed 🖥️
- **Linux Kernel**: Docker relies on Linux "Namespaces" and "Cgroups" 🐧
- **Engine**: Consists of the Daemon, API, and Desktop tools 🔧
- **Isolation**: Host OS stays clean while containers run inside it 🧼

## Slide 15

# 🌏 Docker Registry & Hub

### The Global Image Library
---
- **Registry**: A storage and distribution system for images 📦
- **Docker Hub**: The default public registry with millions of images 📚
- **Private Registry**: Companies host their own for internal security 🔐
- **Push & Pull**: The mechanism for uploading and downloading images 🔄

```bash
docker pull nginx:latest
```

## Slide 16

# 🔄 Build, Ship, and Run

### The Standard Docker Workflow
---
- **BUILD**: Create an image from a Dockerfile 🔨
- **SHIP**: Push that image to a registry like Docker Hub 🚢
- **RUN**: Pull and start the container on any machine 🏃
- **Outcome**: Exact same behavior from Dev to Production 🎯

## Slide 17

# 🏗️ Architecture Diagram Breakdown

### Visualizing the Flow
---
- **Step 1**: Docker Client sends a build command 📤
- **Step 2**: Daemon builds image using local files 🏗️
- **Step 3**: Image is stored in the local cache 💾
- **Step 4**: Container is created and started from that image 🎬

*(Visualize 3D colored boxes for Client, Host, and Registry)*

## Slide 18

# 🕵️ Docker Objects Explained

### The Building Blocks
---
- **Images**: Read-only templates 📜
- **Containers**: Runnable instances 🏃
- **Networks**: Communication channels 🕸️
- **Volumes**: Data storage 💾
- **Plugins**: Extra features like advanced logging/networking 🔌

## Slide 19

# 🚥 Interaction: Client to Daemon

### How They Talk
---
- **Unix Sockets**: Standard communication when on the same machine 🔌
- **TCP**: Used for remote management across network 📡
- **Security Check**: Docker checks permissions before executing anything 👮
- **Persistence**: Daemon keeps containers running even if client disconnects 💡

## Slide 20

# 📝 Summary: Architecture

### Chapter 2 Recap
---
- ✅ **Client-Server** is the core design 🔌
- ✅ **Daemon** does the heavy lifting 🧠
- ✅ **Registry** acts as the store 🏪
- ✅ **Images** are converted into **Containers** 🔄

*(Next: Let's get Docker installed!)*

## Slide 21

# 💿 Docker Editions

### Community vs Enterprise
---
- **Docker CE (Community)**: Free, open-source, great for devs 🆓
- **Docker EE (Enterprise)**: Paid support, advanced security 🔒
- **Docker Desktop**: UI-based tool for Mac/Windows productivity 🖥️
- **Docker Engine**: The core CLI and daemon for Linux servers 🐧

## Slide 22

# 🖥️ Docker Desktop Overview

### Making Docker Easy for Everyone
---
- **All-in-One**: Includes Engine, CLI, Compose, and Kubernetes 🛠️
- **GUI**: Manage containers and images with points & clicks 🖱️
- **Resources**: Easy sliders to set CPU and RAM limits ⚙️
- **Dashboard**: Live view of logs and usage metrics 📊

## Slide 23

# 🐧 Installing on Ubuntu Linux

### The Popular Choice for Servers
---
- **Apt Repo**: Add Docker's official GPG key and repo 🔑
- **Dependencies**: Install `curl`, `ca-certificates`, and `gnupg` 📦
- **Service**: Start and enable the docker service 🎬
- **Sudo**: Remember to add your user to the 'docker' group 👤

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

## Slide 24

# 🍎 Installing on macOS

### Native-like Performance
---
- **DMG File**: Simple drag-and-drop installation 🖱️
- **Virtuaization**: Uses Apple's Hypervisor framework 🍎
- **Menubar App**: Quick access to settings and status 🔝
- **Updates**: Automatic notifications for the latest version 🔄

## Slide 25

# 🪟 Installing on Windows

### Empowering Windows Developers
---
- **WSL 2 Support**: Essential for the best performance ⚙️
- **Hyper-V**: Legacy backend option for older systems 🖥️
- **Linux Integration**: Access Docker from Ubuntu/Debian shells 🐧
- **Installation**: Straightforward EXE installer 💿

## Slide 26

# 📜 The Magic Install Script

### Fastest Setup for Linux
---
- **One Liner**: Great for quick testing and VMs ⚡
- **Official Tool**: Provided by Docker at `get.docker.com` 🌐
- **Automated**: Detects OS and installs all requirements 🤖
- **Note**: Not recommended for high-security production ⚠️

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
```

## Slide 27

# 🏁 Post-Installation Steps

### Setting Up for Success
---
- **Group Access**: Add user to `docker` group to avoid using `sudo` 👤
- **Systemd**: Ensure Docker starts on server boot 🔄
- **Cleanup**: Remove temporary installation files 🧹
- **Config**: Setup `daemon.json` for custom settings ⚙️

```bash
sudo usermod -aG docker $USER
```

## Slide 28

# ✅ Verifying Your Install

### The Ultimate Test
---
- **Version Check**: Confirm Docker is in your path 🔍
- **Hello World**: Run the official test container 🌎
- **Permissions**: Confirm you can run without root 🔐
- **Info**: View detailed system and driver stats 📊

```bash
docker version
docker run hello-world
```

## Slide 29

# 🛠️ Troubleshooting Installation

### When Things Go Wrong
---
- **Daemon Error**: Check if the service is actually running 🧐
- **Permission Denied**: Logout/Login to apply group changes 🚪
- **WSL 2 Errors**: Ensure Linux Kernel update is installed 🐧
- **Logs**: Use `journalctl -u docker` to see deep errors 📜

## Slide 30

# 📝 Summary: Installation

### Chapter 3 Recap
---
- ✅ Choose **Community** for free usage 🆓
- ✅ **Desktop** is best for Mac/Windows 🖥️
- ✅ **USermod** saves you from typing 'sudo' ⌨️
- ✅ `docker run hello-world` is your **best friend** 🤝

*(Next: Dealing with Containers!)*

## Slide 31

# 🧟 What is a Container? (Deep Dive)

### The Running Application
---
- **Namespaces**: Provide isolation (Network, PID, Mount) 🛡️
- **Control Groups**: Manage resource usage (CPU, Memory) 📊
- **Writable Layer**: A thin layer on top of read-only image ✍️
- **Sandbox**: Changes made inside stay inside 🏖️

## Slide 32

# 🎬 Running Your First Container

### Breaking the Ice
---
- **Pulling**: Docker looks for image locally, then Hub 📥
- **Creating**: Sets up the isolated environment 🏗️
- **Starting**: Kicks off the main process (PID 1) 🏁
- **Cleanup**: Auto-removes after exit if `--rm` is used 🧹

```bash
docker run --rm busybox echo "Hello Docker!"
```

## Slide 33

# 🔄 The Container Lifecycle

### From Birth to Death
---
- **Created**: Environment ready, but not running 🥚
- **Running**: Main process is active 🏃
- **Paused**: Process frozen to save CPU ⏸️
- **Stopped**: Process ended, but container still exists 🛑
- **Removed**: Container and its local data deleted 💀

## Slide 34

# 📌 Basic Commands Part 1

### Managing the Runtime
---
- **run**: Create and start one time 🏃
- **stop**: Send SIGTERM to shut down gracefully 🛑
- **kill**: Force shutdown immediately (SIGKILL) 🔫
- **restart**: Stop and start again 🔄

```bash
docker stop my_app
docker restart my_app
```

## Slide 35

# 📌 Basic Commands Part 2

### Information & Inspection
---
- **ps**: View status of running containers 👁️
- **inspect**: Detailed JSON data about a container 🔍
- **stats**: Live CPU/RAM usage stream 📊
- **top**: Look at processes running inside 🕵️

```bash
docker ps -a
docker inspect <id>
```

## Slide 36

# 👁️ Listing Containers (docker ps)

### Monitoring Your Fleet
---
- **Default**: Shows only running containers 🚀
- **-a Flag**: Shows all (stopped, exited, created) 📋
- **-q Flag**: Returns only the container IDs (Quiet mode) 🤫
- **--filter**: Search by status, name, or image 🔍

```bash
docker ps --filter "status=exited"
```

## Slide 37

# 🛑 Starting & Stopping

### Controlling State
---
- **Wait Time**: Docker gives 10s default for graceful stop ⏱️
- **Exit Codes**: 0 is normal; non-zero usually means error 🚩
- **Single vs Multi**: Use space to stop multiple at once 🤝
- **Start**: Resumes a stopped container exactly where it was ⏯️

```bash
docker stop web1 web2 db1
```

## Slide 38

# 💀 Removing Containers

### Keeping Things Tidy
---
- **Pre-requisite**: Container must be stopped first 🛑
- **Force**: Use `-f` to remove a running one (dangerous!) ⚠️
- **Prune**: The "Delete All" button for stopped containers 🧹
- **Volumes**: Volumes stick around even if container is gone 💾

```bash
docker rm -f redundant_container
docker container prune
```

## Slide 39

# 🏷️ Container Naming & IDs

### Identify Your Instances
---
- **UUID**: Long unique string (e.g., a8c3...) 🔢
- **Short ID**: First 12 chars of the UUID 🔡
- **Custom Name**: Use `--name` for pretty identifiers ✨
- **Random Name**: Docker gives funny names like `elated_einstein` 🤪

```bash
docker run --name my-web-server nginx
```

## Slide 40

# 📝 Summary: Container Basics

### Chapter 4 Recap
---
- ✅ Containers are **isolated processes** 🛡️
- ✅ `docker ps -a` shows your **history** 📜
- ✅ Use **meaningful names** with `--name` 🏷️
- ✅ **Prune** often to save disk space 🧹

*(Next: Master advanced operations!)*

## Slide 41

# 👻 Detached Mode (-d)

### Background Operations
---
- **Purpose**: Let servers run "silently" in the background 🤫
- **Output**: Returns the container ID and returns to terminal 🆔
- **Logs**: Use `docker logs` to see what is happening 📜
- **Stop**: Still managed as normal via `docker stop` 🛑

```bash
docker run -d -p 80:80 nginx
```

## Slide 42

# ⌨️ Interactive Mode (-it)

### Getting Hands-On
---
- **-i (Interactive)**: Keep STDIN open 🔌
- **-t (TTY)**: Allocate a "pseudo-terminal" 🖥️
- **Shell**: Usually used with `/bin/bash` or `/bin/sh` 🐚
- **Exit**: Type `exit` to stop the container (or Ctrl+P+Q to detach) 🚪

```bash
docker run -it ubuntu /bin/bash
```

## Slide 43

# 🔌 Port Mapping & Exposure

### Connectivity to the Real World
---
- **Isolation**: Containers are locked away by default 🔒
- **Mapping**: Link HostPort to ContainerPort 🔗
- **Syntax**: `-p <HostPort>:<ContainerPort>` 📍
- **Multiple**: Map as many ports as needed in one command 🚦

```bash
docker run -p 8080:80 -p 3306:3306 my-app
```

## Slide 44

# 📜 Viewing Container Logs

### Troubleshooting in Real-Time
---
- **Stdout/Stderr**: Docker captures all console output 📡
- **-f (Follow)**: Stream logs as they happen 🌊
- **--tail**: See only the last N lines (e.g. 100) 📋
- **Timestamps**: Add `-t` to see exactly when log occurred ⏱️

```bash
docker logs -f --tail 20 my_app
```

## Slide 45

# 🕵️ Executing Commands in Running Containers

### The "Ssh" Alternative
---
- **Context**: Run a new command inside an *already* running container 🏃
- **Maintenance**: Useful for checking configs or db connectivity 🛠️
- **Temporary**: Command ends, but container stays running 🚭
- **Shell**: Most common use is opening an interactive shell 🐚

```bash
docker exec -it my_db psql -U root
```

## Slide 46

# 📂 Copying Files (docker cp)

### Moving Assets Around
---
- **Bi-Directional**: Host to Container AND Container to Host ↔️
- **No Stop Needed**: Works while container is running or stopped ✅
- **Usage**: Copying logs out or config files in for testing 📑
- **Syntax**: `docker cp <src> <dest>` (Container specified as ID:Path) 📍

```bash
docker cp ./config.json my_app:/app/config/
```

## Slide 47

# 🏎️ Resource Limits: CPU

### preventing the "Noisy Neighbor"
---
- **Sharing**: By default, containers use all host CPU ⚖️
- **--cpus**: Limit to a specific number (e.g., "0.5" for half a core) 🔢
- **--cpu-shares**: Relative weight compared to other containers 🏋️
- **Performance**: Prevents one app from crashing the whole server 🛡️

```bash
docker run --cpus="1.5" my-heavy-app
```

## Slide 48

# 🧠 Resource Limits: Memory

### Keeping RAM in Check
---
- **OOM Kill**: Containers using too much RAM will be stopped by OS 🛑
- **-m / --memory**: Hard limit (e.g., 512m, 2g) 📏
- **--memory-reservation**: Soft limit for guaranteed allocation 🧘
- **Swap**: Can also limit swap space if needed 💾

```bash
docker run -m 512m --memory-reservation 256m redis
```

## Slide 49

# 🔄 Restart Policies

### Auto-Healing Systems
---
- **no**: Do not restart (default) 🛑
- **always**: Always restart regardless of exit code 🔄
- **unless-stopped**: Restart unless user manually stopped it 🛡️
- **on-failure**: Restart only if it crashed (exit != 0) 🚩

```bash
docker run -d --restart always nginx
```

## Slide 50

# 📝 Summary: Container Operations

### Chapter 5 Recap
---
- ✅ Use `-d` for **background** servers 👻
- ✅ Use `-p` to **connect** to the web 🔌
- ✅ **Logs** are your first step in debugging 📜
- ✅ **CPU/RAM limits** keep your host safe 🛡️

*(Next: Understanding Images!)*

## Slide 51

# 🖼️ What are Docker Images? (Deep)

### The DNA of a Container
---
- **Template**: Contains OS files, libraries, and your code 🧬
- **Read-Only**: Stored as a set of static files 📑
- **Metadata**: Stores the author, command to run, and ports 🏷️
- **Portable**: Can be compressed and sent over the internet 🌐

## Slide 52

# 🧅 Image Layers Explained

### The Onion Structure
---
- **Union Filesystem**: Layers are stacked to look like one disk 📁
- **Intermediate**: Every instruction in Dockerfile creates a layer 🪜
- **Shared**: If two images use the same base, they share the physical files 🤝
- **Copy-on-Write**: Only changes are written to a thin top layer ✍️

## Slide 53

# 🧱 Base Images vs Parent Images

### Building on Shoulders of Giants
---
- **Base Image**: The very bottom (e.g., `scratch` or `alpine`) 🏁
- **Parent Image**: The image listed in the `FROM` instruction 👨‍👧
- **Official**: Verified images managed by Docker (no prefix) ⭐
- **User**: Custom images made by you or others (prefix: `user/`) 👤

## Slide 54

# 🏷️ Image Tags & Versioning

### Organizing Your Releases
---
- **Format**: `repository:tag` (e.g., `nginx:1.21`) 🏷️
- **Latest**: The default tag if none is specified 🆕
- **Safety**: Avoid using `:latest` in production; be specific! ⚠️
- **Aliases**: One image can have multiple tags (v1.0, stable, latest) 🏷️🏷️

```bash
docker build -t myapp:v1.0 .
```

## Slide 55

# 📋 Listing Images (docker images)

### Managing Your Local Disk
---
- **Columns**: Repository, Tag, Image ID, Created, Size 📊
- **Storage**: Total size shown (doesn't account for layer sharing) 💾
- **Dangling**: Images with no name/tag (result of failed builds) 👻
- **-f dangling=true**: Find these space-wasters easily 🧹

```bash
docker images -a
```

## Slide 56

# 📥 Pulling from Docker Hub

### Downloading the Ingredients
---
- **Automated**: Happens automatically on first `docker run` 🤖
- **Manual**: Use `docker pull` to update before running 📥
- **Digest**: Images also have a unique hash (SHA256) for integrity 🔒
- **Network**: Only pulls layers you don't already have ⚡

```bash
docker pull alpine:3.14
```

## Slide 57

# 🚢 Executing from Images

### From Blueprint to Action
---
- **Run vs Start**: Run creates a new container; Start wakes an old one 🏁
- **Interactive**: Great for testing an image's environment 🐚
- **One-Off**: Use `--rm` for images you only need for one command 🧹
- **Override**: You can change the starting command at runtime 📝

```bash
docker run python:3.9 python --version
```

## Slide 58

# 🔍 Inspecting Images

### Seeing Under the Hood
---
- **Metadata**: See ENV variables, exposed ports, and OS details 🕵️
- **History**: See every command used to build the image 📜
- **Layers**: View the IDs of the static filesystems 🧅
- **Format**: Returns a large JSON object (use `-f` to filter) 📝

```bash
docker image inspect ubuntu
docker history my-app:latest
```

## Slide 59

# 🧹 Removing Images (rmi)

### Reclaiming Space
---
- **Dependency**: Cannot delete an image used by an *existing* container 🛑
- **Untagged**: Deleting a tag only removes the name if other tags exist 🏷️
- **Force**: `-f` can override some safety checks (use carefully) ⚠️
- **Prune**: Removes all unused images at once 🧹

```bash
docker rmi <image_id>
docker image prune -a
```

## Slide 60

# 📝 Summary: Docker Images

### Chapter 6 Recap
---
- ✅ Images are **Onion-like Layers** 🧅
- ✅ They are **Immutable** (Read-Only) ❄️
- ✅ Tagging is for **Versioning** 🏷️
- ✅ `prune` is the key to a **happy SSD** 💾

*(Next: Writing your own Dockerfiles!)*

## Slide 61

# 📜 Introduction to Dockerfile

### The Building Instructions
---
- **Text File**: Simply named `Dockerfile` (no extension) 📝
- **Declarative**: You tell Docker what you want, it handles the "how" 🤖
- **Version Control**: Keep it in Git with your code 👩‍💻
- **Flow**: Processes top to bottom, one line at a time ⬇️

```dockerfile
# Comments start with hash
FROM alpine
CMD ["echo", "Hello!"]
```

## Slide 62

# 🏁 FROM: The Starting Point

### Every Journey has a Beginning
---
- **Requirement**: Must be the first non-comment instruction ☝️
- **Choice**: Can be a full OS (Ubuntu) or a runtime (Node/Python) 🏗️
- **Scratch**: A special empty image for teeny tiny binary builds 🤏
- **Platform**: Multi-arch support is possible here 🌍

```dockerfile
FROM python:3.8-slim
```

## Slide 63

# 🏷️ LABEL: Metadata

### Documenting Your Image
---
- **Info**: Key-value pairs for organization 📑
- **Usage**: Maintainer, Version, Description, License 👤
- **Visibility**: Visible via `docker inspect` 🔍
- **Best Practice**: Combine labels into one line to save layers 🧹

```dockerfile
LABEL maintainer="me@example.com" version="1.0"
```

## Slide 64

# 🧪 ENV: Environment Variables

### Configuring at Runtime
---
- **Availability**: Accessible during build AND when container runs 🌡️
- **Efficiency**: No need to hardcode paths or settings ⚙️
- **Overrides**: Can be changed via `docker run -e ...` 📝
- **Variables**: Use `$VAR` syntax later in the Dockerfile 🧬

```dockerfile
ENV APP_HOME=/usr/src/app
WORKDIR $APP_HOME
```

## Slide 65

# 📂 WORKDIR: Location, Location

### Where Am I?
---
- **Default**: Root `/` if not specified 📂
- **Creation**: Will create the directory if it doesn't exist ✨
- **Persistence**: Affects all subsequent instructions (RUN, CMD, etc.) ⬇️
- **Relative**: Can use relative paths once set 📍

```dockerfile
WORKDIR /app
COPY . .
```

## Slide 66

# 🚛 COPY vs ADD

### Moving Stuff In
---
- **COPY**: Preferred. Simple file transfer from host to image 📄
- **ADD**: "Magic" version. Can extract Tar files and download URLs ✨
- **Src**: Files must be relative to the Dockerfile ("build context") 🗺️
- **Dest**: Absolute or relative to WORKDIR 📍

```dockerfile
COPY requirements.txt ./
ADD https://example.com/data.gz /tmp/
```

## Slide 67

# 🔨 RUN: Building the System

### The Most Common Command
---
- **Execute**: Runs any shell command during the build phase ⌨️
- **Layering**: Every RUN creates a new snapshot layer 🧅
- **Chaining**: Join commands with `&&` to reduce image size 🔗
- **Cache**: Docker skips RUN if the instruction hasn't changed ⚡

```dockerfile
RUN apt-get update && apt-get install -y git
```

## Slide 68

# 🎬 CMD: The Default Action

### What Do We Do Now?
---
- **Start**: The command executed when `docker run` starts 🏁
- **Override**: User can easily replace this with their own command 📝
- **Format**: Use the "Exec" (JSON) array form for better signal handling 📦
- **Limit**: Only ONE CMD allowed; the last one wins 🏁

```dockerfile
CMD ["python", "app.py"]
```

## Slide 69

# 💂 ENTRYPOINT: The Identity

### Making Your Image an Executable
---
- **Fixed**: Harder to override than CMD 🔒
- **Collaboration**: Usually works with CMD to provide arguments 🤝
- **Utility**: Best for images that behave like a command tool 🛠️
- **Syntax**: Use `["executable", "param1"]` format 📦

```dockerfile
ENTRYPOINT ["/usr/bin/git"]
CMD ["--help"]
```

## Slide 70

# 📝 Summary: Dockerfile Basics

### Chapter 7 Recap
---
- ✅ `FROM` defines the **Base** 🏗️
- ✅ `WORKDIR` stops the **path-hunting** 📍
- ✅ `COPY` brings in your **Code** 🚚
- ✅ `RUN` builds the **App** 🔨
- ✅ `CMD` starts the **Show** 🎬

*(Next: Leveling up your builds!)*

## Slide 71

# 🔌 EXPOSE: Documentation Only

### Talking to the Network
---
- **Informative**: Tells Docker which ports the app listens on 📢
- **Internal**: Does NOT actually open ports to host (use `-p` for that) 🔒
- **Metadata**: Visible via `docker inspect` for users to know ports 🕵️
- **Protocols**: Can specify TCP or UDP 🚦

```dockerfile
EXPOSE 80/tcp 443/tcp
```

## Slide 72

# 💾 VOLUME: Auto-Creation

### Preparing the Space
---
- **Persistent**: Marks a directory to survive container removal 💾
- **Auto-create**: Docker creates a 'named' volume at runtime if not mapped 🏷️
- **Security**: Can protect databases from accidental data loss 🛡️
- **Limit**: Cannot set volume source in Dockerfile (Host independence) 🌍

```dockerfile
VOLUME ["/var/lib/mysql"]
```

## Slide 73

# 🏗️ ARG: Build-time Arguments

### Customizable Builds
---
- **Scope**: ONLY available during the build process (unlike ENV) ⏳
- **Flags**: Pass values via `--build-arg key=val` 🚩
- **Security**: DO NOT put passwords in ARGs (visible in history!) ⚠️
- **Dynamic**: Useful for choosing specific versions at build time 🏷️

```dockerfile
ARG VERSION=latest
FROM ubuntu:${VERSION}
```

## Slide 74

# 🪜 Multi-Stage Builds

### The Secret to Tiny Images
---
- **Concept**: Use one stage to build (with compilers) and another for prod 🪜
- **Copying**: Pull only the final binary/files into the clean image 🚚
- **Benefit**: Keeps source code and build tools out of production 🛡️
- **Size**: Can reduce image size from 1GB to 10MB! 📉

```dockerfile
FROM golang:1.16 AS build
ADD . /src
RUN go build -o /app /src/main.go

FROM alpine
COPY --from=build /app /app
```

## Slide 75

# ✨ Dockerfile Best Practices

### Building Like a Pro
---
- **Small Bases**: Use `alpine` or `distroless` where possible 🐜
- **Sort Chains**: Keep multi-line RUNs sorted alphabetically for readability 📋
- **Cleanup**: Delete package manager caches in the SAME layer 🧹
- **Security**: Use official non-root images or create your own user 👤

```dockerfile
RUN apt-get update && apt-get install -y \
    git \
    vim \
 && rm -rf /var/lib/apt/lists/*
```

## Slide 76

# ⚡ Layer Caching Optimization

### Speed Up Your Dev Flow
---
- **Ordering**: Place instructions that change frequently at the bottom ⬇️
- **Copy Smart**: Copy `package.json` first, run install, THEN copy code ⚡
- **Invalidation**: If a layer changes, ALL following layers must rebuild  domino  domino effect 🧱
- **Remote Cache**: Use `--cache-from` in CI pipelines 📡

```dockerfile
COPY package*.json ./
RUN npm install
COPY . .
```

## Slide 77

# 🚫 .dockerignore

### Keep the Trash Out
---
- **Context**: Reduces the data sent to the Docker Daemon 📤
- **Privacy**: Keeps `.git`, local logs, and secrets out of the image 🔒
- **Efficiency**: Prevents cache invalidation from local temp files ⚡
- **Format**: Same syntax as `.gitignore` 📝

```text
node_modules
.git
*.log
.env
```

## Slide 78

# 📝 Summary: Advanced Dockerfile

### Chapter 8 Recap
---
- ✅ Use **Multi-stage** for production 🪜
- ✅ **Chaining** saves precious layers 🔗
- ✅ **ARG** for build-time flexibility 🎨
- ✅ **.dockerignore** is the filter your build needs 🧼

*(Next: Connecting it all with Networking!)*

## Slide 79

# 🕸️ Docker Networking Overview

### The Digital Switchboard
---
- **Isolation**: Containers are on their own private "island" by default 🏝️
- **Drivers**: Re-usable network logic (Bridge, Host, Overlay) 🏗️
- **DNS**: Docker has a built-in DNS for container-to-container talk 📞
- **Security**: Easily firewall off databases from the internet 🧱🛡️

## Slide 80

# 🌉 Bridge Network (Default)

### Private & Secure
---
- **Default**: The `docker0` bridge connects all standard containers 🤝
- **Isolation**: Containers on the same bridge can talk via IP/Name 🖇️
- **Outside Access**: Uses NAT to let containers reach the internet 📡
- **Custom Bridge**: RECOMMENDED. Enables automatic DNS resolution 📛

```bash
docker network create my-net
docker run --network my-net ...
```

## Slide 81

# 🏠 Host Network Mode

### High-Speed & Transparent
---
- **Bypass**: Removes network isolation completely 🚫
- **Performance**: Zero network overhead (fastest option) 🏎️
- **Port Clashes**: Container uses Host's ports directly; watch for conflicts! ⚡
- **Usage**: Good for system monitoring tools or high-load apps 🏢

```bash
docker run --network host prometheus
```

## Slide 82

# ☁️ Overlay Network

### Connecting the Cluster
---
- **Multi-Host**: Lets containers on Server A talk to those on Server B 📡
- **Swarm**: Requires Docker Swarm or Kubernetes to function 🐝
- **Encryption**: Traffic can be automatically encrypted (IPsec) 🔒
- **Abstraction**: Apps don't know they are on different physical machines 🧙‍♂️

## Slide 83

# 🎭 MacVLAN Network

### Making it "Real"
---
- **MAC Address**: Gives each container its own unique physical address 🆔
- **Direct**: Appears as a physical device on your corporate router 🌐
- **Bypass Docker**: No NAT or bridge; it's a "first-class citizen" 🏛️
- **Usage**: Legacy apps that need direct hardware-level access 💾

## Slide 84

# 🛠️ Creating Custom Networks

### Designing Your Topology
---
- **Segmentation**: Put Frontend on one net and Backend on another ⛓️
- **Hot-Plug**: Can connect containers to networks while they are running 🔌
- **Inspection**: See which IPs are assigned to each container 🔍
- **Drivers**: Specify `-d` for custom drivers like Weave or Calico 🎨

```bash
docker network inspect bridge
docker network create --driver bridge my-db-net
```

## Slide 85

# 🖇️ Connecting Containers

### Setting Up the Conversation
---
- **Automatic DNS**: Ping containers by their name (e.g., `ping database`) 📛
- **Multi-Connect**: A single container can be on multiple networks 🕸️
- **Disconnect**: Safely remove a container from a network without stopping 🔌
- **Security**: Only containers on the same network can initiate contact 🛡️

```bash
docker network connect backend-net frontend-api
```

## Slide 86

# 📝 Summary: Networking

### Chapter 9 Recap
---
- ✅ **Bridge** is for local isolation 🌁
- ✅ **Host** is for maximum speed 🏎️
- ✅ **Overlay** is for the cloud scale ☁️
- ✅ Use **Custom Networks** for DNS magic 📛

*(Next: Saving data with Volumes!)*

## Slide 87

# 💾 Why Docker Volumes?

### The Data Dilemma
---
- **Ephemeral**: Standard container data is deleted if the container is 💀
- **Speed**: Writing to volumes is faster than the "Union FS" layering ⚡
- **Decoupling**: Store your data separately from your application code 🔗
- **Backup**: Volumes are easier to snapshot and migrate 📁📦

## Slide 88

# 🏷️ Named Volumes

### The "Docker Managed" Way
---
- **Location**: Docker decides where to store it on the host (e.g., `/var/lib/...`) 📍
- **Simplicity**: No need to worry about host OS paths or permissions 🛡️
- **Safe**: Survives accidental deletions better than host folders 🔒
- **Command**: Managed entirely via the `docker volume` CLI ⌨️

```bash
docker volume create my-database
docker run -v my-database:/data redis
```

## Slide 89

# 👻 Anonymous Volumes

### Disposable Storage
---
- **Auto**: Created when you use `VOLUME` in Dockerfile but don't map it 🤖
- **Random Name**: Impossible to find manually; look like long hashes 🔢
- **Cleanup**: Use `docker rm -v` to delete them when container dies 🧹
- **Usage**: Good for temporary caches or swap files ⏳

## Slide 90

# 🔗 Bind Mounts

### Living on Your Host
---
- **Direct Link**: Maps a specific host folder to a container folder 🗺️
- **Usage**: Great for development (source code live-reloading) 🛠️
- **Permissions**: Beware! Files created by container are owned by container user 👤
- **Warning**: Portability is harder because host path must exist ⚠️

```bash
docker run -v $(pwd):/app -it node
```

## Slide 91

# 🛠️ Volume Commands & Management

### Keeping the Storage Clean
---
- **ls**: View all local volumes 📋
- **prune**: Delete all volumes not currently used by a container 🧹
- **inspect**: See the underlying mount point on your drive 🔍
- **Drivers**: Mount cloud storage (S3, EBX) directly as a volume! ☁️

```bash
docker volume ls
docker volume rm old-data
```

## Slide 92

# 📝 Summary: Volumes

### Chapter 10 Recap
---
- ✅ **Named Volumes** are for production data 💾
- ✅ **Bind Mounts** are for local development 👩‍💻
- ✅ Volumes **survive** container death 🧟
- ✅ `volume prune` is your disk's **best friend** 🧹

*(Next: Composing multi-container apps!)*

## Slide 93

# 🎼 What is Docker Compose?

### The Orchestrator for Everyone
---
- **Multi-Container**: Manage 10 containers as easily as 1 🚦
- **Standardized**: Use a single YAML file to share app config 📄
- **Reproducible**: Same stack for every developer on the team 👥
- **Smart**: Only restarts services that have actually changed 🧠

```bash
docker compose up -d
```

## Slide 94

# 📄 docker-compose.yml Structure

### The Master Plan
---
- **Version**: Defines the schema version (e.g., '3.8') 🏷️
- **Services**: The "list" of containers (web, db, cache) 📋
- **Networks**: defines how services talk to each other 🕸️
- **Volumes**: Defines global storage points 💾

```yaml
services:
  web:
    image: nginx
    ports: ["80:80"]
  db:
    image: mysql
```

## Slide 95

# 🛠️ Docker Compose Commands

### Managing the Ensemble
---
- **up**: Create and start all services 🚀
- **down**: Stop and REMOVE all containers, nets, and images 💀
- **logs**: View multiplexed logs from all apps at once 🌈
- **build**: Force-rebuild only the custom images in the file 🔨

```bash
docker compose down --volumes
docker compose ps
```

## Slide 96

# 📝 Summary: Docker Compose

### Chapter 11 Recap
---
- ✅ 1 File = 1 **Infrastructure** 📄
- ✅ `docker compose up` is the **magic button** 🪄
- ✅ Automated **Networking** happens per-project 🕸️
- ✅ **YAML** keeps things human-readable 👤

*(Next: Keeping it all secure!)*

## Slide 97

# 🛡️ Docker Security Best Practices

### Locking Down the Engine
---
- **Least Privilege**: Run containers as non-root users 👤
- **Resource Limits**: Prevent containers from eating host RAM 🛡️
- **Network**: Use private internal networks for databases 🔒
- **Daemon**: Don't expose the Docker Socket to the internet 🌐🚫

## Slide 98

# 🔍 Scanning & Vulnerabilities

### Trust, but Verify
---
- **Official Images**: Stick to verified publishers on Hub ⭐
- **Docker Scan**: Use the built-in vulnerability scanner 🕵️
- **Minimalism**: Use Alpine to reduce the "Attack Surface" 🐜
- **Updates**: Keep your host OS and Docker engine patched 🔄

```bash
docker scan my-image:latest
```

## Slide 99

# 🔐 Non-Root & Secrets

### Protecting Sensitive Data
---
- **USER Instruction**: Switch to low-privilege user in Dockerfile 👤
- **Enviroment Vars**: Use THEM for config, but NOT for passwords ❌
- **Secrets Management**: Use Docker Secrets or HashiCorp Vault 🔐
- **Read-Only FS**: Make the actual container filesystem unwriteable ✍️🚫

```dockerfile
USER appuser
```

## Slide 100

# 🚢 Production & Final Conclusion

### The World is Your Container
---
- **Orchestration**: Graduate to **Kubernetes** for massive scale 🌩️
- **CI/CD**: Automate everything from commit to cloud 🤖
- **Observability**: Monitor with Prometheus and Grafana 📊
- **Welcome**: You are now a Docker Expert! 🎓🏆

*(Final 3D colorful icons: Stars, Rocket, and Checked Medal)*