# Docker Commands Reference – Day 1

> This document contains every Docker and WSL command used during Day 1 of my Docker learning journey, along with explanations of what each command does and when to use it.

---

# 1. Check WSL Status

## Command

```powershell
wsl --status
```

## Purpose

Displays the current Windows Subsystem for Linux (WSL) configuration.

This command confirms:

- The default Linux distribution
- WSL version
- Whether Docker can use the WSL2 backend

## Why I Used It

Before running Docker Desktop, I needed to verify that WSL2 was correctly installed and configured.

## Expected Output

```text
Default Distribution: Ubuntu-24.04
Default Version: 2
```

---

# 2. Display Docker Version

## Command

```powershell
docker version
```

## Purpose

Displays version information for both:

- Docker Client
- Docker Server (Engine)

## Why I Used It

To verify Docker Desktop was installed correctly and that both the Docker CLI and Docker Engine were communicating successfully.

## Expected Output

```text
Client:
 Version: xx.xx.xx

Server:
 Engine:
 Version: xx.xx.xx
```

---

# 3. Display Docker System Information

## Command

```powershell
docker info
```

## Purpose

Displays detailed information about the Docker environment.

Information includes:

- Docker Engine
- Storage Driver
- CPU
- Memory
- Running Containers
- Images
- Plugins
- Operating System
- Kernel Version

## Why I Used It

To confirm Docker Engine was running properly after installation.

It also helped diagnose an initial connection error while Docker Desktop was still starting.

---

# 4. Run the Hello World Container

## Command

```powershell
docker run hello-world
```

## Purpose

Runs Docker's official Hello World container.

If the image does not already exist locally, Docker automatically:

1. Searches Docker Hub
2. Downloads the image
3. Creates a container
4. Starts the container
5. Displays a success message

## Why I Used It

To verify the complete Docker workflow:

- Docker Client
- Docker Daemon
- Docker Hub
- Image Pull
- Container Creation
- Container Execution

## Expected Output

```text
Hello from Docker!

This message shows that your installation appears to be working correctly.
```

---

# 5. List Local Docker Images

## Command

```powershell
docker images
```

## Purpose

Displays all Docker Images stored locally.

Typical information includes:

- Repository
- Tag
- Image ID
- Creation Date
- Size

## Why I Used It

To verify that the **hello-world** image had been successfully downloaded.

Example:

```text
REPOSITORY     TAG       IMAGE ID
hello-world    latest    xxxxxxxxx
```

---

# 6. List Running Containers

## Command

```powershell
docker ps
```

## Purpose

Shows all currently running Docker containers.

By default, stopped containers are not displayed.

## Why I Used It

To check whether any containers were actively running.

Since **hello-world** exits immediately after execution, no running containers were displayed.

---

# 7. List All Containers

## Command

```powershell
docker ps -a
```

## Purpose

Displays all Docker containers, including stopped containers.

## Why I Used It

To verify that the **hello-world** container had been created successfully.

Example:

```text
CONTAINER ID
IMAGE
STATUS
NAMES
```

---

# Optional Installation Command

During installation I also used the following command.

## Install WSL (without installing a Linux distribution)

```powershell
wsl --install --no-distribution
```

## Purpose

Installs the Windows Subsystem for Linux platform components required for Docker Desktop.

## Why I Used It

Docker Desktop uses WSL2 as its Linux backend on Windows.

This command prepared Windows for Docker before installing Ubuntu.

---

# Optional PowerShell Command

## Allow Local PowerShell Scripts

```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```

## Purpose

Allows locally created PowerShell scripts to run while still protecting downloaded scripts.

## Why I Used It

PowerShell initially blocked my profile script because the default execution policy was **Restricted**.

This change was optional and unrelated to Docker itself.

---

# Day 1 Command Summary

| Command | Description |
|----------|-------------|
| `wsl --status` | Check WSL configuration |
| `docker version` | Display Docker Client and Engine versions |
| `docker info` | Display detailed Docker environment information |
| `docker run hello-world` | Run the first Docker container |
| `docker images` | List downloaded Docker images |
| `docker ps` | List running containers |
| `docker ps -a` | List all containers |
| `wsl --install --no-distribution` | Install WSL platform |
| `Set-ExecutionPolicy RemoteSigned -Scope CurrentUser` | Update PowerShell execution policy |

---

# Key Learning

One of the biggest lessons from Day 1 was that Docker commands interact with different parts of the Docker ecosystem.

For example:

- `docker version` verifies the Docker Client and Server.
- `docker info` checks the health of the Docker Engine.
- `docker run` demonstrates the complete lifecycle of pulling an image, creating a container, and running an application.
- `docker ps` and `docker images` allow you to inspect the current Docker environment.

Understanding the purpose behind each command is just as important as knowing the command itself.