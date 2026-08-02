# Day 3 — Challenges & Lessons Learned

Understanding Docker images required more than memorizing commands. It meant questioning assumptions, validating them through experimentation, and refining my understanding based on what Docker actually did.

This document records the misconceptions I encountered during Day 3, how I verified them, and the lessons I took away.

---

# Challenge 1 — Assuming Smaller Images Mean Less Software

## What Happened

After pulling both `nginx:latest` and `nginx:alpine`, I noticed a significant difference in their sizes.

```text
nginx:latest   238 MB
nginx:alpine    92.7 MB
```

My initial assumption was that the Alpine version contained fewer Nginx features.

---

## Root Cause

I assumed the application itself was responsible for most of the image size.

---

## Investigation

Using:

```bash
docker history nginx
```

I examined the image layer by layer.

The output showed that the largest difference came from the underlying operating system layer rather than Nginx itself.

---

## Resolution

I confirmed that both images provide the same Nginx software.

The size difference comes primarily from the Linux distribution used as the base image.

---

## Lesson Learned

An image's size is not determined solely by the application it contains.

The base operating system often contributes significantly to the final image size.

Rather than guessing, `docker history` provides the evidence.

---

# Challenge 2 — Assuming the Operating System Is Always the Largest Layer

## What Happened

After observing that the Debian base layer dominated the Nginx image size, I expected the same pattern when exploring PostgreSQL.

---

## Investigation

Using:

```bash
docker history postgres
```

I compared the image layers.

Unlike Nginx, PostgreSQL's application installation layer was substantially larger than its operating system layer.

---

## Resolution

This demonstrated that image composition depends on the software being packaged.

Some applications contribute more to image size than the operating system itself.

---

## Lesson Learned

There is no universal rule for image size.

Always inspect the image rather than assuming where the storage is being used.

---

# Challenge 3 — Using an Image ID Instead of a Docker Hub Username

## What Happened

While practicing image tagging, I ran:

```bash
docker tag nginx:latest 5a88c9c45479/my-nginx:v1
```

The command completed successfully, which initially made it appear correct.

---

## Root Cause

I mistakenly used the image ID where a Docker Hub username should normally appear.

Docker accepted it because, locally, it simply treats that section as part of the tag name.

---

## Resolution

After reviewing Docker's tagging convention, I corrected the format to:

```bash
docker tag postgres:latest judoski/my-postgres:v1
```

---

## Lesson Learned

Local tags are flexible.

However, images intended for Docker Hub should follow the format:

```text
username/image-name:tag
```

Using the correct naming convention from the beginning avoids confusion later.

---

# Challenge 4 — Misunderstanding How Image Deletion Works

## What Happened

Initially, I believed removing a custom tag would always leave the original image untouched.

To verify this, I removed multiple tags pointing to the same image.

```bash
docker rmi nginx:latest 5a88c9c45479/my-nginx:v1
```

Docker reported:

```text
Untagged...
Deleted...
```

---

## Investigation

The output showed that once every remaining tag referencing the image had been removed, Docker deleted the underlying image data as well.

---

## Resolution

I repeated the exercise using PostgreSQL and observed the same behaviour.

The important factor was not which tag was removed.

It was whether any tag still referenced the image.

---

## Lesson Learned

Image tags are references.

Docker removes the image itself only after its final reference has been deleted.

---

# Challenge 5 — Discovering Image Metadata

## What Happened

Before using `docker inspect`, I assumed that most runtime behaviour was configured only when starting a container.

Running:

```bash
docker inspect nginx
```

and

```bash
docker inspect postgres
```

showed otherwise.

---

## Investigation

The images already contained important configuration information, including:

- Default startup command
- Entrypoint
- Exposed ports
- Declared volumes

---

## Resolution

I realised that image creators define much of a container's default behaviour long before the container is ever started.

---

## Lesson Learned

Containers inherit much of their behaviour from the image.

Understanding image metadata makes Docker commands easier to understand because many defaults are already built into the image itself.

---

# Reflection

Day 2 taught me how to work with containers.

Day 3 shifted my focus to understanding what containers are actually created from.

Before today, I viewed images as files that Docker downloaded.

Now I understand that they are reusable blueprints built from multiple immutable layers, complete with their own metadata, startup configuration, and version tags.

That shift in understanding made many Docker commands feel much more logical rather than something to memorize.

---

# Looking Ahead

One detail stood out while inspecting the PostgreSQL image.

Unlike Nginx, PostgreSQL declares a volume for its data directory.

That naturally raises the next question:

**How can container data survive after a container is removed?**

That question becomes the focus of **Day 4 — Docker Volumes**.