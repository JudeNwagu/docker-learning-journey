# Day 4 — Commands Reference

This document contains the Docker commands used while learning how Docker volumes solve the problem of container data loss.

The focus of this day was understanding the two most common persistence mechanisms:

- Named Volumes
- Bind Mounts

The exercises used PostgreSQL to demonstrate persistent database storage and Nginx to demonstrate live file sharing between the host and a running container.

---

# 1. Create a Named Volume

```bash
docker volume create pgdata
```

**Purpose**

Creates a Docker-managed named volume called `pgdata`.

Unlike a container, a volume exists independently and remains available until it is explicitly removed.

---

# 2. List Existing Volumes

```bash
docker volume ls
```

**Purpose**

Displays all Docker volumes currently stored on the system.

Useful for confirming that a newly created volume exists.

---

# 3. Inspect a Volume

```bash
docker volume inspect pgdata
```

**Purpose**

Displays detailed metadata about the volume, including:

- Volume name
- Driver
- Mountpoint (physical storage location)
- Labels
- Creation information

This command helps explain where Docker actually stores persistent data.

---

# 4. Run PostgreSQL with a Named Volume

```bash
docker run -d \
-p 5432:5432 \
--name my-postgres \
-e POSTGRES_PASSWORD=mysecretpassword \
-e POSTGRES_USER=judoski \
-e POSTGRES_DB=learning_db \
-v pgdata:/var/lib/postgresql/data \
postgres
```

**Purpose**

Starts a PostgreSQL container while attaching the named volume `pgdata` to PostgreSQL's data directory.

This allows the database files to remain even if the container is removed.

---

# 5. Connect to PostgreSQL

```bash
docker exec -it my-postgres psql -U judoski -d learning_db
```

**Purpose**

Launches PostgreSQL's interactive command-line client (`psql`) inside the running container.

Used to verify that database records persisted after recreating the container.

---

# 6. Create a Host Directory

```bash
mkdir ~/nginx-content
```

**Purpose**

Creates a directory on the host machine that will later be shared with a Docker container.

---

# 7. Create an HTML File

```bash
echo "<h1>Hello from my bind mount</h1>" > ~/nginx-content/index.html
```

**Purpose**

Creates a simple HTML page inside the host directory.

This file becomes the website served by Nginx after the bind mount is attached.

---

# 8. Run Nginx Using a Bind Mount

```bash
docker run -d \
-p 8080:80 \
--name my-nginx \
-v ~/nginx-content:/usr/share/nginx/html \
nginx
```

**Purpose**

Mounts the local `nginx-content` folder directly into Nginx's default web directory.

Instead of serving its default page, Nginx now serves files directly from the host machine.

---

# 9. Update the HTML File

```bash
echo '<h1>Updated live!</h1>' > ~/nginx-content/index.html
```

**Purpose**

Updates the HTML page stored on the host.

Refreshing the browser immediately displays the new content without restarting the container.

This demonstrates one of the main advantages of bind mounts during development.

---

# 10. Remove a Named Volume

```bash
docker volume rm pgdata
```

**Purpose**

Permanently deletes the named volume.

This also removes all data stored inside that volume.

Docker prevents deletion if the volume is still attached to an active container.

---

# Commands Practiced

| Command | Purpose |
|----------|---------|
| `docker volume create` | Create a Docker-managed volume |
| `docker volume ls` | List all existing volumes |
| `docker volume inspect` | View detailed volume information |
| `docker volume rm` | Delete a named volume |
| `docker run -v volume:path` | Attach a named volume to a container |
| `docker run -v host:path` | Create a bind mount between the host and a container |
| `docker exec` | Access PostgreSQL inside the running container |
| `mkdir` | Create a directory on the host machine |
| `echo` | Create or update files used during the bind mount demonstration |

---

# Key Command Takeaways

- Named volumes are managed entirely by Docker and are the preferred option for persistent application data.
- Bind mounts map an existing host directory directly into a container.
- `docker volume inspect` reveals the physical location where Docker stores persistent data.
- PostgreSQL stores its database files inside the attached volume rather than inside the container.
- Bind-mounted files update immediately because the container reads directly from the host filesystem.
- Volumes and bind mounts solve different problems and should be selected based on the application's requirements.