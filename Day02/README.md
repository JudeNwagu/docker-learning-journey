# Day 2 – Core Docker Commands & Container Lifecycle

> **Learning Journey:** 7-Day Docker Sprint  
> **Course:** TechWorld with Nana – Docker Full Course  
> **Day:** 2  
> **Status:** ✅ Completed

---

# Overview

Day 2 focused on mastering Docker's core command-line interface (CLI) by working through the complete container lifecycle using three real-world applications:

- **Nginx** – A lightweight web server
- **Redis** – An in-memory key-value database
- **PostgreSQL** – A relational database management system

Rather than stopping at basic commands, I intentionally explored how containers behave throughout their lifecycle—from downloading an image, creating and interacting with containers, inspecting logs, stopping services, and cleaning up resources.

Throughout the exercises, I encountered several real-world mistakes, investigated their causes, and documented how they were resolved. These troubleshooting experiences became one of the most valuable parts of the day's learning.

---

# Objectives

By the end of Day 2, I aimed to:

- Understand Docker's core CLI commands.
- Learn the complete Docker container lifecycle.
- Pull images from Docker Hub.
- Create and run containers.
- Interact with running containers.
- Inspect logs.
- Stop and restart containers.
- Remove containers and images.
- Understand ephemeral container storage.
- Learn the difference between application configuration and container networking.

---

# Learning Environment

| Component | Details |
|-----------|---------|
| Operating System | Windows 11 Pro |
| Linux Environment | Ubuntu 24.04 (WSL2) |
| Docker Backend | WSL2 |
| Terminal | Ubuntu Bash |
| Docker Engine | Running |

---

# The Docker Container Lifecycle

One of the biggest lessons from Day 2 was understanding that a container follows a predictable lifecycle.

```text
Docker Hub
      │
      ▼
docker pull
      │
      ▼
Docker Image
      │
docker run
      │
      ▼
Running Container
      │
 ┌────┼─────────────┐
 │    │             │
 │ docker logs      │
 │ docker exec      │
 │ docker ps        │
 │                  │
 └──────────┬───────┘
            │
      docker stop
            │
            ▼
   Stopped Container
            │
      docker rm
            │
            ▼
Container Deleted

(Optional)

docker rmi
      │
      ▼
Image Deleted
```

Understanding this lifecycle helped connect individual Docker commands into one complete workflow rather than viewing them as isolated commands.

---

# Hands-on Practice

## Practice 1 – Nginx

The first exercise introduced Docker using the official **Nginx** image.

### Activities completed

- Pulled the Nginx image
- Started a web server container
- Published container ports
- Accessed Nginx from the browser
- Viewed container logs
- Opened an interactive shell inside the container
- Ran multiple containers from a single image
- Stopped and removed containers

### Key Learning

One Docker image can create multiple independent containers.

Each container behaves like its own isolated application, but Docker requires every container to have a unique name.

---

## Practice 2 – Redis

The second exercise focused on Redis, allowing me to interact directly with software running inside a container.

### Activities completed

- Pulled the Redis image
- Started Redis in detached mode
- Connected using `redis-cli`
- Stored key-value pairs
- Retrieved data
- Deleted keys
- Verified data loss after container removal

### Key Learning

Redis demonstrated that application data stored inside a container is temporary by default.

Removing the container also removed the data stored within it, reinforcing the concept of ephemeral container storage.

Another important lesson was understanding the difference between the host shell (Bash) and an application's own interactive shell (`redis-cli`).

---

## Practice 3 – PostgreSQL

The final exercise introduced PostgreSQL.

Unlike Redis and Nginx, PostgreSQL requires startup configuration before it can run successfully.

### Activities completed

- Pulled the PostgreSQL image
- Started a PostgreSQL container
- Configured environment variables
- Connected using `psql`
- Created a database table
- Inserted records
- Queried data
- Verified data loss after container recreation

### Key Learning

This exercise introduced two important Docker concepts:

### Environment Variables

Environment variables configure how an application behaves when the container starts.

Example:

```text
POSTGRES_PASSWORD
POSTGRES_USER
POSTGRES_DB
```

These configure PostgreSQL itself.

---

### Port Mapping

Port mapping controls how traffic reaches a container.

Example:

```text
Host Port 5432
        │
        ▼
Container Port 5432
```

Unlike environment variables, port mappings affect networking rather than application configuration.

Understanding the distinction between these two concepts is fundamental when working with containers.

---

# Real-World Troubleshooting

Today's session included several realistic mistakes that improved my understanding of Docker.

Examples included:

- Forgetting to specify an image name.
- Reusing container names.
- Typographical command errors.
- Executing Docker commands inside `redis-cli`.
- PostgreSQL startup failures due to missing environment variables.
- Bash line-continuation issues.
- Attempting to remove running containers.

Rather than deleting these mistakes, I documented each one together with its root cause and resolution.

A complete breakdown is available in **mistakes-and-lessons.md**.

---

# Commands Practiced

During today's exercises I worked extensively with Docker's core command set.

Topics included:

### Image Management

- `docker pull`
- `docker images`
- `docker rmi`

### Container Management

- `docker run`
- `docker ps`
- `docker logs`
- `docker exec`
- `docker stop`
- `docker start`
- `docker rm`

Detailed explanations are available in **commands.md**.

---

# Screenshots

The following screenshots are included in the **screenshots/** folder:

- Pulling Docker images
- Running Nginx
- Browser displaying the Nginx welcome page
- Redis CLI interaction
- PostgreSQL database creation
- Docker container listings
- Container logs
- Cleanup verification

---

# Reflection

Day 2 transformed Docker from something I could install into something I could confidently operate.

Instead of memorizing commands, I learned how containers behave throughout their lifecycle—from creation to removal—and how Docker manages images, networking, and application processes behind the scenes.

The mistakes I encountered were equally valuable. Each error forced me to investigate Docker's behavior, making the concepts easier to understand and remember than simply following a tutorial.

Working directly with Nginx, Redis, and PostgreSQL also demonstrated that Docker is application-agnostic: the workflow remains largely the same regardless of what software is running inside the container.

---

# Key Takeaways

- Docker images are reusable blueprints.
- Multiple containers can be created from a single image.
- Every running container requires a unique name.
- Containers are isolated from one another.
- Container storage is ephemeral by default.
- Environment variables configure applications.
- Port mappings expose container services.
- Interactive tools such as `redis-cli` and `psql` run inside containers.
- Docker commands operate from the host shell, not from within application CLIs.
- Troubleshooting is an essential part of learning container technology.

---

# Additional Documentation

- 📘 **commands.md** – Complete reference for every Docker command used during Day 2.
- ⚒️ **challenges-and-lessons.md** – Detailed troubleshooting journal documenting every issue encountered and how it was resolved.

---

# Next Steps

Day 3 will focus on understanding Docker Images in greater depth, including image layers, tagging, registries, and how Docker builds and stores images efficiently.
