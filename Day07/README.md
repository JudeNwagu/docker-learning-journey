# Day 7 — Docker Compose

## Objective

Today's objective was to learn how Docker Compose simplifies the deployment of multi-container applications.

Until now, every container had been created manually using separate `docker run` commands. While this works for small experiments, it quickly becomes repetitive when an application requires multiple services that need to communicate with one another.

The goal was to replace that manual process with a single `docker-compose.yml` file and verify two important concepts through hands-on testing:

- automatic container-to-container networking
- persistent storage across complete application teardown and rebuild

This day was intentionally added before building a larger project because understanding Docker Compose makes managing multi-container applications significantly easier.

---

## Environment

- Ubuntu 24.04 (WSL2)
- Docker Engine running directly inside Ubuntu
- Docker Compose V2

During this session, Docker Desktop was removed from Windows to free disk space. Docker Engine was installed directly inside Ubuntu and verified with:

```bash
docker --version
docker compose version
```

This change had no impact on previous work because Docker images, containers and volumes are managed by Docker Engine rather than Docker Desktop itself.

---

# What I Learned

## Docker Compose replaces repetitive Docker commands

Instead of manually creating containers one after another, Compose allows the entire application to be described inside a single YAML file.

Rather than writing commands such as:

```bash
docker network create my-network

docker run ...

docker run ...

docker run ...
```

everything can now be started using:

```bash
docker compose up -d
```

and removed using:

```bash
docker compose down
```

This makes multi-container environments much easier to recreate consistently.

---

## Compose automatically creates networks

One of the biggest discoveries today was that Compose creates a dedicated network automatically.

Every service inside the Compose project can communicate using only its service name.

For example:

```
web
```

can communicate directly with

```
db
```

without ever creating a network manually.

Running:

```bash
ping db
```

from inside the Nginx container confirmed successful communication with the PostgreSQL container.

---

## Named volumes survive container removal

A PostgreSQL database was attached to a named volume.

After creating a table and inserting data:

```sql
CREATE TABLE compose_test (...);
INSERT ...
```

the application was completely stopped using:

```bash
docker compose down
```

After rebuilding the application:

```bash
docker compose up -d
```

all previously inserted records were still present.

This confirmed that named volumes persist independently from containers.

---

## Diagnosing a PostgreSQL startup failure

The PostgreSQL service initially refused to start.

Using:

```bash
docker compose logs db
```

revealed that PostgreSQL 18 expects a different volume mount location than earlier versions.

Changing the Compose file from:

```yaml
/var/lib/postgresql/data
```

to

```yaml
/var/lib/postgresql
```

resolved the issue immediately.

This reinforced the importance of reading container logs rather than guessing the cause of failures.

---

## Containers stopped after closing Ubuntu

After reopening Ubuntu, Docker reported:

```
container is not running
```

This happened because the previous WSL session had ended.

Restarting the project required only:

```bash
docker compose up -d
```

No data was lost because the named volume still existed.

---

## Simplifying the networking experiment

To isolate the networking concept, I built a second Compose project using two Nginx containers only.

Both containers communicated successfully using service names without any manual networking configuration.

This confirmed that automatic networking is a built-in feature of Docker Compose rather than something specific to PostgreSQL.

---

# Key Takeaways

- Docker Compose manages multiple containers from a single configuration file.
- Compose automatically creates an isolated network for every project.
- Services communicate using service names instead of IP addresses.
- Named volumes preserve data even after containers are removed.
- Container logs are often the fastest way to diagnose startup problems.
- Docker Engine can run directly inside Ubuntu without Docker Desktop.

---

## Skills Demonstrated

- Writing Docker Compose files
- Managing multi-container applications
- Automatic Docker networking
- Persistent storage using named volumes
- Diagnosing container startup failures
- Reading Docker Compose logs
- Verifying service communication
- Managing Compose application lifecycle

---

## Repository Structure

```
Day07/
│
├── README.md
├── commands.md
├── challenges-and-lessons-learned.md
├── screenshots/
├── files/
└── output/
```

---

## Resources

- TechWorld with Nana – Docker Compose
- Docker Documentation – Docker Compose Concepts
- Docker Documentation – Compose Specification