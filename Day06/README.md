# Day 6 — Building Images with Dockerfile

## Objective

Learn how to build custom Docker images by writing Dockerfiles from scratch. The focus of this session was understanding how Dockerfile instructions become image layers, building a custom image, and proving the behavioral difference between `CMD` and `ENTRYPOINT` through hands-on experiments rather than theory alone.

---

# Environment

- **Operating System:** Ubuntu 24.04 (WSL2)
- **Container Runtime:** Docker Desktop (WSL2 Backend)
- **Shell:** Bash
- **Primary Learning Resource:** TechWorld with Nana – Dockerfile: Building Our Own Docker Image
- **Supporting Resources:**
  - BMC — Docker CMD vs ENTRYPOINT
  - Dockerfile Hands-on Tutorial
  - Personal practice and experimentation

---

# What I Learned

Until today, I had only been running images created by other people.

This session shifted my understanding from using Docker images to creating them.

I learned that a Docker image is not built as a single file. Instead, every instruction inside a Dockerfile creates a new image layer, which Docker can cache and reuse during future builds.

I also explored one of Docker's most commonly misunderstood topics: the relationship between `CMD` and `ENTRYPOINT`. Rather than relying on documentation alone, I created a small experiment that demonstrated exactly how they work together.

---

# What I Built / Practiced

### Built a Dockerfile from Scratch

Created my first Dockerfile using Alpine Linux to understand the relationship between:

- `FROM`
- `ENTRYPOINT`
- `CMD`

Rather than building a web application immediately, I started with a simple example that isolated Docker's startup behavior.

---

### Proved the Difference Between CMD and ENTRYPOINT

Built a small image using:

```dockerfile
FROM alpine
ENTRYPOINT ["echo"]
CMD ["Hello from default CMD"]
```

Running the container normally produced:

```text
Hello from default CMD
```

Running the same image while supplying a custom argument:

```bash
docker run entrypoint-test "This is my own custom message"
```

produced:

```text
This is my own custom message
```

This demonstrated that `ENTRYPOINT` remains fixed while `CMD` provides default arguments that can be replaced at runtime.

---

### Built My First Custom Nginx Image

Created a simple HTML page:

```html
<h1>Built by Judoski, from a real Dockerfile</h1>
```

Then wrote a Dockerfile that:

- used `nginx:alpine` as the base image
- copied the custom HTML file into Nginx's web directory
- documented port 80 using `EXPOSE`

Built the image successfully using:

```bash
docker build -t judoski/my-first-image:v1 .
```

---

### Verified the Complete Build Pipeline

Started a container from the newly built image.

Visited:

```
http://localhost:8080
```

Instead of the default Nginx welcome page, the browser displayed the custom HTML page created earlier.

This confirmed the complete workflow:

Dockerfile

↓

Docker Image

↓

Docker Container

↓

Running Application

---

### Examined the Image Layers

Used:

```bash
docker history
```

to inspect the newly created image.

Unlike Day 3, where I only inspected images created by others, this time I could clearly identify my own `COPY` and `EXPOSE` instructions as new layers added on top of the base image.

---

# Key Concepts Learned

## A Dockerfile is an Image Blueprint

Every Docker image begins with a Dockerfile.

Each instruction becomes its own immutable image layer.

---

## CMD vs ENTRYPOINT

Although often confused, they serve different purposes.

`ENTRYPOINT` defines the executable that always runs.

`CMD` supplies default arguments that can be replaced when starting the container.

The hands-on experiment made this relationship much clearer than reading documentation alone.

---

## Docker Build Context

The trailing period in:

```bash
docker build -t image-name .
```

is significant.

It tells Docker where to find:

- the Dockerfile
- copied files
- build resources

Everything inside that directory becomes the build context.

---

## Image Layers

Docker builds images layer by layer.

Because layers are cached, rebuilding an image only recreates the layers that have changed.

This explains why later builds become significantly faster.

---

## Image Cleanup

Stopping a container does not remove it.

Even stopped containers continue referencing their images.

An image cannot be removed until every container that depends on it has also been removed.

---

# Key Takeaways

- Dockerfiles define how images are built.
- Every Dockerfile instruction creates an image layer.
- `ENTRYPOINT` remains fixed while `CMD` provides overridable default arguments.
- Docker caches image layers to improve build performance.
- The build context determines which files Docker can access during image creation.
- Stopped containers still reference their images until removed.

---

# Resources

### Primary Resource

- TechWorld with Nana — Dockerfile: Building Our Own Docker Image

### Supporting Resources

- BMC — Docker CMD vs ENTRYPOINT
- Dockerfile Hands-on Tutorial
- Docker Documentation

---

# Repository Structure

```text
Day06/
│
├── README.md
├── commands.md
├── challenges-and-lessons-learned.md
│
├── screenshots/
├── files/
└── output/
```

---

# Day 6 Skills Demonstrated

- ✅ Can explain every core Dockerfile instruction
- ✅ Understand the difference between CMD and ENTRYPOINT
- ✅ Can build a custom Docker image from scratch
- ✅ Can inspect image layers using `docker history`
- ✅ Understand Docker's build context
- ✅ Can explain why images sometimes cannot be removed immediately

---

# Next Steps

Day 7 will build on today's Dockerfile knowledge by exploring how multiple containers and custom images work together in a more complete application workflow using Docker Compose.