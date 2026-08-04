# Day 5 — Docker Networking

## Objective

Understand how Docker enables containers to communicate with one another and with the outside world. The goal for this day was to move beyond port mapping and learn why custom Docker networks are essential for real multi-container applications.

Rather than simply creating a network, I wanted to prove the difference between Docker's default bridge network and a custom bridge network through practical experiments.

---

# Environment

- **Operating System:** Ubuntu 24.04 (WSL2)
- **Container Runtime:** Docker Desktop (WSL2 Backend)
- **Shell:** Bash
- **Primary Learning Resource:** freeCodeCamp – Docker Tutorial for Beginners (Networking section)
- **Supporting Resources:**
  - Docker Official Documentation
  - Medium articles on Docker Networking
  - Stack Overflow discussions on Docker network modes
  - Additional networking reference notes

---

# What I Learned

Before today, I understood how to expose a container to my host machine using port mapping (`-p`). What I hadn't explored was how containers communicate with each other.

Through today's practice I learned:

- why containers are isolated by default
- the purpose of Docker's default bridge network
- why custom bridge networks are recommended for multi-container applications
- how Docker provides automatic DNS resolution on custom networks
- how packets travel from a container to the outside world
- the role of Docker's virtual Ethernet (veth) pairs
- the difference between bridge, host, none, and container networking modes

One of the biggest takeaways was understanding that networking solves an entirely different problem from port mapping.

---

# What I Built / Practiced

To reinforce these concepts, I completed several hands-on exercises.

### Explored Docker's Built-in Networks

Listed Docker's default networking modes and confirmed the automatically created networks:

- bridge
- host
- none

This helped establish the starting point before creating a custom network.

---

### Demonstrated the Limitation of the Default Bridge

Created an Nginx container on the default bridge network and attempted to reach another container by name.

After installing the missing `ping` utility inside the container, the command returned:

```text
ping: net-postgres: Name or service not known
```

This confirmed that Docker's default bridge network does not provide automatic service discovery by container name.

---

### Created a Custom Bridge Network

Created a custom network named:

```text
my-net
```

Started both PostgreSQL and Nginx on that network and repeated the same test.

Instead of failing, Docker resolved:

```text
net-postgres
```

to its internal IP address automatically.

This demonstrated one of the primary reasons custom bridge networks are used in real applications.

---

### Inspected Docker's Network Configuration

Used Docker's inspection tools to examine:

- subnet allocation
- connected containers
- assigned IP addresses

Seeing Docker's internal DNS information alongside the subnet configuration made it much easier to understand how containers locate one another.

---

### Cleaned Up Resources

Stopped and removed:

- both containers
- the custom network

Leaving Docker in a clean state for the next day's practice.

---

# Key Concepts Learned

## Port Mapping vs Networking

These solve different problems.

Port mapping allows traffic from the host machine into a container.

Networking allows containers to communicate directly with one another.

---

## Default Bridge Network

Docker automatically attaches containers to the default bridge network unless another network is specified.

While convenient for simple experiments, it does not provide reliable name-based communication between containers.

---

## Custom Bridge Networks

A custom bridge network adds automatic DNS resolution.

Instead of remembering changing IP addresses, containers can communicate using stable container names.

This is the networking model commonly used by Docker Compose and many production applications.

---

## Docker's Internal Networking

Every container connects to Docker's networking layer through a virtual Ethernet pair.

Traffic travels from:

Container

↓

Virtual Ethernet Pair

↓

Docker Bridge

↓

Network Address Translation (NAT)

↓

Host Network Interface

↓

Internet

Understanding this flow helped explain why Docker networking behaves the way it does.

---

## Network Inspection

Using `docker network inspect` revealed:

- connected containers
- IP addresses
- subnet information

This made Docker's networking much less of a "black box."

---

# Key Takeaways

- Port mapping and networking are different concepts.
- Docker's default bridge network has limitations for multi-container applications.
- Custom bridge networks provide automatic DNS resolution between containers.
- Docker networking relies on virtual Ethernet pairs and network namespaces.
- `docker network inspect` provides valuable insight into Docker's internal networking.
- Docker Compose relies on these same networking principles behind the scenes.

---

# Resources

### Primary Resource

- freeCodeCamp — Docker Tutorial for Beginners (Networking)

### Supporting Resources

- Docker Official Documentation
- Docker Network CLI Reference
- Medium — Understanding Docker Networking
- Stack Overflow discussions on Docker network modes
- Personal networking study notes

---

# Repository Structure

```text
Day05/
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

# Day 5 Checkpoint

- ✅ Understand the purpose of Docker networking
- ✅ Can explain why port mapping and networking solve different problems
- ✅ Understand the limitations of the default bridge network
- ✅ Can create and inspect a custom bridge network
- ✅ Can demonstrate container-to-container communication using DNS
- ✅ Understand Docker's packet flow from container to internet
- ✅ Understand when custom bridge networks should be used

---

# Next Steps

Day 6 will build on today's networking concepts by introducing **Docker Compose**, where multiple containers can be defined, started, and connected using a single configuration file.

Networking is one of the core features that makes Docker Compose practical, since services automatically communicate through the custom network Compose creates for each project.