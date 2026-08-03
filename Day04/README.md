# Day 4 — Docker Volumes

## Objective

Understand how Docker volumes solve the data persistence problem by keeping data outside a container's writable layer. This day focused on both **named volumes** and **bind mounts**, explaining when to use each and verifying their behavior through hands-on practice with PostgreSQL and Nginx.

---

## What I Learned

One of the biggest limitations I discovered on Day 2 was that deleting a container also deleted the data stored inside it. Today, I explored the Docker features designed to solve that problem.

I learned that Docker offers multiple approaches for handling container data:

- **Container writable layer** – data exists only as long as the container exists.
- **Named volumes** – Docker-managed storage that survives container removal.
- **Bind mounts** – direct links between a host directory and a directory inside a container.

Using PostgreSQL, I confirmed that a named volume preserves database data independently of any container. Even after removing and recreating the container, previously stored data remained available because it lived inside the volume rather than inside the container itself.

With Nginx, I experimented with bind mounts by serving a local HTML file from my machine. Updating the file on the host immediately changed what the browser displayed without restarting the container, demonstrating that bind mounts always reflect the current contents of the host directory.

I also investigated a Bash history expansion error caused by using an exclamation mark inside double quotes, learning why the shell interpreted it as a history event and how using single quotes avoided the issue.

---

## What I Practiced

### Named Volumes

- Created Docker-managed volumes
- Listed existing volumes
- Inspected volume metadata
- Attached a named volume to a PostgreSQL container
- Verified that database data persisted after recreating the container

### Bind Mounts

- Created a local directory for Nginx content
- Mounted the directory into the container
- Served custom HTML directly from the host
- Updated files on the host and observed live changes in the browser without restarting the container

---

## Challenges Faced

During the bind mount exercise, Bash returned:

```text
-bash: !: event not found
```

After investigating, I learned that Bash performs history expansion on the `!` character when it appears inside double-quoted strings.

Using single quotes prevented Bash from interpreting the character as a history reference and allowed the command to execute correctly.

This reinforced the importance of understanding shell behavior rather than simply memorizing commands.

---

## Key Takeaways

- Named volumes preserve application data beyond the life of individual containers.
- Volumes exist independently of containers and remain until explicitly removed.
- `docker volume inspect` reveals where Docker stores volume data on the host.
- Bind mounts provide direct access to host files, making them useful during development.
- File changes inside a bind-mounted directory are reflected immediately inside running containers.
- Small shell features, such as Bash history expansion, can affect command execution in unexpected ways.

---

## Resources

### Course

- TechWorld with Nana – Docker Tutorial for Beginners (Volumes section)

### Documentation

- Docker Official Documentation – Volumes

### Additional Reading

- Docker Volumes Explained: Bind Mount vs Named Volume vs tmpfs
- Diving Deeper Into Docker: Volumes and Bind Mounts
- How to Choose Between Docker Bind Mounts and Named Volumes

---

## Output

### PostgreSQL Named Volume

- Created a named volume (`pgdata`)
- Attached it to PostgreSQL
- Verified previously stored database records remained available after recreating the container

### Nginx Bind Mount

- Served a custom HTML page from the host machine
- Updated the HTML file without restarting the container
- Confirmed live changes appeared immediately in the browser

---

## Reflection

Day 2 showed me that containers are disposable.

Day 4 showed me that **data doesn't have to be**.

Understanding the difference between container storage and Docker-managed storage makes it much clearer why databases almost always use volumes in production environments. Containers can be replaced whenever necessary while the data remains intact.

This also connected naturally to what I observed on Day 3 when inspecting the PostgreSQL image. The declared volume wasn't just metadata—it was a signal that PostgreSQL expects its data to live outside the container itself.

---

## Next Steps

Day 5 will explore **Docker Networking**, learning how containers communicate with one another and how Docker provides networking capabilities without requiring manual configuration.