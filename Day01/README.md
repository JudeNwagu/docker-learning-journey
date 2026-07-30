# Day 1 – Docker Foundations & Environment Setup

> **Learning Journey:** 7-Day Docker Sprint  
> **Course:** TechWorld with Nana – Docker Full Course  
> **Day:** 1  
> **Status:** ✅ Completed

---

# Overview

Day 1 focused on understanding the fundamentals of Docker and setting up a fully functional Docker development environment on Windows 11 using the WSL2 backend.

Beyond installing Docker Desktop, I explored the motivation behind Docker, the concept of containers, how Docker differs from Virtual Machines, and why containers have become a foundational technology in modern software development and Data Engineering.

The practical component involved configuring Docker Desktop, enabling hardware virtualization, verifying Docker Engine, and successfully running my first Docker container.

---

# Objectives

By the end of Day 1, I aimed to:

- Understand what Docker is and why it exists.
- Explain the "Works on my machine" problem.
- Understand Containers.
- Compare Docker Containers with Virtual Machines.
- Understand Docker Images vs Docker Containers.
- Install Docker Desktop.
- Configure Docker with the WSL2 backend.
- Verify Docker installation.
- Run my first Docker container.

---

# Why Docker?

One of the biggest challenges in software development is ensuring that applications behave consistently across different environments.

A developer's application may work perfectly on their local machine but fail when deployed because another environment has different:

- Operating System
- Runtime
- Library versions
- Dependencies
- Environment Variables

This challenge is commonly known as:

> **"Works on my machine."**

Docker solves this problem by packaging an application together with everything it needs to run inside a **Container**, ensuring the application behaves consistently regardless of where it is deployed.

---

# Core Concepts Learned

## What is Docker?

Docker is an open-source containerization platform that allows developers to package applications and their dependencies into lightweight, portable containers.

Rather than depending on software already installed on a computer, Docker packages everything an application needs into a standardized unit that can run consistently across development, testing, and production environments.

---

## What is a Container?

A container is a lightweight, isolated runtime environment that contains:

- Application code
- Runtime
- System libraries
- Dependencies
- Configuration

Unlike Virtual Machines, containers do **not** include their own operating system kernel.

Instead, they share the host operating system's kernel while maintaining isolated processes, filesystems, and networking.

This makes containers significantly faster and more resource-efficient.

---

## Docker vs Virtual Machines

| Docker Containers | Virtual Machines |
|-------------------|------------------|
| Share Host OS Kernel | Each VM has its own Guest OS |
| Lightweight | Heavyweight |
| Starts in seconds | Takes longer to boot |
| Uses less RAM and storage | Higher resource consumption |
| Ideal for application deployment | Ideal for complete OS isolation |

### Key Difference

Virtual Machines virtualize **hardware**.

Docker Containers virtualize the **operating system**.

---

## Docker Image vs Docker Container

### Docker Image

A Docker Image is an immutable blueprint containing everything required to run an application.

Think of it as a recipe.

---

### Docker Container

A Docker Container is a running instance of an image.

Think of it as the prepared meal created from the recipe.

One image can create multiple independent containers.

---

# Before Containers vs After Containers

## Before Containers

- Environment inconsistencies
- Manual server configuration
- Missing dependencies
- Configuration drift
- Slow deployments

## After Containers

- Consistent environments
- Portable deployments
- Simplified infrastructure
- Faster deployments
- Easier scaling

---

# Environment Setup

| Component | Details |
|-----------|---------|
| Operating System | Windows 11 Pro (21H2) |
| Processor | Intel Core i3-6100U |
| Memory | 8 GB RAM |
| Docker Desktop | Installed |
| Docker Backend | WSL2 |
| Linux Distribution | Ubuntu 24.04 |
| WSL Version | 2 |

---

# Hands-on Practice

During today's practical session, I successfully:

- Installed Docker Desktop.
- Enabled Intel Virtualization Technology (VT-x) in BIOS.
- Installed WSL2.
- Configured Docker Desktop to use the WSL2 backend.
- Verified Docker Engine.
- Connected Docker CLI to Docker Desktop.
- Pulled my first Docker Image.
- Created my first Docker Container.
- Successfully executed the `hello-world` container.

---

# Commands Practiced

The following commands were used during Day 1.

```powershell
wsl --status

docker version

docker info

docker run hello-world

docker images

docker ps

docker ps -a
```

Detailed explanations of every command can be found in **commands.md**.

---

# Challenges Encountered

The installation process included several real-world challenges that required troubleshooting.

### Challenge 1

Hardware virtualization was disabled in BIOS, preventing WSL2 from starting.

**Resolution**

Enabled Intel VT-x in BIOS.

---

### Challenge 2

Secure Boot requested authorization after BIOS configuration changes.

**Resolution**

Completed firmware authorization using the provided verification code.

---

### Challenge 3

PowerShell blocked profile script execution because of the default execution policy.

**Resolution**

Verified the issue was unrelated to Docker and confirmed WSL functionality independently.

---

### Challenge 4

Docker CLI initially failed to connect because Docker Desktop had not fully initialized.

**Resolution**

Waited for Docker Engine to finish starting before rerunning Docker commands.

---

A detailed troubleshooting log is available in **installation-log.md**.

---

# Verification

Docker installation was successfully verified using:

```powershell
docker run hello-world
```

Successful execution confirmed:

- Docker Client installed
- Docker Engine running
- Docker Hub connectivity
- Image download
- Container creation
- Container execution

---

# Screenshots

The following screenshots are included in the **screenshots/** directory:

- Docker Desktop General Settings
- WSL2 Status
- Docker CLI Verification
- Docker Engine Information
- Hello World Container Execution

---

# Reflection

Day 1 changed my understanding of Docker.

Before this lesson, I viewed Docker as a tool for running applications. After completing the theory and practical setup, I now understand that Docker is fundamentally about creating portable and consistent execution environments through containers.

Troubleshooting the installation also helped me understand the technologies working beneath Docker, including BIOS virtualization, WSL2, Docker Engine, and the relationship between the Docker Client and Docker Daemon.

Rather than simply following installation steps, I gained a better understanding of how these components interact to provide a reliable container platform.

---

# Why This Matters for Data Engineering

Docker is a foundational technology in modern Data Engineering.

Many industry-standard tools are distributed as Docker containers, including:

- Apache Airflow
- Apache Kafka
- PostgreSQL
- Redis
- Spark
- JupyterLab
- MinIO

Understanding Docker provides the foundation for building reproducible, portable, and production-ready data platforms.

---

# Key Takeaways

- Docker solves environment inconsistency.
- Containers share the host operating system kernel.
- Images are immutable blueprints.
- Containers are running instances of images.
- Docker Desktop uses WSL2 on Windows.
- Hardware virtualization is required for WSL2.
- Real-world troubleshooting is an essential part of the installation process.

---

# Resources

- TechWorld with Nana – Docker Full Course
- Docker Official Documentation
- Docker Hub

---

# Next Steps

Day 2 will focus on:

- Docker Images
- Docker Registries
- Docker Hub
- Pulling Images
- Image Lifecycle