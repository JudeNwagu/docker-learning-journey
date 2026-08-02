# Day 3 — Understanding Docker Images

## Objective

The goal of Day 3 was to build a deeper understanding of Docker images beyond simply pulling and running them.

Rather than introducing new applications, I revisited **Nginx** and **PostgreSQL** from Day 2 to focus entirely on how Docker images are structured, how image layers contribute to size, how tags work, and what metadata is baked into an image before a container is ever created.

This day was intentionally hands-on. Every command was run inside Ubuntu 24.04 (WSL2), with observations recorded from the actual output rather than assumptions.

---

## What I Learned

Before today, I thought of a Docker image simply as something that containers run from.

After exploring image history, metadata, and tags, I now understand that an image is a reusable blueprint made up of multiple immutable layers.

Some of the key concepts I learned include:

- Why the same application can have multiple image tags (`latest`, `alpine`) with very different sizes.
- How Docker builds images layer by layer.
- How to inspect the metadata already built into an image.
- The difference between an image tag and the underlying image data.
- When Docker actually deletes image data after running `docker rmi`.

---

## What I Practiced

For this exercise, I worked with two images already familiar from previous practice:

### Nginx

I compared the `latest` and `alpine` tags to understand why both run the same web server while occupying significantly different amounts of disk space.

Using `docker history`, I traced where that size difference came from and confirmed that most of it was introduced by the underlying Linux distribution rather than Nginx itself.

Using `docker inspect`, I examined:

- Default startup command (`Cmd`)
- Entrypoint script
- Exposed ports
- Declared volumes

I also practiced creating additional image tags using `docker tag` and explored how Docker manages image references when tags are removed.

---

### PostgreSQL

I repeated the same investigation with PostgreSQL to reinforce the concepts.

Unlike Nginx, PostgreSQL demonstrated that the application itself can contribute more to the image size than the operating system layer.

Inspecting the image also introduced an important concept for the next stage of learning.

Unlike Nginx, PostgreSQL declares a persistent data directory (`/var/lib/postgresql`) as a Docker volume, highlighting why database containers require persistent storage.

---

## Key Concepts Reinforced

### Image Layers

Every Docker image consists of multiple read-only layers.

Some layers contain operating system files.

Others install application software.

Metadata instructions such as `CMD`, `ENV`, and `LABEL` occupy virtually no space.

Understanding layers explains why Docker can reuse existing data instead of downloading or rebuilding everything repeatedly.

---

### Image Tags

Tags are labels, not copies.

Creating a new tag does not duplicate the image.

Instead, multiple tags simply point to the same image ID.

Removing one tag removes only that reference.

The image itself remains available until its final reference is removed.

---

### Image Metadata

Using `docker inspect` revealed that images already contain important runtime information, including:

- Default startup commands
- Entrypoint scripts
- Network ports
- Environment defaults
- Declared volumes

This explains why many containers can start successfully with very few command-line options.

---

## Challenges Encountered

This session contained fewer command errors than Day 2, but several important misconceptions were corrected through experimentation.

These included:

- Mistakenly using an image ID instead of a Docker Hub username while tagging an image.
- Discovering that image deletion depends on whether other tags still reference the same image.
- Learning that image size cannot be judged by assumptions and should instead be verified using `docker history`.

A detailed breakdown of each challenge, including its cause, resolution, and lesson learned, is documented in **`challenges-and-lessons-learned.md`**.

---

## Key Takeaways

By the end of Day 3, I can confidently:

- Explain what a Docker image really is.
- Interpret image layer history using `docker history`.
- Read important image metadata using `docker inspect`.
- Explain why image sizes differ across tags.
- Create and manage image tags correctly.
- Describe when Docker actually removes image data.

More importantly, I now understand that containers are only one part of Docker.

Everything starts with the image, and understanding how images are built makes the rest of Docker much easier to reason about.

---

## Resources

- TechWorld with Nana — Docker Tutorial for Beginners
- Docker Documentation — Understanding Image Layers
- Docker CLI Documentation
- DEC Resources
---

## Repository Structure

```text
Day03/
│
├── README.md
├── commands.md
├── challenges-and-lessons-learned.md
├── screenshots/
├── files/
└── output/
```

---

## Next Step

Day 4 focuses on **Docker Volumes**, building directly on today's discovery that PostgreSQL declares its data directory as a volume to preserve data beyond the lifecycle of a container.