# Day 3 — Commands Reference

This document contains the Docker commands used while exploring Docker images.

Unlike Day 2, which focused on the container lifecycle, Day 3 focused entirely on understanding images, image layers, metadata, and tags.

---

# 1. List Local Images

```bash
docker images
```

**Purpose**

Displays every Docker image currently stored on the local machine.

Useful information includes:

- Repository
- Tag
- Image ID
- Creation date
- Image size

---

# 2. Pull a Specific Image Tag

### Nginx Alpine

```bash
docker pull nginx:alpine
```

### PostgreSQL Alpine

```bash
docker pull postgres:alpine
```

**Purpose**

Downloads a specific tagged version of an image instead of the default `latest` tag.

This allowed comparison between:

- `latest`
- `alpine`

---

# 3. Compare Image Sizes

```bash
docker images
```

**Purpose**

Compare image sizes after downloading multiple tags.

Example observations:

| Image | Latest | Alpine |
|--------|--------:|--------:|
| Nginx | 238 MB | 92.7 MB |
| PostgreSQL | 644 MB | 433 MB |

---

# 4. View Image Layer History

### Nginx

```bash
docker history nginx
```

### PostgreSQL

```bash
docker history postgres
```

**Purpose**

Displays every layer that makes up an image.

Useful for identifying:

- Base operating system layer
- Application installation layer
- Metadata-only layers
- Layer sizes

---

# 5. Inspect Image Metadata

### Nginx

```bash
docker inspect nginx
```

### PostgreSQL

```bash
docker inspect postgres
```

**Purpose**

Displays detailed image metadata including:

- Image ID
- Architecture
- Default command (`Cmd`)
- Entrypoint
- Exposed ports
- Volumes
- Environment variables

---

# 6. Create a New Image Tag

### Initial Practice

```bash
docker tag nginx:latest 5a88c9c45479/my-nginx:v1
```

This worked locally but used an image ID where a Docker Hub username should have been used.

---

### Correct Practice

```bash
docker tag postgres:latest judoski/my-postgres:v1
```

**Purpose**

Creates another tag pointing to the same image.

No duplicate image is created.

---

# 7. Verify Image Tags

```bash
docker images
```

**Purpose**

Confirm multiple tags point to the same IMAGE ID.

Example:

```text
postgres                latest
judoski/my-postgres     v1
```

Both share the same image ID.

---

# 8. Remove Image Tags

### Remove Multiple Tags

```bash
docker rmi nginx:latest 5a88c9c45479/my-nginx:v1
```

---

### Remove a Single Tag

```bash
docker rmi postgres:alpine
```

**Purpose**

Removes image tags.

Docker deletes the underlying image only when no remaining tags reference it.

---

# Commands Practiced

| Command | Purpose |
|----------|---------|
| `docker images` | List locally stored images |
| `docker pull` | Download an image |
| `docker history` | Inspect image layers |
| `docker inspect` | View image metadata |
| `docker tag` | Create another tag for an existing image |
| `docker rmi` | Remove image tags or images |

---

# Key Command Takeaways

- `docker images` shows what exists locally.
- `docker pull` downloads a specific image or tag.
- `docker history` explains where an image's size comes from.
- `docker inspect` reveals how an image is configured before it ever becomes a container.
- `docker tag` creates another reference, not another copy.
- `docker rmi` removes references first and image data only when the final reference disappears.