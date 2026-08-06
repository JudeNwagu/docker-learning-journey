# Day 6 — Commands Reference

This document contains the commands used while learning how to build Docker images from scratch using a Dockerfile.

The exercises focused on understanding Dockerfile instructions, building custom images, exploring the relationship between `CMD` and `ENTRYPOINT`, inspecting image layers, and troubleshooting common build mistakes.

---

# 1. Create a Project Directory

```bash
mkdir ~/entrypoint-test
cd ~/entrypoint-test
```

## Purpose

Creates a dedicated working directory for the first Dockerfile experiment.

Keeping each project in its own folder makes Docker builds easier because the current directory becomes the build context.

---

# 2. Create a Dockerfile

```bash
nano Dockerfile
```

## Purpose

Creates a new Dockerfile that defines how Docker should build the image.

Docker automatically looks for a file named:

```text
Dockerfile
```

when running `docker build`.

---

# 3. Build the Docker Image

```bash
docker build -t entrypoint-test .
```

## Purpose

Builds a Docker image from the Dockerfile in the current directory.

### Command Breakdown

| Part | Purpose |
|------|---------|
| `docker build` | Starts the image build process |
| `-t` | Assigns a human-readable image name |
| `entrypoint-test` | Image name |
| `.` | Current directory (build context) |

---

# 4. Run the Image

```bash
docker run entrypoint-test
```

## Purpose

Creates and starts a container from the image using the default command defined by the Dockerfile.

Output:

```text
Hello from default CMD
```

This demonstrated how Docker combines `ENTRYPOINT` and `CMD`.

---

# 5. Override the Default CMD

```bash
docker run entrypoint-test "This is my own custom message"
```

## Purpose

Supplies a new argument at runtime.

The `ENTRYPOINT` remained unchanged while only the `CMD` portion was replaced.

This proved the practical difference between the two instructions.

---

# 6. List Existing Containers

```bash
docker ps -a
```

## Purpose

Displays every container on the system, including stopped containers.

This was used to identify containers still referencing the image before attempting cleanup.

---

# 7. Remove Containers

```bash
docker rm <container-id-1> <container-id-2>
```

## Purpose

Deletes stopped containers.

Docker cannot remove an image while containers still reference it.

---

# 8. Remove the Image

```bash
docker rmi entrypoint-test
```

## Purpose

Deletes the Docker image once no containers depend on it.

This reinforced the cleanup rule first encountered on Day 2.

---

# 9. Create Another Project Directory

```bash
mkdir ~/my-first-image
cd ~/my-first-image
```

## Purpose

Creates a new project folder for building a custom Nginx image.

---

# 10. Create the HTML File

```bash
echo '<h1>Built by Judoski, from a real Dockerfile</h1>' > index.html
```

## Purpose

Creates a simple web page that will replace Nginx's default homepage.

This file becomes part of the Docker image during the build process.

---

# 11. Create the Dockerfile

```bash
nano Dockerfile
```

Dockerfile contents:

```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
```

## Purpose

Defines the instructions Docker follows while building the custom image.

Instruction summary:

| Instruction | Purpose |
|-------------|---------|
| `FROM` | Select base image |
| `COPY` | Copy local file into image |
| `EXPOSE` | Document expected application port |

---

# 12. Build the Custom Image

```bash
docker build -t judoski/my-first-image:v1 .
```

## Purpose

Builds the custom Nginx image from the Dockerfile.

Docker processes every instruction one layer at a time.

---

# 13. Run the Custom Image

```bash
docker run -d -p 8080:80 --name my-image-test judoski/my-first-image:v1
```

## Purpose

Starts a container from the newly created image.

### Command Breakdown

| Part | Purpose |
|------|---------|
| `-d` | Run in background |
| `-p 8080:80` | Map host port 8080 to container port 80 |
| `--name` | Assign container name |

---

# 14. View Image History

```bash
docker history judoski/my-first-image:v1
```

## Purpose

Displays every layer used to build the image.

This confirmed that:

- COPY created a new layer
- EXPOSE created another metadata layer
- Base layers came from `nginx:alpine`

---

# 15. Stop the Running Container

```bash
docker stop my-image-test
```

## Purpose

Gracefully stops the running container.

---

# 16. Remove the Container

```bash
docker rm my-image-test
```

## Purpose

Deletes the stopped container after testing is complete.

---

# Commands Practiced

| Command | Purpose |
|----------|---------|
| `mkdir` | Create project directory |
| `cd` | Navigate into project |
| `nano` | Create or edit files |
| `docker build` | Build Docker images |
| `docker run` | Create and start containers |
| `docker ps -a` | List all containers |
| `docker rm` | Remove containers |
| `docker rmi` | Remove images |
| `docker history` | Inspect image layers |
| `echo` | Create a file from the terminal |

---

# Key Command Takeaways

- A Dockerfile is simply a sequence of instructions used to build an image.
- The trailing `.` in `docker build` specifies the build context.
- Docker builds images layer by layer and caches unchanged layers.
- `docker history` allows you to inspect how an image was built.
- `docker run` can override `CMD` while leaving `ENTRYPOINT` unchanged.
- Stopped containers continue referencing their images until removed.
- Cleaning up unused containers and images keeps the Docker environment organized and avoids conflicts during future builds.