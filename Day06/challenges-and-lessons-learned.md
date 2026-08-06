
# `challenges-and-lessons-learned.md`

````markdown
# Day 6 — Challenges and Lessons Learned

One of the biggest goals for today was understanding how a Docker image is actually built rather than simply running images created by someone else. Writing my own Dockerfile exposed several small mistakes that turned into useful learning opportunities.

---

## Challenge 1 — Couldn't remove the image after testing

### What happened

After testing the `entrypoint-test` image, I tried to remove it immediately:

```bash
docker rmi entrypoint-test
````

Docker responded with:

```text
conflict: unable to delete entrypoint-test:latest
(must be forced)
container ... is using its referenced image
```

### Root cause

Although both containers had already exited, Docker keeps stopped containers unless they are explicitly removed.

Those stopped containers were still referencing the image, so Docker protected the image from being deleted.

### Fix

First I confirmed the containers still existed:

```bash
docker ps -a
```

Then I removed them:

```bash
docker rm <container-id-1> <container-id-2>
```

After that:

```bash
docker rmi entrypoint-test
```

worked successfully.

### Lesson learned

An exited container is **not** the same thing as a deleted container.

Docker protects images that are still referenced by existing containers, even if those containers are no longer running.

For quick experiments, using:

```bash
docker run --rm
```

is often cleaner because Docker automatically removes the container after it exits.

---

## Challenge 2 — Bash failed to create the HTML file

### What happened

I accidentally typed:

```bash
echo'<h1>Built by Judoski, from a real Dockerfile</hi>' > index.html
```

Instead of creating the file, Bash returned:

```text
No such file or directory
```

### Root cause

There was no space between:

```text
echo
```

and the opening quotation mark.

Bash therefore treated the entire text as one command name instead of treating the quoted text as an argument.

I also noticed a typo in the closing HTML tag:

```html
</hi>
```

instead of

```html
</h1>
```

### Fix

Adding the missing space and correcting the HTML solved the problem.

```bash
echo '<h1>Built by Judoski, from a real Dockerfile</h1>' > index.html
```

### Lesson learned

Tiny syntax mistakes in Bash often produce errors that appear unrelated.

When a command behaves unexpectedly, checking for missing spaces and small typing errors should be one of the first troubleshooting steps.

---

## Challenge 3 — Docker tried pulling an image from Docker Hub

### What happened

After successfully building my image, I attempted to run it using:

```bash
docker run -d -p 8080:80 --name my-image-test judoski/myfirst-image:v1
```

Docker responded:

```text
pull access denied
repository does not exist
```

### Root cause

I had accidentally omitted the hyphen.

The actual image name was:

```text
judoski/my-first-image:v1
```

not

```text
judoski/myfirst-image:v1
```

Because Docker could not find an exact local match, it assumed I wanted an image from Docker Hub and attempted to download it.

### Fix

Running the command with the correct image name started the container immediately.

```bash
docker run -d -p 8080:80 --name my-image-test judoski/my-first-image:v1
```

### Lesson learned

Docker checks the local image cache first.

If an exact image name is not found, it automatically attempts to pull that image from a registry.

A simple naming mistake can therefore produce an error that initially looks like an authentication or registry problem.

---

# Key Lessons from Day 6

* Writing a Dockerfile makes image creation reproducible instead of relying entirely on public images.
* Every Dockerfile instruction becomes a separate image layer that can later be inspected with `docker history`.
* `ENTRYPOINT` defines the executable that always runs, while `CMD` provides default arguments that can be replaced.
* Stopped containers continue to reference their images until explicitly removed.
* Docker automatically searches Docker Hub when an exact local image name cannot be found.
* Small Bash syntax mistakes can prevent commands from working, making careful typing and reading error messages an important troubleshooting skill.
* Building, running, inspecting, and cleaning up a custom image provided a complete view of the Docker image lifecycle from source files to a running container.
