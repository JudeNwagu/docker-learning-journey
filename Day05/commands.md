# Day 5 — Commands Reference

This document contains the Docker commands used while learning how containers communicate over Docker networks.

The focus of this day was understanding Docker's networking model by comparing the default bridge network with a custom bridge network and observing how containers communicate in each scenario.

---

# 1. List Existing Docker Networks

```bash
docker network ls
```

**Purpose**

Displays all Docker networks currently available on the system.

On a fresh Docker installation, the default networks are:

- bridge
- host
- none

This was used as the starting point before creating a custom network.

---

# 2. View Docker's Default Bridge Interface

```bash
ip addr show docker0
```

**Purpose**

Displays information about Docker's default bridge interface (`docker0`).

Useful for observing:

- Bridge IP address
- Network interface status
- Default subnet

This helps explain how Docker connects containers to the host.

---

# 3. Run an Nginx Container on the Default Network

```bash
docker run -d --name test-nginx nginx
```

**Purpose**

Creates and starts an Nginx container attached to Docker's default bridge network.

This container was used to test whether Docker could resolve another container by name.

---

# 4. Open an Interactive Shell Inside the Container

```bash
docker exec -it test-nginx bash
```

**Purpose**

Starts an interactive Bash session inside the running container.

This allowed additional tools to be installed and network tests to be performed directly from within the container.

---

# 5. Install the Ping Utility

```bash
apt-get update
apt-get install -y iputils-ping
```

**Purpose**

Updates the package index and installs the `ping` command.

The official Nginx image does not include `ping` by default.

Installing it made container-to-container connectivity testing possible.

---

# 6. Test Name Resolution on the Default Bridge

```bash
ping net-postgres
```

**Purpose**

Attempts to contact another container using its container name.

Result:

```text
ping: net-postgres: Name or service not known
```

This demonstrated that the default bridge network does not provide automatic DNS resolution between containers.

---

# 7. Exit the Container

```bash
exit
```

**Purpose**

Leaves the interactive Bash session and returns to the host terminal.

---

# 8. Stop and Remove the Test Container

```bash
docker stop test-nginx
docker rm test-nginx
```

**Purpose**

Stops and removes the temporary Nginx container after completing the default bridge experiment.

---

# 9. Create a Custom Bridge Network

```bash
docker network create my-net
```

**Purpose**

Creates a user-defined bridge network called `my-net`.

Unlike Docker's default bridge network, custom bridge networks provide automatic DNS-based service discovery.

---

# 10. Verify the New Network

```bash
docker network ls
```

**Purpose**

Confirms that the newly created network appears in Docker's list of available networks.

---

# 11. Start PostgreSQL on the Custom Network

```bash
docker run -d \
--name net-postgres \
--network my-net \
-e POSTGRES_PASSWORD=mysecretpassword \
postgres
```

**Purpose**

Starts a PostgreSQL container connected directly to the custom network.

---

# 12. Start Nginx on the Custom Network

```bash
docker run -d \
--name net-nginx \
--network my-net \
nginx
```

**Purpose**

Starts an Nginx container on the same custom bridge network.

Since both containers belong to the same network, Docker automatically creates DNS entries for each container name.

---

# 13. Open Bash Inside the Nginx Container

```bash
docker exec -it net-nginx bash
```

**Purpose**

Enters the running Nginx container to perform network tests.

---

# 14. Install Ping Again

```bash
apt-get update
apt-get install -y iputils-ping
```

**Purpose**

Installs the ping utility inside this container.

Each container has its own isolated filesystem, so software installed in one container is not available in another.

---

# 15. Test Communication by Container Name

```bash
ping net-postgres
```

**Purpose**

Tests communication between containers using Docker's built-in DNS.

Result:

```text
PING net-postgres (172.18.0.2)

64 bytes from net-postgres...
```

This confirmed that containers on the same custom bridge network can communicate using container names instead of IP addresses.

---

# 16. Inspect the Network

```bash
docker network inspect my-net
```

**Purpose**

Displays detailed information about the custom network, including:

- Connected containers
- Assigned IP addresses
- Network driver
- Subnet
- Gateway

This provides insight into Docker's internal networking configuration.

---

# 17. Stop Running Containers

```bash
docker stop net-postgres net-nginx
```

**Purpose**

Gracefully stops both containers before cleanup.

---

# 18. Remove Containers

```bash
docker rm net-postgres net-nginx
```

**Purpose**

Deletes the containers after they have been stopped.

---

# 19. Remove the Custom Network

```bash
docker network rm my-net
```

**Purpose**

Deletes the custom bridge network after the exercises are complete.

Docker only removes networks that are no longer in use.

---

# 20. Verify Cleanup

```bash
docker network ls
```

**Purpose**

Confirms that only Docker's default networks remain.

---

# Commands Practiced

| Command | Purpose |
|----------|---------|
| `docker network ls` | List available Docker networks |
| `docker network create` | Create a custom bridge network |
| `docker network inspect` | View network configuration |
| `docker network rm` | Remove a custom network |
| `docker run --network` | Start a container on a specific network |
| `docker exec` | Execute commands inside a running container |
| `ping` | Test connectivity between containers |
| `apt-get update` | Update package lists |
| `apt-get install` | Install additional software inside a container |
| `docker stop` | Stop running containers |
| `docker rm` | Remove stopped containers |
| `ip addr show docker0` | View Docker's default bridge interface |

---

# Key Command Takeaways

- Docker automatically creates three default networks during installation.
- Containers attached to the default bridge network cannot reliably discover one another by name.
- Custom bridge networks provide automatic DNS resolution between containers.
- `docker network inspect` reveals the IP addresses and subnet Docker assigns internally.
- Software installed inside one container does not affect any other container.
- Docker networks are independent resources and remain available until explicitly removed.
- Cleaning up unused containers and networks helps maintain a tidy development environment.