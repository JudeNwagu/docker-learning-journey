# Mistakes & Lessons Learned – Day 2

> **Learning Journey:** 7-Day Docker Sprint  
> **Day:** 2  
> **Topic:** Core Docker Commands & Container Lifecycle

---

# Overview

Mistakes are an inevitable part of learning any new technology. Rather than removing or hiding them, I documented the issues I encountered during today's Docker exercises, along with how I investigated and resolved them.

This document captures real terminal errors from working with Nginx, Redis, and PostgreSQL. Each issue strengthened my understanding of how Docker works and reinforced good troubleshooting habits.

---

# Challenge 1 — Running `docker run` Without an Image

## What Happened

I attempted to create an Nginx container but forgot to specify the image name.

```bash
docker run -d -p 8080:80 --name jcn-nginx
```

Docker returned:

```text
docker run requires at least 1 argument
```

## Root Cause

The `docker run` command always requires an image name. Docker had no image from which to create the container.

## Resolution

```bash
docker run -d -p 8080:80 --name jcn-nginx nginx
```

## Lesson Learned

Docker commands follow a defined syntax. Even when all required options are provided, the image name is still mandatory because it is the blueprint used to create the container.

---

# Challenge 2 — Reusing a Container Name

## What Happened

I tried creating a second Nginx container using the same name.

```bash
docker run -d -p 8081:80 --name jcn-nginx nginx
```

Docker responded with a name conflict.

## Root Cause

Container names must be unique on a Docker host.

## Resolution

Assigned a different name.

```bash
docker run -d -p 8081:80 --name jcn1-nginx nginx
```

## Lesson Learned

One image can create many containers, but every container must have its own unique identifier.

---

# Challenge  3 — Typographical Error

## What Happened

I accidentally typed:

```bash
docker log jcn-nginx
```

instead of

```bash
docker logs jcn-nginx
```

## Root Cause

Simple typing mistake.

## Resolution

Corrected the command.

## Lesson Learned

Docker's CLI is strict. Small spelling mistakes produce command errors immediately.

Reading the error message carefully often reveals the problem.

---

# Challenge 4 — Referencing a Container That Didn't Exist

## What Happened

While stopping containers I typed an incorrect container name.

```bash
docker stop jcn-nginx jcn1-nginx-2
```

Docker reported:

```text
No such container
```

## Root Cause

The container name didn't match any existing container.

## Resolution

Verified running containers.

```bash
docker ps
```

Then used the correct name.

## Lesson Learned

Always verify container names before issuing management commands.

---

# Challenge  5 — Forgetting the `run` Subcommand

## What Happened

While starting Redis I entered:

```bash
docker -d -p 6379:6379 --name jcn-redis redis
```

Docker interpreted `-d` as an invalid Docker command.

## Root Cause

The `run` subcommand was omitted.

## Resolution

```bash
docker run -d -p 6379:6379 --name jcn-redis redis
```

## Lesson Learned

Docker commands follow this structure:

```text
docker
   │
subcommand
   │
options
   │
arguments
```

Missing the subcommand causes Docker to interpret the remaining options incorrectly.

---

# Challenge 6 — Running Docker Commands Inside Redis CLI

## What Happened

While inside the Redis CLI I accidentally entered another Docker command.

```text
127.0.0.1:6379>
```

I typed:

```bash
docker run ...
```

Redis responded:

```text
ERR unknown command 'docker'
```

## Root Cause

I was no longer inside Bash.

I was interacting directly with the Redis server.

## Resolution

Exited Redis.

```text
exit
```

Returned to Bash before running Docker commands.

## Lesson Learned

Interactive applications create their own command environment.

Once inside tools like:

- redis-cli
- psql

Docker commands no longer execute until returning to the host shell.

---

# Challenge 7 — Stopping the Wrong Container

## What Happened

After recreating the Redis container under a different name, I attempted to stop the original container.

```bash
docker stop jcn-redis
```

Docker reported the container no longer existed.

## Root Cause

The original container had already been removed.

A new container with a different name was running.

## Resolution

Stopped the correct container.

```bash
docker stop my-redis
```

## Lesson Learned

Container names are tied to individual container instances.

Removing a container permanently removes that identity.

---

# Challenge  8 — Multi-line Command Failure

## What Happened

While creating the PostgreSQL container I copied a multi-line command using backslashes.

Instead of executing as one command, Bash treated each line separately.

This produced several errors including:

```text
docker run requires at least 1 argument

command not found

postgres: command not found
```

## Root Cause

A trailing space after one of the backslashes interrupted Bash's line continuation.

## Resolution

Executed the command as one continuous line.

```bash
docker run -d -p 5432:5432 --name my-postgres -e POSTGRES_PASSWORD=mysecretpassword -e POSTGRES_USER=judoski -e POSTGRES_DB=learning_db postgres
```

## Lesson Learned

Multi-line commands improve readability but are sensitive to formatting.

When copying commands from websites or documentation, using a single-line command can be more reliable.

---

# Challenge 9 — Attempting to Remove a Running Container

## What Happened

I attempted to remove PostgreSQL while it was still running.

```bash
docker rm my-postgres
```

Docker refused.

## Root Cause

Docker protects running containers from accidental deletion.

## Resolution

Stopped the container first.

```bash
docker stop my-postgres

docker rm my-postgres
```

## Lesson Learned

The standard lifecycle is:

```text
Running Container
       │
docker stop
       │
Stopped Container
       │
docker rm
       │
Deleted Container
```

Following this sequence avoids unnecessary errors.

---

# Overall Lessons from Day 2

Today's mistakes reinforced several important Docker concepts:

- Docker images are immutable blueprints used to create containers.
- Containers require unique names when explicitly assigned.
- Docker commands must be executed from the host shell.
- Interactive application shells such as `redis-cli` and `psql` are separate environments.
- Container storage is temporary unless persistent storage is configured.
- Environment variables configure application behavior, while published ports control network access.
- Docker's error messages are often descriptive enough to identify the underlying problem without extensive troubleshooting.

---

# Reflection

Although I expected Day 2 to be about memorizing Docker commands, it became an exercise in understanding how Docker behaves under different conditions.

Every mistake exposed an underlying Docker concept that would have been easy to overlook if everything had worked perfectly on the first attempt.

Rather than treating errors as setbacks, I used them as opportunities to investigate Docker's behavior, understand the root cause, and document the solution for future reference.

This approach has given me greater confidence in troubleshooting containers independently instead of relying solely on tutorials.

---

# Best Practices Moving Forward

- Verify container names with `docker ps`.
- Read Docker error messages before retrying commands.
- Use meaningful container names.
- Stop containers before removing them.
- Be cautious when copying multi-line commands.
- Exit interactive shells before returning to Docker commands.
- Document both successful workflows and troubleshooting steps.