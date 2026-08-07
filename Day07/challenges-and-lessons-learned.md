# Day 7 Challenges and Lessons Learned

This document captures the real issues encountered while learning Docker Compose and, more importantly, the reasoning behind each solution.

Rather than removing mistakes, they have been documented because diagnosing them provided a deeper understanding of how Docker Compose works.

---

# Challenge 1 — PostgreSQL container failed to start

## What happened

After creating the initial `docker-compose.yml` file and running:

```bash
docker compose up -d
```

only the Nginx service started successfully.

Running:

```bash
docker compose ps
```

showed the database container was missing.

Inspecting the logs:

```bash
docker compose logs db
```

returned an error explaining that PostgreSQL 18 expected a different data directory.

---

## Root Cause

The Compose file mounted the named volume to:

```yaml
/var/lib/postgresql/data
```

This path had worked previously during Day 4 when using `docker run`.

However, PostgreSQL 18 introduced a change to its expected storage layout.

Rather than silently accepting the older mount location, PostgreSQL detected the mismatch and refused to start to protect the database.

The issue wasn't caused by Docker Compose itself.

It was a version-specific change inside the official PostgreSQL image.

---

## Solution

Updated the Compose file to mount the volume at:

```yaml
/var/lib/postgresql
```

Then rebuilt the project:

```bash
docker compose down -v
docker compose up -d
```

Verified the fix using:

```bash
docker compose logs db
```

which confirmed:

```
database system is ready to accept connections
```

---

## Lesson Learned

Container images evolve over time.

Examples found online or from previous exercises may no longer match the latest image version.

When a service fails to start, checking its logs is often the fastest way to identify whether the problem is configuration-related or image-specific.

---

# Challenge 2 — Containers stopped after closing Ubuntu

## What happened

After reopening Ubuntu, attempting to connect to PostgreSQL returned:

```text
Error response from daemon:
container ... is not running
```

Initially this looked like the project had disappeared.

---

## Root Cause

Closing Ubuntu ended the WSL session.

Since Docker Engine was running inside WSL, the running containers stopped with it.

The Compose project itself was still intact.

The Compose file, named volume, and images all remained on disk.

Only the running containers had stopped.

---

## Solution

Restarted the project with:

```bash
docker compose up -d
```

Everything started again immediately.

The existing named volume was automatically reattached.

---

## Lesson Learned

Running containers are temporary.

The Compose configuration and persistent storage are not.

Restarting a project is often all that's required after a system restart.

---

# Challenge 3 — Running multiple commands together

## What happened

Attempted to execute several unrelated commands together:

```bash
docker exec -it compose-test-web-1 bash apt-get update && apt-get install -y iputils-ping ping db
```

The command became confusing and difficult to troubleshoot.

---

## Root Cause

Different commands belonged in different execution contexts.

Some should have run inside the container.

Others should have run after entering the shell.

Combining everything into one line made it difficult to determine which command failed.

---

## Solution

Executed each command individually:

```bash
docker exec -it compose-test-web-1 bash
```

```bash
apt-get update
```

```bash
apt-get install -y iputils-ping
```

```bash
ping db
```

---

## Lesson Learned

When learning new tools, simple commands are easier to understand and debug than long command chains.

Breaking work into small steps also makes mistakes much easier to isolate.

---

# Challenge 4 — Running Docker commands inside a container

## What happened

While inside the Nginx container shell, attempted to run:

```bash
docker exec -it compose-test-db-1 psql ...
```

which returned:

```text
bash: docker: command not found
```

---

## Root Cause

The command was executed inside the container rather than on the Ubuntu host.

Containers do not normally include the Docker CLI.

Docker commands are executed from the host operating system, not from inside another container.

---

## Solution

Exited the container:

```bash
exit
```

Then ran the Docker command from the Ubuntu terminal.

---

## Lesson Learned

There are two different environments during container work:

- The host machine, where Docker commands are executed.
- The container shell, where only software installed inside that container is available.

Understanding which environment you're currently working in prevents many confusing errors.

---

# Biggest Takeaway

Earlier in this learning journey, creating a multi-container application required several separate Docker commands.

Networks had to be created manually.

Containers had to be attached individually.

Each service had to be started one at a time.

Docker Compose changed that completely.

One YAML file now describes the application's services, networking, environment variables, ports, and storage.

One command starts the application.

One command stops it.

One file becomes the source of truth for the entire deployment.

More importantly, this day reinforced that Docker Compose is not introducing a different way of running containers.

It is automating the Docker concepts learned throughout the previous six days and combining them into a repeatable workflow that is easier to manage, reproduce, and share.